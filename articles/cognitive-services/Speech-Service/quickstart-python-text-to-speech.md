---
title: Metinden konuşmaya dönüştürme, Python - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, Python ve Text-to-Speech REST API'yi kullanarak metinden konuşmaya nasıl dönüştüreceğinizi öğreneceksiniz. Bu kılavuzda yer alan örnek metin Konuşma Sentezi Biçimlendirme Dili (SSML) olarak yapılandırılmıştır. Bu, konuşma yanıtının sesini ve dilini seçmenize olanak tanır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: how-to
ms.date: 04/13/2020
ms.author: trbye
ms.openlocfilehash: 171fdb033cba422d8ba580da3ab54db88ca20872
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400826"
---
# <a name="convert-text-to-speech-using-python"></a>Python'u kullanarak metinden konuşmaya dönüştürme

Bu makalede, Python'u ve metinden konuşmaya REST API'yi kullanarak metinden konuşmaya nasıl dönüştüreceğinizi öğreneceksiniz. Bu kılavuzdaki istek gövdesi, yanıtın sesini ve dilini seçmenize olanak tanıyan [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)olarak yapılandırılmıştır.

Bu makalede, Konuşma hizmeti kaynağı olan bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerekmektedir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Python 2.7.x veya 3.x
* <a href="https://visualstudio.microsoft.com/downloads/" target="_blank">Visual <span class="docon docon-navigate-external x-hidden-focus"> </span>Studio </a>, <a href="https://code.visualstudio.com/download" target="_blank">Visual Studio Code <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>, veya en sevdiğiniz metin editörü
* Konuşma hizmeti için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.py` adlı bir dosyaya kopyalayın.

```python
import os
import requests
import time
from xml.etree import ElementTree
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests`.

Bu modüller, zaman damgalı bir dosyaya konuşma yanıtı yazmak, HTTP isteğini oluşturmak ve metinden konuşmaya API'yi çağırmak için kullanılır.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarını ayarlayın ve TTS için bir istem oluşturun

Sonraki birkaç bölümde yetkilendirmeyi işlemek, metinden konuşmaya API'yi çağırmak ve yanıtı doğrulamak için yöntemler oluşturacaksınız. Bu örneğin Python 2.7.x ve 3.x ile çalışmasını sağlayacak bazı kodlar ekleyerek başlayalım.

```python
try:
    input = raw_input
except NameError:
    pass
```

Sonra, bir sınıf oluşturalım. Burada belirteç değişimi için yöntemlerimizi koyacağız ve metinden konuşmaya API'yi çağıracağız.

```python
class TextToSpeech(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.tts = input("What would you like to convert to speech: ")
        self.timestr = time.strftime("%Y%m%d-%H%M")
        self.access_token = None
```

Azure `subscription_key` portalından benzersiz anahtarınızdır. `tts`kullanıcıdan konuşmaya dönüştürülecek metni girmesini ister. Bu giriş bir dize gerçek, bu nedenle karakterler in kaçmasına gerek yoktur. Son `timestr` olarak, dosyanızı adlandırmak için kullanacağımız geçerli saati alır.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metinden konuşmaya REST API kimlik doğrulaması için bir erişim belirteci gerektirir. Bir erişim jetonu almak için bir değişim gereklidir. Bu örnek, bitiş noktasını kullanarak bir erişim belirteci için Konuşma hizmeti abonelik anahtarınızı `issueToken` değiştirir.

Bu örnek, Konuşma hizmeti aboneliğinizin Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölge kullanıyorsanız, `fetch_token_url`değeri güncelleştirin. Tam liste için [Bölgeler'e](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis)bakın.

Bu kodu sınıfa kopyalayın: `TextToSpeech`

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Kimlik doğrulama hakkında daha fazla bilgi için [erişim belirteciyle Kimlik Doğrulaması'na](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token)bakın.

## <a name="make-a-request-and-save-the-response"></a>İstekte bulunun ve yanıtı kaydedin

Burada isteği oluşturup konuşma yanıtını kaydedebilirsiniz. İlk olarak, ayarlamak `base_url` gerekir `path`ve . Bu örnek, Batı ABD bitiş noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, 'yi `base_url`güncelleştirdiğinizden emin olun Daha fazla bilgi için konuşma [hizmet bölgelerine](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech)bakın.

Ardından, istek için gerekli üstbilgi eklemeniz gerekir. Kaynağınızın adıyla `User-Agent` güncelleştirip (Azure portalında bulunan) ve tercih `X-Microsoft-OutputFormat` ettiğiniz ses çıktısına ayarladığınızdan emin olun. Çıktı biçimlerinin tam listesi için [Ses çıktılarına](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)bakın.

Daha sonra Konuşma Sentezi Biçimlendirme Dili (SSML) kullanarak istek gövdesini oluşturun. Bu örnek yapıyı tanımlar ve `tts` daha önce oluşturduğunuz girişi kullanır.

>[!NOTE]
> Bu örnek `Guy24kRUS` ses yazı tipini kullanır. Microsoft tarafından sağlanan seslerin/dillerin tam listesi için [Dil desteğine](language-support.md)bakın.
> Markanız için benzersiz, tanınabilir bir ses oluşturmak istiyorsanız, [bkz.](how-to-customize-voice-font.md)

Son olarak, servise bir istekte bulunacaksınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, konuşma yanıtı zaman damgalı bir dosyaya yazılır.

Bu kodu sınıfa kopyalayın: `TextToSpeech`

```python
def save_audio(self):
    base_url = 'https://westus.tts.speech.microsoft.com/'
    path = 'cognitiveservices/v1'
    constructed_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token,
        'Content-Type': 'application/ssml+xml',
        'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
        'User-Agent': 'YOUR_RESOURCE_NAME'
    }
    xml_body = ElementTree.Element('speak', version='1.0')
    xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
    voice = ElementTree.SubElement(xml_body, 'voice')
    voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
    voice.set(
        'name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)')
    voice.text = self.tts
    body = ElementTree.tostring(xml_body)

    response = requests.post(constructed_url, headers=headers, data=body)
    if response.status_code == 200:
        with open('sample-' + self.timestr + '.wav', 'wb') as audio:
            audio.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nYour TTS is ready for playback.\n")
    else:
        print("\nStatus code: " + str(response.status_code) +
              "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, sınıf anlık ve işlevleri aramak tır.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = TextToSpeech(subscription_key)
    app.get_token()
    app.save_audio()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, metinden konuşmaya örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
python tts.py
```

İstendiğinde, metinden konuşmaya dönüştürmek istediğiniz her şeyi yazın. Başarılı olursa, konuşma dosyası proje klasörünüzde bulunur. En sevdiğiniz medya oynatıcıkullanarak oynayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Python örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Metinden konuşmaya dönüştürmek için Python ve Speech SDK'yı kullanma](quickstarts/speech-to-text-from-microphone.md)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
