---
title: "Hızlı başlangıç: Python platformu için konuşma SDK 'Sı kurulumu-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK 'Sı ile Python kullanmaya yönelik platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/09/2019
ms.author: erhopf
ms.openlocfilehash: 458a6940ce214ef1931a2cc9ee95f2cb5ca16779
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78925783"
---
Bu kılavuzda, Python için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir. Yalnızca paket adının kendi kendinize başlamanızı istiyorsanız `pip install azure-cognitiveservices-speech`çalıştırın.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Python konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
  - Windows: x64 ve x86
  - Mac: macOS X sürüm 10,12 veya üzeri
  - Linux: Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 8, CentOS 8, x64 üzerinde

## <a name="prerequisites"></a>Önkoşullar

- Desteklenen Linux platformları, belirli kitaplıkların yüklü olmasını gerektirir (Güvenli Yuva Katmanı desteği için`libssl` ve ses desteği için `libasound2`). Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

  - Ubuntu 'da gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - 9\. yıl 'da, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

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

- Windows 'ta, platformunuz için [Visual Studio C++ 2019 için Microsoft Visual yeniden dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gereklidir. Bunu ilk kez yüklemek, bu kılavuza devam etmeden önce Windows 'u yeniden başlatmanızı gerektirebilir.
- Son olarak, [Python 3,5 ile 3,8](https://www.python.org/downloads/)arasında olmalıdır. Yüklemenizi denetlemek için bir komut istemi açın ve komut `python --version` yazın ve sonucu denetleyin. Doğru yüklenmişse, "Python 3.5.1" veya benzer bir yanıt alırsınız.

## <a name="install-the-speech-sdk-from-pypi"></a>Pypı 'den konuşma SDK 'sını yükler

Kendi ortamınızı veya yapı araçlarınızı kullanıyorsanız, [Pypı](https://pypi.org/)'den konuşma SDK 'sını yüklemek için aşağıdaki komutu çalıştırın. Visual Studio Code kullanıcıları için, kılavuzlu yükleme için sonraki alt bölüme atlayın.

```sh
pip install azure-cognitiveservices-speech
```

MacOS kullanıyorsanız, yukarıdaki `pip` komutunu almak için aşağıdaki komutu çalıştırmanız gerekebilir:

```sh
python3 -m pip install --upgrade pip
```

`azure-cognitiveservices-speech`yüklemek için `pip` başarıyla kullandıysanız, ad alanını Python projelerinize aktararak konuşma SDK 'sını kullanabilirsiniz.

```py
import azure.cognitiveservices.speech as speechsdk
```

## <a name="install-the-speech-sdk-using-visual-studio-code"></a>Visual Studio Code kullanarak konuşma SDK 'sını yükler

1. Platformunuz için desteklenen en son [Python](https://www.python.org/downloads/) sürümünü indirip yükleyin, 3,5 3,8.
   - Windows kullanıcıları, yükleme işlemi sırasında "yolunuza Python Ekle" seçeneğini belirlediğinizden emin olmanızı sağlar.
1. [Visual Studio Code](https://code.visualstudio.com/Download)indirin ve yükleyin.
1. Visual Studio Code açın ve Python uzantısını yükler. Menüden **dosya** > **tercihleri** > **uzantıları** ' nı seçin. **Python** için arama yapın ve **Install**'a tıklayın.

   ![Python uzantısını yükler](~/articles/cognitive-services/speech-service/media/sdk/qs-python-vscode-python-extension.png)

1. Ayrıca, Visual Studio Code içinden, tümleşik komut satırından konuşma SDK 'Sı Python paketini de yüklemelisiniz:
   1. Bir Terminal açın (açılan menülerden, **terminal** > **Yeni terminalden**)
   1. Açılan terminalde, komutu girin `python -m pip install azure-cognitiveservices-speech`

Visual Studio Code yeni kullanıyorsanız, daha kapsamlı [Visual Studio Code belgelerine](https://code.visualstudio.com/docs)bakın. Visual Studio Code ve Python hakkında daha fazla bilgi için bkz. [Visual Studio Code Python öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK 'Sı Python paketine yönelik güncelleştirmeler PyPI aracılığıyla dağıtılır ve [sürüm notlarında](~/articles/cognitive-services/speech-service/releasenotes.md)duyurulmuştur.
Yeni bir sürüm varsa, bunu komut `pip install --upgrade azure-cognitiveservices-speech`ile güncelleştirebilirsiniz.
`azure.cognitiveservices.speech.__version__` değişkenini inceleyerek Şu anda yüklü olan sürümü denetleyin.

Bir sorununuz varsa veya bir özellik eksikse, bkz. [destek ve yardım seçenekleri](~/articles/cognitive-services/speech-service/support.md).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
