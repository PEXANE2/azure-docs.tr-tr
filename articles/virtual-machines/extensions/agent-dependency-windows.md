---
title: Windows için Azure Monitör Bağımlılık sanal makine uzantısı
description: Sanal makine uzantısı kullanarak Windows sanal makinesinde Azure Monitörü Bağımlılık aracısını dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: magoedte
ms.openlocfilehash: 27d43af2d5860d287d8b5914379747ae528db34b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250679"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-windows"></a>Windows için Azure Monitör Bağımlılık sanal makine uzantısı

VMs Haritası için Azure Monitor özelliği, verilerini Microsoft Bağımlılık aracısından alır. Windows için Azure VM Bağımlılık aracısı sanal makine uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı, Bağımlılık aracısını Azure sanal makinelerine yükler. Bu belge, Windows için Azure VM Bağımlılık aracısı sanal makine uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

## <a name="operating-system"></a>İşletim sistemi

Windows için Azure VM Bağımlılık aracısı uzantısı, VM dağıtımı makalesiiçin Azure Monitor'un Desteklenen işletim sistemleri bölümünde listelenen desteklenen işletim [sistemleriyle](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) karşı çalıştırılabilir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Azure Windows VM'deki Azure VM Bağımlılık aracısı uzantısı şemasını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Azure VM. Supported Windows Server versions:  2008 R2 and above (x64)."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer/Örnek |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| yayımcı | Microsoft.Azure.Monitoring.DependencyAgent |
| type | BağımlılıkAjanWindows |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure Kaynak Yöneticisi şablonlarıyla Azure VM uzantılarını dağıtabilirsiniz. Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Azure VM Bağımlılık aracısı uzantısını çalıştırmak için Azure Kaynak Yöneticisi şablonunda önceki bölümde ayrıntılı JSON şemasını kullanabilirsiniz.

Sanal makine uzantısı için JSON sanal makine kaynağının içine iç içe olabilir. Veya, kaynak yöneticisi JSON şablonunun köküne veya en üst seviyesine yerleştirebilirsiniz. JSON'un yerleşimi kaynak adının ve türünün değerini etkiler. Daha fazla bilgi için [bkz.](../../azure-resource-manager/templates/child-resource-name-type.md)

Aşağıdaki örnek, Bağımlılık aracısı uzantısı sanal makine kaynağı nın içinde iç içe olduğunu varsayar. Uzantı kaynağını yuvaya yerleştirdiğinizde, JSON `"resources": []` sanal makinenin nesnesine yerleştirilir.


```json
{
    "type": "extensions",
    "name": "DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

UzantıJSON'u şablonun köküne yerleştirdiğinizde, kaynak adı üst sanal makineye bir başvuru içerir. Tür iç içe yapılandırmayı yansıtır.

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtımı

Bağımlılık aracısı `Set-AzVMExtension` sanal makine uzantısını varolan bir sanal makineye dağıtmak için komutu kullanabilirsiniz. Komutu çalıştırmadan önce, genel ve özel yapılandırmaların PowerShell karma tablosunda depolanmış olması gerekir.

```powershell

Set-AzVMExtension -ExtensionName "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ExtensionType "DependencyAgentWindows" `
    -TypeHandlerVersion 9.5 `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure PowerShell modüllerini kullanarak aşağıdaki komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Monitoring.DependencyAgent\
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Veya bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek **al'ı**seçin. Azure Desteği'ni nasıl kullanacağınız hakkında bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
