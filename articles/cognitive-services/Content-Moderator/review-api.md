---
title: İncelemeler, İş Akışları ve İş İlanları - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Bu makalede, Gözden Geçirme aracının temel kavramları hakkında bilgi edineceksiniz; değerlendirmeleri, iş akışları ve işler.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: pafarley
ms.openlocfilehash: 1aba86efb9ea76fbf060e80b47f9f2f6cdf8ee71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221155"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>İçerik denetleme incelemeleri, iş akışları ve işler

İçerik Moderatör gerçek dünya senaryoları için en iyi ılımlılık süreci oluşturmak için insan-in-the-loop yetenekleri ile makine destekli ılımlılık birleştirir. Bunu bulut tabanlı Gözden [Geçirme aracı](https://contentmoderator.cognitive.microsoft.com)aracılığıyla yapar. Bu kılavuzda, Gözden Geçirme aracının temel kavramları hakkında bilgi edineceksiniz: incelemeler, iş akışları ve işler.

## <a name="reviews"></a>İncelemeler

İncelemede, içerik Gözden Geçirme aracına yüklenir ve **Gözden Geçirme** sekmesinin altında görüntülenir. Buradan, kullanıcılar uygulanan etiketleri değiştirebilir ve uygun şekilde kendi özel etiketlerini uygulayabilirler. Bir kullanıcı bir inceleme gönderdiğinde, sonuçlar belirli bir geri arama bitiş noktasına gönderilir ve içerik siteden kaldırılır.

![İnceleme aracı web sitesini bir tarayıcıda, Gözden Geçir sekmesinde açık](./Review-Tool-user-Guide/images/image-workflow-review.png)

İnceleme oluşturmaya başlamak için [Gözden Geçirme aracı kılavuzuna](./review-tool-user-guide/review-moderated-images.md) bakın veya bunu programlı bir şekilde nasıl yapacağınızı öğrenmek için [REST API kılavuzuna](./try-review-api-review.md) bakın.

## <a name="workflows"></a>İş akışları

İş akışı, içerik için bulut tabanlı özelleştirilmiş bir filtredir. İş akışları, içeriği farklı şekillerde filtrelemek ve ardından uygun eylemi yapmak için çeşitli hizmetlere bağlanabilir. İçerik Moderatörü bağlayıcısı ile iş akışı otomatik olarak ılımlılık etiketleri uygulayabilir ve gönderilen içerikle incelemeler oluşturabilir.

### <a name="view-workflows"></a>İş akışlarını görüntüleme

Varolan iş akışlarınızı görüntülemek için [Gözden Geçir aracına](https://contentmoderator.cognitive.microsoft.com/) gidin ve **Ayarlar** > **İş Akışları'nı**seçin.

![Varsayılan iş akışı](images/default-workflow-listed.PNG)

İş akışları tamamen JSON dizeleri olarak tanımlanabilir, bu da onları programlı bir şekilde erişilebilir kılar. İş akışınız için **Edit** seçeneğini ve ardından **JSON** sekmesini seçerseniz, aşağıdaki gibi bir JSON ifadesi görürsünüz:

```json
{
    "Type": "Logic",
    "If": {
        "ConnectorName": "moderator",
        "OutputName": "isAdult",
        "Operator": "eq",
        "Value": "true",
        "Type": "Condition"
        },
    "Then": {
    "Perform": [
    {
        "Name": "createreview",
        "CallbackEndpoint": null,
        "Tags": []
    }
    ],
    "Type": "Actions"
    }
}
```

İş akışlarını oluşturmaya ve kullanmaya başlamak için [Gözden Geçirme aracı kılavuzuna](./review-tool-user-guide/workflows.md) bakın veya bunu programlı olarak nasıl yapacağınızı öğrenmek için [REST API kılavuzuna](./try-review-api-workflow.md) bakın.

## <a name="jobs"></a>İşler

Bir ılımlılık iş içerik ılımlılık, iş akışları ve değerlendirmeleri işlevselliği için sarıcı bir tür olarak hizmet vermektedir. İş, İçerik Moderatörü görüntü moderasyon API'sini veya metin moderasyon API'sini kullanarak içeriğinizi tarar ve ardından belirlenen iş akışıyla karşılaştırır. İş akışı sonuçlarına bağlı olarak, [Gözden Geçirme aracındaki](./review-tool-user-guide/human-in-the-loop.md)içerik için bir inceleme oluşturabilir veya oluşturmayabilir. İncelemeler ve iş akışları kendi API'leri ile oluşturulabilir ve yapılandırılabilir, ancak iş API tüm işlemin ayrıntılı bir rapor elde etmenizi sağlar (belirli bir geri arama bitiş noktasına gönderilebilir).

İş kullanmaya başlamak için [REST API kılavuzuna](./try-review-api-job.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [İş API konsoluna](try-review-api-job.md)test sürüşü yapın ve REST API kod örneklerini kullanın. Visual Studio ve C#'a aşinaysanız, [Jobs .NET quickstart'ına](moderation-jobs-quickstart-dotnet.md)da göz atın. 
* İncelemeler için Gözden [Geçirme API konsolu](try-review-api-review.md)ile başlayın ve REST API kod örneklerini kullanın. Daha sonra [.NET quickstart'ın](dotnet-sdk-quickstart.md)yorumlar bölümüne bakın.
* Video incelemeleri için [Video incelemesini hızlı başlat'ı](video-reviews-quickstart-dotnet.md)kullanın ve [video incelemesine transkript eklemeyi](video-transcript-reviews-quickstart-dotnet.md)öğrenin.
