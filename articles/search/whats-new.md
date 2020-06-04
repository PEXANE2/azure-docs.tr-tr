---
title: Yeni Özellik Duyuruları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 05/19/2020
ms.openlocfilehash: f8ddc3ef56aefc6ea613af19d91cd0122435efd7
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344212"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmette güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

## <a name="feature-announcements"></a>Özellik Duyuruları

### <a name="may-2020-microsoft-build"></a>Mayıs 2020 (Microsoft derleme)

+ [Hata ayıklama oturumları](cognitive-search-debug-session.md) özelliği şu anda önizlemededir. [Erişim istemek Için kaydolun](https://aka.ms/DebugSessions). Hata ayıklama oturumları, beceri ile ilgili sorunları araştırmak ve çözmek için portal tabanlı bir arabirim sağlar. Hata ayıklama oturumunda oluşturulan düzeltmeler üretim becerileri kaydedilebilir. [Bu öğreticiyi](cognitive-search-tutorial-debug-sessions.md)kullanmaya başlayın.

+ Güvenlik geliştirmeleri, genel İnternet üzerinden erişilemeyen [bir özel arama uç noktası (Önizleme) ayarlama](service-create-private-endpoint.md) olanağını içerir. Ayrıca, [bağlantılı güvenlik duvarı desteği (Önizleme) IÇIN IP kurallarını da yapılandırabilirsiniz](service-configure-firewall.md).

+ Dizin oluşturmak üzere bir Azure veri kaynağıyla bağlantı kurmak için [sistem tarafından yönetilen bir kimlik (Önizleme)](search-howto-managed-identities-data-sources.md) kullanın. Azure SQL veritabanı, Azure Cosmos DB ve Azure depolama gibi Azure veri kaynaklarından içerik alan [Dizin oluşturucular](search-indexer-overview.md) için geçerlidir.

+ [ScoringStatistics = Global](index-similarity-and-scoring.md#scoring-statistics) ve SessionID sorgu parametrelerini kullanarak, arama puanlarının her parça başına ve tüm parçalara göre nasıl hesaplandığından bağımsız olarak değişiklik yapın.

### <a name="march-2020"></a>Mart 2020

+ [Yerel blob geçici silme (Önizleme)](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) , Azure bilişsel arama Azure Blob depolama Dizin Oluşturucu, geçici olarak silinen bir durumda olan Blobları tanıyacak ve dizin oluşturma sırasında ilgili arama belgesini kaldıracağı anlamına gelir.

+ Yeni kararlı [yönetim REST API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) artık kullanıma sunuldu. 

### <a name="february-2020"></a>Şubat 2020

+ [PII algılama (Önizleme)](cognitive-search-skill-pii-detection.md) , bir giriş metninin kişisel olarak tanımlanabilir bilgilerini çıkaran ve bu metinden çeşitli yollarla maske sağlayan bir seçenek sunan, dizin oluşturma sırasında kullanılan bilişsel bir yetenördir.

+ [Özel varlık arama (Önizleme)](cognitive-search-skill-custom-entity-lookup.md ) , özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin arar. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. 

### <a name="january-2020"></a>Ocak 2020

+ [Müşterinin yönettiği şifreleme anahtarları](search-security-manage-encryption-keys.md) artık genel kullanıma sunulmuştur. REST kullanıyorsanız, özelliğini kullanarak erişebilirsiniz `api-version=2019-05-06` . Yönetilen kod için, özellik Önizleme dışında olsa bile, doğru paket yine de [.NET SDK sürüm 8,0-Önizleme](search-dotnet-sdk-migration-version-9.md) ' dir. 

+ Bir arama hizmetine özel erişim, şu anda önizleme aşamasında olan iki mekanizma aracılığıyla sunulmaktadır:

  + Hizmeti oluşturmak için yönetim REST API kullanarak belirli IP adreslerine erişimi kısıtlayabilirsiniz `api-version=2019-10-01-Preview` . Preview API 'sinin [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service)'de yeni **ıprule** ve **networkruleset** özellikleri vardır. Bu önizleme özelliği seçili bölgelerde kullanılabilir. Daha fazla bilgi için bkz. [yönetim REST API nasıl kullanılır](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

  + Şu anda sınırlı erişimli bir önizleme aracılığıyla kullanılabilir, aynı sanal ağdaki istemcilerden gelen bağlantılar için Azure özel uç noktasını destekleyen bir Azure Search hizmeti sağlayabilirsiniz. Daha fazla bilgi için bkz. [güvenli bağlantı Için özel uç nokta oluşturma](service-create-private-endpoint.md).

### <a name="december-2019"></a>Aralık 2019

+ [Uygulama oluşturma (Önizleme)](search-create-app-portal.md) , portalda INDIRILEBILIR bir HTML dosyası üreten yeni bir sihirbazdır. Dosya, arama hizmetinizde bir dizine bağlı olan, işlemsel bir "localhost" stili Web uygulaması işleyen katıştırılmış komut dosyası ile birlikte gelir. Sayfalar sihirbazda yapılandırılabilir ve bir arama çubuğu, sonuç alanı, kenar çubuğu gezintisi ve typeahead sorgu desteği bulunabilir. İş akışını veya görünümü genişletmek veya özelleştirmek için HTML 'yi çevrimdışı değiştirebilirsiniz.

+ [Güvenli bağlantılar için özel bir uç nokta oluşturma (Önizleme)](service-create-private-endpoint.md) arama hizmetinize güvenli bağlantılar için özel bir bağlantı ayarlamayı açıklar. Bu önizleme özelliği, istek üzerine kullanılabilir ve çözümün bir parçası olarak [Azure özel bağlantısı](../private-link/private-link-overview.md) ve [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) kullanır.

### <a name="november-2019---ignite-conference"></a>Kasım 2019-Menite Konferansı

+ [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) , bir zenginleştirme ardışık düzenine önbelleğe alma ve statefullstate ekler. böylece, zaten işlenmiş olan içeriği kaybetmeden belirli adımlarla veya aşamalarda çalışabilirsiniz. Daha önce, bir zenginleştirme ardışık düzeninde yapılan herhangi bir değişiklik tam yeniden oluşturma gerektirdi. Artımlı çözümlemenin, özellikle de görüntü analizinden oluşan çıkış, korunur.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge ayıklama (Önizleme)](cognitive-search-skill-document-extraction.md) , dizin oluşturma sırasında kullanılan bilişsel bir beceriye sahiptir ve bir dosyanın içeriğini bir beceri içinden ayıklamanızı sağlar. Daha önce belge çözme yalnızca beceri yürütmeden önce oluşmuştur. Bu beceriye ek olarak, bu işlemi beceri yürütme içinde de gerçekleştirebilirsiniz.

+ [Metin çevirisi](cognitive-search-skill-text-translation.md) , dizinleme sırasında metni değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren bilişsel bir yetenküldür.

+ [Power BI şablonlar](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) , görselleştirmelerinizi ve Power BI masaüstündeki bir bilgi deposunda zenginleştirme içeriği analizini başlatabilir. Bu şablon, [veri Içeri aktarma Sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure Tablo projeksiyonlarını için tasarlanmıştır.

+ [Azure Data Lake Storage 2. (Önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB GREMLIN API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (Önizleme)](search-howto-index-cosmosdb.md) Dizin oluşturucularda artık desteklenmektedir. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)kullanarak kaydolabilirsiniz. Önizleme programına kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="july-2019"></a>Temmuz 2019

+ [Azure Kamu Bulutu](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search)'nda genel olarak kullanılabilir.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Yeni hizmet adı

Azure Search, çekirdek işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş (henüz isteğe bağlı) kullanımını yansıtmak için artık **Azure bilişsel arama** olarak yeniden adlandırıldı. API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Yeni ve mevcut arama çözümleri, hizmet adı değişikliğinden etkilenmez.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.