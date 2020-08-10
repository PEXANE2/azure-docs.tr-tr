---
title: dosya dahil etme
description: dosya dahil etme
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 069baf627c0230b6a4727c375494352ab3e6a803
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67188305"
---
Bir kaynak grubuna iki etiket eklemek için [set-AzResourceGroup](/powershell/module/az.resources/set-azresourcegroup) komutunu kullanın:

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Şimdi üçüncü bir etiket istediğinizi varsayalım. Bir kaynağa veya kaynak grubuna etiket uyguladığınız her durumda ilgili kaynağın veya kaynak grubunun üzerinde mevcut olan etiketlerin üzerine yazarsınız. Mevcut etiketleri kaybetmeden yeni bir etiket eklemek için, mevcut etiketleri almalı, yeni etiketi eklemeli ve etiket koleksiyonunu yeniden uygulamalısınız:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzResourceGroup -Tag $tags -Name myResourceGroup
```

Kaynaklar, kaynak grubundan etiketleri devralmaz. Şu anda, kaynak grubunuzun üç etiketi vardır ama kaynakların hiç etiketi yoktur. Bir kaynak grubundaki tüm etiketleri yinelenmeyen kaynaklardaki mevcut etiketleri koruyarak gruptaki kaynaklara uygulamak için aşağıdaki betiği kullanın:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags
        
        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Alternatif olarak, mevcut etiketleri korumadan kaynak grubundaki etiketleri kaynaklara uygulayabilirsiniz:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Birkaç değeri tek etikette birleştirmek için bir JSON dizesi kullanın.

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Var olan etiketleri kaybetmeden birden fazla değere sahip yeni bir etiket eklemek için mevcut etiketleri almanız, yeni etiket için bir JSON dizesi kullanmanız ve etiketlerin koleksiyonunu yeniden uygulamanız gerekir:

```azurepowershell-interactive
# Get existing tags and add a new tag
$ResourceGroup = Get-AzResourceGroup -Name myResourceGroup
$Tags = $ResourceGroup.Tags
$Tags.Add("CostCenter", "{`"Dept`":`"IT`",`"Environment`":`"Test`"}")

# Reapply the updated set of tags
$ResourceGroup | Set-AzResourceGroup -Tag $Tags
```

Tüm etiketleri kaldırmak için bir boş karma tablosu geçirirsiniz.

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ }
```
