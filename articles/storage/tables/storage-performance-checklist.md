---
title: Tablo depolama için performans ve ölçeklenebilirlik denetim listesi - Azure Depolama
description: Yüksek performanslı uygulamalar geliştirmede Tablo depolama ile kullanılmak üzere kanıtlanmış uygulamaların bir kontrol listesi.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 10/10/2019
ms.author: tamram
ms.subservice: tables
ms.openlocfilehash: 89581c8ae2fbdbb55a2abfbd527c8fdcf4b65761
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75749557"
---
# <a name="performance-and-scalability-checklist-for-table-storage"></a>Tablo depolama için performans ve ölçeklenebilirlik denetim listesi

Microsoft, Table depolama ile yüksek performanslı uygulamalar geliştirmek için kanıtlanmış bir dizi uygulama geliştirmiştir. Bu denetim listesi, geliştiricilerin performansı en iyi duruma getirmek için izleyebilirsiniz önemli uygulamaları tanımlar. Uygulamanızı tasarlarken ve süreç boyunca bu uygulamaları aklınızda bulundurun.

Azure Depolama kapasite, işlem hızı ve bant genişliği için ölçeklenebilirlik ve performans hedefleri vardır. Azure Depolama ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ve Tablo depolama için [Ölçeklenebilirlik ve performans hedefleri ne](scalability-targets.md)bakın.

## <a name="checklist"></a>Denetim Listesi

Bu makalede, Tablo depolama uygulamanızı geliştirirken izleyebileceğiniz bir denetim listesinde performans için kanıtlanmış uygulamaları düzenler.

