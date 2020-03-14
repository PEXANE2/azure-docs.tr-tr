---
title: Azure Event Grid konuları veya etki alanları için IP güvenlik duvarını yapılandırma (Önizleme)
description: Bu makalede, Event Grid konular veya etki alanları için güvenlik duvarı ayarlarının nasıl yapılandırılacağı açıklanır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: b195872ca1002970fa96ae133d5eb47a9267796d
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299874"
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


### <a name="enable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini etkinleştirme
Varsayılan olarak, genel ağ erişimi konular ve etki alanları için etkinleştirilmiştir. Gelen IP güvenlik duvarı kurallarını yapılandırarak trafiği kısıtlayabilirsiniz. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Enabled\""}}"
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Mevcut bir konu için ortak ağ erişimini devre dışı bırak
Bir konu veya etki alanı için ortak ağ erişimi devre dışı bırakıldığında, genel İnternet üzerinden trafiğe izin verilmez. Yalnızca özel uç nokta bağlantılarının bu kaynaklara erişmesine izin verilir. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Disabled\""}}"
```

### <a name="create-topic-with-inbound-ip-rules"></a>Gelen IP kuralları ile konu oluştur
Aşağıdaki örnek CLı komutu, tek bir adımda gelen IP kuralları ile bir olay Kılavuzu konusu oluşturur. 

```azurecli-interactive
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"",\""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}
```

### <a name="create-topic-first-and-then-add-inbound-ip-rules"></a>Önce konu oluştur ve sonra gelen IP kuralları Ekle
Bu örnek ilk olarak bir Event Grid konusu oluşturur ve bu konu başlığı için gelen IP kurallarını ayrı bir komutta ekler. Ayrıca, ikinci komutta ayarlanan gelen IP kurallarını da güncelleştirir. 

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
Bu bölümde, gelen IP güvenlik duvarı kuralları ile Azure Event Grid konu oluşturmak için Azure PowerShell komutlarının nasıl kullanılacağı gösterilmektedir. Bu bölümde gösterilen adımlar konular içindir. **Etki alanları**IÇIN gelen IP kuralları oluşturmak üzere benzer adımları kullanabilirsiniz. 

### <a name="prerequisite"></a>Önkoşul
[Nasıl yapılır: Azure AD uygulaması ve hizmet sorumlusu oluşturmak için, bir Azure Active Directory uygulaması oluşturmak ve aşağıdaki değerleri izlemek üzere kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu oluşturmak için bu](../active-directory/develop/howto-create-service-principal-portal.md) yönergeleri izleyin:

- Dizin (kiracı) KIMLIĞI
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

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
