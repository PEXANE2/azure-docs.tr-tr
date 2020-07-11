---
title: Bilgi deposu kavramları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebileceğiniz, yeniden şekillendirbileceğiniz ve kullanabileceğiniz Azure depolama 'ya zenginleştirilmiş belgeler gönderin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 78a8e0a46fd60f14ea3bae7485c737aa4fe3c60e
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86230783"
---
# <a name="knowledge-store-in-azure-cognitive-search"></a>Azure Bilişsel Arama bilgi deposu

Bilgi deposu, bağımsız analiz veya aşağı akış işleme için bir [AI zenginleştirme ardışık düzeninde](cognitive-search-concept-intro.md) çıkışın devam eden bir Azure bilişsel arama özelliğidir. *Zenginleştirilmiş bir belge* , yapay, yapılandırılmış ve AI süreçlerini kullanılarak çözümlenen içerikten oluşturulan bir ardışık düzen çıktıdır. Standart bir AI ardışık düzeninde, zenginleştirilmiş belgeler yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu oluşturmayı seçmek, zenginleştirilmiş belgeleri korumanıza olanak sağlar. 

Daha önce bilişsel becerileri kullandıysanız, *becerileri* bir belgeyi bir dizi enzenginleştirme aracılığıyla taşıyabileceğinizi zaten anlarsınız. Sonuç, bir arama dizini veya bir bilgi deposundaki projeksiyonlar olabilir. İki çıkış, arama dizini ve bilgi deposu, aynı işlem hattının çarpımlarından oluşur; aynı girdilerden türetilmiş, ancak aynı şekilde yapılandırılmış, depolanmış ve çok farklı yollarla kullanılan çıktının sonucu.

