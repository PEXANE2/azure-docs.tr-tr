---
title: Ağ konusunda dikkat edilmesi gerekenler
description: ASE ağ trafiği ve ASE'nizle ağ güvenlik gruplarını ve kullanıcı tanımlı rotaları nasıl ayarlayabildiğiniz hakkında bilgi edinin.
author: ccompy
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.topic: article
ms.date: 01/24/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: fb931c309b5f85902d8abc9cc6da45576bff4041
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259831"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>App Service Ortamında ağ konusunda dikkat edilmesi gerekenler #

## <a name="overview"></a>Genel Bakış ##

 Azure [Uygulama Hizmet Ortamı,][Intro] Azure Uygulama Hizmeti'nin Azure sanal ağınızdaki (VNet) bir alt ağa dağıtımıdır. Bir Uygulama Hizmeti ortamı (ASE) için iki dağıtım türü vardır:

- **Harici ASE**: ASE tarafından barındırılan uygulamaları internet erişimine dayalı bir IP adresinde ortaya çıkarır. Daha fazla bilgi için [bkz.][MakeExternalASE]
- **ILB ASE**: ASE tarafından barındırılan uygulamaları VNet'inizin içindeki bir IP adresinde ortaya çıkarır. Dahili uç nokta bir iç yük dengeleyicisi (ILB), bu yüzden bir ILB ASE denir. Daha fazla bilgi için [bkz.][MakeILBASE]

Tüm ASE'ler, Harici ve ILB, gelen yönetim trafiği için kullanılan ve ASE'den internete arama yaparken adres olarak kullanılan bir genel VIP'ye sahiptir. İnternete giden bir ASE'den gelen aramalar, ASE için atanan VIP aracılığıyla VNet'ten ayrılır. Bu VIP kamu IP internete gitmek ASE tüm aramalar için kaynak IP' dir. ASE'nizdeki uygulamalar VNet'inizdeki veya VPN üzerinden kaynaklara arama yaparsa, kaynak IP, ASE'niz tarafından kullanılan alt ağdaki IP'lerden biridir. ASE VNet içinde olduğundan, ek yapılandırma olmadan VNet içindeki kaynaklara da erişebilir. VNet şirket içi ağınıza bağlıysa, ASE'nizdeki uygulamalar ek yapılandırma olmadan buradaki kaynaklara da erişebilir.

![Dış ASE][1] 

Harici BIR ASE'niz varsa, genel VIP aynı zamanda ASE uygulamalarınızın çözümlediğiniz son noktadır:

* HTTP/S 
* FTP/S
* Web dağıtımı
* Uzaktan hata ayıklama

![ILB ASE][2]

ILB ASE'niz varsa, ILB adresihttp/S, FTP/S, web dağıtımı ve uzaktan hata ayıklama için son noktadır.

## <a name="ase-subnet-size"></a>ASE alt ağ boyutu ##

ASE'yi barındırmak için kullanılan alt ağın boyutu, ASE dağıtıldıktan sonra değiştirilemez.  ASE, her altyapı rolü için ve her Yalıtılmış Uygulama Hizmeti planı örneği için bir adres kullanır.  Ayrıca, oluşturulan her alt ağ için Azure Ağı tarafından kullanılan beş adres vardır.  Uygulama Hizmeti planı olmayan bir ASE, bir uygulama oluşturmadan önce 12 adres kullanır.  Bir ILB ASE ise, o ASE bir uygulama oluşturmadan önce 13 adresleri kullanır. ASE'nizi ölçeklendirirken, Uygulama Hizmeti planı örneklerinizin 15 ve 20'sinin her katı nda altyapı rolleri eklenir.

   > [!NOTE]
   > Ase'den başka hiçbir şey alt ağda olamaz. Gelecekteki büyümeiçin izin veren bir adres alanı seçtiğinizden emin olun. Bu ayarı daha sonra değiştiremezsiniz. Biz 256 `/24` adresleri ile bir boyut öneririz.

Yukarı veya aşağı ölçeklendirildiğinde, uygun boyutta yeni roller eklenir ve ardından iş yüklerin izlenir, geçerli boyuttan hedef boyutuna geçirilir. Özgün VM'ler yalnızca iş yükleri geçtikten sonra kaldırıldı. 100 ASP örneği içeren bir ASE'niz varsa, VM sayısının iki katına ihtiyacınız olan bir dönem olurdu.  Bu nedenle, gerektirebilecek değişiklikleri karşılamak için bir '/24' kullanılmasını öneririz.  

## <a name="ase-dependencies"></a>ASE bağımlılıkları ##

