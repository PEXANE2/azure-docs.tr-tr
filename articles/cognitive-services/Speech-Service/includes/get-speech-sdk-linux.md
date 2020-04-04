---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: dapine
ms.openlocfilehash: a298e78c32464680dab9feccb3cfc84f686ff81b
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656496"
---
:::row:::
    :::column span="3":::
        Konuşma SDK sadece **Ubuntu 16.04/18.04**destekler , **Debian 9**, **Red Hat Enterprise Linux (RHEL) 8**, ve **CentOS 7/8** Linux ile kullanıldığında aşağıdaki hedef mimarileri:
        - x86
        - x64
        - ARM32
        - ARM64
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Linux" src="https://docs.microsoft.com/media/logos/logo_linux-color.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> Linux ARM64'i hedef alırken ve C# kullanırken .NET Core 3.x (dotnet-sdk-3.x paketi) gereklidir. ARM32 veya ARM64'i hedefliyorsanız, Python desteklenmez.

> [!NOTE]
> Ubuntu 16.04, Ubuntu 18.04 ve Debian 9'un x86 mimarisi sadece Konuşma SDK ile C++ gelişimini destekler.

### <a name="system-requirements"></a>Sistem gereksinimleri

Yerel bir uygulama için, Konuşma SDK dayanır. `libMicrosoft.CognitiveServices.Speech.core.so` Hedef mimarinin (x86, x64) uygulamayla eşleştiğinden emin olun. Linux sürümüne bağlı olarak, ek bağımlılıklar gerekebilir.

- GNU C kütüphanesinin ortak kitaplıkları (POSIX Threads `libpthreads`Programming kitaplığı dahil)
- OpenSSL kitaplığı`libssl.so.1.0.0` `libssl.so.1.0.2`( veya )
- ALSA uygulamaları için ortak`libasound.so.2`kitaplık ( )

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

# <a name="rhel-8-and-centos-78"></a>[RHEL 8 ve CentOS 7/8](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl
```

> [!IMPORTANT]
> [Konuşma SDK için RHEL/CentOS 7 yapılandırma hakkında](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md)yönergeleri izleyin.

> [!TIP]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](../how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

---

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
