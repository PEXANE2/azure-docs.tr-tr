---
title: Örnek-ağ Izleyicisi olmayan bölgeler için denetim
description: Bu örnek ilke tanımı, ağ izleyicisinin belirtilen bölge için etkin olup olmadığını denetler
author: DCtheGeek
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: 3c93fb72be82c1a89ea4420f4b8d34bb98785376
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72254778"
---
# <a name="sample---audit-if-network-watcher-is-not-enabled-for-region"></a>Örnek-ağ izleyicisinin bölge için etkin olup olmadığını denetleyin

Bu ilke, ağ izleyicisinin belirtilen bölge için etkin olup olmadığını denetler. Ağ izleyicisinin etkinleştirilip etkinleştirilmediğini denetlemek için bölgenin adını belirtirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/Network/audit-network-watcher-existence/azurepolicy.json "Audit if Network Watcher is not enabled for region")]

Bu şablonu, [PowerShell](#deploy-with-powershell) veya [Azure CLI](#deploy-with-azure-cli)ile [Azure Portal](#deploy-with-the-portal)kullanarak dağıtabilirsiniz.

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![ Ilke örneğini Azure 'a dağıtın](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Faudit-network-watcher-existence%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name "audit-network-watcher-existence" -DisplayName "Audit if Network Watcher is not enabled for region" -description "This policy audits if Network Watcher is not enabled for a selected region." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/audit-network-watcher-existence/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/audit-network-watcher-existence/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -location <Audit if Network Watcher is not enabled for region> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizle

Kaynak grubunu, VM 'yi ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Azure CLı ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'audit-network-watcher-existence' --display-name 'Audit if Network Watcher is not enabled for region' --description 'This policy audits if Network Watcher is not enabled for a selected region.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/audit-network-watcher-existence/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/audit-network-watcher-existence/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-network-watcher-existence"
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLı dağıtımını Temizleme

Kaynak grubunu, VM 'yi ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerinde](index.md) daha fazla örnek inceleyin