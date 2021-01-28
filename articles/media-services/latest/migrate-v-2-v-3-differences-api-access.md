---
title: Media Services V2 vs v3 API erişimi
description: Bu makalede, Azure Media Services V2 ile v3 arasındaki API erişimi farklılıkları açıklanmaktadır.
services: media-services
documentationcenter: na
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 831bc737fbd54685cdc8358642bc37e7b2df2c42
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98953725"
---
# <a name="api-access-differences-between-azure-media-services-v2-to-v3-api"></a>Azure Media Services V2 ile v3 API arasında API erişimi farkları

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-2.svg)

Bu makalede, Azure Media Services V2 ile v3 arasındaki API erişimi farklılıkları açıklanmaktadır.

## <a name="api-access"></a>API erişimi

Tüm Media Services hesapların v3 API 'sine erişimi olur. Ancak, güncelleştirilmiş kodu mevcut bir v2 hesabına uygulamadan önce yeni bir hesapta geçiş geliştirmeyi önemle öneririz. Bunun nedeni, v3 varlıklarının v2 ile geriye dönük olarak uyumlu değildir. Varlıklar gibi bazı v2 varlıkları, v3 ile ileriye dönük olarak uyumludur.
V2 ve v3 API 'Lerini karışmıyorsanız ve sonra v2 'ye geri dönerek bu önerilmez.

V2 API 'sine erişim, 2024 ' de devre dışı bırakılana kadar kullanılabilir hale gelir.

Geçiş yaparken, hala v2 erişimi olan bir v3 hesabı oluşturabilirsiniz.  Hesabın oluşturulması şu şekilde yapılabilir:

- REST API ve eski sürümü
- Portalda onay kutusu seçiliyor.

> [!div class="mx-imgBorder"]
> [![portalda ](./media/migration-guide/v-3-v-2-access-account-creation-small.png) Hesap oluşturma](./media/migration-guide/v-3-v-2-access-account-creation.png#lightbox)

Tüm .NET, CLı ve diğer SDK 'lar en son 2020-05-01 API 'sini hedefleyecek, bu nedenle eski API sürümlerini bulur veya yapılandırır.

> [!NOTE]
> 2020-05-01 API 'SI ile oluşturulan yeni hesaplar v2 API 'Lerini kullanamaz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
