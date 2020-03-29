---
title: Windows için Azure Tanılama Uzantısı
description: Azure Tanılama Uzantısını kullanarak Azure Windows VM'lerini izleyin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471627"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Windows VM'ler için Azure Tanılama Uzantısı

## <a name="overview"></a>Genel Bakış

Azure Tanılama VM uzantısı, Windows VM'nizden performans sayaçları ve olay günlükleri gibi izleme verileri toplamanızı sağlar. Hangi verileri toplamak istediğinizi ve Azure Depolama hesabı veya Azure Etkinlik Hub'ı gibi verilerin nereye gitmesini istediğinizi ayrıntılı olarak belirtebilirsiniz. Bu verileri Azure portalında grafikler oluşturmak veya metrik uyarılar oluşturmak için de kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Azure Tanılama Uzantısı Windows 10 İstemci, Windows Server 2008 R2, 2012, 2012 R2 ve 2016'ya karşı çalıştırılabilir.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Azure TanıLama Uzantısı, hedef sanal makinenin Internet'e bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

[Azure Tanı Lama Uzantısı şeması ve özellik değerleri bu belgede açıklanmıştır.](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Azure Tanılama uzantısını çalıştırmak için bir Azure Kaynak Yöneticisi şablonunda kullanılabilir. Bkz. [Windows VM ve Azure Kaynak Yöneticisi şablonlarıyla izleme ve tanılama yı kullan.](extensions-diagnostics-template.md)

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, Azure Tanılama uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. Yukarıdaki uzantı şemasından geçerli JSON ile korunan ayarları ve ayarları özelliklerini değiştirin. 

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

Komut, `Set-AzVMDiagnosticsExtension` Varolan bir sanal makineye Azure Tanılama uzantısını eklemek için kullanılabilir. Ayrıca [Windows çalıştıran sanal bir makinede Azure Tanılama'yı etkinleştirmek için PowerShell'i kullanın.](ps-extensions-diagnostics.md)

 


```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Azure Tanılama uzantısı için daha kapsamlı bir sorun giderme kılavuzu için [bu makaleye bakın.](../../azure-monitor/platform/diagnostics-extension-troubleshooting.md)

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Tanılama Uzantısı hakkında daha fazla bilgi edinin](../../azure-monitor/platform/diagnostics-extension-overview.md)
* [Uzantı şema sını ve sürümlerini gözden geçirin](../../azure-monitor/platform/diagnostics-extension-schema-windows.md)
