---
title: Çok bit hızında akışlar oluşturmak için Azure Medya Hizmetlerini kullanırken şirket içi kodlayıcıları yapılandırın | Microsoft Dokümanlar
description: Bu konu, canlı etkinliklerinizi yakalamak ve daha fazla işleme için AMS kanallarına (canlı kodlama etkin olan) tek bir bit hızıyla canlı akış göndermek için kullanabileceğiniz şirket içi canlı kodlayıcıları listeler. Listelenen kodlayıcıların nasıl yapılandırılabildiğini gösteren öğreticilere konu bağlantıları.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 0ec6f046-0841-4673-9057-883bdbc30d5c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 82fa5968d004bd4460f2c05ec944f6e568508675
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641145"
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a>Çok bit hızında akışlar oluşturmak için Azure Medya Hizmetlerini kullanırken şirket içi kodlayıcıları yapılandırma
Bu konu, canlı etkinliklerinizi yakalamak ve daha fazla işleme için AMS kanallarına (canlı kodlama etkin olan) tek bir bit hızıyla canlı akış göndermek için kullanabileceğiniz şirket içi canlı kodlayıcıları listeler. Konu ayrıca, listelenen kodlayıcıların nasıl yapılandırılabildiğini gösteren öğreticilere de bağlanır.

> [!NOTE]
> RTMP üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 1935 ve 1936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.

## <a name="haivision-kb-encoder"></a>Haivision KB Encoder
Bir AMS Kanalına tek bir bitrate canlı akışı göndermek için [Haivision KB Encoder](https://www.haivision.com/products/kb-series/) kodlayıcısı yapılandırma hakkında bilgi için, [Haivision KB Encoder Yapılandırma](media-services-configure-kb-live-encoder.md)bakın.

## <a name="telestream-wirecast"></a>Telestream Wirecast
[Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) kodlayıcısının ams kanalına tek bir bit hızı canlı akışı göndermek üzere nasıl yapılandırılabildiğini öğrenmek için [bkz.](media-services-configure-wirecast-live-encoder.md)

## <a name="elemental-live"></a>Elemental Live
Daha fazla bilgi için [Elemental Live'a](https://www.elementaltechnologies.com/products/elemental-live)bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Çok bit hızında akışlar oluşturmak için Azure Medya Hizmetleri'ni kullanarak canlı akış.](media-services-manage-live-encoder-enabled-channels.md)

