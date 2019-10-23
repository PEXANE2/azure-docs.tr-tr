---
title: Bilişsel arama ve AI zenginleştirme-Azure Search 'ye giriş
description: Bilişsel beceriler ve AI algoritmaları kullanarak Azure Search dizin oluşturma bölümünde aranabilir içerik oluşturmak için içerik ayıklama, doğal dil işleme (NLP) ve görüntü işleme.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.topic: overview
ms.date: 08/15/2019
ms.author: heidist
ms.openlocfilehash: aa8ab3a3bc2f243e8b0ee404228926ef46bc46db
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71266384"
---
# <a name="what-is-cognitive-search-in-azure-search"></a>Azure Search 'de "bilişsel arama" nedir?

Bilişsel arama, görüntü, Bloblar ve diğer yapılandırılmamış veri kaynaklarından metin ayıklamak için kullanılan Azure Search bir AI özelliğidir. içeriği bir Azure Search dizininde daha fazla aranabilir hale getirmek üzere zenginleştirir. Ayıklama ve zenginleştirme, bir dizin oluşturma işlem hattına eklenen bilişsel *yetenekler* aracılığıyla uygulanır. Aşağıdaki yollarla AI zenginleştirme desteklenir: 

+ **Doğal dil işleme** becerileri [varlık tanıma](cognitive-search-skill-entity-recognition.md), [dil algılama](cognitive-search-skill-language-detection.md), [anahtar tümceciği ayıklama](cognitive-search-skill-keyphrases.md), metin işleme ve yaklaşım [algılamayı](cognitive-search-skill-sentiment.md)içerir. Bu becerilerle, yapılandırılmamış metin, bir dizinde aranabilir ve filtrelenebilir alanlar olarak eşlenmiş yeni formları kabul edebilir.

+ **Görüntü işleme** becerileri, [optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) ve yüz algılama, görüntü yorumu, görüntü tanıma (sık sık insanlar ve yer işaretleri) veya gibi özellikler gibi [Görsel özelliklerin](cognitive-search-skill-image-analysis.md)tanımlanmasını içerir. renkler veya resim yönü. Azure Search tüm sorgu yeteneklerini kullanarak aranabilir görüntü içeriğinin metin sunumlarını oluşturabilirsiniz.

![Bilişsel arama ardışık düzen diyagramı](./media/cognitive-search-intro/cogsearch-architecture.png "Bilişsel arama ardışık düzenine genel bakış")

Azure Search bilişsel yetenekler, Bilişsel Hizmetler API'si: [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)için makine öğrenimi modellerini temel alır. 

Doğal dil ve görüntü işleme, veri alma aşamasında uygulanır, sonuçlar, Azure Search aranabilir dizindeki bir belgenin kompozisyonunun bir parçası haline geliyor. Verilerin bir Azure veri kümesi olarak kaynağı oluşturulur ve ardından ihtiyacınız olan [yerleşik becerileri](cognitive-search-predefined-skills.md) kullanarak bir dizin oluşturma işlem hattı üzerinden gönderilir. Mimaride, yerleşik yetenekler yeterli değilse, özel işlemeyi bütünleştirmek için [özel yetenekler](cognitive-search-create-custom-skill-example.md) oluşturup iliştirebilirsiniz. Örnekler, finans, bilimsel yayınlar veya ilaç gibi belirli bir etki alanını hedefleyen özel bir varlık modülü veya belge Sınıflandırıcısı olabilir.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.

## <a name="when-to-use-cognitive-search"></a>Bilişsel arama ne zaman kullanılır?

Önceden oluşturulmuş yetenekler kullanılarak bilişsel arama, aşağıdaki uygulama senaryolarında idealdir:

+ Tam metin araması yapmak istediğiniz taranmış belgeler (JPEG). JPEG dosyalarından metin tanımlamak, ayıklamak ve almak için bir optik karakter tanıma (OCR) yeteneği ekleyebilirsiniz.

+ Birleştirilmiş görüntü ve metin içeren PDF 'Ler. PDF 'Lerdeki metin, bilişsel arama kullanılmadan Azure Search dizin oluşturma sırasında ayıklanabilir, ancak görüntü ve doğal dil işlemenin eklenmesi, genellikle standart bir dizin oluşturma işleminin sağladığı daha iyi bir sonuç oluşturabilir.

+ Dil algılamayı ve muhtemelen metin çevirisini uygulamak istediğiniz çok dilli içerik.

