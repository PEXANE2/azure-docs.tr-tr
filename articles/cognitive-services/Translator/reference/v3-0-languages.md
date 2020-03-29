---
title: Çevirmen Metin API Dilleri Yöntemi
titleSuffix: Azure Cognitive Services
description: Diller yöntemi, şu anda Çevirmen Metin API'sinin diğer işlemleri tarafından desteklenen dil kümesini alır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 37f70399e8125db559098869cdfffdf4533498d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73835826"
---
# <a name="translator-text-api-30-languages"></a>Çevirmen Metin API 3.0: Diller

Şu anda Çevirmen Metin API'sinin diğer işlemleri tarafından desteklenen dil kümesini alır. 

## <a name="request-url"></a>İstek URL'si

Bir `GET` istek gönderin:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>api-sürümü</td>
    <td><em>Gerekli parametre</em>.<br/>İstemci tarafından istenen API sürümü. Değer. `3.0`</td>
  </tr>
  <tr>
    <td>scope</td>
    <td>*İsteğe bağlı parametre*.<br/>Döndürülecek dil grubunu tanımlayan virgülle ayrılmış adlar listesi. İzin verilen grup `translation` `transliteration` adları `dictionary`şunlardır: , ve . Kapsam verilmezse, tüm gruplar döndürülür ve bu `scope=translation,transliteration,dictionary`da 'nın geçirilmesine eşdeğerdir. Hangi desteklenen dil kümesinin senaryonuz için uygun olduğuna karar vermek için [yanıt nesnesinin](#response-body)açıklamasına bakın.</td>
  </tr>
</table> 

İstek üstbilgileri şunlardır:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Accept-Language</td>
    <td>*İsteğe bağlı istek üstbilgi.*<br/>Kullanıcı arabirimi dizelerinde kullanılacak dil. Yanıttaki alanlardan bazıları dillerin adları veya bölgelerin adlarıdır. Bu adların döndürüldildiği dili tanımlamak için bu parametreyi kullanın. Dil, iyi biçimlendirilmiş bir BCP 47 dil etiketi sağlayarak belirtilir. Örneğin, değeri `fr` Fransızca ad istemek için kullanın `zh-Hant` veya Değeri Çince Geleneksel adlar istemek için kullanın.<br/>Hedef dil belirtilmediğinde veya yerelleştirme olmadığında adlar İngilizce dilinde sağlanır.
    </td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı istek üstbilgi.*<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID.</td>
  </tr>
</table> 

Dil kaynaklarını almak için kimlik doğrulama gerekli değildir.

## <a name="response-body"></a>Yanıt gövdesi

İstemci, `scope` hangi dil gruplarıyla ilgilendiğini tanımlamak için sorgu parametresini kullanır.

* `scope=translation`metni bir dilden başka bir dile çevirmek için desteklenen diller sağlar;

* `scope=transliteration`bir dildeki metni bir komut dosyasından diğerine dönüştürme özellikleri sağlar;

* `scope=dictionary`işlemlerin veri döndürdediği `Dictionary` dil çiftleri sağlar.

İstemci, virgülle ayrılmış bir ad listesi belirterek aynı anda birden fazla grup alabilir. Örneğin, `scope=translation,transliteration,dictionary` tüm gruplar için desteklenen dilleri döndürecek.

Başarılı bir yanıt, istenen her grup için bir özelliğe sahip bir JSON nesnesidir:

```json
{
    "translation": {
        //... set of languages supported to translate text (scope=translation)
    },
    "transliteration": {
        //... set of languages supported to convert between scripts (scope=transliteration)
    },
    "dictionary": {
        //... set of languages supported for alternative translations and examples (scope=dictionary)
    }
}
```

Her özellik için değer aşağıdaki gibidir.

* `translation`Özellik

  Özelliğin `translation` değeri (anahtar, değer) çiftleri sözlüğüdür. Her anahtar bir BCP 47 dil etiketidir. Anahtar, metnin çevrilebileceği veya çevrilebileceği bir dili tanımlar. Anahtarla ilişkili değer, dili açıklayan özelliklere sahip bir JSON nesnesidir:

  * `name`: Üstbilgi aracılığıyla `Accept-Language` istenen yerel dilde dilin görüntüadı.

  * `nativeName`: Bu dilin yerel dilindeki dilin görüntüadı.

  * `dir`: Sağdan sola `rtl` veya soldan sağa diller `ltr` için olan yönlülük.

  Bir örnek:
          
  ```json
  {
    "translation": {
      ...
      "fr": {
        "name": "French",
        "nativeName": "Français",
        "dir": "ltr"
      },
      ...
    }
  }
  ```

