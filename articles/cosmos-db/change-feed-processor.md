---
title: Azure Cosmos DB'deki değişiklik akışı işlemcisi
description: Değişiklik akışını, değişiklik akışı işlemcisinin bileşenlerini okumak için Azure Cosmos DB değişiklik akışı işlemcisini nasıl kullanacağınızı öğrenin
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.custom: devx-track-csharp
ms.openlocfilehash: 409b51682700a8b13b2840f171642bdcbee6f6d2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93340235"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Azure Cosmos DB'deki değişiklik akışı işlemcisi
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Değişiklik akışı işlemcisi, [Azure Cosmos DB SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3)'nin bir parçasıdır. Değişiklik akışını okuma ve olay işlemeyi birden çok tüketiciye etkin bir şekilde dağıtma sürecini basitleştirir.

Değişiklik akışı işlemci kitaplığının ana avantajı, değişiklik akışındaki tüm olayların "en az bir kez" teslimini sağlayan hataya dayanıklı davranıştır.

## <a name="components-of-the-change-feed-processor"></a>Değişiklik akışı işlemcisinin bileşenleri

Değişiklik akışı işlemcisi uygulama sürecinin dört ana bileşeni vardır:

1. **İzlenen kapsayıcı:** İzlenen kapsayıcı, değişiklik akışının oluşturulduğu verileri içerir. İzlenen kapsayıcıda yapılan eklemeler veya güncelleştirmeler, kapsayıcının değişiklik akışına yansıtılır.

1. **Kira kapsayıcısı:** Kira kapsayıcısı, durum depolama alanı olarak görev yapar ve birden fazla çalışanda gerçekleştirilen değişiklik akışı işleme sürecini koordine eder. Kira kapsayıcısı, izlenen kapsayıcı ile aynı hesapta veya ayrı bir hesapta depolanabilir.

1. **Konak:** Konak, değişiklikleri dinlemek için değişiklik akışı işlemcisini kullanan bir uygulama örneğidir. Aynı kira yapılandırmasına sahip birden fazla örnek paralel olarak çalıştırılabilir ancak her örnek farklı bir **örnek adına** sahip olmalıdır.

1. **Temsilci:** Temsilci, geliştirici olarak değişiklik akışı işlemcisinin okuduğu her toplu değişiklik sonrasında yapmak istediğiniz işlemi tanımlayan koddur. 

Bu dört öğelerin değişiklik akışı işlemcisi ile birlikte nasıl çalıştığını anlamak için aşağıdaki diyagramda bir örneğe bakalım. İzlenen kapsayıcı belgeleri depolar ve bölüm anahtarı olarak ' City ' kullanır. Bölüm anahtarı değerlerinin öğeler içeren aralıklarda dağıtıldığını görüyoruz. İki ana bilgisayar örneği bulunur ve değişiklik akışı işlemcisi, işlem dağıtımını en üst düzeye çıkarmak için her örneğe farklı bölüm anahtarı değerlerini atanıyor. Her Aralık paralel olarak okunmakta ve ilerleme durumu kira kapsayıcısındaki diğer aralıklardan ayrı olarak korunur.

:::image type="content" source="./media/change-feed-processor/changefeedprocessor.png" alt-text="Akış işlemcisi örneğini değiştirme" border="false":::

## <a name="implementing-the-change-feed-processor"></a>Değişiklik akışı işlemcisini uygulama

Giriş noktası her zaman izlenen kapsayıcıdır, `Container` çağrı yaptığınız bir örnekten `GetChangeFeedProcessorBuilder` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

İlk parametre, bu işlemcinin hedefini açıklayan ayrı bir addır ve ikinci ad, değişiklikleri işleyecek temsilci uygulamasıdır. 

Bir temsilci örneği şöyle olabilir:


