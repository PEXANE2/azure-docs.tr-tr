---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: gelecaro
ms.openlocfilehash: a1bc980f8334ca815a1805f33f3572cded4ba0ef
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156729"
---
Bu kılavuzda, Linux için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem gereksinimleri

Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 8, CentOS 8)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunlar gerekir:

* GCC
* [İkiliye git (1,13 veya üzeri)](https://golang.org/dl/)

* Desteklenen Linux platformları, belirli kitaplıkların yüklü olmasını gerektirir ( `libssl` Güvenli Yuva Katmanı desteği ve `libasound2` ses desteği için). Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

   * Ubuntu 'da:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * On yıl 9 ' da:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

   * RHEL/CentOS 8 ' de:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

[!INCLUDE [linux-install-sdk](linux-install-sdk.md)]


## <a name="configure-go-environment"></a>Go ortamını Yapılandır

1. Bağlamaların bağlı olduğu için `cgo` , git 'ın SDK bulması için ortam değişkenlerini ayarlamanız gerekir:

   ```sh
   export CGO_CFLAGS="-I$SPEECHSDK_ROOT/include/c_api"
   export CGO_LDFLAGS="-L$SPEECHSDK_ROOT/lib -lMicrosoft.CognitiveServices.Speech.core"
   ```

1. Ayrıca, SDK dahil olmak üzere uygulamaları çalıştırmak için, işletim sistemine, libs 'leri nerede bulacağınızı söylememiz gerekir:

   ```sh
   export LD_LIBRARY_PATH="$SPEECHSDK_ROOT/lib/<arch>:$LD_LIBRARY_PATH"
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list-go.md)]