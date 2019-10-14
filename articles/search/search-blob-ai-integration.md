---
title: Blob verilerini anlamak için AI kullanma
titleSuffix: Azure Search
description: Azure Search ' de bir AI zenginleştirme işlem hattı kullanarak Azure bloblarına anlam, doğal dil işleme ve görüntü analizi ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: search
ms.topic: conceptual
ms.date: 10/09/2019
ms.openlocfilehash: 2513825fcb275aeb3c4f0ca49ff5f2a6bd9441f0
ms.sourcegitcommit: bd4198a3f2a028f0ce0a63e5f479242f6a98cc04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303021"
---
# <a name="use-ai-to-understand-blob-data"></a>Blob verilerini anlamak için AI kullanma

Azure Blob depolamada bulunan veriler genellikle görüntüler, uzun metin, PDF 'Ler ve Office belgeleri gibi yapılandırılmamış içerik olan çok çeşitli içeriktir. Azure Search içindeki AI yeteneklerini kullanarak, çeşitli yollarla bloblardan değerli bilgileri anlayabilir ve ayıklayabilirsiniz. Blob içeriğine AI uygulama örnekleri şunlardır:

+ Optik karakter tanıma özelliğini kullanarak görüntülerden metin ayıklama (OCR)
+ Fotoğraftan bir sahne açıklaması veya etiketleri üretme
+ Dili algıla ve metni farklı dillere çevir
+ Kişilere, tarihlere, yerlere veya kuruluşlara yönelik başvuruları bulmak için adlandırılmış varlık tanıma (NER) ile metin işleme 

Bu AI özelliklerinden yalnızca birine ihtiyacınız olabilir, ancak birden çok aynı işlem hattına (örneğin, taranmış bir görüntüden metin ayıklanarak ve bu konumda başvurulan tüm tarihleri ve yerleri buluyor) daha yaygın olarak vardır. 

AI zenginleştirme, metin olarak yakalanan ve alanlarda depolanan yeni bilgiler oluşturur. Son zenginleştirme, bu bilgilere tam metin araması aracılığıyla bir arama dizininden erişebilirsiniz veya bulma veya analiz senaryoları için verileri araştırmayı kapsayan yeni uygulama deneyimlerine Power Storage 'a zenginleştirilmiş belgeler gönderebilirsiniz. 

Bu makalede, Bloblar içindeki ham verileri bir arama dizininde veya bilgi deposunda sorgulanabilir bilgilere dönüştürmek için, tüm süreci hızlı bir şekilde bir lens aracılığıyla bir çok lens aracılığıyla inceleyeceğiz.

## <a name="what-it-means-to-enrich-blob-data"></a>Blob verilerini "zenginleştirme" anlamına gelir

*AI zenginleştirme* , Microsoft 'un veya SAĞLADıĞıNıZ özel AI 'nın yerleşik AI ile tümleşen Azure Search dizin oluşturma mimarisinin bir parçasıdır. Blob 'ları (varsa veya güncelleştirdikleri gibi mevcut olanları ve yeni olanları) işlemek için tüm dosya biçimlerini açmak, çeşitli AI yeteneklerini kullanarak istenen bilgileri ayıklamak, ve ' yi hızlı arama, alma ve araştırma için bir Azure Search dizininde dizin oluşturma. 

Azure Blob depolama alanında, tek bir kapsayıcıda bulunan bloblarınızın girdileri vardır. Blob 'lar neredeyse her türlü metin veya resim verisi olabilir. 

Çıktı her zaman, istemci uygulamalarında hızlı metin arama, alma ve araştırma için kullanılan bir Azure Search dizinidir. Ayrıca, çıktı Ayrıca, belgeleri Azure Blob 'larına veya Power BI ya da veri bilimi iş yükleri gibi araçlarla aşağı akış analizi için Azure tablolarına zenginleştiren bir *bilgi deposu* da olabilir.

