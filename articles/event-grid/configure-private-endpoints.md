---
title: Azure Olay Ağı konuları veya etki alanları için özel uç noktaları yapılandırma
description: Bu makalede, Azure Olay Izgara konuları veya etki alanı için özel uç noktaların nasıl yapılandırılabildiğini açıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: d08afe00c13a3f96b9526c3cb29804cfad688ddc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299913"
---
# <a name="configure-private-endpoints-for-azure-event-grid-topics-or-domains-preview"></a>Azure Olay Ağı konuları veya etki alanları için özel uç noktaları yapılandırma (Önizleme)
Genel internet üzerinden geçmeden özel bir bağlantı üzerinden [güvenli](../private-link/private-link-overview.md) bir şekilde sanal ağınızdan konulara ve etki alanlarınıza doğrudan olayların girişine izin vermek için [özel uç noktaları](../private-link/private-endpoint-overview.md) kullanabilirsiniz. Özel bitiş noktası, konunuz veya etki alanınız için VNet adres alanından bir IP adresi kullanır. Daha fazla kavramsal bilgi için [Ağ güvenliğine](network-security.md)bakın.

Bu makalede, konular veya etki alanları için özel uç noktaların nasıl yapılandırılabildiğini açıklanmaktadır.

> [!IMPORTANT]
> Özel uç noktalar özelliği, yalnızca premium katmandaki konular ve etki alanları için kullanılabilir. Temel katmandan premium katmana yükseltmek için [fiyatlandırma katmanını güncelleştir](update-tier.md) makalesine bakın. 

## <a name="use-azure-portal"></a>Azure portalı kullanma 
Bu bölümde, bir konu veya etki alanı için özel bir bitiş noktası oluşturmak için Azure portalını nasıl kullanacağınızı gösterir.

> [!NOTE]
> Bu bölümde gösterilen adımlar çoğunlukla konular içindir. **Etki alanları**için özel uç noktalar oluşturmak için benzer adımları kullanabilirsiniz. 

