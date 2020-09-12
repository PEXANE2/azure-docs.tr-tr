---
title: Azure sanal makine görüntüsü doğrulaması-Azure Marketi
description: Ticari Market 'te bir sanal makine teklifini test etme ve gönderme hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 08/14/2020
ms.openlocfilehash: 2d19098ec82fe9361154d798b981341a86decf97
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89647816"
---
# <a name="azure-virtual-machine-image-validation"></a>Azure sanal makine görüntüsü doğrulaması

Bu makalede, en son Azure Marketi yayımlama gereksinimlerini karşıladığından emin olmak için ticari Market 'te bir sanal makine (VM) görüntüsünün nasıl test edileceğini ve gönderileceği açıklanır.

VM teklifinizi göndermeden önce şu adımları uygulayın:

- Genelleştirilmiş görüntünüzü kullanarak bir Azure VM dağıtın.
- Doğrulamaları çalıştırın.

## <a name="deploy-an-azure-vm-using-your-generalized-image"></a>Genelleştirilmiş görüntünüzü kullanarak bir Azure VM dağıtma

Bu bölümde, yeni bir Azure VM kaynağı oluşturmak için genelleştirilmiş bir sanal sabit disk (VHD) görüntüsünün nasıl dağıtılacağı açıklanmaktadır. Bu işlem için, sağlanan Azure Resource Manager şablonu ve Azure PowerShell betiği kullanacağız.

### <a name="prepare-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu hazırlama

Bu bölümde, Kullanıcı tarafından belirtilen bir sanal makine (VM) görüntüsünün nasıl oluşturulacağı ve dağıtılacağı açıklanmaktadır. Bunu, Azure tarafından dağıtılan bir sanal sabit diskten işletim sistemi ve veri diski VHD görüntüleri sağlayarak yapabilirsiniz. Bu adımlar, genelleştirilmiş VHD kullanarak VM 'yi dağıtır.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Genelleştirilmiş işletim sistemi VHD 'nizi ve veri diski VHD 'nizi Azure depolama hesabınıza yükleyin.
3. Giriş sayfasında, **kaynak oluştur**' u seçin, "şablon dağıtımı" ifadesini arayın ve **Oluştur**' u seçin.
4. **Düzenleyicide kendi şablonunuzu oluşturun**öğesini seçin.

    :::image type="content" source="media/vm/template-deployment.png" alt-text="Bir şablonun seçimini gösterir.":::

