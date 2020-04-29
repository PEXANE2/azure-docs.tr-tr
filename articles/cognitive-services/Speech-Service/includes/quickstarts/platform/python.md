---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 924532318fc6638059bbe9eef0065349463523ae
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400864"
---
Bu kılavuzda, Python için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir. Yalnızca paket adının kendi kendinize başlamanızı istiyorsanız, ' yi çalıştırın `pip install azure-cognitiveservices-speech`.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Python konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
  - Windows: x64 ve x86
  - Mac: macOS X sürüm 10,12 veya üzeri
  - Linux: Ubuntu 16.04/18.04, de, 9, RHEL 7/8, CentOS 7/8 in x64

## <a name="prerequisites"></a>Ön koşullar

- Desteklenen Linux platformları, belirli kitaplıkların yüklü olmasını gerektirir`libssl` (Güvenli Yuva Katmanı desteği ve `libasound2` ses desteği için). Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

  - Ubuntu 'da gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - 9. yıl 'da, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.2 libasound2
        ```

  - RHEL/CentOS 8 ' de, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo yum update
        sudo yum install alsa-lib openssl python3
        ```

> [!NOTE]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

- Windows 'ta, platformunuz için [Visual Studio 2019 Için yeniden dağıtılabilir Microsoft Visual C++](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerekir. Bunu ilk kez yüklemek, bu kılavuza devam etmeden önce Windows 'u yeniden başlatmanızı gerektirebilir.
- Son olarak, [Python 3,5 ile 3,8](https://www.python.org/downloads/)arasında olmalıdır. Yüklemenizi denetlemek için bir komut istemi açın ve komutu `python --version` yazın ve sonucu denetleyin. Doğru yüklenmişse, "Python 3.5.1" veya benzer bir yanıt alırsınız.

## <a name="install-the-speech-sdk-from-pypi"></a>Pypı 'den konuşma SDK 'sını yükler

Kendi ortamınızı veya yapı araçlarınızı kullanıyorsanız, [Pypı](https://pypi.org/)'den konuşma SDK 'sını yüklemek için aşağıdaki komutu çalıştırın. Visual Studio Code kullanıcıları için, kılavuzlu yükleme için sonraki alt bölüme atlayın.

```sh
pip install azure-cognitiveservices-speech
```

MacOS kullanıyorsanız, yukarıdaki `pip` komutu almak için aşağıdaki komutu çalıştırmanız gerekebilir:

```sh
python3 -m pip install --upgrade pip
```

Uygulamasını yüklemek `azure-cognitiveservices-speech`için başarıyla kullandıysanız `pip` , ad alanını Python PROJELERINIZE aktararak konuşma SDK 'sını kullanabilirsiniz.

```py
import azure.cognitiveservices.speech as speechsdk
```

## <a name="install-the-speech-sdk-using-visual-studio-code"></a>Visual Studio Code kullanarak konuşma SDK 'sını yükler

1. Platformunuz için desteklenen en son [Python](https://www.python.org/downloads/) sürümünü indirip yükleyin, 3,5 3,8.
   - Windows kullanıcıları, yükleme işlemi sırasında "yolunuza Python Ekle" seçeneğini belirlediğinizden emin olmanızı sağlar.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **Dosya** > **tercihleri** > **uzantıları** ' nı seçin. **Python** için arama yapın ve **Install**'a tıklayın.

   ![Python uzantısını yükler](~/articles/cognitive-services/speech-service/media/sdk/qs-python-vscode-python-extension.png)

1. Ayrıca, Visual Studio Code içinden, tümleşik komut satırından konuşma SDK 'Sı Python paketini de yüklemelisiniz:
   1. Bir Terminal açın (açılan menülerden, **terminalden** > **Yeni terminalden**)
   1. Açılan terminalde, komutunu girin`python -m pip install azure-cognitiveservices-speech`

Visual Studio Code yeni kullanıyorsanız, daha kapsamlı [Visual Studio Code belgelerine](https://code.visualstudio.com/docs)bakın. Visual Studio Code ve Python hakkında daha fazla bilgi için bkz. [Visual Studio Code Python öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](~/articles/cognitive-services/speech-service/releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, komutunu komutuyla `pip install --upgrade azure-cognitiveservices-speech`güncelleştirebilirsiniz.
`azure.cognitiveservices.speech.__version__` Değişkeni inceleyerek Şu anda hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](~/articles/cognitive-services/speech-service/support.md).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
