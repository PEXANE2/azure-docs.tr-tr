---
title: Kod örnekleri - Content Moderator, .NET
titleSuffix: Azure Cognitive Services
description: SDK aracılığıyla .NET uygulamalarınızda Content Moderator kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0c7db3c48f1ff7a141fda84caaad84ac8ff7f85d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564422"
---
# <a name="content-moderator-net-sdk-samples"></a>Content Moderator .NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

## <a name="moderation"></a>Denetleme

- **Görüntü denetleme**: [Yetişkin ve kcy içeriği, metin ve yüzler için bir görüntü değerlendirin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [hızlı başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel görüntüler**: [Özel görüntü listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [hızlı başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

- **Metin denetleme**: [Küfür ve kişisel veriler Için ekran metni](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [hızlı başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimler**: [Özel terim listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [hızlı başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

- **Video denetleme**: [Yetişkin ve daha fazla içerik için bir video tarayın ve sonuçları alın](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [hızlı başlangıç](video-moderation-api.md).

## <a name="review"></a>İncele

- **Görüntü işleri**: [İncelemeleri tarayan ve oluşturan bir denetleme Işi başlatın](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü İncelemeleri**: [Kullanıcı döngüsü için Incelemeler oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [hızlı başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Video İncelemeleri**: For [-The-The-The-The-The-The-The-](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs) Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Video dökümü İncelemeleri**: [Kullanıcı döngüsü için video dökümü oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Bkz. [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.
