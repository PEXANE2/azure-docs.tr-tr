---
title: Azure Notification Hubs nedir?
description: Azure Notification Hubs ile anında iletme bildirimi özellikleri eklemeyi öğrenin.
author: sethmanheim
manager: femila
editor: tjsomasundaram
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 03/23/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 04/30/2019
ms.openlocfilehash: f5b59d1669d89c73b93199d1f833da149003d399
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80159345"
---
# <a name="what-is-azure-notification-hubs"></a>Azure Notification Hubs nedir?

Azure Bildirim Hub'ları, herhangi bir arka uçtan (bulut veya şirket içinde) herhangi bir platforma (iOS, Android, Windows, vb.) bildirim göndermenize olanak tanıyan kullanımı kolay ve ölçeklendirilmis bir anında iletme motoru sağlar. Notification Hubs hem kuruluş hem de tüketici senaryoları için sorunsuzca çalışır. Birkaç senaryo örneği aşağıda verilmiştir:

- Düşük gecikme ile milyonlarca kişiye son dakika haber bildirimleri gönderme.
- İlgili kullanıcı segmentlerine konum temelli kuponlar gönderme.
- Medya/spor/finans/oyun uygulamaları için kullanıcılara veya gruplara etkinliklerle ilgili bildirimler gönderme.
- Müşterilerle etkileşimde bulunmak ve pazarlama yapmak için uygulamalara promosyon içeriği gönderme.
- Kullanıcılara yeni iletiler ve iş öğeleri gibi kurumsal olayları bildirin.
- Çok faktörlü kimlik doğrulaması için kod gönderme.

## <a name="what-are-push-notifications"></a>Anında iletme bildirimleri nedir?

Anında iletme bildirimleri, mobil uygulamaların kullanıcılarının genellikle mobil cihazdaki açılır pencere veya iletişim kutusunda olmak üzere istenen belirli bilgilerden haberdar edildiği bir uygulamadan kullanıcıya iletişim biçimidir. Kullanıcılar genellikle iletiyi görüntülemeyi veya reddetmeyi seçer; eski seçerek bildirimi tebliğ mobil uygulama açılır. Bazı bildirimler sessizdir - uygulamanın işlenmesi ve ne yapacağına karar vermesi için arka planda teslim edilir.

Anında iletme bildirimleri, tüketici uygulamalarında uygulama etkileşiminin ve kullanımının artırılması, kurumsal uygulamalarda ise güncel iş bilgilerinin iletilmesi açısından çok önemlidir. Mobil cihazlar için enerji tasarruflu, bildirim gönderenler için esnek ve ilgili uygulamalar etkin olmadığında kullanılabilir olduğundan, en iyi uygulamadan kullanıcıya iletişimdir.

> [!NOTE]
> Azure Bildirim Hub'ları Internet Üzerinden Ses Protokolü (VOIP) anında iletme bildirimlerini resmi olarak desteklemez; ancak [bu makalede, APNS VOIP bildirimlerini](voip-apns.md) Azure Bildirim Hub'ları aracılığıyla nasıl kullanabileceğiniz açıklanmaktadır.

Birkaç popüler platformda anında iletme bildirimleri hakkında daha fazla bilgi için aşağıdaki konulara bakın:

