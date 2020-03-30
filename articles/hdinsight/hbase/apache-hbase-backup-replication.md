---
title: Apache HBase, Phoenix için yedekleme & çoğaltma - Azure HDInsight
description: Azure HDInsight'ta Apache HBase ve Apache Phoenix için yedekleme ve çoğaltma ayarlama
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/19/2019
ms.openlocfilehash: c6d33158b581bf4394a0d1bac2b277830328e110
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495941"
---
# <a name="set-up-backup-and-replication-for-apache-hbase-and-apache-phoenix-on-hdinsight"></a>HDInsight'ta Apache HBase ve Apache Phoenix için yedekleme ve çoğaltma ayarlama

Apache HBase veri kaybına karşı koruma için çeşitli yaklaşımları destekler:

* Klasörü `hbase` kopyalama
* Dışa Aktarma sonra İçe Aktar
* Tabloları kopyalama
* Anlık Görüntüler
* Çoğaltma

> [!NOTE]  
> Apache Phoenix meta verilerini HBase tablolarında saklar, böylece HBase sistem katalog tablolarını yedeklediğinizde meta veriler yedeklenir.

Aşağıdaki bölümlerde bu yaklaşımların her biri için kullanım senaryosu açıklayınız.

## <a name="copy-the-hbase-folder"></a>Hbase klasörünü kopyalama

Bu yaklaşımla, tabloların veya sütun ailelerinin bir alt kümesini seçemeden tüm HBase verilerini kopyalarsınız. Sonraki yaklaşımlar daha fazla denetim sağlar.

HDInsight'taki HBase, küme oluşturulurken seçilen varsayılan depolama alanını (Azure Depolama lekeleri veya Azure Veri Gölü Depolama) kullanır. Her iki durumda da, HBase veri ve meta veri dosyalarını aşağıdaki yol altında depolar:

    /hbase

* Azure Depolama hesabında `hbase` klasör, blob kapsayıcısının kökünde bulunur:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* Azure Veri Gölü Depolama'da `hbase` klasör, küme sağlarken belirttiğiniz kök yolunun altında bulunur. Bu kök yolu genellikle `clusters` HDInsight kümenizin adını taşıyan bir alt klasöriçeren bir klasöre sahiptir:

    ```
    /clusters/<clusterName>/hbase
    ```

Her iki durumda `hbase` da klasör, HBase'in diske çektiği tüm verileri içerir, ancak bellek içi verileri içermeyebilir. HBase verilerinin doğru bir gösterimi olarak bu klasöre güvenmeden önce kümeyi kapatmanız gerekir.

Kümeyi sildikten sonra, verileri yerinde bırakabilir veya verileri yeni bir konuma kopyalayabilirsiniz:

* Geçerli depolama konumunu gösteren yeni bir HDInsight örneği oluşturun. Yeni örnek, varolan tüm verilerle oluşturulur.

* Klasörü `hbase` farklı bir Azure Depolama blob kapsayıcısına veya Veri Gölü Depolama konumuna kopyalayın ve ardından bu verilerle yeni bir küme başlatın. Azure Depolama için [AzCopy'yi](../../storage/common/storage-use-azcopy.md)ve Veri Gölü Depolama için [AdlCopy'i](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md)kullanın.

## <a name="export-then-import"></a>Dışa Aktarma sonra İçe Aktar

