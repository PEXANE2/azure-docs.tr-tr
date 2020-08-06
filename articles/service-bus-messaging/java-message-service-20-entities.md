---
title: Azure Service Bus mesajlaşma-Java ileti hizmeti varlıkları (Önizleme)
description: Bu makalede Java Message Service API 'SI aracılığıyla erişilebilen Azure Service Bus mesajlaşma varlıklarına genel bakış sunulmaktadır.
ms.topic: article
ms.date: 07/20/2020
ms.openlocfilehash: 1a7fe3d6355146ccf0fce50266a6f3b8da5231b3
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87801800"
---
# <a name="java-message-service-jms-20-entities-preview"></a>Java ileti hizmeti (JMS) 2,0 varlıkları (Önizleme)

Azure Service Bus Premium 'a bağlanan ve [JMS kitaplığı Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) kullanan istemci uygulamalar aşağıdaki varlıkların faydalanabilir.

## <a name="queues"></a>Kuyruklar

JMS 'deki kuyruklar, geleneksel [Service Bus kuyruklarında](service-bus-queues-topics-subscriptions.md#queues)anlam ile karşılaştırılabilir.

Bir kuyruk oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
Queue createQueue(String queueName)
```

## <a name="topics"></a>Konu başlıkları

JMS 'deki konular, geleneksel [Service Bus konularıyla](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions)anlam ile karşılaştırılabilir.

Bir konu oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
Topic createTopic(String topicName)
```

## <a name="temporary-queues"></a>Geçici kuyruklar

Bir istemci uygulaması, uygulamanın ömrü için mevcut olan geçici bir varlık gerektirdiğinde geçici kuyrukları kullanabilir. Bunlar, [istek-yanıt](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) modelinde kullanılır.

Geçici bir sıra oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
TemporaryQueue createTemporaryQueue()
```

## <a name="temporary-topics"></a>Geçici konular

Geçici kuyruklarda olduğu gibi, uygulama ömrü için mevcut olan geçici bir varlık aracılığıyla yayımla/abone ol ' u etkinleştirmek için geçici konular vardır.

Geçici bir konu oluşturmak için, sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
TemporaryTopic createTemporaryTopic()
```

## <a name="java-message-service-jms-subscriptions"></a>Java ileti hizmeti (JMS) abonelikleri

Bu sırada, [aboneliklerde](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) (bir konu üzerinde var olan ve yayımlama/abone olma semantiğinin bulunduğu) anlamsal olarak benzerdir. Java ileti hizmeti özelliği, belirli bir abonelik için **paylaşılan**, **paylaşılmayan**, **dayanıklı** ve **dayanıklı olmayan** özniteliklerin kavramlarını tanıtır.

> [!NOTE]
> Aşağıdaki abonelikler, [Azure Service Bus JMS kitaplığı](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms)kullanarak Azure Service Bus bağlanan Istemci uygulamalarının önizlemesi Için Azure Service Bus Premium katmanında kullanılabilir.
>
> Genel önizleme için, bu abonelikler Azure portal kullanılarak oluşturulamaz.
>

### <a name="shared-durable-subscriptions"></a>Paylaşılan dayanıklı abonelikler

Paylaşılan dayanıklı bir abonelik, bir konu üzerinde yayınlanan tüm iletiler, uygulamanın abonelikten her zaman etkin bir şekilde kullanılıp kullanılmadığına bakılmaksızın bir uygulama tarafından alınıp işleneceğinden kullanılır.

Bu paylaşılan bir abonelik olduğundan, Service Bus içinden alma kimliği doğrulanmış tüm uygulamalar abonelikten alabilir.

Paylaşılan dayanıklı bir abonelik oluşturmak için sınıfında aşağıdaki yöntemleri kullanın `JMSContext` -

```java
JMSConsumer createSharedDurableConsumer(Topic topic, String name)

JMSConsumer createSharedDurableConsumer(Topic topic, String name, String messageSelector)
```

Paylaşılan dayanıklı abonelik, sınıfında yöntemi kullanılarak silinmedikleri takdirde mevcut olmaya devam eder `unsubscribe` `JMSContext` .

```java
void unsubscribe(String name)
```

### <a name="unshared-durable-subscriptions"></a>Paylaşılmayan dayanıklı abonelikler

