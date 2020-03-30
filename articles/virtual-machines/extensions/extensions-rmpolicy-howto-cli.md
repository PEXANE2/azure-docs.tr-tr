---
title: VM uzantısı yüklemesini kısıtlamak için Azure İlkesi'ni kullanma
description: VM uzantısı dağıtımlarını kısıtlamak için Azure İlkesi'ni kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: akjosh
ms.reviewer: cynthn
ms.openlocfilehash: 3c660f7e05af43c2aad6f7283e32cfc1d85571ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066840"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Linux VM'lerinde uzantıların yüklenmesini kısıtlamak için Azure İlkesi'ni kullanın

Linux VM'lerinizde belirli uzantıların kullanılmasını veya yüklenmesini engellemek istiyorsanız, kaynak grubundaki VM uzantılarını kısıtlamak için CLI'yi kullanarak bir Azure ilkesi oluşturabilirsiniz. 

Bu öğretici, sürekli olarak en son sürüme güncelleştirilen Azure Bulut Kabuğu içindeki CLI'yi kullanır. Azure CLI'yi yerel olarak çalıştırmak istiyorsanız, sürüm 2.0.26 veya sonraki sürümyüklemeniz gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-rules-file"></a>Kurallar dosyası oluşturma

Hangi uzantıların yüklenebileceğini kısıtlamak için, uzantıyı tanımlamak için mantığı sağlamak için bir [kuralınız](../../governance/policy/concepts/definition-structure.md#policy-rule) olması gerekir.

Bu örnek, Azure Bulut Su Ağıt'ta bir kural dosyası oluşturarak 'Microsoft.OSTCExtensions' tarafından yayınlanan uzantıları yüklemeyi nasıl reddebileceğinizi gösterir, ancak CLI'de yerel olarak çalışıyorsanız, yerel bir dosya oluşturabilir ve yolu (~/clouddrive) makinenizdeki yerel dosyaya giden yolile değiştirebilirsiniz.

Bir [bash Cloud Shell](https://shell.azure.com/bash)olarak , türü:

```bash
vim ~/clouddrive/azurepolicy.rules.json
```

Aşağıdaki .json dosyaya kopyalayıp yapıştırın.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.OSTCExtensions/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/publisher",
                "equals": "Microsoft.OSTCExtensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

İşi bittiğinde **Esc** tuşuna basın ve dosyayı kaydetmek ve kapatmak için **:wq** yazın.


## <a name="create-a-parameters-file"></a>Parametre dosyası oluşturma

Ayrıca, engellemek için uzantıların bir listesini geçmek için kullanmak için bir yapı oluşturan bir [parametre](../../governance/policy/concepts/definition-structure.md#parameters) dosyası gerekir. 

Bu örnek, Cloud Shell'de Linux VM'leri için nasıl bir parametre dosyası oluşturabileceğinizi gösterir, ancak CLI'da yerel olarak çalışıyorsanız, yerel bir dosya oluşturabilir ve yolu (~/clouddrive) makinenizdeki yerel dosyaya giden yol ile değiştirebilirsiniz.

Bash [Cloud Shell'](https://shell.azure.com/bash)de , yazın:

```bash
vim ~/clouddrive/azurepolicy.parameters.json
```

Aşağıdaki .json dosyaya kopyalayıp yapıştırın.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied. Example: CustomScriptForLinux, VMAccessForLinux etc.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

İşi bittiğinde **Esc** tuşuna basın ve dosyayı kaydetmek ve kapatmak için **:wq** yazın.

## <a name="create-the-policy"></a>İlkeyi oluşturma

İlke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı, ilketanımlamak için kurallar ve parametreler dosyaları kullanır. [az ilke tanımını](/cli/azure/role/assignment?view=azure-cli-latest)kullanarak ilke tanımını oluşturun.

Bu örnekte, kurallar ve parametreler oluşturduğunuz ve bulut kabuğunuzda .json dosyaları olarak depolanan dosyalardır.

```azurecli-interactive
az policy definition create \
   --name 'not-allowed-vmextension-linux' \
   --display-name 'Block VM Extensions' \
   --description 'This policy governs which VM extensions that are blocked.' \
   --rules '~/clouddrive/azurepolicy.rules.json' \
   --params '~/clouddrive/azurepolicy.parameters.json' \
   --mode All
```


## <a name="assign-the-policy"></a>İlke atama

Bu örnek, [az ilke atama sı](/cli/azure/policy/assignment)kullanılarak bir kaynak grubuna ilke atar. **myResourceGroup** kaynak grubunda oluşturulan herhangi bir VM, Linux VM Access'i veya Linux için Özel Komut Dosyası uzantılarını yükleyemez. İlke atamadan önce kaynak grubunun var olması gerekir.

Abonelik kimliğinizi örnektekinin yerine kullanmak için [az hesap listesini](/cli/azure/account?view=azure-cli-latest) kullanın.


```azurecli-interactive
az policy assignment create \
   --name 'not-allowed-vmextension-linux' \
   --scope /subscriptions/<subscription Id>/resourceGroups/myResourceGroup \
   --policy "not-allowed-vmextension-linux" \
   --params '{
        "notAllowedExtensions": {
            "value": [
                "VMAccessForLinux",
                "CustomScriptForLinux"
            ]
        }
    }'
```

## <a name="test-the-policy"></a>İlkeyi test etme

Yeni bir VM oluşturup yeni bir kullanıcı eklemeye çalışarak ilkeyi test edin.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

VM Access uzantısını kullanarak **myNewUser** adında yeni bir kullanıcı oluşturmaya çalışın.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --password 'mynewuserpwd123!'
```



## <a name="remove-the-assignment"></a>Atamayı kaldırma

```azurecli-interactive
az policy assignment delete --name 'not-allowed-vmextension-linux' --resource-group myResourceGroup
```
## <a name="remove-the-policy"></a>İlkeyi kaldırma

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure İlkesi](../../governance/policy/overview.md).