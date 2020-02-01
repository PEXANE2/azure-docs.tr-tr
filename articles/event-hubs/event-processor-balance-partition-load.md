---
title: Birden çok örnek arasında bölüm yükünü dengeleme-Azure Event Hubs | Microsoft Docs
description: Olay işlemcisi ve Azure Event Hubs SDK kullanarak, uygulamanızın birden çok örneğinde bölüm yükünün nasıl dengeleneceğini açıklar.
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
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76907662"
---
# <a name="balance-partition-load-across-multiple-instances-of-your-application"></a>Uygulamanızın birden çok örneği arasında bölüm yükünü dengeleme
Olay işleme uygulamanızı ölçeklendirmek için, uygulamanın birden çok örneğini çalıştırabilir ve yükün kendileri arasında dengelenmesi sağlayabilirsiniz. Eski sürümlerde, [Eventprocessorhost](event-hubs-event-processor-host.md) , alma sırasında programınızın birden çok örneği ve kontrol noktası olayları arasındaki yükü dengelemenize izin verildi. Yeni sürümlerde (5,0 sonraki sürümler), **Eventprocessorclient** (.net ve Java) veya **Eventhubconsumerclient** (Python ve JavaScript) aynı şekilde yapmanıza olanak sağlar. Geliştirme modeli olayları kullanarak daha basit hale getirilir. Bir olay işleyicisini kaydederek ilgilendiğiniz olaylara abone olursunuz.

Bu makalede, bir olay hub 'ından olayları okumak için birden çok örnek kullanmanın örnek bir senaryosu açıklanmakta ve daha sonra, aynı anda birden çok bölümden gelen ve yük dengelemeye olanak tanıyan olay işlemcisi istemcisinin özellikleri hakkında ayrıntılı bilgi verir. aynı Olay Hub 'ını ve tüketici grubunu kullanan tüketiciler.

