---
title: Azure Izleyici 'de şablon ile Windows VM ölçümleri toplama
description: Windows sanal makinesi için Kaynak Yöneticisi şablonu kullanarak Azure Izleyici ölçüm veritabanı deposuna Konuk işletim sistemi ölçümleri gönderme
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: bwren
ms.subservice: metrics
ms.openlocfilehash: 14079f42fd857495396a0c44fd3bdeaf4371ea5f
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650552"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-by-using-an-azure-resource-manager-template-for-a-windows-virtual-machine"></a>Windows sanal makinesi için bir Azure Resource Manager şablonu kullanarak Azure Izleyici ölçüm deposuna Konuk işletim sistemi ölçümleri gönderme
Azure sanal makinelerinin Konuk işletim sistemindeki performans verileri, diğer [Platform ölçümleri](../insights/monitor-azure-resource.md#monitoring-data)gibi otomatik olarak toplanmaz. Bir REST API neredeyse gerçek zamanlı uyarı, grafik oluşturma, Yönlendirme ve erişim de dahil olmak üzere Azure Izleyici ölçümlerinin tüm özellikleriyle kullanılabilmesi için, Azure Izleyici [Tanılama uzantısı](diagnostics-extension-overview.md) ' nı ölçüm veritabanına toplayın. Bu makalede, bir Windows sanal makinesi için bir Kaynak Yöneticisi şablonu kullanarak ölçüm veritabanına Konuk işletim sistemi performans ölçümleri gönderme işlemi açıklanır. 

> [!NOTE]
> Azure portal kullanarak Konuk işletim sistemi ölçümleri toplamak üzere tanılama uzantısını yapılandırma hakkında ayrıntılı bilgi için, bkz. [Windows Azure tanılama uzantısı 'nı (WAD) yüklemek ve yapılandırmak](diagnostics-extension-windows-install.md).


Kaynak Yöneticisi şablonlarına yeni başladıysanız, [şablon dağıtımları](../../azure-resource-manager/management/overview.md) ve bunların yapısı ve sözdizimi hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Aboneliğinizin [Microsoft. Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services)'a kayıtlı olması gerekir.

- [Azure PowerShell](/powershell/azure) veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklemiş olmanız gerekir.

- VM kaynağınızın [özel ölçümleri destekleyen bir bölgede](metrics-custom-overview.md#supported-regions)olması gerekir. 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Izleyiciyi bir veri havuzu olarak ayarlama
Azure Tanılama uzantısı, ölçümleri ve günlükleri farklı konumlara yönlendirmek için "veri havuzları" adlı bir özellik kullanır. Aşağıdaki adımlarda, yeni "Azure Izleyici" veri havuzunu kullanarak bir VM dağıtmak için Kaynak Yöneticisi şablonu ve PowerShell 'in nasıl kullanılacağı gösterilmektedir.

## <a name="author-resource-manager-template"></a>Kaynak Yöneticisi şablonu yaz
Bu örnekte, genel kullanıma açık bir örnek şablon kullanabilirsiniz. Başlangıç şablonları https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows .

- **Azuredeploy. JSON** , bir sanal makinenin dağıtımı için önceden yapılandırılmış bir kaynak yöneticisi şablonudur.

- **Azuredeploy. Parameters. JSON** , VM 'niz için ayarlamak istediğiniz Kullanıcı adı ve parola gibi bilgileri depolayan bir parametre dosyasıdır. Dağıtım sırasında Kaynak Yöneticisi şablonu bu dosyada ayarlanan parametreleri kullanır.

Her iki dosyayı da yerel olarak indirin ve kaydedin.

### <a name="modify-azuredeployparametersjson"></a>Azuredeploy. Parameters. JSON öğesini Değiştir
*Azuredeploy. Parameters. JSON* dosyasını açın

1. **AdminUserName** ve VM için **adminPassword** değerlerini girin. Bu parametreler VM 'ye uzaktan erişim için kullanılır. VM 'nizin ele geçirilmesini önlemek için bu şablondaki değerleri kullanmayın. Botlar, genel GitHub depolarında Kullanıcı adları ve parolalar için internet 'i tarar. Bu varsayılanlar ile VM 'Leri test etmek olasıdır.

1. VM için benzersiz bir DnsName oluşturun.

### <a name="modify-azuredeployjson"></a>Azuredeploy. JSON öğesini değiştirme

*Azuredeploy. JSON* dosyasını açın

**StorageAccountName** girdisinden sonra şablonun **değişkenler** bölümüne bir depolama hesabı kimliği ekleyin.

```json
// Find these lines.
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",

// Add this line directly below.
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
```

Bu Yönetilen Hizmet Kimliği (MSI) uzantısını **kaynaklar** bölümünün en üstündeki şablona ekleyin. Uzantı, Azure Izleyici 'nin yayılmakta olan ölçümleri kabul etmesini sağlar.

```json
//Find this code.
"resources": [
// Add this code directly below.
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'), '/', 'WADExtensionSetup')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    },
```

Azure 'un MSI uzantısına bir sistem kimliği atamasını sağlamak için **kimlik** yapılandırmasını VM kaynağına ekleyin. Bu adım, sanal makinenin kendisi hakkındaki Konuk ölçümlerini Azure Izleyici 'ye yaymasını sağlar.

```json
// Find this section
                "subnet": {
            "id": "[variables('subnetRef')]"
            }
        }
        }
    ]
    }
},
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    // add these 3 lines below
    "identity": {
    "type": "SystemAssigned"
    },
    //end of added lines
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
    "hardwareProfile": {
    ...
```

Windows sanal makinesinde tanılama uzantısını etkinleştirmek için aşağıdaki yapılandırmayı ekleyin. Basit Kaynak Yöneticisi tabanlı bir sanal makine için uzantı yapılandırmasını, sanal makine için kaynaklar dizisine ekleyebiliriz. &mdash;Bölümünün devamındaki "" azmonsink "satırı ve karşılık gelen" SinksConfig "satırı, &mdash; uzantının ölçümleri doğrudan Azure izleyici 'ye yaymalıdır. Gerektiğinde performans sayaçlarını ekleme veya kaldırma ücretsizdir.


```json
        "networkProfile": {
            "networkInterfaces": [
            {
                "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
            ]
        },
"diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]"
    }
}
},
//Start of section to add
"resources": [
{
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'), '/', 'Microsoft.Insights.VMDiagnosticsSettings')]",
            "apiVersion": "2017-12-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.12",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "WadCfg": {
                "DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "DiagnosticInfrastructureLogs": {
                    "scheduledTransferLogLevelFilter": "Error"
        },
                    "Directories": {
                    "scheduledTransferPeriod": "PT1M",
    "IISLogs": {
                        "containerName": "wad-iis-logfiles"
                    },
                    "FailedRequestLogs": {
                        "containerName": "wad-failedrequestlogs"
                    }
                    },
                    "PerformanceCounters": {
                    "scheduledTransferPeriod": "PT1M",
                    "sinks": "AzMonSink",
                    "PerformanceCounterConfiguration": [
                        {
                        "counterSpecifier": "\\Memory\\Available Bytes",
                        "sampleRate": "PT15S"
                        },
                        {
                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use",
                        "sampleRate": "PT15S"
                        },
                        {
                        "counterSpecifier": "\\Memory\\Committed Bytes",
                        "sampleRate": "PT15S"
                        }
                    ]
                    },
                    "WindowsEventLog": {
                    "scheduledTransferPeriod": "PT1M",
                    "DataSource": [
                        {
                        "name": "Application!*"
                        }
                    ]
                    },
                    "Logs": {
                    "scheduledTransferPeriod": "PT1M",
                    "scheduledTransferLogLevelFilter": "Error"
                    }
                },
                "SinksConfig": {
                    "Sink": [
                    {
                        "name" : "AzMonSink",
                        "AzureMonitor" : {}
                    }
                    ]
                }
                },
                "StorageAccount": "[variables('storageAccountName')]"
            },
            "protectedSettings": {
                "storageAccountName": "[variables('storageAccountName')]",
                "storageAccountKey": "[listKeys(variables('accountid'),'2015-06-15').key1]",
                "storageAccountEndPoint": "https://core.windows.net/"
            }
            }
        }
        ]
//End of section to add
```


Her iki dosyayı da kaydedin ve kapatın.


## <a name="deploy-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu dağıtma

> [!NOTE]
> Azure Tanılama uzantısı sürüm 1,5 veya üstünü çalıştırıyor olmanız ve Kaynak Yöneticisi şablonunuzda ' true ' **autoUpgradeMinorVersion**olarak ayarlanmış olan ' true ' olarak ayarlamanız gerekir. Ardından Azure, VM 'yi başlattığında uygun uzantıyı yükler. Şablonunuzda bu ayarlara sahip değilseniz, onları değiştirin ve şablonu yeniden dağıtın.


Kaynak Yöneticisi şablonu dağıtmak için Azure PowerShell faydalanır.

1. PowerShell’i başlatın.
1. Kullanarak Azure 'da oturum açın `Login-AzAccount` .
1. Kullanarak abonelik listenizi alın `Get-AzSubscription` .
1. ' De sanal makineyi oluşturmak/güncelleştirmek için kullandığınız aboneliği ayarlayın:

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>"
   ```
1. Dağıtılan VM için yeni bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

   ```powershell
    New-AzResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>"
   ```
   > [!NOTE]
   > [Özel ölçümler için etkin olan bir Azure bölgesi kullanmayı](metrics-custom-overview.md)unutmayın.

1. Kaynak Yöneticisi şablonunu kullanarak VM 'yi dağıtmak için aşağıdaki komutları çalıştırın.
   > [!NOTE]
   > Var olan bir VM 'yi güncelleştirmek isterseniz, aşağıdaki komutun sonuna kadar *artımlı bir mod* eklemeniz yeterlidir.

   ```powershell
   New-AzResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>"
   ```

1. Dağıtımınız başarılı olduktan sonra, sanal makinenin Azure portal olması gerekir ve ölçümleri Azure Izleyici 'ye yayın.

   > [!NOTE]
   > Seçili vmSkuSize etrafında hatalarla karşılaşabilirsiniz. Bu durumda, azuredeploy. JSON dosyanıza dönün ve vmSkuSize parametresinin varsayılan değerini güncelleştirin. Bu durumda, "Standard_DS1_v2" seçeneğini denemeyi öneririz.

## <a name="chart-your-metrics"></a>Ölçümlerinizi grafik yapın

1. Azure portalında oturum açın.

2. Sol taraftaki menüden **izleyici**' yi seçin.

3. Izleyici sayfasında **ölçümler**' i seçin.

   ![Ölçümler sayfası](media/collect-custom-metrics-guestos-resource-manager-vm/metrics.png)

4. Toplama süresini **son 30 dakika**olarak değiştirin.

5. Kaynak açılan menüsünde, oluşturduğunuz VM 'yi seçin. Şablonda adı değiştirmediyseniz, *SimpleWinVM2*olması gerekir.

6. Ad alanları açılan menüsünde **Azure. VM. Windows. Guest** ' i seçin.

7. Ölçümler açılan menüsünde, ** \% Kullanımdaki bellek kaydedilmiş bayt**' ı seçin.


## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

