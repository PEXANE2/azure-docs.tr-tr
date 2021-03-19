---
title: Media Services veri başvurusunu izleme
description: Media Services izlerken gereken önemli başvuru malzemeleri
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femila
ms.topic: reference
ms.service: media-services
ms.date: 03/17/2021
ms.openlocfilehash: 866b2faf473f06fc3f85cdb434d6555504a7f6a8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598193"
---
# <a name="monitoring-media-services-data-reference"></a>Media Services veri başvurusunu izleme

Bu makale Media Services izlemek için yararlı olan verileri içerir. Azure Izleyici 'de desteklenen tüm platform ölçümleri hakkında daha fazla bilgi için [Azure izleyici Ile desteklenen ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)gözden geçirin.

## <a name="media-services-metrics"></a>Media Services ölçümleri

Ölçümler, değerin değiştirilip değişmediğini düzenli aralıklarla toplanır. Bunlar genelde örneklenebilir ve bir uyarı görece basit mantık ile hızlı bir şekilde tetiklenebilir.

Media Services aşağıdaki kaynaklar için izleme ölçümlerini destekler:

* Hesap
* Akış uç noktası

### <a name="account"></a>Hesap

Aşağıdaki hesap ölçümlerini izleyebilirsiniz.

|Ölçüm adı|Görünen ad|Description|
|---|---|---|
|AssetCount|Varlık sayısı|Hesabınızdaki varlıklar.|
|AssetQuota|Varlık kotası|Hesabınızdaki varlık kotası.|
|AssetQuotaUsedPercentage|Kullanılan varlık kotası yüzdesi|Zaten kullanılan varlık kotasının yüzdesi.|
|ContentKeyPolicyCount|İçerik anahtarı Ilke sayısı|Hesabınızdaki içerik anahtarı Ilkeleri.|
|ContentKeyPolicyQuota|İçerik anahtarı Ilke kotası|Hesabınızdaki içerik anahtarı Ilkeleri kotası.|
|ContentKeyPolicyQuotaUsedPercentage|İçerik anahtarı Ilke kotası kullanılan yüzde|Zaten kullanılan Içerik anahtarı Ilke kotasının yüzdesi.|
|Streammingpolicycount|Akış Ilkesi sayısı|Hesabınızdaki akış Ilkeleri.|
|StreamingPolicyQuota|Akış Ilkesi kotası|Hesabınızdaki akış Ilkeleri kotası.|
|StreamingPolicyQuotaUsedPercentage|Akış Ilkesi kotası kullanılan yüzde|Zaten kullanılan akış Ilkesi kotasının yüzdesi.|

[Hesap kotalarını ve sınırlarını](../limits-quotas-constraints.md)de gözden geçirmeniz gerekir.

### <a name="streaming-endpoint"></a>Akış uç noktası

Aşağıdaki Media Services [akış uç noktası](/rest/api/media/streamingendpoints) ölçümleri desteklenir:

|Ölçüm adı|Görünen ad|Description|
|---|---|---|
|İstekler|İstekler|Akış uç noktası tarafından hizmet verilen toplam HTTP isteği sayısını sağlar.|
|Çıkış|Çıkış|Akış uç noktası başına dakika başına toplam çıkış baytı.|
|SuccessE2ELatency|Başarılı uçtan uca gecikme süresi|Akış uç noktasının, yanıtın son baytı gönderilirken isteği aldığı zaman süresi.|
|CPU kullanımı| | Premium akış uç noktaları için CPU kullanımı. Bu veriler standart akış uç noktaları için kullanılamaz. |
|Çıkış bant genişliği | | Bit/saniye cinsinden çıkış bant genişliği.|

## <a name="metric-dimensions"></a>Ölçüm boyutları

Ölçüm boyutları hakkında daha fazla bilgi için bkz. [çok boyutlu ölçümler](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#multi-dimensional-metrics).

<!--**PLACEHOLDER** for dimensions table.-->

## <a name="resource-logs"></a>Kaynak günlükleri

## <a name="media-services-diagnostic-logs"></a>Tanılama günlüklerini Media Services

Tanılama günlükleri, bir Azure kaynağının çalışması hakkında zengin ve sık veriler sağlar. Daha fazla bilgi için bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve kullanma](https://docs.microsoft.com/azure/azure-monitor/essentials/platform-logs-overview.md).

Media Services aşağıdaki tanılama günlüklerini destekler:

* Anahtar teslimi

### <a name="key-delivery"></a>Anahtar teslimi

|Ad|Açıklama|
|---|---|
|Anahtar teslim hizmeti isteği|Anahtar teslim hizmeti istek bilgilerini gösteren Günlükler. Daha fazla bilgi için bkz. [şemalar](monitor-media-services-data-reference.md).|

## <a name="schemas"></a>Şemalar

Üst düzey tanılama günlükleri şemasının ayrıntılı açıklaması için bkz. [Azure tanılama günlükleri Için desteklenen hizmetler, şemalar ve Kategoriler](https://docs.microsoft.com/azure/azure-monitor/essentials/resource-logs-schema.md).

## <a name="key-delivery-log-schema-properties"></a>Anahtar teslim günlüğü şema özellikleri

Bu özellikler, anahtar teslim günlüğü şemasına özeldir.

|Ad|Açıklama|
|---|---|
|keyId|İstenen anahtarın KIMLIĞI.|
|Anahtar|Şu değerlerden biri olabilir: "Clear" (şifreleme yok), "FairPlay", "PlayReady" veya "Widevine".|
|policyName|İlkenin Azure Resource Manager adı.|
|Belirteç|Belirteç türü.|
|statusMessage|Durum iletisi.|

### <a name="example"></a>Örnek

Anahtar teslim istekleri şemasının özellikleri.

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

>[!NOTE]
> Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]
