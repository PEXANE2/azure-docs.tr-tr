---
title: 'Quickstart: C# Unity platform kurulumu için Konuşma SDK - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK ile C# Birlik platformunuzu kurmak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: 8d9e05b5d8105cce8355d217cc6922c25bbe1730
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75466635"
---
Bu kılavuz, [Unity](https://unity3d.com/)için [Konuşma SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yüklenir gösterir.

> [!NOTE]
> Unity için Konuşma SDK Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatörü, ARM32 ve ARM64) destekler

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

- [Unity 2018.3 veya daha sonra](https://store.unity.com/) [Unity 2019.1 ile UWP ARM64 desteği ekleyerek](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 sürümü 15.9 veya üzeri sürüm de kabul edilebilir.
- Windows ARM64 desteği [için ARM64 için isteğe bağlı yapı araçlarını ve ARM64 için Windows 10 SDK'yı yükleyin.](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Unity için Konuşma SDK'sını yüklemek için aşağıdaki adımları izleyin:

1. Birlik varlık paketi (.unitypackage) olarak paketlenmiş olan ve zaten Unity ile ilişkilendirilmesi gereken [Unity için Konuşma SDK'sını](https://aka.ms/csspeech/unitypackage)indirin ve açın. Varlık paketi açıldığında, **İçe Aktarma Birliği Paketi** iletişim kutusu görüntülenir. Bu adımın çalışması için boş bir proje oluşturmanız ve açmanız gerekebilir.

   [![Unity Editor'da Birlik Paketi iletişim kutusunu alma](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png#lightbox)

1. Tüm dosyaların seçildiğinden emin olun ve **İçe Aktar'ı**seçin. Birkaç dakika sonra, Unity varlık paketi projenize aktarılır.

Varlık paketlerini Birlik'e alma hakkında daha fazla bilgi için [Birlik belgelerine](https://docs.unity3d.com/Manual/AssetPackages.html)bakın.

Artık aşağıdaki Sonraki [adımlara](#next-steps) geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
