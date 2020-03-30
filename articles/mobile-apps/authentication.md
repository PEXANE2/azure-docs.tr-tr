---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulamalarınıza kimlik doğrulama ekleme
description: Kullanıcı kimlik doğrulamasını ayarlamaya ve mobil uygulamaların sosyal hesaplar, Azure Active Directory ve özel kimlik doğrulamaile kimlik doğrulamasını sağlayan Visual Studio App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 820005bca008ea3bdb59659c815da3ec36beb0d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241045"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>Mobil uygulamalarınızda kimlik doğrulama ekleme ve kullanıcı kimliklerini yönetme

Uygulamanız genelinde kullanıcının ve davranışlarının görünümüne sahip olmak, geliştiricilerin kullanıcılar için özel deneyimler oluşturarak kullanıcılarla daha iyi etkileşime geçmelerini sağlar. Kuruluşunuzun içindeki kullanıcılar için bir işbirliği uygulaması oluşturan bir uygulama geliştiricisi veya bir sonraki sosyal ağ platformu oluşturuyor olun, kullanıcıların kimliğini doğrulamak ve kullanıcı kimliklerini yönetmek için bir yol gerekir. Kimlik yönetimi hizmeti, mobil arka uç hizmetinin en önemli özelliklerinden biridir.

Mobil uygulamalarınızda kullanıcı kimlik doğrulamasını etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Auth,](/appcenter/auth/) geliştiricilerin kullanıcıların kimliğini doğrulamak ve kullanıcı kimliklerini yönetmek için kullanabileceği bulut tabanlı bir kimlik yönetimi hizmetidir. App Center Auth, Visual Studio App Center'ın diğer bölümleriyle de entegre dir. Geliştiriciler, diğer hizmetlerdeki kullanıcı verilerini görüntülemek ve hatta [tek tek aygıtlar yerine kullanıcılara anında iletme bildirimleri göndermek için](/appcenter/push/push-to-user#setting-user-identity) [kullanıcı](/appcenter/data/index) kimliğini kullanabilir. 

**Önemli özellikler**
- Azure Etkin Dizin B2C (Azure AD B2C) tarafından desteklenmektedir. 
    - Atılgan notu.
    - Yüksek oranda kullanılabilir.
    - Güvenli ve küresel hizmet.
- Kendi kimliğinizi ve Auth0 ve Firebase gibi diğer popüler kimlik ve erişim yönetimi sağlayıcılarını kullanma seçeneğini getirin.
- Azure Active Directory desteği.
    - Varolan Azure AD kiracılarını bağlayın. 
    - Kurumsal bir etki alanına karşı kimlik doğrulamayı etkinleştirin.
    - Hassas verilere erişimi yönetin.
- Microsoft Authentication Library'yi Visual Studio App Center SDK ile saran basit kullanıcı deneyimi ve büyülü SDK deneyimi.
- iOS, Android, Xamarin ve React Native için platform desteği.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs) 
- [App Center Auth ile başlayın](/appcenter/auth/)

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C
[Azure AD B2C,](https://azure.microsoft.com/services/active-directory-b2c/) geliştiricilerin müşterilerinin kimliğini doğrulamak için kullanabilecekleri bir işletmeden tüketiciye (B2C) kimlik yönetimi hizmetidir. Bu beyaz etiket hizmeti, geliştiricilerin kullanıcıların web, masaüstü, mobil veya tek sayfalı uygulamalarıyla güvenli bir şekilde nasıl etkileşimde bulunduklarını özelleştirmelerine ve denetlemelerine olanak tanır. Kullanıcılar Azure AD B2C'yi kullanarak kaydolabilir, oturum açabilir, parolaları sıfırlayabilir ve profilleri güncelleyebilir. Azure AD B2C, OpenID Connect ve OAuth 2.0 protokollerinin bir formunu uygular. 

**Önemli özellikler**
- Müşterilerin tercih ettikleri kimlik sağlayıcısıyla güvenli bir şekilde kimlik doğrulaması.
- Müşteri kimliğini ve erişimi yönetin.
- Facebook, GitHub, Google, LinkedIn, Twitter, WeChat ve Weibo gibi sosyal medya için oturum açma desteği kazanın.
- Çeşitli platformlarda kimlik yönetimini mümkün kılmak için OpenID Connect veya SAML gibi endüstri standardı protokolleri kullanarak kullanıcı hesaplarınıza bağlanın.
- Markalı kayıt ve oturum açma deneyimleri sağlayın.
- CRM veritabanları, pazarlama analiz araçları ve hesap doğrulama sistemleriyle kolayca entegre edin.
- Müşteriler için oturum açma, tercih ve dönüşüm verilerini yakalayın.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Azure AD B2C belgeleri](/azure/active-directory-b2c/)
- [Hızlı başlangıçlar](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [Örnekler](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory,](https://azure.microsoft.com/services/active-directory/) çalışanlarınızın oturum açmasına ve aşağıdakilere erişmesine yardımcı olan Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir:
- Microsoft Office 365, Azure portalı ve hizmet olarak binlerce yazılım (SaaS) uygulaması gibi dış kaynaklar.
- Kurumsal ağınızdaki ve intranetinizdeki uygulamalar gibi dahili kaynakların yanı sıra kendi kuruluşunuz tarafından geliştirilen tüm bulut uygulamaları.

**Önemli özellikler**
- Kullanıcıları ihtiyaç duydukları uygulamalara bağlayarak kesintisiz ve son derece güvenli erişim.
- Kullanıcı, konum, cihaz, veri ve uygulama bağlamına dayalı olarak kimlikler ve erişim için kapsamlı kimlik koruması ve gelişmiş güvenlik.
- Office 365, Salesforce.com ve Box gibi hem ticari hem de özel uygulamalar için binlerce önceden entegre edilmiş uygulama.
- Ölçekte erişimi yönetebilme yeteneği.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Azure AD nedir?](/azure/active-directory/fundamentals/active-directory-whatis)
- [Azure Active Directory ile çalışmaya başlama](/azure/active-directory/fundamentals/active-directory-whatis)
- [Hızlı başlangıçlar](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)