---
title: Azure portal kullanarak bir arama dizinine veri aktarma Azure Search
description: Azure VM 'lerinde Cosmos DB, BLOB depolama, tablo depolama, SQL veritabanı ve SQL Server Azure verilerini gezinmek için Azure portal veri alma Sihirbazı 'nın nasıl kullanılacağını öğrenin.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 89f43227cfca3519a4985c5c961cf0b3c5774177
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936919"
---
# <a name="import-data-wizard-for-azure-search"></a>Azure Search için veri içeri aktarma Sihirbazı

Azure portal, prototip yazmak ve bir dizin yüklemek için Azure Search panosunda **veri alma** Sihirbazı sağlar. Bu makalede, Sihirbazı, girişleri ve çıkışları ve bazı kullanım bilgilerini kullanma avantajları ve sınırlamaları ele alınmaktadır. Yerleşik örnek verileri kullanarak sihirbazda adım adım yönergeler için Azure portal hızlı başlangıcı [kullanılarak Azure Search dizin oluşturma](search-get-started-portal.md) bölümüne bakın.

Bu sihirbazın gerçekleştirdiği işlemler şunlardır:

1-desteklenen bir Azure veri kaynağına bağlanın.

2-kaynak verileri örnekleyerek çıkartılan bir dizin şeması oluşturun.

3-isteğe bağlı olarak, içerik ve yapıyı ayıklamak veya oluşturmak için AI zenginleştirmelerinin ekleyin.

4-nesneleri oluşturmak, verileri içeri aktarmak, bir zamanlama ve diğer yapılandırma seçeneklerini ayarlamak için Sihirbazı çalıştırın.

Sihirbaz, arama hizmetinize kaydedilen, program aracılığıyla veya diğer araçlara erişebileceğiniz birçok nesne çıkarır.

## <a name="advantages-and-limitations"></a>Avantajlar ve sınırlamalar

Herhangi bir kod yazmadan önce, prototip oluşturma ve kavram kanıtı testi için Sihirbazı kullanabilirsiniz. Sihirbaz dış veri kaynaklarına bağlanır, ilk dizin oluşturmak için verileri örnekler ve sonra verileri JSON belgeleri olarak Azure Search bir dizine aktarır. 

Örnekleme, bir dizin şemasının Çıkarsanan ve bazı sınırlamaları olan bir işlemdir. Veri kaynağı oluşturulduğunda, sihirbaz, veri kaynağının bir parçası olan sütunları belirlemek için bir belge örneği seçer. Çok büyük veri kaynakları için saat sürebileceği için tüm dosyalar okunmaz. Belge seçimi verildiğinde, alan adı veya türü gibi kaynak meta veriler dizin şemasında bir alan koleksiyonu oluşturmak için kullanılır. Kaynak verilerin karmaşıklığına bağlı olarak, ilk şemayı doğruluk için düzenlemeniz veya daha fazla işlem için uzatmanız gerekebilir. Değişikliklerinizi Dizin tanımı sayfasında satır içi yapabilirsiniz.

Genel olarak, Sihirbazı kullanmanın avantajları net değildir: gereksinimler karşılandığında, birkaç dakika içinde bir sorgulanabilir Dizin prototipi oluşturabilirsiniz. Verilerin JSON belgeleri olarak sağlanması gibi bazı karmaşıklıklardan bazıları sihirbaz tarafından işlenir.

Bilinen sınırlamalar aşağıdaki gibi özetlenir:

+ Sihirbaz yinelemeyi veya yeniden kullanmayı desteklemez. Sihirbaz aracılığıyla geçen her geçişte yeni bir dizin, beceri ve Dizin Oluşturucu yapılandırması oluşturulur. Yalnızca veri kaynakları kalıcı hale getirilir ve sihirbaz içinde yeniden kullanılabilir. Diğer nesneleri düzenlemek veya iyileştirmek için, yapıları almak ve değiştirmek için REST API 'Lerini veya .NET SDK 'sını kullanmanız gerekir.

+ Kaynak içerik, aynı abonelik kapsamındaki bir hizmette desteklenen bir Azure veri kaynağında bulunmalıdır.

+ Örnekleme, kaynak verilerinin bir alt kümesi üzerinde. Büyük veri kaynakları için sihirbazın alanları kaçırmak mümkündür. Örnekleme yetersizse şemayı genişletmeniz veya çıkarılan veri türlerini düzeltmeniz gerekebilir.

+ Portalda kullanıma sunulan AI zenginleştirme, birkaç yerleşik becerilerle sınırlıdır. 

