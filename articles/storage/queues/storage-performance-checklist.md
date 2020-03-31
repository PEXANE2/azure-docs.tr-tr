---
title: Sıra depolama için performans ve ölçeklenebilirlik denetim listesi - Azure Depolama
description: Yüksek performanslı uygulamalar geliştirmede Sıra depolama ile kullanılmak üzere kanıtlanmış uygulamaların bir kontrol listesi.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 10/10/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: eb1821537e6e25b05dfdca3107729eecf4c6e1bf
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75750502"
---
# <a name="performance-and-scalability-checklist-for-queue-storage"></a>Sıra depolama için performans ve ölçeklenebilirlik denetim listesi

Microsoft, Sıra depolama alanı ile yüksek performanslı uygulamalar geliştirmek için kanıtlanmış bir dizi uygulama geliştirmiştir. Bu denetim listesi, geliştiricilerin performansı en iyi duruma getirmek için izleyebilirsiniz önemli uygulamaları tanımlar. Uygulamanızı tasarlarken ve süreç boyunca bu uygulamaları aklınızda bulundurun.

Azure Depolama kapasite, işlem hızı ve bant genişliği için ölçeklenebilirlik ve performans hedefleri vardır. Azure Depolama ölçeklenebilirlik hedefleri hakkında daha fazla bilgi [için, standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) ve [Sıra depolama için Ölçeklenebilirlik ve performans hedefleri ne](scalability-targets.md)bakın.

## <a name="checklist"></a>Denetim Listesi

Bu makalede, Performans için kanıtlanmış uygulamaları, Sıra depolama uygulamanızı geliştirirken izleyebileceğiniz bir denetim listesi halinde düzenler.

