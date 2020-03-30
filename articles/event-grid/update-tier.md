---
title: Azure Olay Ağı konusunun veya etki alanının fiyatlandırma katmanını güncelleştirme
description: Bu makalede, Azure portalı, Azure CLI ve Azure PowerShell kullanarak bir Azure Olay Ağı konusunun veya etki alanının fiyatlandırma katmanının (temelden premium'a, temelden temele kadar) nasıl güncelleştirilenaçıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: 1e92a9f0b9ba007b289c634995b04f4f1575310a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79300147"
---
# <a name="update-pricing-tier"></a>Fiyatlandırma katmanını güncelleştir 
Bu makalede, Azure portalı, Azure CLI ve Azure PowerShell kullanarak bir Azure Olay Ağı konu veya etki alanının fiyatlandırma katmanını nasıl güncelleştirebilirsiniz gösterilmektedir. 

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, Azure portalındaki bir konunun veya etki alanının fiyatlandırma katmanının nasıl değiştirilen olduğu gösterilmektedir. 

### <a name="overview-page"></a>Genel bakış sayfası
**Genel Bakış** sayfasında bir konunun veya etki alanının fiyatlandırma katmanını değiştirebilirsiniz. Aşağıdaki örnek, bir konuyu temel katmandan premium katmana nasıl yükseltilir gösterir. Premium katmandan temel katmana düşürme adımları benzerdir.

1. Azure [portalında,](https://portal.azure.com)konu veya etki alanı sayfasına gidin. 
2. Genel **Bakış** sayfasında, geçerli fiyatlandırma katmanını seçin (aşağıdaki **örnekte, temel**.)
    
    ![Geçerli fiyatlandırma katmanını seçin](./media/update-tier/select-tier.png)
3. Fiyatlandırma **katmanı** sayfasında, katmanı değiştirin ve **Tamam'ı**seçin. 

    ![Fiyatlandırma katmanını güncelleştirme](./media/update-tier/change-tier.png)
4. **Bildirimler** sayfasında işlemin durumunu kontrol edin.

    ![Durumu güncelleştir](./media/update-tier/status.png)    
5. **Genel Bakış** sayfasında güncelleştirilmiş katmanı gördüğünüzden onaylayın. 

    ![Durumu güncelleştir](./media/update-tier/changed-tier.png)

### <a name="networking-page"></a>Ağ sayfası
Temel katmandan **Ağ** sayfasındaki premium katmana **yükseltebilirsiniz.** Yine de bu sayfadaki premium katmandan temel katmana indiremezsiniz. 

1. Azure [portalında,](https://portal.azure.com)konu veya etki alanı sayfasına gidin. 
2. **Ağ** sayfasında, Özel bitiş **noktası bağlantıları (önizleme)** sekmesine geçin. 
3. Geçerli fiyatlandırma katmanı **temelse,** aşağıdaki iletiyi görürsünüz. Kalem simgesini seçin. 

    ![Özel uç nokta bağlantıları sayfasında katmanı güncelleştirme](./media/update-tier/private-endpoint-connections-page.png)
4. Üstün **fiyatlandırma katmanı sayfasına Güncelleştir'de** **Evet'i**seçin. 
    
    ![Yükseltmeyi onayla](./media/update-tier/message-private-endpoint-connection.png)
5. **Bildirimler** sayfasında işlemin durumunu kontrol edin.

    ![Durumu güncelleştir](./media/update-tier/status.png)



## <a name="use-azure-cli"></a>Azure CLI kullanma
Bu bölümde, bir konunun veya etki alanının fiyatlandırma katmanını değiştirmek için Azure CLI komutlarının nasıl kullanılacağı gösterilmektedir. 

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Bir konuyu temelden premium'a yükseltme

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""premium\""}}"
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Bir konuyu premiumdan temele düşürme

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""basic\""}}"
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Etki alanını temelden premium'a yükseltme

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""premium\""}}"
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Bir etki alanını premium'dan temele düşürme

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""basic\""}}"
```



## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bu bölümde, bir konunun veya etki alanının fiyatlandırma katmanını değiştirmek için PowerShell komutlarının nasıl kullanılacağı gösterilmektedir. 

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

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Bir konuyu temelden premium'a yükseltme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Bir konuyu premiumdan temele düşürme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Etki alanını temelden premium'a yükseltme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Bir etki alanını premium'dan temele düşürme

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

## <a name="next-steps"></a>Sonraki adımlar
Birinci sınıf konular ve etki alanları için, yalnızca seçili sanal ağlardan erişimi kısıtlamak için özel uç noktaları yapılandırabilirsiniz. Adım adım yönergeler için [bkz.](configure-private-endpoints.md)
