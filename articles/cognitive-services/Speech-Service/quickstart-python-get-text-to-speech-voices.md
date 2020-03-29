---
title: Metinden konuşmaya sesleri listele, Python - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, Python'u kullanarak bir bölge/bitiş noktası için standart ve sinirsel seslerin tam listesini nasıl alacağınızı öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: how-to
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 51fe6cea80e097f34432ab8dc7293c758bd8d720
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77119801"
---
# <a name="get-the-list-of-text-to-speech-voices-using-python"></a>Python'u kullanarak metinden konuşmaya seslerin listesini alın

Bu makalede, Python'u kullanarak bir bölge/bitiş noktası için standart ve sinirsel seslerin tam listesini nasıl alacağınızı öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir. Desteklenen bölgelerin listesi için [bkz.](regions.md)

Bu makalede, Konuşma hizmeti kaynağı olan bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerekmektedir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Python 2.7.x veya 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin editörü
* Konuşma hizmeti için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `get-voices.py` adlı bir dosyaya kopyalayın.

```python
import requests
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests`.

İstekler metinden konuşmaya hizmetine HTTP istekleri için kullanılır.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarını ayarlayın ve TTS için bir istem oluşturun

Sonraki birkaç bölümde yetkilendirmeyi işlemek, metinden konuşmaya API'yi çağırmak ve yanıtı doğrulamak için yöntemler oluşturacaksınız. Bir sınıf oluşturarak başlayalım. Burada belirteç değişimi için yöntemlerimizi koyacağız ve metinden konuşmaya API'yi çağıracağız.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

Azure `subscription_key` portalından benzersiz anahtarınızdır.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Bu bitiş noktası kimlik doğrulaması için bir erişim belirteci gerektirir. Bir erişim jetonu almak için bir değişim gereklidir. Bu örnek, bitiş noktasını kullanarak bir erişim belirteci için Konuşma hizmeti abonelik anahtarınızı `issueToken` değiştirir.

Bu örnek, Konuşma hizmeti aboneliğinizin Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölge kullanıyorsanız, `fetch_token_url`değeri güncelleştirin. Tam liste için [Bölgeler'e](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis)bakın.

Bu kodu sınıfa kopyalayın: `GetVoices`

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

Burada isteği oluşturup iade edilen seslerin listesini kaydedebilirsiniz. İlk olarak, ayarlamak `base_url` gerekir `path`ve . Bu örnek, Batı ABD bitiş noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, 'yi `base_url`güncelleştirdiğinizden emin olun Daha fazla bilgi için konuşma [hizmet bölgelerine](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech)bakın.

Ardından, istek için gerekli üstbilgi ekleyin. Son olarak, servise bir istekte bulunacaksınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, yanıt dosyaya yazılır.

Bu kodu sınıfa kopyalayın: `GetVoices`

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

Neredeyse bitti. Son adım, sınıf anlık ve işlevleri aramak tır.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örneği çalıştırmaya hazırsın. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Python örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
