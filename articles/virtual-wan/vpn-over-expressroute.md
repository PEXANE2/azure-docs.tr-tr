---
title: 'ExpressRoute şifrelemeyi yapılandırın: Azure Virtual WAN için ExpressRoute üzerinden IPsec'
description: Bu eğitimde, ExpressRoute özel bakışları üzerinden siteden siteye VPN bağlantısı oluşturmak için Azure Virtual WAN'ı nasıl kullanacağınızı öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 03/19/2020
ms.author: cherylmc
ms.openlocfilehash: b1e6305d142530ab19849f61f12a122d0c6434aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059295"
---
# <a name="expressroute-encryption-ipsec-over-expressroute-for-virtual-wan"></a>ExpressRoute şifreleme: Sanal WAN için ExpressRoute üzerinden IPsec

Bu makalede, bir Azure ExpressRoute devresinin özel bakışları üzerinden şirket içi ağınızdan Azure'a bir IPsec/IKE VPN bağlantısı oluşturmak için Azure Virtual WAN'ı nasıl kullanacağınızı gösterir. Bu teknik, genel internet üzerinden veya genel IP adreslerini kullanmadan, ExpressRoute üzerinden şirket içi ağlar ve Azure sanal ağları arasında şifreli bir geçiş sağlayabilir.

## <a name="topology-and-routing"></a>Topoloji ve yönlendirme

Aşağıdaki diyagram, ExpressRoute özel bakışüzerinde VPN bağlantısının bir örneğini gösterir:

![ExpressRoute üzerinden VPN](./media/vpn-over-expressroute/vwan-vpn-over-er.png)

Diyagram, ExpressRoute özel bakışları üzerinden Azure hub VPN ağ geçidine bağlı şirket içi ağ içindeki bir ağı gösterir. Bağlantı kurulumu basittir:

1. ExpressRoute devresi ve özel bakışile ExpressRoute bağlantısı kurun.
2. Bu makalede açıklandığı gibi VPN bağlantısını kurun.

Bu yapılandırmanın önemli bir yönü, şirket içi ağlar ve Azure arasında hem ExpressRoute hem de VPN yolları üzerinden yönlendirmedir.

### <a name="traffic-from-on-premises-networks-to-azure"></a>Şirket içi ağlardan Azure'a trafik

Şirket içi ağlardan Azure'a giden trafikiçin, Azure önekleri (sanal hub ve hub'a bağlı tüm konuşan sanal ağlar dahil) hem ExpressRoute özel bakan BGP hem de VPN BGP üzerinden duyurulur. Bu, şirket içi ağlardan Azure'a yönelik iki ağ yolu (yol) ile sonuçlanır:

- Bir IPsec korumalı yol üzerinde
- IPsec koruması *olmadan* ExpressRoute üzerinden doğrudan bir 

İletişime şifreleme uygulamak için, diyagramdaki VPN'e bağlı ağ için, şirket içi VPN ağ geçidi üzerinden Azure yollarının doğrudan ExpressRoute yolu üzerinden tercih edildiklerinin emin olmalısınız.

### <a name="traffic-from-azure-to-on-premises-networks"></a>Azure'dan şirket içi ağlara trafik

Aynı gereksinim, Azure'dan şirket içi ağlara olan trafik için de geçerlidir. IPsec yolunun doğrudan ExpressRoute yolu (IPsec olmadan) üzerinde tercih edilmesini sağlamak için iki seçeneğiniz vardır:

- VPN'e bağlı ağ için VPN BGP oturumunda daha spesifik öneklerin reklamını yaptı. VPN BGP oturumunda ExpressRoute özel eşleme üzerinden VPN'e bağlı ağı kapsayan daha geniş bir aralığın reklamını yapabilirsiniz, ardından daha spesifik aralıklar. Örneğin, ExpressRoute üzerinden 10.0.0.0/16 ve VPN üzerinden 10.0.1.0/24 reklamını ver.

- VPN ve ExpressRoute için ayrık öneklerin reklamını sağlayın. VPN'e bağlı ağ aralıkları diğer ExpressRoute bağlantılı ağlardan ayrıysa, vpn ve ExpressRoute BGP oturumlarında sırasıyla önekleri tanıtabilirsiniz. Örneğin, ExpressRoute üzerinden 10.0.0.0/24 ve VPN üzerinden 10.0.1.0/24 reklamını ver.

