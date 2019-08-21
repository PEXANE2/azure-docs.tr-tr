---
title: Ortak arama Dizin Oluşturucu sorunlarını giderme-Azure Search
description: Veri kaynağı bağlantısı, güvenlik duvarı ve eksik belgeler de dahil olmak üzere Azure Search içindeki dizin oluşturucularla ilgili hataları ve yaygın sorunları giderin.
author: mgottein
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: magottei
ms.openlocfilehash: 4692be287e9b38cf116107d2e7c1043f23a6b34b
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640607"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-search"></a>Azure Search ortak Dizin Oluşturucu sorunlarını giderme

Dizin oluşturucular, Azure Search veri dizinlerken çeşitli sorunlar halinde çalışabilir. Hatanın ana kategorileri şunlardır:

* [Bir veri kaynağına bağlanma](#data-source-connection-errors)
* [Belge işleme](#document-processing-errors)
* [Bir dizine belge alımı](#index-errors)

## <a name="data-source-connection-errors"></a>Veri kaynağı bağlantı hataları

### <a name="blob-storage"></a>Blob Depolama Alanı

#### <a name="storage-account-firewall"></a>Depolama hesabı güvenlik duvarı

Azure depolama, yapılandırılabilir bir güvenlik duvarı sağlar. Azure Search depolama hesabınıza bağlanabilmesi için, varsayılan olarak güvenlik duvarı devre dışı bırakılır.

Güvenlik Duvarı etkinleştirildiğinde belirli bir hata iletisi yoktur. Genellikle, güvenlik duvarı hataları benzer `The remote server returned an error: (403) Forbidden`şekilde görünür.

[Portalda](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal)güvenlik duvarının etkinleştirildiğini doğrulayabilirsiniz. Desteklenen tek geçici çözüm, [' tüm ağlar '](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal)'tan erişime izin vermeyi seçerek güvenlik duvarını devre dışı bırakmamak.

Dizin oluşturucuda iliştirilmiş bir beceri yoksa, arama hizmetinizin IP adresleri için [bir özel durum eklemeyi](https://docs.microsoft.com/azure/storage/common/storage-network-security#managing-ip-network-rules) deneyebilirsiniz. Ancak, bu senaryo desteklenmez ve çalışması garanti edilmez.

FQDN (`<your-search-service-name>.search.windows.net`) ile ping yaparak, arama hizmetinizin IP adresini bulabilirsiniz.

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>Dizin oluşturma etkin değil

Azure Search Cosmos DB Dizin oluşturma üzerinde örtük bir bağımlılığı vardır. Cosmos DB Otomatik Dizin oluşturmayı kapatırsanız, Azure Search başarılı bir durum döndürür, ancak kapsayıcı içerikleri dizin haline getirir. Ayarları denetleme ve Dizin oluşturmayı açma hakkında yönergeler için, bkz. [Azure Cosmos DB Dizin oluşturmayı yönetme](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Belge Işleme hataları

### <a name="unprocessable-or-unsupported-documents"></a>Proceslabilen veya desteklenmeyen belgeler

Blob Dizin Oluşturucu, [hangi belge biçimlerinin açıkça desteklendiğini belgeler.](search-howto-indexing-azure-blob-storage.md#supported-document-formats) Bazen bir BLOB depolama kapsayıcısı desteklenmeyen belgeler içeriyor. Diğer zamanlarda sorunlu belgeler olabilir. [Yapılandırma seçeneklerini değiştirerek](search-howto-indexing-azure-blob-storage.md#dealing-with-errors)bu belgelerde Dizin oluşturucuyu durdurmayı önleyebilirsiniz:

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Belge içeriği eksik

Blob Indexer [bir kapsayıcıdaki bloblardan metin bulur ve ayıklar](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). Metin ayıklama ile ilgili bazı sorunlar şunlardır:

* Belge yalnızca taranmış görüntüleri içerir. Taranmış görüntüler (JPGs) gibi metin olmayan içeriğe sahip PDF blob 'ları, standart bir blob dizin oluşturma işlem hattında sonuç üretmez. Metin öğeleriyle görüntü içeriğiniz varsa, bilişsel [arama](cognitive-search-concept-image-scenarios.md) 'yı kullanarak metni bulabilir ve ayıklayabilir.
* Blob Indexer yalnızca dizin meta verileri için yapılandırılmış. İçeriği ayıklamak için, blob dizin oluşturucunun [hem içeriği hem de meta verileri ayıklamak](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed)üzere yapılandırılması gerekir:

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Dizin hataları

### <a name="missing-documents"></a>Eksik belgeler

Dizin oluşturucular bir [veri kaynağından](https://docs.microsoft.com/rest/api/searchservice/create-data-source)belge bulur. Bazen veri kaynağından dizinlenen bir belge, dizinde eksik gibi görünüyor. Bu hataların birçok yaygın nedeni olabilir:

* Belge Dizinlenmemiş. Başarılı bir Dizin Oluşturucu çalıştırması için portalı denetleyin.
* Dizin Oluşturucu çalıştırıldıktan sonra belge güncelleştirildi. Dizinleyicinin bir [zamanlamaya](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule)göre olması, sonunda yeniden çalıştırılır ve belgeyi seçer.
* Veri kaynağında belirtilen [sorgu](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) , belgeyi dışlar. Dizin oluşturucular, veri kaynağının parçası olmayan belgeleri dizinlemez.
* [Alan eşlemeleri](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) veya bilişsel [arama](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) belgeyi değiştirdi ve beklediğinizden farklı görünüyor.
* Belgenizi bulmak için [arama BELGESI API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) 'sini kullanın.
