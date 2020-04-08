---
title: Küme boyutlarını ölçeklendir - Azure HDInsight
description: Azure HDInsight'ta iş yükünüze uyacak şekilde bir Apache Hadoop kümesini elastik olarak ölçeklendirin
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 0fc067f0be4ac5d2b8fa7db9ad7999efe06625a0
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804512"
---
# <a name="scale-azure-hdinsight-clusters"></a>Azure HDInsight kümelerini ölçeklendirin

HDInsight, kümelerinizdeki işçi düğümlerinin sayısını ölçeklendirme ve küçültme seçenekleriyle esneklik sağlar. Bu esneklik, bir kümeyi saatler sonra veya hafta sonları küçültmenizi sağlar. Ve yoğun iş talepleri sırasında genişletin.

Kümenin yeterli kaynağa sahip olması için periyodik toplu işlemden önce kümenizi ölçeklendirin. İşlem tamamlandıktan ve kullanım azaldıktan sonra, HDInsight kümesini daha az alt düğüme küçültün.

Aşağıda özetlenen yöntemlerden birini kullanarak bir kümeyi el ile ölçeklendirebilirsiniz. Belirli ölçümlere yanıt olarak otomatik olarak ölçeklendirmek için [otomatik ölçeklendirme](hdinsight-autoscale-clusters.md) seçeneklerini de kullanabilirsiniz.

> [!NOTE]  
> Yalnızca HDInsight sürüm 3.1.3 veya üzeri olan kümeler desteklenir. Kümenizin sürümünden emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.

## <a name="utilities-to-scale-clusters"></a>Kümeleri ölçeklendirmek için yardımcı programlar

Microsoft kümeleri ölçeklendirmek için aşağıdaki yardımcı programları sağlar:

