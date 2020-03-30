---
title: Azure Virtual WAN iş ortakları otomasyon kuralları | Microsoft Dokümanlar
description: Bu makale, iş ortaklarının Azure Virtual WAN otomasyonu ayarlamalarına yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: cherylmc
ms.openlocfilehash: 7848dda09b39f446dd218b7ce1eb2a07664bcaa6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190410"
---
# <a name="automation-guidelines-for-virtual-wan-partners"></a>Sanal WAN iş ortakları için otomasyon yönergeleri

Bu makale, Azure Virtual WAN için bir şube aygıtına (müşteri şirket içi VPN aygıtı veya SDWAN CPE) bağlanmak ve yapılandırmak için otomasyon ortamını nasıl ayarlayacağınızı anlamanıza yardımcı olur. IPsec/IKEv2 veya IPsec/IKEv1 üzerinden VPN bağlantısı barındırabilen şube aygıtları sağlayan bir sağlayıcıysanız, bu makale tam size göre.

Bir şube aygıtı (müşteri şirket içi VPN aygıtı veya SDWAN CPE) genellikle sağlanması için bir denetleyici/aygıt panosu kullanır. SD-WAN çözüm yöneticileri, bir aygıtı ağa takılmadan önce önceden sağlamak için genellikle bir yönetim konsolu kullanabilir. Bu VPN özellikli cihaz kontrol düzlemi mantığını bir kumandadan alır. VPN Aygıtı veya SD-WAN denetleyicisi, Azure Virtual WAN bağlantısını otomatikleştirmek için Azure API'lerini kullanabilir. Bu tür bir bağlantı, şirket içi aygıtın kendisine dışarıdan bakan bir genel IP adresine sahip olmasını gerektirir.

## <a name="before-you-begin-automating"></a><a name ="before"></a>Otomatikmeye başlamadan önce

