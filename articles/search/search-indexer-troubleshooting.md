---
title: Ortak arama Dizin Oluşturucu sorunlarını giderme
titleSuffix: Azure Cognitive Search
description: Veri kaynağı bağlantısı, güvenlik duvarı ve eksik belgeler dahil olmak üzere Azure Bilişsel Arama 'daki Dizin oluşturucularla ilgili hataları ve yaygın sorunları giderin.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1e3692920c35a6965a23c0305aeeebfc80505d85
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77190923"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-cognitive-search"></a>Azure Bilişsel Arama ortak Dizin Oluşturucu sorunlarını giderme

Dizin oluşturucular, verileri Azure Bilişsel Arama 'da dizin oluştururken çeşitli sorunlar halinde çalışabilir. Hatanın ana kategorileri şunlardır:

* [Bir veri kaynağına veya diğer kaynaklara bağlanma](#connection-errors)
* [Belge işleme](#document-processing-errors)
* [Bir dizine belge alımı](#index-errors)

## <a name="connection-errors"></a>Bağlantı hataları

> [!NOTE]
> Dizin oluşturucular, Azure ağ güvenlik mekanizmaları tarafından güvenliği sağlanan veri kaynaklarına ve diğer kaynaklara erişmek için sınırlı desteğe sahiptir. Şu anda, Dizin oluşturucular yalnızca ilgili IP adres aralığı kısıtlama mekanizmaları veya varsa NSG kuralları aracılığıyla veri kaynaklarına erişebilir. Desteklenen her veri kaynağına erişme ayrıntıları aşağıda bulunabilir.
>
> Tam etki alanı adının (örn. `<your-search-service-name>.search.windows.net`) ping işlemi gerçekleştirerek arama hizmetinizin IP adresini bulabilirsiniz.
>
> `AzureCognitiveSearch` [Hizmet](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) etiketinin IP adresi aralığını [indirilebilir JSON dosyalarını](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) veya [hizmet etiketi bulma API 'si](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview)aracılığıyla bulabilirsiniz. IP adresi aralığı haftalık olarak güncelleştirilir.

### <a name="configure-firewall-rules"></a>Güvenlik duvarı kurallarını yapılandırma

Azure Storage, CosmosDB ve Azure SQL yapılandırılabilir bir güvenlik duvarı sağlar. Güvenlik Duvarı etkinleştirildiğinde belirli bir hata iletisi yoktur. Genellikle, güvenlik duvarı hataları geneldir ve veya `The remote server returned an error: (403) Forbidden` `Credentials provided in the connection string are invalid or have expired`gibi görünür.

Dizin oluşturucularının böyle bir örnekteki bu kaynaklara erişmesine izin vermek için 2 seçenek vardır:

* **Tüm ağlardan** erişime izin vererek (uygulanabiliyorsa) güvenlik duvarını devre dışı bırakın.
* Alternatif olarak, kaynağınızın güvenlik duvarı kurallarında (IP adresi aralığı kısıtlaması), arama hizmetinizin IP adresi ve `AzureCognitiveSearch` [hizmet](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) IP adresi aralığı için erişime izin verebilirsiniz.

Her veri kaynağı türü için IP adresi aralığı kısıtlamalarını yapılandırmaya ilişkin ayrıntılar aşağıdaki bağlantılardan bulunabilir:

* [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Cosmos DB](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#create-and-manage-ip-firewall-rules)

**Kısıtlama**: Azure depolama için yukarıdaki belgelerde belirtildiği gıbı, IP adres aralığı kısıtlamaları yalnızca arama hizmetiniz ve depolama hesabınız farklı bölgelerde olduğunda çalışır.

Azure işlevleri ( [özel bir Web API 'si](cognitive-search-custom-skill-web-api.md)olarak kullanılabilir), [IP adresi kısıtlamalarını](https://docs.microsoft.com/azure/azure-functions/ip-addresses#ip-address-restrictions)da destekler. Yapılandırılacak IP adresleri listesi, arama hizmetinizin IP adresi ve `AzureCognitiveSearch` HIZMET etiketinin IP adresi aralığıdır.

Azure VM 'de SQL Server 'daki verilere erişim ayrıntıları [burada](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) özetlenmiştir

### <a name="configure-network-security-group-nsg-rules"></a>Ağ güvenlik grubu (NSG) kurallarını yapılandırma

Bir SQL yönetilen örneğindeki verilere erişirken veya bir Azure VM, [özel Web API 'si yeteneği](cognitive-search-custom-skill-web-api.md)için Web hizmeti URI 'si olarak kullanıldığında, MÜŞTERILERIN belirli IP adresleriyle ilgilenmemelidir.

Bu gibi durumlarda, Azure VM veya SQL yönetilen örneği bir sanal ağ içinde bulunacak şekilde yapılandırılabilir. Daha sonra bir ağ güvenlik grubu, sanal ağ alt ağlarının ve ağ arabirimlerinin içinde ve dışında akabilir ağ trafiği türünü filtrelemek için yapılandırılabilir.

`AzureCognitiveSearch` Hizmet etiketi, gelen [NSG kurallarında](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#work-with-security-rules) , IP adresi aralığını aramak gerekmeden doğrudan kullanılabilir.

SQL yönetilen örneğindeki verilere erişim hakkında daha fazla ayrıntı [burada](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md) özetlenmiştir

### <a name="cosmosdb-indexing-isnt-enabled"></a>CosmosDB "Dizin oluşturma" etkin değil

Azure Bilişsel Arama Cosmos DB Dizin oluşturma üzerinde örtük bir bağımlılığa sahiptir. Cosmos DB Otomatik Dizin oluşturmayı kapatırsanız Azure Bilişsel Arama başarılı bir durum döndürür, ancak kapsayıcı içerikleri dizinlemeye başarısız olur. Ayarları denetleme ve Dizin oluşturmayı açma hakkında yönergeler için, bkz. [Azure Cosmos DB Dizin oluşturmayı yönetme](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Belge işleme hataları

### <a name="unprocessable-or-unsupported-documents"></a>Proceslabilen veya desteklenmeyen belgeler

Blob Dizin Oluşturucu, [hangi belge biçimlerinin açıkça desteklendiğini belgeler.](search-howto-indexing-azure-blob-storage.md#SupportedFormats) Bazen bir BLOB depolama kapsayıcısı desteklenmeyen belgeler içeriyor. Diğer zamanlarda sorunlu belgeler olabilir. [Yapılandırma seçeneklerini değiştirerek](search-howto-indexing-azure-blob-storage.md#DealingWithErrors)bu belgelerde Dizin oluşturucuyu durdurmayı önleyebilirsiniz:

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
* Veri kaynağında belirtilen [sorgu](/rest/api/searchservice/create-data-source) , belgeyi dışlar. Dizin oluşturucular, veri kaynağının parçası olmayan belgeleri dizinlemez.
* [Alan eşlemeleri](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) veya [AI zenginleştirme](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) belgeyi değiştirdi ve beklediğinizden farklı görünüyor.
* Belgenizi bulmak için [arama BELGESI API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) 'sini kullanın.
