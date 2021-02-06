---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/05/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: eea8cb61ce98b2394abff6995e5cc89f00a7cf46
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628953"
---
1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' nı seçin.
1. **Kullanıcı VPN yapılandırmaları** sayfasında, bir yapılandırma seçin ve ardından **sanal WAN Kullanıcı VPN profilini indir**' i seçin. WAN düzeyi yapılandırmasını indirdiğinizde, yerleşik bir Traffic Manager tabanlı kullanıcı VPN profili alırsınız. Genel profiller veya hub tabanlı bir profil hakkında daha fazla bilgi için bkz. [hub profilleri](../articles/virtual-wan/global-hub-profile.md). Yük devretme senaryoları genel profille basitleştirilmiştir.

   
   Bir hub kullanılamaz durumdaysa, hizmet tarafından sunulan yerleşik trafik yönetimi, Noktadan siteye kullanıcılar için Azure kaynaklarına bağlantıyı (farklı bir hub aracılığıyla) sağlar. Hub 'a giderek, her zaman hub 'a özgü VPN yapılandırmasını indirebilirsiniz. **Kullanıcı VPN (site üzerine gelin)** altında, sanal hub **Kullanıcı VPN** profilini indirin.
1. **Sanal WAN Kullanıcı VPN profilini indir** sayfasında, **kimlik doğrulama türünü** seçin **ve profil oluştur ve indir**' i seçin. Profil paketi oluşturulur ve yapılandırma ayarlarını içeren bir ZIP dosyası indirilir.