| Bitti | Kategori | Tasarım değerlendirmesi |
| --- | --- | --- |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Uygulamanızı maksimum depolama hesabı sayısından daha fazla kullanacak şekilde tasarlayabilir misiniz?](#maximum-number-of-storage-accounts) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Yaklaşan kapasite ve işlem limitlerinden kaçınıyor musunuz?](#capacity-and-transaction-targets) |
| &nbsp; |Ölçeklenebilirlik hedefleri |[Varlıklar için ölçeklenebilirlik hedeflerine saniyede mi yaklaşıyorsunuz?](#targets-for-data-operations) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtlar, gereken performansı elde etmek için yeterince yüksek bant genişliğine ve düşük gecikme adabına sahip mi?](#throughput) |
| &nbsp; |Ağ Oluşturma |[İstemci tarafındaki aygıtların yüksek kaliteli bir ağ bağlantısı var mı?](#link-quality) |
| &nbsp; |Ağ Oluşturma |[İstemci uygulaması depolama hesabıyla aynı bölgede mi?](#location) |
| &nbsp; |Doğrudan İstemci Erişimi |[Azure Depolama'ya doğrudan erişimi etkinleştirmek için paylaşılan erişim imzalarını (SAS) ve orijinler arası kaynak paylaşımını (CORS) mi kullanıyorsunuz?](#sas-and-cors) |
| &nbsp; |Toplu İşleme |[Uygulamanız, varlık grubu hareketlerini kullanarak güncelleştirmeleri toplu olarak mı yapıyor?](#batch-transactions) |
| &nbsp; |.NET yapılandırması |[Optimum performans için .NET Core 2.1 veya daha sonra kullanıyor musunuz?](#use-net-core) |
| &nbsp; |.NET yapılandırması |[İstemcinizi yeterli sayıda eşzamanlı bağlantı kullanacak şekilde yapılandırıldınız mı?](#increase-default-connection-limit) |
| &nbsp; |.NET yapılandırması |[.NET uygulamaları için yeterli sayıda iş parçacığı kullanacak şekilde .NET'i yapılandırdın mı?](#increase-minimum-number-of-threads) |
| &nbsp; |Parallelism |[Paralelliğin, müşterinizin yeteneklerini aşırı yüklememeniz veya ölçeklenebilirlik hedeflerine yaklaşmamanız için uygun şekilde sınırlandırılmanızı sağladınız mı?](#unbounded-parallelism) |
| &nbsp; |Araçlar |[Microsoft tarafından sağlanan istemci kitaplıklarının ve araçlarının en son sürümlerini mi kullanıyorsunuz?](#client-libraries-and-tools) |
| &nbsp; |Yeniden deneme sayısı |[Hataları ve zaman ekmelerini azaltma için üstel geri leme içeren bir yeniden deneme ilkesi mi kullanıyorsunuz?](#timeout-and-server-busy-errors) |
| &nbsp; |Yeniden deneme sayısı |[Uygulamanız yeniden denemeyen hatalar için yeniden denemelerden kaçınıyor mu?](#non-retryable-errors) |
| &nbsp; |Yapılandırma |[Masa istekleriniz için JSON kullanıyor musunuz?](#use-json) |
| &nbsp; |Yapılandırma |[Küçük isteklerin performansını artırmak için Nagle algoritmasını kapattınız mı?](#disable-nagle) |
| &nbsp; |Tablolar ve bölümler |[Verilerinizi düzgün bir şekilde bölümlediniz mi?](#schema) |
| &nbsp; |Sıcak bölümler |[Yalnızca ek ve prepend desenlerinden kaçınıyor musunuz?](#append-only-and-prepend-only-patterns) |
| &nbsp; |Sıcak bölümler |[Eklerin/güncelleştirmeleriniz birçok bölüme mi yayılıyor?](#high-traffic-data) |
| &nbsp; |Sorgu kapsamı |[Şemanızı, çoğu durumda nokta sorgularının ve tablo sorgularının seyrek kullanılmasına izin verecek şekilde tasarladınız mı?](#query-scope) |
| &nbsp; |Sorgu yoğunluğu |[Sorgularınız genellikle yalnızca uygulamanızın kullanacağı satırları tarayıp iade ediyor mu?](#query-density) |
| &nbsp; |Döndürülen verileri sınırlama |[Gerekli olmayan varlıkları döndürmekten kaçınmak için filtreleme kullanıyor musunuz?](#limiting-the-amount-of-data-returned) |
| &nbsp; |Döndürülen verileri sınırlama |[Gerekli olmayan özellikleri niçin döndürmekten kaçınmak için projeksiyon uyguluyor musunuz?](#limiting-the-amount-of-data-returned) |
| &nbsp; |Denormalizasyon |[Veri almaya çalışırken verimsiz sorgulardan veya birden çok okuma isteğinden kaçınmak için verilerinizi normalden arındırdınız mı?](#denormalization) |
| &nbsp; |Ekleme, güncelleme ve silme |[İşlemsel olması gereken veya gidiş-dönüşleri azaltmak için aynı anda yapılabilecek istekleri toplu olarak mı yapıyorsunuz?](#batching) |
| &nbsp; |Ekleme, güncelleme ve silme |[Ekle veya güncelleştirmeyi çağırıp çağırmayacağınıbelirlemek için bir varlığı almaktan kaçınıyor musunuz?](#upsert) |
| &nbsp; |Ekleme, güncelleme ve silme |[Birden çok varlık yerine özellik olarak sık sık tek bir varlıkta birlikte alınacak veri serilerini depolamayı düşündünmü?](#storing-data-series-in-a-single-entity) |
| &nbsp; |Ekleme, güncelleme ve silme |[Her zaman birlikte alınabilecek ve toplu olarak yazılabilecek varlıklar için (örneğin, zaman serisi verileri), tablolar yerine blobkullanmayı düşündüniz mi?](#storing-structured-data-in-blobs) |

## <a name="scalability-targets"></a>Ölçeklenebilirlik hedefleri

Uygulamanız ölçeklenebilirlik hedeflerinden herhangi birini yaklaşır veya aşarsa, işlem gecikmesi veya azaltma da artabilir. Azure Depolama uygulamanızı daralttığında, hizmet 503 (Sunucu meşgul) veya 500 (İşlem zaman) hata kodu döndürmeye başlar. Ölçeklenebilirlik hedeflerinin sınırları içinde kalarak bu hatalardan kaçınmak, uygulamanızın performansını artırmanın önemli bir parçasıdır.

Tablo hizmeti için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için Tablo [depolama için ölçeklenebilirlik ve performans hedeflerine](scalability-targets.md)bakın.

### <a name="maximum-number-of-storage-accounts"></a>Maksimum depolama hesabı sayısı

Belirli bir abonelik/bölge birleşimi için izin verilen maksimum depolama hesabı sayısına yaklaşıyorsanız, girişi, çıkışını, saniyede G/Ç işlemlerini (IOPS) veya kapasiteyi artırmak için birden çok depolama hesabı mı kullanıyorsunuz? Bu senaryoda Microsoft, mümkünse iş yükünüz için gereken depolama hesabı sayısını azaltmak için depolama hesapları için artırılmış sınırlardan yararlanmanızı önerir. Depolama hesabınız için daha fazla sınır istemek için [Azure Desteği'ne](https://azure.microsoft.com/support/options/) başvurun. Daha fazla bilgi için bkz: [Daha büyük, daha yüksek ölçekli depolama hesaplarını duyur.](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/)

### <a name="capacity-and-transaction-targets"></a>Kapasite ve işlem hedefleri

Uygulamanız tek bir depolama hesabı için ölçeklenebilirlik hedeflerine yaklaşıyorsa, aşağıdaki yaklaşımlardan birini benimsemeyi düşünün:  

- Uygulamanızın ölçeklenebilirlik hedefine yaklaşmasına veya aşılmasına neden olan iş yükünü yeniden gözden geçirin. Daha az bant genişliği veya kapasite veya daha az işlem kullanmak için farklı tasarlayabilir misiniz?
- Uygulamanız ölçeklenebilirlik hedeflerinden birini aşması gerekiyorsa, birden çok depolama hesabı oluşturun ve uygulama verilerinizi bu birden çok depolama hesabı arasında bölün. Bu deseni kullanıyorsanız, gelecekte yük dengeleme için daha fazla depolama hesabı ekleyebilmeniz için uygulamanızı tasarladığınızdan emin olun. Depolama hesaplarının, depolanan veriler, yapılan işlemler veya aktarılan veriler açısından kullanımınızdan başka bir maliyeti yoktur.
- Uygulamanız bant genişliği hedeflerine yaklaşıyorsa, verileri Azure Depolama'ya göndermek için gereken bant genişliğini azaltmak için istemci tarafında ki verileri sıkıştırmayı düşünün.
    Verileri sıkıştırmak bant genişliğini kaydedebilir ve ağ performansını artırabilir, ancak performans üzerinde olumsuz etkileri de olabilir. Müşteri tarafında veri sıkıştırma ve dekompresyon için ek işleme gereksinimlerinin performans etkisini değerlendirin. Sıkıştırılmış verileri depolamanın sorun gidermeyi zorlaştırabileceğini unutmayın, çünkü verileri standart araçları kullanarak görüntülemek daha zor olabilir.
- Uygulamanız ölçeklenebilirlik hedeflerine yaklaşıyorsa, yeniden denemeler için üstel bir geri tepme kullandığınızdan emin olun. Bu makalede açıklanan önerileri uygulayarak ölçeklenebilirlik hedeflerine ulaşmaktan kaçınmak en iyisidir. Ancak, yeniden denemeler için üstel bir geri dönüş kullanmak, uygulamanızın hızla yeniden denemesini önler ve bu da azaltmayı daha kötü hale getirebilir. Daha fazla bilgi için [Zaman Önce ve Sunucu Meşgul hataları](#timeout-and-server-busy-errors)başlıklı bölüme bakın.

### <a name="targets-for-data-operations"></a>Veri işlemleri için hedefler

Azure Depolama yük bakiyeleri depolama hesabınıza olan trafik arttıkça dengelenir, ancak trafik ani patlamalar sergilerse, bu iş hacmini hemen alamayabilirsiniz. Azure Depolama tablonuzu otomatik olarak yüklerken, patlama sırasında azaltma ve/veya zaman alamaları görmeyi bekleyin. Yavaş yavaş rampa lama genellikle daha iyi sonuçlar sağlar, çünkü sistemin dengeyi uygun şekilde yüklemek için zamanı vardır.

#### <a name="entities-per-second-storage-account"></a>Saniyedeki varlıklar (depolama hesabı)

Tablolara erişmek için ölçeklenebilirlik sınırı, bir hesap için saniyede 20.000 varlık (her biri 1 KB) kadardır. Genel olarak, eklenen, güncelleştirilen, silinen veya taranan her varlık bu hedefe doğru sayılır. Yani 100 varlık içeren bir toplu iş eklemek 100 varlık olarak sayılır. 1000 varlığı tarayan ve 5 döndüren bir sorgu 1000 varlık olarak sayılır.

#### <a name="entities-per-second-partition"></a>Saniyebaşına varlıklar (bölüm)

Tek bir bölüm içinde, tablolara erişmek için ölçeklenebilirlik hedefi, önceki bölümde açıklandığı gibi aynı saymayı kullanarak saniyede 2.000 varlıktır (her biri 1 KB).

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

## <a name="batch-transactions"></a>Toplu işlemler

Tablo hizmeti, aynı tabloda bulunan ve aynı bölüm grubuna ait varlıklardaki toplu işlemleri destekler. Daha fazla bilgi için [bkz.](/rest/api/storageservices/performing-entity-group-transactions)

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

## <a name="configuration"></a>Yapılandırma

Bu bölümde, Tablo hizmetinde önemli performans iyileştirmeleri yapmak için kullanabileceğiniz birkaç hızlı yapılandırma ayarı listelenir:

### <a name="use-json"></a>JSON kullanın

Depolama hizmeti sürümü 2013-08-15'ten başlayarak Tablo hizmeti, tablo verilerini aktarmak için XML tabanlı AtomPub biçimi yerine JSON'un kullanılmasını destekler. JSON kullanmak, taşıma kapasitesini %75'e kadar azaltabilir ve uygulamanızın performansını önemli ölçüde artırabilir.

Daha fazla bilgi için Microsoft Azure Tabloları sonrası: [Tablo Hizmeti İşlemleri için](https://msdn.microsoft.com/library/azure/dn535600.aspx)JSON ve Yük Biçimi nin [Tanıtılması'](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) na bakın.

### <a name="disable-nagle"></a>Nagle'ı devre dışı

Nagle algoritması yaygın ağ performansını artırmak için bir araç olarak TCP / IP ağları arasında uygulanır. Ancak, her koşulda (son derece etkileşimli ortamlar gibi) en uygun değildir. Nagle algoritması, Azure Tablo hizmetine isteklerin performansı üzerinde olumsuz bir etkiye sahiptir ve mümkünse devre dışı bırakmanız gerekir.

## <a name="schema"></a>Şema

Verilerinizi nasıl temsil ettiğiniz ve sorguladığınız Tablo hizmetinin performansını etkileyen en büyük tek etkendir. Her uygulama farklı olsa da, bu bölümde ilgili bazı genel kanıtlanmış uygulamalar özetlenir:

- Tablo tasarımı
- Verimli sorgular
- Verimli veri güncellemeleri

### <a name="tables-and-partitions"></a>Tablolar ve bölümler

Tablolar bölümlere ayrılır. Bir bölümde depolanan her varlık aynı bölüm anahtarını paylaşır ve bu bölüm içinde tanımlamak için benzersiz bir satır anahtarına sahiptir. Bölümler yarar sağlar, ancak ölçeklenebilirlik sınırları da sunar.

- Avantajlar: 100'e kadar ayrı depolama işlemi (toplam 4 MB sınırı) içeren tek bir atomik, toplu işlemde aynı bölümdeki varlıkları güncelleştirebilirsiniz. Aynı sayıda varlığın alınabileceğini varsayarsak, tek bir bölümdeki verileri, bölümleri kapsayan verilerden daha verimli bir şekilde sorgulayabilirsiniz (ancak tablo verilerini sorgulama yla ilgili diğer öneriler için okumaya devam edebilirsiniz).
- Ölçeklenebilirlik sınırı: Bölümler atomik toplu iş hareketlerini desteklediğinden, tek bir bölümde depolanan varlıklara erişim yük dengelenemez. Bu nedenle, tek bir tablo bölümü için ölçeklenebilirlik hedefi bir bütün olarak Tablo hizmeti için daha düşüktür.

Tabloların ve bölümlerin bu özellikleri nedeniyle, aşağıdaki tasarım ilkelerini benimsemeniz gerekir:

- İstemci uygulamanızın sık sık güncelediği veya aynı mantıksal çalışma biriminde sorguladığı verileri aynı bölümde bulun. Örneğin, uygulamanız yazmaları topluleştiriyorsa veya atomik toplu işlem gerçekleştiriyorsanız, verileri aynı bölümde bulun. Ayrıca, tek bir bölümdeki veriler, tek bir sorguda bölümler arasında verilerden daha verimli bir şekilde sorgulanabilir.
- İstemci uygulamanızın aynı mantıksal çalışma biriminde (yani tek bir sorguveya toplu iş güncelleştirmesinde) eklemediği, güncelleştirmediği veya sorgulamayan verileri ayrı bölümlerde bulun. Tek bir tablodaki bölüm anahtarlarının sayısıiçin bir sınır olmadığını, bu nedenle milyonlarca bölüm anahtarına sahip olmanın bir sorun olmadığını ve performansı etkilemeyeceğini unutmayın. Örneğin, uygulamanız kullanıcı girişi ile popüler bir web sitesiyse, Kullanıcı Kimliğini bölüm anahtarı olarak kullanmak iyi bir seçim olabilir.

#### <a name="hot-partitions"></a>Sıcak bölümler

Sıcak bölüm, bir hesaba trafiğin orantısız bir yüzdesini alan ve tek bir bölüm olduğu için yük dengeli bir bölüm değildir biridir. Genel olarak, sıcak bölümler iki şekilde oluşturulur:

#### <a name="append-only-and-prepend-only-patterns"></a>Yalnızca Ek ve Prepend Yalnızca desenleri

"Yalnızca Ek" deseni, belirli bir bölüm anahtarına trafiğin tamamının (veya neredeyse tamamının) geçerli saate göre arttığı ve azaldığı bir desendir. Örneğin, uygulamanızın günlük verileri için geçerli tarihi bir bölüm anahtarı olarak kullandığını varsayalım. Bu tasarım, tablonuzdaki son bölüme giden tüm eklerin sonuçlanmasına neden oluyor ve sistem dengeyi düzgün şekilde yükleyemez. Bu bölüme gelen trafiğin hacmi bölüm düzeyinde ölçeklenebilirlik hedefini aşarsa, daraltma ile sonuçlanır. İstekleri tablonuzdaki yük dengesini sağlamak için trafiğin birden çok bölüme gönderilmesini sağlamak daha iyidir.

#### <a name="high-traffic-data"></a>Yüksek trafik verileri

Bölümleme düzeniniz, yalnızca diğer bölümlere göre çok daha fazla kullanılan verilere sahip tek bir bölümle sonuçlanırsa, bu bölüm tek bir bölüm için ölçeklenebilirlik hedefine yaklaştıkça azaltmayı da görebilirsiniz. Bölüm şemanIzin ölçeklenebilirlik hedeflerine yaklaşan tek bir bölümle sonuçlanmadığından emin olmak daha iyidir.

### <a name="querying"></a>Sorgulama

Bu bölümde, Tablo hizmetini sorgulamak için kanıtlanmış uygulamalar açıklanmaktadır.

#### <a name="query-scope"></a>Sorgu kapsamı

Sorgulanacak varlıkların aralığını belirtmenin birkaç yolu vardır. Aşağıdaki liste, sorgu kapsamı için her seçeneği açıklar.

- **Nokta sorguları:**- Bir nokta sorgusu, allanacak varlığın hem bölüm anahtarını hem de satır anahtarını belirterek tam olarak bir varlık alır. Bu sorgular verimlidir ve bunları mümkün olan her yerde kullanmalısınız.
- **Bölüm sorguları:** Bölüm sorgusu, ortak bir bölüm anahtarını paylaşan bir veri kümesini alan bir sorgudur. Genellikle, sorgu bir bölüm anahtarına ek olarak bazı varlık özellikleri için bir dizi satır anahtar değerleri veya bir dizi değer belirtir. Bu sorgular nokta sorgularından daha az verimlidir ve dikkatli kullanılmalıdır.
- **Tablo sorguları:** Tablo sorgusu, ortak bir bölüm anahtarını paylaşmayan bir varlık kümesini alan bir sorgudur. Bu sorgular verimli değildir ve mümkünse bunlardan kaçınmalısınız.

Genel olarak, taramalardan kaçının (tek bir varlıktan daha büyük sorgular), ancak tarama yapmanız gerekiyorsa, taramalarınızın ihtiyacınız olan verileri taramadan veya önemli miktarda varlık döndürmeden geri alması için verilerinizi düzenlemeyi deneyin.

#### <a name="query-density"></a>Sorgu yoğunluğu

Sorgu verimliliğindeki bir diğer önemli faktör, döndürülen kümeyi bulmak için taranmış varlık sayısıyla karşılaştırıldığında döndürülen varlık sayısıdır. Uygulamanız, verilerin yalnızca %1'inin paylaştığı bir özellik değeri için filtreli bir tablo sorgusu gerçekleştirirse, sorgu döndürdeceği her varlık için 100 varlığı tarar. Daha önce tartışılan tablo ölçeklenebilirlik hedefleri, taranmış varlıkların sayısıyla ilgilidir ve döndürülen varlık sayısıyla ilgilidir: düşük sorgu yoğunluğu tablo hizmetinin uygulamanızı kolayca azaltmasına neden olabilir, çünkü bu kadar çok varlığı tarayabilmesi gerekir aradığınız varlığı geri alın. Azaltmayı nasıl önleyene ilgili daha fazla bilgi için, [Denormalization](#denormalization)başlıklı bölüme bakın.

#### <a name="limiting-the-amount-of-data-returned"></a>Döndürülen veri miktarını sınırlama

Bir sorgunun istemci uygulamasında gereksinim duymadığınız varlıkları döndüreceğini biliyorsanız, döndürülen kümenin boyutunu azaltmak için bir filtre kullanmayı düşünün. İstemciye döndürülmeyen varlıklar hala ölçeklenebilirlik sınırlarına doğru sayılırken, azalan ağ yükü boyutu ve istemci uygulamanızın işlemesi gereken varlıkların sayısının azalması nedeniyle uygulama performansınız artar. Ölçeklenebilirlik hedeflerinin taranmış varlıkların sayısıyla ilişkili olduğunu unutmayın, bu nedenle birçok varlığı filtreleyen bir sorgu, birkaç varlık döndürülse bile yine de daralmaya neden olabilir. Sorguları verimli hale getirme hakkında daha fazla bilgi için [Sorgu yoğunluğu](#query-density)başlıklı bölüme bakın.

İstemci uygulamanız tablonuzdaki varlıklardan yalnızca sınırlı bir özellik kümesine ihtiyaç duyarsa, döndürülen veri kümesinin boyutunu sınırlamak için projeksiyonu kullanabilirsiniz. Filtrelemede olduğu gibi, projeksiyon ağ yükünü ve istemci işlemeyi azaltmaya yardımcı olur.

#### <a name="denormalization"></a>Denormalizasyon

İlişkisel veritabanları ile çalışmanın aksine, tablo verilerini verimli bir şekilde sorgulamak için kanıtlanmış uygulamalar verilerinizin normalden silinmesine yol açar. Diğer bir deyişle, bir sorgunun, verileri bulmak için çok sayıda varlığı taramaya ihtiyaç etmek yerine, istemcinin ihtiyacı olan verileri bulmak için taradığı varlıkların sayısını en aza indirmek için birden çok varlıkta (verileri bulmak için kullanabileceğiniz her anahtar için bir anahtar) aynı verileri çoğaltmak uygulama ihtiyaçları. Örneğin, bir e-ticaret web sitesinde, hem müşteri kimliğine göre (bana bu müşterinin siparişlerini verin) hem de tarihe göre (bana bir tarihte sipariş verin) bir sipariş bulmak isteyebilirsiniz. Tablo Depolama'da, varlığı (veya ona atıfta bulunmayı) bir kez Tablo Adı, PK ve RK ile iki kez depolamak, müşteri kimliğiyle bulmayı kolaylaştırmak, bir kez de tarihe kadar bulmayı kolaylaştırmak en iyisidir.  

### <a name="insert-update-and-delete"></a>Ekleme, güncelleme ve silme

Bu bölümde, Tablo hizmetinde depolanan varlıkları değiştirmek için kanıtlanmış uygulamalar açıklanmaktadır.  

#### <a name="batching"></a>Toplu İşleme

Toplu işlemler, Azure Depolama'da varlık grubu hareketleri olarak bilinir. Varlık grubu hareketi içindeki tüm işlemler tek bir tabloda tek bir bölüm üzerinde olmalıdır. Mümkün olduğunda, eklemeleri, güncelleştirmeleri ve silmeleri toplu olarak gerçekleştirmek için varlık grubu hareketlerini kullanın. Varlık grubu hareketlerini kullanmak istemci uygulamanızdan sunucuya gidiş dönüş sayısını azaltır, faturalandırılabilir işlem sayısını azaltır (varlık grubu hareketi faturalandırma amacıyla tek bir işlem olarak sayılır ve 100'e kadar depolama işlemleri) ve atomik güncelleştirmeler sağlar (tüm işlemler bir varlık grubu hareketi içinde başarılı veya başarısız). Mobil cihazlar gibi yüksek gecikmelere sahip ortamlar, varlık grubu hareketlerini kullanmaktan büyük ölçüde yararlanacaktır.  

#### <a name="upsert"></a>Upsert

Mümkün olan her yerde tablo **Upsert** işlemlerini kullanın. Her ikisi de geleneksel **Bir Ekle** ve **Güncelleştir** işlemlerinden daha verimli olabilecek iki tür **Upsert**vardır:  

- **InsertOrMerge**: Varlığın özelliklerinin bir alt kümesini yüklemek istediğinizde, ancak varlığın zaten var olup olmadığından emin değilseniz bu işlemi kullanın. Varlık varsa, bu çağrı **Upsert** işleminde yer alan özellikleri güncelleştirir ve varolan tüm özellikleri olduğu gibi bırakır, varlık yoksa, yeni varlığı ekler. Bu, yalnızca değişen özellikleri yüklemeniz gereken bir sorgudaki projeksiyonu kullanmaya benzer.
- **InsertOrReplace**: Tamamen yeni bir varlık yüklemek istediğinizde bu işlemi kullanın, ancak zaten var olup olmadığından emin değilsiniz. Yeni yüklenen varlığın tamamen doğru olduğunu bildiğinizde bu işlemi kullanın, çünkü eski varlığın tamamen üzerine yazar. Örneğin, uygulamanın kullanıcı için daha önce konum verilerini depolayıp depolamadığına bakılmaksızın, kullanıcının geçerli konumunu depolayan varlığı güncelleştirmek istiyorsunuz; yeni konum varlığı tamamlandı ve önceki herhangi bir varlıktan herhangi bir bilgiye ihtiyacınız yok.

#### <a name="storing-data-series-in-a-single-entity"></a>Veri serilerini tek bir varlıkta depolama

Bazen, bir uygulama sık sık aynı anda almak için gereken bir veri dizisi depolar: örneğin, bir uygulama son 24 saat veri yuvarlanma grafiği çizmek için zaman içinde CPU kullanımını izleyebilir. Bir yaklaşım, her varlığın belirli bir saati temsil ettiği ve cpu kullanımını o saat için depolayan saat başına bir tablo varlığına sahip olmaktır. Bu verileri çizmek için, uygulamanın en son 24 saatiçinde verileri tutan varlıkları alması gerekir.  

Alternatif olarak, uygulamanız CPU kullanımını her saat için tek bir varlığın ayrı bir özelliği olarak depolayabilir: her saati güncelleştirmek için, uygulamanız en son saat için değeri güncelleştirmek için tek bir **InsertOrMerge Upsert** çağrısı kullanabilir. Verileri çizmek için, uygulamanın yalnızca 24 yerine tek bir varlık alması gerekir ve bu da verimli bir sorgu sağlar. Sorgu verimliliği hakkında daha fazla bilgi için Sorgu [kapsamı](#query-scope)başlıklı bölüme bakın).

#### <a name="storing-structured-data-in-blobs"></a>Yapılandırılmış verileri blobs'ta depolama

Toplu iş kesici uçlar gerçekleştiriyor ve daha sonra varlıkların aralıklarını birlikte alıyorsanız, tablolar yerine blobs kullanmayı düşünün. İyi bir örnek bir günlük dosyasıdır. Birkaç dakika günlük leri toplu ve bunları eklemek ve sonra bir seferde günlükleri birkaç dakika almak. Bu durumda, tablolar yerine blobs kullanırsanız performans daha iyidir, çünkü yazıldığı veya okunduğu nesne sayısını ve ayrıca yapılması gereken istek sayısını önemli ölçüde azaltabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)
- [Standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json)
- [Durum ve hata kodları](/rest/api/storageservices/Status-and-Error-Codes2)
