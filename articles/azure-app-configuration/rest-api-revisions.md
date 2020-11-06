---
title: Azure Uygulama yapılandırması REST API-anahtar-değer düzeltmeleri
description: Azure Uygulama Yapılandırması ' nı kullanarak anahtar-değer düzeltmeleri ile çalışmaya yönelik başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 7d1990d6bc524a69de2b22b4f7c5aeec88c3ce9d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424482"
---
# <a name="key-value-revisions"></a>Anahtar-değer düzeltmeleri

api sürümü: 1,0

**Anahtar-değer düzeltmesi** , anahtar-değer kaynağının geçmiş gösterimini tanımlar. Düzeltmeler ücretsiz katman depoları için 7 gün sonra veya Standart katman depoları için 30 gün sonra sona erer. Düzeltmeler aşağıdaki işlemleri destekler:

- Liste

Tüm işlemler için ``key`` isteğe bağlı bir parametredir. Atlanırsa, **herhangi bir** anahtar gerekir.
Tüm işlemler için ``label`` isteğe bağlı bir parametredir. Atlanırsa, **herhangi bir** etiketi belirtir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="list-revisions"></a>Düzeltmeleri listele

```http
GET /revisions?label=*&api-version={api-version} HTTP/1.1
```

**Lerinde**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvset+json; charset=utf-8"
Accept-Ranges: items
```

```json
{
    "items": [
        {
          "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
          "key": "{key}",
          "label": "{label}",
          "content_type": null,
          "value": "example value",
          "last_modified": "2017-12-05T02:41:26.4874615+00:00",
          "tags": []
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>Sayfalandırma

Döndürülen öğe sayısı yanıt sınırını aşarsa, sonuç sayfalandırılır. İsteğe bağlı ``Link`` yanıt üst bilgisini izleyin ve ``rel="next"`` gezinme için kullanın.  Alternatif olarak, içerik özellik biçiminde bir sonraki bağlantı sağlar ``@nextLink`` .

```http
GET /revisions?api-version={api-version} HTTP/1.1
```

**Yanıtıyla**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.kvs+json; charset=utf-8
Accept-Ranges: items
Link: <{relative uri}>; rel="next"
```

```json
{
    "items": [
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="list-subset-of-revisions"></a>Düzeltmelerin alt kümesini Listele

`Range`İstek üst bilgisini kullanın. Yanıt bir `Content-Range` üst bilgi içerecektir. Sunucu istenen aralığı karşılayamaz, HTTP `416` (RangeNotSatisfiable) ile yanıt verir

```http
GET /revisions?api-version={api-version} HTTP/1.1
Range: items=0-2
```

**Response**

```http
HTTP/1.1 206 Partial Content
Content-Type: application/vnd.microsoft.appconfig.revs+json; charset=utf-8
Content-Range: items 0-2/80
```

## <a name="filtering"></a>Filtreleme

`key`Ve filtrelemenin bir birleşimi `label` desteklenir.
İsteğe bağlı `key` ve `label` sorgu dizesi parametrelerini kullanın.

```http
GET /revisions?key={key}&label={label}&api-version={api-version}
```

### <a name="supported-filters"></a>Desteklenen filtreler

|Anahtar filtresi|Etki|
|--|--|
|`key` atlanır veya `key=*`|**Herhangi bir** anahtarla eşleşir|
|`key=abc`|**ABC** adlı bir anahtarla eşleşir|
|`key=abc*`|**ABC** ile başlayan anahtar adlarını eşleştirir|
|`key=*abc`|**ABC** ile biten anahtar adlarını eşleştirir|
|`key=*abc*`|**ABC** içeren anahtar adlarını eşleştirir|
|`key=abc,xyz`|**ABC** veya **xyz** anahtar adları Ile eşleşir (5 CSV ile sınırlıdır)|

|Etiket Filtresi|Etki|
|--|--|
|`label` atlanır veya `label=`|Etiketi olmayan girişle eşleşiyor|
|`label=*`|**Herhangi bir** etiketle eşleşir|
|`label=prod`|**Üretim** etiketi ile eşleşir|
|`label=prod*`|**Üretim** ile başlayan etiketlerle eşleşir|
|`label=*prod`|**Üretim** ile biten etiketlerle eşleşir|
|`label=*prod*`|**Üretim** içeren etiketleri eşleştirir|
|`label=prod,test`|Etiketler **Üretim** veya **Test** Ile eşleşir (5 CSV ile sınırlıdır)|

### <a name="reserved-characters"></a>Ayrılmış karakterler

`*`, `\`, `,`

Ayrılmış bir karakter değerin bir parçasıysa, kullanılarak kaçışlı olması gerekir `\{Reserved Character}` . Ayrılmayan karakterlere de kaçışmış olabilir.

### <a name="filter-validation"></a>Filtre doğrulaması

Filtre doğrulama hatası oluşursa, yanıt `400` hata ayrıntılarına sahıp http olur:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid request parameter '{filter}'",
  "name": "{filter}",
  "detail": "{filter}(2): Invalid character",
  "status": 400
}
```

### <a name="examples"></a>Örnekler

- Tümü

    ```http
    GET /revisions
    ```

- Anahtar adının **ABC** ile başladığı öğeler

    ```http
    GET /revisions?key=abc*&api-version={api-version}
    ```

- Anahtar adının **ABC** ya da **xyz** olduğu ve Etiketler **Üretim** 'nin bulunduğu öğeler

    ```http
    GET /revisions?key=abc,xyz&label=*prod*&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>Belirli alanlar iste

İsteğe bağlı `$select` sorgu dizesi parametresini kullanın ve istenen alanların virgülle ayrılmış listesini sağlayın. `$select`Parametresi atlanırsa, yanıt varsayılan kümesini içerir.

```http
GET /revisions?$select=value,label,last_modified&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Time-Based erişim

Sonucun bir önceki zamanda bulunduğu gösterimi elde edin. Bkz. bölüm [2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1)

```http
GET /revisions?api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**Yanıtıyla**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.revs+json"
Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT
Link: <{relative uri}>; rel="original"
```

```json
{
    "items": [
        ....
    ]
}
```
