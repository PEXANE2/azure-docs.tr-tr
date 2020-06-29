---
title: Azure Cosmos DB'de bölümleme
description: Azure Cosmos DB bölümleme hakkında bilgi, bölüm anahtarı seçerken en iyi yöntemler ve mantıksal bölümlerin nasıl yönetileceği hakkında bilgi edinin
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: aa7d67cd6bd1bd422bd257b75ac5bde3bd534d7e
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85481842"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB'de bölümleme

Azure Cosmos DB, uygulamanızın performans ihtiyaçlarını karşılamak üzere bir veritabanındaki ayrı kapsayıcıları ölçeklendirmek için bölümleme kullanır. Bölümleme bölümünde, bir kapsayıcıdaki öğeler *mantıksal bölümler*adlı farklı alt kümelere bölünür. Mantıksal bölümler, bir kapsayıcıdaki her öğeyle ilişkili bir *bölüm anahtarının* değerine göre oluşturulur. Mantıksal bir bölümdeki tüm öğeler aynı bölüm anahtarı değerine sahiptir.

Örneğin, bir kapsayıcı öğeleri barındırır. Her öğenin özellik için benzersiz bir değeri vardır `UserID` . `UserID`Kapsayıcıda öğeler için bölüm anahtarı olarak görev yapar ve 1.000 benzersiz `UserID` değer varsa, kapsayıcı için 1.000 mantıksal bölümler oluşturulur.

Öğenin mantıksal bölümünü belirleyen bir bölüm anahtarına ek olarak, bir kapsayıcıdaki her öğe bir *öğe kimliğine* (mantıksal bölüm içinde benzersiz) sahiptir. Bölüm anahtarını ve *öğe kimliğini* birleştirmek, öğenin benzersiz şekilde tanımlandığı öğenin *dizinini*oluşturur.

