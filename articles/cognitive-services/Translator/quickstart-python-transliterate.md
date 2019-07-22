---
title: "Hızlı Başlangıç: Metin, Python-Translator Metin Çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python ve Translator Metin Çevirisi API'sini kullanarak bir metni bir alfabeden başka bir alfabeye çevirmeyi (dönüştürmeyi) öğreneceksiniz. Bu örnekte Japonca, Latin alfabesine dönüştürülmektedir.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cafaf67cfaa07d27bf4569efbc7f76196222cc2a
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348737"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-python"></a>Hızlı Başlangıç: Python kullanarak metni alfabeden ayırmak için Translator Metin Çevirisi API'si kullanın

Bu hızlı başlangıçta Python ve Translator Metin Çevirisi API'sini kullanarak bir metni bir alfabeden başka bir alfabeye çevirmeyi (dönüştürmeyi) öğreneceksiniz. Verilen örnekte Japonca, Latin alfabesine dönüştürülmektedir.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

>[!TIP]
> Tüm kodu aynı anda görmek isterseniz, bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

En sevdiğiniz IDE veya düzenleyiciyi kullanarak yeni bir proje oluşturun veya masaüstünüzde adlı `transliterate-text.py` bir dosya içeren yeni bir klasör oluşturun. Sonra bu kod parçacığını projenize/dosyanıza kopyalayın:

```python
# -*- coding: utf-8 -*-
import os
import requests
import uuid
import json
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests uuid`.

İlk açıklama Python yorumlayıcısına UTF-8 kodlaması kullanması gerektiğini bildirir. Ardından bir ortam değişkeninden abonelik anahtarınızı okumak, HTTP isteğini yapılandırmak, benzersiz bir tanımlayıcı oluşturmak ve Translator Metin Çevirisi API'si tarafından döndürülen JSON yanıtını işlemek için gereken modüller içeri aktarılır.

## <a name="set-the-subscription-key-base-url-and-path"></a>Abonelik anahtarını, temel URL’yi ve yolu ayarlayın

Bu örnek, `TRANSLATOR_TEXT_KEY` ortam değişkeninden Translator Metin Çevirisi abonelik anahtarınızı okumaya çalışır. Ortam değişkenlerini bilmiyorsanız, `subscriptionKey` öğesini dize olarak ayarlayabilir ve koşul deyimini açıklama satırı yapabilirsiniz.

Bu kodu projenize kopyalayın:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Translator Metin Çevirisi genel uç noktası olarak `base_url`ayarlanır. `path`, `transliterate` rotasını ayarlar ve API sürüm 3’ü kullanmak istediğimizi belirler.

`params`, giriş diline en olarak giriş ve çıkış betiklerini ayarlamak için kullanılır. Bu örnekte Japonca metni Latin alfabesine dönüştürüyoruz.

>[!NOTE]
> Uç noktalar, rotalar ve istek parametreleri hakkında daha fazla bilgi için bkz [. Translator metin çevirisi API'si 3,0: Alfabede olacak şekilde.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate)

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'
params = '&language=ja&fromScript=jpan&toScript=latn'
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Üst bilgileri ekleme

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz. Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Bu kod parçacığını projenize kopyalayın:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

Bilişsel hizmetler çoklu hizmet aboneliği kullanıyorsanız, istek parametrelerinize de dahil `Ocp-Apim-Subscription-Region` etmeniz gerekir. [Multi-Service aboneliğiyle kimlik doğrulama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-request-to-transliterate-text"></a>Metni başka alfabeye çevirmek için bir istek oluşturma

Başka alfabeye çevirmek istediğiniz dizeyi (veya dizeleri) tanımlayın:

```python
# Transliterate "good afternoon" from source Japanese.
# Note: You can pass more than one object in body.
body = [{
    'text': 'こんにちは'
}]
```

Ardından, `requests` modülünü kullanarak bir POST isteği oluşturacağız. Üç bağımsız değişken kabul eder: Birleştirilmiş URL, istek üst bilgileri ve istek gövdesi:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Yanıtı yazdırma

Son adım sonuçları yazdırmaktır. Bu kod parçacığı anahtarları sıralayarak, girintiyi ayarlayarak ve öğe ve anahtar ayırıcıları bildirerek sonuçların daha iyi görünmesini sağlar.

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
python transliterate-text.py
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "script": "latn",
        "text": "konnichiwa"
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Translator Metin Çevirisi API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Ayrıca bkz.

Translator Metin Çevirisi API'si nasıl kullanacağınızı öğrenin:

* [Metin çevirme](quickstart-python-translate.md)
* [Girişe göre dili belirleyin](quickstart-python-detect.md)
* [Alternatif çeviriler edinme](quickstart-python-dictionary.md)
* [Desteklenen dillerin listesini alma](quickstart-python-languages.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-python-sentences.md)
