---
title: Azure Service Bus AMQP 1,0 istek/yanıt işlemleri
description: Bu makalede Microsoft Azure Service Bus içindeki AMQP istek/yanıt tabanlı işlemlerin listesi tanımlanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 82eb968cc952fab6e828c8b98f4ec2ba569b4441
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85337842"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>Microsoft Azure Service Bus içindeki AMQP 1,0: istek-yanıt tabanlı işlemler

Bu makalede, Microsoft Azure Service Bus istek/yanıt tabanlı işlemlerin listesi tanımlanmaktadır. Bu bilgiler, AMQP yönetimi sürüm 1,0 çalışma taslağı ' nı temel alır.  
  
Service Bus, OASIN AMQP teknik belirtiminde nasıl çalıştığını ve oluşturulduğunu anlatan ayrıntılı bir tel düzeyi AMQP 1,0 protokol Kılavuzu için, [Azure Service Bus ve Event Hubs protokol Kılavuzu][AMQP 1,0 protokol Kılavuzu]' 1,0 na bakın.  
  
## <a name="concepts"></a>Kavramlar  
  
### <a name="entity-description"></a>Varlık açıklaması  

Bir varlık açıklaması, Service Bus [Queuedescription sınıfına](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [topicdescription sınıfına](/dotnet/api/microsoft.servicebus.messaging.topicdescription)veya [subscriptiondescription sınıfı](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) nesnesine başvurur.  
  
### <a name="brokered-message"></a>Aracılı ileti  

Bir AMQP iletisiyle eşlenen Service Bus bir iletiyi temsil eder. Eşleme [Service Bus AMQP protokol kılavuzunda](service-bus-amqp-protocol-guide.md)tanımlanmıştır.  
  
## <a name="attach-to-entity-management-node"></a>Varlık Yönetimi düğümüne Ekle  

Bu belgede açıklanan tüm işlemler bir istek/yanıt modelini izler, bir varlığa göre kapsamlandırılır ve bir varlık yönetim düğümüne ekleme yapılmasını gerektirir.  
  
### <a name="create-link-for-sending-requests"></a>İstek göndermek için bağlantı oluştur  

İstekleri göndermek için Yönetim düğümüne bir bağlantı oluşturur.  
  
```
requestLink = session.attach(
role: SENDER,
    target: { address: "<entity address>/$management" },
    source: { address: ""<my request link unique address>" }
)

```
  
### <a name="create-link-for-receiving-responses"></a>Yanıtları almak için bağlantı oluştur  

Yönetim düğümünden yanıt almak için bir bağlantı oluşturur.  
  
```
responseLink = session.attach(
role: RECEIVER,
    source: { address: "<entity address>/$management" }
    target: { address: "<my response link unique address>" }
)

```
  
### <a name="transfer-a-request-message"></a>İstek iletisini aktarma  

İstek iletisini aktarır.  
İşlem durumu, işlemi destekleyen işlemler için isteğe bağlı olarak eklenebilir.

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
  
### <a name="receive-a-response-message"></a>Yanıt iletisi al  

Yanıt bağlantısından yanıt iletisini alır.  
  
```
responseMessage = responseLink.receiveTransfer()
```
  
Yanıt iletisi şu biçimdedir:
  
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
  
### <a name="service-bus-entity-address"></a>Service Bus varlık adresi  

Service Bus varlıkların aşağıdaki şekilde giderilmesi gerekir:  
  
|Varlık türü|Adres|Örnek|  
|-----------------|-------------|-------------|  
|kuyruk|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|konu başlığı|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|aboneliği|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>İleti işlemleri  
  
### <a name="message-renew-lock"></a>İletiyi yenileme kilidi  

Bir iletinin kilidini varlık açıklamasında belirtilen zamana göre genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
 İstek iletisi gövdesi, aşağıdaki girdilerle bir eşleme içeren bir AMQP-Value bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|UUID dizisi|Evet|Yenileme için ileti kilit belirteçleri.|  

> [!NOTE]
> Kilit belirteçleri, `DeliveryTag` alınan iletilerdeki özelliktir. [.NET SDK](https://github.com/Azure/azure-service-bus-dotnet/blob/6f144e91310dcc7bd37aba4e8aebd535d13fa31a/src/Microsoft.Azure.ServiceBus/Amqp/AmqpMessageConverter.cs#L336) 'da bunları alan aşağıdaki örneğe bakın. Belirteç aynı zamanda ' x-opt-Lock-token ' olarak ' teslimat ' olarak da görünebilir, ancak bu garanti edilmez ve `DeliveryTag` tercih edilmelidir. 
> 
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu.|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir eşleme içeren bir AMQP-Value bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|süreleri|zaman damgası dizisi|Evet|İleti kilit belirtecinin istek kilidi belirteçlerine karşılık gelen yeni süre sonu.|  
  
### <a name="peek-message"></a>Iletiye Gözat  

İletileri kilitlemeden atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|long|Evet|Göz atmayı başlatacak sıra numarası.|  
|`message-count`|int|Evet|Göz atma için en fazla ileti sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti içerir<br /><br /> 204: içerik yok – daha fazla ileti yok|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|Haritalar listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
Bir iletiyi temsil eden haritanın aşağıdaki girişleri içermesi gerekir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1,0 kablo kodlu ileti.|  
  
### <a name="schedule-message"></a>Ileti zamanla  

İletileri zamanlar. Bu işlem işlemi destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|Haritalar listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
Bir iletiyi temsil eden haritanın aşağıdaki girişleri içermesi gerekir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|ileti kimliği|string|Evet|`amqpMessage.Properties.MessageId`As String|  
|oturum kimliği|dize|No|`amqpMessage.Properties.GroupId as string`|  
|Bölüm-anahtar|dize|No|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|ile-bölüm-anahtar|dize|No|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|bayt dizisi|Evet|AMQP 1,0 kablo kodlu ileti.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu.|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir eşleme içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|dizi sayıları|Long dizisi|Evet|Zamanlanan iletilerin sıra sayısı. İptal etmek için sıra numarası kullanılır.|  
  
### <a name="cancel-scheduled-message"></a>Zamanlanan Iletiyi iptal et  

Zamanlanan iletileri iptal eder.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|dizi sayıları|Long dizisi|Evet|İptal edilecek zamanlanmış iletilerin sıra numaraları.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu.|  
|statusDescription|dize|No|Durumun açıklaması.|   
  
## <a name="session-operations"></a>Oturum Işlemleri  
  
### <a name="session-renew-lock"></a>Oturum yenileme kilidi  

Bir iletinin kilidini varlık açıklamasında belirtilen zamana göre genişletir.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum KIMLIĞI.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti içerir<br /><br /> 204: içerik yok – daha fazla ileti yok|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir eşleme içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|dolmadan|timestamp|Evet|Yeni süre sonu.|  
  
### <a name="peek-session-message"></a>Oturum Iletisini Peek  

Oturum iletilerini kilitlemeden atar.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|sıra-sayı|long|Evet|Göz atmayı başlatacak sıra numarası.|  
|ileti sayısı|int|Evet|Göz atma için en fazla ileti sayısı.|  
|oturum kimliği|string|Evet|Oturum KIMLIĞI.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti içerir<br /><br /> 204: içerik yok – daha fazla ileti yok|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir eşleme içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|Haritalar listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
 Bir iletiyi temsil eden haritanın aşağıdaki girişleri içermesi gerekir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|message|bayt dizisi|Evet|AMQP 1,0 kablo kodlu ileti.|  
  
### <a name="set-session-state"></a>Oturum durumunu ayarla  

Bir oturumun durumunu ayarlar.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:set-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum KIMLIĞI.|  
|oturum durumu|bayt dizisi|Evet|Donuk ikili veri.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
### <a name="get-session-state"></a>Oturum durumunu al  

Bir oturumun durumunu alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum kimliği|string|Evet|Oturum KIMLIĞI.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|oturum durumu|bayt dizisi|Evet|Donuk ikili veri.|  
  
### <a name="enumerate-sessions"></a>Oturumları listeleme  

Bir mesajlaşma varlığındaki oturumları numaralandırır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Son Güncelleme Zamanı|timestamp|Evet|Yalnızca belirli bir süreden sonra güncellenen oturumları dahil etmek için filtreleyin.|  
|Atla|int|Evet|Bir dizi oturumu atlayın.|  
|üst|int|Evet|En fazla oturum sayısı.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – daha fazla ileti içerir<br /><br /> 204: içerik yok – daha fazla ileti yok|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Atla|int|Evet|Durum kodu 200 ise atlanan oturum sayısı.|  
|oturum kimlikleri|dize dizisi|Evet|Durum kodu 200 ise oturum kimlikleri dizisi.|  
  
## <a name="rule-operations"></a>Kural işlemleri  
  
### <a name="add-rule"></a>Kural Ekle  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil değildir.|  
|kural-açıklama|map|Evet|Sonraki bölümde belirtilen kural açıklaması.|  
  
**Kural açıklaması** haritasında, **SQL-Filter** ve **bağıntı-filtresi** birbirini dışlamalı aşağıdaki girişler yer almalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|SQL filtresi|map|Evet|`sql-filter`, sonraki bölümde belirtildiği gibi.|  
|bağıntı-filtre|map|Evet|`correlation-filter`, sonraki bölümde belirtildiği gibi.|  
|SQL-Rule-Action|map|Evet|`sql-rule-action`, sonraki bölümde belirtildiği gibi.|  
  
SQL-Filter eşlemesi aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|SQL filtresi ifadesi.|  
  
**Bağıntı-filtre** eşlemesi aşağıdaki girdilerden en az birini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|correlation-id|dize|No||  
|ileti kimliği|dize|No||  
|-|dize|No||  
|Yanıtla|dize|No||  
|etiket|dize|No||  
|oturum kimliği|dize|No||  
|Yanıtla oturum kimliği|dize|No||  
|içerik türü|dize|No||  
|properties|map|Hayır|Service Bus [Brokeredmessage. Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)ile eşlenir.|  
  
**SQL-Rule-Action** eşlemesi aşağıdaki girdileri içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|ifade|string|Evet|SQL eylem ifadesi.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
### <a name="remove-rule"></a>Kuralı kaldır  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Kural adı|string|Evet|Kural adı, abonelik ve konu adları dahil değildir.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
### <a name="get-rules"></a>Kuralları al

#### <a name="request"></a>İstek

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:

|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  

İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|üst|int|Evet|Sayfada getirilecek kuralların sayısı.|  
|Atla|int|Evet|Atlanacak kural sayısı. Kurallar listesinde başlangıç dizinini (+ 1) tanımlar. | 

#### <a name="response"></a>Yanıt

Yanıt iletisi aşağıdaki özellikleri içerir:

|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|rules| harita dizisi|Evet|Kural dizisi. Her kural bir eşleme tarafından temsil edilir.|

Dizideki her harita girişi aşağıdaki özellikleri içerir:

|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|kural-açıklama|açıklanan nesneler dizisi|Evet|`com.microsoft:rule-description:list`AMQP ile tanımlanan kod 0x0000013700000004| 

`com.microsoft.rule-description:list`, açıklanan nesnelerden oluşan bir dizidir. Dizi şunları içerir:

|Dizin oluşturma|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | açıklanan nesneler dizisi | Evet | `filter`Aşağıda belirtildiği gibi. |
| 1 | açıklanan nesne dizisi | Evet | `ruleAction`Aşağıda belirtildiği gibi. |
| 2 | string | Evet | kuralın adı. |

`filter`Aşağıdaki türlerden biri olabilir:

| Tanımlayıcı adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | SQL filtresi |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Bağıntı filtresi |
| `com.microsoft:true-filter:list` | 0x000001370000007 | 1 = 1 temsil eden doğru filtre |
| `com.microsoft:false-filter:list` | 0x000001370000008 | 1 = 0 temsil eden yanlış filtre |

`com.microsoft:sql-filter:list`, aşağıdakiler dahil olmak üzere tarif eden bir dizidir:

|Dizin oluşturma|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
| 0 | string | Evet | SQL filtresi ifadesi |

`com.microsoft:correlation-filter:list`, aşağıdakiler dahil olmak üzere tarif eden bir dizidir:

|Dizin (varsa)|Değer Türü|Değer Içeriği|  
|---------|----------------|--------------|
| 0 | string | Bağıntı Kimliği |
| 1 | string | İleti KIMLIĞI |
| 2 | string | Alıcı |
| 3 | string | Yanıtla |
| 4 | string | Etiketle |
| 5 | string | Oturum Kimliği |
| 6 | string | Oturum KIMLIĞINI yanıtla|
| 7 | string | İçerik Türü |
| 8 | Harita | Uygulama tanımlı özelliklerin Haritası |

`ruleAction`Aşağıdaki türlerden biri olabilir:

| Tanımlayıcı adı | Tanımlayıcı kodu | Değer |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Boş kural eylemi-kural eylemi yok |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | SQL kuralı eylemi |

`com.microsoft:sql-rule-action:list`, ilk girdisi SQL kuralı eyleminin ifadesini içeren bir dize olan tanımlı nesnelerden oluşan bir dizidir.

## <a name="deferred-message-operations"></a>Ertelenmiş ileti işlemleri  
  
### <a name="receive-by-sequence-number"></a>Sıra numarasına göre al  

Ertelenmiş iletileri sıra numarasına göre alır.  
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|dizi sayıları|Long dizisi|Evet|Sıra numaraları.|  
|alıcı-kapatma modu|ubde|Evet|AMQP Core v 1.0 'da belirtilen şekilde **alıcı kapatma** modu.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|  
  
Yanıt iletisi gövdesi, aşağıdaki girişlerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|sayısı|Haritalar listesi|Evet|Her haritanın bir iletiyi temsil ettiği iletilerin listesi.|  
  
Bir iletiyi temsil eden haritanın aşağıdaki girişleri içermesi gerekir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|kilit belirteci|uuid|Evet|`receiver-settle-mode`1 ise kilit belirteci.|  
|message|bayt dizisi|Evet|AMQP 1,0 kablo kodlu ileti.|  
  
### <a name="update-disposition-status"></a>Değerlendirme durumunu güncelleştir  

Ertelenmiş iletilerin değerlendirme durumunu güncelleştirir. Bu işlem işlemleri destekler.
  
#### <a name="request"></a>İstek  

İstek iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|çalışmasını|string|Evet|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Hayır|İşlem sunucusu zaman aşımı (milisaniye).|  
  
İstek iletisi gövdesi, aşağıdaki girdilerle bir **eşleme** içeren bir **AMQP-Value** bölümünden oluşmalıdır:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|disposition-durum|string|Evet|dım<br /><br /> bırakıldı<br /><br /> alın|  
|kilit belirteçleri|UUID dizisi|Evet|Değerlendirme durumunu güncelleştirmek için ileti kilit belirteçleri.|  
|sahipsiz neden|dize|No|, Değerlendirme durumu **askıya alındı**olarak ayarlandıysa ayarlanabilir.|  
|sahipsiz-açıklama|dize|No|, Değerlendirme durumu **askıya alındı**olarak ayarlandıysa ayarlanabilir.|  
|Özellikler-değiştirilecek|map|Hayır|Değiştirilecek Service Bus aracılı ileti özellikleri listesi.|  
  
#### <a name="response"></a>Yanıt  

Yanıt iletisi aşağıdaki uygulama özelliklerini içermelidir:  
  
|Anahtar|Değer Türü|Gerekli|Değer Içeriği|  
|---------|----------------|--------------|--------------------|  
|Durum|int|Evet|HTTP yanıt kodu [RFC2616]<br /><br /> 200: Tamam – başarılı, aksi durumda başarısız oldu|  
|statusDescription|dize|No|Durumun açıklaması.|

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:

* [Service Bus AMQP 'ye Genel Bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için Service Bus AMQP]

[Service Bus AMQP 'ye Genel Bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için Service Bus AMQP]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
