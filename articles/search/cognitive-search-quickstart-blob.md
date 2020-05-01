---
title: 'Hızlı başlangıç: Azure portal bir beceri oluşturma'
titleSuffix: Azure Cognitive Search
description: Bu portal hızlı başlangıçta, Azure Bilişsel Arama 'te bir dizin oluşturma işlem hattına bilişsel beceriler eklemek için veri alma Sihirbazı ' nı nasıl kullanacağınızı öğrenin. Yetenekler arasında optik karakter tanıma (OCR) ve doğal dil işleme sayılabilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 12/20/2019
ms.openlocfilehash: e2e17ba6af60fa495a03e7d46a07cfe6b66f4e68
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77472426"
---
# <a name="quickstart-create-an-azure-cognitive-search-cognitive-skillset-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal Azure Bilişsel Arama bilişsel bir beceri oluşturma

Beceri, büyük ölçekli metinden veya resim dosyalarından bilgi ve yapıyı çıkaran ve Azure Bilişsel Arama tam metin arama sorguları için dizinlenebilir ve aranabilir hale getiren bir AI özelliğidir. 

Bu hızlı başlangıçta, beceri oluşturmak için Azure bulutundaki Hizmetleri ve verileri birleştirebilirsiniz. Her şey oluşturulduktan sonra, portalda **verileri Içeri aktarma** Sihirbazı 'nı çalıştırarak tümünü bir araya getirin. Nihai sonuç, AI işleme tarafından oluşturulan verilerle doldurulan ve portalda ([Arama Gezgini](search-explorer.md)) sorgulayabilmeniz için aranabilir bir dizindir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-services-and-load-data"></a>Hizmet oluşturma ve veri yükleme

