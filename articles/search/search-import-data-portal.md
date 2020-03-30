---
title: Azure portalını kullanarak verileri arama dizinine aktarma
titleSuffix: Azure Cognitive Search
description: Cosmos DB, Blob depolama, tablo depolama, SQL Veritabanı ve SQL Server'dan Azure verilerini Azure VM'lerde taramak için Azure portalındaki Veri Alma sihirbazını nasıl kullanacağınızı öğrenin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 0ed2bd7f1e03d8d5fa11f7e76010d087605f0fe1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75460707"
---
# <a name="import-data-wizard-for-azure-cognitive-search"></a>Azure Bilişsel Arama için veri alma sihirbazı

Azure portalı, bir dizin prototipi yazmak ve yüklemek için Azure Bilişsel Arama panosunda bir **Alma veri** sihirbazı sağlar. Bu makalede, sihirbaz, giriş ve çıktılar ve bazı kullanım bilgilerini kullanmanın avantajları ve sınırlamaları yer alır. Yerleşik örnek verileri kullanarak sihirbaza adım atmada uygulamalı kılavuz için Azure portalı hızlı başlat'ı [kullanarak Azure Bilişsel Arama dizini oluştur'a](search-get-started-portal.md) bakın.

Bu sihirbazın gerçekleştirdiği işlemler şunlardır:

1 - Desteklenen bir Azure veri kaynağına bağlanın.

2 - Kaynak verilerin örneklemle çıkarılan bir indeks şeması oluşturun.

3 - İsteğe bağlı olarak, içerik ve yapı ayıklamak veya oluşturmak için AI zenginleştirmeleri ekleyin.

4 - Nesneleri oluşturmak, veri almak, bir zamanlama ve diğer yapılandırma seçenekleri ayarlamak için sihirbazı çalıştırın.

Sihirbaz, programlı olarak veya diğer araçlarda erişebileceğiniz arama hizmetinize kaydedilen bir dizi nesneyi çıkartır.

## <a name="advantages-and-limitations"></a>Avantajları ve sınırlamaları

Herhangi bir kod yazmadan önce, prototip oluşturma ve kavram kanıtı testi için sihirbazı kullanabilirsiniz. Sihirbaz dış veri kaynaklarına bağlanır, bir başlangıç dizin oluşturmak için verileri örnekler ve ardından JSON belgeleri olarak verileri Azure Bilişsel Arama'da bir dizine aktarır. 

Örnekleme, bir dizin şemasının çıkarılan ve bazı sınırlamaları olan işlemdir. Veri kaynağı oluşturulduğunda, sihirbaz hangi sütunların veri kaynağının parçası olduğuna karar vermek için bir belge örneği seçer. Bu, çok büyük veri kaynakları için saatler sürebilir gibi tüm dosyalar okunmaz. Bir belge seçimi verildiğinde, alan adı veya türü gibi kaynak meta verileri, bir dizin şemasında alan koleksiyonu oluşturmak için kullanılır. Kaynak verilerin karmaşıklığına bağlı olarak, doğruluk için ilk şemayı nızı doldurmanız veya tamolarak genişletmeniz gerekebilir. Değişikliklerinizi dizin tanımı sayfasında satır satır yapabilirsiniz.

Genel olarak, sihirbazı kullanmanın avantajları açıktır: gereksinimler karşılandığı sürece, birkaç dakika içinde sorgulanabilir bir dizinin prototipini yapabilirsiniz. Dizin oluşturmanın JSON belgeleri gibi veri sağlama gibi bazı karmaşıklıkları sihirbaz tarafından işlenir.

Bilinen sınırlamalar aşağıdaki gibi özetlenebilir:

+ Sihirbaz yinelemeyi veya yeniden kullanmayı desteklemez. Sihirbazdan geçen her geçiş yeni bir dizin, skillset ve dizinleyici yapılandırması oluşturur. Sihirbaz içinde yalnızca veri kaynakları kalıcı olarak kullanılabilir ve yeniden kullanılabilir. Diğer nesneleri değiştirmek veya bunları hassaslaştırmak için yapıları almak ve değiştirmek için REST API'lerini veya .NET SDK'yı kullanmanız gerekir.

+ Kaynak içeriğidesteklenen bir Azure veri kaynağında olmalıdır.