Paylaşılan dayanıklı bir abonelikte olduğu gibi, bir konu üzerinde yayınlanan tüm iletiler, uygulamanın aboneliğin her zaman etkin bir şekilde tüketilip alınmadığına bakılmaksızın, bir konu üzerinde yayımlanan tüm iletiler bir uygulama tarafından alınıp işlenecektir.

Ancak, bu paylaşılmayan bir abonelik olduğundan, yalnızca aboneliği oluşturan uygulama bundan alabilir.

Paylaşılmayan dayanıklı bir abonelik oluşturmak için, sınıfından aşağıdaki yöntemleri kullanın `JMSContext` - 

```java
JMSConsumer createDurableConsumer(Topic topic, String name)

JMSConsumer createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal)
```

> [!NOTE]
> `noLocal`Özellik şu anda desteklenmiyor ve yoksayıldı.
>

Paylaşılmayan dayanıklı abonelik, sınıfında yöntemi kullanılarak silinmedikleri takdirde mevcut olmaya devam eder `unsubscribe` `JMSContext` .

```java
void unsubscribe(String name)
```

### <a name="shared-non-durable-subscriptions"></a>Paylaşılan, dayanıklı olmayan abonelikler

Birden çok istemci uygulamanın iletileri tek bir abonelikten alması ve işlemesi gerektiğinde, yalnızca bu bilgisayardan etkin bir şekilde tüketerek/alınana kadar, paylaşılan, dayanıklı olmayan bir abonelik kullanılır.

Abonelik dayanıklı olmadığından kalıcı değil. Üzerinde etkin bir tüketici olmadığında bu abonelik tarafından ileti alınmaz.

Paylaşılan, dayanıklı olmayan bir abonelik oluşturmak için, `JmsConsumer` sınıfında aşağıdaki yöntemlerle gösterildiği gibi bir oluşturun `JMSContext` -

```java
JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName)

JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector)
```

Paylaşılan dayanıklı olmayan abonelik, bundan alınan etkin tüketiciler olana kadar mevcut olmaya devam eder.

### <a name="unshared-non-durable-subscriptions"></a>Paylaşılmayan kalıcı olmayan abonelikler

Yönetici olmayan bir abonelik, istemci uygulamanın iletiyi bir abonelikten alması ve işlemesi gerektiğinde, yalnızca onu etkin bir şekilde tüketene kadar kullanılır. Bu abonelikte yalnızca bir tüketici bulunabilir, yani aboneliği oluşturan istemci.

Abonelik dayanıklı olmadığından kalıcı değil. Üzerinde etkin bir tüketici olmadığında bu abonelik tarafından ileti alınmaz.

Paylaşılmayan dayanıklı olmayan bir abonelik oluşturmak için, `JMSConsumer` sınıfından aşağıdaki yöntemlerle gösterildiği gibi bir oluşturun `JMSContext` -

```java
JMSConsumer createConsumer(Destination destination)

JMSConsumer createConsumer(Destination destination, String messageSelector)

JMSConsumer createConsumer(Destination destination, String messageSelector, boolean noLocal)
```

> [!NOTE]
> `noLocal`Özellik şu anda desteklenmiyor ve yoksayıldı.
>

Paylaşılmayan dayanıklı olmayan abonelik, bundan sonra gelen etkin bir tüketici olana kadar mevcut olmaya devam eder.

### <a name="message-selectors"></a>İleti seçicileri

Normal Service Bus abonelikler için **filtreler ve eylemler** gıbı, JMS abonelikleri Için de **ileti seçicileri** vardır.

İleti seçicileri her JMS aboneliğine ayarlanabilir ve ileti üst bilgisi özelliklerinde bir filtre koşulu olarak mevcuttur. Yalnızca ileti seçici ifadesiyle eşleşen üst bilgi özelliklerine sahip iletiler sunulur. Null veya boş bir dize değeri, JMS aboneliği/tüketicisi için bir ileti seçici olmadığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma kullanma hakkında daha fazla bilgi ve örnekler için aşağıdaki gelişmiş konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Azure Service Bus Premium ile Java Ileti hizmeti 2,0 API 'SI kullanma (Önizleme)](how-to-use-java-message-service-20.md)



