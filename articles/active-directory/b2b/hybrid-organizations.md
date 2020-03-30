---
title: Karma kuruluşlar için B2B işbirliği - Azure AD
description: Azure AD B2B işbirliği ile iş ortaklarına şirket içi ve bulut kaynaklarına erişim hakkı verin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 427f7ad4d6a1b9839b1197ef9f7ca15400ea0f59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272480"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Karma kuruluşlar için Azure Active Directory B2B işbirliği

Azure Active Directory (Azure AD) B2B işbirliği, dış iş ortaklarınıza kuruluşunuzdaki uygulamalara ve kaynaklara erişim vermenizi kolaylaştırır. Bu, hem şirket içi hem de bulut tabanlı kaynaklara sahip olduğunuz karma yapılandırmada bile geçerlidir. Şu anda şirket içi kimlik sisteminizde harici iş ortağı hesaplarını yerel olarak yönetip yönetmediğiniz veya buluttaki dış hesapları Azure AD B2B kullanıcıları olarak yönetip yönetmediğiniz önemli değildir. Artık bu kullanıcılara her iki ortam için de aynı oturum açma kimlik bilgilerini kullanarak her iki konumdaki kaynaklara erişim izni verebilirsiniz.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>Azure AD'deki B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme

Kuruluşunuz, ortak kuruluşlardan azure AD'nize konuk kullanıcıları davet etmek için Azure AD B2B işbirliği özelliklerini kullanıyorsa, artık bu B2B kullanıcılarına şirket içi uygulamalara erişim sağlayabilirsiniz.

SAML tabanlı kimlik doğrulaması kullanan uygulamalarda, kimlik doğrulaması için Azure AD Application Proxy'yi kullanarak bu uygulamaları Azure portalı üzerinden B2B kullanıcılarının kullanımına sunabilirsiniz.

Kerberos kısıtlı temsilciliği (KCD) ile Tümleşik Windows Kimlik Doğrulaması (IWA) kullanan uygulamalar için, kimlik doğrulaması için Azure AD Proxy'yi de kullanırsınız. Ancak, çalışma izni için, şirket içi Windows Server Active Directory'de bir kullanıcı nesnesi gereklidir. B2B konuk kullanıcılarınızı temsil eden yerel kullanıcı nesneleri oluşturmak için kullanabileceğiniz iki yöntem vardır.

- Microsoft Identity Manager (MIM) 2016 SP1 ve Microsoft Graph için MIM yönetim aracısını kullanabilirsiniz.
- PowerShell komut dosyası kullanabilirsiniz. (Bu çözüm MIM gerektirmez.)

Bu çözümleri nasıl uygulayacağınız hakkında ayrıntılı bilgi için, [Azure AD'deki Grant B2B kullanıcılarınA şirket içi uygulamalarınıza erişim](hybrid-cloud-to-on-premises.md)bakın.

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Yerel olarak yönetilen iş ortağı hesaplarına bulut kaynaklarına erişim izni verme

Azure AD'den önce, şirket içi kimlik sistemlerine sahip kuruluşlar, şirket içi dizinde geleneksel olarak iş ortağı hesaplarını yönetmektedir. Böyle bir kuruluşsanız, uygulamalarınızı ve diğer kaynaklarınızı buluta taşırken iş ortaklarınızın erişimine devam etmesini istersiniz. İdeal olarak, bu kullanıcıların hem bulut hem de şirket içi kaynaklara erişmek için aynı kimlik bilgilerini kullanmasını istiyorsunuz. 

Artık, bu yerel hesapları bulutla "konuk kullanıcılar" olarak senkronize etmek için Azure AD Connect'i kullanabileceğiniz ve hesapların Azure AD B2B kullanıcıları gibi çalıştığı yöntemler sunuyoruz.

Şirket verilerinizin korunmasına yardımcı olmak için, yalnızca doğru kaynaklara erişimi denetleyebilir ve bu konuk kullanıcılara çalışanlarınızdan farklı davranan yetkilendirme ilkelerini yapılandırabilirsiniz.

Uygulama ayrıntıları için bkz: [Azure AD B2B işbirliğini kullanarak yerel olarak yönetilen iş ortağı hesaplarını bulut kaynaklarına erişin.](hybrid-on-premises-to-cloud.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD'deki B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme](hybrid-cloud-to-on-premises.md)
- [Azure AD B2B işbirliğini kullanarak yerel olarak yönetilen iş ortağı hesaplarına bulut kaynaklarına erişim hakkı verme](hybrid-on-premises-to-cloud.md)


