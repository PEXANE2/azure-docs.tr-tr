---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc9e18427bb3b8094911d2ac7f285d271ecd1a21
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91024907"
---
1. [Azure Portal](https://portal.azure.com) menüsünde **kaynak oluştur**' u seçin.

   ![Kaynak](./media/vpn-gateway-add-local-network-gateway-portal-include/azure-portal-create-resource.png)
2. Market 'te **Ara** alanına **yerel ağ geçidi**yazın ve aramak için **ENTER** tuşuna basın. Böylece sonuçların bir listesi döndürülür. **Yerel ağ geçidi**’ne ve sonra **Oluştur** düğmesine tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

   :::image type="content" source="./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway-ip.png" alt-text="Yerel ağ geçidi IP adresi oluşturma":::

3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

   - **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin.
   - **Uç nokta:** Şirket içi VPN cihazı için uç nokta türünü seçin- **IP adresi** veya **FQDN (tam etki alanı adı)**.
      - **IP adresi**: VPN cihazınız için Internet hizmet sağlayıcınızdan ayrılan statik BIR genel IP ADRESINIZ varsa IP adresi seçeneğini BELIRLEYIN ve IP adresini örnekte gösterildiği gibi girin. Bu, Azure VPN ağ geçidinin bağlanmasını istediğiniz VPN cihazının genel IP adresidir. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
      - **FQDN**: genellikle Internet servis sağlayıcınız tarafından belirlenen belirli bir süre sonunda değişebilir bir dınamık genel IP adresiniz varsa, VPN cihazınızın GEÇERLI genel IP adresini göstermek için dınamık bir DNS hizmeti ile sabıt bir DNS adı kullanabilirsiniz. Azure VPN ağ geçidiniz, bağlanılacak genel IP adresini belirleyecek FQDN 'yi çözer. Aşağıdaki ekran görüntüsünde, IP adresi yerine FQDN kullanma örneği gösterilmektedir.
   - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Şirket içi sitenize bağlanmak istiyorsanız, burada, örnekte gösterilen değerleri değil, kendi değerlerinizi kullanın*.
   - **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
   - **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
   - **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
   - **Konum:** Konum, diğer ayarların **bölgesiyle** aynı olur. Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

   Bu, aynı sayfasıdır, ancak FQDN vurgulanarak:

   :::image type="content" source="./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-gateway-fqdn.png" alt-text="Yerel ağ geçidi FQDN 'SI oluşturma":::

   > [!NOTE]
   >
   > * Azure VPN her FQDN için yalnızca bir IPv4 adresi destekler. Etki alanı adı birden çok IP adresi olarak çözümlenirse Azure VPN Gateway, DNS sunucuları tarafından döndürülen ilk IP adresini kullanır. Belirsizlik ortadan kaldırmak için, FQDN 'nizin her zaman tek bir IPv4 adresine çözümlenmenizi öneririz. IPv6 desteklenmiyor.
   > * Azure VPN Gateway, her 5 dakikada bir DNS önbelleğini korur. Ağ Geçidi, yalnızca bağlantısı kesilen tüneller için FQDN 'leri çözümlemeye çalışır. Ağ geçidini sıfırlamak, FQDN çözümlemesini de tetikler.
   >

4. Değerleri belirtmeyi tamamladığınızda, sayfanın alt kısmındaki **Oluştur** düğmesini seçerek yerel ağ geçidini oluşturun.
