---
title: Ağ geçidiniz için VPN NAT kurallarını yapılandırma
titleSuffix: Azure Virtual WAN
description: VWAN VPN ağ geçidiniz için NAT kurallarını yapılandırma hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 02/17/2021
ms.author: cherylmc
ms.openlocfilehash: a31b3718eb1baa32aef39474383924efe8cf93b6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746933"
---
# <a name="configure-nat-rules-for-your-virtual-wan-vpn-gateway---preview"></a>Sanal WAN VPN Gateway için NAT kurallarını yapılandırma-Önizleme

> [!IMPORTANT]
> NAT kuralları şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sanal WAN VPN ağ geçidinizi statik bire bir NAT kurallarıyla yapılandırabilirsiniz. NAT kuralı, IP adreslerinin bire bir çevirisini ayarlamaya yönelik bir mekanizma sağlar. NAT, uyumsuz veya çakışan IP adreslerine sahip iki IP ağını bağlamak için kullanılabilir. Tipik bir senaryo, Azure VNet kaynaklarına erişmek isteyen çakışan IP 'Lere sahip dallarıdır.

Bu yapılandırma, bir dış (ana bilgisayar) IP adresinden gelen trafiği bir sanal ağ (sanal makine, bilgisayar, kapsayıcı, vb.) içindeki bir uç noktayla ilişkili iç IP adresine yönlendirmek için bir akış tablosu kullanır.

   :::image type="content" source="./media/nat-rules-vpn-gateway/diagram.png" alt-text="Mimariyi gösteren diyagram.":::

## <a name="configure-and-view-rules"></a><a name="view"></a>Kuralları yapılandırma ve görüntüleme

NAT kurallarını VPN Gateway ayarlarınızda dilediğiniz zaman yapılandırabilir ve görüntüleyebilirsiniz.

   :::image type="content" source="./media/nat-rules-vpn-gateway/edit-rules.png" alt-text="Düzenleme kurallarını gösteren ekran görüntüsü.":::

1. Sanal hub 'ınıza gidin.
1. **VPN (siteden siteye)** seçeneğini belirleyin.
1. **NAT kuralları (düzenleme)** seçeneğini belirleyin.
1. **NAT kuralını Düzenle** sayfasında, aşağıdaki değerleri kullanarak bir NAT kuralı **ekleyebilir/düzenleyebilir/silebilirsiniz** :

   * **Ad:** NAT kuralınız için benzersiz bir ad.
   * **Şunu yazın:** Se. Statik bire bir NAT, iç adres ve dış adres arasında bire bir ilişki kurar.
   * **Mod:** Inresssnat veya Yumurresssnat.  
      * IngressSnat modu (giriş kaynağı NAT olarak da bilinir), Azure hub 'ının siteden siteye VPN ağ geçidini giren trafiğe uygulanabilir.
      * Yumurresssnat modu (çıkış kaynağı NAT olarak da bilinir), Azure hub 'ından siteden siteye VPN ağ geçidi olan trafik için geçerlidir.
   * **InternalMapping:** Bir dış IP kümesiyle eşlenecek, içindeki kaynak IP 'lerin adres ön eki aralığı. Diğer bir deyişle, NAT öncesi adres ön eki aralığınız.
   * **Externalmapping:** Kaynak IP 'Lerinin eşlendiği, dış ağdaki hedef IP 'lerin adres ön eki aralığı. Diğer bir deyişle, NAT-posta adresi ön eki aralığıdır.
   * Bağlantı **bağlantısı:** Bir VPN sitesini neredeyse Azure hub 'ından site VPN Gateway 'e bağlayan bağlantı kaynağı.

### <a name="configuration-considerations"></a>Yapılandırma konuları

* Hem iç hem de dış eşleme için alt ağ boyutu statik bire bir NAT için aynı olmalıdır.
* ' Özel adres alanı ' alanına **Externalmapping** öneklerini eklemek IÇIN Azure Portal VPN sitesini düzenlemediğinizden emin olun. Şu anda, BGP etkin olan sitelerin, şirket içi BGP Announcer (cihaz BGP ayarları) dış eşleme önekleri için bir giriş içerdiğinden emin olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Siteden siteye yapılandırma hakkında daha fazla bilgi için bkz. [sanal WAN siteden siteye bağlantı yapılandırma](virtual-wan-site-to-site-portal.md).
