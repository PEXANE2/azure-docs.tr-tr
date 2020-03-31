---
title: Visual Studio App Center ve Azure Bildirim Hub'ları ile mobil uygulamalarınızda anında iletme bildirimlerinin önemi
description: Görsel Stüdyo Uygulama Merkezi gibi mobil uygulama kullanıcılarınızla etkileşimde bulunmak için kullanabileceğiniz hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: bc59857cdb843dfff54e69f2226f13cfe70df8f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235327"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Anında iletme bildirimleri göndererek uygulama kullanıcılarınızla iletişim kurma

İster tüketici ister kurumsal uygulamalar oluşturuyor olun, kullanıcıların uygulamanızı etkin bir şekilde kullanmasını istersiniz. Son dakika haberlerini, oyun güncellemelerini, heyecan verici teklifleri, ürün güncellemelerini veya diğer alakalı bilgileri kullanıcılarınızla paylaşmak istediğiniz zamanlar sık sık vardır. Kullanıcılarınızla etkileşimi artırmak ve uygulamanıza geri dönmelerini sağlamak için, kullanıcılarınızla gerçek zamanlı olarak iletişim kurmanın bir yolunu bulabilmeniz gerekir.

Anında iletme bildirimleri, uygulama kullanıcılarınızla iletişim kurmak için hızlı ve verimli bir yol sağlar. Kullanıcılarınıza doğru zamanda ulaşabilir ve ne yaptıklarından bağımsız olarak, genellikle mobil cihazdaki açılır öğe veya iletişim kutusunda istenen bilgileri bildirebilirsiniz.

## <a name="value-of-push-notifications"></a>Anında iletme bildirimlerinin değeri
Anında iletme bildirimleri kullanıcılara ve işletmelere değer sağlar.

İşletmeler şunları yapabilir:
- Doğru zamanda desteklenen platformlarda mesaj göndererek kullanıcılarla doğrudan iletişim kurun.
- Kullanıcılara gerçek zamanlı güncellemeler ve anımsatıcılar göndererek kullanıcı etkileşimini artırarak, onları uygulamayla düzenli olarak etkileşime girmeye teşvik edin.
- Kullanıcıları koruyun ve uygulamayı indiren ancak yeniden etkin hale getirmek için kullanmayan etkin olmayan kullanıcılarla yeniden etkileşime girişin.
- Kullanıcı davranışını analiz ederek, kullanıcıları bölümlere ayırarak ve kampanyaları mobil anında iletme bildirimlerine dayalı olarak yararlanarak dönüşüm oranlarını artırın.
- Kullanıcılara anında iletme iletileri ve zamanında güncellemeler göndererek ürünleri ve hizmetleri tanıtın.
- Kişiselleştirilmiş anında iletme iletileri göndererek kullanıcıları hedefle.

Uygulama kullanıcıları için anında iletme bildirimleri:
- Gerçek zamanlı olarak değer ve bilgi sağlayın.
- Kullanıcılara uygulamayı kullanmalarını hatırlatın.

Mobil uygulamalarınızda anında iletme bildirimlerini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Push](/appcenter/push/)ile, anında iletme bildirim işlemi hizmetlerini (PNS) kullanarak cihazlara bildirim gönderme sürecini yönetmek zorunda kalmadan iOS, Android ve Windows kullanıcılarına hedefli iletiler göndereceklerini gönderecek. Azure Bildirim Hub'larının üzerine inşa edilen bu hizmet, güçlü bir pano sağlayarak bildirimleri el ile itmeyle ilişkili karmaşıklıkları ortadan kaldırır.

**Önemli özellikler**
- Çeşitli platformlarda mobil cihazlara anında iletme bildirimleri gönderin.
- Bir uygulamaya veri göndermek, kullanıcıya ileti görüntülemek veya uygulama tarafından bir eylemi tetiklemek için bildirimleri kullanın.
- Bildirim hedeflerini şu şekilde kullanın: 
    - İletileri tüm kayıtlı aygıtlara yayınla.
    - Aygıt bilgilerine ve özel özelliklere göre hedef kitlelere bildirim gönderin.
    - Belirli kullanıcılara bildirim gönderin.
    - Belirli aygıtlara bildirim gönderin.
- Uygulama Merkezi portalında bulunan itmelerde, cihazlarda ve hatalarda bulunan zengin telemetriden yararlanın.
- iOS, Android, macOS, Xamarin, React Native, Unity ve Cordova için platform desteği kazanın.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Push ile başlayın](/appcenter/push/)

## <a name="azure-notification-hubs"></a>Azure Notification Hubs
[Bildirim Hub'ları](/azure/notification-hubs/notification-hubs-push-notification-overview) kullanımı kolay ve ölçeklendirilen bir itme motoru sağlar. Herhangi bir platforma ve bulutta veya şirket içinde herhangi bir arka uçtan bildirim göndermek için kullanabilirsiniz.

**Önemli özellikler**
- Herhangi bir platforma herhangi bir arka uçtan, bulutta veya şirket içinde anında iletme bildirimleri gönderin.
- Tek bir API çağrısıyla milyonlarca mobil cihaza hızlı yayın hamlesi.
- Anında iletme bildirimlerini müşteriye, dile ve konuma göre uyarlayın.
- Müşteri segmentlerini dinamik olarak tanımlayın ve bilgilendirin.
- Milyonlarca mobil cihaza anında ölçeklendirin.
- iOS, Android, Windows, Kindle ve Baidu için platform desteği kazanın.
        
**Başvurular**
- [Azure portalında](https://portal.azure.com) 
- [Azure Bildirim Hub'ları ile başlayın](/azure/notification-hubs/)
- [Hızlı başlangıçlar](/azure/notification-hubs/create-notification-hub-portal)
- [Örnekler](/azure/notification-hubs/samples)