[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Son olarak bu işlemci örneği için `WithInstanceName` ve ile kira durumunun bakımını yapılacak kapsayıcı olan bir ad tanımlarsınız `WithLeaseContainer` .

Çağırmak `Build` size, çağırarak başlayabilmeniz için kullanabileceğiniz işlemci örneğini sağlar `StartAsync` .

## <a name="processing-life-cycle"></a>İşlem yaşam döngüsü

Bir konak örneğinin normal yaşam döngüsü şu şekildedir:

1. Değişiklik akışını okuyun.
1. Değişiklik yoksa, önceden tanımlanmış bir süre (Oluşturucu içinde özelleştirilebilir) için uyku moduna `WithPollInterval` geçin ve #1 gidin.
1. Değişiklikler varsa **temsilciyi temsilciye** gönderin.
1. Temsilci değişiklikleri **başarıyla** işlemeyi tamamladığında, kira deposunu en son işlenen zaman noktasıyla güncelleştirin ve #1 gidin.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışı işlemcisi, Kullanıcı kodu hatalarına karşı dayanıklı olur. Bu, temsilci uygulamanızın işlenmeyen bir özel durum (adım #4) varsa, belirli bir değişiklik kümesini işleyen iş parçacığının durdurulması ve yeni bir iş parçacığının oluşturulması anlamına gelir. Yeni iş parçacığı, kira deposunun Bu bölüm anahtarı değerleri aralığına sahip olduğu en son noktayı denetler ve bu durumda, temsilci üzerinde aynı toplu değişiklik kümesini etkin bir şekilde göndererek buradan yeniden başlatılır. Bu davranış, temsilci değişiklikleri doğru şekilde işleyene kadar devam eder ve değişiklik akışı işlemcisinin "en az bir kez" garantisi vardır çünkü temsilci kodu bir özel durum oluşturursa, bu toplu işi yeniden dener.

Değişiklik akışı işlemcinizin "takılmış" olarak aynı değişiklik kümesini sürekli yeniden denemesini engellemek için, özel durum durumunda belgeleri, teslim edilemeyen bir ileti kuyruğuna yazmak üzere temsilci kodunuzda mantık eklemeniz gerekir. Bu tasarım, devam eden değişiklikleri işlemeye devam edebilirken işlenmemiş değişiklikleri izlemenize olanak sağlar. Atılacak mektup kuyruğu başka bir Cosmos kapsayıcısı olabilir. Tam veri deposu, işlenmemiş değişikliklerin kalıcı hale gelen önemi yoktur.

Ek olarak, değişiklik akışını okurken değişiklik akışı işlemci örneklerinizin ilerlemesini izlemek için [akış tahmin aracı](how-to-use-change-feed-estimator.md) ' ı da kullanabilirsiniz. Bu tahmini, değişiklik akışı işlemcinizin CPU, bellek ve ağ bant genişliği gibi kullanılabilir kaynaklar nedeniyle "takılmış" veya geri atma olduğunu anlamak için kullanabilirsiniz.

## <a name="deployment-unit"></a>Dağıtım birimi

Tek bir değişiklik akışı işlemcisi dağıtım birimi, aynı `processorName` ve kira kapsayıcı yapılandırmasına sahip bir veya daha fazla örnek içerir. Her birinin değişiklikler için farklı bir iş akışı ve bir veya daha fazla örnek içeren her dağıtım birimi olduğu birçok dağıtım birimi olabilir. 

Örneğin, kapsayıcıda bir değişiklik olduğu zaman bir dış API 'yi tetikleyen bir dağıtım biriminiz olabilir. Başka bir dağıtım birimi, her değişiklik olduğunda verileri gerçek zamanlı olarak taşıyabilir. İzlenen kapsayıcıda bir değişiklik olduğunda, tüm dağıtım birimleriniz bildirilir.

## <a name="dynamic-scaling"></a>Dinamik ölçeklendirme

Daha önce belirtildiği gibi, bir dağıtım birimi içinde bir veya daha fazla örneğe sahip olabilirsiniz. Dağıtım birimi içindeki işlem dağıtımından faydalanmak için tek önemli gereksinimler şunlardır:

1. Tüm örnekler aynı kira kapsayıcı yapılandırmasına sahip olmalıdır.
1. Tüm örneklerin aynı olması gerekir `processorName` .
1. Her örneğin farklı bir örnek adına (`WithInstanceName`) sahip olması gerekir.

Bu üç koşul geçerliyse, değişiklik akışı işlemcisi, eşit bir dağıtım algoritması kullanarak, bu dağıtım birimi ve paralel hale getirmek işlem örneklerinin tüm çalışan örnekleri genelinde kira kapsayıcısındaki tüm kiraları dağıtır. Tek bir kiralamanın belirli bir zamanda yalnızca bir örneğe ait olması, en fazla örnek sayısının kira sayısına eşit olması için.

Örnek sayısı büyüyebilir ve küçülebilir ve değişiklik akışı işlemcisi, uygun şekilde yeniden dağıtarak yükü dinamik olarak ayarlar.

Üstelik, değişiklik akışı işlemcisi, verimlilik veya depolama arttıkça dinamik olarak kapsayıcılar ölçeğinde değişiklik yapabilir. Kapsayıcınız büyüdükçe, değişiklik akışı işlemcisi, kiralamaları dinamik olarak artırarak ve yeni kiraları mevcut örnekler arasında dağıtarak bu senaryoları saydam olarak gerçekleştirir.

## <a name="change-feed-and-provisioned-throughput"></a>Akışı ve sağlanan aktarım hızını değiştirme

İzlenen kapsayıcıdaki akış okuma işlemlerinin değişiklik, RUs 'yi kullanır. 

Kira kapsayıcısındaki işlemler RUs kullanır. Aynı kira kapsayıcısını kullanan örneklerin sayısı arttıkça, olası RU tüketiminin daha yüksek olması gerekir. Örnek sayısını ölçeklendirmeye ve arttırmaya karar verirseniz, kiralamalar kapsayıcısında RU tüketiminizi izlemeyi unutmayın.

## <a name="starting-time"></a>Başlangıç zamanı

Varsayılan olarak, bir değişiklik akışı işlemcisi ilk kez başladığında, kiralamalar kapsayıcısını başlatır ve [işlem yaşam döngüsünü](#processing-life-cycle)başlatır. Değişiklik akışı işlemcisi ilk kez başlatılmadan önce izlenen kapsayıcıda gerçekleşen tüm değişiklikler algılanmaz.

### <a name="reading-from-a-previous-date-and-time"></a>Önceki bir tarih ve saatten okuma

Değişiklik akışı işlemcisini, **belirli bir tarih ve saatte** başlayan değişiklikleri okumak için, bir a örneğini `DateTime` Oluşturucu uzantısına geçirerek başlatmak mümkündür `WithStartTime` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=TimeInitialization)]

