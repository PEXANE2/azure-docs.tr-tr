---
title: Birden çok örnekte bakiye bölüm yükü - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bir olay işlemcisi ve Azure Etkinlik Hub'ları SDK'yı kullanarak uygulamanızın birden fazla örneği arasında bölüm yükünü nasıl dengeleriz açıklar.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: 1244fe64d0c23782fdae7a0f92415bada4bef55a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76907662"
---
# <a name="balance-partition-load-across-multiple-instances-of-your-application"></a>Uygulamanızın birden çok örneği arasında bakiye bölüm yükü
Olay işleme uygulamanızı ölçeklendirmek için, uygulamanın birden çok örneğini çalıştırabilir ve yükü kendi aralarında dengelemesini sağlayabilirsiniz. Eski [sürümlerde, EventProcessorHost](event-hubs-event-processor-host.md) alırken programınızın birden çok örneği ile denetim noktası olayları arasındaki yükü dengelemenize olanak sağlar. Yeni sürümlerde (5.0 itibaren), **EventProcessorClient** (.NET ve Java) veya **EventHubConsumerClient** (Python ve JavaScript) aynı şeyi yapmanızı sağlar. Geliştirme modeli olaylar kullanılarak daha basit hale getirilir. Bir etkinlik işleyicisi kaydederek ilgilendiğiniz etkinliklere abone olursunuz.

Bu makalede, olayları bir olay merkezinden okumak için birden çok örnek kullanmak için bir örnek senaryo açıklanır ve ardından olay işlemci istemcisinin özellikleri hakkında ayrıntılı bilgi verir, bu da aynı anda birden çok bölümden olayları almanızı ve diğer diğer leriyle bakiye yüklemenize olanak tanır aynı etkinlik merkezini ve tüketici grubunu kullanan tüketiciler.

> [!NOTE]
> Olay Hub'ları için ölçeklendirmenin anahtarı, bölümlenmiş tüketicilerin fikridir. [Rakip tüketici](https://msdn.microsoft.com/library/dn568101.aspx) deseninin aksine, bölümlenmiş tüketici deseni, çekişme darboğazını ortadan kaldırarak ve uçlardan uca paralelliği kolaylaştırarak yüksek ölçek sağlar.

## <a name="example-scenario"></a>Örnek senaryo

Örnek bir senaryo olarak, 100.000 evi izleyen bir ev güvenlik şirketi düşünün. Her dakika, bir hareket dedektörü, kapı / pencere açık sensör, cam kırılma dedektörü, ve benzeri, her evde yüklü gibi çeşitli sensörlerveri alır. Şirket sakinleri için neredeyse gerçek zamanlı olarak evlerinin etkinliğini izlemek için bir web sitesi sağlar.

Her sensör verileri bir olay merkezine iter. Olay hub'ı 16 bölümle yapılandırılır. Tüketen uçta, bu olayları okuyabilen, bunları birleştirebilen (filtre, toplam vb.) ve toplamı kullanıcı dostu bir web sayfasına yansıtılan bir depolama blob'una atabilmeniz için bir mekanizmaya ihtiyacınız vardır.

## <a name="write-the-consumer-application"></a>Tüketici başvurusunu yazın

Tüketiciyi dağıtılmış bir ortamda tasarlarken, senaryo aşağıdaki gereksinimleri karşılamalıdır:

1. **Ölçek:** Her tüketicinin birkaç Etkinlik Hub'ı bölümünden okuma sahipliğini almasıyla birden çok tüketici oluşturun.
2. **Yük dengesi:** Tüketicileri dinamik olarak artırın veya azaltın. Örneğin, her eve yeni bir sensör türü (örneğin, karbon monoksit dedektörü) eklendiğinde, olay sayısı artar. Bu durumda, operatör (bir insan) tüketici örneklerinin sayısını artırır. Daha sonra, tüketicilerin havuzu, yeni eklenen tüketicilerle yükü paylaşmak için sahip oldukları bölüm sayısını yeniden dengeleyebilir.
3. **Hatalarda sorunsuz özgeçmiş:** Bir tüketici **(tüketici A)** başarısız olursa (örneğin, tüketiciyi barındıran sanal makine aniden çöker), diğer tüketiciler **tüketici A'ya** ait bölümleri alıp devam edebilir. Ayrıca, bir *kontrol noktası* veya *ofset*olarak adlandırılan devam noktası, **tüketici A** başarısız olduğu tam noktada olmalıdır, ya da biraz bundan önce.
4. **Olayları tüketin:** Önceki üç nokta tüketici yönetimi ile ilgili olsa da, olayları tüketmek ve onunla yararlı bir şey yapmak için kod olmalıdır. Örneğin, toplayıp blob depolama alanına yükleyin.

