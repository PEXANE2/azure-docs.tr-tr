---
title: Blob depolama verilerini anlamak için AI'yi kullanma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da bir AI zenginleştirme ardışık hattını kullanarak Azure bloblarına anlamsal, doğal dil işleme ve görüntü çözümlemesi ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 591437eb3951164d53388b6164103948e9ad65e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73496429"
---
# <a name="use-ai-to-understand-blob-storage-data"></a>Blob depolama verilerini anlamak için AI'yi kullanma

Azure Blob depolama sındaki veriler genellikle resimler, uzun metin, PDF'ler ve Office belgeleri gibi çeşitli yapılandırılmamış içeriktir. Azure Bilişsel Arama'daki AI özelliklerini kullanarak, lekelerden değerli bilgileri çeşitli şekillerde anlayabilir ve ayıklayabilirsiniz. Blob içeriğine AI uygulama örnekleri şunlardır:

+ Optik karakter tanıma (OCR) kullanarak görüntülerden metin ayıklama
+ Bir fotoğraftan sahne açıklaması veya etiket oluşturma
+ Dili algılama ve metni farklı dillere çevirme
+ Kişilere, tarihlere, yerlere veya kuruluşlara referansbulmak için adlandırılmış varlık tanıma (NER) içeren metni işleme 

Bu AI özelliklerinden yalnızca birine ihtiyacınız olsa da, birden birini aynı ardışık ardışık ardışık alanda birleştirmek yaygındır (örneğin, taranmış bir görüntüden metin ayıklamak ve sonra da içinde başvurulan tüm tarihleri ve yerleri bulmak). 

AI zenginleştirme, metin olarak yakalanan, alanlarda depolanan yeni bilgiler oluşturur. Zenginleştirme sonrası, bu bilgilere tam metin arama yoluyla bir arama dizininden erişebilir veya zenginleştirilmiş belgeleri, keşif veya analiz senaryoları için verileri keşfetmeyi de içeren yeni uygulama deneyimlerine güç sağlamak için Azure depolamaalanına geri gönderebilirsiniz. 

Bu makalede, ai zenginleştirmesini geniş bir lens aracılığıyla görüntüleyebiliriz, böylece blobs'taki ham verileri dönüştürmekten, arama dizininde veya bilgi deposunda sorgulanabilir bilgilere kadar tüm süreci hızlı bir şekilde kavrayabilirsiniz.

## <a name="what-it-means-to-enrich-blob-data-with-ai"></a>AI ile blob verilerini "zenginleştirmek" ne anlama geliyor

*AI zenginleştirme,* Azure Bilişsel Arama'nın Microsoft'tan yerleşik AI'yi veya sağladığınız özel AI'yi entegre eden dizin oluşturma mimarisinin bir parçasıdır. Blobs (geldikleri veya güncelleştirildikçe hem mevcut olanlar hem de yenileri) işlemeniz gereken uçlardan uca senaryoları uygulamanıza), görüntü ve metin ayıklamak için tüm dosya biçimlerini açmanıza, çeşitli AI özelliklerini kullanarak istenen bilgileri ayıklamanıza ve hızlı arama, alma ve keşif için bir arama dizininde dizin. 

Girişler, Azure Blob depolama alanında tek bir kapta lekelerinizledir. Blobs metin veya görüntü veri hemen hemen her türlü olabilir. 

Çıktı her zaman hızlı metin arama, alma ve istemci uygulamalarında arama için kullanılan bir arama dizinidir. Ayrıca, çıktı, Power BI veya veri bilimi iş yükleri gibi araçlarda aşağı akış çözümlemesi için zenginleştirilmiş belgeleri Azure blobs veya Azure tablolarına dönüştüren bir *bilgi deposu* da olabilir.

