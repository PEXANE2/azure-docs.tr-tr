---
title: Linux VM'leri için Azure Anahtar Kasası'nı ayarlama
description: Azure CLI ile bir Azure Kaynak Yöneticisi sanal makinesiyle kullanılmak üzere Key Vault nasıl ayarlaylayın.
author: mimckitt
manager: vashan
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/24/2017
ms.author: mimckitt
ms.openlocfilehash: 9ae486ee522982b116af58cfb7cbfbca66a7ef4a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458752"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Azure CLI ile sanal makineler için Key Vault nasıl ayarlanır?

Azure Kaynak Yöneticisi yığınında, sırlar/sertifikalar Key Vault tarafından sağlanan kaynaklar olarak modellenir. Azure Anahtar Kasası hakkında daha fazla bilgi edinmek için [Azure Anahtar Kasası nedir?](../../key-vault/general/overview.md) Key Vault'un Azure Kaynak Yöneticisi VM'lerle kullanılabilmesi için, Key Vault'taki *EnabledForDeployment* özelliğinin gerçek olarak ayarlanması gerekir. Bu makalede, Azure CLI'yi kullanarak Azure sanal makineleri (VM' ler) ile kullanılmak üzere Key Vault'u nasıl ayarlayabileceğiniz gösterilmektedir. 

Bu adımları gerçekleştirmek için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma
Bir anahtar kasası oluşturun ve [az keyvault oluşturmak](/cli/azure/keyvault)ile dağıtım ilkesi atayın. Aşağıdaki örnek, kaynak grubunda `myKeyVault` adı `myResourceGroup` geçen bir anahtar kasası oluşturur:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>VM'lerde kullanım için Bir Anahtar Kasası güncelleştirme
Dağıtım ilkesini [az keyvault güncelleştirmesi](/cli/azure/keyvault)ile varolan bir anahtar kasasında ayarlayın. Kaynak grubunda adı geçen `myKeyVault` `myResourceGroup` anahtar kasası aşağıdaki güncelleştirmeleri dir:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Key Vault'u ayarlamak için şablonları kullanma
Bir şablon kullandığınızda, `enabledForDeployment` özelliği Key Vault `true` kaynağı için aşağıdaki şekilde ayarlamanız gerekir:

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
Şablonları kullanarak bir Anahtar Kasası oluşturduğunuzda yapılandırabileceğiniz diğer seçenekler [için](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)bkz.