> [!NOTE]
> Event Hubs için ölçeklendirmek için anahtar bölümlenmiş tüketici olur. Tersine [rakip tüketiciler](https://msdn.microsoft.com/library/dn568101.aspx) düzeni, bölünmüş bir tüketici modeli sağlar büyük ölçekli Çekişme performans sorunu kaldırma ve uçtan uca paralellik etkinleştirme.

## <a name="example-scenario"></a>Örnek senaryo

Örnek bir senaryo 100.000 havaalanlarından izleyen bir giriş güvenliği şirketi düşünün. Her dakika, bir hareket algılayıcısı, kapı/pencere açık algılayıcı, cam kesmesi algılayıcısı vb. gibi çeşitli sensörlerden, her girişe yüklenmiş olan verileri alır. Şirket, kendi giriş neredeyse gerçek zamanlı etkinliğini izlemek vatandaşlar için bir web sitesi sağlar.

Her algılayıcı verileri olay hub'ına gönderir. Olay hub'ı 16 bölümleri ile yapılandırılır. Tüketim uçta, bu olayları okuyabilen, bunları birleştirebilen (filtre, toplama ve benzeri) bir mekanizmaya ihtiyacınız vardır ve toplamanın, daha sonra Kullanıcı dostu bir Web sayfasına yansıtılmasına neden olan bir Depolama Blobu ile dökümünü alabilirsiniz.

## <a name="write-the-consumer-application"></a>Tüketici uygulaması yazma

Dağıtılmış bir ortamda tüketici tasarlarken, senaryo aşağıdaki gereksinimleri ele almalısınız.

1. **Ölçek:** birkaç Event Hubs bölümleri okumaya sahipliğini her bir tüketici içeren birden fazla tüketici oluşturun.
2. **Yük Dengeleme:** artırmak veya dinamik olarak tüketiciler azaltın. Örneğin, her giriş için yeni bir algılayıcı türü (örneğin, gizli monoxide algılayıcısı) eklendiğinde, olay sayısını artırır. Bu durumda, ' % s'işleci (insan) tüketici örneği sayısını artırır. Tüketici havuzu oldukları, bölüm sayısını dengeleyebilir sonra yeni eklenen tüketicileriyle yük paylaşma.
3. **Hatalarda sorunsuz bir şekilde sürdürülür:** Bir tüketici (**tüketici a**) başarısız olursa (örneğin, tüketiciyi barındıran sanal makine aniden kilitlenirse), diğer tüketiciler **tüketici a** 'nın sahip olduğu bölümleri alabilir ve devam edebilir. Ayrıca, devamlılık noktası, adlı bir *denetim noktası* veya *uzaklığı*, tam noktada olmalıdır **tüketici A** hatalı veya biraz daha önceki.
4. **Olayları tüketme:** Önceki üç işaret, tüketicinin yönetimiyle ilgilenirken, olayları tüketmek ve bu konuyla ilgili bir şey yapmak için kod olmalıdır. Örneğin, bunu toplayın ve BLOB depolamaya yükleyin.

## <a name="event-processor-or-consumer-client"></a>Olay işlemcisi veya tüketici istemcisi

Bu gereksinimleri karşılamak için kendi çözümünüzü derlemeniz gerekmez. Azure Event Hubs SDK 'Ları bu işlevselliği sağlar. .NET veya Java SDK 'larında, bir olay işlemcisi istemcisi (EventProcessorClient) ve Python ve Java betik SDK 'lerinde EventHubConsumerClient komutunu kullanırsınız. SDK 'nın eski sürümünde bu özellikleri destekleyen olay işleyicisi ana bilgisayarı (EventProcessorHost).

Üretim senaryolarının çoğunluğunda, olayları okumak ve işlemek için olay işlemcisi istemcisini kullanmanızı öneririz. İşlemci istemcisinin, ilerleme durumunu kontrol etmek için bir yol sağlarken bir olay hub 'ının tüm bölümlerindeki olayları performans ve hataya dayanıklı bir şekilde işlemeye yönelik sağlam bir deneyim sağlamaya yöneliktir. Olay işlemcisi istemcileri, belirli bir olay hub 'ı için bir tüketici grubu bağlamında birlikte çalışabilme özelliği de vardır. İstemciler, grup için kullanılabilir veya kullanılamaz hale geldiğinde iş dağıtımını ve iş dengelemeyi otomatik olarak yönetir.

## <a name="partition-ownership-tracking"></a>İzleme bölümü sahipliği

Bir olay işlemcisi örneği, genellikle bir veya daha fazla bölümden olayları sahipleyip işler. Bölümlerin sahipliği, bir olay hub 'ı ve Tüketici grubu birleşimiyle ilişkili tüm etkin olay işlemcisi örnekleri arasında eşit olarak dağıtılır. 

Her olay işlemcisine, bir denetim noktası deposundaki bir girişi ekleyerek veya güncelleştirerek bölümlerin, benzersiz bir tanımlayıcı ve talep sahipliği olarak verilmiş olması sağlanır. Tüm olay işlemcisi örnekleri, kendi işleme durumunu güncelleştirmek ve diğer etkin örnekler hakkında bilgi edinmek için bu mağazanızla düzenli olarak iletişim kurar. Bu veriler daha sonra etkin işlemciler arasında yükü dengelemek için kullanılır. Yeni örnekler, ölçeği genişletmek için işleme havuzuna katılabilir. Örnekler doğru olduğunda, hatalardan veya ölçeği ölçeklendirilerek, Bölüm sahipliği diğer etkin işlemcilere düzgün şekilde aktarılır.

Denetim noktası deposundaki bölüm sahipliği kayıtları, Event Hubs ad alanı, Olay Hub 'ı adı, Tüketici grubu, olay işlemci tanımlayıcısı (sahip olarak da bilinir), bölüm kimliği ve son değiştirilme zamanı izler.



| Event Hubs ad alanı               | Olay Hub'ı adı | **Tüketici grubu** | Sahip                                | Bölüm KIMLIĞI | Son değiştirme zamanı  |
| ---------------------------------- | -------------- | :----------------- | :----------------------------------- | :----------- | :------------------ |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 3be3f9d3-9d9e-4c50-9491-85ece8334ff6 | 0            | 2020-01-15T01:22:15 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | f5cc5176-ce96-4bb4-bbaa-a0e3a9054ecf | 1            | 2020-01-15T01:22:17 |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 72b980e9-2efc-4ca7-ab1b-ffd7bece8472 | 2            | 2020-01-15T01:22:10 |
|                                    |                | :                  |                                      |              |                     |
|                                    |                | :                  |                                      |              |                     |
| mynamespace.servicebus.windows.net | myeventhub     | myconsumergroup    | 844bd8fb-1f3a-4580-984d-6324f9e208af | 15           | 2020-01-15T01:22:00 |

Her olay işlemcisi örneği bir bölümün sahipliğini alır ve bölümü bilinen son [denetim noktasından](# Checkpointing)işlemeye başlar. Bir işlemci başarısız olursa (VM kapanır), diğer örnekler son değiştirilme zamanına bakarak bunu algılar. Diğer örnekler, önceden etkin olmayan örneğe ait olan bölümlerin sahipliğini almaya çalışır ve denetim noktası deposu, bir bölümün sahipliğinin yalnızca birinin başarılı olmasını garanti eder. Bu nedenle, belirli bir zamanda, bir bölümden çok sayıda olay alan bir işlemci vardır.

## <a name="receive-messages"></a>İleti alma

Bir olay işlemcisi oluşturduğunuzda, olayları ve hataları işleyecek işlevleri belirtirsiniz. Olayları işleyen işleve yapılan her çağrı, belirli bir bölümden tek bir olay sağlar. Bu olayı işlemek sizin sorumluluğunuzdadır. Tüketicinin her iletiyi en az bir kez işlediğinden emin olmak istiyorsanız, yeniden deneme mantığı ile kendi kodunuzu yazmanız gerekir. Ancak kired iletiler hakkında dikkatli olun.

Şeyleri nispeten hızlı yapmanızı öneririz. Diğer bir deyişle, mümkün olduğunca az işlem yapın. Depolama alanına yazmanız ve bazı yönlendirme yapmanız gerekiyorsa, iki Tüketici grubu kullanmak ve iki olay işlemcisi olması daha iyidir.

## <a name="checkpointing"></a>Denetim noktası oluşturma

*Checkişaret* , bir olay işlemcisinin bir bölüm içinde son başarılı bir şekilde işlenen etkinliğin konumunu işaretleyen veya işleme yaptığı bir işlemdir. Bir kontrol noktasının işaretlenmesi genellikle olayları işleyen ve bir tüketici grubu içindeki bölüm başına temelinde gerçekleşen işlev içinde yapılır. 

Bir olay işlemcisinin bir bölümden bağlantısı kesilirse, başka bir örnek, bu bölümün daha önce ilgili tüketici grubundaki son işlemci tarafından kaydedilen denetim noktasında bölümü işlemeye da yardımcı olabilir. İşlemci bağlandığı zaman, okumayı başlatacak konumu belirtmek için Olay Hub 'ına geçişi geçirir. Bu şekilde, hem aşağı akış uygulamaları için olayları "tamamlanmış" olarak işaretlemek ve bir olay işlemcisi geçtiğinde dayanıklılık sağlamak için Checkmark 'ı kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. 

Denetim noktası bir olayı işlenen olarak işaretlemek için gerçekleştirildiğinde, denetim noktası deposundaki bir giriş, olayın kayması ve sıra numarası ile eklenir veya güncelleştirilir. Kullanıcılar, denetim noktasını güncelleştirme sıklığından önce karar almalıdır. Başarılı bir şekilde işlenen her olayın ardından güncelleştirme, temel alınan denetim noktası deposuna bir yazma işlemi tetiklediği için performans ve maliyet etkilerine sahip olabilir. Ayrıca, her bir olayın denetim noktası, bir Service Bus sırasının bir olay hub 'ından daha iyi bir seçenek olabileceği sıraya alınmış mesajlaşma deseninin bir göstergesi olduğunu gösteriyor. Event Hubs arkasında büyük ölçekte "en az bir kez" teslim alma olur. Aşağı Akış sistemlerine ıdempotent yaparak bu hatalardan kurtarma kolayca veya sonucunda aynı olayları birden çok kez alınan yeniden başlatır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, olay işlemcisi veya tüketicisi iş parçacığı güvenlidir ve zaman uyumlu bir şekilde davranır. Bir bölüme yönelik olaylar geldiğinde, olayları işleyen işlev çağrılır. İleti göndericisi, arka planda diğer iş parçacıklarında çalışmaya devam ettiğinden, sonraki iletiler ve bu işlev için bu işleve yapılan çağrılar arka planda sıraya açılır. Bu iş parçacığı güvenliği, iş parçacığı güvenli koleksiyonları gereksinimini ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki hızlı çalışmaya bakın:

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
