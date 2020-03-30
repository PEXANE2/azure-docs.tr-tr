---
title: Olay İşlemcisi Ana Bilgisayar - Azure Etkinlik Hub'larını kullanarak etkinlik alma | Microsoft Dokümanlar
description: Bu makalede, azure olay hub'larında etkinlik işlemcisi ana bilgisayaraçıklanır ve bu da denetim noktası yönetimini, kiralamayı ve olayları paralel olarak okumayı kolaylaştırır.
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
ms.openlocfilehash: 485f51e45e342ca28d54d609fd975bef5b204f7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372223"
---
# <a name="event-processor-host"></a>Olay işlemcisi konağı
> [!NOTE]
> Bu makale, Azure Olay Hub'ları SDK'nın eski sürümü için geçerlidir. Kodunuzu SDK'nın yeni sürümüne nasıl geçirteceklerinizi öğrenmek için bu geçiş kılavuzlarına bakın. 
> - [.NET](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md)
> - [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/migration-guide.md)
> - [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md)
> - [Java Script](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md)
>
> Ayrıca, [bkz. Uygulamanızın birden çok örneği arasında Bakiye bölümü yükü.](event-processor-balance-partition-load.md)

Azure Etkinlik Hub'ları, milyonlarca olayı düşük maliyetle aktarmak için kullanılabilen güçlü bir telemetri hizmetidir. Bu makalede, *Olay İşlemcisi Ana Bilgisayar* (EPH) kullanarak yutulan olayların nasıl tüketilen açıklanmıştır; kontrol noktaları, kiralama ve paralel etkinlik okuyucularının yönetimini kolaylaştıran akıllı bir tüketici aracısı.  

Olay Hub'ları için ölçeklendirmenin anahtarı, bölümlenmiş tüketicilerin fikridir. [Rakip tüketici](https://msdn.microsoft.com/library/dn568101.aspx) deseninin aksine, bölümlenmiş tüketici deseni, çekişme darboğazını ortadan kaldırarak ve uçlardan uca paralelliği kolaylaştırarak yüksek ölçek sağlar.

## <a name="home-security-scenario"></a>Ev güvenlik senaryosu

Örnek bir senaryo olarak, 100.000 evi izleyen bir ev güvenlik şirketi düşünün. Her dakika, bir hareket dedektörü, kapı / pencere açık sensör, cam kırılma dedektörü, vb, her evde yüklü gibi çeşitli sensörlerveri alır. Şirket sakinleri için neredeyse gerçek zamanlı olarak evlerinin etkinliğini izlemek için bir web sitesi sağlar.

Her sensör verileri bir olay merkezine iter. Olay hub'ı 16 bölümle yapılandırılır. Tüketen uçta, bu olayları okuyabilen, bunları birleştirebilen (filtre, toplam, vb.) ve toplamı kullanıcı dostu bir web sayfasına yansıtılan bir depolama blob'una atabilmeniz için bir mekanizmaya ihtiyacınız vardır.

## <a name="write-the-consumer-application"></a>Tüketici başvurusunu yazın

Tüketiciyi dağıtılmış bir ortamda tasarlarken, senaryo aşağıdaki gereksinimleri karşılamalıdır:

1. **Ölçek:** Her tüketicinin birkaç Etkinlik Hub'ı bölümünden okuma sahipliğini almasıyla birden çok tüketici oluşturun.
2. **Yük dengesi:** Tüketicileri dinamik olarak artırın veya azaltın. Örneğin, her eve yeni bir sensör türü (örneğin, karbon monoksit dedektörü) eklendiğinde, olay sayısı artar. Bu durumda, operatör (bir insan) tüketici örneklerinin sayısını artırır. Daha sonra, tüketicilerin havuzu, yeni eklenen tüketicilerle yükü paylaşmak için sahip oldukları bölüm sayısını yeniden dengeleyebilir.
3. **Hatalarda sorunsuz özgeçmiş:** Bir tüketici **(tüketici A)** başarısız olursa (örneğin, tüketiciyi barındıran sanal makine aniden çöker), diğer tüketicilerin **a** tüketiciye ait bölümleri alıp devam edebilmeleri gerekir. Ayrıca, bir *kontrol noktası* veya *ofset*olarak adlandırılan devam noktası, **tüketici A** başarısız olduğu tam noktada olmalıdır, ya da biraz bundan önce.
4. **Olayları tüketin:** Önceki üç nokta tüketici yönetimi ile ilgili iken, olayları tüketmek ve onunla yararlı bir şey yapmak için kod olmalıdır; örneğin, toplayıp blob depolamaya yükleyin.

