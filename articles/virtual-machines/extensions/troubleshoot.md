---
title: Windows VM uzantı hataları giderme
description: Azure Windows VM uzantı lı eklenti hatalarıhakkında bilgi edinin
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
ms.openlocfilehash: bc99a9c9e9ff985730ec97dbacd1d7c1de06a45e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073647"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure Windows VM uzantı hataları giderme
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Uzantı durumunu görüntüleme
Azure Kaynak Yöneticisi şablonları Azure PowerShell'den yürütülebilir. Şablon yürütüldükten sonra uzantı durumu Azure Kaynak Gezgini'nden veya komut satırı araçlarından görüntülenebilir.

Örnek aşağıda verilmiştir:

Azure PowerShell:

      Get-AzVM -ResourceGroupName $RGName -Name $vmName -Status

Örnek çıktı aşağıdaki gibidir:

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
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Sorun giderme uzantısı hataları
### <a name="rerun-the-extension-on-the-vm"></a>VM'deki uzantıyı yeniden çalıştırın
Özel Komut Dosyası Uzantısı'nı kullanarak VM'de komut dosyaları çalıştırıyorsanız, bazen VM'nin başarıyla oluşturulduğu ancak komut dosyasının başarısız olduğu bir hatayla karşınıza çıkabilir. Bu koşullar altında, bu hatadan kurtarmanın önerilen yolu uzantıyı kaldırmak ve şablonu yeniden çalıştırmaktır.
Not: Gelecekte, uzantını kaldırma gereksinimini kaldırmak için bu işlevsellik geliştirilmeye yöneliktir.

#### <a name="remove-the-extension-from-azure-powershell"></a>Uzantıyı Azure PowerShell'den kaldırma
    Remove-AzVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Uzantı kaldırıldıktan sonra şablon, VM'deki komut dosyalarını çalıştırmak için yeniden çalıştırılabilir.

