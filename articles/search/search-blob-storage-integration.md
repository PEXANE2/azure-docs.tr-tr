---
title: Azure Blob Depolamasına tam metin arama ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel earch'ta tam metin arama dizini yaparken içeriği ayıklayın ve Azure bloblarına yapı ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: af7d04bd74ada296b9f0e0f7c149c2a781cec579
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73496487"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-cognitive-search"></a>Azure Bilişsel Arama'yı kullanarak Azure blob verilerine tam metin arama ekleme

Azure Blob depolama alanında depolanan çeşitli içerik türlerinde arama yapmak zor bir sorun olabilir. Ancak, Azure Bilişsel Arama'yı kullanarak Blob'larınızın içeriğini yalnızca birkaç tıklamayla [dizine](search-what-is-azure-search.md)ekleyebilir ve arama yapabilirsiniz. Azure Bilişsel Arama, dizine veri kaynağına duyarlı özellikler ekleyen bir [*Blob dizinleyicisi*](search-howto-indexing-azure-blob-storage.md) aracılığıyla Blob depolamasının dizinlerini dizine ekleyen yerleşik tümleştirmeye sahiptir.

## <a name="what-it-means-to-add-full-text-search-to-blob-data"></a>Blob verilerine tam metin araması eklemenin anlamı

Azure Bilişsel Arama, arama hizmetinizde barındırılan kullanıcı tanımlı dizinler üzerinden çalışan dizin oluşturma ve sorgu motorları sağlayan bir bulut arama hizmetidir. Aranabilir içeriğinizi buluttaki sorgu motoruyla birlikte bulmak performans için gereklidir ve kullanıcıların arama sorgularından beklediği bir hızda sonuçları döndürün.

Azure Bilişsel Arama, dizin oluşturma katmanındaki Azure Blob depolamasıyla tümleşir ve blob içeriğinizi *ters dizinlere* ve serbest biçim metin sorgularını ve filtre ifadelerini destekleyen diğer sorgu yapılarına dizine eklenmiş arama belgeleri olarak içe aktarır. Blob içeriğiniz bir arama dizinine dizine eklenmiş olduğundan, blob içeriğine erişim Azure Bilişsel Arama'daki tüm sorgu özelliklerinden yararlanabilir.

Dizin oluşturulduktan ve doldurulduktan sonra, blob kapsayıcınızdan bağımsız olarak var olur, ancak dizininizi temel kapsayıcıdaki değişikliklerle yenilemek için dizinoluşturma işlemlerini yeniden yapabilirsiniz. Tek tek lekeler üzerinde zaman damgası bilgileri değişiklik algılama için kullanılır. Yenileme mekanizması olarak zamanlanmış yürütmeyi veya isteğe bağlı dizini seçmeyi tercih edebilirsiniz.

Girişler, Azure Blob depolama alanında tek bir kapta lekelerinizledir. Blobs metin veri hemen hemen her türlü olabilir. Lekelerin izni görüntüler içeriyorsa, resimlerden metin oluşturmak ve ayıklamak [için blob dizinoluşturmaya AI zenginleştirme](search-blob-ai-integration.md) ekleyebilirsiniz.

Çıktı, istemci uygulamalarında hızlı metin arama, alma ve arama için kullanılan bir Azure Bilişsel Arama dizinidir. Arasında dizin leme boru hattı mimarisinin kendisidir. Boru hattı, bu makalede daha fazla tartışılan *dizinleyici* özelliğine dayanır.

## <a name="start-with-services"></a>Hizmetlerle başlayın

Azure Bilişsel Arama ve Azure Blob depolama alanına ihtiyacınız var. Blob depolama alanı içinde, kaynak içeriği sağlayan bir kapsayıcı gerekir.

Doğrudan Depolama hesabı portalı sayfanızda başlayabilirsiniz. Sol daki gezinti sayfasında, **Blob hizmetinin** altında yeni bir hizmet oluşturmak veya varolan bir hizmet seçmek için **Azure Bilişsel Arama** Ekle'yi tıklatın. 

Depolama hesabınıza Azure Bilişsel Arama'yı ekledikten sonra, blob verilerini dizine eklemek için standart işlemi izleyebilirsiniz. Kolay bir başlangıç için Azure Bilişsel Arama'da **Veri Alma** sihirbazını öneririz veya Postacı gibi bir aracı kullanarak REST API'lerini arayın. Bu öğretici, Postacı'da REST API'yi arama adımlarında size yol sunar: [Azure Bilişsel Arama'da yarı yapılandırılmış verileri (JSON blobs) dizinve arama.](search-semi-structured-data.md) 

## <a name="use-a-blob-indexer"></a>Blob dizinleyici sindin

