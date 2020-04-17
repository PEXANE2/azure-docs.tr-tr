---
title: Olay Izgara kaynağı olarak Azure Anahtar Kasası
description: Azure Olay Ağıtı ile Azure Anahtar Kasası etkinlikleri için sağlanan özellikleri ve şema açıklanır
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: spelluru
ms.openlocfilehash: 40bff9585e64163039a8847ff868c982ffb20414
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458258"
---
# <a name="azure-key-vault-as-event-grid-source"></a>Olay Izgara kaynağı olarak Azure Anahtar Kasası

Bu makalede, şu anda önizlemede olan [Azure Anahtar Kasası'ndaki](../key-vault/index.yml)olaylar için özellikler ve şema sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Anahtar Kasası hesabı aşağıdaki olay türlerini oluşturur:

| Olay tam adı | Olay görüntü adı | Açıklama |
| ---------- | ----------- |---|
| Microsoft.KeyVault.CertificateNewVersionCreated | Sertifika Yeni Sürüm Oluşturuldu | Yeni bir sertifika veya yeni sertifika sürümü oluşturulduğunda tetiklenir. |
| Microsoft.KeyVault.CertificateNearExpiry | Son Kullanma Tarihine Yakın Sertifika | Sertifikanın geçerli sürümünün süresi dolmak üzereyken tetiklenir. (Olay son kullanma tarihinden 30 gün önce tetiklenir.) |
| Microsoft.KeyVault.CertificateExpired | Sertifikanın Süresi Doldu | Sertifikanın süresi dolduğunda tetiklenir. |
| Microsoft.keyvault.keynewversionCreated | Anahtar Yeni Sürüm Oluşturuldu | Yeni bir anahtar veya yeni anahtar sürümü oluşturulduğunda tetiklenir. |
| Microsoft.KeyVault.KeyNearExpiry | Son Kullanma Tarihine Yakın Anahtar | Anahtarın geçerli sürümünün süresi dolmak üzereyken tetiklenir. (Olay son kullanma tarihinden 30 gün önce tetiklenir.) |
| Microsoft.KeyVault.KeyExpired | Anahtar Süresi Doldu | Bir anahtarın süresi dolduğunda tetiklenir. |
| Microsoft.keyvault.secretnewversionCreated | Gizli Yeni Sürüm Oluşturuldu | Yeni bir gizli veya yeni gizli sürüm oluşturulduğunda tetiklenir. |
| Microsoft.KeyVault.SecretNearExpiry | Sona Erme Tarihine Yakın Gizli | Bir sırrın geçerli sürümünün süresi dolmak üzereyken tetiklenir. (Olay son kullanma tarihinden 30 gün önce tetiklenir.) |
| Microsoft.KeyVault.SecretExpired | Gizli Süresi Doldu | Bir sırrın süresi dolduğunda tetiklenir. |

### <a name="event-examples"></a>Olay örnekleri

Aşağıdaki örnek **Microsoft.KeyVault.SecretNewVersionCreated**için şema göstermek:

```JSON
[
   {
      "id":"00eccf70-95a7-4e7c-8299-2eb17ee9ad64",
      "topic":"/subscriptions/{subscription-id}/resourceGroups/sample-rg/providers/Microsoft.KeyVault/vaults/sample-kv",
      "subject":"newsecret",
      "eventType":"Microsoft.KeyVault.SecretNewVersionCreated",
      "eventTime":"2019-07-25T01:08:33.1036736Z",
      "data":{
         "Id":"https://sample-kv.vault.azure.net/secrets/newsecret/ee059b2bb5bc48398a53b168c6cdcb10",
         "vaultName":"sample-kv",
         "objectType":"Secret",
         "objectName ":"newsecret",
         "version":" ee059b2bb5bc48398a53b168c6cdcb10",
         "nbf":"1559081980",
         "exp":"1559082102"
      },
      "dataVersion":"1",
      "metadataVersion":"1"
   }
]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| ---------- | ----------- |---|
| id | string | Bu olayı tetikleyen nesnenin kimliği |
| vaultName | string | Bu olayı tetikleyen nesnenin anahtar kasa adı |
| Nesnetürü | string | Bu olayı tetikleyen nesnenin türü |
| Nesneadı | string | Bu olayı tetikleyen nesnenin adı |
| version | string | Bu olayı tetikleyen nesnenin sürümü |
| nbf | number | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z'den saniyeler önce olmayan tarih |
| Exp | number | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z'den saniyeler içinde son kullanma tarihi |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Azure Olay Ağıt'ı ile Anahtar Kasa etkinliklerini izleme](../key-vault/general/event-grid-overview.md) | Key Vault'u Olay Izgarasıyla tümleştirmeye genel bakış. |
| [Öğretici: Olay Izgarası ile Key Vault etkinliklerini oluşturun ve izleyin](../key-vault/general/event-grid-tutorial.md) | Key Vault için Olay Ağı bildirimlerini nasıl ayarlayatılamayı öğrenin. |


## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için olay [ızgarası nedir'e bakın?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Olay Grid ile Key Vault tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Olay Grid 'li Anahtar Kasasını İzleme (önizleme)](../key-vault/general/event-grid-overview.md)konusuna bakın.
* Olay Grid ile Key Vault tümleştirmesi hakkında bir öğretici için, [Azure Olay Grid (önizleme) ile Anahtar Kasa bildirimlerini Al ve yanıtla'](../key-vault/general/event-grid-tutorial.md)ya bakın.
* Key Vault ve Azure Otomasyonu için ek kılavuz lar almak için bkz:
    - [Azure Anahtar Kasası nedir?](../key-vault/general/overview.md)
    - [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](../key-vault/general/event-grid-overview.md)
    - [Azure Olay Ağı (önizleme) ile önemli kasa bildirimlerini alma ve yanıtla](../key-vault/general/event-grid-tutorial.md)
    - [Azure Otomasyonu’na genel bakış](../automation/index.yml)
