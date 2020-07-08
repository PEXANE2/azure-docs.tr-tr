---
title: Azure Resource Manager şablonu örnekleri - Event Grid | Microsoft Docs
description: Bu makale, GitHub üzerinde Azure Event Grid için Azure Resource Manager şablon örneklerinin bir listesini sağlar.
ms.topic: sample
ms.date: 07/07/2020
ms.openlocfilehash: 910012adf2dc930e6f1a26f1a7fc41f5ed0580c9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119064"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Event Grid için Azure Resource Manager şablonları

Bir şablonda kullanılacak JSON sözdizimi ve özellikler için bkz. [Microsoft. EventGrid kaynak türleri](/azure/templates/microsoft.eventgrid/allversions). Aşağıdaki tablo, Event Grid için Azure Resource Manager şablonlarının bağlantılarını içerir.

## <a name="event-grid-subscriptions"></a>Event Grid abonelikler
- [Web kancası uç noktası Ile özel konu ve abonelik](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) -Event Grid özel bir konu dağıtır. Web Kancası uç noktasını kullanan özel konuya yönelik bir abonelik oluşturur. 
- [EventHub uç noktası Ile özel konu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler) -özel bir konuya Event Grid aboneliği oluşturur. Abonelik, uç nokta için bir Olay Hub’ı kullanır. 
- [Azure aboneliği veya kaynak grubu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) -bir kaynak grubu veya Azure aboneliği için olaylara abone olur. Dağıtım sırasında hedef olarak belirttiğiniz kaynak grubu, olayların kaynağıdır. Abonelik, uç nokta için bir Web Kancası kullanır. 
- [BLOB depolama hesabı ve aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) -bir Azure Blob depolama hesabı dağıtır ve bu depolama hesabı için olaylara abone olur. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örneklere bakın:

- [PowerShell örnekleri](powershell-samples.md)
- [CLı örnekleri](cli-samples.md)
