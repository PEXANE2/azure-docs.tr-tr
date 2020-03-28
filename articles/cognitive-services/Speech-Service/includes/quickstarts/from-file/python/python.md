---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/20/2020
ms.author: dapine
ms.openlocfilehash: e01e6f0e38abe9bdd3afa83306b90725daf287ce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80116986"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma kaynağı oluşturma](../../../../get-started.md)
> * [LUIS uygulaması oluşturun ve bir uç nokta anahtarı alın](../../../../quickstarts/create-luis.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)

[!INCLUDE [Audio input format](~/articles/cognitive-services/speech-service/includes/audio-input-format-chart.md)]

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

> [!NOTE]
> Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

```python
import azure.cognitiveservices.speech as speechsdk

# Creates an instance of a speech config with specified subscription key and service region.
# Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Creates an audio configuration that points to an audio file.
# Replace with your own audio filename.
audio_filename = "whatstheweatherlike.wav"
audio_input = speechsdk.audio.AudioConfig(filename=audio_filename)

# Creates a recognizer with the given settings
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_input)

print("Recognizing first result...")

# Starts speech recognition, and returns after a single utterance is recognized. The end of a
# single utterance is determined by listening for silence at the end or until a maximum of 15
# seconds of audio is processed.  The task returns the recognition text as result. 
# Note: Since recognize_once() returns only a single utterance, it is suitable only for single
# shot recognition like command or query. 
# For long-running multi-utterance recognition, use start_continuous_recognition() instead.
result = speech_recognizer.recognize_once()

# Checks result.
if result.reason == speechsdk.ResultReason.RecognizedSpeech:
    print("Recognized: {}".format(result.text))
elif result.reason == speechsdk.ResultReason.NoMatch:
    print("No speech could be recognized: {}".format(result.no_match_details))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech Recognition canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        print("Error details: {}".format(cancellation_details.error_details))
```

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
   Aksi takdirde, kullanılabilir Python yorumlayıcılarının bir listesini getirin. <kbd>Ctrl+Shift+P</kbd> komut paletini açın ve **Python girin: Yorumlayıcı'yı seçin.** Uygun bir tane seçin.
1. Speech SDK Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcısı için henüz yüklenmediyse bunu yapın.
   Konuşma SDK paketini yüklemek için bir terminal açın. Komut paletini tekrar <kbd>ctrl+Shift+P'yi</kbd> getirin ve **Terminalgirin: Yeni Entegre Terminal oluşturun.**
   Açılan terminalde, sisteminiz için `python -m pip install azure-cognitiveservices-speech` komutu veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde bir yere sağ tıklayın. **Terminal'de Python Dosyasini Çalıştır'ı**seçin.
   Ses dosyanızdan gelen konuşma girişinin ilk 15 saniyesi tanınır ve konsol penceresinde oturum açar.

   ```console
   Recognizing first result...
   We recognized: What's the weather like?
   ```

Bu yönergeleri izleyerek sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticibakın.](https://code.visualstudio.com/docs/python/python-tutorial)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
