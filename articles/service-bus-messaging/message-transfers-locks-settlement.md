---
title: Azure Servis Veri Servisi ileti aktarımları, kilitler ve kapatma
description: Bu makalede, Azure Hizmet Veri Servisi ileti aktarımlarına, kilitlere ve kapatma işlemlerine genel bir bakış sunulmaktadır.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2019
ms.author: aschhab
ms.openlocfilehash: a2c353d612280981a83b32463d34efdc70878495
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261001"
---
# <a name="message-transfers-locks-and-settlement"></a>İleti aktarımları, kilitler ve kapatma

Service Bus gibi bir ileti aracısının merkezi özelliği, iletileri bir kuyruğa veya konuya kabul etmek ve daha sonra almak için kullanılabilir durumda tutmaktır. *Gönder,* iletinin ileti aracısına aktarılması için yaygın olarak kullanılan terimdir. *Alma,* iletinin bir istemciye aktarılması nda yaygın olarak kullanılan terimdir.

İstemci bir ileti gönderdiğinde, genellikle iletinin aracı tarafından düzgün bir şekilde aktarılıp aktarılmadığını veya bir tür hata nın oluşup oluşmadığını bilmek ister. Bu olumlu veya olumsuz bildirim müşteri ve broker anlayışı ile ilgili iletinin transfer durumu hakkında yerleşir ve böylece *uzlaşma*olarak adlandırılır.

Aynı şekilde, komisyoncu bir müşteriye bir ileti aktardığında, komisyoncu ve müşteri mesajın başarılı bir şekilde işlenip işlenmediği ve bu nedenle kaldırılıp kaldırılamayacağı veya ileti nin tesliminin veya işlenmesinin başarısız olup olmadığı ve böylece iletinin yeniden teslim edilmesi gerekebilir.

## <a name="settling-send-operations"></a>Gönderme işlemlerini çözme

Desteklenen Hizmet Veri Gönderi istemcilerinden herhangi birini kullanarak, Hizmetleri Veri Servisi'ne göndermek işlemleri her zaman açıkça kapatılır, bu da API işleminin Servis Veri Servisi'nden gelen kabul sonucunu beklediği ve daha sonra gönderme işlemini tamamladığını anlamına gelir.

İleti Servis Veri Kurumu tarafından reddedilirse, reddi içinde "izleme kimliği" bulunan bir hata göstergesi ve metin içerir. Ret işlemi başarı beklentisi ile yeniden denenebilir olup olmadığı hakkında bilgi içerir. İstemcide, bu bilgiler bir özel durum haline getirilir ve gönderme işlemini arayana yükseltilir. İleti kabul edildiyse, işlem sessizce tamamlanır.

.NET Standart istemcisi ve Java istemcisi için özel protokol olan ve [.NET Framework istemcisi için bir seçenek olan](service-bus-amqp-dotnet.md)AMQP protokolünü kullanırken, ileti aktarımları ve yerleşimleri boru hattı ve tamamen eşzamanlı dır ve eşzamanlı programlama modeli API türevlerini kullanmanız önerilir.

Gönderen, sbmp protokolünde veya HTTP 1.1'de olduğu gibi, her iletinin kabul olmasını beklemek zorunda kalmadan, telüzerinde hızlı bir şekilde birkaç ileti koyabilir. Bu eşzamanlı gönderme işlemleri, ilgili iletiler kabul edilip depolandığında, bölümlenmiş varlıklarda veya farklı varlıklara işlem gönderdiğinde tamamlanır. Tamamlamalar, özgün gönderme siparişinin dışında da oluşabilir.

Gönderme işlemlerinin sonucunu işleme stratejisi, uygulamanız için anında ve önemli bir performans etkisi ne olabilir. Bu bölümdeki örnekler C# ile yazılır ve Java Futures için eşdeğer olarak uygulanır.

Uygulama, burada düz bir döngü yle resimlenen ileti patlamaları üretirse ve bir sonraki ileti, senkron veya eşzamanlı API şekillerini göndermeden önce her gönderme işleminin tamamlanmasını beklerse, 10 ileti yalnızca 10'dan sonra tamamlar yerleşim için sıralı tam gidiş-dönüş geziler.

