---
title: Rol tabanlı erişim denetimini anlama - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'te rol tabanlı erişim denetimi ve izinleri yönetme hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: feda4b3a7f21b581fb4f08aec013f87c0fabb7e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76044942"
---
# <a name="role-based-access-control-in-azure-digital-twins"></a>Azure Digital Twins'te rol tabanlı erişim denetimi

Azure Digital Twins, uzamsal grafiğinizde belirli veriler, kaynaklar ve eylemler üzerinde hassas erişim denetimi sağlar. Bunu, [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) adı verilen parçalı rol ve izin yönetimi yoluyla yapar. RBAC _rolleri_ ve _rol atamaları_oluşur. Roller izin düzeyini tanımlar. Rol atamaları bir rolü bir kullanıcı veya aygıtla ilişkilendirer.

RBAC kullanılarak, izin verilebilir:

- Bir kullanıcı.
- Bir cihaz.
- Bir servis müdürü.
- Kullanıcı tanımlı bir işlev.
- Bir etki alanına ait tüm kullanıcılar.
- Bir kiracı.

Erişim derecesi de ince ayarlı olabilir.

RBAC, izinlerin uzamsal grafikten devralındığı benzersizdir.

## <a name="what-can-i-do-with-rbac"></a>RBAC ile ne yapabilirim?

Bir geliştirici RBAC'ı şu şekilde kullanabilir:

- Bir kullanıcıya tüm bina için veya yalnızca belirli bir oda veya kat için cihazları yönetme olanağı tanıyın.
- Yöneticiye tüm uzamsal grafik düğümlerine tüm grafik düğümlerine veya grafiğin yalnızca bir bölümüne genel erişim hakkı tanıyın.
- Erişim anahtarları dışında bir destek uzmanına grafiğe okuma erişimi hakkı tanıyın.
- Bir etki alanının her üyesine tüm grafik nesnelerine okuma erişimi hakkı tanıyın.

## <a name="rbac-best-practices"></a>RBAC en iyi uygulamaları

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roller

### <a name="role-definitions"></a>Rol tanımları

Rol tanımı, rol oluşturan izinler ve diğer özniteliklertopluluğudur. Rol tanımı, bu role sahip herhangi bir nesnenin gerçekleştirebileceği *CREATE,* *READ*, *UPDATE*ve *DELETE'i* içeren izin verilen işlemleri listeler. Ayrıca, izinlerin hangi nesne türlerine uygulandığını da belirtir.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

>[!NOTE]
> Önceki rollerin tam tanımlarını almak için sistem/roller API'sini sorgulayın.
> Rol atamaları [oluşturma ve yönetme](./security-create-manage-role-assignments.md#retrieve-all-roles)yi okuyarak daha fazla bilgi edinin.

### <a name="object-identifier-types"></a>Nesne tanımlayıcı türleri

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

>[!TIP]
> [Rol atamaları oluşturma ve yönetme](./security-create-manage-role-assignments.md#grant-permissions-to-your-service-principal)yi okuyarak hizmet yöneticinize nasıl izin verilecegini öğrenin.

Aşağıdaki başvuru belgeleri makaleleri açıklayınız:

- Nasıl [Sorgulamak veya bir kullanıcı için nesne kimliği](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0).
- Bir [hizmet sorumlusu için nesne kimliği](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal)nasıl alınır?
- Azure [AD kiracısının nesne kimliğini](../active-directory/develop/quickstart-create-new-tenant.md)alma.

## <a name="role-assignments"></a>Rol atamaları

Azure Digital Twins rol ataması, kullanıcı veya Azure AD kiracısı gibi bir nesneyi rol ve alanla ilişkilendirir. İzinler, o alana ait tüm nesnelere verilir. Uzay altındaki tüm uzamsal grafiği içerir.

Örneğin, bir kullanıcıya, bir binayı `DeviceInstaller` temsil eden uzamsal grafiğin kök düğümü rolünü içeren bir rol ataması verilir. Kullanıcı daha sonra bu düğüm ve binadaki diğer tüm alt boşluklar için cihazları okuyabilir ve güncelleyebilir.

Alıcıya izin vermek için bir rol ataması oluşturun. İzinleri iptal etmek için rol atamasını kaldırın.

>[!IMPORTANT]
> Rol atamaları oluşturma ve [yönetme](./security-create-manage-role-assignments.md)yi okuyarak rol atamaları hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins rol atamaları oluşturma ve yönetme hakkında daha fazla bilgi edinmek için [rol atamaları oluştur ve yönet' okuyun.](./security-create-manage-role-assignments.md)

- [Azure için RBAC](https://docs.microsoft.com/azure/role-based-access-control/)hakkında daha fazla bilgi edinin.
