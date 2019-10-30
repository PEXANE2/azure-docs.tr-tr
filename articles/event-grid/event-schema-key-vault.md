---
title: Azure Event Grid Azure Key Vault olay şeması
description: Azure Event Grid Azure Key Vault olaylar için belirtilen özellikleri ve şemayı açıklar
services: event-grid
author: msmbaldwin
ms.service: event-grid
ms.topic: reference
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: c92352dd28b870c5f58dec0b82a8000f14a8e62d
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033530"
---
# <a name="azure-event-grid-event-schema-for-azure-key-vault-preview"></a>Azure Key Vault için Azure Event Grid olay şeması (Önizleme)

Bu makalede, şu anda önizleme aşamasında olan [Azure Key Vault](../key-vault/index.yml) olayları için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure Key Vault bir hesap aşağıdaki olay türlerini yayar:

| Olay tam adı | Olay görünen adı | açıklama |
| ---------- | ----------- |---|
| Microsoft. Keykasası. Certificatenewversionoluşturuldu | Sertifika yeni sürümü oluşturuldu | Yeni sertifika veya yeni sertifika sürümü oluşturulduğunda tetiklenir |
| Microsoft. Keykasası. Certificatenearsüre sonu | Süresi dolmak üzere olan sertifika | Sertifikanın güncel sürümünün süresi sona ermek üzereyken tetiklenir (varsayılan süre, sona erme tarihinden 30 gün önce) |
| Microsoft. Keykasası. Certificatedolmuştur | Sertifikanın Süresi Doldu | Sertifikanın süre dolduğunda tetiklenir |
| Microsoft. Keykasası. Keynewversionoluşturuldu | Anahtar yeni sürüm oluşturuldu | Yeni anahtar veya yeni anahtar sürümü oluşturulduğunda tetiklenir |
| Microsoft. Keykasası. Keyyaklaştığında süre sonu | Süresi dolmak üzere olan anahtar | Anahtarın geçerli sürümünün süresi sona ermek üzereyken tetiklenir (varsayılan, sona erme tarihinden 30 gün önce) |
| Microsoft. Keykasası. Keyerdim | Anahtarın geçerliliği zaman aşımına uğradı | Anahtarın süre dolduğunda tetiklenir |
| Microsoft. Keykasası. Secretnewversionoluşturuldu | Gizli yeni sürüm oluşturuldu | Yeni gizli dizi veya yeni gizli dizi sürümü oluşturulduğunda tetiklenir |
| Microsoft. Keykasası. Secretyaklaştığında süre sonu | Süre sonu yakın gizli | Geçerli gizli sürümü sona ermek üzereyken tetiklenir (varsayılan süre, sona erme tarihinden 30 gün önce) |
| Microsoft. Keykasası. SecretExpired | Gizli dizi zaman aşımına uğradı | Gizli anahtar dolduğunda tetiklenir |

## <a name="event-examples"></a>Olay örnekleri

Aşağıdaki örnek, **Microsoft. Keykasası. SecretNewVersionCreated**için şemayı gösterir.

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

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| ---------- | ----------- |---|
| id | string | Bu olayı tetikleyen nesnenin KIMLIĞI. |
| vaultName | string | Bu olayı tetikleyen nesnenin Anahtar Kasası adı. |
| Nesne | string | Bu olayı tetikleyen nesnenin türü |
| ObjectName | string | Bu olayı tetikleyen nesnenin adı |
| version | string | Bu olayı tetikleyen nesnenin sürümü |
| NBF | number | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z 'Den bu yana geçen tarihten önce değil |
| Exp | number | Bu olayı tetikleyen nesnenin 1970-01-01T00:00:00Z 'den bu yana geçen sona erme tarihi |


## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Key Vault/Event Grid tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Event Grid Ile izleme Key Vault (Önizleme)](../key-vault/event-grid-overview.md)
* Key Vault/Event Grid tümleştirmede bir öğreticiyi görmek için bkz. [nasıl yapılır: Key Vault olaylarını Automation runbook 'A yönlendirme (Önizleme)](../key-vault/event-grid-tutorial.md).

- [Azure Key Vault genel bakış](../key-vault/key-vault-overview.md)
- [Azure Event Grid genel bakış](overview.md)
- [Azure Event Grid ile Key Vault izleme (Önizleme)](../key-vault/event-grid-overview.md)
- [Nasıl yapılır: Key Vault olaylarını Automation runbook 'A yönlendirme (Önizleme)](../key-vault/event-grid-tutorial.md).
- [Azure Otomasyonu’na genel bakış](../automation/index.yml)
