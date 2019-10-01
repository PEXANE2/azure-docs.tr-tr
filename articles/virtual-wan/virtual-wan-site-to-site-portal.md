---
title: Azure sanal WAN kullanarak Azure ile siteden siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure sanal WAN kullanarak Azure 'a siteden siteye VPN bağlantısı oluşturma hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/25/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 357cc23c9dedb49dfd19dc897102762066ffd1b2
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679355"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Öğretici: Azure sanal WAN kullanarak siteden siteye bağlantı oluşturma

Bu öğreticide, Azure 'daki kaynaklarınıza bir IPSec/ıKE (IKEv1 ve Ikev2) VPN bağlantısı üzerinden bağlanmak için sanal WAN 'ın nasıl kullanılacağı gösterilmektedir. Bu tür bir bağlantı, şirket içinde bulunan ve dışarıdan kullanıma açık IP adresi atanmış bir VPN cihazı gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN 'A genel bakış](virtual-wan-about.md).

> [!NOTE]
> Birçok siteniz varsa, genellikle bu yapılandırmayı oluşturmak için bir [sanal WAN iş ortağı](https://aka.ms/virtualwan) kullanırsınız. Bununla birlikte, kendi VPN cihazınızı yapılandırırken Ağ ve yeterliliğinizden emin değilseniz bu yapılandırmayı kendiniz de oluşturabilirsiniz.
>

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Site oluşturma
> * Merkez oluşturma
> * Bir hub 'ı siteye bağlama
> * Uyumlu VNet oluşturma (henüz bir tane yoksa)
> * VNet 'i bir hub 'a bağlama
> * VPN cihaz yapılandırmasını indirme ve uygulama
> * Sanal WAN 'nizi görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="openvwan"></a>1. sanal WAN oluşturma

Bir tarayıcıdan [Azure Portal](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="site"></a>2. bir site oluşturun

Fiziksel konumlarınıza karşılık gelen ihtiyacınız olan sayıda site oluşturun. Örneğin, bir şube ofisiniz, Londra 'daki bir şube ve bir şube ofisi ve LA varsa, üç ayrı site oluşturursunuz. Bu siteler şirket içi VPN cihaz uç noktalarınızı içerir. Şu anda siteniz için yalnızca bir özel adres alanı belirtebilirsiniz.

1. Oluşturduğunuz WAN 'a tıklayın. WAN sayfasında, **sanal WAN mimarisi**altında **VPN siteleri ' ne tıklayarak VPN** siteleri sayfasını açın.
2. **VPN siteleri** sayfasında **+ site oluştur**' a tıklayın.
3. **Site oluştur** sayfasında, aşağıdaki alanları girin:

   * **Ad** -şirket içi sitenize başvurmak istediğiniz addır.
   * **Genel IP adresi** -şirket içi SITENIZDE bulunan VPN CIHAZıNıN genel IP adresi.
   * **Özel adres alanı** -şirket içi SITENIZDE bulunan IP adres alanıdır. Bu adres alanına giden trafik yerel sitenize yönlendirilir.
   * **Abonelik** -aboneliği doğrulayın.
   * **Kaynak grubu** -kullanmak istediğiniz kaynak grubu.
   * **Konum**
4. Ek ayarları görüntülemek için **Gelişmiş göster** ' e tıklayın. 

   BGP 'yi etkinleştirmek için **BGP** 'yi seçebilirsiniz. bu Işlem, Azure 'da bu site için oluşturulan tüm bağlantılarda BGP işlevlerini etkinleştirir. Sanal WAN üzerinde BGP 'yi yapılandırmak, Azure VPN ağ geçidinde BGP 'yi yapılandırmaya eşdeğerdir. Şirket içi BGP eşi adresiniz, cihaza VPN 'nizin genel IP adresi veya VPN sitesinin VNet adres alanı ile *aynı olmamalıdır.* BGP eşi IP 'niz için VPN cihazında farklı bir IP adresi kullanın. Bu, cihazdaki geri döngü arabirimine atanmış bir adres olabilir. Ancak, bir APIPA (169,254) *olamaz* . *x*. *x*) adresi. Bu adresi, konumu temsil eden karşılık gelen yerel ağ geçidinde belirtin. BGP önkoşulları için bkz. [Azure VPN Gateway Ile BGP hakkında](../vpn-gateway/vpn-gateway-bgp-overview.md).

   Ayrıca, **cihaz bilgilerini** de girebilirsiniz (isteğe bağlı alanlar). Bunun yapılması, Azure ekibinin daha sonra ek iyileştirme olanakları eklemek veya sorunu gidermenize yardımcı olmak için ortamınızı daha iyi anlamasına yardımcı olabilir.
   
5. **Onayla**' ya tıklayın.
6. **Onayla**' ya tıkladıktan sonra, VPN siteleri sayfasında durumu görüntüleyin. Site, **sağlama** durumundan **sağlandı**'e gidecek.

## <a name="hub"></a>3. bir hub oluşturun

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="associate"></a>4. siteleri hub ile ilişkilendirin

Hub 'lar genellikle VNet 'in bulunduğu bölgedeki sitelerle ilişkilendirilmelidir.

1. **VPN siteleri** sayfasında, hub ile ilişkilendirmek istediğiniz site veya siteleri seçin ve **+ yeni hub ilişkilendirmesi**' ne tıklayın.
2. **Siteleri bir veya daha fazla hub Ile ilişkilendir** sayfasında, açılan listeden bir hub seçin. **+ Bir Ilişki Ekle**' ye tıklayarak bir siteyi ek hub 'larla ilişkilendirebilirsiniz.
3. Ayrıca, burada belirli bir **PSK** ekleyebilir veya varsayılanı kullanabilirsiniz.
4. **Onayla**' ya tıklayın.
5. Bağlantı durumunu **VPN siteleri** sayfasında görüntüleyebilirsiniz.

## <a name="vnet"></a>5. sanal ağ oluşturma

Zaten bir sanal ağınız yoksa, PowerShell veya Azure portal kullanarak hızlı bir şekilde bir tane oluşturabilirsiniz. Zaten bir sanal ağınız varsa, bunun gerekli ölçütlere uygun olduğunu ve bir sanal ağ geçidine sahip olmadığını doğrulayın.

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="vnet"></a>6. VNet 'iniz bir hub 'a bağlanır

Bu adımda, hub 'ınız ile VNet arasında eşleme bağlantısı oluşturursunuz. Bağlanmak istediğiniz her VNet için bu adımları tekrarlayın.

1. Sanal WAN 'nizin sayfasında, **sanal ağ bağlantıları**' na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+ bağlantı ekle**' ye tıklayın.
3. **Bağlantı ekle** sayfasında, aşağıdaki alanları girin:

    * **Bağlantı adı** -bağlantınızı adlandırın.
    * **Hub 'lar** -Bu bağlantıyla ilişkilendirmek istediğiniz hub 'ı seçin.
    * **Abonelik** -aboneliği doğrulayın.
    * **Sanal ağ** -bu hub 'a bağlamak istediğiniz sanal ağı seçin. Sanal ağ, zaten var olan bir sanal ağ geçidine sahip olamaz.
4. Eşleme bağlantısını oluşturmak için **Tamam** ' ı tıklatın.

## <a name="device"></a>7. VPN yapılandırmasını indirin

Şirket içi VPN cihazınızı yapılandırmak için VPN cihaz yapılandırmasını kullanın.

1. Sanal WAN 'nizin sayfasında **Genel Bakış ' a**tıklayın.
2. Genel Bakış sayfasının en üstünde **VPN yapılandırmasını indir**' e tıklayın. Azure, ' Microsoft-Network-[location] ' kaynak grubunda, konumun WAN konumunun bulunduğu bir depolama hesabı oluşturur. Yapılandırmayı VPN cihazlarınıza uyguladıktan sonra, bu depolama hesabını silebilirsiniz.
3. Dosya oluşturma işlemi tamamlandıktan sonra, bu bağlantıyı indirmek için bağlantıya tıklayabilirsiniz.
4. Yapılandırmayı VPN cihazınıza uygulayın.

### <a name="understanding-the-vpn-device-configuration-file"></a>VPN cihazı yapılandırma dosyasını anlama

Cihaz yapılandırma dosyası, şirket içi VPN cihazınızı yapılandırırken kullanılacak ayarları içerir. Bu dosyayı görüntülediğinizde, aşağıdaki bilgilere dikkat edin:

* **Vpnsiteconfiguration-** Bu bölüm, sanal WAN 'a bağlanan bir site olarak ayarlanan cihaz ayrıntılarını gösterir. Bu, şube cihazının adını ve genel IP adresini içerir.
* **Vpnsiteconnections-** Bu bölüm aşağıdaki ayarlarla ilgili bilgiler sağlar:

    * Sanal hub 'lar VNet 'in **Adres alanı**<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * Hub 'a bağlı sanal ağların **Adres alanı**<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * Sanal hub vpngateway 'in **IP adresleri** . Vpngateway 'in her bağlantısı etkin-etkin yapılandırmada iki tünelde bulunduğundan, bu dosyada her iki IP adresinin de listelendiğini görürsünüz. Bu örnekte, her site için "Örnek0" ve "Instance1" görüntülenir.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar vb. gibi **Vpngateway bağlantısı yapılandırma ayrıntıları** . PSK, sizin için otomatik olarak oluşturulan önceden paylaşılmış anahtardır. Bağlantıyı, özel bir PSK için genel bakış sayfasında her zaman düzenleyebilirsiniz.
  
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
                  "10.30.0.0/16"
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
> Sanal bir WAN iş ortağı çözümüyle çalışıyorsanız, VPN cihaz yapılandırması otomatik olarak gerçekleşir. Cihaz denetleyicisi yapılandırma dosyasını Azure 'dan alır ve Azure bağlantısı kurmak için cihaza uygulanır. Bu, VPN cihazınızı el ile yapılandırmayı bilmeniz gerekmediği anlamına gelir.
>

Cihazınızı yapılandırmak için yönergeler gerekiyorsa, [VPN cihaz yapılandırma betikleri sayfasındaki](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) yönergeleri aşağıdaki uyarılarla birlikte kullanabilirsiniz:

* VPN cihazları sayfasındaki yönergeler sanal WAN için yazılmaz, ancak VPN cihazınızı el ile yapılandırmak için yapılandırma dosyasındaki sanal WAN değerlerini kullanabilirsiniz. 
* Yapılandırma farklı olduğu için VPN Gateway yönelik indirilebilir cihaz yapılandırma betikleri sanal WAN için çalışmaz.
* Yeni bir sanal WAN, hem IKEv1 hem de IKEv2 destekleyebilir.
* Sanal WAN yalnızca rota tabanlı VPN cihazları ve cihaz yönergeleri kullanabilir.

## <a name="viewwan"></a>8. sanal WAN 'nizi görüntüleyin

1. Sanal WAN 'a gidin.
2. Genel Bakış sayfasında, haritadaki her bir nokta bir hub 'ı temsil eder. Hub sistem durumu özetini görüntülemek için herhangi bir noktanın üzerine gelin.
3. Hub 'Lar ve bağlantılar bölümünde, hub durumu, site, bölge, VPN bağlantı durumu ve baytları içinde ve dışarı izleyebilirsiniz.

## <a name="viewhealth"></a>9. kaynak sistem durumunu görüntüleyin

1. WAN 'nize gidin.
2. WAN sayfanızda, **destek + sorun giderme** bölümünde, **sistem durumu** ' na tıklayın ve kaynağı görüntüleyin.

## <a name="connectmon"></a>10. bir bağlantıyı izleme

Bir Azure VM ile uzak site arasındaki iletişimi izlemeye yönelik bir bağlantı oluşturun. Bir bağlantı izleyicisini ayarlama hakkında daha fazla bilgi için bkz. [ağ Iletişimini izleme](~/articles/network-watcher/connection-monitor.md). Kaynak alanı Azure 'daki VM IP 'si, hedef IP ise site IP 'si olur.

## <a name="cleanup"></a>11. Kaynakları Temizleme

Bu kaynaklara artık ihtiyacınız kalmadığında, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "MyResourceGroup" değerini kaynak grubunuzun adıyla değiştirin ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Site oluşturma
> * Merkez oluşturma
> * Bir hub 'ı siteye bağlama
> * VNet 'i bir hub 'a bağlama
> * VPN cihaz yapılandırmasını indirme ve uygulama
> * Sanal WAN 'nizi görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

Sanal WAN hakkında daha fazla bilgi edinmek için bkz. [sanal WAN genel bakış](virtual-wan-about.md) sayfası.
