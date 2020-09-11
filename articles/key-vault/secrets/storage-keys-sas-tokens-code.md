---
title: Kodda paylaşılan erişim imza belirteçlerini getirme | Azure Key Vault
description: Yönetilen depolama hesabı özelliği, Azure Key Vault ile Azure depolama hesabı arasında sorunsuz bir tümleştirme sağlar. Bu örnek, SAS belirteçlerini yönetmek için .NET için Azure SDK 'sını kullanır.
ms.topic: tutorial
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: afe7d5ce3dd1756ddb9e33fe402fb2eb699ce8f7
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007426"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Kodda paylaşılan erişim imza belirteçlerini getirme

Depolama hesabınızı, Anahtar Kasanızda depolanan paylaşılan erişim imzası (SAS) belirteçleriyle yönetebilirsiniz. Daha fazla bilgi için bkz. [SAS kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../../storage/common/storage-sas-overview.md).

> [!NOTE]
> Paylaşılan anahtar yetkilendirmesi üzerinde üstün güvenlik ve kullanım kolaylığı sağlamak üzere depolama hesabınızı güvenli hale getirmek için [rol tabanlı Access Control (RBAC)](../../storage/common/storage-auth-aad.md) kullanmanızı öneririz.

Bu makalede bir SAS tanımı oluşturan ve SAS belirteçlerini getiren .NET kodu örnekleri sağlanmaktadır. Key Vault yönetilen depolama hesapları için oluşturulan istemci dahil olmak üzere, tüm ayrıntılar için bkz. [sharelink](https://docs.microsoft.com/samples/azure/azure-sdk-for-net/share-link/) örneğimize bakın. SAS belirteçlerini oluşturma ve depolama hakkında daha fazla bilgi için bkz. [Key Vault ve Azure CLI ile depolama hesabı anahtarlarını yönetme](overview-storage-keys.md) veya [Key Vault ve Azure PowerShell depolama hesabı anahtarlarını yönetme](overview-storage-keys-powershell.md).

## <a name="code-samples"></a>Kod örnekleri

Aşağıdaki örnekte SAS şablonu oluşturacağız:

:::code language="csharp" source="~/azure-sdk-for-net/sdk/keyvault/samples/sharelink/Program.cs" range="91-97":::

Bu şablonu kullanarak, şunu kullanarak bir SAS tanımı oluşturuyoruz 

:::code language="csharp" source="~/azure-sdk-for-net/sdk/keyvault/samples/sharelink/Program.cs" range="137-156":::

SAS tanımı oluşturulduktan sonra, kullanarak gizli dizileri gibi SAS belirteçlerini alabilirsiniz `SecretClient` . Gizli adı, depolama hesabı adı ile izleyen bir tire ile önyüklemeniz gerekir:

:::code language="csharp" source="~/azure-sdk-for-net/sdk/keyvault/samples/sharelink/Program.cs" range="52-58":::

Paylaşılan erişim imza belirtecinizin kullanım süreleri dolarsa, yeni bir tane oluşturmak için aynı parolayı yeniden getirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [SAS kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../../storage/common/storage-sas-overview.md)hakkında bilgi edinin.
- [Azure PowerShell](overview-storage-keys-powershell.md) [Key Vault ve Azure CLI veya Azure PowerShell ile depolama hesabı anahtarlarını yönetmeyi](overview-storage-keys.md) öğrenin.
- Bkz. tam parçalı [elink](https://docs.microsoft.com/samples/azure/azure-sdk-for-net/share-link/) örneği.
- Daha fazla [Key Vault örnek](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-key-vault)
- Bkz. [yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
