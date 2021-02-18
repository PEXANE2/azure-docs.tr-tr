---
title: Ölçek medya ayrılmış birimleri (MRUs) CLı
description: Bu konuda, Azure Media Services ile medya işlemeyi ölçeklendirmek için CLı 'nin nasıl kullanılacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/30/2020
ms.author: inhenkel
ms.openlocfilehash: 98e87cf9d1f46ddb8ee1d433bd0b0ba8806fac89
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101091919"
---
# <a name="how-to-scale-media-reserved-units"></a>Medya ayrılmış birimlerini ölçeklendirme

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede, daha hızlı kodlama için medya ayrılmış birimlerinin (MRSs) nasıl ölçeklenmesi gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[Media Services hesabı oluşturun](./create-account-howto.md).

[Medyaya ayrılan birimleri](concept-media-reserved-units.md)anlayın.

## <a name="scale-media-reserved-units-with-cli"></a>CLı ile medya ayrılmış birimlerini ölçeklendirme

`mru` komutunu çalıştırın.

Aşağıdaki [az AMS Account MRU](/cli/azure/ams/account/mru?view=azure-cli-latest) komutu, **Count** ve **Type** parametrelerini kullanarak "Amsaccount" hesabındaki medya ayrılmış birimlerini ayarlar.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturalandırma

Medya ayrılmış birimlerinin hesabınızda sağlanacağı dakika sayısına göre ücretlendirilirsiniz. Bu, hesabınızda çalışan herhangi bir Iş olup olmadığından bağımsız olarak gerçekleşir. Ayrıntılı bir açıklama için [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.   

## <a name="next-step"></a>Sonraki adım

[Videoları analiz etme](analyze-videos-tutorial-with-api.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Kotalar ve sınırlar](limits-quotas-constraints.md)
