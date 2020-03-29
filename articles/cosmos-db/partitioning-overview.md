---
title: Azure Cosmos DB'de bölümleme
description: Azure Cosmos DB'de bölümleme, bölüm anahtarı seçerken en iyi uygulamalar ve mantıksal bölümlerin nasıl yönetilen hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 551703b5dcca082904197010366ee059998dde4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251875"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB'de bölümleme

Azure Cosmos DB, uygulamanızın performans gereksinimlerini karşılamak için bir veritabanındaki tek tek kapsayıcıları ölçeklendirmek için bölümleme kullanır. Bölme de, bir kapsayıcı öğeleri mantıksal bölümler olarak adlandırılan farklı alt *kümelere*ayrılır. Mantıksal bölümler, bir kapsayıcıdaki her öğeyle ilişkili bir *bölüm anahtarının* değerine göre oluşturulur. Mantıksal bölümdeki tüm öğeler aynı bölüm anahtar değerine sahiptir.

Örneğin, bir kapsayıcı öğeleri tutar. Her öğenin `UserID` özellik için benzersiz bir değeri vardır. Kapsayıcıdaki öğeler için bölme anahtarı olarak `UserID` hizmet veriyorsa ve `UserID` 1.000 benzersiz değer varsa, kapsayıcı için 1.000 mantıksal bölüm oluşturulur.

Öğenin mantıksal bölümlemesini belirleyen bir bölüm anahtarına ek olarak, kapsayıcıdaki her öğenin bir *öğe kimliği* vardır (mantıksal bir bölüm içinde benzersizdir). Bölüm anahtarı ve öğe kimliği birleştirerek öğenin *dizin*oluşturur , benzersiz öğeyi tanımlar.

[Bölüm anahtarı seçmek,](partitioning-overview.md#choose-partitionkey) uygulamanızın performansını etkileyecek önemli bir karardır.

## <a name="managing-logical-partitions"></a>Mantıksal bölümleri yönetme

Azure Cosmos DB, kapsayıcının ölçeklenebilirlik ve performans gereksinimlerini verimli bir şekilde karşılamak için fiziksel bölümlere mantıksal bölümlerin yerleştirilmesini saydam ve otomatik olarak yönetir. Bir uygulamanın üretim ve depolama gereksinimleri arttıkça, Azure Cosmos DB mantıksal bölümleri taşıyarak yükü daha fazla sayıda sunucuya otomatik olarak yayıltır. 

Azure Cosmos DB, mantıksal bölümleri fiziksel bölümlere yaymak için karma tabanlı bölümleme kullanır. Azure Cosmos DB, bir öğenin bölüm anahtar değerini ele eder. Hashed sonucu fiziksel bölümü belirler. Ardından, Azure Cosmos DB, bölüm anahtarının anahtar alanını fiziksel bölümler arasında eşit olarak ayırır.

Tek bir mantıksal bölüm içindeki verilere erişen sorgular, birden çok bölüme erişen sorgulardan daha uygun maliyetlidir. Hareketlere (depolanan yordamlarda veya tetikleyicilerde) yalnızca tek bir mantıksal bölümdeki öğelere karşı izin verilir.

Azure Cosmos DB'nin bölümleri nasıl yönettiği hakkında daha fazla bilgi edinmek için [Mantıksal bölümlere](partition-data.md)bakın. (Bu oluşturmak veya uygulamaları çalıştırmak için iç ayrıntıları anlamak için gerekli değildir, ancak meraklı bir okuyucu için buraya eklendi.)

## <a name="choosing-a-partition-key"></a><a id="choose-partitionkey"></a>Bölüm anahtarını seçme

Aşağıdaki bir bölüm anahtarı seçmek için iyi bir kılavuzdur:

* Tek bir mantıksal bölüm, 20 GB depolama üst sınırı vardır.  

* Azure Cosmos kapsayıcıları saniyede en az 400 istek birimi (RU/s) iş başına sahiptir. Bir veritabanında iş başı sağlandığında, kapsayıcı başına en az RUs saniyede 100 istek birimi (RU/s) dır. Aynı bölüm anahtarına gelen istekler, bir bölüme ayrılan iş bilgisini aşamaz. İstekler ayrılan iş ortasını aşıyorsa, istekler oran sınırlıdır. Bu nedenle, uygulamanızda "etkin noktalara" neden olmayan bir bölüm tuşu seçmek önemlidir.

* Çok çeşitli değerlere ve mantıksal bölümlere eşit olarak yayılmış erişim desenleri olan bir bölüm anahtarı seçin. Bu, veri depolama ve iş elde edilecek kaynakların mantıksal bölümler arasında dağıtılaabilmesi için, kapsayıcınızdaki verilerin ve etkinliğin mantıksal bölümler kümesine yayılmasına yardımcı olur.

* İş yükünü tüm bölümlere eşit olarak ve zaman içinde eşit olarak yayan bir bölüm anahtarı seçin. Bölüm anahtarı seçiminiz, ölçeklenebilirlik elde etmek için öğeleri birden çok bölüme dağıtma hedefine karşı verimli bölüm sorguları ve hareketleri gereksinimini dengelemelidir.

* Bölüm anahtarları için adaylar sorgularınızda filtre olarak sık görülen özellikler içerebilir. Sorgular, filtre yüklenine bölüm anahtarı nı ekleyerek verimli bir şekilde yönlendirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure [Cosmos DB'de bölümleme ve yatay ölçekleme](partition-data.md)hakkında bilgi edinin.
* Azure [Cosmos DB'de sağlanan iş ortası](request-units.md)hakkında bilgi edinin.
* Azure [Cosmos DB'de küresel dağıtım](distribute-data-globally.md)hakkında bilgi edinin.