Between, ardışık düzen mimarisinin kendisidir. İşlem hattı, AI sağlayan bir veya daha fazla *beceriden* oluşan bir *beceri*atayabileceğiniz *Dizin Oluşturucu* özelliğini temel alır. İşlem hattının amacı, ham içerik olarak girebileceğiniz ancak ardışık düzen boyunca geçiş yaparken ek yapıyı, bağlamı ve bilgileri alan, *zenginleştirilmiş belgeler* oluşturmak için kullanılır. Zenginleştirilmiş belgeler, tam metin aramasında veya araştırmayla ve analizte kullanılan ters dizinler ve diğer yapılar oluşturmak için dizin oluşturma sırasında tüketilecektir.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Doğrudan depolama hesabı portalı sayfanızda başlayabilirsiniz. **Azure Search Ekle** ' ye tıklayın ve yeni bir Azure Search hizmeti oluşturun veya var olan bir hizmeti seçin. Aynı abonelikte mevcut bir arama hizmetiniz zaten varsa, **Azure Search Ekle** ' ye tıklayarak veri alma Sihirbazı ' nı açar, böylece dizin oluşturma, zenginleştirme ve Dizin tanımı anında ilerlebilmenizi sağlayabilirsiniz.

Depolama hesabınıza Azure Search ekledikten sonra, herhangi bir Azure veri kaynağındaki verileri zenginleştirmek için standart işlemi izleyebilirsiniz. Zaten blob içeriğiniz olduğunu varsayarsak, AI zenginleştirmesi için kolay bir başlangıç girişi için Azure Search veri alma Sihirbazı 'nı kullanabilirsiniz. Bu hızlı başlangıçta, [portalda BIR AI zenginleştirme işlem hattı oluşturma](cognitive-search-quickstart-blob.md)adımları açıklanmaktadır. 

Aşağıdaki bölümlerde, daha fazla bileşen ve kavram keşfedeceğiz.

## <a name="use-blob-indexers"></a>Blob Dizin oluşturucular kullanma

AI zenginleştirme, dizin oluşturma işlem hattının bir eklentisi ve Azure Search, bu işlem hatları bir *dizin oluşturucunun*üzerine kurulmuştur. Dizin Oluşturucu, verileri örnekleme, meta veri verileri okuma, verileri alma ve yerel biçimlerdeki verileri, sonraki içeri aktarma için JSON belgelerine serileştirmede iç mantığa sahip olan veri kaynağı kullanan bir alt hizmettir. Dizin oluşturucular genellikle AI tarafından içeri aktarma için kullanılır, ancak bir AI zenginleştirme işlem hattı oluşturmak istiyorsanız, bir Dizin Oluşturucu ve bir beceri ile birlikte çalışmak için bir gerekir. Bu bölümde, dizin oluşturucunun kendine odaklanacağız.

Azure depolama 'daki Bloblar [Azure Search blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)kullanılarak dizinlenir. Türü ayarlayarak ve bir blob kapsayıcısı ile birlikte bir Azure depolama hesabı içeren bağlantı bilgilerini sağlayarak bu dizin oluşturucuyu çağırabilirsiniz. Daha önce blob 'ları bir sanal dizinde düzenlemediğiniz sürece bir parametre olarak geçirebilmeniz için, blob Indexer kapsayıcının tamamını çeker.

