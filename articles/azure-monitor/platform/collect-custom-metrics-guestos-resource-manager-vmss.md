---
title: Şablonla Azure Monitor'da Windows ölçeği ayar ölçümleri toplama
description: Windows sanal makine ölçeği kümesi için Kaynak Yöneticisi şablonu kullanarak konuk işletim sistemi ölçümlerini Azure Monitor metrik deposuna gönderme
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 24f83e4f6285d045e67bdaef431ebcff2345ef84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663905"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-by-using-an-azure-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Windows sanal makine ölçeği kümesi için Azure Kaynak Yöneticisi şablonu kullanarak konuk işletim sistemi ölçümlerini Azure Monitor metrik deposuna gönderme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor [Windows Azure Tanılama (WAD) uzantısını](diagnostics-extension-overview.md)kullanarak, sanal makine, bulut hizmeti veya Azure Hizmet Dokusu kümesinin bir parçası olarak çalışan konuk işletim sisteminden (konuk işletim sistemi) ölçümlerve günlükler toplayabilirsiniz. Uzantı, telemetriyi daha önce bağlantılı makalede listelenen birçok farklı konuma gönderebilir.  

Bu makalede, Azure Monitor veri deposuna ayarlanan Windows sanal makine ölçeği için konuk işletim sistemi performans ölçümleri gönderme işlemi açıklanmaktadır. Windows Azure Diagnostics sürüm 1.11'den başlayarak, ölçümleri standart platform ölçümlerinin zaten toplandığı Azure Monitor ölçüm mağazasına doğrudan yazabilirsiniz. Bunları bu konumda depolayarak, platform ölçümleri için kullanılabilen aynı eylemlere erişebilirsiniz. Eylemler yakın gerçek zamanlı uyarı, grafik, yönlendirme, REST API erişim ve daha fazlasını içerir. Geçmişte, Windows Azure Tanılama uzantısı Azure Depolama'ya yazdı, ancak Azure Monitor veri deposuna yazmadı.  

Kaynak Yöneticisi şablonlarında yeniyseniz, şablon [dağıtımları](../../azure-resource-manager/management/overview.md) ve bunların yapısı ve sözdizimi hakkında bilgi edinin.  

## <a name="prerequisites"></a>Ön koşullar

