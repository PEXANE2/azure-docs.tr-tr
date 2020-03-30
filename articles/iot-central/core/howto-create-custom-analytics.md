---
title: Azure IoT Central'ı özel analitikle genişletin | Microsoft Dokümanlar
description: Bir çözüm geliştiricisi olarak, özel analiz ve görselleştirmeler yapmak için bir IoT Central uygulamasını yapılandırın. Bu çözüm Azure Databricks kullanır.
author: dominicbetts
ms.author: dobett
ms.date: 12/02/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 7c2c14a937b4ef55d0e5f71e7b20214428ecd68c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158206"
---
# <a name="extend-azure-iot-central-with-custom-analytics-using-azure-databricks"></a>Azure Veri Tuğlaları'nı kullanarak Azure IoT Merkezi'ni özel analizlerle genişletin

Bu nasıl yapılsın kılavuzu, bir çözüm geliştiricisi olarak, IoT Central uygulamanızı özel analitik ve görselleştirmelerle nasıl genişlettiğinizi gösterir. Örnek, IoT Merkezi telemetri akışını çözümlemek ve [kutu çizimleri](https://wikipedia.org/wiki/Box_plot)gibi görselleştirmeler oluşturmak için bir [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) çalışma alanı kullanır.

Bu nasıl yapılacaklar kılavuzu, IoT Central'ı yerleşik analiz [araçlarıyla](./howto-create-custom-analytics.md)yapabileceklerinin ötesine nasıl genişletebileceğinizi gösterir.

Bu nasıl yapılacağını kılavuzunda, nasıl öğrenilir:

* *Sürekli veri dışa aktarma*yı kullanarak Bir IoT Merkezi uygulamasından telemetri akışı.
* Aygıt telemetrisini analiz etmek ve çizmek için bir Azure Databricks ortamı oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliğine ihtiyacınız var.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

### <a name="iot-central-application"></a>IoT Merkezi uygulaması

[Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde aşağıdaki ayarları içeren bir IoT Merkezi uygulaması oluşturun:

| Ayar | Değer |
| ------- | ----- |
| Fiyatlandırma planı | Standart |
| Uygulama şablonu | Mağaza içi analiz – durum izleme |
| Uygulama adı | Varsayılanı kabul edin veya kendi adınızı seçin |
| URL'si | Varsayılanı kabul edin veya kendi benzersiz URL önekinizi seçin |
| Dizin | Azure Etkin Dizin kiracınız |
| Azure aboneliği | Azure aboneliğiniz |
| Bölge | Size en yakın bölge |

Bu makaledeki örnekler ve ekran **görüntüleri, ABD** bölgesini kullanır. Size yakın bir konum seçin ve tüm kaynaklarınızı aynı bölgede oluşturduğunuzdan emin olun.

Bu uygulama şablonu, telemetri gönderen iki simüle termostat aygıtı içerir.

### <a name="resource-group"></a>Kaynak grubu

Oluşturduğunuz diğer kaynakları içerecek şekilde **IoTCentralAnalysis** adlı [bir kaynak grubu oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.ResourceGroup) kullanın. Azure kaynaklarınızı IoT Central uygulamanızla aynı konumda oluşturun.

### <a name="event-hubs-namespace"></a>Event Hubs ad alanı

Aşağıdaki ayarlara sahip [bir Etkinlik Hub'ları ad alanı oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.EventHub) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Adı    | Ad alanı adınızı seçin |
| Fiyatlandırma katmanı | Temel |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | IoTCentralAnalysis |
| Konum | Doğu ABD |
| İşleme Birimleri | 1 |

### <a name="azure-databricks-workspace"></a>Azure Databricks çalışma alanı

Aşağıdaki [ayarlara sahip bir Azure Databricks Hizmeti oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.Databricks) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Çalışma alanı adı    | Çalışma alanı adınızı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | IoTCentralAnalysis |
| Konum | Doğu ABD |
| Fiyatlandırma Katmanı | Standart |

Gerekli kaynakları oluşturduğunuzda, **IoTCentralAnalysis** kaynak grubunuz aşağıdaki ekran görüntüsüne benzer:

![IoT Merkezi analiz kaynak grubu](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir olay hub'ına sürekli telemetri dışa aktarmak için bir IoT Merkezi uygulamasını yapılandırabilirsiniz. Bu bölümde, IoT Central uygulamanızdan telemetri almak için bir etkinlik merkezi oluşturursunuz. Etkinlik merkezi, işlem için Stream Analytics işinize telemetri yi teslim eder.

1. Azure portalında, Etkinlik Hub'larınız ad alanına gidin ve **+ Etkinlik Hub'ını**seçin.
1. Olay **merkezinizin adını centralexport**olarak adlandırın ve **Oluştur'u**seçin.
1. Ad alanınızdaki olay hub'ları listesinde **merkezi dışa aktarmayı**seçin. Ardından **Paylaşılan erişim ilkelerini**seçin.
1. **+ Ekle** öğesini seçin. **Dinle** iddiasıyla **Dinle** adlı bir ilke oluşturun.
1. İlke hazır olduğunda, listede seçin ve ardından **Bağlantı dizesi birincil anahtar** değerini kopyalayın.
1. Bu bağlantı dizesini not edin, daha sonra databricks not defterinizi olay merkezinden okunacak şekilde yapılandırDığınızda kullanırsınız.

Olay Hub'larınız ad alanınız aşağıdaki ekran görüntüsüne benzer:

![Event Hubs ad alanı](media/howto-create-custom-analytics/event-hubs-namespace.png)

## <a name="configure-export-in-iot-central"></a>IoT Central'da ihracatı yapılandırma

Azure [IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde, Contoso şablonundan oluşturduğunuz IoT Central uygulamasına gidin. Bu bölümde, uygulamayı, telemetriyi simüle edilmiş aygıtlarından olay merkezinize aktamak üzere yapılandırırsınız. Dışa aktarmayı yapılandırmak için:

1. **Veri Dışa Aktarma** sayfasına gidin, **+ Yeni**'yi ve ardından Azure **Etkinlik Hub'larını**seçin.
1. Dışa aktarmayı yapılandırmak için aşağıdaki ayarları kullanın ve ardından **Kaydet'i**seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Görünen Ad | Etkinlik Hub'larına Dışa Aktarma |
    | Etkin | Açık |
    | Event Hubs ad alanı | Olay Hub'larınız ad alanı adınız |
    | Olay hub'ı | merkezi ihracat |
    | Ölçümler | Açık |
    | Cihazlar | Kapalı |
    | Aygıt Şablonları | Kapalı |

![Veri dışa aktarma yapılandırması](media/howto-create-custom-analytics/cde-configuration.png)

Devam etmeden önce dışa aktarma durumunun **çalışmasını** bekleyin.

## <a name="configure-databricks-workspace"></a>Databricks çalışma alanını yapılandırma

Azure portalında Azure Databricks hizmetinize gidin ve **Çalışma Alanını Başlat'ı**seçin. Tarayıcınızda yeni bir sekme açılır ve çalışma alanınızda sizi işaretler.

### <a name="create-a-cluster"></a>Küme oluşturma

Azure **Databricks** sayfasında, ortak görevler listesinin altında **Yeni Küme'yi**seçin.

Kümenizi oluşturmak için aşağıdaki tablodaki bilgileri kullanın:

| Ayar | Değer |
| ------- | ----- |
| Küme Adı | merkezi analiz |
| Küme Modu | Standart |
| Databricks Runtime Sürümü | 5.5 LTS (Scala 2.11, Kıvılcım 2.4.3) |
| Python Sürümü | 3 |
| Otomatik ölçekletme etkinleştirme | Hayır |
| Dakikalar süren hareketsizlikten sonra sonlandırma | 30 |
| İşçi Tipi | Standard_DS3_v2 |
| Işçi | 1 |
| Sürücü Tipi | İşçi yle aynı |

Küme oluşturma birkaç dakika sürebilir, devam etmeden önce küme oluşturma tamamlanmasını bekleyin.

### <a name="install-libraries"></a>Kitaplıkları yükleme

**Kümeler** sayfasında, küme durumu **çalışana**kadar bekleyin.

Aşağıdaki adımlar, örneklem izinizin kümeye nasıl aktarıldığını gösterir:

1. **Kümeler** sayfasında, **merkezi analiz** etkileşimli kümenin durumu **çalışana**kadar bekleyin.

1. Kümeyi seçin ve ardından **Kitaplıklar** sekmesini seçin.

1. **Kitaplıklar** sekmesinde **Yeni Yükle'yi**seçin.

1. **Kitaplığı Yükle** sayfasında, kitaplık kaynağı olarak **Maven'i** seçin.

1. **Koordinatlar** metin kutusuna aşağıdaki değeri girin:`com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Kitaplığı kümeye yüklemek için **Yükle'yi** seçin.

1. Kitaplık durumu şimdi **yüklenir:**

    ![Kitaplık yüklendi](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Databricks not defteri alma

IoT Merkezi telemetrinizi analiz etmek ve görselleştirmek için Python kodunu içeren bir Databricks not defterini almak için aşağıdaki adımları kullanın:

1. Databricks ortamınızdaki **Çalışma Alanı** sayfasına gidin. Hesap adınızın yanındaki açılır dosyayı seçin ve ardından **Içe Aktar'ı**seçin.

1. URL'den içeri girmeyi seçin ve aşağıdaki adresi girin:[https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Not defterini almak için **Içe Aktar'ı**seçin.

1. Alınan not defterini görüntülemek için **Çalışma Alanı'nı** seçin:

    ![İthal defter](media/howto-create-custom-analytics/import-notebook.png)

1. Daha önce kaydettiğiniz Olay Hub'ları bağlantı dizesini eklemek için ilk Python hücresindeki kodu edin:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Çalışma analizi

Çözümlemesi çalıştırmak için not defterini kümeye eklemeniz gerekir:

1. **Ayrılmış'yı** seçin ve ardından **merkezi çözümleme** kümesini seçin.
1. Küme çalışmıyorsa başlatın.
1. Not defterini başlatmak için çalıştır düğmesini seçin.

Son hücrede bir hata görebilirsiniz. Bu durumda, önceki hücrelerin çalıştığını denetleyin, bazı verilerin depolama alanına yazılması için bir dakika bekleyin ve son hücreyi yeniden çalıştırın.

### <a name="view-smoothed-data"></a>Düzeltilmiş verileri görüntüleme

Not defterinde, cihaz türüne göre yuvarlanan ortalama nem çizimini görmek için 14 hücresine gidin. Akış telemetrisi geldiğinde bu çizim sürekli olarak güncellenir:

![Düzeltilmiş telemetri arsa](media/howto-create-custom-analytics/telemetry-plot.png)

Not defterindeki grafiği yeniden boyutlandırabilirsiniz.

### <a name="view-box-plots"></a>Kutu çizimlerini görüntüleme

Not defterinde, [kutu çizimlerini](https://en.wikipedia.org/wiki/Box_plot)görmek için hücre 20'ye aşağı kaydırın. Kutu çizimleri statik verilere dayanır, bu nedenle bunları güncelleştirmek için hücreyi yeniden çalıştırmanız gerekir:

![Kutu çizimleri](media/howto-create-custom-analytics/box-plots.png)

Not defterindeki çizimleri yeniden boyutlandırabilirsiniz.

## <a name="tidy-up"></a>Toparla

Bu nasıl yapılacağını toparlamak ve gereksiz maliyetleri önlemek için Azure portalındaki **IoTCentralAnalysis** kaynak grubunu silin.

Uygulama içindeki **Yönetim** sayfasından IoT Merkezi uygulamasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapIlir kılavuzunda, nasıl yapılacağını öğrendiniz:

* *Sürekli veri dışa aktarma*yı kullanarak Bir IoT Merkezi uygulamasından telemetri akışı.
* Telemetri verilerini çözümlemek ve çizmek için bir Azure Databricks ortamı oluşturun.

Artık özel analitik oluşturmayı bildiğinize göre, önerilen bir sonraki adım uygulamanızı nasıl [yöneteceklerini](howto-administer.md)öğrenmektir.
