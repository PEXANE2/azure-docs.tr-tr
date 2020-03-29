---
title: Azure Olay Grid IoT Edge'de çıktı toplu işlemesi | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de çıkış toplu işlemi.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: a6f033af34088081090251f2e5e7cd4a07ce43cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841756"
---
# <a name="output-batching"></a>Çıkışı toplu işleme

Olay Grid tek bir teslim isteği birden fazla olay sunmak için destek vardır. Bu özellik, isteğe düşen http genel giderlerini ödemeden genel teslimat iş buzunu artırmayı mümkün kılar. Toplu iş varsayılan olarak kapatılır ve abonelik başına açılabilir.

> [!WARNING]
> Abone kodu toplu istek başına daha fazla iş yapmak zorunda olsa da, her teslim isteğini işlemek için izin verilen maksimum süre değişmez. Teslim zaman önceliği varsayılan olarak 60 saniyeye kadar dır.

## <a name="batching-policy"></a>Toplu iş ilkesi

Olay Grid'in toplu iş davranışı, aşağıdaki iki ayarı değiştirerek abone başına özelleştirilebilir:

* Toplu iş başına maksimum olaylar

  Bu ayar, toplu teslim isteğine eklenebilecek olay sayısına bir üst sınır belirler.

* Kilobaytlarda Tercih Edilen Toplu Boyut

  Bu tonlama, teslimat isteği başına gönderilebilen maksimum kilobayt sayısını daha fazla kontrol etmek için kullanılır

## <a name="batching-behavior"></a>Toplu işleme davranışı

* Tümü veya hiçbiri

  Olay Izgara all-or-none semantik ile çalışır. Toplu teslimatın kısmi başarısını desteklemez. Aboneler, 60 saniye içinde makul bir şekilde işlenebildikleri kadar olay istemek için dikkatli olmalıdır.

* İyimser toplu iş

  Toplu iş ilkesi ayarları, toplu iş davranışına bağlı olarak katı sınırlar değildir ve en iyi çaba temelinde saygı duyulur. Düşük etkinlik oranlarında, genellikle toplu iş boyutunun toplu iş başına istenen maksimum olaylardan daha az olduğunu gözlemlersiniz.

* Varsayılan, OFF olarak ayarlanır

  Varsayılan olarak, Olay Izgarası her teslim isteğine yalnızca bir olay ekler. Toplu işlemi açmanın yolu, json olayının daha önce makalede belirtilen ayarlardan birini ayarlamaktır.

* Varsayılan değerler

  Bir olay aboneliği oluştururken hem ayarları (toplu iş başına maksimum olay ve kilo baytbaşına yaklaşık toplu iş boyutu) belirtmeniz gerekmez. Yalnızca bir ayar ayarlanırsa, Olay Izgaravarsayılan değerlerini (yapılandırılabilir) kullanır. Varsayılan değerler ve bunları nasıl geçersiz kılabilir için aşağıdaki bölümlere bakın.

## <a name="turn-on-output-batching"></a>Çıktı toplu işlemeyi açma

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
             {
                "endpointUrl": "<your_webhook_url>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 64
             }
        },
    }
}
```

## <a name="configuring-maximum-allowed-values"></a>İzin verilen maksimum değerleri yapılandırma

Aşağıdaki dağıtım süresi ayarları, olay aboneliği oluştururken izin verilen maksimum değeri denetler.

| Özellik Adı | Açıklama |
| ------------- | ----------- | 
| `api__deliveryPolicyLimits__maxpreferredBatchSizeInKilobytes` | Tonlama `PreferredBatchSizeInKilobytes` için izin verilen maksimum değer. Varsayılan `1033`değer.
| `api__deliveryPolicyLimits__maxEventsPerBatch` | Tonlama `MaxEventsPerBatch` için izin verilen maksimum değer. Varsayılan `50`değer.

## <a name="configuring-runtime-default-values"></a>Çalışma zamanı varsayılan değerlerini yapılandırma

Aşağıdaki dağıtım süresi ayarları, Olay Aboneliği'nde belirtilmediğinde her bir kunonun çalışma zamanı varsayılan değerini denetler. Yinelemek için, toplu iş davranışını açmak için Olay Aboneliği'nde en az bir bir düğüm ayarlanmalıdır.

| Özellik Adı | Açıklama |
| ------------- | ----------- |
| `broker__defaultMaxBatchSizeInBytes` | Yalnızca `MaxEventsPerBatch` belirtildiğinde maksimum teslimat isteği boyutu. Varsayılan `1_058_576`değer.
| `broker__defaultMaxEventsPerBatch` | Yalnızca `MaxBatchSizeInBytes` belirtildiğinde toplu iş partisine eklenecek maksimum olay sayısı. Varsayılan `10`değer.
