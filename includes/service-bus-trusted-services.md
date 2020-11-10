---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/05/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bb6f2281cc115904db6babcf31f1af387b254d48
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426577"
---
## <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
**Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** ayarını etkinleştirdiğinizde, aşağıdaki hizmetlere Service Bus kaynaklarınıza erişim verilir.

| Güvenilen hizmet | Desteklenen kullanım senaryoları | 
| --------------- | ------------------------- | 
| Azure Event Grid | Azure Event Grid Service Bus ad uzayındaki kuyruklara veya konulara olay göndermesini sağlar. Ayrıca aşağıdaki adımları gerçekleştirmeniz gerekir: <ul><li>Bir konu veya etki alanı için sistem tarafından atanan kimliği etkinleştirme</li><li>Kimliği Service Bus ad alanındaki Azure Service Bus Data sender rolüne ekleyin</li><li>Ardından, sistem tarafından atanan kimliği kullanmak için bir Service Bus kuyruğu veya konu başlığı kullanan olay aboneliğini uç nokta olarak yapılandırın.</li></ul> <p>Daha fazla bilgi için bkz. [yönetilen kimlik Ile olay teslimi](../articles/event-grid/managed-service-identity.md)</p>|
