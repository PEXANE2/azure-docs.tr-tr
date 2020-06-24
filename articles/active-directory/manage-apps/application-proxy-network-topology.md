---
title: Azure AD Uygulama Ara Sunucusu için ağ topolojisi konuları
description: Azure AD Uygulama Ara Sunucusu kullanırken ağ topolojisi konularını ele alır.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: kenwith
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d814a3ce62d7e9b3ce04953e05293d17ecc08d4
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84764477"
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama Ara Sunucusu kullanırken ağ topolojisi konuları

Bu makalede, uygulamalarınızı uzaktan yayımlamak ve bunlara erişmek için Azure Active Directory (Azure AD) uygulama proxy 'Si kullanılırken ağ topolojisi konuları açıklanmaktadır.

## <a name="traffic-flow"></a>Trafik akışı

Bir uygulama Azure AD Uygulama Ara Sunucusu aracılığıyla yayımlandığında, kullanıcılardan uygulamalara olan trafik üç bağlantıyla akar:

1. Kullanıcı Azure 'da Azure AD Uygulama Ara Sunucusu hizmeti genel uç noktasına bağlanıyor
1. Uygulama proxy 'si hizmeti, uygulama proxy 'Si bağlayıcısına bağlanır
1. Uygulama proxy Bağlayıcısı, hedef uygulamaya bağlanır

