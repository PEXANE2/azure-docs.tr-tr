---
title: Mobil uygulamalarınızda Visual Studio App Center ve Azure Notification Hubs anında iletme bildirimlerinin önemi
description: Mobil uygulama kullanıcılarınız ile birlikte çalışmak için kullanabileceğiniz Visual Studio App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/05/2020
ms.author: masoucou
ms.openlocfilehash: 515583a19c576d25bd9573cbf80974d72c6df1ec
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84482655"
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
- [Hızlı başlangıçlar](/azure/notification-hubs/create-notification-hub-portal)
- [Örnekler](/azure/notification-hubs/samples)
