---
title: Azure HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin
description: Linux tabanlı HDInsight'taki Fırtına Panosu'nu kullanarak Apache Storm topologlarını nasıl dağıtacağını, izleyeceğini ve yöneteceğinizi öğrenin. Visual Studio için Hadoop araçlarını kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: e890289230b3215bd102d8c5a78dca4f1b7b90f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271908"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-azure-hdinsight"></a>Azure HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin

Bu belgede, HDInsight kümelerinde Storm'da çalışan [Apache Storm](https://storm.apache.org/) topolojilerini yönetme ve izleme nin temellerini öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Storm kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **Fırtına'yı**seçin. **Storm**

* (İsteğe bağlı) Güvenli Kabuk (SSH) ve Güvenli Kopya (SCP) ile aşinalık. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* (İsteğe bağlı) Visual Studio, Azure SDK 2.5.1 veya daha yeni ve Visual Studio için Veri Gölü Araçları. Daha fazla bilgi için, [Görsel Stüdyo Veri Gölü Araçları & Apache Hadoop](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)bakın.

## <a name="submit-a-topology-using-visual-studio"></a>Visual Studio'u kullanarak topoloji gönderin

C# veya hibrit topolojileri Fırtına kümenize göndermek için Visual Studio için Veri Gölü Araçlarını kullanabilirsiniz. Aşağıdaki adımlar örnek bir uygulama kullanır. Veri Gölü Araçları'nı kullanarak topoloji oluşturma hakkında bilgi için [Visual Studio ve C# ile Apache Storm topolojilerine](apache-storm-develop-csharp-visual-studio-topology.md)bakın.

1. Visual Studio için Veri Gölü araçlarının en son sürümünü zaten yüklemediyseniz, [Visual Studio için Veri Gölü Araçlarını Kullanın'a](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)bakın.

    > [!NOTE]  
    > Azure Veri Gölü ve Akış Analizi Araçları eskiden VISUAL Studio için HDInsight Araçları olarak adlandırılıyordu.
    >
    > Visual Studio için Azure Veri Gölü ve Akış Analizi Araçları, Visual Studio 2019 için **Azure geliştirme** iş yüküne dahildir.

1. Visual Studio’yu çalıştırın.

1. **Başlat** penceresinde yeni **bir proje oluştur'u**seçin.

1. Yeni **proje oluştur** penceresinde, arama kutusunu seçin `Storm`ve . Ardından sonuç listesinden **Fırtına Örneği'ni** seçin ve **İleri'yi**seçin.

1. Yeni **proje pencerenizi Yapılandır'** da, bir **Proje adı**girin ve yeni projeyi kaydetmek için bir **Konum'a** gidin veya oluşturun. Ardından **Oluştur**’u seçin.

    ![Yeni proje pencerenizi yapılandırın, Visual Studio](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-sample1.png)

1. **Server Explorer'dan** **Azure'a** sağ tıklayın ve Microsoft **Azure Aboneliğine Bağlan'ı seçin...** ve oturum açma işlemini tamamlayın.

1. **Solution Explorer'dan**projeyi sağ tıklatın ve **HDInsight'ta Fırtınaya Gönder'i**seçin.

    > [!NOTE]  
    > İstenirse, Azure aboneliğiniz için giriş kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, HDInsight kümesinde Storm'unuzu içeren bir abonelikte oturum açın.

1. **Topoloji Gönder** iletişim kutusunda, **Fırtına Kümesi** açılır listesinin altında, HDInsight kümesinde Fırtına'nızı seçin ve ardından **Gönder'i**seçin. **Çıktı** bölmesini görüntüleyerek gönderinin başarılı olup olmadığını izleyebilirsiniz.

## <a name="submit-a-topology-using-ssh-and-the-storm-command"></a>SSH ve Fırtına komutunu kullanarak topoloji gönderin

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Ssh oturumunuzdan **WordCount** örnek topolojisini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount
    ```

    Bu komut, kümede örnek WordCount topolojisini başlatır. Bu topoloji rastgele cümleler oluşturur ve sonra cümlelerdeki her sözcüğün oluşumunu sayar.

    > [!NOTE]  
    > Kümeye topoloji gönderirken, komutu kullanmadan önce kümeyi içeren `storm` .jar dosyasını kopyalamanız gerekir. Dosyayı kümeye kopyalamak için komutu `scp` kullanabilirsiniz. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar` girin.
    >
    > *WordCount* örneği ve diğer fırtına başlatıcı örnekleri, 'de `/usr/hdp/current/storm-client/contrib/storm-starter/`kümenizde zaten yer aldı.

## <a name="submit-a-topology-programmatically"></a>Programlı bir topoloji gönderin

Nimbus hizmetini kullanarak bir topolojiyi programlı olarak dağıtabilirsiniz. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology)Nimbus hizmeti aracılığıyla bir topolojinin nasıl dağıtılanve başlatılabildiğini gösteren örnek bir Java uygulaması sağlar.