Arada boru hattı mimarisinin kendisidir. Boru hattı, AI sağlayan bir veya daha fazla *beceriden* oluşan bir *skillset*atayabileceğiniz *dizinleyici* özelliğine dayanır. Boru hattının amacı, ham içerik olarak giren ancak boru hattından geçerken ek yapı, bağlam ve bilgi alan *zenginleştirilmiş belgeler* üretmektir. Zenginleştirilmiş belgeler, tam metin arama veya arama ve analitik kullanılan ters dizinler ve diğer yapılar oluşturmak için dizinleme sırasında tüketilir.

## <a name="start-with-services"></a>Hizmetlerle başlayın

Azure Bilişsel Arama ve Azure Blob depolama alanına ihtiyacınız var. Blob depolama alanı içinde, kaynak içeriği sağlayan bir kapsayıcı gerekir.

Doğrudan Depolama hesabı portalı sayfanızda başlayabilirsiniz. Sol daki gezinti sayfasında, **Blob hizmetinin** altında yeni bir hizmet oluşturmak veya varolan bir hizmet seçmek için **Azure Bilişsel Arama** Ekle'yi tıklatın. 

Depolama hesabınıza Azure Bilişsel Arama ekledikten sonra, herhangi bir Azure veri kaynağındaki verileri zenginleştirmek için standart işlemi izleyebilirsiniz. AI zenginleştirme kolay bir ilk giriş için Azure Bilişsel Arama veri **sihirbazı** öneririz. Bu hızlı başlangıç size adımlardan geçer: [Portalda bir AI zenginleştirme boru hattı oluşturun.](cognitive-search-quickstart-blob.md) 

Aşağıdaki bölümlerde, daha fazla bileşen ve kavram inceleyeceğiz.

## <a name="use-a-blob-indexer"></a>Blob dizinleyici sindin

AI zenginleştirme bir dizin oluşturma ardışık bir eklentidir ve Azure Bilişsel Arama'da bu boru hatları bir *dizin oluşturucunun*üzerine inşa edilir. Dizinleyici, verileri örnekleme, meta veri verilerini okuma, veri alma ve yerel biçimlerden verileri sonraki alma için JSON belgelerine serileştirme için dahili mantıkla donatılmış veri kaynağına duyarlı bir alt hizmettir. Dizin oluşturucular genellikle ai ayrı, ithalat için kendileri tarafından kullanılır, ancak bir AI zenginleştirme boru hattı oluşturmak istiyorsanız, onunla gitmek için bir dizinleyici ve bir skillset gerekir. Bu bölümde dizinleyici vurgular; sonraki bölümde beceri ler üzerinde duruluyor.

Azure Depolama'daki blob'lar [Azure Bilişsel Arama Blob depolama dizinleyicisi](search-howto-indexing-azure-blob-storage.md)kullanılarak dizine eklenir. Bu dizin leyiciyi **Alma veri** sihirbazı, BIR REST API veya .NET SDK'yı kullanarak çağırabilirsiniz. Kodolarak, bu dizin leyiciyi türü ayarlayarak ve bir Azure Depolama hesabı içeren bağlantı bilgilerini blob kapsayıcısıyla birlikte sağlayarak kullanırsınız. Bloblarınızı, parametre olarak geçirebileceğiniz sanal bir dizin oluşturarak veya dosya türü uzantısına filtre uygulayarak alt kümeleyebilirsiniz.

