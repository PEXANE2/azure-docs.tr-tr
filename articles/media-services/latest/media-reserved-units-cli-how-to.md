---
title: Ölçek medya ayrılmış birimleri (MRUs) CLı
description: Bu konuda, Azure Media Services ile medya işlemeyi ölçeklendirmek için CLı 'nin nasıl kullanılacağı gösterilmektedir.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/22/2021
ms.author: inhenkel
ms.openlocfilehash: 06c0c6333b84697415ef598d4c5e853d5c006f08
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104870169"
---
# <a name="how-to-scale-media-reserved-units"></a>Medya ayrılmış birimlerini ölçeklendirme

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede, daha hızlı kodlama için medya ayrılmış birimlerinin (MRSs) nasıl ölçeklenmesi gösterilmektedir.

> [!WARNING]
> Bu komut, API veya üzeri 2020-05-01 sürümü ile oluşturulan Media Services hesapları için artık çalışmayacaktır. Bu hesaplar için, sistem, yük temelinde otomatik olarak yukarı ve aşağı ölçeklenebileceği için medya ayrılmış birimleri artık gerekli değildir. Azure portal MRU 'yi yönetme seçeneğini görmüyorsanız, 2020-05-01 API 'SI veya sonrasında oluşturulmuş bir hesap kullanıyorsunuz demektir.

## <a name="prerequisites"></a>Önkoşullar

[Media Services hesabı oluşturun](./create-account-howto.md).

[Medyaya ayrılan birimleri](concept-media-reserved-units.md)anlayın.

## <a name="scale-media-reserved-units-with-cli"></a>CLı ile medya ayrılmış birimlerini ölçeklendirme

`mru` komutunu çalıştırın.

Aşağıdaki [az AMS Account MRU](/cli/azure/ams/account/mru) komutu, **Count** ve **Type** parametrelerini kullanarak "Amsaccount" hesabındaki medya ayrılmış birimlerini ayarlar.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturalandırma

Medya ayrılmış birimlerinin hesabınızda sağlanacağı dakika sayısına göre ücretlendirilirsiniz. Bu, hesabınızda çalışan herhangi bir Iş olup olmadığından bağımsız olarak gerçekleşir. Ayrıntılı bir açıklama için [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.   

## <a name="next-step"></a>Sonraki adım

[Videoları analiz etme](analyze-videos-tutorial-with-api.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Kotalar ve sınırlar](limits-quotas-constraints.md)
