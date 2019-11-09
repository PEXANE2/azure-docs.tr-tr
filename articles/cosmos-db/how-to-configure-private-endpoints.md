---
title: Azure Cosmos hesabı için Azure özel bağlantısını yapılandırma
description: Bir sanal ağdaki özel IP adresini kullanarak bir Azure Cosmos hesabına erişmek için Azure özel bağlantısı ayarlamayı öğrenin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: thweiss
ms.openlocfilehash: 3f987b9e05bcdcda9afe26a1eb1354e5e2450ac5
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73846531"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account-preview"></a>Azure Cosmos hesabı için Azure özel bağlantısını yapılandırma (Önizleme)

Azure özel bağlantısı ' nı kullanarak bir Azure Cosmos hesabına özel bir uç nokta aracılığıyla bağlanabilirsiniz. Özel uç nokta, sanal ağınız içindeki bir alt ağda bulunan özel IP adresleri kümesidir. Özel bağlantı kullanarak, belirli bir Azure Cosmos hesabına erişimi özel IP adresleri üzerinden sınırlayabilirsiniz. Kısıtlanmış NSG ilkeleriyle birleştirildiğinde, özel bağlantı, veri ayıklanmasının riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel bağlantı](../private-link/private-link-overview.md) makalesi.

Ayrıca, özel bağlantı, bir Azure Cosmos hesabına sanal ağ veya eşlenmiş sanal ağ içinden erişilmesini sağlar. Özel bağlantıyla eşlenen kaynaklara, VPN veya ExpressRoute aracılığıyla özel eşleme üzerinden şirket içinden de erişilebilir. 

"Otomatik" veya "El Ile" onay yöntemlerini kullanarak, özel bağlantıyla yapılandırılmış bir Azure Cosmos hesabına bağlanabilirsiniz. Daha fazla bilgi edinmek için özel bağlantı belgelerinin [onay iş akışı](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) bölümüne bakın. Bu makalede, otomatik onay yöntemi kullandığınızı varsayarak özel bir bağlantı oluşturma adımları açıklanır.

## <a name="create-a-private-link-using-the-azure-portal"></a>Azure portal kullanarak özel bağlantı oluşturma

Azure portal kullanarak mevcut bir Azure Cosmos hesabı için özel bir bağlantı oluşturmak üzere aşağıdaki adımları kullanın:

1. **Tüm kaynaklar** bölmesinden, güvenli hale getirmek istediğiniz Azure Cosmos DB hesabını bulun.

