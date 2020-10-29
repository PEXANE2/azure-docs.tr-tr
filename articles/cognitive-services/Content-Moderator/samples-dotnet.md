---
title: Kod örnekleri - Content Moderator, .NET
titleSuffix: Azure Cognitive Services
description: SDK aracılığıyla .NET uygulamalarınızda Content Moderator Azure bilişsel hizmetler 'i nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: a746a4baf172bcb6728c06f2a31b460139377e46
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912931"
---
# <a name="content-moderator-net-sdk-samples"></a>Content Moderator .NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

## <a name="moderation"></a>Moderasyon

- **Görüntü denetimi** : [Bir görüntüyü yetişkinlere yönelik ve müstehcen içerik, metin ve yüzler için değerlendirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz. [.NET SDK hızlı başlangıç](./client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp).
- **Özel görüntüler** : [Özel görüntü listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz.[.NET SDK hızlı başlangıç](./client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp).

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir** .
>

- **Metin denetleme** : [küfür ve kişisel veriler için ekran metni](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz.[.NET SDK hızlı başlangıç](./client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp).
- **Özel terimler** : [Özel terim listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz. [.NET SDK hızlı başlangıç](./client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp).

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır** .
>

- **Video denetimi** : [Videoda yetişkinlere yönelik ve müstehcen içerik arama ve sonuçları getirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz. [hızlı başlangıç](video-moderation-api.md).

## <a name="review"></a>Gözden geçirme

- **Görüntü işleri** : [Tarama yapan ve gözden geçirmeler oluşturan bir denetim işi başlatma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz. [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri** : [Devrede insan araştıran gözden geçirmeler oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz. [hızlı başlangıç](./client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp).
- **Görüntü incelemeleri** : [Devrede insan araştıran video gözden geçirmeleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Videoyu metne dönüştürme gözden geçirmeleri** : [Devrede insan araştırma için videoyu metne dönüştürme gözden geçirmeleri oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.