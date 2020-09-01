---
title: Azure AD B2C kimlik koruması ve koşullu erişim
description: Kimlik korumasının riskli oturum açma işlemleri ve risk algılamaları hakkında görünürlük sağladığını öğrenin. Ve Koşullu erişimin, Azure AD B2C Kiracılarınızın risk olaylarına bağlı olarak kurumsal ilkeleri zorunlu kılmaya nasıl izin veren hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 921d9e2138c8aa9c09535a673a7cd2d32e9cddad
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89271590"
---
# <a name="identity-protection-and-conditional-access-for-azure-ad-b2c"></a>Azure AD B2C için kimlik koruması ve koşullu erişim

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Azure AD Kimlik Koruması ve koşullu erişimle Azure Active Directory B2C (Azure AD B2C) güvenliğini artırın. Riskli kullanıcılar ve riskli oturum açma işlemleri de dahil olmak üzere kimlik koruması risk algılama özellikleri otomatik olarak algılanır ve Azure AD B2C kiracınızda görüntülenir. Eylemleri belirlemede ve kuruluş ilkelerini zorlayabilmeniz için bu risk algılamalarını kullanan koşullu erişim ilkeleri oluşturabilirsiniz. Bu yetenekler birlikte, riskli kimlik doğrulamaları ve erişim ilkeleri üzerinde Azure AD B2C uygulama sahipleri daha fazla denetim sağlar.
  
Azure AD 'de [kimlik koruması](../active-directory/identity-protection/overview-identity-protection.md) ve [koşullu erişim](../active-directory/conditional-access/overview.md) hakkında zaten bilgi sahibiyseniz bu özellikleri Azure AD B2C ile kullanmak tanıdık bir deneyim olduğunu fark eder ve bu makalede ele alınan küçük farklılıklar vardır.

![B2C kiracısında koşullu erişim](media/conditional-access-identity-protection-overview/conditional-access-b2c.png)

> [!NOTE]
> Koşullu erişimi kullanmak için Azure AD B2C Premium P2 gereklidir.

## <a name="benefits-of-identity-protection-and-conditional-access-for-azure-ad-b2c"></a>Azure AD B2C için kimlik koruması ve Koşullu erişimin avantajları  

Kimlik koruması riski algılama ile koşullu erişim ilkelerini eşleştirerek, riskli kimlik doğrulamaları için uygun ilke eylemiyle yanıt verebilirsiniz.

- **Uygulamalarınız ve müşteri tabanınız için kimlik doğrulama risklerine yeni bir görünürlük düzeyi kazanın**. Azure AD ve Microsoft hesabı genelinde milyarlarca aylık kimlik doğrulama sinyalleriyle, risk algılama algoritmaları artık yerel tüketici veya vatandaşlık kimlik doğrulamalarınız için kimlik doğrulamaları düşük, orta veya yüksek riskli olarak işaretleyecektir.
- **Kendi Uyarlamalı kimlik doğrulamasını yapılandırarak riskleri otomatik olarak**çözün. Belirtilen uygulamalar için, çok faktörlü kimlik doğrulamasında (MFA) olduğu gibi, belirli bir kullanıcı kümesinin ikinci bir kimlik doğrulama faktörü sağlaması gerekir. Veya, algılanan risk düzeyine bağlı olarak erişimi engelleyebilirsiniz. Diğer Azure AD B2C deneyimleriyle birlikte, sonuçta elde edilen son kullanıcı deneyimini kuruluşunuzun sesiyle, stiliyle ve markasına göre özelleştirebilirsiniz. Kullanıcı erişim sağlayamıyorsa, risk azaltma alternatifleri de gösterebilirsiniz.
- **Konuma, gruplara ve uygulamalara göre erişimi denetleyin**.Koşullu erişim, risk tabanlı olmayan durumları denetlemek için de kullanılabilir. Örneğin, belirli bir uygulamaya erişen müşterilere MFA gerektirebilir veya belirtilen coğrafi cihazlardan erişimi engelleyebilirsiniz.
- **Azure AD B2C Kullanıcı akışları ve kimlik deneyimi çerçevesi özel Ilkeleriyle tümleştirin**. Mevcut özelleştirilmiş deneyimlerinizi kullanın ve koşullu erişim ile arabirim için gereken denetimleri ekleyin. Ayrıca, bilgi tabanlı erişim veya kendi tercih ettiğiniz MFA sağlayıcınız gibi erişim vermek için gelişmiş senaryolar da uygulayabilirsiniz.

## <a name="feature-differences-and-limitations"></a>Özellik farklılıkları ve sınırlamaları

Azure AD B2C ' de kimlik koruması ve koşullu erişim, Azure AD 'de olduğu gibi, aşağıdaki özel durumlarla aynı şekilde çalışır:

- Güvenlik Merkezi Azure AD B2C ' de kullanılamaz.

- Kimlik koruması ve koşullu erişim, Azure AD B2C kiracılarındaki ROPC sunucudan sunucuya akışlar için desteklenmez.

- Azure AD B2C kiracılarda, kimlik koruması risk algılamaları yalnızca yerel B2C hesapları için kullanılabilir ve Google veya Facebook gibi sosyal kimlikler için kullanılamaz.

- Azure AD B2C kiracılarda, kimlik koruması risk algılamaları için bir alt küme mevcuttur. Bkz. [kimlik koruması ayarlama](conditional-access-identity-protection-setup.md#set-up-identity-protection).

- Koşullu erişim cihaz uyumluluğu özelliği Azure AD B2C kiracılarda kullanılamaz.


## <a name="integrate-conditional-access-with-user-flows-and-custom-policies"></a>Koşullu erişimi Kullanıcı akışları ve özel ilkelerle tümleştirme

Azure AD B2C, yerleşik Kullanıcı akışlarından koşullu erişim koşullarını tetikleyebilirsiniz. Koşullu erişimi özel ilkelerle de ekleyebilirsiniz. B2C Kullanıcı akışının diğer yönlerini de olduğu gibi, son kullanıcı deneyimi mesajlaşma, kuruluşunuzun ses, marka ve risk azaltma alternatiflerine göre özelleştirilebilir. Bkz. [koşullu erişim teknik profili tanımlama](conditional-access-technical-profile.md).

## <a name="microsoft-graph-api"></a>Microsoft Graph API

Koşullu erişim ilkelerini, Microsoft Graph API ile Azure AD B2C de yönetebilirsiniz. Ayrıntılar için bkz. [koşullu erişim belgeleri](../active-directory/conditional-access/overview.md) ve [Microsoft Graph başvurusu](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C için kimlik koruması ve koşullu erişim ayarlama](conditional-access-identity-protection-setup.md)
- [Azure AD 'de kimlik koruması hakkında bilgi edinin](../active-directory/identity-protection/overview-identity-protection.md)
- [Koşullu erişim hakkında bilgi edinin](../active-directory/conditional-access/overview.md)
