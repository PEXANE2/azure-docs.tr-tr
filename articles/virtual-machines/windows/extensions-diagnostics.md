---
title: Windows için Azure Tanılama uzantısı
description: Azure Tanılama uzantısını kullanarak Azure Windows sanal makinelerini izleme
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: d4ba3e8bbdad72f10c630a056f988ec83e9b1a68
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471627"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Windows VM 'Leri için Azure Tanılama uzantısı

## <a name="overview"></a>Genel Bakış

Azure Tanılama VM uzantısı, Windows VM 'nizden performans sayaçları ve olay günlükleri gibi izleme verilerini toplamanıza olanak sağlar. Toplamak istediğiniz verileri ve bir Azure depolama hesabı ya da Azure Olay Hub 'ı gibi verilerin nereye gitmesini istediğinizi belirleyebilirsiniz. Bu verileri, Azure portal grafik oluşturmak veya ölçüm uyarıları oluşturmak için de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Azure Tanılama uzantısı Windows 10 Istemcisi, Windows Server 2008 R2, 2012, 2012 R2 ve 2016 karşısında çalıştırılabilir.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Azure Tanılama uzantısı, hedef sanal makinenin Internet 'e bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

[Azure Tanılama uzantısı şeması ve özellik değerleri bu belgede açıklanmaktadır.](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması, bir Azure Resource Manager şablon dağıtımı sırasında Azure Tanılama uzantısını çalıştırmak için bir Azure Resource Manager şablonunda kullanılabilir. Bkz. [WINDOWS VM ve Azure Resource Manager şablonlarıyla izleme ve tanılama kullanma](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLı, Azure Tanılama uzantısını var olan bir sanal makineye dağıtmak için kullanılabilir. Korumalı ayarlar ve ayarlar özelliklerini yukarıdaki uzantı şemasından geçerli bir JSON ile değiştirin. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell dağıtımı

`Set-AzVMDiagnosticsExtension` komutu, var olan bir sanal makineye Azure Tanılama uzantısını eklemek için kullanılabilir. Ayrıca bkz. [Windows çalıştıran bir sanal makinede Azure tanılama etkinleştirmek Için PowerShell 'ı kullanma](ps-extensions-diagnostics.md).

 


```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorunları Gider

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Azure Tanılama uzantısı için daha kapsamlı bir sorun giderme kılavuzu için [Bu makaleye bakın](../../azure-monitor/platform/diagnostics-extension-troubleshooting.md) .

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayına dosya. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Tanılama uzantısı hakkında daha fazla bilgi edinin](../../azure-monitor/platform/diagnostics-extension-overview.md)
* [Uzantı şemasını ve sürümlerini gözden geçirin](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)