- [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
- [iOS](https://developer.apple.com/notifications/)
- [Windows](https://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-do-push-notifications-work"></a>Anında iletme bildirimleri nasıl çalışır?

Anında iletme bildirimleri, *Platform Bildirim Sistemleri* (PNS) adlı platforma özgü altyapılar aracılığıyla teslim edilir. Sağlanan tutamacı olan bir aygıta ileti iletmek için temel itme işlevleri sunarlar ve ortak bir arabirimi yoktur. Bir uygulamanın Android, iOS ve Windows sürümlerindeki tüm müşterilere bildirim göndermek için geliştiricinin Apple Push Bildirim Hizmeti (APNS), Firebase Cloud Messaging (FCM) ve Windows Bildirim Hizmeti (WNS) ile ayrı ayrı çalışması gerekir.

Yüksek bir düzeyde gönderme işlemi şu şekilde çalışır:

1. Bir uygulama bir bildirim almak ister, böylece uygulamanın çalıştırıldığı hedef platform için PNS ile bağlantı yada tır ve benzersiz ve geçici bir itme tutamacı ister. Tanıtıcı türü sisteme bağlıdır (örneğin, APNS belirteçleri kullanırken WNS URI kullanır).
2. İstemci uygulaması bu tutamacı uygulama arka ucunda veya sağlayıcıda depolar.
3. Bir anında iletme bildirimi göndermek için, uygulama belirli bir istemci uygulamasını hedeflemek için tutamacı nı kullanarak PNS ile bağlantı kurur.
4. PNS, tanıtıcı tarafından belirtilen cihaza bildirimi iletir.

![Anında iletme bildirimi iş akışı](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Anında iletme bildirimlerinin zorlukları

PNS’ler güçlüdür. Bununla birlikte, segmentlere ayrılmış kullanıcılara anında iletme bildirimleri yayımlamak gibi genel anında iletme bildirimi senaryolarını uygulamak için bile uygulama geliştiricisine çok iş bırakır.

Anında iletme bildirimleri göndermek, uygulamanın ana iş mantığıyla ilgisi olmayan karmaşık altyapı gerektirir. Altyapı sorunlarından bazıları şunlardır:

- **Platform bağımlılığı**
  - Arka uç, pn'ler birleştirilmiş olmadığı için çeşitli platformlardaki aygıtlara bildirim göndermek için karmaşık ve bakımı zor platforma bağımlı mantık gerektirir.
- **Ölçeklendirme**
  - PNS yönergeleriuyarınca, cihaz belirteçleri her uygulama lansmanında yenilenmelidir. Arka uç, belirteçleri güncel tutmak için büyük miktarda trafik ve veritabanı erişimiyle ilgilenir. Cihaz sayısı yüzlerce, binlerce veya milyonlarcaolduğunda, bu altyapıyı oluşturma nın ve sürdürmenin maliyeti çok büyüktür.
  - Çoğu PNS, birden fazla cihaza yayın yapmayı desteklemez. Bir milyon cihaza basit bir yayın yapılması, PNS’lere yönelik bir milyon çağrı ile sonuçlanır. Bu miktarda trafiğin en düşük gecikme ile ölçeklendirilmesi sıradan bir işlem değildir.
- **Yönlendirme**
  - PNS aygıtlara ileti göndermenin bir yolunu sağlasa da, uygulama bildirimlerinin çoğu kullanıcıları veya ilgi alanı gruplarını hedeflemektedir. Arka uç, aygıtları ilgi alanı grupları, kullanıcılar, özellikler vb. ile ilişkilendirmek için bir kayıt defteri tutmalıdır. Bu ek gider, bir uygulamanın pazara ve bakım maliyetlerine zaman kazandırır.

## <a name="why-use-azure-notification-hubs"></a>Azure Notification Hubs neden kullanılır?

Bildirim Hub'ları, uygulama arka uçunuzdan kendi anında iletme bildirimleri göndermeyle ilgili tüm karmaşıklıkları ortadan kaldırır. Çok platformlu, ölçeği genişletilmiş anında iletme bildirimi altyapısı, gönderme ile ilgili kodlama işlemlerini azaltır ve arka ucunuzu basitleştirir. Notification Hubs sayesinde cihazlar yalnızca PNS tanıtıcılarını bir hub’a kaydetmekten sorumludur. Arka uç ise aşağıdaki şekilde gösterildiği gibi kullanıcılara veya ilgili alanı gruplarına iletiler gönderir:

![Bildirim Hub'ı diyagramı](./media/notification-hubs-overview/notification-hub-diagram.png)

Bildirim Hub'ları, aşağıdaki avantajlara sahip kullanıma hazır itme motorunuzdur:

- **Platformlar arası**
  - Tüm büyük itme platformları için destek.
  - Platforma özel bir iş olmaksızın platforma özgü ya da platforma bağımlı biçimlerle tüm platformlara gönderebilen bir ortak arabirimdir.
  - Cihaz tanıtıcısı yönetimi tek yerden yürütülür.
- **Arka uçlar arası**
  - Bulut veya şirket içi.
  - .NET, Node.js, Java, Python, vb.
- **Zengin teslim düzeni kümesi**
  - Bir veya daha fazla platforma yayın: Tek bir API çağrısıyla platformlar arasında milyonlarca cihaza anında yayın yapabilirsiniz.
  - Cihaza gönderme: Bildirimlerle tek cihazları hedefleyebilirsiniz.
  - Kullanıcıya itin: Etiketler ve şablonlar, bir kullanıcı için tüm platformlar arası aygıtlara ulaşmanıza yardımcı olur.
  - Dinamik etiketlerle segmente itin: Etiketler özelliği, ister bir segmente ister segmentlerin bir ifadesine (Örneğin, etkin VE Seattle NOT yeni kullanıcısında yaşıyor olun) ister aygıtları segmente eklemenize ve gereksinimlerinize göre itmenize yardımcı olur. Yayımlama-abone olma ile sınırlı olmak yerine, aygıt etiketlerini her yerde ve istediğiniz zaman güncelleyebilirsiniz.
  - Yerelleştirilmiş itme: Şablonlar özelliği arka uç kodunu etkilemeden yerelleştirme elde yardımcı olur.
  - Sessiz gönderme: Cihazlara sessiz bildirimler göndererek ve belirli çekme veya eylemleri tamamlamak üzere cihazları tetikleyerek gönderme-çekme düzenini etkinleştirebilirsiniz.
  - Zamanlanmış itme: Bildirimleri istediğiniz zaman gönderilecek şekilde zamanlayabilirsiniz.
  - Doğrudan gönderme: Notification Hubs hizmetine cihaz kaydetme işlemini atlayabilir ve doğrudan bir cihaz tanıtıcıları listesine toplu gönderim yapabilirsiniz.
  - Kişiselleştirilmiş itme: Aygıt itme değişkenleri, özelleştirilmiş anahtar değeri çiftleri ile cihaza özgü kişiselleştirilmiş anında iletme bildirimleri göndermenize yardımcı olur.
- **Zengin telemetri**
  - Genel itme, aygıt, hata ve işletim telemetrisi hem Azure portalında hem de programlı olarak kullanılabilir.
  - İleti başına telemetri, ilk istek çağrınızdan Bildirim Hub'ları hizmetine gelen her itme hamlesini başarıyla gönderir.
  - Platform Bildirim Sistemi geri bildirimi, hata ayıklama da yardımcı olmak için PNS'lerden gelen tüm geri bildirimleri iletir.
- **Ölçeklenebilir -lik**
  - Yeniden tasarlama veya aygıt parçalama olmadan milyonlarca cihaza hızlı iletiler gönderin.
- **Güvenlik**
  - Paylaşılan Erişim Gizli Dizisi (SAS) veya şirket dışı kimlik doğrulaması.

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Mobil uygulamalara anında iletme bildirimleri gönderme](notification-hubs-android-push-notification-google-fcm-get-started.md) bölümünü takip ederek bildirim hub’ı oluşturmaya ve kullanmaya başlayın.

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png

[How customers are using Notification Hubs]: https://azure.microsoft.com/services/notification-hubs
[Notification Hubs tutorials and guides]: https://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: https://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: https://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: https://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: https://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure portal]: https://portal.azure.com
[tags]: (https://msdn.microsoft.com/library/azure/dn530749.aspx)
