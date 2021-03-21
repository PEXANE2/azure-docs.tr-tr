---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/04/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f469664c716ecef6b82de2befa40b33f253e229f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99627738"
---
1. Oluşturduğunuz sanal WAN 'ı bulun. Sanal WAN sayfasında, **bağlantı** bölümünde, **hub**' ı seçin.
2. **Hub 'lar** sayfasında **+ yeni hub** ' ı seçerek **sanal hub oluştur** sayfasını açın.

    ![Ekran görüntüsü, temel kavramlar sekmesi seçiliyken sanal hub oluştur bölmesini gösterir.](./media/virtual-wan-tutorial-hub-include/basics.png "Temel Bilgiler")
3. **Sanal hub** sayfası **temel bilgileri** Oluştur sekmesinde aşağıdaki alanları doldurun:

   * **Bölge** (daha önce konum olarak adlandırılır)
   * **Ad**
   * **Hub özel adres alanı** -bir hub oluşturmak için en küçük adres alanı/24 ' dir. /25 ile/32 aralığında herhangi bir şey kullanırsanız, oluşturma sırasında bir hata üretir. Sanal hub 'daki hizmetler için alt ağ adres alanını açıkça planlamanız gerekmez. Azure sanal WAN, yönetilen bir hizmet olduğundan, farklı ağ geçitleri/hizmetler (örneğin, VPN ağ geçitleri, ExpressRoute ağ geçitleri, kullanıcı VPN Noktadan siteye ağ geçitleri, güvenlik duvarı, Yönlendirme ve vb.) için sanal hub 'da uygun alt ağları oluşturur.
4. **İleri ' yi seçin: siteden siteye**.

    ![Ekran görüntüsü, siteye site seçiliyken sanal hub oluştur bölmesini gösterir.](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Siteden siteye")

5. **Siteden siteye** sekmesinde aşağıdaki alanları doldurun:

   * Siteden siteye VPN oluşturmak için **Evet** ' i seçin.
   * AS numarası alanı düzenlenemez.
   * Açılan menüden **ağ geçidi ölçek birimleri** değerini seçin. Ölçek birimi, siteleri bağlamak için sanal hub 'da oluşturulan VPN ağ geçidinin toplam verimini seçmenizi sağlar. 1 ölçek birimi = 500 Mbps ' i seçerseniz, her biri en fazla 500 Mbps hızında olacak şekilde, artıklık için iki örnek oluşturulacaktır. Örneğin, her biri dalda 10 Mbps olan beş dala sahipseniz baş uçta 50 Mbps bir toplama işlemi yapmanız gerekir. Azure VPN ağ geçidinin Birleşik kapasitesini planlama, hub 'a dal sayısını desteklemek için gereken kapasite değerlendirdikten sonra yapılmalıdır.
6. Doğrulamak için **gözden geçir + oluştur** ' u seçin.
7. Hub 'ı oluşturmak için **Oluştur** ' u seçin. 30 dakika **sonra hub 'ı bir sayfada görüntülemek** için **yenileyin** . Kaynağa gitmek için **Kaynağa Git** ' i seçin.
