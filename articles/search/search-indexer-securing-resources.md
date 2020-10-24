---
title: Korumalı kaynaklara Dizin Oluşturucu erişimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de Dizin oluşturucular tarafından Azure veri erişimi için ağ düzeyinde güvenlik seçeneklerine kavramsal genel bakış.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: bcb6e91bba367363385214806077146b1a24fe7b
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92503496"
---
# <a name="indexer-access-to-content-protected-by-azure-network-security-features-azure-cognitive-search"></a>Azure ağ güvenlik özellikleriyle korunan içeriğe Dizin Oluşturucu erişimi (Azure Bilişsel Arama)

Azure Bilişsel Arama Dizin oluşturucular, yürütme sırasında çeşitli Azure kaynaklarına giden çağrılar yapabilir. Bu makalede, IP güvenlik duvarları, Özel uç noktaları veya diğer Azure ağ düzeyinde güvenlik mekanizmaları tarafından korunan içeriğe Dizin Oluşturucu erişimi 'nin arkasındaki kavramlar açıklanmaktadır. Dizin Oluşturucu iki durumda giden çağrılar yapar: Dizin oluşturma sırasında veri kaynaklarına bağlanma ve bir beceri aracılığıyla kapsüllenmiş koda bağlanma. Bir dizin oluşturucunun tipik bir çalıştırmada erişebileceği tüm olası kaynak türlerinin listesi aşağıdaki tabloda listelenmiştir.

