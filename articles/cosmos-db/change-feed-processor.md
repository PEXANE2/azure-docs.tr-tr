---
title: Azure Cosmos DB'de özet akışı işlemci kitaplığını değiştirme
description: Değişiklik akışı işlemcisinin bileşenlerini okumak için Azure Cosmos DB değişiklik akışı işlemci kitaplığını nasıl kullanacağınızı öğrenin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 12/03/2019
ms.reviewer: sngun
ms.openlocfilehash: e71b2807595aebeb1f0c8682fde119f4e267e55d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273317"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Azure Cosmos DB'deki değişiklik akışı işlemcisi 

Değişiklik beslemeişlemcisi [Azure Cosmos DB SDK V3'ün](https://github.com/Azure/azure-cosmos-dotnet-v3)bir parçasıdır. Değişiklik akışını okuma işlemini kolaylaştırır ve olay işlemeyi birden çok tüketiciye etkin bir şekilde dağıtır.

Değişiklik beslemesi işlemci kitaplığı temel yararı, değişiklik akışı tüm olayların "en az bir kez" teslim sağlayan hata toleranslı davranışıdır.

## <a name="components-of-the-change-feed-processor"></a>Besleme işlemcisinin bileşenleri

Değişiklik beslemeişlemcisini uygulamanın dört ana bileşeni vardır: 

1. **İzlenen kapsayıcı:** İzlenen kapsayıcı, değişiklik akışının oluşturulduğu verilere sahiptir. İzlenen kapsayıcıya eklenen ekler ve güncelleştirmeler, kapsayıcının değişiklik akışına yansıtılır.

1. **Kira konteyneri:** Kira kapsayıcısı bir devlet depolama görevi görür ve birden çok işçi arasında değişiklik beslemesi işleme koordine eder. Kira kapsayıcısı, izlenen kapsayıcıyla aynı hesapta veya ayrı bir hesapta depolanabilir. 

1. **Ev sahibi:** Ana bilgisayar, değişiklikleri dinlemek için besleme işlemcisini değiştir'i kullanan bir uygulama örneğidir. Aynı kira yapılandırması ile birden çok örnek paralel olarak çalıştırAbilirsiniz, ancak her örnek farklı bir **örnek adı**olmalıdır. 

1. **Temsilci:** Temsilci, geliştirici olarak değişiklik akışı işlemcisinin okuduğu her değişiklik grubuyla ne yapmak istediğinizi tanımlayan koddur. 

Bu dört değişiklik besleme işlemcisinin elemanlarının birlikte nasıl çalıştığını daha iyi anlamak için aşağıdaki diyagramdaki bir örneğe bakalım. İzlenen kapsayıcı belgeleri depolar ve bölüm anahtarı olarak 'Şehir' kullanır. Bölüm anahtar değerlerinin öğeleri içeren aralıklarda dağıtıldığını görüyoruz. İki ana bilgisayar örneği vardır ve değişiklik akışı işlemcisi, bilgi işlem dağılımını en üst düzeye çıkarmak için her örneğe farklı bölüm anahtar değerleri aralıkları atamaktadır. Her aralık paralel olarak okunuyor ve ilerlemesi kira kapsayıcısındaki diğer aralıklardan ayrı olarak korunuyor.

![Akış işlemcisi örneğini değiştirme](./media/change-feed-processor/changefeedprocessor.png)

## <a name="implementing-the-change-feed-processor"></a>Değişiklik besleme işlemcisinin uygulanması

Giriş noktası her zaman izlenen kapsayıcı, `Container` sizin dedediğiniz `GetChangeFeedProcessorBuilder`bir örnekten:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

İlk parametrenin bu işlemcinin hedefini açıklayan farklı bir ad olduğu ve ikinci adın değişiklikleri işleyecek temsilci uygulaması olduğu durumlarda. 

Bir temsilci örneği:


[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Son olarak bu işlemci örneği `WithInstanceName` için bir ad tanımlarsınız ve `WithLeaseContainer`bu kapsayıcı ile kira durumunu korumak için .

Arama, `Build` arayarak başlatabileceğiniz işlemci örneğini `StartAsync`verecektir.

## <a name="processing-life-cycle"></a>İşleme yaşam döngüsü

Ana bilgisayar örneğinin normal yaşam döngüsü:

1. Değişiklik akışını okuyun.
1. Değişiklik yoksa, önceden tanımlanmış bir süre için uyuyun `WithPollInterval` (Oluşturucu'da özelleştirilebilir) ve #1 gidin.
1. Değişiklikler varsa, bunları **temsilciye**gönderin.
1. Temsilci değişiklikleri **başarıyla**işlemeyi bitirdiğinde, kira deposunu en son işlenen noktayla güncelleştirin ve #1 gidin.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışı işlemcisi kullanıcı kodu hatalarına karşı dayanıklıdır. Bu, temsilci uygulamanızda işlenmemiş bir özel durum (adım #4) varsa, belirli bir değişiklik toplu iş parçacığının işleneceğini ve yeni bir iş parçacığının oluşturulacağı anlamına gelir. Yeni iş parçacığı, kira deposunun bu bölüm anahtar değerleri aralığı için sahip olduğu sürenin en son noktası olduğunu denetler ve aynı değişiklik partisini etkin bir şekilde temsilciye göndererek oradan yeniden başlatır. Bu davranış, temsilciniz değişiklikleri doğru şekilde işleyene kadar devam eder ve değişiklik akışı işlemcisinin "en az bir kez" garantisi olmasının nedeni budur, çünkü temsilci kodu atarsa, bu toplu işlemi yeniden dener.

## <a name="dynamic-scaling"></a>Dinamik ölçeklendirme

Giriş sırasında belirtildiği gibi, değişiklik beslemesi işlemciotomatik olarak birden çok örnek arasında işlem dağıtabilir. Değişiklik besleme işlemcisini kullanarak uygulamanızın birden çok örneğini dağıtabilir ve uygulamadan yararlanabilirsiniz, tek temel gereksinimler şunlardır:

1. Tüm örnekler aynı kira kapsayıcı yapılandırması olmalıdır.
1. Tüm örnekleraynı iş akışı adına sahip olmalıdır.
1. Her örnek farklı bir örnek`WithInstanceName`adı ( olması gerekir.

Bu üç koşul uygulanırsa, değişiklik akışı işlemcisi eşit dağıtım algoritması kullanarak, kira kapsayıcısındaki tüm kiralamaları çalışan tüm örneklere dağıtacak ve bilgi işlemle paralelleştirecektir. Bir kiralama yalnızca belirli bir zamanda bir örneğe sahip olabilir, bu nedenle en fazla örnek sayısı kira sayısına eşittir.

Örnek sayısı büyüyebilir ve küçülebilir ve değişiklik beslemesi işlemcisi buna göre yeniden dağıtarak yükü dinamik olarak ayarlar.

Ayrıca, değişiklik beslemesi işlemcisi, iş akışı veya depolama artışları nedeniyle kapsayıcı ölçeğine dinamik olarak ayarlanabilir. Kapsayıcınız büyüdüğünde, değişiklik akışı işlemcisi, kiralamaları dinamik olarak artırarak ve yeni kiralamaları varolan durumlar arasında dağıtarak bu senaryoları saydam olarak işler.

## <a name="change-feed-and-provisioned-throughput"></a>Beslemeve sağlanan iş akışını değiştirme

Cosmos kaplarına girip çıkan veri hareketi her zaman RUs tükettiği için, tüketilen RUs'lar için ücretlendirilirsiniz. Kira kabı tarafından tüketilen RUs için ücretlendirilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde besleme işlemcisi değiştir hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışı işlemci kitaplığından nasıl geçirilir?](how-to-migrate-from-change-feed-library.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](how-to-configure-change-feed-start-time.md)
