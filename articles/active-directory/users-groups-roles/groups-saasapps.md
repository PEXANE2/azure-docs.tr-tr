---
title: SaaS uygulamalarına erişimi yönetmek için bir grup kullanma-Azure AD | Microsoft Docs
description: Azure Active Directory ile tümleştirilmiş SaaS uygulamalarına erişim atamak için Azure Active Directory içindeki grupları kullanma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d026cd94ba5e51b64a136359ffc76fb59efef2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84727917"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>SaaS uygulamalarına erişimi yönetmek için grup kullanma

Azure AD Premium lisans planıyla Azure Active Directory (Azure AD) kullanarak, Azure AD ile tümleştirilmiş bir SaaS uygulamasına erişim atamak için grupları kullanabilirsiniz. Örneğin, pazarlama departmanı için beş farklı SaaS uygulaması kullanmak üzere erişim atamak istiyorsanız, pazarlama departmanındaki kullanıcıları içeren bir grup oluşturabilir ve ardından bu grubu pazarlama departmanı için gereken beş SaaS uygulamasına atayabilirsiniz. Bu şekilde, pazarlama bölümünün üyeliğini tek bir yerde yöneterek zamandan tasarruf edebilirsiniz. Kullanıcılar, pazarlama grubunun üyesi olarak eklendiklerinde uygulamaya atanır ve bunların atamaları pazarlama grubundan kaldırıldığında uygulamadan kaldırılır. Bu özellik, Azure AD Uygulama Galerisi içinden ekleyebileceğiniz yüzlerce uygulama ile kullanılabilir.

> [!IMPORTANT]
> Bu özelliği, yalnızca bir Azure AD Premium deneme sürümünü başlattıktan veya Azure AD Premium lisans planı satın aldıktan sonra kullanabilirsiniz.
> Grup tabanlı atama yalnızca güvenlik grupları için desteklenir.
> Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmiyor.

## <a name="to-assign-access-for-a-user-or-group-to-a-saas-application"></a>SaaS uygulamasına bir kullanıcı veya grup için erişim atama

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) **Kurumsal uygulamalar**' ı seçin.
2. Uygulama galerisinden eklediğiniz bir uygulamayı açmak için seçin.
3. **Kullanıcılar ve gruplar**' ı seçin ve ardından **Kullanıcı Ekle**' yi seçin.
4. **Atama Ekle**sayfasında kullanıcılar **ve** gruplar ' ı seçerek kullanıcılar ve **gruplar** seçim listesini açın.
6. İstediğiniz sayıda grup veya Kullanıcı seçin, sonra **atama Ekle** listesine eklemek için **Seç** ' e tıklayın veya dokunun. Ayrıca, bu aşamada bir kullanıcıya bir rol atayabilirsiniz.
7. Kullanıcıları veya grupları seçili kurumsal uygulamaya atamak için **ata** ' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