### <a name="ase-inbound-dependencies"></a>ASE gelen bağımlılıklar ###

ASE'nin çalışması için ASE'nin aşağıdaki bağlantı noktalarının açık olmasını gerektirir:

| Kullanım | Başlangıç | Alıcı |
|-----|------|----|
| Yönetim | Uygulama Hizmeti yönetim adresleri | ASE alt ağı: 454, 455 |
|  ASE iç iletişim | ASE subnet: Tüm bağlantı noktaları | ASE subnet: Tüm bağlantı noktaları
|  Azure yük bakiyesi gelen izin ver | Azure yük dengeleyici | ASE alt ağı: 16001

7654 ve 1221 porttazi üzerinde açık olarak gösterilebilen 2 bağlantı noktası daha vardır. Onlar bir IP adresi ve başka bir şey ile cevap. İstenirlerse engellenebilirler. 

Gelen yönetim trafiği, sistem izlemenin yanı sıra ASE'nin komuta ve denetimini de sağlar. Bu trafiğin kaynak adresleri [ASE Yönetimi adresleri][ASEManagement] belgesinde listelenir. Ağ güvenliği yapılandırmasının 454 ve 455 bağlantı noktalarındaki ASE yönetim adreslerinden erişime izin sağlaması gerekir. Bu adreslerden erişimi engellerseniz, ASE'niz sağlıksız hale gelir ve askıya alınır. 454 ve 455 bağlantı noktalarına gelen TCP trafiği aynı VIP'den geri dönmelidir veya asimetrik yönlendirme sorunu olur. 

ASE alt ağı içinde, dahili bileşen iletişimi için kullanılan birçok bağlantı noktası vardır ve bunlar değişebilir. Bu, ASE alt netindeki tüm bağlantı noktalarının ASE alt netinden erişilebilmelidir. 

Azure yük dengeleyicisi ile ASE alt ağı arasındaki iletişim için açık olması gereken minimum bağlantı noktaları 454, 455 ve 16001'dir. 16001 bağlantı noktası, yük dengeleyicisi ve ASE arasındaki trafiği canlı tutmak için kullanılır. ILB ASE kullanıyorsanız, trafiği sadece 454, 455, 16001 bağlantı noktalarına kadar kilitleyebilirsiniz.  Harici ASE kullanıyorsanız, normal uygulama erişim bağlantı noktalarını dikkate almanız gerekir.  

Kendinizi ilgilendiren diğer bağlantı noktaları uygulama bağlantı noktalarıdır:

| Kullanım | Bağlantı Noktaları |
|----------|-------------|
|  HTTP/HTTPS  | 80, 443 |
|  FTP/FTPS    | 21, 990, 10001-10020 |
|  Visual Studio uzaktan hata ayıklama  |  4020, 4022, 4024 |
|  Web Dağıtım hizmeti | 8172 |

Uygulama bağlantı noktalarını engellerseniz, ASE'niz çalışmaya devam edebilir, ancak uygulamanız çalışmayabilir.  Harici ASE ile atanan uygulama IP adreslerini kullanıyorsanız, uygulamalarınıza atanan IP'lerden, ASE portalı > IP Adresleri sayfasında gösterilen bağlantı noktalarındaASE alt ağına trafik izni vermeniz gerekir.

### <a name="ase-outbound-dependencies"></a>ASE giden bağımlılıklar ###

Giden erişim için, bir ASE birden çok dış sisteme bağlıdır. Bu sistem bağımlılıklarının çoğu DNS adlarıyla tanımlanır ve sabit bir IP adresi kümesiyle eşleşmez. Bu nedenle, ASE, ASE alt netinden çeşitli bağlantı noktalarındaki tüm harici IP'lere giden erişim gerektirir. 

ASE, aşağıdaki bağlantı noktalarında internet erişimine açık adreslere iletişim kurar:

| Kullanımlar | Bağlantı Noktaları |
|-----|------|
| DNS | 53 |
| NTP | 123 |
| CRL, Windows güncellemeleri, Linux bağımlılıkları, Azure hizmetleri | 80/443 |
| Azure SQL | 1433 | 
| İzleme | 12000 |

Giden bağımlılıklar, Uygulama Hizmeti Ortamı [giden trafiği kilitlemeyi](./firewall-integration.md)açıklayan belgede listelenir. ASE bağımlılıklarına erişimini kaybederse, çalışmayı durdurur. Bu yeterince uzun olduğunda, ASE askıya alınır. 

### <a name="customer-dns"></a>Müşteri DNS ###

