---
title: Azure Anahtar Kasası için Azure Olay Izgara etkinliği şeması
description: Azure Olay Ağıtı ile Azure Anahtar Kasası etkinlikleri için sağlanan özellikleri ve şema açıklanır
services: event-grid
author: msmbaldwin
ms.service: event-grid
ms.topic: reference
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: fe186e2ba8f3cafeb4d186066ba65ae036302f70
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010520"
---
# <a name="azure-event-grid-event-schema-for-azure-key-vault-preview"></a>Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)

Bu makalede, şu anda önizlemede olan [Azure Anahtar Kasası'ndaki](../key-vault/index.yml)olaylar için özellikler ve şema sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

## <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

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

## <a name="event-examples"></a>Olay örnekleri

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

## <a name="event-properties"></a>Olay özellikleri

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


## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için olay [ızgarası nedir'e bakın?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Olay Grid ile Key Vault tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Olay Grid 'li Anahtar Kasasını İzleme (önizleme)](../key-vault/event-grid-overview.md)konusuna bakın.
* Olay Grid ile Key Vault tümleştirmesi hakkında bir öğretici için, [Azure Olay Grid (önizleme) ile Anahtar Kasa bildirimlerini Al ve yanıtla'](../key-vault/event-grid-tutorial.md)ya bakın.
* Key Vault ve Azure Otomasyonu için ek kılavuz lar almak için bkz:
    - [Azure Anahtar Kasası nedir?](../key-vault/key-vault-overview.md)
    - [Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)](../key-vault/event-grid-overview.md)
    - [Azure Olay Ağı (önizleme) ile önemli kasa bildirimlerini alma ve yanıtla](../key-vault/event-grid-tutorial.md)
    - [Azure Otomasyonu’na genel bakış](../automation/index.yml)