## <a name="event-processor-or-consumer-client"></a>Olay işlemcisi veya tüketici istemcisi

Bu gereksinimleri karşılamak için kendi çözümünüzü oluşturmanız gerekmez. Azure Etkinlik Hub'ları SDK'ları bu işlevselliği sağlar. .NET veya Java SDK'larında bir olay işlemci istemcisi (EventProcessorClient) ve Python ve Java Script SDK'larında EventHubConsumerClient kullanırsınız. SDK'nın eski sürümünde, bu özellikleri destekleyen olay işlemci ana bilgisayarı (EventProcessorHost) idi.

Üretim senaryolarının çoğu için, olayları okumak ve işlemek için olay işlemci istemcisini kullanmanızı öneririz. İşlemci istemcisi, bir olay merkezinin tüm bölümlerindeki olayları gerçekleştirme ve hataya dayanıklı bir şekilde işlemek için sağlam bir deneyim sağlarken, ilerlemesini kontrol etmek için bir araç sağlamayı amaçlamaktadır. Olay işlemci istemcileri, belirli bir olay merkezi için bir tüketici grubu bağlamında işbirliği içinde çalışabilme yeteneğine sahiptir. İstemciler, örnekler kullanılabilir hale geldikçe veya grup için kullanılamadığında, işlerin dağıtımını ve dengelemesini otomatik olarak yönetir.

## <a name="partition-ownership-tracking"></a>Bölüm sahipliği izleme

Olay işlemci si örneği genellikle bir veya daha fazla bölümden olayları alır ve işler. Bölümlerin sahipliği, bir olay merkezi ve tüketici grubu birleşimi ile ilişkili tüm etkin olay işlemci örnekleri arasında eşit olarak dağıtılır. 

Her olay işlemcisi benzersiz bir tanımlayıcı verilir ve bir denetim noktası deposunda bir giriş ekleyerek veya güncelleştirerek bölümlerin sahipliğini talep eder. Tüm olay işlemci örnekleri, kendi işleme durumunu güncelleştirmenin yanı sıra diğer etkin durumlar hakkında bilgi edinmek için bu mağazayla düzenli olarak iletişim kurar. Bu veriler daha sonra etkin işlemciler arasındaki yükü dengelemek için kullanılır. Yeni örnekler ölçeklendirmek için işleme havuzuna katılabilir. Örnekler, hatalar nedeniyle veya küçültülmek için aşağı gittiğinde, bölüm sahipliği incelikle diğer etkin işlemcilere aktarılır.

Denetim noktası deposundaki bölüm sahipliği kayıtları Olay Hub'ları ad alanını, olay hub adını, tüketici grubunu, olay işlemci tanımlayıcısını (sahibi olarak da bilinir), bölüm kimliğini ve son değiştirilen zamanı izler.



| Event Hubs ad alanı               | Olay Hub'ı adı | **Tüketici grubu** | Sahip                                | Bölüm Kimliği | Son değiştirilen süre  |
| ---------------------------------- | -------------- | :----------------- | :----------------------------------- | :----------- | :------------------ |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 3be3f9d3-9d9e-4c50-9491-85ece8334ff6 | 0            | 2020-01-15T01:22:15 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | f5cc5176-ce96-4bb4-bbaa-a0e3a9054ecf | 1            | 2020-01-15T01:22:17 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 72b980e9-2efc-4ca7-ab1b-ffd7bece8472 | 2            | 2020-01-15T01:22:10 |
|                                    |                | :                  |                                      |              |                     |
|                                    |                | :                  |                                      |              |                     |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 844bd8fb-1f3a-4580-984d-6324f9e208af | 15           | 2020-01-15T01:22:00 |

