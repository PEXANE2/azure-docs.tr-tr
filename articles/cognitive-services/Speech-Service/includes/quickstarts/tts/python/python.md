---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, Python-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Python 'da konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: e6daa0aaaca283dec11937d961886f95f1972b49
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400397"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](~/articles/cognitive-services/Speech-Service/releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, komutunu komutuyla `pip install --upgrade azure-cognitiveservices-speech`güncelleştirebilirsiniz.
`azure.cognitiveservices.speech.__version__` Değişkeni inceleyerek Şu anda hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](~/articles/cognitive-services/Speech-Service/support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK 'sını kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Bu hızlı başlangıçta [örnek kodu](#sample-code) bir kaynak dosyaya `quickstart.py` kopyalayabilir ve IDE 'niz içinde veya konsolunda çalıştırabilirsiniz:

```sh
python quickstart.py
```

Ya da bu hızlı başlangıç öğreticisini [konuşma SDK 'sı örnek deposundan](https://aka.ms/csspeech/samples) [Jupyter](https://jupyter.org) Not defteri olarak indirebilir ve Not defteri olarak çalıştırabilirsiniz.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/text-to-speech/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Konuşma SDK 'sını Visual Studio Code ile yükleyip kullanma

1. Bilgisayarınızda [Python](https://www.python.org/downloads/), 3,8 3,5 ' nin 64 bitlik bir sürümünü indirip bilgisayarınıza yükleyin.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **Dosya** > **tercihleri** > **uzantıları** ' nı seçin. **Python**için arama yapın.

   ![Python uzantısını yükler](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Projenin depolandığı bir klasör oluşturun. Bir örnek, Windows Gezgini 'ni kullanmaktır.
1. Visual Studio Code, **Dosya** simgesini seçin. Ardından oluşturduğunuz klasörü açın.

   ![Bir klasörü açın](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni dosya simgesini seçerek yeni bir Python `speechsdk.py`kaynak dosyası oluşturun.

   ![Dosya oluşturma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. [Python kodunu](#sample-code) kopyalayın, yapıştırın ve yeni oluşturulan dosyaya kaydedin.
1. Konuşma hizmeti abonelik bilgilerinizi ekleyin.
1. Seçilirse, pencerenin alt kısmındaki durum çubuğunun sol tarafında bir Python yorumlayıcı görüntülenir.
   Aksi takdirde, kullanılabilir Python yorumlayıcılarını bir listesini alın. Komut paletini açın (Ctrl + Shift + P) ve **Python: yorumlayıcı Seç**' i girin. Uygun olanı seçin.
1. Konuşma SDK 'Sı Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcı için henüz yüklenmemişse bunu yapın.
   Konuşma SDK 'Sı paketini yüklemek için bir Terminal açın. Komut paletini tekrar açın (Ctrl + Shift + P) ve **Terminal: yeni tümleşik Terminal oluştur**' a girin.
   Açılan terminalde, sisteminiz için komutu `python -m pip install azure-cognitiveservices-speech` veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde herhangi bir yere sağ tıklayın. **Terminalde Python dosyasını çalıştır '** ı seçin.
   İstendiğinde metin yazın. Birleştirilen ses kısa bir süre sonra oynatılır.

   ![Örnek çalıştırma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run-tts.png)

Bu talimatları takip eden sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticisine](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
