---
title: 'Azure Sanal WAN: Siteden Siteye bağlantılar oluşturma'
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a siteden siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: b4278cb2e8c5152f522258a37c37acda5efbacf8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239689"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Öğretici: Azure Sanal WAN kullanarak siteden siteye bağlantı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv1 ve IKEv2) VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal WAN oluşturma
> * Hub oluşturma
> * Site oluşturma
> * Bir siteyi hub'a bağlama
> * VPN sitesini hub'a bağlama
> * Bir sanal ağı bir hub'a bağlama
> * Yapılandırma dosyası indirme
> * Sanal WAN'ınızı görüntüleme

> [!NOTE]
> Birden fazla siteniz varsa bu yapılandırmayı oluşturmak için [Sanal WAN iş ortağı](https://aka.ms/virtualwan) kullanmanız gerekir. Ancak ağ bağlantıları ve kendi VPN cihazınızı yapılandırma konularında deneyimliyseniz bu yapılandırmayı kendiniz oluşturabilirsiniz.
>

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın hiçbirinin bağlanmak istediğiniz sanal ağlarla örtüşmediğini doğrulayın. Azure portalında sanal ağ oluşturmak için [Hızlı Başlangıç'a](../virtual-network/quick-create-portal.md)bakın.

* Sanal ağınızın herhangi bir sanal ağ ağ geçidi yok. Sanal ağınızın bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağların bunun yerine Sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, Virtual WAN tarafından oluşturulan ve kullanılan sanal bir ağdır. Hub için belirttiğiniz adres aralığı, bağlandığınız varolan sanal ağlarınizle çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını bilmiyorsanız, bu ayrıntıları sizin için sağlayabilecek biriyle işbirliği yapabilirsiniz.

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="create-a-virtual-wan"></a><a name="openvwan"></a>Sanal WAN oluşturma

Bir tarayıcıdan Azure portalına gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **Sanal WAN** yazın ve Enter'u seçin.
2. Sonuçlardan **Sanal WAN'ı** seçin. Sanal WAN sayfasında, WAN Oluştur sayfasını açmak için **Oluştur'u** tıklatın.
3. WAN **Oluştur** sayfasında, **Temel Bilgiler** sekmesinde aşağıdaki alanları doldurun:

   ![Sanal WAN](./media/virtual-wan-site-to-site-portal/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** - Yeni oluşturun veya varolanları kullanın.
   * **Kaynak grubu konumu** - Açılır kaynak konumundan bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** - WAN adını vermek istediğiniz Adı yazın.
   * **Türü:** Temel veya Standart. Temel WAN oluşturursanız, yalnızca bir Temel hub oluşturabilirsiniz. Temel hub'lar yalnızca VPN'den siteye bağlantı yeteneğine sahiptir.
4. Alanları doldurmayı bitirdikten sonra **Gözden Geçir +Oluştur'u**seçin.
5. Doğrulama geçtikten sonra, sanal WAN oluşturmak için **Oluştur'u** seçin.

## <a name="create-a-hub"></a><a name="hub"></a>Hub oluşturma

Hub, siteden siteye, ExpressRoute'a veya siteden siteye işlevsellik için ağ geçitleri içerebilen sanal ağdır. Hub oluşturulduktan sonra site eklemeseniz bile hub için ücretlendirilirsiniz. Sanal hub'da siteden siteye VPN ağ geçidioluşturmak 30 dakika sürer.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-s2s-hub-include.md)]

## <a name="create-a-site"></a><a name="site"></a>Site oluşturma

Artık fiziksel konumlarınıza karşılık gelen siteleri oluşturmaya hazırsınız. Fiziksel konumlarınıza karşılık gelecek sayıda site oluşturabilirsiniz. Örneğin İstanbul'da, Ankara'da ve İzmir'de birer şubeniz varsa üç ayrı site oluşturmanız gerekir. Bu siteler şirket içi VPN cihazı uç noktalarını içerir. Sanal WAN'da Sanal Hub başına en fazla 1000 site oluşturabilirsiniz. Birden çok hub'ınvarsa, bu hub'ların her biri için 1000 oluşturabilirsiniz. Sanal WAN iş ortağınız (bağlantı ekleme) CPE aygıtınız varsa, Azure'a olan otomasyonları hakkında bilgi edinmek için onlarla birlikte kontrol edin. Genellikle otomasyon, büyük ölçekli şube bilgilerini azure'a aktarmak ve CPE'den Azure Virtual WAN VPN ağ geçidine bağlantı kurmak için basit tıklama deneyimi anlamına gelir. Daha fazla bilgi için [Azure'dan CPE iş ortaklarına Otomasyon kılavuzuna](virtual-wan-configure-automation-providers.md)bakın.

