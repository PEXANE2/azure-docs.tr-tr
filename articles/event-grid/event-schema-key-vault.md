---
title: Event Grid kaynak olarak Azure Key Vault
description: Azure Event Grid Azure Key Vault olaylar için belirtilen özellikleri ve şemayı açıklar
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 1f9cbe85de9423484343e4054be8d2d58c6c5e7e
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109442"
---
# <a name="azure-key-vault-as-event-grid-source"></a>Event Grid kaynak olarak Azure Key Vault

Bu makalede, şu anda önizleme aşamasında olan [Azure Key Vault](../key-vault/index.yml)olaylar için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md).

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir Azure Key Vault hesabı aşağıdaki olay türlerini üretir:

| Olay tam adı | Olay görünen adı | Description |
| ---------- | ----------- |---|
| Microsoft. Keykasası. Certificatenewversionoluşturuldu | Sertifika yeni sürümü oluşturuldu | Yeni bir sertifika veya yeni sertifika sürümü oluşturulduğunda tetiklenir. |
| Microsoft. Keykasası. Certificatenearsüre sonu | Süresi dolmak üzere olan sertifika | Sertifikanın güncel sürümü sona ermek üzereyken tetiklenir. (Olay, sona erme tarihinden itibaren 30 gün önce tetiklenir.) |
| Microsoft. Keykasası. Certificatedolmuştur | Sertifikanın Süresi Doldu | Sertifikanın süre dolduğunda tetiklenir. |
| Microsoft. Keykasası. Keynewversionoluşturuldu | Anahtar yeni sürüm oluşturuldu | Yeni bir anahtar veya yeni anahtar sürümü oluşturulduğunda tetiklenir. |
| Microsoft. Keykasası. Keyyaklaştığında süre sonu | Süresi dolmak üzere olan anahtar | Anahtarın geçerli sürümünün kullanım süreleri dolduğunda tetiklenir. (Olay, sona erme tarihinden itibaren 30 gün önce tetiklenir.) |
| Microsoft. Keykasası. Keyerdim | Anahtarın geçerliliği zaman aşımına uğradı | Anahtarın süre dolduğunda tetiklenir. |
| Microsoft. Keykasası. Secretnewversionoluşturuldu | Gizli yeni sürüm oluşturuldu | Yeni bir gizli dizi veya yeni bir gizli dizi sürümü oluşturulduğunda tetiklenir. |
| Microsoft. Keykasası. Secretyaklaştığında süre sonu | Süre sonu yakın gizli | Bir parolanın geçerli sürümü sona ermek üzereyken tetiklenir. (Olay, sona erme tarihinden itibaren 30 gün önce tetiklenir.) |
| Microsoft. Keykasası. SecretExpired | Gizli dizi zaman aşımına uğradı | Gizli dizi dolduğunda tetiklenir. |

### <a name="event-examples"></a>Olay örnekleri

Aşağıdaki örnek, **Microsoft. Keykasası. Secretnewversiontarafından oluşturulan**şemayı göstermektedir:

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

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Description |
| ---------- | ----------- |---|
| kimlik | string | Bu olayı tetikleyen nesnenin KIMLIĞI |
| vaultName | string | Bu olayı tetikleyen nesnenin Anahtar Kasası adı |
| Nesne | string | Bu olayı tetikleyen nesnenin türü |
| objectName | string | Bu olayı tetikleyen nesnenin adı |
| sürüm | string | Bu olayı tetikleyen nesnenin sürümü |
| NBF | sayı | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z 'den bu yana saniye cinsinden olmayan Tarih |
| exp | sayı | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z 'den bu yana geçen sona erme tarihi |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Azure Event Grid ile Key Vault olaylarını izleme](../key-vault/general/event-grid-overview.md) | Key Vault Event Grid tümleştirilmesine genel bakış. |
| [Öğretici: Event Grid ile Key Vault olayları oluşturma ve izleme](../key-vault/general/event-grid-tutorial.md) | Key Vault için Event Grid bildirimleri ayarlamayı öğrenin. |


## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. Event Grid nedir [?](overview.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Event Grid ile Key Vault tümleştirme hakkında daha fazla bilgi için bkz. [Azure Event Grid Ile izleme Key Vault (Önizleme)](../key-vault/general/event-grid-overview.md).
* Event Grid Key Vault tümleştirme hakkında bir öğretici için bkz. [Azure Event Grid (Önizleme) ile Anahtar Kasası bildirimleri alma ve yanıtlama](../key-vault/general/event-grid-tutorial.md).
* Key Vault ve Azure Otomasyonu hakkında ek rehberlik almak için, bkz.:
    - [Azure Anahtar Kasası nedir?](../key-vault/general/overview.md)
    - [Azure Event Grid ile Key Vault izleme (Önizleme)](../key-vault/general/event-grid-overview.md)
    - [Azure Event Grid ile Anahtar Kasası bildirimlerini alma ve yanıtlama (Önizleme)](../key-vault/general/event-grid-tutorial.md)
    - [Azure Otomasyonu’na genel bakış](../automation/index.yml)
