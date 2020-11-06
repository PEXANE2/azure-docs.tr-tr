---
title: Azure uygulama yapılandırma REST API azaltma
description: Azure Uygulama yapılandırması 'nı kullanırken azaltmayı anlamak için başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 26464138b7739bb476d9d0dd1078194473ecd23e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424477"
---
# <a name="throttling"></a>Azaltma

Yapılandırma depoları, karşılaşabileceği istekler üzerinde sınırlara sahiptir. Bir yapılandırma deposu için ayrılan kotayı aşan tüm istekler, bir HTTP 429 (çok fazla Istek) yanıtı alır.

Azaltma, farklı kota ilkelerine ayrılmıştır:

- **Toplam** istek sayısı-toplam istek sayısı
- **Toplam bant genişliği** -bayt cinsinden giden veri
- **Depolama** -bayt cinsinden Kullanıcı verilerinin toplam depolama alanı boyutu

## <a name="handling-throttled-responses"></a>Kısıtlanmış yanıtları işleme

Belirli bir kota için hız sınırına ulaşıldığında sunucu, _429_ durum kodu ile o türdeki diğer isteklere yanıt verir. _429_ yanıtı, istemciye istek kotasının stoğunu sağlamak için önerilen bekleme süresi (milisaniye cinsinden) sağlayan bir _yeniden deneme-MS_ üst bilgisi içerir.

```http
HTTP/1.1 429 (Too Many Requests)
retry-after-ms: 10
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/too-many-requests",
  "title": "Resource utilization has surpassed the assigned quota",
  "policy": "Total Requests",
  "status": 429
}
```

Yukarıdaki örnekte istemci, izin verilen kotasını aşmıştır ve daha fazla istek denemeden önce bu sürenin yavaşlanmasını ve 10 milisaniyeyi beklemesi önerilir. İstemciler aşamalı geri dönüşü de göz önünde bulundurmalıdır.

## <a name="other-retry"></a>Diğer yeniden deneme

Hizmet, istemci yeniden denemesi gereken daraltma dışında durumları tanımlayabilir (örn: 503 Hizmet kullanılamıyor). Bu gibi durumlarda, `retry-after-ms` yanıt üst bilgisi sağlanacaktır. Sağlamlığını artırmak için, istemcinin önerilen aralığı izlemesi ve yeniden deneme yapması önerilir.

```http
HTTP/1.1 503 Service Unavailable
retry-after-ms: 787
```
