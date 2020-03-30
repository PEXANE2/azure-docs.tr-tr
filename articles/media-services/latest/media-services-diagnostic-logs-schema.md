---
title: Azure Medya Hizmetleri tanılama şemaları - Azure
description: Bu makalede, Azure Medya Hizmetleri tanılama şemaları gösteriş.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2019
ms.author: juliako
ms.openlocfilehash: 37baed076ca074c1d558af36649e90959a0034c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750881"
---
# <a name="diagnostic-logs-schemas"></a>Tanılama günlükleri şemaları

[Azure Monitor,](../../azure-monitor/overview.md) uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak tanır. Medya Hizmetleri tanı günlüklerini izleyebilir ve toplanan ölçümler ve günlükler için uyarılar ve bildirimler oluşturabilirsiniz. Günlükleri [Azure Depolama'ya](https://azure.microsoft.com/services/storage/)gönderebilir, [Azure Etkinlik Hub'larına](https://azure.microsoft.com/services/event-hubs/)aktarabilir ve [Günlük Analitiği'ne](https://azure.microsoft.com/services/log-analytics/)aktarabilir veya üçüncü taraf hizmetlerini kullanabilirsiniz.

Ayrıntılı bilgi için Azure [Monitör Ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure Monitör Tanılama günlüklerine](../../azure-monitor/platform/platform-logs-overview.md)bakın.

Bu makalede, Medya Hizmetleri tanılama şemaları açıklanmaktadır.

## <a name="top-level-diagnostic-logs-schema"></a>Üst düzey tanı günlükleri şeması

Üst düzey tanılama günlükleri şemasının ayrıntılı açıklaması [için, Azure Tanı Günlükleri için Desteklenen hizmetler, şemalar ve kategorilere](../../azure-monitor/platform/tutorial-dashboards.md)bakın.

## <a name="key-delivery-log-schema"></a>Anahtar teslim günlüğü şeması

### <a name="properties"></a>Özellikler

Bu özellikler, anahtar teslim günlüğü şemasına özgüdir.

|Adı|Açıklama|
|---|---|
|keyId|İstenen anahtarın kimliği.|
|Keytype|Aşağıdaki değerlerden biri olabilir: "Clear" (şifreleme yok), "FairPlay", "PlayReady" veya "Widevine".|
|politikaAd|İlkenin Azure Kaynak Yöneticisi adı.|
|Tokentype|Belirteç türü.|
|Statusmessage|Durum iletisi.|

### <a name="examples"></a>Örnekler

Anahtar teslimin özellikleri şema istekleri.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri ölçümlerini ve tanılama günlüklerini izleyin](media-services-metrics-diagnostic-logs.md)