VNet müşteri tanımlı bir DNS sunucusuyla yapılandırılırsa, kiracı iş yükleri bunu kullanır. ASE, yönetim amacıyla Azure DNS kullanır. VNet müşteri tarafından seçilen bir DNS sunucusuyla yapılandırılırsa, DNS sunucusuna ASE'yi içeren alt ağdan ulaşılabilir olmalıdır.

Web uygulamanızdan DNS çözünürlüğünü test etmek için konsol *komutu ad çözücüsünü*kullanabilirsiniz. Uygulamanız için scm sitenizdeki hata ayıklama penceresine gidin veya portaldaki uygulamaya gidin ve konsolu seçin. Kabuk isteminden, yukarı bakmak istediğiniz DNS adı ile birlikte komut *ad çözümleyicisini* düzenleyebilirsiniz. Geri aldığınız sonuç, uygulamanızın aynı aramayı yaparken elde edeceği sonuçla aynıdır. NSLookup kullanıyorsanız, bunun yerine Azure DNS kullanarak bir arama yaparsınız.

ASE'nizin bulunduğu VNet'in DNS ayarını değiştirirseniz, ASE'nizi yeniden başlatmanız gerekir. ASE'nizi yeniden başlatmayı önlemek için, ASE'nizi oluşturmadan önce DNS ayarlarınızı VNet'iniz için yapılandırmanız önerilir.  

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portal bağımlılıkları ##

ASE fonksiyonel bağımlılıklarına ek olarak, portal deneyimiyle ilgili birkaç ekstra öğe vardır. Azure portalındaki bazı özellikler _SCM sitesine_doğrudan erişime bağlıdır. Azure Uygulama Hizmeti'ndeki her uygulama için iki URL vardır. İlk URL, uygulamanıza erişmektir. İkinci URL, _Kudu konsolu_olarak da adlandırılan SCM sitesine erişmektir. SCM sitesini kullanan özellikler şunlardır:

-   Web işleri
-   İşlevler
-   Günlük akışı
-   Kudu
-   Uzantılar
-   İşlem Gezgini
-   Konsol

ILB ASE kullandığınızda, SCM sitesine VNet dışından erişilemez. Bir uygulamanın SCM sitesine erişim gerektirdiğinden, bazı özellikler uygulama portalından çalışmaz. Portalı kullanmak yerine doğrudan SCM sitesine bağlanabilirsiniz. 

ILB ASE'niz *contoso.appserviceenvironment.net* alan adıysa ve uygulama adınız *testapp*ise, uygulamaya *testapp.contoso.appserviceenvironment.net*ulaşılır. Onunla gider SCM sitesi *testapp.scm.contoso.appserviceenvironment.net*ulaşılır.

## <a name="ase-ip-addresses"></a>ASE IP adresleri ##

Bir ASE'nin dikkat etmesi gereken birkaç IP adresi vardır. Bunlar:

- **Genel gelen IP adresi**: Harici ASE'deki uygulama trafiği ve hem Harici ASE hem de ILB ASE'deki yönetim trafiği için kullanılır.
- **Giden genel IP**: VPN'den aşağı yönlendirilen VNet'ten ayrılan ASE'den giden bağlantılar için "kimden" IP olarak kullanılır.
- **ILB IP adresi**: ILB IP adresi yalnızca Bir ILB ASE'de bulunur.
- **Uygulamaya atanan IP tabanlı SSL adresleri**: Yalnızca Harici BIR ASE ile ve IP tabanlı SSL yapılandırıldığında mümkündür.

Tüm bu IP adresleri ASE UI'den Azure portalında görülebilir. ILB ASE'niz varsa, ILB'nin IP'si listelenir.

   > [!NOTE]
   > Bu IP adresleri, ASE'niz çalışır durumda ve çalışmaya devam ettikçe değişmez.  ASE'niz askıya alınır ve geri yüklenirse, ASE'niz tarafından kullanılan adresler değişir. Bir ASE'nin askıya alınmasının normal nedeni, gelen yönetim erişimini engellemeniz veya BIR ASE bağımlılığına erişimi engellemenizdir. 

![IP adresleri][3]

### <a name="app-assigned-ip-addresses"></a>Uygulamaya atanan IP adresleri ###

Harici ASE ile tek tek uygulamalara IP adresleri atayabilirsiniz. Bunu Bir ILB ASE ile yapamazsınız. Uygulamanızın kendi IP adresine sahip olacak şekilde nasıl yapılandırılabildiğiniz hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'nde SSL bağlayıcısı olan özel bir DNS adını Güvenli'ye](../configure-ssl-bindings.md)bakın.

