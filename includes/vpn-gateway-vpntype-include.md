---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b3907882df09bfae1d6453fbffbd3e7562554f7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188253"
---
* **İlke Tabanlı:** İlke Tabanlı VPN'ler daha önce klasik dağıtım modelinde statik yönlendirme ağ geçitleri olarak adlandırılırdı. İlke tabanlı VPN'ler paketleri, şirket içi ağınız ile Azure VNet'i arasındaki adres önekleri birleşimleriyle yapılandırılan IPsec ilkelerine dayalı OLARAK IPsec tünellerinde şifreler ve yönlendirir. İlke (veya trafik seçici) çoğunlukla VPN cihazı yapılandırmasında bir erişim listesi olarak tanımlanır. İlke Tabanlı VPN türü için değer *PolicyBased'dir.* Politika Tabanlı VPN kullanırken aşağıdaki sınırlamaları aklınızda bulundurun:
  
  * İlke Tabanlı VPN'ler **yalnızca** Temel ağ geçidi SKU'da kullanılabilir. Bu VPN türü diğer ağ geçidi SUS'ları ile uyumlu değildir.
  * PolicyBased VPN kullanırken yalnızca 1 tüneliniz olabilir.
  * İlke Tabanlı VPN'leri yalnızca S2S bağlantıları için ve yalnızca belirli yapılandırmalar için kullanabilirsiniz. VPN Ağ Geçidi yapılandırmalarının çoğu RouteBased VPN gerektirir.
* **RouteBased**: RouteBased VPN'ler daha önce klasik dağıtım modelinde dinamik yönlendirme ağ geçitleri olarak adlandırılırdı. RouteBased VPN'ler, paketleri ilgili tünel arabirimlerine yönlendirmek için IP yönlendirme veya yönlendirme tablosunda "rotalar" kullanır. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. RouteBased VPN'ler için ilke (veya trafik seçici) herhangi biri için herhangi bir (veya joker kart) olarak yapılandırılır. RouteBased VPN türü için değer *RouteBased'dir.*
