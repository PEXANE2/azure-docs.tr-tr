---
title: Telemetri işlemcisi örnekleri-Java için Azure Izleyici Application Insights
description: Java için Azure Izleyici Application Insights telemetri işlemcilerini gösteren örnekleri inceleyebilirsiniz.
ms.topic: conceptual
ms.date: 12/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 0978bd669855d264ed6dfa5eeddc45ad499aa2a5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734596"
---
# <a name="telemetry-processor-examples---azure-monitor-application-insights-for-java"></a>Telemetri işlemcisi örnekleri-Java için Azure Izleyici Application Insights

Bu makalede, Java için Application Insights telemetri işlemcisi örnekleri sunulmaktadır. Ekleme ve dışlama yapılandırmalarına yönelik örnekler bulacaksınız. Ayrıca öznitelik işlemcileri ve yayma işlemcileri için örnekler bulacaksınız.
## <a name="include-and-exclude-samples"></a>Örnekleri dahil etme ve hariç tutma

Bu bölümde, yayılmaları dahil etme ve hariç tutma hakkında bilgi edineceksiniz. Ayrıca, birden çok yayılmaları nasıl dışarıda bırakacağınızı ve seçmeli işleme nasıl uygulanacağını da göreceksiniz.
### <a name="include-spans"></a>Yayılmaları dahil et

Bu bölüm, bir öznitelik işlemcisi için yayılmaları nasıl dahil edileceğini gösterir. Özelliklerle eşleşmeyen yayılmalar işlemci tarafından işlenmez.

Bir eşleşme, yayılma adının veya değerine eşit olmasını gerektirir `spanA` `spanB` . 

Bu yayılmalar içerme özellikleriyle eşleşir ve işlemci eylemleri uygulanır:
* Span1 adı: ' spanA ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanB ' öznitelikleri: {env: dev, test_request: false}
* Span3 adı: ' spanA ' öznitelikleri: {env: 1, test_request: dev, credit_card: 1234}

Bu yayılma, içerme özellikleriyle eşleşmez ve işlemci eylemleri uygulanmaz:
* Span4 adı: ' spanC ' öznitelikleri: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="exclude-spans"></a>Yayılmaları hariç tut

Bu bölümde, bir öznitelik işlemcisi için yayılmalar nasıl hariç tutulacak gösterilmektedir. Özelliklerle eşleşen yayılmalar bu işlemci tarafından işlenmemektedir.

Bir eşleşme, yayılma adının veya değerine eşit olmasını gerektirir `spanA` `spanB` .

Aşağıdaki yayılmalar dışlama özellikleriyle eşleşir ve işlemci eylemleri uygulanmaz:
* Span1 adı: ' spanA ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanB ' öznitelikleri: {env: dev, test_request: false}
* Span3 adı: ' spanA ' öznitelikleri: {env: 1, test_request: dev, credit_card: 1234}

Bu yayılma dışlama özellikleriyle eşleşmez ve işlemci eylemleri uygulanır:
* Span4 adı: ' spanC ' öznitelikleri: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="exclude-spans-by-using-multiple-criteria"></a>Birden çok ölçüt kullanarak yayılmaları hariç tut

Bu bölümde, bir öznitelik işlemcisi için yayılmalar nasıl hariç tutulacak gösterilmektedir. Özelliklerle eşleşen yayılmalar bu işlemci tarafından işlenmemektedir.

Bir eşleşme aşağıdaki koşulların karşılanmasını gerektirir:
* Yayılmakta bir öznitelik (örneğin, `env` veya `dev` ) bulunmalıdır.
* Span anahtarı olan bir özniteliğe sahip olmalıdır `test_request` .

Aşağıdaki yayılmalar dışlama özellikleriyle eşleşir ve işlemci eylemleri uygulanmaz.
* Span1 adı: ' spanB ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanA ' öznitelikleri: {env: dev, test_request: false}

Aşağıdaki yayılma dışlama özellikleriyle eşleşmez ve işlemci eylemleri uygulanır:
* Span3 adı: ' spanB ' öznitelikleri: {env: 1, test_request: dev, credit_card: 1234}
* Span4 adı: ' spanC ' öznitelikleri: {env: dev, dev_request: false}


```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ],
          "attributes": [
            {
              "key": "env",
              "value": "dev"
            },
            {
              "key": "test_request"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="selective-processing"></a>Seçmeli işleme

Bu bölümde, bu işlemcinin hangi yayılmaları gerektiğini belirten span özelliklerinin nasıl ayarlanacağı gösterilmektedir. İçerme özellikleri hangi yayılmaları işleneceğini belirtir. Hariç tutma özellikleri, işlenmemelidir.

Aşağıdaki yapılandırmada, bu yayılmalar özelliklerle eşleşir ve işlemci eylemleri uygulanır:

* Span1 adı: ' spanB ' öznitelikleri: {env: Production, test_request: 123, credit_card: 1234, redact_trace: "false"}
* Span2 adı: ' spanA ' öznitelikleri: {env: hazırlama, test_request: false, redact_trace: true}

Bu yayılmalar içerme özellikleriyle eşleşmez ve işlemci eylemleri uygulanmaz:
* Span3 adı: ' spanB ' öznitelikleri: {env: Production, test_request: true, credit_card: 1234, redact_trace: false}
* Span4 adı: ' spanC ' öznitelikleri: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "attributes": [
            {
              "key": "redact_trace",
              "value": "false"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          },
          {
            "key": "duplicate_key",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```