Bir uygulamanın kendi IP tabanlı SSL adresi olduğunda, ASE bu IP adresine eşlenmek için iki bağlantı noktası ayırır. Bir bağlantı noktası HTTP trafiği için, diğer bağlantı noktası ise HTTPS içindir. Bu bağlantı noktaları IP adresleri bölümünde ASE UI'de listelenir. Trafik, VIP'den bu bağlantı noktalarına ulaşabilmeli veya uygulamalara erişilememelidir. Bu gereksinim, Ağ Güvenlik Grupları (NSGs) yapılandırDığınızda hatırlamak önemlidir.

## <a name="network-security-groups"></a>Ağ Güvenlik Grupları ##

[Ağ Güvenlik Grupları,][NSGs] Bir VNet içinde ağ erişimini denetleme olanağı sağlar. Portalı kullandığınızda, her şeyi inkar etmek için en düşük önceliğe sahip üstü kapalı bir inkar kuralı vardır. İnşa ettiğiniz şey izin kurallarınızdır.

BIR ASE'de, ASE'nin kendisini barındırmak için kullanılan VM'lere erişiminiz yoktur. Microsoft tarafından yönetilen bir abonelikteler. ASE'deki uygulamalara erişimi kısıtlamak istiyorsanız, NSG'leri ASE alt ağına ayarlayın. Bunu yaparken, ASE bağımlılıklarına dikkat edin. Herhangi bir bağımlılık ları engellerseniz, ASE çalışmayı durdurur.

NSG'ler Azure portalı veya PowerShell üzerinden yapılandırılabilir. Buradaki bilgiler Azure portalını gösterir. **Portaldaki**NSG'leri Ağ altında üst düzey bir kaynak olarak oluşturur ve yönetirsiniz.

Bir ASE'nin çalışması için NSG'deki gerekli girişler trafiğe izin vermek içindir:

**Gelen**
* bağlantı noktalarındaki AppServiceManagement IP servis etiketinden 454.455
* bağlantı noktası 16001 üzerindeki yük dengeleyicisinden
* tüm bağlantı noktalarında ASE alt ağına KADAR

**Giden**
* bağlantı noktası 123'teki tüm IP'lere
* bağlantı noktalarındaki tüm IP'lere 80, 443
* bağlantı noktalarındaki AzureSQL IP servis etiketine 1433
* bağlantı noktası 12000 tüm IP'ler için
* tüm bağlantı noktalarındaASE alt ağına

DNS'ye trafik NSG kurallarından etkilenmediği için DNS bağlantı noktasının eklenmesi gerekmez. Bu bağlantı noktaları, uygulamalarınızın başarılı kullanım için ihtiyaç duyduğu bağlantı noktalarını içermez. Normal uygulama erişim bağlantı noktaları şunlardır:

| Kullanım | Bağlantı Noktaları |
|----------|-------------|
|  HTTP/HTTPS  | 80, 443 |
|  FTP/FTPS    | 21, 990, 10001-10020 |
|  Visual Studio uzaktan hata ayıklama  |  4020, 4022, 4024 |
|  Web Dağıtım hizmeti | 8172 |

Gelen ve giden gereksinimler dikkate alındığında, NSG'ler bu örnekte gösterilen NSG'lere benzer görünmelidir. 

![Gelen güvenlik kuralları][4]

Varsayılan kural, VNet'teki IP'lerin ASE alt ağıyla konuşmasını sağlar. Başka bir varsayılan kural, ortak VIP olarak da bilinen yük bakiyecinin ASE ile iletişim kurmasını sağlar. Varsayılan kuralları görmek için **Ekle** simgesinin yanındaki **Varsayılan kuralları** seçin. Varsayılan kurallardan önce diğer her şeyi reddet kuralı koyarsanız, VIP ve ASE arasındaki trafiği önlersiniz. VNet'in içinden gelen trafiği önlemek için, gelene izin vermek için kendi kuralınızı ekleyin. AzureLoadBalancer'a eşit bir kaynak kullanın ve **hedef** **\*** alanı Any ve bir bağlantı noktası aralığı. NSG kuralı ASE alt ağına uygulandığından, hedefte belirli olmanız gerekmez.

Uygulamanız için bir IP adresi atadıysanız, bağlantı noktalarını açık tuttuğunızdan emin olun. Bağlantı noktalarını görmek için **App Service Environment** > **IP adreslerini**seçin.  

