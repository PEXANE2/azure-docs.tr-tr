---
title: Azure Bildirim Hub'ları Sık Sorulan Sorular (SSS) | Microsoft Dokümanlar
description: Azure Bildirim Hub'larında çözümler tasarlama ve uygulama hakkında SSS'ler.
services: notification-hubs
documentationcenter: mobile
author: sethmanheim
manager: femila
editor: jwargo
keywords: push bildirimi, push bildirimleri, iOS push bildirimleri, android push bildirimleri, ios push, android push
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 11/13/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 11/13/2019
ms.openlocfilehash: 3212520f37d33a2d8fb1b071506f688b9f75f15c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263838"
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Azure Bildirim Hub'ları ile anında iletme bildirimleri: Sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Bildirim Hub'larının kaynak yapısı nedir?

Azure Bildirim Hub'larının iki kaynak düzeyi vardır: hub'lar ve ad alanları. Hub, bir uygulamanın platform lar arası itme bilgilerini tutabilen tek bir itme kaynağıdır. Ad alanı, bir bölgedeki hub'lar topluluğudur. Önerilen eşleme, bir ad alanıyla tek bir uygulamayla eşleşir. Ad alanı içinde, üretim uygulamanızla çalışan bir üretim hub'ınız, test uygulamanızla çalışan bir test merkezine ve benzeri bir

### <a name="what-is-the-price-model-for-notification-hubs"></a>Bildirim Hub'ları için fiyat modeli nedir?

En son fiyatlandırma ayrıntılarını [Bildirim Hub'ları Fiyatlandırma] sayfasında bulabilirsiniz. Bildirim Hub'ları ad alanı düzeyinde faturalandırılır. (Ad alanı tanımı için bkz: "Bildirim Hub'larının kaynak yapısı nedir?") Bildirim Hub'ları üç katman sunar:

* **Ücretsiz**: Bu katman itme yeteneklerini keşfetmek için iyi bir başlangıç noktasıdır. Üretim uygulamaları için önerilmez. Hizmet düzeyi sözleşmesi (SLA) garantisi olmadan, ayda ad alanı başına 500 cihaz ve 1 milyon itme alanı alırsınız.
* **Temel**: Bu katman (veya Standart katman) daha küçük üretim uygulamaları için önerilir. Temel olarak her ay ad alanı başına 200.000 aygıt ve 10 milyon itme elde elabilirsiniz.
* **Standart**: Bu katman orta ve büyük üretim uygulamaları için önerilir. Temel olarak her ay ad alanı başına 10 milyon aygıt ve 10 milyon itme elde elabilirsiniz. Zengin telemetri (sağlanan itme durumu hakkında ek veriler) içerir.

Standart katman özellikleri:

* **Zengin telemetri**: İleti Başına Bildirim Hub'larını kullanarak hata ayıklama için herhangi bir itme isteklerini ve Platform Bildirim Sistemi Geri bildirimini izleyebilirsiniz.
* **Çoklu kira :** Platform Bildirim Sistemi kimlik bilgileriyle ad alanı düzeyinde çalışabilirsiniz. Bu seçenek, kiracıları aynı ad alanı içindeki hub'lara kolayca bölmenize olanak tanır.
* **Zamanlanmış itme**: Bildirimleri istediğiniz zaman gönderilecek şekilde zamanlayabilirsiniz.
* **Toplu işlemler**: [Dışa Aktarma/Alma] belgelerinde açıklandığı gibi dışa aktarma/alma işlemlerini sağlar.

### <a name="what-is-the-notification-hubs-sla"></a>Bildirim Hub'ları SLA nedir?

Temel ve Standart Bildirim Hub'ları katmanları için, doğru yapılandırılmış uygulamalar anında iletme bildirimleri gönderebilir veya kayıt yönetimi işlemlerini en az yüzde 99,9 oranında gerçekleştirebilir. SLA hakkında daha fazla bilgi edinmek için [Bildirim Hub'ları SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) sayfasına gidin.

> [!NOTE]
> Anında iletme bildirimleri Apple'ın Anında İtme Bildirim Hizmeti (APN' ler) ve Google'ın Firebase Bulut Mesajlaşma (FCM) gibi üçüncü taraf Platform Bildirim Sistemlerine bağlı olduğundan, bu iletilerin teslimi için SLA garantisi yoktur. Bildirim Hub'ları toplu ları Platform Bildirim Sistemleri'ne (SLA garantili) gönderdikten sonra, itmeleri (SLA garantisi yoktur) teslim etmek Platform Bildirim Sistemlerinin sorumluluğundadır.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Hub'ımı veya ad alanımı farklı bir katmana nasıl yükseltebilirim veya düşürebilirim?