## <a name="attribute-processor-samples"></a>Öznitelik işlemcisi örnekleri

### <a name="insert"></a>Ekle

Aşağıdaki örnek, `{"attribute1": "attributeValue1"}` anahtar mevcut olmayan yayılmalar içine yeni özniteliğini ekler `attribute1` .

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "attribute1",
            "value": "attributeValue1",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="insert-from-another-key"></a>Başka bir anahtardan Ekle

Aşağıdaki örnek, `anotherkey` `{"newKey": "<value from attribute anotherkey>"}` anahtar mevcut olmayan yayılmalar içine yeni özniteliği eklemek için özniteliğinden değeri kullanır `newKey` . Özniteliği `anotherkey` yoksa, yayılmalar içine yeni bir öznitelik eklenmez.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "newKey",
            "fromAttribute": "anotherKey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="update"></a>Güncelleştir

Aşağıdaki örnek, özniteliğini olarak güncelleştirir `{"db.secret": "redacted"}` . Özniteliği, `boo` özniteliğinden değeri kullanarak güncelleştirir `foo` . Özniteliği olmayan yayılmalar `boo` .

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "db.secret",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "boo",
            "fromAttribute": "foo",
            "action": "update" 
          }
        ]
      }
    ]
  }
}
```

### <a name="delete"></a>Sil

Aşağıdaki örnek, anahtarına sahip bir özniteliğin nasıl silineceğini gösterir `credit_card` .

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="hash"></a>Karma

Aşağıdaki örnek, var olan öznitelik değerlerinin nasıl karma olduğunu gösterir.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "user.email",
            "action": "hash"
          }
        ]
      }
    ]
  }
}
```

### <a name="extract"></a>Ayıkla

Aşağıdaki örnek, başka bir özniteliğin değerine göre yeni öznitelikler oluşturmak için bir normal ifadenin (Regex) nasıl kullanılacağını gösterir.
Örneğin, verilen `http.url = http://example.com/path?queryParam1=value1,queryParam2=value2` Aşağıdaki öznitelikler eklenir:
* httpProtocol: `http`
* httpDomain: `example.com`
* httpPath: `path`
* httpQueryParams: `queryParam1=value1,queryParam2=value2`
* http. URL: değişiklik *yok*

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "http.url",
            "pattern": "^(?<httpProtocol>.*):\\/\\/(?<httpDomain>.*)\\/(?<httpPath>.*)(\\?|\\&)(?<httpQueryParams>.*)",
            "action": "extract"
          }
        ]
      }
    ]
  }
}
```

Aşağıdaki örnek, Regex desenleriyle eşleşen bir yayma adına sahip olan yayılmaları nasıl işleyeceğini gösterir.
Bu işlemci, `token` özniteliğini kaldırır. `password`Yayılma alanındaki, yayılma adının eşleştiği `auth.*` ve yayılma adının eşleşmediğinden özniteliği belirsizleştirecektir `login.*` .

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "auth.*"
          ]
        },
        "exclude": {
          "matchType": "regexp",
          "spanNames": [
            "login.*"
          ]
        },
        "actions": [
          {
            "key": "password",
            "value": "obfuscated",
            "action": "update"
          },
          {
            "key": "token",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```


## <a name="span-processor-samples"></a>Yayılma işlemcisi örnekleri

### <a name="name-a-span"></a>Bir yayılımı adlandırın

Aşağıdaki örnek, ve özniteliklerinin değerlerini belirtir `db.svc` `operation` `id` . Bu öznitelik, bu öznitelikleri kullanarak, bu değerleri değer ile ayırarak, bu şekilde yayılma alanının yeni adını oluşturur `::` .
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "fromAttributes": [
            "db.svc",
            "operation",
            "id"
          ],
          "separator": "::"
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-a-span-name"></a>Bir yayılma adından öznitelikleri Ayıkla

Giriş alanı adının olduğunu varsayalım `/api/v1/document/12345678/update` . Aşağıdaki örnek, çıkış yayılımı adıyla sonuçlanır `/api/v1/document/{documentId}/update` . Yeni özniteliğini `documentId=12345678` span öğesine ekler.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "toAttributes": {
            "rules": [
              "^/api/v1/document/(?<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-a-span-name-by-using-include-and-exclude"></a>Include ve exclude kullanarak bir span adından öznitelikleri Ayıkla

Aşağıdaki örnek, yayılma adının olarak nasıl değiştirileceğini gösterir `{operation_website}` . `operation_website` `{oldSpanName}` Yayılma aşağıdaki özelliklere sahip olduğunda anahtar ve değere sahip bir öznitelik ekler:
- Yayılma adı, `/` dizenin herhangi bir yerini içerir.
- Yayılma adı değil `donot/change` .
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "^(.*?)/(.*?)$"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "donot/change"
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [
              "(?<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```
