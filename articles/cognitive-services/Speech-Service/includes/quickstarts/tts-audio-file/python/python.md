---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: dapine
ms.openlocfilehash: 34d9dc2bdf99737e6b4c17abe37a18f795c19df8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275315"
---
## <a name="prerequisites"></a>Ön koşullar

* Konuşma hizmeti için azure abonelik anahtarı. [Ücretsiz bir tane alın.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Python 3.5 ile 3.8](https://www.python.org/downloads/)arasında.
* Python Speech SDK paketi bu işletim sistemleri için kullanılabilir:
    * Windows: x64 ve x86.
    * Mac: macOS X sürüm 10.12 veya sonrası.
    * Linux: Ubuntu 16.04/18.04, Debian 9, RHEL 7/8, CentOS 7/8 x64.
* Linux'ta, gerekli paketleri yüklemek için bu komutları çalıştırın:

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

# <a name="rhel--centos-8"></a>[RHEL / CentOS 8](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl python3
```

> [!NOTE]
> RHEL/CentOS 7/8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

---

* Windows'da, platformunuz [için Visual Studio 2019 için Microsoft Visual C++ Yeniden Dağıtılabilir'e](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) ihtiyacınız vardır.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu komut, Konuşma SDK için [PyPI](https://pypi.org/) Python paketini yükler:

```Bash
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Destek ve güncellemeler

Konuşma SDK Python paketindeki güncellemeler PyPI üzerinden dağıtılır ve [Sürüm notlarında duyurulur.](~/articles/cognitive-services/Speech-Service/releasenotes.md)
Yeni bir sürüm varsa, komutu `pip install --upgrade azure-cognitiveservices-speech`ile bunu güncelleyebilirsiniz.
Değişkeni inceleyerek şu anda `azure.cognitiveservices.speech.__version__` hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özelliğiniz yoksa Destek [ve Yardım seçeneklerine](~/articles/cognitive-services/Speech-Service/support.md)bakın.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

[Örnek kodu](#sample-code) bu hızlı başlangıçtan kaynak dosyaya `quickstart.py` kopyalayabilir ve IDE'nizde veya konsolunuzda çalıştırabilirsiniz:

```Bash
python quickstart.py
```

Ya da bu quickstart öğreticisini [Konuşma SDK örnek deposundan](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) [Jupyter](https://jupyter.org) dizüstü bilgisayar olarak indirebilir ve not defteri olarak çalıştırabilirsiniz.

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

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Visual Studio Code ile Konuşma SDK'yı yükleyin ve kullanın

1. [Python'un](https://www.python.org/downloads/)64 bit sürümünü bilgisayarınıza 3,5 ile 3,8 arasında indirin ve yükleyin.
1. [Visual Studio Code'u](https://code.visualstudio.com/Download)indirin ve kurun.
1. Visual Studio Kodunu açın ve Python uzantısını yükleyin. Menüden **Dosya** > **Tercihleri** > **Uzantıları'nı** seçin. **Python'u**arayın.

   ![Python uzantısını yükleme](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Projeyi depolamak için bir klasör oluşturun. Bir örnek Windows Explorer kullanarak.
1. Visual Studio Code'da **Dosya** simgesini seçin. Ardından oluşturduğunuz klasörü açın.

   ![Klasör açma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni dosya simgesini `speechsdk.py`seçerek yeni bir Python kaynak dosyası oluşturun.

   ![Dosya oluşturma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. [Python kodunu](#sample-code) kopyalayın, yapıştırın ve yeni oluşturulan dosyaya kaydedin.
1. Konuşma hizmeti abonelik bilgilerinizi ekleyin.
1. Seçilirse, bir Python yorumlayıcısı pencerenin altındaki durum çubuğunun sol tarafında görüntülenir.
   Aksi takdirde, kullanılabilir Python yorumlayıcılarının bir listesini getirin. Komut paletini<kbd>(Ctrl+Shift+P)</kbd>açın ve **Python girin: Yorumlayıcı'yı seçin.** Uygun bir tane seçin.
1. Speech SDK Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcısı için henüz yüklenmediyse bunu yapın.
   Konuşma SDK paketini yüklemek için bir terminal açın. Komut paletini tekrar getirin<kbd>(Ctrl+Shift+P)</kbd>ve **Terminalgirin: Yeni Entegre Terminal oluşturun.**
   Açılan terminalde, sisteminiz için `python -m pip install azure-cognitiveservices-speech` komutu veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde bir yere sağ tıklayın. **Terminal'de Python Dosyasini Çalıştır'ı**seçin.
   Metniniz konuşmaya dönüştürülür ve belirtilen ses verilerine kaydedilir.

   ```console
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

Bu yönergeleri izleyerek sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticibakın.](https://code.visualstudio.com/docs/python/python-tutorial)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