![Kullanıcıdan hedef uygulamaya trafik akışını gösteren diyagram](./media/application-proxy-network-topology/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Kiracı konumu ve uygulama proxy hizmeti

Bir Azure AD kiracısına kaydolduğunuzda, kiracınızın bölgesi belirttiğiniz ülke/bölge tarafından belirlenir. Uygulama proxy 'Si etkinleştirdiğinizde, kiracınız için uygulama proxy 'Si hizmet örnekleri, Azure AD kiracınızla aynı bölgede veya en yakın bölgede oluşturulur.

Örneğin, Azure AD kiracınızın ülkesi veya Bölgesi Birleşik Krallık ise, tüm uygulama ara sunucu bağlayıcıları Avrupa veri merkezlerinde hizmet örnekleri kullanır. Kullanıcılarınız yayımlanmış uygulamalara erişirken, trafiği bu konumdaki uygulama proxy 'Si hizmet örneklerinden geçer.

## <a name="considerations-for-reducing-latency"></a>Gecikme süresini azaltma konuları

Tüm proxy çözümleri, ağ bağlantınıza gecikme sağlar. Uzaktan erişim çözümünüz olarak hangi proxy veya VPN çözümünü seçtiğiniz, her zaman kurumsal ağınız içinde bağlantı sağlayan bir sunucular kümesi içerir.

Kuruluşlar genellikle çevre ağındaki sunucu uç noktalarını içerir. Bununla birlikte Azure AD Uygulama Ara Sunucusu ile, bağlayıcılar kurumsal ağınızda yer aldığı sırada ağ üzerindeki proxy hizmeti üzerinden trafik akar. Çevre ağı gerekli değildir.

Sonraki bölümlerde, gecikme süresini daha da azaltmanıza yardımcı olacak ek öneriler bulunur. 

### <a name="connector-placement"></a>Bağlayıcı yerleşimi

Uygulama proxy 'Si, kiracı konumunuza göre sizin için örneklerin konumunu seçer. Bununla birlikte, bağlayıcıyı nereye yükleyeceğinize karar verirsiniz ve ağ trafiğinizin gecikme süresini tanımlamak için size güç kazandırın.

Uygulama proxy 'Si hizmetini ayarlarken aşağıdaki soruları sorun:

- Uygulama nerede bulunuyor?
- Uygulamaya erişen çoğu kullanıcı nerede bulunur?
- Uygulama proxy örneği nerede bulunur?
- Azure ExpressRoute veya benzer bir VPN gibi Azure veri merkezlerine ayarlanmış bir özel ağ bağlantınız zaten var mı?

Bağlayıcının hem Azure hem de uygulamalarınızla iletişim kurması gerekir (trafik akış diyagramında adım 2 ve 3), bu nedenle bağlayıcının yerleştirilmesi bu iki bağlantının gecikmesini etkiler. Bağlayıcının yerleşimini değerlendirirken aşağıdaki noktaları göz önünde bulundurun:

- Çoklu oturum açma için Kerberos kısıtlı temsilcisini (KCD) kullanmak istiyorsanız, bağlayıcının bir veri merkezine bir görüş satırı olması gerekir. Ayrıca, bağlayıcı sunucusunun etki alanına katılmış olması gerekir.  
- Şüpheli olduğunda, bağlayıcıyı uygulamaya yaklaştırın.

### <a name="general-approach-to-minimize-latency"></a>Gecikme süresini en aza indirmek için genel yaklaşım

Her ağ bağlantısını en iyi duruma getirerek uçtan uca trafiğin gecikme süresini en aza indirmenize olanak sağlayabilirsiniz. Her bağlantı şu şekilde iyileştirilebilir:

- Atlamanın iki ucu arasındaki mesafeyi azaltma.
- Geçiş yapılacak doğru ağ seçiliyor. Örneğin, genel Internet yerine özel bir ağa geçiş yapmak, adanmış bağlantılar nedeniyle daha hızlı olabilir.

Azure ile kurumsal ağınız arasında adanmış bir VPN veya ExpressRoute bağlantısı varsa, bunu kullanmak isteyebilirsiniz.

## <a name="focus-your-optimization-strategy"></a>En iyi duruma getirme stratejinizi odaklayın

Kullanıcılarınız ile uygulama proxy hizmeti arasındaki bağlantıyı denetlemek için yapabileceğiniz çok şey vardır. Kullanıcılar, uygulamalarınıza bir ev ağı, kahve dükkanı veya farklı bir ülke/bölge aracılığıyla erişebilir. Bunun yerine, uygulamalar için uygulama proxy 'si hizmetindeki bağlantıları uygulama proxy 'Si bağlayıcılarına en iyileştirebilirsiniz. Ortamınıza aşağıdaki desenleri dahil etmeyi düşünün.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>1. Model: bağlayıcıyı uygulamaya yakın yerleştirme

Bağlayıcıyı, müşteri ağındaki hedef uygulamaya yakın yere yerleştirin. Bu yapılandırma, bağlayıcı ve uygulama kapandığı için topografi diyagramında 3. adımı en aza indirir.

Bağlayıcının etki alanı denetleyicisine bir görüş satırı olması gerekiyorsa, bu model avantajlıdır. Çoğu senaryo için iyi şekilde çalıştığı için müşterilerimizin çoğu bu kalıbı kullanır. Bu model, hizmet ve bağlayıcı arasındaki trafiği iyileştirmek için de, 2. düzende birleştirilebilir.

### <a name="pattern-2-take-advantage-of-expressroute-with-microsoft-peering"></a>2. Model: Microsoft eşlemesiyle ExpressRoute avantajlarından yararlanın

ExpressRoute 'u Microsoft eşlemesi ile ayarladıysanız, uygulama proxy 'Si ile bağlayıcı arasındaki trafik için daha hızlı ExpressRoute bağlantısı kullanabilirsiniz. Bağlayıcı hala ağınızda, uygulamaya yakın.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>3. Model: özel eşleme ile ExpressRoute avantajlarından yararlanın

Azure ile kurumsal ağınız arasında özel eşleme ile ayrılmış bir VPN veya ExpressRoute ayarladıysanız, başka bir seçeneğiniz vardır. Bu yapılandırmada, Azure 'daki sanal ağ genellikle kurumsal ağın bir uzantısı olarak kabul edilir. Bu sayede bağlayıcıyı Azure veri merkezine yükleyebilir ve bağlayıcı-uygulama bağlantısının düşük gecikme süresine sahip olmaya devam edebilirsiniz.

Trafik adanmış bir bağlantı üzerinden akdığı için gecikme süresi aşılmış değildir. Ayrıca bağlayıcı, Azure AD kiracı konumunuza yakın bir Azure veri merkezinde yüklü olduğundan, geliştirilmiş uygulama proxy 'Si hizmetten bağlayıcıya gecikme süresi de alırsınız.

![Bir Azure veri merkezinde yüklü bağlayıcıyı gösteren diyagram](./media/application-proxy-network-topology/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Diğer yaklaşımlar

Bu makalenin odağı bağlayıcı yerleşimi olsa da, daha iyi gecikme süresi özellikleri almak için uygulamanın yerleşimini de değiştirebilirsiniz.

Giderek, kuruluşlar ağları barındırılan ortamlara taşıyor. Bu sayede, uygulamalarını şirket ağının parçası olan barındırılan bir ortama yerleştirmeleri ve yine de etki alanı dahilinde olması sağlanır. Bu durumda, önceki bölümlerde ele alınan desenler yeni uygulama konumuna uygulanabilir. Bu seçeneği düşünüyorsanız, bkz. [Azure AD Domain Services](../../active-directory-domain-services/overview.md).

Ayrıca, farklı konumlarda ve ağlarda bulunan uygulamaları hedeflemek için [bağlayıcı gruplarını](application-proxy-connector-groups.md) kullanarak bağlayıcılarınızı düzenlemeyi göz önünde bulundurun.

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Bu bölümde, birkaç yaygın senaryoya kılavuzluk ederiz. Azure AD kiracısının (ve dolayısıyla proxy hizmeti uç noktasının) Birleşik Devletler (US) içinde bulunduğunu varsayın. Bu kullanım durumlarında bahsedilen noktalar, dünyanın dört bir yanındaki diğer bölgelere de uygulanır.

Bu senaryolarda, her bağlantıyı bir "atlama" olarak çağırıyoruz ve daha kolay tartışmak için bu bağlantı sayısını sunuyoruz:

- **Atlama 1**: kullanıcının uygulama proxy 'si hizmeti
- **Atlama 2**: uygulama proxy 'Si hizmetine uygulama proxy 'si Bağlayıcısı
- **Atlama 3**: hedef uygulamaya uygulama proxy Bağlayıcısı 

### <a name="use-case-1"></a>Kullanım durumu 1

**Senaryo:** Uygulama, ABD 'deki bir kuruluşun ağında, aynı bölgedeki kullanıcılarla birlikte bulunur. Azure veri merkezi ve şirket ağı arasında ExpressRoute veya VPN yok.

**Öneri:** Önceki bölümde açıklanan 1. kalıbı izleyin. İyileştirilmiş gecikme için, gerekirse ExpressRoute kullanmayı düşünün.

Bu basit bir modeldir. Bağlayıcıyı uygulamanın yanına yerleştirerek atlama 3 ' ü iyileştirebilirsiniz. Bağlayıcı genellikle uygulamaya ve veri merkezine KCD işlemleri gerçekleştirmeye yönelik bir görüş satırı ile yüklendiği için bu da doğal bir seçimdir.

![Kullanıcıları, proxy, bağlayıcıyı ve uygulamayı gösteren diyagram BIZIMLE](./media/application-proxy-network-topology/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Kullanım durumu 2

**Senaryo:** Uygulama, ABD 'deki bir kuruluşun ağında, kullanıcıların küresel olarak yayılmaktadır. Azure veri merkezi ve şirket ağı arasında ExpressRoute veya VPN yok.

**Öneri:** Önceki bölümde açıklanan 1. kalıbı izleyin.

Daha sonra, bağlayıcıyı uygulamanın yanına yerleştirdiğiniz atlama 3 ' ü optimize etmek de yaygın bir modeldir. Atlama 3 genellikle aynı bölgedeyse pahalı değildir. Ancak, 1. atlama, kullanıcının olduğu yere bağlı olarak daha pahalı olabilir, çünkü dünyanın içindeki kullanıcılar, ABD 'deki uygulama proxy örneğine erişebilmelidir. Tüm proxy çözümünün, kullanıcılar tarafından genel olarak yayılmakta olduğu benzer özelliklere sahip olduğunu belirtmekte de dikkat edin.

![Kullanıcılar Global olarak yayılırlar, ancak diğer her şey ABD 'de](./media/application-proxy-network-topology/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Kullanım durumu 3

**Senaryo:** Uygulama, ABD 'deki bir kuruluşun ağında. Azure ile kurumsal ağ arasında Microsoft eşlemesi ile ExpressRoute var.

**Öneri:** Önceki bölümde açıklanan 1. ve 2. desenleri izleyin.

İlk olarak, bağlayıcıyı uygulamaya olabildiğince yakın yere yerleştirin. Ardından, sistem otomatik olarak ExpressRoute for atlama 2 ' yi kullanır.

ExpressRoute bağlantısı Microsoft eşlemesi kullanıyorsa, proxy ve bağlayıcı arasındaki trafik o bağlantının üzerinden akar. Atlama 2 ' nin en iyi duruma getirilmiş gecikmesi.

![Proxy ve bağlayıcı arasında ExpressRoute gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Kullanım durumu 4

**Senaryo:** Uygulama, ABD 'deki bir kuruluşun ağında. Azure ile kurumsal ağ arasında özel eşleme içeren ExpressRoute var.

**Öneri:** Önceki bölümde açıklanan 3. kalıbı izleyin.

Bağlayıcıyı, ExpressRoute özel eşlemesi aracılığıyla kurumsal ağa bağlı Azure veri merkezine yerleştirin.

Bağlayıcı, Azure veri merkezinde yer alabilir. Bağlayıcının uygulama ve veri merkezinde özel ağ aracılığıyla hala bir görüş satırı olduğundan, atlama 3 en iyi şekilde kalır. Ayrıca, atlama 2 daha fazla iyileştirilmiştir.

![Azure veri merkezinde bağlayıcı, bağlayıcı ve uygulama arasında ExpressRoute](./media/application-proxy-network-topology/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Kullanım durumu 5

**Senaryo:** Uygulama, uygulama proxy 'Si örneği ve ABD 'deki çoğu kullanıcı ile Avrupa 'daki bir kuruluşun ağında bulunur.

**Öneri:** Bağlayıcıyı uygulamanın yanına yerleştirin. ABD kullanıcıları aynı bölgede yer alan bir uygulama proxy örneğine eriştiğinden, atlama 1 çok pahalı değildir. Atlama 3 en iyi duruma getirilmiştir. Atlama 2 ' i iyileştirmek için ExpressRoute kullanmayı düşünün.

![Diyagram, Avrupa 'daki ABD, bağlayıcı ve uygulamadaki kullanıcıları ve proxy 'yi gösterir](./media/application-proxy-network-topology/application-proxy-pattern5b.png)

Ayrıca, bu durumda başka bir değişken kullanmayı da düşünebilirsiniz. Kuruluştaki çoğu kullanıcı ABD 'de yer alıyorsa, ağınız da bıze genişlemektedir. Bağlayıcıyı ABD 'ye yerleştirin ve şirket içi kurumsal ağ hattını Avrupa 'daki uygulamaya kullanın. Bu şekilde atlama 2 ve 3 en iyi duruma getirilmiştir.

![Diyagramda, Avrupa 'daki uygulamalar, ABD 'deki kullanıcılar, proxy ve bağlayıcı gösterilmektedir](./media/application-proxy-network-topology/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
- [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koşullu erişimi etkinleştir](application-proxy-integrate-with-sharepoint-server.md)
- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
