---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 4429a03709d0f565d5fcf98f6999c812c0d43561
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673093"
---
## <a name="prerequisites"></a>Ön koşullar

* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](~/articles/cognitive-services/Speech-Service/get-started.md).
* [Python 3,5 ile 3,8](https://www.python.org/downloads/)arasında.
* Python konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
    * Windows: x64 ve x86.
    * Mac: macOS X sürüm 10,12 veya üzeri.
    * Linux: Ubuntu 16.04/18.04, de, 9, RHEL 7/8, CentOS 7/8 in x64.
* Linux 'ta, gerekli paketleri yüklemek için şu komutları çalıştırın:

# <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

# <a name="debian-9"></a>[Debian 9](#tab/debian)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.2 libasound2
```

# <a name="rhelcentos"></a>[RHEL/CentOS](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl python3
```

> [!NOTE]
> - RHEL/CentOS 7 ' de, [konuşma SDK 'sı IÇIN RHEL/CentOS 7](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md)' yi yapılandırma yönergelerini izleyin.
> - RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

---

* Windows 'ta, platformunuz için [Visual Studio 2019 Için yeniden dağıtılabilir Microsoft Visual C++](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerekir.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu komut, konuşma SDK 'Sı için [Pypı](https://pypi.org/) 'den Python paketini yüklüyor:

```Bash
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](~/articles/cognitive-services/Speech-Service/releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, komutunu komutuyla güncelleştirebilirsiniz `pip install --upgrade azure-cognitiveservices-speech` .
Değişkeni inceleyerek Şu anda hangi sürümün yüklü olduğunu denetleyin `azure.cognitiveservices.speech.__version__` .

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](~/articles/cognitive-services/Speech-Service/support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK 'sını kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Bu hızlı başlangıçta [örnek kodu](#sample-code) bir kaynak dosyaya KOPYALAYABILIR `quickstart.py` ve IDE 'niz içinde veya konsolunda çalıştırabilirsiniz:

```Bash
python quickstart.py
```

Ya da bu hızlı başlangıç öğreticisini [konuşma SDK 'sı örnek deposundan](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) [Jupyter](https://jupyter.org) Not defteri olarak indirebilir ve Not defteri olarak çalıştırabilirsiniz.

### <a name="sample-code"></a>Örnek kod

````python
import azure.cognitiveservices.speech as speechsdk

# Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Creates an audio configuration that points to an audio file.
# Replace with your own audio filename.
audio_filename = "helloworld.wav"
audio_output = speechsdk.audio.AudioOutputConfig(filename=audio_filename)

# Creates a synthesizer with the given settings
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_output)

# Synthesizes the text to speech.
# Replace with your own text.
text = "Hello world!"
result = speech_synthesizer.speak_text_async(text).get()

# Checks result.
if result.reason == speechsdk.ResultReason.SynthesizingAudioCompleted:
    print("Speech synthesized to [{}] for text [{}]".format(audio_filename, text))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech synthesis canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        if cancellation_details.error_details:
            print("Error details: {}".format(cancellation_details.error_details))
    print("Did you update the subscription info?")
````

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Konuşma SDK 'sını Visual Studio Code ile yükleyip kullanma

1. Bilgisayarınızda [Python](https://www.python.org/downloads/), 3,8 3,5 ' nin 64 bitlik bir sürümünü indirip bilgisayarınıza yükleyin.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **Dosya**  >  **tercihleri**  >  **uzantıları** ' nı seçin. **Python**için arama yapın.

   ![Python uzantısını yükler](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Projenin depolandığı bir klasör oluşturun. Bir örnek, Windows Gezgini 'ni kullanmaktır.
1. Visual Studio Code, **Dosya** simgesini seçin. Ardından oluşturduğunuz klasörü açın.

   ![Bir klasörü açın](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni dosya simgesini seçerek yeni bir Python kaynak dosyası oluşturun `speechsdk.py` .

   ![Dosya oluşturma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. [Python kodunu](#sample-code) kopyalayın, yapıştırın ve yeni oluşturulan dosyaya kaydedin.
1. Konuşma hizmeti abonelik bilgilerinizi ekleyin.
1. Seçilirse, pencerenin alt kısmındaki durum çubuğunun sol tarafında bir Python yorumlayıcı görüntülenir.
   Aksi takdirde, kullanılabilir Python yorumlayıcılarını bir listesini alın. Komut paletini açın (<kbd>Ctrl + Shift + P</kbd>) ve **Python: yorumlayıcı Seç**' i girin. Uygun olanı seçin.
1. Konuşma SDK 'Sı Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcı için henüz yüklenmemişse bunu yapın.
   Konuşma SDK 'Sı paketini yüklemek için bir Terminal açın. Komut paletini tekrar açın (<kbd>Ctrl + Shift + P</kbd>) ve **Terminal: yeni tümleşik Terminal oluştur**' a girin.
   Açılan terminalde, `python -m pip install azure-cognitiveservices-speech` sisteminiz için komutu veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde herhangi bir yere sağ tıklayın. **Terminalde Python dosyasını çalıştır '** ı seçin.
   Metniniz konuşmaya dönüştürülür ve belirtilen ses verilerinde kaydedilir.

   ```console
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

Bu talimatları takip eden sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticisine](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
