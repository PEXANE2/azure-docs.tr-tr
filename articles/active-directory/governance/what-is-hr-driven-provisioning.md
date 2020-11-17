---
title: Azure Active Directory ile HR odaklı sağlama nedir? | Microsoft Belgeleri
description: HR odaklı sağlama konusuna genel bakış açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 10/30/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eaf033f7da7a5bd4882d0c78a2321640aa64d8ac
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94646568"
---
# <a name="what-is-hr-driven-provisioning"></a>HR odaklı sağlama nedir?

![HR sağlama](./media/what-is-hr-driven-provisioning/cloud2a.png)

HR odaklı sağlama, bir insan kaynakları sistemine göre Dijital kimlikler oluşturma işlemidir.  IK sistemleri, bu yeni oluşturulan dijital kimlikler için yetki başlangıcı olur ve genellikle çok sayıda sağlama işlemi için başlangıç noktasıdır.  Örneğin, şirketinize yeni bir çalışan katılırsa, bunlar insan kaynakları sisteminde oluşturulur.  Oluşturma, bir kullanıcı hesabının Active Directory ' ye sağlamasını tetikler ve bu hesabı Azure AD ' a, vb. için sağlar Azure AD Connect.

HR odaklı sağlama, şirket içi veya bulut tabanlı olabilir.

## <a name="on-premises-based-hr-provisioning"></a>Şirket içi tabanlı HR sağlama
Şirket içi tabanlı HR sağlama, yerel bir ık sistem kullanılarak ve yeni dijital kimlikler sağlamanın bir yolu ile gerçekleştirilir.

IK sistemleri çeşitli paketlerde, yazılım paketlerinde gelir ve SQL Server 'lar, LDAP dizinleri vb. kullanabilir.

Şu anda, Microsoft şirket içi HR sağlama çözümleri, bu HR sistemlerinde yeni bir kimlik oluşturulduğunda sağlamayı tetiklemek için Microsoft Identity Manager kullanır.

MıM kullanarak, şirket içi HR sistemlerinizden Active Directory veya Azure AD 'ye Kullanıcı sağlayabilirsiniz.

Microsoft Identity Manager ve desteklediği sistemler hakkında bilgi için [Microsoft Identity Manager](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) belgelerine bakın.

[!INCLUDE [active-directory-hr-provisioning.md](../../../includes/active-directory-hr-provisioning.md)]



## <a name="next-steps"></a>Sonraki adımlar 
- [Kimlik yaşam döngüsü yönetimi nedir?](what-is-identity-lifecycle-management.md)
- [Sağlama nedir?](what-is-provisioning.md)
- [Uygulama sağlama nedir?](what-is-app-provisioning.md)
- [Dizinler arası sağlama nedir?](what-is-inter-directory-provisioning.md)
