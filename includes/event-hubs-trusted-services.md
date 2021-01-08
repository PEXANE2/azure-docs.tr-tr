---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 01/05/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 743d7d46474b4ba55df50398f29cbdb964b5ff08
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97978825"
---
## <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
**Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** ayarını etkinleştirdiğinizde, aşağıdaki hizmetlere Event Hubs kaynaklarınıza erişim verilir.

| Güvenilen hizmet | Desteklenen kullanım senaryoları | 
| --------------- | ------------------------- | 
| Azure Event Grid | Azure Event Grid, Event Hubs ad uzayındaki Olay Hub 'larına olay göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>Bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin</li><li>Daha sonra, sistem tarafından atanan kimliği kullanmak için bir olay hub 'ı bir uç nokta olarak kullanan olay aboneliğini yapılandırın.</li></ul> <p>Daha fazla bilgi için bkz. [yönetilen kimlik Ile olay teslimi](../articles/event-grid/managed-service-identity.md)</p>|
| Azure Izleyici (Tanılama Ayarları) | Azure Izleyici 'nin Event Hubs ad alanındaki Olay Hub 'larına tanılama bilgileri göndermesini sağlar. Azure Izleyici Olay Hub 'ından okuyabilir ve ayrıca olay hub 'ına veri yazabilir. |
| Azure Stream Analytics | Azure Stream Analytics bir işin Event Hubs ad alanındaki Olay Hub['larına (](../articles/stream-analytics/event-hubs-output.md)[giriş](../articles/stream-analytics/stream-analytics-add-inputs.md)) veya veri yazmasına izin verir. <p>**Önemli**: Stream Analytics işi, Olay Hub 'ına erişmek için **yönetilen bir kimlik** kullanacak şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz. [Azure Stream Analytics bir Işten Olay Hub 'ına erişmek için Yönetilen kimlikler kullanma (Önizleme)](../articles/stream-analytics/event-hubs-managed-identity.md). </p>|
| Azure IoT Hub | IoT Hub, Olay Hub 'ı ad alanındaki Olay Hub 'larına ileti göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>IoT Hub 'ınız için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği, Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin.</li><li>Ardından, kimlik tabanlı kimlik doğrulamasını kullanmak için bir olay hub 'ı özel uç nokta olarak kullanan IoT Hub yapılandırın.</li></ul>
