---
title: Azure Iletişim hizmetlerinde sohbet kavramları
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri sohbet kavramları hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e05bf1df503a13efc8e4ca30b3341216e01e678e
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105110840"
---
# <a name="chat-concepts"></a>Sohbet kavramları 

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]    

Azure Iletişim Hizmetleri sohbeti SDK 'Ları, uygulamalarınıza gerçek zamanlı metin sohbeti eklemek için kullanılabilir. Bu sayfa, önemli sohbet kavramlarını ve yeteneklerini özetler.    

Belirli SDK dilleri ve özellikleri hakkında daha fazla bilgi için bkz. [Iletişim Hizmetleri sohbet SDK 'Sına genel bakış](./sdk-features.md) .  

## <a name="chat-overview"></a>Sohbete genel bakış    

Sohbet iş parçacıkları içinde sohbet görüşmeleri gerçekleşir. Sohbet iş parçacığı birçok ileti ve çok sayıda kullanıcı içerebilir. Her ileti tek bir iş parçacığına aittir ve bir Kullanıcı bir veya daha fazla iş parçacığının parçası olabilir. Sohbet iş parçacığındaki her kullanıcıya katılımcı denir. Yalnızca iş parçacığı katılımcıları, ileti alıp alabilir ve sohbet iş parçacığında başka kullanıcılar ekleyebilir veya kaldırabilir. İletişim Hizmetleri, sohbet iş parçacığı veya iletisinde silme işlemini yürütmeden veya sohbet iş parçacığında hiçbir katılımcı kalana kadar sohbet geçmişini depolar, bu noktada sohbet iş parçacığı yalnız bırakılmış ve silinmek üzere sıraya alınır. 
    
## <a name="service-limits"></a>Hizmet sınırlamaları   

- Sohbet iş parçacığında izin verilen en fazla katılımcı sayısı 250 ' dir.   
- İzin verilen en büyük ileti boyutu yaklaşık 28 KB 'dir.  
- 20 ' den fazla katılımcı içeren sohbet iş parçacıkları için okundu bilgisi ve yazma göstergesi özellikleri desteklenmez.    
- 
## <a name="chat-architecture"></a>Sohbet mimarisi    

Sohbet mimarisinin iki temel bölümü vardır: 1) Güvenilen hizmet ve 2) Istemci uygulaması.    

:::image type="content" source="../../media/chat-architecture.png" alt-text="Iletişim hizmetlerinin sohbet mimarisini gösteren diyagram."::: 

 - **Güvenilen hizmet:** Bir sohbet oturumunu doğru şekilde yönetmek için, kaynak Bağlantı dizenizi kullanarak Iletişim hizmetlerine bağlanmanıza yardımcı olan bir hizmete ihtiyacınız vardır. Bu hizmet, sohbet iş parçacıkları oluşturmaktan, iş parçacığı katılımcısı listelerinin yönetilmesine ve kullanıcılara erişim belirteçleri sağlamaya sorumludur. Erişim belirteçleri hakkında daha fazla bilgi [erişim belirteçlerimiz](../../quickstarts/access-tokens.md) hızlı başlangıç bölümünde bulunabilir.   
 - **İstemci uygulaması:**  İstemci uygulaması, güvenilen hizmetinize bağlanır ve Iletişim hizmetlerine doğrudan bağlanmak için kullanılan erişim belirteçlerini alır. Bu bağlantı yapıldıktan sonra istemci uygulamanız ileti gönderebilir ve alabilir.   
Güvenilen hizmet katmanını kullanarak erişim belirteçleri oluşturmamız önerilir. Bu senaryoda, sunucu tarafı kullanıcıları oluşturup yönetmekten ve belirteçlerini yayınmaktan sorumludur.   
        
## <a name="message-types"></a>İleti türleri    