Dizinleyici,içeriği incelemek için bir leke açarak "belge çatlamasını" yapar. Veri kaynağına bağlandıktan sonra, bu boru hattının ilk adımıdır. Blob verileri için PDF, ofis dokümanları, resim ve diğer içerik türleri burada algılanır. Metin çıkarma ile belge çatlama sı ücretsizdir. Görüntü çıkarma ile belge çatlama [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabileceğiniz fiyatlarla ücretlendirilir.

Tüm belgeler çatlamış olsa da, zenginleştirme yalnızca bunu açıkça sağlarsanız gerçekleşir. Örneğin, ardınız yalnızca görüntü çözümlemeden oluşuyorsa, kapsayıcınızdaki metin veya belgeler yoksayılır.

Blob dizinleyicisi yapılandırma parametreleriyle birlikte gelir ve temel veriler yeterli bilgi sağlarsa değişiklik izlemeyi destekler. [Azure Bilişsel Arama Blob depolama dizinleyicisinde](search-howto-indexing-azure-blob-storage.md)temel işlevsellik hakkında daha fazla bilgi edinebilirsiniz.

## <a name="add-ai-components"></a>AI bileşenleri ekleme

AI zenginleştirme desenleri veya özellikleri aramak ve daha sonra buna göre bir işlem gerçekleştirmek modülleri anlamına gelir. Fotoğraflardaki yüz tanıma, fotoğrafların metin açıklamaları, belgedeki anahtar tümceciklerin algılanması ve OCR (veya basılı veya el yazısıyla yazılmış metinleri ikili dosyalarda tanıma) açıklayıcı örneklerdir.

Azure Bilişsel Arama'da *beceriler,* tek başına veya diğer becerilerle birlikte kullanabileceğiniz AI işlemenin tek tek bileşenleridir. 

+ Yerleşik beceriler, Bilgisayar Vizyonu'na dayalı görüntü analizi ve Metin Analizi'ne dayalı doğal dil işleme ile Bilişsel Hizmetler tarafından desteklenen bir beceridir. Tam liste [için, içerik zenginleştirme için Yerleşik becerilere](cognitive-search-predefined-skills.md)bakın.

+ Özel beceriler, ardışık ardışık lığa tümleştirme sağlayan bir [arabirim tanımına](cognitive-search-custom-skill-interface.md) sarılmış özel kodlardır. Müşteri çözümlerinde, açık kaynak, üçüncü taraf veya birinci taraf AI modülleri sağlayan özel becerilerle her ikisini de kullanmak yaygın bir uygulamadır.

*Beceri,* bir ardışık alanda kullanılan becerilerin toplanmasıdır ve belge kırma aşaması içeriği kullanılabilir hale getirirsonra çağrılır. Bir dizinleyici tam olarak bir skillset tüketebilir, ancak bu skillset başka senaryolarda yeniden kullanabilirsiniz, böylece bir dizinleyici bağımsız olarak var.

Özel beceriler karmaşık gelebilir, ancak uygulama açısından basit ve basit olabilir. Desen eşleştirme veya sınıflandırma modelleri sağlayan varolan paketleriniz varsa, lekelerden çıkardığınız içerik işlenmek üzere bu modellere aktarılabilir. AI zenginleştirme Azure tabanlı olduğundan, modeliniz de Azure'da olmalıdır. Bazı yaygın barındırma metodolojileri arasında [Azure İşlevlerini](cognitive-search-create-custom-skill-example.md) veya [Kapsayıcıları](https://github.com/Microsoft/SkillsExtractorCognitiveSearch)kullanmayı içerir.

Bilişsel Hizmetler tarafından desteklenen yerleşik beceriler, kaynağa erişmenizi sağlayan [ekli Bir Arada Abonelik](cognitive-search-attach-cognitive-services.md) anahtarı gerektirir. Hepsi bir arada anahtar size görüntü analizi, dil algılama, metin çevirisi ve metin analizi sağlar. Diğer yerleşik beceriler Azure Bilişsel Arama özellikleridir ve ek hizmet veya anahtar gerektirmez. Metin şekillendirici, ayırıcı ve birleşme, ardışık alanı tasarlarken bazen gerekli olan yardımcı becerilere örnektir.

Yalnızca özel beceriler ve yerleşik yardımcı program becerileri kullanıyorsanız, Bilişsel Hizmetlerle ilgili bağımlılık veya maliyet yoktur.

<!-- ## Order of operations

Now we've covered indexers, content extraction, and skills, we can take a closer look at pipeline mechanisms and order of operations.

A skillset is a composition of one or more skills. When multiple skills are involved, the skillset operates as sequential pipeline, producing dependency graphs, where output from one skill becomes input to another. 

For example, given a large blob of unstructured text, a sample order of operations for text analytics might be as follows:

1. Use Text Splitter to break the blob into smaller parts.
1. Use Language Detection to determine if content is English or another language.
1. Use Text Translator to get all text into a common language.
1. Run Entity Recognition, Key Phrase Extraction, or Sentiment Analysis on chunks of text. In this step, new fields are created and populated. Entities might be location, people, organization, dates. Key phrases are short combinations of words that appear to belong together. Sentiment score is a rating on continuum of negative (0) to positive (1) sentiment.
1. Use Text Merger to reconstitute the document from the smaller chunks. -->

## <a name="consume-ai-enriched-output-in-downstream-solutions"></a>Akış aşağı çözümlerde AI ile zenginleştirilmiş çıktı tüketin

AI zenginleştirme çıktısı, Azure Bilişsel Arama'da bir arama dizini veya Azure Depolama'da bir [bilgi deposudur.](knowledge-store-concept-intro.md)

Azure Bilişsel Arama'da, bir istemci uygulamasında ücretsiz metin ve filtrelenmiş sorgular kullanılarak etkileşimli arama için bir arama dizini kullanılır. AI aracılığıyla oluşturulan zenginleştirilmiş belgeler JSON'da biçimlendirilir ve tüm belgeler Azure Bilişsel Arama'da dizine eklenmiş ve bir dizin oluşturucunun sağladığı tüm avantajlardan yararlanır. Örneğin, dizin oluşturma sırasında, blob dizinleyici yapılandırma parametreleri ve ayarları herhangi bir alan eşlemeleri kullanmak veya algılama mantığı değiştirmek anlamına gelir. Bu tür ayarlar, düzenli dizin oluşturma ve AI ile zenginleştirilmiş iş yükleri için tam olarak kullanılabilir. İçeriğin Azure Bilişsel Arama'da depolanmasıyla dizin oluşturma sonrası, içeriğinizi anlamak için zengin sorgular oluşturabilir ve ifadelere filtre uygulayabilirsiniz.

Azure Depolama'da, bir bilgi deposunun iki tezahürü vardır: Tablo depolama alanında bir blob kapsayıcısı veya tablolar. 

+ Bir blob konteyner diğer işlemlere beslemek istiyorsanız yararlı dır, bunların bütünüyle zenginleştirilmiş belgeleri yakalar. 

+ Buna karşılık, Tablo depolama zenginleştirilmiş belgelerin fiziksel projeksiyonları barındırabilir. Belirli bölümleri içeren veya hariç tutan zenginleştirilmiş belge dilimleri veya katmanları oluşturabilirsiniz. Power BI'deki analiz için Azure Tablo depolamasındaki tablolar daha fazla görselleştirme ve keşif için veri kaynağı haline gelir.

Ardışık nedenlerden sonra zenginleştirilmiş bir belge, zenginleştirme sırasında çıkarılan veya oluşturulan yeni bilgiler içeren ek alanların varlığıyla orijinal giriş sürümünden farklıdır. Bu nedenle, kullandığınız çıktı yapısından bağımsız olarak özgün ve oluşturulmuş içeriğin bir birleşimi ile çalışabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Depolama'daki verilerinizden en iyi şekilde elde etmek için AI zenginleştirmeile yapabileceğiniz, Bilişsel Hizmetleri farklı şekillerde birleştirme ve senaryo için mevcut Bilişsel Hizmetin olmadığı durumlar için özel beceriler yazma gibi çok daha fazla şey vardır. Aşağıdaki linkleri takip ederek daha fazla bilgi edinebilirsiniz.

+ [Azure portalı (Azure Blob depolama) ile blob'ları yükleme, indirme ve listele](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
+ [Blob dizinleyici (Azure Bilişsel Arama) ayarlama](search-howto-indexing-azure-blob-storage.md) 
+ [AI zenginleştirme genel bakış (Azure Bilişsel Arama)](cognitive-search-concept-intro.md) 
+ [Bir beceri oluşturma (Azure Bilişsel Arama)](cognitive-search-defining-skillset.md)
+ [Ek açıklama ağacındaki eş düğümleri (Azure Bilişsel Arama)](cognitive-search-output-field-mapping.md)
