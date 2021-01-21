---
title: Telemetri işlemcileri (Önizleme)-Java için Azure Izleyici Application Insights
description: Java için Azure Izleyici Application Insights telemetri işlemcileri yapılandırma
ms.topic: conceptual
ms.date: 10/29/2020
author: kryalama
ms.custom: devx-track-java
ms.author: kryalama
ms.openlocfilehash: c0745dd4069c64292fbcaef666d843ae2d25f7b3
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632589"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>Telemetri işlemcileri (Önizleme)-Java için Azure Izleyici Application Insights

> [!NOTE]
> Bu özellik hala önizleme aşamasındadır.

Application Insights için Java 3,0 Aracısı artık veriler verilene kadar telemetri verilerini işleme yeteneğine sahiptir.

Telemetri işlemcilerin bazı kullanım durumları aşağıda verilmiştir:
 * Hassas verileri maskeleme
 * Koşullu özel boyutlar ekleyin
 * Toplama için kullanılan adı güncelleştirme Azure portal ve görüntüleme
 * Alım maliyetini denetlemek için span özniteliklerini bırakın

## <a name="terminology"></a>Terminoloji

Telemetri işlemcilere atlamadan önce, dönemin ne olduğunu anlamak önemlidir.

Bir yayılma, bu üç şeyin herhangi biri için genel bir terimdir:

* Gelen istek
* Giden bağımlılığı (örn. başka bir hizmete uzaktan çağrı)
* İşlem içi bağımlılık (örneğin, hizmetin alt bileşenleri tarafından gerçekleştirilen iş)

Telemetri işlemcilerin amacı doğrultusunda bir yayılma alanının önemli bileşenleri şunlardır:

* Name
* Öznitelikler

Yayılma adı, Azure portal istekler ve Bağımlılıklar için kullanılan birincil görüntüdir.

Span öznitelikleri, belirli bir isteğin veya bağımlılığın hem standart hem de özel özelliklerini temsil eder.

## <a name="telemetry-processor-types"></a>Telemetri işlemcisi türleri

Şu anda iki tür telemetri işlemcisi vardır.

#### <a name="attribute-processor"></a>Öznitelik işlemcisi

Öznitelik işlemcisi, öznitelikleri ekleme, güncelleştirme, silme veya sağlama yeteneğine sahiptir.
Ayrıca, varolan bir öznitelikten bir veya daha fazla yeni öznitelik çıkarabilir (normal bir ifade aracılığıyla).

#### <a name="span-processor"></a>Yayılma işlemcisi

Bir yayılma işlemcisi telemetri adını güncelleştirebilir.
Ayrıca, yayılma adından bir veya daha fazla yeni öznitelik çıkarabilir (normal bir ifade aracılığıyla).

> [!NOTE]
> Şu anda telemetri işlemcilerin yalnızca dize türündeki öznitelikleri işleyeceğini ve Boole veya sayı türündeki öznitelikleri işlemediğini unutmayın.

## <a name="getting-started"></a>Başlarken

Adlı bir yapılandırma dosyası oluşturun `applicationinsights.json` ve `applicationinsights-agent-*.jar` aşağıdaki şablonla ile aynı dizine yerleştirin.

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

## <a name="includeexclude-criteria"></a>Dahil etme/hariç tutma ölçütleri

Her iki öznitelik işlemcisi ve span işlemcisi isteğe bağlı `include` ve `exclude` ölçütleri destekler.
Bir işlemci yalnızca ölçütleriyle eşleşen (sağlanmışsa) bu yayılanlara uygulanır `include` _ve_ `exclude` (sağlanmışsa) ölçütleriyle eşleşmez.

Bu seçeneği, `include` ve/veya `exclude` en az bir ile, ya da ' nin altında yapılandırmak için `matchType` `spanNames` `attributes` gereklidir.
Dahil etme/hariç tutma yapılandırması, belirtilen birden fazla koşula sahip olmak için desteklenir.
Bir eşleşmenin gerçekleşmesi için belirtilen koşulların tümü doğru olarak değerlendirilmelidir. 

**Gerekli alan**: 
* `matchType` içindeki öğelerin `spanNames` ve dizilerin nasıl `attributes` yorumlandığını denetler. Olası değerler: `regexp` veya `strict`. 

**Isteğe bağlı alanlar**: 
* `spanNames` öğelerin en az biriyle eşleşmesi gerekir. 
* `attributes` eşleştirilecek özniteliklerin listesini belirtir. Bir eşleşmenin gerçekleşmesi için bu özniteliklerin tümünün tam olarak eşleşmesi gerekir.

> [!NOTE]
> `include`Ve belirtilirse özellikler, `exclude` `include` özelliklerden önce denetlenir `exclude` .

#### <a name="sample-usage"></a>Örnek Kullanım

```json

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
```
Daha fazla bilgi edinmek için [telemetri işlemcisi örnekleri](./java-standalone-telemetry-processors-examples.md) belgelerini inceleyin.

## <a name="attribute-processor"></a>Öznitelik işlemcisi

Öznitelikler işlemcisi bir yayılımın özniteliklerini değiştirir. Bu, isteğe bağlı olarak yayılmaları dahil etme/hariç tutma özelliğini destekler. Yapılandırma dosyasında belirtilen sırada gerçekleştirilen eylemlerin bir listesini alır. Desteklenen eylemler şunlardır:

### `insert`

Yayılmalar içinde, anahtarın zaten mevcut olmadığı yeni bir öznitelik ekler.   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      }
    ]
  }
]
```
Eylem için `insert` aşağıdakiler gereklidir
  * `key`
  * biri `value` veya `fromAttribute`
  * `action`:`insert`

### `update`

Anahtar var olduğunda yayılmakta olan bir özniteliği güncelleştirir

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      }
    ]
  }
]
```
Eylem için `update` aşağıdakiler gereklidir
  * `key`
  * biri `value` veya `fromAttribute`
  * `action`:`update`


