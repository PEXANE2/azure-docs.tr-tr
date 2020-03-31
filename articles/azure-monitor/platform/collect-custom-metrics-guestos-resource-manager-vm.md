---
title: Şablonla Azure Monitor'da Windows VM ölçümlerini toplama
description: Windows sanal makinesi için Kaynak Yöneticisi şablonu kullanarak konuk işletim sistemi ölçümlerini Azure Monitor metrik deposuna gönderme
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: e747ca89912c36538bfb9d02986629fe57c5adcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657376"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Windows sanal makinesi için Kaynak Yöneticisi şablonu kullanarak Konuk İşletim Sistemi ölçümlerini Azure Monitor metrik deposuna gönderme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitör [Tanılama uzantısını](diagnostics-extension-overview.md)kullanarak, sanal makine, bulut hizmeti veya Service Fabric kümesinin bir parçası olarak çalışan konuk işletim sisteminden (Konuk İşletim Sistemi) ölçümleri ve günlükleri toplayabilirsiniz. Uzantı, telemetriyi [birçok farklı konuma gönderebilir.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede, Bir Windows sanal makine için Konuk İşletim Sistemi performans ölçümlerini Azure Monitor veri deposuna gönderme işlemi açıklanmaktadır. Diagnostics sürüm 1.11'den başlayarak, ölçümleri standart platform ölçümlerinin zaten toplandığı Azure Monitor ölçüm mağazasına doğrudan yazabilirsiniz.

Bunları bu konumda depolamak, platform ölçümleri için aynı eylemlere erişmenizi sağlar. Eylemler neredeyse gerçek zamanlı uyarı, grafik, yönlendirme ve REST API ve daha fazla erişim içerir. Geçmişte, Tanılama uzantısı Azure Depolama'ya yazdı, ancak Azure Monitor veri deposuna yazmadı.

Kaynak Yöneticisi şablonlarında yeniyseniz, şablon [dağıtımları](../../azure-resource-manager/management/overview.md) ve bunların yapısı ve sözdizimi hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

