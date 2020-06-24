---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 105ab0c71d9e7e935842550ecdc4c8d2ff2a2d8c
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977938"
---
1. Oluşturduğunuz sanal WAN 'ı bulun. Sanal WAN sayfasında, **bağlantı** bölümünde, **hub**' ı seçin.
2. Hub 'Lar sayfasında **+ yeni hub** ' ı seçerek **sanal hub oluştur** sayfasını açın.

    ![Temel Bilgiler](./media/virtual-wan-tutorial-hub-include/basics.png "Temel Bilgiler")
3. **Sanal hub** sayfası **temel bilgileri** Oluştur sekmesinde aşağıdaki alanları doldurun:

    **Proje ayrıntıları**

   * Bölge (daha önce konum olarak adlandırılır)
   * Name
   * Hub özel adres alanı. En küçük adres alanı bir hub oluşturmak için/24 ' tir. Bu,/25 ile/32 arasında herhangi bir aralığın oluşturulması sırasında bir hata üretecektir. Microsoft tarafından yönetilen bir hizmet olan Azure sanal WAN, farklı ağ geçitleri/hizmetler (örneğin, VPN ağ geçitleri, ExpressRoute ağ geçitleri, kullanıcı VPN/Noktadan siteye ağ geçitleri, güvenlik duvarı, yönlendirme vb.) için sanal hub 'da uygun alt ağları oluşturur. Microsoft bunu hizmetin bir parçası olarak yaparken, kullanıcının sanal hub 'daki hizmetler için alt ağ adres alanını açıkça planlaması gerekmez.
4. **İleri ' yi seçin: siteden siteye**.

    ![Siteden siteye](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Siteden siteye")

5. **Siteden siteye** sekmesinde aşağıdaki alanları doldurun:

   * Siteden siteye VPN oluşturmak için **Evet** ' i seçin.
   * AS numarası alanı şu anda sanal hub 'da düzenlenemez.
   * Açılan menüden **ağ geçidi ölçek birimleri** değerini seçin. Ölçek birimi, siteleri bağlamak için sanal hub 'da oluşturulan VPN ağ geçidinin toplam verimini seçmenizi sağlar. 1 ölçek birimi = 500 Mbps ' i seçerseniz, her biri en fazla 500 Mbps hızında olacak şekilde, artıklık için iki örnek oluşturulacaktır. Örneğin, her biri dalda 10 Mbps olan beş dala sahipseniz baş uçta 50 Mbps bir toplama işlemi yapmanız gerekir. Azure VPN ağ geçidinin Birleşik kapasitesini planlama, hub 'a dal sayısını desteklemek için gereken kapasite değerlendirdikten sonra yapılmalıdır.
6. Doğrulamak için **gözden geçir + oluştur** ' u seçin.
7. Hub 'ı oluşturmak için **Oluştur** ' u seçin. 30 dakika **sonra hub 'ı bir sayfada görüntülemek** için **yenileyin** . Kaynağa gitmek için **Kaynağa Git** ' i seçin.