Her olay işlemci örneği bir bölümün sahipliğini kazanır ve bilinen son [denetim noktasından](# Checkpointing)bölümü işlemeye başlar. Bir işlemci başarısız olursa (VM kapanır), diğer örnekler bunu son değiştirilen zamana bakarak algılar. Diğer örnekler, daha önce etkin olmayan örneğin sahip olduğu bölümlerin sahipliğini almaya çalışır ve denetim noktası deposu, örneklerden yalnızca birinin bir bölümün sahipliğini talep etmeyi başardığını garanti eder. Yani, herhangi bir zaman belirli bir noktada, en fazla bir işlemci bir bölümden olayları alan var.

## <a name="receive-messages"></a>İleti alma

Bir olay işlemcisi oluşturduğunuzda, olayları ve hataları işleyecek işlevleri belirtirsiniz. Olayları işleyen işleve yapılan her çağrı, belirli bir bölümden tek bir olay sunar. Bu olayla ilgilenmek senin sorumluluğunda. Tüketicinin her iletiyi en az bir kez işlediğinden emin olmak istiyorsanız, yeniden deneme mantığıyla kendi kodunuzu yazmanız gerekir. Ama zehirli mesajlar konusunda dikkatli olun.

İşleri nispeten hızlı yapmanızı öneririz. Diğer bir zamanda, mümkün olduğunca az işlem yapın. Depolamaya yazmanız ve bazı yönlendirmeler yapmanız gerekiyorsa, iki tüketici grubu kullanmak ve iki olay işlemcisi olması daha iyidir.

## <a name="checkpointing"></a>Denetim noktası oluşturma

*Denetim noktası,* olay işlemcisinin bir bölüm içinde başarıyla işlenen son olayın konumunu işaretlediği veya işlediği bir işlemdir. Bir denetim noktası işaretleme genellikle olayları işleyen ve bir tüketici grubu içinde bölüm başına bazda oluşan işlev içinde yapılır. 

Bir olay işlemcisi bir bölümden kesilirse, başka bir örnek, söz konusu bölümün son işlemcisi tarafından daha önce işlenen denetim noktasındaki bölümü işlemeye devam edebilir. İşlemci bağlandığında, okumaya başlanacak konumu belirtmek için ofset'i olay merkezine geçirir. Bu şekilde, hem alt akış uygulamaları tarafından olayları "tamamlandı" olarak işaretlemek hem de bir olay işlemcisi çöktürdüğünde esneklik sağlamak için denetim noktasını kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. 

Bir olayı işlenmiş olarak işaretlemek için denetim noktası yapıldığında, denetim noktası deposundaki bir giriş, olayın ofset ve sıra numarasıyla eklenir veya güncelleştirilir. Kullanıcılar denetim noktasını güncelleştirme sıklığına karar vermelidir. Başarıyla işlenen her olaydan sonra güncelleştirme, temel denetim noktası deposuna bir yazma işlemini tetikledikçe performans ve maliyet etkilerine sahip olabilir. Ayrıca, her bir olayı denetlemek, Hizmet Veri Servisi sırasının olay hub'ından daha iyi bir seçenek olabileceği sıralanmış bir ileti deseni göstergesidir. Olay Hub'larının arkasındaki fikir, büyük ölçekte "en az bir kez" teslimat elde etmektir. Downstream sistemlerinizi iktidarlı hale getirerek, aynı olayların birden çok kez alınmasıyla sonuçlanan hatalardan veya yeniden başlatmalardan kurtarmak kolaydır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, olay işlemcisi veya tüketici iş parçacığı güvenlidir ve eşzamanlı bir şekilde çalışır. Olaylar bir bölüm için geldiğinde, olayları işleyen işlev çağrılır. İleti pompası diğer iş parçacıkları üzerinde arka planda çalışmaya devam ettikçe sonraki iletiler ve bu işlev için aramalar arka planda sıraya. Bu iş parçacığı güvenliği, iş parçacığı güvenli koleksiyonihtiyacını ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki hızlı başlangıçları görün:

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](get-started-python-send-v2.md)
- [Javascript](get-started-node-send-v2.md)
