---
title: PowerShell kullanarak VM etiketleme
description: PowerShell kullanarak bir sanal makineyi etiketleme hakkında bilgi edinin
author: mmccrory
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 3aa40ed396e87b342207fc51576cd28170c7d4e9
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89322390"
---
# <a name="how-to-tag-a-virtual-machine-in-azure-using-powershell"></a>Azure 'da PowerShell kullanarak bir sanal makineyi etiketleme
Bu makalede, Azure 'daki bir Windows sanal makinesini Kaynak Yöneticisi dağıtım modeliyle etiketlemek için farklı yollar açıklanmaktadır. Etiketler, doğrudan bir kaynağa veya bir kaynak grubuna yerleştirilebilecek Kullanıcı tanımlı anahtar/değer çiftleridir. Azure Şu anda kaynak ve kaynak grubu başına en fazla 50 etiketi desteklemektedir. Etiketler, oluşturma sırasında veya var olan bir kaynağa eklenen bir kaynağa yerleştirilebilir. Lütfen etiketlerin yalnızca Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan kaynaklar için desteklendiğini unutmayın. Azure CLı kullanarak bir sanal makineyi etiketlemek istiyorsanız bkz. Azure [CLI kullanarak Azure 'da bir sanal makineyi etiketleme](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell ile etiketleme
PowerShell aracılığıyla etiket oluşturmak, eklemek ve silmek için önce [PowerShell ortamınızı Azure Resource Manager][PowerShell environment with Azure Resource Manager]ayarlamanız gerekir. Kurulumu tamamladıktan sonra, oluşturma sırasında veya kaynak PowerShell aracılığıyla oluşturulduktan sonra Işlem, ağ ve depolama kaynaklarına Etiketler yerleştirebilirsiniz. Bu makale, sanal makinelere yerleştirilmiş etiketleri görüntüleme/düzenlemeyle ilgili olarak yoğunlaşacaktır.

 

İlk olarak cmdlet üzerinden bir sanal makineye gidin `Get-AzVM` .

```azurepowershell
PS C:\> Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"
```

Sanal makineniz zaten etiketler içeriyorsa, kaynağınızın tüm etiketlerini görürsünüz:

```json
Tags : {
        "Application": "MyApp1",
        "Created By": "MyName",
        "Department": "MyDepartment",
        "Environment": "Production"
        }
```

PowerShell aracılığıyla Etiketler eklemek istiyorsanız `Set-AzResource` komutunu kullanabilirsiniz. Bkz. Etiketler PowerShell aracılığıyla güncelleştirilirken, Etiketler bir bütün olarak güncelleştirilir. Bu nedenle, zaten etiketlere sahip olan bir kaynağa bir etiket ekliyorsanız, kaynağa yerleştirilmesini istediğiniz tüm etiketleri dahil etmeniz gerekir. Aşağıda, PowerShell cmdlet 'Leri aracılığıyla bir kaynağa ek etiketlerin nasıl ekleneceği gösterilmektedir.

Bu ilk cmdlet, ve özelliğini kullanarak *Mytestvm* üzerinde yer alan tüm etiketleri *$Tags* değişkenine ayarlar `Get-AzResource` `Tags` .

```azurepowershell
PS C:\> $tags = (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags
```

İkinci komut, verilen değişkenin etiketlerini görüntüler.

```azurepowershell
PS C:\> $tags

Key           Value
----          -----
Department    MyDepartment
Application   MyApp1
Created By    MyName
Environment   Production
```

Üçüncü komut *$Tags* değişkenine ek bir etiket ekler. **+=** Yeni anahtar/değer çiftini *$Tags* listesine eklemek için öğesinin kullanımını dikkate alın.

```azurepowershell
PS C:\> $tags += @{Location="MyLocation"}
```

Dördüncü komut *$Tags* değişkeninde tanımlanan tüm etiketleri verilen kaynağa ayarlar. Bu durumda, test sanal makinedir.

```azurepowershell
PS C:\> Set-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags
```

Beşinci komut kaynaktaki tüm etiketleri görüntüler. Gördüğünüz gibi, *konum* artık değer olarak *MyLocation* içeren bir etiket olarak tanımlanmıştır.

```azurepowershell
PS C:\> (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Key           Value
----          -----
Department    MyDepartment
Application   MyApp1
Created By    MyName
Environment   Production
Location      MyLocation
```

PowerShell aracılığıyla etiketleme hakkında daha fazla bilgi edinmek için [Azure Kaynak cmdlet 'lerine][Azure Resource Cmdlets]göz atın.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızın etiketlenmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager genel bakış][Azure Resource Manager Overview] ve [etiketleri kullanarak Azure kaynaklarınızı düzenleme][Using Tags to organize your Azure Resources].
* Etiketlerin Azure kaynaklarını kullanımınızı yönetmenize nasıl yardımcı olduğunu görmek için bkz. [Azure Faturanızı Anlama][Understanding your Azure Bill] ve [Microsoft Azure Kaynak tüketiminiz hakkında öngörüler elde][Gain insights into your Microsoft Azure resource consumption]etme.

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/management/manage-resources-powershell.md
[Azure Resource Cmdlets]: /powershell/module/az.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/management/overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/management/tag-resources.md
[Understanding your Azure Bill]:../../cost-management-billing/understand/review-individual-bill.md
[Gain insights into your Microsoft Azure resource consumption]:../../cost-management-billing/manage/usage-rate-card-overview.md