5. Aşağıdaki JSON şablonunu düzenleyiciye yapıştırın ve **Kaydet**' i seçin.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "String"
        },
        "userStorageContainerName": {
            "defaultValue": "vhds",
            "type": "String"
        },
        "dnsNameForPublicIP": {
            "type": "String"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "String"
        },
        "adminPassword": {
            "defaultValue": "",
            "type": "SecureString"
        },
        "osType": {
            "defaultValue": "windows",
            "allowedValues": [
                "windows",
                "linux"
            ],
            "type": "String"
        },
        "subscriptionId": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "vmSize": {
            "type": "String"
        },
        "publicIPAddressName": {
            "type": "String"
        },
        "vmName": {
            "type": "String"
        },
        "virtualNetworkName": {
            "type": "String"
        },
        "nicName": {
            "type": "String"
        },
        "vhdUrl": {
            "type": "String",
            "metadata": {
                "description": "VHD Url..."
            }
        }
    },
    "variables": {
        "addressPrefix": "10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix": "10.0.0.0/24",
        "subnet2Prefix": "10.0.1.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "hostDNSNameScriptArgument": "[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]",
        "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2015-06-15",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                }
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2015-06-15",
            "name": "[parameters('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnet1Name')]",
                        "properties": {
                            "addressPrefix": "[variables('subnet1Prefix')]"
                        }
                    },
                    {
                        "name": "[variables('subnet2Name')]",
                        "properties": {
                            "addressPrefix": "[variables('subnet2Prefix')]"
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2015-06-15",
            "name": "[parameters('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                            },
                            "subnet": {
                                "id": "[variables('subnet1Ref')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2015-06-15",
            "name": "[parameters('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[parameters('vmName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "osDisk": {
                        "name": "[concat(parameters('vmName'),'-osDisk')]",
                        "osType": "[parameters('osType')]",
                        "caching": "ReadWrite",
                        "image": {
                            "uri": "[parameters('vhdUrl')]"
                        },
                        "vhd": {
                            "uri": "[variables('osDiskVhdName')]"
                        },
                        "createOption": "FromImage"
                    }
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                        }
                    ]
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "apiVersion": "2015-06-15",
            "name": "[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "typeHandlerVersion": "1.4",
                "settings": {
                    "fileUris": [
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1",
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe",
                        "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd"
                    ],
                    "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]"
                }
            }
        }
    ]
}
```

<br>

6. Görüntülenmiş özel dağıtım özelliği sayfaları için parametre değerlerini belirtin.

| ResourceGroupName | Mevcut Azure Kaynak grubu adı. Genellikle, anahtar kasanız ile aynı RG 'yi kullanın. |
| --- | --- |
| TemplateFile | VHDtoImage.jsdosyanın tam yol adı. |
| userStorageAccountName | Depolama hesabının adı. |
| Dnsnameforpublicıp | Genel IP için DNS adı; küçük harfle yazılmalıdır. |
| subscriptionId | Azure abonelik tanımlayıcısı. |
| Konum | Kaynak grubunun standart Azure coğrafi konumu. |
| vmName | Sanal makinenin adı. |
| vhdUrl | Sanal sabit diskin Web adresi. |
| vmSize | Sanal makine örneğinin boyutu. |
| Publicıpaddressname | Genel IP adresinin adı. |
| virtualNetworkName | Sanal ağın adı. |
| nicName | Sanal ağ için ağ arabirim kartının adı. |
| adminUserName | Yönetici hesabının Kullanıcı adı. |
| adminPassword | Yönetici parolası. |
|

7. Bu değerleri sağlamadıktan sonra, **satın al**' ı seçin.

Azure, dağıtıma başlayacak. Belirtilen depolama hesabı yolunda belirtilen yönetilmeyen VHD 'ye sahip yeni bir VM oluşturur. Azure portal ilerlemeyi, portalın sol tarafındaki **sanal makineler** ' i seçerek izleyebilirsiniz. VM oluşturulduğunda, durum başlangıç durumundan çalışmaya başlayacak şekilde değişir.

#### <a name="for-generation-2-vm-deployment-use-this-template"></a>2. nesil VM dağıtımı için şu şablonu kullanın:

```JSON
{
   "$schema":"https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{
      "userStorageAccountName":{
         "type":"String"
      },
      "userStorageContainerName":{
         "type":"String",
         "defaultValue":"vhds-86350-720-f4efbbb2-611b-4cd7-ad1e-afdgfuadfluad"
      },
      "dnsNameForPublicIP":{
         "type":"String"
      },
      "adminUserName":{
         "defaultValue":"isv",
         "type":"String"
      },
      "adminPassword":{
         "type":"securestring",
         "defaultValue":"Certcare@86350"
      },
      "osType":{
         "type":"string",
         "defaultValue":"linux",
         "allowedValues":[
            "windows",
            "linux"
         ]
      },
      "subscriptionId":{
         "type":"string"
      },
      "location":{
         "type":"string"
      },
      "vmSize":{
         "type":"string"
      },
      "publicIPAddressName":{
         "type":"string"
      },
      "vmName":{
         "type":"string"
      },
      "vNetNewOrExisting":{
         "defaultValue":"existing",
         "allowedValues":[
            "new",
            "existing"
         ],
         "type":"String",
         "metadata":{
            "description":"Specify whether to create a new or existing virtual network for the VM."
         }
      },
      "virtualNetworkName":{
         "type":"String",
         "defaultValue":""
      },
      "SubnetName":{
         "defaultValue":"subnet-5",
         "type":"String"
      },
      "SubnetPrefix":{
         "defaultValue":"10.0.5.0/24",
         "type":"String"
      },
      "nicName":{
         "type":"string"
      },
      "vhdUrl":{
         "type":"string",
         "metadata":{
            "description":"VHD Url..."
         }
      },
      "Datadisk1":{
         "type":"string",
         "metadata":{
            "description":"datadisk1 Url..."
         }
      },
      "Datadisk2":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      },
      "Datadisk3":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      }
   },
   "variables":{
      "addressPrefix":"10.0.0.0/16",
      "subnet1Name":"[parameters('SubnetName')]",
      "subnet1Prefix":"[parameters('SubnetPrefix')]",
      "publicIPAddressType":"Static",
      "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
      "subnet1Ref":"[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
      "osDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]",
      "dataDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'datadisk')]",
      "imageName":"[concat(parameters('vmName'), '-image')]"
   },
   "resources":[
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/publicIPAddresses",
         "name":"[parameters('publicIPAddressName')]",
         "location":"[parameters('location')]",
         "properties":{
            "publicIPAllocationMethod":"[variables('publicIPAddressType')]",
            "dnsSettings":{
               "domainNameLabel":"[parameters('dnsNameForPublicIP')]"
            }
         }
      },
      {
         "type":"Microsoft.Compute/images",
         "apiVersion":"2019-12-01",
         "name":"[variables('imageName')]",
         "location":"[parameters('location')]",
         "properties":{
            "storageProfile":{
               "osDisk":{
                  "osType":"[parameters('osType')]",
                  "osState":"Generalized",
                  "blobUri":"[parameters('vhdUrl')]",
                  "storageAccountType":"Standard_LRS"
               },
               "dataDisks":[
                  {
                     "lun":0,
                     "blobUri":"[parameters('Datadisk1')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":1,
                     "blobUri":"[parameters('Datadisk2')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":2,
                     "blobUri":"[parameters('Datadisk3')]",
                     "storageAccountType":"Standard_LRS"
                  }
               ]
            },
            "hyperVGeneration":"V2"
         }
      },
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/virtualNetworks",
         "name":"[parameters('virtualNetworkName')]",
         "location":"[parameters('location')]",
         "properties":{
            "addressSpace":{
               "addressPrefixes":[
                  "[variables('addressPrefix')]"
               ]
            },
            "subnets":[
               {
                  "name":"[variables('subnet1Name')]",
                  "properties":{
                     "addressPrefix":"[variables('subnet1Prefix')]"
                  }
               }
            ]
         },
         "condition":"[equals(parameters('vNetNewOrExisting'), 'new')]"
      },
      {
         "apiVersion":"2016-09-01",
         "type":"Microsoft.Network/networkInterfaces",
         "name":"[parameters('nicName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
         ],
         "properties":{
            "ipConfigurations":[
               {
                  "name":"ipconfig1",
                  "properties":{
                     "privateIPAllocationMethod":"Dynamic",
                     "publicIPAddress":{
                        "id":"[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                     },
                     "subnet":{
                        "id":"[variables('subnet1Ref')]"
                     }
                  }
               }
            ]
         }
      },
      {
         "apiVersion":"2019-12-01",
         "type":"Microsoft.Compute/virtualMachines",
         "name":"[parameters('vmName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]",
            "[variables('imageName')]"
         ],
         "properties":{
            "hardwareProfile":{
               "vmSize":"[parameters('vmSize')]"
            },
            "osProfile":{
               "computername":"[parameters('vmName')]",
               "adminUsername":"[parameters('adminUsername')]",
               "adminPassword":"[parameters('adminPassword')]"
            },
            "storageProfile":{
               "imageReference":{
                  "id":"[resourceId('Microsoft.Compute/images', variables('imageName'))]"
               },
               "dataDisks":[
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk0')]",
                     "lun":0,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk1')]",
                     "lun":1,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk2')]",
                     "lun":2,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  }
               ],
               "osDisk":{
                  "name":"[concat(parameters('vmName'),'-OSDisk')]",
                  "createOption":"FromImage",
                  "managedDisk":{
                     "storageAccountType":"Standard_LRS"
                  }
               }
            },
            "networkProfile":{
               "networkInterfaces":[
                  {
                     "id":"[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                  }
               ]
            }
         }
      }
   ]
}
```

### <a name="deploy-an-azure-vm-using-powershell"></a>PowerShell kullanarak bir Azure VM dağıtma

Ve değişkenleri için değerler sağlamak üzere aşağıdaki betiği kopyalayın ve düzenleyin `$storageaccount` `$vhdUrl` . Mevcut Genelleştirilmiş VHD 'nizden bir Azure VM kaynağı oluşturmak için yürütün.

```PowerShell
# storage account of existing generalized VHD

