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
ms.openlocfilehash: 6f910dbe91ed8e1cb65eefa6dfc48c72a689bf25
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73491595"
---
1. Oluşturduğunuz Sanal WAN'ı bulun. Sanal WAN sayfasında, **Bağlantı** bölümü altında **Hub'ları**seçin.
2. Hub'lar sayfasında, **sanal hub oluştur** sayfasını açmak için **+Yeni Hub'ı** seçin.
3. Sanal **hub** sayfası Oluştur **Temelleri** sekmesinde aşağıdaki alanları tamamlayın:

   ![Temel Bilgiler](./media/virtual-wan-tutorial-er-hub-include/hub1.png "Temel Bilgiler")

    **Proje ayrıntıları**

   * Bölge (daha önce Konum olarak adlandırılır)
   * Adı
   * Hub özel adres alanı. Bir hub oluşturmak için minimum adres alanı /24'tür, bu da /25 ile /32 arasında değişen bir şeyin oluşturma sırasında hata oluşturacağını ima eder.
4. **ExpressRoute sekmesini**seçin.

5. **ExpressRoute** sekmesinde aşağıdaki alanları tamamlayın:

   ![ExpressRoute](./media/virtual-wan-tutorial-er-hub-include/hub2.png "ExpressRoute")

   * **ExpressRoute** ağ geçidi oluşturmak için **Evet'i** seçin.
   * Açılır geçitten **Ağ Geçidi ölçeği birimleri** değerini seçin.
6. Doğrulamak için **Gözden Geçir + Oluştur'u** seçin.
7. Hub'ı oluşturmak için **Oluştur'u** seçin. 30 dakika sonra **Hub'lar** sayfasındahub'ı görüntülemek için **yenileyin.** Kaynağa gitmek **için kaynağa** git'i seçin.