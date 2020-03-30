---
title: Medya Hizmetleri hesapları için rol tabanlı erişim denetimi - Azure | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri hesapları için rol tabanlı erişim denetimi (RBAC) açıklanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66236912"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Medya Hizmetleri hesapları için rol tabanlı erişim denetimi (RBAC)

Şu anda Azure Medya Hizmetleri, hizmete özgü özel roller tanımlamamaktadır. Medya Hizmetleri hesabına tam erişim elde etmek için müşteriler **Sahibi** veya **Katılımcı'nın**yerleşik rollerini kullanabilir. Bu roller arasındaki temel fark şudur: **Sahibi** bir kaynağa kimlerin erişebileceğini ve **Katılımcının** erişemeyeceğini denetleyebilir. Yerleşik **Reader** rolü de kullanılabilir, ancak kullanıcı veya uygulama yalnızca Medya Hizmetleri API'lerine okuma erişimine sahip olur. 

## <a name="design-principles"></a>Tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API'ler **Get** or **List** işlemlerindeki sırları veya kimlik bilgilerini döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Kullanıcının sırları veya kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırması gerekir. **Reader** rolü Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets gibi işlemleri arayamaz. Ayrı eylemlere sahip olmak, istenirse özel bir rolde daha ayrıntılı RBAC güvenlik izinleri ayarlamanızı sağlar.

Medya Hizmetleri'nin desteklediği işlemleri listelemek için şunları yapın:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

[Yerleşik rol tanımları](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) makalesi, rolün tam olarak ne sağladığını söyler. 

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Klasik abonelik yönetici rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [Azure kaynakları için RBAC nedir?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Erişimi yönetmek için RBAC'ı kullanın](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Medya Hizmetleri kaynak sağlayıcı işlemleri](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftmedia)

## <a name="next-steps"></a>Sonraki adımlar

- [Medya Hizmetleri v3 API'leri ile geliştirme](media-services-apis-overview.md)
- [Medya Hizmetleri .NET'i kullanarak içerik anahtarı ilkesini elde edin](get-content-key-policy-dotnet-howto.md)