### `delete` 

Yayılma alanından bir özniteliği siler

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      }
    ]
  }
]
```
Eylem için `delete` aşağıdakiler gereklidir
  * `key`
  * `action`: `delete`

### `hash`

Karmalar (SHA1) var olan bir öznitelik değeri

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      }
    ]
  }
]
```
Eylem için `hash` aşağıdakiler gereklidir
* `key`
* `action` : `hash`

### `extract`

> [!NOTE]
> Bu özellik yalnızca 3.0.2 ve üzeri sürümlerde

Kural içinde belirtilen hedef anahtarlara giriş anahtarından bir normal ifade kuralı kullanarak değerleri ayıklar. Bir hedef anahtar zaten varsa, geçersiz kılınır. Kaynak olarak var olan özniteliğiyle [span işlemci](#extract-attributes-from-span-name) ayarına benzer şekilde davranır `toAttributes` .

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      }
    ]
  }
]
```
Eylem için `extract` aşağıdakiler gereklidir
* `key`
* `pattern`
* `action` : `extract`

Daha fazla bilgi edinmek için [telemetri işlemcisi örnekleri](./java-standalone-telemetry-processors-examples.md) belgelerini inceleyin.

## <a name="span-processor"></a>Yayılma işlemcisi

Yayılma işlemcisi, span adına göre bir yayılma alanının yayılma adını veya özniteliklerini değiştirir. Bu, isteğe bağlı olarak yayılmaları dahil etme/hariç tutma özelliğini destekler.

### <a name="name-a-span"></a>Bir yayılımı adlandırın

Ad bölümünün bir parçası olarak aşağıdaki ayar gereklidir:

* `fromAttributes`: Anahtarların öznitelik değeri, yapılandırmada belirtilen sırada yeni bir ad oluşturmak için kullanılır. İşlemcinin yeniden adlandırmak için tüm öznitelik anahtarlarının yayılma alanında belirtilmesi gerekir.

Aşağıdaki ayar isteğe bağlı olarak yapılandırılabilir:

* `separator`: Belirtilen bir dize değerleri ayırmak için kullanılır
> [!NOTE]
> Yeniden adlandırma, öznitelikler işlemcisi tarafından değiştirilmekte olan özniteliklere bağımlıysa, işlem hattı belirtiminde özniteliklerin işlemcisinden sonra yayılma işlemcisinin belirtildiğinden emin olun.

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-span-name"></a>Aralık adından öznitelikleri Ayıkla

Yayılma adı ile eşleşen normal ifadelerin bir listesini alır ve alt deyimlere göre bunların özniteliklerini ayıklar. Bölümünün altında belirtilmelidir `toAttributes` .

Aşağıdaki ayarlar gereklidir:

`rules` : Aralık adından öznitelik değerlerini Ayıklanacak kuralların listesi. Yayılma adındaki değerler ayıklanan öznitelik adlarıyla değiştirilmiştir. Listedeki her bir kural, Regex model dizesidir. Yayılma adı, Regex ile denetlenir. Regex eşleşiyorsa, Regex 'nin adlandırılmış tüm alt ifadeleri öznitelik olarak ayıklanır ve yayılmasına eklenir. Her alt ifade adı bir öznitelik adı olur ve alt ifade ile eşleşen bölüm öznitelik değeri olur. Yayılma adındaki eşleşen bölüm ayıklanan öznitelik adı ile değiştirilmiştir. Öznitelikler zaten yayılmakta mevcutsa, üzerine yazılır. İşlem, tüm kurallar belirtildikleri sırada yinelenir. Sonraki her kural, önceki kural işlendikten sonra çıkış olan yayılma adı üzerinde çalışmaktadır.

```json

"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## <a name="list-of-attributes"></a>Özniteliklerin listesi

Aşağıda telemetri işlemcilerde kullanılabilen bazı ortak span özniteliklerinin listesidir.

### <a name="http-spans"></a>HTTP yayılmaları

| Öznitelik  | Tür | Description | 
|---|---|---|
| `http.method` | dize | HTTP istek yöntemi.|
| `http.url` | string | Formdaki tam HTTP isteği URL 'SI `scheme://host[:port]/path?query[#fragment]` . Genellikle parça HTTP üzerinden aktarılmaz, ancak biliniyorsa, bununla birlikte dahil edilmelidir.|
| `http.status_code` | sayı | [Http yanıtı durum kodu](https://tools.ietf.org/html/rfc7231#section-6).|
| `http.flavor` | string | Kullanılan HTTP protokolü türü |
| `http.user_agent` | string | İstemci tarafından gönderilen [http Kullanıcı Aracısı](https://tools.ietf.org/html/rfc7231#section-5.5.3) üstbilgisinin değeri. |

### <a name="jdbc-spans"></a>JDBC yaymalar

| Öznitelik  | Tür | Description  |
|---|---|---|
| `db.system` | dize | Kullanılan veritabanı yönetim sistemi (DBMS) ürünü için bir tanımlayıcı. |
| `db.connection_string` | string | Veritabanına bağlanmak için kullanılan bağlantı dizesi. Katıştırılmış kimlik bilgilerini kaldırmanız önerilir.|
| `db.user` | string | Veritabanına erişmek için Kullanıcı adı. |
| `db.name` | string | Bu öznitelik, erişilmekte olan veritabanının adını raporlamak için kullanılır. Veritabanına geçiş yapan komutlar için, bu, hedef veritabanına ayarlanmalıdır (komut başarısız olsa bile).|
| `db.statement` | string | Yürütülen veritabanı açıklaması.|
