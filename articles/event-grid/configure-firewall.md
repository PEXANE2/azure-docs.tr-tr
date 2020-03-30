---
title: Azure Olay Ağı konuları veya etki alanları için IP güvenlik duvarını yapılandırın (Önizleme)
description: Bu makalede, Olay Izgara konuları veya etki alanları için güvenlik duvarı ayarlarını nasıl yapılandırılabilen açıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: b195872ca1002970fa96ae133d5eb47a9267796d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299874"
---
# <a name="configure-ip-firewall-for-azure-event-grid-topics-or-domains-preview"></a>Azure Olay Ağı konuları veya etki alanları için IP güvenlik duvarını yapılandırın (Önizleme)
Varsayılan olarak, istek geçerli kimlik doğrulama ve yetkilendirme ile birlikte geldiği sürece, konu ve etki alanına internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir iPv4 adresi kümesi veya IPv4 adres aralıklarıyla daha da kısıtlayabilirsiniz. Başka bir IP adresinden kaynaklanan yayıncılar reddedilir ve 403 (Yasak) yanıt alırsınız. Olay Izgarası tarafından desteklenen ağ güvenliği özellikleri hakkında daha fazla bilgi [için Olay Izgarası için Ağ güvenliği'ne](network-security.md)bakın.

Bu makalede, Azure Olay Ağı konuları veya etki alanları için IP güvenlik duvarı ayarlarının nasıl yapılandırılabildiğini açıklanmaktadır.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, gelen IP güvenlik duvarı kuralları oluşturmak için Azure portalını nasıl kullanacağınızı gösterir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için gelen IP kuralları oluşturmak için benzer adımları kullanabilirsiniz. 

1. Azure [portalında,](https://portal.azure.com)olay ızgarası konunuza veya etki alanınıza gidin ve **Ağ** sekmesine geçin.
2. Internet de dahil olmak üzere tüm ağların kaynağa erişmesine izin vermek için **Genel ağları** seçin. 

    IP tabanlı güvenlik duvarı kurallarını kullanarak trafiği kısıtlayabilirsiniz. Sınıfsız etki alanı yönlendirme (CIDR) gösteriminde tek bir IPv4 adresi veya çeşitli IP adresi belirtin. 

    ![Genel ağlar sayfası](./media/configure-firewall/public-networks-page.png)
3. Yalnızca özel uç nokta bağlantılarının bu kaynağa erişmesine izin vermek için **Özel uç noktaları** seçin. Bağlantıları yönetmek için bu sayfadaki **Özel uç nokta bağlantıları** sekmesini kullanın. 

    ![Genel ağlar sayfası](./media/configure-firewall/private-endpoints-page.png)
4. Araç çubuğunda **Kaydet**’i seçin. 



## <a name="use-azure-cli"></a>Azure CLI kullanma
Bu bölümde, gelen IP kurallarıolan konular oluşturmak için Azure CLI komutlarının nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için gelen IP kuralları oluşturmak için benzer adımları kullanabilirsiniz. 


### <a name="enable-public-network-access-for-an-existing-topic"></a>Varolan bir konu için ortak ağ erişimini etkinleştirme
Varsayılan olarak, konular ve etki alanları için ortak ağ erişimi etkinleştirilir. Gelen IP güvenlik duvarı kurallarını yapılandırarak trafiği kısıtlayabilirsiniz. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Enabled\""}}"
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Varolan bir konu için ortak ağ erişimini devre dışı
Bir konu veya etki alanı için ortak ağ erişimi devre dışı bırakıldığında, genel internet üzerinden trafiğe izin verilmez. Bu kaynaklara yalnızca özel uç nokta bağlantılarının erişmesine izin verilir. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Disabled\""}}"
```

### <a name="create-topic-with-inbound-ip-rules"></a>Gelen ip kurallarıyla konu oluşturma
Aşağıdaki örnek CLI komutu, bir adımda gelen IP kurallarıyla bir olay ızgarası konusu oluşturur. 

```azurecli-interactive
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"",\""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}
```

### <a name="create-topic-first-and-then-add-inbound-ip-rules"></a>Önce konu oluşturun ve sonra gelen ip kuralları ekleyin
Bu örnek, önce bir olay ızgarası konusu oluşturur ve sonra ayrı bir komutla konu için gelen IP kuralları ekler. Ayrıca, ikinci komutta ayarlanan gelen IP kurallarını da güncelleştirir. 

```azurecli-interactive

# create the event grid topic first
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
    --body {\""location\"":\""<LOCATION>\""}

# add inbound IP rules
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" 
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"", \""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}

# later, update topic with additional ip rules or remove them. 
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" 
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"", \""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""}, {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}
```


## <a name="use-powershell"></a>PowerShell kullanma
Bu bölümde, gelen IP güvenlik duvarı kurallarıyla Azure Olay Ağı konularını oluşturmak için Azure PowerShell komutlarının nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**için gelen IP kuralları oluşturmak için benzer adımları kullanabilirsiniz. 

### <a name="prerequisite"></a>Önkoşul
Nasıl olunur: Bir Azure Active Directory uygulaması oluşturmak ve aşağıdaki değerleri not almak için [kaynaklara erişebilen bir Azure REKLAM uygulaması ve hizmet ilkesi oluşturmak](../active-directory/develop/howto-create-service-principal-portal.md) için portalı kullanın:

- Dizin (kiracı) kimliği
- Başvuru (İstemci) Kimliği
- Uygulama (istemci) gizli

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>REST API çağrıları için belirteç ve üstbilgi hazırlayın 
REST API çağrıları, yetkilendirme ve diğer üstbilgi bilgileriyle kullanılacak bir kimlik doğrulama belirteci almak için aşağıdaki ön koşul komutlarını çalıştırın. 

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

### <a name="enable-public-network-access-for-an-existing-topic"></a>Varolan bir konu için ortak ağ erişimini etkinleştirme
Varsayılan olarak, konular ve etki alanları için ortak ağ erişimi etkinleştirilir. Gelen IP güvenlik duvarı kurallarını yapılandırarak trafiği kısıtlayabilirsiniz. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="enabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Varolan bir konu için ortak ağ erişimini devre dışı
Bir konu veya etki alanı için ortak ağ erişimi devre dışı bırakıldığında, genel internet üzerinden trafiğe izin verilmez. Bu kaynaklara yalnızca özel uç nokta bağlantılarının erişmesine izin verilir. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="disabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="create-an-event-grid-topic-with-inbound-rules-in-one-step"></a>Tek adımda gelen kuralları içeren bir olay ızgarası konusu oluşturma

```azurepowershell-interactive

# prepare the body for the REST PUT method. Notice that inbound IP rules are included. 
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"})}} | ConvertTo-Json -Depth 5

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


### <a name="create-event-grid-topic-first-and-then-add-inbound-ip-rules"></a>Önce olay ızgarası konusunu oluşturun ve sonra gelen ip kuralları ekleyin

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

# prepare the body for REST PUT method. Notice that it includes inbound IP rules now. This feature available in both basic and premium tiers.
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"}, @{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"})}} | ConvertTo-Json -Depth 5

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

* Olay teslimlerini izleme hakkında daha fazla bilgi için [Bkz.](monitor-event-delivery.md)
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulama](security-authentication.md)sı'na bakın.
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
