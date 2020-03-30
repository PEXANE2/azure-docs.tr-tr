---
title: VM uzantıları içeren Azure Kaynak Grupları dışa aktarma
description: Sanal makine uzantıları içeren Kaynak Yöneticisi şablonlarını dışa aktarın.
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: akjosh
ms.openlocfilehash: 79991dad96742109817d579b951082d1a30e3951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253916"
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>VM uzantıları içeren Kaynak Grupları verme

Azure Kaynak Grupları, daha sonra yeniden dağıtılabilen yeni bir Kaynak Yöneticisi şablonuna aktarılabilir. Dışa aktarma işlemi varolan kaynakları yorumlar ve benzer bir Kaynak Grubunda dağıtıldığında sonuçları olan bir Kaynak Yöneticisi şablonu oluşturur. Kaynak Grubu dışa aktarma seçeneğini Sanal Makine uzantıları içeren bir Kaynak Grubuna karşı kullanırken, uzantı uyumluluğu ve korumalı ayarlar gibi birkaç öğenin dikkate alınması gerekir.

Bu belge, desteklenen uzantıların listesi ve güvenli verilerin işlenmesiyle ilgili ayrıntılar da dahil olmak üzere sanal makine uzantılarıyla ilgili Kaynak Grubu dışa aktarma işleminin nasıl çalıştığını ayrıntılarıyla anlatır.

## <a name="supported-virtual-machine-extensions"></a>Desteklenen Sanal Makine Uzantıları

Birçok Sanal Makine uzantıları mevcuttur. Tüm uzantılar "Otomasyon Komut Dosyası" özelliği kullanılarak Kaynak Yöneticisi şablonuna aktarılamaz. Sanal makine uzantısı desteklenmiyorsa, dışa aktarılan şablona el ile geri yerleştirilmesi gerekir.

Aşağıdaki uzantılar otomasyon komut dosyası özelliği ile dışa aktarılabilir.

| Dahili numara ||||
|---|---|---|---|
| Acronis Yedekleme | Datadog Windows Aracısı | Linux için Os Patching | VM Snapshot Linux
| Acronis Yedekleme Linux | Docker Uzantısı | Kukla Ajan |
| Bg Bilgi | DSC Uzantısı | Site 24x7 Apm Insight |
| BMC CTM Aracılinux | Dynatrace Linux | Site 24x7 Linux Server |
| BMC CTM Aracısı Pencereler | Dynatrace Windows | Site 24x7 Windows Server |
| Şef Müşteri | HPE Güvenlik Uygulama Defender | Trend Micro DSA |
| Özel Betik | IaaS Antimalware | Trend Micro DSA Linux |
| Özel Betik Uzantısı | IaaS Tanılama | Linux için VM Erişimi |
| Linux için Özel Komut Dosyası | Linux Chef İstemci | Linux için VM Erişimi |
| Datadog Linux Aracısı | Linux Tanılama | Sanal Makine Anlık Görüntüsü |

## <a name="export-the-resource-group"></a>Kaynak Grubunu Dışa Aktarma

Kaynak Grubunu yeniden kullanılabilir bir şablona aktarmak için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açın
2. Hub Menüsünde Kaynak Grupları'nı tıklatın
3. Listeden hedef kaynak grubunu seçme
4. Kaynak Grubu bıçağında Otomasyon Komut Dosyası'nı tıklatın

![Şablon Dışa Aktarma](./media/export-templates/template-export.png)

Azure Kaynak Yöneticisi komut dosyasını otomasyona aktarAn bir Kaynak Yöneticisi şablonu, bir parametre dosyası ve PowerShell ve Azure CLI gibi çeşitli örnek dağıtım komut dosyaları üretir. Bu noktada, dışa aktarılan şablon indirme düğmesini kullanarak indirilebilir, şablon kitaplığına yeni bir şablon olarak eklenebilir veya dağıtma düğmesini kullanarak yeniden dağıtılabilir.

## <a name="configure-protected-settings"></a>Korumalı ayarları yapılandırma

Birçok Azure sanal makine uzantısı, kimlik bilgileri ve yapılandırma dizeleri gibi hassas verileri şifreleyen korumalı ayarlar yapılandırması içerir. Korumalı ayarlar otomasyon komut dosyasıyla dışa aktarılmaz. Gerekirse, korumalı ayarların dışa aktarılan şablona yeniden eklenmesi gerekir.

### <a name="step-1---remove-template-parameter"></a>Adım 1 - Şablon parametresi kaldırma

Kaynak Grubu dışa aktarıldığında, dışa aktarılan korumalı ayarlara değer sağlamak için tek bir şablon parametresi oluşturulur. Bu parametre kaldırılabilir. Parametreyi kaldırmak için parametre listesine bakın ve bu JSON örneğine benzeyen parametreyi silin.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Adım 2 - Korumalı ayarlar özelliklerini alın

Her korumalı ayar gerekli özellikler kümesi olduğundan, bu özelliklerin bir listesinin toplanması gerekir. Korumalı ayarlar yapılandırmasının her parametresi [GitHub'daki Azure Kaynak Yöneticisi şemasında](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json)bulunabilir. Bu şema yalnızca bu belgenin genel bakış bölümünde listelenen uzantılar için parametre kümelerini içerir. 

Şema deposu nun içinden, bu örnek `IaaSDiagnostics`için istenilen uzantıyı arayın. Uzantıları `protectedSettings` nesnesi bulunduğunda, her parametreye dikkat edin. `IaasDiagnostic` Uzantı örneğinde, gerekli parametreler `storageAccountName`, `storageAccountKey`, `storageAccountEndPoint`ve .

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-the-protected-configuration"></a>Adım 3 - Korumalı yapılandırmayı yeniden oluşturma

Dışa aktarılan şablonda, dışa aktarılan korumalı ayar nesnesini, gerekli uzantı parametreleri `protectedSettings` ve her biri için bir değer içeren yeni bir nesneyle arayın ve değiştirin.

`IaasDiagnostic` Uzantı örneğinde, yeni korumalı ayar yapılandırması aşağıdaki örnek gibi görünür:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

Son uzantı kaynağı aşağıdaki JSON örneğine benzer:

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "tags": {
        "displayName": "AzureDiagnostics"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

Özellik değerlerini sağlamak için şablon parametreleri kullanılıyorsa, bunların oluşturulması gerekir. Korumalı ayar değerleri için şablon parametreleri `SecureString` oluştururken, hassas değerlerin sabitlenmesi için parametre türünü kullandığınızdan emin olun. Parametreleri kullanma hakkında daha fazla bilgi için [bkz.](../../resource-group-authoring-templates.md)

`IaasDiagnostic` Uzantı örneğinde, Kaynak Yöneticisi şablonunun parametreler bölümünde aşağıdaki parametreler oluşturulur.

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

Bu noktada, şablon herhangi bir şablon dağıtım yöntemi kullanılarak dağıtılabilir.