$storageaccount = "testwinrm11815" # generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl
$objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD

New-AzResourceGroupDeployment -Name"dplisvvm$postfix" -ResourceGroupName"$rgName" -TemplateFile"C:\certLocation\VHDtoImage.json" - userStorageAccountName"$storageaccount" -dnsNameForPublicIP"$vmName" -subscriptionId"$mysubid" -location"$location" - vmName"$vmName" -vaultName"$kvname" -vaultResourceGroup"$rgName" -certificateUrl$objAzureKeyVaultSecret.Id -vhdUrl"$vhdUrl" - vmSize"Standard\_A2" -publicIPAddressName"myPublicIP1" -virtualNetworkName"myVNET1" -nicName"myNIC1" -adminUserName"isv" - adminPassword$pwd
```

## <a name="run-validations"></a>Doğrulamaları Çalıştır

Dağıtılan görüntüde doğrulamaları çalıştırmanın iki yolu vardır.

### <a name="use-certification-test-tool-for-azure-certified"></a>Azure Sertifikalı sertifika sınama aracını kullanma

#### <a name="download-and-run-the-certification-test-tool"></a>Sertifika test aracını indirme ve çalıştırma

Azure Sertifikalı sertifika test aracı yerel bir Windows makinesinde çalışır, ancak Azure tabanlı bir Windows veya Linux VM 'yi sınar. Kullanıcı VM Görüntünüzün Microsoft Azure birlikte kullanılabileceğini ve VHD 'nizi hazırlama konusunda rehberlik ve gereksinimlerin karşılandığını gösterir.

1. [Azure Sertifikalı en son sertifika test aracı](https://www.microsoft.com/download/details.aspx?id=44299)'nı indirip yükleyin.
2. Sertifika aracını açın ve ardından **Yeni test Başlat**' ı seçin.
3. Test bilgileri ekranında, test çalıştırması için bir **Test adı** girin.
4. **Windows Server** veya **Linux**VM 'niz için platformu seçin. Platform seçiminiz, kalan seçenekleri etkiler.
5. VM 'niz bu veritabanı hizmetini kullanıyorsa **Azure SQL veritabanı Için test** onay kutusunu seçin.

#### <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracını bir VM görüntüsüne bağlama

1. SSH kimlik doğrulama modunu seçin: parola kimlik doğrulaması veya anahtar dosyası kimlik doğrulaması.
2. Parola tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS adı**, **Kullanıcı adı**ve **parola**değerlerini girin. Varsayılan SSH bağlantı noktası numarasını da değiştirebilirsiniz.

    :::image type="content" source="media/vm/azure-vm-cert-2.png" alt-text="VM test bilgilerinin seçimini gösterir.":::

3. Anahtar dosya tabanlı kimlik doğrulaması kullanıyorsanız, VM DNS adı, Kullanıcı adı ve özel anahtar konumu değerlerini girin. Ayrıca bir parola dahil edebilir veya varsayılan SSH bağlantı noktası numarasını değiştirebilirsiniz.
4. Tam VM DNS adını (örneğin, MyVMName.Cloudapp.net) girin.
5. **Kullanıcı adını** ve **parolayı**girin.

    :::image type="content" source="media/vm/azure-vm-cert-4.png" alt-text="VM Kullanıcı adı ve parolasının seçimini gösterir.":::

6. **İleri**’yi seçin.

#### <a name="run-a-certification-test"></a>Sertifika testi çalıştırma

VM Görüntünüz için sertifika aracında parametre değerlerini verdikten sonra, sanal makinenize geçerli bir bağlantı oluşturmak için Bağlantıyı Sına ' yı seçin. Bir bağlantı doğrulandıktan sonra, testi başlatmak için **İleri** ' yi seçin. Test tamamlandığında, sonuçlar bir tabloda görüntülenir. Durum sütununda her test için (başarılı/başarısız/uyarı) gösterilir. Testlerin herhangi biri başarısız olursa, görüntünüz sertifikalı değildir. Bu durumda, gereksinimleri ve hata iletilerini gözden geçirin, önerilen değişiklikleri yapın ve testi yeniden çalıştırın.

Otomatik test tamamlandıktan sonra, soru formu ekranının, Genel değerlendirme ve çekirdek özelleştirmenin iki sekmesinde VM Görüntünüz hakkında ek bilgiler sağlayın ve ardından Ileri ' yi seçin.

Son ekran, bir Linux VM görüntüsü için SSH erişim bilgileri gibi daha fazla bilgi ve özel durumlar arıyorsanız, başarısız olan değerlendirmelerin bir açıklamasını sağlar.

Son olarak, yürütülen test çalışmalarının test sonuçlarını ve günlük dosyalarını, soru formuna yönelik yanıtlarınızla birlikte indirmek için rapor oluştur ' u seçin. Sonuçları VHD 'larınız ile aynı kapsayıcıya kaydedin.

## <a name="how-to-use-powershell-to-consume-the-self-test-api"></a>Self-Test API 'sini kullanmak için PowerShell 'i kullanma

### <a name="on-linux-os"></a>Linux IŞLETIM sisteminde

PowerShell 'de API 'YI çağırın:

1. API 'YI çağırmak için Invoke-WebRequest komutunu kullanın.
2. Aşağıdaki kod örneğinde ve ekran yakalama bölümünde gösterildiği gibi, yöntemi, post ve içerik türü JSON olur.
3. JSON biçiminde gövde parametrelerini belirtin.

Aşağıdaki örnek, API 'ye yönelik bir PowerShell çağrısını göstermektedir:

```POWERSHELL
$accesstoken = “token”
$headers = New-Object “System.Collections.Generic.Dictionary[[String],[String]]”
$headers.Add(“Authorization”, “Bearer $accesstoken”)
$DNSName = “\&lt;\&lt;Machine DNS Name\&gt;\&gt;”
$UserName = “\&lt;\&lt;User ID\&gt;\&gt;”
$Password = “\&lt;\&lt;Password\&gt;\&gt;”
$OS = “Linux”
$PortNo = “22”
$CompanyName = “ABCD”
$AppID = “\&lt;\&lt;Application ID\&gt;\&gt;”
$TenantId = “\&lt;\&lt;Tenant ID\&gt;\&gt;”