* Cihazınızın IPsec IKEv1/IKEv2'yi desteklediğini doğrulayın. [Varsayılan ilkelere](#default)bakın.
* Azure Virtual WAN bağlantısını otomatikleştirmek için kullandığınız [REST API'lerini](#additional) görüntüleyin.
* Azure Virtual WAN'ın portal deneyimini test edin.
* Ardından, bağlantı adımlarının hangi bölümünü otomatikleştirmek istediğinize karar verin. En azından otomatikleme yapmanızı öneririz:

  * Erişim Denetimi
  * Şube aygıtı bilgilerinin Azure Virtual WAN'a yüklenmesi
  * Azure yapılandırmasını indirme ve şube aygıtından Azure Virtual WAN'a bağlantı ayarlama

### <a name="additional-information"></a><a name ="additional"></a>Ek bilgiler

* Sanal Hub oluşturmaotomatikleştirmek için [REST API](https://docs.microsoft.com/rest/api/virtualwan/virtualhubs)
* Virtual WAN için Azure VPN ağ geçidini otomatikleştirmek için [REST API](https://docs.microsoft.com/rest/api/virtualwan/vpngateways)
* VPN Sitesini Azure VPN Hub'ına bağlamak için [REST API](https://docs.microsoft.com/rest/api/virtualwan/vpnconnections)
* [Varsayılan IPsec ilkeleri](#default)

## <a name="customer-experience"></a><a name ="ae"></a>Müşteri deneyimi

Azure Virtual WAN ile birlikte beklenen müşteri deneyimini anlayın.

  1. Genellikle, sanal bir WAN kullanıcısı sanal wan kaynağı oluşturarak işlemi başlatır.
  2. Kullanıcı, Azure Virtual WAN'a şube bilgilerini yazmak için şirket içi sistem (şube denetleyiciniz veya VPN aygıt sağlama yazılımı) için hizmet temeltabanlı kaynak grubu erişimi ayarlar.
  3. Kullanıcı şu anda Kullanıcı Arabirimi'nize giriş yapmaya ve hizmet temel kimlik bilgilerini ayarlamaya karar verebilir. Bu tamamlandığında, kumandanız sağlayacağınız otomasyonla şube bilgilerini yükleyebilmelidir. Bunun Azure tarafındaki manuel karşılığı 'Site Oluştur'dur.
  4. Site (dal aygıtı) bilgileri Azure'da kullanılabilir hale geldikten sonra, kullanıcı siteyi bir hub'a bağlar. Sanal hub, Microsoft tarafından yönetilen bir sanal ağdır. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Azure bölgesi başına yalnızca bir hub olabilir ve bu işlem sırasında içindeki vpn bitiş noktası (vpngateway) oluşturulur. VPN ağ geçidi, bant genişliği ve bağlantı gereksinimlerine göre uygun boyutlara ulaşan ölçeklenebilir bir ağ geçididir. Şube cihaz denetleyici panonuzdan sanal hub ve vpngateway oluşturmayı otomatikleştirmeyi seçebilirsiniz.
  5. Sanal Hub siteyle ilişkilendirildikten sonra, kullanıcının el ile karşıdan yüklenebilmek için bir yapılandırma dosyası oluşturulur. Otomasyonunuzun devreye girdiği ve kullanıcı deneyimini sorunsuz hale getirdiği yer burasıdır. Kullanıcının şube aygıtını el ile indirmesi ve yapılandırması yerine, otomasyonu ayarlayabilir ve kullanıcı aranızda en az tıklama deneyimi sağlayabilir ve böylelikle paylaşılan anahtar uyuşmazlığı, IPSec parametresi gibi tipik bağlantı sorunlarını hafifletebilirsiniz. uyumsuzluk, yapılandırma dosyası okunabilirlik vb.
  6. Çözümünüzdeki bu adımın sonunda, kullanıcı şube aygıtı ile sanal hub arasında sorunsuz bir siteden siteye bağlantıya sahip olacaktır. Ayrıca diğer hub'larda ek bağlantılar da ayarlayabilirsiniz. Her bağlantı etkin-etkin bir tüneldir. Müşteriniz tünelin her bağlantıları için farklı bir ISS kullanmayı tercih edebilir.
  7. CPE yönetim arabiriminde sorun giderme ve izleme özellikleri sağlamayı düşünün. Tipik senaryolar arasında "Müşteri CPE sorunu nedeniyle Azure kaynaklarına erişemez", "CPE tarafında IPsec parametrelerini göster" vb. yer alır.

## <a name="automation-details"></a><a name ="understand"></a>Otomasyon detayları

###  <a name="access-control"></a><a name="access"></a>Erişim denetimi

Müşteriler, sanal wan için uygun erişim denetimini aygıt ui'sinde ayarlayabilmeli. Bu, bir Azure Hizmet Sorumlusu kullanılarak önerilir. Hizmet temel tabanlı erişim, aygıt denetleyicisine şube bilgilerini yüklemek için uygun kimlik doğrulaması sağlar. Daha fazla bilgi için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) Bu işlevsellik Azure Sanal WAN teklifinin dışında olsa da, azure'da erişimi ayarlamak için atılan ve ilgili ayrıntıların aygıt yönetimi panosuna girildiği tipik adımları aşağıda listeliyoruz.

* Şirket içi aygıt denetleyiciniz için bir Azure Etkin Dizin uygulaması oluşturun.
* Uygulama kimliği ve kimlik doğrulama anahtarı nı alma
* Kiracı kimliğini alma
* "Katılımcı" rolüne uygulama atama

###  <a name="upload-branch-device-information"></a><a name="branch"></a>Şube cihaz bilgilerini yükleme

Azure'a şube (şirket içi site) bilgilerini yüklemek için kullanıcı deneyimini tasarlamanız gerekir. Sanal WAN'da site bilgilerini oluşturmak için VPNSitesi için [REST API'lerini](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) kullanabilirsiniz. Tüm dal SDWAN/VPN aygıtlarını sağlayabilir veya uygun şekilde aygıt özelleştirmelerini seçebilirsiniz.

### <a name="device-configuration-download-and-connectivity"></a><a name="device"></a>Cihaz yapılandırma sıyükleme ve bağlantı

Bu adım, Azure yapılandırmasını indirmeyi ve şube aygıtından Azure Virtual WAN'a bağlantı ayarlamayı içerir. Bu adımda, bir sağlayıcı kullanmayan bir müşteri Azure yapılandırmasını el ile karşıdan yükleyip şirket içi SDWAN/VPN aygıtına uygular. Sağlayıcı olarak, bu adımı otomatikleştirmeniz gerekir. Ek bilgi için rest [API'lerini](https://docs.microsoft.com/rest/api/virtualwan/vpnsitesconfiguration/download) indirin. Aygıt denetleyicisi Azure yapılandırmasını indirmek için 'GetVpnConfiguration' REST API'yi arayabilir.

**Yapılandırma notları**

  * Azure VNet'ler sanal hub'a bağlıysa, Bunlar ConnectedSubnets olarak görünür.
  * VPN bağlantısı rota tabanlı yapılandırmayı kullanır ve hem IKEv1 hem de IKEv2 protokollerini destekler.

## <a name="device-configuration-file"></a><a name="devicefile"></a>Aygıt yapılandırma dosyası

Cihaz yapılandırma dosyasında şirket içi VPN cihazınızı yapılandırırken kullanacağınız ayarlar bulunur. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanmış cihazın ayrıntıları yer alır. Dal cihazının adını ve genel IP adresini içerir.
* **vpnSiteConnections -** Bu bölümde aşağıdakilerle ilgili bilgiler yer alır:

    * Sanal hub(lar) VNet adres **alanı.**<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * Hub'a bağlı VNet'lerin **adres alanı.**<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.3.0.0/16"]
         ```
    * vpngateway sanal hub'ının **IP adresleri**. vpngateway, etkin-etkin yapılandırmada 2 tünel içeren bağlantılara sahip olduğundan bu dosyada iki taraftaki IP adreslerinin de listelendiğini göreceksiniz. Bu örnekte her site için "Instance0" ve "Instance1" örneklerini göreceksiniz.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar vb. gibi **Vpngateway bağlantı yapılandırma ayrıntıları** PSK, sizin için otomatik olarak oluşturulan önceden paylaşılan anahtardır. Dilediğiniz zaman genel bakış sayfasındaki bağlantıyı düzenleyerek özel bir PSK ekleyebilirsiniz.
  
**Örnek cihaz yapılandırma dosyası**

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

## <a name="connectivity-details"></a><a name="default"></a>Bağlantı ayrıntıları

Şirket içi SDWAN/VPN aygıtınız veya SD-WAN yapılandırmanız, Azure IPsec/IKE ilkesinde belirttiğiniz aşağıdaki algoritmalar ve parametrelerle eşleşmeli veya içermelidir.

* IKE şifreleme algoritması
* IKE bütünlük algoritması
* DH Grubu
* IPsec şifreleme algoritması
* IPsec bütünlük algoritması
* PFS Grubu

### <a name="default-policies-for-ipsec-connectivity"></a><a name="default"></a>IPSec bağlantısı için varsayılan ilkeler

[!INCLUDE [IPsec Default](../../includes/virtual-wan-ipsec-include.md)]

### <a name="custom-policies-for-ipsec-connectivity"></a><a name="custom"></a>IPsec bağlantısı için özel ilkeler

[!INCLUDE [IPsec Custom](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Virtual WAN hakkında daha fazla bilgi için Azure Sanal WAN ve Azure [Sanal WAN SSS](virtual-wan-faq.md) [hakkında](virtual-wan-about.md) bilgi alabiliyorum.

Herhangi bir ek bilgi için, <azurevirtualwan@microsoft.com>lütfen bir e-posta gönderin. Şirketinizin adını konu satırına “[ ]” içinde yazın.
