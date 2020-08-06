---
title: dosya dahil etme
description: dosya dahil etme
services: service-bus-messaging
author: axisc
ms.service: service-bus-messaging
ms.topic: include
ms.date: 6/9/2020
ms.author: aschhab
ms.custom: include file
ms.openlocfilehash: 9030080d0b8c8e032cb2992a62275efcdb04aabc
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87798146"
---
Aşağıdaki tabloda Azure Service Bus Şu anda desteklediği Java Ileti hizmeti (JMS) özellikleri listelenmektedir. Ayrıca desteklenmeyen özellikleri gösterir.


| Özellik | API |Durum |
|---|---|---|
| Kuyruklar   | <ul> <li> JMSContext. createQueue (dize SıraAdı) </li> </ul>| **Desteklenir** |
| Konu başlıkları   | <ul> <li> JMSContext. createTopic (dize topicName) </li> </ul>| **Desteklenir** |
| Geçici kuyruklar |<ul> <li> JMSContext. createTemporaryQueue () </li> </ul>| **Desteklenir** |
| Geçici konular |<ul> <li> JMSContext. createTemporaryTopic () </li> </ul>| **Desteklenir** |
| İleti üreticisi/<br/> JMSProducer |<ul> <li> JMSContext. createProducer () </li> </ul>| **Desteklenir** |
| Tarayıcıları kuyruğa al |<ul> <li> JMSContext. createBrowser (kuyruk kuyruğu) </li> <li> JMSContext. createBrowser (kuyruk kuyruğu, dize messageSelector) </li> </ul> | **Desteklenir** |
| İleti tüketicisi/ <br/> JMSConsumer | <ul> <li> JMSContext. createConsumer (hedef hedef) </li> <li> JMSContext. createConsumer (hedef hedef, dize messageSelector) </li> <li> JMSContext. createConsumer (hedef hedef, dize messageSelector, Boolean noLocal)</li> </ul>  <br/> noLocal Şu anda desteklenmiyor | **Desteklenir** |
| Paylaşılan dayanıklı abonelikler | <ul> <li> JMSContext. createSharedDurableConsumer (konu başlığı, dize adı) </li> <li> JMSContext. createSharedDurableConsumer (konu başlığı, dize adı, dize messageSelector) </li> </ul>| **Desteklenir**|
| Paylaşılmayan dayanıklı abonelikler | <ul> <li> JMSContext. createDurableConsumer (konu başlığı, dize adı) </li> <li> createDurableConsumer (konu başlığı, dize adı, dize messageSelector, Boolean noLocal) </li> </ul> <br/> noLocal Şu anda desteklenmiyor ve false olarak ayarlanmalıdır | **Desteklenir** |
| Paylaşılan, dayanıklı olmayan abonelikler |<ul> <li> JMSContext. createSharedConsumer (konu başlığı, dize sharedSubscriptionName) </li> <li> JMSContext. createSharedConsumer (konu başlığı, dize sharedSubscriptionName, dize messageSelector) </li> </ul> | **Desteklenir** |
| Paylaşılmayan kalıcı olmayan abonelikler |<ul> <li> JMSContext. createConsumer (hedef hedef) </li> <li> JMSContext. createConsumer (hedef hedef, dize messageSelector) </li> <li> JMSContext. createConsumer (hedef hedef, dize messageSelector, Boolean noLocal) </li> </ul> <br/> noLocal Şu anda desteklenmiyor ve false olarak ayarlanmalıdır | **Desteklenir** |
| İleti seçicileri | oluşturulan tüketiciye bağlıdır | **Desteklenir** |
| Teslim gecikmesi (zamanlanmış iletiler) | <ul> <li> JMSProducer. setDeliveryDelay (uzun deliveryDelay) </li> </ul>|**Desteklenir**|
| İleti oluşturuldu |<ul> <li> JMSContext. createMessage () </li> <li> JMSContext. createBytesMessage () </li> <li> JMSContext. createMapMessage () </li> <li> JMSContext. createObjectMessage (serileştirilebilir nesne) </li> <li> JMSContext. createStreamMessage () </li> <li> JMSContext. createTextMessage () </li> <li> JMSContext. createTextMessage (dize metni) </li> </ul>| **Desteklenir** |
| Dağıtılmış işlemler || Desteklenmez |
