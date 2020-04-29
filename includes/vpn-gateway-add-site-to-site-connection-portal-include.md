---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5149973fe63f867b49e55c970779c005e12536b9
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "68780098"
---
1. Sanal ağ geçidinizin sayfasını açın. Buraya çeşitli yollardan gidebilirsiniz. VNet 'iniz adına giderek ağ geçidine gidebilirsiniz- **> genel bakış-> bağlı cihazlar-ağ geçidinizin > adı**.
2. Ağ geçidinin sayfasında **Bağlantılar**' a tıklayın. Bağlantılar sayfasının üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** sayfasını açın.

   ![Siteden Siteye bağlantısı oluşturma](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. **Bağlantı ekle** sayfasında, bağlantınız için değerleri yapılandırın.

   - **Ad:** Bağlantınızı adlandırın.
   - **Bağlantı türü:****Siteden siteye (IPSec)** seçeneğini belirleyin.
   - **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
   - **Yerel ağ geçidi:****Bir yerel ağ geçidi seçin**'e tıklayıp kullanmak istediğiniz yerel ağ geçidini seçin.
   - **Paylaşılan Anahtar:** Buradaki değer, yerel şirket için VPN cihazınız için kullandığınız değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
   - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.

4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda yanıp sönen *Bağlantısı Oluşturuluyor* yazısını göreceksiniz.
5. Bağlantıyı sanal ağ geçidinin **Bağlantılar** sayfasında görüntüleyebilirsiniz. *Bilinmiyor* Durumu *Bağlanıyor* olarak ve ardından *Başarılı* olarak değişir.
