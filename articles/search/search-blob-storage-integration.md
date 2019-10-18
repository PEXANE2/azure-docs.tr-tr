---
title: Azure Blob depolamaya tam metin araması ekleme
titleSuffix: Azure Search
description: Azure Search ' de tam metin arama dizini oluştururken içeriği ayıklayın ve Azure bloblarına yapı ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: search
ms.topic: conceptual
ms.date: 10/09/2019
ms.openlocfilehash: db54c7886f5256dab41325cd12f1b893be732d72
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529037"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-search"></a>Azure Search kullanarak Azure Blob verilerine tam metin araması ekleme

Azure Blob depolamada depolanan çeşitli içerik türleri arasında arama yapmak zor bir sorun olabilir. Ancak, [Azure Search](search-what-is-azure-search.md)kullanarak bloblarınızın içeriğini yalnızca birkaç tıklamayla dizinleyerek ve arayabilirsiniz. Azure Search, dizin oluşturma için veri kaynağı kullanan yetenekleri ekleyen bir [*BLOB Indexer*](search-howto-indexing-azure-blob-storage.md) aracılığıyla blob depolamayı dizine alma için yerleşik tümleştirmeye sahiptir.

## <a name="what-it-means-to-add-full-text-search-to-blob-data"></a>Blob verilerine tam metin araması eklemenin anlamı

Azure Search, arama hizmetinizde barındırılan Kullanıcı tanımlı dizinlerin üzerinde çalışan dizin oluşturma ve sorgu motorları sağlayan bir bulut arama hizmetidir. Ara değerli içeriğinizi bulutta sorgu altyapısından birlikte bulmak, performans için gerekli olan sonuçları hızlı bir şekilde döndürmek için gereklidir.

Azure Search, dizin oluşturma katmanında Azure Blob depolama ile tümleşir ve bu, serbest biçimli metin sorgularını ve Filtre ifadelerini destekleyen, *ters dizinler* ve diğer sorgu yapılarına dizin oluşturulan arama belgeleri olarak içeri aktarın. Blob içeriğiniz bir arama dizininde dizinlendiği için, blob içeriğine erişim Azure Search içindeki sorgu özelliklerinin tam aralığından faydalanabilir.

Dizin oluşturulup doldurulduktan sonra, blob kapsayıcınızda bağımsız olarak bulunur, ancak dizin oluşturma işlemlerini yeniden çalıştırıp Dizin oluşturmayı temel kapsayıcıda yapılan değişikliklerle yenileyebilirsiniz. Tek bloblarda zaman damgası bilgileri değişiklik algılama için kullanılır. Yenileme mekanizması olarak zamanlanmış yürütme ya da isteğe bağlı dizin oluşturma için kabul edebilirsiniz.

Azure Blob depolama alanında, tek bir kapsayıcıda bulunan bloblarınızın girdileri vardır. Blob 'lar neredeyse her türlü metin verisi olabilir. Bloblarınız görüntü içeriyorsa, görüntülerden metin oluşturup çıkarmak için bir [BLOB dizin oluşturmaya AI zenginleştirme](search-blob-ai-integration.md) ekleyebilirsiniz.

Çıktı her zaman, istemci uygulamalarında hızlı metin arama, alma ve araştırma için kullanılan bir Azure Search dizinidir. Between, dizin oluşturma ardışık düzen mimarisidir. İşlem hattı, bu makalede daha fazla açıklanılan *Dizin Oluşturucu* özelliğine dayanır.

## <a name="start-with-services"></a>Hizmetlerle başlayın

Azure Search ve Azure Blob depolamaya ihtiyacınız vardır. BLOB depolama alanında, kaynak içerik sağlayan bir kapsayıcıya ihtiyacınız vardır.

Doğrudan depolama hesabı portalı sayfanızda başlayabilirsiniz. Sol Gezinti sayfasında, **BLOB hizmeti** altında **Azure Search Ekle** ' ye tıklayarak yeni bir hizmet oluşturun veya var olan bir hizmeti seçin. 

Depolama hesabınıza Azure Search ekledikten sonra, blob verilerini indekslemek için standart işlemi izleyebilirsiniz. Kolay bir başlangıç girişi için Azure Search **veri alma** Sihirbazı 'nı öneririz veya Postman gibi bir araç kullanarak REST API 'leri çağırabilirsiniz. Bu öğretici, Postman 'da REST API çağırma adımlarında size yol gösterir: [Azure Search yarı yapılandırılmış verileri (JSON Blobları) dizin ve arama](search-semi-structured-data.md). 

## <a name="use-a-blob-indexer"></a>Blob Dizin Oluşturucu kullanma

