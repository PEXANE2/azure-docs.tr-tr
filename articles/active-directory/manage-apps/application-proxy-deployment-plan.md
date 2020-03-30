---
title: Azure Active Directory Application Proxy Dağıtımı Planlayın
description: Kuruluşunuz içinde Uygulama proxy'sinin dağıtımını planlamak için uçtan uca kılavuz
services: active-directory
documentationcenter: azure
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: baselden
ms.reviewer: ''
ms.openlocfilehash: edd607c4d708df9fcfd3cbd5fdb71f0a7652d6c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80330901"
---
# <a name="plan-an-azure-ad-application-proxy-deployment"></a>Azure AD Uygulama Proxy dağıtımı planlama

Azure Active Directory (Azure AD) Application Proxy, şirket içi uygulamalar için güvenli ve uygun maliyetli bir uzaktan erişim çözümüdür. "Önce Bulut" kuruluşlarının, henüz modern protokolleri kullanamayan eski şirket içi uygulamalara erişimi yönetmesi için anında bir geçiş yolu sağlar. Ek giriş bilgileri için [Uygulama Proxy nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy)bakın.

Uygulama Proxy uzak kullanıcılara iç kaynaklara erişim vermek için önerilir. Application Proxy, bu uzaktan erişim kullanım örnekleri için VPN veya ters proxy gereksiniminin yerini alır. Şirket ağında ki kullanıcılar için tasarlanmamıştır. İntranet erişimi için Uygulama Proxy'yi kullanan bu kullanıcılar istenmeyen performans sorunlarıyla karşılaşabilir.

Bu makalede, Azure AD Uygulama Proxy'sini planlamak, işletmek ve yönetmek için gereken kaynaklar yer almaktadır. 

## <a name="plan-your-implementation"></a>Uygulamanızı planlayın

Aşağıdaki bölüm, verimli bir dağıtım deneyimi için sizi ayarlayacak temel planlama öğelerinin geniş bir görünümünü sağlar. 

### <a name="prerequisites"></a>Ön koşullar

Uygulamaya başlamadan önce aşağıdaki ön koşulları karşılamanız gerekir. Bu [öğreticide,](application-proxy-add-on-premises-application.md)bu ön koşullar da dahil olmak üzere ortamınızı ayarlama hakkında daha fazla bilgi görebilirsiniz.

* **Konektörler**: Konektörler, üzerine dağıtabileceğiniz hafif aracılardır:
   * Fiziksel donanım şirket içinde
   * Herhangi bir hipervizör çözeltisi içinde barındırılan bir VM
   * Uygulama Proxy hizmetine giden bağlantıyı etkinleştirmek için Azure'da barındırılan bir VM.

