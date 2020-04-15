---
title: Azure Uygulama Yapılandırması ile otomatik VM dağıtımı hızlı başlat
description: Bu hızlı başlangıç, Azure Uygulama Yapılandırma deposunu dağıtmak için Azure PowerShell modülü ve Azure Kaynak Yöneticisi şablonlarının nasıl kullanılacağını gösterir. Ardından, bir VM dağıtmak için mağazadaki değerleri kullanın.
author: lisaguthrie
ms.author: lcozzens
ms.date: 04/14/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 02afa2cb36323e0c3c38c2451b1924b636f7faed
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309106"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template"></a>Quickstart: Uygulama Yapılandırması ve Kaynak Yöneticisi şablonu yla otomatik VM dağıtımı

Azure PowerShell modülü, PowerShell cmdlets veya komut dosyalarını kullanarak Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, bir Azure Uygulama Yapılandırma deposudağıtmak için Azure PowerShell ve Azure Kaynak Yöneticisi şablonlarını nasıl kullanacağınızı gösterir. Ardından, bir VM dağıtmak için mağazadaki anahtar değerlerini nasıl kullanacağınızı öğrenirsiniz.

Bir Uygulama Yapılandırma deposu oluşturmak için ön koşul şablonunu kullanır ve ardından Azure portalını veya Azure CLI'yi kullanarak mağazaya anahtar değerler eklersiniz. Birincil şablon, varolan bir yapılandırma deposundan varolan anahtar değer yapılandırmalarına başvurur. Alınan değerler, bu örnekte bir VM gibi şablon tarafından oluşturulan kaynakların özelliklerini ayarlamak için kullanılır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap oluşturun.](https://azure.microsoft.com/free/)

* Bu hızlı başlatma için Azure PowerShell modülü gerekir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` Komutla Azure aboneliğinizde oturum açın ve açılan tarayıcıya Azure kimlik bilgilerinizi girin:

```azurepowershell-interactive
# Connect to your Azure account
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, aşağıdaki cmdlets'i çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. Aboneliğinizin adıyla `<your subscription name>` değiştirmeyi unutmayın:

```azurepowershell-interactive
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurepowershell-interactive
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="deploy-an-azure-app-configuration-store"></a>Azure Uygulama Yapılandırma mağazalarını dağıtma

Anahtar değerlerini VM'ye uygulayabilmek için, varolan bir Azure Uygulama Yapılandırma mağazanız olması gerekir. Bu bölümde, Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Uygulama Yapılandırma deposunun nasıl dağıtılancaya kadar dağıtılanın. Zaten bir uygulama config deposu varsa, bu makalenin bir sonraki bölümüne taşıyabilirsiniz. 

1. Aşağıdaki json kodunu *prereq.azuredeploy.json*adlı yeni bir dosyaya kopyalayıp yapıştırın.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "type": "string",
        "metadata": {
          "description": "Specifies the name of the app configuration store."
        }
      },
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "Specifies the Azure location where the app configuration store should be created."
        }
      },
      "skuName": {
        "type": "string",
        "defaultValue": "standard",
        "metadata": {
          "description": "Specifies the SKU of the app configuration store."
        }
      }
    },
    "resources": [
      {
        "type": "Microsoft.AppConfiguration/configurationStores",
        "name": "[parameters('configStoreName')]",
        "apiVersion": "2019-10-01",
        "location": "[parameters('location')]",
        "sku": {
          "name": "[parameters('skuName')]"
        }
      }
    ]
   }
   ```