1. [Azure portalında](https://portal.azure.com) oturum açın ve konunuza veya etki alanınıza gidin.
2. Konu sayfanızın **Ağ** sekmesine geçin. Araç çubuğunda **+ Özel bitiş noktası'nı** seçin.

    ![Özel bitiş noktası ekleme](./media/configure-private-endpoints/add-button.png)
2. Temel **Bilgiler** sayfasından biri, aşağıdaki adımları izleyin: 
    1. Özel bitiş noktasını oluşturmak istediğiniz bir **Azure aboneliği** seçin. 
    2. Özel bitiş noktası için bir **Azure kaynak grubu** seçin. 
    3. Bitiş noktası için bir **ad** girin. 
    4. Bitiş noktası için **bölgeyi** seçin. Özel bitiş noktanız sanal ağınızla aynı bölgede olmalıdır, ancak özel bağlantı kaynağından farklı bir bölgede olabilir (bu örnekte, bir olay ızgarası konusu). 
    5. Ardından, **Sonraki: Sayfanın** altındaki kaynak >düğmesini seçin. 

      ![Özel bitiş noktası - temel ler sayfası](./media/configure-private-endpoints/basics-page.png)
3. **Kaynak** sayfasında aşağıdaki adımları izleyin: 
    1. Bağlantı yöntemi için, **dizinimdeki bir Azure kaynağına Bağlan'ı**seçerseniz aşağıdaki adımları izleyin. Bu örnek, dizininizdeki bir Azure kaynağına nasıl bağlanılabildiğini gösterir. 
        1. **Konunun/etki alanınızın** bulunduğu **Azure aboneliğini** seçin. 
        1. **Kaynak türü**için, **Kaynak türü**için **Microsoft.EventGrid/topics** veya **Microsoft.EventGrid/etki alanlarını** seçin.
        2. **Kaynak**için açılan listeden bir konu/etki alanı seçin. 
        3. **Hedef alt kaynağının** **konu** veya **etki alanına** ayarlı olduğunu onaylayın (seçtiğiniz kaynak türüne göre).    
        4. **İleri'yi seçin:** Sayfanın altındaki Configuration >düğmesi. 

            ![Özel bitiş noktası - kaynak sayfası](./media/configure-private-endpoints/resource-page.png)
    2. **Kaynak kimliği veya takma ad kullanarak kaynağa Bağlan'ı**seçerseniz aşağıdaki adımları izleyin:
        1. Kaynağın kimliğini girin. Örneğin: `/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>`.  
        2. **Kaynak** **için, konu** veya **etki alanı**girin. 
        3. (isteğe bağlı) İstek iletisi ekleyin. 
        4. **İleri'yi seçin:** Sayfanın altındaki Configuration >düğmesi. 

            ![Özel bitiş noktası - kaynak sayfası](./media/configure-private-endpoints/connect-azure-resource-id.png)
4. **Yapılandırma** sayfasında, sanal ağdaki alt ağı özel bitiş noktasını dağıtmak istediğiniz yere seçersiniz. 
    1. Sanal **ağ**seçin. Yalnızca seçili abonelik ve konumdaki sanal ağlar açılır listede listelenir. 
    2. Seçtiğiniz sanal ağda bir **alt ağ** seçin. 
    3. **Sonrakini Seçin: Etiketler sayfanın** altındaki >düğmesini etiketler. 

    ![Özel bitiş noktası - yapılandırma sayfası](./media/configure-private-endpoints/configuration-page.png)
5. **Etiketler** sayfasında, özel bitiş noktası kaynağıyla ilişkilendirmek istediğiniz etiketleri (adlar ve değerler) oluşturun. Ardından, sayfanın altındaki **Gözden Geçir + oluştur** düğmesini seçin. 
6. Gözden **Geçir + oluştur,** tüm ayarları gözden geçir ve özel bitiş noktasını oluşturmak için **Oluştur'u** seçin. 

    ![Özel bitiş noktası - sayfa yı & gözden geçirin](./media/configure-private-endpoints/review-create-page.png)
    

## <a name="manage-private-link-connection"></a>Özel bağlantı bağlantısını yönetme

Özel bir bitiş noktası oluşturduğunuzda, bağlantının onaylanması gerekir. Özel bir bitiş noktası oluşturduğunuz kaynak dizininizdeyse, yeterli izine sahip olmak koşuluyla bağlantı isteğini onaylayabilirsiniz. Başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bağlantı isteğinizi bu kaynağın sahibinin onaylamasını beklemeniz gerekir.

Dört hüküm veren durum vardır:

| Hizmet eylemi | Hizmet tüketici özel bitiş noktası durumu | Açıklama |
|--|--|--|
| None | Beklemede | Bağlantı el ile oluşturulur ve özel Bağlantı kaynak sahibinin onayını bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanıma hazır. |
| Reddet | Reddedilen | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantı kesildi | Bağlantı özel bağlantı kaynak sahibi tarafından kaldırıldı, özel bitiş noktası bilgilendirici hale gelir ve temizleme için silinmelidir. |
 
###  <a name="how-to-manage-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını yönetme
Aşağıdaki bölümler, özel bir bitiş noktası bağlantısını nasıl onaylayacağınızı veya reddedeceğinizi gösterir. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Arama **çubuğunda, Olay Izgara konularını** veya **Olay Izgara etki alanlarını**yazın.
1. Yönetmek istediğiniz **konuyu** veya **etki alanını** seçin.
1. **Ağ** sekmesini seçin.
1. Bekleyen herhangi bir bağlantı varsa, sağlama durumunda **Bekleyen** ile listelenen bir bağlantı görürsünüz. 

### <a name="to-approve-a-private-endpoint"></a>Özel bir bitiş noktasını onaylamak için
Bekleme durumunda olan özel bir bitiş noktasını onaylayabilirsiniz. Onaylamak için aşağıdaki adımları izleyin: 

> [!NOTE]
> Bu bölümde gösterilen adımlar çoğunlukla konular içindir. **Etki alanları**için özel uç noktaları onaylamak için benzer adımları kullanabilirsiniz. 

1. Onaylamak istediğiniz **özel bitiş noktasını** seçin ve araç çubuğunda **Onayla'yı** seçin.

    ![Özel bitiş noktası - bekleyen durum](./media/configure-private-endpoints/pending.png)
1. Bağlantı iletişim kutusunu **onayla** kutusuna bir açıklama girin (isteğe bağlı) girin ve **Evet'i**seçin. 

    ![Özel bitiş noktası - onaylayın](./media/configure-private-endpoints/approve.png)
1. Bitiş noktasının durumunu **Onaylı**olarak gördüğünüzü onaylayın. 

    ![Özel bitiş noktası - onaylı durum](./media/configure-private-endpoints/approved-status.png)

### <a name="to-reject-a-private-endpoint"></a>Özel bir bitiş noktasını reddetmek için
Bekleyen durumda veya onaylanan durumda olan özel bir bitiş noktasını reddedebilirsiniz. Reddetmek için aşağıdaki adımları izleyin: 

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için özel uç noktaları reddetmek için benzer adımları kullanabilirsiniz. 

1. Reddetmek istediğiniz **özel bitiş noktasını** seçin ve araç çubuğunda **Reddet'i** seçin.

    ![Özel bitiş noktası - reddetmek](./media/configure-private-endpoints/reject-button.png)
1. Bağlantıyı **Reddet** iletişim kutusunda bir açıklama girin (isteğe bağlı) ve **Evet'i**seçin. 

    ![Özel bitiş noktası - reddetmek](./media/configure-private-endpoints/reject.png)
1. Bitiş noktasının durumunu **Reddedildi**olarak gördüğünüzü doğrulayın. 

    ![Özel bitiş noktası - reddedilen durum](./media/configure-private-endpoints/rejected-status.png)

    > [!NOTE]
    > Azure portalında özel bir bitiş noktasını reddedildikten sonra onaylayamazsınız. 


## <a name="use-azure-cli"></a>Azure CLI kullanma
Özel bir bitiş noktası oluşturmak için, aşağıdaki örnekte gösterildiği gibi [az ağ özel uç noktası oluşturma](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create) yöntemini kullanın:

```azurecli-interactive
az network private-endpoint create \
    --resource-group <RESOURECE GROUP NAME> \
    --name <PRIVATE ENDPOINT NAME> \
    --vnet-name <VIRTUAL NETWORK NAME> \
    --subnet <SUBNET NAME> \
    --private-connection-resource-id "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME> \
    --connection-name <PRIVATE LINK SERVICE CONNECTION NAME> \
    --location <LOCATION> \
    --group-ids topic
```

Örnekte kullanılan parametrelerin açıklamaları için, [az ağı özel bitiş noktası oluşturma](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create)belgelerine bakın. Bu örnekte dikkat edilmesi gereken birkaç nokta şunlardır: 

- Bunun `private-connection-resource-id` **için, konu** nun veya **etki alanının**kaynak kimliğini belirtin. Önceki örnek türü kullanır: konu.
- for `group-ids`, `topic` `domain`belirt in veya . Önceki örnekte, `topic` kullanılır. 

Özel bir bitiş noktasını silmek için, aşağıdaki örnekte gösterildiği gibi [az ağ özel uç nokta silme](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-delete) yöntemini kullanın:

```azurecli-interactive
az network private-endpoint delete --resource-group <RESOURECE GROUP NAME> --name <PRIVATE ENDPOINT NAME>
```

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için özel uç noktalar oluşturmak için benzer adımları kullanabilirsiniz. 

### <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma
Aşağıda, aşağıdaki Azure kaynaklarını oluşturan örnek bir komut dosyası vereme ve özel bir komut dosyası vereme

- Kaynak grubu
- Sanal ağ
- Sanal ağda alt ağ
- Azure Olay Izgara konusu (premium katman)
- Konu için özel bitiş noktası

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. Etki alanları için özel uç noktalar oluşturmak için benzer adımları kullanabilirsiniz.

```azurecli-interactive
subscriptionID="<AZURE SUBSCRIPTION ID>"
resourceGroupName="<RESOURCE GROUP NAME>"
location="<LOCATION>"
vNetName="<VIRTUAL NETWORK NAME>"
subNetName="<SUBNET NAME>"
topicName = "<TOPIC NAME>"
connectionName="<ENDPOINT CONNECTION NAME>"
endpointName=<ENDPOINT NAME>

# URI for the topic. replace <SUBSCRIPTION ID>, <RESOURCE GROUP NAME>, and <TOPIC NAME>
topicUri="/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME>?api-version=2020-04-01-preview"

# resource ID of the topic. replace <SUBSCRIPTION ID>, <RESOURCE GROUP NAME>, and <TOPIC NAME>
topicResourceID="/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME>"

# select subscription
az account set --subscription $subscriptionID

# create resource group
az group create --name $resourceGroupName --location $location

# create vnet 
az network vnet create \
    --resource-group $resourceGroupName \
    --name $vNetName \
    --address-prefix 10.0.0.0/16

# create subnet
az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $vNetName \
    --name $subNetName \
    --address-prefixes 10.0.0.0/24

# disable private endpoint network policies for the subnet
az network vnet subnet update \
    --resource-group $resourceGroupName \
    --vnet-name $vNetName \
    --name $subNetName \
    --disable-private-endpoint-network-policies true

# create event grid topic. update <LOCATION>
az rest --method put \
    --uri $topicUri \
    --body "{\""location\"":\""LOCATION\"", \""sku\"": {\""name\"": \""premium\""}, \""properties\"": {\""publicNetworkAccess\"":\""Disabled\""}}"

# verify that the topic was created.
az rest --method get \
    --uri $topicUri

# create private endpoint for the topic you created
az network private-endpoint create 
    --resource-group $resourceGroupName \
    --name $endpointName \
    --vnet-name $vNetName \
    --subnet $subNetName \
    --private-connection-resource-id $topicResourceID \
    --connection-name $connectionName \
    --location $location \
    --group-ids topic

# get topic 
az rest --method get \
    --uri $topicUri

```

### <a name="approve-a-private-endpoint-connection"></a>Özel bir bitiş noktası bağlantısını onaylama
Aşağıdaki örnek CLI parçacığı, özel bir bitiş noktası bağlantısını nasıl onaylayacağınızı gösterir. 

```azurecli-interactive
az rest --method put --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" --body "{\""properties\"":{\""privateLinkServiceConnectionState\"": {\""status\"":\""approved\"",\""description\"":\""connection approved\"", \""actionsRequired\"": \""none\""}}}"
```


### <a name="reject-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını reddetme
Aşağıdaki örnek CLI parçacığı, özel bir bitiş noktası bağlantısını nasıl reddedeceğinizi gösterir. 

```azurecli-interactive
az rest --method put --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" --body "{\""properties\"":{\""privateLinkServiceConnectionState\"": {\""status\"":\""rejected\"",\""description\"":\""connection rejected\"", \""actionsRequired\"": \""none\""}}}"
```


## <a name="use-powershell"></a>PowerShell kullanma
Bu bölümde, PowerShell'i kullanarak bir konu veya etki alanı için özel bir bitiş noktası nın nasıl oluşturulabileceğiniz gösterilmektedir. 

### <a name="prerequisite"></a>Önkoşul
Nasıl yapılır' dan yönergeleri izleyin: Azure Etkin Dizin uygulaması oluşturmak ve **Dizin (kiracı) Kimliği**, **Uygulama (İstemci) Kimliği**ve Uygulama **(istemci) sırrı**değerlerini not almak için [kaynaklara erişebilen bir Azure REKLAM uygulaması](../active-directory/develop/howto-create-service-principal-portal.md) ve hizmet ilkesi oluşturmak için portalı kullanın. 

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>REST API çağrıları için belirteç ve üstbilgi hazırlayın 
REST API çağrıları, yetkilendirme ve diğer üstbilgi bilgileriyle kullanılacak bir kimlik doğrulama belirteci almak için aşağıdaki ön koşul komutlarını çalıştırın. 

```azurepowershell-interactive
$body = "grant_type=client_credentials&client_id=<CLIENT ID>&client_secret=<CLIENT SECRET>&resource=https://management.core.windows.net"

# get authentication token
$Token = Invoke-RestMethod -Method Post `
    -Uri https://login.microsoftonline.com/<TENANT ID>/oauth2/token  `
    -Body $body  `
    -ContentType 'application/x-www-form-urlencoded' 

# set authorization and content-type headers
$Headers = @{}
$Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
```

### <a name="create-a-subnet-with-endpoint-network-policies-disabled"></a>Uç nokta ağ ilkeleri devre dışı bırakılmış bir alt ağ oluşturma

```azurepowershell-interactive

# create resource group
New-AzResourceGroup -ResourceGroupName <RESOURCE GROUP NAME>  -Location <LOCATION>

# create virtual network
$virtualNetwork = New-AzVirtualNetwork `
                    -ResourceGroupName <RESOURCE GROUP NAME> `
                    -Location <LOCATION> `
                    -Name <VIRTUAL NETWORK NAME> `
                    -AddressPrefix 10.0.0.0/16

# create subnet with endpoint network policy disabled
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
                    -Name example-privatelinksubnet `
                    -AddressPrefix 10.0.0.0/24 `
                    -PrivateEndpointNetworkPoliciesFlag "Disabled" `
                    -VirtualNetwork $virtualNetwork

# update virtual network
$virtualNetwork | Set-AzVirtualNetwork
```

### <a name="create-an-event-grid-topic-with-a-private-endpoint"></a>Özel bitiş noktası olan bir olay ızgarası konusu oluşturma

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için özel uç noktalar oluşturmak için benzer adımları kullanabilirsiniz. 


```azurepowershell-interactive
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="premium"}; "properties"=@{"publicNetworkAccess"="disabled"}} | ConvertTo-Json

