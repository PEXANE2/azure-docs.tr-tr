---
title: Azure Event Grid konusunun veya etki alanının fiyatlandırma katmanını güncelleştirme
description: Bu makalede, Azure portal, Azure CLı ve Azure PowerShell kullanarak bir Azure Event Grid konusunun veya etki alanının fiyatlandırma katmanını (temel-Premium, Premium 'a) güncelleştirme açıklanır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: caea8d515964510fce432eb0497e3af19ecc1369
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82101646"
---
# <a name="update-pricing-tier"></a>Fiyatlandırma katmanını güncelleştirme 
Bu makalede, Azure portal, Azure CLı ve Azure PowerShell kullanarak bir Azure Event Grid konusunun veya etki alanının fiyatlandırma katmanını güncelleştirme konusu gösterilmektedir. 

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir konunun fiyatlandırma katmanının veya Azure portal bir etki alanının nasıl değiştirileceği gösterilmektedir. 

### <a name="overview-page"></a>Genel Bakış sayfası
**Genel bakış** sayfasında bir konunun veya etki alanının fiyatlandırma katmanını değiştirebilirsiniz. Aşağıdaki örnek temel katmandan Premium katmana nasıl bir konunun yükseltilleceğini gösterir. Premium katmandan temel katmana düşürme adımları benzerdir.

1. [Azure Portal](https://portal.azure.com)konu veya etki alanı sayfasına gidin. 
2. **Genel bakış** sayfasında, geçerli fiyatlandırma katmanını seçin (aşağıdaki örnekte, **temel**.)
    
    ![Geçerli fiyatlandırma katmanını seçin](./media/update-tier/select-tier.png)
3. **Fiyatlandırma katmanı** sayfasında katmanı değiştirin ve **Tamam**' ı seçin. 

    ![Fiyatlandırma katmanını güncelleştirme](./media/update-tier/change-tier.png)
4. **Bildirimler** sayfasında işlemin durumunu kontrol edin.

    ![Güncelleştirme durumu](./media/update-tier/status.png)    
5. **Genel bakış** sayfasında güncelleştirilmiş katmanı görtığınızdan emin olun. 

    ![Güncelleştirme durumu](./media/update-tier/changed-tier.png)

### <a name="networking-page"></a>Ağ sayfası
**Ağ** sayfasında Temel katmandan Premium katmanına **yükseltebilirsiniz** . Bu sayfadaki Premium katmandan temel katmana düşürülemiyor. 

1. [Azure Portal](https://portal.azure.com)konu veya etki alanı sayfasına gidin. 
2. **Ağ** sayfasında, **Özel uç nokta bağlantıları (Önizleme)** sekmesine geçin. 
3. Geçerli fiyatlandırma katmanı **temel**ise, aşağıdaki iletiyi görürsünüz. Kalem simgesini seçin. 

    ![Özel uç nokta bağlantıları sayfasında katmanı Güncelleştir sayfası](./media/update-tier/private-endpoint-connections-page.png)
4. **Premium fiyatlandırma katmanında Güncelleştir** sayfasında **Evet**' i seçin. 
    
    ![Yükseltmeyi Onayla](./media/update-tier/message-private-endpoint-connection.png)
5. **Bildirimler** sayfasında işlemin durumunu kontrol edin.

    ![Güncelleştirme durumu](./media/update-tier/status.png)



## <a name="use-azure-cli"></a>Azure CLI kullanma
Bu bölümde, bir konunun fiyatlandırma katmanını değiştirmek için Azure CLı komutlarının nasıl kullanılacağı gösterilmektedir. Bir etki alanının fiyatlandırma katmanını güncelleştirmek için, bu `az eventgrid domain update` komutu benzer bir biçimde kullanın.

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki komutu çalıştırarak CLı için Azure Event Grid uzantısını güncelleştirin: 

```azurecli-interactive
az extension update -n eventgrid
```

Uzantı yüklü değilse, yüklemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az extension add -n eventgrid
```

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Temel ve Premium 'a bir konuyu yükseltme

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --sku "Premium" 
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Premium 'dan temel 'e bir konuyu düşürme

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --sku "Basic" 
```


## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bu bölümde, bir konunun veya etki alanının fiyatlandırma katmanını değiştirmek için PowerShell komutlarının nasıl kullanılacağı gösterilmektedir. 

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

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Temel ve Premium 'a bir konuyu yükseltme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Premium 'dan temel 'e bir konuyu düşürme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Bir etki alanını temel 'den Premium 'a yükseltme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Bir etki alanını Premium 'dan temel sürüme düşürme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

## <a name="next-steps"></a>Sonraki adımlar
Premium katman konuları ve etki alanları için, yalnızca seçili sanal ağlardan erişimi kısıtlamak üzere özel uç noktaları yapılandırabilirsiniz. Adım adım yönergeler için bkz. [Özel uç noktaları yapılandırma](configure-private-endpoints.md).