Tesis içi bir siteden Servis Veri Servisi'ne 70 milisaniyelik tcp gidiş dönüş gecikme mesafesi ve Servis Veri Servisi'nin her iletiyi kabul etmesi ve depolaması için sadece 10 ms veren aşağıdaki döngü, yük aktarım süresini veya potansiyelini saymamak için en az 8 saniye kadar sürer. rota tıkanıklığı etkileri:

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Uygulama 10 asynchronous gönderme işlemlerini hemen art arda başlatır ve bunların tamamlanmasını ayrı ayrı beklerse, bu 10 gönderi işlemi için gidiş-dönüş süresi çakışıyor. 10 ileti, tcp çerçevelerini paylaşarak hemen art arda aktarılır ve genel aktarım süresi büyük ölçüde iletilerin aracıya aktarılması için gereken ağla ilgili süreye bağlıdır.

Önceki döngüyle aynı varsayımları yapmak, aşağıdaki döngü için toplam çakışan yürütme süresi bir saniyenin altında kalabilir:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks);
```

Tüm eşzamanlı programlama modellerinin bekleyen işlemleri tutan bellek tabanlı, gizli iş sırasının bir tür ini kullandığını unutmayın. [SendAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.sendasync#Microsoft_Azure_ServiceBus_QueueClient_SendAsync_Microsoft_Azure_ServiceBus_Message_) (C#) veya **Send** (Java) döndüklerinde, gönderme görevi bu iş kuyruğunda sıraya alınır, ancak protokol hareketi yalnızca görevin çalışma sırası geldiğinde başlar. İleti patlamalarını itme eğiliminde olan ve güvenilirliğin bir sorun olduğu kod için, çok fazla iletinin aynı anda "uçuşa" konulmaması gerektiğine dikkat edilmelidir, çünkü gönderilen tüm iletiler aslında kabloya asılana kadar bellek alır.

Semaforlar, C#'da aşağıdaki kod snippetinde gösterildiği gibi, gerektiğinde bu tür uygulama düzeyinde azaltmayı sağlayan eşitleme nesneleridir. Semaforbu kullanımı en fazla 10 mesajın aynı anda uçuşa izin verir. Mevcut 10 semafor kilidinden biri göndermeden önce alınır ve gönderme tamamlandıkça serbest bırakılır. Döngüden geçen 11.

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks);
```

Uygulamalar, operasyonun sonucunu almadan **asla** "ateş ve unut" şeklinde bir eşzamanlı gönderme işlemi başlatmamalıdır. Bunu yapmak, iç ve görünmez görev sırasını bellek yorgunluğuna kadar yükleyebilir ve uygulamanın gönder hatalarını algılamasını engelleyebilir:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Düşük seviyeli BIR AMQP istemcisi ile Servis Otobüsü de "önceden yerleşmiş" aktarımları kabul eder. Önceden kararlaştırılan bir aktarım, her iki şekilde de sonucun istemciye geri bildirilmediğini ve iletinin gönderildiğinde kapatıldığı kabul edildiği bir yangın ve unut işlemidir. İstemciye geri bildirim eksikliği, tanılama için kullanılabilir bir veri olmadığı anlamına da gelir, bu da bu modun Azure desteği aracılığıyla yardım almaya uygun olmadığı anlamına gelir.

## <a name="settling-receive-operations"></a>Alma işlemlerini niçin halletmek

Alma işlemleri için, Service Bus API istemcileri iki farklı açık modu etkinleştirir: *Al ve Sil* ve *Peek-Lock.*

### <a name="receiveanddelete"></a>ReceiveAndDelete

