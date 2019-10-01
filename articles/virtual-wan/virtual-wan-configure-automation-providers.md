---
title: Azure sanal WAN iş ortakları | Microsoft Docs
description: Bu makale, iş ortaklarının Azure sanal WAN Otomasyonu 'nu ayarlama konusunda yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: cherylmc
ms.openlocfilehash: 72493f084b89d41c1e0d6ff60c35afa3491b0eda
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703451"
---
# <a name="virtual-wan-partners"></a>Sanal WAN iş ortakları

Bu makale, Azure sanal WAN için bir şube cihazını (bir müşteri şirket içi VPN cihazı veya SDWAN CPE) bağlamak ve yapılandırmak üzere otomasyon ortamının nasıl ayarlanacağını anlamanıza yardımcı olur. IPSec/Ikev2 veya IPSec/IKEv1 üzerinden VPN bağlantısı barındırabilecek dal cihazları sağlayan bir sağlayıcısıysanız, bu makale sizin için sizin içindir.

Bir dal aygıtı (bir müşteri şirket içi VPN cihazı veya SDWAN CPE) genellikle sağlanması için bir denetleyici/cihaz panosu kullanır. SD-WAN çözümü yöneticileri, bir cihazı ağa takılmadan önce önceden sağlamak için genellikle bir yönetim konsolu kullanabilirler. Bu VPN özellikli cihaz, bir denetleyiciden denetim düzlemi mantığını alır. VPN cihazı veya SD-WAN denetleyicisi, Azure sanal WAN bağlantısını otomatik hale getirmek için Azure API 'Leri kullanabilir. Bu tür bir bağlantı, şirket içi cihazın kendisine atanmış bir genel IP adresi olmasını gerektirir.

## <a name ="before"></a>Otomatikleştirmeye başlamadan önce

