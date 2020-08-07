---
title: Metin okuma seslerini, Python-konuşma hizmetini listeleme
titleSuffix: Azure Cognitive Services
description: Bu makalede, Python kullanarak bir bölge/uç nokta için standart ve sinir sesin tam listesini nasıl alabileceğinizi öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: how-to
ms.date: 04/13/2020
ms.author: trbye
ms.custom: devx-track-python
ms.openlocfilehash: b19c7f92b38d68b1f072b1bf66f23f1ddeddc197
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87876283"
---
# <a name="get-the-list-of-text-to-speech-voices-using-python"></a>Python kullanarak metin okuma sesin listesini alın

Bu makalede, Python kullanarak bir bölge/uç nokta için standart ve sinir sesin tam listesini nasıl alabileceğinizi öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir. Desteklenen bölgelerin listesi için bkz. [bölgeler](regions.md).

Bu makalede, bir konuşma hizmeti kaynağına sahip bir Azure bilişsel [Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerekir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Python 2.7.x veya 3.x
* <a href="https://visualstudio.microsoft.com/downloads/" target="_blank">Visual Studio <span class="docon docon-navigate-external x-hidden-focus"></span> </a>, <a href="https://code.visualstudio.com/download" target="_blank">Visual Studio Code <span class="docon docon-navigate-external x-hidden-focus"></span> </a>veya sık kullandığınız metin düzenleyiciniz
* Konuşma hizmeti için bir Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `get-voices.py` adlı bir dosyaya kopyalayın.

```python
import requests
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests`.

İstekler, metin okuma hizmetine HTTP istekleri için kullanılır.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarını ayarlama ve TTS için bir istem oluşturma

Sonraki birkaç bölümde, yetkilendirmeyi işlemek, metinden konuşmaya tanıma API 'sini çağırmak ve yanıtı doğrulamak için yöntemler oluşturacaksınız. Bir sınıf oluşturarak başlayalım. Bu, belirteç değişimi için yöntemlerimizi koyacağımız ve metinden konuşmaya API 'sini çağıran yerdir.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

, `subscription_key` Azure Portal benzersiz anahtardır.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Bu uç nokta, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir Exchange gereklidir. Bu örnek, uç noktasını kullanarak bir erişim belirteci için konuşma hizmeti abonelik anahtarınızı değiştirir `issueToken` .

Bu örnek, konuşma hizmeti aboneliğinizin Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölge kullanıyorsanız, değerini güncelleştirin `fetch_token_url` . Tam liste için bkz. [bölgeler](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu `GetVoices` sınıfa kopyalayın:

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
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteciyle kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>İstek yapın ve yanıtı kaydedin

Burada, isteği derleyip döndürülen seslerin listesini kaydedeceklürsünüz. İlk olarak, ve ayarlamanız gerekir `base_url` `path` . Bu örnek Batı ABD uç noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, ' yi güncelleştirdiğinizden emin olun `base_url` . Daha fazla bilgi için bkz. [konuşma hizmeti bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Sonra, istek için gerekli üstbilgileri ekleyin. Son olarak, hizmet için bir istek yaparsınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, yanıt dosyaya yazılır.

Bu kodu `GetVoices` sınıfa kopyalayın:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, sınıfınızın örneğini oluşturmak ve işlevlerinizi çağırmak için kullanılır.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Bu, örneği çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumu), proje dizininize gidin ve şunu çalıştırın:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek uygulamanızın kaynak kodundaki tüm gizli bilgileri abonelik anahtarları gibi kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Python örneklerini keşfet](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
