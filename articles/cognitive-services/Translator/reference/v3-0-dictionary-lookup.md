---
title: Çevirmen Metin API Sözlük Arama Yöntemi
titleSuffix: Azure Cognitive Services
description: Sözlük Arama yöntemi, bir sözcük ve az sayıda deyim ifade için alternatif çeviriler sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: bd27827441082698bb4e0b43e7dd22d5b7e66539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76548960"
---
# <a name="translator-text-api-30-dictionary-lookup"></a>Çevirmen Metin API 3.0: Sözlük Arama

Bir sözcük ve az sayıda deyim ifade için alternatif çeviriler sağlar. Her çevirinin bir konuşma bölümü ve bir geri çeviri listesi vardır. Geri çeviriler, kullanıcının çeviriyi bağlam içinde anlamasını sağlar. [Sözlük Örneği](./v3-0-dictionary-examples.md) işlemi, her çeviri çiftinin örnek kullanımlarını görmek için daha fazla ayrıntıya inmenizi sağlar.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

| Sorgu Parametresi  | Açıklama |
| ------ | ----------- |
| api-sürümü <img width=200/>   | **Gerekli parametre**.<br/>İstemci tarafından istenen API sürümü. Değer,`3.0` |
| Kaynak | **Gerekli parametre**.<br/>Giriş metninin dilini belirtir. Kaynak `dictionary` dil, kapsamda bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır. |
| -   | **Gerekli parametre**.<br/>Çıktı metninin dilini belirtir. Hedef `dictionary` dil, kapsamda bulunan [desteklenen dillerden](v3-0-languages.md) biri olmalıdır. |


İstek üstbilgisi şunları içerir:

| Üst bilgiler  | Açıklama |
| ------ | ----------- |
| Kimlik doğrulama üstbilgisi(lar) <img width=200/>  | **Gerekli istek üstbilgi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulama için kullanılabilir seçeneklere</a>bakın. |
| İçerik Türü | **Gerekli istek üstbilgi**.<br/>Yükün içerik türünü belirtir. Olası değerler `application/json`şunlardır: . |
| İçerik Uzunluğu   | **Gerekli istek üstbilgi**.<br/>İstek gövdesinin uzunluğu. |
| X-ClientTraceId   | **İsteğe bağlı**.<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID. 'li bir sorgu parametresi `ClientTraceId`kullanarak sorgu dizesinde izleme kimliği eklerseniz, bu üstbilginin atlayabilirsiniz. |

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, arama terimini temsil `Text`eden bir dize özelliğine sahip bir JSON nesnesidir.

