---
title: Sorgular için Azure Tablo depolama alanı tasarla | Microsoft Dokümanlar
description: Azure Tablo depolama alanında sorgular için tablolar tasarla.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 41a588ddc0c1be8014a84d8fe181013d8566f68d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457632"
---
# <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo servis çözümleri yoğun okunabilir, yazma yoğun veya ikisinin bir karışımı. Bu makalede, okuma işlemlerini verimli bir şekilde desteklemek için Tablo hizmetinizi tasarlarken aklınızda tutulması gereken şeyler üzerinde duruluyor. Genellikle, okuma işlemlerini verimli bir şekilde destekleyen bir tasarım yazma işlemleri için de etkilidir. Ancak, [veri modifikasyonu için tasarım](table-storage-design-for-modification.md)makalesinde tartışılan yazma işlemlerini desteklerken akılda tutulması gereken ek hususlar vardır.

Verileri verimli bir şekilde okumanızı sağlamak için Tablo hizmeti çözümünüzü tasarlamak için iyi bir başlangıç noktası, "Uygulamamın Tablo hizmetinden ihtiyaç duyduğu verileri almak için hangi sorguları gerçekleştirmesi gerekir?" diye sormaktır.  

> [!NOTE]
> Tablo hizmeti ile, tasarımı önceden doğru hale getirmek önemlidir, çünkü daha sonra değiştirmek zor ve pahalıdır. Örneğin, ilişkisel bir veritabanında, yalnızca varolan bir veritabanına dizinler ekleyerek performans sorunlarını gidermek mümkündür: bu Tablo hizmetiyle ilgili bir seçenek değildir.  
> 
> 

Bu bölümde, sorgu için tablolarınızı tasarlarken ele almanız gereken temel konular üzerinde duruluyor. Bu bölümde ele alınan konular şunlardır:

