---
title: Linux sanal makineleri için Azure Key Vault ayarlama
description: Azure CLı ile Azure Resource Manager sanal makinesiyle kullanılmak üzere Key Vault ayarlama.
author: mimckitt
manager: vashan
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/24/2017
ms.author: mimckitt
ms.openlocfilehash: 9ae486ee522982b116af58cfb7cbfbca66a7ef4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81458752"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Azure CLı ile sanal makineler için Key Vault ayarlama

Azure Resource Manager yığınında, gizlilikler/sertifikalar, Key Vault tarafından sağlanmış kaynaklar olarak modellenir. Azure Key Vault hakkında daha fazla bilgi edinmek için bkz. [Azure Key Vault nedir?](../../key-vault/general/overview.md) Key Vault Azure Resource Manager VM 'lerle birlikte kullanılabilmesi için, Key Vault üzerindeki *Enabledfordeployment* özelliğinin true olarak ayarlanması gerekir. Bu makalede, Azure CLı kullanarak Azure sanal makineler (VM) ile kullanım için Key Vault ayarlama gösterilmektedir. 

Bu adımları gerçekleştirmek için, en son [Azure CLI](/cli/azure/install-az-cli2) 'nın yüklü olması ve [az oturum açma](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açmış olması gerekir.

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma
Bir Anahtar Kasası oluşturun ve [az keykasacreate](/cli/azure/keyvault)ile dağıtım ilkesini atayın. Aşağıdaki örnek, kaynak grubunda adlı bir Anahtar Kasası oluşturur `myKeyVault` `myResourceGroup` :

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Key Vault VM 'lerle kullanmak üzere güncelleştirme
[Az keykasa Update](/cli/azure/keyvault)ile mevcut bir anahtar kasasında dağıtım ilkesini ayarlayın. Aşağıdaki, kaynak grubunda adlı anahtar kasasını güncelleştirir `myKeyVault` `myResourceGroup` :

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Şablonları kullanarak Key Vault ayarlama
Bir şablon kullandığınızda, `enabledForDeployment` özelliği `true` Key Vault kaynağı için aşağıdaki şekilde ayarlamanız gerekir:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak bir Key Vault oluşturduğunuzda yapılandırabileceğiniz diğer seçenekler için bkz. [Anahtar Kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