İletişim Hizmetleri sohbeti, Kullanıcı tarafından üretilen iletileri ve **Iş parçacığı etkinlikleri** adlı sistem tarafından oluşturulan iletileri paylaşır. İş parçacığı etkinlikleri, bir sohbet iş parçacığı güncelleştirildiği zaman üretilir. `List Messages` `Get Messages` Bir sohbet iş parçacığı çağırdığınızda, sonuç Kullanıcı tarafından oluşturulan metin iletilerini ve sistem iletilerini kronolojik sırada içerecektir. Bu, bir katılımcının ne zaman eklendiğini veya kaldırıldığını veya sohbet iş parçacığı konusunun ne zaman güncelleştirildiğini belirlemenize yardımcı olur. Desteklenen ileti türleri şunlardır:  
    
 - `Text`: Bir sohbet görüşmesinin parçası olarak bir kullanıcı tarafından oluşturulan ve gönderilen düz metin iletisi. 
 - `RichText/HTML`: Biçimli bir SMS mesajı. Iletişim Hizmetleri kullanıcılarının şu anda zengin metin iletileri gönderemediğini unutmayın. Bu ileti türü, takımlar tarafından kullanıcılara, takımlar birlikte çalışma senaryolarında Iletişim Hizmetleri kullanıcılarına gönderilen iletiler tarafından desteklenir.   
 - `ThreadActivity/ParticipantAdded`: Sohbet iş parçacığına bir veya daha fazla katılımcı eklendiğini belirten bir sistem iletisi. Örnek: 


``` 
{   
            "id": "1613589626560",  
            "type": "participantAdded", 
            "sequenceId": "7",  
            "version": "1613589626560", 
            "content":  
            {   
                "participants": 
                [   
                    {   
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",    
                        "displayName": "Jane",  
                        "shareHistoryTime": "1970-01-01T00:00:00Z"  
                    }   
                ],  
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:26Z" 
        }   
``` 

- `ThreadActivity/ParticipantRemoved`: Bir katılımcının sohbet iş parçacığından kaldırıldığını belirten sistem iletisi. Örnek:  

``` 
{   
            "id": "1613589627603",  
            "type": "participantRemoved",   
            "sequenceId": "8",  
            "version": "1613589627603", 
            "content":  
            {   
                "participants": 
                [   
                    {   
                        "id": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4df6-f40f-343a0d003226",    
                        "displayName": "Jane",  
                        "shareHistoryTime": "1970-01-01T00:00:00Z"  
                    }   
                ],  
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:27Z" 
        }   
``` 

- `ThreadActivity/TopicUpdate`: İş parçacığı konusunun güncelleştirildiğini belirten sistem iletisi. Örnek:   
``` 
{   
            "id": "1613589623037",  
            "type": "topicUpdated", 
            "sequenceId": "2",  
            "version": "1613589623037", 
            "content":  
            {   
                "topic": "New topic",   
                "initiator": "8:acs:d2a829bc-8523-4404-b727-022345e48ca6_00000008-511c-4ce0-f40f-343a0d003224"  
            },  
            "createdOn": "2021-02-17T19:20:23Z" 
        }   
``` 

## <a name="real-time-signaling"></a>Gerçek zamanlı sinyal  

