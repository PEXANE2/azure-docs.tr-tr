---
title: Olay işlemcisi konağı - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event denetim noktası yönetimini basitleştiren, kiralama ve olayları eçimi paralel okumaya hubs'da, olay işlemcisi konağı açıklanır.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/10/2020
ms.author: shvija
ms.openlocfilehash: 414179d62970315a7575be0411bf1cb152349fdc
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162302"
---
# <a name="event-processor-host"></a>Olay işlemcisi konağı
> [!NOTE]
> Bu makale, Azure Event Hubs SDK 'sının eski sürümü için geçerlidir. Kodunuzu SDK 'nın daha yeni sürümüne nasıl geçirebileceğinizi öğrenmek için, bu geçiş kılavuzlarını inceleyin. 
> - [.NET](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MIGRATIONGUIDE.md)
> - [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/migration-guide.md)
> - [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md)
> - [Java betiği](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md)
>
> Ayrıca bkz. [uygulamanızın birden çok örneğinde bölüm yükünü dengeleme](event-processor-balance-partition-load.md).

Azure Event Hubs, milyonlarca olayı düşük bir maliyet karşılığında kullanılabilir güçlü telemetri alma hizmetidir. Bu makalede, *olay Işlemcisi Konağı* (EPH) kullanılarak alınan olayların nasıl kullanılacağı açıklanır; checkişaret, kiralama ve paralel olay okuyucularının yönetimini kolaylaştıran akıllı bir tüketici Aracısı.  