[Al ve Sil](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, aracıcıya gönderildiğinde alıcı istemciye gönderdiği tüm iletileri yerleşik olarak dikkate almasını söyler. Bu, iletinin komisyoncu nun kabloya koyar koymaz tükettiği anlamına gelir. İleti aktarımı başarısız olursa, ileti kaybolur.

Bu modun iyi tarafı, alıcının ileti üzerinde daha fazla işlem yapmanıza gerek olmaması ve anlaşmanın sonucunu bekleyerek de yavaşlatılmamasıdır. Tek tek iletilerde yer alan veriler düşük değere sahipse ve/veya çok kısa bir süre için anlamlıysa, bu mod makul bir seçimdir.

### <a name="peeklock"></a>PeekLock

[Peek-Lock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, alıcı istemcinin alınan iletileri açıkça kapatmak istediğini komisyoncuya bildirir. İleti, diğer rakip alıcıların göremaması için hizmette özel bir kilit altında tutulurken, alıcının işlemesi için kullanılabilir hale getirilir. Kilidin süresi başlangıçta sıra veya abonelik düzeyinde tanımlanır ve Kilidi niçin sahibi olan istemci tarafından [RenewLock](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_) işlemi yle genişletilebilir.

Bir ileti kilitlendiğinde, aynı kuyruktan veya abonelikten alan diğer istemciler kilitleri alabilir ve etkin kilit altında olmayan bir sonraki kullanılabilir iletileri alabilir. İletideki kilit açıkça serbest bırakıldığında veya kilidin süresi dolduğunda, ileti yeniden teslim için alma siparişinin önünde veya önünde açılır.

İleti alıcılar tarafından tekrar tekrar yayımlandığında veya kilidin belirli sayıda[(maxDeliveryCount)](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount)alınmasına izin verdiklerinde, ileti otomatik olarak kuyruktan veya abonelikten kaldırılır ve ilişkili ölü harf kuyruğuna yerleştirilir.

Alıcı istemci, API düzeyinde [Tam'ı](/dotnet/api/microsoft.servicebus.messaging.queueclient.complete#Microsoft_ServiceBus_Messaging_QueueClient_Complete_System_Guid_) aradığında olumlu bir bildirimle alınan bir iletinin çözüme kavuşması nı başlatır. Bu, aracıcıya iletinin başarıyla işlendiğini ve iletinin kuyruktan veya abonelikten kaldırıldığını gösterir. Komisyoncu, alıcının yerleşim amacına, anlaşmanın yapılıp yapılmayacağını belirten bir yanıtla yanıt verir.

Alıcı istemci bir iletiyi işlemeyi başaramazsa ancak iletinin yeniden iletilmesini istediğinde, [bırak'ı](/dotnet/api/microsoft.servicebus.messaging.queueclient.abandon) arayarak iletinin serbest bırakılmasını ve kilidinin açılmasını açıkça isteyebilir veya hiçbir şey yapamaz ve kilidin açılmasına izin verebilir.

Alıcı istemci bir iletiyi işlemek için başarısız olursa ve iletiyi yeniden teslim etme ve işlemi yeniden denemenin yardımcı olmayacağını bilirse, [deadLetter'ı](/dotnet/api/microsoft.servicebus.messaging.queueclient.deadletter)arayarak ölü harf kuyruğuna taşıyan iletiyi reddedebilir, bu da ölü harf kuyruğundan iletiyle birlikte alınabilecek bir neden kodu da dahil olmak üzere özel bir özellik ayarlamasına olanak tanır.

Ayrı bir makalede tartışılan özel bir uzlaşma durumu tecildir.

**Tam** veya **Deadletter** işlemleri ve **Yenileme Kilidi** işlemleri ağ sorunları nedeniyle, tutulan kilidin süresi dolmuşsa veya yerleşimi engelleyen diğer hizmet tarafı koşulları varsa başarısız olabilir. İkinci durumlardan birinde, hizmet API istemcilerinde bir istisna olarak ortaya çıkan olumsuz bir bildirim gönderir. Bunun nedeni bozuk bir ağ bağlantısıysa, Servis Veri Yolu farklı bir bağlantıdaki varolan AMQP bağlantılarının kurtarılmasını desteklemediğinden kilit bırakılır.

Genellikle ileti işlemeişleminin sonunda ve bazı durumlarda işlem dakikalarının ardından oluşan **Complete** başarısız olursa, alıcı uygulama işin durumunu koruyup korumadığına ve ikinci kez teslim edildiğinde aynı iletiyi yok saymaya mı yoksa ileti yeniden teslim edilirken iş sonucunu ve yeniden denemelerini mi atacağına karar verebilir.

Yinelenen ileti teslimlerini tanımlamak için tipik bir mekanizma, gönderen tarafından büyük olasılıkla kaynak işleminden bir tanımlayıcıyla hizalanmış benzersiz bir değere ayarlanabilen ve ayarlanması gereken ileti kimliğini denetlemektir. Bir iş zamanlayıcısı büyük olasılıkla ileti kimliğini, verilen işçiyle birlikte bir işçiye atamaya çalıştığı işin tanımlayıcısına ayarlar ve bu iş zaten yapılmışsa, işçi iş atamasının ikinci oluşumunu göz ardı eder.

> [!IMPORTANT]
> PeekLock'un iletide edindığı kilidin geçici olduğunu ve aşağıdaki koşullarda kaybolabileceğini unutmayın
>   * Hizmet Güncellemesi
>   * İşletim sistemi güncelleştirmesi
>   * Kilidi tutarken varlık (Sıra, Konu, Abonelik) üzerindeki özellikleri değiştirme.
>
> Kilit kaybolduğunda, Azure Hizmet Veri Tos'u istemci uygulama kodunda su yüzüne çıkacak bir LockLostException oluşturur. Bu durumda, istemcinin varsayılan yeniden deneme mantığı otomatik olarak devreye girip işlemi yeniden denemelidir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
