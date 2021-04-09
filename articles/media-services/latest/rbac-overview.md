---
title: Media Services hesapları için rol tabanlı erişim denetimi
description: Bu makalede, Azure Media Services hesapları için Azure rol tabanlı erişim denetimi (Azure RBAC) açıklanmaktadır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 7c9edce71e0d0565de80153bfd5b3d5f1edb2e8f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98955165"
---
# <a name="azure-role-based-access-control-azure-rbac-for-media-services-accounts"></a>Media Services hesapları için Azure rol tabanlı erişim denetimi (Azure RBAC)

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Şu anda Azure Media Services, hizmete özgü özel bir rol tanımlamaz. Müşteriler Media Services hesabına tam erişim sağlamak için, **sahip** veya **katkıda** bulunan yerleşik rollerini kullanabilir. Bu roller arasındaki temel fark şudur: **sahip** , bir kaynağa kimlerin erişebileceğini denetleyebilir ve **katkıda** bulunan başarısız olur. Yerleşik **okuyucu** rolü de kullanılabilir, ancak kullanıcı veya uygulamanın yalnızca Media Services API 'lerine okuma erişimi olur. 

## <a name="design-principles"></a>Tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API 'Leri, **Get** veya **list** işlemlerinde gizli dizileri veya kimlik bilgilerini döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Kullanıcıların gizli dizileri veya kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırması gerekir. **Okuyucu** rolü, varlık. ListContainerSas, StreamingLocator. ListContentKeys, ContentKeyPolicies. Getpolicypropertieswithgizlilikler gibi işlemleri çağıramaz. Ayrı eylemlere sahip olmak isterseniz özel bir rolde daha ayrıntılı Azure RBAC güvenlik izinleri ayarlamanıza olanak sağlar.

Media Services işlemlerini desteklemek için şunları yapın:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

[Yerleşik rol tanımları](../../role-based-access-control/built-in-roles.md) makalesi, rolün ne olduğunu tam olarak söyler. 

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md)
- [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md)
- [REST API kullanarak Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-rest.md)
- [Media Services kaynak sağlayıcısı işlemleri](../../role-based-access-control/resource-provider-operations.md#microsoftmedia)

## <a name="next-steps"></a>Sonraki adımlar

- [Media Services v3 API 'Leri ile geliştirme](media-services-apis-overview.md)
- [Media Services .NET kullanarak içerik anahtarı ilkesi al](get-content-key-policy-dotnet-howto.md)
