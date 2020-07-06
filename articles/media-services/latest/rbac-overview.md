---
title: Media Services hesapları için rol tabanlı erişim denetimi-Azure | Microsoft Docs
description: Bu makalede Azure Media Services hesapları için rol tabanlı erişim denetimi (RBAC) açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/23/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 93b2cd3a2565b14ea07d6db6b14dd146e4223528
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "66236912"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Media Services hesapları için rol tabanlı erişim denetimi (RBAC)

Şu anda Azure Media Services, hizmete özgü özel bir rol tanımlamaz. Müşteriler Media Services hesabına tam erişim sağlamak için, **sahip** veya **katkıda**bulunan yerleşik rollerini kullanabilir. Bu roller arasındaki temel fark şudur: **sahip** , bir kaynağa kimlerin erişebileceğini denetleyebilir ve **katkıda** bulunan başarısız olur. Yerleşik **okuyucu** rolü de kullanılabilir, ancak kullanıcı veya uygulamanın yalnızca Media Services API 'lerine okuma erişimi olur. 

## <a name="design-principles"></a>Tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API 'Leri, **Get** veya **list** işlemlerinde gizli dizileri veya kimlik bilgilerini döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Kullanıcıların gizli dizileri veya kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırması gerekir. **Okuyucu** rolü, varlık. ListContainerSas, StreamingLocator. ListContentKeys, ContentKeyPolicies. Getpolicypropertieswithgizlilikler gibi işlemleri çağıramaz. Ayrı eylemlere sahip olmak isterseniz özel bir rolde daha ayrıntılı RBAC güvenlik izinleri ayarlamanıza olanak sağlar.

Media Services işlemlerini desteklemek için şunları yapın:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

[Yerleşik rol tanımları](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) makalesi, rolün ne olduğunu tam olarak söyler. 

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Klasik abonelik yönetici rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [Azure kaynakları için RBAC nedir?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Media Services kaynak sağlayıcısı işlemleri](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftmedia)

## <a name="next-steps"></a>Sonraki adımlar

- [Media Services v3 API 'Leri ile geliştirme](media-services-apis-overview.md)
- [Media Services .NET kullanarak içerik anahtarı ilkesi al](get-content-key-policy-dotnet-howto.md)