Kaynak HDInsight kümesinde, kaynak tablodaki verileri varsayılan ekli depolamaalanına aktarmak için [Dışa](https://hbase.apache.org/book.html#export) Aktarma yardımcı programını (HBase ile birlikte) kullanın. Daha sonra dışa aktarılan klasörü hedef depolama konumuna kopyalayabilir ve hedef HDInsight kümesinde [Alma yardımcı programını](https://hbase.apache.org/book.html#import) çalıştırabilirsiniz.

Tablo verilerini dışa aktarmak için, önce kaynak HDInsight kümenizin baş `hbase` düğümüne SSH ve ardından aşağıdaki komutu çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Dışa aktarma dizini zaten var olmamalıdır. Tablo adı büyük/küçük harf duyarlıdır.

Tablo verilerini almak için, SSH hedef HDInsight kümenizin baş `hbase` düğümüne ve ardından aşağıdaki komutu çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Tablo zaten var olmalı.

Varsayılan depolamaya veya ekli depolama seçeneklerinden herhangi biri için tam dışa aktarma yolunu belirtin. Örneğin, Azure Depolama'da:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

Azure Veri Gölü Depolama Gen2'de sözdizimi şudur:

    abfs://<containername>@<accountname>.dfs.core.windows.net/<path>

Azure Veri Gölü Depolama Gen1'de sözdizimi şudur:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Bu yaklaşım, tablo düzeyinde taneciklilik sunar. Ayrıca, işlemi artımlı olarak gerçekleştirmenize olanak tanıyan satırların dahil etmesi için bir tarih aralığı da belirtebilirsiniz. Unix çağından bu yana her tarih milisaniye cinsindendir.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Dışa aktak etmek için her satırın sürümlerinin sayısını belirtmeniz gerektiğini unutmayın. Tarih aralığına tüm sürümleri eklemek `<numberOfVersions>` için, 100000 gibi mümkün olan en yüksek satır sürümlerinizden daha büyük bir değere ayarlayın.

## <a name="copy-tables"></a>Tabloları kopyalama

[CopyTable yardımcı programı,](https://hbase.apache.org/book.html#copy.table) kaynak tablodaki verileri satır satır, kaynakla aynı şemaya sahip varolan bir hedef tabloya kopyalar. Hedef tablo aynı kümede veya farklı bir HBase kümesinde olabilir. Tablo adları büyük/küçük harf duyarlıdır.

CopyTable'ı bir küme içinde kullanmak için, Kaynak HDInsight kümenizin `hbase` baş düğümüne SSH ve sonra bu komutu çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>


Farklı bir kümedeki bir tabloya kopyalamak için `peer` CopyTable'ı kullanmak için, hedef kümenin adresiyle anahtarı ekleyin:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

Hedef adres aşağıdaki üç bölümden oluşur:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>`Örneğin, Apache ZooKeeper düğümlerinin virgülle ayrılmış bir listesidir:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>`HDInsight varsayılan olarak `<ZnodeParent>` 2181'e `/hbase-unsecure`ve , `<destinationAddress>` böylece tam olacaktır:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500x tg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

HDInsight kümeniz için bu değerleri nasıl alabildiğini öğrenmek için bu makalede [Apache ZooKeeper Quorum Listesini El Ile Toplama'ya](#manually-collect-the-apache-zookeeper-quorum-list) bakın.

CopyTable yardımcı programı, kopyalanması gereken satırların zaman aralığını belirtmek ve kopyalanması gereken bir tablodaki sütun ailelerinin alt kümesini belirtmek için parametreleri de destekler. CopyTable tarafından desteklenen parametrelerin tam listesini görmek için CopyTable'ı herhangi bir parametre olmadan çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable, hedef tabloya kopyalanacak tüm kaynak tablo içeriğini tarar. Bu, CopyTable yürütürken HBase kümenizin performansını düşürebilir.

> [!NOTE]  
> Tablolar arasında veri kopyalamayı otomatikleştirmek için, GitHub'daki Azure `hdi_copy_table.sh` [HBase Utils](https://github.com/Azure/hbase-utils/tree/master/replication) deposundaki komut dosyasına bakın.

### <a name="manually-collect-the-apache-zookeeper-quorum-list"></a>Apache ZooKeeper çoğunluk Listesini el ile toplayın

Her iki HDInsight kümesi de daha önce açıklandığı gibi aynı sanal ağda olduğunda, iç ana bilgisayar ad çözünürlüğü otomatiktir. BIR VPN Ağ Geçidi ile bağlanan iki ayrı sanal ağda HDInsight kümeleri için CopyTable'ı kullanmak için, çoğunlukta Zookeeper düğümlerinin ana bilgisayar IP adreslerini sağlamanız gerekir.

Çoğunluk ana bilgisayar adlarını elde etmek için aşağıdaki kıvırma komutunu çalıştırın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

Curl komutu HBase yapılandırma bilgilerine sahip bir JSON belgesini alır ve grep komutu yalnızca "hbase.zookeeper.quorum" girişini döndürür, örneğin:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

Çoğunluk ana bilgisayar adları değeri, iki nokta üst üstenin sağındaki tüm dizedir.

Bu ana bilgisayarların IP adreslerini almak için, önceki listedeki her ana bilgisayar için aşağıdaki kıvrık komutunu kullanın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

Bu kıvırma `<zookeeperHostFullName>` komutunda, örneğin `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`bir ZooKeeper ana bilgisayar tam DNS adıdır. Komutun çıktısı, örneğin belirtilen ana bilgisayar için IP adresini içerir:

    100    "ip" : "10.0.0.9",

Yeterli çoğunluktaki tüm ZooKeeper düğümleri için IP adreslerini topladıktan sonra, hedef adresi yeniden oluşturun:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

Örneğimizde:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Anlık Görüntüler

[Anlık görüntüler,](https://hbase.apache.org/book.html#ops.snapshots) HBase veri deponuzdaki verilerin zamanında yedeklemesini sağlar. Anlık görüntü işlemi, o anda depolama alanındaki tüm dosyaların adlarını yakalayan bir meta veri işlemi olduğundan, anlık görüntüler en az ek yüküne sahiptir ve saniyeler içinde tamamlanır. Anlık görüntü sırasında gerçek veri kopyalanır. Anlık görüntüler, güncelleştirmelerin, silmelerin ve eklerin tümü yeni veri olarak temsil edildiği HDFS'de depolanan verilerin değişmez yapısına dayanır. Aynı kümedeki anlık görüntügeri yükleyebilir *(klonlama)* veya anlık görüntübaşka bir kümeye aktarabilirsiniz.

Anlık görüntü oluşturmak için, HDInsight HBase kümenizin baş düğümünde `hbase` SSH ve kabuğu başlatın:

    hbase shell

Hbase kabuğu içinde, tablonun ve bu anlık görüntünün adlarını içeren anlık görüntü komutunu kullanın:

    snapshot '<tableName>', '<snapshotName>'

`hbase` Kabuk içinde ada göre anlık görüntü geri yüklemek için önce tabloyu devre dışı bırakıp anlık görüntügeri yükleyin ve tabloyu yeniden etkinleştirin:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Anlık görüntüyeni bir tabloya geri yüklemek için clone_snapshot kullanın:

    clone_snapshot '<snapshotName>', '<newTableName>'

Başka bir küme tarafından kullanılmak üzere HDFS'ye anlık görüntü aktarmak için, önce daha önce açıklandığı gibi anlık görüntü oluşturun ve ardından Export Snapshot yardımcı programını kullanın. Bu yardımcı programı SSH oturumunun içinden kabuk içinde `hbase` değil, baş düğümüne çalıştırın:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

Kaynak `<hdfsHBaseLocation>` kümeniz tarafından erişilebilen depolama konumlarından herhangi biri olabilir ve hedef kümeniz tarafından kullanılan hbase klasörünü işaret etmelidir. Örneğin, kaynak kümenize bağlı ikincil bir Azure Depolama hesabınız varsa ve bu hesap hedef kümenin varsayılan depolama alanı tarafından kullanılan kapsayıcıya erişim sağlıyorsa, bu komutu kullanabilirsiniz:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Anlık görüntü dışa aktarıldıktan sonra, SSH hedef kümenin baş düğümüne ve daha önce açıklandığı gibi restore_snapshot komutunu kullanarak anlık görüntüsü geri yükleyin.

Anlık görüntüler, komut sırasında bir tablonun `snapshot` tam bir yedeklemesini sağlar. Anlık görüntüler, zaman pencerelerine göre artımlı anlık görüntü gerçekleştirme veya anlık görüntüye dahil etmek üzere sütun ailelerinin alt kümelerini belirtme olanağı sağlamaz.

## <a name="replication"></a>Çoğaltma

[HBase çoğaltma,](https://hbase.apache.org/book.html#_cluster_replication) kaynak kümeüzerinde en az ek yükü olan bir eşzamanlı mekanizma kullanarak hareketleri bir kaynak kümeden hedef kümeye otomatik olarak iter. HDInsight'ta, kümeler arasında çoğaltmayı şunları yapabilirsiniz:

* Kaynak ve hedef kümeleri aynı sanal ağdadır.
* Kaynak ve hedef kümeleri VPN ağ geçidiyle bağlı farklı sanal ağlarda dır, ancak her iki küme de aynı coğrafi konumda bulunur.
* Kaynak küme ve hedef kümeleri vpn ağ geçidiyle birbirine bağlı farklı sanal ağlarda dır ve her küme farklı bir coğrafi konumda bulunur.

Çoğaltmayı ayarlamak için genel adımlar şunlardır:

1. Kaynak kümede, tabloları oluşturun ve verileri doldurun.
2. Hedef kümede, kaynak tablonun şemasıyla boş hedef tabloları oluşturun.
3. Hedef kümeyi kaynak kümeye eş olarak kaydedin.
4. İstenilen kaynak tablolarda çoğaltmayı etkinleştirin.
5. Varolan verileri kaynak tablolardan hedef tablolara kopyalayın.
6. Çoğaltma, kaynak tablolardaki yeni veri değişikliklerini otomatik olarak hedef tablolara kopyalar.

HDInsight'ta çoğaltmayı etkinleştirmek için, çalışan kaynak HDInsight kümenize bir Komut Dosyası Eylemi uygulayın. Kümenizde çoğaltmayı etkinleştirme veya Azure Kaynak Yönetimi şablonlarını kullanarak sanal ağlarda oluşturulan örnek kümeler üzerinde çoğaltma denemesi yapmak için [Bkz.](apache-hbase-replication.md) Bu makale, Phoenix meta verilerinin çoğaltılmasını etkinleştirmek için yönergeler de içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache HBase çoğaltma yapılandırma](apache-hbase-replication.md)
* [HBase İthalat ve İhracat Programı ile çalışma](https://blogs.msdn.microsoft.com/data_otaku/2016/12/21/working-with-the-hbase-import-and-export-utility/)
