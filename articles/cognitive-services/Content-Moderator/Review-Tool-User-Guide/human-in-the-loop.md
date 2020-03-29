---
title: İnceleme araç kavramlarını öğrenin - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Birleşik Bir Bilgi Ve Insan gözden geçirme Denetleme çabasını koordine eden bir web sitesi olan İçerik Moderatör İnceleme aracı hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: pafarley
ms.openlocfilehash: a23e6d46ee6e79fd7a5cabf4434c561f7d83b31b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76169497"
---
# <a name="content-moderator-review-tool"></a>İçerik Moderatör İnceleme aracı

Azure İçerik Moderatörinsan değerlendirmeleri ile makine öğrenimi içerik ılımlılığı birleştirmek için hizmetler sağlar ve [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com) web sitesi bu hizmetlere ayrıntılı erişim sağlayan kullanıcı dostu bir ön uç.

![Tarayıcıdaki inceleme aracı panosu](./images/0-dashboard.png)

## <a name="what-it-does"></a>Ne yapar?

[Gözden Geçirme aracı,](https://contentmoderator.cognitive.microsoft.com)makine destekli ılımlılık API'leri ile birlikte kullanıldığında, içerik Denetleme işleminde aşağıdaki görevleri gerçekleştirmenize olanak tanır:

- İçeriği birden çok biçimde (metin, resim ve video) ortalamak için bir araç kümesi kullanın.
- Ilımlılık API sonuçları geldiğinde insan [yorumlarının](../review-api.md#reviews) oluşturulmasını otomatikleştirin.
- İçerik kategorisine veya deneyim düzeyine göre düzenlenen birden çok inceleme takımına içerik incelemeleri atama veya yükseltme.
- İçeriği kod yazmadan sıralamak ve izlemek için varsayılan veya özel mantık filtrelerini[(iş akışları)](../review-api.md#workflows)kullanın.
- İçerik Moderatör API'lerine ek olarak Microsoft PhotoDNA, Text Analytics ve Face hizmetleriyle içeriği işlemek için [bağlayıcıları](./configure.md#connectors) kullanın.
- Herhangi bir API veya iş süreci için iş akışları oluşturmak için kendi bağlayıcınızı oluşturun.
- İçerik denetleme süreçleriniz için önemli performans ölçümleri alın.

## <a name="review-tool-dashboard"></a>Araç panosunun gözden geçirilmesi

**Pano** sekmesinde, araç içinde yapılan içerik incelemeleri için önemli ölçümleri görebilirsiniz. Resim, metin ve video içeriği için toplam, eksiksiz ve bekleyen incelemelerin sayısını görün. İncelemeleri tamamlamış kullanıcıların ve ekiplerin dökümünü ve uygulanan ılımlılık etiketlerini de görebilirsiniz.

![Pano'ya Görüntüle](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Araç kimlik bilgilerini gözden geçirme

[İnceleme aracına](https://contentmoderator.cognitive.microsoft.com)kaydolduğunuzda, hesabınız için bir Azure bölgesi seçmeniz istenir. Bunun nedeni, [Gözden Geçirme aracının](https://contentmoderator.cognitive.microsoft.com) Azure İçerik Moderatörü hizmetleri için ücretsiz bir deneme anahtarı oluşturmasıdır; bir REST aramasından veya istemci SDK'dan hizmetlerden herhangi birini erişmek için bu anahtara ihtiyacınız olacaktır. **Ayarlar** > **Kimlik Bilgilerini**seçerek anahtar ve API uç noktası URL'nizi görüntüleyebilirsiniz.

![İçerik Moderatör Kimlik Bilgileri](images/settings-6-credentials.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz. İnceleme araç kaynaklarına nasıl erişeceğinizi ve ayarları nasıl değiştireceğinizi öğrenmek için [Gözden Geçir aracını yapılandırın.](./configure.md)