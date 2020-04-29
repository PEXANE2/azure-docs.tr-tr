---
title: Linux için Azure Izleyici bağımlılığı sanal makine uzantısı
description: Bir sanal makine uzantısı kullanarak Linux sanal makinesine Azure Izleyici bağımlılık aracısını dağıtın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79254046"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-linux"></a>Linux için Azure Izleyici bağımlılığı sanal makine uzantısı

VM'ler için Azure İzleyici Map özelliği, verileri Microsoft bağımlılık Aracısı 'ndan alır. Linux için Azure VM bağımlılık Aracısı sanal makine uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı, Azure sanal makinelerine bağımlılık Aracısı 'nı yüklüyor. Bu belgede, Linux için Azure VM bağımlılık Aracısı sanal makine uzantısı için desteklenen platformlar, konfigürasyonlar ve dağıtım seçenekleri ayrıntılı olarak bulunmaktadır.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Linux için Azure VM bağımlılık Aracısı uzantısı, VM'ler için Azure İzleyici dağıtım makalesinin [desteklenen işletim sistemleri](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) bölümünde listelenen desteklenen işletim sistemlerine karşı çalıştırılabilir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Azure Linux VM 'de Azure VM bağımlılık Aracısı uzantısı için şemayı gösterir. 

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

| Adı | Değer/örnek |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| yayımcı | Microsoft. Azure. Monitoring. DependencyAgent |
| type | DependencyAgentLinux |
| typeHandlerVersion | 9,5 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantılarını Azure Resource Manager şablonlarıyla dağıtabilirsiniz. Bir Azure Resource Manager şablon dağıtımı sırasında Azure VM bağımlılık Aracısı uzantısı 'nı çalıştırmak için bir Azure Resource Manager şablonunun önceki bölümünde ayrıntılı JSON şemasını kullanabilirsiniz.

Bir sanal makine uzantısı için JSON, sanal makine kaynağının içinde iç içe olabilir. Ya da, bunu bir Kaynak Yöneticisi JSON şablonunun kök veya üst düzeyine yerleştirebilirsiniz. JSON yerleştirmesi, kaynak adının ve türün değerini etkiler. Daha fazla bilgi için bkz. [alt kaynaklar için ad ve tür ayarlama](../../azure-resource-manager/templates/child-resource-name-type.md).

Aşağıdaki örnek, bağımlılık Aracısı uzantısının sanal makine kaynağının içinde iç içe olduğunu varsayar. Uzantı kaynağını iç içe aktardığınızda JSON, sanal makinenin `"resources": []` nesnesine yerleştirilir.


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

JSON uzantısını şablon köküne yerleştirdiğinizde, kaynak adı üst sanal makineye bir başvuru içerir. Tür, iç içe geçmiş yapılandırmayı yansıtır. 

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

## <a name="azure-cli-deployment"></a>Azure CLı dağıtımı

Bağımlılık Aracısı VM uzantısını var olan bir sanal makineye dağıtmak için Azure CLı 'yı kullanabilirsiniz.  

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

Uzantı dağıtımlarının durumuyla ilgili veriler Azure portal ve Azure CLı kullanılarak alınabilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için, Azure CLı kullanarak aşağıdaki komutu çalıştırın:

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkışı aşağıdaki dosyaya kaydedilir:

```
/opt/microsoft/dependcency-agent/log/install.log 
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Ya da bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin. Azure desteğinin nasıl kullanılacağı hakkında bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
