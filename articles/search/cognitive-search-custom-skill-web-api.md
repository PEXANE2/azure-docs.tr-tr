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
ms.subservice: cognitive-search
ms.openlocfilehash: a01518158e063d68734b1230f11cae78f461faac
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70183458"
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
| uri | _JSON_ yükünün GÖNDERILECEĞI Web API 'sinin URI 'si. Yalnızca **https** URI şemasına izin veriliyor |
| httpMethod | Yük gönderilirken kullanılacak yöntem. İzin verilen yöntemler `PUT` veya`POST` |
| httpHeaders | Anahtarların üstbilgi adlarını ve değerlerini temsil ettiği anahtar-değer çiftleri koleksiyonu, yük ile birlikte Web API 'nize gönderilecek üst bilgi değerlerini temsil eder. Şu `Accept`üst bilgilerin bu koleksiyonda olması yasaktır:, `Cookie` `Content-Length` `Accept-Charset`, `Accept-Encoding`,, `Content-Type`,, `Host`, `TE`,, `Upgrade``Via` |
| zaman aşımı | Seçim Belirtildiğinde, API çağrısını yapan http istemcisinin zaman aşımını gösterir. XSD "dayTimeDuration" değeri ( [ıso 8601 Duration](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değerinin kısıtlı bir alt kümesi) olarak biçimlendirilmelidir. Örneğin, `PT60S` 60 saniye için. Ayarlanmamışsa, varsayılan değer olan 30 saniye seçilir. Zaman aşımı en fazla 230 saniyeye ayarlanabilir ve en az 1 saniye olabilir. |
| batchSize | Seçim Her API çağrısı için, kaç "veri kaydı" (aşağıdaki _JSON_ yük yapısına bakın) gönderileceğini belirtir. Ayarlanmamışsa, varsayılan olarak 1000 seçilidir. Dizin oluşturma işleme ve API 'niz üzerinde yükleme arasında uygun bir zorunluluğunu getirir elde etmek için bu parametreyi kullanmanızı öneririz |

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

* En üst düzey varlık çağrılır `values` ve bir nesne dizisi olur. Bu tür nesnelerin sayısı en fazla şu kadar olacaktır:`batchSize`
* `values` Dizideki her bir nesne,
    * Bu `recordId` kaydı tanımlamak için kullanılan **benzersiz** bir dize olan bir özellik.
    * JSON `data` nesnesi olan bir özellik . `data` Özelliğin alanları, yetenek tanımının `inputs` bölümünde belirtilen "adlara" karşılık gelir. Bu alanların `source` değeri bu alanlardan (belgedeki bir alandan ya da başka bir beceriye ait olabilir) olacaktır

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

"Output", Web API 'nizden döndürülen yanıta karşılık gelir. Web API 'si yalnızca bir _JSON_ yükü döndürmelidir ( `Content-Type` yanıt üstbilgisine bakılarak doğrulanır) ve aşağıdaki kısıtlamalara uygun olmalıdır:

* Bir nesne dizisi olması gereken adlı `values` üst düzey bir varlık olmalıdır.
* Dizideki nesne sayısı, Web API 'sine gönderilen nesne sayısıyla aynı olmalıdır.
* Her nesne şunları içermelidir:
   * Bir `recordId` Özellik
   * `data` Alanların`output` içindeki "adlar" ile eşleşen ve değeri enzenginleştirme olarak kabul edildiği bir nesne olan bir özellik.
   * Dizin Oluşturucu yürütme geçmişine eklenecek hatalarla karşılaşan bir dizi özellik.`errors` Bu özellik gereklidir, ancak bir `null` değere sahip olabilir.
   * Dizin `warnings` Oluşturucu yürütme geçmişine eklenecek herhangi bir uyarıyı listelemesi için bir özellik. Bu özellik gereklidir, ancak bir `null` değere sahip olabilir.
* `values` Dizideki nesneler, Web API 'sine istek olarak gönderilen `values` dizideki nesnelerle aynı sırada olmamalıdır. Ancak, bağıntısı için kullanıldığında, Web API 'sine yapılan özgün isteğin bir `recordId` parçası olan yanıttaki tüm kayıtlar atılır. `recordId`

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

* Web API 'si bir başarı durum kodu döndürürse ancak yanıt bunun `application/json` olmadığını gösteriyorsa, yanıt geçersiz olarak kabul edilir ve hiçbir zenginleştirilmez.
* Yanıt `recordId` dizisinde`values` geçersiz (orijinal istekte değil veya yinelenen değerler içeren) kayıtları varsa, **Bu** kayıtlar için hiçbir zenginleştirme gerçekleştirilmez.

Web API 'sinin kullanılamadığı veya bir HTTP hatası döndürdüğü durumlarda, Dizin Oluşturucu yürütme geçmişine HTTP hatası ile ilgili tüm ayrıntıları içeren bir kolay hata eklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Bilişsel arama 'ya özel yetenek ekleme](cognitive-search-custom-skill-interface.md)
+ [Örnek: Bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)