---
title: AI zenginleştirmeye giriş
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinlerinde hem önceden tanımlanmış bilişsel becerilere hem de özel AI algoritmalarına sahip, içerik ayıklama, doğal dil işleme (NLP) ve görüntü işleme kullanılır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/24/2020
ms.openlocfilehash: a41dcc9c7ec86f41c64a69ea1aba762b960b2633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283030"
---
# <a name="getting-started-with-ai-enrichment"></a>AI zenginleştirme ile başlarken

AI zenginleştirme, görüntülerden, lekelerden ve diğer yapılandırılmamış veri kaynaklarından metin ayıklamak için kullanılan Azure Bilişsel Arama dizini özelliğidir. Zenginleştirme ve çıkarma, içeriğinizi bir [dizin](search-what-is-an-index.md) veya [bilgi deposunda](knowledge-store-concept-intro.md)daha aranabilir hale getirin. Çıkarma ve zenginleştirme, indeksleme boru hattına bağlı *bilişsel beceriler* kullanılarak uygulanır. Hizmete dahil bilişsel beceriler şu kategorilere girer: 

+ **Doğal dil işleme** becerileri [varlık tanıma,](cognitive-search-skill-entity-recognition.md) [dil algılama,](cognitive-search-skill-language-detection.md) [anahtar cümle çıkarma,](cognitive-search-skill-keyphrases.md)metin manipülasyonu, [duygu algılama](cognitive-search-skill-sentiment.md)ve kişisel [bilgi algılama](cognitive-search-skill-pii-detection.md)içerir. Bu becerilerle, yapılandırılmamış metin bir dizinde aranabilir ve filtrelenebilir alanlar olarak eşlenir.

+ **Görüntü işleme** becerileri [Optik Karakter Tanıma (OCR)](cognitive-search-skill-ocr.md) ve yüz algılama, görüntü yorumu, görüntü tanıma (ünlü insanlar ve yerler) veya görüntü yönlendirme gibi öznitelikleri gibi [görsel özelliklerin](cognitive-search-skill-image-analysis.md)belirlenmesi içerir. Bu beceriler, Azure Bilişsel Arama'nın sorgu özelliklerini kullanarak görüntü içeriğinin metin gösterimlerini oluşturur.

![Zenginleştirme boru hattı diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "zenginleştirme boru hattı genel bakış")

Azure Bilişsel Arama'daki bilişsel beceriler, Bilişsel Hizmetler API'lerinde önceden eğitilmiş makine öğrenimi modellerine dayanmaktadır: [Bilgisayarlı Vizyon](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) ve [Metin Analizi.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) 

Doğal dil ve görüntü işleme, veri alma aşamasında uygulanır ve sonuçlar Azure Bilişsel Arama'da aranabilir bir dizinde belgenin kompozisyonunun bir parçası haline gelir. Veriler bir Azure veri kümesi olarak gelir ve daha sonra ihtiyacınız olan [yerleşik becerileri](cognitive-search-predefined-skills.md) kullanarak bir dizin oluşturma ardışık kaynaktan itilir. Mimari genişletilebilir, bu nedenle yerleşik beceriler yeterli değilse, özel işlemeyi tümleştirmek için [özel beceriler](cognitive-search-create-custom-skill-example.md) oluşturabilir ve ekleyebilirsiniz. Örnekler, finans, bilimsel yayınlar veya tıp gibi belirli bir etki alanını hedefleyen özel bir varlık modülü veya belge sınıflandırıcı olabilir.

## <a name="when-to-use-ai-enrichment"></a>AI zenginleştirme ne zaman kullanılır

Ham içeriğiniz yapılandırılmamış metin, resim içeriği veya dil algılama ve çeviri gerektiren içerikse yerleşik bilişsel becerileri kullanmayı düşünmelisiniz. Yerleşik bilişsel beceriler aracılığıyla AI uygulamak, arama ve veri bilimi uygulamalarınızdaki değerini ve yararını artırarak bu içeriğin kilidini açabilir. 

