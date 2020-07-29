---
title: Yerleşik Dizin oluşturucular kullanarak büyük veri kümesini dizin oluşturma
titleSuffix: Azure Cognitive Search
description: Toplu iş modu, kaynak kaynağı oluşturma ve zamanlanan, paralel ve Dağıtılmış dizin oluşturma teknikleri aracılığıyla büyük veri dizinleme veya hesaplama açısından yoğun dizin oluşturma stratejileri.
manager: liamca
author: dereklegenzoff
ms.author: delegenz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/05/2020
ms.openlocfilehash: e544e720f024b265e957e67d5bd2ee8af91f5c7f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84484572"
---
# <a name="how-to-index-large-data-sets-in-azure-cognitive-search"></a>Azure Bilişsel Arama büyük veri kümelerini dizin oluşturma

Azure Bilişsel Arama, verileri bir arama dizinine aktarmaya yönelik [iki temel yaklaşımı](search-what-is-data-import.md) *destekler: verilerinizi* dizine programlamayla gönderme veya desteklenen bir veri kaynağında [Azure bilişsel arama Dizin oluşturucuyu](search-indexer-overview.md) işaret eden verileri *çekme* .

Veri birimlerinin büyümesi veya işlenmesi değiştikçe, basit veya varsayılan dizin oluşturma stratejilerinin artık pratik olmadığını görebilirsiniz. Azure Bilişsel Arama için, bir veri yükleme isteğini nasıl yapılandıracağınıza, zamanlanmış ve dağıtılmış iş yükleri için kaynağa özel bir dizin oluşturucunun kullanılmasına kadar, daha büyük veri kümelerine sahip olmak için çeşitli yaklaşımlar vardır.

