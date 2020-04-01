---
title: Azure Resource Manager şablonu örnekleri - Event Grid | Microsoft Docs
description: Bu makalede, GitHub'daki Azure Olay Izgarası için Azure Kaynak Yöneticisi şablon örneklerinin bir listesi sağlanmaktadır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 38d8db0bcc504760595fe51b63072f63e785577a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76720631"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Event Grid için Azure Resource Manager şablonları

Şablonda kullanılacak JSON sözdizimi ve özellikleri için [Microsoft.EventGrid kaynak türlerine](/azure/templates/microsoft.eventgrid/allversions)bakın. Aşağıdaki tablo, Event Grid için Azure Resource Manager şablonlarının bağlantılarını içerir.

| | |
|-|-|
|**Olay Izgara abonelikleri**||
| [Web Kancası uç noktası ile özel konu ve abonelik](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Event Grid özel konusunu dağıtır. Web Kancası uç noktasını kullanan özel konuya yönelik bir abonelik oluşturur. |
| [EventHub uç noktası ile özel konu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Bir özel konuya yönelik Event Grid aboneliği oluşturur. Abonelik, uç nokta için bir Olay Hub’ı kullanır. |
| [Azure aboneliği veya kaynak grubu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Azure aboneliği veya bir kaynak grubu için olaylara abone olur. Dağıtım sırasında hedef olarak belirttiğiniz kaynak grubu, olayların kaynağıdır. Abonelik, uç nokta için bir Web Kancası kullanır. |
| [Blob depolama hesabı ve aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Bir Azure Blob depolama hesabı dağıtır ve o depolama hesabı için olaylara abone olur. |
| | |
