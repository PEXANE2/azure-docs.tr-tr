---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 11/09/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: d828a0c3eb2582a833ee8ad07bdf4f18002c9dca
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96015594"
---
## <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
**Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** ayarını etkinleştirdiğinizde, aşağıdaki hizmetlere Event Hubs kaynaklarınıza erişim verilir.

| Güvenilen hizmet | Desteklenen kullanım senaryoları | 
| --------------- | ------------------------- | 
| Azure Event Grid | Azure Event Grid, Event Hubs ad uzayındaki Olay Hub 'larına olay göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>Bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin</li><li>Daha sonra, sistem tarafından atanan kimliği kullanmak için bir olay hub 'ı bir uç nokta olarak kullanan olay aboneliğini yapılandırın.</li></ul> <p>Daha fazla bilgi için bkz. [yönetilen kimlik Ile olay teslimi](../articles/event-grid/managed-service-identity.md)</p>|
| Azure Izleyici (Tanılama Ayarları) | Azure Izleyici 'nin Event Hubs ad alanındaki Olay Hub 'larına tanılama bilgileri göndermesini sağlar. |
| Azure Stream Analytics | Azure Stream Analytics bir işin Event Hubs ad alanındaki Olay Hub['larına (](../articles/stream-analytics/event-hubs-output.md)[giriş](../articles/stream-analytics/stream-analytics-add-inputs.md)) veya veri yazmasına izin verir. |
| Azure IoT Hub | IoT Hub, Olay Hub 'ı ad alanındaki Olay Hub 'larına ileti göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>IoT Hub 'ınız için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği, Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin.</li><li>Ardından, kimlik tabanlı kimlik doğrulamasını kullanmak için bir olay hub 'ı özel uç nokta olarak kullanan IoT Hub yapılandırın.</li></ul>
