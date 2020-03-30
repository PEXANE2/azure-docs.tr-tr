---
title: Arama dizinini yeniden oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinini yenilemek için yeni öğeler ekleyin, varolan öğeleri veya belgeleri güncelleştirin veya eski belgeleri yeniden oluşturma veya kısmi dizini olarak silin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 58b60a0eee8ab407709f33911d3c6b13ffbf301a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77498373"
---
# <a name="how-to-rebuild-an-index-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da dizin oluşturma

Bu makalede, Azure Bilişsel Arama dizininin nasıl yeniden oluşturulması gerektiği, yeniden oluşturmanın gerekli olduğu durumlar ve devam eden sorgu istekleri üzerindeki yeniden oluşturmaların etkisini azaltma önerileri açıklanmaktadır.

*Yeniden oluşturma,* tüm alan tabanlı ters dizinler de dahil olmak üzere bir dizinle ilişkili fiziksel veri yapılarını düşürme ve yeniden oluşturma anlamına gelir. Azure Bilişsel Arama'da tek tek alanları düşürüp yeniden oluşturamazsınız. Bir dizin oluşturmak için, tüm alan depolama silinmesi, varolan veya gözden geçirilmiş dizin şemasını temel alınarak yeniden oluşturulması ve ardından dizin itilen veya dış kaynaklardan çekilen verilerle yeniden doldurulması gerekir. 

Geliştirme sırasında dizinleri yeniden oluşturmak yaygındır, ancak karmaşık türleri ekleme veya önericilere alan ekleme gibi yapısal değişiklikleri karşılamak için üretim düzeyi dizini oluşturmanız da gerekebilir.

## <a name="rebuild-conditions"></a>Yeniden koşullar

Aşağıdaki koşullardan herhangi biri doğruysa bir dizin bırakın ve yeniden oluşturun. 

