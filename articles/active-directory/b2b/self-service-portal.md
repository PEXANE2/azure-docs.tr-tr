---
title: B2B işbirliği için self servis kayıt portalı - Azure AD
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: sample
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: cfa67f529408efcc2a703a4f80b15143c774f0b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77195802"
---
# <a name="self-service-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği için self servis kayıt

Müşteriler, son kullanıcılar için [Azure portalı](https://portal.azure.com) ve [Uygulama Erişim Paneli](https://myapps.microsoft.com) aracılığıyla ortaya çıkan yerleşik özelliklerle birçok işlem yapabilir. Ancak B2B kullanıcıları için ekleme iş akışını, kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirmeniz gerekebilir.

## <a name="azure-ad-entitlement-management-for-b2b-guest-user-sign-up"></a>B2B konuk kullanıcı kaydı için Azure AD yetkilendirme yönetimi

Davetkar bir kuruluş olarak, kaynaklarınıza erişmesi gereken tek tek dış işbirlikçilerin kim olduğunu önceden bilmiyor olabilirsiniz. Ortak şirketlerden kullanıcıların kontrol ettiğiniz ilkelerle kendilerini kaydolmaları için bir yol gerekir. Diğer kuruluşlardan kullanıcıların erişim talebinde bulunmalarını sağlamak istiyorsanız ve onay aldıktan sonra konuk hesaplarıyla birlikte sağlanabilir ve gruplara, uygulamalara ve SharePoint Online sitelerine atanabilirseniz, [dış kullanıcıların erişimini yöneten](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users#how-access-works-for-external-users)ilkeleri yapılandırmak için Azure AD yetkilendirme [yönetimini](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) kullanabilirsiniz.

## <a name="azure-active-directory-b2b-invitation-api"></a>Azure Active Directory B2B davet API

Kuruluşlar, B2B konuk kullanıcıları için kendi onboarding deneyimlerini oluşturmak için [Microsoft Graph davet yöneticisi API'yi](https://docs.microsoft.com/graph/api/resources/invitation?view=graph-rest-1.0) kullanabilir. Self servis B2B konuk kullanıcı kaydı sunmak istediğinizde, Azure [AD yetkilendirme yönetimini](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview)kullanmanızı öneririz. Ancak kendi deneyiminizi oluşturmak istiyorsanız, özelleştirilmiş davet e-postanızı doğrudan B2B kullanıcısına doğrudan göndermek için [oluşturma daveti API'sini](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-1.0&tabs=http) kullanabilirsiniz. Ya da uygulamanız, kendi davetinizi (seçtiğiniz iletişim mekanizması aracılığıyla) davet edilen kullanıcıya yapmak için oluşturma yanıtında döndürülen geri dönen RedeemUrl'yi kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
