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
ms.openlocfilehash: 1aec65ab08cd1c0711e51a222a8e674ef56ef508
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72312189"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-search"></a>Azure Search kullanarak Azure Blob verilerine tam metin araması ekleme

Azure Blob depolamada depolanan çeşitli içerik türleri arasında arama yapmak zor bir sorun olabilir. Ancak, [Azure Search](search-what-is-azure-search.md)kullanarak bloblarınızın içeriğini yalnızca birkaç tıklamayla dizinleyerek ve arayabilirsiniz. Azure Search, dizin oluşturma için veri kaynağı kullanan yetenekleri ekleyen bir [*BLOB Indexer*](search-howto-indexing-azure-blob-storage.md) aracılığıyla blob depolamayı dizine alma için yerleşik tümleştirmeye sahiptir.

## <a name="supported-content-types"></a>Desteklenen içerik türleri

Bir kapsayıcı üzerinde bir blob Indexer çalıştırarak, aşağıdaki içerik türlerinden metin ve meta verileri tek bir sorgu ile ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

İsteğe bağlı olarak, görüntü dosyalarının metin temsilleri dahil olmak üzere bloblardan yeni bilgi ve yapı oluşturmak için bir *beceri* biçiminde [AI zenginleştirme](search-blob-ai-integration.md) ekleyebilirsiniz. AI zenginleştirme 'nin eklenmesi, içerik türü listesini JPEG ve PNG dahil etmek için genişletir. [Optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) gibi görüntü işleme becerileri ve her görüntüde bulunan görsel içerikleri dizinlemek mümkün kılan [Görsel özelliklerin](cognitive-search-skill-image-analysis.md) tanımlanması gibi özellikler ekler.

## <a name="search-through-your-blob-metadata"></a>Blob meta verilerinizdeki arama yapın
Herhangi bir içerik türünün Blobları arasında sıralama yapmayı kolaylaştıran yaygın bir senaryo, her blob için hem özel meta verileri hem de sistem özelliklerini dizinlemek. Bu şekilde, tüm Bloblar için bilgiler belge türünden bağımsız olarak dizinlenir. Daha sonra tüm blob depolama içeriklerine göre sıralama, filtreleme ve model işleme devam edebilirsiniz.

[Blob meta verilerinin dizinini oluşturma hakkında daha fazla bilgi edinin.](https://aka.ms/azsblobmetadataindexing)

## <a name="index-and-search-through-json-blobs"></a>JSON Blobları aracılığıyla Dizin ve arama
Azure Search, JSON içeren bloblarda bulunan yapılandırılmış içerikleri ayıklamak için yapılandırılabilir. Azure Search, JSON bloblarını okuyabilir ve yapılandırılmış içeriği bir Azure Search belgesinin uygun alanlarıyla ayrıştırabilirler. Azure Search Ayrıca, bir dizi JSON nesnesi içeren Blobları alabilir ve her öğeyi ayrı bir Azure Search belgesine eşler.

JSON ayrıştırması Şu anda portal üzerinden yapılandırılamaz. [Azure Search 'de JSON ayrıştırma hakkında daha fazla bilgi edinin.](https://aka.ms/azsjsonblobindexing)

## <a name="quickstart"></a>Hızlı Başlangıç
Azure Search bloblara doğrudan blob Storage Portal sayfasından eklenebilir.

![](./media/search-blob-storage-integration/blob-blade.png)

Var olan bir Azure Search hizmetini seçebileceğiniz veya yeni bir hizmet oluşturabileceğiniz bir akışı başlatmak için **Azure Search Ekle** ' ye tıklayın. Yeni bir hizmet oluşturursanız, depolama hesabınızın Portal deneyiminizden siz gezinileolursunuz. Depolama portalı sayfasına dönüp, mevcut hizmeti seçebileceğiniz **Azure Search Ekle** seçeneğini yeniden seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Tam [belgelerde](https://aka.ms/azsblobindexer)Azure Search blob Dizin Oluşturucu hakkında daha fazla bilgi edinin.
