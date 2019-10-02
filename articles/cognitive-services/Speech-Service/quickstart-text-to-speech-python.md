---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, Python-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Python 'da konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/14/2019
ms.author: yulili
ms.openlocfilehash: fed0dea1a562f5ddea05516125a38c690069f446
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803967"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-python"></a>Hızlı başlangıç: Python için konuşma SDK 'Sı ile konuşmayı Sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-python.md)için de kullanılabilir.

Bu makalede, Python için konuşma SDK 'Sı aracılığıyla konuşma hizmetlerinin nasıl kullanılacağı gösterilmektedir. Metinden konuşmayı nasıl sentezleştirme ve varsayılan ses çıkışıyla nasıl oynayacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma Hizmetleri için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* [Python 3,5 veya üzeri](https://www.python.org/downloads/).
* Python konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
    * Windows 10: x64 ve x86.
    * Mac: macOS X sürüm 10,12 veya üzeri.
    * Linux: Ubuntu 16,04, Ubuntu 18,04, x64 üzerinde debir 9.
* Linux 'ta, gerekli paketleri yüklemek için şu komutları çalıştırın:

  * Ubuntu 'da:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

  * On yıl 9 ' da:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.2 libasound2
    ```

* Windows 'ta, platformunuz için [Visual Studio C++ 2019 için Microsoft Visual yeniden dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gereklidir.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bu öğreticinin SDK sürümü 1.7.0 ' den önceki sürümüyle çalışmadığını unutmayın.

Bu komut, konuşma SDK 'Sı için [Pypı](https://pypi.org/) 'den Python paketini yüklüyor:

```sh
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](./releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, bu `pip install --upgrade azure-cognitiveservices-speech` komutuyla güncelleştirebilirsiniz.
@No__t-0 değişkenini inceleyerek Şu anda hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](./support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK 'sını kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Bu hızlı başlangıçtaki [örnek kodu](#sample-code) `quickstart.py` kaynak dosyasına KOPYALAYABILIR ve IDE 'niz içinde veya konsolunda çalıştırabilirsiniz:

```sh
python quickstart.py
```

Ya da bu hızlı başlangıç öğreticisini [konuşma SDK 'sı örnek deposundan](https://aka.ms/csspeech/samples) [Jupyter](https://jupyter.org) Not defteri olarak indirebilir ve Not defteri olarak çalıştırabilirsiniz.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Konuşma SDK 'sını Visual Studio Code ile yükleyip kullanma

1. Bilgisayarınıza [Python](https://www.python.org/downloads/), 3,5 veya üzeri bir 64 bit sürümü indirin ve yükleyin.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **dosya** > **tercihleri** > **uzantıları** ' nı seçin. **Python**için arama yapın.

   ![Python uzantısını yükler](media/sdk/qs-python-vscode-python-extension.png)

1. Projenin depolandığı bir klasör oluşturun. Bir örnek, Windows Gezgini 'ni kullanmaktır.
1. Visual Studio Code, **Dosya** simgesini seçin. Ardından oluşturduğunuz klasörü açın.

   ![Bir klasörü açın](media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni dosya simgesini seçerek yeni bir Python kaynak dosyası @no__t (0) oluşturun.

   ![Dosya oluşturma](media/sdk/qs-python-vscode-python-newfile.png)

1. [Python kodunu](#sample-code) kopyalayın, yapıştırın ve yeni oluşturulan dosyaya kaydedin.
1. Konuşma Hizmetleri abonelik bilgilerinizi ekleyin.
1. Seçilirse, pencerenin alt kısmındaki durum çubuğunun sol tarafında bir Python yorumlayıcı görüntülenir.
   Aksi takdirde, kullanılabilir Python yorumlayıcılarını bir listesini alın. Komut paletini açın (Ctrl + Shift + P) ve **Python: yorumlayıcı Seç**' i girin. Uygun olanı seçin.
1. Konuşma SDK 'Sı Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcı için henüz yüklenmemişse bunu yapın.
   Konuşma SDK 'Sı paketini yüklemek için bir Terminal açın. Komut paletini tekrar açın (Ctrl + Shift + P) ve **Terminal: yeni tümleşik Terminal oluştur**' a girin.
   Açılan terminalde, `python -m pip install azure-cognitiveservices-speech` komutunu veya sisteminizin uygun komutunu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde herhangi bir yere sağ tıklayın. **Terminalde Python dosyasını çalıştır '** ı seçin.
   İstendiğinde metin yazın. Birleştirilen ses kısa bir süre sonra oynatılır.

   ![Örnek çalıştırma](media/sdk/qs-python-vscode-python-run-tts.png)

Bu talimatları takip eden sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticisine](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Python örneklerini keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
