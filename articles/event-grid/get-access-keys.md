---
title: Event Grid kaynağı için erişim anahtarı al
description: Bu makalede bir Event Grid konusu veya etki alanı için erişim anahtarı alma açıklanmaktadır
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 06/25/2020
ms.author: spelluru
ms.openlocfilehash: 75ab09de40a4305ebd215985f2f9f181bfd69a24
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85414904"
---
# <a name="get-access-keys-for-event-grid-resources-topics-or-domains"></a>Event Grid kaynaklar için erişim anahtarları alın (konular veya etki alanları)
Erişim tuşları, Azure Event Grid kaynaklara (konular ve etki alanları) yönelik bir uygulama yayımlama olaylarının kimliğini doğrulamak için kullanılır. Anahtarlarınızı düzenli olarak yeniden oluşturup güvenli bir şekilde depolamanızı öneririz. Diğer iki erişim anahtarı ile birlikte sunulur. böylece, diğer bir anahtarı kullanarak bağlantıları koruyabilirsiniz.

Bu makalede Azure portal, PowerShell veya CLı kullanarak bir Event Grid kaynağı (konu veya etki alanı) için erişim tuşlarının nasıl alınacağı açıklanmaktadır. 

## <a name="azure-portal"></a>Azure portal
Azure portal, konu veya etki alanınız için **Event Grid konusunun** veya **Event Grid etki alanı** sayfasının **erişim tuşları** sekmesine geçin.  

:::image type="content" source="./media/get-access-keys/azure-portal.png" alt-text="Erişim tuşları sayfası":::

## <a name="azure-powershell"></a>Azure PowerShell
Konular için erişim anahtarları almak için [Get-AzEventGridTopicKey](/powershell/module/az.eventgrid/get-azeventgridtopickey?view=azps-4.3.0) komutunu kullanın. 

```azurepowershell-interactive
Get-AzEventGridTopicKey -ResourceGroup <RESOURCE GROUP NAME> -Name <TOPIC NAME>
```

Etki alanları için erişim anahtarları almak için [Get-AzEventGridDomainKey](/powershell/module/az.eventgrid/get-azeventgriddomainkey?view=azps-4.3.0) komutunu kullanın. 

```azurepowershell-interactive
Get-AzEventGridDomainKey -ResourceGroup <RESOURCE GROUP NAME> -Name <DOMAIN NAME>
```

## <a name="azure-cli"></a>Azure CLI
Konu başlıkları için erişim anahtarları almak için [az eventgrid konu anahtar listesini](/cli/azure/eventgrid/topic/key?view=azure-cli-latest#az-eventgrid-topic-key-list) kullanın. 

```azurecli-interactive
az eventgrid topic key list --resource-group <RESOURCE GROUP NAME> --name <TOPIC NAME>
```

Etki alanları için erişim anahtarları almak için [az eventgrid etki alanı anahtar listesini](/cli/azure/eventgrid/domain/key?view=azure-cli-latest#az-eventgrid-domain-key-list) kullanın. 

```azurecli-interactive
az eventgrid domain key list --resource-group <RESOURCE GROUP NAME> --name <DOMAIN NAME>
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [Yayımlama Istemcilerinin kimlik doğrulaması](security-authenticate-publishing-clients.md). 
