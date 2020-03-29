---
title: Azure Medya Hizmetleri ile kodlamanızın hızını ve eşzamanlılığını yönetin | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri ile kodlama işlerinizin/görevlerinizin hızını ve eşzamanlılığını nasıl yönetebileceğinize kısa bir genel bakış sunulur.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6bcaadc8dd61899aff860ad246e30170c99ec0f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61463761"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Kodlamanızın hızını ve eşzamanlılığını yönetme  

Bu makalede, kodlama işlerinizin/görevlerinizin hızını ve eşzamanlılıkını nasıl yönetebileceğinize kısa bir genel bakış yer almaktadır.

## <a name="overview"></a>Genel Bakış

Ortam Hizmetlerinde, **Ayrılmış Birim Türü,** ortam işleme görevlerinizin işlenme hızını belirler. Aşağıdaki ayrılmış birim türleri arasından seçim yapabilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. [Ölçekleme kodlama birimleri](media-services-scale-media-processing-overview.md) konu farklı kodlama hızları arasında seçim yaparken karar yardımcı olan bir tablo gösterir.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızı Ayrılmış **Birimler'e**sağlamanızı da belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, hesabınızda beş ayrılmış birim varsa, işlenecek görevler olduğu sürece beş ortam görevi aynı anda çalışır. Kalan görevler sırada bekler ve çalışan bir görev bittiğinde sırayla işlenmek için alınır. Bir hesapta ayrılmış herhangi bir birim yoksa, görevler sırayla alınır. Bu durumda, bir görevin tamamlanması ile bir sonraki başlangıç arasındaki bekleme süresi, sistemdeki kaynakların kullanılabilirliğine bağlıdır.

Kodlama birimlerinin nasıl ölçeklendirilebildiğini gösteren ayrıntılı bilgi ve örnekler için [bu](media-services-scale-media-processing-overview.md) konuya bakın.

## <a name="next-step"></a>Sonraki adım

[Ölçek kodlama birimleri](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

