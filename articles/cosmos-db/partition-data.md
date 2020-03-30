---
title: Azure Cosmos DB'de bölümleme ve yatay ölçeklendirme
description: Azure Cosmos DB'de bölümlemenin nasıl çalıştığı, bölümleme ve bölümleme anahtarlarını nasıl yapılandırılabildiğini ve uygulamanız için doğru bölüm anahtarını nasıl seçeceğinizi öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: cbd171e10cc1a8b27de98d9d4d779f345ac5a3ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246623"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Azure Cosmos DB'de bölümleme ve yatay ölçeklendirme

Bu makalede, Azure Cosmos DB’deki fiziksel ve mantıksal bölümler açıklanmaktadır. Makale, ölçeklendirme ve bölümlemeye yönelik en iyi yöntemleri de ele alır. 

## <a name="logical-partitions"></a>Mantıksal bölümler

Mantıksal bölüm, aynı bölüm anahtarına sahip bir öğe kümesinden oluşur. Örneğin, tüm öğelerin bir özellik `City` içerdiği bir `City` kapsayıcıda, kapsayıcının bölüm anahtarı olarak kullanabilirsiniz. Için özel değerlere sahip `City`öğelerin `Paris`grupları `NYC`, , , ve , gibi farklı mantıksal bölümler oluşturur. `London` Temel veriler silindiğinde bir bölümü silme konusunda endişelenmenize gerek yoktur.

Azure Cosmos DB'de kapsayıcı ölçeklenebilirliğin temel birimidir. Kapsayıcıya eklenen veriler ve kapsayıcıda sağlamayaptığınız iş kaynağı otomatik olarak (yatay olarak) bir dizi mantıksal bölüm e göre bölümlenir. Veriler ve iş ortası, Azure Cosmos kapsayıcısı için belirttiğiniz bölümleme anahtarına göre bölümlere ayrılmıştır. Daha fazla bilgi için [bkz.](how-to-create-container.md)

Mantıksal bir bölüm de veritabanı hareketlerinin kapsamını tanımlar. [Anlık görüntü yalıtımı olan](database-transactions-optimistic-concurrency.md)bir hareket kullanarak mantıksal bir bölüm içindeki öğeleri güncelleştirebilirsiniz. Bir kapsayıcıya yeni öğeler eklendiğinde, sistem tarafından saydam olarak yeni mantıksal bölümler oluşturulur.

## <a name="physical-partitions"></a>Fiziksel bölümler

Azure Cosmos kapsayıcısı, çok sayıda mantıksal bölüme veri ve iş bölümü dağıtılarak ölçeklendirilir. Dahili olarak, bir veya daha fazla mantıksal bölüm, [*yineleme kümesinden*](global-dist-under-the-hood.md)oluşan fiziksel bir bölüme eşlenir, ayrıca çoğaltma kümesi olarak da adlandırılır. Her yineleme kümesi Azure Cosmos veritabanı altyapısının bir örneğini barındırıyor. Yineleme kümesi, fiziksel bölüm içinde depolanan verileri dayanıklı, kullanılabilir ve tutarlı hale getirir. Fiziksel bölüm, maksimum depolama ve istek birimi (RUs) miktarını destekler. Fiziksel bölümü oluşturan her yineleme, bölümün depolama kotasını devralır. Fiziksel bölümün tüm kopyaları, fiziksel bölüme ayrılan iş kısmını topluca destekler. 

Aşağıdaki resim, mantıksal bölümlerin genel olarak dağıtılan fiziksel bölümlere nasıl eşlendiğini gösterir:

![Azure Cosmos DB bölümleme gösteren bir görüntü](./media/partition-data/logical-partitions.png)

Bir kapsayıcı için sağlanan iş, fiziksel bölümler arasında eşit olarak bölünür. İş verme isteklerini eşit olarak dağıtmayan bir bölüm anahtarı tasarımı "sıcak" bölümler oluşturabilir. Sıcak bölümler, hız sınırlaması ve sağlanan iş ortası ve daha yüksek maliyetlerin verimsiz kullanımına neden olabilir.

Mantıksal bölümlerin aksine, fiziksel bölümler sistemin iç sel uygulamasıdır. Fiziksel bölümlerin boyutunu, yerleşimini veya sayısını denetleyemezsiniz ve mantıksal bölümler ve fiziksel bölümler arasındaki eşlemi denetemezsiniz. Ancak, [doğru mantıksal bölüm anahtarı seçerek](partitioning-overview.md#choose-partitionkey)mantıksal bölüm sayısını ve veri, iş yükü ve iş bölümü dağılımını denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bölüm [anahtarı seçme](partitioning-overview.md#choose-partitionkey)hakkında bilgi edinin.
* Azure [Cosmos DB'de sağlanan iş ortası](request-units.md)hakkında bilgi edinin.
* Azure [Cosmos DB'de küresel dağıtım](distribute-data-globally.md)hakkında bilgi edinin.
* [Azure Cosmos kapsayıcısına nasıl iş veriş sağlamayı](how-to-provision-container-throughput.md)öğrenin.
* [Azure Cosmos veritabanında iş ortası sağlama](how-to-provision-database-throughput.md)yı öğrenin.
