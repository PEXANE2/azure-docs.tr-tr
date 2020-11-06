---
title: Azure uygulama yapılandırma REST API-kilitler
description: Azure Uygulama yapılandırması ile anahtar-değer kilitleri ile çalışmaya yönelik başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 4949db646c54d75f60d29d3c631d0f4ee8d7c26e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424514"
---
# <a name="locks"></a>Kilitler

api sürümü: 1,0

Bu API, anahtar-değer kaynağı için kilit/kilit açma semantiğini sağlar. Aşağıdaki işlemleri destekler:

- Kilit yerleştir
- Kilidi Kaldır

Varsa, `label` açık bir etiket değeri (joker karakter **değil** ) olmalıdır. Tüm işlemler için isteğe bağlı bir parametredir. Atlanırsa, etiket olmaz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="lock-key-value"></a>Kilit Key-Value

- **Gerekli:** ``{key}`` , ``{api-version}``  
- *Isteğe bağlı:*``label``

```http
PUT /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
```

**Lerinde**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

Anahtar değeri yoksa, aşağıdaki yanıt döndürülür:

```http
HTTP/1.1 404 Not Found
```

## <a name="unlock-key-value"></a>Key-Value kilidini aç

- **Gerekli:** ``{key}`` , ``{api-version}``  
- *Isteğe bağlı:*``label``

```http
DELETE /locks/{key}?label={label}?api-version={api-version} HTTP/1.1
```

**Lerinde**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

Anahtar değeri yoksa, aşağıdaki yanıt döndürülür:

```http
HTTP/1.1 404 Not Found
```

## <a name="conditional-lockunlock"></a>Koşullu kilit/kilit açma

Yarış durumlarını engellemek için, `If-Match` veya `If-None-Match` istek üst bilgilerini kullanın. `etag`Bağımsız değişken, anahtar gösteriminin bir parçasıdır. `If-Match`Veya `If-None-Match` atlanırsa, işlem koşulsuz olur.

Aşağıdaki istek yalnızca geçerli anahtar-değer gösterimi belirtilen değerle eşleşiyorsa işlemi uygular `etag` :

```http
PUT|DELETE /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```

Aşağıdaki istek yalnızca geçerli anahtar-değer gösterimi varsa ancak belirtilen değerle eşleşmiyorsa işlemi uygular `etag` :

```http
PUT|DELETE /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```
