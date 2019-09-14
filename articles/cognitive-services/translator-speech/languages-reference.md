---
title: Translator Konuşma Çevirisi API'si dilleri yöntemi
titleSuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'si dilleri yöntemini kullanın.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 46ac3928238382f56db5a799226bd3d9243b7ca2
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966405"
---
# <a name="translator-speech-api-languages"></a>Translator Konuşma Çevirisi API'si: Languages

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator Konuşma Çevirisi, hizmetlerinin desteklediği dillerin listesini sürekli olarak genişletir. Bu API 'yi, Translator Konuşma Çevirisi Hizmeti ile birlikte kullanılabilecek olan dil kümesini saptamak için kullanın.

Kullanılabilir dilleri almak için API kullanımını gösteren kod örnekleri, [Microsoft Translator GitHub sitesinde](https://github.com/MicrosoftTranslator)bulunabilir.

## <a name="implementation-notes"></a>Uygulama notları

### <a name="get-languages"></a>/Dilleri al

Bir çok dil kümesi, konuşma, metni dönüştürmek ve çeviri üzerinde birleştirilmiş bir konuşma oluşturmak için konuşma için kullanılabilir.

İstemci, `scope` ilgilendiğiniz dil kümelerini tanımlamak için sorgu parametresini kullanır.

* **Konuşmayı metne dönüştürme:** Konuşmayı metne dönüştürmek `scope=speech` için kullanılabilen dillerin kümesini almak için sorgu parametresini kullanın.
* **Metin çevirisi:** Bir metni dönüştürmek `scope=text` için kullanılabilen dillerin kümesini almak için sorgu parametresini kullanın.
* **Metinden konuşmaya:**  Çevrilmiş metni konuşmaya `scope=tts` geri çekmek için kullanılabilen dil ve sesler kümesini almak için sorgu parametresini kullanın.

Bir istemci, virgülle ayrılmış bir seçenek listesi belirterek birden çok kümesi aynı anda alabilir. Örneğin: `scope=speech,text,tts`.

Başarılı bir yanıt, istenen her küme için bir özelliği olan bir JSON nesnesidir.

```
{
    "speech": {
        //... Set of languages supported for speech-to-text
    },
    "text": {
        //... Set of languages supported for text translation
    },
    "tts": {
        //... Set of languages supported for text-to-speech
    }
}
```

İstemci, desteklenen dillerin hangi kümelerinin `scope` döndürülmesi gerektiğini seçmek için sorgu parametresini kullanabilmesi için, gerçek bir yanıt yalnızca yukarıda gösterilen tüm özelliklerin bir alt kümesini içerebilir.

Her bir özellikle birlikte sunulan değer aşağıdaki gibidir.

### <a name="speech-to-text-speech"></a>Konuşmayı metne dönüştürme (konuşma)

Konuşmadan metne özelliği `speech`ile ilişkili değer, (anahtar, değer) çiftlerinin bir sözlüğüdür. Her anahtar, konuşmadan metne yönelik olarak desteklenen bir dili tanımlar. Anahtar, istemcinin API 'ye geçirdiği tanıtıcıdır. Anahtarla ilişkili değer, aşağıdaki özelliklere sahip bir nesnedir:

* `name`: Dilin görünen adı.
* `language`: İlişkili yazılı dilin dil etiketi. Aşağıdaki "metin işlemi" başlığına bakın.
Örnek:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Metin çevirisi (metin)

`text` Özelliği ile ilişkili değer aynı zamanda her bir anahtarın metin çevirisi için desteklenen bir dili tanımladığı bir sözlüktür. Anahtarla ilişkili değer dili açıklar:

* `name`: Dilin görünen adı.
* `dir`: Sağdan sola diller veya `rtl` `ltr` soldan sağa diller için olan yönlülük.

Örnek:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Metinden konuşmaya (TTS)

Metin okuma özelliği, TTS ile ilişkili değer aynı zamanda her bir anahtarın desteklenen bir sesi tanımladığı bir sözlüktür. Bir ses nesnesinin öznitelikleri şunlardır:

* `displayName`: Ses için görünen ad.
* `gender`: Sesin Cinsiyeti (erkek veya kadın).
* `locale`: Birincil dil alt etiketi ve bölgesi alt etiketi ile sesin dil etiketi.
* `language`: İlişkili yazılı dilin dil etiketi.
* `languageName`: Dilin görünen adı.
* `regionName`: Bu dilin bölge görünen adı.

Örnek:

```
{
    "tts": {
        "en-US-Zira": {
            "displayName": "Zira",
            "gender": "female",
            "locale": "en-US",
            "languageName": "English",
            "regionName": "United States",
            "language": "en"
        }
}
```

### <a name="localization"></a>Yerelleştirme
Hizmet, metin çevirisinde desteklenen tüm diller için ' Accept-Language ' üst bilgisinin dilindeki tüm adları döndürür.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
Desteklenen dillerin kümesini açıklayan nesne.

Modelexörnek değeri:

Langagues {konuşma (nesne, isteğe bağlı), metin (nesne, isteğe bağlı), TTS (nesne, isteğe bağlı)}

### <a name="headers"></a>Üst bilgiler

|Üstbilgi|Açıklama|Type|
:--|:--|:--|
X-RequestId|İsteği tanımlamak için sunucu tarafından oluşturulan ve sorun giderme amacıyla kullanılan değer.|dize|

### <a name="parameters"></a>Parametreler

|Parametre|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|
|API sürümü    |İstemci tarafından istenen API 'nin sürümü. İzin verilen değerler: `1.0`.|query|dize|
|scope  |İstemciye döndürülecek desteklenen diller veya sesler kümesi. Bu parametre, anahtar kelimelerin virgülle ayrılmış bir listesi olarak belirtilir. Aşağıdaki anahtar sözcükler kullanılabilir:<ul><li>`speech`: Konuşmayı aktarmak için desteklenen diller kümesini sağlar.</li><li>`tts`: Metin okuma dönüştürmesi için desteklenen ses kümesini sağlar.</li><li>`text`: Metin çevirmek için desteklenen diller kümesini sağlar.</li></ul>Bir değer belirtilmemişse, `scope` varsayılan değer olarak `text`değerine ayarlanır.|query|dize|
|X-Clienttraceıd    |Bir isteği izlemek için kullanılan, istemci tarafından oluşturulan bir GUID. Sorunların sorunlarını gidermeyi kolaylaştırmak için istemciler her bir istekle yeni bir değer sağlamalıdır ve günlüğe kaydeder.|header|dize|
|Kabul etme-dil    |Yanıttaki alanlardan bazıları dillerin veya bölgelerin adlarıdır. Adların döndürüleceği dili tanımlamak için bu parametreyi kullanın. Dil, iyi biçimlendirilmiş bir BCP 47 Language etiketi sağlayarak belirtilir. `text` Kapsamla döndürülen Dil tanımlayıcıları listesinden bir etiket seçin. Desteklenmeyen diller için, adlar Ingilizce dilinde sağlanır.<br/>Örneğin, Fransızca 'da ad istemek `fr` için değerini kullanın veya geleneksel Çince 'de ad istemek `zh-Hant` için değeri kullanın.|header|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Reason|
|:--|:--|
|400|Hatalı istek. Geçerli olduklarından emin olmak için giriş parametrelerini denetleyin. Response nesnesi, hatanın daha ayrıntılı bir açıklamasını içerir.|
|429|Çok fazla istek.|
|500|Bir hata oluştu. Hata devam ederse, lütfen istemci izleme tanımlayıcısı (X-Clienttraceıd) veya istek tanımlayıcısı (X-RequestId) ile bildirin.|
|503|Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Hata devam ederse, lütfen istemci izleme tanımlayıcısı (X-Clienttraceıd) veya istek tanımlayıcısı (X-RequestId) ile bildirin.|
