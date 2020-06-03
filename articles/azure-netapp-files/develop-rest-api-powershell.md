---
title: PowerShell kullanarak REST API Azure NetApp Files için geliştirme | Microsoft Docs
description: PowerShell kullanarak Azure NetApp Files REST API nasıl başlaılacağını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: b-juche
ms.openlocfilehash: 0d76fd9a826750e09ebdc374a30a271879393b66
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300543"
---
# <a name="develop-for-azure-netapp-files-with-rest-api-using-powershell"></a>PowerShell kullanarak REST API Azure NetApp Files için geliştirme

Azure NetApp Files hizmetinin REST API, NetApp hesabı, kapasite havuzu, birimler ve anlık görüntüler gibi kaynaklara karşı HTTP işlemlerini tanımlar. Bu makale, PowerShell kullanarak Azure NetApp Files REST API kullanmaya başlamanıza yardımcı olur.

## <a name="azure-netapp-files-rest-api-specification"></a>Azure NetApp Files REST API belirtimi

Azure NetApp Files için REST API belirtimi [GitHub](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager)aracılığıyla yayımlanır:

`https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager`


## <a name="access-the-azure-netapp-files-rest-api"></a>Azure NetApp Files erişin REST API  

1. Daha önce yapmadıysanız [Azure CLI 'Yı yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) .
2. Azure Active Directory bir hizmet sorumlusu oluşturun (Azure AD):
   1. [Yeterli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)sahip olduğunuzu doğrulayın.

   2. Azure CLı 'de aşağıdaki komutu girin:  

           $RBAC_SP = az ad sp create-for-rbac --name <YOURSPNAMEGOESHERE> | ConvertFrom-Json 

      Hizmet sorumlusu bilgilerini göstermek için yazın `$RBAC_SP` ve ENTER tuşuna basın.

           appId       : appID displays here
           displayName : displayName
           name        : http://SP_Name
           password    : super secret password
           tenant      : your tenant shows here
        
      Çıktı, değişken nesnesine kaydedilir `$RBAC_SP` . `$RBAC_SP.appId`, `$RBAC_SP.password` Ve `$RBAC_SP.tenant` değerlerini kullanacağız.

3. Bir OAuth erişim belirteci isteyin:

    Bu makaledeki örneklerde PowerShell kullanılır. [Postman](https://www.getpostman.com/), [Insomnia](https://insomnia.rest/)ve [Paw](https://paw.cloud/)gibi çeşitli API araçlarını da kullanabilirsiniz.  

    `$RBAC_SP`Bu değişkeni kullanarak artık taşıyıcı belirtecini alacaksınız. 

        $body = "grant_type=client_credentials&client_id=$($RBAC_SP.appId)&client_secret=$($RBAC_SP.password)&resource=https://management.azure.com/"
        $BearerToken = Invoke-RestMethod -Method Post -body $body -Uri https://login.microsoftonline.com/$($RBAC_SP.tenant)/oauth2/token

    Çıktı, bir taşıyıcı belirteç nesnesi sağlar. Yazarak erişim belirtecini görebilirsiniz `$BearerToken.access_token` . Çıktı aşağıdaki örneğe benzer:

        eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9

    Görüntülenmiş belirteç 3600 saniye için geçerlidir. Bundan sonra yeni bir belirteç istemeniz gerekir. Belirteç değişkenine kaydedilir ve bir sonraki adımda kullanılacaktır.

4. Nesneyi oluşturun `headers` :

        $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
        $headers.Add("Authorization", "Bearer $($BearerToken.access_token)")
        $headers.Add("Content-Type", "application/json")

5. REST API erişiminizi doğrulamak için bir test çağrısı gönderin ve belirteci ekleyin:

        $SubId = (Get-AzureRmContext).Subscription.Id 
        Invoke-RestMethod -Method Get -Headers $headers -Uri https://management.azure.com/subscriptions/$SubId/providers/Microsoft.Web/sites?api-version=2019-11-01


## <a name="examples-using-the-api"></a>API kullanan örnekler  

Bu makale, istek taban çizgisi için aşağıdaki URL 'YI kullanır. Bu URL, Azure NetApp Files ad alanının köküne işaret eder. 

`https://management.azure.com/subscriptions/$SUBID/resourceGroups/$RESOURCEGROUP/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01`

Aşağıdaki örnekleri kendi değerlerinizle çalıştırmadan önce değişken değerleri atamanız gerekir. Yazarak PowerShell değişkenlerine erişilir `$variablename` .
PowerShell değişkenleri kullanılarak atanır `$variablename = “value”` .

        $Region = “westus2" 
        $ResourceGroup = “MYTest-RG-63" 
        $ANFvnetname = “NetAppFilesVnet-63"
        $ANFvnetCIDR = “10.63.64.0/18"
        $ANFsubnet = “NetAppFilesSubnet-63"
        $ANFsubnetCIDR = “10.63.120.0/28"
        $ANFsubnetID = “/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.Network/virtualNetworks/$ANFvnetname/subnets/$ANFSubnet"
        $ANFAccount = “TestoftheAPI"
        $ANFCapacityPool = “ANFTestPool"
        $ANFServicelevel = “Standard"
        $ANFVolume = “ANFTestVolume"
        $ANFVolumeShareName = “Share-TEST"
        $ANFVolumesize = 100GB
        $ANFSnapshot = “ANFTestSnapshot"

### <a name="put-request-examples"></a>PUT isteği örnekleri

Aşağıdaki örneklerde gösterildiği gibi Azure NetApp Files yeni nesneler oluşturmak için bir PUT isteği kullanırsınız. PUT isteğinin gövdesi, değişiklikler için JSON biçimli verileri içerir. Bu, PowerShell komutuna metin olarak eklenmesi veya dosya olarak başvurulmalıdır. Gövdeye dosya olarak başvurmak için JSON örneğini bir dosyaya kaydedin ve `-body (Get-Content @<filename>)` PowerShell komutuna ekleyin.


    #create a NetApp account  
    $body = "{ 
        `"name`": `"$ANFAccount`", 
        `"type`": `"Microsoft.NetApp/netAppAccounts`", 
        `"location`": `"$Region`", 
        `"properties`": { 
            `"name`": `"$ANFAccount`" 
        }
    } "

    $api_version = "2020-02-01"
    Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount`?api-version=$api_version"  

    #create a capacity pool  
    $body = "{
      `"location`": `"$Region`",
      `"properties`": {
        `"size`": " + 4TB + ",
        `"serviceLevel`": `"Standard`"
      }
    }"
    $api_version = "2020-02-01"
    Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool`?api-version=$api_version" 

    #create a volume  
    $body = "{
        `"name`": `"$ANFVolume`",
        `"type`": `"Microsoft.NetApp/netAppAccounts/capacityPools/volumes`",
        `"location`": `"$Region`",
        `"properties`": {
            `"serviceLevel`": `"$ANFServicelevel`",
            `"usageThreshold`": `"$ANFVolumesize`",
            `"creationToken`": `"$ANFVolumeShareName`",
            `"snapshotId`": `"`",
            `"subnetId`": `"$ANFSubnetID`"
        }
    }"
    $api_version = "2020-02-01"
    Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume`?api-version=$api_version" 

    #create a volume snapshot
    $body = "{
        `"name`": `"$ANFAccount/$ANFCapacityPool/$ANFVolume/$ANFSnapshot`",
        `"type`": `"Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots`",
        `"location`": `"$Region`",
        `"properties`": {
            `"name`": `"$ANFSnapshot`",
            `"fileSystemId`": `"$FileSystemID`"
        }
    }"
    $api_version = '2020-02-01'
    Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume/Snapshots/$ANFSnapshot`?api-version=$api_version"

### <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnekte bir NetApp hesabının nasıl oluşturulacağı gösterilmektedir:

    { 
        "name": "MYNETAPPACCOUNT", 
        "type": "Microsoft.NetApp/netAppAccounts", 
        "location": "westus2", 
        "properties": { 
            "name": "MYNETAPPACCOUNT" 
        }
    } 

Aşağıdaki örnek, bir kapasite havuzunun nasıl oluşturulacağını gösterir: 

    {
        "name": "MYNETAPPACCOUNT/POOLNAME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
        "location": "westus2",
        "properties": {
            "name": "POOLNAME",
            "size": "4398046511104",
            "serviceLevel": "Premium"
        }
    }

Aşağıdaki örnek, yeni bir birimin nasıl oluşturulacağını göstermektedir. (Birim için varsayılan protokol NFSV3 ' dir.) 

    {
        "name": "MYNEWVOLUME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/volumes",
        "location": "westus2",
        "properties": {
            "serviceLevel": "Premium",
            "usageThreshold": "322122547200",
            "creationToken": "MY-FILEPATH",
            "snapshotId": "",
            "subnetId": "/subscriptions/$SUBID/resourceGroups/$RESOURCEGROUP/providers/Microsoft.Network/virtualNetworks/VNETGOESHERE/subnets/MYDELEGATEDSUBNET.sn"
        }
    }

Aşağıdaki örnekte, bir birimin anlık görüntüsünün nasıl oluşturulacağı gösterilmektedir: 

    {
        "name": "apitest2/apiPool01/apiVol01/snap02",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots",
        "location": "westus2",
        "properties": {
            "name": "snap02",
            "fileSystemId": "0168704a-bbec-da81-2c29-503825fe7420"
        }
    }

> [!NOTE] 
> `fileSystemId`Anlık görüntü oluşturmak için belirtmeniz gerekir.  `fileSystemId`Değeri bir birime YÖNELIK Get isteğiyle elde edebilirsiniz. 

### <a name="get-request-examples"></a>İstek örneklerini al

Kaynak yoksa bir hata oluşur. Aşağıdaki örneklerde gösterildiği gibi, bir abonelikte Azure NetApp Files nesnelerini sorgulamak için GET isteği kullanırsınız:

    #get NetApp accounts 
    Invoke-RestMethod -Method Get -Headers $headers -Uri https://management.azure.com/subscriptions/$SUBID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01 | ConvertTo-Json

    #get capacity pools for NetApp account 
    Invoke-RestMethod -Method Get -Headers $headers -Uri https://management.azure.com/subscriptions/$SUBID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFACCOUNT/capacityPools?api-version=2019-11-01 | ConvertTo-Json

    #get volumes in NetApp account & capacity pool 
    Invoke-RestMethod -Method Get -Headers $headers -Uri https://management.azure.com/subscriptions/$SUBID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFACCOUNT/capacityPools/$ANFCAPACITYPOOL/volumes?api-version=2019-11-01 | ConvertTo-Json

    #get snapshots for a volume 
    Invoke-RestMethod -Method Get -Headers $headers -Uri https://management.azure.com/subscriptions/$SUBID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFACCOUNT/capacityPools/$ANFCAPACITYPOOL/volumes/$ANFVOLUME/snapshots?api-version=2019-11-01 | ConvertTo-Json

### <a name="complete-powershell-scripts"></a>PowerShell betikleri Tamam
Bu bölümde PowerShell için örnek betikler gösterilmektedir.

    <#
    Disclaimer 
    The sample scripts are not supported under any Microsoft standard support program or service. The sample scripts are provided AS IS without warranty of any kind. Microsoft further disclaims all implied warranties including, without limitation, any implied warranties of merchantability or of fitness for a particular purpose. The entire risk arising out of the use or performance of the sample scripts and documentation remains with you. In no event shall Microsoft, its authors, or anyone else involved in the creation, production, or delivery of the scripts be liable for any damages whatsoever (including, without limitation, damages for loss of business profits, business interruption, loss of business information, or other pecuniary loss) arising out of the use of or inability to use the sample scripts or documentation, even if Microsoft has been advised of the possibility of such damages.
    #>
    
    $Region = "westus2"
    $SubID = (Get-AzureRmContext).Subscription.Id
    $MyRandomID =  Get-Random -Minimum 100 -Maximum 999
    $ResourceGroup = "MYTest-RG-" + $MyRandomID
    $ANFAccount = "ANFTestAccount-$Region-" + $MyRandomID
    $ANFCapacityPool =  "ANFTestPool-$Region-" + $MyRandomID
    $ANFVolume = "ANFTestVolume-$Region-" + $MyRandomID
    $ANFVolumesize = 100GB
    $ANFVolumeShareName = "Share-" + $MyRandomID
    $ANFSnapshot = "ANFTestSnapshot-$Region-" + $MyRandomID
    $ANFServicelevel = "Standard"
    $Octet2 = Get-Random -Minimum 1 -Maximum 254
    $ANFvnetname = "NetAppFilesVnet-$Octet2-$Region-" + $MyRandomID
    $ANFvnetCIDR = "10.$Octet2.64.0/18"
    $ANFsubnet = "NetAppFilesSubnet-$Octet2-$Region-" + $MyRandomID
    $ANFsubnetCIDR = "10.$Octet2.120.0/28"
    $vmsubnet = "VM-subnet-$Octet2-$Region-" + $MyRandomID
    $vmsubnetCIDR = "10.$Octet2.121.0/24"
    
    $BearerToken = (az account get-access-token | ConvertFrom-Json).accessToken
    
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer $BearerToken")
    $headers.Add("Content-Type", "application/json")
    
    " ** Creating Resource Group $ResourceGroup *************"
    $api_version = "2020-01-01"
    $body = "    {
      `"location`": `"$Region`",
      `"name`": `"$ResourceGroup`"
    },"
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourcegroups/$ResourceGroup`?api-version=$api_version" 
    While ($response.properties.provisioningState -notmatch "Succeeded") {
       $response.properties.provisioningState
       sleep 5
       $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup`?api-version=$api_version" 
       } 
    
    " ** Creating Virtual Network $ANFvnetname *************"
    $body = "{
      `"properties`": {
        `"addressSpace`": {
          `"addressPrefixes`": [
            `"$ANFvnetCIDR`"
          ]
        },
        `"subnets`": [
          {
            `"name`": `"$ANFsubnet`",
            `"properties`": {
              `"addressPrefix`": `"$ANFsubnetCIDR`",
              `"delegations`": [
                {
                  `"name`": `"Microsoft.NetApp`",
                  `"properties`": {
                    `"serviceName`": `"Microsoft.NetApp/volumes`"
                  }
                }
              ]
            }
          },
         {
            `"name`": `"$vmsubnet`",
            `"properties`": {
              `"addressPrefix`": `"$vmsubnetCIDR`"
                }
         }
        ]
      },
      `"location`": `"$Region`"
    }"
    
    $api_version = "2020-03-01"
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.Network/virtualNetworks/$ANFvnetname`?api-version=$api_version" 
    While ($response.properties.provisioningState -notmatch "Succeeded") {
           sleep 5
           $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.Network/virtualNetworks/$ANFvnetname`?api-version=$api_version" 
           $response.properties.provisioningState
           }  
    $ANFsubnetID = $response.properties.subnets.id[0]
    
    " ** Creating ANF Account $ANFAccount *************"
    $body = "{ 
        `"name`": `"$ANFAccount`", 
        `"type`": `"Microsoft.NetApp/netAppAccounts`", 
        `"location`": `"$Region`", 
        `"properties`": { 
            `"name`": `"$ANFAccount`" 
        }
    } "
    $api_version = "2020-02-01"
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount`?api-version=$api_version" 
    $response.properties.provisioningState
    While ($response.properties.provisioningState -notmatch "Succeeded") {
       $response.properties.provisioningState
       sleep 5
       $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount`?api-version=$api_version" 
       }  
    
    " ** Creating Capacity Pool $ANFCapacityPool *************"
    $body = "{
      `"location`": `"$Region`",
      `"properties`": {
        `"size`": " + 4TB + ",
        `"serviceLevel`": `"Standard`"
      }
    }"
    $api_version = "2020-02-01"
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool`?api-version=$api_version" 
    $response.properties.provisioningState
    While ($response.properties.provisioningState -notmatch "Succeeded") {
       $response.properties.provisioningState
       sleep 5
       $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount`?api-version=$api_version" 
       }  
    
    " ** Creating ANF Volume $ANFVolume *************"
    $body = "{
        `"name`": `"$ANFVolume`",
        `"type`": `"Microsoft.NetApp/netAppAccounts/capacityPools/volumes`",
        `"location`": `"$Region`",
        `"properties`": {
            `"serviceLevel`": `"$ANFServicelevel`",
            `"usageThreshold`": `"$ANFVolumesize`",
            `"creationToken`": `"$ANFVolumeShareName`",
            `"snapshotId`": `"`",
            `"subnetId`": `"$ANFSubnetID`"
        }
    }"
    $api_version = "2020-02-01"
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume`?api-version=$api_version" 
    While ($response.properties.provisioningState -notmatch "Succeeded") {
       $response.properties.provisioningState
       sleep 15
       $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume`?api-version=$api_version" 
       } 
    $Volume = $response 
    $FileSystemID = $response.properties.fileSystemId
    
    " ** Creating ANF Volume Snapshot $ANFSnapshot *************"
    $body = "{
        `"name`": `"$ANFAccount/$ANFCapacityPool/$ANFVolume/$ANFSnapshot`",
        `"type`": `"Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots`",
        `"location`": `"$Region`",
        `"properties`": {
            `"name`": `"$ANFSnapshot`",
            `"fileSystemId`": `"$FileSystemID`"
        }
    }"
    $api_version = '2020-02-01'
    $response = Invoke-RestMethod -Method 'PUT' -Headers $headers -Body $body -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume/Snapshots/$ANFSnapshot`?api-version=$api_version" 
    While ($response.properties.provisioningState -notmatch "Succeeded") {
       $response.properties.provisioningState
       sleep 5
       $response = Invoke-RestMethod -Method ‘GET’ -Headers $headers -Uri "https://management.azure.com/subscriptions/$SubID/resourceGroups/$ResourceGroup/providers/Microsoft.NetApp/netAppAccounts/$ANFAccount/capacityPools/$ANFCapacityPool/volumes/$ANFVolume/Snapshots/$ANFSnapshot`?api-version=$api_version" 
       }  

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Files REST API başvurusuna bakın](https://docs.microsoft.com/rest/api/netapp/)
