---
title: Yerel iş ortağı hesaplarını, B2B kullanıcıları olarak buluta eşitleyin-Azure AD
description: Azure AD B2B işbirliğiyle aynı kimlik bilgilerini kullanarak yerel ve bulut kaynaklarına yerel olarak yönetilen dış iş ortakları için erişim verin.
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
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74272608"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliğini kullanarak bulut kaynaklarına yerel olarak yönetilen iş ortağı hesapları erişimi verme

Azure Active Directory (Azure AD) öncesinde, şirket içi kimlik sistemlerine sahip kuruluşların şirket içi dizininde geleneksel olarak yönetilen iş ortağı hesapları vardır. Böyle bir kuruluşta, uygulamaları Azure AD 'ye taşımaya başladığınızda, iş ortaklarınızın ihtiyaç duydukları kaynaklara erişebildiklerinden emin olmak istersiniz. Kaynakların şirket içinde mi yoksa bulutta mı olduğuna bakılmaksızın. Ayrıca, iş ortağı kullanıcılarınızın hem şirket içi hem de Azure AD kaynakları için aynı oturum açma kimlik bilgilerini kullanmasını istiyorsunuz. 

Şirket içi dizininizde dış iş ortaklarınız için hesaplar oluşturursanız (örneğin, partners.contoso.com etki alanında gülşen Moran adında bir dış Kullanıcı için "wmoran" adlı bir oturum açma adı olan bir hesap oluşturursanız, bu hesapları şu şekilde eşitleyebilirsiniz una. Özellikle, iş ortağı hesaplarını Azure AD B2B kullanıcıları (yani UserType = konuğa sahip kullanıcılar) olarak buluta eşitlemek için Azure AD Connect kullanabilirsiniz. Bu, iş ortağı kullanıcılarınızın, yerel hesaplarıyla aynı kimlik bilgilerini kullanarak bulut kaynaklarına erişmesini sağlar ve bunlara gereksinimlerinden daha fazla erişim izni vermez. 

## <a name="identify-unique-attributes-for-usertype"></a>UserType için benzersiz öznitelikleri tanımla

UserType özniteliğinin eşitlemesini etkinleştirmeden önce, önce şirket içi Active Directory özniteliğini nasıl türetireceğinize karar vermelisiniz. Diğer bir deyişle, şirket içi ortamınızdaki hangi parametreler dış ortak çalışanlarla benzersizdir? Bu dış ortak çalışanları kendi kuruluşunuzun üyelerinden ayıran bir parametre belirleme.

Bunun için iki yaygın yaklaşım şunlardır:

- Kaynak öznitelik olarak kullanmak için kullanılmamış bir şirket içi Active Directory özniteliği (örneğin, extensionAttribute1) belirleyin. 
- Alternatif olarak, diğer özelliklerden UserType özniteliği için değeri türetebilirsiniz. Örneğin, şirket içi Active Directory UserPrincipalName özniteliği *\@* etki alanı ile bitiyorsa tüm kullanıcıları Konuk olarak eşitlemeniz gerekir.
 
Ayrıntılı öznitelik gereksinimleri için bkz. [UserType eşitlemesini etkinleştirme](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Kullanıcıları buluta eşitlemek için Azure AD Connect yapılandırma

Benzersiz özniteliğini tanımladıktan sonra, bu kullanıcıları Azure AD B2B kullanıcıları (yani UserType = konuğa sahip kullanıcılar) olarak buluta eşitlemek için Azure AD Connect yapılandırabilirsiniz. Bir yetkilendirme noktasından bu kullanıcılar, Azure AD B2B işbirliği daveti aracılığıyla oluşturulan B2B kullanıcılarından ayırt edilemez.

Uygulama yönergeleri için bkz. [UserType eşitlemesini etkinleştirme](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için B2B işbirliği Azure Active Directory](hybrid-organizations.md)
- [Şirket içi uygulamalarınıza Azure AD erişimi için B2B kullanıcıları verme](hybrid-cloud-to-on-premises.md)
- Azure AD Connect genel bir bakış için bkz. Şirket [içi dizinlerinizi Azure Active Directory Ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