+ Sihirbaz tarafından oluşturulabilen bir [bilgi deposu](knowledge-store-concept-intro.md), birkaç varsayılan yansıtmayı sınırlamalıdır. Sihirbaz tarafından oluşturulan zenginleştirilmiş belgeleri kaydetmek istiyorsanız, blob kapsayıcısı ve tabloları varsayılan ad ve yapıyla gelir.

<a name="data-source-inputs"></a>

## <a name="data-source-input"></a>Veri kaynağı girişi

**Veri Içeri aktarma** sihirbazı, Azure Search dizin oluşturma, meta veri okuma, verileri içerik ve yapıyı okumak için belgeleri çözerek ve içeriği JSON olarak seri hale getirmek için kullanılan iç mantığı kullanarak bir dış veri kaynağına bağlanır. Azure Search ' ye sonraki içeri aktarma.

Yalnızca tek bir tablo, veritabanı görünümü veya eşdeğer veri yapısından içeri aktarabilirsiniz; ancak yapı hiyerarşik veya iç içe geçmiş alt yapılar içerebilir. Daha fazla bilgi için bkz. [karmaşık türleri Modelme](search-howto-complex-data-types.md).

Sihirbazı çalıştırmadan önce bu tek tablo veya görünüm oluşturmanız gerekir ve içerik içermesi gerekir. Belirgin nedenlerle **veri alma** Sihirbazı ' nı boş bir veri kaynağında çalıştırmak mantıklı değildir.

|  Seçim | Açıklama |
| ---------- | ----------- |
| **Mevcut veri kaynağı** |Arama hizmetinizde zaten tanımlı dizin oluşturucular varsa, yeniden kullanabileceğiniz bir veri kaynağı tanımınız olabilir. Azure Search, veri kaynağı nesneleri yalnızca dizin oluşturucular tarafından kullanılır. Programlı olarak veya **veri Içeri aktarma** Sihirbazı aracılığıyla bir veri kaynağı nesnesi oluşturabilir ve bunları gerektiği gibi yeniden kullanabilirsiniz.|
| **Örnekler**| Azure Search, öğreticiler ve hızlı başlangıçlarda kullanılan iki yerleşik örnek veri kaynağı sağlar: gerçek bir emlak SQL veritabanı ve Cosmos DB üzerinde barındırılan bir oteller veritabanı. Oteller örneğine bağlı olarak bir izlenecek yol için Azure portal hızlı başlangıçta [Dizin oluşturma](search-get-started-portal.md) bölümüne bakın. |
| [**Azure SQL veritabanı**](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) |Hizmet adı, okuma izni olan bir veritabanı kullanıcısının kimlik bilgileri ve bir veritabanı adı, sayfada ya da bir ADO.NET bağlantı dizesi aracılığıyla belirtilebilir. Özellikleri görüntülemek veya özelleştirmek için bağlantı dizesi seçeneğini belirleyin. <br/><br/>Satır kümesini sağlayan tablo veya görünümün sayfada belirtilmesi gerekir. Bu seçenek bağlantı başarılı olduktan sonra, bir seçim yapabilmeniz için bir açılır liste vererek görüntülenir.|
| **Azure VM 'de SQL Server** |Tam hizmet adını, kullanıcı KIMLIĞINI ve parolayı ve bir bağlantı dizesi olarak veritabanını belirtin. Bu veri kaynağını kullanmak için, yerel depoda bağlantıyı şifreleyen bir sertifikayı daha önce yüklemiş olmanız gerekir. Yönergeler için bkz. [SQL VM bağlantısı Azure Search](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>Satır kümesini sağlayan tablo veya görünümün sayfada belirtilmesi gerekir. Bu seçenek bağlantı başarılı olduktan sonra, bir seçim yapabilmeniz için bir açılır liste vererek görüntülenir. |
| [**Azure Cosmos DB**](search-howto-index-cosmosdb.md)|Gereksinimler hesap, veritabanı ve koleksiyonu içerir. Koleksiyondaki tüm belgeler dizine dahil edilir. Satır kümesini düzleştirmek veya filtrelemek için bir sorgu tanımlayabilir veya sorguyu boş bırakabilirsiniz. Bu sihirbazda bir sorgu gerekli değildir.|
| [**Azure Blob depolama**](search-howto-indexing-azure-blob-storage.md) |Gereksinimler depolama hesabı ve bir kapsayıcı içerir. İsteğe bağlı olarak, dizin oluşturma amacıyla blob adları sanal bir adlandırma kuralını izlediyseniz, adın sanal dizin bölümünü kapsayıcı altında bir klasör olarak belirtebilirsiniz. Daha fazla bilgi için bkz. [Dizin blobu depolaması](search-howto-indexing-azure-blob-storage.md) . |
| [**Azure Tablo depolama**](search-howto-indexing-azure-tables.md) |Depolama hesabı ve tablo adı gereksinimleri vardır. İsteğe bağlı olarak, tabloların bir alt kümesini almak için bir sorgu belirtebilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturma tablosu depolaması](search-howto-indexing-azure-tables.md) . |

