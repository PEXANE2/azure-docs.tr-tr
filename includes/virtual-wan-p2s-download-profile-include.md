---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 317d75dae83e8529bda25897b77824d8cd36e72e
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95557050"
---
1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' nı seçin.
1. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırmasını indir**' i seçin. WAN düzeyi yapılandırmasını indirdiğinizde, yerleşik bir Traffic Manager tabanlı kullanıcı VPN profili alırsınız. Genel profiller veya hub tabanlı bir profil hakkında daha fazla bilgi için bkz. [hub profilleri](../articles/virtual-wan/global-hub-profile.md). Yük devretme senaryoları genel profille basitleştirilmiştir.

   Bir hub kullanılamaz durumdaysa, hizmet tarafından sunulan yerleşik trafik yönetimi, Noktadan siteye kullanıcılar için Azure kaynaklarına bağlantıyı (farklı bir hub aracılığıyla) sağlar. Hub 'a giderek, her zaman hub 'a özgü VPN yapılandırmasını indirebilirsiniz. **Kullanıcı VPN (site üzerine gelin)** altında, sanal hub **Kullanıcı VPN** profilini indirin.
1. Dosya oluşturma işlemi tamamlandıktan sonra, indirmek için bağlantıyı seçin.
1. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.