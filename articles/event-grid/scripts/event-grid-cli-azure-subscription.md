---
title: Azure CLI betik örneği - Azure’a abone olma | Microsoft Docs
description: Bu makalede, Azure CLı kullanarak Azure Event Grid olaylarına nasıl abone olunacağı gösteren örnek bir Azure CLı betiği sunulmaktadır.
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 9d40367c8d0f4f1250504c30fcd6c4846921a830
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720903"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-azure-cli"></a>Azure CLI ile Bir Azure aboneliği için olaylara abone olma

Bu betik, bir Azure aboneliği için olaylara bir Event Grid aboneliği oluşturur.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Önizleme örnek betiği için Event Grid uzantısı gerekir. Yüklemek için `az extension add --name eventgrid` komutunu çalıştırın.

## <a name="sample-script---stable"></a>Örnek betik - kararlı

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.sh "Subscribe to Azure subscription")]

## <a name="sample-script---preview-extension"></a>Örnek betik - önizleme uzantısı

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-azure-subscription-preview/subscribe-to-azure-subscription-preview.sh "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, olay aboneliğini oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Event Grid aboneliği oluşturun. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - uzantı sürümü | Event Grid aboneliği oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

* Abonelikleri sorgulama hakkında bilgi edinmek için bkz. [Event Grid aboneliklerini sorgulama](../query-event-subscriptions.md).
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).