[Bölüm anahtarının seçilmesi](partitioning-overview.md#choose-partitionkey) , uygulamanızın performansını etkileyecek önemli bir karardır.

## <a name="managing-logical-partitions"></a>Mantıksal bölümleri yönetme

, Kapsayıcının ölçeklenebilirlik ve performans ihtiyaçlarını etkili bir şekilde karşılamak üzere fiziksel bölümlerin mantıksal bölümlerinin yerleşimini saydam ve otomatik olarak yönetir. Azure Cosmos DB Bir uygulamanın aktarım hızı ve depolama gereksinimleri artdıkça, yükü daha fazla sayıda fiziksel bölümde otomatik olarak yaymak için mantıksal bölümlerin taşınması Azure Cosmos DB. [Fiziksel bölümler](partition-data.md#physical-partitions)hakkında daha fazla bilgi edinebilirsiniz.

Azure Cosmos DB, mantıksal bölümleri fiziksel bölümler arasında yaymak için karma tabanlı bölümleme kullanır. Bir öğenin bölüm anahtarı değerini karma Azure Cosmos DB. Karma hale getirilmiş sonuç fiziksel bölümü belirler. Ardından Azure Cosmos DB, bölüm anahtarı karmalarının anahtar alanını fiziksel bölümler arasında eşit olarak ayırır.

İşlemlere (saklı yordamlarda veya tetikleyicilere) yalnızca tek bir mantıksal bölümdeki öğelere karşı izin verilir.

[Azure Cosmos DB bölümleri nasıl yönettiği](partition-data.md)hakkında daha fazla bilgi edinebilirsiniz. (Uygulamalarınızı derlemek veya çalıştırmak için dahili ayrıntıların anlaşılması gerekmez, ancak bunu merak eden bir okuyucu için buraya eklemiş olmanız gerekmez.)

## <a name="choosing-a-partition-key"></a><a id="choose-partitionkey"></a>Bölüm anahtarını seçme

Bölüm anahtarında iki bileşen vardır: **bölüm anahtar yolu** ve **bölüm anahtarı değeri**. Örneğin, {"UserID" öğesini düşünün: "Andrew", "worksFor": "Microsoft"} bölüm anahtarı olarak "UserID" seçeneğini belirlerseniz, iki bölüm anahtarı bileşeni aşağıda verilmiştir:

* Bölüm anahtarı yolu (örneğin: "/UserID"). Bölüm anahtarı yolu alfasayısal ve alt çizgi (_) karakterlerini kabul eder. Ayrıca, standart yol gösterimini (/) kullanarak iç içe geçmiş nesneleri de kullanabilirsiniz.

* Bölüm anahtarı değeri (örneğin: "Andrew"). Bölüm anahtarı değeri dize veya sayısal türlerde olabilir.

Bölüm anahtarının aktarım hızı, depolama ve uzunluğu hakkında daha fazla bilgi edinmek için [Azure Cosmos DB hizmet kotaları](concepts-limits.md) makalesine bakın.

Bölüm anahtarınızı seçmek Azure Cosmos DB basit ancak önemli bir tasarım seçimdir. Bölüm anahtarınızı seçtikten sonra yerinde değiştirmek mümkün değildir. Bölüm anahtarınızı değiştirmeniz gerekiyorsa, verilerinizi yeni istenen bölüm anahtarınızla yeni bir kapsayıcıya taşımanız gerekir.

**Tüm** kapsayıcılar için, Bölüm anahtarınız şunları yapmalısınız:

* Değişmez değeri olan bir özellik olamaz. Bir özellik bölüm anahtarınıza sahip ise, bu özelliğin değerini güncelleştiremezsiniz.
* Yüksek bir kardinalite olmalıdır. Diğer bir deyişle, özelliği çok sayıda olası değere sahip olmalıdır.
* İstek birimi (RU) tüketimini ve veri depolamayı tüm mantıksal bölümlerde eşit oranda dağıt. Bu da fiziksel bölümlerinizde RU tüketim ve depolama dağıtımı sağlar.

Azure Cosmos DB [Çoklu öğe ACID işlemlerine](database-transactions-optimistic-concurrency.md#multi-item-transactions) ihtiyacınız varsa, [saklı yordamları veya Tetikleyicileri](how-to-write-stored-procedures-triggers-udfs.md#stored-procedures)kullanmanız gerekir. Tüm JavaScript tabanlı saklı yordamlar ve Tetikleyiciler, tek bir mantıksal bölümün kapsamına alınır.

## <a name="partition-keys-for-read-heavy-containers"></a>Okuma ağır kapsayıcılar için bölüm anahtarları

Çoğu kapsayıcı için yukarıdaki ölçüt, bölüm anahtarı seçerken göz önünde bulundurmanız gereken tek şey olur. Öte yandan, büyük okuma ağır kapsayıcılar için, sorgularda filtre olarak sık görüntülenen bir bölüm anahtarı seçmek isteyebilirsiniz. Sorgular, filtre koşuluna bölüm anahtarını ekleyerek [, yalnızca ilgili fiziksel bölümlere etkin](how-to-query-container.md#in-partition-query) bir şekilde yönlendirilebilir.

İş yükünüzün isteklerinin çoğu sorgular ise ve sorgularınızdaki çoğu aynı özellikte bir eşitlik filtresine sahipseniz, bu özellik iyi bir bölüm anahtarı seçeneği olabilir. Örneğin, sık sık filtre uygulayan bir sorgu çalıştırırsanız `UserID` , `UserID` bölüm anahtarı olarak seçilmesi, [bölümler arası sorguların](how-to-query-container.md#avoiding-cross-partition-queries)sayısını azaltır.

Ancak, Kapsayıcınız küçükse, bölümler arası sorguların performans etkisi konusunda endişelenmeniz gereken yeterli fiziksel bölümünüz yok demektir. Azure Cosmos DB çoğu küçük kapsayıcı yalnızca bir veya iki fiziksel bölüm gerektirir.

Kapsayıcınız birkaç fiziksel bölümden daha fazla büyümeye devam ediyorsanız, çapraz bölüm sorgularını en aza indiren bir bölüm anahtarı seçtiğinizden emin olun. Aşağıdakilerden biri geçerliyse, Kapsayıcınız birkaç fiziksel bölümden fazlasını gerektirecektir:

* Kapsayıcıda 30.000 RU üzerinde sağlanan
* Kapsayıcınız 100 GB veri üzerinden depolanacak

## <a name="using-item-id-as-the-partition-key"></a>Bölüm anahtarı olarak öğe KIMLIĞI kullanma

Kapsayıcının çok sayıda olası değeri olan bir özelliği varsa, büyük olasılıkla büyük bir bölüm anahtarı seçimidir. Bu tür bir özelliğin olası bir örneği *öğe kimliğidir*. Küçük okuma ağır kapsayıcılar veya herhangi bir büyüklükte yazma ağır kapsayıcılar için, *öğe kimliği* doğal olarak bölüm anahtarı için harika bir seçimdir.

Sistem özelliği *öğe kimliği* , Cosmos kabınızda her öğede var olduğu garanti edilir. Öğe için bir mantıksal KIMLIĞI temsil eden diğer özelliklere sahip olabilirsiniz. Çoğu durumda bunlar Ayrıca, *öğe kimliği*ile aynı nedenlerden dolayı büyük bölüm anahtarı seçimleridir.

*Öğe kimliği* , aşağıdaki nedenlerden dolayı harika bir bölüm anahtarı seçimleridir:

* Çok sayıda olası değer aralığı vardır (öğe başına bir benzersiz *öğe kimliği* ).
* Öğe başına benzersiz bir *öğe kimliği* olduğundan, *öğe kimliği* , ru tüketimine ve veri depolama alanı üzerinde eşit dengelemeye sahip harika bir iş yapar.
* *Öğe kimliğini*biliyorsanız, öğenin bölüm anahtarını her zaman öğrentireceğiz, kolayca etkili nokta okuma yapabilirsiniz.

Bölüm anahtarı olarak *öğe kimliği* seçerken göz önünde bulundurmanız gereken bazı noktalar şunlardır:

* *Öğe kimliği* bölüm anahtarınkse, kapsayıcının tamamında benzersiz bir tanımlayıcı olur. Yinelenen *öğe kimliği*olan öğeleriniz olamaz.
* Çok sayıda [fiziksel bölüm](partition-data.md#physical-partitions)içeren bir okuma ağır kapsayıcınız varsa, *öğe kimliği*ile bir eşitlik filtresi varsa sorgular daha verimli olacaktır.
* Saklı yordamları veya Tetikleyicileri birden çok mantıksal bölümde çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB bölümünde bölümlendirme ve yatay ölçeklendirme](partition-data.md)hakkında bilgi edinin.
* [Azure Cosmos DB 'de sağlanan aktarım hızı](request-units.md)hakkında bilgi edinin.
* [Azure Cosmos DB 'de küresel dağıtım](distribute-data-globally.md)hakkında bilgi edinin.
