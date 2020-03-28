---
title: 'Quickstart: Konuşma SDK Java (Windows, Linux, macOS) platform kurulumu için - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK ile Java 'yı (Windows, Linux, macOS) kullanmak için platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/11/2019
ms.author: erhopf
ms.openlocfilehash: 7147f0d13c88c1d2e17e81a360a5aee55ee760ed
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925608"
---
Bu kılavuz, 64 bit Java 8 JRE için [Konuşma SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yüklenir gösterir. Paket adının kendi başınızın başlamasını istiyorsanız, Java SDK Maven merkezi deposunda kullanılamaz. İster Gradle ister `pom.xml` bağımlılık dosyası kullanıyor olun, işaret eden `https://csspeechstorage.blob.core.windows.net/maven/` özel bir depo eklemeniz gerekir (paket adı için aşağıya bakın).

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](~/articles/cognitive-services/speech-service/speech-devices-sdk.md).

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Java Konuşma SDK paketi bu işletim sistemleri için kullanılabilir:
  - Windows: yalnızca 64 bit
  - Mac: macOS X sürüm 10.13 veya sonrası
  - Linux: 64-bit sadece Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8

## <a name="prerequisites"></a>Ön koşullar

- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- [Eclipse Java IDE](https://www.eclipse.org/downloads/) (Java zaten yüklü gerektirir)
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
        sudo yum install alsa-lib java-1.8.0-openjdk-devel openssl
        ```

> [!NOTE]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

- Windows'da, platformunuz [için Visual Studio 2019 için Microsoft Visual C++ Yeniden Dağıtılabilir'e](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) ihtiyacınız vardır. Bunu ilk kez yüklemenin, bu kılavuzla devam etmeden önce Windows'u yeniden başlatmanızı gerektirebileceğini unutmayın.

## <a name="create-an-eclipse-project-and-install-the-speech-sdk"></a>Bir Eclipse projesi oluşturun ve Konuşma SDK'yı kurun

[!INCLUDE [](~/includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