Bu hızlı başlangıç, AI için Azure Bilişsel Arama, [Azure Blob depolama](https://docs.microsoft.com/azure/storage/blobs/)ve Azure bilişsel [Hizmetler](https://azure.microsoft.com/services/cognitive-services/) 'i kullanır. 

İş yükü çok küçük olduğu için bilişsel hizmetler, en fazla 20 işlem için ücretsiz işleme sağlamak üzere arka planda dokunduğunda. Bu tür küçük bir veri kümesi için bilişsel hizmetler kaynağı oluşturma veya ekleme işlemini atlayabilirsiniz.

1. Farklı türlerden oluşan küçük bir dosya kümesini içeren [örnek verileri indirin](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). Dosyaları sıkıştırmayı açın.

1. [Bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) veya [var olan bir hesabı bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/). 

   Bant genişliği ücretlerinden kaçınmak için Azure Bilişsel Arama ile aynı bölgeyi seçin. 
   
   Bilgi deposu özelliğini daha sonra denemek istiyorsanız, başka bir yönergede StorageV2 (genel amaçlı v2) hesap türünü seçin. Aksi takdirde herhangi bir tür seçin.

1. Blob Hizmetleri sayfalarını açın ve bir kapsayıcı oluşturun. Varsayılan genel erişim düzeyini kullanabilirsiniz. 

1. Kapsayıcıda, ilk adımda indirdiğiniz örnek dosyaları karşıya yüklemek için **karşıya yükle** ' ye tıklayın. Yerel biçimlerinde aranabilir tam metin olmayan resimler ve uygulama dosyaları dahil olmak üzere çok çeşitli içerik türleri olduğuna dikkat edin.

   ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

1. [Bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

Artık veri alma Sihirbazı 'na taşımaya hazırsınız.

## <a name="run-the-import-data-wizard"></a>Veri alma Sihirbazı 'nı çalıştırma

Arama hizmeti genel bakış sayfasında, komut çubuğunda **verileri Içeri aktar** ' a tıklayarak Bilişsel Zenginleştirme dört adımda ayarlayın.

  ![Verileri içeri aktar komutu](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1---create-a-data-source"></a>1. adım-veri kaynağı oluşturma

1. **Verilerinize bağlanın**bölümünde **Azure Blob depolama**' yı seçin, oluşturduğunuz depolama hesabını ve kapsayıcıyı seçin. Veri kaynağına bir ad verin ve geri kalanı için varsayılan değerleri kullanın. 

   ![Azure blob yapılandırması](./media/cognitive-search-quickstart-blob/blob-datasource.png)

    Sonraki sayfaya devam edin.

### <a name="step-2---add-cognitive-skills"></a>2. adım-bilişsel yetenekler ekleme

Ardından, AI zenginleştirme 'yı OCR, görüntü analizi ve doğal dil işlemeyi çağırmak üzere yapılandırın. 

1. Bu hızlı başlangıçta, **ücretsiz** bilişsel hizmetler kaynağını kullanıyoruz. Örnek veriler 14 dosyadan oluşur, bu nedenle bilişsel hizmetler üzerinde 20 işlemin ücretsiz servis birimi bu hızlı başlangıç için yeterlidir. 

   ![Bilişsel Hizmetleri Ekleme](media/cognitive-search-quickstart-blob/cog-search-attach.png)

1. **Zenginleştirme Ekle** ' yi genişletin ve dört seçim yapın. 

   Sihirbaz sayfasına görüntü analizi becerileri eklemek için OCR 'yi etkinleştirin.

   Metni daha küçük parçalara bölmek için sayfalar için ayrıntı düzeyi ayarlayın. Çeşitli metin becerileri 5 KB 'lik girişlerle sınırlıdır.

   Varlık tanıma (kişiler, kuruluşlar, konumlar) ve görüntü analizi becerileri seçin.

   ![Bilişsel Hizmetleri Ekleme](media/cognitive-search-quickstart-blob/skillset.png)

   Sonraki sayfaya devam edin.

### <a name="step-3---configure-the-index"></a>3. adım-dizini yapılandırma

Bir dizin, aranabilir içeriğinizi içerir ve **veri Içeri aktarma** Sihirbazı, genellikle veri kaynağını örnekleyerek şemayı oluşturabilir. Bu adımda, oluşturulan şemayı gözden geçirin ve ayarları olası olarak gözden geçirin. Demo blob veri kümesi için oluşturulan varsayılan şema aşağıda verilmiştir.

Bu hızlı başlangıç, makul varsayılanlar ayarlanması konusunda iyi bir iş çıkarır:  

+ Varsayılan alanlar, mevcut Blobların özelliklerine ve zenginleştirme çıkışı (örneğin `people` `organizations`,,, `locations`) içeren yeni alanlara dayanır. Veri türleri meta verilerden ve veri örneklemesi tarafından algılanır.

+ Varsayılan belge anahtarı *metadata_storage_path* (alan benzersiz değerler içerdiğinden seçilidir).

+ Varsayılan öznitelikler **alınabilir** ve **aranabilir**. **Aranabilir** , bir alan için tam metin aramasına izin verir. **Alınabilir** anlamına gelir, sonuç olarak alan değerleri döndürülür. Sihirbaz, bir beceri aracılığıyla oluşturduğunuz için bu alanların alınabilir ve aranabilir olmasını istediğiniz varsayılır.

  ![Dizin alanları](media/cognitive-search-quickstart-blob/index-fields.png)

Alana göre alınabilir özniteliğinde üstü çizili ve soru işaretine dikkat edin. **Retrievable** `content` Metin açısından ağır blob belgeleri için, `content` alan büyük olasılıkla binlerce satıra çalışan dosyanın toplu kısmını içerir. Bu gibi bir alan, arama sonuçlarında çok daha gerçekçi bir alandır ve bu demo için dışarıda bırakmanız gerekir. 

Ancak, dosya içeriklerini istemci koduna geçirmeniz gerekirse, bu dosyanın, **alınabilir** olarak seçili kaldığından emin olun. Aksi takdirde, ayıklanan öğeler (örneğin `content` `people` `organizations` `locations`,, vb.) yeterliyse, üzerinde bu özniteliği temizlemeyi göz önünde bulundurun.

Bir alanı **alınabilir** olarak işaretlemek alanın arama sonuçlarında bulunması *gerektiği* anlamına gelmez. Hangi alanların ekleneceğini belirlemek için **$Select** sorgu parametresini kullanarak arama sonuçları kompozisyonunu kesin bir şekilde denetleyebilirsiniz. Gibi `content`metin açısından ağır alanlar için, **$Select** parametresi, uygulamanızın insan kullanıcılarına yönetilebilir arama sonuçları sağlamaya yönelik çözümünüzde, istemci kodunun **alınabilir** öznitelik aracılığıyla ihtiyaç duyacağı tüm bilgilere erişmesine olanak sağlar.
  
Sonraki sayfaya devam edin.

### <a name="step-4---configure-the-indexer"></a>4. adım-Dizin oluşturucuyu yapılandırma

Dizin oluşturucu, dizin oluşturma işlemini destekleyen, yüksek düzeyli bir kaynaktır. Veri kaynağı adı, hedef dizin ve yürütme sıklığını belirtir. **Verileri Içeri aktarma** Sihirbazı birçok nesne oluşturur ve bu her zaman sürekli çalıştırabileceğiniz bir dizin oluşturucudır.

1. **Dizin Oluşturucu** sayfasında, varsayılan adı kabul edebilir ve hemen çalıştırmak Için **bir kez** zamanlama seçeneğine tıklayabilirsiniz. 

   ![Dizin oluşturucu tanımı](media/cognitive-search-quickstart-blob/indexer-def.png)

1. Dizin oluşturucuyu oluşturmak ve aynı anda çalıştırmak için **Gönder** ' e tıklayın.

## <a name="monitor-status"></a>İzleme durumu

Bilişsel yetenekler dizin oluşturma işlemi, genellikle OCR ve resim analizinden daha uzun sürer. İlerlemeyi izlemek için genel bakış sayfasına gidin ve sayfanın ortasında **Dizin oluşturucular** ' ye tıklayın.

  ![Azure Bilişsel Arama bildirimi](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Uyarılar, çok çeşitli içerik türleri için normaldir. Bazı içerik türleri bazı beceriler ve alt katmanlarda, [Dizin Oluşturucu limitleriyle](search-limits-quotas-capacity.md#indexer-limits)ilgili yaygın olarak geçerli değildir. Örneğin, 32.000 karakterlik kesme bildirimleri, ücretsiz katmanda bir Dizin Oluşturucu sınırdır. Bu tanıtımı daha yüksek bir katmanda çalıştırdıysanız birçok kesme uyarısı kaybolur.

Uyarıları veya hataları denetlemek için, Dizin oluşturucular listesindeki uyarı durumuna tıklayarak yürütme geçmişi sayfasını açın.

Bu sayfada, aşağıda gösterilene benzer uyarıların listesini görüntülemek için uyarı durumu ' na tıklayın. 

  ![Dizin Oluşturucu uyarı listesi](./media/cognitive-search-quickstart-blob/indexer-warnings.png)

Belirli bir durum satırına tıkladığınızda ayrıntılar görüntülenir. Bu uyarı, birleştirmenin en büyük eşiğe ulaştıktan sonra durdurulduğunu belirtir (Bu PDF büyük olur).

  ![Uyarı ayrıntıları](./media/cognitive-search-quickstart-blob/warning-detail.png)

## <a name="query-in-search-explorer"></a>Arama gezgininde sorgulama

Bir dizin oluşturulduktan sonra, sonuçları döndürmek için sorguları çalıştırabilirsiniz. Portalda bu görev için **Arama Gezgini** ' ni kullanın. 

1. Arama hizmeti panosu sayfasında, komut çubuğunda **Arama Gezgini** ' ne tıklayın.

1. Oluşturduğunuz dizini seçmek için üst kısımdaki **Dizini değiştir**'e tıklayın.

1. Dizini sorgulamak için bir arama dizesi girin, örneğin `search=Microsoft&$select=people,organizations,locations,imageTags`.

Sonuçlar, özellikle de Azure Bloblarından kaynaklanan büyük belgelerde ayrıntılı ve okunabilir olabilecek JSON olarak döndürülür. Bu araçta arama için bazı ipuçları aşağıdaki teknikleri içerir:

+ Sonuçlara `$select` hangi alanların ekleneceğini belirtmek için Ekle. 
+ Belirli özellikler veya terimler için JSON içinde arama yapmak için CTRL-F kullanın.

Sorgu dizeleri büyük/küçük harfe duyarlıdır; bu nedenle, bir "bilinmeyen alan" iletisi alırsanız, ad ve durumu doğrulamak için **alanları** veya **DIZIN tanımını (JSON)** denetleyin. 

  ![Arama gezgini örneği](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Paketler

Şimdi ilk beceri oluşturdunuz ve kendi verilerinizi kullanarak zenginleştirilmiş bir arama çözümünü prototip oluşturma konusunda yararlı olan öğrendiğimiz önemli kavramlar.

Topladığınız bazı temel kavramlar, Azure veri kaynaklarına bağımlılık içerir. Bir beceri bir dizin oluşturucuya bağlanır ve Dizin oluşturucular Azure ve kaynağa özgüdür. Bu hızlı başlangıçta Azure Blob depolama kullanılsa da, başka Azure veri kaynakları da mümkündür. Daha fazla bilgi için bkz. [Azure bilişsel arama 'Da Dizin oluşturucular](search-indexer-overview.md). 

Diğer önemli bir kavram, yeteneklerin içerik türlerini üzerinde çalışması ve heterojen içerikle çalışırken bazı girişler atlanacak. Ayrıca, büyük dosyalar veya alanlar hizmet katmanınızın Dizin Oluşturucu sınırlarını aşabilir. Bu olaylar gerçekleştiğinde uyarıları görmek normaldir. 

Çıktı bir arama dizinine yönlendirilir ve dizininizdeki tek tek alanlar sırasında oluşturulan ad-değer çiftleri arasında bir eşleme vardır. Dahili olarak portal, [ek açıklamalar](cognitive-search-concept-annotations-syntax.md) ayarlar ve işlemlerin sırasını ve genel akışı oluşturan bir [beceri kümesini](cognitive-search-defining-skillset.md) tanımlar. Bu adımlar portalda gizlenir, ancak kod yazmaya başladığınızda bu kavramlar önemli hale gelir.

Son olarak, dizini sorgulayarak içeriği doğrulayabildiğini öğrendiniz. Son olarak, Azure Bilişsel Arama 'in sağladığı, [basit](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) veya [tam genişletilmiş sorgu söz dizimini](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)kullanarak sorgulayabilmeniz için aranabilir bir dizindir. Zenginleştirilmiş alanlar içeren bir dizin de diğerlerine benzer. Standart veya [özel çözümleyiciler](search-analyzers.md), [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [eş anlamlı](search-synonyms.md), çok [yönlü filtreler](search-filters-facets.md), coğrafi arama veya diğer Azure bilişsel arama özelliklerini birleştirmek istiyorsanız, kesinlikle bunu yapabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Portal, .NET SDK veya REST API kullanarak becerileri oluşturabilirsiniz. Daha ayrıntılı bilgi için Postman ve daha fazla örnek veri kullanarak REST API deneyin.

> [!div class="nextstepaction"]
> [Öğretici: REST API 'Leri kullanarak JSON Bloblarından metin ve yapı ayıklama](cognitive-search-tutorial-blob.md)

> [!Tip]
> Bu alıştırmayı yinelemek veya farklı bir AI zenginleştirme Kılavuzu denemek istiyorsanız, portaldaki Dizin oluşturucuyu silin. Dizin oluşturucunun silinmesi, bilişsel hizmetler işleme için ücretsiz günlük işlem sayacını yeniden sıfır olarak sıfırlar.