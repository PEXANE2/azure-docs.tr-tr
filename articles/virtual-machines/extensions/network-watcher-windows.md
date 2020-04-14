---
title: Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı
description: Sanal makine uzantısı nı kullanarak Windows sanal makinesinde Ağ İzleyicisi Aracısı'nı dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: mimckitt
ms.openlocfilehash: f226e240a59b33c2913919495410b1a4923b4902
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261678"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Windows için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi,](../../network-watcher/network-watcher-monitoring-overview.md) Azure ağlarının izlenmesine olanak tanıyan bir ağ performansı izleme, tanılama ve analiz hizmetidir. Network Watcher Agent sanal makine uzantısı, ağ trafiğini isteğe bağlı olarak ve Azure sanal makinelerdeki diğer gelişmiş işlevleri yakalamak için bir gerekliliktir.


Bu belge, Windows için Ağ İzleyicisi Sanal Makine uzantısı için desteklenen platformları ve dağıtım seçeneklerini ayrıntılarıyla açıklar. Aracının yüklenmesi sanal makinenin yeniden başlatılmasını engellemez veya yeniden başlatmayı gerektirmez. Uzantıyı dağıttığınız sanal makinelere dağıtabilirsiniz. Sanal makine bir Azure hizmeti tarafından dağıtılırsa, sanal makineye uzantı yüklemeye izin verip vermediğini belirlemek için hizmetiçin belgeleri denetleyin.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Windows için Ağ İzleyicisi uzantısı Windows Server 2008 R2, 2012, 2012 R2 ve 2016 sürümlerine karşı çalıştırılabilir. Nano Server şu anda desteklenmez.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Ağ İzleyicisi Aracısı işlevlerinden bazıları, hedef sanal makinenin Internet'e bağlanmasını gerektirir. Giden bağlantılar kurma olanağı olmadan, Ağ İzleyicisi Aracısı paket yakalamalarını depolama hesabınıza yükleyemez. Daha fazla bilgi için lütfen [Ağ İzleyicisi belgelerine](../../network-watcher/network-watcher-monitoring-overview.md)bakın.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Ağ İzleyicisi Uzantısı için şema gösterir. Uzantı, kullanıcı tarafından sağlanan ayarları gerektirmez veya desteklemez ve varsayılan yapılandırmasına dayanır.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| yayımcı | Microsoft.Azure.NetworkWatcher |
| type | Ağ WatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure Kaynak Yöneticisi şablonlarıyla Azure VM uzantılarıdağıtabilirsiniz. Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Ağ İzleyicisi uzantısını çalıştırmak için Azure Kaynak Yöneticisi şablonunda önceki bölümde ayrıntılı JSON şemasını kullanabilirsiniz.

## <a name="powershell-deployment"></a>PowerShell dağıtımı

Network `Set-AzVMExtension` Watcher Agent sanal makine uzantısını varolan bir sanal makineye dağıtmak için komutu kullanın:

```powershell
Set-AzVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Azure portalından ve PowerShell'den uzantı dağıtımlarının durumu yla ilgili verileri alabilirsiniz. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure PowerShell modüllerini kullanarak aşağıdaki komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, Ağ İzleyicisi Kullanım Kılavuzu belgelerine başvurabilir veya [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
