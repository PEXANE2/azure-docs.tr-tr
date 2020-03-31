---
title: Kesintilere ve felaketlere karşı Azure Servis Veri Servisi uygulamalarını yalıtın
description: Bu makaleler, uygulamaları olası bir Azure Hizmet Veri Aracı kesintisine karşı korumak için teknikler sağlar.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: 2a7f5d5eacb2d03e64ae95d34e1cf0bd37bbc7f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259259"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Uygulamaları Service Bus kesintilerine ve olağanüstü durumlarına karşı dayanıklı hale getirmek için en iyi yöntemler

Görev açısından kritik uygulamalar, planlanmamış kesintiler veya felaketler durumunda bile sürekli olarak çalışmalıdır. Bu makalede, Servis Veri Aracı uygulamalarını olası bir hizmet kesintisine veya felakete karşı korumak için kullanabileceğiniz teknikler açıklanmaktadır.

Kesinti, Azure Hizmet Veri Tos'unun geçici olarak kullanılamaması olarak tanımlanır. Kesinti, ileti deposu ve hatta tüm veri merkezi gibi Servis Veri Çileti'nin bazı bileşenlerini etkileyebilir. Sorun giderildikten sonra Servis Otobüsü yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletilerin veya diğer verilerin kaybına neden olmaz. Bileşen hatasına örnek olarak, belirli bir ileti deposunun kullanılamaması örnektir. Veri merkezi genelinde ki kesintiye örnek olarak veri merkezinin güç arızası veya hatalı bir veri merkezi ağ anahtarı dır. Bir kesinti birkaç dakika ile birkaç gün arasında sürebilir.

Felaket, Servis Veri Hizmeti ölçeği biriminin veya veri merkezinin kalıcı kaybı olarak tanımlanır. Veri merkezi yeniden kullanılabilir olabilir veya olmayabilir. Genellikle bir felaket, iletilerin veya diğer verilerin bir kısmının veya tamamının kaybolmasına neden olur. Felaketlere örnek olarak yangın, sel veya deprem verilebilir.

## <a name="protecting-against-outages-and-disasters---service-bus-premium"></a>Kesintilere ve Afetlere Karşı Koruma - Servis Otobüsü Premium
Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma kavramları, hem aynı bölgede (Kullanılabilirlik Bölgeleri üzerinden) hem de farklı bölgelerde (Coğrafi Felaket Kurtarma yoluyla) Azure Hizmet Veri Yolu Premium katmanında yerleşiktir.

### <a name="geo-disaster-recovery"></a>Jeo-Afet Kurtarma

Service Bus Premium, ad alanı düzeyinde Coğrafi felaket kurtarmayı destekler. Daha fazla bilgi için Bkz. [Azure Hizmet Veri Yolu Coğrafi felaket kurtarma.](service-bus-geo-dr.md) Yalnızca [Premium SKU](service-bus-premium-messaging.md) için kullanılabilen olağanüstü durum kurtarma özelliği, meta veri olağanüstü durum kurtarma yı uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarına dayanır.

### <a name="availability-zones"></a>Kullanılabilirlik Alanları

Service Bus Premium SKU, aynı Azure bölgesinde hataya göre yalıtılmış konumlar sağlayarak [Kullanılabilirlik Bölgelerini](../availability-zones/az-overview.md)destekler.

