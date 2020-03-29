---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: 2d2a82552a846cedfa5da3bb6ec6df8a40b67732
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671543"
---
Özelliğin kurulumu kolay olsa da, bu deneyiminizin sorunsuz olacağı anlamına gelmez. İstediğiniz bitiş noktasına erişen sorunlarla karşılaşırsanız, uygulama konsolundan bağlantı test etmek için kullanabileceğiniz bazı yardımcı programlar vardır. Kullanabileceğiniz iki konsol vardır. Bunlardan biri Kudu konsolu, diğeri ise Azure portalındaki konsoldur. Uygulamanızdan Kudu konsoluna ulaşmak için Araçlar -> Kudu'ya gidin. Kudo konsoluna [sitename].scm.azurewebsites.net adresinden de ulaşabilirsiniz. Web sitesi yüklendikten sonra Hata Ayıklama konsolu sekmesine gidin. Azure portal barındırılan konsola ulaşmak için uygulamanızdan Araçlar -> Konsolu'na gidin. 

#### <a name="tools"></a>Araçlar
Ping **,** **nslookup**ve **tracert** araçları güvenlik kısıtlamaları nedeniyle konsol da çalışmaz. Boşluğu doldurmak için iki ayrı araç eklendi. DNS işlevselliğini test etmek için nameresolver.exe adında bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Uygulamanızın bağlı olduğu ana bilgisayar adlarını denetlemek için **ad çözümleyicisini** kullanabilirsiniz. Bu şekilde, DNS'nizle yanlış yapılandırılmış bir şeyiniz olup olmadığını veya dns sunucunuza erişiminiz olup olmadığını test edebilirsiniz. Uygulamanızın konsolda kullanacağı DNS sunucusunu WEBSITE_DNS_SERVER ve WEBSITE_DNS_ALT_SERVER çevresel değişkenlere bakarak görebilirsiniz.

Bir sonraki araç, tcp bağlantısı için bir ana bilgisayar ve bağlantı noktası kombinasyonu için test etmenizi sağlar. Bu araç **tcpping** denir ve sözdizimi:

    tcpping.exe hostname [optional: port]

**Tcpping** programı, belirli bir ana bilgisayara ve bağlantı noktasına ulaşıp ulaşamayacağınızı söyler. Yalnızca başarıyı gösterebilir: ana bilgisayar ve bağlantı noktası birleşimini dinleyen bir uygulama varsa ve uygulamanızdan belirtilen ana bilgisayar ada ve bağlantı noktasına ağ erişimi varsa.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>VNet barındırılan kaynaklara erişim hata ayıklama
Uygulamanızın belirli bir ana bilgisayara ve bağlantı noktasına ulaşmasını engelleyebilir birkaç şey vardır. Çoğu zaman üç şeyden biridir:

* **Bir güvenlik duvarı yolda.** Önünüzde bir güvenlik duvarı varsa, TCP zaman alacaktır. Bu durumda TCP zaman süresi 21 saniyedir. Bağlantıyı test etmek için **tpping** aracını kullanın. TCP zaman zaman ları güvenlik duvarlarının ötesindeki birçok şeyden kaynaklanabilir, ancak buradan başlar. 
* **DNS'ye erişilemez.** DNS zaman dilimi, DNS sunucusu başına üç saniyedir. İki DNS sunucunuz varsa, zaman anına 6 saniye dir. DNS'nin çalışıp çalışmamasını görmek için ad çözümleyicisini kullanın. VNet'inizin yapılandırdığınız DNS'yi kullanmadığı için nslookup'ı kullanamayacağınızı unutmayın. Erişilemiyorsanız, DNS'ye erişimi engelleyen bir güvenlik duvarınız veya NSG'niz olabilir veya kapalı olabilir.

Bu öğeler sorunlarınızı yanıtlamazsa, ilk olarak aşağıdaki gibi şeyler için bakın: 

**bölgesel VNet Entegrasyonu**
* gideceğiniz yer RFC1918 adresi olmayan bir adres ve 1 olarak ayarlanmış WEBSITE_VNET_ROUTE_ALL yok mu?
* Entegrasyon alt netinden çıkış engelleyen bir NSG var mı?
* ExpressRoute veya VPN üzerinden gidiyorsanız, şirket içi ağ geçidiniz trafiği Azure'a yönlendirecek şekilde yapılandırılır mı? VNet'inizde uç noktalara ulaşabilir, ancak şirket içinde değil, rotalarınızı kontrol edin.
* Tümleştirme alt ağına delegasyon ayarlamak için yeterli izniniz var mı? Bölgesel VNet Tümleştirme yapılandırması sırasında, tümleştirme alt ağınız Microsoft.Web'e devredilecektir. VNet Tümleştirme UI alt ağı otomatik olarak Microsoft.Web'e devredecektir. Hesabınızda temsilci belirlemek için yeterli ağ izni yoksa, alt ağınızı devretmek için tümleştirme alt netiniz üzerinde öznitelikleri ayarlayacak birine ihtiyacınız olacaktır. Tümleştirme alt netini el ile devretmek için Azure Sanal Ağ alt ağı web sitesine gidin ve Microsoft.Web için delegasyon ayarlayın. 

