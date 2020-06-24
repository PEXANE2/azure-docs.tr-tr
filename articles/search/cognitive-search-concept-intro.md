---
title: AI zenginleştirme kavramları
titleSuffix: Azure Cognitive Search
description: İçerik ayıklama, doğal dil işleme (NLP) ve görüntü işleme, Azure Bilişsel Arama dizinlerinde hem önceden tanımlanmış bilişsel yetenekler hem de özel AI algoritmaları ile aranabilir içerik oluşturmak için kullanılır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/18/2020
ms.openlocfilehash: 196562d376b8268ecf47f8133a5b1c8a122c38c5
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052272"
---
# <a name="ai-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama AI zenginleştirme

AI zenginleştirme, görüntülerden, bloblardan ve diğer yapılandırılmamış veri kaynaklarından metin ayıklamak için kullanılabilen [dizin oluşturucularının](search-indexer-overview.md) bir uzantısıdır. Zenginleştirme ve ayıklama, Dizin Oluşturucu çıkış nesnelerinde (bir [arama dizini](search-what-is-an-index.md) veya [bilgi deposu](knowledge-store-concept-intro.md)) içeriğinizi daha aranabilir hale getirir. 

Ayıklama ve zenginleştirme, Dizin Oluşturucu odaklı işlem hattına iliştirilmiş bilişsel *yetenekler* kullanılarak uygulanır. Microsoft 'tan yerleşik becerileri kullanabilir veya dış işlemeyi, oluşturduğunuz [*özel bir yeteneğe*](cognitive-search-create-custom-skill-example.md) ekleyebilirsiniz. Özel bir beceri örnekleri, finans, bilimsel yayınlar veya ilaç gibi belirli bir etki alanını hedefleyen özel bir varlık modülü veya belge Sınıflandırıcısı olabilir.

Yerleşik yetenekler şu kategorilere ayrılır: 

+ **Doğal dil işleme** becerileri [varlık tanıma](cognitive-search-skill-entity-recognition.md), [dil algılama](cognitive-search-skill-language-detection.md), [anahtar tümceciği ayıklama](cognitive-search-skill-keyphrases.md), metin işleme, yaklaşım [algılama](cognitive-search-skill-sentiment.md)ve [PII algılamayı](cognitive-search-skill-pii-detection.md)içerir. Bu becerilerle, yapılandırılmamış metin, bir dizinde aranabilir ve filtrelenebilir alanlar olarak eşlenir.

+ **Görüntü işleme** becerileri, [optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) ve yüz algılama, görüntü yorumu, görüntü tanıma (sık sık insanlar ve yer işaretleri) veya görüntü yönü gibi özellikler gibi [Görsel özelliklerin](cognitive-search-skill-image-analysis.md)tanımlanmasını içerir. Bu yetenekler, görüntü içeriğinin metin sunumlarını oluşturur ve bu da Azure Bilişsel Arama 'nin sorgu yeteneklerini kullanarak aranabilir hale getirir.

![Zenginleştirme ardışık düzen diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "zenginleştirme ardışık düzenine genel bakış")

Azure Bilişsel Arama 'daki yerleşik yetenekler, Bilişsel Hizmetler API'si: [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)'de önceden eğitilen makine öğrenimi modellerini temel alır. İçerik işleme sırasında bu kaynaklardan yararlanmak istiyorsanız bilişsel hizmetler kaynağı ekleyebilirsiniz.

Doğal dil ve görüntü işleme, veri alma aşaması sırasında, Azure Bilişsel Arama 'te aranabilir bir dizinde belge kompozisyonunun bir parçası haline gelmesine neden olacak şekilde uygulanır. Verilerin bir Azure veri kümesi olarak kaynağı oluşturulur ve ardından ihtiyacınız olan [yerleşik becerileri](cognitive-search-predefined-skills.md) kullanarak bir dizin oluşturma işlem hattı üzerinden gönderilir.  

## <a name="when-to-use-ai-enrichment"></a>AI zenginleştirme ne zaman kullanılır?