* [PartitionKey ve RowKey seçiminiz sorgu performansını nasıl etkiler?](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için sorguları optimize etme](#optimizing-queries-for-the-table-service)
* [Tablo hizmetinde verileri sıralama](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçiminiz sorgu performansını nasıl etkiler?
Aşağıdaki örnekler, tablo hizmetinin çalışan varlıklarını aşağıdaki yapıyla depolayabettiğini varsayar (örneklerin çoğu netlik için **Zaman Damgası** özelliğini atlar):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **PartitionKey** (Bölüm Adı) |Dize |
| **RowKey** (Çalışan Kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **Yaş** |Tamsayı |
| **Emailaddress** |Dize |

Makale [Azure Tablo depolama genel bakış](table-storage-overview.md) sorgu için tasarım üzerinde doğrudan etkisi olan Azure Tablo hizmetinin bazı temel özellikleri açıklar. Bunlar, Tablo hizmeti sorgularının tasarımı için aşağıdaki genel yönergelerle sonuçlanır. Aşağıdaki örneklerde kullanılan filtre sözdizimi Tablo hizmeti REST API olduğunu unutmayın, daha fazla bilgi için [Sorgu Varlıklar](https://docs.microsoft.com/rest/api/storageservices/Query-Entities)bakın.  

* ***Nokta Sorgusu*** kullanılacak en verimli aramadır ve en düşük gecikme gerektiren yüksek hacimli aramalar veya aramalar için kullanılması önerilir. Böyle bir sorgu, **hem PartitionKey** hem de **RowKey** değerlerini belirterek tek bir varlığı çok verimli bir şekilde bulmak için dizinleri kullanabilir. Örneğin: $filter=(PartitionKey eq 'Satış') ve (RowKey eq '2')  
* İkinci en iyi, **PartitionKey'i** kullanan ve birden fazla varlığı döndürmek için **bir dizi RowKey** değerine filtre leyen bir ***Aralık Sorgusudur.*** **PartitionKey** değeri belirli bir bölümü tanımlar ve **RowKey** değerleri bu bölümdeki varlıkların bir alt kümesini tanımlar. Örneğin: $filter=PartitionKey eq 'Satış' ve RowKey ge 'S' ve RowKey lt 'T'  
* Üçüncü en iyi bölüm ***scan*** **partitionkey** ve filtreler başka bir anahtar olmayan özellik üzerinde kullanır ve birden fazla varlık döndürebilir. **PartitionKey** değeri belirli bir bölümü tanımlar ve özellik değerleri bu bölümdeki varlıkların bir alt kümesi için seçilir. Örneğin: $filter=PartitionKey eq 'Satış' ve LastName eq 'Smith'  
* ***Tablo Taramalari*** **PartitionKey'i** içermez ve eşleşen varlıklar için sırayla tablonuzu oluşturan tüm bölümleri aradığından çok verimsizdir. Filtrenizin **RowKey'i**kullanıp kullanmadığına bakılmaksızın bir tablo tonu gerçekleştirecektir. Örneğin: $filter=LastName eq 'Jones'  
* Birden çok varlığı döndüren sorgular, bunları **PartitionKey** ve **RowKey** sırasına göre sıralanmış olarak döndürür. İstemcideki varlıklara başvurmamak için, en yaygın sıralama sırasını tanımlayan bir **RowKey** seçin.  

**RowKey** değerlerini temel alan bir filtre belirtmek için "**veya**" kullanmanın bir bölüm taramayla sonuçladığını ve aralık sorgusu olarak kabul edilmediğine dikkat edin. Bu nedenle, gibi filtreler kullanan sorguları kaçınmalısınız: $filter=PartitionKey eq 'Satış' ve (RowKey eq '121' veya RowKey eq '322')  

Verimli sorgular yürütmek için Depolama İstemci Kitaplığı'nı kullanan istemci tarafı kodu örnekleri için bkz:  

* [Depolama İstemci Kitaplığı'nı kullanarak nokta sorgusunu yürütme](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden fazla varlık alma](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Sunucu tarafı projeksiyonu](table-storage-design-patterns.md#server-side-projection)  

Aynı tabloda depolanan birden çok varlık türünü işleyebilen istemci tarafı kodu örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
**PartitionKey** seçiminiz, varlıklarınızı birden çok bölüme dağıtma gereksinimine (ölçeklenebilir bir çözüm sağlamak için) EGT'lerin kullanımını etkinleştirme gereksinimini (tutarlılık sağlamak için) dengelemelidir.  

Bir uçta, tüm varlıklarınızı tek bir bölümde depolayabilir, ancak bu çözümünüzün ölçeklenebilirliğini sınırlayabilir ve tablo hizmetinin istekleri ni yükleyebilmelerini engelleyebilir. Diğer uçta, bölüm başına bir varlık depolayabilir, bu da yüksek ölçeklenebilir ve tablo hizmetinin istekleri yükdengesini sağlar, ancak bu da varlık grubu hareketlerini kullanmanızı engeller.  

İdeal **PartitionKey** verimli sorgular kullanmanızı sağlayan ve çözüm ölçeklenebilir olduğundan emin olmak için yeterli bölüme sahip biridir. Genellikle, varlıklarınız, varlıklarınızı yeterli bölümlere dağıtan uygun bir özelliğe sahip olacaktır.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgi depolayan bir sistemde UserID iyi bir PartitionKey olabilir. Bölüm anahtarı olarak belirli bir UserID kullanan birkaç varlığınız olabilir. Kullanıcı hakkında veri depolayan her varlık tek bir bölüm halinde gruplandırılır ve böylece bu varlıklara varlık grubu hareketleri aracılığıyla erişilebilir ve yine de yüksek ölçeklenebilir.
> 
> 

**PartitionKey** seçiminizde varlıkları nasıl ekleyeceğiniz, güncelleştireceğiniz ve sileceğinizle ilgili ek hususlar vardır. Daha fazla bilgi için [bkz.](table-storage-design-for-modification.md)  

## <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için sorguları optimize etme
Tablo hizmeti, tek bir kümelenmiş dizinde **PartitionKey** ve **RowKey** değerlerini kullanarak varlıklarınızı otomatik olarak dizine bağlar, bu nedenle nokta sorgularının kullanımı en verimli olan ın nedenidir. Ancak, **PartitionKey** ve **RowKey**üzerinde kümelenmiş dizin dışında dizin yoktur.

Birçok tasarım, birden çok ölçüte dayalı varlıkların görünümünü etkinleştirmek için gereksinimleri karşılamalıdır. Örneğin, çalışan varlıklarını e-postaya, çalışan kimliğine veya soyadına göre bulma. Tablo Tasarım [Desenleri'nde](table-storage-design-patterns.md) açıklanan desenler bu gereksinim türlerini ele almakta ve Tablo hizmetinin ikincil dizinler sağlamadığı gerçeği etrafında çalışma yollarını açıklar:  

* [Bölüm içi ikincil dizin deseni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - Hızlı ve verimli aramalar ve farklı **RowKey** değerlerini kullanarak alternatif sıralama siparişlerini etkinleştirmek için her varlığın farklı **RowKey** değerlerini (aynı bölümiçinde) kullanarak birden çok kopyasını depolayın.  
* [Bölümler arası ikincil dizin deseni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - Farklı **RowKey** değerlerini kullanarak hızlı ve verimli aramalar ve alternatif sıralama siparişleri etkinleştirmek için her varlığın farklı **RowKey** değerlerini kullanarak farklı bölümlerde veya ayrı tablolarda birden fazla kopyasını saklayın.  
* [Dizin Varlıklar Deseni](table-storage-design-patterns.md#index-entities-pattern) - Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  

## <a name="sorting-data-in-the-table-service"></a>Tablo hizmetinde verileri sıralama
Tablo **hizmeti, PartitionKey'e** ve ardından **RowKey'e**göre artan sırada sıralanmış varlıkları döndürür. Bu tuşlar dize değerleridir ve sayısal değerlerin doğru sıraladığından emin olmak için, bunları sabit bir uzunluğa dönüştürmeli ve sıfırlarla padlemeniz gerekir. Örneğin, **RowKey** olarak kullandığınız çalışan kimlik değeri bir integer değeriyse, çalışan kimliğini **123'ten** **00000123'e**dönüştürmeniz gerekir.  

Birçok uygulamanın farklı siparişlerde sıralanmış verileri kullanmak için gereksinimleri vardır: örneğin, çalışanları ada göre sıralama veya tarihe katılma. Aşağıdaki desenler, varlıklarınız için siparişleri nasıl sıralayabildiğini ele aürün dir:  

* [Bölüm içi ikincil dizin deseni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - Hızlı ve verimli aramalar ve farklı RowKey değerlerini kullanarak alternatif sıralama siparişlerini etkinleştirmek için her varlığın farklı RowKey değerlerini (aynı bölümiçinde) kullanarak birden çok kopyasını depolayın.  
* [Bölümler arası ikincil dizin deseni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - Farklı RowKey değerlerini kullanarak hızlı ve verimli aramalar ve alternatif sıralama siparişleri etkinleştirmek için ayrı tablolarda ayrı bölümlerde farklı RowKey değerlerini kullanarak her varlığın birden çok kopyasını saklayın.
* [Günlük kuyruk deseni](table-storage-design-patterns.md#log-tail-pattern) - En son bir bölüme eklenen *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir **RowKey** değeri kullanarak alın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
