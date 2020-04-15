---
title: Hadoop için yüksek kullanılabilirlik - Azure HDInsight
description: HDInsight kümelerinin ek bir kafa düğümü kullanarak güvenilirliği ve kullanılabilirliği nasıl artırdığını öğrenin. Bunun Ambari ve Hive gibi Hadoop hizmetlerini nasıl etkilediğini ve SSH kullanarak her baş düğümüne tek tek nasıl bağlanabileceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
keywords: hadoop yüksek kullanılabilirlik
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 10/28/2019
ms.openlocfilehash: 767d87efcf94d720159dcf3b9dc42981ec957ef0
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381405"
---
# <a name="availability-and-reliability-of-apache-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği

HDInsight kümeleri, Apache Hadoop hizmetlerinin ve çalışan işlerin kullanılabilirliğini ve güvenilirliğini artırmak için iki kafa düğümü sağlar.

Hadoop, bir kümedeki birden çok düğüm üzerinden hizmetleri ve verileri çoğaltarak yüksek kullanılabilirlik ve güvenilirlik elde eder. Ancak Hadoop standart dağılımları genellikle sadece tek bir baş düğümü var. Tek kafa düğümündeki herhangi bir kesinti kümenin çalışmasını durdurmasına neden olabilir. HDInsight, Hadoop'un kullanılabilirliğini ve güvenilirliğini artırmak için iki başlık sağlar.

## <a name="availability-and-reliability-of-nodes"></a>Düğümlerin kullanılabilirliği ve güvenilirliği

HDInsight kümesindeki düğümler Azure Sanal Makineleri kullanılarak uygulanır. Aşağıdaki bölümlerde HDInsight ile kullanılan tek tek düğüm türleri tartışılır.

