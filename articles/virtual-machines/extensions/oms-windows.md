---
title: Windows için Log Analytics sanal makine uzantısı
description: Log Analytics aracısını bir sanal makine uzantısı kullanarak Windows sanal makinesine dağıtın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79530997"
---
# <a name="log-analytics-virtual-machine-extension-for-windows"></a>Windows için Log Analytics sanal makine uzantısı

Azure Izleyici günlükleri, bulut ve şirket içi varlıklar arasında izleme özellikleri sağlar. Windows için Log Analytics Aracısı sanal makine uzantısı Microsoft tarafından yayımlanır ve desteklenir. Uzantı Log Analytics aracısını Azure sanal makinelerine yükleyip sanal makineleri mevcut bir Log Analytics çalışma alanına kaydeder. Bu belgede, Windows için Log Analytics sanal makine uzantısı için desteklenen platformlar, konfigürasyonlar ve dağıtım seçenekleri ayrıntılı olarak bulunmaktadır.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Windows işletim sistemleri hakkında daha fazla bilgi için [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) makalesine bakın.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM Uzantısı sürümü
Aşağıdaki tabloda, her sürüm için Windows Log Analytics sanal makine uzantısı ve Log Analytics aracı paketi sürümünün bir eşlemesi verilmiştir. 

| Log Analytics Windows Agent paketi sürümü | Log Analytics Windows VM Uzantısı sürümü | Yayın Tarihi | Sürüm Notları |
|--------------------------------|--------------------------|--------------------------|--------------------------|
| 10.20.18029 | 1.0.18029 | Mart 2020   | <ul><li>SHA-2 kod imzalama desteği ekler</li><li>VM Uzantısı yükleme ve yönetimini geliştirir</li><li>Sunucu tümleştirmesi için Azure Arc 'da bir hata çözer</li><li>Müşteri desteği için yerleşik bir sorun giderme aracı ekler</li><li>Ek Azure Kamu bölgeleri için destek ekler</li> |
| 10.20.18018 | 1.0.18018 | Ekim 2019 | <ul><li> Küçük hata düzeltmeleri ve sabitleştirme iyileştirmeleri </li></ul> |
| 10.20.18011 | 1.0.18011 | Temmuz 2019 | <ul><li> Küçük hata düzeltmeleri ve sabitleştirme iyileştirmeleri </li><li> Artan MaxExpressionDepth 10000 </li></ul> |
| 10.20.18001 | 1.0.18001 | Haziran 2019 | <ul><li> Küçük hata düzeltmeleri ve sabitleştirme iyileştirmeleri </li><li> Ara sunucu bağlantısı kurulurken varsayılan kimlik bilgilerini devre dışı bırakma özelliği eklendi (WINHTTP_AUTOLOGON_SECURITY_LEVEL_HIGH için destek) </li></ul>|
| 10.19.13515 | 1.0.13515 | Mart 2019 | <ul><li>Küçük sabitleştirme düzeltmeleri </li></ul> |
| 10.19.10006 | yok | Dec 2018 | <ul><li> Küçük sabitleştirme düzeltmeleri </li></ul> | 
| 8.0.11136 | yok | Eyl 2018 |  <ul><li> VM taşıma üzerinde kaynak KIMLIĞI değişikliğini algılama desteği eklendi </li><li> Uzantı dışı yüklemesi kullanılırken raporlama kaynak KIMLIĞI desteği eklendi </li></ul>| 
| 8.0.11103 | yok |  Nisan 2018 | |
| 8.0.11081 | 1.0.11081 | Kas 2017 | | 
| 8.0.11072 | 1.0.11072 | Eyl 2017 | |
| 8.0.11049 | 1.0.11049 | Şub 2017 | |


### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Log Analytics aracısını otomatik olarak sağlar ve Azure aboneliğinin varsayılan Log Analytics çalışma alanına bağlar. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımlarda çalıştırılmayın. Bunun yapılması, yapılandırılan çalışma alanının üzerine yazar ve Azure Güvenlik Merkezi ile bağlantıyı keser.

### <a name="internet-connectivity"></a>İnternet bağlantısı
Windows için Log Analytics Aracısı uzantısı, hedef sanal makinenin Internet 'e bağlı olmasını gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Log Analytics aracı uzantısının şemasını gösterir. Uzantı, hedef Log Analytics çalışma alanından çalışma alanı KIMLIĞI ve çalışma alanı anahtarı gerektirir. Bunlar, Azure portal çalışma alanı ayarlarından bulunabilir. Çalışma alanı anahtarı gizli veriler olarak değerlendirilmelidir, bu, korumalı bir ayar yapılandırmasında depolanmalıdır. Azure VM Uzantısı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. Çalışma alanı **kimliği** ve **workspaceKey** büyük/küçük harfe duyarlı olduğunu unutmayın.

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

| Adı | Değer/örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| yayımcı | Microsoft. EnterpriseCloud. Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| çalışma alanı kimliği (ör.) * | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (ör.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ = = |

\*Çalışma alanı kimliği, Log Analytics API 'sinde ConsumerID olarak adlandırılır.

> [!NOTE]
> Ek özellikler için bkz. Azure [Windows bilgisayarlarını Azure 'A Bağlama İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması, bir Azure Resource Manager şablon dağıtımı sırasında Log Analytics Aracısı uzantısını çalıştırmak için bir Azure Resource Manager şablonunda kullanılabilir. [Azure hızlı başlangıç galerisinde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm)log ANALYTICS Agent VM uzantısını içeren bir örnek şablon bulunabilir. 

>[!NOTE]
>Bu şablon, aracıyı birden çok çalışma alanına rapor verecek şekilde yapılandırmak istediğinizde birden fazla çalışma alanı KIMLIĞI ve çalışma alanı anahtarı belirtilmesini desteklemez. Aracıyı birden çok çalışma alanına rapor verecek şekilde yapılandırmak için, bkz. [çalışma alanı ekleme veya kaldırma](../../azure-monitor/platform/agent-manage.md#adding-or-removing-a-workspace).  

Bir sanal makine uzantısı için JSON, sanal makine kaynağının içinde iç içe veya Kaynak Yöneticisi JSON şablonunun kök veya üst düzeyine yerleştirilmiş olabilir. JSON yerleştirmesi, kaynak adının ve türün değerini etkiler. Daha fazla bilgi için bkz. [alt kaynaklar için ad ve tür ayarlama](../../azure-resource-manager/templates/child-resource-name-type.md). 

Aşağıdaki örnek, Log Analytics uzantısının sanal makine kaynağının içinde iç içe olduğunu varsayar. Uzantı kaynağını yuvalama sırasında JSON, sanal makinenin `"resources": []` nesnesine yerleştirilir.


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

JSON uzantısını şablon köküne yerleştirirken, kaynak adı üst sanal makineye bir başvuru içerir ve tür iç içe geçmiş yapılandırmayı yansıtır. 

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

Komut `Set-AzVMExtension` , Log Analytics Aracısı sanal makinesi uzantısını var olan bir sanal makineye dağıtmak için kullanılabilir. Komutu çalıştırmadan önce, ortak ve özel yapılandırmaların bir PowerShell karma tablosunda depolanması gerekir. 

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

Uzantı dağıtımlarının durumu hakkındaki veriler Azure portal alabilir ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için Azure PowerShell modülünü kullanarak aşağıdaki komutu çalıştırın.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıkışı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
