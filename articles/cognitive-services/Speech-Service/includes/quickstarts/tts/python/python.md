---
title: 'Quickstart: Synthesize konuşma, Python - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak Python'da konuşmayı nasıl sentezleyin
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: dapine
ms.openlocfilehash: 239bb1e43b6a89ed1e8a18d544cf646b5f2de9b9
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671865"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)

## <a name="support-and-updates"></a>Destek ve güncellemeler

Konuşma SDK Python paketindeki güncellemeler PyPI üzerinden dağıtılır ve [Sürüm notlarında duyurulur.](~/articles/cognitive-services/Speech-Service/releasenotes.md)
Yeni bir sürüm varsa, komutu `pip install --upgrade azure-cognitiveservices-speech`ile bunu güncelleyebilirsiniz.
Değişkeni inceleyerek şu anda `azure.cognitiveservices.speech.__version__` hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özelliğiniz yoksa Destek [ve Yardım seçeneklerine](~/articles/cognitive-services/Speech-Service/support.md)bakın.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

[Örnek kodu](#sample-code) bu hızlı başlangıçtan kaynak dosyaya `quickstart.py` kopyalayabilir ve IDE'nizde veya konsolunuzda çalıştırabilirsiniz:

```sh
python quickstart.py
```

Ya da bu quickstart öğreticisini [Konuşma SDK örnek deposundan](https://aka.ms/csspeech/samples) [Jupyter](https://jupyter.org) dizüstü bilgisayar olarak indirebilir ve not defteri olarak çalıştırabilirsiniz.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/text-to-speech/quickstart.py#code)]

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
   Aksi takdirde, kullanılabilir Python yorumlayıcılarının bir listesini getirin. Komut paletini açın (Ctrl+Shift+P) ve **Python girin: Yorumlayıcı'yı seçin.** Uygun bir tane seçin.
1. Speech SDK Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcısı için henüz yüklenmediyse bunu yapın.
   Konuşma SDK paketini yüklemek için bir terminal açın. Komut paletini tekrar getirin (Ctrl+Shift+P) ve **Terminalgirin: Yeni Entegre Terminal oluşturun.**
   Açılan terminalde, sisteminiz için `python -m pip install azure-cognitiveservices-speech` komutu veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde bir yere sağ tıklayın. **Terminal'de Python Dosyasini Çalıştır'ı**seçin.
   İstendiğinde bazı metinler yazın. Sentezlenen ses kısa bir süre sonra çalınır.

   ![Örnek çalıştırma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run-tts.png)

Bu yönergeleri izleyerek sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticibakın.](https://code.visualstudio.com/docs/python/python-tutorial)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
