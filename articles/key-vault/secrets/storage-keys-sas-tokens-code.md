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
ms.openlocfilehash: 0f81ffb5279e10c71f7d7cccfb6b738bc12e5cf4
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90086785"
---
# <a name="create-sas-definition-and-fetch-shared-access-signature-tokens-in-code"></a>SAS tanımı oluştur ve paylaşılan erişim imza belirteçlerini kodda getir

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

Azure depolama hizmetlerine erişmek için Key Vault SAS belirtecinden alınan kullanımı hakkında kılavuz için bkz. [bir hesap SAS kullanarak blob hizmetine erişim](https://docs.microsoft.com/azure/storage/common/storage-account-sas-create-dotnet#use-an-account-sas-from-a-client)

## <a name="next-steps"></a>Sonraki adımlar
- [SAS kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../../storage/common/storage-sas-overview.md)hakkında bilgi edinin.
- [Azure PowerShell](overview-storage-keys-powershell.md) [Key Vault ve Azure CLI veya Azure PowerShell ile depolama hesabı anahtarlarını yönetmeyi](overview-storage-keys.md) öğrenin.
- Bkz. [yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
