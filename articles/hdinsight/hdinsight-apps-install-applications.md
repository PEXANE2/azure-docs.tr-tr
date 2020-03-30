---
title: Azure HDInsight'ta üçüncü taraf uygulamaları yükleme
description: Azure HDInsight'ta üçüncü taraf Apache Hadoop uygulamalarını nasıl yükleyeceğinizi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: hrasheed
ms.openlocfilehash: 22d27d50a7d2c304e5d1a04a9a5eaa03d2f0bfa6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366351"
---
# <a name="install-third-party-apache-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight'ta üçüncü taraf Apache Hadoop uygulamalarını yükleme

Azure HDInsight'ta üçüncü taraf [Bir Apache Hadoop](https://hadoop.apache.org/) uygulamasını nasıl yükleyeceğinizi öğrenin. Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

HDInsight uygulaması, kullanıcıların bir HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

Aşağıdaki liste yayımlanmış uygulamaları gösterir:

|Uygulama |Küme türü(ler) | Açıklama |
|---|---|---|
|[AtScale İstihbarat Platformu](https://azuremarketplace.microsoft.com/marketplace/apps/atscaleinc.atscale) |Hadoop |AtScale, MICROSOFT Excel, Power BI, Tableau Software'den QlikView'e kadar zaten bildiğiniz, sahip olduğunuz ve sevdiğiniz BI araçlarını kullanarak milyarlarca veri satırını etkileşimli olarak sorgulamanıza olanak tanıyan, HDInsight kümenizi ölçeklendirilmiş bir OLAP sunucusuna dönüştürür. |
|[HDInsight için CDAP](https://azuremarketplace.microsoft.com/marketplace/apps/cask.cdap-for-hdinsight) |HBase |CDAP, Hadoop için değer verme süresini hızlandıran ve BT'nin self servis veri sağlamasını sağlayan büyük veriler için ilk birleşik entegrasyon platformudur. Açık kaynak ve genişletilebilir, CDAP yenilik engelleri kaldırır. Gereksinimleri: 4 Bölge düğümleri, min D3 v2. |
|[Datameer](https://azuremarketplace.microsoft.com/marketplace/apps/datameer.datameer) |Hadoop |Datameer'in verilerinizi analitik için hazırlamak, keşfetmek ve yönetmek için kendi kendine hizmet veren ölçeklenebilir platformu, karmaşık çoklu kaynaklı verileri işletmeye hazır değerli bilgilere dönüştürmeyi hızlandırır ve kurumsal ölçekte daha hızlı ve daha akıllı öngörüler sunar. |
|[Dataiku DSS HDInsight üzerinde](https://azuremarketplace.microsoft.com/marketplace/apps/dataiku.dss-on-hdi) |Hadoop, Kıvılcım |Dataiku DSS, veri bilimcileri ve veri analistlerinin yeni veri ürün ve hizmetlerini daha verimli bir şekilde tasarlamak ve çalıştırmak için işbirliği yapmasına olanak tanıyan kurumsal veri bilimi platformunda ham verileri etkili tahminlere dönüştürür. |
|[WANdisco Fusion HDI Uygulaması](https://azuremarketplace.microsoft.com/marketplace/apps/wandisco.fusion-hdi-app) |Hadoop, Kıvılcım, HBase, Fırtına, Kafka |Dağıtılmış bir ortamda verileri tutarlı tutmak büyük bir veri işlemleri sorunudur. KURUMSAL sınıf bir yazılım platformu olan WANdisco Fusion, herhangi bir ortamda yapılandırılmamış veri tutarlılığını sağlayarak bu sorunu çözer. |
|[HDInsight için H2O Köpüklü Su](https://azuremarketplace.microsoft.com/marketplace/apps/h2o-ai.h2o-sparklingwater) |Spark |H2O Köpüklü Su aşağıdaki dağıtılmış algoritmaları destekler: GLM, Naif Bayes, Dağıtılmış Rastgele Orman, Gradyan Artırma Makinesi, Derin Sinir Ağları, Derin öğrenme, K-means, PCA, Genelleştirilmiş Düşük Rütbeli Modeller, Anomali Algılama, Autoencoders. |
|[HDInsight'a Gerçek Zamanlı Veri Entegrasyonu için Striim](https://azuremarketplace.microsoft.com/marketplace/apps/striim.hdinsightintegration) |Hadoop, HBase, Fırtına, Kıvılcım, Kafka |Striim (telaffuz "akış") bir uçtan uca veri entegrasyonu + istihbarat platformu, sürekli yutma sağlayan, işleme ve farklı veri akışları analitik. |
|[Jumbune Kurumsal-Hızlandıran BigData Analytics](https://azuremarketplace.microsoft.com/marketplace/apps/impetus-infotech-india-pvt-ltd.impetus_jumbune) |Hadoop, Kıvılcım |Yüksek düzeyde, Jumbune tarafından işletmeler, 1 yardımcı olur. Tez, MapReduce & Spark motor tabanlı Hive, Java, Scala iş yükü performansı hızlandırılması. 2. Proaktif Hadoop Küme İzleme, 3. Dağıtılmış dosya sisteminde Veri Kalitesi yönetiminin oluşturulması. |
|[Kyligence Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/kyligence.kyligence) |Hadoop, HBase, Kıvılcım |Apache Kylin tarafından desteklenmektedir, Kyligence Enterprise Büyük Veri BI sağlar. Hadoop bir kurumsal OLAP motoru olarak, Kyligence Enterprise endüstri standardı veri ambarı ve BI metodolojisi ile Hadoop mimar BI iş analisti güçlendirir. |
|[Azure HDInsight için Starburst Presto](https://azuremarketplace.microsoft.com/marketplace/apps/starburst.starburst-presto) |Hadoop |Presto hızlı ve ölçeklenebilir dağıtılmış SQL sorgu motorudur. Depolama ve bilgi işlem ayrımı için tasarlanan Presto, Azure Veri Gölü Depolama, Azure Blob Depolama, SQL ve NoSQL veritabanları ve diğer veri kaynaklarındaki verileri sorgulamak için idealdir. |
|[HDInsight Cloud için Akış Setleri Veri Toplayıcısı](https://azuremarketplace.microsoft.com/marketplace/apps/streamsets.streamsets-data-collector-hdinsight) |Hadoop, HBase, Kıvılcım, Kafka |StreamSets Data Collector, verileri gerçek zamanlı olarak aktaran hafif ve güçlü bir motordur. Veri akışlarınızdaki verileri yönlendirmek ve işlemek için Veri Toplayıcı'yı kullanın. 30 günlük deneme lisansı yla birlikte geliyor. |
|[Trifacta Wrangler Kurumsal](https://azuremarketplace.microsoft.com/marketplace/apps/trifacta.tr01) |Hadoop, Kıvılcım, HBase |HDInsight için Trifacta Wrangler Enterprise, herhangi bir veri ölçeği için kuruluş çapında veri çekişmesini destekler. Azure'da Trifacta çalıştırmanın maliyeti, Trifacta abonelik maliyetlerinin yanı sıra sanal makineler için Azure altyapı maliyetlerinin bir birleşimidir. |
|[Unifi Veri Platformu](https://unifisoftware.com/platform/) |Hadoop, HBase, Fırtına, Kıvılcım |Unifi Veri Platformu, iş kullanıcısını artan gelire yol açan, maliyetleri azaltan veya operasyonel karmaşıklığı azaltan veri zorluklarıyla başa çıkmak için güçlendirmek üzere tasarlanmış, sorunsuz bir şekilde entegre edilmiş bir self servis veri araçları paketidir. |
|[Unraveldata APM](https://azuremarketplace.microsoft.com/marketplace/apps/unravel-data.unravel-app) |Spark |HDInsight Spark kümesi için Veri uygulamasını çözün. |
|[Waterline AI Odaklı Veri Kataloğu](https://azuremarketplace.microsoft.com/marketplace/apps/waterline_data.waterline_data) |Spark |Waterline kataloglar, düzenler ve iş koşulları ile otomatik etiketlemek için AI kullanarak verileri yönetir. Waterline'ın işletme okur yazar kataloğu, self servis analitiği, uyumluluk ve yönetişim ve BT yönetimi girişimleri için kritik, başarı bileşenidir. |

Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Kaynak Yöneticisi şablonu dışa aktarabilir veya satıcılardan Kaynak Yöneticisi şablonunun bir kopyasını alabilir ve şablonu dağıtmak için Azure PowerShell ve Azure Classic CLI'yi kullanabilirsiniz.  Kaynak [Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturma'ya](hdinsight-hadoop-create-linux-clusters-arm-templates.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**HDInsight uygulaması yükleme**

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden Tüm **hizmetler** > **Analytics** > **HDInsight kümelerine**gidin.
3. Listeden bir HDInsight kümesi seçin.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md).
4. **Ayarlar** kategorisi altında **Uygulamalar'ı**seçin. Ana pencerede yüklü uygulamaların listesini görebilirsiniz. 
   
    ![HDInsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Menüden **+Ekle'yi** seçin. Kullanılabilir uygulamaların listesini görebilirsiniz.  **+Ekle** soluksa, HDInsight kümesinin bu sürümü için uygulama olmadığı anlamına gelir.
   
    ![HDInsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list1.png)
6. Kullanılabilir uygulamalardan birini seçin ve yasal koşulları kabul etmek için yönergeleri izleyin.

Yükleme durumunu portal bildirimlerinden görebilirsiniz (portalın üst kısmındaki çan simgesini seçin). Uygulama yüklendikten sonra, uygulama Yüklü Uygulamalar listesinde görünür.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme

Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Azure portalını kullanarak küme oluşturma sırasında uygulamaları **yapılandırma + fiyatlandırma** sekmesinden yüklemek için + Uygulama **Ekle'yi**seçin.

![Azure portal kümesi yapılandırma uygulamaları](./media/hdinsight-apps-install-applications/azure-portal-cluster-configuration-applications.png)

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**HDInsight uygulamasını ve görüntüleme özelliklerini listele**

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden Tüm **hizmetler** > **Analytics** > **HDInsight kümelerine**gidin.
3. Listeden bir HDInsight kümesi seçin.
4. **Ayarlar** kategorisi altında **Uygulamalar'ı**seçin. Ana pencerede yüklü uygulamaların listesini görebilirsiniz. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği göstermek için yüklü uygulamalardan birini seçin. Özellik listeleri:

    |Özellik | Açıklama |
    |---|---|
    |Uygulama adı |Başvuru adı. |
    |Durum |Başvuru durumu. |
    |Web sayfasının |Kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. |
    |SSH bitiş noktası |Kenar düğümüne bağlanmak için SSH'yi kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
    |Açıklama | Uygulama açıklaması. |

6. Bir uygulamayı silmek için uygulamayı sağ tıklatın ve ardından bağlam menüsünden **Sil'i** tıklatın.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun giderme
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel HDInsight uygulamalarını yükleyin:](hdinsight-apps-install-custom-applications.md)yayınlanmamış bir HDInsight uygulamasını HDInsight'a nasıl dağıtılayarak dağıtılamayı öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Linux tabanlı Apache Hadoop kümeleri oluşturun:](hdinsight-hadoop-create-linux-clusters-arm-templates.md)HDInsight kümeleri oluşturmak için Kaynak Yöneticisi şablonlarını nasıl çağıracağınız öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamaları test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.