Bir Dizin Oluşturucu "belgeyi çözme" ve veri kaynağına bağlandıktan sonra, işlem hattının ilk adımıdır. Blob verileri için PDF, Office belgeleri, görüntü ve diğer içerik türlerinin algılandığı yerdir. Metin ayıklama ile belge çözme ücretsizdir. Görüntü ayıklama ile belge çözme, Azure Search [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabileceğiniz oranlar üzerinden ücretlendirilir.

Tüm belgelerin kırdığı halde, zenginleştirme yalnızca açıkça bunu yapmak için becerileri sağladığınızda oluşur. Örneğin, işlem hattınız yalnızca metin analizinden oluşuyorsa, kapsayıcıınızdaki veya belgelerinizdeki tüm görüntüler yok sayılır.

Blob Indexer yapılandırma parametreleriyle birlikte gelir ve temel alınan veriler yeterli bilgi sağlıyorsa değişiklik izlemeyi destekler. [Azure Search blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)'da çekirdek işlevsellik hakkında daha fazla bilgi edinebilirsiniz.

## <a name="add-ai"></a>AI Ekle

*Yetenekler* , tek başına veya sıralı işleme için diğer becerilerle bırlıkte kullanabileceğiniz AI işlemenin tek tek bileşenleridir. 

+ Yerleşik yetenekler bilişsel hizmetler tarafından desteklenir, Görüntü İşleme dayalı görüntü analizi ve Metin Analizi temelinde doğal dil işleme. Birkaç örnek [OCR](cognitive-search-skill-ocr.md), [varlık tanıma](cognitive-search-skill-entity-recognition.md)ve [resim analizlidir](cognitive-search-skill-image-analysis.md). [İçerik zenginleştirme Için önceden tanımlanmış yeteneklerin](cognitive-search-predefined-skills.md)içindeki yerleşik yeteneklerin tam listesini inceleyebilirsiniz.

+ Özel yetenekler, işlem hattına tümleştirmeye izin veren bir arabirim tanımına Sarmalanan özel koddur. Müşteri çözümlerinde, açık kaynaklı, üçüncü taraf veya birinci taraf AI modülleri sağlayan özel yetenekler ile her ikisini de kullanmak yaygın bir uygulamadır.

*Beceri* , bir işlem hattında kullanılan yeteneklerin koleksiyonudur ve belge çözme aşaması içeriği kullanılabilir hale getirdikten sonra çağrılır. Bir Dizin Oluşturucu tam olarak bir beceri tüketebilir, ancak diğer senaryolarda yeniden kullanabilmeniz için bu beceri bir dizin oluşturucudan bağımsız olarak mevcuttur.

Özel yetenekler karmaşık olabilir, ancak uygulama bakımından basit ve kolay olabilir. Model eşleştirme veya sınıflandırma modelleri sağlayan mevcut Paketleriniz varsa, bloblardan ayıklamanız gereken içerik işlenmek üzere bu modellere geçirilebilir. AI zenginleştirme, Azure tabanlı olduğundan modelinize Azure 'da da sahip olmanız gerekir. Bazı yaygın barındırma yöntemleri [Azure işlevleri](cognitive-search-create-custom-skill-example.md) veya [kapsayıcıları](https://github.com/Microsoft/SkillsExtractorCognitiveSearch)kullanmayı içerir.

Bilişsel hizmetler tarafından desteklenen yerleşik yetenekler, kaynağa erişmenizi sağlayan, bağlı bilişsel [hizmetlerin](cognitive-search-attach-cognitive-services.md) hepsi bir arada bir abonelik anahtarı gerektirir. Hepsi bir arada anahtar, görüntü analizi, dil algılama, metin çevirisi ve metin analizi sağlar. Diğer yerleşik yetenekler Azure Search özelliklerdir ve ek hizmet veya anahtar gerektirmez. Metin Shaper, Splitter ve birleşme, işlem hattı tasarlarken bazen gerekli olan yardımcı beceriler örnekleridir.

Yalnızca özel yetenekler ve yerleşik yardımcı program becerileri kullanıyorsanız, bilişsel hizmetlerle ilgili bağımlılık veya maliyet yoktur.

## <a name="order-of-operations"></a>İşlem sırası

Artık Dizin oluşturucular, içerik ayıklama ve beceriler kapsandık, ardışık düzen mekanizmalarına ve işlem sıralarına daha yakından bakalım.

Bir beceri, bir veya daha fazla becerilerin bir bileşim olur. Birden çok yetenek dahil edildiğinde, Beceri ardışık işlem hattı olarak çalışır ve bu da bir beceriye ait çıktının diğerine giriş haline gelir. 

Örneğin, büyük bir yapılandırılmamış metin blobu verildiğinde, metin analizi için bir işlem örnek sırası aşağıdaki gibi olabilir:

1. Blobu daha küçük parçalara bölmek için metin Bölümlendiricisini kullanın.
1. İçeriğin Ingilizce mi yoksa başka bir dil mi olduğunu anlamak için Dil Algılama kullanın.
1. Tüm metni ortak bir dile almak için metin çeviricisini kullanın.
1. Metin öbeklerinin varlık tanıma, Anahtar İfade Ayıklama veya Yaklaşım Analizi çalıştırın. Bu adımda yeni alanlar oluşturulur ve doldurulur. Varlıklar konum, kişi, kuruluş, tarih olabilir. Anahtar tümceleri, birbirine ait olan sözcüklerin kısa birleşimleridir. Yaklaşım puanı negatif (0) ile pozitif (1) yaklaşım için bir derecelendirmenin bir derecelendirmesidir.
1. Belgeyi daha küçük öbeklerden edilmeyen için metin Merkli 'yi kullanın.


## <a name="outputs-and-use-cases"></a>Çıktılar ve kullanım örnekleri

İşlem hattının sonunda zenginleştirilmiş bir belge, zenginleştirme sırasında ayıklanan veya oluşturulan yeni bilgileri içeren ek alanların varlığına göre orijinal giriş sürümünden farklıdır. Bu nedenle, özgün ve oluşturulmuş değerlerin bir bileşimiyle birkaç şekilde çalışabilirsiniz.

Çıktı formaları Azure Search, Azure depolama 'daki bir bilgi deposu veya bir arama dizinidir.

Azure Search, zenginleştirilmiş belgeler JSON 'da biçimlendirilir ve bir dizin oluşturucunun sağladığı avantajlar sayesinde, tüm belgelerin dizinlendiği şekilde dizin oluşturulabilir. Zenginleştirilmiş belgelerden alınan alanlar bir dizin şemasına eşlenir. Dizin oluşturma sırasında, blob Indexer, belirttiğiniz alan eşlemelerini veya değişiklik algılama mantığını kullanmak için yapılandırma parametreleri ve ayarlar anlamına gelir. Dizin oluşturma sonrası, içerik Azure Search depolandığında, içeriğinizi anlamak için zengin sorgular ve filtre ifadeleri oluşturabilirsiniz.

Azure depolama 'da bir bilgi deposunda iki bildirim bulunur: bir blob kapsayıcısı veya tablo depolamadaki tablolar. Blob kapsayıcısı, zenginleştirilmiş belgeleri tamamen yakalar ve diğer işlemlere akış yapmak istiyorsanız yararlı olur. Buna karşılık tablo depolama, zenginleştirilmiş belgelerin fiziksel projeksiyonlarını kapsayabilmelidir. Belirli kısımları dahil eden veya hariç tutkaya ait belgelerin dilimlerini veya katmanlarını oluşturabilirsiniz. Power BI analize yönelik olarak, Azure Tablo depolamadaki tablolar, daha fazla görselleştirme ve araştırma için veri kaynağı haline gelir.

## <a name="next-steps"></a>Sonraki adımlar

Farklı yollarla bilişsel hizmetler 'i birleştirme ve senaryo için mevcut bilişsel hizmet olmadığı durumlar için özel yetenekler yazma dahil olmak üzere, Azure depolama 'daki verilerden en iyi şekilde yararlanmak için, AI zenginleştirmesiyle ilgili çok daha fazla şey vardır. Aşağıdaki bağlantıları izleyerek daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [AI zenginleştirme genel bakış](cognitive-search-concept-intro.md) 
>  bir[ek açıklama ağacında](cognitive-search-output-field-mapping.md) beceri 
>  eşleme düğümleri[oluşturma](cognitive-search-defining-skillset.md)