**[Azure portalı]** > **Bildirim Hub'ları Ad Boşlukları'na** veya Bildirim **Hub'larına**gidin. Güncelleştirmek istediğiniz kaynağı seçin ve **Fiyatlandırma Katmanı'na**gidin. Aşağıdaki gereksinimleri not edin:

* Güncelleştirilmiş fiyatlandırma katmanı, çalıştığınız ad alanındaki *tüm* hub'lar için geçerlidir.
* Cihaz sayınız düşürdüğünüz seviyenin sınırını aşarsa, düşürmeden önce aygıtları silmeniz gerekir.

## <a name="design-and-development"></a>Tasarım ve geliştirme

### <a name="which-server-side-platforms-do-you-support"></a>Hangi sunucu tarafındaki platformları destekliyorsunuz?

Sunucu SDK'ları .NET, Java, Node.js, PHP ve Python için kullanılabilir. Bildirim Hub'ları API'leri REST arabirimlerine dayanır, bu nedenle farklı platformlar kullanıyorsanız veya ekstra bağımlılık istemiyorsanız doğrudan REST API'leri ile çalışabilirsiniz. Daha fazla bilgi için [Bildirim Hub'ları REST API'lerine] gidin.

### <a name="which-client-platforms-do-you-support"></a>Hangi istemci platformlarını destekliyorsunuz?

