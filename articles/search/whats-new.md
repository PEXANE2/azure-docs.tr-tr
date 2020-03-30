---
title: Yeni özellik duyuruları
titleSuffix: Azure Cognitive Search
description: Azure Arama'nın Azure Bilişsel Arama'ya bir hizmet yeniden adı da dahil olmak üzere yeni ve gelişmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 03/24/2020
ms.openlocfilehash: 475f89fc5b33948864fd83c39ee8058ab6908cad
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80247206"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki yenilikler

Hizmetteki yenilikleri öğrenin. Hizmetten haberdar olmak için bu sayfayı yer imi.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Yeni hizmet adı

Azure **Arama,** bilişsel becerilerin genişletilmiş (ancak isteğe bağlı) kullanımını ve temel işlemlerde AI işlemeyi yansıtacak şekilde Azure Bilişsel Arama olarak yeniden adlandırılmıştır. API sürümleri, NuGet paketleri, ad alanları ve uç noktaları değişmez. Yeni ve varolan arama çözümleri hizmet adı değişikliğinden etkilenmez.

## <a name="feature-announcements"></a>Özellik duyuruları

### <a name="march-2020"></a>Mart 2020

+ [Yerel blob yumuşak silme (önizleme),](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) Azure Bilişsel Arama'daki Azure Blob Depolama dizinleyicisinin yumuşak silinmiş durumdaolan lekeleri tanıyacağı ve dizin oluşturma sırasında ilgili arama belgesini kaldıracağı anlamına gelir.

+ Yeni istikrarlı [Yönetim REST API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) artık kullanılabilir. 

### <a name="february-2020"></a>Şubat 2020

+ [PII Algılama (önizleme),](cognitive-search-skill-pii-detection.md) bir giriş metninden kişisel olarak tanımlanabilir bilgileri ayıklayan ve bu metinden çeşitli şekillerde gizleme seçeneği veren dizin oluşturma sırasında kullanılan bilişsel bir beceridir.

+ [Özel Varlık Arama (önizleme),](cognitive-search-skill-custom-entity-lookup.md ) özel, kullanıcı tarafından tanımlanan sözcük ve tümcecikler listesinden metin arar. Bu listeyi kullanarak, tüm belgeleri eşleşen varlıklarla etiketler. Beceri aynı zamanda benzer ama oldukça kesin olmayan eşleşmeleri bulmak için uygulanabilir bulanık eşleştirme bir dereceyi destekler. 

### <a name="january-2020"></a>Ocak 2020

+ [Müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) artık genel olarak kullanılabilir. REST kullanıyorsanız, özelliği kullanarak `api-version=2019-05-06`erişebilirsiniz. Yönetilen kod için, özellik önizleme dışında olsa bile doğru paket hala [.NET SDK sürüm 8.0-önizlemedir.](search-dotnet-sdk-migration-version-9.md) 

+ Bir arama hizmetine özel erişim, her ikisi de şu anda önizlemede olan iki mekanizma aracılığıyla kullanılabilir:

  + Hizmeti oluşturmak için Management REST API'sini `api-version=2019-10-01-Preview` kullanarak belirli IP adreslerine erişimi kısıtlayabilirsiniz. Önizleme API [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service)yeni **IpRule** ve **NetworkRuleSet** özelliklere sahiptir. Bu önizleme özelliği seçili bölgelerde kullanılabilir. Daha fazla bilgi [için, Yönetim REST API'sini nasıl kullanacağız'a](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)bakın.

  + Şu anda sınırlı erişimli bir önizleme aracılığıyla kullanılabilen Azure Arama hizmeti, aynı sanal ağdaki istemcilerin bağlantıları için Azure Özel Bitiş Noktası'nı destekleyen bir Azure Arama hizmeti sağlayabilir. Daha fazla bilgi [için, güvenli bir bağlantı için Özel Bitiş Noktası Oluştur'a](service-create-private-endpoint.md)bakın.

### <a name="december-2019"></a>Aralık 2019

+ [Uygulama Oluştur (önizleme),](search-create-app-portal.md) portalda indirilebilir bir HTML dosyası oluşturan yeni bir sihirbazdır. Dosya, arama hizmetinizdeki bir dizine bağlı olarak operasyonel bir "localhost" tarzı web uygulaması oluşturan gömülü komut dosyasıyla birlikte gelir. Sayfalar sihirbazda yapılandırılabilir ve bir arama çubuğu, sonuç alanı, kenar çubuğu gezintisi ve typeahead sorgu desteği içerebilir. İş akışını veya görünümü genişletmek veya özelleştirmek için HTML'yi çevrimdışı olarak değiştirebilirsiniz.

+ [Güvenli bağlantılar için özel bir bitiş noktası (önizleme) oluşturma,](service-create-private-endpoint.md) arama hizmetinize güvenli bağlantılar için özel bağlantının nasıl ayarlangerektiğini açıklar. Bu önizleme özelliği istek üzerine kullanılabilir ve çözümün bir parçası olarak [Azure Özel Bağlantı](../private-link/private-link-overview.md) ve Azure Sanal [Ağı'nı](../virtual-network/virtual-networks-overview.md) kullanır.

### <a name="november-2019---ignite-conference"></a>Kasım 2019 - Ignite Konferansı

+ [Artımlı zenginleştirme (önizleme),](cognitive-search-incremental-indexing-conceptual.md) zaten işlenmiş içeriği kaybetmeden belirli adımlar veya aşamalar üzerinde çalışabilmeniz için bir zenginleştirme ardışık lığına önbelleğe alma ve durum doluluğu ekler. Daha önce, bir zenginleştirme boru hattı için herhangi bir değişiklik tam bir yeniden gerekli. Artımlı zenginleştirme ile, pahalı analiz çıktısı, özellikle görüntü analizi, korunur.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge Çıkarma (önizleme),](cognitive-search-skill-document-extraction.md) bir dosyanın içeriğini bir beceri nin içinden çıkarmanızı sağlayan dizin oluşturma sırasında kullanılan bilişsel bir beceridir. Daha önce, belge kırma yalnızca skillset yürütme önce oluştu. Bu becerinin eklenmesiyle, bu işlemi skillset execution içinde de gerçekleştirebilirsiniz.

+ [Metin Çevirisi,](cognitive-search-skill-text-translation.md) metni değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren dizin oluşturma sırasında kullanılan bilişsel bir beceridir.

+ [Power BI şablonları,](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) Power BI masaüstündeki bir bilgi deposunda görselleştirmelerinizi ve zenginleştirilmiş içeriğin analizini hızlandırabilir. Bu şablon, [Veri Alma sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure tablo projeksiyonları için tasarlanmıştır.

+ [Azure Veri Gölü Depolama Gen2 (önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB Gremlin API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (önizleme)](search-howto-index-cosmosdb.md) artık dizinleyicilerde desteklenir. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)kullanarak kaydolabilirsiniz. Önizleme programına kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="july-2019"></a>Temmuz 2019

+ Genellikle Azure [Genel Bulut'ta](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search)kullanılabilir.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure web sitesinde bulunabilir.