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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331299"
---
1. Azure [portalı](https://portal.azure.com) menüsünden **kaynak oluştur'u**seçin. 

   ![Azure portalında kaynak oluşturma](./media/vpn-gateway-add-gw-rm-portal-include/azure-portal-create-resource.png)
2. **Marketi Ara** alanında 'Sanal Ağ Ağ Geçidi' yazın. Arama dönüşünde **Sanal ağ ağ geçidini** bulun ve girişi seçin. Sanal **ağ ağ geçidi** sayfasında **Oluştur'u**seçin. Bu işlem **Sanal ağ geçidi oluşturma** sayfasını açar.
3. Temel **Bilgiler** sekmesinde, sanal ağ ağ ağ geçidinizin değerlerini doldurun.

   ![Sanal ağ ağ geçidi sayfa alanları oluşturma](./media/vpn-gateway-add-gw-rm-portal-include/gateway1.png "Sanal ağ ağ geçidi sayfa alanları oluşturma")

   ![Sanal ağ ağ geçidi sayfa alanları oluşturma](./media/vpn-gateway-add-gw-rm-portal-include/gateway2.png "Sanal ağ ağ geçidi sayfa alanları oluşturma")

   **Proje ayrıntıları**

   - **Abonelik**: Açılır açılır yerden kullanmak istediğiniz aboneliği seçin.
   - **Kaynak Grubu**: Bu sayfada sanal ağınızı seçtiğinizde bu ayar otomatik olarak doldurulur.

   **Örnek ayrıntıları**

   - **Ad**: Ağ geçidinizi adlandırın. Ağ geçidinizi adlandırmak, ağ geçidi alt ağı adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
   - **Bölge**: Bu kaynağı oluşturmak istediğiniz bölgeyi seçin. Ağ geçidinin bulunduğu bölge, sanal ağla aynı olmalıdır.
   - **Ağ geçidi türü**: **VPN**’i seçin. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır.
   - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
   - **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   - **Nesil**: VPN Ağ Geçidi Oluşturma hakkında bilgi için [Ağ Geçidi SK'leri'ne](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)bakın.
   - **Sanal ağ**: Açılır geçitten, bu ağ geçidini eklemek istediğiniz sanal ağı seçin.
   - **Ağ geçidi alt ağ adresi aralığı**: Bu alan yalnızca VNet'inizin ağ geçidi alt ağı yoksa görünür. Mümkünse, aralığı /27 veya daha büyük (/26,/25 vb) yapın. /28'den daha küçük bir aralık oluşturmanızı önermiyoruz. Zaten bir ağ geçidi alt ağınız varsa, sanal ağınızda gezinerek GatewaySubnet ayrıntılarını görüntüleyebilirsiniz. Aralığı görüntülemek için **Alt Ağlar'ı** tıklatın. Aralığı değiştirmek istiyorsanız, GatewaySubnet'i silebilir ve yeniden oluşturabilirsiniz.

   **Genel IP adresi**: Bu ayar, VPN ağ geçidiyle ilişkilendirilen genel IP adresi nesnesini belirtir. VPN ağ geçidi oluşturulduğunda genel IP adresi bu nesneye dinamik olarak atanır. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

     - **Genel IP adresi**: Yeni **seçili oluştur'dan** ayrılın.
     - **Genel IP adresi adı**: Metin kutusuna, genel IP adresi örneğiniz için bir ad yazın.
     - **Atama**: VPN ağ geçidi yalnızca Dinamik'i destekler.

   **Etkin-Etkin mod**: Yalnızca etkin ağ geçidi yapılandırması oluşturuyorsanız **Etkin-Etkin modu Etkinleştir'i** seçin. Aksi takdirde bu ayarı seçmeyin.

   Yapılandırmanız için **BGP ASN yapılandır** ayarı özellikle gerekli değilse bu ayarı işaretlemeyin. Bu ayar gerekliyse varsayılan ASN değeri 65515’tir ancak bu değiştirilebilir.
4. Doğrulamayı çalıştırmak için **Gözden Geçir + oluştur'u** seçin. Doğrulama geçtikten sonra VPN ağ geçidini dağıtmak için **Oluştur'u** seçin. Bir ağ geçidinin tam olarak oluşturulması ve dağıtılması 45 dakika kadar sürebilir. Ağ geçidiniz için Genel Bakış sayfasında dağıtım durumunu görebilirsiniz.

Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir.
