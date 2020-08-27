---
title: "Hızlı başlangıç: C# Unity platformu için konuşma SDK 'Sı kurulumu-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK 'Sı ile C# Unity için platformunuzu ayarlamak üzere bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.custom: devx-track-csharp
ms.openlocfilehash: 7678ffa1684fb1fe1ea198e9e1836a152cd51b4b
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88926708"
---
Bu kılavuzda [Unity](https://unity3d.com/)için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir.

> [!NOTE]
> Unity için konuşma SDK 'Sı, Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatör, ARM32 ve ARM64) destekler

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

- Unity [2018,3 veya üzeri](https://store.unity.com/) Unity [2019,1, UWP ARM64 için destek ekleme](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 sürüm 15,9 veya üzeri sürümleri de kabul edilebilir.
- Windows ARM64 desteği için, [ARM64 için isteğe bağlı derleme araçlarını ve ARM64 Için Windows 10 SDK](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)'yı yüklemelisiniz.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Unity için konuşma SDK 'sını yüklemek için şu adımları izleyin:

1. Unity varlık paketi (. unitypackage) olarak paketlenmiş ve zaten Unity ile ilişkilendirilmesi gereken [Unity Için konuşma SDK 'sını](https://aka.ms/csspeech/unitypackage)indirip açın. Varlık paketi açıldığında, **Unity paketini Içeri aktar** iletişim kutusu görünür. Bu adımın çalışması için boş bir proje oluşturmanız ve açmanız gerekebilir.

   [![Unity düzenleyicisinde Unity paketi içeri aktarma iletişim kutusu](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png#lightbox)

1. Tüm dosyaların seçili olduğundan emin olun ve **Içeri aktar**' ı seçin. Birkaç dakika sonra Unity varlık paketi projenize aktarılır.

Varlık paketlerini Unity 'ye aktarma hakkında daha fazla bilgi için [Unity belgelerine](https://docs.unity3d.com/Manual/AssetPackages.html)bakın.

Şimdi aşağıdaki [adımlara](#next-steps) geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
