---
author: baanders
description: Azure dijital TWINS uç noktaları oluşturmak için gereken kaynaklar için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 1/26/2021
ms.author: baanders
ms.openlocfilehash: 58c90bae3dea0f3a47489ea7d8de6a79f823dcab
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "99054522"
---
### <a name="prerequisite-create-endpoint-resources"></a>Önkoşul: uç nokta kaynakları oluşturma

Bir uç noktayı Azure dijital TWINS 'e bağlamak için, uç nokta için kullanmakta olduğunuz olay Kılavuzu konusu, Olay Hub 'ı veya Service Bus konusunun zaten mevcut olması gerekir.

Uç noktanızı oluşturmadan önce hangi kaynakların ayarlanması gerektiğini görmek için aşağıdaki grafiği kullanın.

| Uç nokta türü | Gerekli kaynaklar (oluşturma yönergelerine bağlı) |
| --- | --- |
| Event Grid uç noktası | [olay Kılavuzu konusu](../articles/event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) |
| Event Hubs uç noktası | [Olay &nbsp; hub 'ları &nbsp; ad alanı](../articles/event-hubs/event-hubs-create.md)<br/><br/>[Olay Hub 'ı](../articles/event-hubs/event-hubs-create.md)<br/><br/>Seçim anahtar tabanlı kimlik doğrulaması için [Yetkilendirme kuralı](../articles/event-hubs/authorize-access-shared-access-signature.md) | 
| Service Bus uç noktası | [Service Bus ad alanı](../articles/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md)<br/><br/>[Service Bus konusu](../articles/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md)<br/><br/> Seçim anahtar tabanlı kimlik doğrulaması için [Yetkilendirme kuralı](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md#shared-access-signature)|
