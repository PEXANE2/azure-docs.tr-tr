---
title: Azure Cosmos hesabı için Azure Özel Bağlantısını Yapılandırma
description: Sanal ağda özel bir IP adresi kullanarak Azure Cosmos hesabına erişmek için Azure Özel Bağlantısını nasıl ayarlayabilirsiniz öğrenin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: thweiss
ms.openlocfilehash: 944341b1ef88c7e3d64a74536720eb9fb1d17321
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80152749"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account"></a>Azure Cosmos hesabı için Azure Özel Bağlantısını Yapılandırma

Azure Özel Bağlantı'nı kullanarak, özel bir bitiş noktası üzerinden bir Azure Cosmos hesabına bağlanabilirsiniz. Özel bitiş noktası, sanal ağınızdaki bir alt ağdaki özel IP adresleri kümesidir. Ardından, özel IP adresleri üzerinden azure cosmos hesabına erişimi sınırlandırabilirsiniz. Private Link, kısıtlı NSG ilkeleriyle birleştirildiğinde, veri sızma riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi edinmek için [Azure Özel Bağlantı](../private-link/private-link-overview.md) makalesine bakın.

Private Link, kullanıcıların bir Azure Cosmos hesabına sanal ağ içinden veya herhangi bir eşlenen sanal ağdan erişmesine olanak tanır. Özel Bağlantı'ya eşlenen kaynaklara VPN veya Azure ExpressRoute üzerinden özel bakış lar üzerinden şirket içinde de erişilebilir. 

Otomatik veya manuel onay yöntemini kullanarak Private Link ile yapılandırılan bir Azure Cosmos hesabına bağlanabilirsiniz. Daha fazla bilgi edinmek için, Özel Bağlantı belgelerinin [Onay iş akışı](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) bölümüne bakın. 

Bu makalede, özel bir bitiş noktası oluşturmak için adımları açıklanır. Otomatik onay yöntemini kullandığınızı varsayar.

> [!NOTE]
> Özel uç nokta desteği şu anda yalnızca ağ geçidi bağlantı modu için desteklenen bölgelerde genel olarak kullanılabilir. Doğrudan mod için önizleme özelliği olarak kullanılabilir.

## <a name="create-a-private-endpoint-by-using-the-azure-portal"></a>Azure portalını kullanarak özel bir bitiş noktası oluşturma

Azure portalını kullanarak varolan bir Azure Cosmos hesabı için özel bir bitiş noktası oluşturmak için aşağıdaki adımları kullanın:

1. Tüm **kaynaklar** bölmesinden bir Azure Cosmos hesabı seçin.

