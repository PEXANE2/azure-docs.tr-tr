---
title: VM uzantısı yüklemesini kısıtlamak için Azure İlkesi'ni kullanma
description: Uzantı dağıtımlarını kısıtlamak için Azure İlkesi'ni kullanın.
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
ms.openlocfilehash: 428db340ce43463939ce71ffadd4188060f3e732
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073106"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-windows-vms"></a>Windows VM'lerde uzantıları yüklemeyi kısıtlamak için Azure İlkesi'ni kullanma

Windows VM'lerinizde belirli uzantıların kullanılmasını veya yüklenmesini engellemek istiyorsanız, kaynak grubundaki VM uzantılarını kısıtlamak için PowerShell'i kullanarak bir Azure ilkesi oluşturabilirsiniz. 

Bu öğretici, bulut kabuğu içinde sürekli olarak en son sürüme güncellenen Azure PowerShell'i kullanır. 

 

## <a name="create-a-rules-file"></a>Kurallar dosyası oluşturma

Hangi uzantıların yüklenebileceğini kısıtlamak için, uzantıyı tanımlamak için mantığı sağlamak için bir [kuralınız](../../governance/policy/concepts/definition-structure.md#policy-rule) olması gerekir.

Bu örnek, Azure Bulut Su Bulutu'nda bir kural dosyası oluşturarak 'Microsoft.Compute' tarafından yayınlanan uzantıları nasıl reddebileceğinizi gösterir, ancak PowerShell'de yerel olarak çalışıyorsanız, yerel bir dosya oluşturabilir ve yolu ($home/clouddrive) makinenizdeki yerel dosyaya giden yol ile değiştirebilirsiniz.

Bulut [Kabuğunda](https://shell.azure.com/powershell), yazın:

```azurepowershell-interactive
nano $home/clouddrive/rules.json
```

Aşağıdaki .json dosyaya kopyalayıp yapıştırın.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

İşi bittiğinde **Ctrl + O** tuşuna basın ve dosyayı kaydetmek için enter tuşuna **basın.** Dosyayı kapatmak ve çıkmak için **Ctrl + X** tuşuna basın.

## <a name="create-a-parameters-file"></a>Parametre dosyası oluşturma

Ayrıca, engellemek için uzantıların bir listesini geçmek için kullanmak için bir yapı oluşturan bir [parametre](../../governance/policy/concepts/definition-structure.md#parameters) dosyası gerekir. 

Bu örnek, Bulut Bulut Su'sunlarda VM'ler için nasıl bir parametre dosyası oluşturabileceğinizi gösterir, ancak PowerShell'de yerel olarak çalışıyorsanız, yerel bir dosya oluşturabilir ve yolu ($home/clouddrive) makinenizdeki yerel dosyaya giden yol ile değiştirebilirsiniz.

[Bulut Kabuğu'nda](https://shell.azure.com/powershell), yazın:

```azurepowershell-interactive
nano $home/clouddrive/parameters.json
```

Aşağıdaki .json dosyaya kopyalayıp yapıştırın.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

İşi bittiğinde **Ctrl + O** tuşuna basın ve dosyayı kaydetmek için enter tuşuna **basın.** Dosyayı kapatmak ve çıkmak için **Ctrl + X** tuşuna basın.

## <a name="create-the-policy"></a>İlkeyi oluşturma

İlke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı, ilketanımlamak için kurallar ve parametreler dosyaları kullanır. [New-AzPolicyDefinition](https://docs.microsoft.com/powershell/module/az.resources/new-azpolicydefinition) cmdlet kullanarak bir ilke tanımı oluşturun.

 İlke kuralları ve parametreleri, oluşturduğunuz ve bulut bulutunuzda .json dosyaları olarak depolanan dosyalardır.


```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
   -Name "not-allowed-vmextension-windows" `
   -DisplayName "Not allowed VM Extensions" `
   -description "This policy governs which VM extensions that are explicitly denied."   `
   -Policy 'C:\Users\ContainerAdministrator\clouddrive\rules.json' `
   -Parameter 'C:\Users\ContainerAdministrator\clouddrive\parameters.json'
```




## <a name="assign-the-policy"></a>İlke atama

Bu örnek, ilkeyi [Yeni-AzPolicyAtama](https://docs.microsoft.com/powershell/module/az.resources/new-azpolicyassignment)kullanarak bir kaynak grubuna atar. **myResourceGroup** kaynak grubunda oluşturulan herhangi bir VM, VM Access Agent veya Özel Komut Dosyası uzantılarını yükleyemez. 

[Get-AzSubscription 'ı kullanın | Abonelik](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription) kimliğinizi örnektekinin yerine kullanmak için Format-Tablo cmdlet.

```azurepowershell-interactive
$scope = "/subscriptions/<subscription id>/resourceGroups/myResourceGroup"
$assignment = New-AzPolicyAssignment `
   -Name "not-allowed-vmextension-windows" `
   -Scope $scope `
   -PolicyDefinition $definition `
   -PolicyParameter '{
    "notAllowedExtensions": {
        "value": [
            "VMAccessAgent",
            "CustomScriptExtension"
        ]
    }
}'
$assignment
```

## <a name="test-the-policy"></a>İlkeyi test etme

İlkeyi sınamak için VM Access uzantısını kullanmayı deneyin. Aşağıdaki "Set-AzVMAccessExtension : Kaynak 'myVMAccess' ilkesi tarafından izin verilmedi" iletisi ile başarısız olmalıdır.

```azurepowershell-interactive
Set-AzVMAccessExtension `
   -ResourceGroupName "myResourceGroup" `
   -VMName "myVM" `
   -Name "myVMAccess" `
   -Location EastUS 
```

Portalda, parola değişikliği "Şablon dağıtımı ilke ihlali nedeniyle başarısız oldu" ile başarısız olur. iletisi döndürmektedir.

## <a name="remove-the-assignment"></a>Atamayı kaldırma

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name not-allowed-vmextension-windows -Scope $scope
```

## <a name="remove-the-policy"></a>İlkeyi kaldırma

```azurepowershell-interactive
Remove-AzPolicyDefinition -Name not-allowed-vmextension-windows
```
    
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Azure İlkesi](../../governance/policy/overview.md).