| Kaynak | Dizin Oluşturucu çalıştırma içinde amaç |
| --- | --- |
| Azure depolama (blob 'lar, tablolar, ADLS Gen 2) | Veri kaynağı |
| Azure depolama (blob 'lar, tablolar) | Becerileri (zenginleştirilmiş belgeleri önbelleğe alma ve bilgi deposu projeksiyonlarını depolama) |
| Azure Cosmos DB (çeşitli API 'Ler) | Veri kaynağı |
| Azure SQL Veritabanı | Veri kaynağı |
| Azure sanal makinelerde SQL Server | Veri kaynağı |
| SQL Yönetilen Örnek | Veri kaynağı |
| Azure İşlevleri | Özel Web API becerileri için ana bilgisayar |
| Bilişsel Hizmetler | 20 ücretsiz belge sınırının ötesinde zenginleştirme yapmak için kullanılacak beceri 'ye eklendi |

> [!NOTE]
> Bir beceri 'e bağlı bilişsel hizmet kaynağı, gerçekleştirilen ve arama dizinine yazılan enzenginler temelinde faturalandırma için kullanılır. Bilişsel Hizmetler API'si erişmek için kullanılmaz. Bir dizin oluşturucunun enzenginleştirme ardışık düzeninde, verilerin aktarım sırasında güçlü şekilde şifrelendiği ve hiçbir zaman bekleyen olarak depolanabileceği iç güvenli iletişim kanalı aracılığıyla Bilişsel Hizmetler API'si meydana geldi.

Müşteriler, Azure tarafından sunulan çeşitli ağ yalıtımı mekanizmaları aracılığıyla bu kaynakları güvenli hale getirebilirler. Bilişsel hizmet kaynağı hariç olmak üzere, Dizin oluşturucular, aşağıdaki tabloda özetlenen, ağ yalıtımlı olsalar bile diğer tüm kaynaklara erişim yeteneği sınırlıdır.

| Kaynak | IP kısıtlaması | Özel uç nokta |
| --- | --- | ---- |
| Azure depolama (blob 'lar, tablolar, ADLS Gen 2) | Yalnızca depolama hesabı ve arama hizmeti farklı bölgelerde olduğunda desteklenir | Desteklenir |
| Azure Cosmos DB-SQL API 'SI | Desteklenir | Desteklenir |
| Azure Cosmos DB-Cassandra, Mongo ve Gremlin API 'SI | Desteklenir | Desteklenmeyen |
| Azure SQL Veritabanı | Desteklenir | Desteklenir |
| Azure sanal makinelerde SQL Server | Desteklenir | Yok |
| SQL Yönetilen Örnek | Desteklenir | Yok |
| Azure İşlevleri | Desteklenir | Yalnızca belirli Azure işlevleri katmanlarında desteklenir |

> [!NOTE]
> Kullanıcılar, yukarıda listelenen seçeneklere ek olarak, ağ güvenliği sağlanmış Azure depolama hesapları için Azure Bilişsel Arama 'nin [güvenilir bir Microsoft hizmeti](../storage/common/storage-network-security.md#trusted-microsoft-services)olduğu gerçeden yararlanabilir. Bu, belirli bir arama hizmetinin depolama hesabındaki sanal ağ veya IP kısıtlamalarını atlayabileceği ve depolama hesabında uygun rol tabanlı erişim denetimi etkinse depolama hesabındaki verilere erişebileceği anlamına gelir. Daha fazla bilgi için bkz. [Güvenilen hizmet özel durumunu kullanarak Dizin Oluşturucu bağlantıları](search-indexer-howto-access-trusted-service-exception.md). Depolama hesabı ya da arama hizmeti farklı bir bölgeye taşınamadığından, bu seçenek IP kısıtlama yolu yerine kullanılabilir.

Bir dizin oluşturucunun kullanması gereken güvenli erişim mekanizmasını seçerken aşağıdaki kısıtlamaları göz önünde bulundurun:

- Dizin Oluşturucu bir [sanal ağ hizmeti uç noktasına](../virtual-network/virtual-network-service-endpoints-overview.md)bağlanamaz. Kimlik bilgileri, Özel uç noktalar, güvenilen hizmet ve IP adresleme içeren ortak uç noktalar, Dizin Oluşturucu bağlantıları için desteklenen tek yöntemlerden yalnızca bir hizmettir.
- Bir arama hizmeti, sanal makinede yerel olarak çalışan belirli bir sanal ağa sağlanamıyor. Bu işlevsellik Azure Bilişsel Arama tarafından sunulmayacaktır.
- Dizin oluşturucular, kaynaklara erişmek için özel uç noktaları (giden) kullanır, ek [özel bağlantı ücretleri](https://azure.microsoft.com/pricing/details/search/) de uygulanabilir.

## <a name="indexer-execution-environment"></a>Dizin Oluşturucu yürütme ortamı

Azure Bilişsel Arama Dizin oluşturucular, veri kaynaklarından içerik ayıklayarak, ayıklanan içeriğe zenginler ekleyerek ve isteğe bağlı olarak sonuçları arama dizinine yazmadan önce projeksiyonlar oluşturuyor. Bir dizin oluşturucuya atanan sorumlulukların sayısına bağlı olarak, iki ortamdan birinde çalışabilir:

- Belirli bir arama hizmeti için özel bir ortam. Bu tür ortamlarda çalışan Dizin oluşturucular, kaynakları diğer iş yükleriyle paylaşır (diğer müşteri tarafından başlatılan dizin oluşturma veya sorgulama iş yükleri gibi). Genellikle, bu ortamda yalnızca metin tabanlı dizin oluşturma (örneğin, Beceri kullanmayın) gerçekleştiren Dizin oluşturucular.
- Becerileri gibi kaynak kullanımı yoğun olan dizin oluşturucularının barındırıldığı çok kiracılı bir ortam. Bu ortam, yoğun işlemler için hizmete özgü kaynakları açık bırakarak, yoğun işlem gücü yükünü devretmek için kullanılır. Bu çok kiracılı ortam, Microsoft tarafından, müşterinin ek maliyeti olmadan yönetilir ve korunur.

Belirli bir Dizin Oluşturucu çalıştırması için Azure Bilişsel Arama, dizin oluşturucunun çalıştırılacağı en iyi ortamı belirler. Azure kaynaklarına erişimi denetlemek için bir IP güvenlik duvarı kullanıyorsanız, yürütme ortamları hakkında bilinmesi gereken her ikisi dahil bir IP aralığı ayarlamanıza yardımcı olur.

## <a name="granting-access-to-indexer-ip-ranges"></a>Dizin Oluşturucu IP aralıklarına erişim verme

Dizin oluşturucunun erişmeye çalıştığı kaynak yalnızca belirli bir IP aralığı kümesiyle sınırlı ise, Dizin Oluşturucu isteğinin ulaşabileceği olası IP aralıklarını dahil etmek için kümeyi genişletmeniz gerekir. Yukarıda belirtildiği gibi, dizin oluşturucularının çalıştığı ve hangi erişim isteklerinin kaynaklanbileceği iki olası ortam vardır. Dizin Oluşturucu erişimi için **her iki** ortamın IP adresini de eklemeniz gerekir.

- Arama hizmetine özgü özel ortamın IP adresini `nslookup` (veya `ping` ) arama hizmetinizin tam etki alanı adını (FQDN) almak için. Örneğin, genel buluttaki bir arama hizmetinin FQDN 'SI olacaktır `<service-name>.search.windows.net` . Bu bilgiler Azure portal kullanılabilir.
- Çok kiracılı ortamların IP adresleri, `AzureCognitiveSearch` hizmet etiketi aracılığıyla kullanılabilir. [Azure hizmet etiketlerinin](../virtual-network/service-tags-overview.md) her hizmet için YAYıMLANMıŞ bir IP adresi aralığı vardır. Bu, bir [bulma API 'si (Önizleme)](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) veya [indirilebilir bir JSON dosyası](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)aracılığıyla sağlanır. Her iki durumda da IP aralıkları bölgeye göre bölünür-yalnızca arama hizmetinizin sağlandığı bölge için atanan IP aralıklarını seçebilirsiniz.

Belirli veri kaynakları için, hizmet etiketinin kendisi, IP aralıklarının listesini numaralandırmak yerine doğrudan kullanılabilir (Arama hizmetinin IP adresinin hala açıkça kullanılması gerekir). Bu veri kaynakları, Azure depolama, Cosmos DB, Azure SQL gibi IP kurallarından farklı olarak bir hizmet etiketi eklemeyi yerel olarak destekleyen bir [ağ güvenlik grubu kuralı](../virtual-network/network-security-groups-overview.md)ayarlama yoluyla erişimi kısıtlar. `AzureCognitiveSearch`Arama HIZMETI IP adresine ek olarak, hizmet etiketini doğrudan kullanma becerisini destekleyen veri kaynakları şunlardır:

- [Azure sanal makineler 'de SQL Server](./search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md#restrict-access-to-the-azure-cognitive-search)

- [SQL Yönetilen Örnekleri](./search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md#verify-nsg-rules)

Bu bağlantı seçeneği hakkında daha fazla bilgi için bkz. [IP güvenlik duvarı aracılığıyla Dizin Oluşturucu bağlantıları](search-indexer-howto-access-ip-restricted.md).

## <a name="granting-access-via-private-endpoints"></a>Özel uç noktalar aracılığıyla erişim verme

Dizin oluşturucular, kaynaklara erişmek için [Özel uç noktaları](../private-link/private-endpoint-overview.md) kullanabilir, sanal ağlar seçmek veya hiçbir ortak erişim özelliği etkinleştirilmemiş olarak kilitlenir.
Bu işlevsellik yalnızca faturalandırılabilir Arama hizmetlerinde, oluşturulan özel uç nokta sayısıyla sınırlı olacak şekilde kullanılabilir. Daha fazla bilgi için bkz. [hizmet limitleri](search-limits-quotas-capacity.md#shared-private-link-resource-limits).

### <a name="step-1-create-a-private-endpoint-to-the-secure-resource"></a>1. Adım: güvenli kaynak için özel bir uç nokta oluşturma

Müşteriler, güvenli kaynaklarına (örneğin, bir depolama hesabı) özel bir uç nokta bağlantısı oluşturmak için, **paylaşılan bir özel bağlantı kaynağı**üzerinde arama yönetimi Işlemini, [CREATEORUPDATE API](/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate) 'yi çağırmalıdır. Bu (giden) özel uç nokta bağlantısının üzerinden geçen trafik yalnızca arama hizmeti 'ne özgü "özel" Dizin Oluşturucu yürütme ortamındaki sanal ağdan kaynaklanacaktır.

Azure Bilişsel Arama, bu API 'nin çağıranlarının güvenli kaynağa özel uç nokta bağlantı isteklerini onaylamak için Azure RBAC izinlerine sahip olduğunu doğrular. Örneğin, salt okuma izinlerine sahip bir depolama hesabına özel bir uç nokta bağlantısı istemeniz durumunda bu çağrı reddedilir.

### <a name="step-2-approve-the-private-endpoint-connection"></a>2. Adım: özel uç nokta bağlantısını onaylama

Paylaşılan bir özel bağlantı kaynağı oluşturan (zaman uyumsuz) işlem tamamlandığında, "bekleyen" durumunda özel bir uç nokta bağlantısı oluşturulur. Henüz bağlantı üzerinden trafik akışı yok.
Daha sonra müşterinin bu isteği güvenli kaynağında bulması ve "onaylaması" beklenir. Genellikle, bu, Azure portal veya [REST API](/rest/api/virtualnetwork/privatelinkservices/updateprivateendpointconnection)aracılığıyla yapılabilir.

### <a name="step-3-force-indexers-to-run-in-the-private-environment"></a>3. Adım: dizin oluşturucularının "özel" ortamda çalışmasına zorla

Onaylanan özel uç nokta, arama hizmetinden bir dizi ağ düzeyinde erişim kısıtlaması olan bir kaynağa (örneğin, yalnızca belirli sanal ağlardan erişilecek şekilde yapılandırılmış bir depolama hesabı veri kaynağı) başarılı olmak için giden çağrılara izin verir.
Bu, Özel uç nokta üzerinden bu tür bir veri kaynağına ulaşamayacak tüm dizin oluşturucularının başarılı olacağı anlamına gelir.
Özel uç nokta onaylanmazsa veya Dizin Oluşturucu özel uç nokta bağlantısını kullanmıyorsa, Dizin Oluşturucu çalıştırması içinde sona bırakılır `transientFailure` .

Dizin oluşturucularının özel uç nokta bağlantıları aracılığıyla kaynaklara erişmesini sağlamak için `executionEnvironment` dizin oluşturucunun, `"Private"` tüm dizin oluşturucunun özel uç noktadan yararlanabilebileceği şekilde ayarlanması zorunludur. Bunun nedeni özel uç noktaların özel arama hizmeti 'ne özgü ortamda sağlanması.

```json
    {
      "name" : "myindexer",
      ... other indexer properties
      "parameters" : {
          ... other parameters
          "configuration" : {
            ... other configuration properties
            "executionEnvironment": "Private"
          }
        }
    }
```

Bu adımlar, [Dizin Oluşturucu bağlantılarında özel bir uç nokta aracılığıyla](search-indexer-howto-access-private.md)daha ayrıntılı bir şekilde açıklanmıştır.
Bir kaynağa onaylanan özel uç nokta aldıktan sonra, Özel uç nokta bağlantısı aracılığıyla *özel* olarak erişim denemesi olarak ayarlanan Dizin oluşturucular.

### <a name="limits"></a>Sınırlar

Arama hizmetinin en iyi performans ve kararlılığı sağlamak için, aşağıdaki boyutlarda kısıtlamalar uygulanır (arama hizmeti katmanına göre):

- *Özel*olarak ayarlanabilir Dizin oluşturucular türleri.
- Oluşturulabilen paylaşılan özel bağlantı kaynaklarının sayısı.
- Paylaşılan özel bağlantı kaynaklarının oluşturulabileceği farklı kaynak türleri sayısı.

Bu sınırlar [hizmet sınırları](search-limits-quotas-capacity.md)bölümünde belgelenmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- [IP güvenlik duvarları üzerinden Dizin Oluşturucu bağlantıları](search-indexer-howto-access-ip-restricted.md)
- [Güvenilen hizmet özel durumunu kullanarak Dizin Oluşturucu bağlantıları](search-indexer-howto-access-trusted-service-exception.md)
- [Özel bir uç noktaya Dizin Oluşturucu bağlantıları](search-indexer-howto-access-private.md)