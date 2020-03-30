---
title: Azure Kaynak Yöneticisi şablonundaki sanal makineler | Microsoft Azure
description: Azure Kaynak Yöneticisi şablonunda sanal makine kaynağının nasıl tanımlandığı hakkında daha fazla bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.openlocfilehash: c9bf1cf0564655c932e066e5b74225382375e9c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235414"
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki sanal makineler

Bu makalede, sanal makineler için geçerli olan bir Azure Kaynak Yöneticisi şablonunun yönleri açıklanmaktadır. Bu makalede, sanal bir makine oluşturmak için tam bir şablon açıkdeğildir; bunun için depolama hesapları, ağ arabirimleri, genel IP adresleri ve sanal ağlar için kaynak tanımlarına ihtiyacınız vardır. Bu kaynakların birlikte nasıl tanımlanabileceği hakkında daha fazla bilgi için [Kaynak Yöneticisi şablonu gözden geçirin.](../../azure-resource-manager/resource-manager-template-walkthrough.md)

[Galeride](https://azure.microsoft.com/documentation/templates/?term=VM) VM kaynağını içeren birçok şablon vardır. Şablona dahil edilebilen tüm öğeler burada açıklanmaz.

 

Bu örnek, belirli sayıda VM oluşturmak için şablonun tipik bir kaynak bölümünü gösterir:

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
        "adminUsername": "[parameters('adminUsername')]", 
        "adminPassword": "[parameters('adminPassword')]" 
      }, 
      "storageProfile": { 
        "imageReference": { 
          "publisher": "MicrosoftWindowsServer", 
          "offer": "WindowsServer", 
          "sku": "2012-R2-Datacenter", 
          "version": "latest" 
        }, 
        "osDisk": { 
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
>Bu örnek, daha önce oluşturulmuş bir depolama hesabına dayanır. Şablondan dağıtarak depolama hesabı oluşturabilirsiniz. Örnek, bir ağ arabirimine ve şablonda tanımlanacak bağımlı kaynaklarına da dayanır. Bu kaynaklar örnekte gösterilmez.
>
>

## <a name="api-version"></a>API Sürümü

Kaynakları şablon kullanarak dağıtırken, kullanılacak API'nin bir sürümünü belirtmeniz gerekir. Örnek, bu apiVersion öğesini kullanarak sanal makine kaynağını gösterir:

```json
"apiVersion": "2016-04-30-preview",
```

Şablonunuzda belirttiğiniz API sürümü, şablonda tanımlayabileceğiniz özellikleri etkiler. Genel olarak, şablon oluştururken en son API sürümünü seçmeniz gerekir. Varolan şablonlar için, önceki bir API sürümünü kullanmaya devam etmek isteyip istemediğinize karar verebilir veya yeni özelliklerden yararlanmak için şablonunuzu en son sürüm için güncelleştirebilirsiniz.

En son API sürümlerini almak için bu fırsatları kullanın:

- REST API - [Tüm kaynak sağlayıcılarını listele](https://docs.microsoft.com/rest/api/resources/providers)
- PowerShell - [Get-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/get-azresourceprovider)
- Azure CLI - [az sağlayıcı gösterisi](https://docs.microsoft.com/cli/azure/provider)


## <a name="parameters-and-variables"></a>Parametreler ve değişkenler

[Parametreler,](../../resource-group-authoring-templates.md) şablonu çalıştırdığınızda değerleri belirtmenizi kolaylaştırır. Bu parametreler bölümü örnekte kullanılır:

```json
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Örnek şablonu dağıttığınızda, her VM'de yönetici hesabının adı ve parolası için değerler ve oluşturulacak VM sayısı girersiniz. Şablonla yönetilen ayrı bir dosyada parametre değerlerini belirtme veya istendiğinde değerler sağlama seçeneğiniz vardır.

[Değişkenler,](../../resource-group-authoring-templates.md) şablon boyunca tekrar tekrar kullanılan veya zaman içinde değişebilen değerleri ayarlamanızı kolaylaştırır. Bu değişkenler bölümü örnekte kullanılır:

```json
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

Örnek şablonu dağıttığınızda, daha önce oluşturulmuş depolama hesabının adı ve tanımlayıcısı için değişken değerleri kullanılır. Değişkenler tanılama uzantısı için ayarları sağlamak için de kullanılır. Şablonunuzdaki parametreleri ve değişkenleri nasıl yapılandırmak istediğinize karar vermenize yardımcı olmak için [Azure Kaynak Yöneticisi şablonları oluşturmak](../../resource-manager-template-best-practices.md) için en iyi uygulamaları kullanın.

## <a name="resource-loops"></a>Kaynak döngüleri

Uygulamanız için birden fazla sanal makineye ihtiyacınız olduğunda, şablonda bir kopyalama öğesi kullanabilirsiniz. Bu isteğe bağlı öğe, parametre olarak belirttiğiniz VM sayısını oluşturarak döngüye başlar:

```json
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

Ayrıca, kaynak için bazı değerleri belirtirken döngü dizininin kullanıldığına dikkat edin. Örneğin, üç örnek sayısı girdiyseniz, işletim sistemi disklerinin adları myOSDisk1, myOSDisk2 ve myOSDisk3'tür:

```json
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>Bu örnekte, sanal makineler için yönetilen diskler kullanır.
>
>

Şablondaki bir kaynak için döngü oluşturmanın, diğer kaynakları oluştururken veya erişirken döngüyü kullanmanızı gerektirebileceğini unutmayın. Örneğin, birden çok VM aynı ağ arabirimini kullanamaz, bu nedenle şablonunuz üç VM oluşturarak döngü oluşturuyorsa, üç ağ arabirimi oluşturarak da döngü yemesi gerekir. Bir VM'ye ağ arabirimi atarken, döngü dizini tanımlamak için kullanılır:

```json
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Bağımlılıklar

Kaynakların çoğu, doğru çalışması için diğer kaynaklara bağlıdır. Sanal makineler bir sanal ağ ile ilişkili olmalı ve bunu yapmak için bir ağ arabirimi gerekir. [DependsOn](../../resource-group-define-dependencies.md) öğesi, VM'ler oluşturulmadan önce ağ arabiriminin kullanılmaya hazır olduğundan emin olmak için kullanılır:

```json
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Kaynak Yöneticisi, dağıtılan başka bir kaynağa bağımlı olmayan tüm kaynakları paralel olarak dağıtır. Gereksiz bağımlılıklar belirterek yanlışlıkla dağıtımınızı yavaşlatabileceğiniziçin bağımlılıkları ayarlarken dikkatli olun. Bağımlılıklar birden çok kaynak üzerinden zincirlenebilir. Örneğin, ağ arabirimi ortak IP adresine ve sanal ağ kaynaklarına bağlıdır.

Bir bağımlılık gerekli olup olmadığını nasıl anlarsınız? Şablonda belirlediğiniz değerlere bakın. Sanal makine kaynak tanımındaki bir öğe, aynı şablonda dağıtılan başka bir kaynağa işaret ederse, bir bağımlılık gerekir. Örneğin, örnek sanal makine bir ağ profili tanımlar:

```json
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

Bu özelliği ayarlamak için ağ arabiriminin bulunması gerekir. Bu nedenle, bir bağımlılık gerekir. Ayrıca, bir kaynak (bir alt bölüm) başka bir kaynak (üst öğe) içinde tanımlandığında bir bağımlılık ayarlamanız gerekir. Örneğin, tanılama ayarları ve özel komut dosyası uzantıları nın her ikisi de sanal makinenin alt kaynakları olarak tanımlanır. Sanal makine var olana kadar oluşturulamaz. Bu nedenle, her iki kaynak da sanal makineye bağımlı olarak işaretlenir.

## <a name="profiles"></a>Profiller

Sanal makine kaynağı tanımlanırken çeşitli profil öğeleri kullanılır. Bazıları gereklidir ve bazıları isteğe bağlıdır. Örneğin, donanımProfil, osProfile, storageProfil ve ağProfil öğeleri gereklidir, ancak tanılamaProfil isteğe bağlıdır. Bu profiller şu gibi ayarları tanımlar:
   
- [Boyutu](sizes.md)
- [ad](/azure/architecture/best-practices/resource-naming) ve kimlik bilgileri
- disk ve [işletim sistemi ayarları](cli-ps-findimage.md)
- [ağ arabirimi](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md) 
- önyükleme tanılama

## <a name="disks-and-images"></a>Diskler ve görüntüler
   
Azure'da vhd dosyaları [diskleri veya görüntüleri](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)temsil edebilir. Bir vhd dosyasındaki işletim sistemi belirli bir VM olarak özelleştirilmişse, disk olarak adlandırılır. Bir vhd dosyasındaki işletim sistemi, birçok VM oluşturmak için genelleştirilmiş olduğunda, bu görüntü olarak adlandırılır.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Platform görüntüsünden yeni sanal makineler ve yeni diskler oluşturun

Bir VM oluşturduğunuzda, hangi işletim sistemini kullanacağınıza karar vermeniz gerekir. imageReference öğesi yeni bir VM işletim sistemini tanımlamak için kullanılır. Örnek, Windows Server işletim sisteminin tanımını gösterir:

```json
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Bir Linux işletim sistemi oluşturmak istiyorsanız, şu tanımı kullanabilirsiniz:

```json
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

Işletim sistemi diskinin yapılandırma ayarları osDisk öğesi ile atanır. Örnek, Önbelleğe alma modu **ReadWrite'a** ayarlanmış yeni bir yönetilen disk tanımlar ve disk bir [platform görüntüsünden](cli-ps-findimage.md)oluşturulur:

```json
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Mevcut yönetilen disklerden yeni sanal makineler oluşturma

Varolan disklerden sanal makineler oluşturmak istiyorsanız, imageReference ve osProfile öğelerini kaldırın ve bu disk ayarlarını tanımlayın:

```json
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Yönetilen bir görüntüden yeni sanal makineler oluşturma

Yönetilen bir görüntüden sanal bir makine oluşturmak istiyorsanız, imageReference öğesini değiştirin ve aşağıdaki disk ayarlarını tanımlayın:

```json
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a>Veri diskleri ekleme

İsteğe bağlı olarak VM'lere veri diskleri ekleyebilirsiniz. [Disk sayısı,](sizes.md) kullandığınız işletim sistemi diskinin boyutuna bağlıdır. Standard_DS1_v2 ayarlanan VM'lerin boyutuyla, bunlara eklenebilecek en fazla veri diski sayısı ikiolur. Örnekte, her VM'ye yönetilen bir veri diski ekleniyor:

```json
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a>Uzantılar

[Uzantılar](extensions-features.md) ayrı bir kaynak olsa da, VM'lere yakından bağlıdırlar. Uzantılar VM'nin alt kaynağı olarak veya ayrı bir kaynak olarak eklenebilir. Örnek, VM'lere eklenen [Tanılama Uzantısı'nı](extensions-diagnostics-template.md) gösterir:

```json
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

Bu uzantı kaynağı, değerleri sağlamak için storageName değişkenini ve tanılama değişkenlerini kullanır. Bu uzantı tarafından toplanan verileri değiştirmek istiyorsanız, wadperfcounters değişkenine daha fazla performans sayacı ekleyebilirsiniz. Ayrıca, tanılama verilerini VM disklerinin depolandığı yerden farklı bir depolama hesabına koymayı da seçebilirsiniz.

Bir VM yükleyebilirsiniz birçok uzantıları vardır, ancak en yararlı muhtemelen [Özel Komut Dosyası Uzantısı.](extensions-customscript.md) Örnekte, start.ps1 adlı bir PowerShell komut dosyası ilk başladığında her VM'de çalışır:

```json
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

Start.ps1 komut dosyası birçok yapılandırma görevini gerçekleştirebilir. Örneğin, örnekteki VM'lere eklenen veri diskleri baş harfe çevrilir; bunları başlatmak için özel bir komut dosyası kullanabilirsiniz. Yapmanız gereken birden çok başlangıç göreviniz varsa, Azure depolama alanında diğer PowerShell komut dosyalarını aramak için start.ps1 dosyasını kullanabilirsiniz. Örnek PowerShell kullanır, ancak kullandığınız işletim sisteminde kullanılabilen herhangi bir komut dosyası yöntemini kullanabilirsiniz.

Portaldaki Uzantılar ayarlarından yüklü uzantıların durumunu görebilirsiniz:

![Uzantı durumu alma](./media/template-description/virtual-machines-show-extensions.png)

Ayrıca **Get-AzVMExtension** PowerShell komutunu, **vm uzantısı** azure CLI komutunu veya Get extension information REST API'yi kullanarak **uzantı bilgilerini** de alabilirsiniz.

## <a name="deployments"></a>Dağıtımlar

Bir şablon dağıttığınızda, Azure grup olarak dağıttığınız kaynakları izler ve bu dağıtılan gruba otomatik olarak bir ad atar. DağıtımAdı şablonun adı ile aynıdır.

Dağıtımdaki kaynakların durumunu merak ediyorsanız, Azure portalındaki kaynak grubunu görüntüleyin:

![Dağıtım bilgilerini alın](./media/template-description/virtual-machines-deployment-info.png)
    
Kaynak oluşturmak veya varolan kaynakları güncelleştirmek için aynı şablonu kullanmak sorun değildir. Şablonları dağıtmak için komutları kullandığınızda, hangi [modu](../../resource-group-template-deploy.md) kullanmak istediğinizi söyleme fırsatınız olur. Mod **Tam** veya **Artımlı**olarak ayarlanabilir. Varsayılan değer, artımlı güncelleştirmeler yapmaktır. Kaynakları yanlışlıkla silebilirsiniz, çünkü **Tam** modunu kullanırken dikkatli olun. Modu **Tamamla**olarak ayarladığınızda, Kaynak Yöneticisi kaynak grubundaki şablonda olmayan tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Kaynak Yöneticisi şablonlarını yazma'yı](../../resource-group-authoring-templates.md)kullanarak kendi şablonunuzu oluşturun.
- [Kaynak Yöneticisi şablonuyla Windows sanal makine oluştur'u](ps-template.md)kullanarak oluşturduğunuz şablonu dağıtın.
- [Azure PowerShell modülü yle Windows VM'leri Oluştur'u gözden](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)geçirerek ve yöneterek oluşturduğunuz VM'leri nasıl yönetdiğinizi öğrenin.
- Şablonlarda json sözdizimi ve kaynak türlerinin özellikleri için Bkz. [Azure Kaynak Yöneticisi şablon başvurusu.](/azure/templates/)