**ağ geçidi gerekli VNet Entegrasyonu**
* RFC 1918 aralıklarında yer nokta adres aralığı (10.0.0.0-10.255.255.255 / 172.16.0.0-172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255)?
* Geçit geçitte mi? Eğer geçidiniz aşağıdaysa, geri getirin.
* Sertifikalar eşitlenmiş olarak mı gösteriliyor yoksa ağ yapılandırmasının değiştiğinden mi şüpheleniyorsunuz?  Sertifikalarınız eşitlenmemişse veya VNet yapılandırmanızda ASP'lerinizle eşitlenmemiş bir değişiklik yapıldığından şüpheleniyorsanız, "Eşitleme Ağı"na basın.
* VPN üzerinden geçiyorsanız, şirket içi ağ geçidi trafiği Azure'a yönlendirecek şekilde yapılandırılır mı? VNet'inizde uç noktalara ulaşabilir, ancak şirket içinde değil, rotalarınızı kontrol edin.
* Hem siteye hem de ExpressRoute'u destekleyen bir birlikte yaşam ağ geçidi mi kullanmaya çalışıyorsunuz? Birlikte yaşam ağ geçitleri VNet Tümleştirmesi ile desteklenmez.

Ağ hata ayıklama sorunları bir meydan okumadır, çünkü orada belirli bir ana bilgisayar:bağlantı noktası birleşimine erişimi engelleyen şeyi göremezsiniz. Nedenlerden bazıları şunlardır:

* Ana bilgisayarınızda, uygulama bağlantı noktasınıza erişimin izini sizin noktanızdan site IP aralığına kadar engelleyen bir güvenlik duvarı nız vardır. Alt ağları geçmek genellikle Genel erişim gerektirir.
* Hedef ev sahibiniz düştü.
* Başvurun düştü.
* Yanlış IP veya hostname vardı.
* Uygulamanız beklediğinizden farklı bir bağlantı noktasında dinliyor. Bitiş noktası ana bilgisayarında "netstat -aon" kullanarak işlem kimliğinizi dinleme portuyla eşleştirebilirsiniz. 
* Ağ güvenlik gruplarınız, uygulama ana bilgisayarınıza ve bağlantı noktanıza, site IP aralığınıza erişimi engelleyecek şekilde yapılandırılır.

Uygulamanızın gerçekte hangi adresi kullanacağını bilmediğiniz unutmayın. Tümleştirme alt ağı veya noktaya nokta adres aralığındaherhangi bir adres olabilir, bu nedenle tüm adres aralığından erişime izin verebilirsiniz. 

Ek hata ayıklama adımları şunlardır:

* VNet'inizde bir VM'ye bağlanın ve kaynak ana bilgisayarınız olan bağlantı noktasına oradan ulaşmaya çalışın. TCP erişimini test etmek için PowerShell komut **test-netconnection'ı**kullanın. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* VM'de bir uygulama getirin ve bu ana bilgisayara ve bağlantı noktasına uygulamanızdan **tcpping** kullanarak erişim testi yapın

#### <a name="on-premises-resources"></a>Şirket içi kaynaklar ####

Uygulamanız bir kaynağa şirket içinde ulaşamıyorsa, kaynağa VNet'inizden erişip erişemeyeceğinikontrol edin. TCP erişimini kontrol etmek için **test ağı bağlantısı** PowerShell komutunu kullanın. VM'niz şirket içi kaynağınıza ulaşamazsa, VPN veya ExpressRoute bağlantınız düzgün şekilde yapılandırılamayabilir.

VNet barındırılan VM'niz şirket içi sisteminize ulaşabiliyorsa ancak uygulamanız ulaşamıyorsa, bunun nedeni büyük olasılıkla aşağıdaki nedenlerden biridir:

* Rotalarınız, şirket içi ağ geçidinizdeki alt ağınızla veya nokta yla site adresi aralıklarıyla yapılandırılmaz.
* Ağ güvenlik gruplarınız, Site'den Siteye IP aralığınıza erişimi engelliyor.
* Şirket içi güvenlik duvarlarınız, Site'ye Açılan IP aralığınızdan gelen trafiği engelliyor.
* Bölgesel VNet Tümleştirme özelliğini kullanarak RFC olmayan bir adrese ulaşmaya çalışıyorsunuz.