| Koşul | Açıklama |
|-----------|-------------|
| Alan tanımını değiştirme | Alan adı, veri türü veya belirli [dizin özniteliklerini](https://docs.microsoft.com/rest/api/searchservice/create-index) (aranabilir, filtrelenebilir, sıralanabilir, değiştirilebilir) gözden geçirmek tam yeniden oluşturmayı gerektirir. |
| Bir alana çözümleyici atama | [Çözümleyiciler](search-analyzers.md) bir dizin içinde tanımlanır ve daha sonra alanlara atanır. Bir dizine istediğiniz zaman yeni bir çözümleyici tanımı ekleyebilirsiniz, ancak yalnızca alan oluşturulduğunda bir çözümleyici *atayabilirsiniz.* Bu hem **çözümleyici** hem de **indexAnalyzer** özellikleri için geçerlidir. **searchAnalyzer** özelliği bir özel durumdur (bu özelliği varolan bir alana atayabilirsiniz). |
| Dizindeki çözümleyici tanımını güncelleştirme veya silme | Dizinin tamamını yeniden oluşturmadığınız sürece dizinteki varolan bir çözümleyici yapılandırmasını (çözümleyici, belirteç, belirteç filtresi veya char filtresi) silemez veya değiştiremezsiniz. |
| Önericiye alan ekleme | Bir alan zaten varsa ve bunu [Bir Önerenler](index-add-suggesters.md) yapısına eklemek istiyorsanız, dizini yeniden oluşturmanız gerekir. |
| Alanı silme | Bir alanın tüm izlerini fiziksel olarak kaldırmak için dizini yeniden oluşturmanız gerekir. Hemen yeniden yeniden oluşturma pratik olmadığında, uygulama kodunu "silinen" alana erişimi devre dışı bırakabilecek şekilde değiştirebilir veya sonuç kümesinde hangi alanların temsil edildiğini seçmek için [$select sorgu parametresini](search-query-odata-select.md) kullanabilirsiniz. Fiziksel olarak, alan tanımı ve içeriği, söz konusu alanı atlayan bir şema uyguladığınızda, bir sonraki yeniden yapılana kadar dizinde kalır. |
| Katmanları değiştirin | Daha fazla kapasiteye ihtiyacınız varsa, Azure portalında yerinde yükseltme yoktur. Yeni bir hizmet oluşturulmalı ve dizinler yeni hizmette sıfırdan oluşturulmalıdır. Bu işlemi otomatikleştirmeye yardımcı olmak için, bu [Azure Bilişsel Arama .NET örnek repo'da](https://github.com/Azure-Samples/azure-search-dotnet-samples) **dizin yedekleme-geri yükleme** örnek kodunu kullanabilirsiniz. Bu uygulama dizinizi bir dizi JSON dosyasına yedekleyecek ve ardından belirttiğiniz bir arama hizmetinde dizini yeniden oluşturur.|

## <a name="update-conditions"></a>Güncelleştirme koşulları

Diğer birçok değişiklik varolan fiziksel yapıları etkilemeden yapılabilir. Özellikle, aşağıdaki değişiklikler bir dizin yeniden *gerektirmez.* Bu değişiklikler için, değişiklikleriniz ile [bir dizin tanımı güncelleştirebilirsiniz.](https://docs.microsoft.com/rest/api/searchservice/update-index)

+ Yeni alan ekleme
+ Varolan bir alandaki **alınabilen** özniteliği ayarlama
+ Varolan bir alana **bir searchAnalyzer** ayarlama
+ Dizine yeni bir çözümleyici tanımı ekleme
+ Puanlama profilleri ekleme, güncelleme veya silme
+ CORS ayarlarını ekleme, güncelleme veya silme
+ Eş anlamlı haritalar ekleme, güncelleme veya silme

Yeni bir alan eklediğinizde, varolan dizinlenmiş belgelere yeni alan için null değeri verilir. Gelecekteki bir veri yenilemede, dış kaynak verilerinden elde edilen değerler Azure Bilişsel Arama tarafından eklenen null'ların yerini alır. Dizin içeriğini güncelleştirme hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)

## <a name="how-to-rebuild-an-index"></a>Dizin oluşturma

Geliştirme sırasında, indeks şema sık sık değişir. Küçük bir temsili veri kümesiyle silinebilen, yeniden oluşturulabilen ve hızla yeniden yüklenebilen dizinler oluşturarak bunu planlayabilirsiniz.

Üretimde bulunan uygulamalar için, sorgu kapalı kalma süresini önlemek için varolan bir dizin yan yana çalışan yeni bir dizin oluşturmanızı öneririz. Uygulama kodunuz yeni dizine yeniden yönlendirme sağlar.

Dizin oluşturma arka planda çalışmaz ve hizmet devam eden sorgulara karşı ek dizin oluşturma dengeler. Dizin oluşturma sırasında, sorguların zamanında tamamlanındığından emin olmak için portaldaki [sorgu isteklerini izleyebilirsiniz.](search-monitor-queries.md)

1. Yeniden oluşturma nın gerekli olup olmadığını belirleyin. Yalnızca alan ekliyorsanız veya dizinin alanlarıyla ilgisi olmayan bir bölümünü değiştiriyorsanız, tanımı silmeden, yeniden oluşturmadan ve tam olarak yeniden yüklemeden [güncelleştirmek](https://docs.microsoft.com/rest/api/searchservice/update-index) isteyebilirsiniz.

1. İleride başvuru için ihtiyacınız olması durumunda [dizin tanımı alın.](https://docs.microsoft.com/rest/api/searchservice/get-index)

1. Yeni ve eski dizinleri yan yana çalışmadığınızı varsayarak [varolan dizini](https://docs.microsoft.com/rest/api/searchservice/delete-index)bırakın. 

   Bu dizini hedefleyen tüm sorgular hemen bırakılır. Bir dizin silmenin geri döndürülemez olduğunu ve alanların toplanması ve diğer yapılar için fiziksel depolamayı yok ettiğini unutmayın. Düşürmeden önce etkileri hakkında düşünmek için duraklatın. 

1. İsteğin gövdesinin değiştirilmiş veya değiştirilmiş alan tanımlarını içerdiği [gözden geçirilmiş bir dizin oluşturun.](https://docs.microsoft.com/rest/api/searchservice/create-index)

1. Dizini harici bir kaynaktan gelen [belgelerle yükleyin.](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)

Dizin oluşturduğunuzda, fiziksel depolama dizin şemaher alan için, her aranabilir alan için oluşturulan ters bir dizin ile ayrılır. Aranamayan alanlar filtrelerde veya ifadelerde kullanılabilir, ancak ters dizinleri yoktur ve tam metin veya bulanık aranabilir değildir. Dizin yeniden oluşturmada, bu ters dizinler silinir ve sağladığınız dizin şemasına göre yeniden oluşturulur.

Dizini yüklediğinizde, her alanın ters dizini, her belgedeki benzersiz, belirteçlenmiş sözcüklerin tümüyle ve karşılık gelen belge kimliklerine bir haritayla doldurulur. Örneğin, bir otel veri kümesini dizine ekbelirlerken, Bir Şehir alanı için oluşturulan ters bir dizin Seattle, Portland ve benzeri terimler içerebilir. Şehir alanında Seattle veya Portland içeren belgeler, belge kimlikleriniterimin yanında listelenir. Herhangi bir [Ekle, Güncelleştir veya Sil](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) işleminde, terimler ve belge kimliği listesi buna göre güncelleştirilir.

> [!NOTE]
> Sıkı SLA gereksinimleriniz varsa, geliştirme ve dizine ilişkin bir üretim dizininden tam yalıtımda meydana gelen bu çalışma için özel olarak yeni bir hizmet sağlamayı düşünebilirsiniz. Ayrı bir hizmet, kaynak çekişmesi olasılığını ortadan kaldırarak kendi donanımı üzerinde çalışır. Geliştirme tamamlandığında, sorguları yeni bitiş noktası ve dizinolarak yönlendirerek yeni dizini yerinde bırakır veya özgün Azure Bilişsel Arama hizmetinizde gözden geçirilmiş bir dizin yayımlamak için tamamlanmış kodu çalıştırırdınız. Şu anda kullanıma hazır dizini başka bir hizmete taşımak için bir mekanizma yok.

## <a name="check-for-updates"></a>Güncelleştirmeleri denetle

İlk belge yüklenir yüklenmez dizin sorgulamaya başlayabilirsiniz. Bir belgenin kimliğini biliyorsanız, [Arama Belgesi REST API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) belirli belgeyi döndürür. Daha geniş sınama için, dizin tam yüklenene kadar beklemeniz ve ardından görmeyi beklediğiniz bağlamı doğrulamak için sorguları kullanmanız gerekir.

Güncelleştirilmiş içeriği denetlemek için [Arama Gezgini'ni](search-explorer.md) veya [Postacı](search-get-started-postman.md) gibi bir Web test aracını kullanabilirsiniz.

Bir alan eklediyseniz veya yeniden adlandırıldıysanız, bu alanı döndürmek için [$select](search-query-odata-select.md) kullanın:`search=*&$select=document-id,my-new-field,some-old-field&$count=true`

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Büyük veri kümelerini ölçekte dizine dizine](search-howto-large-index.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL Veritabanı dizinleyici](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizinleyici](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizinleyici](search-howto-indexing-azure-tables.md)
+ [Azure Bilişsel Aramada Güvenlik](search-security-overview.md)