1. Ayarlar menüsünden **Özel uç nokta bağlantısı** ' nı seçin ve **Özel uç nokta** seçeneğini belirleyin:

   ![Azure portal kullanarak özel uç nokta oluşturma](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. **Özel uç nokta oluştur (Önizleme) – temel bilgiler**bölmesinde, aşağıdaki ayrıntıları girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | Kaynak grubunu seçin.|
    | **Örnek ayrıntıları** |  |
    | Ad | Özel uç noktanız için herhangi bir ad girin; Bu ad alındıysanız, benzersiz bir tane oluşturun. |
    |Bölge| Özel bağlantıyı dağıtmak istediğiniz bölgeyi seçin. Özel uç noktanın, sanal ağınızın bulunduğu konumda oluşturulması gerekir.|
    |||
1. **Sonraki: kaynak**' ı seçin.
1. **Özel uç nokta oluştur-kaynak**bölümünde bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimde bir Azure kaynağına bağlan ' ı seçin. <br/><br/> Bu seçenek, bir özel bağlantı kurmak veya sizinle paylaştığınız bir kaynak KIMLIĞI ya da diğer ad ile başka birisinin kaynağına bağlanmak için kaynaklarınızdan birini seçmenize olanak sağlar.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft. Azumisinizsmosdb/databaseAccounts**' ı seçin. |
    | Kaynak |Azure Cosmos hesabınızı seçin |
    |Hedef alt kaynak |Eşlemek istediğiniz Cosmos DB API türünü seçin. Bu, varsayılan olarak SQL, MongoDB ve Cassandra API 'Leri için yalnızca bir seçeneğe sahiptir. Gremlin ve tablo API 'Leri için, bu API 'Ler SQL API 'SI ile birlikte çalışabilen *SQL* ' i de seçebilirsiniz. |
    |||

1. Ileri 'yi seçin **: yapılandırma**.
1. **Özel uç nokta (Önizleme) oluştur-yapılandırma**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ**| |
    | Sanal ağ| Sanal ağınızı seçin. |
    | Alt ağ | Alt ağlarınızı seçin. |
    |**Özel DNS tümleştirme**||
    |Özel DNS bölgesiyle tümleştirin |**Evet**' i seçin. <br><br/> Özel uç noktanıza özel olarak bağlanmak için bir DNS kaydına ihtiyacınız vardır. Özel uç noktanızı özel bir DNS bölgesi ile tümleştirmeniz önerilir. Ayrıca, kendi DNS sunucularınızı kullanabilir veya sanal makinelerinizdeki konak dosyalarını kullanarak DNS kayıtları oluşturabilirsiniz. |
    |Özel DNS bölgesi |*Privatelink.Documents.Azure.com* seçin <br><br/> Özel DNS bölgesi otomatik olarak belirlenir ve şu anda Azure portal kullanılarak değiştirilemez.|
    |||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.
1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

Bir Azure Cosmos hesabı için özel bağlantıları onayladıysanız, **güvenlik duvarı ve sanal ağlar** bölmesindeki **tüm ağlar** seçeneğinin Azure Portal gri renkte olduğunu görürsünüz.

Aşağıdaki tabloda, farklı Azure Cosmos hesabı API türleri, desteklenen alt kaynaklar ve karşılık gelen özel bölge adları arasındaki eşleme gösterilmektedir. Gremlin ve Tablo API'si hesaplarına SQL API aracılığıyla erişilebilir. bu nedenle, bu API 'Ler için 2 giriş vardır:

|Azure Cosmos hesabı API türü  |Desteklenen alt kaynaklar (veya Groupıds) |Özel bölge adı  |
|---------|---------|---------|
|Sql    |   Sql      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  Sql       |  privatelink.documents.azure.com    |
|Tablo    |    Tablo     |   privatelink.table.cosmos.azure.com    |
|Tablo     |   Sql      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>Özel IP adreslerini getir

Özel uç nokta sağlandıktan sonra IP adreslerini sorgulayabilirsiniz. Azure portal IP adreslerini görüntülemek için **tüm kaynaklar**' ı seçin, daha önce oluşturduğunuz özel uç noktasını bu durumda "dbPrivateEndpoint3" olarak ARAYıN ve DNS AYARLARıNı ve IP adreslerini görmek Için genel bakış sekmesini seçin:

![Azure portal özel IP adresleri](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

Özel uç nokta başına birden çok IP adresi oluşturuldu:

* Azure Cosmos hesabının genel (bölge belirsiz) uç noktası için bir tane.
* Azure Cosmos hesabının dağıtıldığı her bölge için bir tane.

## <a name="create-a-private-link-using-azure-powershell"></a>Azure PowerShell kullanarak özel bağlantı oluşturma

Mevcut bir Azure Cosmos hesabı için "MyPrivateEndpoint" adlı özel bir bağlantı oluşturmak için aşağıdaki Powerşehir betiğini çalıştırın. Değişken değerlerini ortamınıza özgü ayrıntılarla değiştirdiğinizden emin olun.

```azurepowershell-interactive
Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Cosmos DB account: Sql or MongoDB or Cassandra or Gremlin or Table
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
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

### <a name="integrate-the-private-endpoint-with-private-dns-zone"></a>Özel uç noktayı özel DNS bölgesiyle tümleştirme

Özel uç noktasını oluşturduktan sonra, aşağıdaki Powerşehir betiğini kullanarak özel bir DNS bölgesi ile tümleştirilebilir:

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

### <a name="fetch-the-private-ip-addresses"></a>Özel IP adreslerini getir

Özel uç nokta sağlandıktan sonra, aşağıdaki PowerShell betiğini kullanarak IP adreslerini ve FQDN eşlemesini sorgulayabilirsiniz:

```azurepowershell-interactive

$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-link-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak özel bağlantı oluşturma

Bir sanal ağ alt ağında özel bir uç nokta oluşturarak özel bir bağlantı kurabilirsiniz. Bu, bir Azure Resource Manager şablonu kullanılarak elde edilir. Aşağıdaki kodla "PrivateEndpoint_template. JSON" adlı bir Kaynak Yöneticisi şablonu oluşturun. Bu şablon var olan bir sanal ağda mevcut bir Azure Cosmos SQL API hesabı için özel bir uç nokta oluşturur:

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

### <a name="define-the-template-parameters-file"></a>Şablon parametreleri dosyasını tanımlayın

Şablon için bir parametre dosyası oluşturun ve "PrivateEndpoint_parameters. JSON" olarak adlandırın. Aşağıdaki kodu parametreler dosyasına ekleyin:

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

### <a name="deploy-the-template-by-using-a-powershell-script"></a>Bir PowerShell betiği kullanarak şablonu dağıtma

Ardından aşağıdaki kodla bir PowerShell betiği oluşturun. Betiği çalıştırmadan önce, abonelik KIMLIĞINI, kaynak grubu adını ve diğer değişken değerlerini ortamınıza özgü ayrıntılarla değiştirdiğinizden emin olun:

```azurepowershell-interactive
### This script creates a private endpoint for an existing Cosmos DB account in an existing VNet

## Step 1: Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Cosmos DB account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
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

PowerShell betiğinde, "GroupID" değişkeni yalnızca bir değer içerebilir. Bu, hesabın API türüdür. İzin verilen değerler şunlardır: SQL, MongoDB, Cassandra, Gremlin ve Table. Bazı Azure Cosmos hesap türlerine birden çok API aracılığıyla erişilebilir. Örneğin:

* Gremlin API hesabına hem Gremlin hem de SQL API hesaplarından erişilebilir.
* Tablo API'si hesaba hem tablo hem de SQL API hesaplarından erişilebilir.

Bu tür hesaplar için, "GroupID" dizisinde belirtilen API türü ile her API türü için bir özel uç nokta oluşturmanız gerekir.

Şablon başarıyla dağıtıldıktan sonra, aşağıdaki görüntüde gösterilene benzer bir çıkış görebilirsiniz. Özel uç noktalar doğru ayarlandıysa, provisioningState değeri "başarılı" olur.

![Kaynak Yöneticisi şablonu dağıtım çıkışı](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

Şablon dağıtıldıktan sonra, özel IP adresleri alt ağ içinde ayrılır. Azure Cosmos hesabının güvenlik duvarı kuralı yalnızca özel uç noktadan gelen bağlantıları kabul edecek şekilde yapılandırılmıştır.

## <a name="configure-custom-dns"></a>Özel API'yi yapılandırma

Özel uç noktanın oluşturulduğu alt ağ içinde özel bir DNS kullanmanız gerekir. Ve uç noktaları, her bir özel IP adresinin bir DNS girdisiyle eşlenmesi için yapılandırın (yukarıda gösterilen yanıtta "FQDN 'ler" özelliğine bakın).

Özel uç nokta oluştururken, Azure 'da özel bir DNS bölgesi ile tümleştirilebilir. Özel uç noktanızı Azure 'daki özel DNS bölgesiyle tümleştirmeyi ve bunun yerine özel bir DNS kullanmayı tercih ederseniz, DNS 'nizi özel uç nokta için ayrılan tüm özel IP adresleri için DNS kayıtları ekleyecek şekilde yapılandırmanız gerekir.

## <a name="firewall-configuration-with-private-link"></a>Özel bağlantıyla güvenlik duvarı yapılandırması

Özel bağlantıyı güvenlik duvarı kurallarıyla birlikte kullandığınızda aşağıdakiler farklı durumlar ve sonuçlar aşağıda verilmiştir:

* Yapılandırılmış bir güvenlik duvarı kuralı yoksa, varsayılan olarak, tüm trafikte bir Azure Cosmos hesabına erişilebilir.

* Ortak trafik veya hizmet uç noktası yapılandırıldıysa ve özel uç noktalar oluşturulduysa, ilgili güvenlik duvarı kuralı tarafından farklı gelen trafik türlerine izin verilir.

* Ortak trafik veya hizmet uç noktası yapılandırılmamışsa ve özel uç noktalar oluşturulmadıysa, Azure Cosmos hesabına yalnızca özel uç noktalar aracılığıyla erişilebilir. Ortak trafik veya hizmet uç noktası yapılandırılmamışsa, tüm onaylanan özel uç noktalar reddedildikten veya silindikten sonra hesap tüm ağa açıktır.

## <a name="update-private-endpoint-when-you-add-or-remove-a-region"></a>Bölge eklediğinizde veya kaldırdığınızda özel uç noktayı güncelleştirme

Azure Cosmos hesabına bölge ekleme veya kaldırma, bu hesaba ait DNS girdilerini eklemenizi veya kaldırmanızı gerektirir. Bu değişiklikler özel uç noktada uygun şekilde güncellenmelidir. Şu anda aşağıdaki adımları kullanarak bu değişikliği el ile yapmalısınız:

1. Azure Cosmos DB Yöneticisi bölgeleri ekler veya kaldırır. Ardından, ağ yöneticilerine bekleyen değişiklikler hakkında bildirim yapılır. Bir Azure Cosmos hesabıyla eşlenen özel uç nokta, "ActionsRequired" özelliklerinin "none" iken "yeniden oluştur" olarak değiştirildiğini görür. Daha sonra, ağ yöneticisi, aynı Kaynak Yöneticisi yükü oluşturmak için kullanılan bir PUT isteği vererek özel uç noktasını güncelleştirir.

1. Bu işlemden sonra, alt ağın özel DNS 'in de eklenen veya kaldırılan DNS girişlerini ve bunlara karşılık gelen özel IP adreslerini yansıtacak şekilde güncellenmesi gerekir.

Örneğin, 3 bölgede bir Azure Cosmos hesabı dağıtırsanız: "Batı ABD", "Orta ABD" ve "Batı Avrupa". Hesabınız için özel bir uç nokta oluşturduğunuzda, alt ağda 4 özel IP ayrılır. Her bölge için bir, toplam 3 ve diğeri ise küresel/bölge agtik uç noktası için.

Daha sonra yeni bir bölge eklerseniz, örneğin, Azure Cosmos hesabına "Doğu ABD". Varsayılan olarak, yeni bölgeye mevcut özel uç noktadan erişilemez. Azure Cosmos Hesap Yöneticisi, yeni bölgeden erişmeden önce özel uç nokta bağlantısını yenilemelidir. 

` Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>` komutunu çalıştırdığınızda komutun çıktısı, "yeniden oluştur" olarak ayarlanan `actionsRequired` parametresini içerir. Bu değer, Özel uç noktanın yenilenmesi gerektiğini gösterir. Daha sonra Azure Cosmos Hesap Yöneticisi, Özel uç nokta yenilemeyi tetiklemek için `Set-AzPrivateEndpoint` komutunu çalıştırır.

```powershell
$pe = Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>

Set-AzPrivateEndpoint -PrivateEndpoint $pe
```

Bu özel uç nokta altındaki alt ağda yeni bir özel IP otomatik olarak ayrılır ve `actionsRequired` değer `None`olur. Herhangi bir özel DNZ bölge tümleştirmesi yoksa (başka bir deyişle, özel bir özel DNS kullanıyorsanız), özel DNS 'nizi yeni bölgeye karşılık gelen özel IP için yeni bir DNS kaydı ekleyecek şekilde yapılandırmanız gerekir.

Bir bölgeyi kaldırırken de aynı adımları kullanabilirsiniz. Kaldırılan bölgenin özel IP 'si otomatik olarak geri kazanılır ve `actionsRequired` bayrağı `None`olur. Özel DNZ bölge tümleştirmesi yoksa, özel DNS 'yi, kaldırılan bölgenin DNS kaydını kaldıracak şekilde yapılandırmanız gerekir.

Özel bir uç nokta silindiğinde veya Azure Cosmos hesabından bir bölge kaldırıldığında özel DNS bölgesindeki DNS kayıtları otomatik olarak kaldırılmaz. DNS kayıtlarını el ile kaldırmanız gerekir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Özel bağlantıyı bir Azure Cosmos hesabıyla kullanırken aşağıdaki sınırlamalar geçerlidir:

* Azure Cosmos hesapları ve sanal ağlar için özel bağlantı desteği yalnızca belirli bölgelerde kullanılabilir. Desteklenen bölgelerin listesi için özel bağlantı makalesinin [kullanılabilir bölgeler](../private-link/private-link-overview.md#availability) bölümüne bakın. **Hem VNET hem de Azure Cosmos hesabının, özel bir uç nokta oluşturabilmek için desteklenen bölgelerde olması gerekir**.

* Doğrudan mod bağlantısı kullanarak Azure Cosmos hesabıyla özel bağlantılar kullanırken, yalnızca TCP protokolünü kullanabilirsiniz. HTTP protokolü henüz desteklenmiyor

* MongoDB hesapları için Azure Cosmos DB API 'SI kullanılırken, Özel uç nokta yalnızca sunucu sürümü 3,6 (Bu, `*.mongo.cosmos.azure.com`biçiminde uç noktası kullanan hesaplardır) için desteklenir. Özel bağlantı, sunucu sürüm 3,2 (`*.documents.azure.com`biçiminde uç nokta kullanan hesaplar) hesapları için desteklenmez. Özel bağlantıyı kullanmak için eski hesapları yeni sürüme geçirmeniz gerekir.

* Özel bağlantısı olan MongoDB hesapları için Azure Cosmos DB API 'SI kullanılırken, Robo 3T, Studio 3T, Mongoose vb. gibi araçları kullanamazsınız. Uç noktanın yalnızca appName =<account name> parametresi belirtilmişse özel bağlantı desteği olabilir. Örneğin: replicaSet = globaldb & appName = mydbaccountname. Bu araçlar, bağlantı dizesindeki uygulama adını hizmete geçirmediğinden özel bağlantıyı kullanamazsınız. Bununla birlikte, 3,6 sürümüyle SDK sürücüleriyle bu hesaplara erişmeye devam edebilirsiniz.

* Bir sanal ağ, özel bağlantı içeriyorsa taşınamaz veya silinemez.

* Bir Azure Cosmos hesabı, özel bir uç noktaya eklenmişse silinemez.

* Hesaba bağlı olan tüm özel uç noktalara eşlenmemiş bir bölgeye Azure Cosmos hesabı devredilemez. Daha fazla bilgi için bkz. önceki bölümde bölge ekleme veya kaldırma.

* Bir yönetici tarafından otomatik olarak onaylanan özel uç noktalar oluşturmak üzere bir yöneticinin Azure Cosmos hesap kapsamında en az "*/PrivateEndpointConnectionsApproval" iznine sahip olması gerekir.

### <a name="private-dns-zone-integration-limitations"></a>Özel DNS bölge tümleştirmesi sınırlamaları

Özel bir uç nokta silindiğinde veya Azure Cosmos hesabından bir bölge kaldırıldığında özel DNS bölgesindeki DNS kayıtları otomatik olarak kaldırılmaz. Önce DNS kayıtlarını el ile kaldırmanız gerekir:

* Bu özel DNS bölgesine bağlı yeni bir özel uç nokta ekleniyor.
* Özel uç noktaları bu özel DNS bölgesine bağlı olan herhangi bir veritabanı hesabına yeni bir bölge ekleme.

DNS kayıtlarını temizlemeden, Özel uç nokta kaldırma veya bölge kaldırma işleminden sonra eklenen bölgelere veri kesintisi gibi beklenmedik veri düzlemi sorunları meydana gelebilir

## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure Cosmos DB güvenlik özellikleri hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

* Azure Cosmos DB için bir güvenlik duvarı yapılandırmak için bkz. [güvenlik duvarı desteği](firewall-support.md).

* [Azure Cosmos hesabınız için sanal ağ hizmet uç noktasını yapılandırma.](how-to-configure-vnet-service-endpoint.md)

* Özel bağlantı hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantı](../private-link/private-link-overview.md) belgeleri.
