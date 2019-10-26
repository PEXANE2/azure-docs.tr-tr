---
title: Özel bilişsel arama yeteneği-Azure Search
description: Web API 'Lerine çağrı yaparak bilişsel arama becerileri 'in yeteneklerini genişletme
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.openlocfilehash: fda4f96c2c73c5a2d39435a509afcf654ed77b70
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901326"
---
# <a name="custom-web-api-skill"></a>Özel Web API 'SI yeteneği

**Özel Web API 'si** yeteneği, özel işlemler sağlayan BIR Web API uç noktasına çağrı yaparak bilişsel aramayı genişletmenizi sağlar. Yerleşik becerilerle benzer şekilde, **özel Web API 'si** becerilerinin giriş ve çıkışları vardır. Girişlere bağlı olarak, Web API 'niz, Dizin Oluşturucu çalıştırıldığında bir JSON yükü alır ve başarılı durum kodu ile birlikte bir JSON yükünün yanıt olarak çıkışını verir. Yanıtın özel becerinize göre belirtilen çıkışların olması beklenir. Diğer herhangi bir yanıt bir hata olarak değerlendirilir ve hiçbir zenginleştirilmez.

JSON yükleri yapısı, bu belgede daha fazla açıklanacaktır.

> [!NOTE]
> Dizin Oluşturucu, Web API 'sinden döndürülen belirli standart HTTP durum kodları için iki kez yeniden dener. Bu HTTP durum kodları şunlardır: 
> * `502 Bad Gateway`
> * `503 Service Unavailable`
> * `429 Too Many Requests`

## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Custom. WebApiSkill

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| Kullanılmamışsa | _JSON_ yükünün GÖNDERILECEĞI Web API 'sinin URI 'si. Yalnızca **https** URI şemasına izin veriliyor |
| httpMethod | Yük gönderilirken kullanılacak yöntem. `PUT` veya `POST` izin verilen Yöntemler |
| httpHeaders | Anahtarların üstbilgi adlarını ve değerlerini temsil ettiği anahtar-değer çiftleri koleksiyonu, yük ile birlikte Web API 'nize gönderilecek üst bilgi değerlerini temsil eder. Şu üst bilgilerin bu koleksiyonda olması yasaktır: `Accept`, `Accept-Charset`, `Accept-Encoding`, `Content-Length`, `Content-Type`, `Cookie`, `Host`, `TE`, `Upgrade`, `Via` |
| timeout | Seçim Belirtildiğinde, API çağrısını yapan http istemcisinin zaman aşımını gösterir. XSD "dayTimeDuration" değeri ( [ıso 8601 Duration](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değerinin kısıtlı bir alt kümesi) olarak biçimlendirilmelidir. Örneğin, 60 saniye için `PT60S`. Ayarlanmamışsa, varsayılan değer olan 30 saniye seçilir. Zaman aşımı en fazla 230 saniyeye ayarlanabilir ve en az 1 saniye olabilir. |
| batchSize | Seçim Her API çağrısı için, kaç "veri kaydı" (aşağıdaki _JSON_ yük yapısına bakın) gönderileceğini belirtir. Ayarlanmamışsa, varsayılan olarak 1000 seçilidir. Dizin oluşturma işleme ve API 'niz üzerinde yükleme arasında uygun bir zorunluluğunu getirir elde etmek için bu parametreyi kullanmanızı öneririz |
| Analyticsunits | Seçim Belirtildiğinde, dizin oluşturucunun verdiğiniz bitiş noktasına paralel olarak kullanacağı çağrı sayısını gösterir. Uç noktanız bir istek yükünün çok yüksek altındaysa bu değeri azaltabilir veya uç noktanız daha fazla istek kabul edebilse ve dizin oluşturucunun performansına bir artış istiyorsanız bu değeri azaltabilirsiniz.  Ayarlanmamışsa, varsayılan 5 değeri kullanılır. Degreeofparalellik, en fazla 10 ve en az 1 olarak ayarlanabilir. |

## <a name="skill-inputs"></a>Beceri girişleri

Bu beceri için "önceden tanımlanmış" giriş yok. Bu beceri yürütme sırasında zaten kullanılabilir olacak bir veya daha fazla alan seçebilirsiniz ve Web API 'sine gönderilen _JSON_ yükünün farklı alanları olacaktır.

## <a name="skill-outputs"></a>Yetenek çıkışları

Bu beceri için "önceden tanımlanmış" çıkış yok. Web API 'nizin döndürdüğü yanıta bağlı olarak, _JSON_ yanıtından alınabilmeleri için çıktı alanları ekleyin.


## <a name="sample-definition"></a>Örnek tanım

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "A custom skill that can count the number of words or characters or lines in text",
        "uri": "https://contoso.count-things.com",
        "batchSize": 4,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          },
          {
            "name": "countOf",
            "source": "/document/propertyToCount"
          }
        ],
        "outputs": [
          {
            "name": "count",
            "targetName": "countOfThings"
          }
        ]
      }
