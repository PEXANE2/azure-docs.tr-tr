---
title: Dizin Oluşturucu kaynaklarına güvenli bir şekilde erişin
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de Dizin oluşturucular tarafından Azure veri erişimi için ağ düzeyinde güvenlik seçeneklerine kavramsal genel bakış.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 5075c4858f9584cb19442e19d9009d46d0e00ff8
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89463909"
---
# <a name="indexer-access-to-data-sources-using-azure-network-security-features"></a>Azure ağ güvenlik özellikleri kullanılarak veri kaynaklarına Dizin Oluşturucu erişimi

Azure Bilişsel Arama Dizin oluşturucular, yürütme sırasında çeşitli Azure kaynaklarına giden çağrılar yapabilir. Bu makalede, bu kaynaklar IP güvenlik duvarları, Özel uç noktaları ve diğer ağ düzeyi güvenlik mekanizmaları tarafından korunduğunda, kaynaklara dizin oluşturucunun arkasındaki kavramlar açıklanmaktadır. Bir dizin oluşturucunun tipik bir çalıştırmada erişebileceği olası kaynak türleri aşağıdaki tabloda listelenmiştir.

| Kaynak | Dizin Oluşturucu çalıştırma içinde amaç |
| --- | --- |
| Azure depolama (blob 'lar, tablolar, ADLS Gen 2) | Veri kaynağı |
| Azure depolama (blob 'lar, tablolar) | Becerileri (zenginleştirilmiş belgeleri önbelleğe alma ve bilgi deposu projeksiyonlarını depolama) |
| Azure Cosmos DB (çeşitli API 'Ler) | Veri kaynağı |
| Azure SQL Veritabanı | Veri kaynağı |
| Azure IaaS VM 'lerinde SQL Server | Veri kaynağı |
| SQL yönetilen örnekleri | Veri kaynağı |
| Azure İşlevleri | Özel Web API becerileri için ana bilgisayar |
| Bilişsel Hizmetler | 20 ücretsiz belge sınırının ötesinde zenginleştirme yapmak için kullanılacak beceri 'ye eklendi |

> [!NOTE]
> Bir beceri 'e bağlı bilişsel hizmet kaynağı, gerçekleştirilen ve arama dizinine yazılan enzenginler temelinde faturalandırma için kullanılır. Bilişsel Hizmetler API'si erişmek için kullanılmaz. Bir dizin oluşturucunun enzenginleştirme ardışık düzeninde, verilerin aktarım sırasında güçlü şekilde şifrelendiği ve hiçbir zaman bekleyen olarak depolanabileceği güvenli bir iletişim kanalı aracılığıyla Bilişsel Hizmetler API'si oluşur.

Müşteriler, Azure tarafından sunulan çeşitli ağ yalıtımı mekanizmaları aracılığıyla bu kaynakları güvenli hale getirebilirler. Bilişsel hizmet kaynağı hariç olmak üzere, Dizin oluşturucular, aşağıdaki tabloda özetlenen ağ yalıtılmış olsalar bile diğer tüm kaynaklara erişim için sınırlı bir becerisine sahiptir.

| Kaynak | IP kısıtlaması | Özel uç nokta |
| --- | --- | ---- |
| Azure depolama (blob 'lar, tablolar, ADLS Gen 2) | Yalnızca depolama hesabı ve arama hizmeti farklı bölgelerde olduğunda desteklenir | Desteklenir |
| Azure Cosmos DB-SQL API 'SI | Desteklenir | Desteklenir |
| Azure Cosmos DB-Cassandra, Mongo ve Gremlin API 'SI | Desteklenir | Desteklenmeyen |
| Azure SQL Veritabanı | Desteklenir | Desteklenir |
| Azure IaaS VM 'lerinde SQL Server | Desteklenir | Yok |
| SQL yönetilen örnekleri | Desteklenir | Yok |
| Azure İşlevleri | Desteklenir | Yalnızca belirli Azure işlevleri SKU 'Ları için desteklenir |

> [!NOTE]
> Kullanıcılar, yukarıda listelenen seçeneklere ek olarak, ağ güvenliği sağlanmış Azure depolama hesapları için Azure Bilişsel Arama 'nin [güvenilir bir Microsoft hizmeti](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services)olduğu gerçeden yararlanabilir. Bu, belirli bir arama hizmetinin depolama hesabındaki sanal ağ veya IP kısıtlamalarını atlayabileceği ve depolama hesabında uygun rol tabanlı erişim denetimi etkinse depolama hesabındaki verilere erişebileceği anlamına gelir. Ayrıntılar [nasıl yapılır kılavuzunda](search-indexer-howto-access-trusted-service-exception.md)bulunabilir. Depolama hesabı ya da arama hizmeti farklı bir bölgeye taşınamadığından, bu seçenek IP kısıtlama yolu yerine kullanılabilir.

Bir dizin oluşturucunun kullanması gereken güvenli erişim mekanizmasını seçerken aşağıdaki kısıtlamaları göz önünde bulundurun:

- [Hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) , hiçbir Azure kaynağı için desteklenmez.
- Arama hizmeti belirli bir sanal ağa sağlanamıyor-Bu işlevsellik Azure Bilişsel Arama tarafından sunulmayacaktır.
- Dizin oluşturucular, kaynaklara erişmek için özel uç noktaları (giden) kullanır, ek [özel bağlantı ücretleri](https://azure.microsoft.com/pricing/details/search/) de uygulanabilir.

## <a name="indexer-execution-environment"></a>Dizin Oluşturucu yürütme ortamı

Azure Bilişsel Arama Dizin oluşturucular, veri kaynaklarından içerik ayıklayarak, ayıklanan içeriğe zenginler ekleyerek ve isteğe bağlı olarak sonuçları arama dizinine yazmadan önce projeksiyonlar oluşturuyor. Bir dizin oluşturucuya atanan sorumlulukların sayısına bağlı olarak, iki ortamdan birinde çalışabilir:

- Belirli bir arama hizmeti için özel bir ortam. Bu tür ortamlarda çalışan Dizin oluşturucular, kaynakları diğer iş yükleri (diğer müşteri tarafından başlatılan dizin oluşturma veya sorgulama iş yükü gibi) paylaşır. Genellikle, bu ortamda yalnızca pek çok kaynak gerektirmeyen (örneğin, Beceri kullanmayın) Dizin oluşturucular.
- Kaynak arayana (beceri gibi) dizin oluşturma olanlarından oluşan, çok kiracılı bir ortam. Kaynak araması kaynakları bu ortamda çalışır ve arama hizmeti kaynaklarının diğer iş yükleri için kullanılabilir olmasını sağlamaya yönelik en iyi performansı sunar. Bu çok kiracılı ortam, hiçbir ek maliyet olmadan Azure Bilişsel Arama tarafından yönetilir ve korunur.

Belirli bir Dizin Oluşturucu çalıştırması için Azure Bilişsel Arama, dizin oluşturucunun çalıştırılacağı en iyi ortamı belirler.

## <a name="granting-access-to-indexer-ip-ranges"></a>Dizin Oluşturucu IP aralıklarına erişim verme

Dizin oluşturucunun erişmeye çalıştığı kaynak yalnızca belirli bir IP aralığı kümesiyle sınırlı ise, Dizin Oluşturucu isteğinin ulaşabileceği olası IP aralıklarını dahil etmek için kümeyi genişletmeniz gerekir. Yukarıda belirtildiği gibi, dizin oluşturucularının çalıştığı ve hangi erişim isteklerinin kaynaklanbileceği iki olası ortam vardır. Dizin Oluşturucu erişimi için __her iki__ ortamın IP adresini de eklemeniz gerekir.

- Arama hizmetine özgü özel ortamın IP adresini `nslookup` (veya `ping` ) arama hizmetinizin tam etki alanı adını (FQDN) almak için. Genel buluttaki bir arama hizmetinin FQDN 'SI (örneğin,) `<service-name>.search.windows.net` . Bu bilgiler Azure portal kullanılabilir.
- Çok kiracılı ortamların IP adresleri, `AzureCognitiveSearch` hizmet etiketi aracılığıyla kullanılabilir. [Azure hizmet etiketlerinin](https://docs.microsoft.com/azure/virtual-network/service-tags-overview) her hizmet için YAYıMLANMıŞ bir IP adresi aralığı vardır. Bu, bir [bulma API 'si (Önizleme)](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview) veya [indirilebilir bir JSON dosyası](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files)aracılığıyla sağlanır. Her iki durumda da IP aralıkları bölgeye göre bölünür-yalnızca arama hizmetinizin sağlandığı bölge için atanan IP aralıklarını seçebilirsiniz.

Belirli veri kaynakları için, hizmet etiketinin kendisi, IP aralıklarının listesini numaralandırmak yerine doğrudan kullanılabilir (Arama hizmetinin IP adresinin hala açıkça kullanılması gerekir). Bu veri kaynakları, bir hizmet etiketi eklemeyi, Azure Storage, CosmosDB, Azure SQL gibi IP kurallarından farklı olarak destekleyen bir [ağ güvenlik grubu kuralı](https://docs.microsoft.com/azure/virtual-network/security-overview)ayarlama yoluyla erişimi kısıtlar, ancak `AzureCognitiveSearch` Arama hizmeti IP adresine ek olarak, hizmet etiketini doğrudan kullanma özelliğini destekleyen veri kaynakları şunlardır:

- [IaaS VM 'lerinde SQL Server](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers#restrict-access-to-the-azure-cognitive-search)

- [SQL yönetilen örnekler](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers#verify-nsg-rules)

Ayrıntılar [nasıl yapılır kılavuzunda](search-indexer-howto-access-ip-restricted.md)açıklanmaktadır.

## <a name="granting-access-via-private-endpoints"></a>Özel uç noktalar aracılığıyla erişim verme

Dizin oluşturucular, kaynaklara erişmek için [Özel uç noktaları](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilir, sanal ağlar seçmek veya hiçbir ortak erişim özelliği etkinleştirilmemiş olarak kilitlenir.
Bu işlevsellik yalnızca ücretli hizmetler için kullanılabilir ve bu, oluşturulan özel uç nokta sayısıyla sınırlı olur. Limitlerin ayrıntıları [Azure Search sınırları sayfasında](search-limits-quotas-capacity.md)belgelenmiştir.

### <a name="step-1-create-a-private-endpoint-to-the-secure-resource"></a>1. Adım: güvenli kaynak için özel bir uç nokta oluşturma

Müşteriler, güvenli kaynaklarına (örneğin, bir depolama hesabı) özel bir uç nokta bağlantısı oluşturmak için arama yönetimi işlemini çağırmalıdır, [ *paylaşılan özel bağlantı kaynak* API 'Sini oluşturur veya güncelleştirir](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate) . Bu (giden) özel uç nokta bağlantısının üzerinden geçen trafik yalnızca arama hizmeti 'ne özgü "özel" Dizin Oluşturucu yürütme ortamındaki sanal ağdan kaynaklanacaktır.

Azure Bilişsel Arama, bu API 'nin çağıranlarının güvenli kaynağa özel uç nokta bağlantı isteklerini onaylama izinlerine sahip olduğunu doğrular. Örneğin, erişiminiz olmayan bir depolama hesabına özel bir uç nokta bağlantısı istemeniz durumunda bu çağrı reddedilir.

### <a name="step-2-approve-the-private-endpoint-connection"></a>2. Adım: özel uç nokta bağlantısını onaylama

Paylaşılan bir özel bağlantı kaynağı oluşturan (zaman uyumsuz) işlem tamamlandığında, "bekleyen" durumunda özel bir uç nokta bağlantısı oluşturulur. Henüz bağlantı üzerinden trafik akışı yok.
Daha sonra müşterinin bu isteği güvenli kaynağında bulması ve "onaylaması" beklenir. Genellikle, bu, Portal aracılığıyla veya [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/privatelinkservices/updateprivateendpointconnection)aracılığıyla yapılabilir.

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

Bu adımlar, [nasıl yapılır kılavuzunda](search-indexer-howto-access-private.md)daha ayrıntılı olarak açıklanmıştır.
Bir kaynağa onaylanan özel uç nokta aldıktan sonra, Özel uç nokta bağlantısı aracılığıyla *özel* olarak erişim denemesi olarak ayarlanan Dizin oluşturucular.

### <a name="limits"></a>Sınırlar

Arama hizmetinin en iyi performans ve kararlılığı sağlamak için, aşağıdaki boyutlarda kısıtlamalar uygulanır (arama hizmeti SKU 'SU tarafından):

- *Özel*olarak ayarlanabilir Dizin oluşturucular türleri.
- Oluşturulabilen paylaşılan özel bağlantı kaynaklarının sayısı.
- Paylaşılan özel bağlantı kaynaklarının oluşturulabileceği farklı kaynak türleri sayısı.

Bu sınırlar [hizmet sınırları](search-limits-quotas-capacity.md)bölümünde belgelenmiştir.
