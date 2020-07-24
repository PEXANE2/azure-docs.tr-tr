---
title: Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma
description: Bu makalede, Azure disk şifrelemesi ile kullanmak üzere bir Anahtar Kasası oluşturma ve yapılandırma adımları sağlanır
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: acd2ae54d81fb508d5f8c02262cf8c2f0f071fb5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080616"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Azure disk şifrelemesi için Anahtar Kasası oluşturma ve yapılandırma

Azure disk şifrelemesi, disk şifreleme anahtarlarını ve gizli dizileri denetlemek ve yönetmek için Azure Key Vault kullanır.  Anahtar kasaları hakkında daha fazla bilgi için bkz. [Azure Key Vault kullanmaya başlama](../key-vault/general/overview.md) ve [anahtar kasanızın güvenliğini sağlama](../key-vault/general/secure-your-key-vault.md).

Azure disk şifrelemesi ile kullanım için bir Anahtar Kasası oluşturmak ve yapılandırmak üç adımdan oluşur:

1. Gerekirse bir kaynak grubu oluşturma.
2. Anahtar Kasası oluşturma. 
3. Anahtar Kasası Gelişmiş erişim ilkeleri ayarlanıyor.

Bu adımlar aşağıdaki hızlı başlangıçlarda gösterilmiştir:

Ayrıca, anahtar şifreleme anahtarı (KEK) oluşturabilir veya içeri aktarabilirsiniz.

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyip Azure 'a bağlanın

Bu makaledeki adımlar [Azure CLI](/cli/azure/), [Azure PowerShell Az Module](/powershell/azure/)veya [Azure Portal](https://portal.azure.com)ile tamamlanabilir.

### <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

Azure CLı veya Azure PowerShell kullanmadan önce, önce Azure aboneliğinize bağlanmanız gerekir. Bunun için [Azure CLI Ile oturum](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)açıp [Azure PowerShell ile oturum](/powershell/azure/authenticate-azureps?view=azps-2.5.0)açarak veya istendiğinde kimlik bilgilerinizi Azure Portal sağlayabilirsiniz.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)
- [Azure CLı kullanarak sanal makine ölçek kümelerini şifreleme](disk-encryption-cli.md)
- [Azure PowerShell kullanarak bir sanal makine ölçek kümelerini şifreleme](disk-encryption-powershell.md)