- Aboneliğiniz [Microsoft.Insights'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services)kayıtlı olmalıdır. 

- [Azure PowerShell](/powershell/azure) yüklü olması gerekir, yoksa [Azure Cloud Shell'i](https://docs.microsoft.com/azure/cloud-shell/overview)kullanabilirsiniz. 

- VM kaynağınız özel [ölçümleri destekleyen](metrics-custom-overview.md#supported-regions)bir bölgede olmalıdır.

## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitörünü veri lavabosu olarak ayarlama 
Azure Tanılama uzantısı, ölçümleri ve günlükleri farklı konumlara yönlendirmek için **veri lavaboları** adı verilen bir özellik kullanır. Aşağıdaki adımlar, yeni Azure Monitor veri lavabosunu kullanarak bir VM dağıtmak için Kaynak Yöneticisi şablonu ve PowerShell'in nasıl kullanılacağını gösterir. 

## <a name="author-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu yazma 
Bu örnek için, genel kullanıma açık bir [örnek şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale)u kullanabilirsiniz:  

- **Azuredeploy.json,** sanal makine ölçeği kümesinin dağıtımı için önceden yapılandırılmış bir Kaynak Yöneticisi şablonudur.

- **Azuredeploy.parameters.json,** VM'niz için ayarlamak istediğiniz kullanıcı adı ve parola gibi bilgileri depolayan bir parametre dosyasıdır. Dağıtım sırasında Kaynak Yöneticisi şablonu bu dosyada ayarlanan parametreleri kullanır. 

Her iki dosyayı da yerel olarak indirin ve kaydedin. 

###  <a name="modify-azuredeployparametersjson"></a>azuredeploy.parameters.json'u değiştirin
**azuredeploy.parameters.json** dosyasını açın:  
 
- Dağıtmak istediğiniz bir **vmSKU** sağlayın. Biz Standard_D2_v3 öneririz. 
- Sanal makine ölçek kümeniz için istediğiniz **bir windowsOSVersion** belirtin. 2016-Datacenter'ı öneriyoruz. 
- **VmssName** özelliği ni kullanarak dağıtılacak sanal makine ölçeği ayar kaynağını adlandırın. Bir örnek **VMSS-WAD-TEST**olduğunu.    
- **InstanceCount** özelliğini kullanarak sanal makine ölçeğinde çalıştırmak istediğiniz VM sayısını belirtin.
- Sanal makine ölçeği kümesi için **adminUsername** ve **adminPassword** için değerleri girin. Bu parametreler, ölçek kümesindeki VM'lere uzaktan erişim için kullanılır. VM'nizin ele geçirilmesini **do not** önlemek için bu şablondakileri kullanmayın. Botlar, herkese açık GitHub depolarında kullanıcı adları ve parolalar için interneti tarar. Bu varsayılanlarla VM'leri sınama olasılıkları yüksektir. 


###  <a name="modify-azuredeployjson"></a>azuredeploy.json'u değiştirin
**azuredeploy.json** dosyasını açın. 

Kaynak Yöneticisi şablonunda depolama hesabı bilgilerini tutmak için bir değişken ekleyin. Tanılama config dosyasında belirtilen günlükler veya performans sayaçları hem Azure Monitor metrik deposuna hem de burada belirttiğiniz depolama hesabına yazılır: 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
```
 
Kaynaklar bölümündesanal makine ölçeği ayar tanımını bulun ve **yapılandırmaya kimlik** bölümünü ekleyin. Bu ek, Azure'un bir sistem kimliği atamasını sağlar. Bu adım, ölçek kümesindeki VM'lerin azure monitor'a kendileri hakkında konuk ölçümleri yayabileceğini de sağlar:  

```json
    { 
      "type": "Microsoft.Compute/virtualMachineScaleSets", 
      "name": "[variables('namingInfix')]", 
      "location": "[resourceGroup().location]", 
      "apiVersion": "2017-03-30", 
      //add these lines below
      "identity": { 
           "type": "systemAssigned" 
       }, 
       //end of lines to add
```

Sanal makine ölçeği ayar kaynağında **virtualMachineProfile** bölümünü bulun. Uzantıları yönetmek için **uzantıProfil** adlı yeni bir profil ekleyin.  


**Uzantıprofilinde,** **VMSS-WAD uzantılı** bölümünde gösterildiği gibi şablona yeni bir uzantı ekleyin.  Bu bölüm, yayılan ölçümlerin Azure Monitor tarafından kabul edilmesini sağlayan Azure kaynakları uzantısı için yönetilen kimliklerdir. **Ad** alanı herhangi bir ad içerebilir. 

MSI uzantısı aşağıdaki kod da sanal makine ölçeği kümesi kaynağına bir uzantı kaynağı olarak tanılama uzantısı ve yapılandırma ekler. Gerektiğinde performans sayaçları eklemekveya kaldırmak için çekinmeyin: 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed identities for Azure resources   
                { 
                 "name": "VMSS-WAD-extension", 
                 "properties": { 
                       "publisher": "Microsoft.ManagedIdentity", 
                       "type": "ManagedIdentityExtensionForWindows", 
                       "typeHandlerVersion": "1.0", 
                       "autoUpgradeMinorVersion": true, 
                       "settings": { 
                             "port": 50342 
                           }, 
                       "protectedSettings": {} 
                     } 
                                
            }, 
            // add diagnostic extension. (Remove this comment after pasting.)
            { 
              "name": "[concat('VMDiagnosticsVmExt','_vmNodeType0Name')]", 
              "properties": { 
                   "type": "IaaSDiagnostics", 
                   "autoUpgradeMinorVersion": true, 
                   "protectedSettings": { 
                        "storageAccountName": "[variables('storageAccountName')]", 
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')),'2015-05-01-preview').key1]", 
                        "storageAccountEndPoint": "https://core.windows.net/" 
                   }, 
                   "publisher": "Microsoft.Azure.Diagnostics", 
                   "settings": { 
                        "WadCfg": { 
                              "DiagnosticMonitorConfiguration": { 
                                   "overallQuotaInMB": "50000", 
                                   "PerformanceCounters": { 
                                       "scheduledTransferPeriod": "PT1M", 
                                       "sinks": "AzMonSink", 
                                       "PerformanceCounterConfiguration": [ 
                                          { 
              "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                                              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Available Bytes", 
              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Committed Bytes", 
              "sampleRate": "PT15S" 
           } 
                                       ] 
                                 }, 
                                 "EtwProviders": { 
                                       "EtwEventSourceProviderConfiguration": [ 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Actors", 
                                              "scheduledTransferKeywordFilter": "1", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                              "eventDestination": "ServiceFabricReliableActorEventTable" 
                                           } 
                                           }, 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Services", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricReliableServiceEventTable" 
                                              } 
                                           } 
                                     ], 
                                     "EtwManifestProviderConfiguration": [ 
                                           { 
                                              "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8", 
                                              "scheduledTransferLogLevelFilter": "Information", 
                                              "scheduledTransferKeywordFilter": "4611686018427387904", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricSystemEventTable" 
                                              } 
                                          } 
                                     ] 
                               } 
                               }, 
                               "SinksConfig": { 
                                     "Sink": [ 
                                          { 
                                              "name": "AzMonSink", 
                                              "AzureMonitor": {} 
                                          } 
                                      ] 
                               } 
                         }, 
                         "StorageAccount": "[variables('storageAccountName')]" 
                         }, 
                        "typeHandlerVersion": "1.11" 
                  } 
           } 
            ] 
          }
          }
      }
    },
    //end of added code plus a few brackets. Be sure that the number and type of brackets match properly when done. 
    {
      "type": "Microsoft.Insights/autoscaleSettings",
...
```


Doğru sırada oluşturulduğundan emin olmak için depolama hesabı için bir **bağlı duruma** göre ekleyin: 

```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
```

Şablonda zaten oluşturulmamadıysa bir depolama hesabı oluşturun: 

```json
"resources": [
// add this code    
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
    "accountType": "Standard_LRS"
    }
},
// end added code
{ 
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
```

Her iki dosyayı da kaydedin ve kapatın. 

## <a name="deploy-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu dağıtma 

> [!NOTE]  
> Azure Diagnostics uzantısı sürüm 1,5 veya daha yüksek bir sürümünü çalıştırıyor **ve** **otomatik YükseltmeMinorVersion:** özellik Kaynak Yöneticisi şablonunuzda **geçerli** olarak ayarlanmış olmalıdır. Azure, VM'yi başlattığında uygun uzantıyı yükler. Şablonunuzda bu ayarlar yoksa, bunları değiştirin ve şablonu yeniden dağıtın. 


Kaynak Yöneticisi şablonuna dağıtmak için Azure PowerShell'i kullanın:  

1. PowerShell’i başlatın. 
1. Azure'da oturum `Login-AzAccount`açarak oturum açın.
1. Kullanarak abonelik listenizi `Get-AzSubscription`alın.
1. Oluşturacağınız aboneliği ayarlayın veya sanal makineyi güncelleştirin: 

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Dağıtılmakta olan VM için yeni bir kaynak grubu oluşturun. Şu komutu çalıştırın: 

   ```powershell
    New-AzResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Özel ölçümler için etkin leştirilmiş bir Azure bölgesi kullanmayı unutmayın. Özel ölçümler [için etkin leştirilmiş](https://github.com/MicrosoftDocs/azure-docs-pr/pull/metrics-custom-overview.md#supported-regions)bir Azure bölgesi kullanmayı unutmayın.
 
1. VM dağıtmak için aşağıdaki komutları çalıştırın:  

   > [!NOTE]  
   > Varolan bir ölçek kümesini güncelleştirmek istiyorsanız, komutun sonuna **-Mod Artımlı** ekleyin. 
 
   ```powershell
   New-AzResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Dağıtımınız başarılı olduktan sonra, Azure portalında ayarlanan sanal makine ölçeğini bulmanız gerekir. Azure Monitor'a ölçümler yatmalıdır. 

   > [!NOTE]  
   > Seçili **vmSkuSize**etrafında hatalarla karşınıza çıkabilir. Bu durumda, **azuredeploy.json** dosyanıza geri dön ve **vmSkuSize** parametresinin varsayılan değerini güncelleştirin. **Standard_DS1_v2**denemenizi öneririz. 


## <a name="chart-your-metrics"></a>Ölçümlerinizi grafik 

1. Azure Portal’da oturum açın. 

1. Sol menüde **Monitör'ü**seçin. 

1. **Monitör** **sayfasında, Ölçümler'i**seçin. 

   ![Monitör - Ölçümler sayfası](media/collect-custom-metrics-guestos-resource-manager-vmss/metrics.png) 

1. Toplama süresini **Son 30 dakika**olarak değiştirin.  

1. Kaynak açılır menüsünde, oluşturduğunuz sanal makine ölçeği kümesini seçin.  

1. Ad boşlukları açılır menüsünde **azure.vm.windows.guest'i**seçin. 

1. Ölçümler açılır menüsünde, **Kullanımda\%Memory Committed Bytes'ı**seçin.  

Daha sonra, belirli bir VM için grafik veya ölçek kümesinde her VM çizim için bu metrik boyutları kullanmayı da seçebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.


