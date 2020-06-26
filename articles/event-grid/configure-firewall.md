---
title: Azure Event Grid konuları veya etki alanları için IP güvenlik duvarını yapılandırma (Önizleme)
description: Bu makalede, Event Grid konular veya etki alanları için güvenlik duvarı ayarlarının nasıl yapılandırılacağı açıklanır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: c64d947c13cf067d4ffc2b32ed655960143ad1c3
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391783"
---
# <a name="configure-ip-firewall-for-azure-event-grid-topics-or-domains-preview"></a>Azure Event Grid konuları veya etki alanları için IP güvenlik duvarını yapılandırma (Önizleme)
Varsayılan olarak, konu ve etki alanına, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz. Başka herhangi bir IP adresinden kaynaklanan yayımcılar reddedilir ve 403 (yasak) yanıtı alacaktır. Event Grid tarafından desteklenen ağ güvenliği özellikleri hakkında daha fazla bilgi için bkz. [Event Grid Için ağ güvenliği](network-security.md).

Bu makalede, Azure Event Grid konular veya etki alanları için IP Güvenlik Duvarı ayarlarının nasıl yapılandırılacağı açıklanır.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, gelen IP güvenlik duvarı kuralları oluşturmak için Azure portal nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**IÇIN gelen IP kuralları oluşturmak üzere benzer adımları kullanabilirsiniz. 

