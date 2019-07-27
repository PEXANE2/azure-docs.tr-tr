---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, Python-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Python için konuşma SDK 'sını kullanan bir konuşmaya metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 6cbe583ca59cb98ba233e58dc665c7e18c2a1f7f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559294"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-python"></a>Hızlı Başlangıç: Python için konuşma SDK 'Sı ile konuşmayı tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Python için konuşma SDK 'Sı aracılığıyla konuşma hizmetlerinin nasıl kullanılacağı gösterilmektedir. Mikrofon girişinden konuşmayı nasıl anlayacağınızı gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma Hizmetleri için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* [Python 3,5 veya üzeri](https://www.python.org/downloads/).
* Python konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
    * Windows: x64 ve x86.
    * Mac: macOS X sürüm 10,12 veya üzeri.
    * Linux: Ubuntu 16,04, Ubuntu 18,04, x64 üzerinde de, 9.
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

Bu komut, konuşma SDK 'Sı için [Pypı](https://pypi.org/) 'den Python paketini yüklüyor:

```sh
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](./releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, komutunu komutuyla `pip install --upgrade azure-cognitiveservices-speech`güncelleştirebilirsiniz.
`azure.cognitiveservices.speech.__version__` Değişkeni inceleyerek Şu anda hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](./support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK 'sını kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Bu hızlı başlangıçta [örnek kodu](#sample-code) bir kaynak dosyaya `quickstart.py` kopyalayabilir ve IDE 'niz içinde veya konsolunda çalıştırabilirsiniz:

```sh
python quickstart.py
```

Ya da bu hızlı başlangıç öğreticisini [konuşma SDK 'sı örnek deposundan](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) [Jupyter](https://jupyter.org) Not defteri olarak indirebilir ve Not defteri olarak çalıştırabilirsiniz.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Konuşma SDK 'sını Visual Studio Code ile yükleyip kullanma

1. Bilgisayarınıza [Python](https://www.python.org/downloads/), 3,5 veya üzeri bir 64 bit sürümü indirin ve yükleyin.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **Dosya** > tercihleriuzantıları > ' nı seçin. **Python**için arama yapın.

   ![Python uzantısını yükler](media/sdk/qs-python-vscode-python-extension.png)

1. Projenin depolandığı bir klasör oluşturun. Bir örnek, Windows Gezgini 'ni kullanmaktır.
1. Visual Studio Code, **Dosya** simgesini seçin. Ardından oluşturduğunuz klasörü açın.

   ![Bir klasörü açın](media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni dosya simgesini seçerek yeni bir Python `speechsdk.py`kaynak dosyası oluşturun.

   ![Dosya oluşturma](media/sdk/qs-python-vscode-python-newfile.png)

1. [Python kodunu](#sample-code) kopyalayın, yapıştırın ve yeni oluşturulan dosyaya kaydedin.
1. Konuşma Hizmetleri abonelik bilgilerinizi ekleyin.
1. Seçilirse, pencerenin alt kısmındaki durum çubuğunun sol tarafında bir Python yorumlayıcı görüntülenir.
   Aksi takdirde, kullanılabilir Python yorumlayıcılarını bir listesini alın. Komut paletini açın (Ctrl + Shift + P) ve Python girin **: Yorumlayıcı**' yı seçin. Uygun olanı seçin.
1. Konuşma SDK 'Sı Python paketini Visual Studio Code içinden yükleyebilirsiniz. Seçtiğiniz Python yorumlayıcı için henüz yüklenmemişse bunu yapın.
   Konuşma SDK 'Sı paketini yüklemek için bir Terminal açın. Komut paletini tekrar açın (Ctrl + Shift + P) ve Terminal girin **: Yeni tümleşik Terminal**oluşturun.
   Açılan terminalde, sisteminiz için komutu `python -m pip install azure-cognitiveservices-speech` veya uygun komutu girin.
1. Örnek kodu çalıştırmak için, düzenleyicinin içinde herhangi bir yere sağ tıklayın. **Terminalde Python dosyasını çalıştır '** ı seçin.
   İstendiğinde birkaç sözcükten konuşun. Yeniden yazılı metin kısa bir süre sonra görüntülenir.

   ![Örnek çalıştırma](media/sdk/qs-python-vscode-python-run.png)

Bu talimatları takip eden sorunlarınız varsa, daha kapsamlı [Visual Studio Code Python öğreticisine](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Python örneklerini keşfet](https://aka.ms/csspeech/samples)