## <a name="monitor-and-manage-a-topology-in-visual-studio"></a>Visual Studio'da bir topolojiyi izleyin ve yönetin

Visual Studio'yu kullanarak bir topoloji gönderdiğinde, **Storm Topologies View** penceresi görüntülenir. Çalışan topoloji hakkındaki bilgileri görüntülemek için listeden topolojiyi seçin.

![Monitör topoloji, Fırtına Topolojileri Görünüm penceresi, Visual Studio](./media/apache-storm-deploy-monitor-topology-linux/visual-studio-monitor.png)

> [!NOTE]  
> Ayrıca Server **Explorer'dan Fırtına Topolojilerini** de görüntüleyebilirsiniz. **Server Explorer** **Azure** > **HDInsight'ı**genişletin, HDInsight kümesinde Bir Fırtına'ya sağ tıklayın ve ardından Fırtına **Topolojilerini Görüntüle'yi**seçin.

Bu bileşenler hakkındaki bilgileri görüntülemek için emzme lerin veya cıvataların şeklini seçin. Seçilen öğe için bileşen bilgilerini içeren bir araç ucu görüntülenir.

### <a name="deactivate-and-reactivate-a-topology"></a>Bir topolojiyi devre dışı bırakma ve yeniden etkinleştirme

Bir topolojiyi devre dışı bırakarak, topoloji öldürülene veya yeniden etkinleştirilene kadar onu duraklatır. Bu işlemleri yapmak için, **Fırtına Topolojileri Görünümü** penceresinin üst kısmındaki **Eylemler** alanında Devre **Dışı Bırak** ve Yeniden **Etkinleştir** düğmelerini kullanın.

### <a name="rebalance-a-topology"></a>Bir topolojiyi yeniden dengeleme

Bir topolojinin yeniden dengelenmesi, sistemin topolojinin paralelliğini gözden geçirmesini sağlar. Örneğin, kümeyi daha fazla not eklemek için yeniden boyutlandırmettiyseniz, yeniden dengeleme bir topolojinin yeni düğümleri görmesini sağlar.

Bir topolojiyi yeniden dengelemek **için, Fırtına Topolojileri Görünümü** penceresinin **Eylemler** alanında **Yeniden Denge** düğmesini kullanın.

> [!WARNING]  
> Bir topolojiyi yeniden dengelemek topolojiyi devre dışı bırakır, işçileri küme boyunca eşit olarak dağıtır ve sonra topolojiyi yeniden dengeleme oluşmadan önceki durumuna döndürür. Topoloji aktifse, tekrar aktif hale gelir. Topoloji devre dışı bırakılmışsa, devre dışı kalır.

### <a name="kill-a-running-topology"></a>Çalışan bir topolojiyi öldür

Fırtına topolojileri durdurulana veya küme silinene kadar çalışmaya devam ederler. Bir topolojiyi durdurmak için **Eylemler** alanındaki **Öldür** düğmesini kullanın.

## <a name="monitor-and-manage-a-topology-using-ssh-and-the-storm-command"></a>SSH ve Storm komutunu kullanarak bir topolojiyi izleyin ve yönetin

Yardımcı `storm` program, komut satırından çalışan topolojilerle çalışmanızı sağlar. Komutların tam listesi için kullanın. `storm -h`

### <a name="list-topologies"></a>Topolojileri listele

