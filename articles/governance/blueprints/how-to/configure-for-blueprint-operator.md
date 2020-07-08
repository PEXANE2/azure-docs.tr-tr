---
title: Ortamınızı Blueprint Işleci için ayarlama
description: Azure ortamınızı Blueprint Işleci yerleşik rol tabanlı erişim denetimi (RBAC) rolüyle kullanılmak üzere nasıl yapılandıracağınızı öğrenin.
ms.date: 05/06/2020
ms.topic: how-to
ms.openlocfilehash: a61997f5e3247e5b42d5e2f07c0fc38cc1b5114d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82864020"
---
# <a name="configure-your-environment-for-a-blueprint-operator"></a>Ortamınızı Blueprint İşleci için yapılandırma

Şema tanımlarınızın ve şema atamalarının yönetimi farklı takımlara atanabilir. Bir işlem ekibinin, merkezi olarak denetlenen şema tanımlarının atamalarını yönetmekten sorumlu olması halinde, bir mimarın ve idare ekibinin, şema tanımlarınızın yaşam döngüsü yönetiminden sorumlu olması yaygındır.

Şema **operatörü** yerleşik rol tabanlı erişim denetımı (RBAC), bu tür bir senaryoda kullanılmak üzere özel olarak tasarlanmıştır. Rol, kuruluşların şema tanımlarının atanmasını yönetmesine, ancak bunları değiştirebilme yeteneğine izin vermez. Bunun yapılması, Azure ortamınızda bazı yapılandırmalar gerektirir ve bu makalede gerekli adımlar açıklanmaktadır.

## <a name="grant-permission-to-the-blueprint-operator"></a>Blueprint Işlecine izin verme

Birinci adım, şema atamak için şema **operatörü** rolünü hesaba veya güvenlik grubuna (önerilen) vermekle aynıdır. Bu eylem, yönetim grubu hiyerarşisinde, işlemler ekibinin şema atama erişiminin olması gereken tüm yönetim gruplarını ve abonelikleri kapsayan en yüksek düzeyde yapılmalıdır. Bu izinleri verirken en az ayrıcalık ilkesini izlemeniz önerilir.

1. Önerilen [Güvenlik grubu oluşturma ve üye ekleme](../../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

1. Hesap veya güvenlik grubuna bir **Blueprint işlecinin** [rol atamasını ekleyin](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment)

## <a name="user-assign-managed-identity"></a>Kullanıcı tarafından yönetilen kimliği atama

Şema tanımı, sistem tarafından atanan veya Kullanıcı tarafından atanan yönetilen kimlikleri kullanabilir. Ancak, **şema operatörü** rolünü kullanırken, şema tanımının Kullanıcı tarafından atanan yönetilen kimlik kullanacak şekilde yapılandırılması gerekir. Ayrıca, şema **operatörü** rolüne verilen hesap veya güvenlik grubuna kullanıcı tarafından atanan yönetilen kimlik Için **yönetilen kimlik operatörü** rolü verilmelidir. Bu izin olmadan, şema atamaları izin eksikliği nedeniyle başarısız olur.

1. Atanan bir şema tarafından kullanılmak üzere [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)

1. Hesap veya güvenlik grubuna **yönetilen kimlik işlecinin** [rol atamasını ekleyin](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment) . Rol atamasını Kullanıcı tarafından atanan yeni yönetilen kimliğe kapsam.

1. **Şema operatörü**olarak, Kullanıcı tarafından atanan yeni yönetilen kimliği kullanan [bir şema atayın](../create-blueprint-portal.md#assign-a-blueprint) .

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.