Ayrıca, açık kaynak, üçüncü taraf veya ardışık ardışık bölüme entegre etmek istediğiniz birinci taraf kodunuz varsa özel bir beceri eklemeyi düşünebilirsiniz. Çeşitli belge türlerinin belirgin özelliklerini tanımlayan sınıflandırma modelleri bu kategoriye girer, ancak içeriğinize değer katan herhangi bir paket kullanılabilir.

### <a name="more-about-built-in-skills"></a>Yerleşik beceriler hakkında daha fazla bilgi

Yerleşik beceriler kullanılarak bir araya getirilen bir [beceri](cognitive-search-defining-skillset.md) aşağıdaki uygulama senaryoları için uygundur:

+ Tam metin aranabilir hale getirmek istediğiniz taranan belgeler (JPEG). JPEG dosyalarından metni tanımlamak, ayıklamak ve almak için optik karakter tanıma (OCR) becerisi ekleyebilirsiniz.

+ Birleşik görüntü ve metin içeren PDF'ler. PDF'lerde metin zenginleştirme adımları kullanılmadan dizinleme sırasında ayıklanabilir, ancak görüntü ve doğal dil işleme eklenmesi genellikle standart dizin oluşturma nın sağladığından daha iyi bir sonuç üretebilir.

+ Dil algılama ve muhtemelen metin çevirisi uygulamak istediğiniz çok dilli içerik.

+ Daha büyük belgede gizli olan doğal anlamı veya bağlamı içeren içerik içeren yapılandırılmamış veya yarı yapılandırılmış belgeler. 

  Özellikle blobs genellikle tekli bir "alan" içine paketlenmiş içerik büyük bir gövde içerir. Görüntü ve doğal dil işleme becerilerini bir dizin oluşturucuya ekleyerek, ham içerikte bulunan ancak farklı alanlar olarak su yüzüne çıkmayan yeni bilgiler oluşturabilirsiniz. Bazı kullanıma hazır yerleşik bilişsel beceriler yardımcı olabilir: anahtar ifade çıkarma, duygu analizi ve varlık tanıma (insanlar, kuruluşlar ve konumlar).

  Ayrıca, yerleşik beceriler metin bölme, birleştirme ve şekil işlemleri yoluyla içeriği yeniden yapılandırmada da kullanılabilir.

### <a name="more-about-custom-skills"></a>Özel beceriler hakkında daha fazla bilgi

