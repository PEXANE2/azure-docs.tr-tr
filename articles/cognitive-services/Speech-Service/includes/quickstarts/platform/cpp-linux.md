---
title: 'Hızlı başlangıç: konuşma SDK C++ (Linux) Platform Kurulumu-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Linux üzerinde C++ için platformunuzu konuşma hizmeti SDK 'Sı ile ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: 099c835ae2047538c882414660f699341af60b8b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673263"
---
Bu kılavuzda, Linux için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem Gereksinimleri

Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 7/8, CentOS 7/8)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunlar gerekir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

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

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