1. Aşağıdaki json kodunu *prereq.azuredeploy.parameters.json*adlı yeni bir dosyaya kopyalayıp yapıştırın. **GET-UNIQUE'i** Configuration Store'unuz için benzersiz bir adla değiştirin.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "configStoreName": {
        "value": "GET-UNIQUE"
      }
    }
   }
   ```

1. PowerShell pencerenizde, Azure Uygulama Yapılandırma mağazasını dağıtmak için aşağıdaki komutu çalıştırın. Kaynak grubu adını, şablon dosya yolunu ve şablon parametre dosya yolunu değiştirmeyi unutmayın.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>" 
       -TemplateFile "<path to prereq.azuredeploy.json>" `
       -TemplateParameterFile "<path to prereq.azuredeploy.parameters.json>"
   ```

## <a name="add-vm-configuration-key-values"></a>VM yapılandırma anahtar değerleri ekleme

Azure Kaynak Yöneticisi şablonu kullanarak bir Uygulama Yapılandırma mağazası oluşturabilirsiniz, ancak Azure portalını veya Azure CLI'yi kullanarak anahtar değerler eklemeniz gerekir. Bu hızlı başlangıçta, Azure portalını kullanarak anahtar değerleri eklersiniz.

1. Dağıtım tamamlandıktan sonra Azure [portalında](https://portal.azure.com)yeni oluşturulan Uygulama Yapılandırma mağazasına gidin.

1. **Ayarlar** > **Erişim Tuşları'nı**seçin. Birincil salt okunur anahtar bağlantı dizesini not edin. Bu bağlantı dizesini daha sonra, uygulamanızı oluşturduğunuz Uygulama Yapılandırma deposuyla iletişim kurmak üzere yapılandırmak için kullanırsınız.

1. Aşağıdaki anahtar değeri çiftleri eklemek için Configuration **Explorer** > **Create'i** seçin:

   |Anahtar|Değer|
   |-|-|
   |windowsOsVersion|2019-Datacenter|
   |diskSizeGB|1023|
  
   **Etiket**için *şablon* girin, ancak **İçerik Türünü** boş tutun.

## <a name="deploy-vm-using-stored-key-values"></a>Depolanan anahtar değerlerini kullanarak VM dağıtma

Mağazaya anahtar değerler eklediğinize göre, Azure Kaynak Yöneticisi şablonu kullanarak bir VM dağıtmaya hazırsınız. Şablon, oluşturduğunuz **windowsOsVersion** ve **diskSizeGB** tuşlarına başvurur.

> [!WARNING]
> ARM şablonları, Özel Bağlantı etkinleştirilmiş bir Uygulama Yapılandırma mağazasında ki tuşlara başvuru yapamaz.

1. Aşağıdaki json kodunu *azuredeploy.json*adlı yeni bir dosyaya kopyalayıp yapıştırın veya dosyayı [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.json)indirin.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "appConfigStoreName": {
            "type": "string",
            "metadata": {
                "description": "App configuration store name."
            }
        },
        "appConfigStoreResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Name of the resource group for the app config store."
            }
        },
        "domainNameLabel": {
            "type": "string",
            "metadata": {
                "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_D2_v3",
            "metadata": {
                "description": "Size of the VM"
            }
        },
        "vmSkuKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM windows sku"
            }
        },
        "diskSizeKey": {
            "type": "string",
            "metadata": {
                "description": "Name of the key in the app config store for the VM disk size"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "The name of the storage account."
            }
        }
    },
    "variables": {
        "nicName": "myVMNic",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressName": "myPublicIP",
        "vmName": "SimpleWinVM",
        "virtualNetworkName": "MyVNET",
        "subnetRef": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('virtualNetworkName'), variables('subnetName'))]",
        "appConfigRef": "[resourceId(parameters('appConfigStoreResourceGroup'), 'Microsoft.AppConfiguration/configurationStores', parameters('appConfigStoreName'))]",
        "windowsOSVersionParameters": {
            "key": "[parameters('vmSkuKey')]",
            "label": "template"
        },
        "diskSizeGBParameters": {
            "key": "[parameters('diskSizeKey')]",
            "label": "template"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-11-01",
            "name": "[parameters('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2018-11-01",
            "name": "[variables('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('domainNameLabel')]"
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2018-11-01",
            "name": "[variables('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2018-11-01",
            "name": "[variables('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2018-10-01",
            "name": "[variables('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computerName": "[variables('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('windowsOSVersionParameters')).value]",
                        "version": "latest"
                    },
                    "osDisk": {
                        "createOption": "FromImage"
                    },
                    "dataDisks": [
                        {
                            "diskSizeGB": "[listKeyValue(variables('appConfigRef'), '2019-10-01', variables('diskSizeGBParameters')).value]",
                            "lun": 0,
                            "createOption": "Empty"
                        }
                    ]
                },
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
                        "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))).primaryEndpoints.blob]"
                    }
                }
            }
        }
    ],
    "outputs": {
        "hostname": {
            "type": "string",
            "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
        }
    }
   }
   ```

