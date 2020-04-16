---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 924532318fc6638059bbe9eef0065349463523ae
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400864"
---
Bu kılavuz, Python için [Konuşma SDK'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yüklenir olduğunu gösterir. Paket adının kendi başınızın başlamasını istiyorsanız, `pip install azure-cognitiveservices-speech`çalıştırın.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Python Speech SDK paketi bu işletim sistemleri için kullanılabilir:
  - Windows: x64 ve x86
  - Mac: macOS X sürüm 10.12 veya sonrası
  - Linux: Ubuntu 16.04/18.04, Debian 9, RHEL 7/8, CentOS 7/8 x64 üzerinde

## <a name="prerequisites"></a>Ön koşullar

- Desteklenen Linux platformları belirli kitaplıkların (güvenli soket `libasound2` katmanı desteği ve ses desteği`libssl` için) yüklenmesini gerektirir. Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

  - Ubuntu'da, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - Debian 9'da, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.2 libasound2
        ```

  - RHEL/CentOS 8'de, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo yum update
        sudo yum install alsa-lib openssl python3
        ```

> [!NOTE]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

- Windows'da, platformunuz [için Visual Studio 2019 için Microsoft Visual C++ Yeniden Dağıtılabilir'e](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) ihtiyacınız vardır. Bunu ilk kez yüklemenin, bu kılavuzla devam etmeden önce Windows'u yeniden başlatmanızı gerektirebileceğini unutmayın.
- Ve son olarak, [Python 3.5-3.8](https://www.python.org/downloads/)gerekir. Yüklemenizi denetlemek için bir komut istemi `python --version` açın ve komutu yazın ve sonucu kontrol edin. Düzgün yüklenirse, "Python 3.5.1" veya benzeri bir yanıt alırsınız.

## <a name="install-the-speech-sdk-from-pypi"></a>PyPI'den Konuşma SDK'sını yükleyin

Kendi ortamınızı kullanıyorsanız veya araçlar oluşturuyorsanız, [PyPI'den](https://pypi.org/)Konuşma SDK'sını yüklemek için aşağıdaki komutu çalıştırın. Visual Studio Code kullanıcıları için, kılavuzlu yükleme için bir sonraki alt bölüme atlayın.

```sh
pip install azure-cognitiveservices-speech
```

macOS'taysanız, yukarıdaki komutun `pip` çalışması için aşağıdaki komutu çalıştırmanız gerekebilir:

```sh
python3 -m pip install --upgrade pip
```

Yüklemeyi `pip` `azure-cognitiveservices-speech`başarıyla kullandıktan sonra, Python projelerinize ad alanını aktararak Speech SDK'yı kullanabilirsiniz.

```py
import azure.cognitiveservices.speech as speechsdk
```

## <a name="install-the-speech-sdk-using-visual-studio-code"></a>Visual Studio Kodunu kullanarak Konuşma SDK'sını yükleyin

1. [Python'un](https://www.python.org/downloads/) platformunuz için desteklenen en son sürümünü 3,5 ile 3,8 arasında indirin ve kurun.
   - Windows kullanıcıları yükleme işlemi sırasında "PATH'inize Python ekle" seçeneğini seçtiğinizden emin olun.
1. [Visual Studio Code'u](https://code.visualstudio.com/Download)indirin ve kurun.
1. Visual Studio Kodunu açın ve Python uzantısını yükleyin. Menüden **Dosya** > **Tercihleri** > **Uzantıları'nı** seçin. **Python'u** arayın ve **Yükle'yi**tıklatın.

   ![Python uzantısını yükleme](~/articles/cognitive-services/speech-service/media/sdk/qs-python-vscode-python-extension.png)

1. Ayrıca Visual Studio Code içinden, tümleşik komut satırından Konuşma SDK Python paketini yükleyin:
   1. Bir terminal açın (açılan menülerden, **Terminal** > **Yeni Terminal)**
   1. Açılan terminalde, komutu girin`python -m pip install azure-cognitiveservices-speech`

Visual Studio Code'da yeniyseniz, daha kapsamlı [Visual Studio Code Dokümantasyonu'na](https://code.visualstudio.com/docs)bakın. Visual Studio Code ve Python hakkında daha fazla bilgi için [Visual Studio Code Python öğreticisi'ne](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

## <a name="support-and-updates"></a>Destek ve güncellemeler

Konuşma SDK Python paketindeki güncellemeler PyPI üzerinden dağıtılır ve [Sürüm notlarında duyurulur.](~/articles/cognitive-services/speech-service/releasenotes.md)
Yeni bir sürüm varsa, komutu `pip install --upgrade azure-cognitiveservices-speech`ile bunu güncelleyebilirsiniz.
Değişkeni inceleyerek şu anda `azure.cognitiveservices.speech.__version__` hangi sürümün yüklü olduğunu denetleyin.

Bir sorununuz varsa veya bir özelliğiniz yoksa Destek [ve Yardım seçeneklerine](~/articles/cognitive-services/speech-service/support.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
