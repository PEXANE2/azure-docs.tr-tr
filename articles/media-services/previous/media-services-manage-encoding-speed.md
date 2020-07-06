---
title: Azure Media Services ile kodlarınızın hızını ve Eşzamanlılığını yönetin | Microsoft Docs
description: Bu makalede, Azure Media Services ile kodlama işlerinizin/görevlerinin hızını ve Eşzamanlılığını nasıl yönetebileceğinizi açıklayan kısa bir genel bakış sunulmaktadır.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61463761"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Kodlamanızın hızını ve eşzamanlılığını yönetme  

Bu makalede, kodlama işlerinizin/görevlerinizin hızını ve Eşzamanlılığını nasıl yönetebileceğinizi açıklayan kısa bir genel bakış sunulmaktadır.

## <a name="overview"></a>Genel Bakış

Media Services, **ayrılmış birim türü** , medya işleme görevlerinizin işlenme hızını belirler. Şu ayrılmış birim türleri arasından seçim yapabilirsiniz: **S1**, **S2**veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. [Kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md) konusunda, farklı kodlama hızları arasında seçim yapma kararı vermenize yardımcı olan bir tablo gösterilir.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızı **ayrılmış birimlerle**sağlamayı belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, hesabınızda ayrılmış beş birim varsa, işlenmek üzere görevler olduğu sürece beş medya görevi eşzamanlı olarak çalışır. Kalan görevler sırada beklecektir ve çalışan bir görev tamamlandığında sırasıyla işleme için alınır. Bir hesabın sağlanan ayrılmış birimi yoksa, görevler sırayla oluşturulur. Bu durumda, bir görev bitmeden ve sonraki bir sıradaki bir sonraki bekleme süresi sistemdeki kaynakların kullanılabilirliğine bağlıdır.

Kodlama birimlerinin nasıl ölçeklenmesi gerektiğini gösteren ayrıntılı bilgiler ve örnekler için [Bu](media-services-scale-media-processing-overview.md) konuya bakın.

## <a name="next-step"></a>Sonraki adım

[Kodlama birimlerini ölçeklendirme](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

