---
title: Azure'da Windows VM kaynağı nasıl etiketilir?
description: Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da oluşturulan windows sanal makinesini etiketleme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: b646b1a14d6cedcafa662192229daa570a0d2441
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616422"
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makinesini etiketleme
Bu makalede, Kaynak Yöneticisi dağıtım modeli aracılığıyla Azure'daki bir Windows sanal makinesini etiketlemenin farklı yolları açıklanmaktadır. Etiketler, doğrudan bir kaynağa veya kaynak grubuna yerleştirilebilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure şu anda kaynak ve kaynak grubu başına en fazla 50 etiketi destekler. Etiketler, oluşturulduğu sırada bir kaynağa yerleştirilebilir veya varolan bir kaynağa eklenebilir. Etiketlerin yalnızca Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan kaynaklar için desteklenir. Bir Linux sanal makinesini etiketlemek istiyorsanız, [Azure'da bir Linux sanal makineyi nasıl etiketleyeceksiniz 'e](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>PowerShell ile etiketleme
PowerShell aracılığıyla etiketler oluşturmak, eklemek ve silmek için öncelikle [PowerShell ortamınızı Azure Kaynak Yöneticisi ile][PowerShell environment with Azure Resource Manager]ayarlamanız gerekir. Kurulumu tamamladıktan sonra, oluşturma sırasında veya kaynak PowerShell üzerinden oluşturulduktan sonra Bilgi İşlem, Ağ ve Depolama kaynaklarına etiketler yerleştirebilirsiniz. Bu makale, Sanal Makineler'e yerleştirilen etiketleri görüntüleme/düzenleme üzerine yoğunlaşacaktır.

 

İlk olarak, `Get-AzVM` cmdlet üzerinden sanal makine gidin.

        PS C:\> Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Sanal Makineniz zaten etiketler içeriyorsa, kaynağınızdaki tüm etiketleri görürsünüz:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

PowerShell üzerinden etiket eklemek isterseniz, komutu `Set-AzResource` kullanabilirsiniz. PowerShell üzerinden etiketleri güncellerken, etiketlerin bir bütün olarak güncelleştirilecek. Bu nedenle, zaten etiketleri olan bir kaynağa bir etiket ekliyorsanız, kaynağa yerleştirilmesini istediğiniz tüm etiketleri eklemeniz gerekir. Aşağıda PowerShell Cmdlets aracılığıyla kaynağa ek etiketler innasıl ekleyeceğiniz e-bir örnek verilmiştir.

Bu ilk cmdlet, *MyTestVM'e* yerleştirilen tüm etiketleri, `Get-AzResource` `Tags` özelliği ve özelliğini kullanarak *$tags* değişkenine ayarlar.

        PS C:\> $tags = (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

İkinci komut, verilen değişkenin etiketlerini görüntüler.

```
    PS C:\> $tags
    
    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
```

Üçüncü *komut, $tags* değişkenine ek bir etiket ekler. Yeni anahtar/değer **+=** çiftini *$tags* listesine eklemek için kullanın.

        PS C:\> $tags += @{Location="MyLocation"}

Dördüncü komut, *$tags* değişkeninde tanımlanan tüm etiketleri verilen kaynağa ayarlar. Bu durumda, MyTestVM olduğunu.

        PS C:\> Set-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Beşinci komut kaynaktaki tüm etiketleri görüntüler. Gördüğünüz gibi, *Konum* artık değeri olarak *MyLocation* etiketi olarak tanımlanır.

```
    PS C:\> (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
    Location      MyLocation
```

PowerShell'de etiketleme hakkında daha fazla bilgi edinmek için [Azure Kaynak Cmdlets'e][Azure Resource Cmdlets]göz atın.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi edinmek için Azure Kaynakları Yöneticinize [Genel Bakış][Azure Resource Manager Overview] ve [Azure Kaynaklarınızı düzenlemek için Etiketleri Kullanma'ya][Using Tags to organize your Azure Resources]bakın.
* Etiketlerin Azure kaynaklarını kullanımınızı yönetmenize nasıl yardımcı olabileceğini görmek için Azure [Faturanızı Anlama][Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketiminize ilişkin öngörüler elde][Gain insights into your Microsoft Azure resource consumption]etme bölümüne bakın.

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/management/manage-resources-powershell.md
[Azure Resource Cmdlets]: https://docs.microsoft.com/powershell/module/az.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/management/overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/management/tag-resources.md
[Understanding your Azure Bill]:../../cost-management-billing/understand/review-individual-bill.md
[Gain insights into your Microsoft Azure resource consumption]:../../cost-management-billing/manage/usage-rate-card-overview.md
