---
title: Event Hubs olay kaynağı ekleme-Azure Time Series Insights | Microsoft Docs
description: Time Series Insights ortamınıza Azure Event Hubs olay kaynağı eklemeyi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/15/2020
ms.custom: seodec18
ms.openlocfilehash: 021ac5fccf4d694895ab9941bd46dd2388f49af9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81407452"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Time Series Insights ortamınıza bir olay hub 'ı olay kaynağı ekleme

Bu makalede, Azure Event Hubs verileri Azure Time Series Insights ortamınıza okuyan bir olay kaynağı eklemek için Azure portal nasıl kullanılacağı açıklanır.

> [!NOTE]
> Bu makalede açıklanan adımlar, Time Series Insights GA ve Time Series Insights önizleme ortamları için de geçerlidir.

## <a name="prerequisites"></a>Önkoşullar

- [Azure Time Series Insights ortamı oluşturma](./time-series-insights-update-create-environment.md)bölümünde açıklandığı gibi bir Time Series Insights ortamı oluşturun.
- Bir olay hub'ı oluşturun. [Azure Portal kullanarak Event Hubs ad alanı ve Olay Hub 'ı oluşturun](../event-hubs/event-hubs-create.md).
- Olay Hub 'ına, etkin ileti olayları gönderilmesi gerekir. [.NET Framework kullanarak olayları Azure Event Hubs gönderme](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md)hakkında bilgi edinin.
- Time Series Insights ortamının tüketebileceği Olay Hub 'ında adanmış bir tüketici grubu oluşturun. Her bir Time Series Insights olay kaynağı, başka bir tüketici ile paylaşılmayan kendi adanmış bir tüketici grubuna sahip olmalıdır. Birden çok okuyucu aynı tüketici grubundan olayları tükettiği takdirde, tüm okuyucular hatalara neden olabilir. Olay Hub 'ı başına 20 Tüketici grubu sınırı vardır. Ayrıntılar için [Event Hubs programlama kılavuzunu](../event-hubs/event-hubs-programming-guide.md)okuyun.

### <a name="add-a-consumer-group-to-your-event-hub"></a>Olay Hub 'ınıza bir tüketici grubu ekleme

Uygulamalar, Azure Event Hubs verileri çekmek için tüketici gruplarını kullanır. Olay Hub 'ından verileri güvenle okumak için, yalnızca bu Time Series Insights ortamı tarafından kullanılan ayrılmış bir tüketici grubu sağlayın.

Olay Hub 'ınıza yeni bir tüketici grubu eklemek için:

