---
title: Sık sorulan sorular (SSS)
titleSuffix: Azure Cognitive Search
description: Microsoft Azure'da bulut barındırılan bir arama hizmeti olan Microsoft Azure Bilişsel Arama hizmeti yle ilgili sık sorulan soruların yanıtlarını alın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/10/2020
ms.openlocfilehash: 520699b81024de9491f34263f16872428ddbd487
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81618032"
---
# <a name="azure-cognitive-search---frequently-asked-questions-faq"></a>Azure Bilişsel Arama - sık sorulan sorular (SSS)

 Azure Bilişsel Arama ile ilgili kavramlar, kodlar ve senaryolarla ilgili sık sorulan soruların yanıtlarını bulun.

## <a name="platform"></a>Platform

### <a name="how-is-azure-cognitive-search-different-from-full-text-search-in-my-dbms"></a>Azure Bilişsel Arama'nın DBMS'imdeki tam metin aramasından farkı nedir?

Azure Bilişsel Arama birden çok veri kaynağını, [birçok dil için dil analizini,](https://docs.microsoft.com/rest/api/searchservice/language-support)ilginç ve sıradışı veri girişleri için özel [analizi,](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search)puanlama profilleri aracılığıyla arama sıralaması [denetimlerini](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)ve typeahead, hit vurgulama ve yönlü gezinme gibi kullanıcı deneyimi özelliklerini destekler. Eşanlamlılar ve zengin sorgu sözdizimi gibi diğer özellikleri de içerir, ancak bunlar genellikle ayırt edici özellikler değildir.

### <a name="can-i-pause-azure-cognitive-search-service-and-stop-billing"></a>Azure Bilişsel Arama hizmetini duraklatabilir ve faturalandırmayı durdurabilir miyim?

Hizmeti duraklatamazsınız. Hizmet oluşturulduğunda özel kullanımınız için hesaplama ve depolama kaynakları ayrılır. İsteğe bağlı olarak bu kaynakları serbest bırakmak ve geri almak mümkün değildir.

## <a name="indexing-operations"></a>Dizin Oluşturma İşlemleri

### <a name="move-backup-and-restore-indexes-or-index-snapshots"></a>Dizinleri veya dizin anlık görüntülerini taşımak, yedeklemek ve geri yüklemek?

Geliştirme aşamasında, dizininizi arama hizmetleri arasında taşımak isteyebilirsiniz. Örneğin, dizinigeliştirmek için Temel veya Ücretsiz fiyatlandırma katmanı kullanabilir ve ardından bunu üretim kullanımı için Standart veya daha yüksek katmana taşımak isteyebilirsiniz. 

Veya, dizin anlık görüntüsünü daha sonra geri yüklemek için kullanılabilecek dosyalara yedeklemek isteyebilirsiniz. 

Tüm bunları bu [Azure Bilişsel Arama .NET örnek repo'sundaki](https://github.com/Azure-Samples/azure-search-dotnet-samples) **dizin yedekleme-geri yükleme** örnek koduyla yapabilirsiniz. 

Azure Bilişsel Arama REST API'sini kullanarak istediğiniz zaman [bir dizin tanımı da alabilirsiniz.](https://docs.microsoft.com/rest/api/searchservice/get-index)

Şu anda Azure portalında yerleşik dizin çıkarma, anlık görüntü veya yedekleme geri yükleme özelliği yok. Ancak, gelecekte bir sürümde yedekleme ve geri yükleme işlevselliğini eklemeyi düşünüyoruz. Bu özellik için desteğinizi göstermek istiyorsanız, [Kullanıcı Sesi'ni](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index)oyverin.

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>Silindikten sonra dizinimi veya hizmetimi geri yükleyebilir miyim?

Hayır, bir Azure Bilişsel Arama dizinini veya hizmetini silerseniz, kurtarılamaz. Bir Azure Bilişsel Arama hizmetini sildiğinizde, hizmetteki tüm dizinler kalıcı olarak silinir. Bir veya daha fazla Azure Bilişsel Arama hizmeti içeren bir Azure kaynak grubunu silerseniz, tüm hizmetler kalıcı olarak silinir.  

Dizinler, dizin oluşturanlar, veri kaynakları ve beceri kümeleri gibi kaynakları yeniden oluşturmak, bunları koddan yeniden oluşturmanızı gerektirir. 

Bir dizin yeniden oluşturmak için, dış kaynaklardan verileri yeniden dizine gerekir. Bu nedenle, Azure SQL Veritabanı veya Cosmos DB gibi başka bir veri deposunda orijinal verilerin ana kopyasını veya yedekini saklamanız önerilir.

Alternatif olarak, bu [Azure Bilişsel Arama .NET örnek repo'sundaki](https://github.com/Azure-Samples/azure-search-dotnet-samples) **dizin yedekleme-geri yükleme** örnek kodunu kullanarak bir dizi JSON dosyasına dizin tanımını ve dizin anlık görüntüsünü yedekleyebilirsiniz. Daha sonra, gerekirse dizini geri yüklemek için aracı ve dosyaları kullanabilirsiniz.  

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexers"></a>SQL veritabanı yinelemelerinden dizin alabilir miyim [(Azure SQL Veritabanı dizinleyicileri](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers)için geçerlidir)

Sıfırdan bir dizin inşa ederken birincil veya ikincil yinelemelerin veri kaynağı olarak kullanımına ilişkin herhangi bir kısıtlama yoktur. Ancak, bir dizini artımlı güncelleştirmelerle (değiştirilen kayıtlara dayalı olarak) yenilemek birincil yinelemeyi gerektirir. Bu gereksinim, yalnızca birincil yinelemelerde değişiklik izlemeyi garanti eden SQL Veritabanı'ndan gelir. Dizin yenileme iş yükü için ikincil yinelemeler kullanmayı denerseniz, tüm verileri aldığınızın garantisi yoktur.

## <a name="search-operations"></a>Arama İşlemleri

### <a name="can-i-search-across-multiple-indexes"></a>Birden çok dizin arasında arama yapabilir miyim?

Hayır, bu işlem desteklenmiyor. Arama her zaman tek bir dizin kapsamındadır.

### <a name="can-i-restrict-search-index-access-by-user-identity"></a>Arama dizini erişimini kullanıcı kimliğine göre kısıtlayabilir miyim?

[Güvenlik filtrelerini](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) filtreyle `search.in()` uygulayabilirsiniz. Filtre, tanımlı kullanıcı grubu üyeliğine dayalı arama sonuçlarını kırpmak için [Azure Active Directory (AAD) gibi kimlik yönetimi hizmetleriyle](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) iyi bir şekilde bir araya gelir.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Geçerli olduğunu bildiğim şartlarda neden sıfır eşleşme var?

En yaygın durum, her sorgu türünün farklı arama davranışlarını ve dilsel analiz düzeylerini desteklediğini bilmemektir. Baskın iş yükü olan tam metin arama, terimleri kök formlara ayıran bir dil çözümleme aşaması içerir. Sorgu ayrıştırma bu yönü olası eşleşmeler üzerinde daha geniş bir net dökümleri, belirteçli terim varyantları daha fazla sayıda eşleşir, çünkü.

Joker, bulanık ve regex sorguları, ancak, normal terim veya tümcecik sorguları gibi çözümlenmez ve sorgu arama dizinindeki sözcüğün çözümlenmiş biçimiyle eşleşmezse kötü geri çağırmaya yol açabilir. Sorgu ayrıştma ve çözümleme hakkında daha fazla bilgi için [sorgu mimarisine](https://docs.microsoft.com/azure/search/search-lucene-query-architecture)bakın.

### <a name="my-wildcard-searches-are-slow"></a>Joker aramalarım yavaş.

Önek, bulanık ve regex gibi joker karakter arama sorgularının çoğu, arama dizinindeki eşleşen terimlerle birlikte dahili olarak yeniden yazılır. Arama dizinini taramanın bu ekstra işlemesi gecikmeye katkıda dır. Ayrıca, örneğin, birçok `a*` terimle yeniden yazılması muhtemel geniş arama sorguları çok yavaş olabilir. Performant joker karakter aramaları için özel bir [çözümleyici](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search)tanımlamayı düşünün.

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Neden arama sıralaması her vuruş için sabit veya eşit puan 1.0 nedir?

Varsayılan olarak, arama sonuçları [eşleşen terimlerin istatistiksel özelliklerine](search-lucene-query-architecture.md#stage-4-scoring)göre puanlandırılır ve sonuç kümesinde yüksekten düşüke doğru sıralanır. Ancak, bazı sorgu türleri (joker karakter, önek, regex) her zaman genel belge puanına sabit bir puan ekler. Bu davranış tasarım gereğidir. Azure Bilişsel Arama, sorgu genişletme yoluyla bulunan eşleşmelerin sıralamayı etkilemeden sonuçlara dahil edilmesine izin vermek için sabit bir puan uygular.

Örneğin, joker karakter aramasında "tour*" girişinin "tur", "tourettes" ve "tourmaline" üzerinde eşleşmeler ürettiğini varsayalım. Bu sonuçların doğası göz önüne alındığında, hangi terimlerin diğerlerinden daha değerli olduğu konusunda makul bir çıkarı elde etmenin bir yolu yoktur. Bu nedenle, joker karakter, önek ve regex türlerinin sorgularında sonuç alırken terim sıklıklarını yoksak. Kısmi bir girişe dayalı arama sonuçlarına, beklenmeyen olası eşleşmelere karşı önyargıdan kaçınmak için sabit bir puan verilir.

## <a name="skillset-operations"></a>Skillset İşlemleri

### <a name="are-there-any-tips-or-tricks-to-reduce-cognitive-services-charges-on-ingestion"></a>Yutma bilişsel hizmetler ücretleri azaltmak için herhangi bir ipucu veya püf noktaları var mı?

Özellikle işlemek için milyonlarca belgeyle uğraşıyorsanız, yerleşik becerileri veya özel becerileri kesinlikle gerekli olandan daha fazla yürütmek istememeniz anlaşılabilir bir durumdur. Bunu göz önünde bulundurarak, beceri yürütmeiçin "artımlı zenginleştirme" yetenekleri ekledik. Özünde, "ara" zenginleştirme adımlarının çıktısını depolamak için kullanılacak bir önbellek konumu (blob depolama bağlantı dizesi) sağlayabilirsiniz.  Bu zenginleştirme boru hattı akıllı olmasını sağlar ve becerilerinizi değiştirdiğinizde gerekli olan sadece zenginleştirmeler uygulamak. Bu doğal olarak da boru hattı daha verimli olacak gibi dizinleme zaman tasarrufu sağlayacaktır.

[Artımlı zenginleştirme](cognitive-search-incremental-indexing-conceptual.md) hakkında daha fazla bilgi edinin

## <a name="design-patterns"></a>Tasarım desenleri

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Yerelleştirilmiş aramayı uygulamak için en iyi yaklaşım nedir?

Çoğu müşteri, aynı dizindeki farklı yerel alanları (dilleri) destekleme söz konusu olduğunda, koleksiyon yerine özel alanları seçer. Yerel e-özel alanlar, uygun bir çözümleyici atamayı mümkün kılar. Örneğin, Microsoft Fransızca Çözümleyicisini Fransızca dizeleri içeren bir alana atama. Ayrıca filtreleme kolaylaştırır. Fr-fr sayfasında bir sorgu başlatıldığını biliyorsanız, arama sonuçlarını bu alanla sınırlandırabilirsiniz. Veya, alana daha göreli ağırlık vermek için bir [puanlama profili](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) oluşturun. Azure Bilişsel Arama, aralarından seçim yapabileceğiniz 50'den fazla [dil çözümleyicisini](https://docs.microsoft.com/azure/search/search-language-support) destekler.

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz eksik bir özellik veya işlevsellik hakkında mı? [Kullanıcı Sesi web sitesindeki](https://feedback.azure.com/forums/263029-azure-search)özelliği isteyin.

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Azure Bilişsel Arama](https://stackoverflow.com/questions/tagged/azure-search)   
 [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)  
 [Azure Bilişsel Arama nedir?](search-what-is-azure-search.md)