Bu örneklerin her ikisinde de Azure, VPN koruması olmadan doğrudan ExpressRoute üzerinden değil, VPN bağlantısı üzerinden 10.0.1.0/24'e trafik gönderir.

> [!WARNING]
> Hem ExpressRoute hem de VPN bağlantıları üzerinden *aynı* öneklerin reklamını yaparsanız, Azure Doğrudan VPN koruması olmadan ExpressRoute yolunu kullanır.
>

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="1-create-a-virtual-wan-and-hub-with-gateways"></a><a name="openvwan"></a>1. Ağ geçitleri olan sanal bir WAN ve hub oluşturun

Aşağıdaki Azure kaynakları ve ilgili şirket içi yapılandırmalar devam etmeden önce yerinde olmalıdır:

- Azure sanal WAN
- [ExpressRoute ağ geçidi](virtual-wan-expressroute-portal.md) ve VPN [ağ geçidi](virtual-wan-site-to-site-portal.md) olan sanal bir WAN hub'ı

Bir Azure sanal WAN ve ExpressRoute ilişkisi olan bir hub oluşturma adımları [için](virtual-wan-expressroute-portal.md)bkz. Sanal WAN'da VPN ağ geçidi oluşturma adımları [için](virtual-wan-site-to-site-portal.md)bkz.

## <a name="2-create-a-site-for-the-on-premises-network"></a><a name="site"></a>2. Şirket içi ağ için bir site oluşturma

Site kaynağı, sanal bir WAN için ExpressRoute VPN olmayan siteyle aynıdır. Şirket içi VPN aygıtının IP adresi artık özel bir IP adresi veya adım 1'de oluşturulan ExpressRoute özel eşleme ile erişilebilen şirket içi ağdaki genel bir IP adresi olabilir.

> [!NOTE]
> Şirket içi VPN aygıtının IP adresi, Azure ExpressRoute özel bakışları aracılığıyla sanal WAN hub'ına reklamı yapılan adres önekleri kapsamında *olmalıdır.*
>

1. Tarayıcınızdaki Azure portalına gidin. 
1. Oluşturduğunuz WAN'ı seçin. WAN sayfasında, **Bağlantı**altında, **VPN sitelerini**seçin.
1. VPN **siteleri** sayfasında **+Site oluştur'u**seçin.
1. **Site oluştur** sayfasında aşağıdaki alanları doldurun:
   * **Abonelik**: Aboneliği doğrulayın.
   * **Kaynak Grubu**: Kullanmak istediğiniz kaynak grubunu seçin veya oluşturun.
   * **Bölge**: VPN sitesi kaynağı için Azure bölgesini girin.
   * **Adı**: Şirket içi sitenize başvurmak istediğiniz adı girin.
   * **Aygıt satıcısı**: Şirket içi VPN aygıtının satıcısını girin.
   * **Kenarlık Ağ Geçidi Protokolü**: Şirket içi ağınız BGP kullanıyorsa "Etkinleştir"i seçin.
   * **Özel adres alanı**: Şirket içi sitenizde bulunan IP adres alanını girin. Bu adres alanı için ayrılan trafik VPN ağ geçidi üzerinden şirket içi ağa yönlendirilir.
   * **Hub'lar**: Bu VPN sitesini bağlamak için bir veya daha fazla hub seçin. Seçili hub'ların VPN ağ geçitleri zaten oluşturulmuş olmalıdır.
