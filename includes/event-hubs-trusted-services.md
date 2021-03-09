---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 03/08/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 0e487b3ab3663c765c052f2064201865508ef57f
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102510989"
---
## <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
**Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** ayarını etkinleştirdiğinizde, aşağıdaki hizmetlere Event Hubs kaynaklarınıza erişim verilir.

| Güvenilen hizmet | Desteklenen kullanım senaryoları | 
| --------------- | ------------------------- | 
| Azure Event Grid | Azure Event Grid, Event Hubs ad uzayındaki Olay Hub 'larına olay göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>Bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin</li><li>Daha sonra, sistem tarafından atanan kimliği kullanmak için bir olay hub 'ı bir uç nokta olarak kullanan olay aboneliğini yapılandırın.</li></ul> <p>Daha fazla bilgi için bkz. [yönetilen kimlik Ile olay teslimi](../articles/event-grid/managed-service-identity.md)</p>|
| Azure Izleyici (Tanılama Ayarları) | Azure Izleyici 'nin Event Hubs ad alanındaki Olay Hub 'larına tanılama bilgileri göndermesini sağlar. Azure Izleyici Olay Hub 'ından okuyabilir ve ayrıca olay hub 'ına veri yazabilir. |
| Azure Stream Analytics | Azure Stream Analytics bir işin Event Hubs ad alanındaki Olay Hub['larına (](../articles/stream-analytics/event-hubs-output.md)[giriş](../articles/stream-analytics/stream-analytics-add-inputs.md)) veya veri yazmasına izin verir. <p>**Önemli**: Stream Analytics işi, Olay Hub 'ına erişmek için **yönetilen bir kimlik** kullanacak şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz. [Azure Stream Analytics bir Işten Olay Hub 'ına erişmek için Yönetilen kimlikler kullanma (Önizleme)](../articles/stream-analytics/event-hubs-managed-identity.md). </p>|
| Azure IoT Hub | IoT Hub, Olay Hub 'ı ad alanındaki Olay Hub 'larına ileti göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>IoT Hub 'ınız için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği, Event Hubs ad alanındaki Azure Event Hubs Data sender rolüne ekleyin.</li><li>Ardından, kimlik tabanlı kimlik doğrulamasını kullanmak için bir olay hub 'ı özel uç nokta olarak kullanan IoT Hub yapılandırın.</li></ul>
| Azure API Management | <p>API Management hizmeti, Event Hubs ad alanındaki bir olay hub 'ına olayları göndermenize olanak tanır.</p> <ul><li>[Gönderme isteği ilkesi](../articles/api-management/api-management-sample-send-request.md)KULLANıLARAK bir API çağrıldığında olayları Olay Hub 'ınıza göndererek özel iş akışlarını tetikleyebilirsiniz.</li><li>Ayrıca bir API 'de arka ucunuz olarak bir olay hub 'ını da deneyebilirsiniz. Örnek bir ilke için bkz. bir [Olay Hub 'ına erişmek için yönetilen kimlik kullanarak kimlik doğrulama](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Authenticate%20using%20Managed%20Identity%20to%20access%20Event%20Hub.xml). Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir:<ol><li>API Management örneğinde sistem tarafından atanan kimliği etkinleştirin. Yönergeler için bkz. [Azure API Management 'da Yönetilen kimlikler kullanma](../articles/api-management/api-management-howto-use-managed-service-identity.md).</li><li>Kimliği Event Hubs ad alanındaki **Azure Event Hubs Data sender** rolüne ekleyin</li></ol></li></ul> | 
