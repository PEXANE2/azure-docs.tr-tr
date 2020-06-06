---
title: Mobil uygulamalarınızda Visual Studio App Center ve Azure Notification Hubs anında iletme bildirimlerinin önemi
description: Mobil uygulama kullanıcılarınız ile birlikte çalışmak için kullanabileceğiniz Visual Studio App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: c9ae5e268066e498b3a341a9c54391ee5d15d592
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450657"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Anında iletme bildirimleri göndererek uygulama kullanıcılarınıza katılın

Tüketici veya kurumsal uygulamalar oluştururken kullanıcıların uygulamanızı etkin bir şekilde kullanmasını istersiniz. Son haberleri, oyun güncelleştirmelerini, heyecan verici teklifleri, ürün güncelleştirmelerini veya diğer ilgili bilgileri kullanıcılarınız ile paylaşmak istediğinizde sıkça zaman vardır. Kullanıcılarınıza yönelik katılımı artırmak ve uygulamanıza geri dönmek için, kullanıcılarınız ile gerçek zamanlı iletişim kurmak için bir yol gerekir.

Anında iletme bildirimleri, uygulama kullanıcılarınız ile iletişim kurmanın hızlı ve verimli bir yolunu sağlar. Kullanıcılarınıza doğru zamanda ulaşın ve bu bilgilere, genellikle bir mobil cihazdaki bir açılır öğe veya iletişim kutusunda, ne yapacaklarından bağımsız olarak, istenen bilgileri bildirebilirsiniz.

## <a name="value-of-push-notifications"></a>Anında iletme bildirimlerinin değeri
Anında iletme bildirimleri, kullanıcılara ve işletmelere değer sağlar.

İşletmeler şunları yapabilir:
- Yalnızca desteklenen platformlarda iletileri doğru zamanda göndererek kullanıcılarla doğrudan iletişim kurun.
- Kullanıcılara gerçek zamanlı güncelleştirmeler ve anımsatıcılar göndererek kullanıcı etkileşimini güçlendirin ve uygulamayı düzenli olarak teşvik.
- Uygulamayı indirmiş ancak tekrar etkin hale gelmek için kullanmayan etkin olmayan kullanıcılarla kullanıcıları koruyun ve yeniden devreye sok.
- Kullanıcı davranışını çözümleyerek, kullanıcıları bölümleyerek ve mobil anında iletme bildirimlerine göre kampanyaları kullanarak dönüştürme oranlarını artırın.
- Kullanıcılara anında iletme iletileri ve güncel güncelleştirmeler göndererek ürün ve Hizmetleri yükseltin.
- Kişiselleştirilmiş anında iletme iletileri göndererek kullanıcıları hedefleyin.

Uygulama kullanıcıları için anında iletme bildirimleri:
- Gerçek zamanlı olarak değer ve bilgi sağlayın.
- Kullanıcılara uygulamayı kullanmasını hatırlatın.

Mobil uygulamalarınızda anında iletme bildirimlerini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center gönderimi](/appcenter/push/)sayesinde, anında iletme bildirimi Hizmetleri 'NI (PNS) kullanarak cihazlara bildirim gönderme işlemini yönetmek zorunda kalmadan IOS, Android ve Windows kullanıcılarına hedeflenmiş iletileri gönderebilirsiniz. Azure Notification Hubs üzerine inşa ederek, bu hizmet güçlü bir pano sunarak bildirimleri el ile iletme ile ilişkili karmaşıklıkları ortadan kaldırır.

**Önemli özellikler**
- Çeşitli platformlar arasında mobil cihazlara anında iletme bildirimleri gönderin.
- Bir uygulamaya veri göndermek, kullanıcıya bir ileti göstermek veya uygulama tarafından bir eylem tetiklemek için bildirimleri kullanın.
- Bildirim hedeflerini şu şekilde kullanın: 
    - İletileri tüm kayıtlı cihazlara yayınla.
    - Cihaz bilgilerine ve özel özelliklere göre hedef kitlelere bildirimler gönderin.
    - Belirli kullanıcılara bildirimler gönderin.
    - Belirli cihazlara bildirim gönderin.
- App Center portalında bulunan gönderimlerin, cihazların ve hataların zengin telemetrisini kullanın.
- İOS, Android, macOS, Xamarin, yerel, Unity ve Cordova için platform desteği kazanın.

**Başvurular**
- [Visual Studio App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Push ile çalışmaya başlama](/appcenter/push/)

## <a name="azure-notification-hubs"></a>Azure Bildirim Merkezleri
[Notification Hubs](/azure/notification-hubs/notification-hubs-push-notification-overview) kullanımı kolay ve ölçeği genişletilmiş bir gönderim altyapısı sağlar. Bunu, herhangi bir platforma ve buluttaki ya da Şirket içindeki herhangi bir arka uçtan bildirim göndermek için kullanabilirsiniz.

**Önemli özellikler**
- Bulutta veya şirket içinde herhangi bir arka uçtan herhangi bir platforma anında iletme bildirimleri gönderin.
- Tek bir API çağrısıyla milyonlarca mobil cihaza hızlı yayın gönderme.
- Anında iletme bildirimlerini müşteri, dil ve konuma göre uyarlayın.
- Müşteri segmentlerini dinamik olarak tanımlayın ve bilgilendirin.
- Milyonlarca mobil cihaza anında ölçeklendirin.
- İOS, Android, Windows, Ille ve Baidu için platform desteği kazanın.
        
**Başvurular**
- [Azure portal](https://portal.azure.com) 
- [Azure Notification Hubs kullanmaya başlama](/azure/notification-hubs/)
- [Hızlı Başlangıçlar](/azure/notification-hubs/create-notification-hub-portal)
- [Örnekler](/azure/notification-hubs/samples)
