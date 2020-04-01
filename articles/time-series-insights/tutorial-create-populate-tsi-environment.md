---
title: 'Öğretici: Ortam oluşturma - Azure Time Series Öngörüleri | Microsoft Dokümanlar'
description: Benzetimli aygıtlardan gelen verilerle dolu bir Time Series Öngörüleri ortamını nasıl oluşturursun öğrenin.
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 7bebc9e682f5156fa235b77ff020e502695a28be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76981200"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Öğretici: Azure Time Series Insights ortamı oluşturma

Bu öğretici, simüle edilmiş aygıtlardan gelen verilerle dolu bir Azure Time Series Insights ortamı oluşturma sürecinde size rehberlik eder. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Zaman Serisi Öngörüleri ortamı oluşturun.
> * IoT hub'ı içeren bir aygıt simülasyon çözümü oluşturun.
> * Time Series Insights ortamını IoT hub'ına bağlayın.
> * Zaman Serisi Öngörüleri ortamına veri akışı sağlamak için bir aygıt simülasyonu çalıştırın.
> * Benzetimli telemetri verilerini doğrulayın.

> [!IMPORTANT]
> Zaten bir [Azure aboneliğiniz](https://azure.microsoft.com/free/) yoksa ücretsiz bir Azure aboneliği için kaydolun.

## <a name="prerequisites"></a>Ön koşullar

* Azure oturum açma hesabınız da aboneliğin **Sahibi** rolünün bir üyesi olmalıdır. Daha fazla bilgi [için, rol tabanlı erişim denetimi ve Azure portalını kullanarak erişimi yönet'i](../role-based-access-control/role-assignments-portal.md)okuyun.

## <a name="review-video"></a>Videoyu incele

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Veri oluşturmak ve Time Series Insights ile başlamak için Azure IoT çözüm hızlandırıcısını nasıl kullanacağınızı öğrenin. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="overview"></a>Genel Bakış

Zaman Serisi Öngörüleri ortamı, aygıt verilerinin toplandığı ve depolandığı yerdir. Depolandıktan sonra, [Azure Zaman Serisi Öngörüleri explorer](time-series-quickstart.md) ve Time Series [Insights Query API](/rest/api/time-series-insights/ga-query-api) verileri sorgulamak ve analiz etmek için kullanılabilir.

Azure IoT Hub, verileri Azure bulutunuza güvenli bir şekilde bağlamak ve iletmek için öğreticideki tüm aygıtlar (simüle edilmiş veya fiziksel) tarafından kullanılan etkinlik kaynağıdır.

Bu öğretici, örnek telemetri verilerini oluşturmak ve IoT Hub'a aktarmak için bir [IoT çözüm hızlandırıcısı](https://www.azureiotsolutions.com) da kullanır.

>[!TIP]
> [IoT çözüm hızlandırıcıları, özel IoT](https://www.azureiotsolutions.com) çözümlerinin geliştirilmesini hızlandırmak için kullanabileceğiniz kurumsal dereceli önceden yapılandırılmış çözümler sağlar.

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

İlk olarak, Zaman Serisi Öngörüleri ortamınızı doldurmak için test verileri oluşturan aygıt simülasyon uytunu oluşturun.

1. Ayrı bir pencerede veya [sekmede, azureiotsolutions.com](https://www.azureiotsolutions.com)gidin. Aynı Azure abonelik hesabını kullanarak oturum açın ve **Aygıt Simülasyonu** hızlandırıcısını seçin.

   [![Cihaz Benzetimi hızlandırıcısını çalıştırma](media/tutorial-create-populate-tsi-environment/iot-solution-accelerators-landing-page.png)](media/tutorial-create-populate-tsi-environment/iot-solution-accelerators-landing-page.png#lightbox)

1.  **Şimdi Deneyin'i**seçin. Ardından, **Aygıt Simülasyonu Oluştur çözüm** sayfasına gerekli parametreleri girin.

   Parametre|Açıklama
   ---|---
   **Dağıtım adı** | Bu benzersiz değer, yeni bir kaynak grubu oluşturmak için kullanılır. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Azure aboneliği** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin.
   **Dağıtım seçenekleri** | Bu öğreticiye özgü yeni bir IoT hub'ı oluşturmak için **Yeni IoT Hub'ı sağlama'yı** seçin.
   **Azure konumu** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölgeyi belirtin.

   İşi nizi bitirdiğinde, çözümün Azure kaynaklarını sağlamak için **Oluştur'u** seçin. Bu işlemin tamamlanması 20 dakika kadar sürebilir.

   [![Cihaz benzetimi çözümünü sağlama](media/tutorial-create-populate-tsi-environment/iot-solution-accelerators-configuration.png)](media/tutorial-create-populate-tsi-environment/iot-solution-accelerators-configuration.png#lightbox)

1. Sağlama tamamlandıktan sonra, dağıtım durumunun **Provisioning'den** **Hazır'a**taşındığını bildiren iki güncelleştirme görüntülenir. 

   >[!IMPORTANT]
   > Çözüm hızlandırıcınızı henüz girmeyin! Daha sonra geri döneceğiniz için bu web sayfasını açık tutun.

   [![Cihaz benzetimi çözümünü sağlama tamamlandı](media/tutorial-create-populate-tsi-environment/iot-solution-accelerator-ready.png)](media/tutorial-create-populate-tsi-environment/iot-solution-accelerator-ready.png#lightbox)

1. Şimdi, Azure portalında yeni oluşturulan kaynakları inceleyin. Kaynak **grupları** sayfasında, son adımda sağlanan **Çözüm adı** kullanılarak yeni bir kaynak grubunun oluşturulduğuna dikkat edin. Aygıt simülasyonu için oluşturulan kaynaklara dikkat edin.

   [![Cihaz simülasyon kaynakları](media/tutorial-create-populate-tsi-environment/tsi-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/tsi-device-sim-solution-resources.png#lightbox)

## <a name="create-an-environment"></a>Ortam oluşturma

İkinci olarak, Azure aboneliğinizde bir Time Series Öngörüleri ortamı oluşturun.

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın. 
1. Üstteki menüden **+ Kaynak oluştur**'u seçin. 
1. Nesnelerin **İnterneti** kategorisini seçin ve ardından **Zaman Serisi Öngörüleri'ni**seçin. 

   [![Zaman Serisi Öngörüleri ortam kaynağını seçin](media/tutorial-create-populate-tsi-environment/tsi-create-new-environment.png)](media/tutorial-create-populate-tsi-environment/tsi-create-new-environment.png#lightbox)

1. Zaman **Serisi Öngörüler ortamı** sayfasında, gerekli parametreleri doldurun.

   Parametre|Açıklama
   ---|---
   **Ortam adı** | Time Series Insights ortamı için benzersiz bir ad seçin. Adlar Zaman Serisi Öngörüleri gezgini ve [Sorgu API'leri](https://docs.microsoft.com/rest/api/time-series-insights/ga-query)tarafından kullanılır.
   **Abonelik** | Abonelikler, Azure kaynaklarına yönelik kapsayıcılardır. Time Series Insights ortamını oluşturmak için bir abonelik seçin.
   **Kaynak grubu** | Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. Varolan bir kaynak grubu seçin veya Time Series Insights ortam kaynağı için yeni bir kaynak oluşturun.
   **Konum** | Time Series Insights ortamınız için bir veri merkezi bölgesi seçin. Ek gecikmeyi önlemek için, diğer IoT kaynaklarıyla aynı bölgede Zaman Serisi Öngörüleri ortamını oluşturun.
   **Katmanı** | Gerekli aktarım hızını seçin. **S1'i**seçin.
   **Kapasite** | Kapasite, seçilen SKU ile ilişkili giriş hızına ve depolama kapasitesine uygulanan çarpandır. Oluşturmadan sonra kapasiteyi değiştirebilirsiniz. **1**kapasite seçin.

   Tamamlandığında, bir sonraki adıma geçmek için **Sonraki: Olay Kaynağı'nı** seçin.

   [![Time Series Insights ortam kaynağı oluşturma](media/tutorial-create-populate-tsi-environment/tsi-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/tsi-create-resource-tsi-params.png#lightbox)

1. Şimdi, Zaman Serisi Öngörüleri ortamını Çözüm Hızlandırıcısı tarafından oluşturulan IoT hub'ına bağlayın. **Hub'ı ayarla** ' nın ' için `Select existing` Ardından, IoT Hub adını ayarlarken Çözüm Hızlandırıcı tarafından oluşturulan **IoT hub'ını**seçin.

   [![Zaman Serisi Öngörüleri ortamını oluşturulan IoT hub'ına bağlama](media/tutorial-create-populate-tsi-environment/tsi-create-resource-iot-hub.png)](media/tutorial-create-populate-tsi-environment/tsi-create-resource-iot-hub.png#lightbox)

   Son olarak, **Gözden Geçir + oluştur'u**seçin.

1. Dağıtımın tamamlanmasını izlemek için **Bildirimler** panelini kontrol edin. 

   [![Time Series Insights ortamının dağıtımı başarılı oldu](media/tutorial-create-populate-tsi-environment/create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="run-device-simulation"></a>Çalıştır cihazı simülasyonu

Dağıtım ve ilk yapılandırma tamamlanında, Zaman Serisi Öngörüleri ortamını [hızlandırıcı tarafından oluşturulan simüle edilmiş aygıtlardan](#create-a-device-simulation)alınan örnek verilerle doldurun.

IoT hub'ı ile birlikte, simüle edilmiş aygıt telemetrisi oluşturmak ve iletmek için bir Azure App Service web uygulaması oluşturuldu.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Gerekirse bu eğitimde kullandığınız Azure hesabını kullanarak yeniden oturum açın. "Aygıt Çözümünüzü" seçin ve ardından dağıtılan çözümünüzü başlatmak için **çözüm hızlandırıcınıza gidin.**

   [![Çözüm hızlandırıcıları panosu](media/tutorial-create-populate-tsi-environment/iot-solution-accelerator-ready.png)](media/tutorial-create-populate-tsi-environment/iot-solution-accelerator-ready.png#lightbox)

1. Cihaz simülasyonu web uygulaması, web uygulamasına Oturum Açma'yı ve **profil izninizi okumanızı** istenerek başlar. Bu izin, uygulamanın işleyişini desteklemek için gerekli kullanıcı profili bilgilerini almak için uygulama sağlar.

   [![Cihaz benzetimi web uygulaması onayı](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Seçin **+ Yeni simülasyon**. Simülasyon kurulum sayfası **yüklendikten** sonra gerekli parametreleri girin.

   Parametre|Açıklama
   ---|---
   **Hedef IoT Hub'ı** | **Önceden sağlanan IoT Hub'ı kullan'ı**seçin.
   **Cihaz modeli** | **Chiller'ı**seçin.
   **Cihaz sayısı**  | `10` **Tutarın**altına girin.
   **Telemetri sıklığı** | Saniyeler girin. `10`
   **Benzetim süresi** | **Son'u seçin:** ve dakikaları girin. `5`

   Bitirdikten sonra **Simülasyonu Başlat'ı**seçin. Simülasyon toplam 5 dakika sürer. Her 10 saniyede 1.000 simüle edilmiş cihazdan veri üretir. 

   [![Cihaz benzetimi ayarı](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Simülasyon çalışırken, saniye deki **toplam iletive** **iletilerin** yaklaşık her 10 saniyede bir güncelleştirilmelerine dikkat edin. Simülasyon yaklaşık 5 dakika sonra sona erer ve **Simülasyon kurulumu**için döndürür.

   [![Cihaz benzetimi çalışıyor](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Telemetri verilerini doğrulama

Bu son bölümde, telemetri verilerinin Zaman Serisi Öngörüleri ortamında oluşturulduğunu ve depolandığını doğrularsınız. Verileri doğrulamak için, telemetri verilerini sorgulamak ve çözümlemek için kullanılan Time Series Insights gezgininden yararlanın.

1. Zaman Serisi Öngörüleri ortamının kaynak grubuna **Genel Bakış** sayfasına geri dönün. Zaman Serisi Öngörüleri ortamını seçin.

   [![Zaman Serisi Öngörüler çevre kaynak grubu ve çevre](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Zaman Serisi Öngörüler ortamına **Genel Bakış** sayfasında, Time Series Insights explorer'ı açmak için Zaman **Serisi Öngörüler explorer URL'sini** seçin.

   [![Time Series Insights gezgini](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Zaman Serisi Öngörüleri explorer, Azure portal hesabınızı kullanarak yükler ve kimlik doğrulaması yapar. Başlangıçta, Zaman Serisi Öngörüleri ortamının simüle edilmiş telemetri verileriyle birlikte doldurulan grafik alanı görüntülenir. Daha dar bir zaman aralığını filtrelemek için sol üst köşedeki açılır bölümü seçin. Aygıt simülasyonunun süresini kaplayacak kadar büyük bir zaman aralığı girin. Sonra arama büyüteç seçin.

   [![Zaman Serisi Insights explorer zaman aralığı filtresi](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Zaman aralığının daraltlanması, grafiğin IoT hub'ına ve Time Series Insights ortamına farklı veri aktarım patlamalarına yakınlaştırmasına olanak tanır. Ayrıca, bulunan toplam olay sayısını gösteren sağ üst köşedeki **Akış tam** metnine de dikkat edin. Ayrıca grafikteki çizim taneciklerini denetlemek için **Interval boyutu** kaydırıcısını sürükleyebilirsiniz.

   [![Zaman Serisi Insights explorer zaman aralığı filtreli görünüm](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Son olarak, bir aralığı filtrelemek için bir bölgeyi sol tıklatabilirsiniz. Ardından, olay ayrıntılarını tabular **Olaylar** görünümünde göstermek için **Olayları Keşfet'e** sağ tıklayın ve kullanın.

   [![Zaman Serisi Insights explorer zaman aralığı filtreli görünüm ve olaylar](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici, Time Series Insights ortamını ve aygıt simülasyon çözümlerini desteklemek için çalışan birkaç Azure hizmeti oluşturur. Bunları kaldırmak için Azure portalına geri gidin.

Azure portalında soldaki menüden:

1. Kaynak **grupları** simgesini seçin. Ardından, Time Series Insights ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında **kaynak grubunu sil'i,** kaynak grubunun adını girin ve **Sil'i**seçin.

1. Kaynak **grupları** simgesini seçin. Ardından, aygıt simülasyonçözüm hızlandırıcısı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında **kaynak grubunu sil'i,** kaynak grubunun adını girin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Zaman Serisi Öngörüleri ortamı oluşturun.
> * IoT hub'ı içeren bir aygıt simülasyon çözümü oluşturun.
> * Time Series Insights ortamını IoT hub'ına bağlayın.
> * Zaman Serisi Öngörüleri ortamına veri akışı sağlamak için bir aygıt simülasyonu çalıştırın.
> * Benzetimli telemetri verilerini doğrulayın.

Artık kendi Time Series Öngörüleri ortamınızı nasıl oluşturabileceğinizi bildiğinize göre, Time Series Insights ortamından veri tüketen bir web uygulamasını nasıl oluşturabileceğinizi öğrenin:

> [!div class="nextstepaction"]
> [Barındırılan istemci SDK görselleştirme örneklerini okuyun](https://tsiclientsample.azurewebsites.net/)