+ Örnekleme, kaynak verilerin bir alt kümesi üzerindedir. Büyük veri kaynakları için sihirbazın alanları kaçırması mümkündür. Örnekleme yetersizse şemayı genişletmeniz veya çıkarılan veri türlerini düzeltmeniz gerekebilir.

+ Portalda açığa çıkan AI zenginleştirme, birkaç yerleşik beceri ile sınırlıdır. 

+ Sihirbaz tarafından oluşturulabilen bir [bilgi deposu](knowledge-store-concept-intro.md)birkaç varsayılan projeksiyonla sınırlıdır. Sihirbaz tarafından oluşturulan zenginleştirilmiş belgeleri kaydetmek istiyorsanız, blob kapsayıcı ve tablolar varsayılan adlar ve yapı ile birlikte gelir.

<a name="data-source-inputs"></a>

## <a name="data-source-input"></a>Veri kaynağı girişi

**Alma veri** sihirbazı, kaynağı örneklemek, meta verileri okumak, içeriği ve yapıyı okumak için belgeleri kırmak ve azure Bilişsel Arama'ya sonraki içe aktarma için içeriği JSON olarak serihale getirmek için donatılmış Azure Bilişsel Arama dizinleyicileri tarafından sağlanan dahili mantığı kullanarak harici bir veri kaynağına bağlanır.

Yalnızca tek bir tablodan, veritabanı görünümünden veya eşdeğer veri yapısından içe aktarabilirsiniz, ancak yapı hiyerarşik veya iç içe olan alt yapıları içerebilir. Daha fazla bilgi için [karmaşık türleri nasıl modellendirin.](search-howto-complex-data-types.md)

Sihirbazı çalıştırmadan önce bu tek tabloyu veya görünümü oluşturmanız gerekir ve içeriği içermelidir. Bilinen nedenlerden dolayı, **İçe Aktarma veri** sihirbazını boş bir veri kaynağında çalıştırmak mantıklı değildir.

|  Seçim | Açıklama |
| ---------- | ----------- |
| **Var olan veri kaynağı** |Arama hizmetinizde zaten dizin oluşturma biçiminiz tanımlanmışsa, yeniden kullanabileceğiniz varolan bir veri kaynağı tanımına sahip olabilirsiniz. Azure Bilişsel Arama'da veri kaynağı nesneler yalnızca dizin leyiciler tarafından kullanılır. Bir veri kaynağı nesnesini programlı olarak veya **Alma veri** sihirbazı aracılığıyla oluşturabilir ve gerektiğinde yeniden kullanabilirsiniz.|
| **Örnekler**| Azure Bilişsel Arama, öğreticilerde ve hızlı başlangıçlarda kullanılan iki yerleşik örnek veri kaynağı sağlar: Cosmos DB'de barındırılan bir gayrimenkul SQL veritabanı ve Oteller veritabanı. Oteller örneğine dayalı bir yürüyüş için Azure portalında hızlı [başlat'ta dizin oluştur'a](search-get-started-portal.md) bakın. |
| [**Azure SQL Veritabanı**](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) |Hizmet adı, okuma iznine sahip bir veritabanı kullanıcısının kimlik bilgileri ve veritabanı adı, sayfa üzerinde ya da ADO.NET bağlantı dizesi aracılığıyla belirtilebilir. Özellikleri görüntülemek veya özelleştirmek için bağlantı dizesini seçin. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir.|
| **Azure VM’lerde SQL Server** |Bağlantı dizesi olarak tam nitelikli bir hizmet adı, kullanıcı kimliği ve parola ve veritabanı belirtin. Bu veri kaynağını kullanmak için bağlantıyı şifreleyen yerel depoya daha önce bir sertifika yüklemiş olmanız gerekir. Talimatlar için Azure [Bilişsel Arama'ya SQL VM bağlantısına](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)bakın. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| [**Azure Cosmos DB**](search-howto-index-cosmosdb.md)|Hesap, veritabanı ve bağlantı gereklidir. Koleksiyondaki tüm belgeler dizine dahil edilir. Satır kümesini düzleştirmek veya filtrelemek için bir sorgu tanımlayabilir veya sorguyu boş bırakabilirsiniz. Bu sihirbazda bir sorgu gerekmez.|
| [**Azure Blob Depolama**](search-howto-indexing-azure-blob-storage.md) |Depolama hesabı ve bir kapsayıcı gereklidir. İsteğe bağlı olarak, gruplandırma amacıyla blob adlarından önce bir sanal adlandırma kuralı varsa adın sanal dizin kısmını kapsayıcı altındaki bir klasör olarak belirtebilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama Dizini Oluşturma](search-howto-indexing-azure-blob-storage.md). |
| [**Azure Tablo Depolama**](search-howto-indexing-azure-tables.md) |Depolama hesabı ve bir tablo adı gereklidir. İsteğe bağlı olarak, tabloların bir alt kümesini almak için sorgu belirtebilirsiniz. Daha fazla bilgi için bkz. [Tablo Depolama Dizini Oluşturma](search-howto-indexing-azure-tables.md). |

