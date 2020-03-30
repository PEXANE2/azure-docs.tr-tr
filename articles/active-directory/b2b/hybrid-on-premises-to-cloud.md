---
title: Yerel iş ortağı hesaplarını Bulut'u B2B kullanıcısı olarak senkronize etme - Azure AD
description: Azure AD B2B işbirliği ile aynı kimlik bilgilerini kullanarak yerel olarak yönetilen dış iş ortaklarına hem yerel hem de bulut kaynaklarına erişim hakkı verin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcc8c0538bb3362818a4172dd42905fd72b19812
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272608"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliğini kullanarak yerel olarak yönetilen iş ortağı hesaplarına bulut kaynaklarına erişim hakkı verme

Azure Etkin Dizini'den (Azure AD) önce, şirket içi kimlik sistemlerine sahip kuruluşlar, şirket içi dizinde geleneksel olarak iş ortağı hesaplarını yönetmektedir. Böyle bir kuruluşta, uygulamaları Azure AD'ye taşımaya başladığınızda, iş ortaklarınızın gereksinim duydukları kaynaklara erişebildiklerinden emin olmak istersiniz. Kaynakların şirket içinde mi yoksa bulutta mı olduğu önemli olmamalıdır. Ayrıca, iş ortağı kullanıcılarınızın hem şirket içinde hem de Azure REKLAM kaynakları için aynı oturum açma kimlik bilgilerini kullanabilmesini istiyorsunuz. 

Şirket içi dizininizde harici iş ortaklarınızın hesaplarını oluşturursanız (örneğin, partners.contoso.com etki alanınızda Wendy Moran adlı harici bir kullanıcı için oturum açma adı taşıyan bir hesap oluşturursanız), artık bu hesapları Bulut. Özellikle, iş ortağı hesaplarını Bulut'ta Azure AD B2B kullanıcıları (diğer bir deyişle UserType = Guest olan kullanıcılar) olarak senkronize etmek için Azure AD Connect'i kullanabilirsiniz. Bu, iş ortağı kullanıcılarınızın bulut kaynaklarına yerel hesaplarıyla aynı kimlik bilgilerini kullanarak, ihtiyaç duyduklarından daha fazla erişim sağlamadan erişmelerini sağlar. 

## <a name="identify-unique-attributes-for-usertype"></a>UserType için benzersiz öznitelikleri belirleme

UserType özniteliğinin eşitlemesini etkinleştirmeden önce, öncelikle UserType özniteliğinin şirket içi Active Directory'den nasıl türetileceğine karar vermeniz gerekir. Başka bir deyişle, şirket içi ortamınızdaki parametreler harici işbirlikçilerinize özgütür? Bu dış ortak çalışanları kendi kuruluşunuzun üyelerinden ayıran bir parametre belirleyin.

Bunun için iki yaygın yaklaşım şunlardır:

- Kaynak özniteliği olarak kullanmak üzere kullanılmayan bir şirket içi Active Directory özniteliği (örneğin, uzantıAttribute1) belirleyin. 
- Alternatif olarak, UserType özniteliğinin değerini diğer özelliklerden türetin. Örneğin, şirket içi Active Directory UserPrincipalName özniteliği etki alanı * \@partners.contoso.com*ile biterse tüm kullanıcıları Konuk olarak senkronize etmek istiyorsunuz.
 
Ayrıntılı öznitelik gereksinimleri için [bkz.](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype) 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Kullanıcıları bulutla senkronize etmek için Azure AD Connect'i yapılandırma

Benzersiz özniteliği tanımladıktan sonra, Azure AD Connect'i bu kullanıcıları bulutla Azure AD B2B kullanıcıları (diğer bir deyişle UserType = Guest olan kullanıcılar) olarak eşitlemek için yapılandırabilirsiniz. Yetkilendirme açısından bakıldığında, bu kullanıcılar Azure AD B2B işbirliği daveti işlemi yle oluşturulan B2B kullanıcılarından ayırt edilemez.

Uygulama yönergeleri için [bkz.](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype)

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için Azure Active Directory B2B işbirliği](hybrid-organizations.md)
- [Azure AD'deki B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme](hybrid-cloud-to-on-premises.md)
- Azure AD Connect'e genel bir bakış için [bkz.](../hybrid/whatis-hybrid-identity.md)

