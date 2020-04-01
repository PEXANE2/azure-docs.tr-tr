---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3bd9489adaf46e604393fc7059d37443bdd5ec3e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73488880"
---
1. Oluşturduğunuz Sanal WAN'ı bulun. Sanal WAN sayfasında, **Bağlantı** bölümü altında **Hub'ları**seçin.
2. Hub'lar sayfasında, **sanal hub oluştur** sayfasını açmak için **+Yeni Hub'ı** seçin.

    ![Temel Bilgiler](./media/virtual-wan-tutorial-hub-include/basics.png "Temel Bilgiler")
3. Sanal **hub** sayfası Oluştur **Temelleri** sekmesinde aşağıdaki alanları tamamlayın:

    **Proje ayrıntıları**

   * Bölge (daha önce Konum olarak adlandırılır)
   * Adı
   * Hub özel adres alanı. Bir hub oluşturmak için minimum adres alanı /24'tür, bu da /25 ile /32 arasında değişen bir şeyin oluşturma sırasında hata oluşturacağını ima eder.
4. **Sonraki'ni Seçin: Siteden siteye.**

    ![Siteden siteye](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Siteden siteye")

5. **Siteden siteye** sekmesinde, aşağıdaki alanları tamamlayın:

   * Siteden siteye VPN oluşturmak için **Evet'i** seçin.
   * AS Numarası alanı şu anda sanal hub'da edinilemez.
   * Açılır geçitten **Ağ Geçidi ölçeği birimleri** değerini seçin. Ölçek birimi, siteleri bağlamak için sanal hub'da oluşturulan VPN ağ geçidinin toplam iş çıkışını seçmenizinbir parçası. 1 ölçek birimi = 500 Mbps seçerseniz, her biri maksimum 500 Mbps verimiçeren iki artıklık örneği oluşturulacağı anlamına gelir. Örneğin, her biri dalda 10 Mbps yapan beş şubeniz varsa, baş ucunda toplam 50 Mbps'ye ihtiyacınız olacaktır. Azure VPN ağ geçidinin toplam kapasitesinin planlanması, hub'daki şube sayısını desteklemek için gereken kapasite değerlendirildikten sonra yapılmalıdır.
6. Doğrulamak için **Gözden Geçir + Oluştur'u** seçin.
7. Hub'ı oluşturmak için **Oluştur'u** seçin. 30 dakika sonra **Hub'lar** sayfasındahub'ı görüntülemek için **yenileyin.** Kaynağa gitmek **için kaynağa** git'i seçin.