Event Hubs için ölçeklendirmek için anahtar bölümlenmiş tüketici olur. [Rekabet tüketicilerinin](https://msdn.microsoft.com/library/dn568101.aspx) deseninin aksine, bölümlenmiş tüketici deseninin çekişme sorunlarını ortadan kaldırarak ve uçtan uca paralellik için kolaylaştırarak yüksek ölçeklenmesini sağlar.

## <a name="home-security-scenario"></a>Giriş güvenlik senaryosu

Örnek bir senaryo 100.000 havaalanlarından izleyen bir giriş güvenliği şirketi düşünün. Her dakika hareket algılayıcısı, kapı/penceresi açık algılayıcı, cam sonu algılayıcısı, her giriş sayfasında yüklü vb. gibi çeşitli sensörlerden alınan verileri alır. Şirket, kendi giriş neredeyse gerçek zamanlı etkinliğini izlemek vatandaşlar için bir web sitesi sağlar.

Her algılayıcı verileri olay hub'ına gönderir. Olay hub'ı 16 bölümleri ile yapılandırılır. Alıcı uçta ise bu etkinlikleri, birleştirmek (filtreleme, toplama, vb.) ve ardından kullanıcı dostu bir web sayfasına gsyih bir depolama blobu için toplama dökümünü bir mekanizma gerekir.

## <a name="write-the-consumer-application"></a>Tüketici uygulaması yazma

Dağıtılmış bir ortamda tüketici tasarlarken, senaryo aşağıdaki gereksinimleri ele almalısınız.

1. **Ölçek:** Her tüketiciye birkaç Event Hubs bölümden okuma sahipliği alan birden çok tüketici oluşturun.
2. **Yük Dengeleme:** Tüketicileri dinamik olarak artırın veya azaltın. Örneğin, her giriş için yeni bir algılayıcı türü (örneğin, gizli monoxide algılayıcısı) eklendiğinde, olay sayısını artırır. Bu durumda, ' % s'işleci (insan) tüketici örneği sayısını artırır. Tüketici havuzu oldukları, bölüm sayısını dengeleyebilir sonra yeni eklenen tüketicileriyle yük paylaşma.
3. **Hatalarda sorunsuz bir şekilde sürdürülür:** Bir tüketici (**tüketici a**) başarısız olursa (örneğin, tüketiciyi barındıran sanal makine aniden kilitlenirse), diğer tüketiciler **tüketici a** 'nın sahip olduğu bölümleri çekebilmelidir ve devam edebilir. Ayrıca, *denetim noktası* ya da *konum*olarak adlandırılan devamlılık noktası, **tüketicisinin** başarısız olduğu veya bundan biraz önce olması gereken kesin bir noktada olmalıdır.
4. **Olayları tüketme:** Önceki üç işaret, tüketicinin yönetimiyle ilgilenirken, olayları tüketmek ve bununla ilgili bir şey yapmak için kod olmalıdır; Örneğin, bunu toplayın ve BLOB depolamaya yükleyin.

Bunun için kendi çözümünüzü oluşturmak yerine, Event Hubs bu işlevselliği [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi ve [eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı aracılığıyla sağlar.

## <a name="ieventprocessor-interface"></a>Ieventprocessor arabirimini

İlk olarak, uygulamaları kullanan [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimini uygular ve bu dört yöntem Içerir: [openAsync, CloseAsync, Processerrorasync ve ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Bu arabirim, Event Hubs gönderdiği olayları kullanma gerçek kodu içerir. Aşağıdaki kod, basit bir uygulama gösterilmektedir:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
       Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
       return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
       Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
       return Task.CompletedTask;
     }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
       Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
       return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
       foreach (var eventData in messages)
       {
          var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
             Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
       }
       return context.CheckpointAsync();
    }
}
```

Sonra, bir [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği örneği oluşturun. Yük yüküne bağlı olarak, oluşturucuda [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği oluşturulurken aşağıdaki parametreler kullanılır:

- **ana bilgisayar adı:** her bir tüketici örneğinin adı. Her **Eventprocessorhost** örneğinin bir tüketici grubu içindeki bu değişken için benzersiz bir değeri olması gerekir, bu nedenle bu değeri sabit olarak kodlayın.
- **Eventhubpath:** Olay Hub 'ının adı.
- **Consumergroupname:** Event Hubs varsayılan tüketici grubunun adı olarak **$Default** kullanır, ancak belirli bir işlem yönüdür için bir tüketici grubu oluşturmak iyi bir uygulamadır.
- **Eventhubconnectionstring:** Azure portal alınabilecek Olay Hub 'ına yönelik bağlantı dizesi. Bu bağlantı dizesinin Olay Hub 'ında **dinleme** izinlerine sahip olması gerekir.
- **StorageConnectionString:** İç kaynak yönetimi için kullanılan depolama hesabı.

Son olarak, tüketiciler [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneğini Event Hubs hizmetiyle kaydeder. Bir olay işlemcisi sınıfı EventProcessorHost örneğini kaydetme, olay işleme başlar. Kayıt, Event Hubs hizmetine tüketici uygulamasının bazı bölümlerinden olayları tüketmesini ve kullanım olaylarını her geldiğinde [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulama kodunu çağırmayı beklemesini ister. 


### <a name="example"></a>Örnek

Örneğin, 5 sanal makineleri (VM'ler) Imagine olayları ve her VM'de bir basit bir konsol uygulaması kullanan için ayrılmış, hangi gerçek tüketim çalışır. Ardından, her bir konsol uygulaması bir [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği oluşturur ve Event Hubs hizmetine kaydeder.

Bu örnek senaryoda, 5 **Eventprocessorhost** örneklerine 16 Bölüm ayrıldığınızı varsayalım. Bazı **Eventprocessorhost** örnekleri diğerlerinden daha fazla bölümden oluşabilir. Bir **Eventprocessorhost** örneğinin sahip olduğu her bölüm için `SimpleEventProcessor` sınıfının bir örneğini oluşturur. Bu nedenle, her bölüme atanan bir genel `SimpleEventProcessor` 16 örnek vardır.

Aşağıdaki listede, bu örnek özetlenmektedir:

- 16 event Hubs bölümler.
- 5 VM, her VM'deki 1 tüketici uygulama (örneğin, Consumer.exe).
- 5 EPH örneklerin kayıtlı, her VM Consumer.exe tarafından 1.
- 5 EPH örneği tarafından oluşturulan 16 `SimpleEventProcessor` nesne.
- 1 `SimpleEventProcessor` nesnesi, 1 EPH örneği 4 bölümden kaynaklanabilir.

## <a name="partition-ownership-tracking"></a>İzleme bölümü sahipliği

Bir bölüm EPH örneği (veya bir tüketici) sahipliğini, izleme için belirtilen Azure depolama hesabıyla izlenir. Basit bir tablo olarak izleme gibi görselleştirebilirsiniz. Sağlanan depolama hesabı altındaki BLOB'ları inceleyerek, gerçek uygulama görebilirsiniz:

| **Tüketici grubu adı** | **Bölüm KIMLIĞI** | **Ana bilgisayar adı (sahip)** | **Kira (veya sahiplik) Alım Süresi** | **Bölümdeki konum (kontrol noktası)** |
| --- | --- | --- | --- | --- |
| $Default | 0 | Tüketici\_VM3 | 2018-04-15T01:23:45 | 156 |
| $Default | 1 | Tüketici\_VM4 | 2018-04-15T01:22:13 | 734 |
| $Default | 2 | Tüketici\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| $Default | 15 | Tüketici\_VM3 | 2018-04-15T01:22:56 | 976 |

Burada, her konağın belirli bir süre için (kiralama süresi) bir bölüm sahipliğini alır. Ardından konak başarısız olursa (VM kapatıldığında) kira süresi dolar. Diğer ana bölüm sahipliğini almayı denemek ve konaklardan birine başarılı olur. Bu işlem yeni bir sahip olan bölüm kirayı sıfırlar. Bu şekilde, aynı anda yalnızca tek bir okuyucu herhangi belirli bir bölümünün bir tüketici grubundaki okuma yapabilirsiniz.

## <a name="receive-messages"></a>İleti alma

[Processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) öğesine yapılan her çağrı bir olay koleksiyonu sunar. Bu olayları işlemek için sizin sorumluluğunuzdur. İşlemci konağının her iletiyi en az bir kez işlediğinden emin olmak istiyorsanız, kendi yeniden deneme kodu yazmanız gerekir. Ancak kired iletiler hakkında dikkatli olun.

Görece şeyler yapmanız önerilir. diğer bir deyişle, mümkün olduğunca az miktarda işleme olarak yapın. Bunun yerine, tüketici gruplarını kullanın. Depolama alanına yazmanız ve bazı yönlendirme yapmanız gerekiyorsa, iki Tüketici grubu kullanmak ve ayrı olarak çalışan iki [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulaması olması daha iyidir.

İşleme sırasında belirli bir noktada ne artık okuma tamamlandı ve izlemek isteyebilirsiniz. Akış başlangıcına döndürmeyin için okuma, yeniden başlatırsanız izleme tutmak önemlidir. [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) , bu izlemeyi *kontrol noktaları*kullanarak basitleştirir. Bir konum veya uzaklık, belirli bir tüketici grubundaki belirli bir bölüm için bir denetim noktası, işlenen olduğunuz bu noktaya memnun iletileri. **Eventprocessorhost** içindeki bir kontrol noktasının Işaretlenmesi, [Partitioncontext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) nesnesinde [checkpointasync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yöntemi çağırarak gerçekleştirilir. Bu işlem [Processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) yöntemi içinde yapılır, ancak [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync)içinde de yapılabilir.

## <a name="checkpointing"></a>Denetim noktası oluşturma

[Checkpointasync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yönteminin iki aşırı yüklemesi vardır: ilki parametre olmadan, [Processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)tarafından döndürülen koleksiyonda en yüksek olay kaydırmasına denetim noktası ekler. Bu uzaklık, bir "yüksek su" işaretidir; Bu, çağırdığınızda, tüm son olayları işleyip varsayar. Bu şekilde bu yöntemi kullanırsanız, bir olay işleme kodunuzu döndürdü sonra çağrısından beklenir unutmayın. İkinci aşırı yükleme, denetim noktası için bir [eventdata](/dotnet/api/microsoft.azure.eventhubs.eventdata) örneği belirtmenizi sağlar. Bu yöntem, farklı türde bir filigran kontrol noktasına kullanmanıza olanak sağlar. Bu eşik ile bir "Düşük" Filigran uygulayabilir: olduğunuz belirli en düşük sıralı olay işlendi. Bu aşırı yükleme uzaklık yönetim esneklik olanağı sağlanır.

Denetim noktası gerçekleştirildiğinde bölüme özgü bilgileri olan bir JSON dosyası (özellikle de, fark), oluşturucuda [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)'a sağlanan depolama hesabına yazılır. Bu dosya sürekli olarak güncelleştirilir. Denetim noktası oluşturma bağlamda göz önünde bulundurmanız önemlidir - kontrol noktası için uygun her ileti. Denetim noktası oluşturma için büyük olasılıkla kullanılan depolama hesabı, bu yükle başa değil, ancak denetim noktası her olay bir Service Bus kuyruğuna bir olay hub'den daha iyi bir seçenek olabilir bir kuyruğa alınan Mesajlaşma modeli simulatorda daha da önemlisi. Event Hubs arkasında büyük ölçekte "en az bir kez" teslim alma olur. Aşağı Akış sistemlerine ıdempotent yaparak bu hatalardan kurtarma kolayca veya sonucunda aynı olayları birden çok kez alınan yeniden başlatır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) iş parçacığı güvenlidir ve [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor)örneğine göre zaman uyumlu bir şekilde davranır. Bir bölüme yönelik olaylar geldiğinde, bu bölüm için **ıeventprocessor** örneğinde [processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) çağrılır ve bölüm Için **processeventsasync** 'e yönelik daha fazla çağrı engellenir. İleti göndericisi, arka planda diğer iş parçacıklarında çalışmaya devam ettiğinden, izleyen iletiler ve arka planda gerçekleştirilen **Processeventsasync** kuyruğuna yapılan çağrılar. Bu iş parçacığı güvenliği, iş parçacığı güvenli koleksiyonları gereksinimini ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="shut-down-gracefully"></a>Düzgün biçimde kapatılamadı

Son olarak, [eventprocessorhost. UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) , tüm bölüm okuyucularının temiz bir şekilde kapatılmasını mümkün ve [eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)örneği kapatılırken her zaman çağrılmalıdır. Bunun yapılmaması, kira süre sonu ve dönem çakışmaları nedeniyle diğer **Eventprocessorhost** örnekleri başlatılırken gecikmelere neden olabilir. Dönem yönetimi, makalenin [Dönem](#epoch) bölümünde ayrıntılı olarak ele alınmıştır. 

## <a name="lease-management"></a>Kiralama Yönetimi
Bir olay işlemcisi sınıfı EventProcessorHost örneğini kaydetme, olay işleme başlar. Ana bilgisayar örneği, büyük olasılıkla tüm konak örneklerinde bölümlerin eşit dağıtımı üzerinde uygun sonuç verir şekilde diğer konak örneklerden bazıları kapmasını bazı bölümlerini bir olay hub'ı üzerinde kiraları alır. Kiralanmış her bölüm için ana bilgisayar örneği sağlanan olay işlemcisi sınıfının bir örneğini oluşturur, ardından bu bölümün dışında olaylarını alır ve bunları olay işlemcisi örneğine geçirir. Daha fazla örnek eklenir ve daha fazla kira yakaladı EventProcessorHost sonunda tüm tüketicileri arasında yük dengeleyen.

Daha önce açıklandığı gibi, izleme tablosu [Eventprocessorhost. UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync)öğesinin otomatik ölçeklendirme yapısını büyük ölçüde basitleştirir. Bir **Eventprocessorhost** örneği başlatıldıktan sonra, mümkün olduğunca fazla kira edinir ve olayları okumaya başlar. Süresi dolmak üzere olan kiralamalar, **Eventprocessorhost** , bir ayırma yerleştirerek bunları yenilemeye çalışır. Kira yenilemeye uygunsa, işlemci okumaya devam eder, ancak yoksa, okuyucu kapanır ve [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) çağırılır. **CloseAsync** , bu bölüm için son temizleme işlemini gerçekleştirmek için uygun bir zamandır.

**Eventprocessorhost** , bir [partitionmanageroptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) özelliği içerir. Bu özellik, kiralama yönetimine üzerinde denetim sağlar. [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulamanızı kaydetmeden önce bu seçenekleri ayarlayın.

## <a name="control-event-processor-host-options"></a>Olay işlemcisi konağı seçeneklerini denetleme

Ayrıca, [Registereventprocessorasync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) 'in bir aşırı yüklemesi parametre olarak bir [eventprocessoroptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) nesnesi alır. [Eventprocessorhost. UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) öğesinin davranışını denetlemek için bu parametreyi kullanın. [Eventprocessoroptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) dört özelliği ve bir olayı tanımlar:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): [Processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)çağrısında almak istediğiniz koleksiyonun en büyük boyutu. Bu boyut en düşük, yalnızca en büyük boyutu değil. Alınacak daha az ileti varsa, **Processeventsasync** , kullanılabilir olduğu kadar çalışır.
- [Prefetchcount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): istemcinin kaç ileti alacağını öğrenmek için temel AMQP kanalı tarafından kullanılan bir değer. Bu değer [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize)değerinden büyük veya buna eşit olmalıdır.
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): Bu parametre **true**ise, bir bölümdeki olayları almak için temeldeki çağrı zaman aşımına uğrarsa [processeventsasync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) çağırılır. Bu yöntem, bölüm üzerinde işlem yapılmayan işlemler sırasında zaman tabanlı eylemler gerçekleştirmek için yararlıdır.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): bir okuyucu bölüm okumaya başladığında ilk sapmayı sağlamak için çağrılan bir işlev işaretçisi veya lambda ifadesinin ayarlanmasını sağlar. Bu sapmayı belirtmeden, bir uzaklığa sahip bir JSON dosyası [Eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) oluşturucusuna sağlanan depolama hesabına zaten kaydedilmediği için okuyucu en eski olayda başlar. Bu yöntem, okuyucu başlangıç davranışını değiştirmek istediğinizde kullanışlıdır. Bu yöntem çağrıldığında, nesne parametresi okuyucu başlatıldığına bölüm Kimliğini içerir.
- [Exceptionreceivedeventargs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): [eventprocessorhost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)içinde oluşan temeldeki tüm özel durumlarla ilgili bildirim almanızı sağlar. Beklediğiniz gibi şeyler çalışmıyorsanız, bu olay bakmaya başlamak için iyi bir yerdir.

## <a name="epoch"></a>Süresinin

Alma süresinin nasıl çalıştığı aşağıda verilmiştir:

### <a name="with-epoch"></a>Dönem ile
Dönem, hizmetin kullandığı, Bölüm/kira sahipliğini zorlamak için benzersiz bir tanımlayıcıdır (dönem değeri). [Createdönemler Chahize](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createepochreceiver?view=azure-dotnet) yöntemini kullanarak dönem tabanlı bir alıcı oluşturun. Bu yöntem, dönem tabanlı bir alıcı oluşturur. Alıcı, belirtilen tüketici grubundan belirli bir olay hub 'ı bölümü için oluşturulur.

Dönem özelliği, kullanıcılara, aşağıdaki kurallara göre herhangi bir zamanda bir tüketici grubunda yalnızca bir alıcı olmasını sağlar:

- Bir tüketici grubunda mevcut alıcı yoksa, Kullanıcı herhangi bir dönem değeri olan bir alıcı oluşturabilir.
- Bir dönem değeri olan bir alıcı varsa ve bir dönem değeri olan E2 (E1 < = E2) ile yeni bir alıcı oluşturulduysa, E1 ile alıcının bağlantısı otomatik olarak kesilir, E2 içeren alıcı başarıyla oluşturulur.
- Bir dönem değeri E1 olan bir alıcı varsa ve bir dönem değeri > E2 ile yeni bir alıcı oluşturulduysa, bu durumda E2 oluşturma işlemi şu hatayla başarısız olur: "dönemi olan bir alıcı zaten var.

### <a name="no-epoch"></a>Dönem yok
[Createreceiver](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createreceiver?view=azure-dotnet) yöntemini kullanarak dönem tabanlı olmayan bir alıcı oluşturursunuz. 

Akış işlemede, kullanıcıların tek bir tüketici grubunda birden çok alıcı oluşturmak istediğiniz bazı senaryolar vardır. Bu tür senaryoları desteklemek için, dönem olmadan bir alıcı oluşturabilir ve bu durumda tüketici grubunda 5 ' e kadar eşzamanlı alıcıya izin veririz.

### <a name="mixed-mode"></a>Karma mod
Dönemi olan bir alıcı oluşturduğunuz ve sonra aynı tüketici grubunda dönem olmayan veya bunun tersini alan uygulama kullanımını önermiyoruz. Ancak, bu davranış oluştuğunda hizmet aşağıdaki kuralları kullanarak bunu işler:

- Zaten dönem E1 ile oluşturulmuş bir alıcı varsa ve etkin bir şekilde olay alıyorsa ve yeni bir alıcı, dönem olmadan oluşturulursa, yeni alıcı oluşturulması başarısız olur. Dönem alıcıları, her zaman sistemde önceliğe sahip olur.
- Zaten bir dönem E1 ile oluşturulmuş ve bağlantısı kesilen bir alıcı varsa ve yeni bir MessagingFactory üzerinde dönem olmadan yeni bir alıcı oluşturulduysa yeni alıcı oluşturma işlemi başarılı olur. Burada sistemimizin, yaklaşık 10 dakika sonra "alıcı bağlantısının kesilmesi" algılayacağı bir desteklenmediği uyarısıyla vardır.
- Dönem olmadan oluşturulmuş bir veya daha fazla alıcı varsa ve dönem E1 ile yeni bir alıcı oluşturulduysa, tüm eski alıcıların bağlantısı kesilir.


> [!NOTE]
> Dönemleri kullanan uygulamalar için farklı tüketici grupları kullanmanızı ve hataları önlemek için dönemler kullanmayan dönemleri kullanmanızı öneririz. 


## <a name="next-steps"></a>Sonraki adımlar

Artık Event Processor Host ile ilgili bilgi sahibi olduğunuza göre Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [GitHub 'daki Event Hubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples)
