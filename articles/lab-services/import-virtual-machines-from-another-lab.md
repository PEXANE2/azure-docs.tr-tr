---
title: Azure DevTest Labs'daki başka bir laboratuvardan sanal makineler alma
description: Bu makalede, sanal makinelerin başka bir laboratuvardan Azure DevTest Labs'daki geçerli laboratuvara nasıl aktarılacak olduğu açıklanmaktadır.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 299d5c8758a13edded63b99abb2f12ddf9fa14be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759525"
---
# <a name="import-virtual-machines-from-another-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki başka bir laboratuvardan sanal makineler alma
Bu makalede, başka bir laboratuvardan laboratuarınıza sanal makinelerin nasıl aktarılacak olduğu hakkında bilgi verilmektedir.

## <a name="scenarios"></a>Senaryolar
Bir laboratuvardan başka bir laboratuvara VM almanız gereken bazı senaryolar şunlardır:

- Ekipteki bir kişi kuruluş içindeki başka bir gruba taşınıyor ve geliştirici masaüstünü yeni takımın DevTest Labs'ına götürmek istiyor.
- Grup abonelik düzeyinde bir [kotaya](../azure-resource-manager/management/azure-subscription-service-limits.md) ulaştı ve takımları birkaç aboneye bölmek istiyor
- Şirket Express Route (veya başka bir yeni ağ topolojisi) için hareket ediyor ve ekip bu yeni altyapıyı kullanmak için Sanal Makineler taşımak istiyor

## <a name="solution-and-constraints"></a>Çözüm ve kısıtlamalar
Bu özellik, bir laboratuardaki (kaynak) VM'leri başka bir laboratuvara (hedef) içe aktarmanızı sağlar. İsteğe bağlı olarak işlemde hedef VM için yeni bir ad verebilirsiniz. Alma işlemi diskler, zamanlamalar, ağ ayarları ve benzeri tüm bağımlılıkları içerir.

İşlem biraz zaman alır ve aşağıdaki faktörlerden etkilenir:

- Kaynak makineye bağlı disklerin numarası/boyutu (taşıma işlemi değil, kopyalama işlemi olduğundan)
- Hedefe uzaklık (Örneğin, Doğu ABD bölgesiile Güneydoğu Asya).

İşlem tamamlandıktan sonra, kaynak Sanal Makine kapatma ya kalır ve yenisi hedef laboratuarda çalışır.

Bir laboratuvardan başka bir laboratuvara VM'leri aktarmayı planlarken dikkat edilmesi gereken iki önemli kısıtlama vardır:

- Abonelikler ve bölgeler arasında Sanal Makine içeri aktarımları desteklenir, ancak aboneliklerin aynı Azure Etkin Dizin kiracıyla ilişkilendirilmesi gerekir.
- Sanal Makineler kaynak laboratuarda talep edilebilir durumda olmamalıdır.
- Kaynak laboratuardaki VM'nin sahibi ve hedef laboratuardaki laboratuvarın sahibisin.
- Şu anda, bu özellik yalnızca Powershell ve REST API ile desteklenir.

## <a name="use-powershell"></a>PowerShell kullanma
[GitHub'dan](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImportVirtualMachines)ImportVirtualMachines.ps1 dosyasını indirin. Komut dosyasını, kaynak laboratuvardaki tek bir VM'yi veya tüm VM'leri hedef laboratuvara almak için kullanabilirsiniz.

### <a name="use-powershell-to-import-a-single-vm"></a>Tek bir VM almak için PowerShell'i kullanın
Bu powershell komut dosyasının yürütülmesi, kaynak VM'yi ve hedef laboratuvarı tanımlamayı ve isteğe bağlı olarak hedef makine için kullanılacak yeni bir ad sağlamayı gerektirir:

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -SourceVirtualMachineName "<Name of the VM to be imported from the source lab> " `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>" `
                            -DestinationVirtualMachineName "<Optional: specify a new name for the imported VM in the destination lab>"
```

### <a name="use-powershell-to-import-all-vms-in-the-source-lab"></a>Kaynak laboratuvardaki tüm VM'leri almak için PowerShell'i kullanın
Kaynak Sanal Makine belirtilmemişse, komut dosyası DevTest Labs'daki tüm VM'leri otomatik olarak içeri aktarın.  Örnek:

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>"
```

## <a name="use-http-rest-to-import-a-vm"></a>VM almak için HTTP REST'i kullanın
REST arama basittir. Kaynak ve hedef kaynaklarını tanımlamak için yeterli bilgi verirsiniz. İşlemin hedef laboratuvar kaynağında gerçekleştiğini unutmayın.

```REST
POST https://management.azure.com/subscriptions/<DestinationSubscriptionID>/resourceGroups/<DestinationResourceGroup>/providers/Microsoft.DevTestLab/labs/<DestinationLab>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<SourceSubscriptionID>/resourcegroups/<SourceResourceGroup>/providers/microsoft.devtestlab/labs/<SourceLab>/virtualmachines/<NameofVMTobeImported>",
   destinationVirtualMachineName: "<NewNameForImportedVM>"
}
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar için ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
