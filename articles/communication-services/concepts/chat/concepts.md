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
ms.openlocfilehash: c3b4970b70a9bd5399199771a081481e17e2efb1
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91667002"
---
# <a name="chat-concepts"></a>Sohbet kavramları

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri sohbet istemci kitaplıkları, uygulamalarınıza gerçek zamanlı metin sohbeti eklemek için kullanılabilir. Bu sayfa, önemli sohbet kavramlarını ve yeteneklerini özetler.

Belirli istemci kitaplığı dilleri ve özellikleri hakkında daha fazla bilgi edinmek için [Iletişim Hizmetleri sohbeti istemci kitaplığına genel bakış](./sdk-features.md) bölümüne bakın.

## <a name="chat-overview"></a>Sohbete genel bakış 

Sohbet iş parçacıkları içinde sohbet görüşmeleri gerçekleşir. Sohbet iş parçacığı birçok ileti ve çok sayıda kullanıcı içerebilir. Her ileti tek bir iş parçacığına aittir ve bir Kullanıcı bir veya daha fazla iş parçacığının parçası olabilir. 

Sohbet iş parçacığındaki her kullanıcıya üye denir. Bir sohbet iş parçacığında en fazla 250 üye olabilir. Yalnızca iş parçacığı üyeleri bir sohbet iş parçacığına ileti gönderebilir ve alabilir ya da üye ekleyebilir/kaldırabilir. İzin verilen en büyük ileti boyutu yaklaşık 28KB 'dir. İşlem kullanarak bir sohbet iş parçacığındaki tüm iletileri alabilirsiniz `List/Get Messages` . İletişim Hizmetleri, sohbet iş parçacığı veya iletisi üzerinde silme işlemi yürütülene veya sohbet iş parçacığında kalan ve silinmek üzere işlendiği bir üye kalmayana kadar sohbet geçmişini depolar.   

20 ' den fazla üye içeren sohbet iş parçacıkları için okundu bilgisi ve yazma göstergesi özellikleri devre dışı bırakılır. 

## <a name="chat-architecture"></a>Sohbet mimarisi

Sohbet mimarisinin iki temel bölümü vardır: 1) Güvenilen hizmet ve 2) Istemci uygulaması.

:::image type="content" source="../../media/chat-architecture.png" alt-text="Iletişim hizmetlerinin sohbet mimarisini gösteren diyagram.":::

 - **Güvenilen hizmet:** Bir sohbet oturumunu doğru şekilde yönetmek için, kaynak Bağlantı dizenizi kullanarak Iletişim hizmetlerine bağlanmanıza yardımcı olan bir hizmete ihtiyacınız vardır. Bu hizmet, sohbet iş parçacıkları oluşturmaktan, iş parçacığı üyeliklerini yönetmeye ve kullanıcılara erişim belirteçleri sağlamaya sorumludur. Erişim belirteçleri hakkında daha fazla bilgi [erişim belirteçlerimiz](../../quickstarts/access-tokens.md) hızlı başlangıç bölümünde bulunabilir.

 - **İstemci uygulaması:**  İstemci uygulaması, güvenilen hizmetinize bağlanır ve Iletişim hizmetlerine doğrudan bağlanmak için kullanılan erişim belirteçlerini alır. Bu bağlantı yapıldıktan sonra istemci uygulamanız ileti gönderebilir ve alabilir.
    
## <a name="message-types"></a>İleti türleri

İletişim Hizmetleri sohbeti, Kullanıcı tarafından üretilen iletileri ve **Iş parçacığı etkinlikleri**adlı sistem tarafından oluşturulan iletileri paylaşır. İş parçacığı etkinlikleri, bir sohbet iş parçacığı güncelleştirildiği zaman üretilir. `List Messages` `Get Messages` Bir sohbet iş parçacığı çağırdığınızda, sonuç Kullanıcı tarafından oluşturulan metin iletilerini ve sistem iletilerini kronolojik sırada içerecektir. Bu, bir üyenin ne zaman eklendiğini veya kaldırıldığını veya sohbet iş parçacığı konusunun ne zaman güncelleştirildiğini tanımlamanızı sağlar. Desteklenen ileti türleri şunlardır:  

 - `Text`: Sohbet konuşması kapsamında Kullanıcı tarafından oluşturulan ve gönderilen gerçek ileti. 
 - `ThreadActivity/AddMember`: Sohbet iş parçacığına bir veya daha fazla üye eklendiğini belirten sistem iletisi. Örnek:

```xml

<addmember>
    <eventtime>1598478187549</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <detailedinitiatorinfo>
        <friendlyName>User 1</friendlyName>
    </detailedinitiatorinfo>
    <rosterVersion>1598478184564</rosterVersion>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</id>
        <friendlyName>User 1</friendlyName>
    </detailedtargetinfo>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</id>
        <friendlyName>User 2</friendlyName>
    </detailedtargetinfo>
</addmember>

```  

