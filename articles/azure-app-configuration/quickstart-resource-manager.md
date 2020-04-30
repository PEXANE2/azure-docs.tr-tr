---
title: Azure Uygulama yapılandırması hızlı başlangıç ile otomatik VM dağıtımı
description: Bu hızlı başlangıçta, Azure uygulama yapılandırma deposu dağıtmak için Azure PowerShell modülünün ve Azure Resource Manager şablonlarının nasıl kullanılacağı gösterilmektedir. Ardından, bir VM dağıtmak için depodaki değerleri kullanın.
author: lisaguthrie
ms.author: lcozzens
ms.date: 04/14/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 96d09de73e8b904a8e26eb4f365d34fab1401203
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82137561"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template"></a>Hızlı başlangıç: uygulama yapılandırması ve Kaynak Yöneticisi şablonuyla otomatik VM dağıtımı

Azure PowerShell modülü, PowerShell cmdlet 'leri veya betikleri kullanılarak Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta, Azure uygulama yapılandırma deposu dağıtmak için Azure PowerShell ve Azure Resource Manager şablonlarının nasıl kullanılacağı gösterilmektedir. Daha sonra, bir VM 'yi dağıtmak için depodaki anahtar değerlerini nasıl kullanacağınızı öğrenirsiniz.

Uygulama yapılandırma deposu oluşturmak için önkoşul şablonunu kullanır ve ardından Azure portal veya Azure CLı kullanarak depoya anahtar değerleri ekleyebilirsiniz. Birincil şablon var olan bir yapılandırma deposundan mevcut anahtar-değer yapılandırmalarına başvurur. Alınan değerler, bu örnekteki bir VM gibi, şablon tarafından oluşturulan kaynakların özelliklerini ayarlamak için kullanılır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

* Bu hızlı başlangıç Azure PowerShell modülünü gerektirir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` Komutuyla Azure aboneliğinizde oturum açın ve açılır tarayıcıda Azure kimlik bilgilerinizi girin:

```azurepowershell-interactive
# Connect to your Azure account
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, aşağıdaki cmdlet 'leri çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. Aboneliğinizin adıyla değiştirmeyi `<your subscription name>` unutmayın:

```azurepowershell-interactive
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)Ile bir Azure Kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurepowershell-interactive
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="deploy-an-azure-app-configuration-store"></a>Azure uygulama yapılandırma deposu dağıtma

SANAL makineye anahtar değerleri uygulayabilmeniz için önce var olan bir Azure uygulama yapılandırma deposuna sahip olmanız gerekir. Bu bölümde, bir Azure Resource Manager şablonu kullanarak bir Azure uygulama yapılandırma deposunun nasıl dağıtılacağı anlatılmaktadır. Zaten bir uygulama yapılandırma depoluize sahipseniz bu makalenin sonraki bölümüne geçebilirsiniz. 

1. Aşağıdaki JSON kodunu kopyalayın ve *prereq. azuredeploy. JSON*adlı yeni bir dosyaya yapıştırın.

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

1. Aşağıdaki JSON kodunu kopyalayın ve *prereq. azuredeploy. Parameters. JSON*adlı yeni bir dosyaya yapıştırın. Yapılandırma deponuzda benzersiz bir ad ile **Get-UNIQUE** değerini değiştirin.

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

1. PowerShell pencerenizde, Azure uygulama yapılandırma deposunu dağıtmak için aşağıdaki komutu çalıştırın. Kaynak grubu adını, şablon dosya yolunu ve şablon parametresi dosya yolunu değiştirmeyi unutmayın.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>" `
       -TemplateFile "<path to prereq.azuredeploy.json>" `
       -TemplateParameterFile "<path to prereq.azuredeploy.parameters.json>"
   ```

## <a name="add-vm-configuration-key-values"></a>VM yapılandırması anahtar değerlerini ekleme

Bir Azure Resource Manager şablonu kullanarak bir uygulama yapılandırma deposu oluşturabilirsiniz, ancak Azure portal veya Azure CLı kullanarak anahtar değerleri eklemeniz gerekir. Bu hızlı başlangıçta Azure portal kullanarak anahtar değerleri eklersiniz.