# create topic
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $body

# verify that the topic was created
$topic=Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview"   `
    -Headers $Headers  

# create private link service connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name "<PRIVATE LINK SERVICE CONNECTION NAME>" `
                                -PrivateLinkServiceId $topic.Id `
                                -GroupId "topic"

# get virtual network info 
$virtualNetwork = Get-AzVirtualNetwork  `
                    -ResourceGroupName  <RESOURCE GROUP NAME> `
                    -Name <VIRTUAL NETWORK NAME>

# get subnet info
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                             | Where-Object  {$_.Name -eq <SUBNET NAME>}  

# now, you are ready to create a private endpoint 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName <RESOURCE GROUP NAME>  `
                                        -Name <PRIVATE ENDPOINT NAME>   `
                                        -Location <LOCATION> `
                                        -Subnet  $subnet   `
                                        -PrivateLinkServiceConnection $privateEndpointConnection

# verify that the endpoint was created
Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections?api-version=2020-04-01-preview"   `
    -Headers $Headers   `
    | ConvertTo-Json -Depth 5

```

Bitiş noktasının oluşturulduğunu doğrularken, sonucu aşağıdakilere benzer şekilde görmeniz gerekir:

```json

{
  "value": [
    {
      "properties": {
        "privateEndpoint": {
          "id": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Network/privateEndpoints/<PRIVATE ENDPOINT NAME>"
        },
        "groupIds": [
          "topic"
        ],
        "privateLinkServiceConnectionState": {
          "status": "Approved",
          "description": "Auto-approved",
          "actionsRequired": "None"
        },
        "provisioningState": "Succeeded"
      },
      "id": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>",
      "name": "myConnection",
      "type": "Microsoft.EventGrid/topics/privateEndpointConnections"
    }
  ]
}
```

