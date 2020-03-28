---
title: 'Hızlı başlangıç: Konuşma SDK C++ (macOS) platformu kurulumu - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK ile macOS'ta C++ platformunuzu kurmak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: d0f9bc44145f75d7c3b503fffe43042c650025f1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75468140"
---
Bu kılavuz, C++ için [Speech SDK'nın](~/articles/cognitive-services/speech-service/speech-sdk.md) macOS 10.13 ve üzeri üzerine nasıl yüklenirken nasıl yüklenirolduğunu gösterir.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Sistem gereksinimleri

macOS 10.13 ve üzeri

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK `.zip` çerçevesini içeren arşivi indirin ve ayıklayın:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizin listesi, üçüncü taraf bildirim ve lisans dosyalarının `MicrosoftCognitiveServicesSpeech.framework` yanı sıra bir dizin içermelidir.

Artık aşağıdaki Sonraki [adımlara](#next-steps) geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