1. [Azure Portal](https://portal.azure.com), Olay Hub 'ınızın ad alanının **genel bakış** bölmesinden Olay Hub örneğinizi bulun ve açın. **> Event Hubs varlıkları** seçin veya örneğinizi **ad**altında bulun.

    [![Olay Hub 'ı ad alanınızı açın](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png#lightbox)

1. Olay Hub örneğiniz **>, tüketici grupları**' nı seçin. Ardından, yeni bir tüketici grubu eklemek için **+ Tüketici grubu** ' nu seçin. 

   [![Olay Hub 'ı-Tüketici grubu ekleme](media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png#lightbox)

   Aksi takdirde, mevcut bir tüketici grubunu seçin ve sonraki bölüme atlayın.

1. **Tüketici grupları** sayfasında, **ad**için yeni bir benzersiz değer girin.  Time Series Insights ortamında yeni bir olay kaynağı oluştururken aynı adı kullanın.

1. **Oluştur**'u seçin.

## <a name="add-a-new-event-source"></a>Yeni bir olay kaynağı ekleyin

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Sol taraftaki menüden **tüm kaynaklar**' ı ve ardından Time Series Insights ortamınızı seçin.

1. **Olay kaynakları**' nı seçin ve ardından **Ekle**' yi seçin.

   [![Olay kaynakları altında Ekle düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png#lightbox)

1. Bu Time Series Insights ortamı için benzersiz olan **olay kaynağı adı** için bir değer girin, örneğin `Contoso-TSI-GA-Event-Hub-ES` .

1. **Kaynak**Için **Olay Hub**'ı ' nı seçin.

1. **Içeri aktarma seçeneği**için uygun değerleri seçin:

   * Aboneliklerinizden birinde mevcut bir olay hub 'ınız varsa, **kullanılabilir aboneliklerden Olay Hub 'ını kullan**' ı seçin. Bu seçenek en kolay yaklaşımdır.

     [![Bir olay kaynağı içeri aktarma seçeneği seçin](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png#lightbox)

    *  Aşağıdaki tabloda, **kullanılabilir aboneliklerden Event hub 'ı kullan** seçeneği için gereken özellikler açıklanmaktadır:

       [![Abonelik ve Olay Hub 'ı ayrıntıları](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png#lightbox)

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik | Abonelik, istenen Olay Hub 'ı örneği ve ad alanı öğesine aittir. |
       | Olay hub'ı ad alanı | İstenen olay hub 'ının örneğine ait olan olay hub 'ı ad alanı. |
       | Olay Hub'ı adı | İstenen olay hub 'ının adı. |
       | Olay Hub 'ı ilke değeri | İstenen paylaşılan erişim ilkesini seçin. Paylaşılan erişim ilkesini Event hub 'ı **Yapılandır** sekmesinde oluşturabilirsiniz. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. Olay kaynağınız için paylaşılan erişim ilkesinin **okuma** izinleri *olmalıdır* . |
       | Olay Hub 'ı ilke anahtarı | Seçili olay hub 'ı ilke değerinden önceden dolduruldu. |

    * Olay Hub 'ı abonelikleriniz dışında ise veya Gelişmiş Seçenekler ' i seçmek istiyorsanız, **Olay Hub 'ı ayarlarını el Ile sağla**' yı seçin.

       Aşağıdaki tabloda, **Olay Hub 'ı ayarlarının el Ile sağlanması** için gereken özellikler açıklanmaktadır:
 
       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | Abonelik, istenen Olay Hub 'ı örneği ve ad alanı öğesine aittir. |
       | Kaynak grubu | İstenen olay hub 'ı örneğinin ve ad alanının ait olduğu kaynak grubu. |
       | Olay hub'ı ad alanı | İstenen olay hub 'ının örneğine ait olan olay hub 'ı ad alanı. |
       | Olay Hub'ı adı | İstenen olay hub 'ının adı. |
       | Olay Hub 'ı ilke değeri | İstenen paylaşılan erişim ilkesini seçin. Paylaşılan erişim ilkesini Event hub 'ı **Yapılandır** sekmesinde oluşturabilirsiniz. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. Olay kaynağınız için paylaşılan erişim ilkesinin **okuma** izinleri *olmalıdır* . |
       | Olay Hub 'ı ilke anahtarı | Service Bus ad alanına erişimin kimliğini doğrulamak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin. |

    * Her iki seçenek de aşağıdaki yapılandırma seçeneklerini paylaşır:

       | Özellik | Açıklama |
       | --- | --- |
       | Olay Hub'ı tüketici grubu | Olay Hub 'ından olayları okuyan Tüketici grubu. Olay kaynağınız için adanmış bir tüketici grubu kullanmanızı önemle tavsiye ederiz. |
       | Olay serileştirme biçimi | Şu anda JSON tek kullanılabilir serileştirme biçimidir. Olay iletilerinin bu biçimde olması veya verilerin okunamaz olması gerekir. |
       | Zaman damgası özellik adı | Bu değeri öğrenmek için, Olay Hub 'ına gönderilen ileti verilerinin ileti biçimini anlamanız gerekir. Bu değer, olay zaman damgası olarak kullanmak istediğiniz ileti verilerinde belirli olay özelliğinin **adıdır** . Değer, büyük/küçük harfe duyarlıdır. Boş bırakılırsa olay kaynağı olay **sıraya alma süresi** olay zaman damgası olarak kullanılır. |

1. Olay Hub 'ınıza eklediğiniz adanmış Time Series Insights Tüketici grubu adını ekleyin.

1. **Oluştur**'u seçin.

   Olay kaynağı oluşturulduktan sonra, Time Series Insights ortamınızda akış verilerini otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* Verilerin güvenliğini sağlamak için [veri erişim Ilkeleri tanımlayın](time-series-insights-data-access.md) .

* [Olayları](time-series-insights-send-events.md) olay kaynağına gönderin.

* [Time Series Insights Gezgini](https://insights.timeseries.azure.com)' nde ortamınıza erişin.