| Bitti | Kategori | Tasarım değerlendirmesi |
| --- | --- | --- |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Uygulamanızı maksimum depolama hesabı sayısından daha fazla kullanacak şekilde tasarlayabilir misiniz?](#maximum-number-of-storage-accounts) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Yaklaşan kapasite ve işlem limitlerinden kaçınıyor musunuz?](#capacity-and-transaction-targets) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtlar, gereken performansı elde etmek için yeterince yüksek bant genişliğine ve düşük gecikme adabına sahip mi?](#throughput) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtların yüksek kaliteli bir ağ bağlantısı var mı?](#link-quality) |
| &nbsp; |Ağ Oluşturma |[İstemci uygulaması depolama hesabıyla aynı bölgede mi?](#location) |
| &nbsp; |Doğrudan İstemci Erişimi |[Azure Depolama'ya doğrudan erişimi etkinleştirmek için paylaşılan erişim imzalarını (SAS) ve orijinler arası kaynak paylaşımını (CORS) mi kullanıyorsunuz?](#sas-and-cors) |
| &nbsp; |.NET yapılandırması |[Optimum performans için .NET Core 2.1 veya daha sonra kullanıyor musunuz?](#use-net-core) |
| &nbsp; |.NET yapılandırması |[İstemcinizi yeterli sayıda eşzamanlı bağlantı kullanacak şekilde yapılandırıldınız mı?](#increase-default-connection-limit) |
| &nbsp; |.NET yapılandırması |[.NET uygulamaları için yeterli sayıda iş parçacığı kullanacak şekilde .NET'i yapılandırdın mı?](#increase-minimum-number-of-threads) |
| &nbsp; |Parallelism |[Paralelliğin, müşterinizin yeteneklerini aşırı yüklememeniz veya ölçeklenebilirlik hedeflerine yaklaşmamanız için uygun şekilde sınırlandırılmanızı sağladınız mı?](#unbounded-parallelism) |
| &nbsp; |Araçlar |[Microsoft tarafından sağlanan istemci kitaplıklarının ve araçlarının en son sürümlerini mi kullanıyorsunuz?](#client-libraries-and-tools) |
| &nbsp; |Yeniden deneme sayısı |[Hataları ve zaman ekmelerini azaltma için üstel geri leme içeren bir yeniden deneme ilkesi mi kullanıyorsunuz?](#timeout-and-server-busy-errors) |
| &nbsp; |Yeniden deneme sayısı |[Uygulamanız yeniden denemeyen hatalar için yeniden denemelerden kaçınıyor mu?](#non-retryable-errors) |
| &nbsp; |Yapılandırma |[Küçük isteklerin performansını artırmak için Nagle algoritmasını kapattınız mı?](#disable-nagle) |
| &nbsp; |İleti boyutu |[İletileriniz sıranın performansını artırmak için kompakt mı?](#message-size) |
| &nbsp; |Toplu alma |[Tek bir GET işleminde birden çok ileti alıyor musunuz?](#batch-retrieval) |
| &nbsp; |Yoklama sıklığı |[Başvurunuzun algılanan gecikme süresini azaltmak için yeterince sık yoklama yapıyor musunuz?](#queue-polling-interval) |
| &nbsp; |İletiyi Güncelleştir |[İletileri işlemede ilerlemeyi depolamak için İletiyi Güncelleştir işlemini kullanıyor musunuz, böylece bir hata oluşursa iletinin tamamını yeniden işlemek zorunda kalmamak için mi?](#use-update-message) |
| &nbsp; |Mimari |[Uzun süredir devam eden iş yüklerini kritik yolun dışında tutarak ve bağımsız olarak ölçeklendirerek tüm uygulamanızı daha ölçeklenebilir hale getirmek için kuyruklar mı kullanıyorsunuz?](#application-architecture) |

## <a name="scalability-targets"></a>Ölçeklenebilirlik hedefleri

Uygulamanız ölçeklenebilirlik hedeflerinden herhangi birini yaklaşır veya aşarsa, işlem gecikmesi veya azaltma da artabilir. Azure Depolama uygulamanızı daralttığında, hizmet 503 (Sunucu meşgul) veya 500 (İşlem zaman) hata kodu döndürmeye başlar. Ölçeklenebilirlik hedeflerinin sınırları içinde kalarak bu hatalardan kaçınmak, uygulamanızın performansını artırmanın önemli bir parçasıdır.

Kuyruk hizmetinin ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için Azure [Depolama ölçeklenebilirliği ve performans hedefleri'ne](/azure/storage/queues/scalability-targets#scale-targets-for-queue-storage)bakın.

### <a name="maximum-number-of-storage-accounts"></a>Maksimum depolama hesabı sayısı

Belirli bir abonelik/bölge birleşimi için izin verilen maksimum depolama hesabı sayısına yaklaşıyorsanız, girişi, çıkışını, saniyede G/Ç işlemlerini (IOPS) veya kapasiteyi artırmak için birden çok depolama hesabı mı kullanıyorsunuz? Bu senaryoda Microsoft, mümkünse iş yükünüz için gereken depolama hesabı sayısını azaltmak için depolama hesapları için artırılmış sınırlardan yararlanmanızı önerir. Depolama hesabınız için daha fazla sınır istemek için [Azure Desteği'ne](https://azure.microsoft.com/support/options/) başvurun. Daha fazla bilgi için bkz: [Daha büyük, daha yüksek ölçekli depolama hesaplarını duyur.](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/)

### <a name="capacity-and-transaction-targets"></a>Kapasite ve işlem hedefleri

Uygulamanız tek bir depolama hesabı için ölçeklenebilirlik hedeflerine yaklaşıyorsa, aşağıdaki yaklaşımlardan birini benimsemeyi düşünün:  

- Kuyrukların ölçeklenebilirlik hedefleri uygulamanız için yetersizse, birden çok kuyruk kullanın ve iletileri bunların arasında dağıtın.
- Uygulamanızın ölçeklenebilirlik hedefine yaklaşmasına veya aşılmasına neden olan iş yükünü yeniden gözden geçirin. Daha az bant genişliği veya kapasite veya daha az işlem kullanmak için farklı tasarlayabilir misiniz?
- Uygulamanız ölçeklenebilirlik hedeflerinden birini aşması gerekiyorsa, birden çok depolama hesabı oluşturun ve uygulama verilerinizi bu birden çok depolama hesabı arasında bölün. Bu deseni kullanıyorsanız, gelecekte yük dengeleme için daha fazla depolama hesabı ekleyebilmeniz için uygulamanızı tasarladığınızdan emin olun. Depolama hesaplarının, depolanan veriler, yapılan işlemler veya aktarılan veriler açısından kullanımınızdan başka bir maliyeti yoktur.
- Uygulamanız bant genişliği hedeflerine yaklaşıyorsa, verileri Azure Depolama'ya göndermek için gereken bant genişliğini azaltmak için istemci tarafında ki verileri sıkıştırmayı düşünün.
    Verileri sıkıştırmak bant genişliğini kaydedebilir ve ağ performansını artırabilir, ancak performans üzerinde olumsuz etkileri de olabilir. Müşteri tarafında veri sıkıştırma ve dekompresyon için ek işleme gereksinimlerinin performans etkisini değerlendirin. Sıkıştırılmış verileri depolamanın sorun gidermeyi zorlaştırabileceğini unutmayın, çünkü verileri standart araçları kullanarak görüntülemek daha zor olabilir.
- Uygulamanız ölçeklenebilirlik hedeflerine yaklaşıyorsa, yeniden denemeler için üstel bir geri tepme kullandığınızdan emin olun. Bu makalede açıklanan önerileri uygulayarak ölçeklenebilirlik hedeflerine ulaşmaktan kaçınmak en iyisidir. Ancak, yeniden denemeler için üstel bir geri dönüş kullanmak, uygulamanızın hızla yeniden denemesini önler ve bu da azaltmayı daha kötü hale getirebilir. Daha fazla bilgi için [Zaman Önce ve Sunucu Meşgul hataları](#timeout-and-server-busy-errors)başlıklı bölüme bakın.

## <a name="networking"></a>Ağ Oluşturma

Uygulamanın fiziksel ağ kısıtlamaları performans üzerinde önemli bir etkiye sahip olabilir. Aşağıdaki bölümlerde, kullanıcıların karşılaşabileceği bazı sınırlamalar açıklanabilir.  

### <a name="client-network-capability"></a>İstemci ağ özelliği

Bant genişliği ve ağ bağlantısının kalitesi, aşağıdaki bölümlerde açıklandığı gibi uygulama performansında önemli roller oynar.

#### <a name="throughput"></a>Aktarım hızı

Bant genişliği için sorun genellikle istemcinin yetenekleridir. Daha büyük Azure örneklerinin daha fazla kapasiteye sahip NIC'leri vardır, bu nedenle tek bir makineden daha yüksek ağ sınırlarına ihtiyacınız varsa daha büyük bir örnek veya daha fazla VM kullanmayı düşünmelisiniz. Azure Depolama'ya şirket içi bir uygulamadan erişiyorsanız, aynı kural geçerlidir: istemci aygıtının ağ özelliklerini ve Azure Depolama konumuna ağ bağlantısını anlamak ve gerektiğinde bunları geliştirmek veya tasarımı kendi yetenekleri dahilinde çalışmak için uygulama.

#### <a name="link-quality"></a>Bağlantı kalitesi

Her ağ kullanımında olduğu gibi, hatalara ve paket kaybına neden olan ağ koşullarının etkili iş verime yavaşlayacaktır.  WireShark veya NetMon kullanarak bu sorunu tanılamayardımcı olabilir.  

### <a name="location"></a>Konum

Dağıtılmış herhangi bir ortamda, istemciyi sunucunun yakınına yerleştirmek en iyi performansı sunar. Azure Depolama'ya en düşük gecikme gecikmesiyle erişmek için istemciniz için en iyi konum aynı Azure bölgesi içindedir. Örneğin, Azure Depolama'yı kullanan bir Azure web uygulamanız varsa, her ikisini de ABD Batı veya Asya Güneydoğu gibi tek bir bölgede bulun. Tek bir bölgedeki bant genişliği kullanımı ücretsiz olduğundan, kaynakların birlikte bulunması gecikme süresini ve maliyeti azaltır.  

İstemci uygulamaları Azure Depolama'ya erişecek ancak mobil aygıt uygulamaları veya şirket içi kurumsal hizmetler gibi Azure'da barındırılamıyorsa, depolama hesabını bu istemcilere yakın bir bölgede bulmak gecikme süresini azaltabilir. Müşterileriniz geniş bir şekilde dağıtılıyorsa (örneğin, bazıları Kuzey Amerika'da, bazıları Avrupa'da), bölge başına bir depolama hesabı kullanmayı düşünün. Uygulamanın depolayan verileri tek tek kullanıcılara özelse ve depolama hesapları arasında veri çoğaltmayı gerektirmeyecekse, bu yaklaşımın uygulanması daha kolaydır.

## <a name="sas-and-cors"></a>SAS ve CORS

Azure Depolama'daki verilere erişmek için kullanıcının web tarayıcısında veya bir cep telefonu uygulamasında çalışan JavaScript gibi kodları yetkilendirmeniz gerektiğini varsayalım. Bir yaklaşım, proxy gibi davranan bir hizmet uygulaması oluşturmaktır. Kullanıcının aygıtı, hizmetle ilgili olarak kimlik doğrulaması verir ve bu da Azure Depolama kaynaklarına erişimyetkisi verir. Bu şekilde, depolama hesabı anahtarlarınızı güvenli olmayan aygıtlarda açığa çıkarmaktan kaçınabilirsiniz. Ancak, kullanıcının aygıtı ile Azure Depolama arasında aktarılan tüm verilerin hizmet uygulamasından geçmesi gerektiğinden, bu yaklaşım hizmet uygulamasına önemli bir ek yük yerleştirir.

Paylaşılan erişim imzalarını (SAS) kullanarak bir hizmet uygulamasını Azure Depolama için proxy olarak kullanmaktan kaçınabilirsiniz. SAS'ı kullanarak, sınırlı bir erişim belirteci kullanarak kullanıcınızın cihazının doğrudan Azure Depolama'ya istekte bulunmasını sağlayabilirsiniz. Örneğin, bir kullanıcı uygulamanıza bir fotoğraf yüklemek isterse, hizmet uygulamanız bir SAS oluşturabilir ve kullanıcının aygıtına gönderebilir. SAS belirteci, belirli bir süre için bir Azure Depolama kaynağına yazma izni verebilir ve bundan sonra SAS belirteci sona erer. SAS hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../common/storage-sas-overview.md)ver.  

Genellikle, bir web tarayıcısı, bir etki alanında bir web sitesi tarafından barındırılan bir sayfada JavaScript'in yazma işlemleri gibi belirli işlemleri başka bir etki alanına gerçekleştirmesine izin vermez. Aynı kaynak ilkesi olarak bilinen bu ilke, bir sayfadaki kötü amaçlı bir komut dosyasının başka bir web sayfasındaki verilere erişmesini engeller. Ancak, bulutta bir çözüm oluştururken aynı kaynak ilkesi bir sınırlama olabilir. Orijinler arası kaynak paylaşımı (CORS), hedef etki alanının kaynak etki alanından kaynaklanan isteklere güvendiği tarayıcıyla iletişim kurmasını sağlayan bir tarayıcı özelliğidir.

Örneğin, Azure'da çalışan bir web uygulamasının bir Azure Depolama hesabına kaynak isteği nde bulunduğunu varsayalım. Web uygulaması kaynak etki alanıdır ve depolama hesabı hedef etki alanıdır. Kaynak etki alanından gelen isteklerin Azure Depolama tarafından güvenilen web tarayıcısına iletişim kurmak için Azure Depolama hizmetlerinden herhangi biri için CORS'ü yapılandırabilirsiniz. CORS hakkında daha fazla bilgi için [Azure Depolama için başlangıçlar arası kaynak paylaşımı (CORS) desteğine](/rest/api/storageservices/Cross-Origin-Resource-Sharing--CORS--Support-for-the-Azure-Storage-Services)bakın.  
  
Hem SAS hem de CORS, web uygulamanızda gereksiz yükleri önlemenize yardımcı olabilir.  

## <a name="net-configuration"></a>.NET yapılandırması

.NET Framework kullanıyorsanız, bu bölümde önemli performans iyileştirmeleri yapmak için kullanabileceğiniz birkaç hızlı yapılandırma ayarı listelenir.  Diğer dilleri kullanıyorsanız, seçtiğiniz dilde benzer kavramların geçerli olup olmadığını kontrol edin.  

### <a name="use-net-core"></a>.NET Core kullanın

Performans geliştirmelerinden yararlanmak için Azure Depolama uygulamalarınızı .NET Core 2.1 veya sonraki lerle geliştirin. Mümkün olduğunda .NET Core 3.x kullanılması önerilir.

.NET Core'daki performans iyileştirmeleri hakkında daha fazla bilgi için aşağıdaki blog gönderilerine bakın:

- [.NET Core 3.0'da Performans Geliştirmeleri](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-3-0/)
- [.NET Core 2.1'de Performans Geliştirmeleri](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-2-1/)

### <a name="increase-default-connection-limit"></a>Varsayılan bağlantı sınırını artırma

.NET'te, aşağıdaki kod varsayılan bağlantı sınırını (istemci ortamında genellikle 2 veya sunucu ortamında 10 olan) 100'e yükseltir. Genellikle, değeri uygulamanız tarafından kullanılan iş parçacığı sayısına ayarlamanız gerekir.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Bağlantıları açmadan önce bağlantı sınırını ayarlayın.  

Diğer programlama dilleri için bağlantı sınırının nasıl ayarlan da belirlendiğini belirlemek için o dilin belgelerine bakın.  

Daha fazla bilgi için [web](https://blogs.msdn.microsoft.com/darrenj/2005/03/07/web-services-concurrent-connections/)hizmetleri: Eşzamanlı Bağlantılar adlı blog gönderisini görün.  

### <a name="increase-minimum-number-of-threads"></a>En az iş parçacığı sayısını artırma

Eşzamanlı aramaları eşzamanlı görevlerle birlikte kullanıyorsanız, iş parçacığı havuzundaki iş parçacığı sayısını artırmak isteyebilirsiniz:

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Daha fazla bilgi için [ThreadPool.SetMinThreads](/dotnet/api/system.threading.threadpool.setminthreads) yöntemine bakın.  

## <a name="unbounded-parallelism"></a>Sınırsız paralellik

Paralellik performans için harika olsa da, sınırlanmamış paralellik kullanırken dikkatli olun, yani iş parçacığı veya paralel istek sayısına uygulanan bir sınır yoktur. Veri yüklemek veya indirmek, aynı depolama hesabında birden çok bölüme erişmek veya aynı bölümdeki birden çok öğeye erişmek için paralel istekleri sınırlamayı unutmayın. Paralellik sınırlandırılmamışsa, uygulamanız istemci aygıtının yeteneklerini veya depolama hesabının ölçeklenebilirlik hedeflerini aşarak daha uzun gecikmeler ve daralmayla sonuçlanabilir.  

## <a name="client-libraries-and-tools"></a>İstemci kitaplıkları ve araçları

En iyi performans için her zaman Microsoft tarafından sağlanan en son istemci kitaplıklarını ve araçlarını kullanın. Azure Depolama istemci kitaplıkları çeşitli diller için kullanılabilir. Azure Depolama, PowerShell ve Azure CLI'yi de destekler. Microsoft, bu istemci kitaplıklarını ve araçlarını performans göz önünde bulundurarak etkin bir şekilde geliştirir, en son hizmet sürümleriyle güncel tutar ve kanıtlanmış performans uygulamalarının çoğunu dahili olarak ele almalarını sağlar. Daha fazla bilgi için [Azure Depolama başvuru belgelerine](/azure/storage/#reference)bakın.

## <a name="handle-service-errors"></a>Hizmet hatalarını işleme

Hizmet bir isteği işleyemediğinde Azure Depolama bir hata döndürür. Belirli bir senaryoda Azure Depolama tarafından döndürülebilecek hataları anlamak, performansı optimize etmek için yararlıdır.

### <a name="timeout-and-server-busy-errors"></a>Zaman Ara ve Sunucu Meşgul hataları

Azure Depolama, ölçeklenebilirlik sınırlarına yaklaşırsa uygulamanızı azaltabilir. Bazı durumlarda, Azure Depolama bazı geçici koşullar nedeniyle bir isteği işleyemeyebilir. Her iki durumda da, hizmet 503 (Sunucu Meşgul) veya 500 (Timeout) hatası döndürebilir. Hizmet, daha yüksek iş elde edilebilmesi için veri bölümlerini yeniden dengeliyorsa, bu hatalar da oluşabilir. İstemci uygulaması genellikle bu hatalardan birine neden olan işlemi yeniden denemelidir. Ancak, Azure Depolama ölçeklenebilirlik hedeflerini aştığı için uygulamanızı daraltıyorsa veya hizmet isteği başka bir nedenle yanıtveremese bile, agresif yeniden denemeler sorunu daha da kötüleştirebilir. Üstel geri leme ilkesini kullanman önerilir ve istemci kitaplıkları bu davranış için varsayılan olarak. Örneğin, uygulamanız 2 saniye, sonra 4 saniye, sonra 10 saniye, sonra 30 saniye sonra yeniden deneyebilir ve sonra tamamen vazgeçebilir. Bu şekilde, uygulamanız, daralmaya yol açabilecek davranışları şiddetlendirmek yerine hizmet üzerindeki yükünü önemli ölçüde azaltır.  

Bağlantı hataları, azaltmanın sonucu olmadığından ve geçici olması beklendiğiiçin hemen yeniden denenebilir.  

### <a name="non-retryable-errors"></a>Yeniden denemeyen hatalar

İstemci kitaplıkları, hangi hataların yeniden denenebileceğini ve hangilerinin yapamayacağına farkında olarak yeniden denemeleri yönetir. Ancak, Azure Depolama REST API'sini doğrudan arıyorsanız, yeniden denememeniz gereken bazı hatalar vardır. Örneğin, 400 (Kötü İstek) hatası, istemci uygulamasının beklenen formda olmadığı için işlenmeyen bir istek gönderdiğini gösterir. Bu isteği yeniden göndermek her seferinde aynı yanıtı verir, bu nedenle yeniden denemenin bir anlamı yoktur. Azure Depolama REST API'sini doğrudan arıyorsanız, olası hatalar ve bunların yeniden denenip denenmemesi gerektiğini unutmayın.

Azure Depolama hata kodları hakkında daha fazla bilgi için [Durum ve hata kodlarına](/rest/api/storageservices/status-and-error-codes2)bakın.

## <a name="disable-nagle"></a>Nagle'ı devre dışı

Nagle algoritması yaygın ağ performansını artırmak için bir araç olarak TCP / IP ağları arasında uygulanır. Ancak, her koşulda (son derece etkileşimli ortamlar gibi) en uygun değildir. Nagle algoritması, Azure Tablo hizmetine isteklerin performansı üzerinde olumsuz bir etkiye sahiptir ve mümkünse devre dışı bırakmanız gerekir.

## <a name="message-size"></a>İleti boyutu

İleti boyutu arttıkça sıra performansı ve ölçeklenebilirlik azalır. Yalnızca alıcının ihtiyacı olan bilgileri bir iletiye koyun.  

## <a name="batch-retrieval"></a>Toplu alma

Tek bir işlemde bir kuyruktan en fazla 32 ileti alabilirsiniz. Toplu işlem, özellikle mobil cihazlar gibi yüksek gecikme süresi olan ortamlar için kullanışlı olan istemci uygulamasından gelen gidiş dönüş sayısını azaltabilir.  

## <a name="queue-polling-interval"></a>Sıra yoklama aralığı

Çoğu uygulama, bu uygulama için en büyük hareket kaynaklarından biri olabilecek bir kuyruktaki iletileri yoklar. Yoklama aralığınızı akıllıca seçin: çok sık yoklama, uygulamanızın kuyruk için ölçeklenebilirlik hedeflerine yaklaşmasına neden olabilir. Ancak, 0,01 $ (yazma sırasında) için 200.000 işlemleri, tek bir işlemci bir ay boyunca her saniye yoklama daha az mal olacak 15 sent maliyet genellikle yoklama aralığı seçiminizi etkileyen bir faktör değildir.  

Güncel maliyet bilgileri için Azure [Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.  

## <a name="use-update-message"></a>İletiyi Güncelle'yi Kullan

Görünmezlik zaman anına artırmak veya iletinin durum bilgilerini güncelleştirmek için **İletiyi Güncelleştir** işlemini kullanabilirsiniz. **İletiyi Güncelleştir'i** kullanmak, işin her adımı tamamlandığında, bir işi bir kuyruktan diğerine geçiren bir iş akışına sahip olmaktan daha verimli bir yaklaşım olabilir. Uygulamanız, bir adım tamamlandığında işin bir sonraki adımı için iletiyi yeniden sıralamak yerine iş durumunu iletiye kaydedebilir ve ardından çalışmaya devam edebilir. Her **Güncelleştirme İletisi** işleminin ölçeklenebilirlik hedefine doğru sayıldığını unutmayın.

## <a name="application-architecture"></a>Uygulama mimarisi

Uygulama mimarinizi ölçeklenebilir hale getirmek için kuyrukları kullanın. Aşağıdaki, uygulamanızı daha ölçeklenebilir hale getirmek için kuyrukları kullanabileceğiniz bazı yolları listeler:  

- Uygulamanızda iş yüklerini işlemek ve iş yüklerini düzeltmek için biriş biriktirme listesi oluşturmak için kuyrukları kullanabilirsiniz. Örneğin, yüklenen görüntüleri yeniden boyutlandırma gibi işlemci yoğun çalışmayı gerçekleştirmek için kullanıcılardan gelen istekleri sıraya alabilirsiniz.
- Bağımsız ölçeklendirmek için uygulamanızın bölümlerini ayırmak için kuyrukları kullanabilirsiniz. Örneğin, bir web ön uç daha sonra çözümleme ve depolama için bir kuyruğa kullanıcıların anket sonuçları yerleştirebilir. Sıra verilerini gerektiği gibi işlemek için daha fazla alt rol örneği ekleyebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

- [Sıra depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)
- [Standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
- [Durum ve hata kodları](/rest/api/storageservices/Status-and-Error-Codes2)
