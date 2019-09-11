---
title: 'Hızlı Başlangıç: Azure portal Azure Search bir AI zenginleştirme dizini oluşturun'
description: Azure portal ve örnek verileri kullanarak Azure Search dizin oluşturma portalındaki veri ayıklama, doğal dil ve görüntü işleme becerileri.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: quickstart
ms.date: 07/09/2019
ms.author: heidist
ms.openlocfilehash: 72546e6327fc3286455482943dcaedbd5a8e2943
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70744742"
---
# <a name="quickstart-create-an-ai-indexing-pipeline-using-cognitive-skills-in-azure-search"></a>Hızlı Başlangıç: Azure Search bilişsel becerileri kullanarak bir AI dizin oluşturma işlem hattı oluşturun

Azure Search bilişsel [Hizmetler](https://azure.microsoft.com/services/cognitive-services/)ile tümleşir, içerik ayıklama, doğal dil işleme (NLP) ve görüntü işleme yeteneklerini bir Azure Search dizin oluşturma işlem hattına ekleyerek aranabilir veya yapılandırılmamış içeriğin daha fazla aranabilir olmasını sağlar. 

[OCR](cognitive-search-skill-ocr.md), [dil algılama](cognitive-search-skill-language-detection.md), ad adlandırma için [varlık tanıma](cognitive-search-skill-entity-recognition.md) gibi birçok bilişsel hizmet kaynağı bir dizin oluşturma işlemine eklenebilir. Bilişsel hizmetler 'in AI algoritmaları, kaynak verilerde desenler, Özellikler ve özellikler bulmak için kullanılır ve Azure Search göre tam metin arama çözümlerinde kullanılabilecek yapıları ve metin içeriğini döndürür.

Bu hızlı başlangıçta, tek bir kod satırı yazmadan önce [Azure Portal](https://portal.azure.com) ilk zenginleştirme ardışık düzenini oluşturun:

> [!div class="checklist"]
> * Azure Blob depolamada örnek verileri kullanmaya başlama
> * Bilişsel dizin oluşturma ve zenginleştirme için [**veri Içeri aktarma**](search-import-data-portal.md) Sihirbazı 'nı yapılandırın 
> * Sihirbazı çalıştırma (bir varlık becerisi, kişileri, konumu ve kuruluşları algılar)
> * Zenginleştirilmiş verileri sorgulamak için [**Arama Gezgini**](search-explorer.md) 'ni kullanma

Bu hızlı başlangıç ücretsiz hizmette çalışır, ancak ücretsiz işlem sayısı günde 20 belge ile sınırlıdır. Bu hızlı başlangıcı günde bir kereden fazla çalıştırmak istiyorsanız, daha fazla çalıştırmaya uyabilmeniz için daha küçük bir dosya kümesi kullanın.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

Bilişsel [HIZMETLER](https://azure.microsoft.com/services/cognitive-services/) AI sağlar. Bu hızlı başlangıç, işlem hattını belirtirken bu kaynakları satır içinde eklemeye yönelik adımları içerir. Hesapları önceden ayarlamak gerekli değildir.

Dizin oluşturma işlem hattının girdilerini sağlamak için Azure Hizmetleri gereklidir. [Azure Search Dizin oluşturucular](search-indexer-overview.md)tarafından desteklenen herhangi bir veri kaynağını kullanabilirsiniz. Bu hızlı başlangıç, kaynak veri dosyaları için bir kapsayıcı olarak [Azure Blob depolamayı](https://azure.microsoft.com/services/storage/blobs/) kullanır. 

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Azure Blob hizmetini ayarlama ve örnek veriler yükleme

1. Farklı türlerden oluşan küçük bir dosya kümesini içeren [örnek verileri indirin](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). 

1. [Azure Blob depolama alanına kaydolun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal), bir depolama hesabı oluşturun, blob Hizmetleri sayfalarını açın ve bir kapsayıcı oluşturun.  Depolama hesabını Azure Search ile aynı bölgede oluşturun.

1. Oluşturduğunuz kapsayıcıda, önceki bir adımda indirdiğiniz örnek dosyaları karşıya yüklemek için **karşıya yükle** ' ye tıklayın.

   ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

## <a name="create-the-enrichment-pipeline"></a>Zenginleştirme işlem hattı oluşturma

Azure Search hizmet panosu sayfasına dönün ve dört adımda Bilişsel Zenginleştirme ayarlamak için komut çubuğunda **verileri Içeri aktar** ' a tıklayın.

  ![Verileri içeri aktar komutu](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>1\. adım: Bir veri kaynağı oluşturun

**Verilerinize bağlanın**bölümünde **Azure Blob depolama**' yı seçin, oluşturduğunuz hesabı ve kapsayıcıyı seçin. Veri kaynağına bir ad verin ve geri kalanı için varsayılan değerleri kullanın. 

  ![Azure blob yapılandırması](./media/cognitive-search-quickstart-blob/blob-datasource.png)

Sonraki sayfaya devam edin.

  ![Bilişsel arama için sonraki sayfa düğmesi](media/cognitive-search-quickstart-blob/next-button-add-cog-search.png)

### <a name="step-2-add-cognitive-skills"></a>2\. adım: Bilişsel yetenekler ekleme

Daha sonra dizin oluşturma işlem hattına zenginleştirme adımları ekleyin. Bilişsel hizmetler kaynağınız yoksa, size günlük 20 işlem sağlayan ücretsiz bir sürüm için kaydolabilirsiniz. Örnek veriler 14 dosyadan oluşur; bu nedenle günlük ayırma, bu Sihirbazı çalıştırdığınızda genellikle kullanılır.

1. Bilişsel Hizmetler API'si kaynak alma seçeneklerini görüntülemek için bilişsel **Hizmetler Ekle** ' yi genişletin. Bu öğreticinin amaçları doğrultusunda **ücretsiz** kaynağı kullanabilirsiniz.

   ![Bilişsel hizmetler iliştirme](media/cognitive-search-quickstart-blob/cog-search-attach.png)

2. **Zenginleştirme Ekle** ' yi genişletin ve doğal dil işleme yapan becerileri seçin. Bu hızlı başlangıç için, kişiler, kuruluşlar ve konumlar için varlık tanımayı seçin.

   ![Bilişsel hizmetler iliştirme](media/cognitive-search-quickstart-blob/skillset.png)

   Portal, OCR işleme ve metin analizi için yerleşik yetenekler sunar. Portalda beceri kümesi, tek bir kaynak alanının üzerinde çalışır. Bu küçük bir hedef gibi görünebilir, ancak Azure blobları için `content` alanı, blob belgesinin çoğunu içerir (örneğin, Word belgesi veya PowerPoint destesi). Aynı şekilde bir blobun tüm içeriği de bu alanda bulunduğundan bu alan ideal bir giriştir.

3. Sonraki sayfaya devam edin.

   ![Sonraki sayfa dizini Özelleştir](media/cognitive-search-quickstart-blob/next-button-customize-index.png)

> [!NOTE]
> Doğal dil işleme becerileri, örnek veri kümesindeki metin içeriği üzerinde çalışır. OCR seçeneğini seçmediğinizden, örnek veri kümesinde bulunan JPEG ve PNG dosyaları bu hızlı başlangıçta işlenmez. 

### <a name="step-3-configure-the-index"></a>3\. adım: Dizini yapılandırma

Sihirbaz genellikle varsayılan bir dizin çıkarsalabilir. Bu adımda, oluşturulan dizin şemasını görüntüleyebilir ve olabilecek ayarları değiştirebilirsiniz. Demo blob veri kümesi için oluşturulan varsayılan dizin aşağıda verilmiştir.

Bu hızlı başlangıç, makul varsayılanlar ayarlanması konusunda iyi bir iş çıkarır: 

+ Varsayılan ad, veri kaynağı türüne göre *azureblob-index* ' dir. 

+ Varsayılan alanlar, özgün kaynak veri alanını`content`() ve bilişsel işlem hattı tarafından oluşturulan çıkış alanlarını (`people`, `locations` `organizations`ve) temel alır. Varsayılan veri türleri meta verilerden ve veri örneklemede algılanır.

+ Varsayılan anahtar *metadata_storage_path* 'dir (Bu alan benzersiz değerler içerir).

+ Varsayılan öznitelikler **alınabilir** ve bu alanlar için **aranabilir** . **Aranabilir** bir alanın aranamayacağını gösterir. **Alınabilir** , sonuçlarda döndürülebilecek anlamına gelir. Sihirbaz, bir beceri aracılığıyla oluşturduğunuz için bu alanların alınabilir ve aranabilir olmasını istediğiniz varsayılır.

  ![Dizin alanları](media/cognitive-search-quickstart-blob/index-fields.png)

`content` Alana göre **alınabilir** özniteliğinde üstü çizili ve soru işaretine dikkat edin. Metin açısından ağır blob belgeleri `content` için, alan büyük olasılıkla binlerce satıra çalışan dosyanın toplu kısmını içerir. Dosya içeriğini istemci koduna geçirmeniz gerekirse, **alınabilir** ' ın seçili olduğundan emin olun. Aksi takdirde `content` , ayıklanan öğeler (`people`, `organizations`, ve `locations`) amacınıza uygun ise bu özniteliği temizlemeyi göz önünde bulundurun.

Bir alanı **alınabilir** olarak işaretlemek alanın arama sonuçlarında bulunması *gerektiği* anlamına gelmez. Hangi alanların ekleneceğini belirlemek için **$Select** sorgu parametresini kullanarak arama sonuçları kompozisyonunu kesin bir şekilde denetleyebilirsiniz. Gibi `content`metin açısından ağır alanlar için, **$Select** parametresi, uygulamanızın insan kullanıcılarına yönetilebilir arama sonuçları sağlamaya yönelik çözümünüzde, istemci kodunun, **gereken tüm bilgilere Alınabilir** öznitelik.
  
Sonraki sayfaya devam edin.

  ![Sonraki sayfa Dizin Oluşturucu oluştur](media/cognitive-search-quickstart-blob/next-button-create-indexer.png)

### <a name="step-4-configure-the-indexer"></a>4\. Adım: Dizin oluşturucuyu yapılandırma

Dizin oluşturucu, dizin oluşturma işlemini destekleyen, yüksek düzeyli bir kaynaktır. Veri kaynağı adı, hedef dizin ve yürütme sıklığını belirtir. **Verileri içeri aktar** sihirbazının nihai sonucunda her zaman art arda çalıştırabileceğiniz bir dizin oluşturucu elde edilir.

**Dizin Oluşturucu** sayfasında, varsayılan adı kabul edebilir ve hemen çalıştırmak için **bir kez çalıştır** zamanlama seçeneğini kullanabilirsiniz. 

  ![Dizin oluşturucu tanımı](media/cognitive-search-quickstart-blob/indexer-def.png)

Dizin oluşturucuyu oluşturmak ve aynı anda çalıştırmak için **Gönder** ' e tıklayın.

## <a name="monitor-indexing"></a>Dizin oluşturmayı izle

Zenginleştirme adımları, tipik metin tabanlı dizin oluşturma işleminin tamamlanması için daha uzun sürer. İlerlemeyi izleyebilmek için sihirbazın Genel Bakış sayfasında Dizin Oluşturucu listesini açması gerekir. Kendi kendine gezinme için genel bakış sayfasına gidin ve **Dizin oluşturucular**' ye tıklayın.

JPG ve PNG dosyaları görüntü dosyaları olduğu ve bu işlem hattından OCR becerinizi atdığımız için uyarı oluşur. Ayrıca, kesme bildirimleri de bulacaksınız. Azure Search, serbest katmandaki ayıklama ile 32.000 karakter arasında sınırlar.

  ![Azure Search bildirimi](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Dizin oluşturma ve zenginleştirme zaman alabilir; ilk keşif için daha küçük veri kümelerinin önerilmesinin nedeni budur. 

## <a name="query-in-search-explorer"></a>Arama gezgininde sorgulama

Bir dizin oluşturulduktan sonra, dizinden belgeleri döndürmek için sorgular gönderebilirsiniz. Portalda **Arama gezgini**’ni kullanarak sorgular çalıştırın ve sonuçları görüntüleyin. 

1. Arama hizmeti panosu sayfasında, komut çubuğunda **Arama gezgini**’ne tıklayın.

1. Oluşturduğunuz dizini seçmek için üst kısımdaki **Dizini değiştir**'e tıklayın.

1. Dizini sorgulamak için bir arama dizesi girin, örneğin `search=Microsoft&searchFields=organizations`.

Sonuçlar JSON olarak döndürülür, bu nedenle özellikle de Azure bloblardan gelen büyük belgelerde ayrıntılı ve okuması zor olabilir. Sonuçları kolayca tarayamazsanız, belgeler içinde arama yapmak için CTRL-F tuşlarını kullanın. Bu sorgu için, JSON içinde belirli koşullara göre arama yapabilirsiniz. 

CTRL-F tuş birleşimi, belirli bir sonuç kümesinde kaç tane belgenin bulunduğunu belirlemenize de yardımcı olabilir. Her değer belge için benzersiz olduğundan Azure blobları için portal, anahtar olarak "metadata_storage_path" öğesini seçer. CTRL-F kullanarak, "metadata_storage_path" araması yapıp belgelerin sayısını alın. 

  ![Arama gezgini örneği](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Paketler

Artık ilk bilişsel, zenginleştirilmiş dizin oluşturma Alıştırmayı tamamladınız. Bu hızlı başlangıcın amacı, kendi verilerinizi kullanarak hızlı şekilde bir bilişsel arama çözümünün prototipini oluşturabilmeniz için sihirbazda size yol göstermek ve önemli kavramları tanıtmaktır.

Topladığınız bazı temel kavramlar, Azure veri kaynaklarına bağımlılık içerir. Bilişsel arama zenginleştirmesi, dizin oluşturuculara bağlıdır ve dizin oluşturucular, Azure’a ve kaynağa özgüdür. Bu hızlı başlangıçta Azure Blob depolama kullanılsa da, başka Azure veri kaynakları da mümkündür. Daha fazla bilgi için bkz. [Azure Search’te dizin oluşturucular](search-indexer-overview.md).

Başka bir önemli kavram da, becerilerin giriş alanları üzerinde çalışmasıdır. Portalda, tüm yetenekler için tek kaynak alan seçmeniz gerekir. Kodda girişler, başka alanlar olabilir veya bir yukarı akış becerisinin çıktısı olabilir.

 Bir beceriye yönelik girişler, bir dizindeki çıktı alanına eşlenir. Dahili olarak portal, [ek açıklamalar](cognitive-search-concept-annotations-syntax.md) ayarlar ve işlemlerin sırasını ve genel akışı oluşturan bir [beceri kümesini](cognitive-search-defining-skillset.md) tanımlar. Bu adımlar portalda gizlenir, ancak kod yazmaya başladığınızda bu kavramlar önemli hale gelir.

Son olarak, görüntüleme sonuçlarının dizinin sorgulanmasıyla elde edildiğini öğrendiniz. Sonunda Azure Search, [basit](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) veya [tamamen genişletilmiş sorgu sözdizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) kullanarak sorgulayabileceğiniz, aranabilir bir dizin sağlar. Zenginleştirilmiş alanlar içeren bir dizin de diğerlerine benzer. Standart veya [özel çözümleyiciler](search-analyzers.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [eş anlamlılar](search-synonyms.md), [ayrıntılı filtreler](search-filters-facets.md), coğrafi arama veya başka bir Azure Search özelliğine yer vermek istiyorsanız bunu yapabilirsiniz.

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, sizin oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için bir projenin sonunda iyi bir fikir olur. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler kaynağını nasıl sağladığınıza bağlı olarak, Sihirbazı farklı beceriler ve kaynak veri alanlarıyla yeniden çalıştırarak dizin oluşturma ve zenginleştirme ile denemeler yapabilirsiniz. Adımları yinelemek için, dizini ve dizin oluşturucuyu silin, ardından dizin oluşturucuyu yeni seçimlerle yeniden oluşturun.

+ **Genel Bakış** > **Dizinler** bölümünde, oluşturduğunuz dizini seçin ve sonra **Sil**’e tıklayın.

+ **Genel Bakış**’ta **Dizin Oluşturucular** kutucuğuna çift tıklayın. Oluşturduğunuz dizin oluşturucuyu bulup silin.

Alternatif olarak, oluşturduğunuz örnek veri ve hizmetleri yeniden kullanın ve sonraki öğreticide aynı görevleri programlama yoluyla nasıl gerçekleştireceğinizi öğrenin. 

> [!div class="nextstepaction"]
> [Öğretici: Bilişsel arama REST API 'Lerini öğrenin](cognitive-search-tutorial-blob.md)