1. Aşağıdaki json kodunu *azuredeploy.parameters.json*adlı yeni bir dosyaya kopyalayıp yapıştırın veya dosyayı [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.parameters.json)indirin.

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "adminPassword": {
        "value": "GEN-PASSWORD"
      },
      "appConfigStoreName":{
        "value": "GEN-APPCONFIGSTORE-NAME"
      },
      "appConfigStoreResourceGroup": {
         "value": "GEN-APPCONFIGSTORE-RESOURCEGROUP-NAME"
      },
      "vmSkuKey":{
        "value": "GEN-APPCONFIGSTORE-WINDOWSOSVERSION"
      },
      "diskSizeKey" :{
         "value": "GEN-APPCONFIGSTORE-DISKSIZEGB"
      },
      "adminUsername":{
        "value": "GEN-UNIQUE"
      },
      "storageAccountName":{
        "value": "GEN-UNIQUE"
      },
      "domainNameLabel":{
        "value": "GEN-UNIQUE"
      }
    }
   }
   ```

   Şablondaki parametre değerlerini aşağıdaki değerlerle değiştirin:

   |Parametre|Değer|
   |-|-|
   |adminPassword|VM için bir yönetici parolası.|
   |appConfigStoreName|Azure Uygulama Yapılandırma mağazanızın adı.|
   |appConfigStoreResourceGroup|Uygulama Yapılandırma mağazanızı içeren kaynak grubu.|
   |vmSkuKey|*windowsOSVersion*|
   |diskSizeKey|*diskSizeGB*|
   |adminUsername|VM için bir yönetici kullanıcı adı.|
   |storageAccountName|VM ile ilişkili bir depolama hesabı için benzersiz bir ad.|
   |domainNameLabel|Benzersiz bir etki alanı adı.|

1. PowerShell pencerenizde VM'yi dağıtmak için aşağıdaki komutu çalıştırın. Kaynak grubu adını, şablon dosya yolunu ve şablon parametre dosya yolunu değiştirmeyi unutmayın.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>"
       -TemplateFile "<path to azuredeploy.json>" `
       -TemplateParameterFile "<path to azuredeploy.parameters.json>"
   ```

Tebrikler! Azure Uygulama Yapılandırmasında depolanan yapılandırmaları kullanarak bir VM dağıttınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde kaynak grubunu, Uygulama Yapılandırma deposunu, VM'yi ve ilgili tüm kaynakları silin. Gelecekte App Configuration mağazasını veya VM'yi kullanmayı planlıyorsanız, silmeyi atlayabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz, aşağıdaki cmdlet'i çalıştırarak bu hızlı başlatmanın oluşturduğu tüm kaynakları silin:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Kaynak Yöneticisi şablonu ve Azure Uygulama Yapılandırması'ndan anahtar değerleri kullanarak bir VM dağıttınız.

Azure Uygulama Yapılandırması ile diğer uygulamaları oluşturma hakkında bilgi edinmek için aşağıdaki makaleye devam edin:

> [!div class="nextstepaction"]
> [Quickstart: Azure Uygulama Yapılandırması ile ASP.NET Core uygulaması oluşturun](quickstart-aspnet-core-app.md)
