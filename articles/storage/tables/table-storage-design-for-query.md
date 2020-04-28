---
title: Sorgular için Azure Tablo Depolaması tasarlama | Microsoft Docs
description: Azure Tablo depolamadaki sorgular için tabloları tasarlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 41a588ddc0c1be8014a84d8fe181013d8566f68d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75457632"
---
# <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo hizmeti çözümleri yoğun bir şekilde okunabilir, yoğun yazma veya ikisinin karışımı olabilir. Bu makalede, tablo hizmetinizi, okuma işlemlerini verimli bir şekilde destekleyecek şekilde tasarlarken göz önünde bulundurmanız gereken noktalar ele alınmaktadır. Genellikle, okuma işlemlerini etkin şekilde destekleyen bir tasarım, yazma işlemleri için de etkilidir. Bununla birlikte, [veri değişikliği Için tasarım tasarımında](table-storage-design-for-modification.md)bahsedilen yazma işlemlerini desteklemek için tasarlanırken dikkate alınması gereken ek hususlar vardır.

Tablo hizmeti çözümünüzü tasarlamak için iyi bir başlangıç noktası, verileri etkili bir şekilde okumanızı sağlamak üzere "uygulamamın hangi sorguları tablo hizmeti 'nden alması gerekir?" sormanız.  

> [!NOTE]
> Tablo hizmeti ile, daha sonra değiştirilmesi zor ve pahalı olduğundan tasarımın doğru olması önemlidir. Örneğin, ilişkisel bir veritabanında yalnızca var olan bir veritabanına dizinler ekleyerek performans sorunlarını ele almak mümkündür: Bu tablo hizmetinde bir seçenek değildir.  
> 
> 

Bu bölümde, tabloları sorgulamak için tasarlarken bilmeniz gereken önemli sorunlar ele alınmaktadır. Bu bölümde ele alınan konular şunları içerir:

