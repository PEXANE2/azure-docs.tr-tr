---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e1a9256cc9a015a5d8286de5e5bd7b61ed915a3b
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812796"
---
1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' nı seçin.
1. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırmasını indir**' i seçin. WAN düzeyi yapılandırmasını indirdiğinizde, yerleşik bir Traffic Manager tabanlı kullanıcı VPN profili alırsınız. Genel profiller veya hub tabanlı bir profil hakkında daha fazla bilgi için bkz. [hub profilleri](https://docs.microsoft.com/azure/virtual-wan/global-hub-profile). Yük devretme senaryoları genel profille basitleştirilmiştir.

   Bir hub kullanılamaz durumdaysa, hizmet tarafından sunulan yerleşik trafik yönetimi, Noktadan siteye kullanıcılar için Azure kaynaklarına bağlantıyı (farklı bir hub aracılığıyla) sağlar. Hub 'a giderek, her zaman hub 'a özgü VPN yapılandırmasını indirebilirsiniz. **Kullanıcı VPN (site üzerine gelin)** altında, sanal hub **Kullanıcı VPN** profilini indirin.
1. Dosya oluşturma işlemi tamamlandıktan sonra, indirmek için bağlantıyı seçin.
1. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.