+ Büyük bir belgede gizli bir anlamı veya bağlamı olan içerik içeren yapılandırılmamış veya yarı yapılandırılmış belgeler. 

  Genellikle, tek bir "alan" ile paketlenmiş büyük bir içerik gövdesini içeren bloblardır. Bir dizin oluşturucuya görüntü ve doğal dil işleme becerileri ekleyerek, ham içerikte yer alan yeni bilgiler oluşturabilir, aksi takdirde ayrı alanlar olarak ortaya çıkmış olursunuz. Kullanıma hazır kullanıma hazır yerleşik bilişsel yetenekler: anahtar tümceciği ayıklama, yaklaşım Analizi ve varlık tanıma (kişiler, kuruluşlar ve konumlar).

  Ayrıca, önceden oluşturulmuş yetenekler metin bölme, birleştirme ve şekil işlemleri aracılığıyla içeriği yeniden yapılandırmak için de kullanılabilir.

Özel yetenekler, formları tanıma gibi daha karmaşık senaryoları veya özel [Beceri Web arabirimine](cognitive-search-custom-skill-interface.md)sağladığınız ve kaydırabileceğiniz bir modeli kullanarak özel varlık algılamayı destekleyebilir. Özel yeteneklerin çeşitli örnekleri, [form tanıyıcı](/azure/cognitive-services/form-recognizer/overview), [Bing varlık arama API'si](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example)tümleştirmesi ve [özel varlık tanıma](https://github.com/Microsoft/SkillsExtractorCognitiveSearch)içerir.


## <a name="component-pipeline-of-cognitive-search"></a>Bilişsel arama bileşen işlem hattı

Bilişsel arama işlem hattı, veri kaynaklarını gezme ve uçtan uca Dizin işleme sağlayan [Azure Search *dizin oluşturucularının* ](search-indexer-overview.md) temel alır. Yetenekler artık, tanımladığınız beceri göre belgeleri kesintiye uğratan ve zenginleştirmektedir. Dizin oluşturulduktan sonra, [Azure Search tarafından desteklenen tüm sorgu türleri](search-query-overview.md)aracılığıyla arama istekleri aracılığıyla içeriğe erişebilirsiniz.  Dizin oluşturuculardan yeni olduğunuzda, bu bölüm adımlarda size yol gösterir.

### <a name="step-1-connection-and-document-cracking-phase"></a>1\. Adım: bağlantı ve belge çözme aşaması

İşlem hattının başlangıcında, yapılandırılmamış metin veya metin olmayan içerik (örneğin, görüntü ve taranmış belge JPEG dosyaları) vardır. Verilerin bir Dizin Oluşturucu tarafından erişilebilen bir Azure veri depolama hizmetinde bulunması gerekir. Dizin oluşturucular, kaynak verilerden metin ayıklamak için kaynak belgeleri "çözebilir".

![Belge çözme aşaması](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Belge çözme")

 Desteklenen kaynaklar Azure Blob depolama, Azure Tablo depolama, Azure SQL veritabanı ve Azure Cosmos DB içerir. Metin tabanlı içerik şu dosya türlerinden ayıklanabilir: PDF 'Ler, Word, PowerPoint, CSV dosyaları. Tam liste için bkz. [desteklenen biçimler](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>2\. Adım: bilişsel beceriler ve zenginleştirme aşaması

Zenginleştirme, Atomik işlemler gerçekleştirirken bilişsel *yetenekler* aracılığıyla yapılır. Örneğin, bir PDF 'den metin içeriğine sahip olduktan sonra, dizinde yerel olarak kullanılamayan yeni alanlar oluşturmak için varlık tanıma dil algılamayı veya anahtar tümceciği ayıklama uygulayabilirsiniz. Tamamen, işlem hattınızda kullanılan yeteneklerin koleksiyonuna bir *beceri*denir.  

![Zenginleştirme aşaması](./media/cognitive-search-intro/enrichment-phase-blowup.png "zenginleştirme aşaması")

Bir beceri, sağladığınız [önceden tanımlanmış](cognitive-search-predefined-skills.md) bilişsel becerileri veya [özel becerileri](cognitive-search-create-custom-skill-example.md) temel alır ve beceri 'e bağlanırsınız. Bir beceri, en düşük veya çok karmaşık olabilir ve yalnızca işleme türü değil, işlem sırasını da belirler. Bir beceri ve bir dizin oluşturucunun parçası olarak tanımlanan alan eşlemeleri, zenginleştirme işlem hattını tam olarak belirtir. Tüm bu parçaları birlikte çekme hakkında daha fazla bilgi için bkz. [beceri tanımlama](cognitive-search-defining-skillset.md).

Dahili olarak, işlem hattı zenginleştirilmiş bir belge koleksiyonu oluşturur. Zenginleştirilmiş belgelerin hangi bölümlerinin arama dizininizdeki dizinlenebilir alanlarla eşlenmeli olduğuna karar verebilirsiniz. Örneğin, anahtar tümceleri ayıklama ve varlık tanıma becerileri uyguladıysanız, bu yeni alanlar zenginleştirilmiş belgenin bir parçası haline gelir ve dizininizdeki alanlarla eşleştirilebilir. Giriş/çıkış formaları hakkında daha fazla bilgi için bkz. [ek açıklamalar](cognitive-search-concept-annotations-syntax.md) .

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Zenginleştirmelerin kaydedileceği bir knowledgeStore öğesi ekleme

[Arama hizmeti REST api-Version = 2019-05-06](search-api-preview.md) becerileri, Azure depolama bağlantısı sağlayan bir knowledgeStore tanımıyla ve zenginleştirmeleri 'in nasıl depolandığını betimleyen projeksiyonları genişletir. 

Bir beceri bilgi deposu eklemek, tam metin arama dışındaki senaryolar için kendi zenginleştirmelerinin bir temsilini proje olanağı sağlar. Daha fazla bilgi için bkz. [bilgi deposu nedir](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>3\. Adım: arama dizini ve sorgu tabanlı erişim

İşlem tamamlandığında, Azure Search ' de tam metin araması yapılan, zenginleştirilmiş belgelerden oluşan bir arama dizinine sahip olursunuz. [Dizin sorgulama](search-query-overview.md) , geliştiricilerin ve kullanıcıların işlem hattı tarafından oluşturulan zenginleştirilmiş içeriğe nasıl erişmesinden oluşur. 

![Arama simgesiyle Dizin](./media/cognitive-search-intro/search-phase-blowup.png "Arama simgesiyle Dizin")

Dizin, Azure Search için oluşturabileceğiniz herhangi bir diğer durum olabilir: özel çözümleyiciler ile destekleyebilirsiniz, benzer arama sorgularını çağırabilir, filtrelenmiş arama ekleyebilir veya arama sonuçlarını yeniden şekillendirmek için Puanlama profilleriyle denemeler yapabilirsiniz.

Dizinler, Puanlama profilleri ve eş anlamlı haritalar gibi belirli bir dizine iliştirilmiş alanları, öznitelikleri ve diğer yapıları tanımlayan bir dizin şemasından oluşturulur. Bir dizin tanımlandıktan ve doldurulduktan sonra, yeni ve güncelleştirilmiş kaynak belgeleri almak için artımlı olarak dizin oluşturabilirsiniz. Belirli değişiklikler tam yeniden oluşturma gerektirir. Şema tasarımı kararlı olana kadar küçük bir veri kümesi kullanmanız gerekir. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Önemli özellikler ve kavramlar

| Kavram | Açıklama| Bağlantılar |
|---------|------------|-------|
| Beceri | Beceri koleksiyonu içeren bir üst düzey adlandırılmış kaynak. Bir beceri, enzenginleştirme ardışık düzeni. Dizin Oluşturucu tarafından dizin oluşturma sırasında çağrılır. | [Beceri tanımlama](cognitive-search-defining-skillset.md) |
| Bilişsel Beceri | Zenginleştirme ardışık düzeninde atomik bir dönüşüm. Genellikle, yapısını çıkaran veya veren ve bu nedenle giriş verilerini kavramak için bir bileşendir. Neredeyse her zaman, çıktı metin tabanlıdır ve işleme, görüntü girişlerinden metin çıkaran veya üreten doğal dil işleme veya görüntü işleme olur. Bir beceriye ait çıkış, dizindeki bir alanla eşleştirilebilir veya bir aşağı akış zenginleştirme için giriş olarak kullanılabilir. Bir yetenek önceden tanımlanmış ve Microsoft tarafından sağlanıyor ya da özel: sizin tarafınızdan oluşturulup dağıtılır. | [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md) |
| Veri ayıklama | Geniş kapsamlı bir işleme sahiptir, ancak bilişsel arama ile ilgili olarak, varlık tanıma yeteneği genellikle bu bilgileri yerel olarak sağlamayan bir kaynaktaki verileri (varlık) ayıklamak için kullanılır. | [Varlık tanıma yeteneği](cognitive-search-skill-entity-recognition.md)| 
| Görüntü işleme | Bir yer işareti tanıma veya bir görüntüden metin çıkaran gibi bir görüntüden metin alır. Yaygın örneklerde, taranmış bir belge (JPEG) dosyasından karakter kaldırma için OCR veya sokak işareti içeren bir fotoğrafta cadde adı tanıma gösterilebilir. | [Görüntü analizi yeteneği](cognitive-search-skill-image-analysis.md) veya [OCR yeteneği](cognitive-search-skill-ocr.md)
| Doğal dil işleme | Öngörüler ve metin girişleri hakkındaki bilgiler için metin işleme. Dil algılama, yaklaşım Analizi ve anahtar tümceciği ayıklama, doğal dil işleme altına giren yeteneklerdir.  | [Anahtar ifade ayıklama yetenek](cognitive-search-skill-keyphrases.md), [dil algılama beceri](cognitive-search-skill-language-detection.md) [yaklaşım Analizi beceri](cognitive-search-skill-sentiment.md) |
| Belge çözme | Dizin oluşturma sırasında metin olmayan kaynaklardaki metin içeriğini ayıklama veya oluşturma işlemi. Optik karakter tanıma (OCR) bir örnektir, ancak genellikle Dizin Oluşturucu uygulama dosyalarından içerik çıkaran için temel Dizin Oluşturucu işlevselliğine başvurur. Kaynak dosya konumu sağlayan veri kaynağı ve alan eşlemeleri sağlayan Dizin Oluşturucu tanımı, belge çözme içindeki anahtar faktörlerdir. | Bkz. [Dizin oluşturucular](search-indexer-overview.md) |
| Biçimlendirmesini | Metin parçalarını daha büyük bir yapıda birleştirin veya daha büyük bir şekilde daha fazla aşağı akış işleme için daha büyük metin öbeklerini yönetilebilir bir boyuta bölün. | [Beceri başına mil](cognitive-search-skill-shaper.md), [metin merkli beceri](cognitive-search-skill-textmerger.md), [metin bölünmüş beceri](cognitive-search-skill-textsplit.md) |
| Zenginleştirilmiş belgeler | İşlem sırasında oluşturulan, son çıktı bir arama dizinine yansıtılmış bir geçişli iç yapı. Bir beceri, hangi enzenginlerin gerçekleştirileceğini belirler. Alan eşlemeleri dizine hangi veri öğelerinin ekleneceğini belirleme. İsteğe bağlı olarak, Depolama Gezgini, Power BI veya Azure Blob depolama alanına bağlanan herhangi bir araçla ilgili araçları kullanarak zenginleştirilmiş belgeleri kalıcı hale getirmek ve incelemek için bir bilgi deposu oluşturabilirsiniz. | Bkz. [bilgi deposu (Önizleme)](knowledge-store-concept-intro.md). |
| Dizinleyici |  Bir dış veri kaynağından aranabilir verileri ve meta verileri çıkaran ve belge çözme için dizin ve veri kaynağınız arasındaki alan eşlemelerine göre bir dizini dolduran bir gezgin. Bilişsel arama zenginleştirmelerinde, Dizin Oluşturucu bir beceri çağırır ve zenginleştirme çıktısını dizindeki hedef alanlarla ilişkilendirirken alan eşlemelerini içerir. Dizin Oluşturucu tanımı, işlem hattı işlemlerine yönelik tüm yönergeleri ve başvuruları içerir ve Dizin oluşturucuyu çalıştırdığınızda işlem hattı çağrılır. | [Dizin Oluşturucular](search-indexer-overview.md) |
| Veri kaynağı  | Azure üzerinde desteklenen türlerin dış veri kaynağına bağlanmak için bir Dizin Oluşturucu tarafından kullanılan nesne. | Bkz. [Dizin oluşturucular](search-indexer-overview.md) |
| Dizin | Alan yapısını ve kullanımını tanımlayan bir dizin şemasından oluşturulan Azure Search kalıcı arama dizini. | [Azure Search dizinler](search-what-is-an-index.md) | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. Adım: [Azure Search kaynak oluşturma](search-create-service-portal.md)** 

**2. Adım: bazı hızlı başlangıçlara ve uygulamalı deneyim için örneklere deneme yapın**

+ [Hızlı Başlangıç (portal)](cognitive-search-quickstart-blob.md)
+ [Öğretici (HTTP istekleri)](cognitive-search-tutorial-blob.md)
+ [Örnek: bilişsel arama (C#) için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)

Ücretsiz hizmetin öğrenme amacıyla önerilmesini öneririz, ancak ücretsiz işlem sayısı günde 20 belge ile sınırlıdır. Hem hızlı başlangıç hem de Öğreticiyi bir gün içinde çalıştırmak için, her iki alıştırmada de sığdırabilmeniz veya hızlı başlangıçta veya öğreticide kullandığınız dizin oluşturucuyu silebilmeniz için daha küçük bir dosya kümesi (10 belge) kullanın.

**3. Adım: API 'YI gözden geçirme**

REST `api-version=2019-05-06` istekleri veya .NET SDK 'sını kullanabilirsiniz. 

Bu adım bilişsel arama çözümü oluşturmak için REST API 'Lerini kullanır. Bilişsel arama için yalnızca iki API eklenir veya genişletilir. Diğer API 'Ler, genel olarak kullanılabilen sürümlerle aynı söz dizimine sahiptir.

| REST API | Açıklama |
|-----|-------------|
| [Veri Kaynağı Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeler oluşturmak için kullanılan kaynak verileri sağlayan bir dış veri kaynağını tanımlayan kaynak.  |
| [Beceri oluşturma (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Bu API, bilişsel aramaya özeldir. Dizin oluşturma sırasında bir zenginleştirme ardışık düzeninde kullanılan [önceden tanımlanmış yeteneklerin](cognitive-search-predefined-skills.md) ve özel bilişsel [yeteneklerin](cognitive-search-custom-skill-interface.md) kullanımını koordine eden bir kaynaktır. |
| [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizinini ifade eden şema. Dizindeki alanlar, kaynak verilerdeki alanlara veya zenginleştirme aşamasında üretilen alanlara (örneğin, varlık tanıma tarafından oluşturulan kuruluş adları için bir alan) eşlenir. |
| [Dizin Oluşturucu oluştur (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Dizin oluşturma sırasında kullanılan bileşenleri tanımlayan kaynak: veri kaynağı, Beceri, kaynak ve ara veri yapılarından hedef dizine ve dizinin kendisi de dahil olmak üzere. Dizin oluşturucuyu çalıştırmak, veri alımı ve zenginleştirme için tetikleyiciydi. Çıktı, kaynak verilerle doldurulan, becerileri aracılığıyla zenginleştirilmiş, Dizin şemasını temel alan bir arama dizinidir. Bu var olan API, Beceri özelliği dahil olmak üzere bilişsel arama senaryolarında genişletilir. |

**Denetim listesi: tipik bir iş akışı**

1. Azure Kaynak verilerinizi bir temsili örneğine alt küme haline koyun. Dizin oluşturma zaman alır, bu nedenle küçük, temsili bir veri kümesiyle başlar ve sonra çözüm malarınız kadar artımlı olarak oluşturun.

1. Veri alımı için bağlantı dizesi sağlamak üzere Azure Search ' de bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) oluşturun.

1. Zenginleştirme adımları ile bir [beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) oluşturun.

1. [Dizin şemasını](https://docs.microsoft.com/rest/api/searchservice/create-index)tanımlayın. *Alanlar* koleksiyonu, kaynak verilerden alanlar içerir. Ayrıca, zenginleştirme sırasında oluşturulan içerikler için oluşturulan değerleri tutmak üzere ek alanlar da saplama yapmanız gerekir.

1. Veri kaynağına, beceri ve dizine başvuran [Dizin oluşturucuyu](https://docs.microsoft.com/rest/api/searchservice/create-skillset) tanımlayın.

1. Dizin Oluşturucu içinde *Outputfieldmappings*ekleyin. Bu bölüm, Beceri (adım 3 ' te) çıktısını Dizin şemasındaki (adım 4 ' te) giriş alanlarına eşler.

1. Azure Search Dizin oluşturucuyu ifade etmek için az önce oluşturduğunuz (istek gövdesinde bir Dizin Oluşturucu tanımı olan bir POST isteği) *oluşturma Dizin* Oluşturucu isteği gönderin. Bu adım, işlem hattını çağırarak Dizin oluşturucuyu nasıl çalıştıracağınızı açıklamaktadır.

1. Sonuçları değerlendirmek için sorguları çalıştırın ve becerileri, şema veya Dizin Oluşturucu yapılandırmasını güncelleştirmek için kodu değiştirin.

1. İşlem hattını yeniden oluşturmadan önce [Dizin oluşturucuyu sıfırlayın](search-howto-reindex.md) .

Belirli sorular veya sorunlar hakkında daha fazla bilgi için bkz. [sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

+ [Bilişsel arama belgeleri](cognitive-search-resources-documentation.md)
+ [Hızlı başlangıç: bir portal 'da bilişsel aramayı deneme](cognitive-search-quickstart-blob.md)
+ [Öğretici: bilişsel arama API 'Lerini öğrenme](cognitive-search-tutorial-blob.md)
+ [Bilgi deposuna genel bakış](knowledge-store-concept-intro.md)
+ [Bilgi deposu izlenecek yol](knowledge-store-howto.md)
