---
title: 'Quickstart: Konuşma SDK C++ (Linux) platformu kurulumu - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK ile Linux'ta C++ platformunuzu kurmak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: 4f211e4b90dcc8bffa2fbba6fa4783caf846f50c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78384016"
---
Bu kılavuz, Linux için [Konuşma SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yüklenir gösterir

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem gereksinimleri

Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yapmanız gerekir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

* Desteklenen Linux platformları belirli kitaplıkların (güvenli soket `libasound2` katmanı desteği ve ses desteği`libssl` için) yüklenmesini gerektirir. Bu kitaplıkların doğru sürümlerini yüklemek için gereken komutlar için aşağıdaki dağıtıma bakın.

   * Ubuntu üzerinde:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * Debian 9'da:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

   * RHEL/CentOS 8'de:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

Linux için Konuşma SDK'sı, gerek 64 bit gerekse 32 bit uygulamalar derlemek için kullanılabilir. Gerekli kitaplıklar ve üstbilgi dosyaları tar dosyası https://aka.ms/csspeech/linuxbinaryolarak indirilebilir.

SDK'yı aşağıda gösterildiği gibi indirin ve yükleyin:

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK'sı ikili dosyalarını içeren `.tar.gz` arşivini indirin ve açın:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizinde üçüncü taraf bildirim ve lisans dosyaları, ayrıca üstbilgi (`.h`) dosyalarını içeren bir `include` dizini ve kitaplıkları içeren bir `lib` dizini olmalıdır.

   [!INCLUDE [Linux Binary Archive Content](~/includes/cognitive-services-speech-service-linuxbinary-content.md)]

Artık aşağıdaki Sonraki [adımlara](#next-steps) geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