* Daha ayrıntılı bir genel bakış için [Azure AD App Proxy Bağlayıcılarını Anlayın'](application-proxy-connectors.md) a bakın.

     * Konektörleri takmadan önce [TLS 1.2 için](application-proxy-add-on-premises-application.md) konektör makineleri etkinleştirilmelidir.

     * Mümkünse, eksgetirdiğileri arka uç web uygulaması sunucuları ile [aynı ağ](application-proxy-network-topology.md) ve segmente dağıtın. Uygulamaların keşfini tamamladıktan sonra bağlayıcıları dağıtmak en iyisidir.
     * Her bağlayıcı grubunun yüksek kullanılabilirlik ve ölçek sağlamak için en az iki konektöre sahip olmasını öneririz. Herhangi bir noktada bir makineye hizmet vermeniz gerekebilir sayılsa da üç konektöre sahip olmak en uygun uyrabilir. Konektörleri hangi makine türünü takacağına karar vermek için [bağlayıcı kapasite tablosunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#capacity-planning) gözden geçirin. Makine ne kadar büyükse konektör o kadar fazla tampon ve performant olur.

* **Ağ erişim ayarları**: Azure AD Application Proxy bağlayıcıları [HTTPS (TCP Port 443) ve HTTP (TCP Port 80) üzerinden Azure'a bağlanır.](application-proxy-add-on-premises-application.md) 

   * Bağlayıcı TLS trafiğini sona erdirme desteklenmez ve bağlayıcıların kendi Azure App Proxy uç noktalarıyla güvenli bir kanal oluşturmalarını engeller.

   * Bağlayıcılar ve Azure arasındaki giden TLS iletişimlerinde her türlü satır ara denetiminden kaçının. Bağlayıcı ve arka uç uygulamaları arasında dahili denetim mümkündür, ancak kullanıcı deneyimini bozabilir ve bu nedenle önerilmez.

   * Konektörlerin yük dengelemesi de desteklenmez, hatta gerekli değildir.

### <a name="important-considerations-before-configuring-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy'yi yapılandırmadan önce önemli noktalar

Azure AD Application Proxy'yi yapılandırmak ve uygulamak için aşağıdaki temel gereksinimlerin karşılanması gerekir.

*  **Azure onboarding**: Uygulama proxy'si dağıtmadan önce, kullanıcı kimliklerinin şirket içi bir dizinden eşitlenmeleri veya doğrudan Azure AD kiracılarınızın içinde oluşturulması gerekir. Kimlik eşitleme, Azure AD'nin kullanıcılara App Proxy tarafından yayınlanan uygulamalara erişim hakkı vermeden önce kullanıcıların kimlik doğrulaması için ön kimlik doğrulamasına ve tek oturum açma (SSO) gerçekleştirmek için gerekli kullanıcı tanımlayıcı bilgilerine sahip olmasını sağlar.

* **Koşullu Erişim gereksinimleri**: İntranet erişimi için Uygulama Proxy'si kullanmanızı önermiyoruz, çünkü bu, kullanıcıları etkileyecek gecikme süresi ekler. Internet'ten uzaktan erişim için ön kimlik doğrulama ve Koşullu Erişim ilkeleriyle Uygulama Proxy'yi kullanmanızı öneririz.  Intranet kullanımı için Koşullu Erişim sağlamak için bir yaklaşım doğrudan AAD ile kimlik doğrulaması böylece uygulamaları modernize etmektir. Daha fazla bilgi [için AAD'ye geçiş uygulamaları için Kaynaklar'a](https://docs.microsoft.com/azure/active-directory/manage-apps/migration-resources) bakın. 

* **Hizmet limitleri**: Bireysel kiracılar tarafından kaynakların aşırı tüketimine karşı korumak için uygulama ve kiracı başına belirlenen azaltma limitleri vardır. Bu sınırları görmek için [Azure AD hizmet sınırları ve kısıtlamalarına](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions)bakın. Bu azaltma sınırları, tipik kullanım hacminin çok üzerinde bir kıyaslama temel alınarak dağıtımların çoğunluğu için yeterli arabellek sağlar.

* **Genel sertifika**: Özel alan adları kullanıyorsanız, bir TLS/SSL sertifikası edinmeniz gerekir. Kuruluş gereksinimlerinize bağlı olarak, sertifika almak biraz zaman alabilir ve işleme mümkün olduğunca erken başlamanızı öneririz. Azure Application Proxy standart, [joker karakter](application-proxy-wildcard.md)veya SAN tabanlı sertifikaları destekler. Daha fazla ayrıntı için Azure [AD Application Proxy ile özel etki alanlarını yapılandırın.](application-proxy-configure-custom-domain.md)

* **Etki alanı gereksinimleri**: Kerberos Kısıtlı Delegasyonu (KCD) kullanarak yayınlanan uygulamalarınızda tek oturum açma, Bağlayıcıyı çalıştıran sunucunun ve uygulamayı çalıştıran sunucunun etki alanının birleştirilmesini ve aynı etki alanının veya güvenilir etki alanlarının bir parçası olmasını gerektirir.
Konu hakkında ayrıntılı bilgi için, Uygulama Proxy ile [tek oturum açma için KCD'ye](application-proxy-configure-single-sign-on-with-kcd.md) bakın. Bağlayıcı hizmeti yerel sistem bağlamında çalışır ve özel bir kimlik kullanacak şekilde yapılandırılmamalıdır.

* **URL'ler için DNS kayıtları**

   * Uygulama Proxy'de özel etki alanlarını kullanmadan önce, istemcilerin özel tanımlı dış URL'yi önceden tanımlanmış Uygulama Proxy adresine çözmesine olanak tanıyan, ortak DNS'de bir CNAME kaydı oluşturmanız gerekir. Özel etki alanı kullanan bir uygulama için CNAME kaydı oluşturulamamak, uzak kullanıcıların uygulamaya bağlanmasını engeller. CNAME kayıtları eklemek için gereken adımlar DNS sağlayıcısından sağlayıcıya değişebilir, bu nedenle [Azure portalını kullanarak DNS kayıtlarını ve kayıt kümelerini](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-portal)nasıl yönetebileceğinizi öğrenin.

   * Benzer şekilde, bağlayıcı ana bilgisayarlar da yayımlanmakta olan uygulamaların dahili URL'sini çözebilmelidir.

* **İdari haklar ve roller**

   * **Bağlayıcı yükleme,** yüklendiğinin Windows sunucusunun yerel yönetici haklarını gerektirir. Ayrıca, bağlayıcı örneğini doğrulamak ve Azure AD kiracınıza kaydetmek için en az bir *Uygulama Yöneticisi* rolü gerektirir. 

   * **Uygulama yayımlama ve yönetim,** *Uygulama Yöneticisi* rolünü gerektirir. Uygulama Yöneticileri, kayıtlar, SSO ayarları, kullanıcı ve grup atamaları ve lisanslama, Uygulama Proxy ayarları ve onayı dahil olmak üzere dizindeki tüm uygulamaları yönetebilir. Koşullu Erişimi yönetme olanağı vermez. *Bulut Uygulama Yöneticisi* rolü, Uygulama Proxy ayarlarının yönetimine izin vermemek dışında, Uygulama Yöneticisi'nin tüm yeteneklerine sahiptir.

* **Lisanslama**: Application Proxy, Azure AD Premium aboneliği aracılığıyla kullanılabilir. Lisans seçenekleri nin ve özelliklerinin tam listesi için [Azure Active Directory Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory/) bakın.  

### <a name="application-discovery"></a>Uygulama Bulma

Aşağıdaki bilgileri toplayarak Application Proxy aracılığıyla yayınlanan tüm kapsam içi uygulamaların envanterini derle:

| Bilgi Türü| Toplanacak bilgiler |
|---|---|
| Hizmet Türü| Örneğin: SharePoint, SAP, CRM, Özel Web Uygulaması, API |
| Uygulama platformu | Örneğin: Windows IIS, Linux'ta Apache, Tomcat, NGINX |
| Etki alanı üyeliği| Web sunucusunun tam nitelikli etki alanı adı (FQDN) |
| Başvuru yeri | Web sunucusunun veya çiftliğinin altyapınızda bulunduğu yer |
| Dahili erişim | Uygulama nın dahili olarak erişimirken kullanılan tam URL. <br> Bir çiftlik varsa, ne tür yük dengelemesi kullanılır? <br> Uygulamanın kendisinden başka kaynaklardan içerik alıp almadığı.<br> Uygulamanın WebSockets üzerinden çalışır mı kararbelirleyin. |
| Dış erişim | Uygulamanın zaten harici olarak maruz olduğu satıcı çözümü. <br> Harici erişim için kullanmak istediğiniz URL. SharePoint ise, Alternatif Erişim [Eşlemeleri'nin bu kılavuza](https://docs.microsoft.com/SharePoint/administration/configure-alternate-access-mappings)göre yapılandırıldığından emin olun. Değilse, harici URL'leri tanımlamanız gerekir. |
| Genel sertifika | Özel bir etki alanı kullanıyorsanız, ilgili konu adına sahip bir sertifika satın alabilen. sertifika varsa, seri numarasını ve alınabileceği yeri not edin. |
| Kimlik doğrulaması türü| Temel, Windows Tümleştirme Kimlik Doğrulaması, form tabanlı, üstbilgi tabanlı ve talepler gibi uygulama desteği tarafından desteklenen kimlik doğrulama türü. <br>Uygulama belirli bir etki alanı hesabı altında çalışacak şekilde yapılandırılırsa, hizmet hesabının Tam Nitelikli Alan Adı'na (FQDN) dikkat edin.<br> SAML tabanlıise, tanımlayıcı ve yanıt URL'leri. <br> Üstbilgi tabanlıysa, satıcı çözümü ve kimlik doğrulama türünü işlemek için özel gereksinim. |
| Bağlayıcı grup adı | Bu arka uç uygulamasına kanal ve SSO sağlamak üzere atanacak bağlayıcılar grubunun mantıksal adı. |
| Kullanıcılar/Gruplar erişimi | Uygulamaya harici erişim hakkı verilecek olan kullanıcı veya kullanıcı grupları. |
| Ek gereksinimler | Uygulamayı yayımlamada etken olması gereken ek uzaktan erişim veya güvenlik gereksinimlerine dikkat edin. |

Bu uygulama [envanteri elektronik tablosunu](https://aka.ms/appdiscovery) uygulamalarınızın envanterini çıkarmak için indirebilirsiniz.

### <a name="define-organizational-requirements"></a>Kuruluş gereksinimlerini tanımlama

Aşağıda, kuruluşunuzun iş gereksinimlerini tanımlamanız gereken alanlar verilmelidir. Her alan gereksinim örnekleri içerir

 **Erişim**

* Etki alanına katılan veya Azure AD'ye katılan aygıtlara sahip uzak kullanıcılar, yayımlanmış uygulamalara sorunsuz tek oturum açma (SSO) ile güvenli bir şekilde erişebilir.

* Onaylı kişisel aygıtlara sahip uzak kullanıcılar, MFA'ya kaydolmaları ve Microsoft Authenticator uygulamasını cep telefonlarına kimlik doğrulama yöntemi olarak kaydettirmeleri koşuluyla yayımlanmış uygulamalara güvenli bir şekilde erişebilir.

**İdare** 

* Yöneticiler, Uygulama Proxy aracılığıyla yayınlanan uygulamalara kullanıcı atamalarının yaşam döngüsünü tanımlayabilir ve izleyebilir.

**Güvenlik**

* Uygulamalara yalnızca grup üyeliği veya tek tek atanan kullanıcılar bu uygulamalara erişebilir.

**Performans**

* İç ağdan uygulamaya erişmeyle karşılaştırıldığında uygulama performansında herhangi bir bozulma yoktur.

**Kullanıcı Deneyimi**

* Kullanıcılar, herhangi bir cihaz platformunda tanıdık şirket URL'lerini kullanarak uygulamalarına nasıl erişeceklerini biliyor.

**Denetim**
* Yöneticiler kullanıcı erişim etkinliğini denetler.


### <a name="best-practices-for-a-pilot"></a>Bir pilot için en iyi uygulamalar

Tek oturum açma (SSO) ile uzaktan erişim için tek bir uygulamayı tam olarak devreye almak için gereken süre ve çabayı belirleyin. Bunu, ilk keşfini, yayımını ve genel testini dikkate alan bir pilot çalıştırarak yapın. Zaten Entegre Windows Kimlik Doğrulama (IWA) için önceden yapılandırılmış basit bir IIS tabanlı web uygulaması kullanarak, bu kurulum başarıyla uzaktan erişim ve SSO pilot için en az çaba gerektirdiğinden, bir temel oluşturmaya yardımcı olacaktır.

Aşağıdaki tasarım öğeleri, pilot uygulamanızın başarısını doğrudan bir üretim kiracısında artırmalıdır.  

**Konektör yönetimi**:  

* Konektörler, uygulamalarınız için şirket içi kanalı sağlamada önemli bir rol oynar. **Varsayılan** bağlayıcı grubunu kullanmak, yayımlanmış uygulamaların üretime dahil edilmeden önce ilk pilot testini test etmek için yeterlidir. Başarıyla test edilmiş uygulamalar daha sonra üretim konektör gruplarına taşınabilir.

**Uygulama yönetimi:**

* İş gücünüzbüyük olasılıkla harici bir URL'nin tanıdık ve alakalı olduğunu hatırlatır. Önceden tanımlanmış msappproxy.net veya onmicrosoft.com soneklerimizi kullanarak uygulamanızı yayınlamaktan kaçının. Bunun yerine, *intranet.<customers_domain>.com*gibi mantıksal bir ana bilgisayar adı ile önceden belirlenmiş tanıdık bir üst düzey doğrulanmış etki alanı sağlayın.

* Başlatma simgesini gutarak pilot uygulamanın simgesinin görünürlüğünü bir pilot grupla sınırlandırın Ve Azure MyApps portalı. Üretime hazır olduğunuzda, uygulamayı aynı üretim öncesi kiracıda veya uygulamanızı üretim kiracınızda yayımlayarak ilgili hedef kitlesine göre kapsamda tizleyebilirsiniz.

**Tek oturum açma ayarları**: Bazı SSO ayarlarının kurulumu zaman alabilecek belirli bağımlılıkları vardır, bu nedenle bağımlılıkların önceden ele alınmasını sağlayarak değişiklik denetimi gecikmelerini önler. Buna, Kerberos Kısıtlı Delegasyonu (KCD) kullanarak SSO'yu gerçekleştirmek ve diğer zaman alan etkinliklerle ilgilenmek için etki alanı birleştirme bağlantısı ana bilgisayarlarını içerir. Örneğin, üstbilgi tabanlı SSO gerekiyorsa, ping erişimi örneğini ayarlama.

**TlS Connector Host ve Target Application arasında**: Güvenlik her şeyden önemlidir, bu nedenle bağlayıcı ana bilgisayar ve hedef uygulamalar arasındaki TLS her zaman kullanılmalıdır. Özellikle web uygulaması form tabanlı kimlik doğrulaması (FBA) için yapılandırılırsa, kullanıcı kimlik bilgileri daha sonra açık metin olarak etkili bir şekilde iletilir.

**Artımlı olarak uygulayın ve her adımı test edin.** Tüm kullanıcı ve iş gereksinimlerinin aşağıdaki yönergeleri izleyerek karşılandığından emin olmak için bir uygulamayı yayımladıktan sonra temel işlevsel testler yapın:

1. Ön kimlik doğrulama devre dışı bırakılmış web uygulamasına genel erişimi test edin ve doğrulayın.
2. Başarılı sayılsa, ön kimlik doğrulamayı etkinleştirin ve kullanıcıları ve grupları atayın. Erişimi test edin ve doğrulayın.
3. Ardından uygulamanız için SSO yöntemini ekleyin ve erişimi doğrulamak için yeniden test edin.
4. Koşullu Erişim ve MFA ilkelerini gerektiği gibi uygulayın. Erişimi test edin ve doğrulayın.

**Sorun Giderme Araçları**: Sorun giderme araçları, her zaman bağlı bağlantı ana bilgisayarındaki tarayıcıdan yayınlanan uygulamaya erişimi doğrulayarak başlayın ve uygulamanın beklendiği gibi çalıştığını onaylayın. Kurulumunuz ne kadar basitse, kök nedenini belirlemek o kadar kolay olur, bu nedenle yalnızca tek bir konektör ve SSO kullanmamak gibi en az yapılandırmayla sorunları yeniden oluşturmaya çalışmamayı düşünün. Bazı durumlarda, Telerik's Fiddler gibi web hata ayıklama araçları, proxy aracılığıyla erişilen uygulamalardaki erişim veya içerik sorunlarını gidermek için vazgeçilmez olabilir. Fiddler ayrıca iOS ve Android gibi mobil platformlar için izleme ve hata ayıklama trafiği yardımcı olmak için bir proxy olarak hareket edebilir, ve bir proxy üzerinden rota için yapılandırılabilir hemen hemen her şey. Daha fazla bilgi için [sorun giderme kılavuzuna](application-proxy-troubleshoot.md) bakın.

## <a name="implement-your-solution"></a>Çözümünüzü Uygulayın

### <a name="deploy-application-proxy"></a>Uygulama Proxy'yi Dağıt

Uygulama Proxy'nizi dağıtma adımları, [uzaktan erişim için şirket içi bir uygulama eklemek için](application-proxy-add-on-premises-application.md)bu öğreticide yer alıyor. Yükleme başarılı olmazsa, portalda **Sorun Giderme Uygulama Proxy'sini** seçin veya [Application Proxy Agent Connector'ı yükleme yle ilgili sorunlar için](application-proxy-connector-installation-problem.md)sorun giderme kılavuzunu kullanın.

### <a name="publish-applications-via-application-proxy"></a>Uygulamaları Uygulama Proxy ile yayımla

Uygulamaları yayımlama, tüm ön koşulları karşıladığınızı ve Uygulama Proxy sayfasında kayıtlı ve etkin olarak gösteren birkaç bağlayıcınız olduğunu varsayar.

Ayrıca [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)kullanarak uygulamaları yayımlayabilirsiniz.

Aşağıda, bir uygulamayı yayınlarken izlenen bazı en iyi uygulamalar verilmiştir:

* **Bağlayıcı Grupları Kullanın**: Her bir uygulamayı yayımlamak üzere atanmış bir bağlayıcı grubu atayın. Her bağlayıcı grubunun yüksek kullanılabilirlik ve ölçek sağlamak için en az iki konektöre sahip olmasını öneririz. Herhangi bir noktada bir makineye hizmet vermeniz gerekebilir sayılsa da üç konektöre sahip olmak en uygun uyrabilir. Ayrıca, bağlayıcılarınızı ağa veya konuma göre segmentlemek için bağlayıcı gruplarını nasıl kullanabileceğinizi görmek için [bağlayıcı gruplarını kullanarak ayrı ağlarda ve konumlarda uygulamaları](application-proxy-connector-groups.md) yayımla'ya bakın.

* **Geri Uç Uygulama Zaman Sonu Ayarlaması**: Bu ayar, uygulamanın istemci işlemini işlemek için 75 saniyeden fazla zaman alabilecekleri senaryolarda yararlıdır. Örneğin, istemci bir web uygulamasına veritabanına ön uç görevi gören bir sorgu gönderdiğinde. Ön uç, bu sorguyu arka uç veritabanı sunucusuna gönderir ve bir yanıt bekler, ancak yanıt aldığında, konuşmanın istemci tarafı zaman ları devre dışı kalır. Zaman aşımını Uzun'a ayarlamak, daha uzun işlemlerin tamamlanması için 180 saniye sağlar.

* **Uygun Çerez Türlerini Kullanma**

   * **HTTP-Only Cookie**: Uygulama Proxy'sinin set tanımlama http yanıt üstbilgilerinde HTTPOnly bayrağını eklemesini sağlayarak ek güvenlik sağlar. Bu ayar, siteler arası komut dosyası oluşturma (XSS) gibi açıkları azaltmaya yardımcı olur. Oturum çerezine erişim gerektiren istemciler/kullanıcı aracıları için bu seti Hayır'a bırakın. Örneğin, App Proxy üzerinden yayınlanan uzak bir masaüstü ağ geçidine bağlanan RDP/MTSC istemcisi.

   * **Güvenli Çerez**: Güvenli öznitelik ile bir çerez ayarlandığında, kullanıcı aracısı (İstemci tarafı uygulaması) yalnızca istek TLS güvenli bir kanal üzerinden iletilirse, çerezi HTTP isteklerine dahil eder. Bu, bir çerezin açık metin kanalları üzerinden tehlikeye atılma riskini azaltmaya yardımcı olur, bu nedenle etkinleştirilmelidir.

   * **Kalıcı Çerez**: Uygulama Proxy oturum çerezinin süresi dolana veya silinene kadar geçerli kalarak tarayıcı kapanışları arasında devam etmesini sağlar. Office gibi zengin bir uygulamanın, kullanıcı kimlik doğrulaması için yeniden istenmeden, yayınlanmış bir web uygulaması içindeki bir belgeye eriştiği senaryolar için kullanılır. Ancak, kalıcı tanımlama bilgileri, diğer telafi denetimleriyle birlikte kullanılmadığı takdirde bir hizmeti yetkisiz erişim riskine bırakabileceğinden dikkatli olun. Bu ayar yalnızca tanımlama bilgilerini işlemler arasında paylaşılamadı eski uygulamalar için kullanılmalıdır. Bu ayarı kullanmak yerine çerezleri işlemler arasında paylaşmayı işlemek için uygulamanızı güncelleştirmek daha iyidir.

* **URL'leri Üstbilgide Çevirme**: İç DNS'nin kuruluşun ortak ad alanıyla (diğer adıyla Bölünmüş DNS) eşleşecek şekilde yapılandırılamayacağı senaryolar için bunu etkinleştirin. Başvurunuz istemci isteğinde orijinal ana bilgisayar üstbilgisini gerektirmediği sürece, bu değer kümesini Evet olarak bırakın. Alternatif, konektörün fqdn'yi gerçek trafiğin yönlendirmesi için dahili URL'de, FQDN'yi ise ana bilgisayar başlığı olarak harici URL'de kullanmasını sağlamaktır. Çoğu durumda bu alternatif, uzaktan erişildiğinde uygulamanın normal olarak çalışmasına izin vermelidir, ancak kullanıcılarınız URL dışında & içinde eşleşen bir avantajdan kaybeder.

* **URL'leri Uygulama Gövdesinde Çevir**: Uygulama Gövdesi bağlantı çevirisini, o uygulamadaki bağlantıların istemciye yanıt olarak çevrilmesini istediğinizde açın. Bu işlev etkinse, App Proxy'nin HTML ve CSS yanıtlarında bulduğu tüm dahili bağlantıları n için çevirmeçabası sağlar. İçerikte sabit kodlanmış mutlak veya NetBIOS kısa ad bağlantıları içeren uygulamalar veya diğer şirket içi uygulamalara bağlantı veren içeriğe sahip uygulamalar yayımlarken kullanışlıdır.

Yayınlanmış bir uygulamanın diğer yayınlanmış uygulamalara bağlandığı senaryolarda, her uygulama için bağlantı çevirisini etkinleştirin, böylece uygulama başına kullanıcı deneyimi üzerinde denetim edebilirsiniz.

Örneğin, Application Proxy aracılığıyla yayınlanan ve hepsi birbirine bağlanan üç uygulamanız olduğunu varsayalım: Avantajlar, Giderler ve Seyahat, ayrıca Application Proxy aracılığıyla yayınlanmayan dördüncü bir uygulama, Geri Bildirim.

![Resim 1](media/App-proxy-deployment-plan/link-translation.png)

Avantajlar uygulamasının bağlantı çevirisini etkinleştirdiğinizde, Giderler ve Seyahat bağlantıları bu uygulamaların harici URL'lerine yönlendirilir, böylece uygulamalara şirket ağının dışından erişen kullanıcılar bunlara erişebilir. Bu iki uygulama için bağlantı çevirisi etkinleştirilemediği için Giderler ve Avantajlara Geri Seyahat'ten bağlantılar çalışmaz. Harici URL olmadığı için Geri Bildirim bağlantısı yönlendirilmez, bu nedenle Avantajlar uygulamasını kullanan kullanıcılar şirket ağının dışından geri bildirim uygulamasına erişemez. Bağlantı çevirisi [ve diğer yönlendirme seçenekleri](application-proxy-configure-hard-coded-link-translation.md)hakkında ayrıntılı bilgi için bkz.

### <a name="access-your-application"></a>Başvurunuza erişin

App Proxy'de yayınlanan kaynaklara erişimi yönetmek için çeşitli seçenekler vardır, bu nedenle verilen senaryo ve ölçeklenebilirlik gereksinimleriniz için en uygun seçeneği seçin. Yaygın yaklaşımlar şunlardır: Azure AD Connect üzerinden senkronize edilen şirket içi grupları kullanmak, kullanıcı özelliklerine göre Azure AD'de Dinamik Gruplar oluşturmak, bir kaynak sahibi tarafından yönetilen self servis gruplarını kullanmak veya bunların bir birleşimi. Her birinin yararları için bağlantılı kaynaklara bakın.

Kullanıcılara bir uygulamaya erişim atamanın en doğrudan yolu, yayınlanan uygulamanızın sol bölmesinden **Kullanıcılar ve Gruplar** seçeneklerine girmek ve doğrudan grup veya kişileri atamaktır.

![Resim 24](media/App-proxy-deployment-plan/add-user.png)

Ayrıca, kullanıcıların şu anda üyesi olmadıkları bir grup atayarak ve self servis seçeneklerini yapılandırarak uygulamanıza self servis erişimine izin verebilirsiniz.

![Resim 25](media/App-proxy-deployment-plan/allow-access.png)

Etkinleştirilirse, kullanıcılar MyApps portalına giriş yapıp erişim talebinde bulunabilecek ve otomatik olarak onaylanıp zaten izin verilen self servis grubuna eklenebilir veya belirlenmiş bir onaylayıcının onayına ihtiyaç duyar.

Konuk kullanıcılar ayrıca [Azure AD B2B üzerinden Application Proxy aracılığıyla yayınlanan dahili uygulamalara erişmeye de davet](https://docs.microsoft.com/azure/active-directory/b2b/add-users-information-worker)edilebilir.

Normalde anonim olarak erişilebilen ve kimlik doğrulaması gerektirmeyen şirket içi uygulamalarda, uygulamanın **Özellikleri'nde**bulunan seçeneği devre dışı bırakmayı tercih edebilirsiniz.

![Resim 26](media/App-proxy-deployment-plan/assignment-required.png)


Bu seçeneği Hayır olarak ayarlamak, kullanıcıların şirket içi uygulamaya Azure AD App Proxy üzerinden izin almadan erişmesine izin verir, bu nedenle dikkatli kullanın.

Uygulamanız yayınlandıktan sonra, harici URL'sini bir tarayıcıya veya 'deki [https://myapps.microsoft.com](https://myapps.microsoft.com/)simgesine yazarak erişilebilir olmalıdır.

### <a name="enable-pre-authentication"></a>Ön kimlik doğrulamayı etkinleştirme

Uygulamanızın harici URL üzerinden erişen Application Proxy aracılığıyla erişilebilir olduğunu doğrulayın. 

1. Azure **Active Directory** > **Enterprise uygulamalarına** > gidin**Tüm uygulamalar** ve yönetmek istediğiniz uygulamayı seçin.

2. **Uygulama Proxy'yi**seçin.

3. Kimlik **Doğrulama Öncesi** alanında, **Azure Etkin Dizini'ni**seçmek için açılır listeyi kullanın ve **Kaydet'i**seçin.

Ön kimlik doğrulaması etkinleştirildiğinde, Azure AD önce kimlik doğrulama için kullanıcılara meydan okur ve tek oturum açma yapılandırılırsa, uygulama izni verilmeden önce arka uç uygulaması da kullanıcıyı doğrular. Ön kimlik doğrulama modunu Passthrough'dan Azure AD'ye değiştirmek, harici URL'yi HTTPS ile de yapılandırır, bu nedenle başlangıçta HTTP için yapılandırılan tüm uygulamalar artık HTTPS ile güvenli hale gelecektir.

### <a name="enable-single-sign-on"></a>Tek Oturum Açma'yı etkinleştirme

SSO, kullanıcıların Azure AD'ye erişirken oturum açmaları gerektiğinden mümkün olan en iyi kullanıcı deneyimini ve güvenliğini sağlar. Bir kullanıcı önceden kimlik doğrulaması yaptıktan sonra, SSO, kullanıcı adına şirket içi uygulamaya kimlik doğrulayan Uygulama Proxy bağlayıcısı tarafından gerçekleştirilir. Arka uç uygulaması, oturum açma işlemlerini kullanıcının kendisiyişolarak işler. 

**Geçiş Seçeneği'ni** seçmek, kullanıcıların Azure AD'ye kimlik doğrulamasına gerek kalmadan yayımlanmış uygulamaya erişmesine olanak tanır.

SSO'yu gerçekleştirmek yalnızca Azure AD'nin bir kaynağa erişmek isteyen kullanıcıyı belirleyebildiği takdirde mümkündür, bu nedenle uygulamanız SSO'nun çalışması için erişim sırasında Azure AD'li kullanıcıların ön kimlik doğrulaması için yapılandırılmalıdır, aksi takdirde SSO seçenekleri devre dışı bırakılır.

Uygulamalarınızı yapılandırırken en uygun SSO yöntemini seçmenize yardımcı olmak için [Azure AD'deki uygulamalarda Tek](what-is-single-sign-on.md) oturum açma'yı okuyun.

###  <a name="working-with-other-types-of-applications"></a>Diğer uygulama türleri ile çalışma

Azure AD Application Proxy, Azure AD Kimlik Doğrulama Kitaplığımızı[(ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)veya Microsoft Kimlik Doğrulama Kitaplığımızı[(MSAL)](https://azure.microsoft.com/blog/start-writing-applications-today-with-the-new-microsoft-authentication-sdks/)kullanmak üzere geliştirilen uygulamaları da destekleyebilir. Kullanıcılar adına ön kimlik doğrulama gerçekleştirmek için istemci isteğinin üstbilgi bilgilerinde alınan Azure AD belirteçlerini tüketerek yerel istemci uygulamalarını destekler.

Uygulama Proxy'nin kullanılabilir [yapılandırmaları](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-claims-aware-apps) hakkında bilgi edinmek için [yerel ve mobil istemci uygulamalarını](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-native-client) ve talep tabanlı uygulamaları okuyun.

### <a name="use-conditional-access-to-strengthen-security"></a>Güvenliği güçlendirmek için Koşullu Erişimi Kullanma

Uygulama güvenliği, şirket içinde ve bulutta karmaşık tehditlere karşı korunabilen ve buna yanıt verebilen gelişmiş bir güvenlik özellikleri kümesi gerektirir. Saldırganlar genellikle zayıf, varsayılan veya çalınan kullanıcı kimlik bilgileri aracılığıyla şirket ağı erişimi elde eder.  Microsoft kimlik odaklı güvenlik, hem ayrıcalıklı hem de ayrıcalıklı olmayan kimlikleri yöneterek ve koruyarak çalınan kimlik bilgilerinin kullanımını azaltır.

Aşağıdaki özellikler Azure AD Uygulama Proxy'sini desteklemek için kullanılabilir:

* Kullanıcı ve konum tabanlı Koşullu Erişim: Konum [tabanlı Koşullu Erişim ilkelerine](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations)sahip bir IP adresi veya coğrafi konuma göre kullanıcı erişimini sınırlandırarak hassas verileri korumalı tutun.

* Aygıt tabanlı Koşullu Erişim: Yalnızca kayıtlı, onaylanmış ve uyumlu cihazların [aygıt tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications)ile şirket verilerine erişebilmesini sağlayın.

* Uygulama tabanlı Koşullu Erişim: Kullanıcı şirket ağında olmadığında çalışmanın durması gerekmez. [Kurumsal bulut ve şirket içi uygulamalara erişimi güvence](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) altına almak ve Koşullu Erişim ile kontrolü sağlamak.

* Risk Tabanlı Koşullu Erişim: İster şirket içinde ister bulutta olsun, tüm uygulamalara ve tüm kullanıcılara uygulanabilen [risk tabanlı Koşullu Erişim ilkesiyle](https://www.microsoft.com/cloud-platform/conditional-access) verilerinizi kötü amaçlı korsanlardan koruyun.

* Azure AD Erişim Paneli: Uygulama Proxy hizmetiniz dağıtılırken ve uygulamalar güvenli bir şekilde yayınlanmışsa, kullanıcılarınıza tüm uygulamalarını keşfetmek ve bunlara erişmek için basit bir merkez sunar. [Access Paneli](https://aka.ms/AccessPanelDPDownload)aracılığıyla yeni uygulamalara ve gruplara erişim isteğinde bulunma veya bu kaynaklara başkaları adına erişimi yönetme gibi self servis özellikleriyle üretkenliği artırın.

## <a name="manage-your-implementation"></a>Uygulamanızı yönetme

### <a name="required-roles"></a>Gerekli roller

Microsoft, Azure AD ile gerekli görevleri gerçekleştirmek için mümkün olan en az ayrıcalığı verme ilkesini savunur. [Kullanılabilen farklı Azure rollerini gözden geçirin](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve her kişiliğin gereksinimlerini karşılamak için doğru rolleri seçin. Bazı rollerin geçici olarak uygulanması ve dağıtım tamamlandıktan sonra kaldırılması gerekebilir.

| İş rolü| İş görevleri| Azure AD rolleri |
|---|---|---|
| Yardım masası yöneticisi | Genellikle son kullanıcının bildirdiği sorunları niteleme ve kullanıcıların parolalarını değiştirme, yenileme belirteçlerini geçersiz kılma ve hizmet durumunu izleme gibi sınırlı görevleri yerine getirmeyle sınırlıdır. | Yardım Masası Yöneticisi |
| Kimlik yöneticisi| App Proxy ile ilgili sorunları hata ayıklamak için Azure AD oturum açma raporlarını ve denetim günlüklerini okuyun.| Güvenlik okuyucusu |
| Uygulama sahibi| Kurumsal uygulamaların, uygulama kayıtlarının ve uygulama proxy ayarlarının tüm yönlerini oluşturun ve yönetin.| Uygulama Yöneticisi |
| Altyapı yöneticisi | Sertifika Rollover Sahibi | Uygulama Yöneticisi |

Güvenli bilgilere veya kaynaklara erişimi olan kişi sayısını en aza indirmek, kötü amaçlı bir aktörün yetkisiz erişim elde etme veya yetkili bir kullanıcının hassas bir kaynağı istemeden etkileme olasılığını azaltmaya yardımcı olur. 
 
Ancak, kullanıcıların yine de günlük ayrıcalıklı işlemleri gerçekleştirmeleri gerekir, bu nedenle Azure kaynaklarına isteğe bağlı ayrıcalıklı erişim sağlamak için tam zamanında (JIT) tabanlı [Ayrıcalıklı Kimlik Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ilkelerini uygulamak ve Azure AD, yönetimsel erişimi ve denetimi etkin bir şekilde yönetmeye yönelik önerilen yaklaşımımızdır.

### <a name="reporting-and-monitoring"></a>Raporlama ve izleme

Azure AD, [denetim günlükleri ve raporlar](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)aracılığıyla kuruluşunuzun uygulama kullanımı ve operasyonel durumu hakkında ek bilgiler sağlar. Application Proxy ayrıca Azure AD portalıve Windows Olay Günlükleri'ndeki bağlayıcıları izlemeyi de kolaylaştırır.

#### <a name="application-audit-logs"></a>Uygulama denetim günlükleri

Bu günlükler, Application Proxy ve cihaz ve uygulamaya erişen kullanıcı ile yapılandırılan uygulamalara giriş hakkında ayrıntılı bilgi sağlar. [Denetim günlükleri](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) Azure portalında ve dışa aktarma için [Denetim API'sında](https://docs.microsoft.com/graph/api/resources/directoryaudit?view=graph-rest-beta) bulunur. Ayrıca, [uygulamanız](../reports-monitoring/concept-usage-insights-report.md?context=azure/active-directory/manage-apps/context/manage-apps-context) için kullanım ve öngörü raporları da mevcuttur.

#### <a name="application-proxy-connector-monitoring"></a>Uygulama Proxy Konektörü izleme

Konektörler ve hizmet tüm yüksek kullanılabilirlik görevlerini halleder. Azure AD Portalı'ndaki Uygulama Proxy sayfasından bağlayıcılarınızın durumunu izleyebilirsiniz. Bağlayıcı bakımı hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#maintenance)bkz.

![Örnek: Azure AD Application Proxy bağlayıcıları](./media/application-proxy-connectors/app-proxy-connectors.png)

#### <a name="windows-event-logs-and-performance-counters"></a>Windows olay günlükleri ve performans sayaçları

Bağlayıcılar hem yönetici hem de oturum günlüklerine sahiptir. Yönetici günlükleri önemli olayları ve hatalarını içerir. Oturum günlükleri tüm hareketleri ve bunların işlem ayrıntılarını içerir. Günlükler ve sayaçlar daha fazla bilgi için Windows Olay Günlükleri'nde yer almaktadır [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#under-the-hood) Azure [Monitor'da olay günlüğü veri kaynaklarını yapılandırmak için](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events)bu öğreticiyi izleyin.

### <a name="troubleshooting-guide-and-steps"></a>Sorun giderme kılavuzu ve adımlar

Sorun [giderme](application-proxy-troubleshoot.md) kılavuzumuzla sık karşılaşılan sorunlar ve bunları nasıl çözeceğiniz hakkında daha fazla bilgi edinin. 

Aşağıdaki makaleler, destek kuruluşunuz için sorun giderme kılavuzları oluşturmak için de kullanılabilecek yaygın senaryoları kapsar. 

* [Uygulama sayfası görüntülenirken sorun oluşuyor](application-proxy-page-appearance-broken-problem.md)
* [Uygulamanın yüklenmesi çok uzun sürüyor](application-proxy-page-load-speed-problem.md)
* [Uygulama sayfasındaki bağlantılar çalışmıyor](application-proxy-page-links-broken-problem.md)
* [Uygulamam için açılacak bağlantı noktaları](application-proxy-connectivity-ports-how-to.md)
* [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı yok](application-proxy-connectivity-no-working-connector.md)
* [Azure portalda yapılandırma](application-proxy-config-how-to.md)
* [Uygulamamda çoklu oturum açmayı yapılandırma](application-proxy-config-sso-how-to.md)
* [Yönetici portalında uygulama oluştururken sorun oluşuyor](application-proxy-config-problem.md)
* [Kerberos Kısıtlanmış Temsilini Yapılandırma](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [PingAccess ile yapılandırma](application-proxy-back-end-ping-access-how-to.md)
* [“Bu Şirket Uygulamasına Erişilemiyor” hatası](application-proxy-sign-in-bad-gateway-timeout-error.md)
* [Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor](application-proxy-connector-installation-problem.md)
* [Oturum açma sorunu](application-sign-in-problem-on-premises-application-proxy.md)
