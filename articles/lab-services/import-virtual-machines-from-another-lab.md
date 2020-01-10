---
title: Azure DevTest Labs | sanal makinelerini başka bir laboratuvardan içeri aktarın | Microsoft Docs
description: Sanal makineleri başka bir laboratuvardan geçerli laboratuvara nasıl içeri aktaracağınızı öğrenin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 0778759958e70c564779f5493d7cf8b646f6ced0
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644656"
---
# <a name="import-virtual-machines-from-another-lab-in-azure-devtest-labs"></a>Azure DevTest Labs içindeki başka bir laboratuvardan sanal makineleri içeri aktarma
Bu makalede, sanal makinelerin başka bir laboratuvardan laboratuvarınızda nasıl içeri aktarılacağı hakkında bilgi sağlanır.

## <a name="scenarios"></a>Senaryolar
VM 'Leri bir laboratuvardan başka bir laboratuvarya içeri aktarmanız gereken bazı senaryolar aşağıda verilmiştir:

- Ekipte bulunan bir kişi kuruluş içindeki başka bir gruba taşınıyor ve geliştirici masaüstünü yeni ekibin DevTest Labs 'e almak istiyor.
- Grup, [abonelik düzeyi kotasına](../azure-resource-manager/management/azure-subscription-service-limits.md) ulaştı ve takımları birkaç aboneliğe bölmek istiyor
- Şirket hızlı rotaya (veya başka bir yeni ağ topolojisine) taşınıyor ve takım sanal makineleri bu yeni altyapıyı kullanacak şekilde taşımak istiyor

## <a name="solution-and-constraints"></a>Çözüm ve kısıtlamalar
Bu özellik, VM 'Leri bir laboratuvarda (kaynak) başka bir laboratuvarda (hedef) içeri aktarmanızı sağlar. İsteğe bağlı olarak, işlemdeki hedef sanal makine için yeni bir ad verebilirsiniz. İçeri aktarma işlemi diskler, zamanlamalar, ağ ayarları vb. gibi tüm bağımlılıkları içerir.

İşlem biraz zaman alır ve aşağıdaki faktörlerden etkilenir:

- Kaynak makineye bağlı disklerin sayısı/boyutu (bir kopyalama işlemi olduğundan ve taşıma işlemi değil)
- Hedefe mesafe (örneğin, Doğu ABD bölge, Güneydoğu Asya).

İşlem tamamlandıktan sonra kaynak sanal makine kapalı kalır ve yeni bir hedef laboratuvarda çalışır.

' De bir laboratuvardan başka bir laboratuvara VM 'Leri içeri aktarmayı planlarken dikkat etmeniz için iki temel kısıtlama vardır:

- Abonelikler arasında ve bölgeler arasında sanal makine içeri aktarmaları desteklenir, ancak abonelikler aynı Azure Active Directory kiracısıyla ilişkilendirilmelidir.
- Sanal makineler, kaynak laboratuvarında çakışmaz bir durumda olmamalıdır.
- Kaynak laboratuvarında sanal makinenin sahibi olursunuz ve hedef laboratuvardaki laboratuvarın sahibidir.
- Şu anda bu özellik yalnızca PowerShell ve REST API aracılığıyla desteklenir.

## <a name="use-powershell"></a>PowerShell'i kullanma
[GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImportVirtualMachines)'Dan ımportvirtualmachines. ps1 dosyasını indirin. Betiği, kaynak laboratuvardaki tüm VM 'Leri hedef laboratuvarya aktarmak için kullanabilirsiniz.

### <a name="use-powershell-to-import-a-single-vm"></a>Tek bir VM 'yi içeri aktarmak için PowerShell 'i kullanma
Bu PowerShell betiğini yürütmek için kaynak VM ve hedef laboratuvarın belirlenmesi ve isteğe bağlı olarak hedef makine için kullanılacak yeni bir ad sağlama gerekir:

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -SourceVirtualMachineName "<Name of the VM to be imported from the source lab> " `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>" `
                            -DestinationVirtualMachineName "<Optional: specify a new name for the imported VM in the destination lab>"
```

### <a name="use-powershell-to-import-all-vms-in-the-source-lab"></a>Kaynak laboratuvardaki tüm VM 'Leri içeri aktarmak için PowerShell 'i kullanma
Kaynak sanal makine belirtilmemişse, komut dosyası DevTest Labs içindeki tüm sanal makineleri otomatik olarak içeri aktarır.  Örneğin:

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>"
```

## <a name="use-http-rest-to-import-a-vm"></a>VM 'yi içeri aktarmak için HTTP REST kullanma
REST çağrısı basittir. Kaynak ve hedef kaynakları belirlemek için yeterli bilgi verirsiniz. İşlemin hedef laboratuvar kaynağında gerçekleşeceğini unutmayın.

```REST
POST https://management.azure.com/subscriptions/<DestinationSubscriptionID>/resourceGroups/<DestinationResourceGroup>/providers/Microsoft.DevTestLab/labs/<DestinationLab>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<SourceSubscriptionID>/resourcegroups/<SourceResourceGroup>/providers/microsoft.devtestlab/labs/<SourceLab>/virtualmachines/<NameofVMTobeImported>",
   destinationVirtualMachineName: "<NewNameForImportedVM>"
}
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar ilkelerini ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