Ham içeriğiniz yapılandırılmamış metin, görüntü içeriği veya dil algılama ve çeviri gerektiren içerikler ise, yerleşik bilişsel yetenekler kullanmayı göz önünde bulundurmanız gerekir. Yerleşik bilişsel yetenekler aracılığıyla AI uygulamak, bu içeriğin kilidini açabilir ve arama ve veri bilimi uygulamalarınızda değerini ve yardımcı programını artırır. 

Ayrıca, ardışık düzen ile birleştirmek istediğiniz açık kaynaklı, üçüncü taraf veya birinci taraf kodunuz varsa, özel bir yetenek eklemeyi de düşünebilirsiniz. Çeşitli belge türlerinin salgörüme özelliklerini tanımlayan sınıflandırma modelleri bu kategoriye girer, ancak içeriğinize değer ekleyen herhangi bir paket kullanılabilir.

### <a name="more-about-built-in-skills"></a>Yerleşik yetenekler hakkında daha fazla bilgi

Yerleşik yetenekler kullanılarak derlenen bir [beceri](cognitive-search-defining-skillset.md) , aşağıdaki uygulama senaryolarında iyi şekilde uygundur:

+ Tam metin araması yapmak istediğiniz taranmış belgeler (JPEG). JPEG dosyalarından metin tanımlamak, ayıklamak ve almak için bir optik karakter tanıma (OCR) yeteneği ekleyebilirsiniz.

+ Birleştirilmiş görüntü ve metin içeren PDF 'Ler. PDF 'Lerdeki metin, enzenginleştirme adımlarının kullanımı olmadan dizin oluşturma sırasında ayıklanabilir, ancak görüntü ve doğal dil işlemenin eklenmesi genellikle standart bir dizin oluşturma işleminin sağladığı daha iyi bir sonuç üretebilir.

+ Dil algılamayı ve muhtemelen metin çevirisini uygulamak istediğiniz çok dilli içerik.

+ Büyük bir belgede gizli bir anlamı veya bağlamı olan içerik içeren yapılandırılmamış veya yarı yapılandırılmış belgeler. 

  Genellikle, tek bir "alan" ile paketlenmiş büyük bir içerik gövdesini içeren bloblardır. Bir dizin oluşturucuya görüntü ve doğal dil işleme becerileri ekleyerek, ham içerikte yer alan yeni bilgiler oluşturabilir, aksi takdirde ayrı alanlar olarak ortaya çıkmış olursunuz. Kullanıma hazır kullanıma hazır yerleşik bilişsel yetenekler: anahtar tümceciği ayıklama, yaklaşım Analizi ve varlık tanıma (kişiler, kuruluşlar ve konumlar).

  Ayrıca, yerleşik yetenekler metin bölünmüş, birleştirme ve şekil işlemleri aracılığıyla içeriği yeniden yapılandırmak için de kullanılabilir.

### <a name="more-about-custom-skills"></a>Özel yetenekler hakkında daha fazla bilgi

