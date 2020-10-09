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
ms.openlocfilehash: 134f1dc7cb6e53c181b2f518055e5cb758fccf31
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91812784"
---
1. Sanal WAN 'ınız altında, hub ' ı seçin ve **+ yeni merkez**' i seçin.

   :::image type="content" source="media/virtual-wan-p2s-hub/new-hub.jpg" alt-text="Yeni Merkez":::

1. Sanal hub Oluştur sayfasında aşağıdaki alanları girin.

   * **Bölge** -sanal hub 'ı dağıtmak istediğiniz bölgeyi seçin.
   * **Ad** -sanal hub 'ınızı çağırmak istediğiniz adı girin.
   * **Hub özel adres alanı** -hub 'ın CIDR gösteriminde adres aralığı.

   :::image type="content" source="media/virtual-wan-p2s-hub/create-hub.jpg" alt-text="Yeni Merkez":::

1. Noktadan siteye sekmesinde aşağıdaki alanları doldurun:

   * **Ağ geçidi ölçek birimleri** -Kullanıcı VPN ağ geçidinin toplam kapasitesini temsil eder.
   * Önceki adımda oluşturduğunuz **Site Yapılandırması ' nın üzerine gelin** .
   * **Istemci adres havuzu** -uzak kullanıcılar için.
   * **Özel DNS sunucusu IP 'si**.

   :::image type="content" source="media/virtual-wan-p2s-hub/hub-with-p2s.png" alt-text="Yeni Merkez":::

1. **Gözden geçir ve oluştur**’u seçin.
1. **Doğrulama başarılı** sayfasında **Oluştur**' u seçin.