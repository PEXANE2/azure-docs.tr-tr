---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulamalarınıza kimlik doğrulaması ekleme
description: Kullanıcı kimlik doğrulamasını ayarlamaya ve mobil uygulamaların sosyal hesaplar, Azure Active Directory ve özel kimlik doğrulamasıyla kimlik doğrulaması yapmasını sağlayan Visual Studio App Center gibi hizmetler hakkında bilgi edinin.
author: elamalani
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: bc7d8d4aaec6ebe27a0f8d2ecc11ca408266f7ef
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453236"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>Mobil uygulamalarınızdaki kimlik doğrulaması ve Kullanıcı kimliklerini yönetme

Kullanıcı görünümünü ve uygulamanızda davranışlarını, geliştiricilerin kendilerine yönelik özel deneyimler oluşturarak daha iyi bir şekilde devreye girmesini sağlar. Kuruluşunuzdaki kullanıcılar için işbirliği uygulaması oluşturan veya bir sonraki sosyal ağ platformunu oluşturduğunuz bir uygulama geliştiricisi olup olmadığınız, kullanıcıların kimliğini doğrulamak ve Kullanıcı kimliklerini yönetmek için bir yol gerekir. Bir kimlik yönetimi hizmeti, bir mobil arka uç hizmetinin en önemli özelliklerinden biridir.

Mobil uygulamalarınızda Kullanıcı kimlik doğrulamasını etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center auth](/appcenter/auth/) , geliştiricilerin kullanıcıların kimliğini doğrulamak ve Kullanıcı kimliklerini yönetmek için kullanabileceği bulut tabanlı bir kimlik yönetimi hizmetidir. App Center kimlik doğrulaması, Visual Studio App Center diğer bölümleriyle de tümleştirilir. Geliştiriciler Kullanıcı kimliğini diğer hizmetlerde [Kullanıcı verilerini görüntülemek](/appcenter/data/index) ve hatta [tek tek cihazlar yerine kullanıcılara anında iletme bildirimleri göndermek](/appcenter/push/push-to-user#app-center-auth-set-identity)için kullanabilir. 

**Önemli özellikler**
- Azure Active Directory B2C ile desteklenir (Azure AD B2C). 
    - Kurumsal sınıf.
    - Yüksek oranda kullanılabilir.
    - Güvenli ve küresel hizmet.
- Auth0 ve Firebase gibi diğer popüler kimlik ve erişim yönetimi sağlayıcılarını kullanma seçeneğini ve kendi kimliğinizi getirin.
- Azure Active Directory desteği.
    - Mevcut Azure AD kiracılarını bağlayın. 
    - Şirket etki alanında kimlik doğrulamasını etkinleştirin.
    - Gizli verilere erişimi yönetin.
- Visual Studio App Center SDK ile Microsoft kimlik doğrulama kitaplığı 'nı sarmalayarak basit kullanıcı deneyimi ve mıknatıik SDK deneyimi.
- İOS, Android, Xamarin ve yerel olarak tepki verme için platform desteği.

**Başvurular**
- [Visual Studio App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs) 
- [App Center kimlik doğrulaması ile çalışmaya başlama](/appcenter/auth/)

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C
[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) , geliştiricilerin müşterilerinin kimliğini doğrulamak için kullanabileceği, işletmeden MÜŞTERIYE (B2C) kimlik yönetimi hizmetidir. Bu beyaz etiket Hizmeti, geliştiricilerin Web, Masaüstü, mobil veya tek sayfalı uygulamalarla güvenli şekilde nasıl etkileşim kuracağını özelleştirip denetlemesine olanak tanır. Azure AD B2C kullanarak, kullanıcılar kaydolabilir, oturum açabilir, parolaları sıfırlayabilir ve profilleri düzenleyebilir. Azure AD B2C, OpenID Connect ve OAuth 2,0 protokollerinin bir formunu uygular. 

**Önemli özellikler**
- Tercih edilen kimlik sağlayıcısıyla müşterilerin kimliklerini güvenli bir şekilde doğrulayın.
- Müşteri kimliğini ve erişimini yönetin.
- Facebook, GitHub, Google, LinkedIn, Twitter, WeChat ve Weibo gibi sosyal medya için oturum açma desteği elde edin.
- Çeşitli platformlarda kimlik yönetimine olanak sağlamak için OpenID Connect veya SAML gibi sektör standardı protokollerini kullanarak kullanıcı hesaplarınıza bağlanın.
- Markalı kayıt ve oturum açma deneyimleri sağlayın.
- CRM veritabanları, pazarlama analizi araçları ve hesap doğrulama sistemleriyle kolayca tümleştirin.
- Müşteriler için oturum açma, tercih ve dönüştürme verilerini yakalayın.

**Başvurular**
- [Azure Portal](https://portal.azure.com/)
- [Azure AD B2C belgeleri](/azure/active-directory-b2c/)
- [Hızlı başlangıçlar](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [Örnekler](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) , çalışanlarınızın Microsoft 'un bulut tabanlı kimlik ve erişim yönetimi hizmeti olduğundan, çalışanlarınızın oturum açmasını ve erişim sağlamasına yardımcı olur:
- Microsoft Office 365, Azure portal ve diğer binlerce hizmet olarak yazılım (SaaS) uygulamaları gibi dış kaynaklar.
- Şirket ağına ve intranet, kendi kuruluşunuzun içinde geliştirilen bir bulut uygulamalarının yanı sıra uygulamaları gibi iç kaynaklara.

**Önemli özellikler**
- Kullanıcıları ihtiyaç duydukları uygulamalara bağlayarak sorunsuz, yüksek güvenlikli erişim.
- Kullanıcı, konum, cihaz, veri ve uygulama bağlamına göre kimlikler ve erişim için kapsamlı kimlik koruması ve gelişmiş güvenlik.
- Office 365, Salesforce.com ve Box gibi ticari ve özel uygulamalar için önceden tümleştirilmiş binlerce uygulama.
- Ölçeği ölçeğinde erişimi yönetme özelliği.

**Başvurular**
- [Azure Portal](https://portal.azure.com/)
- [Azure AD nedir?](/azure/active-directory/fundamentals/active-directory-whatis)
- [Azure Active Directory ile çalışmaya başlama](/azure/active-directory/fundamentals/active-directory-whatis)
- [Hızlı başlangıçlar](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)