Çalışan tüm topolojileri listelemek için aşağıdaki komutu kullanın:

```shell
storm list
```

Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

```shell
Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
-------------------------------------------------------------------
WordCount            ACTIVE     29         2            263
```

### <a name="deactivate-and-reactivate-a-topology"></a>Bir topolojiyi devre dışı bırakma ve yeniden etkinleştirme

Bir topolojiyi devre dışı bırakarak, topoloji öldürülene veya yeniden etkinleştirilene kadar onu duraklatır. Devre dışı bırakmak veya yeniden etkinleştirmek için aşağıdaki komutları kullanın:

```shell
storm Deactivate TOPOLOGYNAME
```

```shell
storm Activate TOPOLOGYNAME
```

### <a name="kill-a-running-topology"></a>Çalışan bir topolojiyi öldür

Fırtına topolojileri, bir kez başladı, durdurulana kadar çalışmaya devam ediyor. Bir topolojiyi durdurmak için aşağıdaki komutu kullanın:

```shell
storm kill TOPOLOGYNAME
```

### <a name="rebalance-a-topology"></a>Bir topolojiyi yeniden dengeleme

Bir topolojinin yeniden dengelenmesi, sistemin topolojinin paralelliğini gözden geçirmesini sağlar. Örneğin, kümeyi daha fazla not eklemek için yeniden boyutlandırmettiyseniz, yeniden dengeleme bir topolojinin yeni düğümleri görmesini sağlar.

> [!WARNING]  
> Bir topolojiyi yeniden dengelemek topolojiyi devre dışı bırakır, işçileri küme boyunca eşit olarak dağıtır ve sonra topolojiyi yeniden dengeleme oluşmadan önceki durumuna döndürür. Topoloji aktifse, tekrar aktif hale gelir. Devre dışı bırakıldıysa, devre dışı kalır.

```shell
storm rebalance TOPOLOGYNAME
```

## <a name="monitor-and-manage-a-topology-using-the-storm-ui"></a>Storm UI'yi kullanarak bir topolojiyi izleyin ve yönetin

Storm UI, çalışan topolojilerle çalışmak için bir web arabirimi sağlar ve HDInsight kümenize dahildir. Storm UI'yi görüntülemek için `https://CLUSTERNAME.azurehdinsight.net/stormui` *CLUSTERNAME'nin* kümenizin adı olduğu bir web tarayıcısı kullanın.

> [!NOTE]  
> Bir kullanıcı adı ve parola sağlamanız istenirse, küme oluştururken kullandığınız küme yöneticisi kullanıcı adını ve parolasını girin.

### <a name="storm-ui-main-page"></a>Fırtına UI ana sayfası

Fırtına Kullanıcı Çağrı Birimi'nin ana sayfası aşağıdaki bilgileri sağlar:

| Section | Açıklama |
| --- | --- |
| Küme özeti| Fırtına kümesi hakkında temel bilgiler. |
| Nimbus özeti | Temel Nimbus bilgilerinin listesi. |
| Topoloji özeti | Çalışan topolojilerin listesi. Belirli bir topoloji hakkında daha fazla bilgi görüntülemek için **Ad** sütunundaki bağlantısını seçin. |
| Denetçi özeti | Fırtına amiri hakkında bilgi. Belirli bir gözetmenle ilişkili alt kaynakları görmek **için, Ana Bilgisayar** veya **Kimlik** sütunundaki bağlantısını seçin. |
| Nimbus yapılandırması | Küme için Nimbus yapılandırması. |

Storm UI ana sayfası bu web sayfasına benzer:

![Ana sayfa, Fırtına UI, Apache Storm topolojileri, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-main-page.png)

#### <a name="topology-summary"></a>Topoloji özeti

**Topoloji özet** bölümünden bir bağlantı seçilmesi topoloji hakkında aşağıdaki bilgileri görüntüler:

