---
title: Azure Event Grid olayları için bir olay işleyicisi olarak karma bağlantı
description: Azure Event Grid olayları için Azure Relay karma bağlantıları olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 0631724e688a71d7e9685f5f0ad738d81e2a8034
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598433"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid olayları için bir olay işleyicisi olarak karma bağlantı
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için başka bir eylem gerçekleştirir. Çeşitli Azure Hizmetleri, olayları işleyecek şekilde otomatik olarak yapılandırılır ve bunlardan biridir **Azure Relay** . 

Kurumsal ağ içinde olan uygulamalara olay göndermek için Azure **geçişi karma bağlantılar** kullanın ve genel olarak erişilebilen bir uç nokta yoktur.

## <a name="tutorials"></a>Öğreticiler
Olay işleyicisi olarak Azure Relay karma bağlantı kullanmanın bir örneği için aşağıdaki öğreticiye bakın. 

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: olayları karma bağlantıya gönder](custom-event-to-hybrid-connection.md) | Bir dinleyici uygulaması tarafından işlenmek üzere var olan karma bağlantıya özel bir olay gönderir. |

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 