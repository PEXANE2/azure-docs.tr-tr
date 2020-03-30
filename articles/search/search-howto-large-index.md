---
title: Yerleşik dizin oluşturierler kullanarak büyük veri kümesini dizine
titleSuffix: Azure Cognitive Search
description: Toplu iş modu, kaynak kullanımı ve zamanlanmış, paralel ve dağıtılmış dizin oluşturma teknikleri aracılığıyla büyük veri dizini oluşturma veya hesaplama lı yoğun dizin oluşturma stratejileri.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 4ad5e961e390b60784355ff3bc72aca4a2f73e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190957"
---
# <a name="how-to-index-large-data-sets-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da büyük veri kümelerini dizine verme

Veri birimleri büyüdükçe veya işleme gereksinimleri değiştikçe, basit veya varsayılan dizin oluşturma stratejilerinin artık pratik olmadığını görebilirsiniz. Azure Bilişsel Arama için, veri yükleme isteğini nasıl yapılandırdığınıza, zamanlanmış ve dağıtılmış iş yükleri için kaynağa özgü bir dizin oluşturmaya kadar daha büyük veri kümelerini barındıran çeşitli yaklaşımlar vardır.

Aynı teknikler uzun süren süreçler için de geçerlidir. Özellikle, [paralel dizin oluşturmada](#parallel-indexing) özetlenen adımlar, görüntü çözümlemesi veya [bir AI zenginleştirme ardışık boru hattında](cognitive-search-concept-intro.md)doğal dil işleme gibi hesaplama açısından yoğun dizinoluşturma için yararlıdır.

Aşağıdaki bölümlerde büyük miktarda veri dizine ekinleme için üç teknik inceleyi.

## <a name="option-1-pass-multiple-documents"></a>Seçenek 1: Birden çok belge yi geç

Daha büyük bir veri kümesini dizine ekinyapmak için en basit mekanizmalardan biri, tek bir istekte birden çok belge veya kayıt göndermektir. Tüm yük 16 MB'ın altında olduğu sürece, bir istek toplu yükleme işleminde en fazla 1000 belgeyi işleyebilir. Bu sınırlar, .NET SDK'daki [Belge Ekle API'sını](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) veya [Dizin yöntemini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) kullanıyor sanız geçerlidir. Her iki API için de, her isteğin gövdesinde 1000 belge paketlersiniz.

Toplu iş dizini oluşturma, REST veya .NET kullanarak veya dizinleyiciler aracılığıyla tek tek istekler için uygulanır. Birkaç dizinleyici farklı sınırlar altında çalışır. Azure Blob dizin oluşturma, daha büyük ortalama belge boyutunun tanınması için toplu iş boyutunu 10 belge olarak ayarlar. Dizin oluştur [Dizinleyici REST API'sini](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer)temel `BatchSize` alan dizin oluşturcular için, bu ayarı verilerinizin özellikleriyle daha iyi eşleşecek şekilde özelleştirecek bağımsız değişkeni ayarlayabilirsiniz. 

> [!NOTE]
> Belge boyutunu düşük tutmak için, bir dizin için sorgulanabilir olmayan veriler eklemekten kaçının. Görüntüler ve diğer ikili veriler doğrudan aranabilir değildir ve dizinde depolanmamalıdır. Sorgulanamayan verileri arama sonuçlarına entegre etmek için, kaynağa URL başvurusu depolayan aranamayan bir alan tanımlamanız gerekir.

## <a name="option-2-add-resources"></a>Seçenek 2: Kaynak ekleme

[Standart fiyatlandırma katmanlarından](search-sku-tier.md) birinde sağlanan hizmetler genellikle hem depolama hem de iş yükleri (sorgular veya dizin oluşturma) için yetersiz kapasiteye sahiptir ve bu da [bölmeve yinelemenin artırılmasını](search-capacity-planning.md) daha büyük veri kümelerini barındıran bariz bir çözüm yapar. En iyi sonuçlar için her iki kaynağa da ihtiyacınız vardır: depolama için bölümler ve veri alma çalışması için yinelemeler.

Çoğaltmaları ve bölümleri artırmak, maliyetinizi artıran faturalandırılabilir olaylardır, ancak sürekli olarak maksimum yük altında dizine ekleme diğiniz sürece, dizin oluşturma işlemi süresince ölçek ekleyebilir ve dizin oluşturma işleminden sonra kaynak düzeylerini aşağı doğru ayarlayabilirsiniz Bit -miş.

## <a name="option-3-use-indexers"></a>Seçenek 3: Dizin oluşturma

[Dizin leyiciler,](search-indexer-overview.md) desteklenen Azure veri kaynaklarını aranabilir içerik için taramak için kullanılır. Özellikle büyük ölçekli dizin oluşturma için tasarlanmamış olsa da, çeşitli dizinleyici yetenekleri özellikle daha büyük veri kümelerini barındırmak için yararlıdır:

+ Zamanlayıcılar, zamana yayabilmeniz için dizini düzenli aralıklarla parsellemenize olanak sağlar.
+ Zamanlanmış dizin oluşturma bilinen son durdurma noktasında devam edebilir. Bir veri kaynağı 24 saatlik bir süre içinde tam olarak taranmazsa, dizinleyici kaldığı her yerde ikinci günde dizini ekingörevine devam edecektir.
+ Verilerin daha küçük tek tek veri kaynaklarına bölünmesi paralel işleme sağlar. Kaynak verileri Azure Blob depolamasındabirden çok kapsayıcıya ayırabilir ve azure bilişsel aramada paralel olarak dizine ekilebilen karşılık gelen, birden çok [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) oluşturabilirsiniz.

> [!NOTE]
> Dizin leyiciler veri kaynağına özgüdür, bu nedenle dizinleyici yaklaşımı kullanmak yalnızca Azure'daki seçili veri kaynakları için kullanılabilir: [SQL Veritabanı,](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) [Blob depolama,](search-howto-indexing-azure-blob-storage.md) [Tablo depolama,](search-howto-indexing-azure-tables.md) [Cosmos DB](search-howto-index-cosmosdb.md).

### <a name="scheduled-indexing"></a>Zamanlanmış dizin oluşturma

Dizin leyici zamanlama, büyük veri kümelerinin işlenmesinin yanı sıra bilişsel arama ardışık bir ardışık alanda görüntü analizi gibi yavaş çalışan işlemler için önemli bir mekanizmadır. Dizinleyici işleme 24 saatlik bir süre içinde çalışır. İşlem 24 saat içinde tamamlanırsa, dizin leyici zamanlamanın davranışları sizin yararınıza çalışabilir. 

Tasarım olarak, zamanlanmış dizin oluşturma belirli aralıklarla başlar ve bir sonraki zamanlanan aralıkta devam etmeden önce genellikle tamamlanan bir iş ile. Ancak, işlem aralık içinde tamamlanmazsa, dizinleyici durur (çünkü zaman tükendi. Sonraki aralıkta, işleme, sistemin nerede oluştuğunu izlemesi yle, en son kaldığı yerden devam eder. 

Pratik açıdan, birkaç güne yayılan dizin yükleri için dizin leyiciyi 24 saatlik bir zamanlamaya koyabilirsiniz. Dizin oluşturma sonraki 24 saatlik döngü için devam ettiğinde, bilinen son iyi belgede yeniden başlatılır. Bu şekilde, işlenebilir tüm belgeler işlenene kadar bir dizi gün içinde bir belge biriktirme listesi ile yolunu çalışabilir. Bu yaklaşım hakkında daha fazla bilgi için Azure [Blob depolama alanında büyük veri kümelerini dizine ekin oluşturma'ya](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)bakın. Zamanlamaları genel olarak ayarlama hakkında daha fazla bilgi için, Dizin [Oluştur REST API'sine](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) bakın veya [Azure Bilişsel Arama için dizin oluşturucuları nasıl zamanlayabilirsiniz'a](search-howto-schedule-indexers.md)bakın.

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Paralel dizin oluşturma

Paralel dizin oluşturma stratejisi, her veri kaynağı tanımının verilerin bir alt kümesini belirlediği birden çok veri kaynağını bir arada dizine ekleyerek temel adatır. 

Bilişsel arama boru hattında, görüntü analizinde veya doğal dil işlemede taranmış belgelerdeki OCR gibi rutin olmayan, hesaplama açısından yoğun indeksleme gereksinimleri için, paralel dizinoluşturma stratejisi genellikle bir en kısa sürede uzun süren bir süreç. Sorgu isteklerini ortadan kaldırabilir veya azaltabilirseniz, sorguları aynı anda işlemeyan bir hizmette paralel dizine ekleme, büyük bir yavaş işleme içeriği gövdesi nde çalışmak için en iyi strateji seçeneğinizdir. 

Paralel işleme şu öğelere sahiptir:

+ Kaynak verileri aynı kapsayıcının içinde birden çok kapsayıcı veya birden çok sanal klasör arasında bölmek. 
+ Harita her mini veri seti kendi [veri kaynağı,](https://docs.microsoft.com/rest/api/searchservice/create-data-source)kendi [dizinleyici](https://docs.microsoft.com/rest/api/searchservice/create-indexer)ile eşleştirilmiş.
+ Bilişsel arama için, her dizinleyici tanımında aynı [skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) başvuru.
+ Aynı hedef arama dizinine yazın. 
+ Tüm dizinleyicileri aynı anda çalışacak şekilde zamanlayın.

> [!NOTE]
> Azure Bilişsel Arama'da, tek tek yinelemeleri veya bölümleri dizin oluşturma veya sorgu işlemeye atamazsınız. Sistem kaynakların nasıl kullanılacağını belirler. Sorgu performansı üzerindeki etkisini anlamak için, bir test ortamında paralel dizin oluşturmayı deneyebilirsiniz.  

### <a name="how-to-configure-parallel-indexing"></a>Paralel dizin oluşturma nasıl yapılandırılır?

Dizin leyiciler için, işleme kapasitesi, arama hizmetiniz tarafından kullanılan her hizmet birimi (SU) için bir dizinleyici alt sistemine dayanır. Temel veya Standart katmanlarda en az iki yinelemeye sahip Azure Bilişsel Arama hizmetlerinde birden çok eşzamanlı dizinleyici mümkündür. 

1. Azure [portalında](https://portal.azure.com), arama hizmeti **panosuna Genel Bakış** sayfanızda, paralel dizin oluşturmayı karşılayabileceğinden onaylamak için Fiyatlandırma **katmanını** kontrol edin. Hem Temel hem de Standart katmanlar birden çok yineleme sunar.

2. **Ayarlar** > **Ölçeği'nde,** paralel işleme için [yinelemeleri artırın:](search-capacity-planning.md) her dizinleyici iş yükü için bir ek yineleme. Varolan sorgu hacmi için yeterli sayı bırakın. Dizin oluşturma için sorgu iş yüklerinden ödün vermek iyi bir dengeleme değildir.

3. Verileri Azure Bilişsel Arama dizinleyicilerin ulaşabileceği bir düzeyde birden çok kapsayıcıya dağıtın. Bu, Azure SQL Veritabanı'nda birden çok tablo, Azure Blob depolama alanında birden çok kapsayıcı veya birden çok koleksiyon olabilir. Her tablo veya kapsayıcı için bir veri kaynağı nesnesi tanımlayın.

4. Paralel olarak çalışacak birden çok dizin oluştur ve zamanlama:

   + Altı kopyaiçeren bir hizmeti varsayalım. Tüm veri kümesinin 6 yönlü bölünmesi için veri kümesinin altıda birini içeren bir veri kaynağına eşlenen altı dizinleyiciyi yapılandırın. 

   + Her dizinleyiciyi aynı dizinle işaretedin. Bilişsel arama iş yükleri için, her dizinleyiciyi aynı beceriye yönlendirin.

   + Her dizinleyici tanımı içinde, aynı çalışma zamanı yürütme deseni zamanlayın. Örneğin, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` tüm dizin oluşturanlar için 2018-05-15'te sekiz saatlik aralıklarla çalışan bir zamanlama oluşturur.

Zamanlanan zamanda, tüm dizinleyiciler yürütmeye, veri yüklemeye, zenginleştirmeleri uygulamaya (bilişsel bir arama ardışık lığı yapılandırdıysanız) ve dizin yazmaya başlar. Azure Bilişsel Arama güncelleştirmedizini kilitlemez. Belirli bir yazma ilk denemede başarılı olmazsa, eşzamanlı yazmalar yeniden çalışarak yönetilir.

> [!Note]
> Yinelemeleri artırırken, dizin boyutunun önemli ölçüde artması öngörülüyorsa, bölüm sayısını artırmayı düşünün. Bölümler, dizine eklenmiş içeriğin dilimlerini depolar; ne kadar çok bölüme sahipseniz, her birinin saklaması gereken dilim o kadar küçük tür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL Veritabanı dizinleyici](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizinleyici](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizinleyici](search-howto-indexing-azure-tables.md)
+ [Azure Bilişsel Aramada Güvenlik](search-security-overview.md)