1. [Azure Portal](https://portal.azure.com), olay Kılavuzu konu başlığına veya etki alanına gidin ve **ağ** sekmesine geçin.
2. İnternet dahil tüm ağın kaynağa erişmesine izin vermek için **ortak ağları** seçin. 

    Trafiği IP tabanlı güvenlik duvarı kurallarını kullanarak kısıtlayabilirsiniz. Tek bir IPv4 adresi veya sınıfsız etki alanları arası yönlendirme (CıDR) gösteriminde bir IP adresi aralığı belirtin. 

    ![Ortak ağlar sayfası](./media/configure-firewall/public-networks-page.png)
3. Yalnızca özel uç nokta bağlantılarının bu kaynağa erişmesine izin vermek için **Özel uç noktaları** seçin. Bağlantıları yönetmek için bu sayfadaki **Özel uç nokta bağlantıları** sekmesini kullanın. 

    ![Ortak ağlar sayfası](./media/configure-firewall/private-endpoints-page.png)
4. Araç çubuğunda **Kaydet**’i seçin. 



## <a name="use-azure-cli"></a>Azure CLI kullanma
Bu bölümde, gelen IP kuralları ile konular oluşturmak için Azure CLı komutlarının nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**IÇIN gelen IP kuralları oluşturmak üzere benzer adımları kullanabilirsiniz. 


### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki komutu çalıştırarak CLı için Azure Event Grid uzantısını güncelleştirin: 

```azurecli-interactive
az extension update -n eventgrid
```

Uzantı yüklü değilse, yüklemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az extension add -n eventgrid
```

### <a name="enable-or-disable-public-network-access"></a>Ortak ağ erişimini etkinleştirme veya devre dışı bırakma
Varsayılan olarak, genel ağ erişimi konular ve etki alanları için etkinleştirilmiştir. Ayrıca, açıkça etkinleştirebilir veya devre dışı bırakabilirsiniz. Gelen IP güvenlik duvarı kurallarını yapılandırarak trafiği kısıtlayabilirsiniz. 

#### <a name="enable-public-network-access-while-creating-a-topic"></a>Konu oluştururken ortak ağ erişimini etkinleştirin

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled
```


#### <a name="disable-public-network-access-while-creating-a-topic"></a>Konu oluştururken genel ağ erişimini devre dışı bırakma

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access disabled
```

> [!NOTE]
> Bir konu veya etki alanı için ortak ağ erişimi devre dışı bırakıldığında, genel İnternet üzerinden trafiğe izin verilmez. Yalnızca özel uç nokta bağlantılarının bu kaynaklara erişmesine izin verilir. 


#### <a name="enable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini etkinleştirme

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled 
```

#### <a name="disable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini devre dışı bırak 

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access disabled
```

### <a name="create-a-topic-with-single-inbound-ip-rule"></a>Tek gelen IP kuralıyla bir konu oluşturun
Aşağıdaki örnek CLı komutu, gelen IP kuralları ile bir olay Kılavuzu konusu oluşturur. 

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR or CIDR MASK> allow 
```

### <a name="create-a-topic-with-multiple-inbound-ip-rules"></a>Birden çok gelen IP kuralına sahip bir konu oluşturun

Aşağıdaki örnek CLı komutu, tek adımda iki gelen IP kuralı olan bir Event Grid konusuna sahiptir: 

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow \
    --inbound-ip-rules <IP ADDR 2 or CIDR MASK 2> allow
```

### <a name="update-an-existing-topic-to-add-inbound-ip-rules"></a>Gelen IP kuralları eklemek için mevcut bir konuyu güncelleştirin
Bu örnek ilk olarak bir Event Grid konusu oluşturur ve bu konu başlığı için gelen IP kurallarını ayrı bir komutta ekler. Ayrıca, ikinci komutta ayarlanan gelen IP kurallarını da güncelleştirir. 

```azurecli-interactive

# create the event grid topic first
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location

# add inbound IP rules to an existing topic
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR or CIDR MASK> allow

# later, update topic with additional ip rules
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow \
    --inbound-ip-rules <IP ADDR 2 or CIDR MASK 2> allow
```

### <a name="remove-an-inbound-ip-rule"></a>Gelen IP kuralını kaldır
Aşağıdaki komut, ayarı güncelleştirirken yalnızca ilk kuralı belirterek önceki adımda oluşturduğunuz ikinci kuralı kaldırır. 

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow
```


## <a name="use-powershell"></a>PowerShell kullanma
Bu bölümde, gelen IP güvenlik duvarı kuralları ile Azure Event Grid konu oluşturmak için Azure PowerShell komutlarının nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**IÇIN gelen IP kuralları oluşturmak üzere benzer adımları kullanabilirsiniz. 

### <a name="prerequisites"></a>Ön koşullar
[Nasıl yapılır: Azure AD uygulaması ve hizmet sorumlusu oluşturmak için, bir Azure Active Directory uygulaması oluşturmak ve aşağıdaki değerleri izlemek üzere kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu oluşturmak için bu](../active-directory/develop/howto-create-service-principal-portal.md) yönergeleri izleyin:

- Dizin (kiracı) kimliği
- Uygulama (Istemci) KIMLIĞI
- Uygulama (istemci) gizli anahtarı

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>REST API çağrıları için belirteci ve üstbilgileri hazırlama 
REST API çağrılarıyla ve yetkilendirme ve diğer üst bilgi bilgileriyle kullanılacak bir kimlik doğrulama belirteci almak için aşağıdaki önkoşul komutlarını çalıştırın. 

```azurepowershell-interactive
# replace <CLIENT ID> and <CLIENT SECRET>
$body = "grant_type=client_credentials&client_id=<CLIENT ID>&client_secret=<CLIENT SECRET>&resource=https://management.core.windows.net"

# get the authentication token. Replace <TENANT ID>
$Token = Invoke-RestMethod -Method Post `
    -Uri https://login.microsoftonline.com/<TENANT ID>/oauth2/token `
    -Body $body `
    -ContentType 'application/x-www-form-urlencoded'

# set authorization and content-type headers
$Headers = @{}
$Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
$Headers.Add("Content-Type","application/json")
```

### <a name="enable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini etkinleştirme
Varsayılan olarak, genel ağ erişimi konular ve etki alanları için etkinleştirilmiştir. Gelen IP güvenlik duvarı kurallarını yapılandırarak trafiği kısıtlayabilirsiniz. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="enabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini devre dışı bırak
Bir konu veya etki alanı için ortak ağ erişimi devre dışı bırakıldığında, genel İnternet üzerinden trafiğe izin verilmez. Yalnızca özel uç nokta bağlantılarının bu kaynaklara erişmesine izin verilir. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="disabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="create-an-event-grid-topic-with-inbound-rules-in-one-step"></a>Tek adımda gelen kurallarla bir olay Kılavuzu konusu oluşturun

```azurepowershell-interactive

# prepare the body for the REST PUT method. Notice that inbound IP rules are included. 
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"})}} | ConvertTo-Json -Depth 5

# create the event grid topic with inbound IP rules
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body

# verify that the topic was created
Invoke-RestMethod -Method 'Get' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5
```


### <a name="create-event-grid-topic-first-and-then-add-inbound-ip-rules"></a>Önce olay kılavuzu oluşturma konusu oluştur ve sonra gelen IP kuralları Ekle

```azurepowershell-interactive

# prepare the body for the REST PUT method. Notice that no inbound IP rules are specified. 
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled";}} | ConvertTo-Json -Depth 5

# create the Event Grid topic
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body`

# verify that the topic was created
Invoke-RestMethod -Method 'Get' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5

# prepare the body for REST PUT method. Notice that it includes inbound IP rules now. This feature is available in both basic and premium tiers.
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"}, @{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"})}} | ConvertTo-Json -Depth 5

# update the topic with inbound IP rules
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body

# verify that the topic was updated
Invoke-RestMethod -Method 'Get' 
    -Uri "https://management.azure.com/subscriptions/<AzURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5

```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Ağ bağlantısı sorunlarını gidermek için bkz. [ağ bağlantısı sorunlarını giderme](troubleshoot-network-connectivity.md)
