---
title: "Hızlı başlangıç: konuşma C++ SDK 'sı (MacOS) Platform Kurulumu-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK 'Sı ile macOS C++ 'ta platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: d0f9bc44145f75d7c3b503fffe43042c650025f1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468140"
---
Bu kılavuzda, macOS 10,13 ve üzeri sürümlerde için C++ [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem gereksinimleri

macOS 10,13 ve üzeri

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK çerçevesini içeren `.zip` Arşivi indirin ve ayıklayın:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizin listesi, üçüncü taraf bildirim ve lisans dosyalarının yanı sıra `MicrosoftCognitiveServicesSpeech.framework` bir dizin içermelidir.

Şimdi aşağıdaki [adımlara](#next-steps) geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
