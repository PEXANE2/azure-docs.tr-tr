---
title: Azure AD Uygulama Proxy için ağ topolojisi hususları
description: Azure AD Application Proxy'yi kullanırken ağ topolojisi hususlarını kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: eaceaf1f5e9b6e34ced5db39b61e607fffcb5953
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295129"
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy kullanırken ağ topolojisi hususları

Bu makalede, uygulamalarınızı uzaktan yayımlama ve bunlara erişmek için Azure Active Directory (Azure AD) Uygulama Proxy'si kullanırken ağ topolojisi hususları açıklanmaktadır.

## <a name="traffic-flow"></a>Trafik akışı

Bir uygulama Azure AD Application Proxy üzerinden yayımlandığında, kullanıcılardan uygulamalara olan trafik üç bağlantıüzerinden akar:

1. Kullanıcı Azure'daki Azure AD Uygulama Proxy hizmetinin genel bitiş noktasına bağlanır
1. Application Proxy hizmeti, Application Proxy konektörüne bağlanır
1. Application Proxy bağlayıcısı hedef uygulamaya bağlanır

![Kullanıcıdan hedef uygulamaya trafik akışını gösteren diyagram](./media/application-proxy-network-topology/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Kiracı konumu ve Uygulama Proxy hizmeti

Azure AD kiracısı için kaydolduğunuzda, kiracınızın bölgesi belirttiğiniz ülke/bölge tarafından belirlenir. Uygulama Proxy'sini etkinleştirdiğinizde, kiracınızın Uygulama Proxy hizmeti örnekleri Azure AD kiracınızla veya en yakın bölgeyle aynı bölgede seçilir veya oluşturulur.

Örneğin, Azure AD kiracınızın ülkesi veya bölgesi Birleşik Krallık ise, tüm Uygulama Proxy bağlayıcılarınız Avrupa veri merkezlerinde hizmet örneklerini kullanır. Kullanıcılarınız yayımlanmış uygulamalara erişiğinde, trafiği bu konumdaki Uygulama Proxy hizmet örneklerinden geçer.

## <a name="considerations-for-reducing-latency"></a>Gecikme süresini azaltmak için dikkat edilmesi gerekenler

Tüm proxy çözümleri ağ bağlantınıza gecikme gecikmesi getirir. Uzaktan erişim çözümünüz olarak hangi proxy veya VPN çözümünün seçtiğini zedelemeniz, her zaman şirket ağınızın içine bağlantı sağlayan bir sunucu kümesi içerir.

Kuruluşlar genellikle çevre ağında sunucu uç noktaları içerir. Ancak Azure AD Application Proxy ile, bağlayıcılar şirket ağınızda yaşarken trafik buluttaki proxy hizmetinden akar. Çevre ağına gerek yoktur.

Sonraki bölümler, gecikme süresini daha da azaltmanıza yardımcı olacak ek öneriler içerir. 

### <a name="connector-placement"></a>Bağlayıcı yerleşimi

Uygulama Proxy, kiracı konumunuza bağlı olarak sizin için örneklerin konumunu seçer. Ancak, ağ trafiğinizin gecikme özelliklerini tanımlama gücü vererek bağlayıcıyı nereye yükleyeceğine siz karar verirsiniz.

Uygulama Proxy hizmetini ayarlarken aşağıdaki soruları sorun:

- Uygulama nerede bulunuyor?
- Uygulamaya erişen çoğu kullanıcı nerede bulunur?
- Uygulama Proxy örneği nerede bulunur?
- Azure ExpressRoute veya benzer bir VPN gibi Ayarlanmış Azure veri merkezlerine özel bir ağ bağlantınız var mı?

Bağlayıcının hem Azure ile hem de uygulamalarınızla (Trafik akış diyagramındaki adım 2 ve 3) iletişim kurması gerekmektedir, bu nedenle bağlayıcının yerleşimi bu iki bağlantının gecikmesini etkiler. Bağlayıcının yerleşimini değerlendirirken aşağıdaki noktaları göz önünde bulundurun:

- Tek oturum açma için Kerberos kısıtlı delegasyonu (KCD) kullanmak istiyorsanız, bağlayıcının bir veri merkezine görüş hattı na ihtiyacı vardır. Ayrıca, bağlayıcı sunucuetki alanı katıldı olması gerekir.  
- Şüpheye düştüğünüzde, konektörü uygulamaya daha yakın tonuyla tö

### <a name="general-approach-to-minimize-latency"></a>Gecikmeyi en aza indirmek için genel yaklaşım

Her ağ bağlantısını en iyi duruma alarak uçtan uca trafiğin gecikmesini en aza indirebilirsiniz. Her bağlantı şu şekilde optimize edilebilir:

- Atlamanın iki ucu arasındaki mesafeyi azaltmak.
- Geçmek için doğru ağı seçme. Örneğin, özel bağlantılar nedeniyle, genel Internet yerine özel bir ağa geçiş yapmak daha hızlı olabilir.

Azure ile kurumsal ağınız arasında özel bir VPN veya ExpressRoute bağlantınız varsa, bunu kullanmak isteyebilirsiniz.

## <a name="focus-your-optimization-strategy"></a>Optimizasyon stratejinizi odakla

Kullanıcılarınızla Uygulama Proxy hizmeti arasındaki bağlantıyı denetlemek için yapabileceğiniz çok az şey vardır. Kullanıcılar uygulamalarınız için bir ev ağından, kafeden veya farklı bir ülkeden/bölgeden erişebilir. Bunun yerine, Uygulamalar Proxy hizmetinden uygulamalara uygulama proxy bağlayıcılarına kadar olan bağlantıları optimize edebilirsiniz. Aşağıdaki desenleri ortamınızda birleştirmeyi düşünün.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>Desen 1: Konektörü uygulamaya yakın bir yere koyun

Konektörü müşteri ağında hedef uygulamaya yakın yerleştirin. Konektör ve uygulama yakın olduğundan, bu yapılandırma topografya diyagramında 3.

Bağlayıcınızın etki alanı denetleyicisine bir görüş hattı na ihtiyacı varsa, bu desen avantajlıdır. Müşterilerimizin çoğu bu deseni kullanır, çünkü çoğu senaryoiçin iyi çalışır. Bu desen, hizmet ve bağlayıcı arasındaki trafiği optimize etmek için desen 2 ile de birleştirilebilir.

### <a name="pattern-2-take-advantage-of-expressroute-with-microsoft-peering"></a>Desen 2: Microsoft'un bakışlarıyla ExpressRoute'dan yararlanın

ExpressRoute, Microsoft'un bakışlarıyla ayarlanmışsa, Application Proxy ile konektör arasındaki trafik için daha hızlı ExpressRoute bağlantısını kullanabilirsiniz. Konektör hala ağınızda, uygulamaya yakın.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Desen 3: ExpressRoute'dan özel bakışlarla yararlanın

Azure ile kurumsal ağınız arasında özel bir bakışla ayarlanmış özel bir VPN veya ExpressRoute'unuz varsa, başka bir seçeneğiniz vardır. Bu yapılandırmada, Azure'daki sanal ağ genellikle şirket ağının bir uzantısı olarak kabul edilir. Böylece bağlayıcıyı Azure veri merkezine yükleyebilir ve yine de konektörden uygulamaya bağlantının düşük gecikme gereksinimlerini karşılayabilirsiniz.

Trafik özel bir bağlantı üzerinden aktığı için gecikme den ödün verilmez. Bağlayıcı Azure AD kiracı konumunuza yakın bir Azure veri merkezine yüklendiğinden, uygulama proxy'den bağlayıcıya gecikme süresi de artırılır.

![Azure veri merkezi içinde yüklü olan bağlayıcıyı gösteren diyagram](./media/application-proxy-network-topology/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Diğer yaklaşımlar

Bu makalenin odak noktası bağlayıcı yerleşim olsa da, daha iyi gecikme süresi özellikleri elde etmek için uygulamanın yerleşimini de değiştirebilirsiniz.

Kuruluşlar, ağlarını barındırılan ortamlara giderek daha fazla taşıyor. Bu, uygulamalarını aynı zamanda şirket ağlarının bir parçası olan ve hala etki alanı içinde olan barındırılan bir ortama yerleştirmelerini sağlar. Bu durumda, önceki bölümlerde tartışılan desenler yeni uygulama konumuna uygulanabilir. Bu seçeneği düşünüyorsanız, Azure [AD Etki Alanı Hizmetleri'ne](../../active-directory-domain-services/overview.md)bakın.

Ayrıca, bağlayıcılarınızı farklı konumlarda ve ağlarda bulunan uygulamaları hedeflemek için [bağlayıcı gruplarını](application-proxy-connector-groups.md) kullanarak düzenlemeyi düşünün.

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Bu bölümde, birkaç yaygın senaryoüzerinden yürüyoruz. Azure AD kiracısının (ve bu nedenle proxy hizmet bitiş noktasının) ABD'de (ABD) bulunduğunu varsayalım. Bu kullanım durumlarında tartışılan hususlar dünya çapındaki diğer bölgeler için de geçerlidir.

Bu senaryolar için, her bağlantıya "atlama" diyoruz ve daha kolay bir tartışma için numaralandırmak:

- **Hop 1**: Uygulama Proxy hizmetine kullanıcı
- **Hop 2**: Uygulama Proxy konektörüne Uygulama Proxy hizmeti
- **Atlama 3**: Hedef uygulamaya Uygulama Proxy konektörü 

### <a name="use-case-1"></a>Büyük/küçük harf kullanımı 1

**Senaryo:** Uygulama, aynı bölgedeki kullanıcılarla birlikte ABD'deki bir kuruluşun ağında dır. Azure veri merkezi ile şirket ağı arasında ExpressRoute veya VPN yoktur.

**Tavsiye:** Önceki bölümde açıklanan desen 1'i izleyin. Daha iyi gecikme süresi için gerekirse ExpressRoute'u kullanmayı düşünün.

Bu basit bir model. Konektörü uygulamanın yakınına yerleştirerek hop 3'u optimize elersiniz. Konektör genellikle kcd işlemlerini gerçekleştirmek için uygulamaya ve veri merkezine görüş hattı yla yüklendiğinden, bu da doğal bir seçimdir.

![Kullanıcıların, proxy'nin, bağlayıcının ve uygulamanın tümü ABD'de olduğunu gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Kullanım örneği 2

**Senaryo:** Uygulama, kullanıcıların küresel olarak yayıldığı, ABD'deki bir kuruluşun ağında dır. Azure veri merkezi ile şirket ağı arasında ExpressRoute veya VPN yoktur.

**Tavsiye:** Önceki bölümde açıklanan desen 1'i izleyin.

Yine, ortak desen, bağlayıcıyı uygulamanın yakınına yerleştirdiğiniz hop 3'e optimize etmektir. Hop 3 genellikle pahalı değildir, hepsi aynı bölge içinde yse. Ancak, dünya çapındaki kullanıcıların ABD'deki Uygulama Proxy örneğine erişmesi gerektiğinden, kullanıcının nerede olduğuna bağlı olarak hop 1 daha pahalı olabilir. Herhangi bir proxy çözüm kullanıcıların küresel olarak yayılıyor ile ilgili benzer özelliklere sahip olduğunu kayda değer.

![Kullanıcılar küresel olarak yayılır, ancak geri kalan her şey ABD'de](./media/application-proxy-network-topology/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Kullanım örneği 3

**Senaryo:** Uygulama, ABD'deki bir kuruluşun ağındadır. Microsoft'un bakışlı ExpressRoute, Azure ile şirket ağı arasında mevcuttur.

**Tavsiye:** Önceki bölümde açıklanan 1 ve 2 desenlerini izleyin.

İlk olarak, konektörü uygulamaya mümkün olduğunca yakın yerleştirin. Daha sonra sistem, hop 2 için ExpressRoute'u otomatik olarak kullanır.

ExpressRoute bağlantısı Microsoft'un eşlebatını kullanıyorsa, proxy ve bağlayıcı arasındaki trafik bu bağlantı üzerinden akar. Hop 2 gecikmeyi optimize etti.

![Proxy ve bağlayıcı arasında ExpressRoute gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Kullanım örneği 4

**Senaryo:** Uygulama, ABD'deki bir kuruluşun ağındadır. Azure ve şirket ağı arasında özel bir bakış içeren ExpressRoute bulunur.

**Tavsiye:** Önceki bölümde açıklanan desen 3'u izleyin.

ExpressRoute özel bakışları aracılığıyla konektörü şirket ağına bağlı Azure veri merkezine yerleştirin.

Bağlayıcı, Azure veri merkezine yerleştirilebilir. Konektör hala özel ağ üzerinden uygulama ve veri merkezi görüş hattı olduğundan, hop 3 optimize kalır. Buna ek olarak, hop 2 daha optimize edilir.

![Azure veri merkezinde bağlayıcı, bağlayıcı ve uygulama arasında ExpressRoute](./media/application-proxy-network-topology/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Kullanım örneği 5

**Senaryo:** Uygulama, Application Proxy örneği ve ABD'deki çoğu kullanıcıyla birlikte Avrupa'daki bir kuruluşun ağındadır.

**Tavsiye:** Bağlayıcıyı uygulamanın yakınına yerleştirin. ABD kullanıcıları aynı bölgede bulunan bir Uygulama Proxy örneğine erişinden, atlama 1 çok pahalı değildir. Hop 3 optimize edidir. Hop 2'yi optimize etmek için ExpressRoute'u kullanmayı düşünün.

![Diyagram, Abd'deki kullanıcıları ve proxy'yi Avrupa'daki bağlayıcıyı ve uygulamayı gösterir](./media/application-proxy-network-topology/application-proxy-pattern5b.png)

Bu durumda başka bir varyant kullanmayı da düşünebilirsiniz. Kuruluştaki kullanıcıların çoğu ABD'deyse, ağınızın ABD'ye de uzanması olasılığı yüksektir. Konektörü ABD'ye yerleştirin ve Avrupa'daki uygulamaya özel dahili kurumsal ağ hattını kullanın. Bu şekilde hops 2 ve 3 optimize edilir.

![Diyagram, ABD'deki kullanıcıları, proxy'yi ve bağlayıcıyı Avrupa'da uygulama olarak gösterir](./media/application-proxy-network-topology/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
- [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koşullu Erişimi Etkinleştir](application-proxy-integrate-with-sharepoint-server.md)
- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
