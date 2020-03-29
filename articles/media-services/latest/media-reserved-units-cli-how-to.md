---
title: Ortam Ayrılmış Birimleri ölçeklendirmek için CLI'yi kullanın - Azure | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri ile medya işlemeyi ölçeklendirmek için CLI'nin nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 9f0a7425fc09d391828a748832f662f02c6022cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970775"
---
# <a name="scaling-media-processing"></a>Medya işlemeyi ölçekleme

Azure Media Services ile sunulan Medya Ayrılmış Birimlerini (MRU) yöneterek hesabınızdaki medya işleme kapasitesini ölçeklendirebilirsiniz. MR'lar medya işleme görevlerinizin işlenme hızını belirler. Aşağıdaki ayrılmış birim türleri arasından seçim yapabilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. 

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızı ayrılmış birimlerle birlikte sağlamanızı da belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, hesabınızda beş ayrılmış birim varsa, işlenecek görevler olduğu sürece beş ortam görevi aynı anda çalışır. Kalan görevler sırada bekler ve çalışan bir görev bittiğinde sırayla işlenmek için alınır. Bir hesapta ayrılmış herhangi bir birim yoksa, görevler sırayla alınır. Bu durumda, bir görevin tamamlanması ile bir sonraki başlangıç arasındaki bekleme süresi, sistemdeki kaynakların kullanılabilirliğine bağlıdır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türleri arasında seçim

Aşağıdaki tablo, farklı kodlama hızları arasında seçim yaparken bir karar vermenize yardımcı olur. Ayrıca, kendi testlerinizi gerçekleştirmek için [indirebileceğiniz bir videoda](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) birkaç kıyaslama durumda sağlar:

|RU tipi|Senaryo|7 dk [1080p video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) için örnek sonuçlar|
|---|---|---|
| **S1**|Tek bithızı kodlama. <br/>SD veya çözünürlüklerin altında dosyalar, zamanduyarlı değil, düşük maliyetli.|"H264 Tek Bitrate SD 16x9" kullanarak tek bit hızı SD çözünürlüğü MP4 dosyası kodlama yaklaşık 7 dakika sürer.|
| **S2**|Tek bithızı ve birden çok bitrate kodlama.<br/>Hem SD hem de HD kodlama için normal kullanım.|"H264 Single Bitrate 720p" ön ayarlı kodlama yaklaşık 6 dakika sürer.<br/><br/>"H264 Çoklu Bitrate 720p" ön ayarlı kodlama yaklaşık 12 dakika sürer.|
| **S3**|Tek bithızı ve birden çok bitrate kodlama.<br/>Full HD ve 4K çözünürlüklü videolar. Zamana duyarlı, daha hızlı geri dönüş kodlaması.|"H264 Single Bitrate 1080p" ön ayarlı kodlama yaklaşık 3 dakika sürer.<br/><br/>"H264 Çoklu Bitrate 1080p" ön ayarlı kodlama yaklaşık 8 dakika sürer.|

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Media Services v3 veya Video Indexer tarafından tetiklenen Ses Analizi ve Video Analizi işleri için, S3 birim türü şiddetle tavsiye edilir.
* Paylaşılan havuzu kullanıyorsanız, yani ayrılmış birimler olmadan, kodlama görevleriniz S1 RUs ile aynı performansa sahiptir. Ancak, Görevlerinizin sıraya girerek harcayabileceği zamana bağlı bir üst sınır yoktur ve herhangi bir zamanda en fazla bir Görev çalışıyor olacaktır.

Makalenin geri kalanı, Media [Services v3 CLI'nin](https://aka.ms/ams-v3-cli-ref) MR'ları ölçeklendirmek için nasıl kullanılacağını gösterir.

> [!NOTE]
> Media Services v3 veya Video Indexer ile tetiklenen Ses Analizi ve Video Analizi İşleri için hesabınıza 10 S3 MRU sağlamanız önerilir. 10'dan fazla S3 MRUs'a ihtiyacınız varsa, [Azure portalını](https://portal.azure.com/)kullanarak bir destek bileti açın.

## <a name="prerequisites"></a>Ön koşullar 

[Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>CLI ile Ölçek Medya Ayrılmış Birimler

`mru` komutunu çalıştırın.

Aşağıdaki [az ams hesabı mru komutu,](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) **sayım** ve **tür** parametrelerini kullanarak Ortam Ayrılmış Birimleri "amsaccount" hesabına ayarlar.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturalandırma

Medya Ayrılmış Birimlerin hesabınızda verilen dakika sayısına bağlı olarak ücretlendirilirsiniz. Bu, hesabınızda çalışan herhangi bir İş olup olmadığından bağımsız olarak gerçekleşir. Ayrıntılı bir açıklama için Medya Hizmetleri [fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.   

## <a name="next-step"></a>Sonraki adım

[Videoları analiz etme](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

* [Kotalar ve sınırlamalar](limits-quotas-constraints.md)
* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
