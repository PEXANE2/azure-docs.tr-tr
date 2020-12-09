---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: gelecaro
ms.openlocfilehash: f674edd15b86f49d60450a53f5df5852b32f95a4
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96906445"
---
Bu kılavuzda, Linux için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem gereksinimleri

'Un [desteklenen Linux dağıtımları ve hedef mimarilerin](~/articles/cognitive-services/speech-service/speech-sdk.md)listesine bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunlar gerekir:

* GCC
* [İkiliye git (1,13 veya üzeri)](https://golang.org/dl/)

* Desteklenen Linux platformları, belirli kitaplıkların yüklü olmasını gerektirir ( `libssl` Güvenli Yuva Katmanı desteği ve `libasound2` ses desteği için). Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

   * Ubuntu/de, üzerinde:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

     Libssl 1.0.0 yoksa, bunun yerine libssl 1.0. x (x 0 ' dan büyük) veya libssl 1.1 sürümünü yüklemelisiniz.

   * RHEL/CentOS üzerinde:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> - RHEL/CentOS 7 ' de, [konuşma SDK 'sı IÇIN RHEL/CentOS 7](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md)' yi yapılandırma yönergelerini izleyin.
> - RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

[!INCLUDE [linux-install-sdk](linux-install-sdk.md)]


## <a name="configure-go-environment"></a>Go ortamını Yapılandır

Konuşma SDK 'sını bulmak üzere go ortamınızı ayarlamak için aşağıdaki adımları gerçekleştirin. Her iki adımda de `<architecture>` CPU 'larınızın işlemci mimarisi ile değiştirin. Bu,, `x86` `x64` veya olur `arm32` `arm64` .

1. Bağlamaların bağlı olduğu için `cgo` , git 'ın SDK bulması için ortam değişkenlerini ayarlamanız gerekir:

   ```sh
   export CGO_CFLAGS="-I$SPEECHSDK_ROOT/include/c_api"
   export CGO_LDFLAGS="-L$SPEECHSDK_ROOT/lib/<architecture> -lMicrosoft.CognitiveServices.Speech.core"
   ```

1. SDK dahil olmak üzere uygulamaları çalıştırmak için, işletim sistemine her bir libs 'nin nerede bulacağınızı söylememiz gerekir:

   ```sh
   export LD_LIBRARY_PATH="$SPEECHSDK_ROOT/lib/<architecture>:$LD_LIBRARY_PATH"
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list-go.md)]
