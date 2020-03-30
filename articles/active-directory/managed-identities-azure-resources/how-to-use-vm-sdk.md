---
title: Azure SDK'ları olan bir Azure VM'de yönetilen kimlikleri kullanma - Azure AD
description: Azure kaynaklarının kimliklerini yöneten bir Azure VM'si ile Azure SDK'larını kullanmak için kod örnekleri.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 422b4c63a86904721fc6910193ffa63a8fe46f0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74184162"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Azure SDK'lı bir Azure VM'de Azure kaynakları için yönetilen kimlikler nasıl kullanılır? 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, ilgili Azure SDK'nın Azure kaynakları için yönetilen kimlikler için verdiği desteğin kullanımını gösteren SDK örneklerinin bir listesi sağlanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Bu makaledeki tüm örnek kod/komut dosyası, istemcinin Azure kaynakları için yönetilen kimliklerle yönetilen bir VM üzerinde çalıştığını varsayar. VM'inize uzaktan bağlanmak için Azure portalındaki VM "Bağlan" özelliğini kullanın. VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme yle ilgili ayrıntılar için, Azure portalını veya varyant makalelerinden birini (PowerShell, CLI, şablon veya Azure SDK kullanarak) [kullanarak VM'deki Azure kaynakları için yönetilen kimlikleri yapılandır'](qs-configure-portal-windows-vm.md)a bakın. 

## <a name="sdk-code-samples"></a>SDK kod örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| .NET            | [Azure kaynakları için yönetilen kimlikleri kullanarak Windows VM'den bir Azure Kaynak Yöneticisi şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure kaynakları için yönetilen kimlikleri kullanarak Bir Linux VM'den Azure hizmetlerini arayın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Azure kaynakları için yönetilen kimlikleri kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Azure kaynaklarının yalnızca bir VM'nin içinden kimlik doğrulaması için yönetilen kimlikleri kullanma](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Azure kaynakları için yönetilen kimliklere sahip bir VM'den kaynakları yönetme](https://github.com/Azure-Samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Sonraki adımlar

- Kitaplık [indirmeleri,](https://azure.microsoft.com/downloads/) dokümantasyon ve daha fazlası dahil olmak üzere Azure SDK kaynaklarının tam listesi için Azure SDK'larına bakın.
- Azure VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirmek [için, Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](qs-configure-portal-windows-vm.md)bakın.