Değişiklik akışı işlemcisi, belirli bir tarih ve saat için başlatılır ve sonrasında gerçekleşen değişiklikleri okumaya başlar.

### <a name="reading-from-the-beginning"></a>Baştan itibaren okunuyor

Veri geçişleri veya bir kapsayıcının tüm geçmişinin çözümlenmesi gibi diğer senaryolarda, **Bu kapsayıcının yaşam süresinden itibaren** değişiklik akışını okuduk. Bunu yapmak için, Oluşturucu uzantısı üzerinde kullanabiliriz, ancak bu, `WithStartTime` `DateTime.MinValue.ToUniversalTime()` En düşük değerin UTC gösterimini oluşturan, `DateTime` şöyle olabilir:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartFromBeginningInitialization)]

Değişiklik akışı işlemcisi başlatılır ve kapsayıcının yaşam süresinden itibaren yapılan değişiklikleri okumaya başlar.

> [!NOTE]
> Bu özelleştirme seçenekleri yalnızca değişiklik akışı işlemcisinin başlangıç noktasını ayarlamak için çalışır. Kiralamalar kapsayıcısı ilk kez başlatıldıktan sonra, bunların değiştirilmesinin etkisi olmaz.

## <a name="where-to-host-the-change-feed-processor"></a>Değişiklik akışı işlemcisinin nerede barındırkaydedileceği

Değişiklik akışı işlemcisi, uzun süre çalışan işlemleri veya görevleri destekleyen herhangi bir platformda barındırılabilir:

* Sürekli çalışan bir [Azure WebJob](/learn/modules/run-web-app-background-task-with-webjobs/).
* [Azure sanal makinesindeki](/azure/architecture/best-practices/background-jobs#azure-virtual-machines)bir işlem.
* [Azure Kubernetes hizmetinde](/azure/architecture/best-practices/background-jobs#azure-kubernetes-service)bir arka plan işi.
* Bir [ASP.NET barındırılan hizmeti](/aspnet/core/fundamentals/host/hosted-services).

Değişiklik akışı işlemcisi kısa süreli ortamlarda çalışabilir, ancak kira kapsayıcısı durumu koruduğundan, bu ortamların başlangıç döngüleri bildirimleri almaya yönelik gecikme ekler (ortamın her başlatılışında işlemciyi başlatma yükünden kaynaklanır).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da örnek uygulamayı tamamlıyorum](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)
* [GitHub 'da ek kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Değişiklik akışı işlemcisi için Cosmos DB Workshop Labs](https://azurecosmosdb.github.io/labs/dotnet/labs/08-change_feed_with_azure_functions.html#consume-cosmos-db-change-feed-via-the-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde akış işlemcisini Değiştir hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışı çekme modeli](change-feed-pull-model.md)
* [Değişiklik akışı işlemci kitaplığından geçiş yapma](how-to-migrate-from-change-feed-library.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](#starting-time)