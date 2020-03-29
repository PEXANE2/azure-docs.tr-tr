---
title: Azure Cosmos DB Gremlin API için grafik veri modellemesi
description: Azure Cosmos DB Gremlin API'yi kullanarak grafik veritabanını nasıl modellendireceklerini öğrenin. Bu makalede, varlıkları ve ilişkileri modellemek için grafik veritabanı ve en iyi uygulamaları kullanmak için ne zaman açıklanır.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: lbosq
ms.openlocfilehash: dc9a5616aa2bb1f7e09045b9cfe4f4d7e9c69be2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898322"
---
# <a name="graph-data-modeling-for-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB Gremlin API için grafik veri modellemesi

Aşağıdaki belge grafik veri modelleme önerileri sağlamak için tasarlanmıştır. Bu adım, veriler geliştikçe grafik veritabanı sisteminin ölçeklenebilirliğini ve performansını sağlamak için hayati önem taşır. Verimli bir veri modeli özellikle büyük ölçekli grafikler için önemlidir.

## <a name="requirements"></a>Gereksinimler

Bu kılavuzda özetlenen işlem aşağıdaki varsayımlara dayanmaktadır:
 * Sorun-uzaydaki **varlıklar** tanımlanır. Bu varlıklar her istek için _atomik olarak_ tüketilmelidir. Başka bir deyişle, veritabanı sistemi birden çok sorgu isteğinde tek bir varlığın verilerini almak için tasarlanmaz.
 * Veritabanı sistemi için **okuma ve yazma gereksinimleri** bir anlayış vardır. Bu gereksinimler, grafik veri modeli için gereken optimizasyonları yönlendirecektir.
 * [Apache Tinkerpop özellik grafiği standardının](https://tinkerpop.apache.org/docs/current/reference/#graph-computing) ilkeleri iyi anlaşılmıştır.

## <a name="when-do-i-need-a-graph-database"></a>Grafik veritabanına ne zaman ihtiyacım var?

Bir veri etki alanındaki varlıklar ve ilişkiler aşağıdaki özelliklerden herhangi biri varsa, grafik veritabanı çözümü en iyi şekilde uygulanabilir: 

* Varlıklar son derece açıklayıcı ilişkiler yoluyla **bağlı.** Bu senaryodaki yarar, ilişkilerin depolamada kalıcı olmasıdır.
* **Döngüsel ilişkiler** veya **kendini referans alan varlıklar**vardır. Bu desen genellikle ilişkisel veya belge veritabanları kullanırken bir sorundur.
* Varlıklar arasında **dinamik olarak gelişen ilişkiler** vardır. Bu desen özellikle birçok düzeyi olan hiyerarşik veya ağaç yapılı veriler için geçerlidir.
* Varlıklar arasında **çok-çok ilişkileri** vardır.
* Hem **varlıklar hem de ilişkiler de yazma ve okuma gereksinimleri**vardır. 

Yukarıdaki ölçütler karşılanırsa, grafik veritabanı yaklaşımının **sorgu karmaşıklığı,** veri **modeli ölçeklenebilirliği**ve **sorgu performansı**için avantajlar sağlaması olasıdır.

Bir sonraki adım, grafiğin analitik veya işlemamaçlı olarak kullanıp kullanılmayacığını belirlemektir. Grafik ağır hesaplama ve veri işleme iş yükleri için kullanılmak üzere tasarlanmıştır, [cosmos DB Spark bağlayıcı](https://docs.microsoft.com/azure/cosmos-db/spark-connector) ve [GraphX kitaplığı](https://spark.apache.org/graphx/)kullanımını keşfetmek için değer olacaktır. 

## <a name="how-to-use-graph-objects"></a>Grafik nesneleri nasıl kullanılır?

[Apache Tinkerpop özellik grafiği standart](https://tinkerpop.apache.org/docs/current/reference/#graph-computing) nesneleri **Vertices** ve **Kenarları**iki tür tanımlar. 

Grafik nesnelerindeki özellikler için en iyi uygulamalar şunlardır:

| Nesne | Özellik | Tür | Notlar |
| --- | --- | --- |  --- |
| Köşe | Kimlik | Dize | Bölüm başına benzersiz bir şekilde uygulanır. Bir değer ekleme üzerine sağlanmazsa, otomatik olarak oluşturulan GUID depolanır. |
| Köşe | etiket | Dize | Bu özellik, tepe noktasının temsil ettiği varlık türünü tanımlamak için kullanılır. Bir değer sağlanmazsa, varsayılan değer "tepe noktası" kullanılır. |
| Köşe | properties | Yaylı Çalgılar, Boolean, Sayısal | Her tepe de anahtar değer çiftleri olarak depolanan ayrı özelliklerin listesi. |
| Köşe | bölüm tuşu | Yaylı Çalgılar, Boolean, Sayısal | Bu özellik, tepe noktasının ve giden kenarlarının nerede depolanacağını tanımlar. [Grafik bölümleme](graph-partitioning.md)hakkında daha fazla bilgi edinin. |
| Edge | Kimlik | Dize | Bölüm başına benzersiz bir şekilde uygulanır. Varsayılan olarak otomatik olarak oluşturulur. Kenarlar genellikle bir kimlik tarafından benzersiz olarak alınması gerekmez. |
| Edge | etiket | Dize | Bu özellik, iki vertices sahip ilişki türünü tanımlamak için kullanılır. |
| Edge | properties | Yaylı Çalgılar, Boolean, Sayısal | Her kenarda anahtar değer çiftleri olarak depolanan ayrı özelliklerin listesi. |

> [!NOTE]
> Değeri kaynak tepe noktalarına göre otomatik olarak atanır, çünkü kenarları bir bölüm anahtar değeri gerektirmez. [Grafik bölümleme](graph-partitioning.md) makalesinde daha fazla bilgi edinin.

## <a name="entity-and-relationship-modeling-guidelines"></a>Varlık ve ilişki modelleme yönergeleri

Aşağıda, Azure Cosmos DB Gremlin API grafik veritabanı için veri modellemeye yaklaşım için bir kılavuz kümesi verilmiştir. Bu yönergeler, bir veri etki alanının varolan bir tanımı olduğunu ve bunun için sorgular olduğunu varsayar.

> [!NOTE]
> Aşağıda özetlenen adımlar öneriler olarak sunulmuştur. Son model, üretime hazır olarak değerlendirilmeden önce değerlendirilmeli ve test edilmelidir. Ayrıca, aşağıdaki öneriler Azure Cosmos DB'nin Gremlin API uygulamasına özgüdür. 

### <a name="modeling-vertices-and-properties"></a>Vertices ve özellikleri modelleme 

Grafik veri modeli için ilk adım, tanımlanan her varlığı bir **tepe noktası nesnesi**ile eşlemektir. Tüm varlıkların vertices için bire bir eşleme bir ilk adım ve değişime tabi olmalıdır.

Ortak bir tuzak ayrı vertices olarak tek bir varlığın özelliklerini haritalamaktır. Aynı varlığın iki farklı şekilde temsil edildiği aşağıdaki örneği göz önünde bulundurun:

* **Vertex tabanlı özellikleri**: Bu yaklaşımda, varlık özelliklerini tanımlamak için üç ayrı tepe noktası ve iki kenar kullanır. Bu yaklaşım artıklığı azaltabilir, ancak model karmaşıklığını artırır. Model karmaşıklığındaki artış, ek gecikme, sorgu karmaşıklığı ve hesaplama maliyetine neden olabilir. Bu model aynı zamanda bölümleme zorlukları sunabilir.

![Özellikleri için vertices ile varlık modeli.](./media/graph-modeling/graph-modeling-1.png)

* **Özellik gömülü vertices**: Bu yaklaşım, bir tepe noktası içindeki varlığın tüm özelliklerini temsil etmek için anahtar değeri çifti listesinden yararlanır. Bu yaklaşım, daha basit sorgular ve daha düşük maliyetli geçişler yol açacaktır azaltılmış model karmaşıklığı sağlar.

![Özellikleri için vertices ile varlık modeli.](./media/graph-modeling/graph-modeling-2.png)

> [!NOTE]
> Yukarıdaki örnekler, varlık özelliklerini bölmenin iki yolu arasındaki karşılaştırmayı yalnızca göstermek için basitleştirilmiş bir grafik modelini gösterir.

**Özellik gömülü vertices** desen genellikle daha performant ve ölçeklenebilir bir yaklaşım sağlar. Yeni bir grafik veri modeline varsayılan yaklaşım bu desen doğru çekilmek gerekir.

Ancak, bir özelliğe başvurmanın avantaj sağlayabileceği senaryolar vardır. Örneğin: başvurulan özellik sık sık güncelleştirilmişse. Sürekli değiştirilen bir özelliği temsil etmek için ayrı bir vertex kullanmak, güncelleştirmenin gerektirdiği yazma işlemleri miktarını en aza indirir.

### <a name="relationship-modeling-with-edge-directions"></a>Kenar yönleri ile ilişki modelleme

Vertices modellendikten sonra, kenarlar aralarındaki ilişkileri belirtmek için eklenebilir. Değerlendirilmesi gereken ilk yönü **ilişkinin yönüdür.** 

Kenar `out()` nesneleri, işlevi `outE()` kullanırken bir geçiş tarafından izlenen varsayılan bir yöne sahiptir. Tüm tepe uçları giden kenarları ile depolanır beri bu doğal yön kullanarak verimli bir çalışma ile sonuçlanır. 

Ancak, `in()` işlevi kullanarak bir kenarın ters yönde geçiş, her zaman bir çapraz bölüm sorgusu neden olur. [Grafik bölümleme](graph-partitioning.md)hakkında daha fazla bilgi edinin. `in()` İşlevi kullanarak sürekli olarak geçiş yapmanız gerekirse, her iki yöne de kenarlar eklemeniz önerilir.

Gremlin adımına ait `.to()` `.from()` veya yüklemleri kullanarak kenar yönünü belirleyebilirsiniz. `.addE()` Veya [Gremlin API için toplu yürütme kitaplığı](bulk-executor-graph-dotnet.md)kullanarak .

> [!NOTE]
> Kenar nesneleri varsayılan olarak bir yöne sahiptir.

### <a name="relationship-labeling"></a>İlişki etiketleme

Açıklayıcı ilişki etiketleri kullanmak, kenar çözünürlüğü işlemlerinin verimliliğini artırabilir. Bu desen aşağıdaki şekillerde uygulanabilir:
* İlişkiyi etiketlemek için genel olmayan terimler kullanın.
* Kaynak tepe noktasının etiketini, hedef tepe noktasının etiketiyle ilişki adı ile ilişkilendirin.

![İlişki etiketleme örnekleri.](./media/graph-modeling/graph-modeling-3.png)

Geçiş incisinin kenarları filtrelemek için kullanacağı etiket ne kadar spesifikse, o kadar iyi. Bu karar, sorgu maliyeti üzerinde de önemli bir etkiye sahip olabilir. YürütmeProfil [adımını kullanarak](graph-execution-profile.md)istediğiniz zaman sorgu maliyetini değerlendirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar: 
* Desteklenen [Gremlin adımlarının](gremlin-support.md)listesine göz atın.
* Büyük ölçekli grafiklerle başa çıkmak için [grafik veritabanı bölümleme](graph-partitioning.md) hakkında bilgi edinin.
* [Yürütme Profili adımını](graph-execution-profile.md)kullanarak Gremlin sorgularınızı değerlendirin.
