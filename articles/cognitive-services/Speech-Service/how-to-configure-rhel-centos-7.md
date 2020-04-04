---
title: RHEL/CentOS 7 nasıl yapılandırılır - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sının kullanılabilması için RHEL/CentOS 7'yi nasıl yapılandırabileceğinizi öğrenin.
services: cognitive-services
author: pankopon
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: pankopon
ms.openlocfilehash: dc09d517d95b5a3f2a88504a14f1451d1de5ffc9
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639168"
---
# <a name="configure-rhelcentos-7-for-speech-sdk"></a>Konuşma SDK için RHEL/CentOS 7'yi yapılandırın

Red Hat Enterprise Linux (RHEL) 8 x64 ve CentOS 8 x64 resmi olarak Speech SDK sürüm 1.10.0 ve sonrası tarafından desteklenir. RHEL/CentOS 7 x64'teki Speech SDK'yı kullanmak da mümkündür, ancak bunun için C++ derleyicisinin (C++ geliştirme için) ve sisteminizdeki paylaşılan C++ çalışma zamanı kitaplığını günceller.

C++ derleyici sürümünü denetlemek için çalıştırın:

```bash
g++ --version
```

Derleyici yüklüyse, çıktı aşağıdaki gibi görünmelidir:

```bash
g++ (GCC) 4.8.5 20150623 (Red Hat 4.8.5-39)
```

Bu ileti, GCC ana sürüm 4 yüklü olduğunu bilmenizi sağlar. Bu sürüm, Konuşma SDK kullandığı C++ 11 standardı için tam destek yok. Bu GCC sürümü ve Konuşma SDK üstbilgileriyle bir C++ programı derlemeye çalışmak derleme hatalarına neden olur.

Paylaşılan C++ çalışma zamanı kitaplığı (libstdc++) sürümünü denetlemek de önemlidir. Konuşma SDK'sının çoğu ana C++ kitaplıkları olarak uygulanır, yani uygulamaları geliştirmek için kullandığınız dilden bağımsız olarak libstdc++'ya bağlıdır.

Sisteminizde libstdc++ konumunu bulmak için çalıştırın:

```bash
ldconfig -p | grep libstdc++
```

Vanilya RHEL/CentOS 7 (x64) çıktısı:

```
libstdc++.so.6 (libc6,x86-64) => /lib64/libstdc++.so.6
```

Bu iletiyi temel alan sürüm tanımlarını bu komutla denetlemek istersiniz:

```bash
strings /lib64/libstdc++.so.6 | egrep "GLIBCXX_|CXXABI_"
```

Çıktı olmalıdır:

```
...
GLIBCXX_3.4.19
...
CXXABI_1.3.7
...
```

Konuşma SDK **CXXABI_1.3.9** ve **GLIBCXX_3.4.21**gerektirir. Bu bilgileri Linux paketinden Konuşma SDK kitaplıklarında çalıştırarak `ldd libMicrosoft.CognitiveServices.Speech.core.so` bulabilirsiniz.

> [!NOTE]
> Sisteme yüklenen GCC sürümünün çalışma zamanı kitaplıklarıyla eşleşen en az **5.4.0**olması önerilir.

## <a name="example"></a>Örnek

Bu, RHEL/CentOS 7 x64'ün geliştirme için nasıl yapılandırılabildiğini (C++, C#, Java, Python) SDK 1.10.0 veya sonraki konuşmalarla nasıl yapılandırılabildiğini gösteren bir örnek komuttur:

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

# Build GCC 5.4.0 and runtimes and install them under /usr/local
sudo yum install -y gmp-devel mpfr-devel libmpc-devel
curl https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/gcc-5.4.0.tar.bz2 -O
tar jxf gcc-5.4.0.tar.bz2
mkdir gcc-5.4.0-build && cd gcc-5.4.0-build
../gcc-5.4.0/configure --enable-languages=c,c++ --disable-bootstrap --disable-multilib --prefix=/usr/local
make -j$(nproc)
sudo make install-strip

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
