---
title: Azure Hizmet Veri Servisi'nde AMQP 1.0 istek/yanıt işlemleri
description: Bu makalede, Microsoft Azure Hizmet Veri Tos'undaki AMQP istek/yanıt tabanlı işlemlerin listesi tanımlanır.
services: service-bus-messaging
documentationcenter: na
author: axisc
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: aschhab
ms.openlocfilehash: 338b411eade35fa357f64131c64b8ef2034c92b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76761092"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>Microsoft Azure Hizmet Veri Servisi'nde AMQP 1.0: istek yanıtı tabanlı işlemler

Bu makalede, Microsoft Azure Hizmet Veri Servisi istek/yanıt tabanlı işlemler listesi tanımlanır. Bu bilgiler, AMQP Management Version 1.0 çalışma taslağına dayanmaktadır.  
  
Hizmet Veri Servisi'nin OASIS AMQP teknik belirtimini nasıl uyguladığını ve oluşturduğunu açıklayan ayrıntılı bir tel düzeyindeki AMQP[1.0 protokol]kılavuzu için Azure Hizmet Veri Ve Etkinlik [Hub'ları protokol kılavuzuNDA BULUNAN AMQP 1.0]protokol kılavuzuna bakın.  
  
## <a name="concepts"></a>Kavramlar  
  
### <a name="entity-description"></a>Varlık açıklaması  

Varlık açıklaması, Service Bus [QueueDescription sınıfı,](/dotnet/api/microsoft.servicebus.messaging.queuedescription) [TopicDescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.topicdescription)veya [AbonelikAçıklama sınıfı](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) nesnesi anlamına gelir.  
  
### <a name="brokered-message"></a>Aracılı ileti  

Hizmet Veri Servisi'nde bir AMQP iletisine eşlenen bir iletiyi temsil eder. Eşleme, Service [Bus AMQP protokol kılavuzunda](service-bus-amqp-protocol-guide.md)tanımlanır.  
  
## <a name="attach-to-entity-management-node"></a>Varlık yönetim düğümüne ekleme  

Bu belgede açıklanan tüm işlemler bir istek/yanıt deseni izler, bir varlığa kapsamlıdır ve bir varlık yönetim düğümüne eklenmesini gerektirir.  
  
### <a name="create-link-for-sending-requests"></a>İstek göndermek için bağlantı oluşturma  

İstek göndermek için yönetim düğümüne bir bağlantı oluşturur.  
  
```
requestLink = session.attach(
role: SENDER,
    target: { address: "<entity address>/$management" },
    source: { address: ""<my request link unique address>" }
)

```
  
### <a name="create-link-for-receiving-responses"></a>Yanıt almak için bağlantı oluşturma  

Yönetim düğümünden yanıt almak için bir bağlantı oluşturur.  
  
```
responseLink = session.attach(
role: RECEIVER,
    source: { address: "<entity address>/$management" }
    target: { address: "<my response link unique address>" }
)

```
  
### <a name="transfer-a-request-message"></a>İstek iletisi aktarma  

İstek iletisi aktarın.  
Hareketi destekleyen işlemler için isteğe bağlı olarak bir işlem durumu eklenebilir.

```
requestLink.sendTransfer(
        Message(
                properties: {
                        message-id: <request id>,
                        reply-to: "<my response link unique address>"
                },
                application-properties: {
                        "operation" -> "<operation>",
                }
        ),
        [Optional] State = transactional-state: {
                txn-id: <txn-id>
        }
)
```
  
### <a name="receive-a-response-message"></a>Yanıt iletisi alma  

Yanıt bağlantısını yanıt lailgili alır.  
  
```
responseMessage = responseLink.receiveTransfer()
```
  
Yanıt iletisi aşağıdaki şekildedir:
  
```
Message(
properties: {
        correlation-id: <request id>
    },
    application-properties: {
            "statusCode" -> <status code>,
            "statusDescription" -> <status description>,
           },
)

```
  
### <a name="service-bus-entity-address"></a>Servis Veri Servisi kuruluş adresi  