- Aboneliğiniz [Microsoft.Insights'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services)kayıtlı olmalıdır.

- [Azure PowerShell](/powershell/azure) veya Azure [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü olması gerekir.

- VM kaynağınız özel [ölçümleri destekleyen](metrics-custom-overview.md#supported-regions)bir bölgede olmalıdır. 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitörünü veri lavabosu olarak ayarlama
Azure Tanılama uzantısı, ölçümleri ve günlükleri farklı konumlara yönlendirmek için "veri lavaboları" adı verilen bir özellik kullanır. Aşağıdaki adımlar, yeni "Azure Monitor" veri lavabosunu kullanarak bir VM dağıtmak için Kaynak Yöneticisi şablonu ve PowerShell'in nasıl kullanılacağını gösterir.

## <a name="author-resource-manager-template"></a>Yazar Kaynak Yöneticisi şablonu
Bu örnekte, herkese açık bir örnek şablon u kullanabilirsiniz. Başlangıç şablonları ' https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windowsda'

- **Azuredeploy.json,** sanal bir makinenin dağıtımı için önceden yapılandırılmış bir Kaynak Yöneticisi şablonudur.

- **Azuredeploy.parameters.json,** VM'niz için hangi kullanıcı adı ve parola gibi bilgileri depolayan bir parametre dosyasıdır. Dağıtım sırasında Kaynak Yöneticisi şablonu bu dosyada ayarlanan parametreleri kullanır.

Her iki dosyayı da yerel olarak indirin ve kaydedin.

### <a name="modify-azuredeployparametersjson"></a>azuredeploy.parameters.json'u değiştirin
*azuredeploy.parameters.json* dosyasını aç

1. VM için **adminUsername** ve **adminPassword** için değerleri girin. Bu parametreler VM'ye uzaktan erişim için kullanılır. VM'nizin ele geçirilmesini önlemek için bu şablondaki değerleri KULLANMAYIN. Botlar, herkese açık GitHub depolarında kullanıcı adları ve parolalar için interneti tarar. Bu varsayılanlarla VM'leri sınama olasılıkları yüksektir.

1. VM için benzersiz bir dnsname oluşturun.

### <a name="modify-azuredeployjson"></a>azuredeploy.json'u değiştirin

*azuredeploy.json* dosyasını açma

**DepolamaHesap Adı** girişinden sonra şablonun **değişkenler** bölümüne bir depolama hesabı kimliği ekleyin.

```json
// Find these lines.
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",

// Add this line directly below.
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
```

Bu Yönetilen Hizmet Kimliği (MSI) uzantısını **kaynaklar** bölümünün üst kısmındaki şablona ekleyin. Uzantı, Azure Monitor'un yayılan ölçümleri kabul edilmesini sağlar.

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

Azure'un MSI uzantısına bir sistem kimliği atadığından emin olmak için **kimlik** yapılandırmasını VM kaynağına ekleyin. Bu adım, VM'nin kendisi hakkında konuk ölçümleri Azure Monitor'a yatmasını sağlar.

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

Windows sanal makinesinde Tanılama uzantısını etkinleştirmek için aşağıdaki yapılandırmayı ekleyin. Basit bir Kaynak Yöneticisi tabanlı sanal makine için, uzantı yapılandırmasını sanal makinenin kaynak dizisine ekleyebiliriz. "Lavabolar"&mdash; "AzMonSink" ve ilgili "SinksConfig" bölümü&mdash;daha sonra uzantısı doğrudan Azure Monitor ölçümleri yatmak için etkinleştirmek. Gerektiğinde performans sayaçları eklemekveya kaldırmak için çekinmeyin.


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
> Azure Diagnostics uzantısı sürüm 1.5 veya daha yüksek sürümü çalıştırıyor ve **otomatik YükseltmeMinorVersion**sahip olmalıdır: özellik Kaynak Yöneticisi şablonunuzda 'true' olarak ayarlanmış. Azure, VM'yi başlattığında uygun uzantıyı yükler. Şablonunuzda bu ayarlar yoksa, bunları değiştirin ve şablonu yeniden dağıtın.


Kaynak Yöneticisi şablonunu dağıtmak için Azure PowerShell'den yararlanıyoruz.

1. PowerShell’i başlatın.
1. Azure'da oturum `Login-AzAccount`açarak oturum açın.
1. Kullanarak abonelik listenizi `Get-AzSubscription`alın.
1. Sanal makineyi oluşturmak/güncellemek için kullandığınız aboneliği ayarlayın:

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>"
   ```
1. Dağıtılmakta olan VM için yeni bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

   ```powershell
    New-AzResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>"
   ```
   > [!NOTE]
   > Özel [ölçümler için etkin leştirilmiş bir Azure bölgesi kullanmayı](metrics-custom-overview.md)unutmayın.

1. Kaynak Yöneticisi şablonunu kullanarak VM'yi dağıtmak için aşağıdaki komutları çalıştırın.
   > [!NOTE]
   > Varolan bir VM'yi güncelleştirmek istiyorsanız, aşağıdaki komutun sonuna *-Mod Artımlı'yı* eklemeniz yeterlidir.

   ```powershell
   New-AzResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>"
   ```

1. Dağıtımınız başarılı olduktan sonra, VM Azure portalında olmalı ve Azure Monitor'a ölçümler yayMalıdır.

   > [!NOTE]
   > Seçili vmSkuSize etrafında hatalarla karşınıza çıkabilir. Bu durumda azuredeploy.json dosyanıza geri dön ve vmSkuSize parametresinin varsayılan değerini güncelleştirin. Bu durumda, "Standard_DS1_v2" denemenizi öneririz.

## <a name="chart-your-metrics"></a>Ölçümlerinizi grafik

1. Azure portalında oturum açın.

2. Sol menüde **Monitör'ü**seçin.

3. Monitör **sayfasında, Ölçümler'i**seçin.

   ![Ölçümler sayfası](media/collect-custom-metrics-guestos-resource-manager-vm/metrics.png)

4. Toplama süresini **Son 30 dakika**olarak değiştirin.

5. Kaynak açılır menüsünde, oluşturduğunuz VM'yi seçin. Şablondaki adı değiştirmediyseniz, *SimpleWinVM2*olmalıdır.

6. Ad boşlukları açılır menüsünde **azure.vm.windows.guest'i** seçin

7. Ölçümler açılır menüsünde, **Kullanımda\%Memory Committed Bytes'ı**seçin.


## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

