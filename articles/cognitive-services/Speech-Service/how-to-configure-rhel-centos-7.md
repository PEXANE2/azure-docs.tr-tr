---
title: RHEL/CentOS 7-konuşma hizmetini yapılandırma
titleSuffix: Azure Cognitive Services
description: RHEL/CentOS 7 ' yi konuşma SDK 'sının kullanılabilmesi için nasıl yapılandıracağınızı öğrenin.
services: cognitive-services
author: pankopon
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: pankopon
ms.openlocfilehash: ba531164e024f96d3bdd23912f3f6e90275edda4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589746"
---
# <a name="configure-rhelcentos-7-for-speech-sdk"></a>Konuşma SDK 'Sı için RHEL/CentOS 7 yapılandırma

Red Hat Enterprise Linux (RHEL) 8 x64 ve CentOS 8 x64, konuşma SDK sürümü 1.10.0 ve üzeri tarafından resmi olarak desteklenir. Ayrıca, RHEL/CentOS 7 x64 üzerinde konuşma SDK 'sını kullanmak da mümkündür, ancak bu C++ derleyicisinin (C++ geliştirme için) ve sisteminizde paylaşılan C++ çalışma zamanı kitaplığının güncelleştirilmesini gerektirir.

C++ derleyici sürümünü denetlemek için şunu çalıştırın:

```bash
g++ --version
```

Derleyici yüklüyse, çıktının şöyle görünmesi gerekir:

```bash
g++ (GCC) 4.8.5 20150623 (Red Hat 4.8.5-39)
```

Bu ileti, GCC ana sürüm 4 ' ün yüklü olduğunu bilmenizi sağlar. Bu sürüm, konuşma SDK 'sının kullandığı C++ 11 standardı için tam desteğe sahip değildir. Bu GCC sürümüyle bir C++ programı derlemeye çalışılması ve konuşma SDK başlıkları derleme hatalarına neden olur.

Paylaşılan C++ çalışma zamanı kitaplığının (libstdc + +) sürümünü denetlemek de önemlidir. Konuşma SDK 'sının çoğu yerel C++ kitaplıkları olarak uygulanır, yani uygulama geliştirmek için kullandığınız dilden bağımsız olarak lıbdc + + ' ye bağlıdır.

Sisteminizdeki libstdc + + konumunu bulmak için şunu çalıştırın:

```bash
ldconfig -p | grep libstdc++
```

Vanilla RHEL/CentOS 7 (x64) çıkışı:

```bash
libstdc++.so.6 (libc6,x86-64) => /lib64/libstdc++.so.6
```

Bu iletiye bağlı olarak, sürüm tanımlarını bu komutla denetlemek isteyeceksiniz:

```bash
strings /lib64/libstdc++.so.6 | egrep "GLIBCXX_|CXXABI_"
```

Çıktının olması gerekir:

```bash
...
GLIBCXX_3.4.19
...
CXXABI_1.3.7
...
```

Konuşma SDK 'Sı **CXXABI_1.3.9** ve **GLIBCXX_3.4.21**gerektirir. Bu bilgileri `ldd libMicrosoft.CognitiveServices.Speech.core.so` Linux paketindeki konuşma SDK kitaplıkları üzerinde çalıştırarak bulabilirsiniz.

> [!NOTE]
> Sistemde yüklü GCC sürümünün, eşleşen çalışma zamanı kitaplıklarıyla en az **5.4.0**olması önerilir.

## <a name="example"></a>Örnek

Bu, geliştirme için RHEL/CentOS 7 x64 (C++, C#, Java, Python) konuşma SDK 1.10.0 veya üzerini kullanarak nasıl yapılandırılacağını gösteren örnek bir komut kümesidir:

### <a name="1-general-setup"></a>1. genel kurulum

İlk olarak tüm genel bağımlılıkları yükler:

```bash
# Only run ONE of the following two commands
# - for CentOS 7:
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
# - for RHEL 7:
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Install development tools and libraries
sudo yum update -y
sudo yum groupinstall -y "Development tools"
sudo yum install -y alsa-lib dotnet-sdk-2.1 java-1.8.0-openjdk-devel openssl python3
sudo yum install -y gstreamer1 gstreamer1-plugins-base gstreamer1-plugins-good gstreamer1-plugins-bad-free gstreamer1-plugins-ugly-free
```

### <a name="2-cc-compiler-and-runtime-libraries"></a>2. C/C++ derleyicisi ve çalışma zamanı kitaplıkları

Önkoşul paketlerini şu komutla birlikte yükler:

```bash
sudo yum install -y gmp-devel mpfr-devel libmpc-devel
```

> [!NOTE]
> Libmpc-delevel paketi RHEL 7,8 güncelleştirmesinde kullanımdan kaldırılmıştır. Önceki komutun çıktısı bir ileti içeriyorsa
>
> ```bash
> No package libmpc-devel available.
> ```
>
> ardından gerekli dosyaların özgün kaynaklardan yüklenmesi gerekir. Aşağıdaki komutları çalıştırın:
>
> ```bash
> curl https://ftp.gnu.org/gnu/mpc/mpc-1.1.0.tar.gz -O
> tar zxf mpc-1.1.0.tar.gz
> mkdir mpc-1.1.0-build && cd mpc-1.1.0-build
> ../mpc-1.1.0/configure --prefix=/usr/local --libdir=/usr/local/lib64
> make -j$(nproc)
> sudo make install-strip
> ```

Daha sonra derleyici ve çalışma zamanı kitaplıklarını güncelleştirin:

```bash
# Build GCC 5.4.0 and runtimes and install them under /usr/local
curl https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/gcc-5.4.0.tar.bz2 -O
tar jxf gcc-5.4.0.tar.bz2
mkdir gcc-5.4.0-build && cd gcc-5.4.0-build
../gcc-5.4.0/configure --enable-languages=c,c++ --disable-bootstrap --disable-multilib --prefix=/usr/local
make -j$(nproc)
sudo make install-strip
```

Güncelleştirilmiş derleyicisinin ve kitaplıkların çeşitli makinelere dağıtılması gerekiyorsa, bunları `/usr/local` diğer makinelere kopyalamak yeterlidir. Yalnızca çalışma zamanı kitaplıkları gerekliyse, içindeki dosyalar `/usr/local/lib64` yeterli olacaktır.

### <a name="3-environment-settings"></a>3. ortam ayarları

Yapılandırmayı gerçekleştirmek için aşağıdaki komutları çalıştırın:

```bash
# Set SSL cert file location
# (this is required for any development/testing with Speech SDK)
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt

# Add updated C/C++ runtimes to the library path
# (this is required for any development/testing with Speech SDK)
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH

# For C++ development only:
# - add the updated compiler to PATH
#   (note, /usr/local/bin should be already first in PATH on vanilla systems)
# - add Speech SDK libraries from the Linux tar package to LD_LIBRARY_PATH
#   (note, use the actual path to extracted files!)
export PATH=/usr/local/bin:$PATH
hash -r # reset cached paths in the current shell session just in case
export LD_LIBRARY_PATH=/path/to/extracted/SpeechSDK-Linux-1.10.0/lib/x64:$LD_LIBRARY_PATH

# For Python: install the Speech SDK module
python3 -m pip install azure-cognitiveservices-speech --user
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK'sı hakkında](speech-sdk.md)