1. Ayarlar listesinden **Özel Bitiş Noktası Bağlantıları'nı** seçin ve ardından **Özel bitiş noktasını**seçin:

   ![Azure portalında özel bir bitiş noktası oluşturmak için seçimler](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. Özel **bir bitiş noktası oluştur - Temel ler** bölmesinde aşağıdaki ayrıntıları girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Kaynak grubunu seçin.|
    | **Örnek ayrıntıları** |  |
    | Adı | Özel bitiş noktanız için herhangi bir ad girin. Bu ad alınırsa, benzersiz bir ad oluşturun. |
    |Bölge| Özel Bağlantı dağıtmak istediğiniz bölgeyi seçin. Sanal ağınızın bulunduğu konumda özel bitiş noktası oluşturun.|
    |||
1. **Sonraki seçin: Kaynak**.
1. **Özel bir bitiş noktası oluştur - Kaynak**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | **Dizinimdeki Bir Azure kaynağına Bağlan'ı**seçin. <br/><br/> Daha sonra Özel Bağlantı'yı ayarlamak için kaynaklarınızdan birini seçebilirsiniz. Veya sizinle paylaştığı kaynak kimliği veya takma ad kullanarak başka birinin kaynağına bağlanabilirsiniz.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft.AzureCosmosDB/databaseAccounts'u**seçin. |
    | Kaynak |Azure Cosmos hesabınızı seçin. |
    |Hedef alt kaynak |Haritalamak istediğiniz Azure Cosmos DB API türünü seçin. Bu, SQL, MongoDB ve Cassandra API'leri için yalnızca bir seçenek varsayılandır. Gremlin ve Tablo API'leri için, bu API'ler SQL API ile birlikte çalışabilir olduğundan **Sql'i** de seçebilirsiniz. |
    |||

1. **Sonraki'ni seçin: Yapılandırma.**
1. **Özel bir bitiş noktası oluşturma - Yapılandırma**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ Oluşturma**| |
    | Sanal ağ| Sanal ağınızı seçin. |
    | Alt ağ | Alt ağınızı seçin. |
    |**Özel DNS Entegrasyonu**||
    |Özel DNS bölgesiyle tümleştirme |**Evet'i**seçin. <br><br/> Özel bitiş noktanızla özel olarak bağlanmak için bir DNS kaydına ihtiyacınız vardır. Özel bitiş noktanızı özel bir DNS bölgesiyle tümleştirmenizi öneririz. Ayrıca, sanal makinelerinizdeki ana bilgisayar dosyalarını kullanarak kendi DNS sunucularınızı kullanabilir veya DNS kayıtları oluşturabilirsiniz. |
    |Özel DNS Bölgesi |**privatelink.documents.azure.com**seçin. <br><br/> Özel DNS bölgesi otomatik olarak belirlenir. Azure portalını kullanarak değiştiremezsiniz.|
    |||

1. **İncele ve oluştur**’u seçin. Gözden **Geçir + oluştur** sayfasında Azure yapılandırmanızı doğrular.
1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

Azure portalında bir Azure Cosmos hesabı için Özel Bağlantı'yı onayladığınızda, **Güvenlik Duvarı ve sanal ağlar** bölmesindeki Tüm **ağlar** seçeneği kullanılamaz.

Aşağıdaki tablo, farklı Azure Cosmos hesap API türleri, desteklenen alt kaynaklar ve ilgili özel bölge adları arasındaki eşleşme gösterir. Ayrıca, SQL API üzerinden Gremlin ve Tablo API hesaplarına erişebilirsiniz, bu nedenle bu API'ler için iki giriş vardır.

|Azure Cosmos hesap API türü  |Desteklenen alt kaynaklar (veya grup disleri) |Özel bölge adı  |
|---------|---------|---------|
|Sql    |   Sql      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  Sql       |  privatelink.documents.azure.com    |
|Tablo    |    Tablo     |   privatelink.table.cosmos.azure.com    |
|Tablo     |   Sql      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>Özel IP adreslerini getirin

Özel bitiş noktası sağlandıktan sonra IP adreslerini sorgulayabilirsiniz. Azure portalından IP adreslerini görüntülemek için:

1. **Tüm kaynaklar**’ı seçin.
1. Daha önce oluşturduğunuz özel bitiş noktasını arayın. Bu durumda, **cdbPrivateEndpoint3**.
1. DNS ayarlarını ve IP adreslerini görmek için **Genel Bakış** sekmesini seçin.

![Azure portalında özel IP adresleri](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

Özel bitiş noktası başına birden çok IP adresi oluşturulur:

* Azure Cosmos hesabının genel (bölge agnostik) bitiş noktası için bir tane
* Azure Cosmos hesabının dağıtıldığı her bölge için bir tane

## <a name="create-a-private-endpoint-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak özel bir bitiş noktası oluşturun

Varolan bir Azure Cosmos hesabı için "MyPrivateEndpoint" adlı özel bir bitiş noktası oluşturmak için aşağıdaki PowerShell komut dosyasını çalıştırın. Değişken değerlerini ortamınızın ayrıntılarıyla değiştirin.

```azurepowershell-interactive
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Özel bitiş noktasını özel bir DNS bölgesiyle tümleştirme

Özel bitiş noktasını oluşturduktan sonra, aşağıdaki PowerShell komut dosyasını kullanarak özel bir DNS bölgesiyle tümleştirebilirsiniz:

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>Özel IP adreslerini getirin

Özel bitiş noktası sağlandıktan sonra, aşağıdaki PowerShell komut dosyasını kullanarak IP adreslerini ve FQDN eşlemi sorgulayabilirsiniz:

```azurepowershell-interactive
$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-endpoint-by-using-azure-cli"></a>Azure CLI'yi kullanarak özel bir bitiş noktası oluşturma

Varolan bir Azure Cosmos hesabı için "myPrivateEndpoint" adlı özel bir bitiş noktası oluşturmak için aşağıdaki Azure CLI komut dosyasını çalıştırın. Değişken değerlerini ortamınızın ayrıntılarıyla değiştirin.

```azurecli-interactive
# Resource group where the Azure Cosmos account and virtual network resources are located
ResourceGroupName="myResourceGroup"

# Subscription ID where the Azure Cosmos account and virtual network resources are located
SubscriptionId="<your Azure subscription ID>"

# Name of the existing Azure Cosmos account
CosmosDbAccountName="mycosmosaccount"

# API type of your Azure Cosmos account: Sql, MongoDB, Cassandra, Gremlin, or Table
CosmosDbApiType="Sql"

# Name of the virtual network to create
VNetName="myVnet"

# Name of the subnet to create
SubnetName="mySubnet"

# Name of the private endpoint to create
PrivateEndpointName="myPrivateEndpoint"

# Name of the private endpoint connection to create
PrivateConnectionName="myConnection"

az network vnet create \
 --name $VNetName \
 --resource-group $ResourceGroupName \
 --subnet-name $SubnetName

az network vnet subnet update \
 --name $SubnetName \
 --resource-group $ResourceGroupName \
 --vnet-name $VNetName \
 --disable-private-endpoint-network-policies true

az network private-endpoint create \
    --name $PrivateEndpointName \
    --resource-group $ResourceGroupName \
    --vnet-name $VNetName  \
    --subnet $SubnetName \
    --private-connection-resource-id "/subscriptions/$SubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.DocumentDB/databaseAccounts/$CosmosDbAccountName" \
    --group-ids $CosmosDbApiType \
    --connection-name $PrivateConnectionName
```

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Özel bitiş noktasını özel bir DNS bölgesiyle tümleştirme

Özel bitiş noktasını oluşturduktan sonra, aşağıdaki Azure CLI komut dosyasını kullanarak özel bir DNS bölgesiyle tümleştirebilirsiniz:

```azurecli-interactive
zoneName="privatelink.documents.azure.com"

az network private-dns zone create --resource-group $ResourceGroupName \
   --name  $zoneName

az network private-dns link vnet create --resource-group $ResourceGroupName \
   --zone-name  $zoneName\
   --name myzonelink \
   --virtual-network $VNetName \
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name $PrivateEndpointName --resource-group $ResourceGroupName --query 'networkInterfaces[0].id' -o tsv)
 
# Copy the content for privateIPAddress and FQDN matching the Azure Cosmos account 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
 
#Create DNS records 
az network private-dns record-set a create --name recordSet1 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName
az network private-dns record-set a add-record --record-set-name recordSet2 --zone-name privatelink.documents.azure.com --resource-group $ResourceGroupName -a <Private IP Address>
```

## <a name="create-a-private-endpoint-by-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak özel bir bitiş noktası oluşturma

Sanal ağ alt ağında özel bir bitiş noktası oluşturarak Private Link'i ayarlayabilirsiniz. Azure Kaynak Yöneticisi şablonu kullanarak bunu elde elabilirsiniz.

"PrivateEndpoint_template.json" adlı bir Kaynak Yöneticisi şablonu oluşturmak için aşağıdaki kodu kullanın. Bu şablon, varolan bir sanal ağdaki varolan Bir Azure Cosmos SQL API hesabı için özel bir bitiş noktası oluşturur.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

**Şablon için parametre dosyasını tanımlama**

Şablon için bir parametre dosyası oluşturun ve "PrivateEndpoint_parameters.json" adını verdi. Parametreler dosyasına aşağıdaki kodu ekleyin:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

**PowerShell komut dosyası kullanarak şablonu dağıtma**

Aşağıdaki kodu kullanarak bir PowerShell komut dosyası oluşturun. Komut dosyasını çalıştırmadan önce, abonelik kimliğini, kaynak grubu adını ve diğer değişken değerlerini ortamınızın ayrıntılarıyla değiştirin.

```azurepowershell-interactive
### This script creates a private endpoint for an existing Azure Cosmos account in an existing virtual network

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

PowerShell komut `GroupId` dosyasında, değişken yalnızca bir değer içerebilir. Bu değer hesabın API türüdür. İzin verilen `Sql`değerler `MongoDB` `Cassandra`şunlardır: , , `Gremlin`, , ve `Table`. Bazı Azure Cosmos hesap türlerine birden çok API üzerinden erişilebilir. Örnek:

* Gremlin API hesabına hem Gremlin hem de SQL API hesaplarından erişilebilir.
* Tablo API hesabına hem Tablo hem de SQL API hesaplarından erişilebilir.

Bu hesaplar için, her API türü için bir özel bitiş noktası oluşturmanız gerekir. İlgili API türü `GroupId` dizide belirtilir.

Şablon başarıyla dağıtıldıktan sonra, aşağıdaki görüntünün gösterdiğine benzer bir çıktı görebilirsiniz. Değer, `provisioningState` `Succeeded` özel uç noktaların doğru şekilde ayarlanışolmasıdır.

![Kaynak Yöneticisi şablonu için dağıtım çıktısı](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

Şablon dağıtıldıktan sonra, özel IP adresleri alt ağ içinde ayrılmıştır. Azure Cosmos hesabının güvenlik duvarı kuralı, yalnızca özel bitiş noktasından bağlantıları kabul etmek üzere yapılandırılır.

### <a name="integrate-the-private-endpoint-with-a-private-dns-zone"></a>Özel bitiş noktasını Özel DNS Bölgesiyle tümleştirme

"PrivateZone_template.json" adlı bir Kaynak Yöneticisi şablonu oluşturmak için aşağıdaki kodu kullanın. Bu şablon, varolan bir sanal ağdaki varolan bir Azure Cosmos SQL API hesabı için özel bir DNS bölgesi oluşturur.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "type": "string"
        },
        "VNetId": {
            "type": "string"
        }        
    },
    "resources": [
        {
            "name": "[parameters('privateZoneName')]",
            "type": "Microsoft.Network/privateDnsZones",
            "apiVersion": "2018-09-01",
            "location": "global",
            "properties": {                
            }
        },
        {
            "type": "Microsoft.Network/privateDnsZones/virtualNetworkLinks",
            "apiVersion": "2018-09-01",
            "name": "[concat(parameters('privateZoneName'), '/myvnetlink')]",
            "location": "global",
            "dependsOn": [
                "[resourceId('Microsoft.Network/privateDnsZones', parameters('privateZoneName'))]"
            ],
            "properties": {
                "registrationEnabled": false,
                "virtualNetwork": {
                    "id": "[parameters('VNetId')]"
                }
            }
        }        
    ]
}
```

"PrivateZoneRecords_template.json" adlı kaynak yöneticisi şablonu oluşturmak için aşağıdaki kodu kullanın.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "type": "string"
        },
        "IPAddress": {
            "type":"string"
        }        
    },
    "resources": [
         {
            "type": "Microsoft.Network/privateDnsZones/A",
            "apiVersion": "2018-09-01",
            "name": "[parameters('DNSRecordName')]",
            "properties": {
                "ttl": 300,
                "aRecords": [
                    {
                        "ipv4Address": "[parameters('IPAddress')]"
                    }
                ]
            }
        }    
    ]
}
```

**Şablon için parametre dosyasını tanımlama**

Şablon için aşağıdaki iki parametre dosyasını oluşturun. "PrivateZone_parameters.json"u oluşturun. aşağıdaki kod ile:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateZoneName": {
            "value": ""
        },
        "VNetId": {
            "value": ""
        }
    }
}
```

