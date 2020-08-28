---
title: Azure SDK 'Ları ile Azure VM 'de Yönetilen kimlikler kullanma-Azure AD
description: Azure kaynakları için Yönetilen kimlikler içeren bir Azure VM ile Azure SDK 'Ları kullanmak için kod örnekleri.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecfb2fa5f45a23d387741d4865aa9707df960e86
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89018425"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Azure SDK 'Ları ile Azure VM 'de Azure kaynakları için Yönetilen kimlikler kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makale, ilgili Azure SDK 'sının Azure kaynakları için Yönetilen kimlikler desteğinin kullanımını gösteren SDK örneklerinin bir listesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Bu makaledeki tüm örnek kod/betik, istemcinin Azure kaynakları için yönetilen kimliklere sahip bir VM 'de çalıştığını varsayar. Sanal makinenize uzaktan bağlanmak için Azure portal VM "Connect" özelliğini kullanın. Bir VM 'de Azure kaynakları için yönetilen kimlikleri etkinleştirme hakkında daha fazla bilgi için bkz. Azure portal veya varyant makalelerinden birini [kullanarak BIR VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vm.md)(POWERSHELL, CLI, bir şablon veya BIR Azure SDK kullanarak). 

## <a name="sdk-code-samples"></a>SDK kodu örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| .NET            | [Azure kaynakları için Yönetilen kimlikler kullanarak Windows VM 'den bir Azure Resource Manager şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure kaynakları için Yönetilen kimlikler kullanarak bir Linux VM 'den Azure hizmetlerini çağırma](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Azure kaynakları için yönetilen kimlikleri kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Yalnızca bir VM 'nin içinden kimlik doğrulaması yapmak için Azure kaynakları için Yönetilen kimlikler kullanın](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Azure kaynakları için yönetilen kimliklere sahip bir VM 'den kaynak yönetme etkin](https://github.com/Azure-Samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Sonraki adımlar

- Kitaplık indirmeleri, belgeler ve daha fazlası dahil olmak üzere Azure SDK kaynaklarının tam listesi için bkz. [Azure SDK 'ları](https://azure.microsoft.com/downloads/) .
- Azure VM 'de Azure kaynakları için yönetilen kimlikleri etkinleştirmek üzere, [Azure Portal kullanarak BIR VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vm.md)konusuna bakın.