Özel yetenekler, formları tanıma gibi daha karmaşık senaryoları veya özel [Beceri Web arabirimine](cognitive-search-custom-skill-interface.md)sağladığınız ve kaydırabileceğiniz bir modeli kullanarak özel varlık algılamayı destekleyebilir. Özel yeteneklerin çeşitli örnekleri, [form tanıyıcı](/azure/cognitive-services/form-recognizer/overview), [Bing varlık arama API'si](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example)tümleştirmesi ve [özel varlık tanıma](https://github.com/Microsoft/SkillsExtractorCognitiveSearch)içerir.

<a name="enrichment-steps">Bir zenginleştirme ardışık düzeninde # # adım</a>

Bir zenginleştirme işlem hattı, [*Dizin oluşturucular*](search-indexer-overview.md)temel alır. Dizin oluşturucular, belge çözme için dizin ve veri kaynağınız arasındaki alan eşlemelerine göre bir dizini doldurur. Artık, Özellikleri Dizin oluşturucular, kesme ve zenginleştirmenin tanımladığınız beceri (ler) e göre belgeleri. Dizin oluşturulduktan sonra, [Azure bilişsel arama tarafından desteklenen tüm sorgu türleri](search-query-overview.md)aracılığıyla arama istekleri aracılığıyla içeriğe erişebilirsiniz.  Dizin oluşturuculardan yeni olduğunuzda, bu bölüm adımlarda size yol gösterir.

### <a name="step-1-connection-and-document-cracking-phase"></a>1. Adım: bağlantı ve belge çözme aşaması

İşlem hattının başlangıcında, yapılandırılmamış metin veya metin olmayan içerik (örneğin, görüntüler, taranmış belgeler veya JPEG dosyaları) vardır. Verilerin bir Dizin Oluşturucu tarafından erişilebilen bir Azure veri depolama hizmetinde bulunması gerekir. Dizin oluşturucular, kaynak verilerden metin ayıklamak için kaynak belgeleri "çözebilir". Belge çözme işlemi, dizin oluşturma sırasında metin olmayan kaynaklardaki metin içeriğini ayıklama veya oluşturma işlemidir.

![Belge çözme aşaması](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Belge çözme")

 Desteklenen kaynaklar Azure Blob depolama, Azure Tablo depolama, Azure SQL veritabanı ve Azure Cosmos DB içerir. Metin tabanlı içerik şu dosya türlerinden ayıklanabilir: PDF 'Ler, Word, PowerPoint, CSV dosyaları. Tam liste için bkz. [desteklenen biçimler](search-howto-indexing-azure-blob-storage.md#supported-document-formats). Dizin oluşturma zaman alır, bu nedenle küçük, temsili bir veri kümesiyle başlar ve sonra çözüm malarınız kadar artımlı olarak oluşturun.

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>2. Adım: bilişsel beceriler ve zenginleştirme aşaması

Zenginleştirme, Atomik işlemler gerçekleştirirken bilişsel *yetenekler* ile gerçekleştirilir. Örneğin, bir PDF 'YI kırdıktan sonra, dizinde yerel olarak kullanılamayan yeni alanlar oluşturmak için varlık tanıma, dil algılama veya anahtar tümceciği ayıklama uygulayabilirsiniz. Tamamen, işlem hattınızda kullanılan yeteneklerin koleksiyonuna bir *beceri*denir.  

![Zenginleştirme aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "zenginleştirme aşaması")

Bir beceri, sağladığınız yerleşik bilişsel [becerileri](cognitive-search-predefined-skills.md) veya [özel becerileri](cognitive-search-create-custom-skill-example.md) temel alır ve beceri 'e bağlanırsınız. Bir beceri, en düşük veya çok karmaşık olabilir ve yalnızca işleme türü değil, işlem sırasını da belirler. Bir beceri ve bir dizin oluşturucunun parçası olarak tanımlanan alan eşlemeleri, zenginleştirme işlem hattını tam olarak belirtir. Tüm bu parçaları birlikte çekme hakkında daha fazla bilgi için bkz. [beceri tanımlama](cognitive-search-defining-skillset.md).

Dahili olarak, işlem hattı zenginleştirilmiş bir belge koleksiyonu oluşturur. Zenginleştirilmiş belgelerin hangi bölümlerinin arama dizininizdeki dizinlenebilir alanlarla eşlenmeli olduğuna karar verebilirsiniz. Örneğin, anahtar tümceciği ayıklama ve varlık tanıma becerileri uyguladıysanız, bu yeni alanlar zenginleştirilmiş belgenin bir parçası haline gelir ve dizininizdeki alanlarla eşleştirilebilir. Giriş/çıkış formaları hakkında daha fazla bilgi için bkz. [ek açıklamalar](cognitive-search-concept-annotations-syntax.md) .

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Zenginleştirmelerin kaydedileceği bir knowledgeStore öğesi ekleme

[Arama REST API-sürüm = 2019-05 -06-Preview](search-api-preview.md) , becerileri 'Yi `knowledgeStore` , Azure depolama bağlantısı ve zenginlerin nasıl depolandığını tanımlayan projeksiyonlar sağlayan bir tanım ile genişletir. Bu, dizininiz için de bir ektir. Standart bir AI ardışık düzeninde, zenginleştirilmiş belgeler yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile zenginleştirilmiş belgeler korunur. Daha fazla bilgi için bkz. [bilgi deposu (Önizleme)](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>3. Adım: arama dizini ve sorgu tabanlı erişim

İşlem tamamlandığında, Azure Bilişsel Arama 'de tam metin araması yapılan, zenginleştirilmiş belgelerden oluşan bir arama dizinine sahip olursunuz. [Dizin sorgulama](search-query-overview.md) , geliştiricilerin ve kullanıcıların işlem hattı tarafından oluşturulan zenginleştirilmiş içeriğe nasıl erişmesinden oluşur. 

![Arama simgesiyle Dizin](./media/cognitive-search-intro/search-phase-blowup.png "Arama simgesiyle Dizin")

Dizin, Azure Bilişsel Arama için oluşturabileceğiniz herhangi bir diğer benzer: özel çözümleyiciler ile destekleyebilirsiniz, benzer arama sorgularını çağırabilir, filtrelenmiş arama ekleyebilir veya arama sonuçlarını yeniden şekillendirmek için Puanlama profilleriyle denemeler yapabilirsiniz.

Dizinler, Puanlama profilleri ve eş anlamlı haritalar gibi belirli bir dizine iliştirilmiş alanları, öznitelikleri ve diğer yapıları tanımlayan bir dizin şemasından oluşturulur. Bir dizin tanımlandıktan ve doldurulduktan sonra, yeni ve güncelleştirilmiş kaynak belgeleri almak için artımlı olarak dizin oluşturabilirsiniz. Belirli değişiklikler tam yeniden oluşturma gerektirir. Şema tasarımı kararlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

**Denetim listesi: tipik bir iş akışı**

1. Azure Kaynak verilerinizi bir temsili örneğine alt küme haline koyun. Dizin oluşturma zaman alır, bu nedenle küçük, temsili bir veri kümesiyle başlar ve sonra çözüm malarınız kadar artımlı olarak oluşturun.

1. Veri alımı için bir bağlantı dizesi sağlamak üzere Azure Bilişsel Arama 'de bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) oluşturun.

1. Zenginleştirme adımları ile bir [beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) oluşturun.

1. [Dizin şemasını](https://docs.microsoft.com/rest/api/searchservice/create-index)tanımlayın. *Alanlar* koleksiyonu, kaynak verilerden alanlar içerir. Ayrıca, zenginleştirme sırasında oluşturulan içerikler için oluşturulan değerleri tutmak üzere ek alanlar da saplama yapmanız gerekir.

1. Veri kaynağına, beceri ve dizine başvuran [Dizin oluşturucuyu](https://docs.microsoft.com/rest/api/searchservice/create-indexer) tanımlayın.

1. Dizin Oluşturucu içinde *Outputfieldmappings*ekleyin. Bu bölüm, Beceri (adım 3 ' te) çıktısını Dizin şemasındaki (adım 4 ' te) giriş alanlarına eşler.

1. Azure Bilişsel Arama Dizin oluşturucuyu ifade etmek için az önce oluşturduğunuz (istek gövdesinde bir Dizin Oluşturucu tanımına sahip bir POST isteği) *Create Indexer* isteği gönderin. Bu adım, işlem hattını çağırarak Dizin oluşturucuyu nasıl çalıştıracağınızı açıklamaktadır.

1. Sonuçları değerlendirmek için sorguları çalıştırın ve becerileri, şema veya Dizin Oluşturucu yapılandırmasını güncelleştirmek için kodu değiştirin.

1. İşlem hattını yeniden oluşturmadan önce [Dizin oluşturucuyu sıfırlayın](search-howto-reindex.md) .

## <a name="next-steps"></a>Sonraki adımlar

+ [AI zenginleştirme belgeleri bağlantıları](cognitive-search-resources-documentation.md)
+ [Örnek: AI zenginleştirme için özel bir yetenek oluşturma (C#)](cognitive-search-create-custom-skill-example.md)
+ [Hızlı başlangıç: bir portalda AI zenginleştirme yapmayı deneyin](cognitive-search-quickstart-blob.md)
+ [Öğretici: AI zenginleştirme API 'Leri hakkında bilgi edinin](cognitive-search-tutorial-blob.md)
+ [Bilgi deposu (Önizleme)](knowledge-store-concept-intro.md)
+ [REST 'te bilgi deposu oluşturma](knowledge-store-create-rest.md)
+ [Sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md)
