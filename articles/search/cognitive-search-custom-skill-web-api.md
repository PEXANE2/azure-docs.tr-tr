---
title: Özel Web API beceri beceri
titleSuffix: Azure Cognitive Search
description: Web API'lerine seslenerek Azure Bilişsel Arama beceri lerinin yeteneklerini genişletin. Özel kodunuzu tümleştirmek için Özel Web API becerisini kullanın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 29928d78c2cfc2f21def363341f8383c4efa89d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74484123"
---
# <a name="custom-web-api-skill-in-an-azure-cognitive-search-enrichment-pipeline"></a>Azure Bilişsel Arama zenginleştirme boru hattında özel Web API becerisi

**Özel Web API** becerisi, özel işlemler sağlayan bir Web API bitiş noktasına seslenerek AI zenginleştirme sini genişletmenize olanak tanır. Yerleşik becerilere benzer şekilde, **Özel Web API** becerisi giriş ve çıktılara sahiptir. Girişlere bağlı olarak, dizinleyici çalıştığında Web API'niz bir JSON yükü alır ve yanıt olarak bir JSON yükü yle birlikte bir başarı durum kodu çıkarır. Yanıtın, özel beceriniz tarafından belirtilen çıktılara sahip olması beklenir. Başka bir yanıt hata olarak kabul edilir ve hiçbir zenginleştirme gerçekleştirilir.

JSON yüklerinin yapısı bu belgede daha aşağıda açıklanmıştır.

> [!NOTE]
> Dizinleyici, Web API'sinden döndürülen belirli standart HTTP durum kodları için iki kez yeniden dener. Bu HTTP durum kodları şunlardır: 
> * `502 Bad Gateway`
> * `503 Service Unavailable`
> * `429 Too Many Requests`

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Custom.WebApiSkill

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| Urı | _JSON_ yükünün gönderilen Web API'sinin URI'si. Yalnızca **https URI** şemasına izin verilir |
| httpYöntem | Yükü gönderirken kullanılacak yöntem. İzin verilen `PUT` yöntemler,`POST` |
| http://namalar | Anahtarların üstbilgi adlarını ve değerlerini temsil ettiği anahtar değer çiftleri topluluğu, yükle birlikte Web API'nize gönderilecek üstbilgi değerlerini temsil eder. Aşağıdaki üstbilginin bu koleksiyonda olması `Accept`yasaktır: `Content-Type`, `Cookie` `Host`, `TE` `Accept-Charset` `Accept-Encoding` `Content-Length` `Upgrade`, , , , ,`Via` |
| timeout | (İsteğe bağlı) Belirtildiğinde, API araması yapan http istemcisinin zaman anına işaret ediyor. XSD "dayTimeDuration" değeri [(ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değerinin sınırlı bir alt kümesi) olarak biçimlendirilmelidir. Örneğin, `PT60S` 60 saniye. Ayaredilmezse, varsayılan değeri 30 saniye seçilir. Zaman açısından en fazla 230 saniye ve en az 1 saniye olarak ayarlanıyor. |
| batchSize | (İsteğe bağlı) API çağrısı başına kaç "veri kaydı" (aşağıdaki _JSON_ yük yapısına bakın) gönderilecek olduğunu gösterir. Ayaredilmezse, varsayılan olarak 1000 seçilir. API'nize dizin oluşturma iş ve yükleme arasında uygun bir denge elde etmek için bu parametreyi kullanmanızı öneririz |
| dereceOfParallelism | (İsteğe bağlı) Belirtildiğinde, dizinleyicinin sağladığınız bitiş noktasına paralel olarak yapacağı çağrı sayısını gösterir. Bitiş noktanız çok yüksek bir istek yükü altında başarısız oluyorsa bu değeri azaltabilir veya bitiş noktanız daha fazla isteği kabul edebilirse ve dizinleyicinin performansında artış istiyorsanız yükseltebilirsiniz.  Ayaredilmezse, varsayılan değeri 5 kullanılır. Paralellik derecesi en fazla 10 ve en az 1 olarak ayarlanabilir. |

## <a name="skill-inputs"></a>Beceri girdileri

Bu beceri için "önceden tanımlanmış" giriş ler yoktur. Bu becerinin yürütülmesi sırasında zaten mevcut olan bir veya daha fazla alanı giriş olarak seçebilirsiniz ve Web API'sine gönderilen _JSON_ yükü farklı alanlara sahip olacaktır.

## <a name="skill-outputs"></a>Beceri çıktıları

Bu beceri için "önceden tanımlanmış" çıktılar yoktur. Web API'nizin geri döneceği yanıta bağlı olarak, _JSON_ yanıtından alınabilmeleri için çıktı alanları ekleyin.


## <a name="sample-definition"></a>Örnek tanımı

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "A custom skill that can identify positions of different phrases in the source text",
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
            "name": "phraseList",
            "source": "/document/keyphrases"
          }
        ],
        "outputs": [
          {
            "name": "hitPositions"
          }
        ]
      }
