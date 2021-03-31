---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 03/08/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd4d5de5d93e4aea862aaabd10fb5d3c6d45cb1c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102510968"
---
## <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
**Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** ayarını etkinleştirdiğinizde, aşağıdaki hizmetlere Service Bus kaynaklarınıza erişim verilir.

| Güvenilen hizmet | Desteklenen kullanım senaryoları | 
| --------------- | ------------------------- | 
| Azure Event Grid | Azure Event Grid Service Bus ad uzayındaki kuyruklara veya konulara olay göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>Bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği Service Bus ad alanındaki Azure Service Bus Data sender rolüne ekleyin</li><li>Ardından, sistem tarafından atanan kimliği kullanmak için bir Service Bus kuyruğu veya konu başlığı kullanan olay aboneliğini uç nokta olarak yapılandırın.</li></ul> <p>Daha fazla bilgi için bkz. [yönetilen kimlik Ile olay teslimi](../articles/event-grid/managed-service-identity.md)</p>|
| Azure API Management | <p>API Management hizmeti, Service Bus ad alanındaki bir Service Bus kuyruğuna/konuya ileti göndermenize olanak tanır.</p><ul><li>[Gönderme isteği ilkesi](../articles/api-management/api-management-sample-send-request.md)KULLANıLARAK bir API çağrıldığında, Service Bus kuyruğuna/konuya ileti göndererek özel iş akışlarını tetikleyebilirsiniz.</li><li>Ayrıca bir API 'de arka ucunuz olarak bir Service Bus kuyruğu/konuyu da deneyebilirsiniz. Örnek bir ilke için bkz. [Service Bus kuyruğuna veya konuya erişmek için yönetilen kimlik kullanarak kimlik doğrulama](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Authenticate%20using%20Managed%20Identity%20to%20access%20Service%20Bus.xml). Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir:<ol><li>API Management örneğinde sistem tarafından atanan kimliği etkinleştirin. Yönergeler için bkz. [Azure API Management 'da Yönetilen kimlikler kullanma](../articles/api-management/api-management-howto-use-managed-service-identity.md).</li><li>Kimliği Service Bus ad alanındaki **Azure Service Bus Data sender** rolüne ekleyin</li></ol></li></ul> | 