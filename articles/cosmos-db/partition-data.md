---
title: Azure Cosmos DB'de bölümleme ve yatay ölçeklendirme
description: Bölümlemenin Azure Cosmos DB nasıl çalıştığına, bölümleme ve bölüm anahtarlarının nasıl yapılandırılacağını ve uygulamanız için doğru bölüm anahtarının nasıl seçeceğinizi öğrenin.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 19e4c61ba930bb9b127e2401174bcea3fd240dce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82234239"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Azure Cosmos DB'de bölümleme ve yatay ölçeklendirme

Bu makalede, mantıksal ve fiziksel bölümler arasındaki ilişki açıklanmaktadır. Ayrıca bölümlendirme için en iyi yöntemleri açıklar ve yatay ölçeklendirmenin Azure Cosmos DB nasıl çalıştığına ilişkin derinlemesine bir görünüm sağlar. [Bölüm anahtarınızı seçmek](partitioning-overview.md#choose-partitionkey) için bu iç ayrıntıları anlamanız gerekli değildir, ancak Azure Cosmos DB nasıl çalıştığına ilişkin netlik sahibi olmak üzere kapsandık.

## <a name="logical-partitions"></a>Mantıksal bölümler

Mantıksal bir bölüm, aynı bölüm anahtarına sahip bir öğe kümesinden oluşur. Örneğin, yiyecek ile ilgili verileri içeren bir kapsayıcıda, tüm öğeler bir `foodGroup` özellik içerir. Kapsayıcı için bölüm `foodGroup` anahtarı olarak ' i kullanabilirsiniz. `Beef Products`,`Baked Products`Ve gibi belirli değerlere `Sausages and Luncheon Meats` `foodGroup`sahip öğe grupları, ve gibi mantıksal bölümler oluşturur. Temel alınan veriler silindiğinde mantıksal bir bölümü silme konusunda endişelenmeniz gerekmez.

Mantıksal bir bölüm ayrıca veritabanı işlemlerinin kapsamını tanımlar. Bir mantıksal bölüm içindeki öğeleri, [anlık görüntü yalıtımıyla bir işlem](database-transactions-optimistic-concurrency.md)kullanarak güncelleştirebilirsiniz. Bir kapsayıcıya yeni öğeler eklendiğinde, yeni mantıksal bölümler sistem tarafından saydam olarak oluşturulur.

Kapsayıcıınızda mantıksal bölüm sayısı için bir sınır yoktur. Her mantıksal bölüm, 20 GB 'a kadar veri saklayabilir. İyi bölüm anahtarı seçimleri çok sayıda olası değer aralığına sahiptir. Örneğin, tüm öğelerin bir `foodGroup`özelliği içerdiği bir kapsayıcıda, `Beef Products` mantıksal bölümdeki veriler 20 GB 'a kadar büyüyebilir. Çok sayıda olası değeri olan [bir bölüm anahtarını seçmek](partitioning-overview.md#choose-partitionkey) kapsayıcının ölçeklenmesini sağlar.

## <a name="physical-partitions"></a>Fiziksel bölümler

Azure Cosmos kapsayıcısı, verileri ve aktarım hızını fiziksel bölümler arasında dağıtarak ölçeklendirilir. Dahili olarak, bir veya daha fazla mantıksal bölüm tek bir fiziksel bölüme eşlenir. Çoğu küçük Cosmos kapsayıcısının birçok mantıksal bölümü vardır ancak yalnızca tek bir fiziksel bölüm gerektirir. Mantıksal bölümlerin aksine, fiziksel bölümler sistemin dahili bir uygulamasıdır ve tamamen Azure Cosmos DB tarafından yönetilir.

Cosmos kapsayıcılarınızın fiziksel bölümlerinin sayısı aşağıdakilere bağlıdır:

- Sağlanan üretilen iş miktarı (her bir fiziksel bölüm, saniyede en fazla 10.000 istek birimi sağlar)
- Toplam veri depolaması (her bir fiziksel Bölüm 50 GB 'a kadar depolama alanı olabilir)

Kapsayıcıınızda bulunan toplam fiziksel bölüm sayısı için bir sınır yoktur. Sağlanan aktarım hızı veya veri boyutunuz büyüdükçe, Azure Cosmos DB var olanları bölerek otomatik olarak yeni fiziksel bölümler oluşturacak. Fiziksel bölüm bölmelerini uygulamanızın kullanılabilirliğini etkilemez. Fiziksel bölüm ayrıldıktan sonra, tek bir mantıksal bölüm içindeki tüm veriler aynı fiziksel bölümde saklanmaya devam eder. Fiziksel bölüm ayırma, mantıksal bölümlerin fiziksel bölümlere yeni bir eşlemesini oluşturur.

Bir kapsayıcı için sağlanan aktarım hızı fiziksel bölümler arasında eşit olarak bölünür. İşleme isteklerini eşit olarak dağıtmayan bölüm anahtarı tasarımı "sık erişimli" bölümler oluşturabilir. Sık kullanılan bölümler, sağlanan verimlilik ve daha yüksek maliyetler için hız sınırlaması ve verimsiz bir şekilde kullanılmasına neden olur.

Kapsayıcının fiziksel bölümlerinin Azure portal **ölçüm dikey** penceresinin **depolama** bölümünde görebilirsiniz:

[![Fiziksel bölüm](./media/partition-data/view-partitions-zoomed-out.png) sayısını görüntüleme](./media/partition-data/view-partitions-zoomed-in.png#lightbox)

Bölüm anahtarımız olarak seçtiğiniz `/foodGroup` Bu örnek kapsayıcıda, üç dikdörtgenin her biri fiziksel bir bölümü temsil eder. Görüntüde, **bölüm anahtar aralığı** fiziksel bir bölümle aynıdır. Seçilen fiziksel bölüm üç mantıksal bölüm içerir: `Beef Products`, `Vegetable and Vegetable Products`, ve. `Soups, Sauces, and Gravies`

Saniyede 18.000 istek birimi (RU/sn) aktarım hızı sağladığımızda, üç fiziksel bölümün her biri, sağlanan toplam üretilen iş üretiminin 1/3 ' i kullanabilir. Seçilen fiziksel bölümde, mantıksal bölüm anahtarları `Beef Products` `Vegetable and Vegetable Products`ve `Soups, Sauces, and Gravies` toplu olarak, fiziksel bölümün 6.000 tarafından sağlanan ru/s 'yi kullanabilir. Sağlanan aktarım hızı kapsayıcının fiziksel bölümlerine eşit olarak bölündüğü için [doğru mantıksal bölüm anahtarını seçerek](partitioning-overview.md#choose-partitionkey)işleme tüketimini eşit bir şekilde dağıtan bir bölüm anahtarı seçmeniz önemlidir. Mantıksal bölümlerde üretilen iş tüketimini eşit bir şekilde dağıtan bir bölüm anahtarı seçerseniz, fiziksel bölümlerde üretilen iş tüketiminin dengelenmesi güvence altına alınır.

## <a name="replica-sets"></a>Çoğaltma kümeleri

Her fiziksel bölüm, [*çoğaltma kümesi*](global-dist-under-the-hood.md)olarak da adlandırılan bir çoğaltmalar kümesinden oluşur. Her çoğaltma kümesi, Azure Cosmos veritabanı altyapısının bir örneğini barındırır. Bir çoğaltma kümesi, fiziksel bölümde depolanan verilerin dayanıklı, yüksek oranda kullanılabilir ve tutarlı olmasını sağlar. Fiziksel bölümü oluşturan her çoğaltma, bölümün depolama kotasını devralır. Fiziksel bölümün tüm çoğaltmaları, fiziksel bölüme ayrılan üretilen işi topluca destekler. Azure Cosmos DB çoğaltma kümelerini otomatik olarak yönetir.

Çoğu küçük Cosmos kapsayıcısı yalnızca tek bir fiziksel bölüm gerektirir ancak yine de en az 4 çoğaltma olur.

Aşağıdaki görüntüde, mantıksal bölümlerin küresel olarak dağıtılan fiziksel bölümlerle nasıl eşlendiği gösterilmektedir:

![Azure Cosmos DB bölümleme gösteren bir görüntü](./media/partition-data/logical-partitions.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Bölüm anahtarı seçme](partitioning-overview.md#choose-partitionkey)hakkında bilgi edinin.
* [Azure Cosmos DB 'de sağlanan aktarım hızı](request-units.md)hakkında bilgi edinin.
* [Azure Cosmos DB 'de küresel dağıtım](distribute-data-globally.md)hakkında bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.