| Section | Açıklama |
| --- | --- |
| Topoloji özeti | Topoloji hakkında temel bilgiler. |
| Topoloji eylemleri| Topoloji için yapabileceğiniz yönetim eylemleri. Kullanılabilir eylemler daha sonra bu bölümde açıklanmıştır. |
| Topoloji istatistikleri | Topoloji ile ilgili istatistikler. Bu bölümdeki bir girişin zaman çerçevesini ayarlamak için **Pencere** sütunundaki bağlantısını seçin. |
| Spouts *(zaman dilimi)* | Topoloji tarafından kullanılan emzitler. Belirli bir spout hakkında daha fazla bilgi görüntülemek **için, Id** sütunundaki bağlantısını seçin. |
| Cıvatalar *(zaman dilimi)* | Topoloji tarafından kullanılan cıvatalar. Belirli bir cıvata hakkında daha fazla bilgi görüntülemek **için, Id** sütunundaki bağlantısını seçin. |
| İşçi kaynakları | İşçi kaynaklarının listesi. Belirli bir alt kaynak hakkında daha fazla bilgi görüntülemek için **Ana Bilgisayar** sütunundaki bağlantısını seçin. |
| Topoloji görselleştirme | Topolojinin görselleştirmesini görüntüleyen **Görselleştirme** göster düğmesi. |
| Topoloji yapılandırması | Seçilen topolojinin yapılandırması. |

Fırtına topoloji özet sayfası bu web sayfasına benzer:

![Topoloji özet sayfası, Storm UI, Apache Storm, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-topology-summary.png)

**Topoloji eylemleri** bölümünde, bir eylem yapmak için aşağıdaki düğmeleri seçebilirsiniz:

| Düğme | Açıklama |
| --- | --- |
| Etkinleştir | Devre dışı bırakılmış bir topolojinin işlemesini devam ettirer. |
| Devre dışı bırak | Çalışan bir topolojiyi duraklatabilir. |
| Yeniden dengeleme | Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bu işlem, topolojinin kümedeki ek veya azaltılmış düğüm sayısını telafi etmek için paralelliği ayarlamasına olanak tanır.<br/><br/>Daha fazla bilgi için bkz: <a href="https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Apache Storm topolojisinin paralelliğini anlamak.</a>
| Öldür | Belirtilen zaman anından sonra Fırtına topolojisini sonlandırır. |
| Hata ayıklama | Çalışan topoloji için bir hata ayıklama oturumu başlar. |
| Hata Ayıklama'yı Durdur | Çalışan topoloji için hata ayıklama oturumunu sona erdirin. |
| Günlük Düzeyini Değiştir | Hata ayıklama günlüğü düzeyini değiştirir. |

##### <a name="spout-and-bolt-summary"></a>Spout ve cıvata özeti

**Spouts** veya **Bolts** bölümlerinden bir emme seçmek seçili öğe hakkında aşağıdaki bilgileri görüntüler:

| Section | Açıklama |
| --- | --- |
| Bileşen özeti | Emzme veya cıvata hakkında temel bilgiler. |
| Bileşen eylemleri | **Hata Ayıklama** ve **Hata Ayıklama'yı Durdur** düğmeleri. |
| Spout istatistikleri veya cıvata istatistikleri | Emzit veya cıvata ile ilgili istatistikler. Bu bölümdeki bir girişin zaman çerçevesini ayarlamak için **Pencere** sütunundaki bağlantısını seçin. |
| (Yalnızca Cıvata)<br/>Giriş istatistikleri *(zaman dilimi)* | Cıvata tarafından tüketilen giriş akışları hakkında bilgi. |
| Çıktı istatistikleri *(zaman dilimi)* | Emzit veya cıvata tarafından yayılan akışlar hakkında bilgi. |
| Profil oluşturma ve hata ayıklama | Bu sayfadaki bileşenlerin profilini çıkarma ve hata ayıklama denetimleri. Durum / **Zaman Ekme (Dakika)** değerini ayarlayabilir ve **JStack,** **Restart Worker**ve **Heap**için düğmeleri seçebilirsiniz. |
| Uygulayıcılar *(zaman dilimi)* | Emzme veya cıvata örnekleri hakkında bilgi. Bu örnek için üretilen tanılama bilgilerinin günlüğünü görüntülemek için, belirli bir uygulayıcı için **Bağlantı Noktası** girişini seçin. **Ana Bilgisayar** sütunundaki bağlantısını seçerek belirli bir yürütücüyle ilişkili alt kaynakları da görebilirsiniz. |
| Hatalar | Emzme veya cıvata için herhangi bir hata bilgisi. |