$body =
@{
DNSName = $DNSName
UserName = $UserName
Password = $Password
OS = $OS
PortNo = $PortNo
CompanyName = $CompanyName
AppID = $AppID
TenantId = $TenantId
}| ConvertTo-Json

$body

$uri = “URL”

$res = (Invoke-WebRequest -Method “Post” -Uri $uri -Body $body -ContentType “application/json” -Headers $headers).Content
```

<br>PowerShell 'de API çağırma örneği aşağıda verilmiştir:

[![API 'yi PowerShell 'de çağırmaya yönelik ekran örneği.](media/vm/call-api-in-powershell.png)](media/vm/call-api-in-powershell.png#lightbox)

<br>Önceki örneği kullanarak, JSON 'u alabilir ve aşağıdaki ayrıntıları almak için ayrıştırın.

```PowerShell
$resVar=$res|ConvertFrom-Json

$actualresult =$resVar.Response |ConvertFrom-Json

Write-Host”OSName: $($actualresult.OSName)”Write-Host”OSVersion: $($actualresult.OSVersion)”Write-Host”Overall Test Result: $($actualresult.TestResult)”For ($i=0; $i -lt$actualresult.Tests.Length; $i++){ Write-Host”TestID: $($actualresult.Tests[$i].TestID)”Write-Host”TestCaseName: $($actualresult.Tests[$i].TestCaseName)”Write-Host”Description: $($actualresult.Tests[$i].Description)”Write-Host”Result: $($actualresult.Tests[$i].Result)”Write-Host”ActualValue: $($actualresult.Tests[$i].ActualValue)”}
```

<br>Bunu gösteren bu örnek ekran, `$res.Content` JSON biçiminde Test sonuçlarınızın ayrıntılarını gösterir:

[![Test sonuçlarının ayrıntıları ile PowerShell 'de API çağırma için ekran örneği.](media/vm/call-api-in-powershell-details.png)](media/vm/call-api-in-powershell-details.png#lightbox)

<br>Çevrimiçi bir JSON görüntüleyicisinde ( [Code Beautify](https://codebeautify.org/jsonviewer) veya [JSON Viewer](https://jsonformatter.org/json-viewer)gibi) görüntülenen JSON test sonuçlarının bir örneği aşağıda verilmiştir.

![Çevrimiçi bir JSON görüntüleyicisine daha fazla test sonucu.](media/vm/test-results-json-viewer-1.png)

### <a name="on-windows-os"></a>Windows işletim sistemi üzerinde

PowerShell 'de API 'YI çağırın:

1. API 'YI çağırmak için Invoke-WebRequest komutunu kullanın.
2. Aşağıdaki kod örneğinde ve örnek ekranda gösterildiği gibi, yöntemi, post ve içerik türü JSON olur.
3. JSON biçiminde gövde parametreleri oluşturun.

Bu kod örneği, API 'ye yönelik bir PowerShell çağrısını gösterir:

```PowerShell
$accesstoken = “Get token for your Client AAD App”$headers = New-Object”System.Collections.Generic.Dictionary[[String],[String]]”$headers.Add(“Authorization”, “Bearer $accesstoken”)$Body = @{ “DNSName” = “XXXX.westus.cloudapp.azure.com”“UserName” = “XXX”“Password” = “XXX@123456”“OS” = “Windows”“PortNo” = “5986”“CompanyName” = “ABCD” “AppID” = “XXXX-XXXX-XXXX” “TenantId” = “XXXX-XXXX-XXXX” } | ConvertTo-Json$res = Invoke-WebRequest -Method”Post” -Uri$uri -Body$Body -ContentType”application/json” –Headers $headers;$Content = $res | ConvertFrom-Json
```

Bu örnek ekranlar, PowerShell 'de API çağırma örneği gösterir:

**SSH anahtarıyla**:

 :::image type="content" source="media/vm/call-api-with-ssh-key.png" alt-text="PowerShell 'de API 'yi bir SSH anahtarıyla çağırma.":::

Şu **parolayla**:

 :::image type="content" source="media/vm/call-api-with-password.png" alt-text="PowerShell 'de API 'yi bir parolayla çağırma.":::

<br>Önceki örneği kullanarak, JSON 'u alabilir ve aşağıdaki ayrıntıları almak için ayrıştırın.

```PowerShell
$resVar=$res|ConvertFrom-Json

