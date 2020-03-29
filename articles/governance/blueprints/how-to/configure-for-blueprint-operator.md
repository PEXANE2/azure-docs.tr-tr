---
title: Blueprint Operator için ortamınızı düzene sin
description: Blueprint Operator yerleşik rol tabanlı erişim denetimi (RBAC) rolüyle kullanılmak üzere Azure ortamınızı nasıl yapılandıracaklarınızı öğrenin.
ms.date: 08/26/2019
ms.topic: how-to
ms.openlocfilehash: fba0dd3f2eeb69f768800d1d04640510462d3c86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873225"
---
# <a name="configure-your-environment-for-a-blueprint-operator"></a>Ortamınızı Blueprint İşleci için yapılandırma

Plan tanımlarınızın ve plan atamalarınızın yönetimi farklı takımlara atanabilir. Bir operasyon ekibi merkezi olarak denetlenir plan tanımlarının atamalarını yönetmekten sorumluyken, bir mimar veya yönetim ekibinin plan tanımlarınızın yaşam döngüsü yönetiminden sorumlu olması yaygındır.

**Blueprint Operatörü** dahili rol tabanlı erişim denetimi (RBAC), bu tür senaryolarda kullanılmak üzere özel olarak tasarlanmıştır. Rol, operasyon türü ekiplerinin kuruluşların plan tanımlarının atanmasını yönetmesine olanak sağlar, ancak bunları değiştirme yeteneğini değil. Bunu yapmak için Azure ortamınızda bazı yapılandırmalar gerekir ve bu makalede gerekli adımlar açıklanmaktadır.

## <a name="grant-permission-to-the-blueprint-operator"></a>Blueprint Operatörüne izin verme

İlk adım, blueprintat atayacak olan hesap veya güvenlik grubuna (önerilen) **Blueprint Operatörü** rolünü vermektir. Bu eylem, yönetim grubu hiyerarşisinde, tüm yönetim gruplarını ve operasyon ekibinin plan atamaerişimine sahip olması gereken abonelikleri kapsayan en üst düzeyde yapılmalıdır. Bu izinleri verirken en az ayrıcalık ilkesine uyman önerilir.

1. (Tavsiye edilir) [Güvenlik grubu oluşturma ve üye ekleme](../../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

1. Hesap veya güvenlik grubuna **Blueprint Operatörü'nün** [rol atamasını ekleme](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment)

## <a name="user-assign-managed-identity"></a>Kullanıcı atama yönetilen kimlik

Plan tanımı, sistem tarafından atanan veya kullanıcı tarafından atanan yönetilen kimlikleri kullanabilir. Ancak, Blueprint **Operator** rolünü kullanırken, plan tanımının kullanıcı tarafından atanmış yönetilen bir kimlik kullanacak şekilde yapılandırılması gerekir. Ayrıca, **Blueprint Operatörü** rolü verilen hesap veya güvenlik grubuna, kullanıcı tarafından atanan yönetilen kimlikte Yönetilen **Kimlik Operatörü** rolünün verilmesi gerekir. Bu izin olmadan, plan atamaları izin eksikliği nedeniyle başarısız.

1. Atanmış bir plan tarafından kullanılmak üzere [kullanıcı tarafından atanmış yönetilen](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity) bir kimlik oluşturma

1. **Yönetilen Kimlik İşlemi'nin** [rol atamasını](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment) hesaba veya güvenlik grubuna ekleyin. Rol atamasını yeni kullanıcı tarafından atanan yönetilen kimliğe göre dürtün.

1. Blueprint **Operatörü**olarak, yeni kullanıcı tarafından atanan yönetilen kimliği kullanan [bir plan atayın.](../create-blueprint-portal.md#assign-a-blueprint)

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.