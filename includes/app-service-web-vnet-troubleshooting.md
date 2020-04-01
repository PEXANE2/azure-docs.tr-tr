---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: 652d42d6e2d9e909c3a03bd82a3a36f91bc73807
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419533"
---
Bu özelliğin kurulumu kolaydır, ancak bu deneyiminizin sorunsuz olacağı anlamına gelmez. İstediğiniz bitiş noktasına erişmede sorun yaşıyorsanız, uygulama konsolundan bağlantı test etmek için kullanabileceğiniz bazı yardımcı programlar vardır. Kullanabileceğiniz iki konsol vardır. Bunlardan biri Kudu konsolu, diğeri ise Azure portalındaki konsoldur. Uygulamanızdan Kudu konsoluna ulaşmak için **Tools** > **Kudu'ya**gidin. Kudo konsoluna [sitename].scm.azurewebsites.net adresinden de ulaşabilirsiniz. Web sitesi yüklendikten sonra **Hata Ayıklama konsolu** sekmesine gidin. Uygulamanızdan Azure portal barındırılan konsola ulaşmak için **Araçlar** > **Konsolu'na**gidin.

#### <a name="tools"></a>Araçlar
Ping **,** **nslookup**ve **tracert** araçları güvenlik kısıtlamaları nedeniyle konsol da çalışmaz. Boşluğu doldurmak için iki ayrı araç eklenir. DNS işlevselliğini test etmek için **nameresolver.exe**adlı bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Uygulamanızın bağlı olduğu ana bilgisayar adlarını denetlemek için ad çözümleyicisini kullanabilirsiniz. Bu şekilde, DNS'nizle yanlış yapılandırılmış bir şeyiniz olup olmadığını veya DNS sunucunuza erişiminiz olup olmadığını test edebilirsiniz. Uygulamanızın konsolda kullandığı DNS sunucusunu WEBSITE_DNS_SERVER ve WEBSITE_DNS_ALT_SERVER çevresel değişkenlere bakarak görebilirsiniz.

TCP bağlantısıiçin ana bilgisayar ve bağlantı noktası birleşimini sınamak için bir sonraki aracı kullanabilirsiniz. Bu araç **tcpping** denir ve sözdizimi:

    tcpping.exe hostname [optional: port]

**Tcpping** programı, belirli bir ana bilgisayara ve bağlantı noktasına ulaşıp ulaşamayacağınızı söyler. Başarıyı yalnızca ana bilgisayar ve bağlantı noktası birleşimini dinleyen bir uygulama varsa ve uygulamanızdan belirtilen ana bilgisayar ve bağlantı noktasına ağ erişimi varsa gösterilebilir.

#### <a name="debug-access-to-virtual-network-hosted-resources"></a>Sanal ağ barındırılan kaynaklara hata ayıklama erişimi
Uygulamanızın belirli bir ana bilgisayara ve bağlantı noktasına ulaşmasını engelleyebilir. Çoğu zaman bunlardan biri:

* **Bir güvenlik duvarı yolda.** Önünüzde bir güvenlik duvarı varsa, TCP zaman ağını vurursunuz. Bu durumda TCP zaman süresi 21 saniyedir. Bağlantıyı test etmek için **tpping** aracını kullanın. TCP zaman zaman ları güvenlik duvarlarının ötesindeki birçok şeyden kaynaklanabilir, ancak buradan başlayın.
* **DNS'ye erişilemez.** DNS zaman dilimi, DNS sunucusu başına 3 saniyedir. İki DNS sunucunuz varsa, zaman anına 6 saniye dir. DNS'nin çalışıp çalışmamasını görmek için ad çözümleyicisini kullanın. Sanal ağınızın yapılandırıldığı DNS'yi kullanmadığından nslookup kullanamazsınız. Erişilemiyorsanız, DNS'ye erişimi engelleyen bir güvenlik duvarınız veya NSG'niz olabilir veya kapalı olabilir.

Bu öğeler sorunlarınızı yanıtlamazsa, ilk olarak aşağıdaki gibi şeyler için bakın:

**Bölgesel VNet Entegrasyonu**
* Varış noktanız RFC1918 adresi olmayan bir adres mive 1 olarak ayarlanmış WEBSITE_VNET_ROUTE_ALL yok mu?
* Entegrasyon alt ağınızdan çıkış engelleyen bir NSG var mı?
* Azure ExpressRoute veya VPN üzerinden geçiyorsanız, şirket içi ağ geçidiniz trafiği Azure'a yönlendirecek şekilde yapılandırıldı mı? Sanal ağınızda uç noktalara ulaşabiliyor, ancak şirket içinde değilseniz, rotalarınızı kontrol edin.
* Tümleştirme alt ağına delegasyon ayarlamak için yeterli izniniz var mı? Bölgesel VNet Tümleştirme yapılandırması sırasında, tümleştirme alt ağınız Microsoft.Web'e devredilir. VNet Tümleştirme UI alt ağı otomatik olarak Microsoft.Web'e verir. Hesabınızda temsilci belirlemek için yeterli ağ izni yoksa, alt ağı nızı devretmek için tümleştirme alt netiniz üzerinde öznitelikleri ayarlayacak birine ihtiyacınız vardır. Tümleştirme alt netini el ile devretmek için Azure Sanal Ağ alt ağı web sitesine gidin ve microsoft.web için delegasyonu ayarlayın.

