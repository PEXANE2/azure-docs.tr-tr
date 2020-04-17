---
title: IoT hub etkinlik kaynağı nasıl eklenir - Azure Time Series Insights | Microsoft Dokümanlar
description: Time Series Insights ortamınıza nasıl bir IoT hub etkinlik kaynağı ekleyeceğinizi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: seodec18
ms.openlocfilehash: a0a2f703d9224b8b9dd77c80b2b6a7faee70f5bb
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538112"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>Time Series Insights ortamınıza bir IoT hub etkinlik kaynağı ekleyin

Bu makalede, Azure IoT Hub'ından Azure Zaman Serisi Öngörüleri ortamınıza veri okuyan bir etkinlik kaynağı eklemek için Azure portalının nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Bu makaledeki yönergeler hem Azure Time Series Insights GA hem de Time Series Insights Preview ortamlarına uygulanır.

## <a name="prerequisites"></a>Ön koşullar

* Azure [Zaman Serisi Öngörüleri ortamı](time-series-insights-update-create-environment.md)oluşturun.
* Azure [portalını kullanarak bir IoT hub'ı](../iot-hub/iot-hub-create-through-portal.md)oluşturun.
* IoT hub'ında etkin ileti olayları gönderiliyor olmalıdır.
* Zaman Serisi Öngörüleri ortamı için IoT hub'ında, tüketilmesi gereken özel bir tüketici grubu oluşturun. Her Time Series Insights etkinlik kaynağının, başka bir tüketiciyle paylaşılmayan kendi özel tüketici grubuna sahip olması gerekir. Birden çok okuyucu aynı tüketici grubundan olayları tüketirse, tüm okuyucular büyük olasılıkla hata lar sergiler. Ayrıntılar için [Azure IoT Hub geliştirici kılavuzunu](../iot-hub/iot-hub-devguide.md)okuyun.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>IoT hub'ınıza bir tüketici grubu ekleme

Uygulamalar, Azure IoT Hub'ından veri çekmek için tüketici gruplarını kullanır. IoT hub'ınızdaki verileri güvenilir bir şekilde okumak için, yalnızca bu Zaman Serisi Öngörüleri ortamı tarafından kullanılan özel bir tüketici grubu sağlayın.

IoT hub'ınıza yeni bir tüketici grubu eklemek için:

1. Azure [portalında](https://portal.azure.com)IoT hub'ınızı bulun ve açın.

1. **Ayarlar**altında **Yerleşik Uç Noktaları'nı**seçin ve ardından **Olaylar** bitiş noktasını seçin.

   [![Yap-in Bitiş Noktaları sayfasında Etkinlikler düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png#lightbox)

1. **Tüketici grupları**altında, tüketici grubu için benzersiz bir ad girin. Yeni bir etkinlik kaynağı oluştururken Zaman Serisi Öngörüleri ortamınızda bu aynı adı kullanın.

1. **Kaydet**’i seçin.

## <a name="add-a-new-event-source"></a>Yeni bir etkinlik kaynağı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Soldaki menüden **Tüm kaynaklar**'ı seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. **Ayarlar'ın**altında **Olay Kaynakları'nı**seçin ve sonra **Ekle'yi**seçin.

   [![Olay Kaynakları'nı seçin ve sonra Ekle düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png#lightbox)

1. Yeni **olay kaynağı** bölmesinde, **Olay kaynak adı**için, bu Zaman Serisi Öngörüleri ortamına özgü bir ad girin. Örneğin, **olay akışı**girin.

1. **Kaynak** **için, IoT Hub'ı**seçin.

1. **Alma seçeneği**için bir değer seçin:

   * Aboneliklerinizden birinde zaten bir IoT hub'ınız varsa, **kullanılabilir aboneliklerden IoT Hub'ını kullanın'ı**seçin. Bu seçenek en kolay yaklaşımdır.
   
     [![Yeni etkinlik kaynağı bölmesinde seçenekleri seçin](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png#lightbox)

    * Aşağıdaki **tabloda, kullanılabilir abonelikler seçeneğinden IoT Hub'ı kullanma** için gerekli olan özellikler açıklanmaktadır:

       [![Yeni olay kaynağı bölmesi - Kullanılabilir abonelikler seçeneğinden IoT Hub'ı kullan'da ayarlanan özellikler](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png#lightbox)

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik | İstenilen iot hub'ın ait olduğu abonelik. |
       | IoT hub adı | Seçili iot hub'ının adı. |
       | IoT hub ilkesi adı | Paylaşılan erişim ilkesini seçin. Paylaşılan erişim ilkesini IoT hub ayarları sekmesinde bulabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. Olay kaynağınızın paylaşılan erişim ilkesinin **hizmet bağlama** izinleri *olmalıdır.* |
       | IoT hub ilkesi anahtarı | Anahtar önceden doldurulmuş. |

    * IoT hub'ı aboneliklerinizin dışındaysa veya gelişmiş seçenekler seçmek istiyorsanız, **IoT Hub ayarlarını el ile sağlayın'ı**seçin.

      Aşağıdaki tabloda **IoT Hub ayarları el ile sağla**için gerekli özellikleri açıklanır:

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | İstenilen iot hub'ın ait olduğu abonelik. |
       | Kaynak grubu | IoT hub'ın oluşturulduğu kaynak grubu adı. |
       | IoT hub adı | IoT hub'ınızın adı. IoT hub'ınızı oluşturduğunuzda, IoT hub'ı için bir ad girdiniz. |
       | IoT hub ilkesi adı | Paylaşılan erişim ilkesi. IoT hub ayarları sekmesinde paylaşılan erişim ilkesini oluşturabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. Olay kaynağınızın paylaşılan erişim ilkesinin **hizmet bağlama** izinleri *olmalıdır.* |
       | IoT hub ilkesi anahtarı | Azure Hizmet Veri Servisi ad alanına erişim in kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin. |

    * Her iki seçenek de aşağıdaki yapılandırma seçeneklerini paylaşır:

       | Özellik | Açıklama |
       | --- | --- |
       | IoT hub tüketici grubu | Olayları IoT hub'ından okuyan tüketici grubu. Etkinlik kaynağınız için özel bir tüketici grubu kullanmanızı şiddetle öneririz. |
       | Olay serileştirme biçimi | Şu anda, JSON kullanılabilir tek serileştirme biçimidir. Olay iletileri bu biçimde olmalıdır veya hiçbir veri okunamaz. |
       | Zaman damgası özellik adı | Bu değeri belirlemek için, IoT hub'ına gönderilen ileti verilerinin ileti biçimini anlamanız gerekir. Bu değer, olay zaman damgası olarak kullanmak istediğiniz ileti verilerindeki belirli olay özelliğinin **adıdır.** Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa, olay kaynağındaki **olay enqueue zamanı** olay zaman damgası olarak kullanılır. |


1. IoT hub'ınıza eklediğiniz özel Time Series Insights tüketici grubu adını ekleyin.

1. **Oluştur**’u seçin.

1. Etkinlik kaynağını oluşturduktan sonra, Time Series Insights otomatik olarak ortamınıza veri akışı yapmaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

* Verileri güvence altına almak için [veri erişim ilkelerini tanımlayın.](time-series-insights-data-access.md)

* Olayları olay kaynağına [gönderin.](time-series-insights-send-events.md)

* [Zaman Serisi Öngörüler kaşifinde](https://insights.timeseries.azure.com)ortamınıza erişin.
