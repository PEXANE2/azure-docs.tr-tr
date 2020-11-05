---
title: Telemetri işlemcileri (Önizleme)-Azure Izleyici Application Insights Java
description: Azure Izleyici için telemetri işlemcileri Application Insights Java
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 1908e36dcf5b24301a08e543c5cf7cc106af6bff
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380334"
---
# <a name="telemetry-processors-preview-for-azure-monitor-application-insights-java"></a>Azure Izleyici Application Insights Java için telemetri işlemcileri (Önizleme)

> [!NOTE]
> Bu özellik hala önizleme aşamasındadır.

Application Insights için Java 3,0 Aracısı artık veriler verilene kadar telemetri verilerini işleme yeteneğine sahiptir.

### <a name="some-use-cases"></a>Bazı kullanım örnekleri:
 * Hassas verileri maskeleme
 * Koşullu özel boyutlar ekleyin
 * Toplama ve görüntüleme için kullanılan telemetri adını güncelleştirme

### <a name="supported-processors"></a>Desteklenen İşlemciler:
 * Öznitelik Işlemcisi
 * Yayılma Işlemcisi

## <a name="to-get-started"></a>Başlamak için

Adlı bir yapılandırma dosyası oluşturun `applicationinsights.json` ve `applicationinsights-agent-***.jar` aşağıdaki şablonla ile aynı dizine yerleştirin.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="includeexclude-spans"></a>Yayılmaları ekle/çıkar

Öznitelik işlemcisi ve span işlemcisi, yayılma alanının işlemciye dahil edilip edilmediğini veya işlemciyi dışarıda bırakılmasını gerekip gerekmediğini öğrenmek için, ile eşleşecek bir yayılımın özelliklerinin bir kümesini sağlama seçeneğini sunar. Bu seçeneği, `include` ve/veya `exclude` en az bir ile, ya da ' nin altında yapılandırmak için `matchType` `spanNames` `attributes` gereklidir. Dahil etme/hariç tutma yapılandırması, belirtilen birden fazla koşula sahip olmak için desteklenir. Bir eşleşmenin gerçekleşmesi için belirtilen koşulların tümü doğru olarak değerlendirilmelidir. 

**Gerekli alan** : 
* `matchType` içindeki öğelerin `spanNames` ve dizilerin nasıl `attributes` yorumlandığını denetler. Olası değerler: `regexp` veya `strict`. 

**Isteğe bağlı alanlar** : 
* `spanNames` öğelerin en az biriyle eşleşmesi gerekir. 
* `attributes` eşleştirilecek özniteliklerin listesini belirtir. Bir eşleşmenin gerçekleşmesi için bu özniteliklerin tümünün tam olarak eşleşmesi gerekir.

> [!NOTE]
> `include`Ve belirtilirse özellikler, `exclude` `include` özelliklerden önce denetlenir `exclude` .

#### <a name="sample-usage"></a>Örnek kullanım

Aşağıda, bu işlemcinin hangi yayılmaları gerektiğini göstermek için span özellikleri kümesinin belirtilmesi gösterilmektedir. Özellikleri, hangilerinin `include` dahil edileceğini ve `exclude` özellikleri işlenmemelidir.

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
            "svcA",
            "svcB"
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

Yukarıdaki yapılandırmayla, aşağıdaki yayılmalar özelliklerle eşleşir ve işlemci eylemleri uygulanır:

* Span1 adı: ' svcB ' öznitelikler: {env: Production, test_request: 123, credit_card: 1234, redact_trace: "false"}

* Span2 adı: ' svcA ' öznitelikleri: {env: hazırlama, test_request: false, redact_trace: true}

Aşağıdaki yayılmalar içerme özellikleriyle eşleşmez ve işlemci eylemleri uygulanmaz:

* Span3 adı: ' svcB ' öznitelikler: {env: Production, test_request: true, credit_card: 1234, redact_trace: false}

* Span4 adı: ' svcC ' öznitelikleri: {env: dev, test_request: false}

## <a name="attribute-processor"></a>Öznitelik işlemcisi 