```
## <a name="sample-input-json-structure"></a>Örnek giriş JSON yapısı

Bu _JSON_ yapısı, Web API'nize gönderilecek yükü temsil eder.
Her zaman şu kısıtlamaları takip edecektir:

* Üst düzey varlık denir `values` ve nesnelerin bir dizi olacaktır. Bu tür nesnelerin sayısı en fazla olacak`batchSize`
* Dizideki `values` her nesne,
    * Bu `recordId` kaydı tanımlamak için kullanılan **benzersiz** bir dize olan bir özellik.
    * `data` _JSON_ nesnesi olan bir özellik. Özelliğin `data` alanları, beceri tanımının `inputs` bölümünde belirtilen "adlara" karşılık gelir. Bu alanların değeri bu alanların `source` (belgedeki bir alandan veya potansiyel olarak başka bir beceriden olabilir) olacaktır.

```json
{
    "values": [
      {
        "recordId": "0",
        "data":
           {
             "text": "Este es un contrato en Inglés",
             "language": "es",
             "phraseList": ["Este", "Inglés"]
           }
      },
      {
        "recordId": "1",
        "data":
           {
             "text": "Hello world",
             "language": "en",
             "phraseList": ["Hi"]
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Hello world, Hi world",
             "language": "en",
             "phraseList": ["world"]
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "Test",
             "language": "es",
             "phraseList": []
           }
      }
    ]
}
```

## <a name="sample-output-json-structure"></a>Örnek çıktı JSON yapısı

"Çıktı", Web API'nizden döndürülen yanıta karşılık gelir. Web API'si yalnızca bir _JSON_ yükünü döndürmeli `Content-Type` (yanıt üstbilgisini bakarak doğrulanmış) ve aşağıdaki kısıtlamaları karşılamalıdır:

* Bir dizi nesne olması gereken `values` üst düzey bir varlık olmalıdır.
* Dizideki nesne sayısı, Web API'sine gönderilen nesne sayısıyla aynı olmalıdır.
* Her nesne olmalıdır:
   * Bir `recordId` özellik
   * `data` Alanların zenginleştiği ve değeri zenginleştirme olarak kabul edilen bir `output` özelliktir.
   * Dizinleyici `errors` yürütme geçmişine eklenecek karşılaşılan hataları listeleyen bir özellik. Bu özellik gereklidir, ancak `null` bir değeri olabilir.
   * Dizinleyici `warnings` yürütme geçmişine eklenecek karşılaşılan uyarıları listeleyen bir özellik. Bu özellik gereklidir, ancak `null` bir değeri olabilir.
* Dizideki `values` nesnelerin, Web API'sine istek olarak gönderilen `values` dizideki nesnelerle aynı sırada olması gerekmez. Ancak, `recordId` web API orijinal isteğinin bir `recordId` parçası değildi yanıtta herhangi bir kayıt atılır böylece korelasyon için kullanılır.

```json
{
    "values": [
        {
            "recordId": "3",
            "data": {
            },
            "errors": [
              {
                "message" : "'phraseList' should not be null or empty"
              }
            ],
            "warnings": null
        },
        {
            "recordId": "2",
            "data": {
                "hitPositions": [6, 16]
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "0",
            "data": {
                "hitPositions": [0, 23]
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "1",
            "data": {
                "hitPositions": []
            },
            "errors": null,
            "warnings": {
                "message": "No occurrences of 'Hi' were found in the input text"
            }
        },
    ]
}

```

## <a name="error-cases"></a>Hata örnekleri
Web API'nizin kullanılamamasına veya başarısız durum kodları göndermesine ek olarak aşağıdaki hatalı durumlar olarak kabul edilir:

* Web API bir başarı durum kodu döndürür ancak `application/json` yanıt o zaman yanıt geçersiz kabul edilir ve hiçbir zenginleştirme ler gerçekleştirilecektir olduğunu gösterir.
* Yanıt `values` dizisinde **geçersiz** (özgün istekte `recordId` veya yinelenen değerlerle) kayıtlar varsa, **bu** kayıtlar için zenginleştirme yapılmaz.

Web API'sının kullanılamadığı veya bir HTTP hatası döndürdeceği durumlarda, HTTP hatasıyla ilgili kullanılabilir ayrıntıları içeren dostane bir hata dizinleyici yürütme geçmişine eklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Bir AI zenginleştirme boru hattına özel beceri ekleme](cognitive-search-custom-skill-interface.md)
+ [Örnek: AI zenginleştirme için özel bir beceri oluşturma](cognitive-search-create-custom-skill-example.md)