Azure [depolama](https://docs.microsoft.com/azure/storage/common/storage-account-overview), Azure Tablo depolama, Azure Blob depolama ya da her ikisi de fiziksel olarak bir bilgi deposu. Azure depolama 'ya bağlanabilecek herhangi bir araç veya işlem bilgi deposunun içeriğini kullanabilir.


> [!VIDEO https://www.youtube.com/embed/XWzLBP8iWqg?version=3&start=235&end=426]


![Ardışık düzen diyagramında bilgi deposu](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Ardışık düzen diyagramında bilgi deposu")

## <a name="benefits-of-knowledge-store"></a>Bilgi deposunun avantajları

Bilgi deposu, yerleşik olarak yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından, uygulanan Analize sahip görüntü dosyalarında veya hatta yapılandırılmış veriler, yeni formlara yeniden şekillendirilecek şekilde yapı, bağlam ve gerçek içerik sağlar. [Adım adım](knowledge-store-create-rest.md)bir kılavuzda, en çok yoğun bir JSON belgesinin alt yapılara nasıl bölümlendiğini, yeni yapılara reconstituted ve makine öğrenimi ve veri bilimi iş yükleri gibi aşağı akış işlemlerinde nasıl kullanılabilir yapıldığını görebilirsiniz.

Bir AI zenginleştirme ardışık düzeninin ne işe yarayabileceği görmek faydalı olsa da, bir bilgi deposunun gerçek potansiyeli verileri yeniden şekillendirmeye olanak tanır. Temel bir beceri ile başlayabilir ve daha sonra yeni yapılar halinde birleştirebileceğiniz, daha sonra Azure Bilişsel Arama yanı sıra diğer uygulamalarda kullanabileceğiniz daha fazla yapı düzeyleri eklemek için bunu yineleyebilirsiniz.

Numaralandırılmış, bilgi deposunun avantajları şunları içerir:

+ Arama dışındaki [analiz ve raporlama araçlarında](#tools-and-apps) zenginleştirilmiş belgeler kullanın. Power Query ile Power BI etkileyici bir seçimdir, ancak Azure depolama 'ya bağlanabilecek herhangi bir araç veya uygulama, oluşturduğunuz bir bilgi deposundan çekebilirsiniz.

+ Adımlarda ve beceri tanımlarında hata ayıklama sırasında bir AI dizin oluşturma işlem hattını daraltın. Bilgi deposu, bir AI dizin oluşturma işlem hattında bir beceri tanımının çarpımını gösterir. Bu sonuçları daha iyi bir beceri tasarlamak için kullanabilirsiniz, çünkü tam olarak neyin nasıl göründüğünü görebilirsiniz. Bir bilgi deposunun içeriğini görüntülemek için Azure Storage 'da [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanabilirsiniz.

+ Verileri yeni formlara şekillendirin. Yeniden şekillendirme becerileri içinde ortaklaşa bulunur, ancak nokta bir beceri artık bu özelliği sağlayabiliriz. Azure Bilişsel Arama 'de [beceri başına mil](cognitive-search-skill-shaper.md) bu görevi kapsayacak şekilde genişletildi. Yeniden şekillendirme, ilişkileri korurken verilerin amaçlanan kullanımı ile hizalanan bir projeksiyon tanımlamanızı sağlar.

> [!Note]
> AI zenginleştirme ve bilişsel becerileri yenilikleri nelerdir? Azure Bilişsel Arama, görüntü dosyaları üzerinden optik karakter tanıma (OCR), metin dosyalarından varlık tanıma ve anahtar tümceciği ayıklama ve daha fazlasını kullanarak kaynak verileri ayıklamak ve zenginleştirmek için bilişsel hizmetler düzenleme ve dil özellikleriyle tümleşir. Daha fazla bilgi için bkz. [Azure bilişsel arama 'de AI zenginleştirme](cognitive-search-concept-intro.md).

## <a name="physical-storage"></a>Fiziksel depolama alanı


> [!VIDEO https://www.youtube.com/embed/XWzLBP8iWqg?version=3&start=455&end=542]


Bilgi deposunun fiziksel ifadesi, `projections` beceri içindeki bir tanımın öğesi aracılığıyla ifade edilir `knowledgeStore` . Projeksiyon, bir çıktının yapısını, amaçlanan kullanım ile eşleşecek şekilde tanımlar.

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

Bu yapıda belirttiğiniz projeksiyonun türü, bilgi deposu tarafından kullanılan depolama türünü belirler.

+ Tanımladığınız sırada tablo depolaması kullanılır `tables` . Analitik araçlara giriş veya diğer veri depolarına veri çerçeveleri olarak dışarı aktarma için tablo Raporlama yapılarına ihtiyacınız olduğunda tablo projeksiyonu tanımlayın. `tables`Zenginleştirilmiş belgelerin alt kümesini veya çapraz bölümünü almak için birden çok belirtebilirsiniz. Aynı projeksiyon grubu içinde, tüm bunlarla çalışabilmeniz için tablo ilişkileri korunur.

+ Veya tanımladığınızda BLOB depolama kullanılır `objects` `files` . Öğesinin fiziksel temsili, `object` zenginleştirilmiş bir belgeyi temsil eden hiyerarşik BIR JSON yapısıdır. `file`, Bir belgeden ayıklanan bir görüntüdür ve BLOB depolama alanına bozulmadan aktarılmıştır.

Tek bir projeksiyon nesnesi bir,, ve kümesi içerir `tables` `objects` `files` ve birçok senaryo için, bir projeksiyon oluşturulması yeterli olabilir. 

Ancak, birden çok `table` - `object` - `file` projeksiyonun kümesi oluşturmak mümkündür ve farklı veri ilişkileri istiyorsanız bunu yapabilirsiniz. Bir küme içinde, bu ilişkilerin mevcut olduğu ve algılanabilmesi durumunda veriler birbiriyle ilgilidir. Ek kümeler oluşturursanız, her bir gruptaki belgeler hiçbir şekilde ilgili değildir. Birden çok projeksiyon grubu kullanmanın bir örneği, aynı verilerin bir çevrimiçi sisteminizle birlikte kullanılması ve belirli bir şekilde temsil edilebilmesi için aynı verilerin de aynı şekilde temsil edildiği bir veri bilimi ardışık düzeninde kullanılmak üzere de olmasını ister.

## <a name="requirements"></a>Gereksinimler 

[Azure depolama alanı](https://docs.microsoft.com/azure/storage/) gereklidir. Fiziksel depolama alanı sağlar. BLOB depolama, tablo depolama veya her ikisini de kullanabilirsiniz. BLOB depolama, genellikle çıkış aşağı akış işlemlerine giderken, genellikle zenginleştirilmiş belgeler için kullanılır. Tablo depolama, genellikle analiz ve raporlama için kullanılan, zenginleştirilmiş belgelerin dilimlerine yöneliktir.

[Beceri](cognitive-search-working-with-skillsets.md) gereklidir. `knowledgeStore`Tanımı içerir ve zenginleştirilmiş bir belgenin yapısını ve birleşimini belirler. Boş bir beceri kullanarak bilgi deposu oluşturamazsınız. Beceri içinde en az bir beceriye sahip olmanız gerekir.

[Dizin Oluşturucu](search-indexer-overview.md) gereklidir. Bir beceri, yürütmeyi yönlendiren bir Dizin Oluşturucu tarafından çağrılır. Dizin oluşturucular kendi gereksinimler ve öznitelikleri kümesiyle gelir. Bu özniteliklerin birkaçı bir bilgi deposunda doğrudan bir pul alır:

+ Dizin oluşturucular [desteklenen bir Azure veri kaynağı](search-indexer-overview.md#supported-data-sources) gerektirir (Azure 'da desteklenen bir kaynaktan veri çekilerek bilgi deposunu oluşturan işlem hattı başlatılır). 

+ Dizin oluşturucular bir arama dizini gerektirir. Bir Dizin Oluşturucu, hiç bir şekilde kullanmayı planlamıyorsanız bile bir dizin şeması sağlamanızı gerektirir. En az dizinde anahtar olarak atanan bir dize alanı bulunur.

+ Dizin oluşturucular, hedef alana kaynak alanı diğer adı için kullanılan isteğe bağlı alan eşlemeleri sağlar. Bir varsayılan alan eşlemesinin değiştirilmesine ihtiyacı varsa (farklı bir ad veya tür kullanmak için), bir dizin oluşturucu içinde [alan eşlemesi](search-indexer-field-mappings.md) oluşturabilirsiniz. Bilgi deposu çıktısı için, hedef bir blob nesnesi veya tablosunda bir alan olabilir.

+ Dizin oluşturucular zamanlamalar ve çeşitli veri kaynakları tarafından sunulan değişiklik algılama mekanizmaları gibi diğer özellikleri de bir bilgi deposuna uygulanabilir. Örneğin, içeriği yenilemek için düzenli aralıklarla zenginleştirme [zamanlayabilirsiniz](search-howto-schedule-indexers.md) . 

## <a name="how-to-create-a-knowledge-store"></a>Bilgi deposu oluşturma

Bilgi deposu oluşturmak için portalı veya REST API ( `api-version=2020-06-30` ) kullanın.

### <a name="use-the-azure-portal"></a>Azure portalını kullanma

**Veri Içeri aktarma** Sihirbazı, bilgi deposu oluşturma seçeneklerini içerir. İlk araştırma için, [ilk bilgi deponuzu dört adımda oluşturun](knowledge-store-connect-power-bi.md).

1. Desteklenen bir veri kaynağı seçin.

1. Zenginleştirme belirtin: kaynak iliştirme, beceriler seçme ve bir bilgi deposu belirtme. 

1. Dizin şeması oluşturun. Sihirbaz için gereklidir ve bir tane olabilir.

1. Sihirbazı çalıştırın. Ayıklama, zenginleştirme ve depolama bu son adımda oluşur.

### <a name="use-create-skillset-rest-api"></a>Create beceri kullanın (REST API)]

Bir `knowledgeStore` [beceri](cognitive-search-working-with-skillsets.md)içinde tanımlanır, bu, sırasıyla bir [Dizin Oluşturucu](search-indexer-overview.md)tarafından çağırılır. Zenginleştirme sırasında Azure Bilişsel Arama, Azure depolama hesabınızda bir alan oluşturur ve yapılandırmanıza bağlı olarak, belgeleri Bloblar veya tablolar olarak zenginleştirir.

REST API, programlama yoluyla bir bilgi deposu oluşturabileceğiniz bir mekanizmadır. Araştırmanın kolay bir yolu, [Postman ve REST API kullanarak ilk bilgi deponuzi oluşturmaktır](knowledge-store-create-rest.md).

<a name="tools-and-apps"></a>

## <a name="how-to-connect-with-tools-and-apps"></a>Araçlar ve uygulamalar ile bağlanma

Depolarda enzenginler varsa, Azure Blob veya tablo depolama 'ya bağlanan herhangi bir araç veya teknoloji, içeriği incelemek, çözümlemek veya kullanmak için kullanılabilir. Aşağıdaki liste bir başlangıç:

+ Zenginleştirilmiş belge yapısını ve içeriğini görüntülemek için [Depolama Gezgini](knowledge-store-view-storage-explorer.md) . Bilgi deposu içeriğini görüntülemek için bunu taban çizgisi aracınız olarak düşünün.

+ Raporlama ve analiz için [Power BI](knowledge-store-connect-power-bi.md) . 

+ Daha fazla düzenleme için [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) .

<a name="kstore-rest-api"></a>

## <a name="api-reference"></a>API başvurusu

REST API sürüm `2020-06-30` , becerileri üzerinde ek tanımlar aracılığıyla bilgi deposu sağlar. Başvuruya ek olarak, API 'Leri çağırma hakkında ayrıntılı bilgi için bkz. [Postman kullanarak bilgi deposu oluşturma](knowledge-store-create-rest.md) .

+ [Beceri oluşturma (api-Version = 2020-06-30)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Güncelleştirme beceri (api-Version = 2020-06-30)](https://docs.microsoft.com/rest/api/searchservice/update-skillset)


## <a name="next-steps"></a>Sonraki adımlar

Bilgi deposu, bir Azure depolama hesabına erişim özellikli herhangi bir istemci uygulaması tarafından tüketimine yönelik bir beceri tasarımı veya yeni yapıların oluşturulması için yararlı olan belgelerin kalıcılığını, yararlı bir şekilde veya yeni yapıların oluşturulmasını sağlar.

Zenginleştirilmiş belgeler oluşturmak için en basit yaklaşım [Portal üzerinden](knowledge-store-create-portal.md)yapılır, ancak nesnelerin nasıl oluşturulduğuna ve başvurulduğunu öğrenmek isterseniz daha kullanışlı olan Postman ve REST API de kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Postman ve REST kullanarak bilgi deposu oluşturma](knowledge-store-create-rest.md)

Tahminler hakkında daha fazla bilgi edinmek için, bu özellikleri ve [beceri nasıl tanımlacağınızı](knowledge-store-projection-overview.md) öğrenin

> [!div class="nextstepaction"]
> [Bilgi deposundaki projeksiyonler](knowledge-store-projection-overview.md)

Dilimleme, satır içi şekillendirme ve ilişkiler gibi gelişmiş projeksiyonun kavramlarını kapsayan bir öğreticide, [bir bilgi deposunda projeksiyonları tanımlamaya](knowledge-store-projections-examples.md) başlayın

> [!div class="nextstepaction"]
> [Bir bilgi deposunda projeksiyonlarını tanımlama](knowledge-store-projections-examples.md)