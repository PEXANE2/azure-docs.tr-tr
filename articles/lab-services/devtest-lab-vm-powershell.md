---
title: Azure PowerShell ile DevTest Labs'da sanal bir makine oluşturun
description: Azure PowerShell ile sanal makineler oluşturmak ve yönetmek için Azure DevTest Laboratuvarlarını nasıl kullanacağınızı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 13014c39641203bddadf858c34cff67462b3a4b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76167102"
---
# <a name="create-a-virtual-machine-with-devtest-labs-using-azure-powershell"></a>Azure PowerShell'i kullanarak DevTest Labs ile sanal bir makine oluşturun
Bu makalede, Azure PowerShell'i kullanarak Azure DevTest Labs'da sanal bir makine nin nasıl oluşturulabileceğiniz gösterilmektedir. Azure DevTest Labs'daki bir laboratuvarda sanal makinelerin oluşturulmasını otomatikleştirmek için PowerShell komut dosyalarını kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce:

- Bu makaledeki komut dosyasını veya komutları sınamak için varolan bir laboratuarı kullanmak istemiyorsanız [bir laboratuvar oluşturun.](devtest-lab-create-lab.md) 
- [Azure PowerShell'i yükleyin](/powershell/azure/install-az-ps?view=azps-1.7.0) veya Azure portalına entegre edilmiş Azure Bulut Shell'i kullanın. 

## <a name="powershell-script"></a>PowerShell betiği
Bu bölümdeki örnek komut dosyası [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) cmdlet kullanır.  Bu cmdlet, laboratuvarın kaynak kimliğini, gerçekleştirecek eylemin`createEnvironment`adını ( ) ve gerekli parametreleri alır. Parametreler, tüm sanal makine açıklama özelliklerini içeren karma tabloda bulunmaktadır. 

```powershell
[CmdletBinding()]

Param(
[Parameter(Mandatory = $false)]  $SubscriptionId,
[Parameter(Mandatory = $true)]   $LabResourceGroup,
[Parameter(Mandatory = $true)]   $LabName,
[Parameter(Mandatory = $true)]   $NewVmName,
[Parameter(Mandatory = $true)]   $UserName,
[Parameter(Mandatory = $true)]   $Password
)
 
pushd $PSScriptRoot

try {
    if ($SubscriptionId -eq $null) {
        $SubscriptionId = (Get-AzContext).Subscription.SubscriptionId
    }

    $API_VERSION = '2016-05-15'
    $lab = Get-AzResource -ResourceId "/subscriptions/$SubscriptionId/resourceGroups/$LabResourceGroup/providers/Microsoft.DevTestLab/labs/$LabName"

    if ($lab -eq $null) {
       throw "Unable to find lab $LabName resource group $LabResourceGroup in subscription $SubscriptionId."
    }

    #For this example, we are getting the first allowed subnet in the first virtual network
    #  for the lab.
    #If a specific virtual network is needed use | to find it. 
    #ie $virtualNetwork = @(Get-AzResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION) | Where-Object Name -EQ "SpecificVNetName"

    $virtualNetwork = @(Get-AzResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION)[0]

    $labSubnetName = $virtualNetwork.properties.allowedSubnets[0].labSubnetName

    #Prepare all the properties needed for the createEnvironment
    # call used to create the new VM.
    # The properties will be slightly different depending on the base of the vm
    # (a marketplace image, custom image or formula).
    # The setup of the virtual network to be used may also affect the properties.
    # This sample includes the properties to add an additional disk under dataDiskParameters
    
    $parameters = @{
       "name"      = $NewVmName;
       "location"  = $lab.Location;
       "properties" = @{
          "labVirtualNetworkId"     = $virtualNetwork.ResourceId;
          "labSubnetName"           = $labSubnetName;
          "notes"                   = "Windows Server 2016 Datacenter";
          "osType"                  = "windows"
          "expirationDate"          = "2019-12-01"
          "galleryImageReference"   = @{
             "offer"     = "WindowsServer";
             "publisher" = "MicrosoftWindowsServer";
             "sku"       = "2016-Datacenter";
             "osType"    = "Windows";
             "version"   = "latest"
          };
          "size"                    = "Standard_DS2_v2";
          "userName"                = $UserName;
          "password"                = $Password;
          "disallowPublicIpAddress" = $true;
          "dataDiskParameters" = @(@{
            "attachNewDataDiskOptions" = @{
                "diskName" = "adddatadisk"
                "diskSizeGiB" = "1023"
                "diskType" = "Standard"
                }
          "hostCaching" = "ReadWrite"
          })
       }
    }
    
    #The following line is the same as invoking
    # https://azure.github.io/projects/apis/#!/Labs/Labs_CreateEnvironment rest api

    Invoke-AzResourceAction -ResourceId $lab.ResourceId -Action 'createEnvironment' -Parameters $parameters -ApiVersion $API_VERSION -Force -Verbose
}
finally {
   popd
}
```

Yukarıdaki komut dosyasındaki sanal makinenin özellikleri, Windows Server 2016 DataCenter işletim sistemi olarak sanal bir makine oluşturmamıza olanak sağlar. Sanal makine her türü için, bu özellikleri biraz farklı olacaktır. [Sanal makineyi Tanımla](#define-virtual-machine) bölümü, bu komut dosyasında hangi özellikleri kullanacağınızı nasıl belirleyebildiğinizi gösterir.

Aşağıdaki komut, bir dosya adına kaydedilen komut dosyasını çalıştırmaya bir örnek sağlar: Create-LabVirtualMachine.ps1. 

```powershell
 PS> .\Create-LabVirtualMachine.ps1 -ResourceGroupName 'MyLabResourceGroup' -LabName 'MyLab' -userName 'AdminUser' -password 'Password1!' -VMName 'MyLabVM'
```

## <a name="define-virtual-machine"></a>Sanal makineyi tanımlama
Bu bölümde, oluşturmak istediğiniz bir sanal makine türüne özgü özellikleri nasıl elde edebilirsiniz. 

### <a name="use-azure-portal"></a>Azure portalı kullanma
Azure portalında Bir VM oluştururken bir Azure Kaynak Yöneticisi şablonu oluşturabilirsiniz. VM oluşturma işlemini tamamlamanız gerekmez. Yalnızca şablonu görene kadar adımları izlersiniz. Bu zaten oluşturulan bir laboratuvar VM yoksa gerekli JSON açıklamasını almak için en iyi yoldur. 

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol navigasyon menüsünden **Tüm Hizmetler'i** seçin.
3. Hizmetler listesinden **DevTest Laboratuvarlarını** arayın ve seçin. 
4. **DevTest Labs** sayfasında, laboratuvarlar listesinde laboratuvarınızı seçin.
5. Laboratuvarınızın ana sayfasında araç çubuğuna **+ Ekle'yi** seçin. 
6. VM için bir **temel görüntü** seçin. 
7. **Gönder** düğmesinin üzerindeki sayfanın altındaki **otomasyon seçeneklerini** seçin. 
8. Sanal makineyi oluşturmak için **Azure Kaynak Yöneticisi şablonuna** bakın. 
9. **Kaynaklar** bölümündeki JSON kesimi, daha önce seçtiğiniz görüntü türü için tanıma sahiptir. 

    ```json
    {
      "apiVersion": "2018-10-15-preview",
      "type": "Microsoft.DevTestLab/labs/virtualmachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "notes": "Windows Server 2019 Datacenter",
        "galleryImageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2019-Datacenter",
          "osType": "Windows",
          "version": "latest"
        },
        "size": "[parameters('size')]",
        "userName": "[parameters('userName')]",
        "password": "[parameters('password')]",
        "isAuthenticationWithSshKey": false,
        "labSubnetName": "[variables('labSubnetName')]",
        "disallowPublicIpAddress": true,
        "storageType": "Standard",
        "allowClaim": false,
        "networkInterface": {
          "sharedPublicIpAddressConfiguration": {
            "inboundNatRules": [
              {
                "transportProtocol": "tcp",
                "backendPort": 3389
              }
            ]
          }
        }
      }
    }
    ```

Bu örnekte, Azure Market Yeri görüntüsünün tanımını nasıl alacağınızı görürsünüz. Özel bir görüntü, formül veya ortamın tanımını aynı şekilde alabilirsiniz. Sanal makine için gereken yapıları ekleyin ve gerekli gelişmiş ayarları ayarlayın. **Otomasyon seçenekleri** düğmesini seçmeden önce, gerekli alanlar ve isteğe bağlı alanlar için değerler sağladıktan sonra.

### <a name="use-azure-rest-api"></a>Azure REST API'sini kullanma
Aşağıdaki yordam, REST API'sini kullanarak görüntünün özelliklerini almak için adımlar verir: Bu adımlar yalnızca laboratuardaki varolan bir VM için çalışır. 

1. [Sanal Makineler](/rest/api/dtl/virtualmachines/list) e git - liste sayfası, **taç düğmesini** seçin. 
2. Azure **aboneliğinizi**seçin.
3. Laboratuvar **için kaynak grubunu**girin.
4. **Laboratuvarın adını**girin. 
5. **Çalıştır**'ı seçin.
6. VM'nin oluşturulduğu **görüntünün özelliklerini** görürsünüz. 

## <a name="set-expiration-date"></a>Son kullanma tarihini ayarlama
Eğitim, demolar ve denemeler gibi senaryolarda, gereksiz maliyetlere maruz kalmak sizin için sanal makineler oluşturmak ve sabit bir süre sonra bunları otomatik olarak silmek isteyebilirsiniz. PowerShell [komut dosyası](#powershell-script) bölümünde gösterildiği gibi PowerShell'i kullanarak bir VM için son kullanma tarihini ayarlayabilirsiniz.

Laboratuvardaki tüm mevcut VM'lerin son kullanma tarihini belirleyen örnek bir PowerShell komut dosyası aşağıda veda edebilirsiniz:

```powershell
# Values to change
$subscriptionId = '<Enter the subscription Id that contains lab>'
$labResourceGroup = '<Enter the lab resource group>'
$labName = '<Enter the lab name>'
$VmName = '<Enter the VmName>'
$expirationDate = '<Enter the expiration date e.g. 2019-12-16>'

# Log into your Azure account
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId
$VmResourceId = "subscriptions/$subscriptionId/resourcegroups/$labResourceGroup/providers/microsoft.devtestlab/labs/$labName/virtualmachines/$VmName"

$vm = Get-AzureRmResource -ResourceId $VmResourceId -ExpandProperties

# Get all the Vm properties
$VmProperties = $vm.Properties

# Set the expirationDate property
If ($VmProperties.expirationDate -eq $null) {
    $VmProperties | Add-Member -MemberType NoteProperty -Name expirationDate -Value $expirationDate
} Else {
    $VmProperties.expirationDate = $expirationDate
}

Set-AzureRmResource -ResourceId $VmResourceId -Properties $VmProperties -Force
```


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki içeriğe bakın: [Azure DevTest Labs için Azure PowerShell belgeleri](/powershell/module/az.devtestlabs/)
