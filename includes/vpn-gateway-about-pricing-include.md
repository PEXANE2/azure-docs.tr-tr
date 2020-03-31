---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/08/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8f8d366961049deb3eda193718ccb553aac930e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67666364"
---
İki şey için ödeme yaparsınız: sanal ağ geçidi için saatlik işlem maliyetleri ve sanal ağ geçidinden çıkış veri aktarımı. Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir. Eski ağ geçidi SKU fiyatlandırması için [ExpressRoute fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/expressroute) bakın ve **Sanal Ağ Ağ Ağ Geçitleri** bölümüne gidin.

**Sanal ağ geçidi işlem maliyetleri**<br>Her sanal ağ geçidi saatlik bir işlem maliyetine sahiptir. Fiyat bir sanal ağ geçidi oluştururken belirttiğiniz ağ geçidi SKU’sunu temel alır. Maliyet, ağ geçidinin kendisi içindir ve ağ geçidinden geçen veri aktarımlarına eklenir. Etkin-etkin kurulumun maliyeti, etkin-pasif ile aynıdır.

**Veri aktarım maliyetleri**<br>Veri aktarım maliyetleri, kaynak sanal ağ geçidinden çıkış trafiğine göre hesaplanır.

* Şirket içi VPN cihazınıza trafik gönderiyorsanız İnternet çıkış veri aktarım hızına göre ücretlendirilir.
* Farklı bölgelerdeki sanal ağlar arasında trafik gönderiyorsanız fiyatlandırma bölgeye göre hesaplanır.
* Yalnızca aynı bölgedeki sanal ağlar arasında trafik gönderiyorsanız veri maliyeti yoktur. Aynı bölgedeki sanal ağlar arasında gerçekleşen trafik ücretsizdir.