1. Dağıtım tamamlandıktan sonra, [Azure Portal](https://portal.azure.com)yeni oluşturulan uygulama yapılandırma deposuna gidin.

1. **Ayarlar** > **erişim anahtarları**' nı seçin. Birincil salt okunurdur anahtar bağlantı dizesini bir yere unutmayın. Uygulamanızı oluşturduğunuz uygulama yapılandırma deposuyla iletişim kuracak şekilde yapılandırmak için bu bağlantı dizesini daha sonra kullanacaksınız.

1. Aşağıdaki anahtar-değer çiftlerini eklemek için yapılandırma **Gezgini** > **Oluştur** ' u seçin:

   |Anahtar|Değer|
   |-|-|
   |windowsOsVersion|2019-veri merkezi|
   |diskSizeGB|1023|
  
   **Etiket**için *şablon* girin, ancak **içerik türünü** boş tutun.

## <a name="deploy-vm-using-stored-key-values"></a>Depolanan anahtar değerlerini kullanarak VM 'yi dağıtma

Artık anahtar değerlerini depoya eklemişseniz, bir Azure Resource Manager şablonu kullanarak bir VM dağıtmaya hazırsınız demektir. Şablon, oluşturduğunuz **Windowsosversion** ve **disksizegb** anahtarlarına başvurur.

> [!WARNING]
> ARM şablonları, özel bağlantısı etkinleştirilmiş bir uygulama yapılandırma deposundaki anahtarlara başvuramaz.

1. Aşağıdaki JSON kodunu kopyalayın ve *azuredeploy. JSON*adlı yeni bir dosyaya yapıştırın veya [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.json)dosyayı indirin.

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

1. Aşağıdaki JSON kodunu kopyalayıp *azuredeploy. Parameters. JSON*adlı yeni bir dosyaya yapıştırın veya [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-app-configuration/azuredeploy.parameters.json)dosyayı indirin.

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
   |adminPassword|VM için yönetici parolası.|
   |appConfigStoreName|Azure uygulama yapılandırma deponuzın adı.|
   |appConfigStoreResourceGroup|Uygulama yapılandırma deponuzi içeren kaynak grubu.|
   |vmSkuKey|*windowsOSVersion*|
   |diskSizeKey|*diskSizeGB*|
   |adminUsername|VM için Yönetici Kullanıcı adı.|
   |storageAccountName|VM ile ilişkili bir depolama hesabı için benzersiz bir ad.|
   |Etkialanıadetiketi|Benzersiz bir etki alanı adı.|

1. PowerShell pencerenizde, VM 'yi dağıtmak için aşağıdaki komutu çalıştırın. Kaynak grubu adını, şablon dosya yolunu ve şablon parametresi dosya yolunu değiştirmeyi unutmayın.

   ```azurepowershell
   New-AzResourceGroupDeployment `
       -ResourceGroupName "<your resource group>"
       -TemplateFile "<path to azuredeploy.json>" `
       -TemplateParameterFile "<path to azuredeploy.parameters.json>"
   ```

Tebrikler! Azure Uygulama yapılandırması 'nda depolanan yapılandırmaları kullanarak bir VM dağıttık.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, uygulama yapılandırma deposunu, VM 'yi ve tüm ilgili kaynakları silin. Uygulama yapılandırma deposunu veya VM 'yi gelecekte kullanmayı planlıyorsanız, silme işlemini atlayabilirsiniz. Bu işi kullanmaya devam edemeyecekecekseniz, aşağıdaki cmdlet 'i çalıştırarak bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Uygulama yapılandırmasından Azure Resource Manager şablonu ve anahtar değerlerini kullanarak bir VM dağıttınız.

Azure Uygulama yapılandırması ile başka uygulamalar oluşturma hakkında bilgi edinmek için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure Uygulama yapılandırmasıyla ASP.NET Core uygulaması oluşturma](quickstart-aspnet-core-app.md)