Son öğe hariç, aşağıdaki giden kurallarda gösterilen tüm öğeler gereklidir. Bu makalede daha önce belirtildiği ASE bağımlılıklarına ağ erişimini sağlarlar. Bunlardan herhangi birini engellerseniz, ASE'niz çalışmayı durdurur. Listedeki son öğe, ASE'nizin VNet'inizdeki diğer kaynaklarla iletişim kurmasını sağlar.

![Giden güvenlik kuralları][5]

NSG'leriniz tanımlandıktan sonra, bunları ASE'nizin üzerinde olduğu alt ağa atayın. ASE VNet veya subnet'i hatırlamıyorsanız, ASE portal sayfasından görebilirsiniz. NSG'yi alt ağınıza atamak için alt ağı nın ui'sine gidin ve NSG'yi seçin.

## <a name="routes"></a>Yollar ##

Zorunlu tünelleme, Giden trafiğin doğrudan internete değil, ExpressRoute ağ geçidi veya sanal cihaz gibi başka bir yere gitmesi için VNet'inizde rotalar belirlediğinizde olur.  ASE'nizi bu şekilde yapılandırmanız gerekiyorsa, Uygulama Hizmet [Ortamınızı Zorunlu Tünelleme ile Yapılandırma][forcedtunnel]ile ilgili belgeyi okuyun.  Bu belge, ExpressRoute ve zorunlu tünel ile çalışmak için kullanılabilir seçenekleri söyleyecektir.

Portalda bir ASE oluşturduğunuzda, alt ağüzerinde ASE ile oluşturulan bir rota tabloları kümesi de oluştururuz.  Bu güzergahlar sadece doğrudan internete giden trafik göndermek için söylüyorlar.  
Aynı yolları el ile oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalına gidin. Ağ**Rotası Tabloları'nı** **Networking** > seçin.

2. VNet'inizle aynı bölgede yeni bir rota tablosu oluşturun.

3. Rota tablonuz içinden Ara Birimi' ni seçin **Rotalar** > **Ekle'yi**seçin.

4. Sonraki **atlama türünü** **Internet'e** ve **Adres önekini** **0.0.0.0/0**olarak ayarlayın. **Kaydet'i**seçin.

    Daha sonra aşağıdaki gibi bir şey görmek:

    ![Fonksiyonel rotalar][6]

5. Yeni rota tablosunu oluşturduktan sonra, ASE'nizi içeren alt ağa gidin. Portaldaki listeden rota tablonuzu seçin. Değişikliği kaydettikten sonra, alt ağınızla birlikte belirtilen NSG'leri ve rotaları görmeniz gerekir.

    ![NSG'ler ve rotalar][7]

## <a name="service-endpoints"></a>Hizmet Uç Noktaları ##

Hizmet Uç Noktaları, çok kiracılı hizmetlere erişimi bir dizi Azure sanal ağı ve alt ağı ile kısıtlamanızı sağlar. [Sanal Ağ Hizmet Uç Noktaları][serviceendpoints] belgelerinde Hizmet Uç Noktaları hakkında daha fazla bilgi edinebilirsiniz. 

Bir kaynakta Hizmet Uç Noktalarını etkinleştirdiğinizde, diğer tüm yönlendiricilerden daha yüksek öncelikle oluşturulmuş rotalar vardır. Zorla tünellenmiş BIR ASE ile herhangi bir Azure hizmetinde Hizmet Bitiş Noktaları kullanırsanız, bu hizmetlere giden trafik tünele atılmaz. 

Azure SQL örneği içeren bir alt ağda Hizmet Uç Noktaları etkinleştirilirse, o alt ağa veya alt ağdan bağlanan tüm Azure SQL örnekleri için Hizmet Uç Noktaları etkinleştirilmiş olmalıdır. Aynı alt ağdan birden fazla Azure SQL örneğine erişmek istiyorsanız, tek bir Azure SQL örneğinde Hizmet Uç Noktalarını etkinleştirebilir, başka bir örnekte etkinleştiremezsiniz. Başka hiçbir Azure hizmeti, Hizmet Bitiş Noktaları'na göre Azure SQL gibi şekilde hizmet etmez. Azure Depolama ile Hizmet Uç Noktalarını etkinleştirdiğinizde, alt ağınızdan o kaynağa erişimi kilitlersiniz, ancak Hizmet Uç Noktaları etkinleştirilmiş olmasa da diğer Azure Depolama hesaplarından erişmeye devam edebilirsiniz.  

![Hizmet Uç Noktaları][8]

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png
[8]: ./media/network_considerations_with_an_app_service_environment/serviceendpoint.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ss-cert.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[forcedtunnel]: ./forced-tunnel-support.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
