---
title: Etkinlik Hub'ları etkinlik kaynağı ekleme - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Zaman Serisi Öngörüleri ortamınıza Azure Etkinlik Hub'ları etkinlik kaynağını nasıl ekleyeceğinizi öğrenin.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407452"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Time Series Insights ortamınıza etkinlik merkezi etkinlik kaynağı ekleme

Bu makalede, Azure Etkinlik Hub'larından Azure Zaman Serisi Öngörüleri ortamınıza veri okuyan bir etkinlik kaynağı eklemek için Azure portalının nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Bu makalede açıklanan adımlar hem Zaman Serisi Öngörüleri GA hem de Zaman Serisi Öngörüleri Önizleme ortamlarına uygulanır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Zaman Serisi Öngörüleri Oluştur ortamında açıklandığı gibi Bir Zaman [Serisi Öngörüleri ortamı oluşturun.](./time-series-insights-update-create-environment.md)
- Bir olay hub'ı oluşturun. [Azure portalını kullanarak Etkinlik Hub'ları ad alanı ve etkinlik merkezi oluşturun'u](../event-hubs/event-hubs-create.md)okuyun.
- Olay hub'ında etkin ileti olayları nın gönderilmesi gerekir. [.NET Framework'u kullanarak etkinlikleri Azure Etkinlik Hub'larına](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md)nasıl göndereceğinizi öğrenin.
- Zaman Serisi Öngörüleri ortamının tüketebileceği etkinlik merkezinde özel bir tüketici grubu oluşturun. Her Time Series Insights etkinlik kaynağının, başka bir tüketiciyle paylaşılmayan kendi özel tüketici grubuna sahip olması gerekir. Birden çok okuyucu aynı tüketici grubundan olayları tüketirse, tüm okuyucular büyük olasılıkla hata lar sergiler. Etkinlik merkezi başına 20 tüketici grubu sınırı vardır. Ayrıntılar için [Olay Hubları programlama kılavuzunu](../event-hubs/event-hubs-programming-guide.md)okuyun.

### <a name="add-a-consumer-group-to-your-event-hub"></a>Etkinlik merkezinize bir tüketici grubu ekleme

Uygulamalar, Azure Etkinlik Hub'larından veri çekmek için tüketici gruplarını kullanır. Etkinlik merkezinizdeki verileri güvenilir bir şekilde okumak için, yalnızca bu Zaman Serisi Öngörüleri ortamı tarafından kullanılan özel bir tüketici grubu sağlayın.

Etkinlik merkezinize yeni bir tüketici grubu eklemek için:

