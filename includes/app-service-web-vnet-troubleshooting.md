---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: 2d2a82552a846cedfa5da3bb6ec6df8a40b67732
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671543"
---
Özelliği kolayca ayarlanabilir, bu da deneyiminizin ücretsiz olacağı anlamına gelmez. İstenen uç noktanıza erişmede sorun yaşamanız gerekir, bu, uygulama konsolundan bağlantıyı test etmek için kullanabileceğiniz bazı yardımcı programlar vardır. Kullanabileceğiniz iki konsol vardır. Birisi kudu konsoludur ve diğeri Azure portal konsoludur. Uygulamanızın kudu konsoluna ulaşmak için Araçlar-> kudu ' ye gidin. Ayrıca, Kudo konsoluna [SiteName]. scm. azurewebsites. net adresinden ulaşabilirsiniz. Web sitesi yüklendiğinde, hata ayıklama konsolu sekmesine gidin. Daha sonra Azure portal barındırılan konsola ulaşmak için Araçlar-> konsoluna gidin. 

#### <a name="tools"></a>Araçlar
**Ping**, **nslookup**ve **tracert** araçları, güvenlik kısıtlamaları nedeniyle konsolda çalışmaz. Void 'yi dolduracak şekilde iki ayrı araç eklenmiştir. DNS işlevselliğini test etmek için, nameresolver. exe adlı bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Uygulamanızın bağımlı olduğu ana bilgisayar adlarını denetlemek için **nameresolver** kullanabilirsiniz. Bu şekilde, DNS 'niz ile yanlış yapılandırılmış herhangi bir şey varsa veya belki DNS sunucunuza erişiminiz yoksa test edebilirsiniz. Uygulamanızın konsolda kullanacağı DNS sunucusunu, WEBSITE_DNS_SERVER ve WEBSITE_DNS_ALT_SERVER ortam değişkenlerine bakarak görebilirsiniz.

Sonraki araç, bir konak ve bağlantı noktası birleşimine TCP bağlantısı için test etmenizi sağlar. Bu araca, **tcma** adı verilir ve sözdizimi şöyledir:

    tcpping.exe hostname [optional: port]

**Tcter** yardımcı programı, belirli bir konağa ve bağlantı noktasına ulaşabilseniz size bildirir. Yalnızca şu durumlarda başarı gösterebilir: konak ve bağlantı noktası birleşimine dinleme yapan bir uygulama varsa ve uygulamanızdan belirtilen konağa ve bağlantı noktasına ağ erişimi varsa.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>VNet barındırılan kaynaklara erişim hatası ayıklama
Uygulamanızın belirli bir konağa ve bağlantı noktasına ulaşmasını engelleyebilen birçok şey vardır. Çoğu zaman üç işlemlerden biridir:

* **Bir güvenlik duvarı bu şekilde yapılır.** Bu şekilde bir güvenlik duvarınız varsa, TCP zaman aşımına uğrayacaktır. Bu durumda TCP zaman aşımı 21 saniyedir. Bağlantıyı sınamak için **tcma** aracını kullanın. TCP zaman aşımları, güvenlik duvarlarının ötesinde çok sayıda şey olabilir ancak buradan başlayabilir. 
* **DNS erişilebilir değil.** DNS sunucusu başına DNS zaman aşımı üç saniyedir. İki DNS sunucunuz varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Sanal ağınızın yapılandırıldığı DNS 'i kullanmayan için nslookup 'ı kullanamıyoruz. Erişilemezse, DNS erişimi engelleyen bir güvenlik duvarına veya NSG 'ye sahip olabilirsiniz veya bu durumda olabilir.

Bu öğeler sorunlarınızı yanıtlamazsanız, şunun gibi şeyler için göz atın: 

**bölgesel VNet tümleştirmesi**
* hedef, RFC1918 olmayan bir adres değil ve WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmamış.
* Tümleştirme alt ağınızdan çıkış bloke bir NSG var mı?
* ExpressRoute veya VPN üzerinden gitecekseniz, şirket içi ağ geçidiniz trafiği Azure 'a yeniden yönlendirmek üzere yapılandırılmış mı? VNet 'iniz içindeki uç noktalara ulaşabilseniz de şirket içi değil, rotalarınızı denetleyin.
* Tümleştirme alt ağında temsili ayarlamak için yeterli izinlere sahip misiniz? Bölgesel VNet tümleştirme yapılandırması sırasında, tümleştirme alt ağınız Microsoft. Web 'e Temsilcili olur. VNet tümleştirme Kullanıcı arabirimi, alt ağı Microsoft. Web 'e otomatik olarak devredebilir. Hesabınız temsilci seçmek için yeterli ağ izinlerine sahip değilse, alt ağı devretmek için tümleştirme alt ağlarınızın özniteliklerini ayarlayabilen bir kişiye ihtiyacınız olacaktır. Tümleştirme alt ağını el ile atamak için Azure sanal ağ alt ağı Kullanıcı arabirimine gidin ve Microsoft. Web için temsilci belirleyin. 