- `ThreadActivity/DeleteMember`: Bir üyenin sohbet iş parçacığından kaldırıldığını belirten sistem iletisi. Örnek:

```xml

<deletemember>
    <eventtime>1598478187642</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <detailedinitiatorinfo>
        <friendlyName>User 1</friendlyName>
    </detailedinitiatorinfo>
    <rosterVersion>1598478184564</rosterVersion>
    <target>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</target>
    <detailedtargetinfo>
        <id>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_8540c0de-899f-5cce-acb5-3ec493af3800</id>
        <friendlyName>User 2</friendlyName>
    </detailedtargetinfo>
</deletemember>

```

- `ThreadActivity/TopicUpdate`: Konunun güncelleştirildiğini belirten sistem iletisi. Örnek:

```xml

<topicupdate>
    <eventtime>1598477591811</eventtime>
    <initiator>8:acs:57b9bac9-df6c-4d39-a73b-26e944adf6ea_0e59221d-0c1d-46ae-9544-c963ce56c10b</initiator>
    <value>New topic</value>
</topicupdate>

```

## <a name="real-time-signaling"></a>Gerçek zamanlı sinyal 

Sohbet JavaScript istemci kitaplığı gerçek zamanlı sinyal içerir. Bu, istemcilerin API 'Leri yoklamaya gerek kalmadan bir sohbet iş parçacığına gerçek zamanlı güncelleştirmeleri ve gelen iletileri dinlemesine olanak tanır. Kullanılabilir olaylar şunlardır:

 - `ChatMessageReceived` -kullanıcının üyesi olduğu bir sohbet iş parçacığına yeni bir ileti gönderildiğinde. Bu olay, önceki konu başlığında açıklandığımız otomatik oluşturulan sistem iletileri için gönderilmez.  
 - `ChatMessageEdited` -kullanıcının üyesi olduğu bir sohbet iş parçacığında bir ileti düzenlendiğinde. 
 - `ChatMessageDeleted` -kullanıcının üyesi olduğu bir sohbet iş parçacığında bir ileti silindiğinde. 
 - `TypingIndicatorReceived` -başka bir üye, kullanıcının üyesi olduğu bir sohbet iş parçacığına ileti yazarken. 
 - `ReadReceiptReceived` -başka bir üye kullanıcının sohbet iş parçacığında gönderdiği iletiyi okumalıdır. 

## <a name="chat-events"></a>Sohbet etkinlikleri 

Gerçek zamanlı sinyal, kullanıcılarınızın gerçek zamanlı olarak sohbet etmesine olanak tanır. Hizmetleriniz, sohbet ile ilgili olaylara abone olmak için Azure Event Grid kullanabilir. Daha ayrıntılı bilgi için bkz. [olay işleme kavramsal](../event-handling.md).

## <a name="using-cognitive-services-with-chat-client-library-to-enable-intelligent-features"></a>Akıllı özellikleri etkinleştirmek için öğretici hizmetlerini sohbet istemci kitaplığı ile kullanma

Uygulamalarınıza akıllı özellikler eklemek için sohbet istemci kitaplığıyla Azure bilişsel [API 'leri](https://docs.microsoft.com/azure/cognitive-services/) kullanabilirsiniz. Örneğin, şunları yapabilirsiniz:

- Kullanıcıların farklı dillerde birbirleriyle sohbet kurmasını sağlar. 
- Bir müşterinin gelen bir sorunun olumsuz bir yaklaşımını algılayarak, destek aracısına bilet önceliklendirmesine yardımcı olun.
- Anahtar algılama ve varlık tanıma için gelen iletileri çözümleyin ve ileti içeriğine göre uygulamanızdaki kullanıcıya ilgili bilgileri yazın.

Bunu başarmanın bir yolu, güvenilen hizmetinizin sohbet iş parçacığı üyesi olarak davranmasından biridir. Dil çevirisini etkinleştirmek istediğinizi varsayalım. Bu hizmet, diğer üyelerin [1] tarafından düzenlenmekte olan iletileri dinlemeden, bilişsel API 'Leri çağırarak içeriği istenen dile [2, 3] dönüştürmek ve [4] sohbet iş parçacığında çevrilmiş sonucu bir ileti olarak göndermek için sorumlu olacaktır. 

Bu şekilde, ileti geçmişi hem özgün hem de çevrilmiş iletileri içerir. İstemci uygulamasında, orijinal veya çevrilmiş iletiyi göstermek için Logic ekleyebilirsiniz. Bilişsel API 'Leri kullanarak metni farklı dillere çevirme hakkında bilgi edinmek için [Bu hızlı](https://docs.microsoft.com/azure/cognitive-services/translator/quickstart-translate) başlangıca bakın. 

:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Iletişim hizmetlerinin sohbet mimarisini gösteren diyagram.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sohbetle çalışmaya başlama](../../quickstarts/chat/get-started.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [Sohbet istemci kitaplığı](sdk-features.md) hakkında bilgi edinin