Aynı teknikler de uzun süreli süreçler için de geçerlidir. Özellikle, [paralel dizin oluşturma](#parallel-indexing) bölümünde özetlenen adımlar, bir [AI zenginleştirme ardışık düzeninde](cognitive-search-concept-intro.md)görüntü analizi veya doğal dil işleme gibi yoğun hesaplama gerektiren dizin oluşturma için yararlıdır.

Aşağıdaki bölümlerde, hem anında iletme API 'SI hem de Dizin oluşturucular kullanarak büyük miktarlarda veri dizinlemeye yönelik teknikler araştırmaktadır.

## <a name="push-api"></a>Anında iletme API 'SI

Verileri bir dizine gönderdiğinizde, anında iletme API 'sinin dizin oluşturma hızlarını etkileyen bazı önemli noktalar vardır. Bu faktörler aşağıdaki bölümde özetlenmiştir. 

Bu makaledeki bilgilere ek olarak, daha fazla bilgi edinmek için [Dizin oluşturma hızını iyileştirme öğreticisindeki](tutorial-optimize-indexing-push-api.md) kod örneklerinden de yararlanabilirsiniz.

### <a name="service-tier-and-number-of-partitionsreplicas"></a>Hizmet katmanı ve bölüm/çoğaltma sayısı

Bölüm ekleme veya arama hizmetinizin katmanını artırma, her ikisi de dizin oluşturma hızlarını artırır.

Ek çoğaltmalar eklemek, dizin oluşturma hızlarını da artırabilir, ancak garanti edilmez. Diğer yandan, ek çoğaltmalar, arama hizmetinizin işleyebileceği sorgu birimini arttırır. Çoğaltmalar Ayrıca [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)almak için bir anahtar bileşendir.

Bölüm/çoğaltmalar eklemeden veya daha yüksek bir katmana yükseltmeden önce, parasal maliyeti ve ayırma süresini göz önünde bulundurun. Bölüm ekleme, dizin oluşturma hızını önemli ölçüde artırabilir, ancak bunları eklemek/kaldırmak 15 dakikadan birkaç saate kadar sürebilir. Daha fazla bilgi için bkz. [kapasiteyi ayarlama](search-capacity-planning.md)hakkındaki belgeler.

### <a name="index-schema"></a>Dizin şeması

Dizininizin şeması, veri dizinleme verilerinde önemli bir rol oynar. Alanlar ekleme ve bu alanlara ek özellikler ekleme ( *aranabilir*, çok *yönlü tablo*veya *filtrelenebilir*gibi) her ikisi de dizin oluşturma hızlarını azaltır.

Genel olarak, yalnızca kullanmak istiyorsanız alanlara ek özellikler eklenmesini öneririz.

> [!NOTE]
> Belge boyutunu aşağı tutmak için, sorgulanabilir olmayan verileri bir dizine eklemekten kaçının. Görüntüler ve diğer ikili veriler doğrudan aranabilir değildir ve dizinde depolanmamalıdır. Sorgulanabilir olmayan verileri arama sonuçlarıyla bütünleştirmek için, kaynağa bir URL başvurusu depolayan aranabilir olmayan bir alan tanımlamalısınız.

### <a name="batch-size"></a>Toplu İş Boyutu

Daha büyük bir veri kümesini dizinlemeye yönelik en basit mekanizmalardan biri, tek bir istekte birden fazla belge veya kayıt gönderkullanmaktır. Tüm yükün 16 MB altında olduğu sürece, bir istek toplu karşıya yükleme işleminde en fazla 1000 belge işleyebilir. Bu sınırlar, .NET SDK 'da [belge ekle REST API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) veya [Dizin yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) kullanıp kullanmayacağınızı uygular. Her iki API için de, her isteğin gövdesinde 1000 belge paketlemeyi yapmanız gerekir.

Belge dizini oluşturmak için toplu iş kullanımı, dizin oluşturma performansını önemli ölçüde iyileştirir. Verilerinizin en iyi toplu iş boyutunu belirlemek, dizin oluşturma hızlarını iyileştirmek için önemli bir bileşendir. En iyi toplu iş boyutunu etkileyen iki birincil etken şunlardır:
+ Dizininizin şeması
+ Verilerinizin boyutu

En iyi toplu iş boyutu dizininize ve verilerinize bağlı olduğundan, senaryonuza en hızlı Dizin oluşturma hızlarındaki sonuçları belirlemek için farklı toplu iş boyutlarını test etmek en iyi yaklaşımdır. Bu [öğretici](tutorial-optimize-indexing-push-api.md) , .NET SDK kullanarak toplu iş boyutlarını test etmek için örnek kod sağlar. 

### <a name="number-of-threadsworkers"></a>İş parçacığı/çalışan sayısı

Azure Bilişsel Arama 'in dizin oluşturma hızlarından tam olarak yararlanmak için, çok sayıda iş parçacığını kullanarak toplu işlem dizini oluşturma isteklerini hizmete eşzamanlı olarak gönderebilirsiniz.  

En iyi iş parçacığı sayısı şu şekilde belirlenir:

+ Arama hizmetinizin katmanı
+ Bölüm sayısı
+ Yığınlarınızın boyutu
+ Dizininizin şeması

Senaryonuz için en iyi iş parçacığı sayısını öğrenmek için bu örneği değiştirebilir ve farklı iş parçacığı sayısı ile test edebilirsiniz. Bununla birlikte, eşzamanlı olarak çalışan birkaç iş parçacığı olduğu sürece, verimlilik kazanmalarının avantajlarından faydalanabilirsiniz. 

> [!NOTE]
> Arama hizmetinizin katmanını artırarak veya bölümleri artırdıkça eşzamanlı iş parçacıklarının sayısını da artırmanız gerekir.

Arama hizmetine vurur istekleri artırdığınız için, isteğin tam olarak başarılı olmadığını belirten [http durum kodları](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) ile karşılaşabilirsiniz. Dizin oluşturma sırasında iki ortak HTTP durum kodu şunlardır:

+ **503 Hizmet kullanılamıyor** -bu hata, sistem ağır yükün altında ve isteğiniz şu anda işlenemediği anlamına gelir.
+ **207 çok durum** -bu hata, bazı belgelerin başarılı olduğu, ancak en az bir başarısız olduğu anlamına gelir.

### <a name="retry-strategy"></a>Yeniden deneme stratejisi 

Bir hata oluşursa, istekler [üstel geri alma yeniden deneme stratejisi kullanılarak yeniden](https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/implement-retries-exponential-backoff)denenmelidir.

Azure Bilişsel Arama .NET SDK, 503s ve diğer başarısız istekleri otomatik olarak yeniden dener, ancak 20 7s 'yi yeniden denemek için kendi mantığınızı uygulamanız gerekir. Yeniden deneme stratejisi uygulamak için, [Polly](https://github.com/App-vNext/Polly) gibi açık kaynaklı araçlar da kullanılabilir.

### <a name="network-data-transfer-speeds"></a>Ağ verileri aktarım hızları

Ağ veri aktarım hızları, verileri dizinlerken sınırlayıcı bir faktör olabilir. Azure ortamınızdaki verileri dizinlemek, dizin oluşturmayı hızlandırmanın kolay bir yoludur.

## <a name="indexers"></a>Dizin Oluşturucular

[Dizin oluşturucular](search-indexer-overview.md) , aranabilir içerik Için desteklenen Azure veri kaynaklarını gezinmek üzere kullanılır. Özellikle büyük ölçekli dizin oluşturma için özel bir yöntem olmasa da, birkaç dizin oluşturucu özelliği özellikle daha büyük veri kümelerine göz sallarken yararlı olur:

+ Zamanlayıcılar, zaman içinde yayabilmeniz için dizin oluşturmayı düzenli aralıklarla kullanmanıza olanak sağlar.
+ Zamanlanan dizin oluşturma, bilinen son durdurma noktasında sürdürülür. Bir veri kaynağı 24 saatlik bir pencerede tam olarak gezinilmemişse, Dizin Oluşturucu, her yerde iki gün boyunca Dizin oluşturmayı sürdürür.
+ Verileri daha küçük tekil veri kaynaklarına bölümlemek paralel işleme etkinleştirilir. Kaynak verileri Azure Blob depolamada birden çok kapsayıcı gibi daha küçük bileşenlere bölebilir ve ardından, paralel olarak dizinlenebilir Azure Bilişsel Arama karşılık gelen, birden fazla [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) oluşturabilirsiniz.

> [!NOTE]
> Dizin oluşturucular veri kaynağına özgüdür, bu nedenle bir Dizin Oluşturucu yaklaşımı kullanmak yalnızca Azure 'daki seçili veri kaynakları için geçerlidir: [SQL veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [BLOB depolama](search-howto-indexing-azure-blob-storage.md), [Tablo depolama](search-howto-indexing-azure-tables.md), [Cosmos DB](search-howto-index-cosmosdb.md).

### <a name="batch-size"></a>Toplu İş Boyutu

Push API 'sinde olduğu gibi, Dizin oluşturucular, toplu iş başına öğe sayısını yapılandırmanıza izin verir. [Create Indexer REST API](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer)temel alan Dizin oluşturucular için, `batchSize` Bu ayarı özelleştirmek üzere bağımsız değişkenini, verilerinizin özellikleriyle daha iyi eşleşecek şekilde ayarlayabilirsiniz. 

Varsayılan toplu işlem boyutları veri kaynağına özeldir. Azure SQL veritabanı ve Azure Cosmos DB varsayılan toplu iş boyutu 1000 ' dir. Buna karşılık, Azure Blob dizinlemesi, 10 belge üzerinde toplu iş boyutunu daha büyük ortalama belge boyutunu tanımaya göre ayarlar. 

### <a name="scheduled-indexing"></a>Zamanlanan dizin oluşturma

Dizin Oluşturucu zamanlaması, büyük veri kümelerini işlemeye yönelik önemli bir mekanizmadır ve bilişsel arama işlem hattındaki görüntü analizi gibi yavaş çalışan işlemleri. Dizin Oluşturucu işleme, 24 saatlik bir pencere içinde çalışır. İşlem, 24 saat içinde sonlanamazsa, Dizin Oluşturucu zamanlamalarının davranışları avantajınıza çalışabilir. 

Tasarıma göre, zamanlanan dizin oluşturma işlemi, genellikle zamanlanan bir sonraki aralıkta devam etmeden önce tamamlanan bir iş ile belirli aralıklarla başlar. Ancak, işlem Aralık içinde tamamlanmazsa, Dizin Oluşturucu duraklar (zaman aşımına uğradığından). Sonraki aralıkta, işleme en son kaldığınız yerden devam eder ve sistem nerede olursa olduğu gibi izler. 

Pratik koşullarda, birkaç güne yayılan Dizin yükleri için Dizin oluşturucuyu 24 saatlik bir zamanlamaya yerleştirebilirsiniz. Sonraki 24 saat boyunca yeniden çalışmaya devam ederse, en son bilinen iyi belgede yeniden başlatılır. Bu şekilde, bir Dizin Oluşturucu, tüm işlenmemiş belgeler işlenene kadar bir dizi gün boyunca bir belge biriktirme listesi aracılığıyla çalışmasını sağlayabilir. Bu yaklaşım hakkında daha fazla bilgi için bkz. [Azure Blob depolamada büyük veri kümelerini dizine alma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Zamanlamaları genel olarak ayarlama hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluşturma REST API](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) veya [Azure bilişsel arama için Dizin oluşturucuyu zamanlama](search-howto-schedule-indexers.md).

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Paralel dizin oluşturma

Paralel dizin oluşturma stratejisi, her veri kaynağı tanımının verilerin bir alt kümesini belirttiği, uyum içinde birden çok veri kaynağını dizine almayı temel alır. 

Bir bilişsel arama işlem hattındaki taranmış belgelerde OCR gibi rutin olmayan, yoğun şekilde yoğun dizin oluşturma gereksinimleri için, paralel bir dizin oluşturma stratejisi genellikle uzun süre çalışan bir işlemin en kısa sürede tamamlanmasına yönelik doğru yaklaşımdır. Sorgu isteklerini ortadan kaldırabilir veya azaltabiliyorsanız, sorguları eşzamanlı olarak işleme olmayan bir hizmette Paralel dizin oluşturma, büyük bir yavaş işleme içeriği ile çalışmaya yönelik en iyi strateji seçeneğidir. 

Paralel işleme şu öğelere sahiptir:

+ Kaynak verileri birden çok kapsayıcı veya aynı kapsayıcı içindeki birden çok sanal klasör arasında alt bölümlere ayır. 
+ Her bir mini veri kümesini kendi [veri kaynağına](https://docs.microsoft.com/rest/api/searchservice/create-data-source)eşleyin ve kendi [Dizin oluşturucula](https://docs.microsoft.com/rest/api/searchservice/create-indexer)eşleştirilmiş olarak eşleştirin.
+ Bilişsel arama için, her bir Dizin Oluşturucu tanımında aynı [beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) başvurun.
+ Aynı hedef arama dizinine yazın. 
+ Tüm dizin oluşturucularının aynı anda çalışmasını zamanlayın.

> [!NOTE]
> Azure Bilişsel Arama, dizin oluşturma veya sorgu işlemeye bireysel çoğaltmalar veya bölümler atayamazsınız. Sistem, kaynakların nasıl kullanıldığını belirler. Sorgu performansına etkisini anlamak için, bir test ortamında, üretime vermeden önce Paralel dizin oluşturmayı deneyebilirsiniz.  

### <a name="how-to-configure-parallel-indexing"></a>Paralel dizin oluşturmayı yapılandırma

Dizin oluşturucular için, işlem kapasitesi, arama hizmetiniz tarafından kullanılan her bir hizmet birimi (SU) için bir Dizin Oluşturucu alt sistemine bağlıdır. En az iki çoğaltmaya sahip temel veya Standart katmanlarda sağlanan Azure Bilişsel Arama hizmetlerinde birden çok eş zamanlı Dizin Oluşturucu mümkündür. 

1. [Azure Portal](https://portal.azure.com), arama hizmeti panonuz **genel bakış** sayfasında, **fiyatlandırma katmanını** denetleyerek Paralel dizin oluşturma işleminin uyumlu olduğunu doğrulayın. Hem temel hem de Standart katmanlar birden çok çoğaltma sunar.

2. Hizmetinizde arama birimi sayısı olarak paralel olarak çok sayıda Dizin Oluşturucu çalıştırabilirsiniz. **Ayarlar**  >  **ölçeği**' nde, paralel işleme için [çoğaltmaları](search-capacity-planning.md) veya bölümleri artırın: her bir dizin oluşturucu iş yükü için bir ek çoğaltma veya bölüm. Mevcut sorgu birimi için yeterli bir sayı bırakın. Dizin oluşturma için sorgu iş yüklerinin Fede iyi bir zorunluluğunu getirir değildir.

3. Azure Bilişsel Arama dizin oluşturucularının ulaşabileceği bir düzeyde verileri birden çok kapsayıcıya dağıtın. Bu, Azure SQL veritabanı 'nda birden çok tablo, Azure Blob depolamada birden çok kapsayıcı veya birden çok koleksiyon olabilir. Her tablo veya kapsayıcı için bir veri kaynağı nesnesi tanımlayın.

4. Paralel olarak çalışmak için birden çok Dizin Oluşturucu oluşturun ve zamanlayın:

   + Altı çoğaltmalarla bir hizmet varsayın. Her biri, veri kümesinin tamamının 6 yönlü bölünmesi için veri kümesinin altıncı bir kısmını içeren bir veri kaynağıyla eşleştirilmiş altı Dizin oluşturucuyu yapılandırın. 

   + Her dizin oluşturucuyu aynı dizine işaret edin. Bilişsel arama iş yükleri için, her bir dizin oluşturucuyu aynı beceri işaret edin.

   + Her bir Dizin Oluşturucu tanımı içinde aynı çalışma zamanı yürütme modelini zamanlayın. Örneğin, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` sekiz saatlik aralıklarla çalışan tüm dizin oluşturucular üzerinde 2018-05-15 üzerinde bir zamanlama oluşturur.

Zamanlanan zamanda, tüm dizin oluşturucular yürütme, veri yükleme, zenginleştirme uygulama (bilişsel arama işlem hattı yapılandırdıysanız) ve dizine yazma işlemini başlatır. Azure Bilişsel Arama, güncelleştirme dizinini kilitlemez. İlk denemede belirli bir yazma işlemi başarısız olursa, eşzamanlı yazma işlemleri yönetilir.

> [!Note]
> Çoğaltmaları arttırdığınızda, dizin boyutu önemli ölçüde artırmak için yansıtıldıysanız bölüm sayısını artırmayı göz önünde bulundurun. Bölümler, dizinli içeriğin dilimlerini depolar; ne kadar çok bölüm varsa, dilimin her birinin depolanması gerekir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL veritabanı Dizin Oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob depolama Dizin Oluşturucu](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo depolama Dizin Oluşturucu](search-howto-indexing-azure-tables.md)
+ [Azure Bilişsel Arama güvenliği](search-security-overview.md)
