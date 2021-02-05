---
title: 'Hızlı başlangıç: Azure Active Directory kiracı oluşturma'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, kimlik doğrulaması ve yetkilendirme için Microsoft Identity platformunu kullanan uygulamalar geliştirmede kullanılmak üzere Azure Active Directory kiracı oluşturmayı öğreneceksiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: quickstart
ms.date: 03/12/2020
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 41d70b20708f0f355fab5b5a06790c1c0c6530c6
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99583561"
---
# <a name="quickstart-set-up-a-tenant"></a>Hızlı Başlangıç: Kiracıyı ayarlama

Microsoft kimlik platformu geliştiricilerin çok çeşitli özel Microsoft 365 ortamlarını ve kimliklerini hedefleyen uygulamalar derlemesine olanak tanır. Microsoft Identity platform 'u kullanmaya başlamak için, Azure AD kiracısı olarak da adlandırılan bir ortama erişmeniz gerekir. Bu işlem, uygulamaları kaydedebilir ve yönetebilir, Microsoft 365 verilerine erişim sağlayabilir ve özel koşullu erişim ve kiracı kısıtlamaları dağıtabilir.

Kiracı, bir kuruluşu temsil eden birimdir. Kuruluş veya uygulama geliştirici Microsoft'la bir ilişki oluşturduğunda, örneğin Azure'a, Microsoft Intune'a veya Microsoft 365'e kaydolduğunda kuruluşun veya uygulama geliştiricinin aldığı özel bir Azure AD örneğidir.

Her Azure AD kiracısı ayrıdır ve diğer Azure AD kiracılarından ayrılmıştır. Kendi iş ve okul kimlikleri, tüketici kimlikleri (bir Azure AD B2C kiracısıysa) ve uygulama kayıtları gösterimi vardır. Kiracınızın içinde yapılan bir uygulama kaydı, yalnızca kiracınızın veya tüm kiracıların içinden hesapların kimlik doğrulaması yapmasına izin verebilir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="determining-environment-type"></a>Ortam türünü saptama

İki tür ortam oluşturabilirsiniz. Hangisine ihtiyacınız olduğuna karar verirken yalnızca uygulamanızın kimliklerini doğrulayacağı kullanıcı türlerine bakılır.

* İş ve okul hesapları (Azure AD hesapları) veya Microsoft hesapları (outlook.com ve live.com gibi)
* Sosyal ve yerel hesaplar (Azure AD B2C)

Bu hızlı başlangıç, derlemek istediğiniz uygulamanın türüne bağlı olarak iki senaryoya bölünmüştür.

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>İş ve okul hesapları veya kişisel Microsoft hesapları

### <a name="use-an-existing-tenant"></a>Mevcut kiracıyı kullanma

Birçok geliştiricinin, Azure AD kiracılarına bağlı hizmetler veya abonelikler (örneğin, Microsoft 365 veya Azure abonelikleri) aracılığıyla zaten kiracıları vardır.

1. Kiracıyı denetlemek için, uygulamanızı yönetmek üzere kullanmak istediğiniz hesapla <a href="https://portal.azure.com/" target="_blank">Azure Portal <span class="docon docon-navigate-external x-hidden-focus"></span> </a> oturum açın.
1. Sağ üst köşeye bakın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz.
   * Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin.
   * Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır.

> [!TIP]
> Kiracı KIMLIĞINI bulmak için şunları yapabilirsiniz:
> * Dizin / kiracı kimliği değerini almak için hesap adınızın üzerine gelin, veya
> * **Azure Active Directory > özellikler > KIRACı kimliği** Azure Portal arama ve seçme

Hesabınızla ilişkilendirilmiş bir kiracınız yoksa, hesap adınızın altında bir GUID görürsünüz ve sonraki bölümde verilen adımları izleyene kadar uygulamaları kaydetme gibi eylemler gerçekleştiremezsiniz.

### <a name="create-a-new-azure-ad-tenant"></a>Yeni Azure AD kiracısı oluşturma

Zaten bir Azure AD kiracınız yoksa veya geliştirme için yeni bir tane oluşturmak istiyorsanız [hızlı başlangıç](../fundamentals/active-directory-access-create-new-tenant.md) bölümüne bakın veya yalnızca [Dizin oluşturma deneyimini](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory)izleyin. Yeni kiracı oluşturmak için aşağıdaki bilgileri sağlamanız gerekecektir:

- **Kuruluş adı**
- **İlk etki alanı** - Bu, *.onmicrosoft.com'un bir parçası olacaktır. Etki alanını daha sonra özelleştirebilirsiniz.
- **Ülke veya bölge**

> [!NOTE]
> Kiracınızı adlandırırken, alfasayısal karakterler kullanın. Özel karakterlere izin verilmez. Ad 256 karakterden uzun olmamalıdır.

## <a name="social-and-local-accounts"></a>Sosyal ve yerel hesaplar

Sosyal ve yerel hesapların oturumunu açan bir uygulama derlemeye başlamak için, bir Azure AD B2C kiracısı oluşturmanız gerekir. Başlamak için, [Azure AD B2C kiracısı oluşturma](../../active-directory-b2c/tutorial-create-tenant.md) yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> Microsoft Identity platform ile tümleştirilecek [uygulamayı kaydedin](quickstart-register-app.md) .
