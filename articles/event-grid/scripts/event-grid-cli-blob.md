---
title: Azure CLI betik örneği - Blob depolama hesabına abone olma | Microsoft Docs
description: Bu makalede bir Azure Blob depolama hesabı için olaylara nasıl abone olunacağı gösteren örnek bir Azure CLı betiği sunulmaktadır.
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/08/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: ab0995a49f944344b6914bd0e51f1d437dd28c69
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107766776"
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-azure-cli"></a>Azure CLI ile bir Blob depolama hesabı için olaylara abone olma

Bu betik, bir Blob depolama hesabı için olaylara bir Event Grid aboneliği oluşturur. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.sh "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, olay aboneliğini oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create) | Event Grid aboneliği oluşturun. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - uzantı sürümü | Event Grid aboneliği oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

* Abonelikleri sorgulama hakkında bilgi edinmek için bkz. [Event Grid aboneliklerini sorgulama](../query-event-subscriptions.md).
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