"PrivateZoneRecords_parameters.json"u oluşturun. aşağıdaki kod ile:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "DNSRecordName": {
            "value": ""
        },
        "IPAddress": {
            "type":"object"
        }
    }
}
```

**PowerShell komut dosyası kullanarak şablonu dağıtma**

Aşağıdaki kodu kullanarak bir PowerShell komut dosyası oluşturun. Komut dosyasını çalıştırmadan önce, abonelik kimliğini, kaynak grubu adını ve diğer değişken değerlerini ortamınızın ayrıntılarıyla değiştirin.

```azurepowershell-interactive
### This script:
### - creates a private zone
### - creates a private endpoint for an existing Cosmos DB account in an existing VNet
### - maps the private endpoint to the private zone

## Step 1: Fill in these details. Replace the variable values with the details for your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Azure Cosmos account and virtual network resources are located
$ResourceGroupName = "myResourceGroup"
# Name of the Azure Cosmos account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Azure Cosmos account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing virtual network
$VNetName = "myVnet"
# Name of the target subnet in the virtual network
$SubnetName = "mySubnet"
# Name of the private zone to create
$PrivateZoneName = "myPrivateZone.documents.azure.com"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateZoneTemplateFilePath = "PrivateZone_template.json"
$PrivateZoneParametersFilePath = "PrivateZone_parameters.json"
$PrivateZoneRecordsTemplateFilePath = "PrivateZoneRecords_template.json"
$PrivateZoneRecordsParametersFilePath = "PrivateZoneRecords_parameters.json"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Login your Azure account and select the target subscription
Login-AzAccount 
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private zone
New-AzResourceGroupDeployment -Name "PrivateZoneDeployment" `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $PrivateZoneTemplateFilePath `
    -TemplateParameterFile $PrivateZoneParametersFilePath `
    -PrivateZoneName $PrivateZoneName `
    -VNetId $VNetResourceId