1. Azure [portalında,](https://portal.azure.com)etkinlik merkezi ad alanınızın **Genel Bakış** bölmesinden olay merkezi örneğini bulun ve açın. **Etkinlik Hub'ları > Varlıklar'ı** seçin veya **Adınızı**n altında örneğinizi bulun.

    [![Etkinlik merkezinizin ad alanını açma](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-connect-event-hub-namespace.png#lightbox)

1. Olay merkezi örneğinde, **Tüketici grupları > Varlıklar'ı**seçin. Ardından, yeni bir tüketici grubu eklemek için **+ Tüketici grubu'nun** 

   [![Etkinlik merkezi - Tüketici grubu ekleme](media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/add-event-hub-consumer-group.png#lightbox)

   Aksi takdirde, varolan bir tüketici grubu seçin ve bir sonraki bölüme atlayın.

1. Tüketici **grupları** sayfasına, **Ad**için yeni bir benzersiz değer girin.  Zaman Serisi Öngörüleri ortamında yeni bir olay kaynağı oluştururken bu aynı adı kullanın.

1. **Oluştur**’u seçin.

## <a name="add-a-new-event-source"></a>Yeni bir etkinlik kaynağı ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Öngörüleri ortamınızı bulun. Sol menüde **Tüm kaynakları**seçin ve ardından Time Series Öngörüleri ortamınızı seçin.

1. **Olay Kaynakları'nı**seçin ve sonra **Ekle'yi**seçin.

   [![Olay Kaynakları altında Ekle düğmesini seçin](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-add-an-event-source.png#lightbox)

1. Bu Zaman Serisi Öngörüleri ortamına özgü **Olay kaynak adı** `Contoso-TSI-GA-Event-Hub-ES`için bir değer girin, örneğin.

1. **Kaynak**için **Olay Hub'ı'nı**seçin.

1. **Alma seçeneği**için uygun değerleri seçin:

   * Aboneliklerinizden birinde varolan bir etkinlik hub'ınız **varsa, kullanılabilir aboneliklerden Etkinlik Merkezini Kullan'ı**seçin. Bu seçenek en kolay yaklaşımdır.

     [![Olay Kaynağı alma seçeneği ni seçin](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-event-hub-select-import-option.png#lightbox)

    *  Aşağıdaki **tabloda, kullanılabilir abonelikler seçeneğinden Kullanım Etkinliği Hub'ı** için gerekli özellikler açıklanmaktadır:

       [![Abonelik ve etkinlik merkezi ayrıntıları](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/tsi-configure-create-confirm.png#lightbox)

       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik | İstenilen olay merkezi örneği ve ad alanı aboneliği aittir. |
       | Olay hub'ı ad alanı | İstenilen olay göbeği adı alanı, ait. |
       | Olay Hub'ı adı | İstenilen olay hub örneğinin adı. |
       | Olay Hub ilke değeri | İstenilen paylaşılan erişim ilkesini seçin. Olay merkezi Yapılandırma sekmesinde paylaşılan erişim **ilkesini** oluşturabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. Etkinlik kaynağınızın paylaşılan erişim ilkesinin **okuma** izinleri *olması gerekir.* |
       | Olay Hub ilkesi anahtarı | Seçili Olay Hub ilkesi değerinden önceden doldurulmuş. |

    * Etkinlik merkezi aboneliklerinizin dışındaysa veya gelişmiş seçenekler seçmek **istiyorsanız, Etkinlik Hub ayarlarını el ile sağlayın'ı**seçin.

       Aşağıdaki tabloda **Olay Hub'ı sağla seçenekleri için** gerekli özellikler açıklanmaktadır:
 
       | Özellik | Açıklama |
       | --- | --- |
       | Abonelik Kimliği | İstenilen olay merkezi örneği ve ad alanı aboneliği aittir. |
       | Kaynak grubu | Kaynak grubu istenilen olay hub örneği ve ad alanı aittir. |
       | Olay hub'ı ad alanı | İstenilen olay göbeği adı alanı, ait. |
       | Olay Hub'ı adı | İstenilen olay hub örneğinin adı. |
       | Olay Hub ilke değeri | İstenilen paylaşılan erişim ilkesini seçin. Olay merkezi Yapılandırma sekmesinde paylaşılan erişim **ilkesini** oluşturabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. Etkinlik kaynağınızın paylaşılan erişim ilkesinin **okuma** izinleri *olması gerekir.* |
       | Olay Hub ilkesi anahtarı | Hizmet Veri Servisi ad alanına erişimi doğrulamak için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin. |

    * Her iki seçenek de aşağıdaki yapılandırma seçeneklerini paylaşır:

       | Özellik | Açıklama |
       | --- | --- |
       | Olay Hub'ı tüketici grubu | Olayları olay merkezinden okuyan tüketici grubu. Etkinlik kaynağınız için özel bir tüketici grubu kullanmanızı şiddetle öneririz. |
       | Olay serileştirme biçimi | Şu anda, JSON kullanılabilir tek serileştirme biçimidir. Olay iletileri bu biçimde olmalıdır veya veriler okunamıyor. |
       | Zaman damgası özellik adı | Bu değeri belirlemek için, olay merkezine gönderilen ileti verilerinin ileti biçimini anlamanız gerekir. Bu değer, olay zaman damgası olarak kullanmak istediğiniz ileti verilerindeki belirli olay özelliğinin **adıdır.** Değer büyük/küçük harf duyarlıdır. Boş bırakılırsa, olay kaynağındaki **olay enqueue zamanı** olay zaman damgası olarak kullanılır. |

1. Etkinlik merkezinize eklediğiniz özel Time Series Insights tüketici grubu adını ekleyin.

1. **Oluştur**’u seçin.

   Etkinlik kaynağı oluşturulduktan sonra, Time Series Insights otomatik olarak ortamınıza veri akışı başlar.

## <a name="next-steps"></a>Sonraki adımlar

* Verileri güvence altına almak için [veri erişim ilkelerini tanımlayın.](time-series-insights-data-access.md)

* Olayları olay kaynağına [gönderin.](time-series-insights-send-events.md)

* [Zaman Serisi Öngörüler kaşifinde](https://insights.timeseries.azure.com)ortamınıza erişin.