*Dizin Oluşturucu* , verileri örnekleme, meta veri verileri okuma, verileri alma ve yerel biçimlerdeki verileri, sonraki içeri aktarma için JSON belgelerine serileştirmede iç mantığa sahip olan veri kaynağı kullanan bir alt hizmettir. 

Azure depolama 'daki Bloblar [Azure Search blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)kullanılarak dizinlenir. **Veri alma** Sihirbazı 'nı, bir REST API veya .NET SDK 'sını kullanarak bu dizin oluşturucuyu çağırabilirsiniz. Kod içinde, bu dizin oluşturucuyu türü ayarlayarak ve bir blob kapsayıcısı ile birlikte bir Azure depolama hesabı içeren bağlantı bilgilerini sağlayarak kullanırsınız. Daha sonra bir parametre olarak geçirebilen veya bir dosya türü uzantısı üzerinde filtreleyerek sanal bir dizin oluşturarak bloblarınızı alt kümelayabilirsiniz.

Bir Dizin Oluşturucu, içeriği incelemek için bir blob açan "belgeyi çözme" yapmaz. Veri kaynağına bağlandıktan sonra, işlem hattının ilk adımı vardır. Blob verileri için PDF, Office belgeleri ve diğer içerik türlerinin algılandığı yerdir. Metin ayıklama ile belge çözme ücretsizdir. Bloblarınız görüntü içeriği içeriyorsa, [AI zenginleştirme](search-blob-ai-integration.md)eklemediğiniz takdirde görüntüler yok sayılır. Standart dizin oluşturma yalnızca metin içeriği için geçerlidir.

Blob Indexer yapılandırma parametreleriyle birlikte gelir ve temel alınan veriler yeterli bilgi sağlıyorsa değişiklik izlemeyi destekler. [Azure Search blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)'da çekirdek işlevsellik hakkında daha fazla bilgi edinebilirsiniz.

### <a name="supported-content-types"></a>Desteklenen içerik türleri

Bir kapsayıcı üzerinde bir blob Indexer çalıştırarak, aşağıdaki içerik türlerinden metin ve meta verileri tek bir sorgu ile ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

### <a name="indexing-blob-metadata"></a>Blob meta verileri dizinleniyor

Herhangi bir içerik türünün Blobları arasında sıralama yapmayı kolaylaştıran yaygın bir senaryo, her blob için hem özel meta verileri hem de sistem özelliklerini dizinlemek. Bu şekilde, tüm Bloblar için bilgi, Azure Search bir dizinde depolanan belge türünden bağımsız olarak dizinlenir. Yeni dizininizi kullanarak, tüm blob depolama içeriklerine göre sıralama, filtreleme ve model işleme devam edebilirsiniz.

### <a name="indexing-json-blobs"></a>JSON bloblarını dizine alma
Azure Search, JSON içeren bloblarda bulunan yapılandırılmış içerikleri ayıklamak için yapılandırılabilir. Azure Search, JSON bloblarını okuyabilir ve yapılandırılmış içeriği bir Azure Search belgesinin uygun alanlarıyla ayrıştırabilirler. Azure Search Ayrıca, bir dizi JSON nesnesi içeren Blobları alabilir ve her öğeyi ayrı bir Azure Search belgesine eşler. Dizin Oluşturucu tarafından oluşturulan JSON nesnesinin türünü etkilemek için bir ayrıştırma modu ayarlayabilirsiniz.

## <a name="search-blob-content-in-a-search-index"></a>Arama dizininde blob içeriğini ara 

Bir dizin oluşturma işleminin çıktısı, bir istemci uygulamasında ücretsiz metin ve filtrelenmiş sorgular kullanan etkileşimli araştırma için kullanılan bir arama dizinidir. İçeriğin ilk araştırılması ve doğrulanması için, belge yapısını incelemek üzere portalda [Arama Gezgini](search-explorer.md) ile başlamasını öneririz. Arama Gezgini 'nde [basit sorgu söz dizimi](query-simple-syntax.md), [tam sorgu söz](query-lucene-syntax.md)dizimi ve [filtre ifadesi sözdizimini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz.

Daha kalıcı bir çözüm, sorgu girişleri toplamaktır ve yanıtı bir istemci uygulamasında arama sonuçları olarak sunar. Aşağıdaki C# öğreticide, bir arama uygulamasının nasıl oluşturulacağı açıklanmaktadır: [Ilk uygulamanızı Azure Search oluşturma](tutorial-csharp-create-first-app.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Blob Dizin Oluşturucu ayarlama](search-howto-indexing-azure-blob-storage.md) 