> [!NOTE]
> Azure Hizmet Veri Servisi Premium için Kullanılabilirlik Bölgeleri desteği yalnızca kullanılabilirlik bölgelerinin bulunduğu [Azure bölgelerinde](../availability-zones/az-overview.md#services-support-by-region) kullanılabilir.

Kullanılabilirlik Bölgelerini yalnızca yeni ad alanlarında Azure portalını kullanarak etkinleştirebilirsiniz. Hizmet Veri Servisi varolan ad boşluklarının geçişini desteklemez. Ad alanınızda etkinleştirdikten sonra bölge artıklığını devre dışı bırakamazsınız.

![1][]


## <a name="protecting-against-outages-and-disasters---service-bus-standard"></a>Kesintilere ve Afetlere Karşı Koruma - Servis Veri Günü Standardı
Standart mesajlaşma fiyatlandırma katmanını kullanırken veri merkezi kesintilerine karşı esneklik sağlamak için Service Bus iki yaklaşımı destekler: *etkin* ve *pasif* çoğaltma. Her yaklaşım için, belirli bir kuyruk veya konu nun veri merkezi kesintisi varlığında erişilebilir kalması gerekiyorsa, bunu her iki ad alanında da oluşturabilirsiniz. Her iki varlık da aynı ada sahip olabilir. Örneğin, birincil sıraya **contosoPrimary.servicebus.windows.net/myQueue**altında ulaşılabilirken, ikincil muadili **contosoSecondary.servicebus.windows.net/myQueue**altında ulaşılabilir.

>[!NOTE]
> **Etkin Çoğaltma** ve **Pasif Çoğaltma** kurulumu, Hizmet Veri Tos'un belirli özellikleri değil, genel amaçlı çözümlerdir. Çoğaltma mantığı (2 farklı ad alanlarına gönderme) gönderen uygulamalarda yaşar ve alıcının yinelenen algılama için özel mantığı olmalıdır.

Uygulama kalıcı gönderenden alıcıya iletişim gerektiriyorsa, uygulama ileti kaybını önlemek ve göndereni geçici Servis Veri Servisi hatalarından korumak için dayanıklı bir istemci tarafı sırası uygulayabilir.

### <a name="active-replication"></a>Etkin çoğaltma
Etkin çoğaltma, her işlem için her iki ad alanında da varlıklar kullanır. İleti gönderen herhangi bir istemci aynı iletinin iki kopyasını gönderir. İlk kopya birincil varlığa (örneğin, **contosoPrimary.servicebus.windows.net/sales**) gönderilir ve iletinin ikinci kopyası ikincil varlığa gönderilir (örneğin, **contosoSecondary.servicebus.windows.net/sales).**

İstemci her iki kuyruktan da ileti alır. Alıcı iletinin ilk kopyasını işler ve ikinci kopya bastırılır. Yinelenen iletileri bastırmak için, gönderenin her iletiyi benzersiz bir tanımlayıcıyla etiketlemesi gerekir. İletinin her iki kopyası da aynı tanımlayıcıyla etiketlenmelidir. [Aracılı Mesaj.MessageId][BrokeredMessage.MessageId] veya [BrokeredMessage.Label][BrokeredMessage.Label] özelliklerini veya iletiyi etiketlemek için özel bir özelliği kullanabilirsiniz. Alıcı, aldığı iletilerin listesini tutmalıdır.

[Service Bus Standart Tier][Geo-replication with Service Bus Standard Tier] örneği ile Coğrafi çoğaltma, ileti varlıklarının etkin çoğaltılması gösterir.

> [!NOTE]
> Etkin çoğaltma yaklaşımı işlem sayısını iki katına çıkar, bu nedenle bu yaklaşım daha yüksek maliyete yol açabilir.
> 
> 

### <a name="passive-replication"></a>Pasif çoğaltma
Hatasız durumda, pasif çoğaltma yalnızca iki ileti varlıklarından birini kullanır. İstemci iletiyi etkin varlığa gönderir. Etkin varlık takiişlemi, etkin varlığı barındıran veri merkezinin kullanılamayabileceğini gösteren bir hata koduyla başarısız olursa, istemci iletinin bir kopyasını yedek varlığa gönderir. Bu noktada etkin ve yedek varlıklar rolleri değiştirir: gönderen istemci eski etkin varlığı yeni yedek varlık olarak kabul eder ve eski yedekleme varlığı yeni etkin varlıktır. Her iki gönderme işlemi de başarısız olursa, iki varlığın rolleri değişmeden kalır ve bir hata döndürülür.

İstemci her iki kuyruktan da ileti alır. Alıcının aynı iletinin iki kopyasını alma olasılığı olduğundan, alıcının yinelenen iletileri bastırması gerekir. Etkin çoğaltma için açıklandığı şekilde yinelenenleri bastırabilirsiniz.

Çoğu durumda yalnızca bir işlem gerçekleştirilir, çünkü genel olarak, pasif çoğaltma etkin çoğaltma daha ekonomiktir. Gecikme sonu, iş ortası ve parasal maliyet, çoğaltılmayan senaryoyla aynıdır.

Pasif çoğaltma kullanırken, aşağıdaki senaryolarda iletiler kaybedilebilir veya iki kez alınabilir:

* **İleti gecikmesi veya kaybı**: Gönderenin birincil kuyruğa başarılı bir şekilde m1 ileti gönderdiğini ve alıcı m1 almadan önce sıranın kullanılamadığını varsayalım. Gönderen sonraki ileti m2'yi ikincil sıraya gönderir. Birincil sıra geçici olarak kullanılamıyorsa, sıra yeniden kullanılabilir hale geldikten sonra alıcı m1 alır. Bir felaket durumunda alıcı hiçbir zaman m1 almayabilir.
* **Yinelenen alım**: Gönderenin birincil kuyruğa m iletisi gönderdiğini varsayalım. Servis Veri Servisi m'yi başarıyla işler ancak yanıt gönderemezse. Gönderme işlemi süreleri bittikten sonra, gönderen ikincil kuyruğa m'nin aynı kopyasını gönderir. Birincil sıra kullanılamaz hale gelmeden önce alıcı m'nin ilk kopyasını alabilirse, alıcı yaklaşık olarak aynı anda her iki m kopyasını da alır. Birincil sıra kullanılamaz hale gelmeden önce alıcı m'nin ilk kopyasını alamıyorsa, alıcı başlangıçta yalnızca m'nin ikinci kopyasını alır, ancak birincil sıra kullanılabilir olduğunda ikinci bir m kopyasını alır.

[Service Bus Standart Tier][Geo-replication with Service Bus Standard Tier] örneği ile Coğrafi çoğaltma, ileti varlıklarının pasif çoğaltılması gösterir.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Röle uç noktalarını veri merkezi kesintilerine veya felaketlere karşı koruma
[Azure Röle](../service-bus-relay/relay-what-is-it.md) uç noktalarının coğrafi çoğaltması, bir röle bitiş noktasını hizmet veri ağacı kesintileri varlığında ulaşılabilir hale getiren bir hizmete olanak tanır. Coğrafi çoğaltma elde etmek için, hizmetin farklı ad alanlarında iki geçiş uç noktası oluşturması gerekir. Ad alanları farklı veri merkezlerinde yer almalıdır ve iki uç nokta farklı adlara sahip olmalıdır. Örneğin, birincil bitiş noktasına **contosoPrimary.servicebus.windows.net/myPrimaryService**altında ulaşılabilirken, ikincil muadili **contosoSecondary.servicebus.windows.net/mySecondaryService**altında ulaşılabilir.

Hizmet daha sonra her iki uç noktada da dinler ve istemci her iki uç nokta üzerinden de hizmeti çağırabilir. İstemci uygulaması, rölelerden birini birincil bitiş noktası olarak rasgele seçer ve isteğini etkin bitiş noktasına gönderir. İşlem bir hata koduyla başarısız olursa, bu hata röle bitiş noktasının kullanılmadığını gösterir. Uygulama, yedekleme bitiş noktasına bir kanal açar ve isteği yeniden yayımlar. Bu noktada etkin ve yedek uç noktaları geçiş rolleri: istemci uygulaması eski etkin bitiş noktasını yeni yedekleme bitiş noktası, eski yedekleme bitiş noktasını da yeni etkin bitiş noktası olarak kabul eder. Her iki gönderme işlemi de başarısız olursa, iki varlığın rolleri değişmeden kalır ve bir hata döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu makalelere bakın:

* [Azure Servis Veri Yolu Coğrafi felaket kurtarma](service-bus-geo-dr.md)
* [Azure SQL Veritabanı İş Sürekliliği][Azure SQL Database Business Continuity]
* [Azure için dayanıklı uygulamalar tasarlama][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Geo-replication with Service Bus Standard Tier]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png
