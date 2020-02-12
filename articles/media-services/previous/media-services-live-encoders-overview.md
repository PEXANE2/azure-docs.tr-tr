---
title: Çoklu bit hızına sahip akışlar oluşturmak için Azure Media Services kullanırken şirket içi kodlayıcıları yapılandırma | Microsoft Docs
description: Bu konuda, daha fazla işleme için canlı olaylarınızı yakalamak ve AMS kanallarına (canlı kodlama etkin) tek bir bit hızlı canlı akış göndermek için kullanabileceğiniz şirket içi canlı kodlayıcıları listelenmektedir. Bu konu başlığı altında listelenen kodlayıcılara nasıl yapılandırılacağı gösterilmektedir.
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
ms.openlocfilehash: 71a31228602ef161158eaa05c80d50f65de98a50
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133277"
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a>Çoklu bit hızına sahip akışlar oluşturmak için Azure Media Services kullanırken şirket içi kodlayıcıları yapılandırma
Bu konuda, daha fazla işleme için canlı olaylarınızı yakalamak ve AMS kanallarına (canlı kodlama etkin) tek bir bit hızlı canlı akış göndermek için kullanabileceğiniz şirket içi canlı kodlayıcıları listelenmektedir. Konu başlığı altında, listelenen kodlayıcılara nasıl yapılandırılacağını gösteren öğreticilerin bağlantıları da yer almaktadır.

> [!NOTE]
> RTMP üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 1935 ve 1936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.

## <a name="haivision-kb-encoder"></a>Haivision KB Kodlayıcısı
[HAIVISION KB kodlayıcı](https://www.haivision.com/products/kb-series/) Encoder ' ı BIR AMS kanalına tek bit hızlı canlı akış gönderecek şekilde yapılandırma hakkında bilgi için bkz. [Haivision KB Kodlayıcısı yapılandırma](media-services-configure-kb-live-encoder.md).

## <a name="telestream-wirecast"></a>Telestream kablolu dönüştürme
[Telestream kablolu dönüştürme](https://www.telestream.net/wirecast/overview.htm) Kodlayıcısı 'nın bir AMS kanalına tek bir bit hızlı canlı akış göndermek için nasıl yapılandırılacağı hakkında bilgi için bkz. [kablolandırma yapılandırma](media-services-configure-wirecast-live-encoder.md).

## <a name="newtek-tricaster"></a>NewTek karmaşık Aster
Bir AMS kanalına tek bit hızlı canlı akış göndermek için, [karmaşık Aster](https://newtek.com/products/tricaster-40.html) Encoder 'ın nasıl yapılandırılacağı hakkında bilgi için, bkz. [karmaşık Aster yapılandırma](media-services-configure-tricaster-live-encoder.md).

## <a name="elemental-live"></a>Elemental Live
Daha fazla bilgi için bkz. [on canlı](https://www.elementaltechnologies.com/products/elemental-live).

## <a name="media-services-learning-paths"></a>Media Services öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Çoklu bit hızına sahip akışlar oluşturmak için Azure Media Services kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).