## <a name="wizard-output"></a>Sihirbaz çıktısı

Arka planda, sihirbaz aşağıdaki nesneleri oluşturur, yapılandırır ve çağırır. Sihirbaz çalıştırdıktan sonra çıktısını portal sayfalarında bulabilirsiniz. Hizmetinizin Genel Bakış sayfasında dizinler, dizinleyiciler, veri kaynakları ve beceri kümeleri listeleri vardır. Dizin tanımları portalda tam JSON olarak görüntülenebilir. Diğer tanımlar için, belirli nesneleri almak için [REST API'yi](https://docs.microsoft.com/rest/api/searchservice/) kullanabilirsiniz.

| Nesne | Açıklama | 
|--------|-------------|
| [Veri Kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Bağlantı bilgilerini kimlik bilgileri de dahil olmak üzere kaynak verilere kadar devam eder. Bir veri kaynağı nesne yalnızca dizinleyiciler ile kullanılır. | 
| [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) | Tam metin arama ve diğer sorgular için kullanılan fiziksel veri yapısı. | 
| [Skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) | Görüntü dosyalarından bilgileri analiz etmek ve ayıklamak da dahil olmak üzere içeriği işlemek, dönüştürmek ve şekillendirmek için eksiksiz bir yönerge ler kümesi. Çok basit ve sınırlı yapılar dışında, zenginleştirme sağlayan bir Bilişsel Hizmetler kaynağına bir referans içerir. İsteğe bağlı olarak, bir bilgi deposu tanımı da içerebilir.  | 
| [Dizin Oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer)  | Hata teslimi ve base-64 kodlaması için veri kaynağı, hedef dizini, isteğe bağlı skillset, isteğe bağlı zamanlama ve isteğe bağlı yapılandırma ayarlarını belirten bir yapılandırma nesnesi. |


## <a name="how-to-start-the-wizard"></a>Sihirbaz nasıl başlatılır?

İçe Alma verisi sihirbazı hizmete Genel Bakış sayfasındaki komut çubuğundan başlatılır.

1. Azure [portalında,](https://portal.azure.com)panodan arama hizmeti sayfasını açın veya [hizmet listenizde servisinizi bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)

2. Üstteki hizmete genel bakış **sayfasında, verileri içe aktar'ı**tıklatın.

   ![Portalda veri komutunu alma](./media/search-import-data-portal/import-data-cmd2.png "Veri Aktar sihirbazını başlatın")

Azure Cosmos DB, Azure SQL Veritabanı ve Azure Blob depolama gibi diğer Azure hizmetlerinden **alma verilerini** de başlatabilirsiniz. Hizmete genel bakış sayfasındaki sol gezinme bölmesinde **Azure Bilişsel Arama Ekle'ye** bakın.

<a name="index-definition"></a>

## <a name="how-to-edit-or-finish-an-index-schema-in-the-wizard"></a>Sihirbazda bir dizin şemasını nasıl dizin le tamamlarveya tamamlar

Sihirbaz, giriş veri kaynağından elde edilen belgelerle doldurulacak eksik bir dizin oluşturur. İşlevsel bir dizin için aşağıdaki öğelerin tanımlandığınızdan emin olun.

1. Alan listesi tamamlandı mı? Örneklenen yeni alanlar ekleyin ve bir arama deneyimine değer katmayen veya [filtre ifadesinde](search-query-odata-filter.md) veya [puanlama profilinde](index-add-scoring-profiles.md)kullanılmayacak alanları kaldırın.

1. Veri türü gelen veriler için uygun mu? Azure Bilişsel [Arama, varlık veri modelini (EDM) veri türlerini](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)destekler. Azure SQL verileri için, eşdeğer değerleri ortaya koyan [eşleme grafiği](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#TypeMapping) vardır. Daha fazla arka plan için [Alan eşlemeleri ve dönüşümleri'ne](search-indexer-field-mappings.md)bakın.

1. *Anahtar*olarak hizmet verebilir bir alan var mı? Bu alan Edm.string olmalı ve bir belgeyi benzersiz olarak tanımlamalıdır. İlişkisel veriler için birincil anahtara eşlenebilir. Lekeler için, bu `metadata-storage-path`olabilir . Alan değerleri boşluklar veya tireler içeriyorsa, bu karakterlerin doğrulama denetimini bastırmak için **Gelişmiş seçenekler**altında **Bir Dizin Oluştur** adımında **Base-64 Encode Key** seçeneğini ayarlamanız gerekir.

1. Bu alanın bir dizinde nasıl kullanıldığını belirlemek için öznitelikleri ayarlayın. 

   Öznitelikler dizindeki alanların fiziksel ifadesini belirlediğinden, bu adımla zaman ayırın. Öznitelikleri daha sonra, hatta programlı olarak değiştirmek istiyorsanız, hemen hemen her zaman dizin bırakın ve yeniden gerekir. Aranabilir ve **Geri Alınabilen** gibi temel özniteliklerin depolama üzerinde ihmal edilebilir bir **Searchable** [etkisi](search-what-is-an-index.md#index-size)vardır. Filtreleri etkinleştirmek ve önericikullanmak depolama gereksinimlerini artırır. 
   
   + **Aranabilir** tam metin arama sağlar. Serbest biçim sorgularında veya sorgu ifadelerinde kullanılan her alan bu özelliğe sahip olmalıdır. Ters dizinler, **Aranabilir**olarak işaretlediğiniz her alan için oluşturulur.

   + **Alınabilir** arama sonuçlarında alanı döndürür. Arama sonuçlarına içerik sağlayan her alan bu özelliğe sahip olmalıdır. Bu alanın ayarlanması, dizin boyutunu kayda değer bir şekilde etkilemez.

   + **Filtrelenebilir** alan filtre ifadeleri başvurulmasını sağlar. **$filter** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır. Filtre ifadeleri tam eşleşmeler içindir. Metin dizeleri bozulmadan kaldığından, tam olarak içeriği barındırmak için ek depolama alanı gerekir.

   + **Facetable** yönlü navigasyon için alan sağlar. Yalnızca **Filtrelenebilir** olarak işaretlenmiş alanlar **Facetable**olarak işaretlenebilir.

   + **Sıralanabilir** alan bir tür kullanılmasına izin verir. **$Orderby** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır.

1. [Sözlü analize](search-lucene-query-architecture.md#stage-2-lexical-analysis)mi ihtiyacınız var? **Aranabilir**Edm.string alanları için, dil destekli dizin oluşturma ve sorgulama istiyorsanız bir **Çözümleyici** ayarlayabilirsiniz. 

   Varsayılan değer *Standart Lucene'dir,* ancak microsoft'un çözümleyicisini düzensiz isim ve fiil biçimlerini çözme gibi gelişmiş sözlü işlemler için kullanmak isterseniz *Microsoft English'i* seçebilirsiniz. Portalda yalnızca dil çözümleyicileri belirtilebilir. Özel bir çözümleyici veya Anahtar Kelime, Desen ve benzeri gibi bir dil çözümleyicisi kullanarak, programlı yapılmalıdır. Çözümleyiciler hakkında daha fazla bilgi için [bkz.](search-language-support.md)

1. Otomatik tamamlama veya önerilen sonuçlar şeklinde typeahead işlevselliğe mi ihtiyacınız var? [İleride sorgu önerilerini](index-add-suggesters.md) etkinleştirmek ve seçili alanlarda otomatik tamamlamayı etkinleştirmek için **Öneren** onay kutusunu seçin. Önericiler, dizininizdeki belirteç terimlerin sayısına eklenir ve böylece daha fazla depolama alanı tüketir.


## <a name="next-steps"></a>Sonraki adımlar

Sihirbazın yararlarını ve sınırlamalarını anlamanın en iyi yolu, sihirbazın içinden geçmektir. Aşağıdaki hızlı başlatma her adımda size yol gösterin.

> [!div class="nextstepaction"]
> [Azure portalını kullanarak Azure Bilişsel Arama dizini oluşturma](search-get-started-portal.md)