## Step 5: Create the private endpoint
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName
$deploymentOutput

## Step 6: Map the private endpoint to the private zone
$networkInterface = Get-AzResource -ResourceId $deploymentOutput.Outputs.privateEndpointNetworkInterface.Value -ApiVersion "2019-04-01"
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) {
    foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) {
        $recordName = $fqdn.split('.',2)[0]
        $dnsZone = $fqdn.split('.',2)[1]
        Write-Output "Deploying PrivateEndpoint DNS Record $($PrivateZoneName)/$($recordName) Template on $($resourceGroupName)"
        New-AzResourceGroupDeployment -Name "PrivateEndpointDNSDeployment" `
            -ResourceGroupName $ResourceGroupName `
            -TemplateFile $PrivateZoneRecordsTemplateFilePath `
            -TemplateParameterFile $PrivateZoneRecordsParametersFilePath `
            -DNSRecordName "$($PrivateZoneName)/$($RecordName)" `
            -IPAddress $ipconfig.properties.privateIPAddress
    }
}
```

## <a name="configure-custom-dns"></a>Özel API'yi yapılandırma

Alt ağ içinde özel bitiş noktasını oluşturduğunuz özel bir DNS bölgesi kullanmanız gerekir. Uç noktaları, her özel IP adresinin bir DNS girişine eşlenecek şekilde yapılandırın. (Daha `fqdns` önce gösterilen yanıttaki özelliğe bakın.)

Özel bitiş noktasını oluştururken, azure'da özel bir DNS bölgesiyle tümleştirebilirsiniz. Bunun yerine özel bir DNS bölgesi kullanmayı seçerseniz, özel bitiş noktası için ayrılmış tüm özel IP adresleri için DNS kayıtları eklemek için yapılandırmanız gerekir.

## <a name="private-link-combined-with-firewall-rules"></a>Özel Bağlantı, güvenlik duvarı kurallarıyla birleştirildi

Güvenlik duvarı kurallarıyla birlikte Private Link'i kullandığınızda aşağıdaki durumlar ve sonuçlar mümkündür:

* Herhangi bir güvenlik duvarı kuralı yapılandırmazsanız, varsayılan olarak tüm trafik bir Azure Cosmos hesabına erişebilir.

* Genel trafiği veya hizmet bitiş noktasını yapılandırırsanız ve özel bitiş noktaları oluşturursanız, farklı gelen trafik türleri ilgili güvenlik duvarı kuralı türütarafından yetkilendirilir.

* Herhangi bir genel trafik veya hizmet bitiş noktasını yapılandırmaz ve özel bitiş noktaları oluşturursanız, Azure Cosmos hesabına yalnızca özel uç noktalardan erişilebilir. Genel trafiği veya hizmet bitiş noktasını yapılandırmazsanız, onaylanan tüm özel uç noktalar reddedildikten veya silindikten sonra, hesap tüm ağa açıktır.

## <a name="update-a-private-endpoint-when-you-add-or-remove-a-region"></a>Bir bölge eklediğinizde veya kaldırdığınızda özel bir bitiş noktasını güncelleştirme

Azure Cosmos hesabına bölge ekleme veya kaldırma, o hesap için DNS girişleri eklemenizi veya kaldırmanızı gerektirir. Bölgeler eklendikten veya kaldırıldıktan sonra, eklenen veya kaldırılan DNS girişlerini ve bunlara karşılık gelen özel IP adreslerini yansıtacak şekilde alt netin özel DNS bölgesini güncelleştirebilirsiniz.

Örneğin, bir Azure Cosmos hesabını üç bölgede dağıtdığınızı düşünün: "Batı ABD", "Orta ABD" ve "Batı Avrupa." Hesabınız için özel bir bitiş noktası oluşturduğunuzda, alt ağda dört özel IP ayrılmıştır. Üç bölgenin her biri için bir IP ve küresel/bölge agnostik bitiş noktası için bir IP vardır.

Daha sonra, Azure Cosmos hesabına yeni bir bölge (örneğin, "Doğu ABD") ekleyebilirsiniz. Yeni bölgeyi ekledikten sonra, özel DNS bölgenize veya özel DNS'nize karşılık gelen bir DNS kaydı eklemeniz gerekir.

Bir bölgeyi kaldırdığınızda aynı adımları kullanabilirsiniz. Bölgeyi kaldırdıktan sonra, ilgili DNS kaydını özel DNS bölgenizden veya özel DNS'nizden kaldırmanız gerekir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Azure Cosmos hesabında Özel Bağlantı kullanırken aşağıdaki sınırlamalar geçerlidir:

* Azure Cosmos hesapları ve sanal ağlar için Özel Bağlantı desteği yalnızca belirli bölgelerde kullanılabilir. Desteklenen bölgelerin listesi için, Özel Bağlantı makalesinin [Kullanılabilir bölgeler](../private-link/private-link-overview.md#availability) bölümüne bakın. 

  > [!NOTE]
  > Özel bir bitiş noktası oluşturmak için hem sanal ağın hem de Azure Cosmos hesabının desteklenen bölgelerde olduğundan emin olun.

* Doğrudan mod bağlantısı kullanarak Azure Cosmos hesabıyla Özel Bağlantı kullanıyorsanız, yalnızca TCP protokolünü kullanabilirsiniz. HTTP protokolü henüz desteklenmedi.

* Özel uç nokta desteği şu anda yalnızca ağ geçidi bağlantı modu için desteklenen bölgelerde genel olarak kullanılabilir. Doğrudan mod için önizleme özelliği olarak kullanılabilir.

* Azure Cosmos DB'nin MongoDB hesapları için API'sini kullanırken, yalnızca sunucu sürümü 3.6'daki hesaplar için özel bir bitiş `*.mongo.cosmos.azure.com`noktası desteklenir (diğer bir deyişle, biçimdeki bitiş noktasını kullanan hesaplar). Private Link, sunucu sürüm 3.2'deki hesaplar için desteklenmez (diğer `*.documents.azure.com`bir deyişle, biçimdeki bitiş noktasını kullanan hesaplar). Özel Bağlantı'yı kullanmak için eski hesapları yeni sürüme geçirmeniz gerekir.

* Azure Cosmos DB'nin Özel Bağlantı'ya sahip MongoDB hesapları için API'sini kullanırken Robo 3T, Studio 3T ve Mongoose gibi araçları kullanamazsınız. Parametre belirtilirse, bitiş noktası `appName=<account name>` Yalnızca Özel Bağlantı desteğine sahip olabilir. `replicaSet=globaldb&appName=mydbaccountname` bunun bir örneğidir. Bu araçlar hizmete bağlantı dizesinde uygulama adını geçiremediğinden, Özel Bağlantı'yı kullanamazsınız. Ancak 3.6 sürümü ile SDK sürücüleri kullanarak bu hesaplara erişebilirsiniz.

* Bir sanal ağı Private Link içeriyorsa taşıyamaz veya silemezsiniz.

* Bir Azure Cosmos hesabını özel bir bitiş noktasına bağlıysa silemezsiniz.

* Bir Azure Cosmos hesabı üzerinde, hesaba eklenen tüm özel uç noktalarına eşlenmemiş bir bölge üzerinde başarısız olamazsınız.

* Otomatik olarak onaylanmış özel uç noktaları oluşturmak için bir ağ yöneticisine Azure Cosmos hesap kapsamında en az "*/PrivateEndpointConnectionsApproval" izni verilmelidir.

### <a name="limitations-to-private-dns-zone-integration"></a>Özel DNS bölge tümleştirmesi için sınırlamalar

Özel bir bitiş noktasını sildiğinizde veya bir bölgeyi Azure Cosmos hesabından kaldırdığınızda, özel DNS bölgesindeki DNS kayıtları otomatik olarak kaldırılmaz. DNS kayıtlarını önce el ile kaldırmanız gerekir:

* Bu özel DNS bölgesine bağlı yeni bir özel bitiş noktası ekleme.
* Bu özel DNS bölgesine bağlı özel uç noktaları olan tüm veritabanı hesabına yeni bir bölge ekleme.

DNS kayıtlarını temizlemezseniz, beklenmeyen veri düzlemi sorunları ortaya çıkabilir. Bu sorunlar, özel uç nokta kaldırma veya bölge kaldırma sonra eklenen bölgelere veri kesintisi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB güvenlik özellikleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* Azure Cosmos DB için bir güvenlik duvarı yapılandırmak için [Bkz. Güvenlik Duvarı desteği.](firewall-support.md)

* Azure Cosmos hesabınız için sanal ağ hizmeti bitiş noktasını nasıl yapılandırıştırılaceksiniz öğrenmek için [bkz.](how-to-configure-vnet-service-endpoint.md)

* Private Link hakkında daha fazla bilgi edinmek için [Azure Özel Bağlantı](../private-link/private-link-overview.md) belgelerine bakın.