Bunun için kendi çözümünüzü oluşturmak yerine, Olay Hub'ları bu işlevselliği [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi ve [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı aracılığıyla sağlar.

## <a name="ieventprocessor-interface"></a>IEventProcessor arabirimi

İlk olarak, tüketen uygulamalar dört yöntem vardır [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arayüzü, uygulamak: [OpenAsync, CloseAsync, ProcessErrorAsync ve ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Bu arabirim, Olay Hub'larının gönderdiği olayları tüketmek için gerçek kodu içerir. Aşağıdaki kod basit bir uygulama gösterir:

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

Ardından, bir [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneğini anında anons edin. Aşırı yüke bağlı olarak, oluşturucuda [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneğini oluştururken aşağıdaki parametreler kullanılır:

- **hostName:** her tüketici örneğinin adı. **EventProcessorHost** her örneği bir tüketici grubu içinde bu değişken için benzersiz bir değere sahip olmalıdır, bu nedenle bu değeri sabit kod yok.
- **olayHubPath:** Olay merkezinin adı.
- **consumerGroupName:** Olay Hub'ları varsayılan tüketici grubunun adı olarak **$Default** kullanır, ancak belirli işleme yönünüz için bir tüketici grubu oluşturmak iyi bir uygulamadır.
- **olayHubConnectionString:** Azure portalından alınabilen olay hub'ına bağlantı dizesi. Bu bağlantı dizesi olay hub'ında **Dinleme** izinlerine sahip olmalıdır.
- **depolamaConnectionString:** İç kaynak yönetimi için kullanılan depolama hesabı.

Son olarak, tüketiciler [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneğini Event Hub'ları hizmetine kaydeder. EventProcessorHost bir örneği ile bir olay işlemci sınıfı kayıt olay işleme başlar. Kaydolmak, Olay Hub'ları hizmetine, tüketici uygulamasının bazı bölümlerindeki olayları tüketmesini beklemesini ve olayları tüketime ittiği zaman [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulama kodunu çağırmasını sağlar. 


### <a name="example"></a>Örnek

Örnek olarak, olayları tüketmeye adanmış 5 sanal makine (VM) ve her VM'de gerçek tüketim işini yapan basit bir konsol uygulaması olduğunu düşünün. Her konsol uygulaması daha sonra bir [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği oluşturur ve olay hub'ları hizmetine kaydeder.

Bu örnek senaryoda, 5 **EventProcessorHost** örneklerine 16 bölüm ayrıldı diyelim. Bazı **EventProcessorHost** örnekleri diğerlerinden daha birkaç bölüm kendi olabilir. **Bir EventProcessorHost** örneğinin sahip olduğu her bölüm için `SimpleEventProcessor` sınıfın bir örneğini oluşturur. Bu nedenle, `SimpleEventProcessor` her bölüme bir atanmış olmak üzere genel olarak 16 örnek vardır.

Aşağıdaki liste şu örneği özetley:

- 16 Olay Hub'ları bölümleri.
- Her VM'de 5 VM, 1 tüketici uygulaması (örneğin Consumer.exe).
- Consumer.exe tarafından her VM'de 1 adet kayıtlı 5 EPH örneği.
- 5 `SimpleEventProcessor` EPH örneği tarafından oluşturulan 16 nesne.
- 1 Consumer.exe uygulaması `SimpleEventProcessor` 4 nesne içerebilir, çünkü 1 EPH örneği 4 bölüme sahip olabilir.

## <a name="partition-ownership-tracking"></a>Bölüm sahipliği izleme

Bir EPH örneğine (veya tüketiciye) bir bölümün sahipliği, izleme için sağlanan Azure Depolama hesabı üzerinden izlenir. İzlemeyi aşağıdaki gibi basit bir tablo olarak görüntüleyebilirsiniz. Sağlanan Depolama hesabı nın altındaki lekeleri inceleyerek gerçek uygulamayı görebilirsiniz:

| **Tüketici grubu adı** | **Bölüm Kimliği** | **Ana bilgisayar adı (sahibi)** | **Kira (veya mülkiyet) edinilen süre** | **Bölüm içinde ofset (denetim noktası)** |
| --- | --- | --- | --- | --- |
| $Default | 0 | Tüketici\_VM3 | 2018-04-15T01:23:45 | 156 |
| $Default | 1 | Tüketici\_VM4 | 2018-04-15T01:22:13 | 734 |
| $Default | 2 | Tüketici\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| $Default | 15 | Tüketici\_VM3 | 2018-04-15T01:22:56 | 976 |

Burada, her ana bilgisayar belirli bir süre (kira süresi) için bir bölümün sahipliğini satın aldı. Bir ana bilgisayar başarısız olursa (VM kapanır), kira süresi dolur. Diğer ana bilgisayarlar bölümün sahipliğini almaya çalışır ve ana bilgisayarlardan biri başarılı olur. Bu işlem, yeni bir sahiple bölümün kirasını sıfırlar. Bu şekilde, bir seferde yalnızca tek bir okuyucu bir tüketici grubu içinde herhangi bir bölümden okuyabilirsiniz.

## <a name="receive-messages"></a>İleti alma

[ProcessEventsAsync'e](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) yapılan her çağrı bir etkinlik koleksiyonu sunar. Bu olaylarla başa çıkmak senin sorumluluğunda. İşlemci ana bilgisayarın her iletiyi en az bir kez işlediğinden emin olmak istiyorsanız, kendi kodunuzu yeniden denemeye devam etme nizi yazmanız gerekir. Ama zehirli mesajlar konusunda dikkatli olun.

Bu nispeten hızlı şeyler yapmanız tavsiye edilir; diğer bir şey, mümkün olduğunca az işlem yapmak. Bunun yerine, tüketici gruplarını kullanın. Depolamaya yazmanız ve bazı yönlendirmeler yapmanız gerekiyorsa, iki tüketici grubu kullanmak ve ayrı olarak çalışan iki [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulamasına sahip olmak daha iyidir.

İşleminiz sırasında bir noktada, okuduklarınızı ve tamamladıklarınızı izlemek isteyebilirsiniz. Okumayı yeniden başlatmanız gerekiyorsa, akışı yeniden başlatmanız gerekiyorsa, izlemeyi sürdürmek önemlidir. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) *denetim noktaları*kullanarak bu izlemeyi basitleştirir. Denetim noktası, belirli bir tüketici grubu içinde belirli bir bölüm için bir konum veya ofsettir ve bu noktada iletileri işlediğinizi memnun etmişsinizdir. **EventProcessorHost'ta** bir denetim noktasını işaretleme, [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) nesnesindeki [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yöntemini çağırarak gerçekleştirilir. Bu işlem [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) yöntemi içinde yapılır, ancak [CloseAsync'de](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync)de yapılabilir.

## <a name="checkpointing"></a>Denetim noktası oluşturma

[CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yöntemiiki aşırı yüke sahiptir: ilki, parametreleri olmayan, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)tarafından döndürülen koleksiyondaki en yüksek olay mahsup noktalarıdır. Bu ofset bir "yüksek su" işaretidir; aradığınızda tüm son olayları işlediğinizi varsayar. Bu yöntemi bu şekilde kullanırsanız, diğer olay işleme kodunuz döndükten sonra aramanızın beklendiğini unutmayın. İkinci aşırı yükleme, denetim noktasına bir [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) örneği belirtmenize olanak tanır. Bu yöntem, denetim noktasına farklı bir filigran türü kullanmanızı sağlar. Bu filigranla bir "düşük su" işareti uygulayabilirsiniz: emin olduğunuz en düşük sıralı olay işlenmiştir. Bu aşırı yükleme, ofset yönetiminde esneklik sağlamak için sağlanır.

Denetim noktası yapıldığında, bölüme özgü bilgileri (özellikle ofset) içeren bir JSON dosyası, [EventProcessorHost'a](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)kurucu olarak verilen depolama hesabına yazılır. Bu dosya sürekli olarak güncelleştirilir. Bu bağlamda kontrol noktası dikkate almak önemlidir - her mesajı kontrol etmek akıllıca olacaktır. Denetim noktası için kullanılan depolama hesabı büyük olasılıkla bu yükü işlemez, ancak daha da önemlisi her olay kontrol noktası, hizmet veri servisi sırasının olay hub'ından daha iyi bir seçenek olabileceği sıralanmış bir ileti deseni göstergesidir. Olay Hub'larının arkasındaki fikir, büyük ölçekte "en az bir kez" teslimat elde etmektir. Downstream sistemlerinizi iktidarlı hale getirerek, aynı olayların birden çok kez alınmasıyla sonuçlanan hatalardan veya yeniden başlatmalardan kurtarmak kolaydır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) iş parçacığı güvenli ve [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor)örneğine göre senkron bir şekilde çalışır. Olaylar bir bölüm için geldiğinde, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) bu bölüm için **IEventProcessor** örneğine çağrılır ve bölüm için **ProcessEventsAsync'e** yapılacak diğer çağrıları engeller. Sonraki iletiler ve **ProcessEventsAsync** çağrıları, ileti pompası diğer iş parçacıkları üzerinde arka planda çalıştırmaya devam ettikçe arka planda sıraya alınır. Bu iş parçacığı güvenliği, iş parçacığı güvenli koleksiyonihtiyacını ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="shut-down-gracefully"></a>Zarif bir şekilde kapatın

Son olarak, [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) tüm bölüm okuyucuların temiz bir kapatma sağlar ve her zaman [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)bir örnek kapatırken çağrılmalıdır. Aksi takdirde, kira süresi nin sona ermesi ve Dönem çakışmaları nedeniyle **EventProcessorHost'un** diğer örnekleri başlatıldığında gecikmelere neden olabilir. Dönem yönetimi makalenin [Dönem](#epoch) bölümünde ayrıntılı olarak ele alınmıştır. 

## <a name="lease-management"></a>Kira yönetimi
EventProcessorHost bir örneği ile bir olay işlemci sınıfı kayıt olay işleme başlar. Ana bilgisayar örneği, olay hub'ının bazı bölümleri için kiralama lar alır ve büyük olasılıkla diğer ana bilgisayar örneklerinden bazılarını, tüm ana bilgisayar örnekleriarasında bölümlerin eşit dağılımında biraraya gelen bir şekilde kaplar. Kiralanan her bölüm için, ana bilgisayar örneği sağlanan olay işlemci sınıfının bir örneğini oluşturur, ardından bu bölümden olayları alır ve bunları olay işlemcisi örneğine geçirir. Daha fazla örnek eklendikçe ve daha fazla kiralama alındıkça, EventProcessorHost sonunda tüm tüketiciler arasındaki yükü dengeler.

Daha önce açıklandığı gibi, izleme tablosu büyük ölçüde [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync)otomatik ölçekdoğasını kolaylaştırır. **EventProcessorHost** bir örnek başlar gibi, mümkün olduğunca çok sayıda kira kazanır ve olayları okumaya başlar. Kiralama lar sona ermek üzere olduğu için **EventProcessorHost** rezervasyon yaptırarak bunları yenilemeye çalışır. Kiralama yenileme için kullanılabilirse, işlemci okumaya devam ediyor, ancak değilse, okuyucu kapatılır ve [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) çağrılır. **CloseAsync** bu bölüm için herhangi bir son temizleme gerçekleştirmek için iyi bir zaman.

**EventProcessorHost** bir [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) özelliği içerir. Bu özellik, kira yönetimi üzerinde denetim sağlar. [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulamanızı kaydetmeden önce bu seçenekleri ayarlayın.

## <a name="control-event-processor-host-options"></a>Olay İşlemcisi Ana Bilgisayar seçeneklerini denetleme

Ayrıca, [RegisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) bir aşırı yükleme bir parametre olarak [Bir EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) nesne alır. [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) kendisi davranışını denetlemek için bu parametreyi kullanın. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) dört özellik ve bir olay tanımlar:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)bir çağrı almak istediğiniz koleksiyonun maksimum boyutu. Bu boyut minimum değil, yalnızca maksimum boyut. Alınacak daha az ileti varsa, **ProcessEventsAsync** kullanılabilir olduğu kadar çok iletiyle yürütülür.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): Istemcinin alması gereken ileti sayısının üst sınırını belirlemek için temel AMQP kanalı tarafından kullanılan değerdir. Bu değer [MaxBatchSize'dan](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize)büyük veya eşit olmalıdır.
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): Bu parametre **doğruysa,** [ProcessEventsAsync,](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) altta yatan çağrı bir bölüm üzerinde olayları almak için zaman dışarı çağrıldı. Bu yöntem, bölüm üzerinde hareketsizlik dönemlerinde zaman tabanlı eylemler icra etmek için yararlıdır.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): Bir okuyucu bir bölümü okumaya başladığında ilk ofset sağlamak için çağrılan bir işlev işaretçisi veya lambda ifadesinin ayarlanmasını sağlar. Bu mahsup belirtmeden, bir ofset ile JSON dosyası zaten [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) oluşturucuya verilen depolama hesabına kaydedilmiş sürece, okuyucu en eski olay başlar. Bu yöntem, okuyucu başlangıç davranışını değiştirmek istediğinizde yararlıdır. Bu yöntem çağrıldığı zaman, nesne parametresi okuyucunun başlatıldığı bölümkimliği içerir.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): [EventProcessorHost'ta](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost)meydana gelen altta yatan özel durumlar hakkında bildirim almanızı sağlar. Işler beklediğiniz gibi çalışmıyorsa, bu olay aramaya başlamak için iyi bir yerdir.

## <a name="epoch"></a>Dönem

Alma dönemi şu şekilde çalışır:

### <a name="with-epoch"></a>Epoch ile
Dönem, bölüm/kira sahipliğini zorlamak için hizmetin kullandığı benzersiz bir tanımlayıcıdır (dönem değeri). [CreateEpochReceiver](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createepochreceiver?view=azure-dotnet) yöntemini kullanarak Epoch tabanlı bir alıcı oluşturursunuz. Bu yöntem, Dönem tabanlı bir alıcı oluşturur. Alıcı, belirtilen tüketici grubundan belirli bir olay merkezi bölümü için oluşturulur.

Çağ özelliği, kullanıcılara aşağıdaki kurallarla herhangi bir zamanda bir tüketici grubunda yalnızca bir alıcı olduğundan emin olma olanağı sağlar:

- Bir tüketici grubunda varolan bir alıcı yoksa, kullanıcı herhangi bir dönem değeri olan bir alıcı oluşturabilir.
- E1 değeri olan bir alıcı varsa ve e1 <= e2'nin bulunduğu e2'nin e2 değerine sahip yeni bir alıcı oluşturulursa, e1'li alıcı otomatik olarak kesilir, e2 ile alıcı başarıyla oluşturulur.
- E1 değeri olan bir alıcı varsa ve e1 e2'nin e2'ye > bir çağ değeri ile yeni bir alıcı oluşturulursa, e2'nin hata ile başarısız olması durumunda e2'nin oluşturulması: E1 e1'li bir alıcı zaten vardır.

### <a name="no-epoch"></a>Hayır Dönem
[CreateReceiver](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createreceiver?view=azure-dotnet) yöntemini kullanarak Dönem tabanlı olmayan bir alıcı oluşturursunuz. 

Akış işleminde, kullanıcıların tek bir tüketici grubunda birden çok alıcı oluşturmak istediği bazı senaryolar vardır. Bu tür senaryoları desteklemek için, biz çığır açan bir alıcı oluşturmak için yeteneği var ve bu durumda biz tüketici grubunda en fazla 5 eşzamanlı alıcılar izin verir.

### <a name="mixed-mode"></a>Karışık Mod
Çağlı bir alıcı oluşturup aynı tüketici grubunda çağ alabilmemeye veya tam tersi bir alıcıya geçtiğiniz uygulama kullanımını önermiyoruz. Ancak, bu davranış oluştuğunda, hizmet aşağıdaki kuralları kullanarak işler:

- Zaten epoch e1 ile oluşturulan bir alıcı varsa ve aktif olaylar alıyorsa ve yeni bir alıcı hiçbir dönem ile oluşturulur, yeni alıcı oluşturulması başarısız olur. Dönem alıcıları her zaman sistemde önceliklidir.
- Zaten epoch e1 ile oluşturulan bir alıcı vardı ve bağlantısı var ve yeni bir alıcı yeni bir MessagingFactory üzerinde hiçbir dönem ile oluşturulur, yeni alıcı oluşturulması başarılı olacaktır. Burada sistemimizin ~10 dakika sonra "alıcı kopukluğu"nu tespit edeceği bir uyarı vardır.
- Bir veya daha fazla alıcı hiçbir dönem ile oluşturulur ve yeni bir alıcı dönem e1 ile oluşturulur, tüm eski alıcılar kesilir.


> [!NOTE]
> Çağ kullanan uygulamalar ve hatalardan kaçınmak için çağ kullanmayanlar için farklı tüketici grupları kullanmanızı öneririz. 


## <a name="next-steps"></a>Sonraki adımlar

Olay İşlemcisi Ana Bilgisayar'ı bildiğinize göre, Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [GitHub'da Etkinlik Hub'ları örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples)