**Ağ Geçidi gerekli VNet tümleştirmesi**
* , RFC 1918 aralıklarında (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255) Noktadan siteye adres aralığıdır mi?
* Ağ Geçidi portalda mi çalışıyor? Ağ geçidinizin kapalıysa, daha sonra geri getirin.
* Sertifikalar eşitlenmiş olarak gösteriliyor mu veya ağ yapılandırmasının değiştiğini kuşkulu mı?  Sertifikalarınız eşitlenmemiş veya VNet yapılandırmanızda ASPs 'niz ile eşitlenmemiş bir değişiklik yapıldığından şüphelenirseniz, "ağa eşitleme" düğmesine basın.
* VPN üzerinden gitecekseniz, trafiği Azure 'a yönlendirmek için yapılandırılmış şirket içi ağ geçidi mi? VNet 'iniz içindeki uç noktalara ulaşabilseniz de şirket içi değil, rotalarınızı denetleyin.
* hem site hem de ExpressRoute 'a işaret eden bir birlikte bulunma ağ geçidi kullanmayı deniyor musunuz? Birlikte bulunma ağ geçitleri, VNet tümleştirmesinde desteklenmez.

Belirli bir konağa erişimi engelleyip engellemediğini göremediği için, ağ sorunlarını ayıklama işlemi bir zorluk mıdır: bağlantı noktası birleşimi. Bazı nedenler şunlardır:

* Ana bilgisayarınızda, Noktadan siteye IP aralığına olan uygulama bağlantı noktasına erişimi engellediği bir güvenlik duvarınız var. Çakışan alt ağlar genellikle genel erişim gerektirir.
* Hedef ana bilgisayarınız çalışmıyor.
* Uygulamanız çalışmıyor.
* Yanlış IP veya ana bilgisayar adı vardı.
* Uygulamanız, beklediğinizden farklı bir bağlantı noktasını dinliyor. Uç nokta konağında "netstat-Aon" öğesini kullanarak, işlem KIMLIĞINIZI dinleme bağlantı noktasıyla eşleştirebilirsiniz. 
* Ağ güvenlik gruplarınız, Noktadan siteye IP aralığına, uygulama ana bilgisayarınıza ve bağlantı noktasına erişimi engelleyecek şekilde yapılandırılır.

Uygulamanızın gerçekten kullanacağı adresi bilmiyorsanız unutmayın. Tümleştirme alt ağında veya Noktadan siteye adres aralığında herhangi bir adres olabilir, bu nedenle tüm adres aralığından erişime izin vermeniz gerekir. 

Ek hata ayıklama adımları şunlardır:

* VNet 'iniz içindeki bir VM 'ye bağlanın ve kaynak konağınız üzerinde bağlantı noktasını buradan ulaşmaya çalışın. TCP erişimini test etmek için PowerShell komut **testi-NetConnection**komutunu kullanın. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* bir VM üzerinde bir uygulama açın ve **tcup** kullanarak uygulamadan konsoldan bu konağa ve bağlantı noktasına erişimi test edin

#### <a name="on-premises-resources"></a>Şirket içi kaynaklar ####

Uygulamanız Şirket içindeki bir kaynağa ulaşamadıysanız, sanal ağınızdan kaynağa ulaşabilseniz kontrol edin. TCP erişimini denetlemek için **Test-NetConnection** PowerShell komutunu kullanın. VM 'niz şirket içi kaynağına ulaşamadıysa, VPN veya ExpressRoute bağlantınız düzgün şekilde yapılandırılmamış olabilir.

VNet barındırılan VM 'niz şirket içi sisteminize ulaşılırsa ancak uygulamanız bu nedenle, nedeni aşağıdakilerden biri olabilir:

* Rotalarınız alt ağlarınız ile yapılandırılmaz veya şirket içi ağ geçidinizdeki site adres aralıklarını işaret etmez.
* Ağ güvenlik gruplarınız, Noktadan siteye IP aralığınız için erişimi engelliyor.
* Şirket içi güvenlik duvarları, Noktadan siteye IP aralığından trafiği engelliyor.
* Bölgesel VNet tümleştirme özelliğini kullanarak, RFC 1918 olmayan bir adrese ulaşmaya çalışıyorsunuz.