---
title: Araç kavramlarını gözden geçirmeyi öğrenin-Content Moderator
titleSuffix: Azure Cognitive Services
description: Birleştirilmiş bir AI ve insan incelemesi denetleme çabasının eşgüdümünü sağlayan bir Web sitesi olan Content Moderator gözden geçirme aracı hakkında bilgi edinin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: sajagtap
ms.openlocfilehash: 7f20b9c824045ac2f8c13df3ed8f776195de611a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564515"
---
# <a name="content-moderator-review-tool"></a>Content Moderator Inceleme aracı

Azure Content Moderator, makine öğrenimi içerik Incelemesini insan incelemeleri ile birleştirmek için hizmetler sağlar ve [Gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) Web sitesi, bu hizmetlere ayrıntılı erişim sağlayan Kullanıcı dostu bir ön uçdır.

![Tarayıcıda inceleme aracı panosu](./images/0-dashboard.png)

## <a name="what-it-does"></a>Ne yapar?

Makine destekli denetleme API 'Leriyle birlikte kullanıldığında [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com), içerik denetleme işleminde aşağıdaki görevleri gerçekleştirmenize olanak tanır:

- Birden çok biçimdeki (metin, resim ve video) içerik için tek bir araç kümesi kullanın.
- API sonuçlarının denetlemesi sırasında insan [incelemelerinin](../review-api.md#reviews) oluşturulmasını otomatikleştirin.
- İçerik kategorilerine veya deneyim düzeyine göre düzenlenmiş şekilde, birden çok gözden geçirme ekiplerine içerik incelemeleri atayın veya ilerletin.
- Herhangi bir kod yazmadan içeriği sıralamak ve izlemek için varsayılan veya özel mantık filtreleri ([iş akışları](../review-api.md#workflows)) kullanın.
- Content Moderator API 'Lerine ek olarak Microsoft PhotoDNA, Metin Analizi ve yüz tanıma API 'Leriyle içerik işlemek için [bağlayıcıları](./configure.md#connectors) kullanın.
- Herhangi bir API veya iş işlemi için iş akışları oluşturmak üzere kendi bağlayıcınızı oluşturun.
- İçerik denetleme işlemlerinizde önemli performans ölçümlerini alın.

## <a name="review-tool-dashboard"></a>Araç panosunu gözden geçir

**Pano** sekmesinde, araç içinde yapılan içerik incelemeleri için temel ölçümleri görebilirsiniz. Görüntü, metin ve video içeriği için toplam, tam ve bekleyen incelemeler sayısını görün. Ayrıca, yapılan gözden geçirmeler ve uygulanan denetleme etiketlerinin yanı sıra, yapılan İncelemeleri içeren Kullanıcı ve takımların dökümünü de görebilirsiniz.

![Panoyu Görüntüle](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Araç kimlik bilgilerini gözden geçirme

[İnceleme aracı](https://contentmoderator.cognitive.microsoft.com)ile kaydolduğunuzda, hesabınız Için bir Azure bölgesi seçmeniz istenir. Bunun nedeni, [Gözden geçirme aracının](https://contentmoderator.cognitive.microsoft.com) Azure Content moderator Hizmetleri için ücretsiz bir deneme anahtarı üretmesine yöneliktir; bir REST çağrısından veya istemci SDK 'sından hizmetlerden herhangi birine erişmek için bu anahtara ihtiyacınız olacak. Anahtar ve API uç nokta URL 'nizi, **Ayarlar** > **kimlik bilgileri**' ni seçerek görüntüleyebilirsiniz.

![Content Moderator kimlik bilgileri](images/settings-6-credentials.png)

## <a name="next-steps"></a>Sonraki adımlar

Inceleme aracı kaynaklarına erişme ve ayarları değiştirme hakkında bilgi edinmek için bkz. [İnceleme aracını yapılandırma](./configure.md) .