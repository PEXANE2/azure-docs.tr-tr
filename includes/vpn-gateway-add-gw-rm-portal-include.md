---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c3c9dbca6a76bf0b10a83087fc31d9fa41c6bd03
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78331299"
---
1. [Azure Portal](https://portal.azure.com) menüsünde **kaynak oluştur**' u seçin. 

   ![Azure portal bir kaynak oluşturun](./media/vpn-gateway-add-gw-rm-portal-include/azure-portal-create-resource.png)
2. Market 'te **Ara** alanına ' sanal ağ geçidi ' yazın. Arama dönüşindeki **sanal ağ geçidini** bulun ve girişi seçin. **Sanal ağ geçidi** sayfasında **Oluştur**' u seçin. Bu işlem **Sanal ağ geçidi oluşturma** sayfasını açar.
3. **Temel bilgiler** sekmesinde, sanal ağ geçidinizin değerlerini girin.

   ![Sanal ağ geçidi sayfası alanı oluştur](./media/vpn-gateway-add-gw-rm-portal-include/gateway1.png "Sanal ağ geçidi sayfası alanı oluştur")

   ![Sanal ağ geçidi sayfası alanı oluştur](./media/vpn-gateway-add-gw-rm-portal-include/gateway2.png "Sanal ağ geçidi sayfası alanı oluştur")

   **Proje ayrıntıları**

   - **Abonelik**: açılan listeden kullanmak istediğiniz aboneliği seçin.
   - **Kaynak grubu**: Bu sayfada Sanal ağınızı seçtiğinizde bu ayar tekrar doldurulur.

   **Örnek ayrıntıları**

   - **Ad**: Ağ geçidinizi adlandırın. Ağ geçidinizi adlandırma ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
   - **Bölge**: Bu kaynağı oluşturmak istediğiniz bölgeyi seçin. Ağ geçidinin bölgesi, sanal ağ ile aynı olmalıdır.
   - **Ağ geçidi türü**: **VPN**’i seçin. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır.
   - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
   - **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   - **Oluşturma**: VPN Gateway oluşturma hakkında bilgi için bkz. [ağ geçidi SKU 'ları](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).
   - **Sanal ağ**: açılan listeden, bu ağ geçidini eklemek istediğiniz sanal ağı seçin.
   - **Ağ geçidi alt ağ adres aralığı**: Bu alan yalnızca VNET 'iniz bir ağ geçidi alt ağına sahip değilse görüntülenir. Mümkünse, aralığı/27 veya daha büyük (/26,/25 vb.) aralığı yapın. /28 ' den küçük bir Aralık oluşturmanız önerilmez. Zaten bir ağ geçidi alt ağınız varsa, sanal ağınıza giderek GatewaySubnet ayrıntılarını görüntüleyebilirsiniz. Aralığı görüntülemek için **alt ağlar** ' a tıklayın. Aralığı değiştirmek istiyorsanız, GatewaySubnet 'i silip yeniden oluşturabilirsiniz.

   **Genel IP adresi**: Bu ayar, VPN ağ geçidiyle ilişkilendirilen genel IP adresi nesnesini belirtir. VPN ağ geçidi oluşturulduğunda genel IP adresi bu nesneye dinamik olarak atanır. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

     - **Genel IP adresi**: **Yeni oluştur** seçili kalsın.
     - **Genel IP adresi adı**: metin kutusuna, genel IP adresi örneğiniz için bir ad yazın.
     - **Atama**: VPN Gateway yalnızca dinamik destekler.

   **Etkin-etkin mod**: yalnızca etkin-etkin ağ geçidi yapılandırması oluşturuyorsanız **etkin-etkin modunu etkinleştir** ' i seçin. Aksi takdirde bu ayarı seçmeyin.

   Yapılandırmanız için **BGP ASN yapılandır** ayarı özellikle gerekli değilse bu ayarı işaretlemeyin. Bu ayar gerekliyse varsayılan ASN değeri 65515’tir ancak bu değiştirilebilir.
4. Doğrulamayı çalıştırmak için **gözden geçir + oluştur** ' u seçin. Doğrulama başarılı olduktan sonra, VPN ağ geçidini dağıtmak için **Oluştur** ' u seçin. Bir ağ geçidinin, tam olarak oluşturulması ve dağıtılması 45 dakika sürebilir. Dağıtım durumunu ağ geçidinizin Genel Bakış sayfasında görebilirsiniz.

Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir.