> [!NOTE]  
> Küme türü için tüm düğüm türleri kullanılmaz. Örneğin, Hadoop küme türünde Nimbus düğümleri yoktur. HDInsight küme türleri tarafından kullanılan düğümler hakkında daha fazla bilgi için, [HDInsight belgesinde Linux tabanlı Hadoop kümeleri oluştur'un](hdinsight-hadoop-provision-linux-clusters.md#cluster-type) Küme türleri bölümüne bakın.

### <a name="head-nodes"></a>Kafa düğümleri

Hadoop hizmetlerinin yüksek kullanılabilirliğini sağlamak için HDInsight iki baş düğümü sağlar. Her iki kafa düğümü de etkindir ve HDInsight kümesi içinde aynı anda çalışır. Apache HDFS veya Apache Hadoop YARN gibi bazı hizmetler, herhangi bir anda tek bir baş düğümüüzerinde yalnızca 'etkin' dir. HiveServer2 veya Hive MetaStore gibi diğer hizmetler aynı anda her iki baş düğümlerinde de etkindir.

Kümenizdeki farklı düğüm türleri için ana bilgisayar adlarını elde etmek için lütfen [Ambari REST API'sini](hdinsight-hadoop-manage-ambari-rest-api.md#get-the-fqdn-of-cluster-nodes)kullanın.

> [!IMPORTANT]  
> Sayısal değeri düğümün birincil mi yoksa ikincil mi olduğuyla ilişkilendirme. Sayısal değer yalnızca her düğüm için benzersiz bir ad sağlamak için bulunur.

### <a name="nimbus-nodes"></a>Nimbus Düğümleri

Nimbus düğümleri Apache Storm kümeleri ile kullanılabilir. Nimbus düğümleri, işçi düğümleri arasında işlem dağıtarak ve izleyerek Hadoop JobTracker'a benzer işlevsellik sağlar. HDInsight, Fırtına kümeleri için iki Nimbus düğümü sağlar

### <a name="apache-zookeeper-nodes"></a>Apache Zookeeper düğümleri

[ZooKeeper](https://zookeeper.apache.org/) düğümleri baş düğümleri üzerinde ana hizmetlerin lider seçimi için kullanılır. Ayrıca, hizmetlerin, verilerin (alt) düğümlerinin ve ağ geçitlerinin ana hizmetin hangi baş düğümünde etkin olduğunu bilmesini sağlamak için de kullanılırlar. Varsayılan olarak, HDInsight üç ZooKeeper düğümü sağlar.

### <a name="worker-nodes"></a>İşçi düğümleri

Bir iş kümeye gönderildiğinde, alt düğümler gerçek veri çözümlemesi gerçekleştirir. Bir alt düğüm başarısız olursa, gerçekleştirdiği görev başka bir alt düğüme gönderilir. Varsayılan olarak, HDInsight dört alt düğüm oluşturur. Bu sayıyı küme oluşturma sırasında ve sonrasında ihtiyaçlarınıza uyacak şekilde değiştirebilirsiniz.

### <a name="edge-node"></a>kenar düğümü

Kenar düğümü küme içindeki veri çözümlemesi etkin olarak katılmaz. Hadoop ile çalışırken geliştiriciler veya veri bilimciler tarafından kullanılır. Kenar düğümü kümedeki diğer düğümlerle aynı Azure Sanal Ağ'da yaşar ve diğer düğümlere doğrudan erişebilir. Kenar düğümü, kaynakları kritik Hadoop hizmetlerinden veya analiz işlerinden uzağa götürmeden kullanılabilir.

Şu anda, HDInsight'taki ML Services varsayılan olarak bir kenar düğümü sağlayan tek küme türüdür. HDInsight'taki ML Hizmetleri için kenar düğümü, dağıtılmış işleme için kümeye göndermeden önce düğümüzerinde yerel olarak test R kodu kullanılır.

Diğer küme türleri ile bir kenar düğümü kullanma hakkında bilgi için [HDInsight belgesinde Kullanım kenarı düğümlerine](hdinsight-apps-use-edge-node.md) bakın.

## <a name="accessing-the-nodes"></a>Düğümlere erişim

Kümeye internet üzerinden erişim genel bir ağ geçidi aracılığıyla sağlanır. Erişim, kafa düğümlerine ve varsa kenar düğümüne bağlanmakla sınırlıdır. Kafa düğümlerinde çalışan hizmetlere erişim, birden çok kafa düğümünden etkilenmez. Ortak ağ geçidi isteklerini istenen hizmeti barındıran baş düğüme yönlendirir. Örneğin, Apache Ambari şu anda ikincil baş düğümünde barındırılan varsa, ambari için gelen istekleri bu düğüme ağ geçidi yönlendirir.

Ortak ağ geçidi üzerinden erişim 443 (HTTPS), 22 ve 23 bağlantı noktalarıyla sınırlıdır.

|Bağlantı noktası |Açıklama |
|---|---|
|443|Ambari ve diğer web UI veya REST API'lerine erişmek için kullanılır.|
|22|SSH ile birincil baş düğümü ne de kenar düğümüne erişmek için kullanılır.|
|23|SSH ile ikincil kafa düğümüne erişmek için kullanılır. Örneğin, `ssh username@mycluster-ssh.azurehdinsight.net` **mycluster**adlı kümenin birincil baş düğümüne bağlanır.|

SSH kullanımı hakkında daha fazla bilgi için [HDInsight belgeli SSH'yi kullanın.](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Dahili tam nitelikli alan adları (FQDN)

HDInsight kümesindeki düğümlerin yalnızca kümeden erişilebilen dahili bir IP adresi ve FQDN'si vardır. Dahili FQDN veya IP adresini kullanarak kümedeki hizmetlere erişirken, hizmete erişirken kullanılacak IP veya FQDN'yi doğrulamak için Ambari'yi kullanmalısınız.

Örneğin, Apache Oozie hizmeti yalnızca bir baş düğümünde çalıştırılabilir ve bir SSH oturumundaki `oozie` komutu kullanarak hizmetin URL'sini gerektirir. Bu URL Ambari'den aşağıdaki komutu kullanarak alınabilir:

```bash
export password='PASSWORD'
export clusterName="CLUSTERNAME"

curl -u admin:$password "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url
```

Bu komut, `oozie` komutla birlikte kullanılacak dahili URL'yi içeren aşağıdakilere benzer bir değer döndürür:

```output
"oozie.base.url": "http://<ACTIVE-HEADNODE-NAME>cx.internal.cloudapp.net:11000/oozie"
```

Ambari REST API ile çalışma hakkında daha fazla bilgi için, [Apache Ambari REST API'yi kullanarak HDInsight'ı Izleyin ve Yönetin.](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="accessing-other-node-types"></a>Diğer düğüm türlerine erişim

Aşağıdaki yöntemleri kullanarak internet üzerinden doğrudan erişilemeyen düğümlere bağlanabilirsiniz:

|Yöntem |Açıklama |
|---|---|
|SSH|SSH kullanarak bir baş düğümüne bağlandıktan sonra, kümedeki diğer düğümlere bağlanmak için kafa düğümünden SSH'yi kullanabilirsiniz. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.|
|SSH Tüneli|Internet'e maruz kalmayan düğümlerden birinde barındırılan bir web hizmetine erişmeniz gerekiyorsa, bir SSH tüneli kullanmanız gerekir. Daha fazla bilgi için [HDInsight belgesine sahip bir SSH tüneli kullanın](hdinsight-linux-ambari-ssh-tunnel.md) bölümüne bakın.|
|Azure Sanal Ağ|HDInsight kümeniz bir Azure Sanal Ağı'nın parçasıysa, aynı Sanal Ağdaki herhangi bir kaynak kümedeki tüm düğümlere doğrudan erişebilir. Daha fazla bilgi için [HDInsight](hdinsight-plan-virtual-network-deployment.md) belgesi için sanal ağ Planı'na bakın.|

## <a name="how-to-check-on-a-service-status"></a>Hizmet durumunu denetleme

Baş düğümlerinde çalışan hizmetlerin durumunu kontrol etmek için Ambari Web UI veya Ambari REST API'yi kullanın.

### <a name="ambari-web-ui"></a>Ambari Web UI

Ambari Web UI'de `https://CLUSTERNAME.azurehdinsight.net`görüntülenebilir. **CLUSTERNAME'yi** kümenizin adı ile değiştirin. İstenirse, kümeniz in http kullanıcı kimlik bilgilerini girin. Varsayılan HTTP kullanıcı adı **yöneticidir** ve parola küme oluştururken girdiğiniz paroladır.

Ambari sayfasına vardığınızda, yüklenen hizmetler sayfanın solunda listelenir.

![Apache Ambari yüklü hizmetler](./media/hdinsight-high-availability-linux/hdinsight-installed-services.png)

Durumu belirtmek için bir hizmetin yanında görünebilecek bir dizi simge vardır. Bir hizmetle ilgili tüm uyarılar sayfanın üst kısmındaki **Uyarılar** bağlantısını kullanarak görüntülenebilir.  Ambari birkaç önceden tanımlanmış uyarılar sunuyor.

Aşağıdaki uyarılar, bir kümenin kullanılabilirliğini izlemeye yardımcı olur:

| Uyarı Adı                               | Açıklama                                                                                                                                                                                  |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Metrik Monitör Durumu                    | Bu uyarı, monitör durum komut dosyası tarafından belirlenen Ölçümler İzleyici işleminin durumunu gösterir.                                                                                   |
| Ambari Ajan Kalp Atışı                   | Sunucu bir aracıyla teması kaybetmişse, bu uyarı tetiklenir.                                                                                                                        |
| ZooKeeper Sunucu Süreci                 | ZooKeeper sunucu işleminin ağda dolup tarayıp dinlediği belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                               |
| IOCache Metadata Sunucu Durumu           | IOCache Metadata Server'ın açıktı ve istemci isteklerine yanıt veremezse bu ana bilgisayar düzeyinde uyarı tetiklenir                                                            |
| JournalNode Web UI                       | JournalNode Web UI'ye erişilemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                                 |
| Spark2 Thrift Sunucusu                     | Spark2 Thrift Server'ın açık olduğu belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                |
| Geçmiş Sunucu Süreci                   | Geçmiş Sunucu işlemi ağda dolup taşıyorsa, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                |
| Geçmiş Sunucu Web Web Web UI                    | Geçmiş Sunucu Web UI'ye erişilemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                              |
| `ResourceManager`Web UI                   | Web UI'ye erişilemezse, `ResourceManager` bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                             |
| NodeManager Sistem Durumu Özeti               | Sağlıksız NodeManagers varsa bu hizmet düzeyi uyarısı tetiklenir                                                                                                                    |
| Uygulama Zaman Çizelgesi Web Web Web UI                      | App Timeline Server Web UI'ye erişilemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                         |
| DataNode Sağlık Özeti                  | Sağlıksız DataNodes varsa bu hizmet düzeyi uyarısı tetiklenir                                                                                                                       |
| NameNode Web Kullanıcı Arabirimi                          | NameNode Web Kullanıcı Arabirimi erişilemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                                    |
| ZooKeeper Failover Denetleyici Süreci    | ZooKeeper Failover Controller işleminin ağda dinleyip dinlediği doğrulanamazsa, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                   |
| Oozie Server Web UI                      | Oozie sunucusu Web UI'ye erişilemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                                |
| Oozie Sunucu Durumu                      | Oozie sunucusu nun istemci isteklerine yanıt vereme konusunda belirlenemediyse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                      |
| Kovan Metastore Süreci                   | Hive Metastore işlemi ağüzerinde dinleniyor ve yukarı sınanacak şekilde belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                 |
| HiveServer2 Süreci                      | HiveServer'ın istemci isteklerine yanıt vereme konusunda belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                        |
| WebHCat Sunucu Durumu                    | `templeton` Sunucu durumu sağlıklı değilse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                            |
| Yüzde Zookeeper Sunucular Kullanılabilir      | Kümedeki aşağı ZooKeeper sunucularının sayısı yapılandırılan kritik eşikten büyükse bu uyarı tetiklenir. ZooKeeper işlem kontrollerinin sonuçlarını toplar.     |
| Spark2 Livy Sunucu                       | Livy2 Sunucusu'nun hazır olduğu belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                        |
| Spark2 Tarih Sunucusu                    | Spark2 History Server'ın yukarı olduğu belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                               |
| Ölçümler Kollektör Süreci                | Bu uyarı, Ölçümler Toplayıcı'nın eşiğe eşit saniye sayısı için yapılandırılmış bağlantı noktasında dolandırılan ve dinleyen metrikler için onaylanamazsa tetiklenir.                                 |
| Metrik Toplayıcı - HBase Master Process | Bu uyarı, Ölçümler Toplayıcısı'nın HBase ana işlemlerinin saniyeler içinde verilen yapılandırılmış kritik eşik için ağda dolandırılan ve dinleyen olduğu doğrulanamazsa tetiklenir. |
| Yüzde ÖlçümlerI Mevcut Monitörler       | Bu uyarı, yapılandırılmış uyarı ve kritik eşikler için Ağ'da Ölçüm İzleyici işlemlerinin bir yüzdesi dinlenmediyse ve ağda dinlemiyorsa tetiklenir.                             |
| Yüzde NodeManagers Kullanılabilir           | Kümedeki down NodeManagers sayısı yapılandırılmış kritik eşiğe göre büyükse, bu uyarı tetiklenir. NodeManager işlem kontrollerinin sonuçlarını toplar.        |
| DüğümYönetici Sağlık                       | Bu ana bilgisayar düzeyinde uyarı, NodeManager bileşeninden kullanılabilen düğüm sistem durumu özelliğini denetler.                                                                                              |
| NodeManager Web UI                       | Bu ana bilgisayar düzeyinde uyarı, NodeManager Web UI'ye erişilemezse tetiklenir.                                                                                                                 |
| NameNode Yüksek Kullanılabilirlik Sağlık        | Etkin NameNode veya Bekleme NameNode çalışmıyorsa bu hizmet düzeyi uyarısı tetiklenir.                                                                                     |
| DataNode Süreci                         | Tek tek DataNode işlemleri ağüzerinde dinlenecek şekilde kurulamazsa, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                         |
| DataNode Web UI                          | DataNode Web UI'ye ulaşılamazsa, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                                                                    |
| Yüzde JournalNodes Kullanılabilir           | Kümedeki aşağı JournalNodes sayısı yapılandırılmış kritik eşikten büyükse bu uyarı tetiklenir. JournalNode işlem kontrollerinin sonuçlarını toplar.        |
| Mevcut Yüzde DataNodes              | Kümedeki aşağı DataNode sayısı yapılandırılan kritik eşikten büyükse bu uyarı tetiklenir. DataNode işlem kontrollerinin sonuçlarını toplar.              |
| Zeppelin Sunucu Durumu                   | Zeppelin sunucusunun istemci isteklerine yanıt vereme konusunda belirlenemediyse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                                   |
| HiveServer2 İnteraktif İşlem          | HiveServerInteractive'in istemci isteklerine yanıt vereme nedeni belirlenemezse, bu ana bilgisayar düzeyinde uyarı tetiklenir.                                                             |
| LLAP Uygulaması                         | LLAP Uygulaması nın yukarı ve isteklere yanıt vermeye kararlı değilse bu uyarı tetiklenir.                                                                                    |

Daha fazla bilgi görüntülemek için her hizmeti seçebilirsiniz.

Hizmet sayfası her hizmetin durumu ve yapılandırması hakkında bilgi sağlarken, hizmetin hangi kafa düğümünde çalıştığını zedemez. Bu bilgileri görüntülemek için sayfanın üst kısmındaki **Ana Bilgisayarlar** bağlantısını kullanın. Bu sayfa, kafa düğümleri de dahil olmak üzere küme içindeki ana bilgisayarları görüntüler.

![Apache Ambari headnode hosts listesi](./media/hdinsight-high-availability-linux/hdinsight-hosts-list.png)

Kafa düğümlerinden biri için bağlantıyı seçmek, bu düğümüzerinde çalışan hizmetleri ve bileşenleri görüntüler.

![Apache Ambari bileşen durumu](./media/hdinsight-high-availability-linux/hdinsight-node-services.png)

Ambari'yi kullanma hakkında daha fazla bilgi için, [Apache Ambari Web UI'yi kullanarak HDInsight'ı izleyin ve yönetin.](hdinsight-hadoop-manage-ambari.md)

### <a name="ambari-rest-api"></a>Ambari REST API

Ambari REST API internet üzerinden kullanılabilir. HDInsight genel ağ geçidi, şu anda REST API'ye ev sahipliği yapan baş düğümüne yönlendirme isteklerini işler.

Ambari REST API aracılığıyla bir hizmetin durumunu denetlemek için aşağıdaki komutu kullanabilirsiniz:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state
```

* **PAROLA'yı** HTTP kullanıcı (yönetici) hesap parolasıile değiştirin.
* **CLUSTERNAME** değerini kümenin adıyla değiştirin.
* **SERVICENAME'i** durumunu denetlemek istediğiniz hizmetin adı ile değiştirin.

Örneğin, **mycluster**adlı bir kümedeki **HDFS** hizmetinin durumunu denetlemek için **, parola**ile, aşağıdaki komutu kullanırsınız:

```bash
curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state
```

Yanıt aşağıdaki JSON benzer:

```json
{
    "href" : "http://mycluster.wutj3h4ic1zejluqhxzvckxq0g.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
    "ServiceInfo" : {
    "cluster_name" : "mycluster",
    "service_name" : "HDFS",
    "state" : "STARTED"
    }
}
```

URL hizmet şu anda **mycluster.wutj3h4ic1zejluqhxzvckxq0g**adlı bir baş düğüm üzerinde çalışıyor söyler.

Durum, hizmetin şu anda çalıştığını veya **BAŞLATİLTIĞUNU**söyler.

Kümeye hangi hizmetlerin yüklenmiş olduğunu bilmiyorsanız, bir liste almak için aşağıdaki komutu kullanabilirsiniz:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services
```

Ambari REST API ile çalışma hakkında daha fazla bilgi için, [Apache Ambari REST API'yi kullanarak HDInsight'ı Izleyin ve Yönetin.](hdinsight-hadoop-manage-ambari-rest-api.md)

#### <a name="service-components"></a>Servis bileşenleri

Hizmetler, durumunu tek tek denetlemek istediğiniz bileşenler içerebilir. Örneğin, HDFS NameNode bileşenini içerir. Bir bileşendeki bilgileri görüntülemek için komut şu şekilde olacaktır:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component
```

Bir hizmet tarafından hangi bileşenlerin sağlandığını bilmiyorsanız, bir liste almak için aşağıdaki komutu kullanabilirsiniz:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component
```

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Kafa düğümlerinde günlük dosyalarına nasıl erişilir?

### <a name="ssh"></a>SSH

SSH aracılığıyla bir kafa düğümüne bağlanırken, **/var/log**altında günlük dosyaları bulunabilir. Örneğin, **/var/log/hadoop-iplik/iplik,** İplik için günlükler içerir.

Her kafa düğümübenzersiz günlük girişleri ne olabilir, bu nedenle her iki sinin de günlüklerini kontrol etmelisiniz.

### <a name="sftp"></a>SFTP

Ayrıca SSH Dosya Aktarım Protokolü veya Güvenli Dosya Aktarım Protokolü (SFTP) kullanarak baş düğümüne bağlanabilir ve günlük dosyalarını doğrudan indirebilirsiniz.

Bir SSH istemcisi kullanmaya benzer şekilde, kümeye bağlanırken SSH kullanıcı hesabı adını ve kümenin SSH adresini sağlamanız gerekir. Örneğin, `sftp username@mycluster-ssh.azurehdinsight.net`. İstendiğinde hesabın parolasını sağlayın veya parametreyi `-i` kullanarak ortak bir anahtar sağlayın.

Bağlandıktan sonra, bir `sftp>` istem ile sunulur. Bu komut isteminden dizinleri değiştirebilir, dosya yükleyebilir ve indirebilirsiniz. Örneğin, aşağıdaki komutlar dizinleri **/var/log/hadoop/hdfs** dizinine değiştirir ve ardından dizindeki tüm dosyaları karşıdan yüksünü karşıdan yüklez.

    cd /var/log/hadoop/hdfs
    get *

Kullanılabilir komutların listesi için `help` `sftp>` istek te girin.

> [!NOTE]  
> SFTP kullanarak bağlandığında dosya sistemini görselleştirmenize olanak tanıyan grafik arabirimler de vardır. Örneğin, [MobaXTerm,](https://mobaxterm.mobatek.net/) Windows Gezgini'ne benzer bir arabirimi kullanarak dosya sistemine göz atmanızı sağlar.

### <a name="ambari"></a>Ambari

> [!NOTE]  
> Ambari kullanarak günlük dosyalarına erişmek için bir SSH tüneli kullanmanız gerekir. Tek tek hizmetlerin web arabirimleri Internet'te herkese açık değildir. Bir SSH tüneli kullanma hakkında bilgi için [SSH Tünel kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belgesini kullanın.

From the Ambari Web UI, select the service you wish to view logs for (for example, YARN). Ardından, günlükleri görüntülemek için hangi kafa düğümünün kullanılacağını seçmek için **Hızlı Bağlantılar'ı** kullanın.

![Günlükleri görüntülemek için hızlı bağlantılar kullanma](./media/hdinsight-high-availability-linux/quick-links-view-logs.png)

## <a name="how-to-configure-the-node-size"></a>Düğüm boyutu nasıl yapılandırılır?

Düğümün boyutu yalnızca küme oluşturma sırasında seçilebilir. [HDInsight fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/hdinsight/)HDInsight için mevcut olan farklı VM boyutlarının bir listesini bulabilirsiniz.

Bir küme oluştururken, düğümlerin boyutunu belirtebilirsiniz. Aşağıdaki bilgiler, [Azure portalı,](https://portal.azure.com/) [Azure PowerShell modülü Az](/powershell/azureps-cmdlets-docs)ve Azure [CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak boyutu nasıl belirteceklerine ilişkin kılavuz sağlar:

* **Azure portalı**: Küme oluştururken, küme tarafından kullanılan düğümlerin boyutunu ayarlayabilirsiniz:

    ![Düğüm boyutu seçimi ile küme oluşturma sihirbazı görüntüsü](./media/hdinsight-high-availability-linux/azure-portal-cluster-configuration-pricing-hadoop.png)

* **Azure CLI**: [`az hdinsight create`](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) Komutu kullanırken, baş, işçi ve ZooKeeper düğümlerinin boyutunu `--headnode-size`, `--workernode-size`ve `--zookeepernode-size` parametreleri kullanarak ayarlayabilirsiniz.

* **Azure PowerShell**: [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) cmdlet kullanırken, kafa, işçi ve ZooKeeper düğümlerinin boyutunu `-HeadNodeSize`, `-WorkerNodeSize`, `-ZookeeperNodeSize` ve parametreleri kullanarak ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tartışılan öğeler hakkında daha fazla bilgi edinmek için bkz:

* [Apache Ambari REST Referans](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Azure CLI'yi yükleme ve yapılandırma](https://docs.microsoft.com//cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure PowerShell modüllerini az yükleme ve yapılandırma](/powershell/azure/overview)
* [Apache Ambari'yi kullanarak HDInsight'ı yönetin](hdinsight-hadoop-manage-ambari.md)
* [Linux tabanlı HDInsight kümelerini sağlama](hdinsight-hadoop-provision-linux-clusters.md)