$actualresult =$resVar.Response |ConvertFrom-Json

Write-Host”OSName: $($actualresult.OSName)”Write-Host”OSVersion: $($actualresult.OSVersion)”Write-Host”Overall Test Result: $($actualresult.TestResult)”For ($i=0; $i -lt$actualresult.Tests.Length; $i++){ Write-Host”TestID: $($actualresult.Tests[$i].TestID)”Write-Host”TestCaseName: $($actualresult.Tests[$i].TestCaseName)”Write-Host”Description: $($actualresult.Tests[$i].Description)”Write-Host”Result: $($actualresult.Tests[$i].Result)”Write-Host”ActualValue: $($actualresult.Tests[$i].ActualValue)”}
```

<br>Bu ekran, `$res.Content` Test SONUÇLARıNıN JSON biçiminde ayrıntılarını gösterir:

 :::image type="content" source="media/vm/test-results-json-format.png" alt-text="JSON biçimindeki test sonuçlarının ayrıntıları.":::

<br>Çevrimiçi bir JSON görüntüleyicisinde ( [Code Beautify](https://codebeautify.org/jsonviewer) veya [JSON Viewer](https://jsonformatter.org/json-viewer)gibi) görüntülenen test sonuçlarının bir örneği aşağıda verilmiştir.

![Çevrimiçi bir JSON görüntüleyicisindeki sonuçları test edin.](media/vm/test-results-json-viewer-2.png)

## <a name="how-to-use-curl-to-consume-the-self-test-api-on-linux-os"></a>Linux IŞLETIM sisteminde kendi kendine test API 'sini kullanmak için KıVRıMLı kullanma

API 'YI KıVRıMLı olarak çağırın:

1. API 'YI çağırmak için kıvrımlı komutunu kullanın.
2. Aşağıdaki kod parçacığında gösterildiği gibi, yöntemi, post ve içerik türü JSON olur.

```JSON
CURL POST -H “Content-Type:application/json”

-H “Authorization: Bearer XXXXXX-Token-XXXXXXXX”

[https://isvapp.azure-api.net/selftest-vm](https://isvapp.azure-api.net/selftest-vm)

-d ‘{ “DNSName”:”XXXX.westus.cloudapp.azure.com”, “UserName”:”XXX”, “Password”:”XXXX@123456”, “OS”:”Linux”, “PortNo”:”22”, “CompanyName”:”ABCD”, “AppId”:”XXXX-XXXX-XXXX”, “TenantId “XXXX-XXXX-XXXX”}’
```

<br>API 'yi çağırmak için KıVRıMLı kullanmaya bir örnek aşağıda verilmiştir:

![API 'yi çağırmak için KıVRıMLı kullanma örneği.](media/vm/use-curl-call-api.png)

<br>Bu, KıVRıMLı çağrının JSON sonuçları:

![KıVRıMLı çağrının JSON sonuçları.](media/vm/test-results-json-viewer-3.png)

## <a name="next-step"></a>Sonraki adım

- [Ortak SAS URI sorunlarını ve düzeltmeleri](common-sas-uri-issues.md)okuyun.