Özel beceriler, özel [beceri web arabirimini](cognitive-search-custom-skill-interface.md)sağladığınız ve kaydırdığınız bir modeli kullanarak formları tanıma veya özel varlık algılama gibi daha karmaşık senaryoları destekleyebilir. Özel becerilere örnek olarak [Formlar Tanıyıcı,](/azure/cognitive-services/form-recognizer/overview) [Bing Varlık Arama API'sinin](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example)entegrasyonu ve [özel varlık tanıma](https://github.com/Microsoft/SkillsExtractorCognitiveSearch)sayılabilir.


## <a name="steps-in-an-enrichment-pipeline"></a>Zenginleştirme boru hattındaki adımlar

Bir zenginleştirme boru hattı [*dizinleyiciler*](search-indexer-overview.md)dayanmaktadır. Dizin leyiciler, belge kırılması için dizin ve veri kaynağınız arasındaki alana alan eşlemelerini temel alanbir dizini doldurur. Artık dizin leyicilere bağlı olan beceriler, tanımladığınız becerilere göre belgeleri yakalar ve zenginleştirir. Dizine ekildikten sonra, [Azure Bilişsel Arama tarafından desteklenen](search-query-overview.md)tüm sorgu türleri aracılığıyla içeriğe arama istekleri yoluyla erişebilirsiniz.  Dizinleyiciler için yeniyseniz, bu bölüm size adımları ile yürür.

### <a name="step-1-connection-and-document-cracking-phase"></a>Adım 1: Bağlantı ve belge kırma aşaması

Ardışık birimin başlangıcında, yapılandırılmamış metin veya metin dışı içeriğe (resimler, taranan belgeler veya JPEG dosyaları gibi) sahipsiniz. Veriler, bir dizin leyici tarafından erişilebilen bir Azure veri depolama hizmetinde bulunmalıdır. Dizin leyiciler kaynak verilerden metin ayıklamak için kaynak belgeleri "kırabilir". Belge kırma, dizin oluşturma sırasında metin olmayan kaynaklardan metin içeriği ayıklama veya oluşturma işlemidir.

![Belge kırma aşaması](./media/cognitive-search-intro/document-cracking-phase-blowup.png "belge çatlama")

 Desteklenen kaynaklar arasında Azure tablo depolama, Azure tablo depolama, Azure SQL Veritabanı ve Azure Cosmos DB yer almaktadır. Metin tabanlı içerik aşağıdaki dosya türlerinden ayıklanabilir: PDF' ler, Word, PowerPoint, CSV dosyaları. Tam liste için [Desteklenen biçimlere](search-howto-indexing-azure-blob-storage.md#supported-document-formats)bakın. Dizin oluşturma, küçük, temsili bir veri kümesiyle başlamak ve ardından çözümünüz olgunlaştıkça artımlı olarak oluşturmak için zaman alır.

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Adım 2: Bilişsel beceriler ve zenginleştirme aşaması

Zenginleştirme atomik işlemleri gerçekleştiren *bilişsel beceriler* ile gerçekleştirilir. Örneğin, bir PDF kırdıktan sonra, dizininizde kaynağınızda yerel olarak bulunmayan yeni alanlar oluşturmak için varlık tanıma, dil algılama veya anahtar sözcük çıkarma uygulayabilirsiniz. Tamamen, boru hattı kullanılan becerilerin toplama bir *skillset*denir.  

![Zenginleştirme aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "zenginleştirme aşaması")

Bir skillset yerleşik [bilişsel beceriler](cognitive-search-predefined-skills.md) veya sağlamak ve skillset bağlanmak özel [becerileri](cognitive-search-create-custom-skill-example.md) dayanmaktadır. Bir skillset en az veya son derece karmaşık olabilir ve yalnızca işleme türünü değil, aynı zamanda işlem sırasını da belirler. Bir skillset artı bir dizinleyicinin parçası olarak tanımlanan alan eşlemeleri zenginleştirme ardışık ardışık lığı tam olarak belirtir. Tüm bu parçaları bir araya çekme hakkında daha fazla bilgi için [bkz.](cognitive-search-defining-skillset.md)

Dahili olarak, boru hattı zenginleştirilmiş belgelerin bir koleksiyon oluşturur. Zenginleştirilmiş belgelerin hangi bölümlerinin arama dizininizdeki dizine eşlenecek şekilde eşlendirilmesi gerektiğine karar verebilirsiniz. Örneğin, anahtar tümcecik ayıklama ve varlık tanıma becerilerini uyguladıysanız, bu yeni alanlar zenginleştirilmiş belgenin bir parçası olur ve dizininizdeki alanlara eşlenebilir. Giriş/çıktı oluşumları hakkında daha fazla bilgi edinmek için [Ek Açıklamalar'a](cognitive-search-concept-annotations-syntax.md) bakın.

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Zenginleştirmelerden tasarruf etmek için bir knowledgeStore öğesi ekleyin

[Search REST api-version=2019-05-06-Preview,](search-api-preview.md) azure `knowledgeStore` depolama bağlantısı sağlayan bir tanım ve zenginleştirmelerin nasıl depolanır olduğunu açıklayan projeksiyonlarla birlikte beceri kümelerini genişletir. Bu, dizininize ek olarak. Standart bir AI ardışık boru hattında, zenginleştirilmiş belgeler geçicidir, yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile zenginleştirilmiş belgeler korunur. Daha fazla bilgi için [Bilgi deposuna (önizleme)](knowledge-store-concept-intro.md)bakın.

### <a name="step-3-search-index-and-query-based-access"></a>Adım 3: Arama dizini ve sorgu tabanlı erişim

İşlemtamamlandığında, Azure Bilişsel Arama'da tamamen metin araması yapılan zenginleştirilmiş belgelerden oluşan bir arama dizininizin var. [Dizin sorgulanması, geliştiricilerin](search-query-overview.md) ve kullanıcıların ardışık hatlar tarafından oluşturulan zenginleştirilmiş içeriğe nasıl eriştİğİdir. 

![Arama simgesiyle dizin](./media/cognitive-search-intro/search-phase-blowup.png "Arama simgesiyle dizin")

Dizin, Azure Bilişsel Arama için oluşturabileceğiniz diğer dizinlere benzer: özel çözümleyicilerle tamamlayabilir, bulanık arama sorgularını çağırabilir, filtrelenmiş arama ekleyebilir veya arama sonuçlarını yeniden şekillendirmek için puanlama profilleriyle denemeler yapabilirsiniz.

Dizinler, puanlama profilleri ve eşanlamlı haritalar gibi belirli bir dizine bağlı alanları, öznitelikleri ve diğer yapıları tanımlayan bir dizin şemasından oluşturulur. Bir dizin tanımlandıktan ve doldurultuktan sonra, yeni ve güncelleştirilmiş kaynak belgeleri almak için artımlı olarak dizine dizine bilirsiniz. Bazı değişiklikler tam bir yeniden gerektirir. Şema tasarımı kararlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

**Denetim Listesi: Tipik bir iş akışı**

1. Azure kaynak verilerinizi temsili bir örneğe batırın. Dizin oluşturma, küçük, temsili bir veri kümesiyle başlamak ve ardından çözümünüz olgunlaştıkça artımlı olarak oluşturmak için zaman alır.

1. Veri alma için bir bağlantı dizesi sağlamak için Azure Bilişsel Arama'da bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) oluşturun.

1. Zenginleştirme adımları ile bir [beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) oluşturun.

1. [Dizin şema](https://docs.microsoft.com/rest/api/searchservice/create-index)sını tanımlayın. *Alanlar* koleksiyonu kaynak verilerden alanları içerir. Ayrıca, zenginleştirme sırasında oluşturulan içerik için oluşturulan değerleri tutmak için ek alanlar oluşturmanız gerekir.

1. Veri kaynağı, skillset ve dizin başvuran [dizin tanımlayın.](https://docs.microsoft.com/rest/api/searchservice/create-skillset)

1. Dizinleyici içinde, *outputFieldMappings*ekleyin. Bu bölüm, skillset'ten (adım 3)'tan, indeks şemasındaki (adım 4'teki) giriş alanlarına çıktıları eşler.

1. Azure Bilişsel *Arama'daki* dizin oluşturucuyu ifade etmek için oluşturduğunuz Dizin Oluştur isteğini gönderin (istek gövdesinde dizin oluşturma tanımıbulunan bir POST isteği). Bu adım, dizini çalıştırarak ardışık hattı nasıl çalıştırdığınızdır.

1. Sonuçları değerlendirmek ve beceri kümelerini, şemayı veya dizin leyici yapılandırmasını güncelleştirmek için kodu değiştirmek için sorguları çalıştırın.

1. Ardışık lığı yeniden oluşturmadan önce [dizinleyiciyi sıfırla.](search-howto-reindex.md)

## <a name="next-steps"></a>Sonraki adımlar

+ [AI zenginleştirme dokümantasyon bağlantıları](cognitive-search-resources-documentation.md)
+ [Örnek: AI zenginleştirme için özel bir beceri oluşturma (C#)](cognitive-search-create-custom-skill-example.md)
+ [Quickstart: Bir portal walk-through AI zenginleştirme deneyin](cognitive-search-quickstart-blob.md)
+ [Öğretici: AI zenginleştirme API'leri hakkında bilgi edinin](cognitive-search-tutorial-blob.md)
+ [Bilgi deposu (önizleme)](knowledge-store-concept-intro.md)
+ [REST'te bir bilgi deposu oluşturun](knowledge-store-create-rest.md)
+ [Sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md)
