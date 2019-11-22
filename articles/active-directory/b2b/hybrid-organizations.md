---
title: Karma kuruluşlar için B2B işbirliği-Azure AD
description: Azure AD B2B işbirliğiyle iş ortakları için hem şirket içi hem de bulut kaynaklarına erişim izni verin.
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
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74272480"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Karma kuruluşlar için B2B işbirliği Azure Active Directory

Azure Active Directory (Azure AD) B2B işbirliği, dış iş ortaklarınıza kuruluşunuzdaki uygulamalar ve kaynaklar için erişim vermenizi kolaylaştırır. Bu, hem şirket içi hem de bulut tabanlı kaynaklarınızın bulunduğu karma bir yapılandırmada bile geçerlidir. Dış iş ortağı hesaplarını şirket içi kimlik sisteminizde yerel olarak yönettiğinizde veya bulutta dış hesapları Azure AD B2B kullanıcıları olarak yönetiyorsanız bu değildir. Artık her iki ortam için de aynı oturum açma kimlik bilgilerini kullanarak bu kullanıcılara her iki konumdaki kaynaklara erişim izni verebilirsiniz.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>Şirket içi uygulamalarınıza Azure AD erişimi için B2B kullanıcıları verme

Kuruluşunuz, Konuk kullanıcıları iş ortağı kuruluşlarından Azure AD 'ye davet etmek için Azure AD B2B işbirliği özelliklerini kullanıyorsa, bu B2B kullanıcılarına şirket içi uygulamalara erişimi sağlayabilirsiniz.

SAML tabanlı kimlik doğrulaması kullanan uygulamalar için, kimlik doğrulaması için Azure AD Uygulama Ara Sunucusu kullanarak bu uygulamaları Azure portal aracılığıyla B2B kullanıcıları için kullanılabilir hale getirebilirsiniz.

Kerberos kısıtlanmış temsili (KCD) ile tümleşik Windows kimlik doğrulaması (ıWA) kullanan uygulamalar için kimlik doğrulaması için Azure AD proxy 'yi de kullanırsınız. Ancak, yetkilendirmenin çalışması için, şirket içi Windows Server Active Directory bir kullanıcı nesnesi gerekir. B2B Konuk kullanıcılarınızı temsil eden yerel kullanıcı nesneleri oluşturmak için kullanabileceğiniz iki yöntem vardır.

- Microsoft Graph için Microsoft Identity Manager (MıM) 2016 SP1 ve MıM Yönetim Aracısı 'nı kullanabilirsiniz.
- PowerShell betiğini kullanabilirsiniz. (Bu çözüm MıM gerektirmez.)

Bu çözümlerin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Azure AD 'de B2B kullanıcılarına şirket içi uygulamalarınıza erişim verme](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Yerel olarak yönetilen iş ortağı hesaplarına bulut kaynaklarına erişim izni verme

Azure AD 'den önce, şirket içi kimlik sistemlerine sahip kuruluşların şirket içi dizininde geleneksel olarak yönetilen iş ortağı hesapları vardır. Böyle bir kuruluşunuz varsa, uygulamalarınızı ve diğer kaynaklarınızı buluta taşırken iş ortaklarınızın erişime sahip olmaya devam etmesini sağlamak istersiniz. İdeal olarak, bu kullanıcıların hem buluta hem de şirket içi kaynaklara erişmek için aynı kimlik bilgileri kümesini kullanmasını istersiniz. 

Artık, hesapların Azure AD B2B kullanıcıları gibi davranan "Konuk kullanıcılar" olarak bu yerel hesapları buluta eşitlemek için Azure AD Connect kullanabileceğiniz yöntemler sunuyoruz.

Şirket verilerinizi korumaya yardımcı olmak için, yalnızca doğru kaynaklara erişimi denetleyebilir ve bu konuk kullanıcıları çalışanlarınızdan farklı şekilde işleyecek yetkilendirme ilkelerini yapılandırabilirsiniz.

Uygulama ayrıntıları için bkz. [Azure AD B2B işbirliğini kullanarak bulut kaynaklarına yerel olarak yönetilen iş ortağı hesaplarına erişim verme](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Sonraki adımlar

- [GRANT B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](hybrid-cloud-to-on-premises.md)
- [Azure AD B2B işbirliğini kullanarak bulut kaynaklarına yerel olarak yönetilen iş ortağı hesapları erişimi verme](hybrid-on-premises-to-cloud.md)