1. **Sonraki'ni seçin:** VPN bağlantı ayarları için >bağlantılar:
   * **Bağlantı Adı**: Bu bağlantıya başvurmak istediğiniz ad.
   * **Sağlayıcı Adı**: Bu sitenin internet servis sağlayıcısının adı. Bir ExpressRoute şirket içi ağ için, ExpressRoute servis sağlayıcısının adıdır.
   * **Hız**: Internet servis bağlantısı nın veya ExpressRoute devresinin hızı.
   * **IP adresi**: Şirket içi sitenizde bulunan VPN cihazının genel IP adresi. Veya, ExpressRoute şirket içinde, ExpressRoute üzerinden VPN cihazının özel IP adresidir.

   BGP etkinse, Azure'da bu site için oluşturulan tüm bağlantılar için geçerli olacaktır. BGP'yi sanal bir WAN üzerinde yapılandırmak, BGP'yi Azure VPN ağ geçidinde yapılandırmaya eşdeğerdir. 
   
   Şirket içi BGP eş adresiniz, VPN'inizin cihaza ip adresiyle veya VPN sitesinin sanal ağ adres alanıyla aynı *olmamalıdır.* BGP eş IP'niz için VPN aygıtında farklı bir IP adresi kullanın. Bu aygıttaki geri döngü arabirimine atanmış bir adres olabilir. Ancak, bir APIPA (169.254) *olamaz.* * x*. *x*) adresi. Bu adresi, konumu temsil eden ilgili yerel ağ ağ ağ geçidinde belirtin. BGP ön koşulları için [Azure VPN Ağ Geçidi ile BGP Hakkında'ya](../vpn-gateway/vpn-gateway-bgp-overview.md)bakın.

1. Sonraki'ni seçin: Ayar değerlerini kontrol etmek ve VPN sitesini oluşturmak için >gözden **geçirin + oluşturun.** Bağlanmak için **Hub'ları** seçtiyseniz, şirket içi ağ ile hub VPN ağ geçidi arasında bağlantı kurulur.

## <a name="3-update-the-vpn-connection-setting-to-use-expressroute"></a><a name="hub"></a>3. ExpressRoute'u kullanmak için VPN bağlantı ayarını güncelleştirin

VPN sitesini oluşturduktan ve hub'a bağlandıktan sonra, ExpressRoute özel eşlemi kullanmak üzere bağlantıyı yapılandırmak için aşağıdaki adımları kullanın:

1. Sanal WAN kaynak sayfasına geri dön ve hub kaynağını seçin. Veya VPN sitesinden bağlı hub'a gidin.
1. **Connectivity**altında VPN **(Siteden Siteye)** seçin.
1. ExpressRoute üzerinden VPN sitesindeki elipsleri (**...**) seçin ve **bu hub'a VPN bağlantısını edit'i**seçin.
1. **Azure Özel IP Adresini Kullanmak**için **Evet'i**seçin. Ayar, hub VPN ağ geçidini, ortak IP adresleri yerine bu bağlantı için ağ geçidindeki hub adresi aralığında ki özel IP adreslerini kullanacak şekilde yapılandırır. Bu, şirket içi ağdan gelen trafiğin, bu VPN bağlantısı için genel interneti kullanmak yerine ExpressRoute özel bakış yollarından geçişini sağlayacaktır. Aşağıdaki ekran görüntüsü ayarı gösterir.

   ![VPN bağlantısı için özel bir IP adresi kullanma ayarlaması](./media/vpn-over-expressroute/vpn-link-configuration.png)
   
1. **Kaydet'i**seçin.

Değişikliklerinizi kurtardıktan sonra, hub VPN ağ geçidi, ExpressRoute üzerinden şirket içi VPN aygıtıyla IPsec/IKE bağlantılarını kurmak için VPN ağ geçidindeki özel IP adreslerini kullanır.

## <a name="4-get-the-private-ip-addresses-for-the-hub-vpn-gateway"></a><a name="associate"></a>4. Hub VPN ağ geçidi için özel IP adreslerini alın

Hub VPN ağ geçidinin özel IP adreslerini almak için VPN aygıt yapılandırmasını indirin. Şirket içi VPN aygıtını yapılandırmak için bu adreslere ihtiyacınız vardır.

1. Hub'ınızın sayfasında, Bağlantı altında **VPN (Siteden Siteye)** seçeneğini **belirleyin.**
1. **Genel Bakış** sayfasının üst kısmında **VPN Config'i İndir'i**seçin. 

   Azure, *konumun* WAN'ın konumu olduğu kaynak grubunda "microsoft-network-[location]" adlı kaynak grubunda bir depolama hesabı oluşturur. Yapılandırmayı VPN aygıtlarınıza uyguladıktan sonra bu depolama hesabını silebilirsiniz.
1. Dosya oluşturulduktan sonra, dosyayı indirmek için bağlantıyı seçin.
1. Yapılandırmayı VPN cihazınıza uygulayın.

### <a name="vpn-device-configuration-file"></a>VPN cihaz yapılandırma dosyası

Cihaz yapılandırma dosyası, şirket içi VPN aygıtınızı yapılandırırken kullanılacak ayarları içerir. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration**: Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanan cihaz ayrıntılarını belirtir. Şube aygıtının adını ve genel IP adresini içerir.
* **vpnSiteConnections**: Bu bölümde aşağıdaki ayarlar hakkında bilgi verilmektedir:

    * Sanal hub'ın sanal ağının adres alanı.<br/>Örnek:
           ```
           "AddressSpace":"10.51.230.0/24"
           ```
    * Hub'a bağlı sanal ağların adres alanı.<br>Örnek:
           ```
           "ConnectedSubnets":["10.51.231.0/24"]
            ```
    * Sanal hub'ın VPN ağ geçidinin IP adresleri. VPN ağ geçidinin her bağlantısı etkin etkin yapılandırmadaki iki tünelden oluştuğundan, bu dosyada listelenen her iki IP adresini de görürsünüz. Bu örnekte, `Instance0` her `Instance1` site için gördüğünüz ve bunlar ortak IP adresleri yerine özel IP adresleridir.<br>Örnek:
           ``` 
           "Instance0":"10.51.230.4"
           "Instance1":"10.51.230.5"
           ```
    * BGP ve önceden paylaşılan anahtar gibi VPN ağ geçidi bağlantısı için yapılandırma ayrıntıları. Önceden paylaşılan anahtar sizin için otomatik olarak oluşturulur. Önceden paylaşılan özel bir anahtar için bağlantıyı **genel bakış** sayfasında her zaman edinebilirsiniz.
  
### <a name="example-device-configuration-file"></a>Örnek cihaz yapılandırma dosyası

```
[{
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"5b096293-edc3-42f1-8f73-68c14a7c4db3"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-ER-site",
        "IPAddress":"172.24.127.211",
        "LinkName":"VPN-over-ER"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"10.51.230.4",
            "Instance1":"10.51.230.5"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
    },
    {
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"fbdb34ea-45f8-425b-9bc2-4751c2c4fee0"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-INet-site",
        "IPAddress":"13.75.195.234",
        "LinkName":"VPN-over-INet"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"51.143.63.104",
            "Instance1":"52.137.90.89"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
}]
```

### <a name="configuring-your-vpn-device"></a>VPN cihazınızı yapılandırma

Cihazınızı yapılandırma yönergelerine ihtiyaç duyarsanız [VPN cihazı yapılandırma betikleri sayfasındaki](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) yönergeleri aşağıdaki uyarılarla birlikte kullanabilirsiniz:

* VPN aygıtı sayfasındaki talimatlar sanal bir WAN için yazılmamaktadır. Ancak VPN aygıtınızı el ile yapılandırmak için yapılandırma dosyasındaki sanal WAN değerlerini kullanabilirsiniz. 
* Yapılandırma farklı olduğundan, VPN ağ geçidi için indirilebilir aygıt yapılandırma komut dosyaları sanal WAN için çalışmaz.
* Yeni bir sanal WAN hem IKEv1 hem de IKEv2'yi destekleyebilir.
* Sanal WAN yalnızca rota tabanlı VPN aygıtlarını ve aygıt yönergelerini kullanabilir.

## <a name="5-view-your-virtual-wan"></a><a name="viewwan"></a>5. Sanal WAN'ınızı görüntüleyin

1. Sanal WAN'a gidin.
1. Genel **Bakış** sayfasında, haritadaki her nokta bir hub'ı temsil eder.
1. **Hub'lar ve bağlantılar** bölümünde hub, site, bölge ve VPN bağlantı durumunu görüntüleyebilirsiniz. Ayrıca baytları girip çıkarabilirsiniz.

## <a name="7-monitor-a-connection"></a><a name="connectmon"></a>7. Bir bağlantıyı izleme

Bir Azure sanal makinesi (VM) ile uzak bir site arasındaki iletişimi izlemek için bir bağlantı oluşturun. Bağlantı izleyici oluşturma hakkında bilgi almak için bkz. [Ağ iletişimini izleme](~/articles/network-watcher/connection-monitor.md). Kaynak alan Azure'daki VM IP'dir ve hedef IP site IP'sidir.

## <a name="8-clean-up-resources"></a><a name="cleanup"></a>8. Kaynakları temizleme

Bu kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz. Aşağıdaki PowerShell komutunu çalıştırın ve kaynak grubunuzun adı ile değiştirin: `myResourceGroup`

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, Virtual WAN kullanarak ExpressRoute özel bakanlık üzerinden bir VPN bağlantısı oluşturmanıza yardımcı olur. Sanal WAN ve ilgili özellikler hakkında daha fazla bilgi edinmek için [Virtual WAN genel bakış ına](virtual-wan-about.md)bakın.
