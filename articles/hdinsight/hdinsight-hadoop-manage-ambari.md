---
title: Ambari Web Kullanıcı UI'yi kullanarak Azure HDInsight'ı izleyin ve yönetin
description: HDInsight kümelerini izlemek ve yönetmek için Apache Ambari Kullanıcı Arabirimi'ni nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/16/2020
ms.openlocfilehash: 2ed3d6b1088315b580ab8ddc4f12d8d61434ec53
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606552"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-web-ui"></a>HDInsight kümelerini Apache Ambari Web arabiriminden yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari, bir Apache Hadoop kümesinin yönetimini ve izlenmesini kolaylaştırır. Bu basitleştirme web UI ve REST API kullanımı kolay sağlayarak yapılır. Ambari HDInsight kümelerinde yer alan ve kümeizlemek ve yapılandırma değişiklikleri yapmak için kullanılır.

Bu belgede, Ambari Web UI'yi hdinsight kümesiyle nasıl kullanacağınızı öğrenirsiniz.

## <a name="what-is-apache-ambari"></a><a id="whatis"></a>Apaçi Ambari nedir?

[Apache Ambari,](https://ambari.apache.org) kullanımı kolay bir web kullanıcı arabirimi sağlayarak Hadoop yönetimini basitleştirir. Hadoop kümelerini yönetmek ve izlemek için Ambari'yi kullanabilirsiniz. Geliştiriciler [Ambari REST API'lerini](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)kullanarak bu yetenekleri uygulamalarına entegre edebilirler.

## <a name="connectivity"></a>Bağlantı

Ambari Web Kullanıcı Arabirimi, HDInsight `https://CLUSTERNAME.azurehdinsight.net`kümenizde `CLUSTERNAME` , kümenizin adı olan yerde kullanılabilir.

> [!IMPORTANT]  
> HDInsight'ta Ambari'ye bağlanmak IÇIN HTTPS gerekiyor. Kimlik doğrulaması istendiğinde, küme oluşturulduğunda sağladığınız yönetici hesap adını ve parolasını kullanın. Kimlik bilgileri istenmezse, istemci ile Azure HDInsight Kümeleri arasında bağlantı sorunu olmadığını doğrulamak için ağ ayarlarınızı kontrol edin.

## <a name="ssh-tunnel-proxy"></a>SSH tüneli (proxy)

Kümeniz için Ambari'ye doğrudan Internet üzerinden erişilebilir olsa da, Ambari Web UI'den (JobTracker gibi) bazı bağlantılar internette açık değildir. Bu hizmetlere erişmek için bir SSH tüneli oluşturmanız gerekir. Daha fazla bilgi için [hdinsight ile SSH Tünel Kullanma'ya](hdinsight-linux-ambari-ssh-tunnel.md)bakın.

## <a name="ambari-web-ui"></a>Ambari Web UI

> [!WARNING]  
> Ambari Web UI'nin tüm özellikleri HDInsight'ta desteklenmez. Daha fazla bilgi için bu belgenin [Desteklenmeyen işlemler](#unsupported-operations) bölümüne bakın.

When connecting to the Ambari Web UI, you're prompted to authenticate to the page. Küme oluşturma sırasında kullandığınız küme yöneticisi kullanıcısını (varsayılan Yönetici) ve parolayı kullanın.

Sayfa açıldığında, üstteki çubuğu not edin. Bu çubuk aşağıdaki bilgileri ve denetimleri içerir:

![Apache Ambari panoya genel bakış](./media/hdinsight-hadoop-manage-ambari/apache-ambari-dashboard.png)

|Öğe |Açıklama |
|---|---|
|Ambari logosu|Kümeyi izlemek için kullanılabilecek panoyu açar.|
|Küme adı # ops|Devam eden Ambari operasyonlarının sayısını görüntüler. Küme adı veya **# ops** seçilmesi arka plan işlemlerinin bir listesini görüntüler.|
|# uyarıları|Küme için uyarılar veya kritik uyarılar görüntüler.|
|Pano|Panonu görüntüler.|
|Hizmetler|Kümedeki hizmetler için bilgi ve yapılandırma ayarları.|
|Ana bilgisayarlar|Kümedeki düğümler için bilgi ve yapılandırma ayarları.|
|Uyarılar|Bilgi, uyarı ve kritik uyarıların günlüğü.|
|Yönetici|Kümeye yüklenen yazılım yığını/hizmetler, hizmet hesabı bilgileri ve Kerberos güvenliği.|
|Yönetici düğmesi|Ambari yönetimi, kullanıcı ayarları ve oturum dışı.|

## <a name="monitoring"></a>İzleme

### <a name="alerts"></a>Uyarılar

Aşağıdaki liste Ambari tarafından kullanılan ortak uyarı durumlarını içerir:

* **Tamam**
* **Uyarı**
* **Kritik**
* **Bilinmeyen**

**Tamam** dışındaki **uyarılar, # uyarılarının** sayfanın üst kısmındaki uyarıların sayısını görüntülemesine neden olur. Bu girişi seçmek uyarıları ve durumlarını görüntüler.

Uyarılar, **Uyarılar** sayfasından görüntülenebilen birkaç varsayılan gruba ayrılır.

![Apache Ambari uyarılar sayfa özeti](./media/hdinsight-hadoop-manage-ambari/hdinsight-alerts-page.png)

**Eylemler** menüsünü kullanarak ve **Uyarı Gruplarını Yönet'i**seçerek grupları yönetebilirsiniz.

![Apache Ambari uyarı gruplarını yönetiyor](./media/hdinsight-hadoop-manage-ambari/ambari-manage-alerts.png)

Uyarı yöntemlerini yönetiyorsunuz ve Bildirimleri Yönet'i seçerek **Eylemler** menüsünden uyarı __bildirimleri oluşturuyorsunuz.__ Geçerli bildirimler görüntülenir. Buradan bildirimler oluşturun. Bildirimler, belirli uyarı/önem kombinasyonları oluştuğunda **E-posta** veya **SNMP** üzerinden gönderilebilir. Örneğin, **İplik Varsayılan** grubundaki uyarılardan herhangi biri **Kritik**olarak ayarlandığında bir e-posta iletisi gönderebilirsiniz.

![Apache Ambari uyarı bildirimi oluşturmak](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Son olarak, __Eylemler__ menüsünden __Uyarı Ayarlarını Yönet'i__ seçmek, bir bildirim gönderilmeden önce bir uyarının kaç kez gerçekleşmesi gerektiğini ayarlamanızı sağlar. Bu ayar, geçici hatalar için bildirimleri önlemek için kullanılabilir.

Ücretsiz [sendGrid hesabı](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email)nı kullanarak bir uyarı bildirimi öğreticisi için Azure [HDInsight'ta Apache Ambari e-posta bildirimlerini yapılandırın'](./apache-ambari-email.md)a bakın.

### <a name="cluster"></a>Küme

Panonun **Ölçümler** sekmesi, kümenizin durumunu bir bakışta izlemeyi kolaylaştıran bir dizi widget içerir. **CPU Kullanımı**gibi çeşitli widget'lar tıklatıldığında ek bilgiler sağlar.

![Ölçümleri ile Apache Ambari pano](./media/hdinsight-hadoop-manage-ambari/hdi-metrics-dashboard.png)

**Heatmaps** sekmesi, yeşilden kırmızıya doğru giden renkli ısı haritaları olarak ölçümleri görüntüler.

![Isı haritaları ile Apache Ambari pano](./media/hdinsight-hadoop-manage-ambari/hdi-heatmap-dashboard.png)

Küme içindeki düğümler hakkında daha fazla bilgi için **Ana Bilgisayarlar'ı**seçin. Ardından ilgilendiğiniz belirli düğümü seçin.

![Apache Ambari ev sahibi özet detayları](./media/hdinsight-hadoop-manage-ambari/ambari-host-details1.png)

### <a name="services"></a>Hizmetler

Panodaki **Hizmetler** kenar çubuğu, kümede çalışan hizmetlerin durumu hakkında hızlı bir fikir sağlar. Durum veya yapılması gereken eylemleri belirtmek için çeşitli simgeler kullanılır. Örneğin, bir hizmetin geri dönüştürülmesi gerekiyorsa sarı bir geri dönüşüm simgesi görüntülenir.

![Apache Ambari hizmetleri yan bar](./media/hdinsight-hadoop-manage-ambari/apache-ambari-service-bar.png)

> [!NOTE]  
> Görüntülenen hizmetler HDInsight küme türleri ve sürümleri arasında farklılık gösterir. Burada görüntülenen hizmetler kümeniz için görüntülenen hizmetlerden farklı olabilir.

Bir hizmet seçmek, hizmet hakkında daha ayrıntılı bilgiler görüntüler.

![Apache Ambari hizmet özeti bilgileri](./media/hdinsight-hadoop-manage-ambari/ambari-service-details.png)

#### <a name="quick-links"></a>Hızlı bağlantılar

Bazı hizmetler sayfanın üst kısmında **hızlı bağlantılar** bağlantısı görüntüler. Bu bağlantı, aşağıdakiler gibi hizmete özel web UI'lerine erişmek için kullanılabilir:

* **İş Geçmişi** - MapReduce iş geçmişi.
* **Kaynak Yöneticisi** - İplik Kaynak Yöneticisi UI.
* **NameNode** - Hadoop Dağıtılmış Dosya Sistemi (HDFS) NameNode UI.
* **Oozie Web UI** - Oozie UI.

Bu bağlantılardan herhangi birini seçmek, tarayıcınızda seçili sayfayı görüntüleyen yeni bir sekme açar.

> [!NOTE]  
> Bir hizmet için **Hızlı Bağlantılar** girişini seçmek bir "sunucu bulunamadı" hatasını döndürebilir. Bu hatayla karşılaşırsanız, bu hizmet için **Hızlı Bağlantılar** girişini kullanırken bir SSH tüneli kullanmanız gerekir. Daha fazla bilgi için [HDInsight ile SSH Tünel'i Kullan](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Yönetim

### <a name="ambari-users-groups-and-permissions"></a>Ambari kullanıcıları, grupları ve izinleri

HdInsight kümesine katılan bir [etki alanı](./domain-joined/hdinsight-security-overview.md) kullanırken kullanıcılarla, gruplarla ve izinlerle çalışma desteklenir. Ambari Management UI'yi etki alanı birleştirilmiş kümede kullanma hakkında daha fazla bilgi için [bkz.](./domain-joined/hdinsight-security-overview.md)

> [!WARNING]  
> Linux tabanlı HDInsight kümenizdeki Ambari izleme örgütünün (hdinsightwatchdog) parolasını değiştirmeyin. Parolayı değiştirmek, komut dosyası eylemlerini kullanma veya kümenizle ölçekleme işlemleri gerçekleştirme yeteneğini bozar.

### <a name="hosts"></a>Ana bilgisayarlar

**Ana Bilgisayarlar** sayfası kümedeki tüm ana bilgisayarları listeler. Ana bilgisayarları yönetmek için aşağıdaki adımları izleyin.

![Apache Ambari sayfaya genel bakış ev sahipliği yapıyor](./media/hdinsight-hadoop-manage-ambari/hdinsight-hosts-page.png)

> [!NOTE]  
> Bir ana bilgisayar ekleme, devre dışı bırakma ve yeniden devreye alma HDInsight kümeleri ile kullanılmamalıdır.

1. Yönetmek istediğiniz ana bilgisayarını seçin.

2. Yapmak istediğiniz eylemi seçmek için **Eylemler** menüsünü kullanın:

    |Öğe |Açıklama |
    |---|---|
    |Tüm bileşenleri başlatın|Ana bilgisayardaki tüm bileşenleri başlatın.|
    |Tüm bileşenleri durdurun|Ana bilgisayardaki tüm bileşenleri durdurun.|
    |Tüm bileşenleri yeniden başlatın|Ana bilgisayardaki tüm bileşenleri durdurun ve çalıştırın.|
    |Bakım modunu açma|Ana bilgisayar için uyarıları bastırır. Uyarılar oluşturan eylemler yapıyorsanız, bu mod etkinleştirilmelidir. Örneğin, durdurma ve bir hizmet başlatma.|
    |Bakım modunu kapatma|Ana bilgisayarı normal uyarıya döndürür.|
    |Durdur|Ana bilgisayarda DataNode veya NodeManagers'ı durdurur.|
    |Başlat|Ana bilgisayarda DataNode veya NodeManagers başlatır.|
    |Yeniden Başlatma|Ana bilgisayarda DataNode veya NodeManagers'ı durdurur ve başlatır.|
    |Hizmet dışı bırakma|Bir ana bilgisayarı kümeden kaldırır. **Bu eylemi HDInsight kümelerinde kullanmayın.**|
    |Yeniden devreye alma|Kümeye daha önce kullanımdan kaldırılmış bir ana bilgisayar ekler. **Bu eylemi HDInsight kümelerinde kullanmayın.**|

### <a name="services"></a><a id="service"></a>Hizmetleri

**Pano** veya **Hizmetler** sayfasından, tüm hizmetleri durdurmak ve başlatmak için hizmetler listesinin altındaki **Eylemler** düğmesini kullanın.

![Apache Ambari hizmet eylemleri listesi](./media/hdinsight-hadoop-manage-ambari/ambari-service-actions.png)

> [!WARNING]  
> **Hizmet Ekle** bu menüde listelenmiş olsa da, HDInsight kümesine hizmet eklemek için kullanılmamalıdır. Küme sağlama sırasında komut dosyası eylemi kullanılarak yeni hizmetler eklenmelidir. Komut Dosyası Eylemleri'ni kullanma hakkında daha fazla bilgi için, [Komut Dosyası Eylemleri'ni kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md)bakın.

**Eylemler** düğmesi tüm hizmetleri yeniden başlatabilirken, genellikle belirli bir hizmeti başlatmak, durdurmak veya yeniden başlatmak istiyorsunuz. Tek bir hizmetüzerinde eylemlerde bulunmak için aşağıdaki adımları kullanın:

1. **Pano** veya **Hizmetler** sayfasından bir hizmet seçin.

2. **Özet** sekmesinin üst kısmından, **Hizmet Eylemleri** düğmesini kullanın ve yapılacak eylemi seçin. Bu eylem tüm düğümlerde hizmeti yeniden başlatır.

    ![Apache Ambari bireysel hizmet eylemleri](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]  
   > Küme çalışırken bazı hizmetleri yeniden başlatmak uyarılar oluşturabilir. Uyarıları önlemek için, yeniden başlatmayı gerçekleştirmeden önce hizmetin **Bakım modunu** etkinleştirmek için **Servis Eylemleri** düğmesini kullanabilirsiniz.

3. Bir eylem seçildikten sonra, sayfanın üst kısmındaki **# op** girişi, arka plan işleminin gerçekleştiğini göstermek için artışlar gösterir. Görüntülemek için yapılandırılırsa, arka plan işlemleri listesi görüntülenir.

   > [!NOTE]  
   > Hizmet için **Bakım modunu** etkinleştirdiyseniz, işlem tamamlandıktan sonra **Servis Eylemleri** düğmesini kullanarak devre dışı etmeyi unutmayın.

Bir hizmeti yapılandırmak için aşağıdaki adımları kullanın:

1. **Pano** veya **Hizmetler** sayfasından bir hizmet seçin.

2. **Configs** sekmesini seçin. Geçerli yapılandırma görüntülenir. Önceki yapılandırmaların listesi de görüntülenir.

    ![Apache Ambari hizmet yapılandırması](./media/hdinsight-hadoop-manage-ambari/ambari-service-configs.png)

3. Yapılandırmayı değiştirmek için görüntülenen alanları kullanın ve sonra **Kaydet'i**seçin. Veya önceki yapılandırmayı seçin ve ardından önceki ayarlara geri dönmek için **geçerli ol'u** seçin.

## <a name="ambari-views"></a>Ambari görünümleri

Ambari Görünümler geliştiriciler [In Apache Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views)kullanarak Ambari Web UI içine UI öğeleri takmak için izin verir. HDInsight Hadoop küme türleri ile aşağıdaki görünümleri sağlar:

* Hive View: Hive View doğrudan web tarayıcınızdan Hive sorguları çalıştırmak için izin verir. Sorguları kaydedebilir, sonuçları görüntüleyebilir, sonuçları küme depolamasına kaydedebilir veya sonuçları yerel sisteminize indirebilirsiniz. Hive Görünümleri'ni kullanma hakkında daha fazla bilgi için [HDInsight ile Apache Hive Görünümlerini Kullan'a](hadoop/apache-hadoop-use-hive-ambari-view.md)bakın.

* Tez Görünümü: Tez Görünümü, işleri daha iyi anlamanızı ve optimize etmenizi sağlar. Tez işlerinin nasıl yürütüldürün ve hangi kaynakların kullanıldığı yla ilgili bilgileri görüntüleyebilirsiniz.

## <a name="unsupported-operations"></a>Desteklenmeyen işlemler

Aşağıdaki Ambari işlemleri HDInsight'ta desteklenmez:

* __Metrikler Toplayıcı hizmeti taşıma__. Metrikler Toplayıcısı hizmetindeki bilgileri görüntülerken, Hizmet Eylemleri menüsünden kullanılabilen eylemlerden biri __Ölçümleri Taşı toplayıcısi'dir.__ Bu eylem HDInsight ile desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* HDInsight ile [Apache Ambari REST API.](hdinsight-hadoop-manage-ambari-rest-api.md)
* [HDInsight kümesi yapılandırmalarını iyileştirmek için Apache Ambari'yi kullanma](./hdinsight-changing-configs-via-ambari.md)
* [Azure HDInsight kümelerini ölçeklendirin](./hdinsight-scaling-best-practices.md)