Fırtına cıvata özet sayfası bu web sayfasına benzer:

![Cıvata özeti sayfası, Storm UI, Apache Storm, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-bolt-summary.png)

## <a name="monitor-and-manage-the-topology-using-the-rest-api"></a>REST API'yi kullanarak topolojiyi izleyin ve yönetin

Storm UI, REST API'nin üzerine inşa edilmiştir, böylece REST API'sini kullanarak benzer yönetim ve izleme görevlerini yapabilirsiniz. Storm topolojilerini yönetmek ve izlemek için özel araçlar oluşturmak için REST API'sini kullanabilirsiniz.

Daha fazla bilgi için, [Apache Storm UI REST API'ye](https://storm.apache.org/releases/current/STORM-UI-REST-API.html)bakın. Aşağıdaki bilgiler HDInsight'ta Apache Storm ile REST API'yi kullanmaya özeldir.

> [!IMPORTANT]  
> Fırtına REST API internet üzerinden kamuya açık değildir. HDInsight küme kafa düğümüne bir SSH tüneli kullanılarak erişilmelidir. Bir SSH tüneli oluşturma ve kullanma hakkında daha fazla bilgi için [Azure HDInsight'a erişmek için SSH tünellerini kullan'a](../hdinsight-linux-ambari-ssh-tunnel.md)bakın.

### <a name="base-uri"></a>Baz URI

Linux tabanlı HDInsight kümelerinde REST API'si için temel `https://HEADNODEFQDN:8744/api/v1/`URI, *HEADNODEFQDN'yi* baş düğümüyle değiştirdiğiniz URL adresinde niçin kullanılabilir. Kafa düğümünün etki alanı adı küme oluşturma sırasında oluşturulur ve statik değildir.

Küme kafası düğümü için tam nitelikli etki alanı adını (FQDN) çeşitli şekillerde bulabilirsiniz:

| FQDN bulma yöntemi | Açıklama |
| --- | --- |
| SSH oturumu | Bir SSH oturumundan kümeye komutu `headnode -f` kullanın. |
| Ambari Web | Ambari küme web sayfasında`https://CLUSTERNAME.azurehdinsight.net`( ), Sayfanın üst kısmından **Hizmetleri** seçin ve ardından **Fırtına'yı**seçin. **Özet** sekmesinden, **Storm UI Server'ı**seçin. Fırtına UI ve REST API'sini barındıran düğümün FQDN'si sayfanın üst kısmında görüntülenir. |
| Ambari REST API | Storm UI ve REST API'nin üzerinde çalıştırdığı düğüm hakkında bilgi almak için komutu `curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` kullanın. *CLUSTERNAME'nin* iki örneğini küme adı ile değiştirin. Sizden istendiğinde, kullanıcı (yönetici) hesabının parolasını girin. Yanıtta, JSON çıkışının "host_name" girişi düğümün FQDN'sini içerir. |

### <a name="authentication"></a>Kimlik doğrulaması

REST API'ye gelen istekler *temel kimlik doğrulamasını*kullanmalıdır, bu nedenle HDInsight kümesi için yönetici adını ve parolasını kullanmanız gerekir.

> [!NOTE]  
> Temel kimlik doğrulaması açık metin kullanılarak gönderildiğinden, kümeyle iletişimi sağlamak için *HER zaman* HTTPS'yi kullanmanız gerekir.

### <a name="return-values"></a>Döndürülen değerler

REST API'sinden döndürülen bilgiler yalnızca küme içinden kullanılabilir. Örneğin, [Apache ZooKeeper](https://zookeeper.apache.org/) sunucuları için döndürülen tam nitelikli alan adı (FQDN) internetten erişilebilir değildir.

## <a name="next-steps"></a>Sonraki adımlar

[Apache Maven kullanarak Java tabanlı topolojileri](apache-storm-develop-java-topology.md)nasıl geliştireceğinizi öğrenin.

Daha fazla örnek topoloji listesi için, [Azure HDInsight'taki Örnek Apache Storm topolojilerine](apache-storm-example-topology.md)bakın.
