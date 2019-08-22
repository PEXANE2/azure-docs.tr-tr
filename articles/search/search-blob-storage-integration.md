---
title: Azure Blob depolama alanına tam metin araması ekleme-Azure Search
description: HTTP REST API kullanarak kodda Azure Search dizin oluşturma için Azure Blob depolamada gezinme metin içeriği.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/01/2019
author: mgottein
manager: nitinme
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: f0801931b57302ae1d627dab783a40d2407c19ac
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650082"
---
# <a name="searching-blob-storage-with-azure-search"></a>Azure Search ile Blob deposu arama

Azure Blob depolamada depolanan çeşitli içerik türleri arasında arama yapmak zor bir sorun olabilir. Ancak, Azure Search kullanarak Bloblarınızın içeriğini yalnızca birkaç tıklamayla dizinleyerek ve arayabilirsiniz. Blob Storage üzerinde arama yapmak için bir Azure Search hizmeti sağlanması gerekir. Azure Search çeşitli hizmet limitleri ve fiyatlandırma katmanları [fiyatlandırma sayfasında](https://aka.ms/azspricing)bulunabilir.

## <a name="what-is-azure-search"></a>Azure Search nedir?
[Azure Search](https://aka.ms/whatisazsearch) , geliştiricilerin Web ve mobil uygulamalara güçlü tam metin arama deneyimleri eklemesini kolaylaştıran bir arama hizmetidir. Hizmet olarak, Azure Search [% 99,9 çalışma süresi SLA 'sı](https://aka.ms/azuresearchsla)sunarak herhangi bir arama altyapısını yönetme gereksinimini ortadan kaldırır.

## <a name="index-and-search-enterprise-document-formats"></a>Kurumsal belge biçimlerini dizin ve arama
Azure Blob depolamada [belge ayıklama](https://aka.ms/azsblobindexer) desteğiyle aşağıdaki içeriği dizini oluşturabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Bu dosya türlerinden metin ve meta verileri ayıklayarak, tek bir sorgu ile birden çok dosya biçimi arasında arama yapabilirsiniz. 

## <a name="search-through-your-blob-metadata"></a>Blob meta verilerinizdeki arama yapın
Herhangi bir içerik türünün Blobları arasında sıralama yapmayı kolaylaştıran yaygın bir senaryo, her blob için hem özel meta verileri hem de sistem özelliklerini dizinlemek. Bu şekilde, tüm Bloblar için bilgiler belge türünden bağımsız olarak dizinlenir. Daha sonra tüm blob depolama içeriklerine göre sıralama, filtreleme ve model işleme devam edebilirsiniz.

[Blob meta verilerinin dizinini oluşturma hakkında daha fazla bilgi edinin.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Görüntü arama
Azure Search tam metin araması, çok yönlü gezinme ve sıralama özellikleri artık bloblarda depolanan görüntülerin meta verilerine uygulanabilir.

Bilişsel arama, [optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) gibi görüntü işleme becerilerini ve her görüntüde bulunan görsel içeriğin dizinini oluşturmak mümkün kılan [Görsel özelliklerin](cognitive-search-skill-image-analysis.md) tanımlanmasını içerir.

## <a name="index-and-search-through-json-blobs"></a>JSON Blobları aracılığıyla Dizin ve arama
Azure Search, JSON içeren bloblarda bulunan yapılandırılmış içerikleri ayıklamak için yapılandırılabilir. Azure Search, JSON bloblarını okuyabilir ve yapılandırılmış içeriği bir Azure Search belgesinin uygun alanlarıyla ayrıştırabilirler. Azure Search Ayrıca, bir dizi JSON nesnesi içeren Blobları alabilir ve her öğeyi ayrı bir Azure Search belgesine eşler.

JSON ayrıştırması Şu anda portal üzerinden yapılandırılamaz. [Azure Search 'de JSON ayrıştırma hakkında daha fazla bilgi edinin.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Hızlı başlangıç
Azure Search bloblara doğrudan blob Storage Portal sayfasından eklenebilir.

![](./media/search-blob-storage-integration/blob-blade.png)

Var olan bir Azure Search hizmetini seçebileceğiniz veya yeni bir hizmet oluşturabileceğiniz bir akışı başlatmak için **Azure Search Ekle** ' ye tıklayın. Yeni bir hizmet oluşturursanız, depolama hesabınızın Portal deneyiminizden siz gezinileolursunuz. Depolama portalı sayfasına dönüp, mevcut hizmeti seçebileceğiniz **Azure Search Ekle** seçeneğini yeniden seçebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Tam [belgelerde](https://aka.ms/azsblobindexer)Azure Search blob Dizin Oluşturucu hakkında daha fazla bilgi edinin.