## <a name="wizard-output"></a>Sihirbaz çıkışı

Sihirbaz, arka planda aşağıdaki nesneleri oluşturur, yapılandırır ve çağırır. Sihirbaz çalıştıktan sonra, çıktısını Portal sayfalarında bulabilirsiniz. Hizmetinizin Genel Bakış sayfasında Dizin, Dizin oluşturucular, veri kaynakları ve becerileri listeleri vardır. Dizin tanımları portalda tam JSON olarak görüntülenebilir. Diğer tanımlarda, belirli nesneleri almak için [REST API](https://docs.microsoft.com/rest/api/searchservice/) kullanabilirsiniz.

| Nesne | Açıklama | 
|--------|-------------|
| [Veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Bağlantı bilgilerini, kimlik bilgileri de dahil olmak üzere kaynak verilere devam ettirir. Veri kaynağı nesnesi yalnızca dizin oluşturucular ile kullanılır. | 
| [Index](https://docs.microsoft.com/rest/api/searchservice/create-index) | Tam metin araması ve diğer sorgular için kullanılan fiziksel veri yapısı. | 
| [Beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) | Görüntü dosyalarından bilgi çözümleme ve ayıklama da dahil olmak üzere içeriği işleme, dönüştürmeye ve şekillendirmeye yönelik bir dizi yönergedir. Çok basit ve sınırlı yapılar haricinde, zenginleştirme sağlayan bilişsel hizmetler kaynağına bir başvuru içerir. İsteğe bağlı olarak, bir bilgi deposu tanımı da içerebilir.  | 
| [Dizinleyic](https://docs.microsoft.com/rest/api/searchservice/create-indexer)  | Bir veri kaynağı, hedef dizin, isteğe bağlı bir beceri, isteğe bağlı zamanlama ve hata elde etme ve Base-64 kodlaması için isteğe bağlı yapılandırma ayarları belirleyen bir yapılandırma nesnesi. |


## <a name="how-to-start-the-wizard"></a>Sihirbazı başlatma

Veri Içeri aktarma Sihirbazı, hizmet Genel Bakış sayfasındaki komut çubuğundan başlatılır.

1. [Azure Portal](https://portal.azure.com), panodaki arama hizmeti sayfasını açın veya hizmet listesinde [hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) .

2. En üstteki hizmete genel bakış sayfasında, **verileri Içeri aktar**' a tıklayın.

   ![Verileri Içeri aktarma Portal 'da](./media/search-import-data-portal/import-data-cmd2.png "Verileri İçeri Aktarma Sihirbazı 'nı başlatma")

Ayrıca Azure Cosmos DB, Azure SQL veritabanı ve Azure Blob depolama da dahil olmak üzere diğer Azure hizmetlerinden **Içeri aktarma verilerini** de başlatabilirsiniz. Hizmet genel bakış sayfasında sol gezinti bölmesinde **Azure Search Ekle** ' ye bakın.

<a name="index-definition"></a>

## <a name="how-to-edit-or-finish-an-index-schema-in-the-wizard"></a>Sihirbazda Dizin şemasını düzenleme veya tamamlama

Sihirbaz, giriş veri kaynağından elde edilen belgelerle doldurulacak tamamlanmamış bir dizin oluşturur. İşlevsel dizin için aşağıdaki öğelerin tanımlandığından emin olun.

1. Alan listesi tamamlansın mı? Eksik örneklemenin bulunduğu yeni alanlar ekleyin ve bir arama deneyimine değer eklememe veya bir [filtre ifadesinde](search-query-odata-filter.md) veya [Puanlama profilinde](index-add-scoring-profiles.md)kullanılmayacak olan her türlü değeri kaldırın.

1. Veri türü gelen veriler için uygun mu? Azure Search, [varlık veri modeli (EDM) veri türlerini](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)destekler. Azure SQL verileri için eşdeğer değerleri oluşturan [eşleme grafiği](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#mapping-between-sql-and-azure-search-data-types) vardır. Daha fazla arka plan için bkz. [alan eşlemeleri ve dönüştürmeleri](search-indexer-field-mappings.md).

1. *Anahtar*olarak kullanılabilecek bir alanınız var mı? Bu alan Edm. String olmalı ve bir belgeyi benzersiz şekilde tanımlamalıdır. İlişkisel veriler için bir birincil anahtarla eşlenmiş olabilir. Blob 'lar için `metadata-storage-path` olabilir. Alan değerleri boşluk veya tire içeriyorsa, bu karakterlerin doğrulama denetimini bastırmak için, **Dizin Oluşturucu oluştur** adımında, **Gelişmiş Seçenekler**altında **Base-64 kodlama anahtarı** seçeneğini ayarlamanız gerekir.

1. Bu alanın bir dizinde nasıl kullanıldığını belirleme özniteliklerini belirleyin. 

   Öznitelikteki alanların fiziksel ifadesini belirlerken, bu adımla zamandan yararlanın. Öznitelikleri daha sonra, programlama yoluyla da değiştirmek istiyorsanız, neredeyse her zaman dizini bırakıp yeniden oluşturmanız gerekir. **Aranabilir** ve **alınabilir** gibi temel özniteliklerin [depolama alanı üzerinde daha okunaklı bir etkisi](search-what-is-an-index.md#storage-implications)vardır. Filtreleri etkinleştirme ve öneri araçları kullanma depolama gereksinimlerini artırma. 
   
   + **Aranabilir** tam metin aramasını sunar. Serbest form sorgularında veya sorgu ifadelerinde kullanılan her alan bu özniteliğe sahip olmalıdır. Ters çevrilen dizinler, **aranabilir**olarak işaretlediğiniz her alan için oluşturulur.

   + **Alınabilir** , arama sonuçlarında alanı geri döndürür. Arama sonuçlarına içerik sağlayan her alan bu özniteliğe sahip olmalıdır. Bu alanın ayarlanması, etkin dizin boyutunu etkilemez.

   + **Filtrelenebilir** , alana filtre ifadelerinde başvurulmak için izin verir. **$Filter** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır. Filtre ifadeleri tam eşleşmeler içindir. Metin dizeleri değişmeden kaldığı için, tam içeriğe uyum sağlamak için ek depolama alanı gereklidir.

   + Çok **yönlü tablo** , çok yönlü gezinme için alanı sunar. Yalnızca **Filterable** olarak işaretlenen alanlar, çok **yönlü tablo**olarak işaretlenebilir.

   + **Sıralanabilir** , alanın bir sıralama içinde kullanılmasına izin verir. **$OrderBy** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır.

1. [Sözlü Analize](search-lucene-query-architecture.md#stage-2-lexical-analysis)ihtiyacınız var mı? **Aranabilir**olan Edm. String alanları için, dil ile Gelişmiş dizin oluşturma ve sorgulama Istiyorsanız bir **çözümleyici** ayarlayabilirsiniz. 

   Varsayılan değer *Standart Lucene* ' dir, ancak düzensiz ad ve fiil formlarını çözme gibi gelişmiş sözcük Işleme için Microsoft 'un çözümleyicisini kullanmak Istiyorsanız *Microsoft English* ' i seçebilirsiniz. Portalda yalnızca dil Çözümleyicileri belirtilebilir. Özel bir çözümleyici veya anahtar sözcük, model, vb. gibi dil olmayan bir çözümleyici kullanmak programlı olarak yapılmalıdır. Çözümleyiciler hakkında daha fazla bilgi için bkz. [dil Çözümleyicileri ekleme](search-language-support.md).

1. Otomatik tamamlama veya önerilen sonuçlar biçiminde typeahead işlevselliği mi gerekiyor? Seçili alanlarda [typeahead sorgu önerilerini ve otomatik tamamlamayı](index-add-suggesters.md) etkinleştirmek için **öneri aracı** onay kutusunu seçin. Öneri araçları, dizininizdeki simgeleştirilmiş koşulların sayısına ekleyin ve bu nedenle daha fazla depolama alanı kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Sihirbazın sağladığı avantajları ve sınırlamaları anlamanın en iyi yolu, adım adım ilermektir. Aşağıdaki hızlı başlangıç, her adımda size rehberlik eder.

> [!div class="nextstepaction"]
> [Azure portal kullanarak Azure Search dizin oluşturma](search-get-started-portal.md)