```
## <a name="sample-input-json-structure"></a>Örnek giriş JSON yapısı

Bu _JSON_ yapısı, Web API 'nize gönderilecek yükü temsil eder.
Her zaman şu kısıtlamalara uyar:

* En üst düzey varlık `values` olarak adlandırılır ve bir nesne dizisi olur. Bu tür nesnelerin sayısı en fazla `batchSize` olacaktır
* `values` dizideki her bir nesne,
    * Bu kaydı tanımlamak için kullanılan **benzersiz** bir dize olan `recordId` özelliği.
    * _JSON_ nesnesi olan bir `data` özelliği. `data` özelliğinin alanları, yetenek tanımının `inputs` bölümünde belirtilen "adlara" karşılık gelir. Bu alanların değeri, bu alanların `source` (belgedeki bir alandan ya da başka bir beceriye ait olabilir) ait olacaktır

```json
{
    "values": [
      {
        "recordId": "0",
        "data":
           {
             "text": "Este es un contrato en Inglés",
             "language": "es",
             "countOf": "words"
           }
      },
      {
        "recordId": "1",
        "data":
           {
             "text": "Hello world",
             "language": "en",
             "countOf": "characters"
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Hello world \r\n Hi World",
             "language": "en",
             "countOf": "lines"
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "Test",
             "language": "es",
             "countOf": null
           }
      }
    ]
}
```

## <a name="sample-output-json-structure"></a>Örnek çıkış JSON yapısı

"Output", Web API 'nizden döndürülen yanıta karşılık gelir. Web API 'si yalnızca bir _JSON_ yükü döndürmelidir (`Content-Type` yanıt başlığına bakarak doğrulanır) ve aşağıdaki kısıtlamalara uygun olmalıdır:

* Bir nesne dizisi olması gereken `values` adlı bir üst düzey varlık olmalıdır.
* Dizideki nesne sayısı, Web API 'sine gönderilen nesne sayısıyla aynı olmalıdır.
* Her nesne şunları içermelidir:
   * `recordId` özelliği
   * Alanların `output` "adlar" ile eşleşen ve değeri enzenginleştirme olarak kabul edildiği bir nesne olan `data` özelliği.
   * Bir `errors` özelliği, Dizin Oluşturucu yürütme geçmişine eklenecek hataları listelemesi ile karşılaşıldı. Bu özellik gereklidir, ancak `null` bir değere sahip olabilir.
   * Bir `warnings` özelliği, Dizin Oluşturucu yürütme geçmişine eklenecek uyarıları listelemesi ile karşılaşıldı. Bu özellik gereklidir, ancak `null` bir değere sahip olabilir.
* `values` dizisindeki nesneler, Web API 'sine istek olarak gönderilen `values` dizisindeki nesnelerle aynı sırada olmamalıdır. Ancak, `recordId` bağıntı için kullanılır, böylece Web API 'sine yönelik özgün isteğin bir parçası olmayan bir `recordId` içeren yanıttaki tüm kayıtlar atılır.

```json
{
    "values": [
        {
            "recordId": "3",
            "data": {
            },
            "errors": [
              {
                "message" : "Cannot understand what needs to be counted"
              }
            ],
            "warnings": null
        },
        {
            "recordId": "2",
            "data": {
                "count": 2
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "0",
            "data": {
                "count": 6
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "1",
            "data": {
                "count": 11
            },
            "errors": null,
            "warnings": null
        },
    ]
}

```

## <a name="error-cases"></a>Hata durumları
Web API 'nizin kullanılamaz hale veya başarılı olmayan durum kodları gönderilmesine ek olarak aşağıdakiler hatalı durumlar olarak kabul edilir:

* Web API 'SI bir başarı durum kodu döndürürse ancak yanıt `application/json` olmadığını gösteriyorsa, yanıt geçersiz olarak kabul edilir ve hiçbir zenginleştirilmez.
* Yanıt `values` dizisinde **geçersiz** (özgün istekte değil `recordId` veya yinelenen değerler içeren) kayıtları varsa, **Bu** kayıtlar için hiçbir zenginleştirme gerçekleştirilmez.

Web API 'sinin kullanılamadığı veya bir HTTP hatası döndürdüğü durumlarda, Dizin Oluşturucu yürütme geçmişine HTTP hatası ile ilgili tüm ayrıntıları içeren bir kolay hata eklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Güç becerileri: özel yeteneklerin bir deposu](https://aka.ms/powerskills)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Bilişsel arama 'ya özel yetenek ekleme](cognitive-search-custom-skill-interface.md)
+ [Örnek: bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)