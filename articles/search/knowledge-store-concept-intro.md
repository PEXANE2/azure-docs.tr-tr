---
title: Bilgi deposuna giriş (önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebileceğiniz, yeniden şekillendirebileceğiniz ve tüketebileceğiniz zenginleştirilmiş belgeleri Azure Depolama'ya gönderin. Bu özellik genel önizleme aşamasındadır.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 0ad780c04954c09ddfd432b3c7de3dc65f0841bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942993"
---
# <a name="introduction-to-knowledge-stores-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da bilgi depolarına giriş

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Bilgi deposu, bağımsız analiz veya akış aşağı işleme için bir [AI zenginleştirme ardışık boru hattından](cognitive-search-concept-intro.md) çıktıyı devam eden Azure Bilişsel Arama'nın bir özelliğidir. *Zenginleştirilmiş belge,* AI işlemleri kullanılarak ayıklanmış, yapılandırılan ve analiz edilen içerikten oluşturulan bir boru hattı çıktısI. Standart bir AI ardışık boru hattında, zenginleştirilmiş belgeler geçicidir, yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile zenginleştirilmiş belgeler korunur. 

Geçmişte bilişsel becerileri kullandıysanız, becerilerin bir *belgeyi* bir dizi zenginleştirme yoluyla hareket ettirdiğini zaten biliyorsunuz. Sonuç bir arama dizini veya (bu önizlemede yeni) bir bilgi deposunda projeksiyonlar olabilir. Arama dizini ve bilgi deposu olan iki çıkış aynı boru hattının ürünleridir; aynı girdilerden elde edilir, ancak yapılandırılmış, depolanmış ve çok farklı şekillerde kullanılan çıktı ile sonuçlanır.

Fiziksel olarak, bir bilgi deposu [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-account-overview), Azure Tablo depolama, Azure Blob depolama, ya da her ikisi de. Azure Depolama'ya bağlanabilen herhangi bir araç veya işlem, bir bilgi deposunun içeriğini tüketebilir.

![Boru hattı diyagramında bilgi deposu](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Boru hattı diyagramında bilgi deposu")

## <a name="benefits-of-knowledge-store"></a>Bilgi deposunun faydaları

Bir bilgi deposu size yapı, bağlam ve gerçek içerik sağlar - blobs gibi yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından toplanan, analiz edilmiş görüntü dosyaları, hatta yapılandırılmış veri, yeni formlar halinde yeniden şekillendirilmiş. Adım [adım bir gözden geçirmede,](knowledge-store-create-rest.md)yoğun bir JSON belgesinin alt yapılara nasıl bölündüğünü, yeni yapılara nasıl dönüştürüldüğünü ve makine öğrenimi ve veri bilimi iş yükleri gibi alt akış işlemleri için nasıl kullanıma sunulduğuni ilk elden görebilirsiniz.

Bir AI zenginleştirme ardışık boru hattının neler üretebileceğini görmek yararlı olsa da, bir bilgi deposunun gerçek potansiyeli verileri yeniden şekillendirme yeteneğidir. Temel bir beceri yle başlayabilir ve daha sonra artan yapı düzeyleri eklemek için üzerine tekrarlayabilir, bu da azure Bilişsel Arama'nın yanı sıra diğer uygulamalarda da tüketilen yeni yapılarda birleştirebilir.

Numaralandırılmış, bilgi deposunun yararları şunlardır:

+ Zenginleştirilmiş belgeleri arama dışındaki [analiz ve raporlama araçlarında](#tools-and-apps) tüketin. Power Query ile Power BI ilgi çekici bir seçimdir, ancak Azure Depolama'ya bağlanabilen herhangi bir araç veya uygulama oluşturduğunuz bir bilgi deposundan çekebilir.

+ Adımları ve skillset tanımlarını hata ayıklarken bir AI dizine alma ardışık hattını hassaslaştırın. Bir bilgi deposu, bir AI indeksleme ardışık boru hattında bir skillset tanımının ürün gösterir. Zenginleştirmelerin tam olarak neye benzediğini görebildiğiniz için bu sonuçları daha iyi bir beceri tasarlamak için kullanabilirsiniz. Bir bilgi deposunun içeriğini görüntülemek için Azure Depolama'daki [Depolama Gezgini'ni](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanabilirsiniz.

+ Verileri yeni formlara dönüştürün. Yeniden şekillendirme beceriler kodlanmış, ama nokta bir skillset artık bu yeteneği sağlayabilir olmasıdır. Azure Bilişsel Arama'daki [Şekillendirici becerisi](cognitive-search-skill-shaper.md) bu görevi yerine getirmek için genişletildi. Yeniden şekillendirme, ilişkileri korurken verileri istediğiniz şekilde kullanan bir projeksiyon tanımlamanıza olanak tanır.

> [!Note]
> Yeni AI zenginleştirme ve bilişsel becerileri? Azure Bilişsel Arama, görüntü dosyaları üzerinden Optik Karakter Tanıma (OCR) kullanarak kaynak verilerini ayıklamak ve zenginleştirmek, metin dosyalarından varlık tanıma ve anahtar ifade çıkarma ve daha fazlasını yapmak için Bilişsel Hizmetler Vizyonu ve Dil özellikleriyle bütünleşir. Daha fazla bilgi için [Azure Bilişsel Arama'da AI zenginleştirme](cognitive-search-concept-intro.md)bilgisine bakın.

## <a name="physical-storage"></a>Fiziksel depolama

Bir bilgi deposunun fiziksel ifadesi, `projections` Skillset'teki bir `knowledgeStore` tanım öğesi aracılığıyla ifade edilir. Projeksiyon, çıkışın bir yapısını, amaçlanan kullanımınızla eşleşebilecek şekilde tanımlar.

Projeksiyonlar tablo, nesne veya dosya olarak ifade edilebilir.

```json
"knowledgeStore": { 
    "storageConnectionString": "<YOUR-AZURE-STORAGE-ACCOUNT-CONNECTION-STRING>", 
    "projections": [ 
        { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        },
                { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        }
```

Bu yapıda belirttiğiniz projeksiyon türü, bilgi deposu tarafından kullanılan depolama türünü belirler.

+ Tablo depolama tanımlarken `tables`kullanılır. Analitik araçlara girişler için tablo raporlama yapılarına ihtiyaç duyduğunuzda veya veri çerçevesi olarak diğer veri depolarına dışa aktarmanız gerektiğinde tablo projeksiyonu tanımlayın. Zenginleştirilmiş belgelerin `tables` bir alt kümesini veya kesitini almak için birden çok belirtebilirsiniz. Aynı projeksiyon grubu içinde, tablo ilişkileri korunur, böylece hepsiyle çalışabilirsiniz.

+ Blob depolama tanımlarsanız `objects` kullanılır `files`veya . Bir `object` fiziksel gösterim, zenginleştirilmiş bir belgeyi temsil eden hiyerarşik bir JSON yapısıdır. A, `file` blob depolamasına bozulmamış olarak aktarılan bir belgeden çıkarılan bir görüntüdür.

Tek bir projeksiyon nesnesi, `tables`bir dizi içerir , `objects`, `files`, ve birçok senaryo için, bir projeksiyon oluşturma yeterli olabilir. 

Ancak, birden `table` - `object` - `file` çok projeksiyon kümesi oluşturmak mümkündür ve farklı veri ilişkileri istiyorsanız bunu yapabilirsiniz. Bir küme içinde, bu ilişkilerin var olduğunu ve algılanabileceğini varsayarak veriler ilişkilidir. Ek kümeler oluşturursanız, her gruptaki belgeler hiçbir zaman ilişkili değildir. Birden çok projeksiyon grubu kullanmanın bir örneği, aynı verilerin çevrimiçi sisteminizle kullanılmak üzere yansıtılmasını istiyorsanız ve belirli bir şekilde temsil edilmesi gerekiyorsa, aynı verilerin temsil edilen bir veri bilimi ardışık alanında kullanılmak üzere yansıtılmasını da isteyebilirsiniz. Farklı.

## <a name="requirements"></a>Gereksinimler 

[Azure Depolama](https://docs.microsoft.com/azure/storage/) gereklidir. Fiziksel depolama sağlar. Blob depolama, Tablo depolama veya her ikisini de kullanabilirsiniz. Blob depolama bozulmamış zenginleştirilmiş belgeler için kullanılır, genellikle çıktı aşağı akış işlemleri ne zaman. Tablo depolama, genellikle analiz ve raporlama için kullanılan zenginleştirilmiş belge dilimleri içindir.

[Skillset](cognitive-search-working-with-skillsets.md) gereklidir. Tanımını `knowledgeStore` içerir ve zenginleştirilmiş bir belgenin yapısını ve bileşimini belirler. Boş bir skillset kullanarak bir bilgi deposu oluşturamazsınız. Bir beceri en az bir beceri olmalıdır.

[Dizinleyici](search-indexer-overview.md) gereklidir. Bir beceri yürütme sürücüler bir dizinleyici tarafından çağrılır. Dizin leyiciler kendi gereksinimleri ve öznitelikleri ile birlikte gelir. Bu özelliklerin bazıları bir bilgi deposu üzerinde doğrudan bir etkiye sahiptir:

+ Dizin oluşturi ersiatörler desteklenen bir [Azure veri kaynağına](search-indexer-overview.md#supported-data-sources) (sonuçta bilgi deposunu oluşturan ardışık ardışık kaynak, Azure'da desteklenen bir kaynaktan veri çekerek başlar). 

+ Dizin leyiciler bir arama dizini gerektirir. Dizinleyici, kullanmayı hiç planlamasanız bile bir dizin şeması sağlamanızı gerektirir. En az dizin, anahtar olarak belirlenmiş bir dize alanı vardır.

+ Dizin leyiciler, hedef alana kaynak alan takma adıyla kullanılan isteğe bağlı alan eşlemeleri sağlar. Varsayılan alan eşlemenin değiştirilmesi gerekiyorsa (farklı bir ad veya tür kullanmak için), dizin oluşturucu içinde bir [alan eşleme](search-indexer-field-mappings.md) oluşturabilirsiniz. Bilgi deposu çıktısı için, hedef bir blob nesnesi veya tabloda bir alan olabilir.

+ Dizin leyicilerin zamanlamaları vardır ve çeşitli veri kaynakları tarafından sağlanan değişiklik algılama mekanizmaları gibi diğer özellikler de bir bilgi deposuna uygulanabilir. Örneğin, içeriği [schedule](search-howto-schedule-indexers.md) yenilemek için zenginleştirmeyi düzenli aralıklarla zamanlayabilirsiniz. 

## <a name="how-to-create-a-knowledge-store"></a>Bilgi deposu nasıl oluşturulur?

Bilgi deposu oluşturmak için, portalı veya önizleme`api-version=2019-05-06-Preview`REST API 'yi ( ).

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

**Veri Alma** sihirbazı, bilgi deposu oluşturmak için seçenekler içerir. İlk keşif için, [dört adımda ilk bilgi deposu oluşturun.](knowledge-store-connect-power-bi.md)

1. Desteklenen bir veri kaynağı seçin.

1. Zenginleştirme belirtin: bir kaynak eklemek, becerileri seçin ve bir bilgi deposu belirtin. 

1. Bir dizin şeması oluşturun. Sihirbaz bunu gerektirir ve sizin için bir çıkarabilirsiniz.

1. Büyücüyü çalıştırın. Çıkarma, zenginleştirme ve depolama bu son adımda gerçekleşir.

### <a name="use-create-skillset-and-the-preview-rest-api"></a>Create Skillset ve önizleme REST API'sini kullanın

A, `knowledgeStore` bir [dizin leyici](search-indexer-overview.md)tarafından çağrılan bir [skillset](cognitive-search-working-with-skillsets.md)içinde tanımlanır. Zenginleştirme sırasında Azure Bilişsel Arama, Azure Depolama hesabınızda bir alan oluşturur ve yapılandırmanıza bağlı olarak zenginleştirilmiş belgeleri blob seve veya tablolara dönüştürür.

Şu anda, önizleme REST API programlı bir bilgi deposu oluşturabilirsiniz tek mekanizmadır. Keşfetmek için kolay bir yolu [Postman ve REST API kullanarak ilk bilgi deposu oluşturmaktır.](knowledge-store-create-rest.md)

Bu önizleme özelliğiiçin başvuru içeriği bu makalenin [API başvuru](#kstore-rest-api) bölümünde yer almaktadır. 

<a name="tools-and-apps"></a>

## <a name="how-to-connect-with-tools-and-apps"></a>Araçlarve uygulamalarla nasıl bağlantı kurabilirsiniz?

Zenginleştirmeler depolama alanında var olduktan sonra, Azure Blob veya Tablo depolamasına bağlanan herhangi bir araç veya teknoloji içeriği keşfetmek, analiz etmek veya tüketmek için kullanılabilir. Aşağıdaki liste bir başlangıçtır:

+ Zenginleştirilmiş belge yapısını ve içeriğini görüntülemek için [Depolama Gezgini.](knowledge-store-view-storage-explorer.md) Bunu bilgi deposu içeriğini görüntülemek için temel aracınız olarak düşünün.

+ Raporlama ve analiz için [Güç BI.](knowledge-store-connect-power-bi.md) 

+ Daha fazla manipülasyon için [Azure Veri Fabrikası.](https://docs.microsoft.com/azure/data-factory/)

<a name="kstore-rest-api"></a>

## <a name="api-reference"></a>API başvurusu

REST API `2019-05-06-Preview` sürümü, beceriler üzerinde ek tanımlar aracılığıyla bilgi deposu sağlar. Başvuruya ek olarak, API'lerin nasıl çağrılması yla ilgili ayrıntılar için [Postacı'yı kullanarak bir bilgi deposu oluştur'a](knowledge-store-create-rest.md) bakın.

+ [Skillset Oluştur (api-version=2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/create-skillset) 
+ [Skillset'i Güncelleştir (api-version=2019-05-06-Önizleme)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/update-skillset) 


## <a name="next-steps"></a>Sonraki adımlar

Bilgi deposu, bir Azure Depolama hesabına erişebilen tüm istemci uygulamaları tarafından bir beceri yapısı tasarlarken yararlı olan zenginleştirilmiş belgelerin kalıcılığını veya yeni yapıların ve içeriğin oluşturulmasını sunar.

Zenginleştirilmiş belgeler oluşturmak için en basit yaklaşım [portal aracılığıyla,](knowledge-store-create-portal.md)ama aynı zamanda nesnelerin oluşturulur ve başvurulan nasıl içgörü istiyorsanız daha yararlı postacı ve REST API, kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Postacı ve REST kullanarak bir bilgi deposu oluşturun](knowledge-store-create-rest.md)

Projeksiyonlar, yetenekler ve bunları bir beceri de nasıl [tanımladığınız](knowledge-store-projection-overview.md) hakkında daha fazla bilgi edinmek için

> [!div class="nextstepaction"]
> [Bilgi deposundaki projeksiyonlar](knowledge-store-projection-overview.md)

Dilimleme, satır ara şekillendirme ve ilişkiler gibi gelişmiş projeksiyonkavramlarını kapsayan bir öğretici için, [bir bilgi deposundaki projeksiyonları tanımlamaile](knowledge-store-projections-examples.md) başlayın

> [!div class="nextstepaction"]
> [Projeksiyonları bilgi deposunda tanımlama](knowledge-store-projections-examples.md)