**Ağ geçidi gerekli VNet Entegrasyonu**
* RFC 1918 aralıklarında (10.0.0.0-10.255.255.255 / 172.16.0.0-172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) adres aralığı var mıdır?
* Geçit geçitte olduğu mu gibi mi? Eğer geçidiniz aşağıdaysa, geri getirin.
* Sertifikalar eşitlenmiş olarak mı gösteriliyor, yoksa ağ yapılandırmasının değiştiğinden mi şüpheleniyorsunuz?  Sertifikalarınız eşitlenmemişse veya SANAL Ağ yapılandırmanızda ASP'lerinizle eşitlenmemiş bir değişiklik yapıldığından şüpheleniyorsanız, **Eşitleme Ağı'nı**seçin.
* VPN üzerinden geçiyorsanız, şirket içi ağ geçidi trafiği Azure'a yönlendirecek şekilde yapılandırıldı mı? Sanal ağınızda uç noktalara ulaşabiliyor, ancak şirket içinde değilseniz, rotalarınızı kontrol edin.
* Hem siteye hem de ExpressRoute'u destekleyen bir birlikte yaşam ağ geçidi mi kullanmaya çalışıyorsunuz? Birlikte yaşam ağ geçitleri VNet Tümleştirme si ile desteklenmez.

Ağ hata ayıklama sorunları, belirli bir ana bilgisayar:bağlantı noktası birleşimine erişimi engelleyen şeyi göremediğiniz için bir sorundur. Bazı nedenler şunlardır:

* Ana bilgisayarınızda, uygulama bağlantı noktasına sayfa-to-site IP aralığınızdan erişimi engelleyen bir güvenlik duvarı vardır. Alt ağları geçmek genellikle genel erişim gerektirir.
* Hedef ev sahibiniz düştü.
* Başvurun düştü.
* Yanlış IP veya hostname vardı.
* Uygulamanız beklediğinizden farklı bir bağlantı noktasında dinliyor. Bitiş noktası ana bilgisayarında "netstat -aon" kullanarak işlem kimliğinizi dinleme portuyla eşleştirebilirsiniz.
* Ağ güvenlik gruplarınız, uygulama ana bilgisayarınıza ve bağlantı noktanıza bağlantı noktası IP aralığınızdan erişimi engelleyecek şekilde yapılandırılır.

Uygulamanızın gerçekte hangi adresi kullandığını bilmiyorsun. Tümleştirme alt ağı veya noktaya nokta adres aralığındaherhangi bir adres olabilir, bu nedenle tüm adres aralığından erişime izin verebilirsiniz.

Ek hata ayıklama adımları şunlardır:

* Sanal ağınızdaki bir VM'ye bağlanın ve kaynak ana bilgisayarınız olan bağlantı noktasına oradan ulaşmaya çalışın. TCP erişimini test etmek için PowerShell komut **test-netconnection'ı**kullanın. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* Bir VM üzerinde bir uygulama getirin ve **tpping**kullanarak uygulamakonsoldan bu ana bilgisayar ve bağlantı noktası erişim test .

#### <a name="on-premises-resources"></a>Şirket içi kaynaklar ####

Uygulamanız bir kaynağa şirket içinde ulaşamazsa, kaynağa sanal ağınızdan erişip erişemeyeceğinizi kontrol edin. TCP erişimini kontrol etmek için **test ağı bağlantısı** PowerShell komutunu kullanın. VM'niz şirket içi kaynağınıza ulaşamazsa, VPN veya ExpressRoute bağlantınız düzgün şekilde yapılandırılamayabilir.

Sanal ağ barındırılan VM'niz şirket içi sisteminize erişebiliyorsa ancak uygulamanız erişemiyorsa, bunun nedeni büyük olasılıkla aşağıdaki nedenlerden biridir:

* Rotalarınız şirket içi ağ geçidinizdeki alt ağınızla veya noktadan siteye adres aralıklarınızla yapılandırılmamıştır.
* Ağ güvenlik gruplarınız, noktadan noktaya IP aralığınıza erişimi engelliyor.
* Şirket içi güvenlik duvarlarınız, noktadan siteye IP aralığınızdan gelen trafiği engelliyor.
* Bölgesel VNet Tümleştirme özelliğini kullanarak RFC olmayan bir adrese ulaşmaya çalışıyorsunuz.