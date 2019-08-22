---
title: Sık sorulan sorular (SSS)-Azure Search
description: Microsoft Azure üzerinde bir bulut barındırılan arama hizmeti olan Microsoft Azure Arama Hizmeti hakkında sık sorulan soruların yanıtlarını alın.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/03/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: d4aae2f2ef9ccbc645647125682d999c11c99ab6
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649824"
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Azure Search-sık sorulan sorular (SSS)

 Azure Search ilgili kavramlar, kod ve senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Azure Search, DBMS 'imde tam metin aramasından farklı midir?

Azure Search birden çok veri kaynağını, [birçok dil için dil analizini](https://docs.microsoft.com/rest/api/searchservice/language-support), [ilginç ve olağandışı veri girişlerinin özel analizini](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)aracılığıyla arama derecelendirme denetimleri ve gibi kullanıcı deneyimi özellikleri destekler. typeahead, isabet vurgulama ve çok yönlü gezinme. Ayrıca, eş anlamlılar ve zengin sorgu söz dizimi gibi diğer özellikleri de içerir, ancak bunlar genellikle özellikleri farklılaştırmaz.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Azure Search ve elaa araması arasındaki fark nedir?

Arama teknolojilerini karşılaştırırken, müşteriler, Azure Search Elau araması ile nasıl Karşılaştırıldığı hakkında daha fazla ayrıntı ister. Önemli bir görevi daha kolay hale yaptığımız veya diğer Microsoft teknolojileriyle yerleşik tümleştirmeye gerek duyduğumuz için, kendi arama uygulaması projeleri için Elaor araması üzerinden Azure Search seçen müşteriler genellikle bu şekilde yapılır.

+ Azure Search,% 99,9 hizmet düzeyi sözleşmeleri (SLA) ile tam olarak yönetilen bir bulut hizmetidir (okuma erişimi için 2 çoğaltma, okuma-yazma için 3 çoğaltma).
+ Microsoft 'un [doğal dil işlemcileri](https://docs.microsoft.com/rest/api/searchservice/language-support) önde gelen Edge dil analizi sunar.  
+ [Azure Search Dizin oluşturucular](search-indexer-overview.md) , ilk ve artımlı dizin oluşturma Için çeşitli Azure veri kaynaklarına gezinebilirler.
+ Sorgu veya dizin oluşturma birimlerindeki dalgalanmalara hızlı yanıt almanız gerekiyorsa Azure portal [kaydırıcı denetimlerini](search-manage.md#scale-up-or-down) kullanabilir veya bir [PowerShell betiği](search-manage-powershell.md)çalıştırarak doğrudan yönetimi doğrudan atlayarak yönetebilirsiniz.  
+ [Puanlama ve ayarlama özellikleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) , arama altyapısının tek başına sağlayabildiklerinin ötesinde, arama sıra puanlarını etkileyen yolları sağlar.

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Azure Search hizmeti duraklatabilir ve faturalandırma durabilir miyim?

Hizmeti duraklatırsınız. Hesaplama ve depolama kaynakları, hizmet oluşturulduğunda özel kullanım için ayrılır. Bu kaynakları isteğe bağlı olarak serbest bırakmak ve geri kazanmak mümkün değildir.

## <a name="indexing-operations"></a>Dizin oluşturma Işlemleri

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Dizinler veya dizin anlık görüntülerini yedekleyin ve geri yükleyin (ya da indirin ve taşıyın)?

Herhangi bir zamanda [bir dizin tanımı alabilmeniz](https://docs.microsoft.com/rest/api/searchservice/get-index) mümkün olsa da, bulutta çalışan *doldurulmuş* bir dizini bir yerel sisteme indirmek ya da başka bir Azure Search hizmetine taşımak için Dizin ayıklama, anlık görüntü veya yedekleme-geri yükleme özelliği yoktur.

Dizinler, yazdığınız koddan oluşturulup doldurulur ve yalnızca Bulutta Azure Search çalışır. Genellikle, bir dizini başka bir hizmete taşımak isteyen müşteriler, kendi kodlarını yeni bir uç nokta kullanacak şekilde düzenleyerek ve sonra Dizin oluşturmayı yeniden çalıştırabilmenizi sağlar. Bir anlık görüntü almak veya bir dizini yedeklemek istiyorsanız, [Kullanıcı sesine](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index)bir oy atayın.

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>Dizin veya hizmetmi silindikten sonra geri yükleyebilir miyim?

Hayır, dizinleri veya hizmetleri geri yükleyemezsiniz. Bir Azure Search dizini silerseniz, işlem son ' dur ve Dizin kurtarılamıyor. Bir Azure Search hizmetini sildiğinizde, hizmette bulunan tüm dizinler kalıcı olarak silinir. Ayrıca, bir veya daha fazla Azure Search hizmeti içeren bir Azure kaynak grubunu silerseniz, tüm hizmetler kalıcı olarak silinir.  

Dizinler, Dizin oluşturucular, veri kaynakları ve becerileri gibi kaynakları geri yükleme, bunları koddan yeniden oluşturmanız gerekir. Dizinler söz konusu olduğunda, dış kaynaklardaki verileri yeniden oluşturmanız gerekir. Bu nedenle, Azure SQL veritabanı veya Cosmos DB gibi başka bir veri deposunda orijinal verilerin bir ana kopyasını veya yedeğini tutmanız önemle tavsiye edilir.

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>SQL veritabanı Çoğaltmalarından dizin oluşturabilir miyim ( [Azure SQL veritabanı Dizin oluşturucular](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers)için geçerlidir)

Sıfırdan bir dizin oluştururken bir veri kaynağı olarak birincil veya ikincil çoğaltmaların kullanılmasına yönelik bir kısıtlama yoktur. Ancak, artımlı güncelleştirmeler (değiştirilen kayıtlara göre) ile bir dizini yenilemek, birincil çoğaltmayı gerektirir. Bu gereksinim yalnızca birincil çoğaltmalarda değişiklik izlemeyi garanti eden SQL veritabanından gelir. Bir dizin yenileme iş yükü için ikincil çoğaltmaları kullanmayı denerseniz, tüm verileri alacağınız garanti yoktur.

## <a name="search-operations"></a>Işlemleri ara

### <a name="can-i-search-across-multiple-indexes"></a>Birden çok dizin arasında arama yapabilir miyim?

Hayır, bu işlem desteklenmiyor. Arama her zaman tek bir dizine eklenir.

### <a name="can-i-restrict-search-index-access-by-user-identity"></a>Kullanıcı kimliğine göre arama dizini erişimini kısıtlayabilirim miyim?

`search.in()` Filtreyle [Güvenlik filtreleri](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) uygulayabilirsiniz. Filtre, arama sonuçlarını tanımlanan Kullanıcı grubu üyeliğine göre kırpmak için [Azure Active Directory (AAD) gibi kimlik yönetimi hizmetleri](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) ile iyi bir bileşim sağlar.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Neden geçerli olduğunu bildiğiniz koşullarda hiç sıfır eşleşme var mı?

En yaygın durum, her bir sorgu türünün farklı arama davranışlarını ve dil analizlerinin düzeylerini desteklediğini bilmektir. Önceden baskın iş yükü olan tam metin araması, koşulları kök formlara kesen bir dil analizi aşaması içerir. Bu sorgu ayrıştırma özelliği, belirteçlerin çok sayıda çeşitle eşleştiğinden, olası eşleşmelerin daha geniş bir kısmını yayınlar.

Ancak, joker karakter, benzer ve Regex sorguları, normal terim veya tümcecik sorguları gibi çözümlenmez ve sorgu, arama dizininde bulunan sözcüğün çözümlenme formuyla eşleşmezse kötü bir geri aramaya yol açabilir. Sorgu ayrıştırma ve çözümleme hakkında daha fazla bilgi için lütfen bkz. [sorgu mimarisi](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>Joker karakter aramalarım yavaş.

Önek, benzer ve Regex gibi çoğu joker karakter arama sorguları, arama dizininde eşleşen koşullara göre dahili olarak yeniden yazılır. Arama dizinini taramanın bu ek işlemesi gecikme süresine eklenir. Ayrıca, örneğin, birçok terim ile `a*` yeniden yazılması muhtemel büyük olasılıkla çok yavaş arama sorguları çok yavaş olabilir. Performanslı joker karakter aramaları için [özel bir çözümleyici](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search)tanımlamayı düşünün.

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Arama neden her isabet için bir sabit veya eşit puanı 1,0 olarak derecelendirmelidir?

Varsayılan olarak, arama sonuçları, [eşleşen koşulların istatistiksel özelliklerine](search-lucene-query-architecture.md#stage-4-scoring)göre puanlanır ve sonuç kümesinde yüksek ile düşük sıralanır. Ancak, bazı sorgu türleri (joker karakter, ön ek, Regex) her zaman genel belge puanına bir sabit puanı katkıda bulunur. Bu davranış tasarım gereğidir. Azure Search, sorgu genişletmesi aracılığıyla bulunan eşleşmelerin, derecelendirmeyi etkilemeden, sonuçlara dahil edilmesini sağlamak için sabit bir puan uygular.

Örneğin, joker karakter aramasında "Tur" girişinin "turları", "touretes" ve "Tourmaline" ile eşleşmeleri olduğunu varsayalım. Bu sonuçların doğası göz önüne alındığında, hangi koşulların diğerlerinden daha değerli olduğunu makul bir şekilde çıkarmanın bir yolu yoktur. Bu nedenle, Puanlama joker karakter, ön ek ve normal ifade türündeki sorgularda sonuçladığı zaman terim sıklıklarını yok saydık. Kısmi girişe dayalı arama sonuçlarına, beklenmedik potansiyel eşleşmelerin önüne geçmek için sabit bir puan verilir.

## <a name="design-patterns"></a>Tasarım desenleri

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Yerelleştirilmiş arama uygulamak için en iyi yaklaşım nedir?

Çoğu müşteri, aynı dizinde farklı yerel ayarları (diller) desteklemeye geldiğinde bir koleksiyon üzerinde adanmış alanlar seçer. Yerel ayara özgü alanlar, uygun bir çözümleyici atanmasını olanaklı kılar. Örneğin, Microsoft Fransızca Çözümleyicisi 'ni Fransızca dizeleri içeren bir alana atama. Ayrıca filtrelemeyi basitleştirir. Bir fr-fr sayfasında bir sorgunun başlatıldığını biliyorsanız, arama sonuçlarını bu alanla sınırlayabilirsiniz. Veya alana daha fazla ağırlık sağlamak için bir [Puanlama profili](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) oluşturun. Azure Search, arasından seçim yapabileceğiniz [50 dil çözümleyicilerinin](https://docs.microsoft.com/azure/search/search-language-support) kullanılmasını destekler.

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz, eksik bir özellik veya işlevlerle ilgili mi? Özelliği [Kullanıcı Voice Web sitesinde](https://feedback.azure.com/forums/263029-azure-search)isteyin.

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Tam metin aramasının Azure Search nasıl çalıştığı](search-lucene-query-architecture.md)  
 [Azure Search nedir?](search-what-is-azure-search.md)