* [PartitionKey ve RowKey seçimlerinizin sorgu performansını nasıl etkilediği](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için sorguları iyileştirme](#optimizing-queries-for-the-table-service)
* [Tablo hizmetindeki verileri sıralama](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçimlerinizin sorgu performansını nasıl etkilediği
Aşağıdaki örneklerde, tablo hizmetinin çalışan varlıklarını aşağıdaki yapıyla depoladığını varsaymaktadır (örneklerin çoğu açıklık için **zaman damgası** özelliğini atlayın):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **Partitionkey** (bölüm adı) |Dize |
| **Rowkey** (çalışan kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **Yaş** |Tamsayı |
| **EmailAddress** |Dize |

[Azure Tablo depolama genel bakış](table-storage-overview.md) makalesi, Azure Tablo hizmeti 'nin sorgu tasarımı üzerinde doğrudan bir etkisi olan temel özelliklerden bazılarını açıklar. Bunlar, tablo hizmeti sorguları tasarlamak için aşağıdaki genel yönergelere yol açabilir. Aşağıdaki örneklerde kullanılan filtre sözdiziminin tablo hizmeti REST API olduğunu unutmayın. daha fazla bilgi için bkz. [Sorgu varlıkları](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* ***Nokta sorgusu*** , en verimli arama ve en düşük gecikme süresi gerektiren aramalar için kullanılması önerilir. Bu tür bir sorgu, her iki **partitionkey** ve **rowkey** değerlerini belirterek, her bir varlığı çok verimli bir şekilde bulmak için dizinleri kullanabilir. Örneğin: $filter = (PartitionKey EQ ' Sales ') ve (RowKey EQ ' 2 ')  
* İkinci değer en iyisi, birden fazla varlık döndürmek için **partitionkey** değerlerini bir aralıktaki **rowkey** değerleri ve filtreleri kullanan bir ***Aralık sorgusudur*** . **Partitionkey** değeri belirli bir bölümü tanımlar ve **rowkey** değerleri söz konusu bölümdeki varlıkların bir alt kümesini tanımlar. Örneğin: $filter = PartitionKey EQ ' Sales ' ve RowKey Ge ' ve RowKey lt 'T '  
* Üçüncü en iyisi, bir diğer anahtar olmayan özellikte ve birden fazla varlık döndürebilen **Partitionkey** ve Filters kullanan bir ***bölüm taramasından*** biridir. **Partitionkey** değeri belirli bir bölümü tanımlar ve özellik değerleri söz konusu bölümdeki varlıkların bir alt kümesini seçer. Örneğin: $filter = PartitionKey EQ ' Sales ' ve LastName EQ ' Smith '  
* ***Tablo taraması*** **partitionkey** içermez ve tüm eşleşen varlıklar için tabloyu oluşturan tüm bölümleri aradığı için çok verimsiz olur. Filtrenizin **Rowkey**kullanıp kullanmadığını bakılmaksızın tablo taraması gerçekleştirir. Örneğin: $filter = LastName EQ ' Jones '  
* Birden çok varlık döndüren sorgular, bunları **partitionkey** ve **rowkey** sırasına göre sıralanmış olarak döndürür. İstemcideki varlıkları yeniden kullanmaktan kaçınmak için en yaygın sıralama düzenini tanımlayan bir **Rowkey** seçin.  

**Rowkey** değerlerini temel alan bir filtre belirtmek için "**veya**" kullanmanın, Bölüm taramasına neden olduğunu ve Aralık sorgusu olarak değerlendirilmediğini unutmayın. Bu nedenle, şu gibi filtreler kullanan sorgulardan kaçınmalısınız: $filter = PartitionKey EQ ' Sales ' ve (RowKey EQ ' 121 ' veya RowKey EQ ' 322 ')  

Etkili sorgular yürütmek için depolama Istemci kitaplığını kullanan istemci tarafı kodu örnekleri için, bkz.:  

* [Depolama Istemci kitaplığını kullanarak bir nokta sorgusu yürütme](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlık alma](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Sunucu tarafı projeksiyonu](table-storage-design-patterns.md#server-side-projection)  

Aynı tabloda depolanan birden çok varlık türünü işleyebilen istemci tarafı kodu örnekleri için, bkz.:  

* [Heterojen varlık türleriyle çalışma](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
**Partitionkey** tercih ettiğiniz bölüm, varlıklarınızı birden çok bölüme dağıtmak (ölçeklenebilir bir çözüm sağlamak için) gereksinimini karşılamak Için gereken yumurtların kullanılmasını (tutarlılık sağlamak için) sağlar.  

Bir Extreme 'de, tüm varlıklarınızı tek bir bölümde saklayabilirsiniz, ancak bu, çözümünüzün ölçeklenebilirliğini sınırlayabilir ve tablo hizmeti 'nin istekleri yük dengelemesine engel olabilir. Diğer bir deyişle, bölüm başına bir varlık saklayabilir, bu durum yüksek düzeyde ölçeklenebilir olur ve tablo hizmetini yük dengeleme isteklerini ve bu sayede varlık grubu işlemlerini kullanmanızı önler.  

İdeal bir **Partitionkey** , çözümünüzün ölçeklenebilir olduğundan emin olmak için verimli sorgular kullanmanıza ve yeterli bölüm olmasına olanak sağlar. Genellikle, varlıklarınızın varlıklarınızı yeterli bölümler arasında dağıtan uygun bir özelliği olacağını göreceksiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgi depolayan bir sistemde, Kullanıcı kimliği iyi bir PartitionKey olabilir. Bölüm anahtarı olarak belirli bir kullanıcı kimliğini kullanan birkaç varlık olabilir. Bir kullanıcı hakkında veri depolayan her varlık tek bir bölümde gruplandırılır ve bu varlıkların varlık grubu işlemleri aracılığıyla erişilebilir, ancak hala yüksek oranda ölçeklenebilir hale getirilir.
> 
> 

Varlık **anahtarınız** , varlıkları ekleme, güncelleştirme ve silme ile ilgili ek hususlar vardır. Daha fazla bilgi için bkz. [veri değişikliği için tabloları tasarlama](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için sorguları iyileştirme
Tablo hizmeti, tek bir kümelenmiş dizindeki **partitionkey** ve **rowkey** değerlerini kullanarak varlıklarınızı otomatik olarak dizinlenir, bu nedenle nokta sorgularının kullanım açısından en verimli olması neden olur. Ancak, **partitionkey** ve **rowkey**üzerindeki kümelenmiş dizinin dışında başka bir dizin yoktur.

Birçok tasarım, varlıkların birden çok ölçüte göre aramasını etkinleştirmek için gereksinimlere uymalıdır. Örneğin, e-posta, çalışan kimliği veya soyadı temelinde çalışan varlıklarını bulma. [Tablo tasarım desenlerinde](table-storage-design-patterns.md) açıklanan desenler, bu tür gereksinimleri ele almaz ve tablo hizmetinin ikincil dizinler sağlamadığı gerçeğe geçici çözüm yolları açıklanmaktadır:  

* [Bölüm içi ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) **-farklı** rowkey değerleri kullanılarak hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirmek için, her bir varlığın farklı **rowkey** değerlerini kullanarak birden çok kopyasını depolayın (aynı bölümde).  
* [Bölümler arası ikincil dizin düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -ayrı bölümlerde veya ayrı tablolarda farklı **rowkey** değerleri kullanarak her bir varlığın birden çok kopyasını depolayın ve farklı **rowkey** değerleri kullanarak hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirin.  
* [Dizin varlıkları model](table-storage-design-patterns.md#index-entities-pattern) -varlık listeleri döndüren etkili aramaları etkinleştirmek için Dizin varlıklarını koruyun.  

## <a name="sorting-data-in-the-table-service"></a>Tablo hizmetindeki verileri sıralama
Tablo hizmeti, **partitionkey** ve sonra **rowkey**'e göre artan sırada sıralanan varlıkları döndürür. Bu anahtarlar dize değerleridir ve sayısal değerlerin doğru şekilde sıralanmasını sağlamak için, bunları sabit bir uzunluğa dönüştürmeniz ve sıfırlarla birlikte ayarlayabilmeniz gerekir. Örneğin, **Rowkey** olarak kullandığınız çalışan kimliği değeri bir tamsayı değeri ise, **123** çalışan kimliğini **00000123**olarak dönüştürmeniz gerekir.  

Birçok uygulamanın, farklı siparişlerde sıralanmış verileri kullanma gereksinimleri vardır: Örneğin, çalışanları ada göre veya birleştirme tarihine göre sıralama. Aşağıdaki desenler, varlıklarınız için sıralama emirlerinin nasıl farklı olduğunu ele almalardır:  

* [Bölüm içi ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -farklı rowkey değerleri kullanılarak hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirmek için, her bir varlığın farklı rowkey değerlerini kullanarak birden çok kopyasını depolayın (aynı bölümde).  
* [Bölümler arası ikincil dizin düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -farklı rowkey değerleri kullanarak, hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirmek üzere ayrı bölümlerde ayrı bölümlerdeki farklı rowkey değerleri kullanarak her bir varlığın birden çok kopyasını saklayın.
* [Günlük kuyruğu düzeni](table-storage-design-patterns.md#log-tail-pattern) -ters tarih ve saat düzeninde sıralama yaparak bir **rowkey** değeri kullanarak bir bölüme en son eklenen *n* varlıklarını alın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
