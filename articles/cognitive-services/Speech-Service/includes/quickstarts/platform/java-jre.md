---
title: 'Hızlı başlangıç: Java için konuşma SDK (Windows, Linux, macOS) Platform Kurulumu-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bunu konuşma hizmeti SDK 'Sı ile Java (Windows, Linux, macOS) kullanmaya yönelik platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/11/2019
ms.author: erhopf
ms.openlocfilehash: 95bf939feaad39d261e03938a3c7dd12a76cdd99
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85805910"
---
Bu kılavuzda, 64 bit Java 8 JRE için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir. Yalnızca paket adının kendi kendinize başlatılmasını istiyorsanız, Java SDK 'Sı Maven merkezi deposunda kullanılamaz. Gradle veya bir bağımlılık dosyası kullanıp kullanmayacağınızı `pom.xml` gösteren özel bir depo eklemeniz gerekir `https://csspeechstorage.blob.core.windows.net/maven/` (paket adı için aşağıya bakın).

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](~/articles/cognitive-services/speech-service/speech-devices-sdk.md).

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Java konuşma SDK 'Sı paketi, bu işletim sistemleri için kullanılabilir:
  - Windows: yalnızca 64 bit
  - Mac: macOS X sürüm 10,13 veya üzeri
  - Linux: 64-bit yalnızca Ubuntu 16,04, Ubuntu 18,04, detem 9, RHEL 7/8, CentOS 7/8

## <a name="prerequisites"></a>Önkoşullar

- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- [Tutulma Java IDE](https://www.eclipse.org/downloads/) (Java 'nın zaten yüklü olması gerekir)
- Desteklenen Linux platformları, belirli kitaplıkların yüklü olmasını gerektirir ( `libssl` Güvenli Yuva Katmanı desteği ve `libasound2` ses desteği için). Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

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

  - RHEL/CentOS üzerinde, gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

    ```sh
    sudo yum update
    sudo yum install alsa-lib java-1.8.0-openjdk-devel openssl
    ```

> [!NOTE]
> - RHEL/CentOS 7 ' de, [konuşma SDK 'sı IÇIN RHEL/CentOS 7](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md)' yi yapılandırma yönergelerini izleyin.
> - RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

- Windows 'ta, platformunuz için [Visual Studio 2019 Için yeniden dağıtılabilir Microsoft Visual C++](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerekir. Bunu ilk kez yüklemek, bu kılavuza devam etmeden önce Windows 'u yeniden başlatmanızı gerektirebilir.

## <a name="create-an-eclipse-project-and-install-the-speech-sdk"></a>Bir tutulma projesi oluşturma ve konuşma SDK 'sını yüklemesi

[!INCLUDE [](~/includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