[!INCLUDE [Create a site](../../includes/virtual-wan-tutorial-s2s-site-include.md)]

## <a name="connect-the-vpn-site-to-the-hub"></a><a name="connectsites"></a>VPN sitesini hub'a bağlayın

Bu adımda VPN sitenizi hub'a bağlarsınız.

[!INCLUDE [Connect VPN sites](../../includes/virtual-wan-tutorial-s2s-connect-vpn-site-include.md)]

## <a name="connect-the-vnet-to-the-hub"></a><a name="vnet"></a>VNet'i hub'a bağlayın

Bu adımda, hub'ınız la VNet arasındaki bağlantıyı oluşturursunuz. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Sanal ağ bağlantısını oluşturmak için **Tamam'ı** tıklatın.

## <a name="download-vpn-configuration"></a><a name="device"></a>VPN yapılandırmasını indirme

Şirket içi VPN cihazınızı yapılandırmak için VPN cihazı yapılandırmasını kullanın.

1. Sanal WAN'ınızın sayfasında **Genel bakış**'a tıklayın.
2. **Hub ->VPNSite** sayfasının üst kısmında VPN **config indir'i**tıklatın. Azure, konumun WAN'ın konumu olduğu kaynak grubunda 'microsoft-network-[location]' adresinde bir depolama hesabı oluşturur. Yapılandırmayı VPN cihazlarınıza uyguladıktan sonra bu depolama hesabını silebilirsiniz.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. Yapılandırmayı şirket içi VPN aygıtınıza uygulayın.

### <a name="understanding-the-vpn-device-configuration-file"></a>VPN cihazı yapılandırma dosyasını anlama

Cihaz yapılandırma dosyasında şirket içi VPN cihazınızı yapılandırırken kullanacağınız ayarlar bulunur. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanmış cihazın ayrıntıları yer alır. Dal cihazının adını ve genel IP adresini içerir.
* **vpnSiteBağlantıları -** Bu bölümde aşağıdaki ayarlar hakkında bilgi verilmektedir:

    * Sanal hub sanal ağının **adres alanı**<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * Hub'a bağlı sanal ağların **adres alanı**<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.3.0.0/16"]
         ```
    * vpngateway sanal hub'ının **IP adresleri**. Vpnağ geçidinin her bağlantısı etkin etkin yapılandırmada iki tünelden oluştuğundan, bu dosyada listelenen her iki IP adresini de görürsünüz. Bu örnekte her site için "Instance0" ve "Instance1" örneklerini göreceksiniz.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar vb. gibi **Vpngateway bağlantı yapılandırma ayrıntıları** PSK, sizin için otomatik olarak oluşturulan önceden paylaşılan anahtardır. Dilediğiniz zaman genel bakış sayfasındaki bağlantıyı düzenleyerek özel bir PSK ekleyebilirsiniz.
  
### <a name="example-device-configuration-file"></a>Örnek cihaz yapılandırma dosyası

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.3.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>VPN cihazınızı yapılandırma

>[!NOTE]
> Bir Sanal WAN iş ortağı çözümü ile çalışıyorsanız VPN cihazı yapılandırması otomatik olarak gerçekleşir. Cihaz denetleyicisi, Azure'dan yapılandırma dosyasını alarak Azure bağlantısını kurmak için cihaza uygular. Bu da VPN cihazını el ile yapılandırmayı bilmenize gerek olmadığı anlamına gelir.
>

Cihazınızı yapılandırma yönergelerine ihtiyaç duyarsanız [VPN cihazı yapılandırma betikleri sayfasındaki](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) yönergeleri aşağıdaki uyarılarla birlikte kullanabilirsiniz:

* VPN cihazları sayfasındaki yönergeler Sanal WAN için yazılmamıştır ancak yapılandırma dosyasındaki Sanal WAN değerlerini kullanarak VPN cihazınızı el ile yapılandırabilirsiniz. 
* Farklı yapılandırmaya sahip olduğundan VPN Gateway'e özgü indirilebilir cihaz yapılandırma betikleri Sanal WAN ile çalışmaz.
* Yeni bir Sanal WAN hem IKEv1 hem de IKEv2'yi destekleyebilir.
* Virtual WAN hem ilke tabanlı hem de rota tabanlı VPN aygıtlarını ve aygıt yönergelerini kullanabilir.

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel **Bakış** sayfasında, haritadaki her nokta bir hub'ı temsil eder. Hub sistem durumu özetini, bağlantı durumunu ve baytları girip çıkarmak için herhangi bir noktanın üzerine tinleyip görüntüleyin.
3. Hub'lar ve bağlantılar bölümünde hub durumunu, VPN sitelerini vb. görüntüleyebilirsiniz. Belirli bir hub adını tıklatabilir ve ek ayrıntılar için VPN Sitesine gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