|Yardımcı program | Açıklama|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[`Set-AzHDInsightClusterSize`](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[`Set-AzureRmHDInsightClusterSize`](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) | [`az hdinsight resize`](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) `--resource-group RESOURCEGROUP --name CLUSTERNAME --workernode-count NEWSIZE`|
|[Azure Klasik CLI](hdinsight-administer-use-command-line.md)|`azure hdinsight cluster resize CLUSTERNAME NEWSIZE` |
|[Azure portal](https://portal.azure.com)|HDInsight küme bölmenizi açın, sol menüde **Küme boyutunu** seçin, ardından Küme boyutu bölmesinde, alt düğüm sayısını yazın ve Kaydet'i seçin.|  

![Azure portal ölçeği kümeleme seçeneği](./media/hdinsight-scaling-best-practices/azure-portal-settings-nodes.png)

Bu yöntemlerden herhangi birini kullanarak, HDInsight kümenizi birkaç dakika içinde yukarı veya aşağı ölçeklendirebilirsiniz.

> [!IMPORTANT]  
> * Azure klasik CLI'si amortismana hazırdır ve yalnızca klasik dağıtım modeliyle kullanılmalıdır. Diğer tüm dağıtımlar için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanın.
> * PowerShell AzureRM modülü amortismana dahil edildi.  Lütfen mümkün olduğunca [Az modüllerini](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) kullanın.

## <a name="impact-of-scaling-operations"></a>Ölçekleme işlemlerinin etkisi

Çalışan HDInsight kümenize düğüm **eklediğinizde** (ölçeklendirin), işler etkilenmez. Ölçekleme işlemi çalışırken yeni işler güvenli bir şekilde gönderilebilir. Ölçekleme işlemi başarısız olursa, hata kümenizi işlevsel bir durumda bırakır.

Düğümleri **kaldırırsanız** (küçültün), ölçeklendirme işlemi tamamlandığında bekleyen veya çalışan işler başarısız olur. Bu hata, ölçekleme işlemi sırasında yeniden başlayan bazı hizmetlerden kaynaklanır. Kümeniz, el ile ölçekleme işlemi sırasında güvenli modda sıkışıp kalabilir.

HDInsight tarafından desteklenen her küme türü için veri düğümlerinin sayısını değiştirmenin etkisi değişir:

* Apache Hadoop

    Çalışan bir Hadoop kümesindeki işçi düğümlerinin sayısını, herhangi bir işi etkilemeden sorunsuz bir şekilde artırabilirsiniz. Operasyon devam ederken yeni işler de sunulabilir. Ölçeklendirme işlemindeki hatalar incelikle işlenir. Küme her zaman işlevsel bir durumda bırakılır.

    Hadoop kümesi daha az veri düğümüyle küçültüldüğünde, bazı hizmetler yeniden başlatılır. Bu davranış, ölçekleme işlemi tamamlandığında tüm çalışan ve bekleyen işlerin başarısız olmasını neden olur. Ancak, işlem tamamlandıktan sonra işleri yeniden gönderebilirsiniz.

* Apache HBase

    Çalışırken HBase kümenize sorunsuz bir şekilde düğüm ekleyebilir veya kaldırabilirsiniz. Bölgesel Sunucular, ölçekleme işlemini tamamladıktan sonra birkaç dakika içinde otomatik olarak dengelenir. Ancak, bölgesel sunucuları el ile dengeleyebilirsiniz. Küme başlığına giriş yapın ve aşağıdaki komutları çalıştırın:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    HBase kabuğunu kullanma hakkında daha fazla bilgi için [HDInsight'ta Apache HBase örneğine başlayın' a](hbase/apache-hbase-tutorial-get-started-linux.md)bakın.

* Apache Storm

    Storm çalışırken veri düğümlerini sorunsuz bir şekilde ekleyebilir veya kaldırabilirsiniz. Ancak, ölçekleme işleminin başarıyla tamamlanmasından sonra, topolojiyi yeniden dengelemeniz gerekir.

    Yeniden dengeleme iki şekilde gerçekleştirilebilir:

  * Fırtına web UI
  * Komut satırı arabirimi (CLI) aracı

    Daha fazla bilgi için [Apache Storm belgelerine](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)bakın.

    Storm web UI HDInsight kümesinde kullanılabilir:

    ![HDInsight Fırtına ölçeği yeniden dengeleme](./media/hdinsight-scaling-best-practices/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Fırtına topolojisini yeniden dengelemek için örnek bir CLI komutu aşağıda verilmiştir:

    ```console
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

## <a name="how-to-safely-scale-down-a-cluster"></a>Bir kümeyi güvenli bir şekilde küçültme

### <a name="scale-down-a-cluster-with-running-jobs"></a>Çalışan işlerde kümeyi küçültme

Çalışan işlerinizin küçültme işlemi sırasında başarısız olmasını önlemek için üç şey deneyebilirsiniz:

1. Kümenizi ölçeklemeden önce işlerin tamamlanmasını bekleyin.
1. İşleri el ile bitirin.
1. Ölçekleme işlemi tamamlandıktan sonra işleri yeniden gönderin.

Bekleyen ve çalışan işlerin listesini görmek için aşağıdaki adımları izleyerek İplik **Kaynak Yöneticisi UI'yi**kullanabilirsiniz:

1. Azure [portalından](https://portal.azure.com/)kümenizi seçin.  Bkz. Yönergeler için [liste ve gösteri kümeleri.](./hdinsight-administer-use-portal-linux.md#showClusters) Küme yeni bir portal sayfasında açılır.
2. Ana görünümden,**Ambari ana kümesi** **panolarına** > gidin. Küme kimlik bilgilerinizi girin.
3. Ambari UI'den, sol menüdeki hizmetler listesinde **İplik'i** seçin.  
4. İPLik sayfasından **Hızlı Bağlantılar'ı** seçin ve etkin kafa düğümünün üzerine gidin, ardından **Kaynak Yöneticisi UI'yi**seçin.

    ![Apache Ambari hızlı bağlantılar Kaynak Yöneticisi UI](./media/hdinsight-scaling-best-practices/resource-manager-ui1.png)

Kaynak Yöneticisi UI'ye doğrudan `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`erişebilirsiniz.

Mevcut durumlarıyla birlikte bir iş listesi görüyorsunuz. Ekran görüntüsünde, şu anda çalışan bir iş var:

![Kaynak Yöneticisi UI uygulamaları](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Çalışan uygulamayı el ile öldürmek için SSH kabuğundan aşağıdaki komutu uygulayın:

```bash
yarn application -kill <application_id>
```

Örneğin:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Güvenli modda sıkışıp kalma

Bir kümeyi küçültttüğettiğinizde, HDInsight, fazladan çalışan düğümlerini devre dışı bırakmak için Önce Apache Ambari yönetim arabirimlerini kullanır. Düğümler HDFS bloklarını diğer çevrimiçi alt düğümlere kopyalar. Bundan sonra, HDInsight kümeyi güvenli bir şekilde küçülter. HDFS ölçekleme işlemi sırasında güvenli moda gider. ÖLÇEKLEME BITTIKTEN SONRA HDFS'nin çıkması gerekiyor. Ancak bazı durumlarda, HDFS dosya bloğu altında çoğaltma nedeniyle ölçekleme işlemi sırasında güvenli modda sıkışıp kalır.

Varsayılan olarak, HDFS, her `dfs.replication` dosya bloğunun kaç kopyasının kullanılabilir olduğunu denetleyen 1 ayarı ile yapılandırılır. Dosya bloğunun her kopyası kümenin farklı bir düğümünde depolanır.

Beklenen blok kopya sayısı kullanılamıyorsa, HDFS güvenli moduna girer ve Ambari uyarılar üretir. HDFS ölçekleme işlemi için güvenli mod girebilir. Çoğaltma için gerekli sayıda düğüm algılanmamışsa küme güvenli modda sıkışıp kalabilir.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Güvenli mod açık olduğunda örnek hatalar

```output
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```output
org.apache.http.conn.HttpHostConnectException: Connect to active-headnode-name.servername.internal.cloudapp.net:10001 [active-headnode-name.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Kümenin güvenli moda ne zaman `/var/log/hadoop/hdfs/` girdiğini görmek için kümenin ölçeklendirildiğinde, klasörden ad düğümü günlüklerini gözden geçirebilirsiniz. Günlük dosyaları . `Hadoop-hdfs-namenode-<active-headnode-name>.*`

Temel nedeni Hive sorguları çalıştırırken HDFS geçici dosyalara bağlıdır. HDFS güvenli moduna girdiğinde, Hive HDFS'ye yazamadığı için sorguları çalıştıramaz. HDFS'deki geçici dosyalar, tek tek işçi düğümü VM'lere monte edilmiş yerel sürücüde bulunur. Dosyalar, en az üç yinelemede diğer alt düğümler arasında çoğaltılır.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>HDInsight'ın güvenli modda sıkışıp kalması nasıl engellenir?

HDInsight'ın güvenli modda bırakılmasını önlemenin birkaç yolu vardır:

* HDInsight'ı ölçeklemeden önce tüm Hive işlerini durdurun. Alternatif olarak, çalışan Hive işleri ile çakışan önlemek için ölçek aşağı işlemi zamanlayın.
* Aşağı ölçekleme önce HDFS Hive'S scratch `tmp` directory dosyaları el ile temizleyin.
* HDInsight'ı yalnızca en az üç alt düğüme küçültün. Tek bir işçi düğümü kadar düşük gitmekten kaçının.
* Gerekirse güvenli mod bırakmak için komutu çalıştırın.

Aşağıdaki bölümlerde bu seçenekleri açıklayınız.

#### <a name="stop-all-hive-jobs"></a>Tüm Kovan işlerini durdurun

Tek bir işçi düğümüne kadar ölçeklemeden önce tüm Hive işlerini durdurun. İş yükünüz zamanlanmışsa, Hive işi bittikten sonra küçültün.

Ölçeklemeden önce Kovan işlerini durdurma, tmp klasöründeki çizilme dosyalarının sayısını en aza indirmeye yardımcı olur (varsa).

#### <a name="manually-clean-up-hives-scratch-files"></a>Hive'ın karalama dosyalarını el ile temizleyin

Hive geçici dosyaları geride bıraktıysa, güvenli moddan kaçınmak için bu dosyaları ölçeklemeden önce el ile temizleyebilirsiniz.

1. Yapılandırma özelliğine `hive.exec.scratchdir` bakarak Kovan geçici dosyaları için hangi konumun kullanıldığını denetleyin. Bu parametre içinde `/etc/hive/conf/hive-site.xml`ayarlanır:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Kovan hizmetlerini durdurun ve tüm sorguların ve işlerin tamamlandığından emin olun.

1. Herhangi bir dosya bulunıp içermeden görmek `hdfs://mycluster/tmp/hive/` için yukarıda bulunan karalama dizininin içeriğini listeleyin:

    ```bash
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Dosyalar var olduğunda örnek çıktı aşağıda veda edebilirsiniz:

    ```output
    sshuser@scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

1. Hive'ın bu dosyalarla yapıldığını biliyorsanız, bunları kaldırabilirsiniz. Hive'ın İplik Kaynak Yöneticisi UI sayfasına bakarak çalışan herhangi bir sorgusu olmadığından emin olun.

    Dosyaları HDFS'den kaldırmak için örnek komut satırı:

    ```bash
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>HDInsight'ı üç veya daha fazla alt düğüme ölçeklendirin

Kümeleriniz üçten az altdüğüme küçültülürken sık sık güvenli modda sıkışıp kalıyorsa, en az üç alt düğüm tutun.

Üç işçi düğümüolması, yalnızca bir işçi düğümüne ölçekleme yapmaktan daha pahalıdır. Ancak, bu eylem kümenizin güvenli modda sıkışıp kalmalarını önler.

### <a name="scale-hdinsight-down-to-one-worker-node"></a>HDInsight'ı tek bir çalışan düğümüne küçült

Küme tek bir düğüme küçültülse bile, işçi düğümü 0 yine de hayatta kalır. İşçi düğümü 0 asla kullanımdan kaldırılamaz.

#### <a name="run-the-command-to-leave-safe-mode"></a>Güvenli mod bırakmak için komutu çalıştırın

Son seçenek, güvenli mod komutunu çalıştırmaktır. HDFS, Kovan dosyasının altında çoğaltma nedeniyle güvenli mod girdiyse, güvenli modbırakmak için aşağıdaki komutu uygulayın:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Apache HBase kümesini küçültme

Bölge sunucuları, ölçekleme işlemini tamamladıktan sonra birkaç dakika içinde otomatik olarak dengelenir. Bölge sunucularını el ile dengelemek için aşağıdaki adımları tamamlayın:

1. SSH'yi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. HBase kabuğunu başlatın:

    ```bash
    hbase shell
    ```

3. Bölge sunucularını el ile dengelemek için aşağıdaki komutu kullanın:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight kümelerini otomatik olarak ölçeklendirin](hdinsight-autoscale-clusters.md)
* [Azure HDInsight'a Giriş](hadoop/apache-hadoop-introduction.md)