*Dizinleyici,* verileri örnekleme, meta veri verilerini okuma, veri alma ve yerel biçimlerden verileri sonraki alma için JSON belgelerine serileştirme için dahili mantıkla donatılmış veri kaynağına duyarlı bir alt hizmettir. 

Azure Depolama'daki blob'lar [Azure Bilişsel Arama Blob depolama dizinleyicisi](search-howto-indexing-azure-blob-storage.md)kullanılarak dizine eklenir. Bu dizin leyiciyi **Alma veri** sihirbazı, BIR REST API veya .NET SDK'yı kullanarak çağırabilirsiniz. Kodolarak, bu dizin leyiciyi türü ayarlayarak ve bir Azure Depolama hesabı içeren bağlantı bilgilerini blob kapsayıcısıyla birlikte sağlayarak kullanırsınız. Bloblarınızı, parametre olarak geçirebileceğiniz sanal bir dizin oluşturarak veya dosya türü uzantısına filtre uygulayarak alt kümeleyebilirsiniz.

Dizinleyici,içeriği incelemek için bir leke açarak "belge çatlamasını" yapar. Veri kaynağına bağlandıktan sonra, bu boru hattının ilk adımıdır. Blob verileri için PDF, office dokümanları ve diğer içerik türleri burada algılanır. Metin çıkarma ile belge çatlama sı ücretsizdir. Lekeleringörüntü içeriği içeriyorsa, [AI zenginleştirme eklemediğiniz](search-blob-ai-integration.md)sürece görüntüler yok sayılır. Standart dizin oluşturma yalnızca metin içeriği için geçerlidir.

Blob dizinleyicisi yapılandırma parametreleriyle birlikte gelir ve temel veriler yeterli bilgi sağlarsa değişiklik izlemeyi destekler. [Azure Bilişsel Arama Blob depolama dizinleyicisinde](search-howto-indexing-azure-blob-storage.md)temel işlevsellik hakkında daha fazla bilgi edinebilirsiniz.

### <a name="supported-content-types"></a>Desteklenen içerik türleri

Bir kapsayıcı üzerinde blob dizinleyici çalıştırarak, tek bir sorgu ile aşağıdaki içerik türlerinden metin ve meta veri ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

### <a name="indexing-blob-metadata"></a>Blob meta verilerini dizine ekinleme

Herhangi bir içerik türülekeleri arasında sıralamayı kolaylaştıran yaygın bir senaryo, her bir blob için hem özel meta verileri hem de sistem özelliklerini dizine dizine getirmektir. Bu şekilde, tüm blobs için bilgiler, arama hizmetinizde bir dizin saklanan belge türü ne olursa olsun dizine dizinlenir. Yeni dizini nizi kullanarak, tüm Blob depolama içeriğini sıralamaya, filtrelemeye ve yönünü kullanmaya devam edebilirsiniz.

### <a name="indexing-json-blobs"></a>Dizin oluşturma JSON lekeleri
Dizin leyiciler, JSON içeren lekelerde bulunan yapılandırılmış içeriği ayıklamak için yapılandırılabilir. Bir dizinleyici JSON blobs okuyabilir ve yapılandırılmış içeriği bir arama belgesinin uygun alanlarına ayrıştırabilir. Dizin oluşturicular ayrıca bir dizi JSON nesnesi içeren lekeleri alabilir ve her öğeyi ayrı bir arama belgesiyle eşleyebilir. Dizinleyici tarafından oluşturulan JSON nesnesinin türünü etkileyecek bir ayrışma modu ayarlayabilirsiniz.

## <a name="search-blob-content-in-a-search-index"></a>Arama dizinindeki blob içeriğini arama 

Dizin oluşturma çıktısı, istemci uygulamasında boş metin ve filtrelenmiş sorgular kullanılarak etkileşimli arama için kullanılan bir arama dizinidir. İçeriğin ilk araştırılması ve doğrulanması için, belge yapısını incelemek için portaldaki [Arama Gezgini](search-explorer.md) ile başlayarak öneririz. [Arama gezgininde basit sorgu sözdizimi,](query-simple-syntax.md) [tam sorgu sözdizimi](query-lucene-syntax.md)ve filtre ifade [sözdizimini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz.

Daha kalıcı bir çözüm sorgu girişleri toplamak ve bir istemci uygulamasında arama sonuçları olarak yanıt sunmaktır. Aşağıdaki C# öğreticisi, bir arama uygulamasının nasıl oluşturulacağını açıkça açıklar: [Azure Bilişsel Arama' da ilk uygulamanızı oluÅ](tutorial-csharp-create-first-app.md)turun.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure portalı (Azure Blob depolama) ile blob'ları yükleme, indirme ve listele](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
+ [Blob dizinleyici (Azure Bilişsel Arama) ayarlama](search-howto-indexing-azure-blob-storage.md) 
