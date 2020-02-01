---
title: IoT Hub olay kaynağı ekleme-Azure Time Series Insights | Microsoft Docs
description: Time Series Insights ortamınıza IoT Hub olay kaynağı eklemeyi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/30/2020
ms.custom: seodec18
ms.openlocfilehash: 3ea73e2ca20faea30294bc5d5e1788415095c39f
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905374"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>Time Series Insights ortamınıza IoT Hub olay kaynağı ekleme

Bu makalede, Azure IoT Hub verileri Azure Time Series Insights ortamınıza okuyan bir olay kaynağı eklemek için Azure portal nasıl kullanılacağı açıklanır.

> [!NOTE]
> Bu makaledeki yönergeler Azure Time Series Insights GA 'ye ve önizleme ortamlarını Time Series Insights için geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

* [Azure Time Series Insights ortamı](time-series-insights-update-create-environment.md)oluşturun.
* [Azure Portal kullanarak bir IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md)oluşturun.
* IoT Hub 'ında etkin ileti olayları gönderilmesi gerekir.
* IoT Hub 'ında kullanacağı Time Series Insights ortamı için özel bir tüketici grubu oluşturun. Her zaman serisi görüşleri olay kaynağı, diğer bir tüketici ile paylaşılmaz kendi adanmış bir tüketici grubu olması gerekir. Birden çok okuyucu aynı tüketici grubundan olayları tükettiği takdirde, tüm okuyucular hatalara neden olabilir. Ayrıntılar için [Azure IoT Hub Geliştirici Kılavuzu](../iot-hub/iot-hub-devguide.md)' nu okuyun.

### <a name="add-a-consumer-group-to-your-iot-hub"></a>IoT Hub 'ınıza bir tüketici grubu ekleme

Uygulamalar, Azure IoT Hub verileri çekmek için tüketici gruplarını kullanır. IoT Hub 'ınızdaki verileri güvenle okumak için, yalnızca bu Time Series Insights ortamı tarafından kullanılan ayrılmış bir tüketici grubu sağlayın.

IoT Hub 'ınıza yeni bir tüketici grubu eklemek için:

1. [Azure Portal](https://portal.azure.com)IoT Hub 'ınızı bulun ve açın.

1. **Ayarlar**' ın altında, **yerleşik uç noktalar**' ı seçin ve ardından **Olaylar** uç noktasını seçin.

   [Derleme uç noktaları sayfasında ![olaylar düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png#lightbox)

1. **Tüketici grupları**' nın altında, Tüketici grubu için benzersiz bir ad girin. Yeni bir olay kaynağı oluştururken Time Series Insights ortamınızda aynı adı kullanın.

1. **Kaydet**’i seçin.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. Soldaki menüden **Tüm kaynaklar**'ı seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. **Ayarlar**altında **olay kaynakları**' nı ve ardından **Ekle**' yi seçin.

   [Olay kaynaklarını seçin ![ve ardından Ekle düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png#lightbox)

1. **Yeni olay kaynağı** bölmesinde, **olay kaynağı adı**için, bu Time Series Insights ortamına özgü bir ad girin. Örneğin, **olay akışını**girin.

1. **Kaynak**için **IoT Hub**seçin.

1. **Içeri aktarma seçeneği**için bir değer seçin:

   * Aboneliklerinizden birinde bir IoT Hub 'ınız zaten varsa, **kullanılabilir aboneliklerden IoT Hub kullan**' ı seçin. Bu seçenek için kolay bir yaklaşımdır.
   
     [Yeni olay kaynağı bölmesindeki seçenekleri seçin ![](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png#lightbox)

    * Aşağıdaki tabloda, **kullanılabilir abonelikler arasından IoT Hub kullan** seçeneği için gereken özellikler açıklanmaktadır:

       [Yeni olay kaynağı bölmesi ![-kullanılabilir aboneliklerden IoT Hub kullan seçeneğinde ayarlanacak Özellikler](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png#lightbox)

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik | İstenen IoT Hub 'ına ait abonelik. |
       | IoT Hub adı | Seçilen IoT Hub 'ının adı. |
       | IoT Hub ilkesi adı | Paylaşılan erişim ilkesi seçin. Paylaşılan erişim ilkesini IoT Hub ayarları sekmesinde bulabilirsiniz. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. Olay kaynağınıza ait paylaşılan erişim ilkesinde **hizmet bağlantısı** izinleri *olmalıdır* . |
       | IoT Hub ilke anahtarı | Anahtar önceden doldurulur. |

    * IoT Hub 'ı abonelikleriniz için ise veya Gelişmiş Seçenekler ' i seçmek istiyorsanız, **IoT Hub ayarlarını el Ile sağla**' yı seçin.

      Aşağıdaki tabloda, **IoT Hub ayarlarının el Ile sağlanması**için gereken özellikler açıklanmaktadır:

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | İstenen IoT Hub 'ına ait abonelik. |
       | Kaynak grubu | IoT Hub 'ının oluşturulduğu kaynak grubu adı. |
       | IoT Hub adı | IoT Hub 'ınızın adı. IoT Hub 'ınızı oluşturduğunuzda IoT Hub için bir ad girdiniz. |
       | IoT Hub ilkesi adı | Paylaşılan erişim ilkesi. Paylaşılan erişim ilkesini IoT Hub ayarları sekmesinde oluşturabilirsiniz. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. Olay kaynağınıza ait paylaşılan erişim ilkesinde **hizmet bağlantısı** izinleri *olmalıdır* . |
       | IoT Hub ilke anahtarı | Azure Service Bus ad alanına erişimin kimliğini doğrulamak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin. |

    * Her iki seçenek de aşağıdaki yapılandırma seçeneklerini paylaşır:

       | Özellik | Açıklama |
       | --- | --- |
       | IoT Hub 'ı Tüketici grubu | IoT Hub 'ından olayları okuyan Tüketici grubu. Olay kaynağınız için ayrılmış bir tüketici grubu kullanmanızı öneririz. |
       | Olay serileştirme biçimi | Şu anda, JSON yalnızca serileştirme biçimidir. Olay iletileri bu biçimde olmalıdır veya hiçbir veri okunamaz. |
       | Zaman damgası özellik adı | Bu değeri öğrenmek için, IoT Hub 'ına gönderilen ileti verilerinin ileti biçimini anlamanız gerekir. Bu değer **adı** ileti verileri, olay zaman damgası kullanmak istediğiniz belirli olay özelliğini. Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa **olay sıraya alma süresi** olay kaynağı olarak olay zaman damgası kullanılır. |


1. IoT Hub 'ınıza eklediğiniz adanmış Time Series Insights Tüketici grubu adını ekleyin.

1. **Oluştur**’u seçin.

1. Olay kaynağını oluşturduktan sonra, Time Series Insights ortamınızda akış verilerini otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri erişim ilkelerini tanımlama](time-series-insights-data-access.md) verilerin güvenliğini sağlamak için.

* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.

* Ortamınızda erişim [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
