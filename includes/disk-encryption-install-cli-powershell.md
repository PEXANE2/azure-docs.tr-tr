---
title: include dosyası
description: include dosyası
services: virtual-machines
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 10/06/2019
ms.author: mbaldwin
ms.custom: include file
ms.openlocfilehash: 05794a046fdcb15a91145a75717a6a454d15a8da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72511532"
---
Azure Disk Şifrelemesi Azure [CLI](/cli/azure) ve [Azure PowerShell](/powershell/azure/new-azureps-module-az)aracılığıyla etkinleştirilebilir ve yönetilebilir. Bunu yapmak için araçları yerel olarak yüklemeniz ve Azure aboneliğinize bağlanmanız gerekir.

### <a name="azure-cli"></a>Azure CLI

[Azure CLI 2.0,](/cli/azure) Azure kaynaklarını yönetmek için bir komut satırı aracıdır. CLI, verileri esnek bir şekilde sorgulamak, engelleme olmayan işlemler olarak uzun süren işlemleri desteklemek ve komut dosyası yürütmeyi kolaylaştırmak için tasarlanmıştır. [Azure CLI'yi](/cli/azure/install-azure-cli?view=azure-cli-latest)yükleyin'deki adımları izleyerek yerel olarak yükleyebilirsiniz.

[Azure CLI ile Azure hesabınızda oturum açmak](/cli/azure/authenticate-azure-cli)için az [giriş](/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanın.

```azurecli
az login
```

Oturum açacak bir kiracı seçmek istiyorsanız şunları kullanın:
    
```azurecli
az login --tenant <tenant>
```

Birden çok aboneliğiniz varsa ve belirli bir abonelik belirtmek istiyorsanız, az hesap listesi yle abonelik [listenizi](/cli/azure/account#az-account-list) alın ve [az hesap kümesi](/cli/azure/account#az-account-set)yle belirtin.
     
```azurecli
az account list
az account set --subscription "<subscription name or ID>"
```

Daha fazla bilgi için Azure [CLI 2.0 ile başlayın' a](/cli/azure/get-started-with-azure-cli)bakın. 

### <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell az modülü,](/powershell/azure/new-azureps-module-az) Azure kaynaklarınızı yönetmek için [Azure Kaynak Yöneticisi](/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir cmdlets kümesi sağlar. [Azure Cloud Shell](/azure/cloud-shell/overview)ile tarayıcınızda kullanabilirsiniz veya Azure [PowerShell modüllerini yükleyin](/powershell/azure/install-az-ps)yönergelerini kullanarak yerel makinenize yükleyebilirsiniz. 

Yerel olarak zaten yüklediyseniz, Azure Disk Şifrelemesini yapılandırmak için Azure PowerShell SDK sürümünün en son sürümünü kullandığınızdan emin olun. [Azure PowerShell sürümünün](https://github.com/Azure/azure-powershell/releases)en son sürümünü indirin.

[Azure PowerShell ile Azure hesabınızda oturum açabilmek](/powershell/azure/authenticate-azureps?view=azps-2.5.0)için [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) cmdlet'ini kullanın.

```powershell
Connect-AzAccount
```

Birden çok aboneliğiniz varsa ve bir tane sini belirtmek istiyorsanız, bunları listelemek için [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) cmdlet'ini kullanın ve ardından [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) cmdlet'i ekleyin:

```powershell
Set-AzContext -Subscription -Subscription <SubscriptionId>
```

[Get-AzContext](/powershell/module/Az.Accounts/Get-AzContext) cmdlet'inçalıştırıldığı, doğru aboneliğin seçildiğini doğrular.

Azure Disk Şifreleme cmdletlerinin yüklü olduğunu doğrulamak için [Get-command](/powershell/module/microsoft.powershell.core/get-command?view=powershell-6) cmdlet'ini kullanın:
     
```powershell
Get-command *diskencryption*
```
Daha fazla bilgi için Azure [PowerShell'e başlarken](/powershell/azure/get-started-azureps)bakın. 