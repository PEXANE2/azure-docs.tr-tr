---
title: Azure Event Grid nedir?
description: Azure Olay Ağıt'ı ile işleyicilere bir kaynaktan olay verileri gönderin. Etkinlik tabanlı uygulamalar oluşturun ve Azure hizmetleriyle tümleştirin.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: overview
ms.date: 02/20/2020
ms.author: babanisa
ms.custom: seodec18
ms.openlocfilehash: 65fd9d06c8b25e0aff94429f8eb95bb922da330c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77505915"
---
# <a name="what-is-azure-event-grid"></a>Azure Event Grid nedir?

Azure Event Grid, olay temelli mimarilerle kolayca uygulamalar derlemenize olanak tanır. İlk olarak, abone olmak istediğiniz Azure kaynağını seçin ve ardından olayı göndermek için olay işleyicisine veya WebHook bitiş noktasına verin. Event Grid, depolama blobları ve kaynak grupları gibi Azure hizmetlerinden gelen olaylar için yerleşik destek sunar. Event Grid, özel konular kullanarak kendi olaylarınızı oluşturmanızı da destekler. 

Belirli olayları farklı uç noktalara yönlendirmek, birden fazla uç noktaya yayın yapmak ve olaylarınızın güvenilir bir şekilde teslim edildiğinden emin olmak üzere filtreleri kullanabilirsiniz.

Azure Olay Ağıtı, her bölgedeki birden çok hata etki alanına ve kullanılabilirlik bölgelerine (bunları destekleyen bölgelerde) yerel olarak yayılarak kullanılabilirliği en üst düzeye çıkarmak için dağıtılır. Olay Izgaratarafından desteklenen bölgelerin listesi için, [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)bakın.

Bu makalede Azure Event Grid’e genel bir bakış sağlanmıştır. Event Grid kullanmaya başlamak istiyorsanız bkz. [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md). 

![Kaynakların ve işleyicilerin Olay Izgara modeli](./media/overview/functional-model.png)

Bu resim, Olay Izgara'sının kaynakları ve işleyicileri nasıl bağdaştırdığını gösterir ve desteklenen tümleştirmelerin kapsamlı bir listesi değildir.

## <a name="event-sources"></a>Olay kaynakları

Kaynağın özellikleriyle ilgili tüm ayrıntılar ve ilgili makaleler için bkz. [olay kaynakları](event-sources.md). Şu anda Event Grid’e olay gönderme özelliği aşağıdaki Azure hizmetleri tarafından desteklenmektedir:

* [Azure abonelikleri (yönetim işlemleri)](event-sources.md#azure-subscriptions)
* [Konteyner Kayıt Defteri](event-sources.md#container-registry)
* [Özel konular](event-sources.md#custom-topics)
* [Etkinlik Hub'ları](event-sources.md#event-hubs)
* [IoT Hub](event-sources.md#iot-hub)
* [Anahtar Vault (önizleme)](event-sources.md#key-vault-preview)
* [Media Services](event-sources.md#media-services)
* [Kaynak grupları (yönetim işlemleri)](event-sources.md#resource-groups)
* [Service Bus](event-sources.md#service-bus)
* [Depolama Blobu](event-sources.md#storage)
* [Azure Haritalar](event-sources.md#maps)
* [Uygulama Yapılandırması](event-sources.md#app-configuration)
* [Sinyal R](event-sources.md#azure-signalr)
* [Makine Öğrenimi](event-sources.md#azure-machine-learning)

## <a name="event-handlers"></a>Olay işleyicileri

İşleyicinin özellikleriyle ilgili tüm ayrıntılar ve ilgili makaleler için bkz. [olay işleyicileri](event-handlers.md). Şu anda Event Grid’den olay işleme özelliği aşağıdaki Azure hizmetleri tarafından desteklenmektedir: 

* [Azure Otomasyonu](event-handlers.md#azure-automation)
* [Azure İşlevleri](event-handlers.md#azure-functions)
* [Etkinlik Hub'ları](event-handlers.md#event-hubs)
* [Karma Bağlantılar](event-handlers.md#hybrid-connections)
* [Logic Apps](event-handlers.md#logic-apps)
* [Güç Otomatikleştirme (Eskiden Microsoft Akışı olarak bilinir)](https://preview.flow.microsoft.com/connectors/shared_azureeventgrid/azure-event-grid/)
* [Service Bus](event-handlers.md#service-bus)
* [Sıra Depolama](event-handlers.md#queue-storage)
* [WebHooks](event-handlers.md#webhooks)

## <a name="concepts"></a>Kavramlar

Azure Event Grid’de başlangıç yapmanızı sağlayan beş kavram vardır:

* **Olaylar** - Ne olduğu.
* **Olay kaynakları** - Olayın gerçekleştiği yer.
* **Konu Başlıkları** - Yayımcıların olayları gönderdiği uç nokta.
* **Olay abonelikleri** - Olayları bazen birden fazla işleyiciye yönlendirmek için bitiş noktası veya yerleşik mekanizma. Abonelikler ayrıca işleyiciler tarafından gelen olayları akıllıca filtrelemek için de kullanılır.
* **Olay işleyicileri** - Olaya tepki veren uygulama ya da hizmet.

Bu kavramlar hakkında daha fazla bilgi için bkz. [Azure Event Grid’de Kavramlar](concepts.md).

## <a name="capabilities"></a>Özellikler

Azure Event Grid’in önemli özelliklerinden bazıları şunlardır:

* **Basitlik** - Azure kaynağınızdan herhangi bir olay işleyici ya da uç noktaya olayları hedeflemek için üzerine gelip tıklayın.
* **Gelişmiş filtreleme** - Olay işleyicilerinin yalnızca ilgili olayları aldığından emin olmak için olay türüne veya olay yayımlama yoluna filtre uygulayın.
* **Fan-out** - Etkinliğin kopyalarını gerektiği kadar yere göndermek için aynı etkinliğe birkaç uç noktaya abone olun.
* **Güvenilirlik** - Olayların teslim olduğundan emin olmak için üstel geri tepme ile 24 saat yeniden deneme.
* **Olay başına ödeme** - Yalnızca Event Grid’i kullandığınız miktar için ödeme yapın.
* **Yüksek aktarım hızı** - Saniyede milyonlarca olay desteği ile Event Grid’de yüksek hacimli iş yükleri oluşturun.
* **Yerleşik Olaylar** - Kaynak tarafından tanımlanan yerleşik olaylarla hızlıca çalışmaya başlayın.
* **Özel Etkinlikler** - Uygulamanızda özel etkinlikleri yönlendirmek, filtrelemek ve güvenilir bir şekilde sunmak için Olay Izgarasını kullanın.

Event Grid, Event Hubs ve Service Bus hizmetlerinin bir karşılaştırması için bkz. [İleti teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).

## <a name="what-can-i-do-with-event-grid"></a>Event Grid ile ne yapabilirim?

Azure Olay Idamı, sunucusuz, ops otomasyonunu ve [tümleştirme](https://azure.com/integration) çalışmalarını büyük ölçüde iyileştiren çeşitli özellikler sağlar: 

### <a name="serverless-application-architectures"></a>Sunucusuz uygulama mimarileri

![Sunucusuz uygulama mimarisi](./media/overview/serverless_web_app.png)

Event Grid, veri kaynaklarını ve olay işleyicilerini bağlar. Örneğin, blob depolama kapsayıcısına eklendiğinde görüntüleri çözümleyen sunucusuz bir işlevi tetiklemek için Olay Izgarasını kullanın. 

### <a name="ops-automation"></a>İşlem Otomasyonu

![Operasyon otomasyonu](./media/overview/Ops_automation.png)

Event Grid, otomasyonu hızlandırmanızı ve ilke uygulamayı basitleştirmenizi sağlar. Örneğin, sanal bir makine veya SQL veritabanı oluşturulduğunda Azure Otomasyonu'na bildirimde bulunmak için Olay Grid'i kullanın. Hizmet yapılandırmalarının uyumlu olup olmadığını otomatik olarak denetlemek, işlem araçlarına meta verileri koymak, sanal makineleri etiketlemek veya iş öğelerini dosyalamak için olayları kullanın.

### <a name="application-integration"></a>Uygulama tümleştirme

![Azure ile uygulama tümleştirmesi](./media/overview/app_integration.png)

Event Grid, uygulamanızı diğer hizmetlere bağlar. Örneğin, uygulamanızın olay verilerini Event Grid’e göndermek için özel bir konu başlığı oluşturup Azure ile doğrudan tümleştirme, güvenilir teslim ve gelişmiş yönlendirme özelliklerinden yararlanabilirsiniz. Veya, verileri her yerde, kod yazmadan işlemek için Logic Apps ile Olay Grid'i kullanabilirsiniz. 

## <a name="how-much-does-event-grid-cost"></a>Event Grid’in maliyeti ne kadardır?

Azure Event Grid, olay başına ödeme fiyatlandırma modeli kullanır, bu nedenle yalnızca kullandığınız kadar ödeme yaparsınız. Her ay ilk 100.000 işlem ücretsizdir. İşlemler olay girişi, abonelik teslim denemeleri, yönetim çağrıları ve konu sonekine göre filtreleme olarak tanımlanır. Ayrıntılar için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Blobu olaylarını yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Event Grid kullanarak depolama blobu olaylarına yanıt verin.
* [Özel olay oluşturma ve özel olaylara abone olma](custom-event-quickstart.md)  
  Azure Event Grid hızlı başlangıcını kullanarak hemen giriş yapın ve kendi özel olaylarınızı herhangi bir uç noktaya göndermeye başlayın.
* [Olay İşleyicisi olarak Logic Apps kullanma](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Event Grid tarafından gönderilen olaylara yanıt vermek üzere Logic Apps kullanarak uygulama derleme öğreticisi.
* [Büyük verileri bir veri ambarına akışla aktarma](event-grid-event-hubs-integration.md)  
  Azure İşlevleri’ni kullanarak Event Hubs’dan SQL Veri Ambarı’na veri akışı yapan öğretici.
* [Event Grid REST API başvurusu](/rest/api/eventgrid)  
  Olay Aboneliklerini yönetmek, yönlendirme ve filtreleme için referans içeriği sağlar.
