---
title: Kod örnekleri - Content Moderator, .NET
titleSuffix: Azure Cognitive Services
description: SDK aracılığıyla .NET uygulamalarınızda Azure Bilişsel Hizmetler İçerik Moderatörü'nün nasıl kullanılacağını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 930571c841146e0b12efbf7325915ba2b23a7efa
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73744335"
---
# <a name="content-moderator-net-sdk-samples"></a>İçerik Moderatör .NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

## <a name="moderation"></a>Denetleme

- **Görüntü denetimi**: [Bir görüntüyü yetişkinlere yönelik ve müstehcen içerik, metin ve yüzler için değerlendirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz. [.NET SDK hızlı başlat.](dotnet-sdk-quickstart.md)
- **Özel görüntüler**: [Özel görüntü listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz.[.NET SDK hızlı başlat.](dotnet-sdk-quickstart.md)

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

- **Metin moderasyonu**: [Küfür ve kişisel veriler için ekran metni.](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs) Bkz.[.NET SDK hızlı başlat.](dotnet-sdk-quickstart.md)
- **Özel terimler**: [Özel terim listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz. [.NET SDK hızlı başlat.](dotnet-sdk-quickstart.md)

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

- **Video denetimi**: [Videoda yetişkinlere yönelik ve müstehcen içerik arama ve sonuçları getirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). [Bkz. quickstart](video-moderation-api.md).

## <a name="review"></a>Gözden geçirme

- **Görüntü işleri**: [Tarama yapan ve gözden geçirmeler oluşturan bir denetim işi başlatma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). [Bkz. quickstart](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran gözden geçirmeler oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). [Bkz. quickstart](dotnet-sdk-quickstart.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran video gözden geçirmeleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Videoyu metne dönüştürme gözden geçirmeleri**: [Devrede insan araştırma için videoyu metne dönüştürme gözden geçirmeleri oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.
