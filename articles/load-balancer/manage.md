---
title: Azure Load Balancer portal ayarları
description: Azure Load Balancer portal ayarları hakkında bilgi edinmeye başlayın
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/8/2020
ms.author: allensu
ms.openlocfilehash: e1080aea12e70f4312fbee07b063d5a5cfbd1201
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89596315"
---
# <a name="azure-load-balancer-portal-settings"></a>Azure Load Balancer portal ayarları

Azure Load Balancer oluştururken, bu makaledeki bilgiler, bireysel ayarlar ve doğru yapılandırma hakkında daha fazla bilgi edinmenize yardımcı olur.

## <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Azure Load Balancer, trafiği arka uç havuzundaki VM örnekleri arasında dağıtan bir ağ yükü dengeleyicidir.

### <a name="basics"></a>Temel Bilgiler

Yük dengeleyici portalı Oluştur sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri görürsünüz:



| Ayar |  Ayrıntılar |
| ---------- | ---------- |
| Abonelik  | Aboneliğinizi seçin. Bu seçim, yük dengeleyicinizin dağıtılmasını istediğiniz aboneliğiniz. |
| Kaynak grubu | **Yeni oluştur** ' u seçin ve metin kutusuna kaynak grubunuzun adını yazın. Oluşturulmuş bir kaynak grubunuz varsa, bunu seçin. |
| Name | Bu ayar Azure Load Balancer adıdır. |
| Bölge | Yük dengeleyicinizi dağıtmak istediğiniz bir Azure bölgesi seçin. |
| Tür | Yük dengeleyici iki türe sahiptir: </br> **İç (özel)** </br> **Ortak (harici)**.</br> İç yük dengeleyici (ıLB), trafiği özel bir IP adresi aracılığıyla arka uç havuzu üyelerine yönlendirir.</br> Ortak yük dengeleyici, istekleri Internet üzerinden arka uç havuzuna yönlendirir.</br> [Yük dengeleyici türleri](components.md#frontend-ip-configuration-)hakkında daha fazla bilgi edinin.|
| SKU  | **Standart**' ı seçin. </br> Yük dengeleyicinin iki SKU 'su vardır: **temel** ve **Standart**. </br> Temel, sınırlı işlevlere sahiptir. </br> **Standart** , üretim iş yükleri için önerilir. </br> [SKU 'lar](skus.md)hakkında daha fazla bilgi edinin. |

Türü olarak **genel** ' i seçerseniz, aşağıdaki bilgileri görürsünüz:

| Ayar |  Ayrıntılar |
| ---------- | ---------- |
| Genel IP adresi | Ortak Yük Dengeleyiciye genel IP adresi oluşturmak için **Yeni oluştur** ' u seçin. </br> Var olan bir genel IP varsa **Varolanı kullan**' ı seçin.  |
| Genel IP adresi adı | Genel IP adresinin adı.|
| Genel IP adresi SKU 'SU | Genel IP adreslerinin iki SKU 'su vardır: **temel** ve **Standart**. </br> Temel, bölge dayanıklılığı ve bölgesel özniteliklerini desteklemez. </br> **Standart** , üretim iş yükleri için önerilir. </br> Yük dengeleyici ve genel IP adresi SKU 'Larının **eşleşmesi gerekir**. |
| Atama | **Statik** , standart için otomatik olarak seçilir. </br> Temel genel IP 'Lerde iki tür vardır: **dinamik** ve **statik**. </br> Dinamik genel IP adresleri, oluşturma yapılıncaya kadar atanmaz. </br> Kaynak silinirse IP 'Ler kaybolabilir. </br> Statik IP adresleri önerilir. |
| Kullanılabilirlik alanı | Esnek yük dengeleyici oluşturmak için **bölge yedekli** ' ı seçin. </br> Bir ZGen yük dengeleyici oluşturmak için, **1**, **2**veya **3**' ten belirli bir bölge seçin. </br> Standart yük dengeleyici ve genel IP 'Ler bölgeleri destekler. </br> [Yük Dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin. </br> Temel için bölge seçimi görmezsiniz. Temel yük dengeleyici bölgeleri desteklemez. |
| Genel IPv6 adresi ekle | Yük dengeleyici ön uçta **IPv6** adreslerini destekler. </br> [Yük Dengeleyici ve IPv6](load-balancer-ipv6-overview.md) hakkında daha fazla bilgi edinin
| Yönlendirme tercihi | **Microsoft ağı**' nı seçin. </br> Microsoft ağı trafiğin Microsoft Global ağı aracılığıyla yönlendirildiği anlamına gelir. </br> Internet, trafiğin internet servis sağlayıcı ağı aracılığıyla yönlendirildiği anlamına gelir. </br> [Yönlendirme tercihleri](../virtual-network/routing-preference-overview.md) hakkında daha fazla bilgi edinin|

:::image type="content" source="./media/manage/create-public-load-balancer-basics.png" alt-text="Yük dengeleyici ortak oluşturma." border="true":::

Türünde **dahili** ' ı seçerseniz, aşağıdaki bilgileri görürsünüz:

| Ayar |  Ayrıntılar |
| ---------- | ---------- |
| Sanal ağ | İç yük dengeleyicinizin bir parçası olmasını istediğiniz sanal ağ. </br> İç yük dengeleyiciniz için seçtiğiniz özel ön uç IP adresi bu sanal ağdan alınacaktır. |
| IP adresi ataması | Seçenekleriniz **statik** veya **dinamik**. </br> Statik, IP 'nin değişmemesini sağlar. Dinamik bir IP değişebilir. |
| Kullanılabilirlik alanı | Seçenekleriniz şunlardır: </br> **Bölge yedekli** </br> **Bölge 1** </br> **Bölge 2** </br> **Bölge 3** </br> Yüksek oranda kullanılabilir ve kullanılabilirlik bölgesi hatalarıyla dayanıklı olan bir yük dengeleyici oluşturmak için, bölgesel olarak **yedekli** bir IP seçin. |

:::image type="content" source="./media/manage/create-internal-load-balancer-basics.png" alt-text="Yük Dengeleyici iç oluşturma." border="true":::

## <a name="frontend-ip-configuration"></a>Ön uç IP yapılandırması

Azure Load Balancer IP adresi. Bu, istemcilerle ilgili iletişim noktasıdır. 

Bir veya birden çok ön uç IP yapılandırmasına sahip olabilirsiniz. Yukarıdaki temel bilgiler bölümünden karşılaşırsanız, yük dengeleyiciniz için önceden bir ön uç oluşturmuş olursunuz. 

Yük dengeleyicisine bir ön uç IP yapılandırması eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **ön uç IP yapılandırması**' nı seçin ve **+ Ekle**' yi seçin.

| Ayar |  Ayrıntılar |
| ---------- | ---------- |
| Name | Ön uç IP yapılandırmanızın adı. |
| IP sürümü | Ön uçta istediğiniz IP adresi sürümü. </br> Yük dengeleyici hem IPv4 hem de IPv6 ön uç IP yapılandırmasını destekler. |
| IP türü | IP türü, tek bir IP adresinin ön uçta veya IP öneki kullanarak bir IP adresi aralığı ile ilişkili olup olmadığını belirler. </br> [Genel BIR IP öneki](../virtual-network/public-ip-address-prefix.md) , aynı uç noktaya sürekli olarak bağlanmanız gerektiğinde yardımcı olur. Ön ek, SNAT bağlantı noktası sorunlarıyla ilgili yardım almak için yeterli bağlantı noktası verilmesini sağlar. |
| Genel IP adresi (yukarıdaki öneki seçtiyseniz önek) | Yük dengeleyici ön ucu için yeni bir genel IP (veya ön ek) seçin veya oluşturun. |

:::image type="content" source="./media/manage/frontend.png" alt-text="Ön uç IP yapılandırması oluştur sayfası." border="true":::

## <a name="backend-pools"></a>Arka uç havuzları

Arka uç adres havuzu, arka uç havuzundaki sanal ağ arabirimlerinin IP adreslerini içerir. 

Yük dengeleyicisine bir arka uç havuzu eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **arka uç havuzları**' nı seçin ve **+ Ekle**' yi seçin.

| Ayar | Ayrıntılar |
| ---------- |  ---------- |
| Name | Arka uç havuzunuzun adı. |
| Sanal ağ | Arka uç örneklerinizin bulunduğu sanal ağ. |
| IP sürümü | Seçenekleriniz **IPv4** veya **IPv6**. |

Azure Load Balancer arka uç havuzuna sanal makineler veya sanal makine ölçek kümeleri ekleyebilirsiniz. Önce sanal makineler veya sanal makine ölçek kümeleri oluşturun. Sonra, bunları portalda yük dengeleyiciye ekleyin.

:::image type="content" source="./media/manage/backend.png" alt-text="Arka uç Havuzu Oluştur sayfası." border="true":::

## <a name="health-probes"></a>Sistem durumu araştırmaları

Arka uç sanal makinelerinizin veya örneklerinizin durumunu izlemek için bir sistem durumu araştırması kullanılır. Durum araştırma durumu, bir örneğe yeni bağlantıların durum denetimleri temelinde ne zaman gönderileceğini belirler. 

Yük dengeleyicisine bir sistem durumu araştırması eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **sistem durumu araştırmaları**' nı seçin ve **+ Ekle**' yi seçin.

| Ayar | Ayrıntılar |
| ---------- | ---------- |
| Name | Sistem durumu araştırmanın adı. |
| Protokol | Seçtiğiniz protokol, arka uç örneklerinin sağlıklı olup olmadığını belirlemek için kullanılan denetim türünü belirler. </br> Seçenekleriniz şunlardır: </br> **TCP** </br> **HTTPS** </br> **HTTP** </br> Doğru protokolü kullandığınızdan emin olun. Bu seçim, uygulamanızın yapısına göre değişir. </br> Sistem durumu araştırması ve araştırma yanıtlarının yapılandırması, hangi arka uç havuzu örneklerinin yeni akışlar alacağını belirler. </br> Bir arka uç uç noktasındaki uygulamanın başarısızlığını algılamak için sistem durumu araştırmalarını kullanabilirsiniz. </br> [Sistem durumu araştırmaları](load-balancer-custom-probe-overview.md)hakkında daha fazla bilgi edinin. |
| Bağlantı noktası | Durum araştırması için hedef bağlantı noktası. </br> Bu ayar, durum araştırmasının, örneğin durumunu belirlemede kullanacağı arka uç örneğindeki bağlantı noktasıdır. |
| Aralık | Yoklama denemeleri arasındaki saniye sayısı. </br> Aralık, sistem durumu araştırmasının ne sıklıkta arka uç örneğine ulaşmaya çalışacağını belirleyecek. </br> 5 ' i seçerseniz ikinci araştırma denemesi 5 saniye sonra yapılır ve bu şekilde devam eder. |
| İyi durumda olmayan durum eşiği | Bir VM sağlıksız olarak kabul edilmeden önce gerçekleşmesi gereken arka arkaya araştırma hatalarının sayısı.</br> 2 ' yi seçerseniz, art arda iki hatadan sonra bu arka uç örneğine yeni akış ayarlanmayacak. |

:::image type="content" source="./media/manage/health-probe.png" alt-text="Sistem durumu araştırması ekleyin." border="true":::

## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları

Gelen trafiğin arka uç havuzundaki tüm örneklere nasıl dağıtıldığını tanımlar. Yük Dengeleme kuralı, belirli bir ön uç IP yapılandırmasını ve bağlantı noktasını birden çok arka uç IP adresine ve bağlantı noktasına eşler.

Yük dengeleyicisine yük dengeleyici kuralı eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **Yük Dengeleme kuralları**' nı seçin ve **+ Ekle**' yi seçin.
    
| Ayar | Ayrıntılar |
| ---------- | ---------- |
| Name | Yük dengeleyici kuralının adı. |
| IP sürümü | Seçenekleriniz **IPv4** veya **IPv6**.  |
| Ön uç IP adresi | Ön uç IP adresini seçin. </br> Yük dengeleyicinin yük dengeleyici kuralını ilişkilendirilmesini istediğiniz ön uç IP adresi.|
| Protokol | Azure Load Balancer, katman 4 ağ yükü dengeleyicidir. </br> Seçenekleriniz şunlardır: **TCP** veya **UDP**. |
| Bağlantı noktası | Bu ayar, trafiğin bu yük dengeleme kuralına göre dağıtılmasını istediğiniz ön uç IP 'si ile ilişkili bağlantı noktasıdır. |
| Arka uç bağlantı noktası | Bu ayar, yük dengeleyicinin trafik göndermesini istediğiniz arka uç havuzundaki örneklerde yer alan bağlantı noktasıdır. Bu ayar ön uç bağlantı noktası ile aynı veya uygulamanız için esnekliğe ihtiyacınız varsa farklı olabilir. |
| Arka uç havuzu | Bu yük dengeleyici kuralının uygulanmasını istediğiniz arka uç Havuzu. |
| Durum yoklaması | Arka uç havuzundaki örneklerin durumunu denetlemek için oluşturduğunuz sistem durumu araştırması. </br> Yalnızca sağlıklı örnekler yeni trafik alır. |
| Oturum kalıcılığı |  Seçenekleriniz şunlardır: </br> **Hiçbiri** </br> **İstemci IP'si** </br> **İstemci IP 'si ve Protokolü**</br> </br> Arka uç havuzundaki bir istemciden aynı sanal makineye giden trafiği saklayın. Bu trafik, oturum süresince tutulacaktır. </br> **Hiçbiri** , aynı istemciden gelen ardışık isteklerin herhangi bir sanal makine tarafından işlenebileceğini belirtir. </br> **ISTEMCI IP** adresi aynı istemci IP adresinden gelen ardışık isteklerin aynı sanal makine tarafından işleneceğini belirtir. </br> **İstemci IP 'si ve Protokolü** , aynı istemci IP adresinden ve protokolden gelen isteklerin aynı sanal makine tarafından işlenmesini güvence altına aldığından emin olur. </br> [Dağıtım modları](load-balancer-distribution-mode.md)hakkında daha fazla bilgi edinin. |
| Boşta kalma zaman aşımı (dakika) | Etkin tut iletileri göndermek için istemcilere bağlı olmadan bir **TCP** veya **http** bağlantısını açık tutun |  
| TCP sıfırlaması | Yük dengeleyici, bağlantı boşta kaldığında üzerinde daha öngörülebilir bir uygulama davranışı oluşturmaya yardımcı olmak için **TCP sıfırlamaları** gönderebilir. </br> [TCP sıfırlaması](load-balancer-tcp-reset.md) hakkında daha fazla bilgi edinin|
| Kayan IP | Kayan IP, **doğrudan sunucu dönüşü (DSR)** olarak bilinen bir kısmı için Azure 'un terminolojileridir. </br> DSR iki bölümden oluşur: <br> 1. Flow topolojisi </br> 2. bir platform düzeyinde bir IP adresi eşleme şeması. </br></br> Azure Load Balancer, kayan IP 'nin etkin olup olmadığı bir DSR Flow topolojisinde her zaman çalışır. </br> Bu işlem, bir akışın giden bölümünün her zaman doğrudan başlangıca akışa doğru şekilde yeniden yazılması anlamına gelir. </br> Kayan IP olmadan Azure, geleneksel bir yük dengeleme IP adresi eşleme şeması, sanal makine örneklerinin IP 'sini gösterir. </br> Kayan IP 'nin etkinleştirilmesi, ek esneklik sağlamak için IP adresi eşlemesini yük dengeleyicinin ön uç IP 'si olarak değiştirir. </br> Daha fazla bilgi için bkz. [Azure Load Balancer Için birden fazla ön uçlar](load-balancer-multivip-overview.md).|
| Örtük giden kuralları oluşturma | **Hayır**'ı seçin. </br> Varsayılan: **Disableoutboundsnat = false**  </br> Bu durumda, giden bir ön uç IP 'si ile meydana gelir. </br></br> **disableOutboundSnat = true** </br>Bu durumda giden kuralları giden için gerekir. |

:::image type="content" source="./media/manage/load-balancing-rule.png" alt-text="Yük Dengeleme kuralı ekle." border="true":::

## <a name="inbound-nat-rules"></a>Gelen NAT kuralları

Gelen NAT kuralı, ön uç IP adresine ve bağlantı noktası birleşimine gönderilen gelen trafiği iletir. 

Trafik, arka uç havuzundaki belirli bir sanal makineye veya örneğe gönderilir. Bağlantı noktası iletme, Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır.

Senaryonuz, arka uç havuzundaki sanal makine örneklerini ayırmak için Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumları gerektiriyorsa. Aynı ön uç IP adresindeki bağlantı noktalarıyla birden çok iç uç nokta eşlenebilir. 

Ön uç IP adresleri, ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanılabilir.

Yük dengeleyicisine bir gelen NAT kuralı eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **gelen NAT kuralları**' nı seçin ve **+ Ekle**' yi seçin.

| Ayar | Ayrıntılar |
| ---------- | ---------- |
| Name | Gelen NAT kuralınızın adı |
| Ön uç IP adresi | Ön uç IP adresini seçin. </br> Gelen NAT kuralını ilişkilendirmek istediğiniz yük dengeleyicinizin ön uç IP adresi. |
| IP sürümü | Seçenekleriniz IPv4 ve IPv6. |
| Hizmet | Azure Load Balancer üzerinde çalıştırdığınız hizmetin türü. </br> Burada bir seçim, bağlantı noktası bilgilerini uygun şekilde güncelleştirecek. |
| Protokol | Azure Load Balancer, katman 4 ağ yükü dengeleyicidir. </br> Seçenekleriniz şunlardır: TCP veya UDP. |
| Boşta kalma zaman aşımı (dakika) | Etkin tut iletileri göndermek için istemcilere bağlı kalmadan bir TCP veya HTTP bağlantısını açık tutun. |
| TCP sıfırlaması | Load Balancer, bağlantı boşta kaldığında üzerinde daha öngörülebilir bir uygulama davranışı oluşturmaya yardımcı olmak için TCP sıfırlamaları gönderebilir. </br> [TCP sıfırlaması](load-balancer-tcp-reset.md) hakkında daha fazla bilgi edinin |
| Bağlantı noktası | Bu ayar, trafiğin bu gelen NAT kuralına göre dağıtılmasını istediğiniz ön uç IP 'si ile ilişkili bağlantı noktasıdır. |
| Hedef sanal makine | Bu kuralın ilişkilendirilmesini istediğiniz arka uç havuzunun sanal makine parçası. |
| Bağlantı noktası eşleme | Bu ayar, uygulama tercihlerinize göre varsayılan veya özel olabilir. |

:::image type="content" source="./media/manage/inbound-nat-rule.png" alt-text="Gelen NAT kuralı ekle." border="true":::

## <a name="outbound-rules"></a>Giden kuralları

Yük dengeleyici giden kuralları arka uç havuzundaki VM 'Ler için giden SNAT 'yi yapılandırır.

Yük dengeleyicisine bir giden kuralı eklemek istiyorsanız, Azure portal yük dengeleyicisine gidin, **giden kuralları**' nı seçin ve **+ Ekle**' yi seçin.

| Ayar | Ayrıntılar |
| ------- | ------ |
| Name | Giden kuralınızın adı. |
| Ön uç IP adresi | Ön uç IP adresini seçin. </br> Giden kuralının ilişkilendirilmesini istediğiniz yük dengeleyicinizin ön uç IP adresi. |
| Protokol | Azure Load Balancer, katman 4 ağ yükü dengeleyicidir. </br> Seçenekleriniz şunlardır: **Tümü**, **TCP**veya **UDP**. |
| Boşta kalma zaman aşımı (dakika) | Etkin tut iletileri göndermek için istemcilere bağlı kalmadan bir **TCP** veya **http** bağlantısını açık tutun. |
| TCP sıfırlaması | Yük dengeleyici, bağlantı boşta kaldığında üzerinde daha öngörülebilir bir uygulama davranışı oluşturmaya yardımcı olmak için **TCP sıfırlamaları** gönderebilir. </br> [TCP sıfırlaması](load-balancer-tcp-reset.md) hakkında daha fazla bilgi edinin |
| Arka uç havuzu | Bu giden kuralın uygulanmasını istediğiniz arka uç Havuzu. |

### <a name="port-allocation"></a>Bağlantı noktası ayırma

| Ayar | Ayrıntılar |
| ------- | ------ |
| Bağlantı noktası ayırma | **Giden bağlantı noktası sayısını el ile seçmenizi**öneririz.|

### <a name="outbound-ports"></a>Giden bağlantı noktaları

| Ayar | Ayrıntılar |
| ------- | ------ |
| Seçme ölçütü | **Örnek başına bağlantı noktası** seçin |
| Örnek başına bağlantı noktası | **10.000**girin. |

:::image type="content" source="./media/manage/outbound-rule.png" alt-text="Gelen giden kuralı ekle." border="true":::

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede, Azure Load Balancer için Azure portal farklı hüküm ve ayarlar hakkında bilgi edindiniz.

* Azure Load Balancer hakkında daha fazla [bilgi edinin](./load-balancer-overview.md) .
* Azure Load Balancer için [SSS](./load-balancer-faqs.md) .