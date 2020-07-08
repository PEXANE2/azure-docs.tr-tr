---
title: Azure Cosmos DB'deki değişiklik akışı işlemcisi
description: Değişiklik akışını, değişiklik akışı işlemcisinin bileşenlerini okumak için Azure Cosmos DB değişiklik akışı işlemcisini nasıl kullanacağınızı öğrenin
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/13/2020
ms.reviewer: sngun
ms.openlocfilehash: 4325f75ac8181e088d64e53d3f65e085a09c0224
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85119418"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Azure Cosmos DB'deki değişiklik akışı işlemcisi

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
1. Değişiklikler varsa **temsilciyi temsilciye**gönderin.
1. Temsilci değişiklikleri **başarıyla**işlemeyi tamamladığında, kira deposunu en son işlenen zaman noktasıyla güncelleştirin ve #1 gidin.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışı işlemcisi, Kullanıcı kodu hatalarına karşı dayanıklı olur. Bu, temsilci uygulamanızın işlenmeyen bir özel durum (adım #4) varsa, belirli bir değişiklik kümesini işleyen iş parçacığının durdurulması ve yeni bir iş parçacığının oluşturulması anlamına gelir. Yeni iş parçacığı, kira deposunun Bu bölüm anahtarı değerleri aralığına sahip olduğu en son noktayı denetler ve bu durumda, temsilci üzerinde aynı toplu değişiklik kümesini etkin bir şekilde göndererek buradan yeniden başlatılır. Bu davranış, temsilci değişiklikleri doğru şekilde işleyene kadar devam eder ve değişiklik akışı işlemcisinin "en az bir kez" garantisi vardır çünkü temsilci kodu bir özel durum oluşturursa, bu toplu işi yeniden dener.

Değişiklik akışı işlemcinizin "takılmış" olarak aynı değişiklik kümesini sürekli yeniden denemesini engellemek için, özel durum durumunda belgeleri, teslim edilemeyen bir ileti kuyruğuna yazmak üzere temsilci kodunuzda mantık eklemeniz gerekir. Bu tasarım, devam eden değişiklikleri işlemeye devam edebilirken işlenmemiş değişiklikleri izlemenize olanak sağlar. Atılacak mektup kuyruğu yalnızca başka bir Cosmos kapsayıcısı olabilir. Tam veri deposu, işlenmemiş değişikliklerin kalıcı hale gelen önemi yoktur.

Ek olarak, değişiklik akışını okurken değişiklik akışı işlemci örneklerinizin ilerlemesini izlemek için [akış tahmin aracı](how-to-use-change-feed-estimator.md) ' ı da kullanabilirsiniz. Değişiklik akışı işlemcisinin "takılı" olarak aynı değişiklik kümesini sürekli yeniden denemesinin ne olduğunu izlemeye ek olarak, değişiklik akışı işlemcinizin CPU, bellek ve ağ bant genişliği gibi kullanılabilir kaynaklar nedeniyle gerisinde olup olmadığını da anlayabilirsiniz.

## <a name="deployment-unit"></a>Dağıtım birimi

Tek bir değişiklik akışı işlemcisi dağıtım birimi, aynı `processorName` ve kira kapsayıcı yapılandırmasına sahip bir veya daha fazla örnek içerir. Her birinin değişiklikler için farklı bir iş akışı ve bir veya daha fazla örnek içeren her dağıtım birimi olduğu birçok dağıtım birimi olabilir. 

Örneğin, kapsayıcıda bir değişiklik olduğu zaman bir dış API 'yi tetikleyen bir dağıtım biriminiz olabilir. Başka bir dağıtım birimi, her değişiklik olduğunda verileri gerçek zamanlı olarak taşıyabilir. İzlenen kapsayıcıda bir değişiklik olduğunda, tüm dağıtım birimleriniz bildirilir.

## <a name="dynamic-scaling"></a>Dinamik ölçeklendirme

Daha önce belirtildiği gibi, bir dağıtım birimi içinde bir veya daha fazla örneğe sahip olabilirsiniz. Dağıtım birimi içindeki işlem dağıtımından faydalanmak için tek önemli gereksinimler şunlardır:

1. Tüm örneklerin aynı kira kapsayıcı yapılandırmasına sahip olması gerekir.
1. Tüm örneklerin aynı olması gerekir `processorName` .
1. Her örneğin farklı bir örnek adına (`WithInstanceName`) sahip olması gerekir.

Bu üç koşul geçerliyse, değişiklik akışı işlemcisi, eşit bir dağıtım algoritması kullanarak, bu dağıtım birimi ve paralel hale getirmek işlem örneklerinin tüm çalışan örnekleri genelinde kira kapsayıcısındaki tüm kiraları dağıtır. Tek bir kiralamanın belirli bir zamanda yalnızca bir örneğe ait olması, en fazla örnek sayısının kira sayısına eşit olması için.

Örnek sayısı büyüyebilir ve küçülebilir ve değişiklik akışı işlemcisi, uygun şekilde yeniden dağıtarak yükü dinamik olarak ayarlar.

Üstelik, değişiklik akışı işlemcisi, verimlilik veya depolama arttıkça dinamik olarak kapsayıcılar ölçeğinde değişiklik yapabilir. Kapsayıcınız büyüdükçe, değişiklik akışı işlemcisi, kiralamaları dinamik olarak artırarak ve yeni kiraları mevcut örnekler arasında dağıtarak bu senaryoları saydam olarak gerçekleştirir.

## <a name="change-feed-and-provisioned-throughput"></a>Akışı ve sağlanan aktarım hızını değiştirme

Cosmos kapsayıcılarının içindeki ve içindeki veri hareketleri her zaman RUs kullandığından, kullanılan ru için ücretlendirilirsiniz. Kira kapsayıcısı tarafından tüketilen RUs için ücretlendirilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub 'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde akış işlemcisini Değiştir hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışı çekme modeli](change-feed-pull-model.md)
* [Değişiklik akışı işlemci kitaplığından geçiş yapma](how-to-migrate-from-change-feed-library.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](how-to-configure-change-feed-start-time.md)
