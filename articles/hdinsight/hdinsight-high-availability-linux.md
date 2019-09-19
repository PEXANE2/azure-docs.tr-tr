---
title: Hadoop için yüksek kullanılabilirlik-Azure HDInsight
description: HDInsight kümelerinin ek bir baş düğüm kullanarak güvenilirliği ve kullanılabilirliği nasıl artıracağınızı öğrenin. Bunun, her bir baş düğüme tek tek nasıl bağlanacağınızı, örneğin, hem ambarı hem de Hive gibi Hadoop hizmetlerini nasıl etkilediği hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
keywords: Hadoop yüksek kullanılabilirlik
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: 615b1e4c5684084b6c5f88d26293b993c1efbf1f
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104418"
---
# <a name="availability-and-reliability-of-apache-hadoop-clusters-in-hdinsight"></a>HDInsight 'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği

HDInsight kümeleri, çalıştıran Apache Hadoop Hizmetleri ve işlerin kullanılabilirliğini ve güvenilirliğini artırmak için iki baş düğüm sağlar.

Hadoop, bir kümedeki birden çok düğümdeki Hizmetleri ve verileri çoğaltarak yüksek kullanılabilirlik ve güvenilirlik elde eder. Ancak, Hadoop 'un standart dağıtımları genellikle yalnızca tek bir baş düğüme sahiptir. Tek baş düğümden herhangi bir kesinti, kümenin çalışmayı durdurmasına neden olabilir. HDInsight, Hadoop 'un kullanılabilirliğini ve güvenilirliğini artırmak için iki headnode sağlar.

## <a name="availability-and-reliability-of-nodes"></a>Düğümlerin kullanılabilirliği ve güvenilirliği

HDInsight kümesindeki düğümler Azure sanal makineleri kullanılarak uygulanır. Aşağıdaki bölümlerde HDInsight ile kullanılan tek düğüm türleri ele alınmaktadır. 

