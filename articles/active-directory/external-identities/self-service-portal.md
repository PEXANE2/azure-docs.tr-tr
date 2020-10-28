---
title: B2B işbirliği için self servis kaydolma portalı-Azure AD
description: Azure Active Directory B2B kullanıcılarına kuruluşunuzun ihtiyaçlarına uyacak şekilde ekleme iş akışını özelleştirmeyi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f9cd421224ad134b4df853b06b3bc34333a0613
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92896115"
---
# <a name="self-service-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kaydı için self servis

Müşteriler, son kullanıcılar için [Azure portalı](https://portal.azure.com) ve [Uygulama Erişim Paneli](https://myapps.microsoft.com) aracılığıyla ortaya çıkan yerleşik özelliklerle birçok işlem yapabilir. Ancak B2B kullanıcıları için ekleme iş akışını, kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirmeniz gerekebilir.

## <a name="azure-ad-entitlement-management-for-b2b-guest-user-sign-up"></a>B2B Konuk Kullanıcı kaydı için Azure AD Yetkilendirme Yönetimi

Davet edilen bir kuruluş olarak, tek tek dış çalışanların kaynaklarınıza erişmesi gereken sürenin önünde haberdar olmayabilirsiniz. İş ortağı şirketlerinden kullanıcıların denetlediğiniz ilkelerle kaydolmalarına yönelik bir yönteme ihtiyacınız vardır. Diğer kuruluşlardan kullanıcıların erişim istemesine ve Grup, uygulamalar ve SharePoint Online sitelerine atanmasını sağlamak istiyorsanız, [dış kullanıcılara erişimi yöneten](../governance/entitlement-management-external-users.md#how-access-works-for-external-users)ilkeleri yapılandırmak IÇIN [Azure AD Yetkilendirme Yönetimi](../governance/entitlement-management-overview.md) ' ni kullanabilirsiniz.

## <a name="azure-active-directory-b2b-invitation-api"></a>Azure Active Directory B2B davet API 'SI

Kuruluşlar, B2B Konuk kullanıcıları için kendi ekleme deneyimlerini derlemek üzere [Microsoft Graph davet YÖNETICISI API](/graph/api/resources/invitation?view=graph-rest-1.0) 'sini kullanabilir. Self Servis B2B Konuk Kullanıcı kaydı sunmak istediğinizde [Azure AD yetkilendirme yönetimini](../governance/entitlement-management-overview.md)kullanmanızı öneririz. Ancak kendi deneyiminizi derlemek isterseniz, özelleştirilmiş davet e-postanızı doğrudan B2B kullanıcısına otomatik olarak göndermek için [davet oluşturma API](/graph/api/invitation-post?tabs=http&view=graph-rest-1.0) 'sini kullanabilirsiniz. Ya da uygulamanız, davet edilen kullanıcıya kendi davetinizi (iletişim mekanizmanız aracılığıyla) oluşturmak için oluşturma yanıtında döndürülen davet edilen Teredeemurl 'Yi kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Dış Kimlikler fiyatlandırması](external-identities-pricing.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)