* `transliteration`Özellik

  Özelliğin `transliteration` değeri (anahtar, değer) çiftleri sözlüğüdür. Her anahtar bir BCP 47 dil etiketidir. Anahtar, metnin bir komut dosyasından başka bir komut dosyasına dönüştürülebileceği bir dili tanımlar. Anahtarla ilişkili değer, dili ve desteklenen komut dosyalarını açıklayan özelliklere sahip bir JSON nesnesidir:

  * `name`: Üstbilgi aracılığıyla `Accept-Language` istenen yerel dilde dilin görüntüadı.

  * `nativeName`: Bu dilin yerel dilindeki dilin görüntüadı.

  * `scripts`: Dönüştürülecek komut dosyaları listesi. Listenin her `scripts` öğesinin özellikleri vardır:

    * `code`: Komut dosyasını tanımlayan kod.

    * `name`: Üstbilgi aracılığıyla `Accept-Language` istenen yerel bölmede komut dosyasının görüntüadı.

    * `nativeName`: Dilin yerel dilindeki dilin görüntüadı.

    * `dir`: Sağdan sola `rtl` veya soldan sağa diller `ltr` için olan yönlülük.

    * `toScripts`: Metni dönüştürmek için kullanılabilen komut dosyaları listesi. Listenin `toScripts` her öğesiözellikleri `code` `name`vardır `nativeName`, `dir` , , ve daha önce açıklandığı gibi.

  Bir örnek:

  ```json
  {
    "transliteration": {
      ...
      "ja": {
        "name": "Japanese",
        "nativeName": "日本語",
        "scripts": [
          {
            "code": "Jpan",
            "name": "Japanese",
            "nativeName": "日本語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Latn",
                "name": "Latin",
                "nativeName": "ラテン語",
                "dir": "ltr"
              }
            ]
          },
          {
            "code": "Latn",
            "name": "Latin",
            "nativeName": "ラテン語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Jpan",
                "name": "Japanese",
                "nativeName": "日本語",
                "dir": "ltr"
              }
            ]
          }
        ]
      },
      ...
    }
  }
  ```

* `dictionary`Özellik

  Özelliğin `dictionary` değeri (anahtar, değer) çiftleri sözlüğüdür. Her anahtar bir BCP 47 dil etiketidir. Anahtar, alternatif çevirilerin ve geri çevirilerin kullanılabildiği bir dili tanımlar. Değer, kaynak dili ve kullanılabilir çevirileri olan hedef dilleri açıklayan bir JSON nesnesidir:

  * `name`: Üstbilgi aracılığıyla `Accept-Language` istenen yerel deki kaynak dilin görüntü adı.

  * `nativeName`: Bu dilin yerel dilindeki dilin görüntüadı.

  * `dir`: Sağdan sola `rtl` veya soldan sağa diller `ltr` için olan yönlülük.

  * `translations`: Kaynak dilde ifade edilen sorgu için alternatif çevirileri olan dillerin listesi ve örnekler. Listenin her `translations` öğesinin özellikleri vardır:

    * `name`: Üstbilgi aracılığıyla `Accept-Language` istenen yerel dilde hedef dilin görüntüadı.

    * `nativeName`: Hedef dil için yerel dilde hedef dilin görüntülenin adı.

    * `dir`: Sağdan sola `rtl` veya soldan sağa diller `ltr` için olan yönlülük.
    
    * `code`: Hedef dili tanımlayan dil kodu.

  Bir örnek:

  ```json
  "es": {
    "name": "Spanish",
    "nativeName": "Español",
    "dir": "ltr",
    "translations": [
      {
        "name": "English",
        "nativeName": "English",
        "dir": "ltr",
        "code": "en"
      }
    ]
  },
  ```

Yanıt nesnesinin yapısı API sürümünde bir değişiklik olmadan değişmez. API'nin aynı sürümünde, Microsoft Translator hizmetleri tarafından desteklenen dillerin listesini sürekli olarak genişlettiklerinden, kullanılabilir dillerin listesi zaman içinde değişebilir.

Desteklenen dillerin listesi sık sık değişmez. Ağ bant genişliğini kaydetmek ve yanıt verme yeteneğini artırmak için, istemci uygulaması`ETag`dil kaynaklarını ve ilgili varlık etiketini önbelleğe almayı düşünmelidir ( ). Daha sonra, istemci uygulaması düzenli aralıklarla (örneğin, her 24 saatte bir) desteklenen dillerin en son kümesini almak için hizmeti sorgulayabilir. Üstbilgi `ETag` alanındaki geçerli değerin geçirilmesi, hizmetin yanıtı optimize etmesine olanak sağlar. `If-None-Match` Kaynak değiştirilmemişse, hizmet durum kodu 304 ve boş bir yanıt gövdesi döndürecek.

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Etag</td>
    <td>Desteklenen dillerin istenen grupları için varlık etiketinin geçerli değeri. Sonraki istekleri daha verimli hale getirmek `ETag` için, `If-None-Match` istemci değeri bir üstbilgi alanına gönderebilir.
    </td>
  </tr>
  <tr>
    <td>X-RequestId</td>
    <td>İsteği tanımlamak için hizmet tarafından oluşturulan değer. Sorun giderme amacıyla kullanılır.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Aşağıda, bir isteğin döndürdettiği olası HTTP durum kodları vereme olasılığı ve 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>304</td>
    <td>Kaynak, istek üstbilgisi `If-None-Match`tarafından belirtilen sürümden beri değiştirilmedi.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerinden biri eksik veya geçersiz. Yeniden denemeden önce istek parametrelerini düzeltin.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>İstemci istek sınırlarını aştığı için sunucu isteği reddetti.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, `X-RequestId`yanıt üstbilgisinden identifier `X-ClientTraceId`ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, `X-RequestId`yanıt üstbilgisinden identifier `X-ClientTraceId`ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
</table> 

Bir hata oluşursa, istek de bir JSON hata yanıtı döndürecek. Hata kodu, hatayı daha fazla kategorilere ayırmak için 3 basamaklı HTTP durum kodunu ve ardından 3 basamaklı bir sayıyı birleştiren 6 basamaklı bir sayıdır. Yaygın hata kodları [v3 Translator Text API başvuru sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, metin çevirisi için desteklenen dillerin nasıl alınabildiğini gösterir.

```curl
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```
