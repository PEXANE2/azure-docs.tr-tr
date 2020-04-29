---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: e47c8bc4dc814f1d4c5cb115a2da911544dd55f8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399904"
---
:::row:::
    :::column span="3":::
        Konuşma SDK 'Sı, Linux ile kullanıldığında aşağıdaki hedef mimarilerde yalnızca **Ubuntu 16.04/18.04**, **delk 9**, **Red Hat Enterprise Linux (RHEL) 7/8**ve **CentOS 7/8** ' yi destekler:
        - x64
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Linux" src="https://docs.microsoft.com/media/logos/logo_linux-color.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> Linux ARM64 hedefleme ve C# kullanarak .NET Core 3. x (DotNet-SDK-3. x paketi) gereklidir. ARM32 veya ARM64 'yi hedefliyorsanız Python desteklenmez.

> [!NOTE]
> Ubuntu 16,04, Ubuntu 18,04 ve de, 9 ' un x86 mimarileri yalnızca konuşma SDK 'Sı ile C++ geliştirmeyi destekler.

### <a name="system-requirements"></a>Sistem gereksinimleri

Yerel bir uygulama için konuşma SDK 'Sı kullanır `libMicrosoft.CognitiveServices.Speech.core.so`. Hedef mimarinin (x86, x64) uygulamayla eşleştiğinden emin olun. Linux sürümüne bağlı olarak ek bağımlılıklar de gerekebilir.

- GNU C kitaplığının paylaşılan kitaplıkları (POSIX Iş parçacıkları programlama kitaplığı dahil `libpthreads`)
- OpenSSL kitaplığı (`libssl.so.1.0.0` veya `libssl.so.1.0.2`)
- ALSA uygulamaları için paylaşılan kitaplık (`libasound.so.2`)

# <a name="ubuntu-16041804"></a>[Ubuntu 16.04/18.04](#tab/ubuntu)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

# <a name="debian-9"></a>[Debian 9](#tab/debian)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.2 libasound2
```

# <a name="rhel-78-and-centos-78"></a>[RHEL 7/8 ve CentOS 7/8](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl
```

> [!IMPORTANT]
> [Konuşma SDK 'sı IÇIN RHEL/CentOS 7 ' yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md)yönergelerini izleyin.

> [!TIP]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](../how-to-configure-openssl-linux.md)yönergelerini izleyin.

---

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