```json
[
    {"Text":"fly"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğeye sahip olabilir.
* Bir dizi öğesinin metin değeri boşluklar da dahil olmak üzere 100 karakteri geçemez.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her dize için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terimin normalleştirilmiş biçimini veren bir dize. Örneğin, istek "JOHN" ise, normalleştirilmiş form "john" olacaktır. Bu alanın [içeriği, arama örneklerine](./v3-0-dictionary-examples.md)giriş olur.
    
  * `displaySource`: Son kullanıcı görüntülemesi için en uygun biçimde kaynak terimi veren dize. Örneğin, giriş "JOHN" ise, görüntü formu adın olağan yazım ını yansıtır: "John". 

  * `translations`: Kaynak terime ait çevirilerin listesi. Listenin her öğesi aşağıdaki özelliklere sahip bir nesnedir:

    * `normalizedTarget`: Hedef dilde bu terimin normalleştirilmiş biçimini veren bir dize. Bu değer, [arama örnekleri](./v3-0-dictionary-examples.md)için giriş olarak kullanılmalıdır.

    * `displayTarget`: Hedef dilde ve son kullanıcı görüntülemesi için en uygun biçimde terimi veren bir dize. Genel olarak, bu sadece `normalizedTarget` büyük harf açısından farklı olacaktır. Örneğin, "Juan" gibi uygun bir `normalizedTarget = "juan"` ad `displayTarget = "Juan"`olacak ve .

    * `posTag`: Bu terimi bir konuşma parçası etiketiyle ilişkilendiren bir dize.

        | Etiket adı | Açıklama  |
        |----------|--------------|
        | S      | Sıfatlar   |
        | Adv      | Zarf      |
        | CONJ     | Bağlaçlar |
        | Det      | Belirleyiciler  |
        | Kalıcı    | Fiiller        |
        | Isim     | İsimler        |
        | Hazırlık     | Edatlar |
        | Pron     | Zamirler     |
        | Fiil     | Fiiller        |
        | Diğer    | Diğer        |

        Uygulama notu olarak, bu etiketler İngilizce tarafını etiketleyen ve daha sonra her kaynak/hedef çifti için en sık etiket alan konuşma nın bir parçası tarafından belirlenir. Bu nedenle, insanlar İspanyolca bir kelimeyi Sık sık İngilizce'de farklı bir konuşma bölümü etiketine çevirirlerse, etiketler (İspanyolca kelimeyle ilgili olarak) yanlış olabilir.

    * `confidence`: 0.0 ile 1.0 arasındaki bir değer, bu çeviri çiftinin "güven"ini (ya da belki daha doğrusu "eğitim verilerindeki olasılık") temsil eder. Bir kaynak sözcüğün güven puanlarının toplamı 1,0'a kadar olabilir veya toplamı olmayabilir. 

    * `prefixWord`: Çevirinin öneki olarak görüntülenecek sözcüğü veren bir dize. Şu anda, bu cinsiyet belirleyicileri olan dillerde, adların cinsiyet belirleyicisi olduğunu. Örneğin, İspanyolca "mosca" kelimesinin öneki "la"dır, çünkü "mosca" İspanyolca'da dişil bir alamettir. Bu yalnızca çeviriye bağlıdır, kaynağa değil. Önek yoksa, boş dize olacaktır.
    
    * `backTranslations`: Hedefin "geri çevirileri" listesi. Örneğin, hedefin çevirebileceği kaynak sözcükler. Listenin istenen kaynak sözcüğü içerdiği garanti edilir (örneğin, yukarı bakan kaynak sözcük "sinek" ise, listede "sinek" olacağı `backTranslations` garanti edilir). Ancak, ilk konumda olması garanti edilmez ve genellikle olmayacaktır. Listenin `backTranslations` her öğesi aşağıdaki özellikler tarafından açıklanan bir nesnedir:

        * `normalizedText`: Hedefin geri çevirisi olan kaynak terimin normalleştirilmiş biçimini veren bir dize. Bu değer, [arama örnekleri](./v3-0-dictionary-examples.md)için giriş olarak kullanılmalıdır.        

        * `displayText`: Son kullanıcı görüntülemesi için en uygun biçimde hedefin geri çevirisi olan kaynak terimi veren dize.

        * `numExamples`: Bu çeviri çifti için kullanılabilen örnek sayısını temsil eden bir tamsayı. Gerçek [örnekler, arama örnekleri](./v3-0-dictionary-examples.md)için ayrı bir çağrı ile alınmalıdır. Sayı çoğunlukla bir UX görüntü kolaylaştırmak için tasarlanmıştır. Örneğin, örnek sayısı sıfırdan büyükse, kullanıcı arabirimi arka çeviriye bir köprü ekleyebilir ve örnek yoksa arka çeviriyi düz metin olarak gösterebilir. ["Kötü" örnekleri](./v3-0-dictionary-examples.md) kaldırmak için anında ek filtreleme `numExamples`uygulanabileceğinden, arama örnekleri için bir çağrı tarafından döndürülen gerçek örnek sayısının daha az olabileceğini unutmayın.
        
        * `frequencyCount`: Verilerdeki bu çeviri çiftinin sıklığını temsil eden bir tamsayı. Bu alanın temel amacı, en sık terimlerilk böylece geri çevirileri sıralamak için bir araç ile bir kullanıcı arabirimi sağlamaktır.

    > [!NOTE]
    > Arama terimi sözlükte yoksa, yanıt 200 (Tamam) ancak `translations` liste boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnek, İngilizce terimin `fly` İspanyolca alternatif çeviriler nasıl araştırıldığını gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

Yanıt gövdesi (netlik için kısaltılır):

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

Bu örnek, bakılması gereken terim geçerli sözlük çifti için olmadığında ne olduğunu gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

Terim sözlükte bulunmadığından, yanıt gövdesi boş `translations` bir liste içerir.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
