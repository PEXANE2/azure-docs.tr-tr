---
title: Linux için Azure Monitör Bağımlılık sanal makine uzantısı
description: Sanal makine uzantısı kullanarak Azure Monitör Bağımlılığı aracısını Linux sanal makinesine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: magoedte
ms.openlocfilehash: 82f9c5a67cb056752cf8310be3b7c9f0bd2501e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254046"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-linux"></a>Linux için Azure Monitör Bağımlılık sanal makine uzantısı

VMs Haritası için Azure Monitor özelliği, verilerini Microsoft Bağımlılık aracısından alır. Linux için Azure VM Bağımlılık aracısı sanal makine uzantısı Microsoft tarafından yayınlanır ve desteklenir. Uzantı, Bağımlılık aracısını Azure sanal makinelerine yükler. Bu belge, Linux için Azure VM Bağımlılık aracısı sanal makine uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Linux için Azure VM Bağımlılık aracısı uzantısı, VM dağıtımı makalesinin Desteklenen işletim sistemleri bölümünde listelenen desteklenen işletim [sistemleriyle](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) karşı çalıştırılabilir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Azure Linux VM'deki Azure VM Bağımlılık aracısı uzantısı şemasını gösterir. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux Azure VM."
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
        "type": "DependencyAgentLinux",
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
| type | BağımlılıkAgentLinux |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure Kaynak Yöneticisi şablonlarıyla Azure VM uzantılarıdağıtabilirsiniz. Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Azure VM Bağımlılık aracısı uzantısını çalıştırmak için Azure Kaynak Yöneticisi şablonunda önceki bölümde ayrıntılı JSON şemasını kullanabilirsiniz.

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
        "type": "DependencyAgentLinux",
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
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Bağımlılık aracısı VM uzantısını varolan bir sanal makineye dağıtmak için Azure CLI'yi kullanabilirsiniz.  

```azurecli

az vm extension set \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın:

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

```
/opt/microsoft/dependcency-agent/log/install.log 
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurun. Veya bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek **al'ı**seçin. Azure Desteği'ni nasıl kullanacağınız hakkında bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