Öznitelikler işlemcisi bir yayılımın özniteliklerini değiştirir. Bu, isteğe bağlı olarak yayılmaları dahil etme/hariç tutma özelliğini destekler.
Yapılandırma dosyasında belirtilen sırada gerçekleştirilen eylemlerin bir listesini alır. Desteklenen eylemler şunlardır:

* `insert` : Bu anahtarın zaten mevcut olmadığı yayılmalar içine yeni bir öznitelik ekler
* `update` : Anahtarın bulunduğu yayılmalar içindeki bir özniteliği güncelleştirir
* `delete` : Bir özniteliği bir yayılma alanından siler
* `hash`   : Karmaları (SHA1) var olan bir öznitelik değeri

Eylemler `insert` ve `update`
* `key` gereklidir
* `value`ya da `fromAttribute` gerekli
* `action` gereklidir.

Eylem için `delete` ,
* `key` gereklidir
* `action`: `delete` gereklidir.

Eylem için `hash` ,
* `key` gereklidir
* `action` : `hash` gereklidir.

Eylem listesi, yeniden doldurma özniteliği, değerleri yeni bir anahtara kopyalama ve hassas bilgileri redakleştirme gibi zengin senaryolar oluşturmak için kullanılabilir.

#### <a name="sample-usage"></a>Örnek kullanım

Aşağıdaki örnek, yayılmalar için anahtar/değer eklemeyi gösterir:

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
            "value": "value1",
            "action": "insert"
          },
          {
            "key": "key1",
            "fromAttribute": "anotherkey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

Aşağıdaki örnek, işlemcinin yalnızca bir öznitelikte bulunan anahtarları güncelleştirmek için yapılandırılmasını gösterir:

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "piiattribute",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "credit_card",
            "action": "delete"
          },
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

## <a name="span-processors"></a>Yayılma işlemcileri

Yayılma işlemcisi, span adına göre bir yayılma alanının yayılma adını veya özniteliklerini değiştirir. Bu, isteğe bağlı olarak yayılmaları dahil etme/hariç tutma özelliğini destekler.

### <a name="name-a-span"></a>Bir yayılımı adlandırın

Ad bölümünün bir parçası olarak aşağıdaki ayar gereklidir:

* `fromAttributes`: Anahtarların öznitelik değeri, yapılandırmada belirtilen sırada yeni bir ad oluşturmak için kullanılır. İşlemcinin yeniden adlandırmak için tüm öznitelik anahtarlarının yayılma alanında belirtilmesi gerekir.

Aşağıdaki ayar isteğe bağlı olarak yapılandırılabilir:

* `separator`: Belirtilen bir dize değerleri ayırmak için kullanılır
> [!NOTE]
> Yeniden adlandırma, öznitelikler işlemcisi tarafından değiştirilmekte olan özniteliklere bağımlıysa, işlem hattı belirtiminde özniteliklerin işlemcisinden sonra yayılma işlemcisinin belirtildiğinden emin olun.

#### <a name="sample-usage"></a>Örnek kullanım

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

Yayılma adı ile eşleşen normal ifadelerin bir listesini alır ve alt deyimlere göre bunların özniteliklerini ayıklar. Bölümünün altında belirtilmelidir `toAttributes` .

Aşağıdaki ayarlar gereklidir:

`rules` : Aralık adından öznitelik değerlerini Ayıklanacak kuralların listesi. Yayılma adındaki değerler ayıklanan öznitelik adlarıyla değiştirilmiştir. Listedeki her bir kural, Regex model dizesidir. Yayılma adı, Regex ile denetlenir. Regex eşleşiyorsa, Regex 'nin adlandırılmış tüm alt ifadeleri öznitelik olarak ayıklanır ve yayılmasına eklenir. Her alt ifade adı bir öznitelik adı olur ve alt ifade ile eşleşen bölüm öznitelik değeri olur. Yayılma adındaki eşleşen bölüm ayıklanan öznitelik adı ile değiştirilmiştir. Öznitelikler zaten yayılmakta mevcutsa, üzerine yazılır. İşlem, tüm kurallar belirtildikleri sırada yinelenir. Sonraki her kural, önceki kural işlendikten sonra çıkış olan yayılma adı üzerinde çalışmaktadır.

#### <a name="sample-usage"></a>Örnek kullanım

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
              "^/api/v1/document/(?P<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

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
              "(?P<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```