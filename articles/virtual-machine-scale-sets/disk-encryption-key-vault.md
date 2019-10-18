---
title: Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma
description: Bu makalede, Azure disk şifrelemesi ile kullanmak üzere bir Anahtar Kasası oluşturma ve yapılandırma adımları sağlanır
ms.service: virtual-machine-scale-sets
ms.topic: article
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: e4f6dc068969efd3f66e6808531594ed4063347f
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72530881"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption"></a>Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma

Azure disk şifrelemesi, disk şifreleme anahtarlarını ve gizli dizileri denetlemek ve yönetmek için Azure Key Vault kullanır.  Anahtar kasaları hakkında daha fazla bilgi için bkz. [Azure Key Vault kullanmaya başlama](../key-vault/key-vault-get-started.md) ve [anahtar kasanızın güvenliğini sağlama](../key-vault/key-vault-secure-your-key-vault.md).

Azure disk şifrelemesi ile kullanım için bir Anahtar Kasası oluşturmak ve yapılandırmak üç adımdan oluşur:

1. Gerekirse bir kaynak grubu oluşturma.
2. Anahtar Kasası oluşturma. 
3. Anahtar Kasası Gelişmiş erişim ilkeleri ayarlanıyor.

Bu adımlar aşağıdaki hızlı başlangıçlarda gösterilmiştir:

Ayrıca, anahtar şifreleme anahtarı (KEK) oluşturabilir veya içeri aktarabilirsiniz.

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyip Azure 'a bağlanın

Bu makaledeki adımlar [Azure CLI](/cli/azure/), [Azure PowerShell Az Module](/powershell/azure/overview)veya [Azure Portal](https://portal.azure.com)ile tamamlanabilir.

Portal, tarayıcınız aracılığıyla erişilebilir olsa da, Azure CLı ve Azure PowerShell yerel yükleme gerektirir.

### <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

Azure CLı veya Azure PowerShell kullanmadan önce, önce Azure aboneliğinize bağlanmanız gerekir. [Azure CLI Ile oturum](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)açarak, [Azure PowerShell ile oturum](/powershell/azure/authenticate-azureps?view=azps-2.5.0)açarak veya istendiğinde kimlik bilgilerinizi Azure Portal sağlayarak bunu yapabilirsiniz.

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