Servis Veri Günü varlıkları aşağıdaki gibi ele alınmalıdır:  
  
|Varlık türü|Adres|Örnek|  
|-----------------|-------------|-------------|  
|kuyruk|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|konu başlığı|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|aboneliği|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>İleti işlemleri  
  
### <a name="message-renew-lock"></a>İleti Yenile Kilidi  

İletinin kilitlerini varlık açıklamasında belirtilen zamana kadar uzatır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
 İstek iletisi gövdesi, aşağıdaki girişleri içeren bir harita içeren bir amqp değeri bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|uuid dizi|Evet|İleti kilidi belirteçleri yenilemek için.|  

> [!NOTE]
> Kilit belirteçleri `DeliveryTag` alınan iletilerin özelliğidir. Bunları alan [.NET SDK'daki](https://github.com/Azure/azure-service-bus-dotnet/blob/6f144e91310dcc7bd37aba4e8aebd535d13fa31a/src/Microsoft.Azure.ServiceBus/Amqp/AmqpMessageConverter.cs#L336) aşağıdaki örneğe bakın. Belirteç de 'DeliveryAnnotations' olarak görünebilir 'x-opt-lock-belirteci' ancak bu garanti `DeliveryTag` edilmez ve tercih edilmelidir. 
> 
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız oldu.|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir harita içeren bir amqp değeri bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Süre sonu|zaman damgası dizisi|Evet|İleti kilidi belirteçleri istek kilidi belirteçleri karşılık gelen yeni sona erme.|  
  
### <a name="peek-message"></a>Peek İletisi  

İletileri kilitlemeden gözetliyor.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|long|Evet|Hangi peek başlatmak için sıra numarası.|  
|`message-count`|int|Evet|Gözetlemeiçin maksimum ileti sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam - daha fazla mesaj var<br /><br /> 204: İçerik yok – mesaj yok|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|harita listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
İletiyi temsil eden harita aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1.0 kablokodlu ileti.|  
  
### <a name="schedule-message"></a>İletiyi Zamanla  

İletileri zamanlar. Bu işlem hareketi destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|harita listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
İletiyi temsil eden harita aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|mesaj kimliği|string|Evet|`amqpMessage.Properties.MessageId`dize olarak|  
|oturum-id|string|Hayır|`amqpMessage.Properties.GroupId as string`|  
|bölüm anahtarı|string|Hayır|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|via-partition-key|string|Hayır|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|bayt dizisi|Evet|AMQP 1.0 kablokodlu ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız oldu.|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir harita içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|Zamanlanan iletilerin sıra numarası. Sıra numarası iptal etmek için kullanılır.|  
  
### <a name="cancel-scheduled-message"></a>Zamanlanan İletiyi İptal Et  

Zamanlanan iletileri iptal eder.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|İptal etmek için zamanlanmış iletilerin sıra numaraları.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız oldu.|  
|statusDescription|string|Hayır|Durumun açıklaması.|   
  
## <a name="session-operations"></a>Oturum İşlemleri  
  
### <a name="session-renew-lock"></a>Oturum Yenile Kilidi  

İletinin kilitlerini varlık açıklamasında belirtilen zamana kadar uzatır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|oturum-id|string|Evet|Oturum Kimliği.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam - daha fazla mesaj var<br /><br /> 204: İçerik yok – mesaj yok|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir harita içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Sona erme|timestamp|Evet|Yeni bir son kullanma tarihi.|  
  
### <a name="peek-session-message"></a>Peek Oturum Mesajı  

Oturum iletilerini kilitlemeden gözetliyor.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|diziden numara|long|Evet|Hangi peek başlatmak için sıra numarası.|  
|ileti sayısı|int|Evet|Gözetlemeiçin maksimum ileti sayısı.|  
|oturum-id|string|Evet|Oturum Kimliği.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam - daha fazla mesaj var<br /><br /> 204: İçerik yok – mesaj yok|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir harita içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|harita listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
 İletiyi temsil eden harita aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1.0 kablokodlu ileti.|  
  
### <a name="set-session-state"></a>Oturum Durumunu Ayarla  

Oturumun durumunu ayarlar.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:set-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|oturum-id|string|Evet|Oturum Kimliği.|  
|oturum durumu|bayt dizisi|Evet|Opak ikili veri.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
### <a name="get-session-state"></a>Oturum Durumunu Al  

Bir oturumun durumunu alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|oturum-id|string|Evet|Oturum Kimliği.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|oturum durumu|bayt dizisi|Evet|Opak ikili veri.|  
  
### <a name="enumerate-sessions"></a>Oturumları Sayısallat  

Bir ileti kuruluşunda oturumları oyalar.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|son güncelleştirilen zaman|timestamp|Evet|Yalnızca belirli bir süre den sonra güncelleştirilen oturumları içerecek şekilde filtre uygulayın.|  
|Atla|int|Evet|Birkaç oturumu atlayın.|  
|üst|int|Evet|Maksimum oturum sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam - daha fazla mesaj var<br /><br /> 204: İçerik yok – mesaj yok|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Atla|int|Evet|Durum kodu 200 ise atlanan oturum sayısı.|  
|oturumlar-ids|dize dizisi|Evet|Durum kodu 200 ise oturum iI'leri dizisi.|  
  
## <a name="rule-operations"></a>Kural işlemleri  
  
### <a name="add-rule"></a>Kural Ekle  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil değildir.|  
|kural açıklaması|map|Evet|Sonraki bölümde belirtildiği gibi kural açıklaması.|  
  
**Kural açıklaması** eşlemi, **sql-filter** ve **korelasyon filtresinin** birbirini dışladığı aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sql-filtre|map|Evet|`sql-filter`, bir sonraki bölümde belirtildiği gibi.|  
|korelasyon filtresi|map|Evet|`correlation-filter`, bir sonraki bölümde belirtildiği gibi.|  
|sql-rule-action|map|Evet|`sql-rule-action`, bir sonraki bölümde belirtildiği gibi.|  
  
sql-filter eşlemi aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|Sql filtre ifadesi.|  
  
**Korelasyon filtresi** eşlemi aşağıdaki girişlerden en az birini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|correlation-id|string|Hayır||  
|mesaj kimliği|string|Hayır||  
|-|string|Hayır||  
|yanıt-to|string|Hayır||  
|etiket|string|Hayır||  
|oturum-id|string|Hayır||  
|oturuma yanıt-id|string|Hayır||  
|içerik türü|string|Hayır||  
|properties|map|Hayır|Haritalar Servis Veri [AracılıMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)için .|  
  
**sql-rule-action** eşlemi aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|Sql eylem ifadesi.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
### <a name="remove-rule"></a>Kuralı Kaldır  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil değildir.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
### <a name="get-rules"></a>Kurallar alın

#### <a name="request"></a>İstek

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:

|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  

İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|üst|int|Evet|Sayfada getirilen kural sayısı.|  
|Atla|int|Evet|Atlasılması gereken kural sayısı. Kurallar listesindeki başlangıç dizini (+1) tanımlar. | 

#### <a name="response"></a>Yanıt

Yanıt iletisi aşağıdaki özellikleri içerir:

|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|rules| harita dizisi|Evet|Bir dizi kural. Her kural bir harita ile temsil edilir.|

Dizideki her harita girişi aşağıdaki özellikleri içerir:

|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|kural açıklaması|açıklanan nesneler dizisi|Evet|`com.microsoft:rule-description:list`AMQP açıklanan kod 0x0000013700000004 ile| 

`com.microsoft.rule-description:list`açıklanan nesnelerin bir dizidir. Dizi aşağıdakileri içerir:

|Dizin oluşturma|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | açıklanan nesneler dizisi | Evet | `filter`aşağıda belirtildiği gibi. |
| 1 | açıklanan nesne dizisi | Evet | `ruleAction`aşağıda belirtildiği gibi. |
| 2 | string | Evet | kuralın adı. |

`filter`aşağıdaki türlerden biri olabilir:

| Tanımlayıcı Adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x00000137000006 | SQL filtresi |
| `com.microsoft:correlation-filter:list` | 0x00000137000009 | Korelasyon filtresi |
| `com.microsoft:true-filter:list` | 0x00000137000007 | 1=1'i temsil eden gerçek filtre |
| `com.microsoft:false-filter:list` | 0x00000137000008 | 1=0'ı temsil eden yanlış filtre |

`com.microsoft:sql-filter:list`içeren açıkbir dizidir:

|Dizin oluşturma|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | string | Evet | Sql Filtre ifadesi |

`com.microsoft:correlation-filter:list`içeren açıkbir dizidir:

|Dizin (varsa)|Değer Türü|Değer İçeriği|  
|---------|----------------|--------------|
| 0 | string | Bağıntı Kimliği |
| 1 | string | İleti Kimliği |
| 2 | string | Alıcı |
| 3 | string | Yanıtla |
| 4 | string | Etiketle |
| 5 | string | Oturum Kimliği |
| 6 | string | Oturum Kimliğini Yanıtlama|
| 7 | string | İçerik Türü |
| 8 | Eşleme | Uygulama tanımlı özellikleriharitası |

`ruleAction`aşağıdaki türlerden biri olabilir:

| Tanımlayıcı Adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x000001370000005 | Boş Kural Eylemi - Kural eylemi yok |
| `com.microsoft:sql-rule-action:list` | 0x000001370000006 | SQL Kural Eylemi |

`com.microsoft:sql-rule-action:list`ilk girişi SQL kural eyleminin ifadesini içeren bir dize olan açıklanan nesneler dizisidir.

## <a name="deferred-message-operations"></a>Ertelenmiş ileti işlemleri  
  
### <a name="receive-by-sequence-number"></a>Sıra numarasına göre alma  

Sıra numarasına göre ertelenmiş iletileri alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sıra numaraları|uzun dizi|Evet|Sıra numaraları.|  
|alıcı-yerleşme modu|ubayt|Evet|**AMQP** çekirdek v1.0'da belirtildiği gibi alıcı yerleşme modu.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|harita listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
İletiyi temsil eden harita aşağıdaki girişleri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|kilit belirteci|uuid|Evet|1 ise `receiver-settle-mode` belirteci kilitleyin.|  
|message|bayt dizisi|Evet|AMQP 1.0 kablokodlu ileti.|  
  
### <a name="update-disposition-status"></a>Kullanım durumunu güncelleştirme  

Ertelenmiş iletilerin eğilim durumunu güncelleştirir. Bu işlem hareketleri destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Işlem|string|Evet|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Hayır|Milisaniye cinsinden sunucu zaman dışarısı işlemi.|  
  
İstek iletisi gövdesi, aşağıdaki girişleri içeren bir **harita** içeren bir **amqp değeri** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|disposition-durum|string|Evet|Tamamlandı<br /><br /> Terk edilmiş<br /><br /> Askıya|  
|kilit belirteçleri|uuid dizi|Evet|Kullanım durumunu güncelleştirmek için ileti kilidi belirteçleri.|  
|deadletter-neden|string|Hayır|Disposition durumu **askıya alınacak**şekilde ayarlanmışsa ayarlanabilir.|  
|deadletter açıklama|string|Hayır|Disposition durumu **askıya alınacak**şekilde ayarlanmışsa ayarlanabilir.|  
|özellikleri-değiştirmek için|map|Hayır|Değişiklik yapmak üzere Servis Veri Kurumu aracılı ileti özelliklerinin listesi.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer İçeriği|  
|---------|----------------|--------------|--------------------|  
|Statuscode|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarı, aksi takdirde başarısız|  
|statusDescription|string|Hayır|Durumun açıklaması.|

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Servis Veri Yolu hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:

* [Servis Veri Servisi AMQP'ye genel bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için Hizmet Veri Servisi'nde AMQP]

[Servis Veri Servisi AMQP'ye genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için Hizmet Veri Servisi'nde AMQP]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
