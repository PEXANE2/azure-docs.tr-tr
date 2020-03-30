---
title: ILB ASE v1 oluşturma
description: ILB ile ASE oluşturma ve kullanma. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 0c03905017629e28e41cce2adaa65eac347b8185
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294729"
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Uygulama Servis Ortamı ile Dahili Yük Dengeleyici kullanma

> [!NOTE] 
> Bu makale, App Service Environment v1 hakkındadır. Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
>

Uygulama Hizmet Ortamı (ASE) özelliği, Azure Uygulama Hizmeti'nin çok kiracılı pullarda bulunmayan gelişmiş bir yapılandırma özelliği sunan Premium hizmet seçeneğidir. ASE özelliği, Azure Uygulama Hizmetini Azure Sanal Ağınızda (VNet) temel olarak dağıtır. App Service Environments tarafından sunulan özellikler hakkında daha iyi bir anlayış kazanmak için [Uygulama Hizmeti Ortamı][WhatisASE] nedir belgelerini okuyun. Bir VNet'te faaliyet vermenin faydalarını bilmiyorsanız [Azure Sanal Ağ SSS'sini][virtualnetwork]okuyun. 

## <a name="overview"></a>Genel Bakış
Ase, internet erişimine açık bir bitiş noktası yla veya VNet'inizde bir IP adresiyle dağıtılabilir. IP adresini bir VNet adresine ayarlamak için ASE'nizi bir Dahili Yük Dengeleyicisi (ILB) ile dağıtmanız gerekir. ASE'niz bir ILB ile yapılandırıldığında şunları sağlarsınız:

* kendi etki alanı nızı veya alt etki alanınız. Kolaylaştırmak için, bu belge alt etki alanı varsayar, ancak her iki şekilde de yapılandırabilirsiniz. 
* HTTPS için kullanılan sertifika
* Alt etki alanınız için DNS yönetimi. 

Buna karşılık, şunun gibi şeyler yapabilirsiniz:

* bir Siteden Siteye veya ExpressRoute VPN üzerinden erişebildiğiniz bulutta güvenli bir şekilde iş yeri uygulamaları gibi intranet uygulamalarını barındır
* genel DNS sunucularında listelenmemiş bulutta ana bilgisayar uygulamaları
* ön uç uygulamalarınızın güvenli bir şekilde entegre edilebildiği internet yalıtılmış arka uç uygulamaları oluşturun

#### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler
ILB ASE kullanırken yapamayacağınız bazı şeyler vardır. Bu şeyler şunlardır:

* IPSSL kullanma
* belirli uygulamalara IP adresleri atama
* portal üzerinden bir uygulama ile sertifika satın alma ve kullanma. Elbette, sertifikaları doğrudan bir Sertifika Yetkilisi ile alabilir ve bunları uygulamalarınızla kullanabilirsiniz, ancak Azure portalı üzerinden kullanamazsınız.

## <a name="creating-an-ilb-ase"></a>ILB ASE oluşturma
Bir ILB ASE oluşturma, normalde bir ASE oluşturmaktan çok farklı değildir. ASE oluşturma konusunda daha derin bir tartışma için, [Uygulama Hizmeti Ortamı Nasıl Oluşturulur'a][HowtoCreateASE]bakın. ILB ASE oluşturma işlemi, ASE oluşturma sırasında bir VNet oluşturma veya önceden varolan bir VNet seçmek arasında aynıdır. ILB ASE oluşturmak için: 

1. Azure portalında, **Kaynak Oluştur -> Web + Mobil -> Uygulama Hizmeti Ortamı'nı**seçin.
2. Aboneliğinizi seçin.
3. Kaynak grubunu seçin veya oluşturun.
4. Bir VNet seçin veya oluşturun.
5. VNet seçerken bir alt ağ oluşturun.
6. **Sanal Ağ/Konum -> VNet Yapılandırmasını** seçin ve VIP Türünü Dahili olarak ayarlayın.
7. Bir alt etki alanı adı sağlayın (bu ad, bu ASE'de oluşturulan uygulamalar için kullanılan alt alan adıdır).
8. **Tamam'ı** seçin ve ardından **Oluştur'u oluşturun.**

![][1]

Sanal Ağ bölmesinde, Harici VIP veya Dahili VIP arasında seçim yapmanızı sağlayan bir VNet Yapılandırma seçeneği vardır. Varsayılan seçenek Dış’tır. Harici olarak ayarlanmışsa, ASE'niz internet erişimine uygun bir VIP kullanır. İç’i seçerseniz ASE’niz sanal ağınızın içindeki bir IP adresinde bulunan ILB ile yapılandırılır. 

Dahili'yi seçtikten sonra, ASE'nize daha fazla IP adresi ekleme özelliği kaldırılır ve bunun yerine ASE'nin alt etki alanını sağlamanız gerekir. Harici VIP'li bir ASE'de, ASE adı alt etki alanında bu ASE'de oluşturulan uygulamalar için kullanılır. ASE'niz ***aktosotest*** olarak adlandırılırve bu ASE'deki uygulamanız ***mytest***olarak adlandırılırsa, alt etki alanı ***contosotest.p.azurewebsites.net*** biçimindedir ve bu uygulamanın URL'si ***mytest.contosotest.p.azurewebsites.net.*** VIP Türünü İçE ayarlarsanız, ASE adınız ASE'nin alt etki alanında kullanılmaz. Alt etki alanını açıkça belirtirsiniz. Alt etki alanınız ***contoso.corp.net*** ve bu ASE'de ***zaman raporlaması***adlı bir uygulama yaptıysanız, bu uygulamanın URL'si ***timereporting.contoso.corp.net.***

## <a name="apps-in-an-ilb-ase"></a>ILB ASE'deki uygulamalar
ILB ASE'de uygulama oluşturmak, normalde BIR ASE'de uygulama oluşturmakla aynıdır. 

1. Azure portalında, **Kaynak Oluştur -> Web + Mobil -> Web** veya **Mobil** veya **API Uygulaması'nı**seçin.
2. Uygulamanın adını girin.
3. Aboneliğinizi seçin.
4. Kaynak grubunu seçin veya oluşturun.
5. Bir Uygulama Hizmet Planı (ASP) seçin veya oluşturun. Yeni bir ASP oluşturuyorsanız, konum olarak ASE'nizi seçin ve ASP'nizin oluşturulmasını istediğiniz işçi havuzunu seçin. ASP'yi oluşturduğunuzda, konum ve çalışan havuzu olarak ASE'nizi seçersiniz. Uygulamanın adını belirttiğinizde, uygulama adınız altındaki alt alan adının ASE'niz için alt alan adı ile değiştirildiğini görürsünüz. 
6. **Oluştur'u**seçin. Uygulamanın panonuzda gösterilmesini **istiyorsanız, pano onay kutusuna Pin'i** seçtiğinizden emin olun. 

![][2]

Uygulama adı altında alt etki alanı adı ASE alt etki alanını yansıtacak şekilde güncellenir. 

## <a name="post-ilb-ase-creation-validation"></a>Sonrası ILB ASE oluşturma doğrulaması
Bir ILB ASE, ILB olmayan ASE’den biraz farklıdır. Daha önce de belirtildiği gibi kendi DNS yönetmek gerekir ve aynı zamanda HTTPS bağlantıları için kendi sertifika nızı sağlamak zorunda. 

ASE'nizi oluşturduktan sonra, alt etki alanının belirttiğiniz alt etki alanını gösterdiğini ve **Ayar** menüsünde **ILB Sertifikası**adı verilen yeni bir öğe olduğunu fark edeceksiniz. ASE, HTTPS testini kolaylaştıran kendi imzalı bir sertifikayla oluşturulur. Portal, HTTPS için kendi sertifikanızı sağlamanız gerektiğini söyler, ancak bu, alt etki alanınızın yanına giden bir sertifikaya sahip olmanız için sizi teşvik etmek içindir. 

![][3]

Yalnızca bir şeyler deniyorsanız ve nasıl sertifika oluşturacağınıbilmiyorsanız, kendi imzalı bir sertifika oluşturmak için IIS MMC konsol uygulamasını kullanabilirsiniz. Oluşturulduktan sonra, bir .pfx dosyası olarak dışa aktarabilir ve ardından ILB Sertifika UI'ye yükleyebilirsiniz. Kendi imzalı bir sertifikayla güvenli bir siteye erişdiğinizde, tarayıcınız, sertifikayı doğrulayamadığıiçin erişmediğiniz sitenin güvenli olmadığı konusunda size bir uyarı verir. Bu uyarıdan kaçınmak istiyorsanız, alt etki alanınızla eşleşen ve tarayıcınız tarafından tanınan bir güven zincirine sahip düzgün imzalanmış bir sertifikaya ihtiyacınız vardır.

![][6]

Akışı kendi sertifikalarınızla denemek ve HEM HTTP hem de HTTPS erişimini ASE'nize test etmek istiyorsanız:

1. **ASE -> Ayarları -> ILB Sertifikaları**oluşturulduktan sonra ASE UI'ye gidin.
2. Sertifika pfx dosyaseçerek ILB sertifikası ayarlayın ve şifre sağlayın. Bu adımın işlenmesi biraz zaman alır ve ölçekleme işleminin devam ettiği iletisi görüntülenir.
3. ASE **(ASE -> Properties -> Sanal IP Adresiniz**için ILB adresini alın).
4. Oluşturulduktan sonra ASE'de bir web uygulaması oluşturun. 
5. VNET'te yoksa bir VM oluşturun (ASE ile aynı alt ağda değil veya bir şeyler kırılır).
6. Alt etki alanınız için DNS'yi ayarlayın. DNS'nizde alt etki alanınızın olduğu bir joker karakter kullanabilir veya bazı basit testler yapmak istiyorsanız, web uygulaması adını VIP IP adresine ayarlamak için VM'nizdeki ana bilgisayar dosyasını değiştirin. ASE alt etki alanı adı .ilbase.com olsaydı ve web uygulaması mytestapp böylece mytestapp.ilbase.com adresinde ele alınacak, bunu ev sahibi dosyanızda ayarlayın. (Windows'da, ana bilgisayar dosyası C:\Windows\System32\drivers\etc\)
7. Bu VM'de bir tarayıcı `https://mytestapp.ilbase.com` kullanın ve (veya web uygulama adınız alt alan adınız ne olursa olsun) gidin.
8. Veya bu sanal makinedeki bir tarayıcıyı kullanıp `https://mytestapp.ilbase.com` sayfasına gidin. Kendi imzalı bir sertifika kullanıyorsanız, güvenlik eksikliğini kabul etmelisiniz. 

ILB'nizin IP adresi Özelliklerinizde Sanal IP Adresi olarak listelenir.

![][4]

## <a name="using-an-ilb-ase"></a>ILB ASE kullanma
#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
ILB ASE, uygulamalarınız için ağ yalıtımı sağlar. Uygulamalara internet tarafından erişilemez ve hatta bilinmez. Bu yaklaşım, iş hattı uygulamaları gibi intranet sitelerini barındırmak için mükemmeldir. Erişimi daha da kısıtlamanız gerektiğinde, ağ düzeyinde erişimi denetlemek için Ağ Güvenlik Grupları'nı (NSGs) kullanmaya devam edebilirsiniz. 

Erişimi daha da kısıtlamak için NSG'leri kullanmak istiyorsanız, ase'nin çalışması için ihtiyaç duyduğu iletişimi kesmediğinden emin olmalısınız. HTTP/HTTPS erişimi yalnızca ASE tarafından kullanılan ILB üzerinden olsa da, ASE hala VNet dışındaki kaynaklara bağlıdır. Hangi ağ erişiminin hala gerekli olduğunu görmek [için, ExpressRoute ile Uygulama Hizmeti Ortamları için][ExpressRoute]Gelen Trafiği Bir [Uygulama Hizmeti Ortamına ve][ControlInbound] Ağ Yapılandırma Ayrıntılarına Denetleme'ye bakın. 

NSG'lerinizi yapılandırmak için, Azure tarafından ASE'nizi yönetmek için kullanılan IP adresini bilmeniz gerekir. Bu IP adresi, internet isteklerini yerine getirirse, ASE'nizden giden IP adresidir. ASE'nizin giden IP adresi, ASE'nizin ömrü boyunca sabit kalır. ASE'nizi siler ve yeniden oluşturursanız, yeni bir IP adresi alırsınız. IP adresini bulmak için **Ayarlar -> Özellikleri'ne** gidin ve **Giden IP Adresini**bulun. 

![][5]

#### <a name="general-ilb-ase-management"></a>Genel ILB ASE yönetimi
ILB ASE'yi yönetmek, normalde bir ASE'yi yönetmekle büyük ölçüde aynıdır. Daha fazla ASP örneğine ev sahipliği yapacak şekilde çalışan havuzlarınızı ölçeklendirmeniz ve artan miktarda HTTP/HTTPS trafiğini işlemek için ön uç sunucularınızı ölçeklendirmeniz gerekir. Bir ASE yapılandırmasını yönetme hakkında genel bilgi [için][ASEConfig]bkz. 

Ek yönetim öğeleri sertifika yönetimi ve DNS yönetimidir. ILB ASE oluşturulduktan sonra HTTPS için kullanılan sertifikayı almalı ve yüklemeli ve süresi dolmadan önce değiştirmelisiniz. Azure temel etki alanına sahip olduğundan, Bir Dış VIP ile AS'ler için sertifikalar sağlayabilir. Bir ILB ASE tarafından kullanılan alt etki alanı herhangi bir şey olabileceğinden, HTTPS için kendi sertifikanızı sağlamanız gerekir. 

#### <a name="dns-configuration"></a>DNS Yapılandırması
Harici VIP kullanırken, DNS Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. contoso.corp.net gibi belirli bir alt etki alanı için, ILB adresinizi işaret eden DNS A kayıtları oluşturmanız gerekir:

    * 
    *.scm ftp yayın 


## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamlarına Giriş][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
