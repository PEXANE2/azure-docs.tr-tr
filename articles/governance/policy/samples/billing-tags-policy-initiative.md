---
title: Örnek-faturalandırma etiketleri ilke girişimi
description: Bu örnek ilke tanımı kümesi, maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir.
author: DCtheGeek
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: f2190b5759c53d645c1d0150004271ba04669c94
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981410"
---
# <a name="sample---billing-tags-policy-initiative"></a>Örnek-faturalandırma etiketleri ilke girişimi

Bu ilke kümesi, maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Bu şablonu [PowerShell](#deploy-with-powershell)ile dağıtabilirsiniz.

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Mevcut kaynaklara etiket uygulama

İlkeleri atadıktan sonra, eklediğiniz etiket ilkelerini zorlamak için tüm mevcut kaynaklara bir güncelleştirme tetikleyebilirsiniz. Aşağıdaki betik, kaynaklarda var olmuş diğer tüm etiketleri korur:

```azurepowershell-interactive
$resources = Get-AzResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin