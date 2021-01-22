---
title: Telemetri işlemcileri örnekleri-Java için Azure Izleyici Application Insights
description: Java için Azure Izleyici Application Insights telemetri işlemcilerini gösteren örnekler
ms.topic: conceptual
ms.date: 12/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: 9b29c9611359c97c4097ad0b90ee2673bb28f37c
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696321"
---
# <a name="telemetry-processors-examples---azure-monitor-application-insights-for-java"></a>Telemetri işlemcileri örnekleri-Java için Azure Izleyici Application Insights

## <a name="includeexclude-samples"></a>Örnek Ekle/Dışla

### <a name="1-include-spans"></a>1. yayılmaları dahil et

Aşağıda, bu öznitelikler işlemcisinin yayılmaları dahil gösterilmektedir. Özelliklerle eşleşen tüm diğer yayılmalar bu işlemci tarafından işlenmez.

Bir eşleşme için karşılanması gereken koşullar aşağıda verilmiştir:
* Yayılma adı "spanA" veya "spanB" değerine eşit olmalıdır 

Aşağıdakiler, içerme özellikleriyle eşleşen yayılmalar ve işlemci eylemleri uygulanır.
* Span1 adı: ' spanA ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanB ' öznitelikleri: {env: dev, test_request: false}
* Span3 adı: ' spanA ' öznitelikleri: {env: 1, test_request: dev, credit_card: 1234}

Aşağıdaki yayılma, içerme özellikleriyle eşleşmez ve işlemci eylemleri uygulanmaz.
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

### <a name="2-exclude-spans"></a>2. yayılmaları hariç tut

Aşağıda, bu öznitelik işlemcisi için yayılmalar dışlanıyor gösterilmektedir. Özelliklerle eşleşen tüm yayılmalar bu işlemci tarafından işlenmez.

Bir eşleşme için karşılanması gereken koşullar aşağıda verilmiştir:
* Yayılma adı "spanA" veya "spanB" değerine eşit olmalıdır 

Aşağıdakiler dışlama özellikleriyle eşleşen yayılmalar ve işlemci eylemleri uygulanmaz.
* Span1 adı: ' spanA ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanB ' öznitelikleri: {env: dev, test_request: false}
* Span3 adı: ' spanA ' öznitelikleri: {env: 1, test_request: dev, credit_card: 1234}

Aşağıdaki Aralık dışlama özellikleriyle eşleşmez ve işlemci eylemleri uygulanır.
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

### <a name="3-excludemulti-spans"></a>3. ExcludeMulti yayılmaları

Aşağıda, bu öznitelik işlemcisi için yayılmalar dışlanıyor gösterilmektedir. Özelliklerle eşleşen tüm yayılmalar bu işlemci tarafından işlenmez.

Bir eşleşme için karşılanması gereken koşullar aşağıda verilmiştir:
* Bir öznitelik (' env ', ' dev '), bir eşleşme için yayılma içinde bulunmalıdır.
* Yayılma alanında ' test_request ' anahtarına sahip bir öznitelik olduğu sürece eşleşme vardır.

Aşağıdakiler dışlama özellikleriyle eşleşen yayılmalar ve işlemci eylemleri uygulanmaz.
* Span1 adı: ' spanB ' öznitelikleri: {env: dev, test_request: 123, credit_card: 1234}
* Span2 adı: ' spanA ' öznitelikleri: {env: dev, test_request: false}

Aşağıdaki Aralık dışlama özellikleriyle eşleşmez ve işlemci eylemleri uygulanır.
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

### <a name="4-selective-processing"></a>4. Seçmeli işleme

Aşağıda, bu işlemcinin hangi yayılmaları gerektiğini göstermek için span özellikleri kümesinin belirtilmesi gösterilmektedir. Özellikleri, hangilerinin `include` dahil edileceğini ve `exclude` özellikleri işlenmemelidir.

Aşağıdaki yapılandırmayla, aşağıdaki yayılmalar özelliklerle eşleşir ve işlemci eylemleri uygulanır:

* Span1 adı: ' spanB ' öznitelikleri: {env: Production, test_request: 123, credit_card: 1234, redact_trace: "false"}
* Span2 adı: ' spanA ' öznitelikleri: {env: hazırlama, test_request: false, redact_trace: true}

Aşağıdaki yayılmalar içerme özellikleriyle eşleşmez ve işlemci eylemleri uygulanmaz:
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
## <a name="attribute-processor-samples"></a>Öznitelik Işlemcisi örnekleri

### <a name="insert"></a>Ekle

Aşağıda, "Attribute1" anahtarının bulunmadığı yere yayıldığında {"Attribute1": "attributeValue1"} yeni bir özniteliği eklenir.

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

Aşağıdaki, "anotherkey" özniteliği "anotherkey" özniteliği, "newKey" anahtarındaki "NewKey": "değerini kapsayacak şekilde yeni bir öznitelik eklemek için öznitelikten değeri kullanır. ' Anotherkey ' özniteliği yoksa, yayılmalar için yeni bir öznitelik eklenmez.

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

Şu özniteliği {"DB. Secret": "redaksiyonu"} olarak güncelleştirir ve ' foo ' özniteliğinden değeri kullanarak ' Boo ' özniteliğini güncelleştirir. ' Boo ' özniteliği olmayan yayılma alanları değişmez.

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

' Credit_card ' anahtarına sahip özniteliği silme aşağıda gösterilmiştir.

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

Aşağıdaki karma var olan öznitelik değerlerini gösterir.

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

Aşağıdaki örnekte, başka bir özniteliğin değerine göre yeni öznitelikler oluşturmak için Regex kullanılarak gösterilmiştir.
Örneğin, verilen http. URL = ' http://example.com/path?queryParam1=value1 , queryParam2 = değer2 ' için aşağıdaki öznitelikler eklenecektir:
* httpProtocol: http
* httpDomain: example.com
* httpPath: yol
* httpQueryParams: queryParam1 = değer1, queryParam2 = değer2
* http. URL değeri değişmez.

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

Aşağıdaki örnek, RegExp desenleriyle eşleşen bir yayma adına sahip olan yayılmaları nasıl işleyeceğini gösterir.
Bu işlemci "belirteç" özniteliğini kaldıracak ve "parola" özniteliğini, yayılma alanındaki "Auth" ile eşleşen yayılmalar halinde \* kaldırır. ve yayılma adı "Login" ile eşleşmez \* .

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


## <a name="span-processor-samples"></a>Yayılma Işlemcisi örnekleri

### <a name="name-a-span"></a>Bir yayılımı adlandırın

Aşağıdaki örnek, "DB. svc", "Operation" ve "id" özniteliği değerlerinin "::" değeriyle ayırarak, bu sırada yayılma alanının yeni adını oluşturur.
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

### <a name="extract-attributes-from-span-name"></a>Aralık adından öznitelikleri Ayıkla

Giriş alanı adının/api/v1/Document/12345678/updateolduğunu varsayalım. Şu sonuçların çıkış yayma adı/api/v1/Document/{documententid}/Update 'e uygulanması, "Documenttıd" = "12345678" adlı yeni bir özniteliği yayılmasına ekleyecek.
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

### <a name="extract-attributes-from-span-name-with-include-and-exclude"></a>Dahil etme ve hariç bırakma adından öznitelikleri Ayıkla

Yayılma adının "{operation_website}" olarak yeniden adlandırılması ve yayılma aşağıdaki özelliklere sahip olduğunda {Key: operation_website, value: oldSpanName} özniteliği eklenmesi gösterilmektedir:
- Yayılma adı, dizenin herhangi bir yerini '/' içeriyor.
- Yayılma adı ' donot/Change ' değil.
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