### <a name="approve-a-private-endpoint-connection"></a>Özel bir bitiş noktası bağlantısını onaylama
Aşağıdaki örnek PowerShell snippet nasıl özel bir bitiş noktası onaylamak gösterir. 

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için özel uç noktaları onaylamak için benzer adımları kullanabilirsiniz. 

```azurepowershell-interactive
$approvedBody = @{"properties"=@{"privateLinkServiceConnectionState"=@{"status"="approved";"description"="connection approved";"actionsRequired"="none"}}} | ConvertTo-Json

# approve endpoint connection
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AzuRE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $approvedBody

# confirm that the endpoint connection was approved
Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers

```

### <a name="reject-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını reddetme
Aşağıdaki örnek, PowerShell'i kullanarak özel bir bitiş noktasını nasıl reddedeceğinizi gösterir. Guid'i önceki GET komutunun sonucundan özel bitiş noktası için alabilirsiniz. 

> [!NOTE]
> Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için özel uç noktaları reddetmek için benzer adımları kullanabilirsiniz. 


```azurepowershell-interactive
$rejectedBody = @{"properties"=@{"privateLinkServiceConnectionState"=@{"status"="rejected";"description"="connection rejected";"actionsRequired"="none"}}} | ConvertTo-Json

# reject private endpoint
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $rejectedBody

# confirm that endpoint was rejected
Invoke-RestMethod -Method 'Get' 
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" ` 
    -Headers $Headers
```

Bağlantıyı API üzerinden reddedildikten sonra bile onaylayabilirsiniz. Azure portalı kullanıyorsanız, reddedilen bir bitiş noktasını onaylamıyorsunuz. 

## <a name="next-steps"></a>Sonraki adımlar
IP güvenlik duvarı ayarlarını nasıl yapılandırılabildiğini öğrenmek için Azure [Olay Ağı konuları veya etki alanları için IP güvenlik duvarını yapılandır'a](configure-firewall.md)bakın.