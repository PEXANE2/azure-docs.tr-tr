---
title: Windows için Log Analytics sanal makine uzantısı
description: Sanal makine uzantısı nı kullanarak Windows sanal makinesinde Log Analytics aracısını dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/30/2020
ms.author: akjosh
ms.openlocfilehash: 85977819d30ddc8745eb9231242eb1990222676c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530997"
---
# <a name="log-analytics-virtual-machine-extension-for-windows"></a>Windows için Log Analytics sanal makine uzantısı

Azure Monitör Günlükleri, bulut ve şirket içi varlıklar arasında izleme özellikleri sağlar. Windows için Log Analytics aracısı sanal makine uzantısı Microsoft tarafından yayınlanır ve desteklenir. Uzantı, Log Analytics aracısını Azure sanal makinelerine yükler ve sanal makineleri varolan bir Log Analytics çalışma alanına kaydeder. Bu belge, Windows için Log Analytics sanal makine uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Windows işletim sistemleri hakkında daha fazla bilgi için [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) makalesine bakın.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM Uzantı sürümü
Aşağıdaki tablo, her sürüm için Windows Log Analytics VM uzantısı ve Log Analytics aracı paketinin sürümünün eşmesini sağlar. 

| Log Analytics Windows aracı paketi sürümü | Log Analytics Windows VM uzantılı sürümü | Yayın Tarihi | Sürüm Notları |
|--------------------------------|--------------------------|--------------------------|--------------------------|
| 10.20.18029 | 1.0.18029 | Mart 2020   | <ul><li>SHA-2 kod imzalama desteği ekler</li><li>VM uzantısı kurulumunu ve yönetimini geliştirir</li><li>Sunucular için Azure Arc'taki bir hatayı giderir tümleştirme</li><li>Müşteri desteği için yerleşik bir sorun giderme aracı ekler</li><li>Ek Azure Devlet bölgeleri için destek ekler</li> |
| 10.20.18018 | 1.0.18018 | Ekim 2019 | <ul><li> Küçük hata düzeltmeleri ve stabilizasyon iyileştirmeleri </li></ul> |
| 10.20.18011 | 1.0.18011 | Temmuz 2019 | <ul><li> Küçük hata düzeltmeleri ve stabilizasyon iyileştirmeleri </li><li> MaxExpressionDepth'i 10000'e yükseltti </li></ul> |
| 10.20.18001 | 1.0.18001 | Haziran 2019 | <ul><li> Küçük hata düzeltmeleri ve stabilizasyon iyileştirmeleri </li><li> Proxy bağlantısı yaparken varsayılan kimlik bilgilerini devre dışı düşürme özelliği eklendi (WINHTTP_AUTOLOGON_SECURITY_LEVEL_HIGH desteği) </li></ul>|
| 10.19.13515 | 1.0.13515 | Mart 2019 | <ul><li>Küçük stabilizasyon düzeltmeleri </li></ul> |
| 10.19.10006 | yok | Aralık 2018 | <ul><li> Küçük stabilizasyon düzeltmeleri </li></ul> | 
| 8.0.11136 | yok | Eylül 2018 |  <ul><li> VM move'da kaynak kimliği değişikliğini algılamak için ek destek </li><li> Uzantılı olmayan yüklemeyi kullanırken kaynak kimliğini bildirmek için destek eklendi </li></ul>| 
| 8.0.11103 | yok |  Nisan 2018 | |
| 8.0.11081 | 1.0.11081 | Kasım 2017 | | 
| 8.0.11072 | 1.0.11072 | Eylül 2017 | |
| 8.0.11049 | 1.0.11049 | Şubat 2017 | |


### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Log Analytics aracısını otomatik olarak sağlar ve Azure aboneliğinin varsayılan Log Analytics çalışma alanına bağlar. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunu yapmak, yapılandırılan çalışma alanının üzerine yazar ve Azure Güvenlik Merkezi ile bağlantıyı kopar.

### <a name="internet-connectivity"></a>İnternet bağlantısı
Windows için Log Analytics aracıuzantısı, hedef sanal makinenin internete bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, Log Analytics aracıuzantısı için şema gösterir. Uzantı, hedef Log Analytics çalışma alanından çalışma alanı kimliği ve çalışma alanı anahtarı gerektirir. Bunlar, Azure portalındaki çalışma alanı ayarlarında bulunabilir. Çalışma alanı anahtarı hassas veri olarak ele alınması gerektiğinden, korumalı bir ayar yapılandırmasında depolanmalıdır. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. **WorkspaceId** ve **workspaceKey'in** büyük/küçük harf duyarlı olduğunu unutmayın.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| yayımcı | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| çalışma alanıId (örn.) * | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| çalışma alanıAnahtar (örn. ) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwvv35W0pOqQAU7uQ== |

\*WorkspaceId, Log Analytics API'sinde consumerId olarak adlandırılır.

> [!NOTE]
> Ek özellikler için [Connect Windows Computers to Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)bkz.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Log Analytics aracı uzantısını çalıştırmak için bir Azure Kaynak Yöneticisi şablonunda kullanılabilir. Log Analytics aracısı VM uzantısını içeren bir örnek şablon [Azure Quickstart Galerisi'nde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm)bulunabilir. 

>[!NOTE]
>Şablon, aracıyı birden çok çalışma alanına rapor yapacak şekilde yapılandırmak istediğinizde birden fazla çalışma alanı kimliği ve çalışma alanı anahtarı belirtmeyi desteklemez. Aracıyı birden çok çalışma alanına rapor yapacak şekilde yapılandırmak için [bkz.](../../azure-monitor/platform/agent-manage.md#adding-or-removing-a-workspace)  

Sanal makine uzantısı için JSON, sanal makine kaynağının içine yerleştirilebilir veya Kaynak Yöneticisi JSON şablonunun köküne veya üst seviyesine yerleştirilebilir. JSON'un yerleşimi kaynak adının ve türünün değerini etkiler. Daha fazla bilgi için [bkz.](../../azure-resource-manager/templates/child-resource-name-type.md) 

Aşağıdaki örnek, Log Analytics uzantısının sanal makine kaynağının içine girdiğini varsayar. Uzantı kaynağını iç içe alırken, JSON `"resources": []` sanal makinenin nesnesine yerleştirilir.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

UzantıJSON'u şablonun köküne yerleştirirken, kaynak adı üst sanal makineye bir başvuru içerir ve tür iç içe yapılan yapılandırmayı yansıtır. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtımı

Komut, `Set-AzVMExtension` Log Analytics aracısı sanal makine uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. Komutu çalıştırmadan önce, genel ve özel yapılandırmaların PowerShell karma tablosunda depolanmış olması gerekir. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzVMExtension -ExtensionName "MicrosoftMonitoringAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure PowerShell modüllerini kullanarak aşağıdaki komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
