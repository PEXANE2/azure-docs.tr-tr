---
title: İçeriğinizi oynatmak için mevcut oyuncuları kullanma - Azure | Microsoft Dokümanlar
description: Bu makalede, içeriğinizi oynatmak için kullanabileceğiniz varolan oyuncular listelenmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 07537d3d67e41f7e1179a709ffa19f3d84aa4539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565834"
---
# <a name="playing-your-content-with-existing-players"></a>İçeriğinizi mevcut oynatıcılarda oynatma
Azure Medya Hizmetleri, Düzgün Akış, HTTP Live Streaming ve MPEG-Dash gibi birçok popüler akış biçimini destekler. Bu konu, akışlarınızı test etmek için kullanabileceğiniz mevcut oyunculara işaret eder.

### <a name="the-azure-portal-media-services-content-player"></a>Azure portalı Medya Hizmetleri içerik oynatıcı
**Azure** portalı, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğin videoya tıklayın [(yayınlandığından](media-services-portal-publish.md)emin olun) ve portalın altındaki **Oynat** düğmesine tıklayın.

Bazı dikkate alınması gereken noktalar vardır:

* **MEDYA HİZMETLERİ İÇERİK OYNATICISI** varsayılan akış uç noktasından oynatır. Varsayılan dışı bir akış uç noktasından oynatmak istiyorsanız başka bir oynatıcı kullanın. Örneğin, [Azure Media Player](https://aka.ms/azuremediaplayer).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player

İçeriğinizi aşağıdaki biçimlerden herhangi birinde oynatmak için [Azure Media Player'ı](https://aka.ms/azuremediaplayer) kullanın (açık veya korumalı) aşağıdaki biçimlerden herhangi birinde:

* Kesintisiz Akış
* MPEG DASH
* HLS
* Progresif MP4

### <a name="flash-player"></a>Flash Oyuncu

#### <a name="aes-encrypted-with-token"></a>Token ile AES şifreli

[https://aestoken.azurewebsites.net](https://aestoken.azurewebsites.net)

#### <a name="playready-with-token"></a>Jetonile PlayReady

[https://sltoken.azurewebsites.net](https://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH Oyuncuları

[https://dashplayer.azurewebsites.net](https://dashplayer.azurewebsites.net)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Diğer
HLS URL'lerini test etmek için şunları da kullanabilirsiniz:

* Bir iOS aygıtında **safari** veya
* Windows'da **3ivx HLS Oynatıcı.**

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