Sohbet JavaScript SDK 'Sı gerçek zamanlı sinyal içerir. Bu, istemcilerin API 'Leri yoklamaya gerek kalmadan bir sohbet iş parçacığına gerçek zamanlı güncelleştirmeleri ve gelen iletileri dinlemesine olanak tanır. Kullanılabilir olaylar şunlardır:

 - `ChatMessageReceived` -bir sohbet iş parçacığına yeni bir ileti gönderildiğinde. Bu olay, önceki konu başlığında ele alınan otomatik oluşturulan sistem iletileri için gönderilmez.   
 - `ChatMessageEdited` -bir sohbet iş parçacığında bir ileti düzenlendiğinde. 
 - `ChatMessageDeleted` -bir sohbet iş parçacığında bir ileti silindiğinde.   
 - `TypingIndicatorReceived` -başka bir katılımcı sohbet iş parçacığında bir ileti yazarken.   
 - `ReadReceiptReceived` -başka bir katılımcı bir kullanıcının sohbet iş parçacığında gönderdiği iletiyi okumıştır.     
 - `ChatThreadCreated` -bir iletişim kullanıcısı tarafından bir sohbet iş parçacığı oluşturulduğunda. 
 - `ChatThreadDeleted` -bir sohbet iş parçacığı bir iletişim kullanıcısı tarafından silindiğinde. 
 - `ChatThreadPropertiesUpdated` -sohbet iş parçacığı özellikleri güncelleniyorsa; Şu anda, iş parçacığı için yalnızca konuyu güncelleştirmeyi destekliyoruz.   
 - `ParticipantsAdded` -bir Kullanıcı bir sohbet iş parçacığına katılımcı olarak eklendiğinde.  
 - `ParticipantsRemoved` -Mevcut bir katılımcı sohbet iş parçacığından kaldırıldığında.


## <a name="chat-events"></a>Sohbet etkinlikleri  

Gerçek zamanlı sinyal, kullanıcılarınızın gerçek zamanlı olarak sohbet etmesine olanak tanır. Hizmetleriniz, sohbet ile ilgili olaylara abone olmak için Azure Event Grid kullanabilir. Daha ayrıntılı bilgi için bkz. [olay işleme kavramsal](https://docs.microsoft.com/azure/event-grid/event-schema-communication-services?tabs=event-grid-event-schema).


## <a name="using-cognitive-services-with-chat-sdk-to-enable-intelligent-features"></a>Akıllı özellikleri etkinleştirmek için öğretici hizmetlerini sohbet SDK ile kullanma    

Uygulamalarınıza akıllı özellikler eklemek için sohbet SDK ile Azure bilişsel [API 'leri](../../../cognitive-services/index.yml) kullanabilirsiniz. Örneğin, şunları yapabilirsiniz: 

- Kullanıcıların farklı dillerde birbirleriyle sohbet kurmasını sağlar.  
- Bir müşterinin gelen bir sorunun olumsuz bir yaklaşımını algılayarak, destek aracısına bilet önceliklendirmesine yardımcı olun.   
- Anahtar algılama ve varlık tanıma için gelen iletileri çözümleyin ve ileti içeriğine göre uygulamanızdaki kullanıcıya ilgili bilgileri yazın.

Bunu başarmanın bir yolu, güvenilen hizmetinizin sohbet iş parçacığı katılımcısı olarak davranmasından biridir. Dil çevirisini etkinleştirmek istediğinizi varsayalım. Bu hizmet, diğer katılımcılar tarafından düzenlenmekte olan iletilere dinleme yapmaktan sorumlu olacaktır. [1], içeriği istenen dile dönüştürmek için bilişsel API 'Ler çağrılıyor [2, 3] ve çevrilmiş sonucu [4] sohbet iş parçacığında ileti olarak gönderiyor.

Bu şekilde, ileti geçmişi hem özgün hem de çevrilmiş iletileri içerir. İstemci uygulamasında, orijinal veya çevrilmiş iletiyi göstermek için Logic ekleyebilirsiniz. Bilişsel API 'Leri kullanarak metni farklı dillere çevirme hakkında bilgi edinmek için [Bu hızlı](../../../cognitive-services/translator/quickstart-translator.md) başlangıca bakın. 
    
:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Iletişim hizmetleriyle etkileşime geçen bilişsel hizmetler 'i gösteren diyagram."::: 

## <a name="next-steps"></a>Sonraki adımlar   

> [!div class="nextstepaction"] 
> [Sohbetle çalışmaya başlama](../../quickstarts/chat/get-started.md)    

Aşağıdaki belgeler sizin için ilginç olabilir:  
- [Sohbet SDK 'sını](sdk-features.md) öğrenmeye