> [!NOTE]  
> Küme türü için tüm düğüm türleri kullanılmaz. Örneğin, bir Hadoop kümesi türünün herhangi bir Nimbus düğümü yoktur. HDInsight küme türleri tarafından kullanılan düğümler hakkında daha fazla bilgi için bkz. [HDInsight 'Ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) ' nın küme türleri bölümü.

### <a name="head-nodes"></a>Baş düğümler

HDInsight, Hadoop Hizmetleri üzerinde yüksek kullanılabilirlik sağlamak için iki baş düğüm sağlar. Her iki baş düğüm de aynı anda HDInsight kümesi içinde etkin ve çalışır. Apache, veya Apache Hadoop YARN gibi bazı hizmetler, belirli bir zamanda yalnızca bir baş düğümde bulunan ' etkin' ' dir. HiveServer2 veya Hive meta veri deposu gibi diğer hizmetler aynı anda her iki baş düğümde de etkindir.

Baş düğümlerin (ve HDInsight 'taki diğer düğümlerin), düğümün ana bilgisayar adının bir parçası olarak sayısal bir değeri vardır. Örneğin, `hn0-CLUSTERNAME` veya `hn4-CLUSTERNAME`.

> [!IMPORTANT]  
> Sayısal değeri bir düğümün birincil veya ikincil olup olmadığı ile ilişkilendirmeyin. Sayısal değer yalnızca her düğüm için benzersiz bir ad sağlamak üzere mevcuttur.

### <a name="nimbus-nodes"></a>Nimbus Düğümleri

Nimbus düğümleri Apache Storm kümeleriyle kullanılabilir. Nimbus düğümleri, çalışan düğümleri arasında işlemeyi dağıtarak ve izleyerek Hadoop JobTracker 'e benzer işlevler sağlar. HDInsight, fırtınası kümeleri için iki Nimbus düğümü sağlar

### <a name="apache-zookeeper-nodes"></a>Apache Zookeeper düğümleri

[ZooKeeper](https://zookeeper.apache.org/) düğümleri, baş düğümlerdeki ana hizmetlerin öncü seçimi için kullanılır. Hizmetler, veri (çalışan) düğümlerinin ve ağ geçitlerinin hangi baş düğümü bir ana hizmetin etkin olduğunu bilmesini sağlamak için de kullanılır. HDInsight, varsayılan olarak üç ZooKeeper düğümü sağlar.

### <a name="worker-nodes"></a>Çalışan düğümleri

Çalışan düğümleri, kümeye bir iş gönderildiğinde gerçek veri analizini gerçekleştirir. Bir çalışan düğümü başarısız olursa, gerçekleştirdiği görev başka bir çalışan düğümüne gönderilir. HDInsight, varsayılan olarak dört çalışan düğümü oluşturur. Bu numarayı, küme oluşturma sırasında ve sonrasında gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

### <a name="edge-node"></a>Uç düğüm

Bir Edge düğümü, küme içindeki veri analizine etkin bir şekilde katılmaz. Hadoop ile çalışırken geliştiriciler veya veri bilimcileri tarafından kullanılır. Edge düğümü, kümedeki diğer düğümlerle aynı Azure sanal ağında yer alabilir ve diğer tüm düğümlere doğrudan erişebilir. Edge düğümü, önemli Hadoop hizmetlerinden veya analiz işlerinin dışında, kaynakları almadan kullanılabilir.

Şimdilik, HDInsight üzerindeki ML Hizmetleri varsayılan olarak bir kenar düğümü sağlayan tek küme türüdür. HDInsight üzerinde ML Hizmetleri için, Edge düğümü, dağıtılmış işleme için kümeye göndermeden önce düğüm üzerinde yerel olarak test R kodu kullanır.

Diğer küme türleriyle bir Edge düğümü kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Edge düğümlerini kullanma](hdinsight-apps-use-edge-node.md) belgesi.

## <a name="accessing-the-nodes"></a>Düğümlere erişme

Internet üzerinden kümeye erişim, genel bir ağ geçidi üzerinden sağlanır. Erişim, baş düğümlere ve (varsa) kenar düğümüne bağlanmak için sınırlıdır. Baş düğümlerde çalışan hizmetlere erişim, birden fazla baş düğüm olmadan etkilenmez. Ortak ağ geçidi, istekleri istenen hizmeti barındıran baş düğüme yönlendirir. Örneğin, Apache ambarı Şu anda ikincil baş düğümünde barındırılıyorsa, ağ geçidi, ambarı için gelen istekleri bu düğüme yönlendirir.

Ortak ağ geçidi üzerinden erişim bağlantı noktası 443 (HTTPS), 22 ve 23 ile sınırlıdır.

* __443__ numaralı bağlantı noktası, baş düğümlerde barındırılan ambarı ve diğer Web Kullanıcı ARABIRIMINE veya REST API 'lerine erişmek için kullanılır.

* Bağlantı noktası __22__ , birincil baş düğüme veya uç düğümüne SSH ile erişmek için kullanılır.

* Bağlantı noktası __23__ , ikincil baş düğümüne SSH ile erişmek için kullanılır. Örneğin, `ssh username@mycluster-ssh.azurehdinsight.net` **MyCluster**adlı kümenin birincil baş düğümüne bağlanır.

SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight Ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesi.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>İç tam etki alanı adları (FQDN)

HDInsight kümesindeki düğümlerin iç IP adresi ve yalnızca kümeden erişilebilen bir FQDN vardır. İç FQDN veya IP adresini kullanarak kümedeki hizmetlere erişirken, hizmete erişirken kullanılacak IP veya FQDN 'yi doğrulamak için ambarı 'nı kullanmanız gerekir.

Örneğin, Apache Oozie hizmeti yalnızca bir baş düğümde çalışabilir ve bir SSH oturumundan `oozie` komutun kullanılması hizmetin URL 'sini gerektirir. Bu URL, aşağıdaki komutu kullanarak ambarı 'ndan alınabilir:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Bu komut, `oozie` komutuyla birlikte kullanılacak iç URL 'yi içeren aşağıdaki komuta benzer bir değer döndürür:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

REST API ambarı ile çalışma hakkında daha fazla bilgi için bkz. [HDInsight 'ı Apache ambarı REST API kullanarak izleme ve yönetme](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Diğer düğüm türlerine erişme

Aşağıdaki yöntemleri kullanarak Internet üzerinden doğrudan erişilemeyen düğümlere bağlanabilirsiniz:

* **SSH**: SSH kullanarak bir baş düğüme bağlandıktan sonra, kümedeki diğer düğümlere bağlanmak için baş düğümden SSH kullanabilirsiniz. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

* **SSH tüneli**: İnternet 'e açık olmayan düğümlerden birinde barındırılan bir Web hizmetine erişmeniz gerekiyorsa, bir SSH tüneli kullanmanız gerekir. Daha fazla bilgi için [HDInsight Ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belgesine bakın.

* **Azure sanal ağı**: HDInsight kümeniz bir Azure sanal ağının parçasıysa, aynı sanal ağ üzerindeki herhangi bir kaynak kümedeki tüm düğümlere doğrudan erişebilir. Daha fazla bilgi için bkz. [HDInsight için sanal ağ planlaması](hdinsight-plan-virtual-network-deployment.md) belgesi.

## <a name="how-to-check-on-a-service-status"></a>Hizmet durumunu denetleme

Baş düğümlerde çalışan hizmetlerin durumunu denetlemek için, ambarı Web Kullanıcı arabirimi veya ambarı REST API kullanın.

### <a name="ambari-web-ui"></a>Ambari Web UI

Ambarı Web Kullanıcı arabirimi, ' de `https://CLUSTERNAME.azurehdinsight.net`görüntülenebilir. **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, kümenizin HTTP Kullanıcı kimlik bilgilerini girin. Varsayılan HTTP Kullanıcı adı **yönetici** ve parola, kümeyi oluştururken girdiğiniz paroladır.

Ambarı sayfasına geldiğinizde, yüklü hizmetler sayfanın solunda listelenir.

![Apache ambarı yüklü hizmetler](./media/hdinsight-high-availability-linux/hdinsight-installed-services.png)

Durumu göstermek için bir hizmetin yanında görünebilen bir dizi simge vardır. Bir hizmetle ilgili tüm uyarılar, sayfanın üst kısmındaki **Uyarılar** bağlantısı kullanılarak görüntülenebilir.  Ambarı önceden tanımlanmış birkaç uyarı sunar.

Aşağıdaki uyarılar bir kümenin kullanılabilirliğini izlemeye yardımcı olur:

| Uyarı Adı                               | Açıklama                                                                                                                                                                                  |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ölçüm Izleyici durumu                    | Bu uyarı, izleme durumu betiği tarafından belirlendiği şekilde ölçüm Izleyici işleminin durumunu gösterir.                                                                                   |
| Ambarı aracı sinyali                   | Bu uyarı, sunucuda bir aracıyla ilgili iletişim kesildiğinde tetiklenir.                                                                                                                        |
| ZooKeeper sunucusu Işlemi                 | Bu ana bilgisayar düzeyi uyarı, ZooKeeper sunucu işleminin ağa açık ve dinleme için belirlenemediği durumlarda tetiklenir.                                                               |
| IOCache meta veri sunucusu durumu           | IOCache meta veri sunucusunun istemci isteklerine yanıt verip vermediğini tespit edilemez durumunda bu konak düzeyi uyarısı tetiklenir                                                            |
| JournalNode Web Kullanıcı arabirimi                       | Bu konak düzeyi uyarısı, JournalNode Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                                 |
| Spark2 Thrift sunucusu                     | Bu konak düzeyi uyarısı, Spark2 Thrift sunucusu etkin olarak belirlenemiyorsa tetiklenir.                                                                                                |
| Geçmiş sunucu Işlemi                   | Bu konak düzeyi uyarısı, geçmiş sunucu işleminin ağ üzerinde ve dinlenecek şekilde kurulamazsa tetiklenir.                                                                |
| Geçmiş sunucusu Web Kullanıcı arabirimi                    | Bu konak düzeyi uyarı, geçmiş sunucusu Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                              |
| ResourceManager Web Kullanıcı arabirimi                   | ResourceManager Web Kullanıcı arabirimine ulaşılamıyorsa bu konak düzeyi uyarısı tetiklenir.                                                                                                             |
| NodeManager sistem durumu Özeti               | Sağlıksız NodeManager varsa, bu hizmet düzeyi uyarı tetiklenir                                                                                                                    |
| Uygulama zaman çizelgesi Web Kullanıcı arabirimi                      | Bu konak düzeyi uyarı, uygulama zaman çizelgesi sunucusu Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                         |
| Dadtanode sistem durumu Özeti                  | Sağlıksız bir kades varsa, bu hizmet düzeyi uyarı tetiklenir                                                                                                                       |
| Süs Code Web Kullanıcı arabirimi                          | Bu konak düzeyi uyarısı, süs Code Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                                    |
| ZooKeeper yük devretme denetleyicisi Işlemi    | Bu konak düzeyi uyarısı, ZooKeeper yük devretme denetleyicisi işleminin ağ üzerinde çalışıyor ve dinlemek için onaylanmazsa tetiklenir.                                                   |
| Oozie sunucusu Web Kullanıcı arabirimi                      | Bu konak düzeyi uyarı, Oozie sunucu Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                                |
| Oozie sunucu durumu                      | Bu konak düzeyi uyarısı, Oozie sunucusunun istemci isteklerine yanıt verip vermediğini tespit edilemez.                                                                      |
| Hive meta veri deposu Işlemi                   | Bu konak düzeyi uyarısı, Hive meta veri deposu işlemi, ağı dinleyerek ve dinlemeden saptanamıyor.                                                                 |
| HiveServer2 Işlemi                      | Bu konak düzeyi uyarısı, HiveServer 'ın istemci isteklerine yanıt verip vermeyeceğini tespit edilemez.                                                                        |
| WebHCat sunucu durumu                    | Bu konak düzeyi uyarısı, Templeton sunucu durumu sağlıklı değilse tetiklenir.                                                                                                            |
| Kullanılabilir ZooKeeper sunucusu yüzdesi      | Kümedeki aşağı ZooKeeper sunucularının sayısı yapılandırılan kritik eşikten büyükse bu uyarı tetiklenir. ZooKeeper işlem denetimlerinin sonuçlarını toplar.     |
| Spark2 Livy sunucusu                       | Bu konak düzeyi uyarısı, Livy2 sunucusu çalışır olarak belirlenemiyorsa tetiklenir.                                                                                                        |
| Spark2 geçmiş sunucusu                    | Bu ana bilgisayar düzeyi uyarı, Spark2 geçmiş sunucusunun olarak belirlenemediği takdirde tetiklenir.                                                                                               |
| Ölçüm toplayıcı Işlemi                | Bu uyarı, ölçüm toplayıcısının yapılandırılmış bağlantı noktasında eşiğe eşit olması onaylanmazsa tetiklenemez.                                 |
| Ölçüm toplayıcısı-HBase Master Işlemi | Bu uyarı, ölçüm toplayıcısının HBase ana işlemlerinin, saniye olarak verilen yapılandırılmış kritik eşik için ağ üzerinde çalışmaya ve dinlemek onaylanmazsa tetiklenir. |
| Kullanılabilir ölçüm Izleyicileri yüzdesi       | Bu uyarı, ölçüm Izleme işlemlerinin bir yüzdesi, yapılandırılan uyarı ve kritik eşiklere göre ağda dinlemeiyorsa tetiklenir.                             |
| Kullanılabilir düğüm yöneticileri yüzdesi           | Bu uyarı, kümedeki aşağı aşağı NodeManager sayısı yapılandırılan kritik eşikten fazlaysa tetiklenir. NodeManager işlem denetimlerinin sonuçlarını toplar.        |
| NodeManager sistem durumu                       | Bu konak düzeyinde uyarı, NodeManager bileşeninden kullanılabilen düğüm durumu özelliğini denetler.                                                                                              |
| NodeManager Web Kullanıcı arabirimi                       | NodeManager Web Kullanıcı arabirimine ulaşılamıyorsa bu konak düzeyi uyarısı tetiklenir.                                                                                                                 |
| Süs Yot yüksek kullanılabilirlik durumu        | Bu hizmet düzeyi uyarı, etkin bir süs Code veya standby süs ODE çalışmıyorsa tetiklenir.                                                                                     |
| Dadtanode Işlemi                         | Bu ana bilgisayar düzeyi uyarı, tek tek bir ağ üzerinde bir işlem ve ağ üzerinde dinleme yapmak için tek bir Davode işlemi kurulamazsa tetiklenir.                                                         |
| Dadtanode Web Kullanıcı arabirimi                          | Bu konak düzeyi uyarısı, Dadtanode Web Kullanıcı arabirimine ulaşılamıyorsa tetiklenir.                                                                                                                    |
| Kullanılabilir JournalNodes yüzdesi           | Bu uyarı, kümedeki aşağı yönelik JournalNodes sayısı yapılandırılan kritik eşikten fazlaysa tetiklenir. Bu, JournalNode işlem denetimlerinin sonuçlarını toplar.        |
| Kullanılabilir gün yüzdesi              | Bu uyarı, kümedeki aşağı doğru eşik sayısı yapılandırılan kritik eşikten büyükse tetiklenir. Bu, Davtanode işlem denetimlerinin sonuçlarını toplar.              |
| Zeppelin sunucu durumu                   | Zeppelin sunucusunun istemci isteklerine yanıt verip vermediğini tespit edilemez bu konak düzeyi uyarısı tetiklenir.                                                                   |
| HiveServer2 etkileşimli Işlemi          | Hiveserverınteractive 'in istemci isteklerine yanıt verip vermediğini tespit etmediği durumlarda bu konak düzeyi uyarısı tetiklenir.                                                             |
| LLAP uygulaması                         | Bu uyarı, LLAP uygulamasının isteklere yanıt verip vermeyeceğini tespit edilemez.                                                                                    |

Hakkında daha fazla bilgi görüntülemek için her bir hizmeti seçebilirsiniz.

Hizmet sayfası her bir hizmetin durumu ve yapılandırması hakkında bilgi sağladığından, hizmetin üzerinde çalıştığı baş düğüm hakkında bilgi sağlamaz. Bu bilgileri görüntülemek için sayfanın üst kısmındaki **konaklar** bağlantısını kullanın. Bu sayfa, baş düğümler de dahil olmak üzere küme içindeki Konakları görüntüler.

![Apache ambarı baş düğümüne ana bilgisayar listesi](./media/hdinsight-high-availability-linux/hdinsight-hosts-list.png)

Baş düğümlerden birine yönelik bağlantıyı seçmek o düğümde çalışan hizmetleri ve bileşenleri görüntüler.

![Apache ambarı bileşen durumu](./media/hdinsight-high-availability-linux/hdinsight-node-services.png)

Ambarı kullanma hakkında daha fazla bilgi için bkz. [Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight 'ı izleme ve yönetme](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>Ambarı REST API

REST API ambarı Internet üzerinden kullanılabilir. HDInsight genel ağ geçidi, yönlendirme isteklerini REST API Şu anda barındırmakta olan baş düğüme işler.

Bir hizmetin durumunu REST API ambarı aracılığıyla denetlemek için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* **Parolayı** http Kullanıcı (yönetici) hesabı parolasıyla değiştirin.
* **CLUSTERNAME** değerini kümenin adıyla değiştirin.
* **HizmetAdı** durumunu denetlemek istediğiniz hizmetin adıyla değiştirin.

Örneğin, **MyCluster**adlı bir kümede, bir **parola parolasıyla**, **Bu hizmetin durumunu** denetlemek için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

Yanıt aşağıdaki JSON ile benzerdir:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

URL, hizmetin **hn0-clustername**adlı bir baş düğümde çalışmakta olduğunu bize söyler.

Durum, hizmetin Şu anda çalıştığını veya **başlatıldığını**söyler.

Kümede hangi hizmetlerin yüklü olduğunu bilmediğinizde, bir liste almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

REST API ambarı ile çalışma hakkında daha fazla bilgi için bkz. [HDInsight 'ı Apache ambarı REST API kullanarak izleme ve yönetme](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Hizmet bileşenleri

Hizmetler, tek tek durumunu denetlemek istediğiniz bileşenleri içerebilir. Örneğin,,, bir süs Code bileşeni içerir. Bir bileşen hakkındaki bilgileri görüntülemek için, komut şöyle olur:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Bir hizmet tarafından hangi bileşenlerin sağlandığını bilmediğinizde, bir liste almak için aşağıdaki komutu kullanabilirsiniz:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Baş düğümlerdeki günlük dosyalarına erişme

### <a name="ssh"></a>SSH

SSH aracılığıyla bir baş düğüme bağlıyken günlük dosyaları **/var/log**altında bulunabilir. Örneğin, **/var/log/Hadoop-Yarn/Yarn** Yarn için günlükleri içerir.

Her bir baş düğüm benzersiz günlük girişlerine sahip olabilir, bu nedenle günlükleri her ikisinde de denetlemeniz gerekir.

### <a name="sftp"></a>SFTP

Ayrıca SSH Dosya Aktarım Protokolü veya güvenli Dosya Aktarım Protokolü (SFTP) kullanarak baş düğüme bağlanabilir ve günlük dosyalarını doğrudan indirebilirsiniz.

SSH istemcisi kullanmaya benzer şekilde, kümeye bağlanırken SSH kullanıcı hesabı adını ve kümenin SSH adresini sağlamanız gerekir. Örneğin: `sftp username@mycluster-ssh.azurehdinsight.net`. İstendiğinde hesabın parolasını sağlayın veya `-i` parametresini kullanarak bir ortak anahtar sağlayın.

Bağlandıktan sonra size bir `sftp>` istem sunulur. Bu istem içinden dizinleri değiştirebilir, karşıya yükleyebilir ve dosyaları indirebilirsiniz. Örneğin, aşağıdaki komutlar dizinleri **/var/log/Hadoop/HDFS** diziniyle değiştirir ve sonra dizindeki tüm dosyaları indirir.

    cd /var/log/hadoop/hdfs
    get *

Kullanılabilir komutların listesi için, `help` `sftp>` istemine yazın.

> [!NOTE]  
> Ayrıca, SFTP kullanarak bağlıyken dosya sistemini görselleştirmenize imkan tanıyan grafiksel arabirimler de vardır. Örneğin, [MobaXterm](https://mobaxterm.mobatek.net/) , Windows Gezgini ' ne benzer bir arabirim kullanarak dosya sistemine gözatmanızı sağlar.

### <a name="ambari"></a>Ambari

> [!NOTE]  
> Ambarı kullanarak günlük dosyalarına erişmek için bir SSH tüneli kullanmanız gerekir. Bireysel hizmetlerin Web arabirimleri, Internet üzerinde herkese açık bir şekilde gösterilmez. SSH tüneli kullanma hakkında daha fazla bilgi için bkz. [SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md) belgesi.

Ambarı web kullanıcı arabiriminden, günlükleri görüntülemek istediğiniz hizmeti seçin (örneğin, YARN). Ardından, günlüklerin görüntüleneceği baş düğümü seçmek için **hızlı bağlantılar** ' ı kullanın.

![Günlükleri görüntülemek için hızlı bağlantıları kullanma](./media/hdinsight-high-availability-linux/quick-links-view-logs.png)

## <a name="how-to-configure-the-node-size"></a>Düğüm boyutunu yapılandırma

Bir düğümün boyutu yalnızca küme oluşturma sırasında seçilebilir. HDInsight [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/hdinsight/)HDInsight için KULLANILABILIR farklı VM boyutlarının listesini bulabilirsiniz.

Bir küme oluştururken düğümlerin boyutunu belirtebilirsiniz. Aşağıdaki bilgiler [Azure Portal][preview-portal], [Azure PowerShell modül az][azure-powershell]ve [Azure CLI][azure-cli]kullanarak boyutun nasıl belirtilbileceğine ilişkin yönergeler sağlar:

* **Azure portalı**: Bir küme oluştururken, küme tarafından kullanılan düğümlerin boyutunu ayarlayabilirsiniz:

    ![Düğüm boyutu seçimiyle küme oluşturma Sihirbazı görüntüsü](./media/hdinsight-high-availability-linux/hdinsight-headnodesize.png)

* **Azure CLI**: [Az HDInsight Create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) komutunu kullanırken,, ve `--headnode-size` `--zookeepernode-size` parametrelerini kullanarak `--workernode-size`baş, çalışan ve ZooKeeper düğümlerinin boyutunu ayarlayabilirsiniz.

* **Azure PowerShell**: [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) cmdlet 'ini kullanırken,, ve `-HeadNodeSize` `-ZookeeperNodeSize` parametrelerini kullanarak `-WorkerNodeSize`baş, çalışan ve ZooKeeper düğümlerinin boyutunu ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede bahsedilen şeyler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

* [Apache ambarı REST başvurusu](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Azure CLı 'yı yükleyip yapılandırma](https://docs.microsoft.com//cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure PowerShell modülünü yükleyip yapılandırın az](/powershell/azure/overview)
* [Apache ambarı kullanarak HDInsight 'ı yönetme](hdinsight-hadoop-manage-ambari.md)
* [Linux tabanlı HDInsight kümeleri sağlama](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
