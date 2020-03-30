---
title: Sık karşılaşılan arama dizini sorunları nın giderimi
titleSuffix: Azure Cognitive Search
description: Veri kaynağı bağlantısı, güvenlik duvarı ve eksik belgeler de dahil olmak üzere Azure Bilişsel Arama'daki dizin oluşturma hataları ve sık karşılaşılan sorunları giderin.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1e3692920c35a6965a23c0305aeeebfc80505d85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190923"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sık karşılaşılan dizin oluşturma sorunları nın giderme

Dizin leyiciler, verileri Azure Bilişsel Arama'ya dizine ekrirken bir dizi sorunla karşılayabiliyor. Başarısızlığın ana kategorileri şunlardır:

* [Bir veri kaynağına veya diğer kaynaklara bağlanma](#connection-errors)
* [Belge işleme](#document-processing-errors)
* [Bir dizin için belge alma](#index-errors)

## <a name="connection-errors"></a>Bağlantı hataları

> [!NOTE]
> Dizin leyicilerin, Azure ağ güvenliği mekanizmaları tarafından güvence altına alınan veri kaynaklarına ve diğer kaynaklara erişmek için sınırlı desteği vardır. Şu anda, dizinleyiciler veri kaynaklarına yalnızca ilgili IP adresi aralığı kısıtlama mekanizmaları veya gerektiğinde NSG kuralları aracılığıyla erişebilirler. Desteklenen her veri kaynağına erişmek için ayrıntılar aşağıda bulunabilir.
>
> Arama hizmetinizin IP adresini tam nitelikli alan adını pingleyerek öğrenebilirsiniz `<your-search-service-name>.search.windows.net`(örn. ).
>
> [Indirilebilir JSON dosyalarını](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) kullanarak `AzureCognitiveSearch` veya Service Tag Discovery [API](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview)aracılığıyla hizmet [etiketinin](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) IP adres aralığını öğrenebilirsiniz. IP adres aralığı haftalık olarak güncellenir.

### <a name="configure-firewall-rules"></a>Güvenlik duvarı kurallarını yapılandırma

Azure Depolama, CosmosDB ve Azure SQL yapılandırılabilir bir güvenlik duvarı sağlar. Güvenlik duvarı etkinleştirildiğinde belirli bir hata iletisi yoktur. Genellikle, güvenlik duvarı hataları geneldir `The remote server returned an error: (403) Forbidden` `Credentials provided in the connection string are invalid or have expired`ve beğenin veya .

Dizin leyicilerin bu tür durumlarda bu kaynaklara erişmesine izin vermek için 2 seçenek vardır:

* **Tüm Ağlar'dan** erişime izin vererek (mümkünse) güvenlik duvarını devre dışı kılabilir.
* Alternatif olarak, kaynağınızın güvenlik duvarı kurallarında arama hizmetinizin `AzureCognitiveSearch` IP adresine ve [HIZMET etiketinin](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) IP adresi aralığına erişime izin verebilirsiniz (IP adresi aralığı kısıtlaması).

Her veri kaynağı türü için IP adresi aralığı kısıtlamalarını yapılandırmak için ayrıntılar aşağıdaki bağlantılardan bulunabilir:

* [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Cosmos DB](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#create-and-manage-ip-firewall-rules)

**Sınırlama**: Azure Depolama için yukarıdaki belgelerde belirtildiği gibi, IP adresi aralığı kısıtlamaları yalnızca arama hizmetiniz ve depolama hesabınız farklı bölgelerdeyse çalışır.

Azure işlevleri [(Özel Web Api becerisi](cognitive-search-custom-skill-web-api.md)olarak kullanılabilir) [IP adresi kısıtlamalarını](https://docs.microsoft.com/azure/azure-functions/ip-addresses#ip-address-restrictions)da destekler. Yapılandırmak için IP adresleri listesi, arama hizmetinizin IP adresi ve `AzureCognitiveSearch` hizmet etiketinin IP adresi aralığı dır.

Azure VM'de SQL sunucusundaki verilere erişmeayrıntıları [burada](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) özetlenmiştir

### <a name="configure-network-security-group-nsg-rules"></a>Ağ güvenlik grubu (NSG) kurallarını yapılandırma

SQL yönetilen bir örnekte verilere erişirken veya Bir Azure VM [Özel Web Api becerisi](cognitive-search-custom-skill-web-api.md)için web hizmeti URI olarak kullanıldığında, müşterilerin belirli IP adresleriyle ilgilenmesi gerekmez.

Bu gibi durumlarda, Azure VM veya SQL yönetilen örneği sanal ağ içinde ikamet etmek üzere yapılandırılabilir. Daha sonra bir ağ güvenlik grubu, sanal ağ alt ağlarına ve ağ arabirimlerine girip çıkabilen ağ trafiği türünü filtrelemek için yapılandırılabilir.

Servis `AzureCognitiveSearch` etiketi, IP adresi aralığına bakmaya gerek kalmadan gelen [NSG kurallarında](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#work-with-security-rules) doğrudan kullanılabilir.

SQL yönetilen bir örnekte verilere erişmek için daha fazla ayrıntı [burada](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md) özetlenmiştir

### <a name="cosmosdb-indexing-isnt-enabled"></a>CosmosDB "Dizin oluşturma" etkin değil

Azure Bilişsel Arama, Cosmos DB dizin oluşturma konusunda örtük bir bağımlılığa sahiptir. Cosmos DB'de otomatik dizin oluşturmayı kapatırsanız, Azure Bilişsel Arama başarılı bir durum döndürür, ancak kapsayıcı içeriğini dizine ekmezse. Ayarları nasıl denetleyip dizin oluşturmayı nasıl açıklayabilenlere ilişkin talimatlar için azure [cosmos DB'de dizin oluşturmayı yönet'e](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal)bakın.

## <a name="document-processing-errors"></a>Belge işleme hataları

### <a name="unprocessable-or-unsupported-documents"></a>İşlenemeyen veya desteklenmeyen belgeler

Biçimleri belgeleyen blob dizinleyici [belgeleri açıkça desteklenir. .](search-howto-indexing-azure-blob-storage.md#SupportedFormats) Bazen, bir blob depolama kapsayıcısı desteklenmeyen belgeler içerir. Diğer zamanlarda sorunlu belgeler olabilir. [Yapılandırma seçeneklerini değiştirerek](search-howto-indexing-azure-blob-storage.md#DealingWithErrors)bu belgelerde dizin leyicinizi durdurmaktan kaçınabilirsiniz:

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Eksik belge içeriği

Blob dizinleyici [bulur ve bir kaplekeleri metin ayıklar.](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs) Metin ayıklama ile ilgili bazı sorunlar şunlardır:

* Belge yalnızca taranan görüntüler içerir. Taranmış görüntüler (JpGs) gibi metin olmayan içeriğe sahip PDF blob'ları standart bir blob dizinleme ardışık boru hattıyla sonuç üretmez. Metin öğeleriiçeren resim içeriğiniz varsa, metni bulmak ve ayıklamak için [bilişsel aramayı](cognitive-search-concept-image-scenarios.md) kullanabilirsiniz.
* Blob dizinleyici yalnızca dizin meta verileri için yapılandırılır. İçerik ayıklamak için, blob dizinleyici [hem içerik hem de meta veri ayıklamak](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed)için yapılandırılmalıdır:

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

Dizin leyiciler belgeleri bir [veri kaynağından](https://docs.microsoft.com/rest/api/searchservice/create-data-source)bulur. Bazen veri kaynağından dizine alınması gereken bir belge bir dizinden eksik görünür. Bu hataların gerçekleşmesinin birkaç yaygın nedeni vardır:

* Belge dizine eklenmedi. Başarılı bir dizin leyici çalışması için portalı denetleyin.
* Belge dizinleyici çalıştırıldıktan sonra güncelleştirildi. Dizin leyiciniz bir [schedule](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule)zamanlamadaysa, sonunda yeniden çalışır ve belgeyi alır.
* Veri kaynağında belirtilen [sorgu](/rest/api/searchservice/create-data-source) belgeyi dışlar. Dizin leyiciler, veri kaynağının parçası olmayan belgeleri dizine ekemez.
* [Alan eşlemeleri](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) veya [AI zenginleştirme](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) belgedeğişti ve beklediğiniz daha farklı görünüyor.
* Belgenizi bulmak için [arama belgesi API'sini](https://docs.microsoft.com/rest/api/searchservice/lookup-document) kullanın.