Push [bildirimleri iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md)için desteklenir , [Android](notification-hubs-android-push-notification-google-fcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), Android [Çin (Baidu üzerinden)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) ve Android, ve [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Daha fazla bilgi [için, Eğitimlere Başlarken Bildirim Hub'ları] sayfasına gidin.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Kısa mesajı, e-postayı veya web bildirimlerini destekliyor musunuz?

Bildirim Hub'ları mobil uygulamaları çalıştıran cihazlara bildirim gönderir. E-posta veya kısa mesaj özellikleri sağlamaz. Bildirim Hub'ları da kutunun dışında bir tarayıcı içi anında iletme bildirimi teslim hizmeti sağlamaz. Müşteriler bu özelliği desteklenen sunucu tarafındaki platformların üzerine SignalR kullanarak uygulayabilir. 

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Bildirim Hub'ları aracılığıyla anında iletme bildirimleri gönderirsem kaç cihazı destekleyebilirim?

Desteklenen aygıt sayısıyla ilgili ayrıntılar için [Bildirim Hub'ları Fiyatlandırma] sayfasına bakın.

10 milyondan fazla kayıtlı aygıt için desteğe ihtiyacınız varsa, aygıtlarınızı birden çok hub'a bölmeniz gerekir.

### <a name="how-many-push-notifications-can-i-send-out"></a>Kaç tane anında iletme bildirimi gönderebilirim?

Seçilen katmana bağlı olarak, Azure Bildirim Hub'ları sistemden akan bildirim sayısına bağlı olarak otomatik olarak ölçeklendirilir.

> [!NOTE]
> Genel kullanım maliyeti, gönderilen anında iletme bildirimleri sayısına bağlı olarak artabilir. [Bildirim Hub'ları Fiyatlandırma] sayfasında belirtilen katman sınırlarını bildiğinizden emin olun.

Müşterilerimiz, her gün milyonlarca anında iletme bildirimi göndermek için Bildirim Hub'larını kullanır. Azure Bildirim Hub'larını kullandığınız sürece anında iletme bildirimlerinizin erişimini ölçeklendirmek için özel bir şey yapmanız gerekmez.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Gönderilen anında iletme bildirimlerinin aygıtıma ulaşması ne kadar sürer?

Gelen yükün tutarlı ve eşit olduğu normal kullanım senaryosunda, Azure Bildirim Hub'ları dakikada en az *1 milyon anında iletme bildirimini*işleyebilir. Bu oran etiket sayısına, gelen göndermelerin yapısına ve diğer dış etkenlere bağlı olarak değişebilir.

Tahmini teslim süresi boyunca, hizmet platform başına hedefleri hesaplar ve iletileri kayıtlı etiketlere veya etiket ifadelerine göre Anında İtme Bildirim Hizmeti'ne (PNS) yönlendirir. Cihaza bildirim göndermek PNS'nin sorumluluğundadır.

PNS, bildirimlerin teslimi için herhangi bir SLA garantisi vermez. Ancak, anında iletme bildirimleri, bildirim hub'larına gönderildikleri andan itibaren birkaç dakika içinde (genellikle 10 dakika içinde) hedef aygıtlara teslim edilir. Birkaç bildirim daha fazla zaman alabilir.

> [!NOTE]
> Azure Bildirim Hub'larının, PNS'ye teslim edilmeyen anında iletme bildirimlerini 30 dakika içinde bırakma politikası vardır. Bu gecikme birkaç nedenden dolayı olabilir, ancak en yaygın olarak PNS uygulamanızı daralttığı için.

### <a name="is-there-any-latency-guarantee"></a>Gecikme garantisi var mı?

Anında iletme bildirimlerinin doğası gereği (harici, platforma özgü bir PNS tarafından teslim edilirler), gecikme garantisi yoktur. Genellikle, anında iletme bildirimlerinin çoğunluğu birkaç dakika içinde teslim edilir.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Ad boşlukları ve bildirim hub'ları içeren bir çözüm tasarlarken nelere dikkat etmem gerekir?

#### <a name="mobile-appenvironment"></a>Mobil uygulama/ortam

* Mobil uygulama başına ortam başına bir bildirim hub'ı kullanın.
* Çok kiracılı bir senaryoda, her kiracının ayrı bir hub'ı olmalıdır.
* Üretim ve test ortamları için asla aynı bildirim merkezini paylaşmayın. Bu uygulama, bildirim gönderirken sorunlara neden olabilir. (Apple, her biri ayrı kimlik bilgilerine sahip Olan Sandbox ve Üretim Push uç noktaları sunar.)
* Varsayılan olarak, Kayıtlı aygıtlarınıza Azure portalı veya Visual Studio'daki Azure tümleşik bileşeni aracılığıyla test bildirimleri gönderebilirsiniz. Eşik, kayıt havuzundan rastgele seçilen 10 aygıta ayarlanır.

> [!NOTE]
> Hub'ınız ilk olarak bir Apple sandbox sertifikasıyla yapılandırıldıysa ve ardından Apple üretim sertifikası kullanacak şekilde yeniden yapılandırıldıysa, özgün aygıt belirteçleri geçersizdir. Geçersiz belirteçler başarısız olmaya iter neden olur. Üretim ve test ortamlarınızı ayırın ve farklı ortamlar için farklı hub'lar kullanın.

#### <a name="pns-credentials"></a>PNS kimlik bilgileri

Bir mobil uygulama bir platformun geliştirici portalına (örneğin, Apple veya Google) kaydedildiğinde, bir uygulama tanımlayıcısı ve güvenlik belirteçleri gönderilir. Uygulama arka ucu, anında iletme bildirimlerinin aygıtlara gönderilebilmeleri için platformun PNS'sine bu belirteçleri sağlar. Güvenlik belirteçleri sertifika (örneğin, Apple iOS veya Windows Phone) veya güvenlik anahtarları (örneğin, Google Android veya Windows) şeklinde olabilir. Bildirim hub'larında yapılandırılması gerekir. Yapılandırma genellikle bildirim merkezi düzeyinde yapılır, ancak çok kiracılı bir senaryoda ad alanı düzeyinde de yapılabilir.

#### <a name="namespaces"></a>Ad Alanları

Ad alanları dağıtım gruplandırması için kullanılabilir. Ayrıca, aynı uygulamanın tüm kiracıları için çok kiracılı bir senaryoda tüm bildirim hub'larını temsil etmek için de kullanılabilirler.

#### <a name="geo-distribution"></a>Coğrafi dağılım

Anında iletme bildirimi senaryolarında coğrafi dağılım her zaman kritik değildir. Aygıtlara anında iletme bildirimleri sağlayan çeşitli Bilgisayarlar (örneğin, APN'ler veya FCM) eşit olarak dağıtılmaz.

Genel olarak kullanılan bir uygulamanız varsa, dünyanın farklı Azure bölgelerindeki Bildirim Hub'ları hizmetini kullanarak farklı ad alanlarında hub oluşturabilirsiniz.

> [!NOTE]
> Bu düzenlemeyi, özellikle kayıtlar için yönetim maliyetinizi artırdığı için önermiyoruz. Yalnızca açık bir ihtiyaç varsa yapılmalıdır.

### <a name="should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>Uygulama arka ucundan veya doğrudan istemci aygıtları üzerinden kayıt yapmalı mıyım?

Uygulama arka ucundan gelen kayıtlar, kaydı oluşturmadan önce istemcilerin kimliğini doğrulamanız gerektiğinde yararlıdır. Uygulama mantığına bağlı olarak uygulama arka ucu tarafından oluşturulması veya değiştirilmesi gereken etiketleri olduğunda da yararlıdır. Daha fazla bilgi için [Backend Kayıt kılavuzuna] ve [Arka Son Kayıt kılavuzuna 2] sayfa gidin.

### <a name="what-is-the-push-notification-delivery-security-model"></a>Anında iletme bildirimi teslim güvenlik modeli nedir?

Azure Bildirim Hub'ları [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md)tabanlı güvenlik modeli kullanır. Paylaşılan erişim imza belirteçlerini kök ad alanı düzeyinde veya parçalı bildirim hub düzeyinde kullanabilirsiniz. Paylaşılan erişim imza belirteçleri, ileti izinleri göndermek veya bildirim izinlerini dinlemek gibi farklı yetkilendirme kurallarına uyacak şekilde ayarlanabilir. Daha fazla bilgi için [Bildirim Hub'ları güvenlik modeli] belgesine bakın.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Anında iletme bildirimlerinde hassas yükü nasıl idare etmeliyim?

Tüm bildirimler platformun PNS tarafından hedef cihazlara teslim edilir. Bir bildirim Azure Bildirim Hub'larına gönderildiğinde işlenir ve ilgili PNS'ye iletilir.

Gönderenden Azure Bildirim Hub'larına ve PNS'ye kadar tüm bağlantılar HTTPS'yi kullanın.

> [!NOTE]
> Azure Bildirim Hub'ları iletilerin yükünü kaydetmez.

Hassas yükler göndermek için Güvenli İtme deseni kullanmanızı öneririz. Gönderen, hassas yükü olmadan aygıta ileti tanımlayıcısı içeren bir ping bildirimi gönderir. Cihazdaki uygulama yükü aldığında, uygulama ileti ayrıntılarını almak için doğrudan güvenli bir API çağırır. Bu desenin nasıl uygulanacağı yla ilgili bir kılavuz için [Bildirim Hub'ları Güvenli Anında Niçin öğretici sayfasına] gidin.

## <a name="operations"></a>İşlemler

### <a name="what-support-is-provided-for-disaster-recovery"></a>Olağanüstü durum kurtarma için ne gibi destek sağlanır?

Sonunda meta veri olağanüstü durum kurtarma kapsamı (Bildirim Hub'ları adı, bağlantı dizesi ve diğer kritik bilgiler) sağlarız. Olağanüstü durum kurtarma senaryosu tetiklendiğinde, kayıt verileri Bildirim Hub'ları altyapısının kaybolan *tek kesimidir.* Bu verileri kurtarma sonrası yeni hub'ınıza yeniden doldurmak için bir çözüm uygulamanız gerekir:

1. Farklı bir veri merkezinde ikincil bildirimler merkezi oluşturun. Sizi yönetim yeteneklerinizi etkileyebilecek bir olağanüstü durum kurtarma olayından korumak için en başından itibaren bir tane oluşturmanızı öneririz. Ayrıca, olağanüstü durum kurtarma olayı sırasında da bir tane oluşturabilirsiniz.

2. İkincil bildirim merkezini birincil bildirim merkezinizdeki kayıtlarla doldurun. Her iki merkezde de kayıtları korumaya ve kayıtlar gelirken senkronize olmaya çalışmanızı önermiyoruz. Bu uygulama, kayıtların PNS tarafında sona erme eğilimi nedeniyle iyi çalışmaz. Bildirim Hub'ları, süresi dolmuş veya geçersiz kayıtlar hakkında PNS geri bildirimi aldığında bunları temizler.  

Uygulama geri dönüşleri için iki önerimiz var:

* Sonunda belirli bir kayıt kümesini tutan bir uygulama arka ucu kullanın. Daha sonra ikincil bildirim hub'ına toplu ekleme gerçekleştirebilir.
* Yedekleme olarak birincil bildirim hub'ından düzenli bir kayıt dökümü alan bir uygulama arka uç kullanın. Daha sonra ikincil bildirim hub'ına toplu ekleme gerçekleştirebilir.

> [!NOTE]
> Standart katmanda bulunan Dışa Aktarma/Alma işlemleri, [Dışa Aktarma/Alma] Belgelerinde açıklanmıştır.

Arka ucun yoksa, uygulama hedef aygıtlarda başlatıldığında, ikincil bildirim hub'ında yeni bir kayıt gerçekleştirirler. Sonunda ikincil bildirim hub'ı tüm etkin aygıtları kaydettirecek.

Açılmamış uygulamalara sahip cihazların bildirim alamayacağı bir zaman dilimi olacaktır.

### <a name="is-all-of-my-data-stored-in-encrypted-form"></a>Tüm verilerim şifreli olarak mı depolanır?

Azure Bildirim Hub'ları, kayıt etiketleri dışında tüm müşteri verilerini yerinde olarak şifreler. Bu nedenle, etiketleri kullanarak kişisel veya gizli verileri saklamamalısınız.

### <a name="is-there-audit-log-capability"></a>Denetim günlüğü özelliği var mı?

Evet. Tüm Bildirim Hub'ları yönetim işlemleri, [Azure portalında]açıkta olan Azure Etkinlik Günlüğü'nün güncellenir. Azure Etkinlik Günlüğü, aboneliklerinizdeki kaynaklar üzerinde gerçekleştirilen işlemlerle ilgili öngörüler sunar. Etkinlik Günlüğü'nü kullanarak, aboneliğinizdeki kaynaklar için yapılan herhangi bir yazma işleminin (PUT, POST, DELETE) ne zaman, kim olduğunu ve ne zaman yapıldığını belirleyebilirsiniz. Ayrıca işlemlerin ve diğer ilgili özelliklerin durumunu da anlayabilirsiniz. Ancak. Etkinlik Günlüğü okuma (GET) işlemini içermez.

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

### <a name="what-troubleshooting-capabilities-are-available"></a>Hangi sorun giderme özellikleri kullanılabilir?

Azure Bildirim Hub'ları, özellikle en yaygın bırakılan bildirimler senaryosu için sorun giderme için çeşitli özellikler sağlar. Ayrıntılar için [Bildirim Hub'ları sorun giderme] teknik incelemesine bakın.

### <a name="what-telemetry-features-are-available"></a>Hangi telemetri özellikleri mevcuttur?

Azure Bildirim Hub'ları, Azure [portalında]telemetri verilerini görüntülemeyi sağlar. Ölçümlerin [ayrıntıları, Bildirim Hub'ları Ölçümleri] sayfasında mevcuttur.

Ölçümlere programlı olarak da erişebilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [.NET ile Azure Monitör ölçümleri alın.](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) Bu örnek, kullanıcı adı ve parolakullanır. Bir sertifika kullanmak için, [bu örnekte](https://github.com/Azure/azure-libraries-for-net/blob/master/src/ResourceManagement/ResourceManager/Authentication/AzureCredentialsFactory.cs)gösterildiği gibi bir sertifika sağlamak için FromServicePrincipal yöntemini aşırı yükleyin. 
- [Kaynak için ölçümler ve etkinlik günlükleri alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-query-metrics-activitylogs/)
- [Azure İzleme REST API izlenecek](../azure-monitor/platform/rest-api-walkthrough.md)

> [!NOTE]
> Başarılı bildirimler, anında iletme bildirimlerinin harici PNS'ye (örneğin, iOS ve macOS veya Android cihazlar için FCM APN'leri) teslim edildiği anlamına gelir. Bildirimleri hedef aygıtlara ulaştırmak PNS'nin sorumluluğundadır. Genellikle, PNS üçüncü taraflara teslim ölçümlerini göstermez.  

[Azure portalında]: https://portal.azure.com
[Bildirim Hub'ları Fiyatlandırma]: https://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: https://azure.microsoft.com/support/legal/sla/
[Bildirim Hub'ları REST API'leri]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Bildirim Hub'ları Öğreticilere Başlıyor]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Mobile Services Pricing]: https://azure.microsoft.com/pricing/details/mobile-services/
[Arka uç Kayıt kılavuzu]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Arka uç Kayıt kılavuzu 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Bildirim Hub'ları güvenlik modeli]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Bildirim Hub'ları Güvenli Push öğretici]: https://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Bildirim Hub'ları sorun giderme]: https://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Bildirim Hub'ları Ölçümleri]: ../azure-monitor/platform/metrics-supported.md#microsoftnotificationhubsnamespacesnotificationhubs
[Kayıtlar İhracat/İthalat]: https://docs.microsoft.com/azure/notification-hubs/export-modify-registrations-bulk
[Azure portalında]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[App Service Pricing]: https://azure.microsoft.com/pricing/details/app-service/
