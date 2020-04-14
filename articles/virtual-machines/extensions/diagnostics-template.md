---
title: Azure sanal makinesine izleme & tanılama ekleme
description: Azure tanılama uzantısı içeren yeni bir Windows sanal makinesi oluşturmak için Azure Kaynak Yöneticisi şablonu kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/31/2017
ms.author: mimckitt
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d100f054da5f82bc4dea51e054a28cca07f5de7b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258839"
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Windows VM ve Azure Kaynak Yöneticisi şablonlarıyla izleme ve tanılama kullanma
Azure TanıLama Uzantısı, Windows tabanlı bir Azure sanal makinede izleme ve tanılama özellikleri sağlar. Uzantıyı Azure Kaynak Yöneticisi şablonunun bir parçası olarak ekleyerek bu özellikleri sanal makinede etkinleştirebilirsiniz. Sanal makine şablonunun bir parçası olarak herhangi bir uzantı dahil etme hakkında daha fazla bilgi için [VM Uzantılı Azure Kaynak Yöneticisi Şablonları Yazma](../windows/template-description.md#extensions) bölümüne bakın. Bu makalede, windows sanal makine şablonuna Azure Tanılama uzantısını nasıl ekleyebileceğiniz açıklanmaktadır.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>VM kaynak tanımına Azure Tanılama uzantısını ekleme
Windows Virtual Machine'deki tanılama uzantısını etkinleştirmek için uzantıyı Kaynak Yöneticisi şablonuna VM kaynağı olarak eklemeniz gerekir.

Basit bir Kaynak Yöneticisi tabanlı Sanal Makine için, uzantı yapılandırmasını Sanal Makine için *kaynak* dizisine ekleyin: 

```json
"resources": [
    {
        "name": "Microsoft.Insights.VMDiagnosticsSettings",
        "type": "extensions",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-06-15",
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
                "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                "storageAccountEndPoint": "https://core.windows.net"
            }
        }
    }
]
```

Başka bir yaygın kuralı, uzantı yapılandırmasını sanal makinenin kaynak düğümü altında tanımlamak yerine şablonun kök kaynakları düğümüne eklemektir. Bu yaklaşımla, uzantı ile ad *ve* *tür* değerleri yle sanal makine arasında hiyerarşik bir ilişki açıkça belirtmeniz gerekir. Örneğin: 

```json
"name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
"type": "Microsoft.Compute/virtualMachines/extensions",
```

Uzantısı her zaman sanal makine ile ilişkilidir, ya doğrudan sanal makinenin kaynak düğümü altında doğrudan tanımlamak ya da temel düzeyde tanımlamak ve sanal makine ile ilişkilendirmek için hiyerarşik adlandırma kuralı nı kullanabilirsiniz.

Sanal Makine Ölçeği için ayarlar uzantıları yapılandırma *VirtualMachineProfile* *uzantısıProfil* özelliği belirtilir.

**Microsoft.Azure.Diagnostics** değerine ve **IaaSDiagnostics** değerine sahip *tür* özelliğine sahip *yayımcı* özelliği Azure Tanılama uzantısını benzersiz bir şekilde tanımlar.

*Ad* özelliğinin değeri kaynak grubundaki uzantıya başvurmak için kullanılabilir. Özellikle **Microsoft.Insights.VMDiagnosticsSettings** olarak ayarlanması, izleme grafiklerinin Azure portalında doğru şekilde gösterilmesini sağlayarak Azure portalı tarafından kolayca tanımlanmasını sağlar.

*TypeHandlerVersion,* kullanmak istediğiniz uzantının sürümünü belirtir. *autoUpgradeMinorVersion* küçük sürümünü **doğru** ayarlamak, kullanılabilir uzantının en son Minor sürümünü elde etmenizi sağlar. Her zaman tüm yeni özellikler ve hata düzeltmeleri ile en son kullanılabilir tanılama uzantısı kullanmak için olsun böylece her zaman **doğru** olması için *autoUpgradeMinorVersion* ayarlamak şiddetle tavsiye edilir. 

*Ayarlar* öğesi, uzantıdan ayarlanabilen ve geri okunabilen uzantı için yapılandırma özellikleri içerir (bazen genel yapılandırma olarak da adlandırılır). *Xmlcfg* özelliği tanılama girişleri, performans sayaçları vb tanılama aracısı tarafından toplanan xml tabanlı yapılandırma içerir. XML şema kendisi hakkında daha fazla bilgi için [Tanılama Yapılandırma Şeması'na](https://msdn.microsoft.com/library/azure/dn782207.aspx) bakın. Yaygın bir uygulama Azure Kaynak Yöneticisi şablonunda bir değişken olarak gerçek xml yapılandırmasını depolamak ve daha sonra *xmlcfg*değerini ayarlamak için bunları concatenate ve base64 kodlamaktır. Xml'in değişkenlerde nasıl depolayacağı hakkında daha fazla şey anlamak için [tanılama yapılandırma değişkenleri](#diagnostics-configuration-variables) bölümüne bakın. *StorageAccount* özelliği, tanılama verilerinin aktarıldığı depolama hesabının adını belirtir. 

*protectedSettings'teki* (bazen özel yapılandırma olarak da adlandırılır) özellikleri ayarlanabilir, ancak ayarlandıktan sonra geri okunamaz. *ProtectedSettings'in* yalnızca yazma özelliği, tanılama verilerinin yazıldığı depolama hesabı anahtarı gibi sırları depolamayı kullanışlı hale getirir.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Tanılama depolama hesabını parametreler olarak belirtme
Yukarıdaki tanılama uzantısı json snippet tanılama verilerinin depolandığı tanılama depolama hesabını belirtmek için iki paratanılama *varalamaStorageAccountName* ve *mevcut diagnosticsStorageResourceGroup* varsayar. Tanılama depolama hesabını bir parametre olarak belirtmek, tanılama depolama hesabını farklı ortamlarda değiştirmeyi kolaylaştırır, örneğin test için farklı bir tanılama depolama hesabı ve üretim dağıtımınız için farklı bir tanılama depolama hesabı kullanmak isteyebilirsiniz.  

```json
"existingdiagnosticsStorageAccountName": {
    "type": "string",
    "metadata": {
"description": "The name of an existing storage account to which diagnostics data is transfered."
    }
},
"existingdiagnosticsStorageResourceGroup": {
    "type": "string",
    "metadata": {
"description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
    }
}
```

Sanal makine için kaynak grubundan farklı bir kaynak grubunda tanılama depolama hesabı belirtmek en iyi yöntemdir. Bir kaynak grubu kendi ömrü ne kadar bir dağıtım birimi olarak kabul edilebilir, sanal bir makine dağıtılabilir ve yeni yapılandırmagüncelleştirmeleri yapıldıkça yeniden dağıtılabilir, ancak tanılama verilerini bu sanal makine dağıtımları arasında aynı depolama hesabında depolamaya devam etmek isteyebilirsiniz. Depolama hesabının farklı bir kaynakta olması, depolama hesabının çeşitli sanal makine dağıtımlarından gelen verileri kabul etmesini sağlayarak çeşitli sürümlerdeki sorunları gidermenizi kolaylaştırır.

> [!NOTE]
> Visual Studio'dan bir windows sanal makine şablonu oluşturursanız, varsayılan depolama hesabı sanal makine VHD'nin yüklendiği aynı depolama hesabını kullanacak şekilde ayarlanabilir. Bu, VM'nin ilk kurulumlarını basitleştirmek içindir. Şablonu parametre olarak geçirilebilen farklı bir depolama hesabı kullanmak için yeniden eşitleyin. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Tanılama yapılandırma değişkenleri
Önceki tanılama uzantısı json snippet tanılama depolama için depolama hesabı anahtarı alma kolaylaştırmak için bir *hesap değişkeni* tanımlar:   

```json
"accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"
```

Tanılama uzantısı için *xmlcfg* özelliği birlikte concatenated birden çok değişken kullanılarak tanımlanır. Bu değişkenlerin değerleri xml'de olduğundan json değişkenlerini ayarlarken doğru bir şekilde kaçmaları gerekir.

Aşağıdaki örnekte, bazı windows olay günlükleri ve tanılama altyapı günlükleri ile birlikte standart sistem düzeyi performans sayaçları toplayan tanılama yapılandırma xml açıklanır. Yapılandırmanın doğrudan şablonunuzun değişkenler bölümüne yapıştırılması için kaçtı ve doğru biçimlendirildi. Yapılandırma xml daha insan okunabilir bir örnek için [Tanılama Yapılandırma Şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) bakın.

```json
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
"wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
"wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"
```

Yukarıdaki yapılandırmadaki Metrics tanımı xml düğümü, *PerformanceCounter* düğümünde xml'de daha önce tanımlanan performans sayaçlarının nasıl toplandığını ve depolandığını tanımladığı için önemli bir yapılandırma öğesidir. 

> [!IMPORTANT]
> Bu ölçümler, Azure portalındaki izleme grafiklerini ve uyarıları yönlendiriyor.  Azure portalındaki VM izleme verilerini görmek *istiyorsanız, resourceID* ve **MetricAggregation** ile **Metrik** düğüm, VM'nizin tanılama yapılandırmasına dahil edilmelidir. 
> 
> 

Aşağıdaki örnek, metrik tanımları için xml'yi gösterir: 

```xml
<Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
    <MetricAggregation scheduledTransferPeriod="PT1H"/>
    <MetricAggregation scheduledTransferPeriod="PT1M"/>
</Metrics>
```

*ResourceID* özniteliği, aboneliğinizdeki sanal makineyi benzersiz olarak tanımlar. Şablonun, dağıtmakta olduğunuz abonelik ve kaynak grubuna bağlı olarak bu değerleri otomatik olarak güncellemesi için abonelik() ve resourceGroup() işlevlerini kullandığınızdan emin olun.

Bir döngü içinde birden çok Sanal Makine oluşturuyorsanız, her bir VM'yi doğru bir şekilde ayırt etmek için *resourceID* değerini bir copyIndex() işleviyle doldurmanız gerekir. *XmlCfg* değeri aşağıdaki gibi bu desteği güncellenebilir:  

```json
"xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 
```

*PT1M* ve *PT1H'nin* MetricAggregation değeri, sırasıyla bir dakika dan fazla bir toplama ve bir saat içinde bir toplama anlamına eder.

## <a name="wadmetrics-tables-in-storage"></a>WadMetrics tabloları depoda
Yukarıdaki Ölçümler yapılandırması, tanılama depolama hesabınızda aşağıdaki adlandırma kurallarıyla tablolar oluşturur:

* **WADMetrics**: Tüm WADMetrics tabloları için standart öneki
* **PT1H** veya **PT1M**: Tablonun 1 saat veya 1 dakikadan fazla toplam veri içerdiğini belirtir
* **P10D**: Tablonun veri toplamaya başladığı andan itibaren 10 gün boyunca veri içereceğini belirtir
* **V2S**: Dize sabiti
* **yyyymmdd**: Tablonun veri toplamaya başladığı tarih

Örnek: *WADMetricsPT1HP10DV2S20151108,* 11-Kasım-2015 tarihinden itibaren 10 gün boyunca bir saatten fazla toplanan metrik verileri içerir    

Her WADMetrics tablosu aşağıdaki sütunları içerir:

* **PartitionKey**: Bölüm anahtarı, VM kaynağını benzersiz olarak tanımlamak için *resourceID* değerine göre oluşturulur. Örneğin, `002Fsubscriptions:<subscriptionID>:002FresourceGroups:002F<ResourceGroupName>:002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>`  
* **RowKey**: Biçimini `<Descending time tick>:<Performance Counter Name>`izler. Azalan zaman işareti hesaplaması, toplama döneminin başlangıcının zamanı eksi, maksimum zaman keneleridir. Örneğin örnek dönem 10-Kasım-2015 ve 00:00Hrs UTC'de başlamışsa hesaplama şöyle olacaktır: `DateTime.MaxValue.Ticks - (new DateTime(2015,11,10,0,0,0,DateTimeKind.Utc).Ticks)`. Kullanılabilir bellek için bayt performans sayacı satır anahtarı aşağıdaki gibi görünecektir:`2519551871999999999__:005CMemory:005CAvailable:0020Bytes`
* **CounterName**: Performans sayacının adıdır. Bu, xml config'inde tanımlanan *karşıSpecifier* ile eşleşir.
* **Maksimum**: Toplama dönemi boyunca performans sayacının maksimum değeri.
* **Minimum**: Toplama dönemi içinde performans sayacının minimum değeri.
* **Toplam**: Toplama dönemi içinde bildirilen performans sayacının tüm değerlerinin toplamıdır.
* **Sayı**: Performans sayacı için bildirilen toplam değer sayısı.
* **Ortalama**: Performans sayacının toplama dönemi ndeki ortalama (toplam/sayım) değeri.

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama uzantılı bir Windows sanal makinesinin tam bir örnek şablonu için [bkz: 201-vm-monitoring-diagnostics-extension](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* [Azure PowerShell](../windows/ps-template.md) veya Azure Komut [Satırı'nı](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak Azure Kaynak Yöneticisi şablonunu dağıtma
* [Azure Kaynak Yöneticisi şablonları yazma](../../resource-group-authoring-templates.md) hakkında daha fazla bilgi edinin
