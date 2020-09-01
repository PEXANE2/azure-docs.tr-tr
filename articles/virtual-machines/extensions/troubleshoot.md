---
title: Windows VM Uzantısı hatalarında sorun giderme
description: Azure Windows VM Uzantısı hatalarında sorun giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: gwallace
editor: ''
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: ad3197f20428ec751b4e3520af72dc5f8eb9ad28
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180364"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure Windows VM Uzantısı hatalarında sorun giderme
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Uzantı durumunu görüntüleme
Azure Resource Manager Şablonlar Azure PowerShell yürütülebilir. Şablon yürütüldüğünde, uzantı durumu Azure Kaynak Gezgini veya komut satırı araçlarından görüntülenebilir.

Aşağıda bir örnek verilmiştir:

Azure PowerShell:

```azurepowershell
Get-AzVM -ResourceGroupName $RGName -Name $vmName -Status
```

Örnek çıktı aşağıdaki gibidir:

```output
Extensions:  {
  "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
  "Name": "myCustomScriptExtension",
  "SubStatuses": [
    {
      "Code": "ComponentStatus/StdOut/succeeded",
      "DisplayStatus": "Provisioning succeeded",
      "Level": "Info",
      "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
          \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
          test.txt                          \\n\\n",
                  "Time": null
      },
    {
      "Code": "ComponentStatus/StdErr/succeeded",
      "DisplayStatus": "Provisioning succeeded",
      "Level": "Info",
      "Message": "",
      "Time": null
    }
  ]
}
```

## <a name="troubleshooting-extension-failures"></a>Uzantı hatalarında sorun giderme

### <a name="rerun-the-extension-on-the-vm"></a>Uzantıyı VM 'de yeniden çalıştırın
Özel Betik uzantısı kullanarak VM 'de komut dosyaları çalıştırıyorsanız, bazen VM 'nin başarıyla oluşturulduğu ancak betiğin başarısız olduğu bir hata ile çalışabilir. Bu koşullar altında, bu hatayı kurtarmak için önerilen yol, uzantıyı kaldırmak ve şablonu yeniden çalıştıramaktır.
Note: gelecekte, uzantıyı kaldırma gereksinimini ortadan kaldırmak için bu işlev geliştirilir.

#### <a name="remove-the-extension-from-azure-powershell"></a>Uzantıyı Azure PowerShell kaldır
```azurepowershell
Remove-AzVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"
```

Uzantı kaldırıldıktan sonra, şablon, betikleri sanal makinede çalıştırmak için yeniden çalıştırılabilir.

### <a name="trigger-a-new-goalstate-to-the-vm"></a>VM 'ye yeni bir GoalState tetikleme
Bir uzantının çalıştırılmadığını veya eksik bir "Windows Azure CRP sertifika Oluşturucusu" (Bu sertifika, uzantının korunan ayarlarının aktarımını korumak için kullanılır) nedeniyle yürütülebileceğini fark edebilirsiniz.
Bu sertifika, sanal makinenin içinden Windows Konuk Aracısı yeniden başlatılarak otomatik olarak yeniden oluşturulur:
- Görev Yöneticisi 'Ni açın
- Ayrıntılar sekmesine gidin
- WindowsAzureGuestAgent.exe işlemini bulun
- Sağ tıklayın ve "Görevi Sonlandır" seçeneğini belirleyin. İşlem otomatik olarak yeniden başlatılacak


Ayrıca, "boş güncelleştirme" yürüterek VM 'ye yeni bir GoalState tetikleyebilirsiniz:

Azure PowerShell:

```azurepowershell
$vm = Get-AzureRMVM -ResourceGroupName <RGName> -Name <VMName>  
Update-AzureRmVM -ResourceGroupName <RGName> -VM $vm  
```

Azure CLı:

```azurecli
az vm update -g <rgname> -n <vmname>
```

"Boş güncelleştirme" işe yaramazsa, Azure Yönetim Portalı sanal makinesine yeni bir boş veri diski ekleyebilir ve daha sonra sertifika yeniden eklendikten sonra kaldırabilirsiniz.
