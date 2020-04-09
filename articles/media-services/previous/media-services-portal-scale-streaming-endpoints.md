---
title: Azure portalı ile akış uç noktalarını ölçeklendirin | Microsoft Dokümanlar
description: Bu öğretici, Azure portalı ile akış uç noktalarını ölçekleme adımlarında size yol açar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 40820170bae275f090c5f898387698fc562e59ae
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985550"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Azure portal ile akış uç noktalarını ölçeklendirme
## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

**Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. **Premium** akış uç noktası olan müşteriler, varsayılan olarak bir akış birimi (SU) alır. Akış uç noktası, SU’lar eklenerek ölçeklendirilebilir. Her SU, uygulamaya ek bant genişliği kapasitesi sağlar. Akış uç nokta türleri ve CDN yapılandırması hakkında daha fazla bilgi [için, Akış Uç Noktası genel bakış](media-services-streaming-endpoints-overview.md) konusuna bakın.
 
Bu konu, akış uç noktasının nasıl ölçeklendirilenini gösterir.

Fiyatlandırma ayrıntıları hakkında bilgi için [Medya Hizmetleri Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/media-services/)bakın.

## <a name="scale-streaming-endpoints"></a>Akış uç noktalarını ölçeklendir

Akış birimlerinin sayısını değiştirmek için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** **penceresinde, Akış uç noktalarını**seçin.
3. Ölçeklendirmek istediğiniz akış bitiş noktasını tıklatın. 

    > [!NOTE] 
    > Yalnızca **Premium** akış uç noktalarını ölçeklendirebilirsiniz.

4. Akış birimlerinin sayısını belirtmek için kaydırıcıyı taşıyın.

    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