* Cihazınızın IPSec IKEv1/Ikev2 desteklediğini doğrulayın. Bkz. [varsayılan ilkeler](#default).
* Azure sanal WAN bağlantısını otomatik hale getirmek için kullanacağınız [REST API 'lerine](https://docs.microsoft.com/rest/api/azure/) bakın.
* Azure sanal WAN 'ın Portal deneyimini test edin.
* Sonra, bağlantı adımlarının hangi bölümünü otomatikleştirmek istediğinizi belirleyin. En azından, otomatikleştirilmesi önerilir:

  * Access Control
  * Şube cihaz bilgilerini Azure sanal WAN 'a yükleme
  * Azure yapılandırmasını indirme ve şube cihazından Azure sanal WAN 'a bağlantı ayarlama

* Beklenen müşteri deneyimini Azure sanal WAN ile birlikte anlayın.

  1. Genellikle, bir sanal WAN kullanıcısı bir sanal WAN kaynağı oluşturarak bu işlemi başlatır.
  2. Kullanıcı, şube bilgilerini Azure sanal WAN 'a yazmak için şirket içi sistem (Şube denetleyiciniz veya VPN cihaz sağlama yazılımınız) için bir hizmet sorumlusu tabanlı kaynak grubu erişimi ayarlar.
  3. Kullanıcı, şu anda Kullanıcı ARABIRIMINDE oturum açıp hizmet sorumlusu kimlik bilgilerini ayarlama kararı verebilir. Bu işlem tamamlandıktan sonra denetleyicinizin, sağlayabileceğiniz Otomasyon ile dal bilgilerini karşıya yükleyebilmeleri gerekir. Azure tarafında bu değerin el ile eşdeğeri ' site oluştur ' ' tur.
  4. Site (Şube aygıtı) bilgileri Azure 'da kullanılabilir olduğunda, kullanıcı siteyi bir hub 'a bağlayacaktır. Bir sanal hub, Microsoft tarafından yönetilen bir sanal ağ. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Bu işlem sırasında, Azure bölgesi başına yalnızca bir hub olabilir ve içindeki VPN uç noktası (vpngateway) oluşturulur. VPN Gateway, bant genişliği ve bağlantı ihtiyaçlarına göre uygun şekilde boyutlardaki ölçeklenebilir bir ağ geçidindir. Sanal hub ve vpngateway oluşturma 'yı dal cihaz denetleyicisi panoınızdan otomatik hale getirmeyi seçebilirsiniz.
  5. Sanal hub siteyle ilişkilendirildiğinde, kullanıcının el ile indirilmesi için bir yapılandırma dosyası oluşturulur. Bu, otomasyonunun içinde geldiği ve kullanıcının sorunsuz bir şekilde karşılaşmasına neden olur. Şube cihazını el ile indirip yapılandırmak zorunda kalmadan, Otomasyonu ayarlayabilir ve Kullanıcı ARABIRIMINIZDEKI en az tıklama deneyimi sağlayabiliyor, böylece paylaşılan anahtar uyumsuzluğu, IPSec parametresi gibi tipik bağlantı sorunları hafifletmesini uyumsuzluk, yapılandırma dosyası okunabilirlik vb.
  6. Çözümünüzde bu adımın sonunda, kullanıcının şube cihazı ile sanal hub arasında sorunsuz bir siteden siteye bağlantısı olacaktır. Ayrıca, diğer hub 'larda ek bağlantılar da ayarlayabilirsiniz. Her bağlantı etkin-etkin bir tüneldir. Müşteriniz, tünele ilgili bağlantıların her biri için farklı bir ISS kullanmayı tercih edebilir.
  7. CPE yönetim arabiriminde sorun giderme ve izleme özellikleri sağlamayı düşünün. Tipik senaryolar, "bir CPE sorunu nedeniyle Azure kaynaklarına erişemeyebilirsiniz", "CPE tarafında IPSec parametrelerini göster" vb. arasında yer alır.

## <a name ="understand"></a>Otomasyon ayrıntılarını anlama


###  <a name="access"></a>Erişim denetimi

Müşteriler, cihaz Kullanıcı arabirimindeki sanal WAN için uygun erişim denetimini ayarlayabilmelidir. Bu, bir Azure hizmet sorumlusu kullanılarak önerilir. Hizmet sorumlusu tabanlı erişim, dal bilgilerini karşıya yüklemek için cihaz denetleyicisine uygun kimlik doğrulaması sağlar. Daha fazla bilgi için bkz. [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application). Bu işlev, Azure sanal WAN teklifi dışında olduğundan, Azure 'da ilgili ayrıntıların cihaz yönetimi panosuna nasıl alınacağından önce Azure 'da erişim ayarlamak için geçen tipik adımların aşağıda listelenmektedir

* Şirket içi cihaz denetleyiciniz için bir Azure Active Directory uygulaması oluşturun.
* Uygulama KIMLIĞINI ve kimlik doğrulama anahtarını al
* Kiracı kimliğini alma
* Uygulamayı "katkıda bulunan" rolüne ata

###  <a name="branch"></a>Şube cihaz bilgilerini karşıya yükle

Şube (Şirket içi site) bilgilerini Azure 'a yüklemek için Kullanıcı deneyimini tasarlayın. VPNSite için [REST API 'leri](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) , site BILGILERINI sanal WAN 'da oluşturmak için kullanılabilir. Tüm dal SDWAN/VPN cihazlarını verebilir veya uygun şekilde cihaz özelleştirmeleri seçebilirsiniz.


### <a name="device"></a>Cihaz yapılandırması indirme ve bağlantı

Bu adım, Azure yapılandırmasını indirmeyi ve şube cihazından Azure sanal WAN 'a bağlantı kurmayı içerir. Bu adımda, sağlayıcı kullanmayan bir müşteri Azure yapılandırmasını el ile indirip şirket içi SDWAN/VPN cihazına uygular. Sağlayıcı olarak, bu adımı otomatikleştirmelisiniz. Cihaz denetleyicisi, genellikle aşağıdaki dosyaya benzer olacak şekilde Azure yapılandırmasını indirmek için ' GetVpnConfiguration ' REST API arayabilir.

**Yapılandırma notları**

  * Azure VNET 'ler sanal hub 'a bağlıysa, bu kişiler Connectedalt ağları olarak görünürler.
  * VPN bağlantısı, rota tabanlı yapılandırmayı kullanır ve hem IKEv1 hem de IKEv2 protokollerini destekler.

#### <a name="understanding-the-device-configuration-file"></a>Cihaz yapılandırma dosyasını anlama

Cihaz yapılandırma dosyasında şirket içi VPN cihazınızı yapılandırırken kullanacağınız ayarlar bulunur. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanmış cihazın ayrıntıları yer alır. Dal cihazının adını ve genel IP adresini içerir.
* **vpnSiteConnections -** Bu bölümde aşağıdakilerle ilgili bilgiler yer alır:

    * Sanal hub 'lar VNet 'in **Adres alanı** .<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * Hub 'a bağlı sanal ağların **Adres alanı** .<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * vpngateway sanal hub'ının **IP adresleri**. vpngateway, etkin-etkin yapılandırmada 2 tünel içeren bağlantılara sahip olduğundan bu dosyada iki taraftaki IP adreslerinin de listelendiğini göreceksiniz. Bu örnekte her site için "Instance0" ve "Instance1" örneklerini göreceksiniz.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar vb. gibi **Vpngateway bağlantısı yapılandırma ayrıntıları** . PSK, sizin için otomatik olarak oluşturulan önceden paylaşılmış anahtardır. Dilediğiniz zaman genel bakış sayfasındaki bağlantıyı düzenleyerek özel bir PSK ekleyebilirsiniz.
  
#### <a name="example-device-configuration-file"></a>Örnek cihaz yapılandırma dosyası

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

## <a name="default"></a>IPSec bağlantısı için varsayılan ilkeler

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-include.md)]

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>Sanal hub vpngateway ilkesi ile şirket içi SDWAN/VPN cihazı veya SD-WAN yapılandırması arasında her şeyin eşleşmesi gerekir mi?

Şirket içi SDWAN/VPN cihazınız veya SD-WAN yapılandırmanızın Azure IPSec/ıKE ilkesinde belirttiğiniz aşağıdaki algoritmaların ve parametrelerin eşleşmesi veya içermesi gerekir.

* IKE şifreleme algoritması
* IKE bütünlük algoritması
* DH Grubu
* IPsec şifreleme algoritması
* IPsec bütünlük algoritması
* PFS Grubu

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [Azure sanal WAN](virtual-wan-about.md) ve [Azure sanal WAN hakkında SSS](virtual-wan-faq.md).

Ek bilgi için lütfen <azurevirtualwan@microsoft.com> adresine bir e-posta gönderin. Şirketinizin adını konu satırına “[ ]” içinde yazın.