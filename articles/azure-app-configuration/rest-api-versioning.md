---
title: Azure Uygulama yapılandırması REST API-sürüm oluşturma
description: Azure Uygulama yapılandırması 'nı kullanarak sürüm oluşturma için başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 90d131cdc7c496853f2520951c95b9903d69f8fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424474"
---
# <a name="versioning"></a>Sürüm Oluşturma

api sürümü: 1,0

Her istemci isteğinin sorgu dizesi parametresi olarak açık API sürümü sağlaması gerekir. Örnek: `https://{myconfig}.azconfig.io/kv?api-version=1.0`

`api-version` SemVer (ana. Minor) biçiminde ifade edilir. Aralık veya sürüm anlaşması desteklenmez.

## <a name="error-response"></a>Hata yanıtı

Aşağıda, istenen API sürümü eşleştirilemezse sunucu tarafından döndürülen olası hata yanıtlarının Özeti özetlenmektedir.

### <a name="api-version-unspecified"></a>API sürümü belirtilmedi

İstemci bir API sürümü sağlamadan istek yaptığında gerçekleşir.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "API version is not specified",
  "name": "api-version",
  "detail": "An API version is required, but was not specified.",
  "status": 400
}
```

### <a name="unsupported-api-version"></a>Desteklenmeyen API sürümü

İstemci tarafından istenen API sürümü, sunucu tarafından desteklenen API sürümlerinden hiçbiriyle eşleşmediği zaman gerçekleşir.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Unsupported API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="invalid-api-version"></a>Geçersiz API sürümü

İstemci bir API sürümü ile istek yaptığında, ancak değer yanlış biçimlendirilmediğinde veya sunucu tarafından ayrıştırılamadığından gerçekleşir.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="ambiguous-api-version"></a>Belirsiz API sürümü

İstemci, sunucu için belirsiz bir API sürümü istediğinde gerçekleşir. Örneğin, birden çok farklı değer.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Ambiguous API version",
  "name": "api-version",
  "detail": "The following API versions were requested: {comma separated api versions}. At most, only a single API version may be specified. Please update the intended API version and retry the request.",
  "status": 400
}
```
