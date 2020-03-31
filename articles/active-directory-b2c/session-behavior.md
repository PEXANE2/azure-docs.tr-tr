---
title: Oturum davranışını yapılandırma - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Etkin Dizin B2C'de oturum davranışını yapılandırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c1d39fdbca9484f47ce0c8537c82247b75b2e3db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186820"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Azure Etkin Dizin B2C'de oturum davranışını yapılandırma

Bu özellik, [kullanıcı başına akış bazında,](user-flow-overview.md)ince taneli denetim sağlar:

- Azure AD B2C tarafından yönetilen web uygulama oturumlarının ömür boyu.
- Azure AD B2C kiracınızda birden çok uygulama ve kullanıcı akışı nda tek oturum açma (SSO) davranışı.

Bu ayarlar parola sıfırlama kullanıcı akışları için kullanılamaz.

Azure AD B2C, web uygulamalarında güvenli oturum açmasağlamak için [OpenID Connect kimlik doğrulama protokolünü](openid-connect.md) destekler. Web uygulama oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

## <a name="session-behavior-properties"></a>Oturum davranış özellikleri

- **Web uygulaması oturumu nun ömrü (dakika)** - Azure AD B2C'nin oturum çerezinin kullanım ömrü, başarılı bir kimlik doğrulaması üzerine kullanıcının tarayıcısında depolanır.
    - Varsayılan = 1440 dakika.
    - Minimum (dahil) = 15 dakika.
    - Maksimum (dahil) = 1440 dakika.
- **Web uygulaması oturum zaman aşımı** - Bu anahtar **Mutlak**olarak ayarlanırsa, kullanıcı Web **uygulaması oturum süresi (dakika)** tarafından belirtilen süre geçtikten sonra yeniden kimlik doğrulaması yapmak zorunda kalır. Bu anahtar **Rolling** (varsayılan ayar) olarak ayarlanmışsa, kullanıcı web uygulamanızda sürekli olarak etkin olduğu sürece oturum açmış olarak kalır.
- **Tek oturum açma yapılandırması** B2C kiracınızda birden çok uygulama nız ve kullanıcı akışınız varsa, **Tek oturum açma yapılandırma** özelliğini kullanarak kullanıcı etkileşimlerini yönetebilirsiniz. Özelliği aşağıdaki ayarlardan birine ayarlayabilirsiniz:
    - **Kiracı** - Bu ayar varsayılandır. Bu ayarı kullanmak, B2C kiracınızdaki birden çok uygulamanın ve kullanıcı akışının aynı kullanıcı oturumunu paylaşmasına olanak tanır. Örneğin, bir kullanıcı bir uygulamayı oturum alabildiği zaman, kullanıcı bir uygulamaya erişerek başka bir uygulama olan Contoso Pharmacy'de sorunsuz bir şekilde oturum açabilir.
    - **Uygulama** - Bu ayar, diğer uygulamalardan bağımsız olarak yalnızca bir uygulama için bir kullanıcı oturumu tutmanızı sağlar. Örneğin, kullanıcının Contoso Pharmacy'de (aynı kimlik bilgileriyle) oturum açmasını istiyorsanız, kullanıcı aynı B2C kiracısı üzerinde başka bir uygulama olan Contoso Shopping'de zaten oturum açmış olsa bile.
    - **İlke** - Bu ayar, kullanıcı oturumunu yalnızca kullanan uygulamalardan bağımsız olarak bir kullanıcı akışı için korumanızı sağlar. Örneğin, kullanıcı zaten oturum açmış ve çok faktörlü bir kimlik doğrulama (MFA) adımını tamamlamışsa, kullanıcı akışına bağlı oturumun süresi dolmadığı sürece, kullanıcıya birden çok uygulamanın daha yüksek güvenlikli bölümlerine erişim izni verilebilir.
    - **Devre Dışı -** Bu ayar, kullanıcıyı ilkenin her yürütülmesinde tüm kullanıcı akışını çalıştırmaya zorlar.

Aşağıdaki kullanım örnekleri bu özellikler kullanılarak etkinleştirilir:

- Uygun web uygulaması oturumu yaşam ömürlerini ayarlayarak endüstrinizin güvenlik ve uyumluluk gereksinimlerini karşılayın.
- Kullanıcının web uygulamanızın yüksek güvenlikli bir bölümüyle etkileşimi sırasında belirli bir süre sonra kimlik doğrulamasını zorlar.

## <a name="configure-the-properties"></a>Özellikleri yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD B2C kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Kullanıcı akışlarını (ilkeler)** seçin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikleri**seçin.
7. Web **uygulaması oturum ömrünü (dakika)**, **Web uygulaması oturum zaman aşım**, Tek oturum **yapılandırma**ve gerektiğinde oturum **açma isteklerinde Kimlik Belirteci gerektiren** yapılandırma.

    ![Azure portalında oturum davranışı özelliği ayarları](./media/session-behavior/session-behavior.png)

8. **Kaydet**'e tıklayın.
