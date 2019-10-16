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
ms.openlocfilehash: 24886d8b8a7b679f6474789748a002c8d045a746
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331288"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-search"></a>Azure Search kullanarak Azure Blob verilerine tam metin araması ekleme

Azure Blob depolamada depolanan çeşitli içerik türleri arasında arama yapmak zor bir sorun olabilir. Ancak, [Azure Search](search-what-is-azure-search.md)kullanarak bloblarınızın içeriğini yalnızca birkaç tıklamayla dizinleyerek ve arayabilirsiniz. Azure Search, dizin oluşturma için veri kaynağı kullanan yetenekleri ekleyen bir [*BLOB Indexer*](search-howto-indexing-azure-blob-storage.md) aracılığıyla blob depolamayı dizine alma için yerleşik tümleştirmeye sahiptir.

## <a name="supported-content-types"></a>Desteklenen içerik türleri

Bir kapsayıcı üzerinde bir blob Indexer çalıştırarak, aşağıdaki içerik türlerinden metin ve meta verileri tek bir sorgu ile ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

İsteğe bağlı olarak, görüntü dosyalarının metin temsilleri dahil olmak üzere bloblardan yeni bilgi ve yapı oluşturmak için bir *beceri* biçiminde [AI zenginleştirme](search-blob-ai-integration.md) ekleyebilirsiniz. AI zenginleştirme 'nin eklenmesi, içerik türü listesini JPEG ve PNG dahil etmek için genişletir. [Optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) gibi görüntü işleme becerileri ve her görüntüde bulunan görsel içerikleri dizinlemek mümkün kılan [Görsel özelliklerin](cognitive-search-skill-image-analysis.md) tanımlanması gibi özellikler ekler.

## <a name="search-through-your-blob-metadata"></a>Blob meta verilerinizdeki arama yapın
Herhangi bir içerik türünün Blobları arasında sıralama yapmayı kolaylaştıran yaygın bir senaryo, her blob için hem özel meta verileri hem de sistem özelliklerini dizinlemek. Bu şekilde, tüm Bloblar için bilgi, Azure Search bir dizinde depolanan belge türünden bağımsız olarak dizinlenir. Yeni dizininizi kullanarak, tüm blob depolama içeriklerine göre sıralama, filtreleme ve model işleme devam edebilirsiniz.

### <a name="indexing-json-blobs"></a>JSON bloblarını dizine alma
Azure Search, JSON içeren bloblarda bulunan yapılandırılmış içerikleri ayıklamak için yapılandırılabilir. Azure Search, JSON bloblarını okuyabilir ve yapılandırılmış içeriği bir Azure Search belgesinin uygun alanlarıyla ayrıştırabilirler. Azure Search Ayrıca, bir dizi JSON nesnesi içeren Blobları alabilir ve her öğeyi ayrı bir Azure Search belgesine eşler. Dizin Oluşturucu tarafından oluşturulan JSON nesnesinin türünü etkilemek için bir ayrıştırma modu ayarlayabilirsiniz.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Doğrudan depolama hesabı portalı sayfanızda başlayabilirsiniz. Var olan bir Azure Search hizmetini seçebileceğiniz veya yeni bir hizmet oluşturabileceğiniz bir akışı başlatmak için **Azure Search Ekle** ' ye tıklayın. Yeni bir hizmet oluşturursanız, depolama hesabınızın Portal deneyiminizden siz gezinileolursunuz. Depolama portalı sayfasına dönüp, mevcut hizmeti seçebileceğiniz **Azure Search Ekle** seçeneğini yeniden seçebilirsiniz. 

![](./media/search-blob-storage-integration/blob-blade.png)

Aynı abonelikte mevcut bir arama hizmetiniz zaten varsa, **Azure Search Ekle** ' ye tıklayarak veri alma Sihirbazı ' nı açar, böylece dizin oluşturma, zenginleştirme ve Dizin tanımı anında ilerlebilmenizi sağlayabilirsiniz.

Ayrıca, [bir Azure Search hizmeti oluşturabilir](search-create-index-portal.md) ve BLOB kapsayıcılarından Içerik çeken blob Dizinleyicileri tanımlayabilirsiniz.

Aşağıdaki hızlı başlangıç ve öğreticiler blob verilerini kullanır:

+ [REST API 'Leri kullanarak yarı yapılandırılmış Blobların dizinini oluştur](search-semi-structured-data.md)
+ [Portalda bir AI zenginleştirme işlem hattı oluşturma](cognitive-search-quickstart-blob.md)
+ [İçinde bir AI zenginleştirme işlem hattı oluşturmaC#](cognitive-search-tutorial-blob-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Blob Dizin Oluşturucu ayarlama](search-howto-indexing-azure-blob-storage.md) 
