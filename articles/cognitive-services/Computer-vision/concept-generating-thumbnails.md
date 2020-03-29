---
title: Akıllı kırpılmış küçük resimler - BilgisayarLı Vizyon
titleSuffix: Azure Cognitive Services
description: Bilgisayarlı Görme API'sini kullanarak görüntüler için küçük resimler oluşturmayla ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4874910f37b49990a659b48af0cf27921c3fcd5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68945227"
---
# <a name="generating-smart-cropped-thumbnails-with-computer-vision"></a>Computer Vision ile akıllı kırpılmış küçük resimler oluşturma

Küçük resim, görüntünün azaltılmış boyutlu bir temsilidir. Küçük resimler, görüntüleri ve diğer verileri daha ekonomik ve düzen dostu bir şekilde temsil etmek için kullanılır. Computer Vision API, belirli bir görüntü için sezgisel küçük resimler oluşturmak için görüntüyü yeniden boyutlandırmayla birlikte akıllı kırpma kullanır.

Computer Vision küçük resim oluşturma algoritması aşağıdaki gibi çalışır:

1. Dikkat dağıtıcı öğeleri görüntüden çıkarın ve ana nesnenin(ler) göründüğü görüntünün ilgi _alanını_&mdash;tanımlayın.
1. Görüntüyü tanımlanan _ilgi alanına_göre kırpma.
1. En boy oranını hedef küçük resim boyutlarına uyacak şekilde değiştirin.

## <a name="area-of-interest"></a>İlgi alanı

Bir görüntü yüklediğinizde, Computer Vision API ilgi *alanını*belirlemek için görüntü analiz eder. Daha sonra görüntü kırpma nasıl belirlemek için bu bölgeyi kullanabilirsiniz. Kırpma işlemi, ancak, her zaman bir belirtilmişse istenen en boy oranı eşleşir.

Ayrıca, bu aynı *ilgi alanının* ham sınırlayıcı kutu koordinatlarını, bunun yerine **alanOfInterest** API'yi arayarak alabilirsiniz. Daha sonra bu bilgileri, orijinal görüntüyü istediğiniz gibi değiştirmek için kullanabilirsiniz.

## <a name="examples"></a>Örnekler

Oluşturulan küçük resim, aşağıdaki resimde gösterildiği gibi yükseklik, genişlik ve akıllı kırpma için belirttiğiniz şeye bağlı olarak büyük ölçüde değişebilir.

![Çeşitli kırpma yapılandırmalarının yanında bir dağ görüntüsü](./Images/thumbnail-demo.png)

Aşağıdaki tablo, örnek görüntüler için Computer Vision tarafından oluşturulan tipik küçük resimleri göstermektedir. Küçük resimler, akıllı kırpma özelliği etkinleştirilmiş, 50 piksellik belirli bir hedef yükseklik ve genişlik için oluşturulmuştur.

| Görüntü | Küçük |
|-------|-----------|
|![Gün batımında Açık Dağ, bir kişinin silueti ile](./Images/mountain_vista.png) | ![Gün batımında Açık Dağ Küçük Resim, bir kişinin silueti ile](./Images/mountain_vista_thumbnail.png) |
|![Yeşil arka planlı beyaz bir çiçek](./Images/flower.png) | ![Vizyon Analiz Çiçek küçük resim](./Images/flower_thumbnail.png) |
|![Bir apartmanın çatısında bir kadın](./Images/woman_roof.png) | ![bir apartmanın çatısında bir kadının küçük resmi](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Sonraki adımlar

Görüntüleri [etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilere ayırma](concept-categorizing-images.md)hakkında bilgi edinin.
