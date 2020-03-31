---
title: SaaS uygulamalarına erişimi yönetmek için bir grup kullanın - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory ile tümleşik SaaS uygulamalarına erişim atamak için Azure Active Directory'deki grupları kullanma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51375f057543c86fe021822eb9722ffd1be16804
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74026847"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>SaaS uygulamalarına erişimi yönetmek için grup kullanma

Azure AD Premium lisans planıyla Azure Active Directory'yi (Azure AD) kullanarak, grupları Azure AD ile entegre edilmiş bir SaaS uygulamasına erişim atamak için kullanabilirsiniz. Örneğin, pazarlama departmanına beş farklı SaaS uygulamasını kullanmak üzere erişim atamak istiyorsanız, pazarlama departmanındaki kullanıcıları içeren bir grup oluşturabilir ve bu grubu bu beş SaaS uygulamasına pazarlama departmanı. Bu şekilde pazarlama departmanı nın üyeliğini tek bir yerde yöneterek zamandan tasarruf edebilirsiniz. Kullanıcılar daha sonra pazarlama grubunun üyesi olarak eklendiklerinde uygulamaya atanır ve pazarlama grubundan çıkarıldıklarında atamaları uygulamadan kaldırılır. Bu özellik, Azure AD Uygulama Galerisi'nden ekleyebileceğiniz yüzlerce uygulamayla kullanılabilir.

> [!IMPORTANT]
> Bu özelliği yalnızca Azure AD Premium deneme sürümünü başlattıktan veya Azure AD Premium lisans planını satın aldıktan sonra kullanabilirsiniz.
> Grup tabanlı atama yalnızca güvenlik grupları için desteklenir.
> Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmiyor.

## <a name="to-assign-access-for-a-user-or-group-to-a-saas-application"></a>Bir Kullanıcı veya grup için bir SaaS uygulamasına erişim atamak için

1. Azure [AD yönetici merkezinde](https://aad.portal.azure.com) **Kurumsal uygulamaları**seçin.
2. Açmak için Uygulama Galerisi'nden eklediğiniz bir uygulamayı seçin.
3. **Kullanıcıları ve grupları**seçin ve ardından kullanıcı **ekle'yi**seçin.
4. **Atama**Ekle'de, Kullanıcılar ve **gruplar** seçim listesini açmak için **Kullanıcıları ve grupları** seçin.
6. İstediğiniz kadar grup veya kullanıcı seçin, ardından **Atama Ekle** listesine eklemek için **Seç'e** tıklayın veya dokunun. Bu aşamada bir kullanıcıya bir rol de atayabilirsiniz.
7. Kullanıcıları veya grupları seçili kurumsal uygulamaya atamak için **Atama'yı** seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Etkin Dizin nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
