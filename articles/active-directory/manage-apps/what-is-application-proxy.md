---
title: Azure AD Uygulama Proxy ile şirket içi uygulamaları yayımlama
description: Şirket içi web uygulamalarını uzak kullanıcılara dışarıdan yayınlamak için Uygulama Proxy'sini neden kullandığınızı anlayın. Uygulama Proxy mimarisi, bağlayıcılar, kimlik doğrulama yöntemleri ve güvenlik avantajları hakkında bilgi edinin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 05/31/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7047dfd0f02ffe95dcacfdf4ddc014047a338513
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79481203"
---
# <a name="using-azure-ad-application-proxy-to-publish-on-premises-apps-for-remote-users"></a>Uzak kullanıcılar için şirket içi uygulamaları yayımlamak için Azure AD Application Proxy'yi kullanma

Azure Etkin Dizin (Azure AD), bulutta ve şirket içinde kullanıcıları, uygulamaları ve verileri korumak için birçok özellik sunar. Özellikle, Azure AD Uygulama Proxy özelliği, şirket içi web uygulamalarını dışarıdan yayınlamak isteyen BT uzmanları tarafından uygulanabilir. Dahili uygulamalara erişmeye ihtiyaç duyan uzak kullanıcılar bu uygulamalara güvenli bir şekilde erişebilir.

Ağınızın dışından dahili uygulamalara güvenli bir şekilde erişebilme özelliği, modern işyerinde daha da önemli hale gelir. BYOD (Kendi Cihazını Getir) ve mobil cihazlar gibi senaryolarda BT uzmanları iki hedefe ulaşmak için meydan okuyor:

* Son kullanıcıların her zaman ve her yerde üretken olmalarını sağlamak
* Kurumsal varlıkları her zaman koruyun

Birçok kuruluş, kaynaklar şirket ağlarının sınırları içinde bulunduğunda kontrol altında olduklarına ve korunduklarına inanır. Ancak günümüzün dijital işyerinde, bu sınır buluttaki yönetilen mobil cihazlar, kaynaklar ve hizmetlerle genişledi. Artık kullanıcılarınızın kimliklerini ve cihazlarında ve uygulamalarında depolanan verileri korumanın karmaşıklığını yönetmeniz gerekir.

Buluttaki Office 365 ve diğer SaaS uygulamalarına ve şirket içinde barındırılan web uygulamalarına erişmeihtiyacı olan kullanıcıları yönetmek için Azure AD'yi zaten kullanıyorsunuzdur. Azure AD'niz zaten varsa, şirket içi uygulamalarınıza sorunsuz ve güvenli erişim sağlamak için bu uygulamadan tek bir denetim düzlemi olarak yararlanabilirsiniz. Ya da belki hala buluta doğru bir hareket yapmayı düşünüyorsundur. Eğer öyleyse, Uygulama Proxy uygulayarak ve güçlü bir kimlik temeli oluşturma yolunda ilk adımı atarak buluta yolculuk başlayabilirsiniz.

Kapsamlı olmasa da, aşağıdaki liste, app proxy'yi karma bir arada yaşama senaryosunda uygulayarak etkinleştirebileceğiniz bazı şeyleri göstermektedir:

* Şirket içi web uygulamalarını DMZ olmadan basitleştirilmiş bir şekilde harici olarak yayımlama
* Bulutta ve şirket içinde cihazlar, kaynaklar ve uygulamalar arasında tek oturum açma (SSO) desteği
* Buluttaki ve şirket içi uygulamalar için çok faktörlü kimlik doğrulamayı destekleme
* Microsoft Cloud'un güvenliğiyle bulut özelliklerinden hızla yararlanın
* Kullanıcı hesabı yönetimini merkezileştirme
* Kimlik ve güvenlik denetimini merkezileştirme
* Grup üyeliğine dayalı uygulamalara kullanıcı erişimini otomatik olarak ekleme veya kaldırma

Bu makalede, Azure AD ve Application Proxy'nin uzak kullanıcılara nasıl tek bir oturum açma (SSO) deneyimi verdiği açıklanmaktadır. Kullanıcılar, VPN veya çift ev sahibi sunucular ve güvenlik duvarı kuralları olmadan şirket içi uygulamalara güvenli bir şekilde bağlanır. Bu makale, Uygulama Proxy'nin bulutun özelliklerini ve güvenlik avantajlarını şirket içi web uygulamalarınıza nasıl getirdiğini anlamanıza yardımcı olur. Ayrıca mümkün olan mimari ve topolojileri açıklar.

## <a name="remote-access-in-the-past"></a>Geçmişte uzaktan erişim

Daha önce, uzak kullanıcılar tarafından erişimi kolaylaştırırken iç kaynakları saldırganlardan korumak için kontrol düzleminizin tümü DMZ'deydi veya çevre ağındaydı. Ancak dmz'de dağıtılan VPN ve ters proxy çözümleri, şirket kaynaklarına erişmek için harici istemciler tarafından kullanılan bulut dünyasına uygun değildir. Genellikle aşağıdaki dezavantajları muzdarip:

* Donanım maliyetleri
* Güvenliğin korunması (yama, izleme bağlantı noktaları, vb.)
* Kullanıcıların kenarda kimlik doğrulaması
* Kullanıcıların çevre ağındaki web sunucularına doğrulanması
* VPN istemci yazılımının dağıtımı ve yapılandırması ile uzak kullanıcılar için VPN erişiminin sağlanması. Ayrıca, dmz'de etki alanına birleştirilmiş sunucuların bakımı, dış saldırılara karşı savunmasız olabilir.

Günümüzün bulutların ilk dünyasında Azure AD, ağınıza kimlerin ve nelerin girdiğini denetlemek için en uygun şekilde uygundur. Azure AD Application Proxy, SaaS uygulamaları ve kimlik sağlayıcıları gibi modern kimlik doğrulama ve bulut tabanlı teknolojilerle tümleşir. Bu tümleştirme, kullanıcıların uygulamalara her yerden erişmesini sağlar. App Proxy günümüzün dijital işyeri için daha uygun olmasının yanında, VPN ve ters proxy çözümlerine göre daha güvenli ve uygulanması daha kolay. Uzak kullanıcılar şirket içi uygulamalarınıza, Azure AD ile entegre edilmiş O365 ve diğer SaaS uygulamalarına eriştisikleri gibi erişebilir. Uygulama Ara Sunucusu ile çalışması için uygulamanızı değiştirmeniz veya güncelleştirmeniz gerekmez. Ayrıca, App Proxy güvenlik duvarınız aracılığıyla gelen bağlantıları açmanızı gerektirmez. App Proxy ile, sadece ayarlayın ve unutun.

## <a name="the-future-of-remote-access"></a>Uzaktan erişimin geleceği

Günümüzün dijital iş yerinde, kullanıcılar her yerde birden fazla cihaz ve uygulamayla çalışır. Tek sabit kullanıcı kimliğidir. Bu nedenle bugün güvenli bir ağın ilk adımı, [Azure AD'nin kimlik yönetimi](https://docs.microsoft.com/azure/security/security-identity-management-overview) yeteneklerini güvenlik kontrol uçağınız olarak kullanmaktır. Denetim düzleminiz olarak kimliği kullanan bir model genellikle aşağıdaki bileşenlerden oluşur:

* Kullanıcıları ve kullanıcıyla ilgili bilgileri izlemek için bir kimlik sağlayıcısı.
* Şirket kaynaklarına erişimi olan aygıtların listesini tutmak için aygıt dizini. Bu dizin, ilgili aygıt bilgilerini (örneğin, aygıt türü, bütünlük vb.) içerir.
* Bir kullanıcı nın ve cihazın güvenlik yöneticileri tarafından belirlenen ilkeye uygun olup olmadığını belirlemek için ilke değerlendirme hizmeti.
* Kuruluş kaynaklarına erişim izni verme veya reddetme yeteneği.

Azure AD, Uygulama Proxy ile şirket içinde ve bulutta yayınlanan web uygulamalarına erişmeihtiyacı duyan kullanıcıları izler. Bu uygulamalar için merkezi bir yönetim noktası sağlar. Gerekli olmamakla birlikte, Azure AD Koşullu Erişimi de etkinleştirmeniz önerilir. Kullanıcıların kimlik doğrulama ve erişim kazanma koşullarına yönelik koşulları tanımlayarak, doğru kişilerin uygulamalara erişmesini de sağlarsınız.

**Not:** Azure AD Application Proxy'nin, dahili kaynaklara erişilmesi gereken dolaşım (veya uzak) kullanıcılar için VPN veya ters proxy yerine yönelik olduğunu anlamak önemlidir. Şirket ağındaki dahili kullanıcılar için tasarlanmamıştır. Uygulama Proxy'yi gereksiz yere kullanan dahili kullanıcılar beklenmeyen ve istenmeyen performans sorunlarına neden olabilir.

![Azure Active Directory ve tüm uygulamalarınız](media/what-is-application-proxy/azure-ad-and-all-your-apps.png)

### <a name="an-overview-of-how-app-proxy-works"></a>App Proxy'nin nasıl çalıştığına genel bakış

Application Proxy, Azure portalında yapılandırdığınız bir Azure REKLAM hizmetidir. Azure Bulutu'nda, kuruluşunuzdaki bir dahili uygulama sunucusu URL'sine bağlanan harici bir HTTP/HTTPS URL bitiş noktası yayımlamanızı sağlar. Bu şirket içi web uygulamaları, tek bir oturum açmayı desteklemek için Azure AD ile tümleştirilebilir. Son kullanıcılar daha sonra şirket içi web uygulamalarına Office 365 ve diğer SaaS uygulamalarına erişdikleri şekilde erişebilir.

Bu özelliğin bileşenleri arasında bulutta çalışan Application Proxy hizmeti, şirket içi sunucuda çalışan hafif bir aracı olan Application Proxy bağlayıcısı ve kimlik sağlayıcısı olan Azure AD yer almaktadır. Her üç bileşen de, kullanıcıya şirket içi web uygulamalarına erişmek için tek bir oturum açma deneyimi sağlamak için birlikte çalışır.

Oturum açtıktan sonra, harici kullanıcılar masaüstü veya iOS/MAC aygıtlarından tanıdık bir URL veya [MyApps erişim paneli](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access) kullanarak şirket içi web uygulamalarına erişebilirler. Örneğin, App Proxy uzak masaüstüne uzaktan erişim ve tek oturum açma sağlayabilir, SharePoint siteleri, Tableau, Qlik, Web'de Outlook ve iş satırı (LOB) uygulamaları.

![Azure AD Application Proxy mimarisi](media/what-is-application-proxy/azure-ad-application-proxy-architecture.png)

### <a name="authentication"></a>Kimlik doğrulaması

Tek oturum açma için bir uygulamayı yapılandırmanın birkaç yolu vardır ve seçtiğiniz yöntem uygulamanızın kullandığı kimlik doğrulamasına bağlıdır. Application Proxy aşağıdaki uygulama türlerini destekler:

* Web uygulamaları
* Farklı cihazlardaki zengin uygulamalara maruz bırakmak istediğiniz Web API'leri
* Uzak Masaüstü Ağ Geçidi'nin arkasında barındırılan uygulamalar
* Active Directory Authentication Library (ADAL) ile entegre edilmiş zengin istemci uygulamaları

App Proxy, aşağıdaki yerel kimlik doğrulama protokolünü kullanan uygulamalarla çalışır:

* **[Tümleşik Windows Kimlik Doğrulama (IWA)](application-proxy-configure-single-sign-on-with-kcd.md).** IWA için, Uygulama Proxy bağlayıcıları Kerberos uygulamasına kullanıcıları doğrulamak için Kerberos Kısıtlı Delegasyonu (KCD) kullanır.

App Proxy ayrıca, üçüncü taraf tümleştirmesiyle veya belirli yapılandırma senaryolarında aşağıdaki kimlik doğrulama protokollerini destekler:

* [**Üstbilgi tabanlı kimlik doğrulaması.**](application-proxy-configure-single-sign-on-with-ping-access.md) Bu oturum açma yöntemi PingAccess adlı bir üçüncü taraf kimlik doğrulama hizmeti kullanır ve uygulama kimlik doğrulaması için üstbilgi kullandığında kullanılır. Bu senaryoda, kimlik doğrulama PingAccess tarafından işlenir.
* [**Formlar veya parola tabanlı kimlik doğrulama.**](application-proxy-configure-single-sign-on-password-vaulting.md) Bu kimlik doğrulama yöntemiyle, kullanıcılar uygulamaya ilk eriştiklerinde bir kullanıcı adı ve parolayla uygulama oturum unu oturum alar. Azure AD, ilk oturum açmadan sonra uygulamaya kullanıcı adı ve parola sağlar. Bu senaryoda, kimlik doğrulaması Azure AD tarafından işlenir.
* [**SAML kimlik doğrulaması.**](application-proxy-configure-single-sign-on-on-premises-apps.md) SAML tabanlı tek oturum açma, SAML 2.0 veya WS-Federation protokollerini kullanan uygulamalar için desteklenir. SAML tek oturum açma ile Azure AD, kullanıcının Azure AD hesabını kullanarak uygulamaya kimlik doğrulaması sağlar.

Desteklenen yöntemler hakkında daha fazla bilgi için [bkz.](what-is-single-sign-on.md#choosing-a-single-sign-on-method)

### <a name="security-benefits"></a>Güvenlik avantajları

Application Proxy ve Azure AD tarafından sunulan uzaktan erişim çözümü, müşterilerin yararlanabileceği çeşitli güvenlik avantajlarını destekler:

* **Kimlik doğrulaması erişim.** Uygulama Proxy en iyi yalnızca kimlik doğrulama bağlantıları ağınıza isabet emin olmak için [ön kimlik doğrulama](application-proxy-security.md#authenticated-access) ile uygulamaları yayınlamak için uygundur. Ön kimlik doğrulaması ile yayınlanan uygulamalarda, geçerli bir belirteç olmadan App Proxy hizmetinden şirket ortamınıza trafik geçişine izin verilmez. Ön kimlik doğrulama, doğası gereği, yalnızca kimlik doğrulamalı kimlikler arka uç uygulamasına erişebildiği için önemli sayıda hedeflenen saldırıyı engeller.
* **Koşullu Erişim**. Ağınıza bağlantılar kurulmadan önce daha zengin ilke denetimleri uygulanabilir. Koşullu Erişim ile, arka uç uygulamanızı vurmak için izin trafik kısıtlamaları tanımlayabilirsiniz. Konuma, kimlik doğrulama nın gücüne ve kullanıcı risk profiline göre oturum açmaları kısıtlayan ilkeler oluşturursunuz. Koşullu Erişim geliştikçe, Microsoft Cloud App Security (MCAS) ile tümleştirme gibi ek güvenlik sağlamak için daha fazla denetim ekleniyor. MCAS entegrasyonu, Koşullu Erişim ilkelerine dayalı olarak oturumları gerçek zamanlı olarak izlemek ve denetlemek için Koşullu Erişim'den yararlanarak [gerçek zamanlı izleme](application-proxy-integrate-with-microsoft-cloud-application-security.md) için şirket içi bir uygulamayı yapılandırmanızı sağlar.
* **Trafik sonlandırma**. Oturum arka uç sunucusuyla yeniden kurulurken, buluttaki Application Proxy hizmetinde arka uç uygulamasına yönelik tüm trafik sonlandırılır. Bu bağlantı stratejisi, arka uç sunucularınızın doğrudan HTTP trafiğine maruz kalınmadığı anlamına gelir. Güvenlik duvarınız saldırı altında olmadığından, hedeflenen DoS (hizmet reddi) saldırılarına karşı daha iyi korunurlar.
* **Tüm erişim giden.** Application Proxy bağlayıcıları yalnızca 80 ve 443 bağlantı noktaları üzerinde bulutta Application Proxy hizmetine giden bağlantıları kullanır. Gelen bağlantı olmadan, DMZ'de gelen bağlantılar veya bileşenler için güvenlik duvarı bağlantı noktalarını açmaya gerek yoktur. Tüm bağlantılar giden ve güvenli bir kanal üzerinden.
* **Güvenlik Analitiği ve Makine Öğrenimi (ML) tabanlı zeka.** Azure Etkin Dizin'in bir parçası olduğundan, Uygulama [Proxy'si Azure AD Kimlik Koruması'ndan](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) yararlanabilir [(Premium P2 lisanslama](https://azure.microsoft.com/pricing/details/active-directory/)gerektirir). Azure AD Kimlik Koruması, güvenlik teni alınan hesapları proaktif olarak tanımlamak için makine öğrenimi güvenlik zekasını [Microsoft'un Dijital Suçlar Birimi](https://news.microsoft.com/stories/cybercrime/index.html) ve [Microsoft Güvenlik Yanıt Merkezi'nden](https://www.microsoft.com/msrc) gelen veri akışlarıyla birleştirir. Kimlik Koruması, yüksek riskli oturum açmalara karşı gerçek zamanlı koruma sağlar. Virüslü cihazlardan, anonim ağların aracılığıyla veya bir oturumun risk profilini artırmak için atipik ve olası olmayan konumlardan erişimler gibi faktörleri dikkate alır. Bu risk profili gerçek zamanlı koruma için kullanılır. Bu raporların ve olayların çoğu, SIEM sistemlerinizle tümleştirme için bir API aracılığıyla zaten kullanılabilir.

* **Bir hizmet olarak uzaktan erişim.** Uzaktan erişim sağlamak için şirket içi sunucuları koruma ve düzeltme konusunda endişelenmenize gerek yoktur. Application Proxy Microsoft'un sahip olduğu bir internet ölçekli hizmettir, böylece her zaman en son güvenlik düzeltme emültleri ve yükseltmeleri olsun. Yamasız yazılım hala çok sayıda saldırıyı hesaba katsın. İç Güvenlik Bakanlığı'na [göre, hedeflenen saldırıların yüzde 85'i önlenebilir.](https://www.us-cert.gov/ncas/alerts/TA15-119A) Bu hizmet modeli yle, artık kenar sunucularınızı yönetmenin ağır yükünü taşımak ve gerektiğinde yamalamak için çabalamak zorunda değilsiniz.

* **Intune entegrasyonu**. Intune ile şirket trafiği kişisel trafikten ayrı olarak yönlendirilir. Application Proxy, şirket trafiğinin doğrulamasını sağlar. [Uygulama Proxy ve Intune Yönetilen Tarayıcı](https://docs.microsoft.com/intune/app-configuration-managed-browser#how-to-configure-application-proxy-settings-for-protected-browsers) özelliği, uzak kullanıcıların iOS ve Android cihazlardan dahili web sitelerine güvenli bir şekilde erişmesini sağlamak için birlikte kullanılabilir.

### <a name="roadmap-to-the-cloud"></a>Buluta yol haritası

Uygulama Proxy'sini uygulamanın bir diğer önemli avantajı da Azure AD'yi şirket içi ortamınıza genişletmektir. Aslında, App Proxy'yi uygulamak, kuruluşunuzu ve uygulamalarınızı buluta taşımada önemli bir adımdır. Buluta taşınarak ve şirket içi kimlik doğrulamadan uzaklaşarak, şirket içi ayak izinizi azaltır ve kontrol uçağınız olarak Azure AD'nin kimlik yönetimi yeteneklerini kullanırsınız. Varolan uygulamalarda en az veya hiç güncelleştirme yle, tek oturum açma, çok faktörlü kimlik doğrulama ve merkezi yönetim gibi bulut özelliklerine erişebilirsiniz. App Proxy için gerekli bileşenleri yükleme uzaktan erişim çerçevesi oluşturmak için basit bir işlemdir. Buluta geçerek, yüksek kullanılabilirlik ve olağanüstü durum kurtarma gibi en son Azure AD özelliklerine, güncelleştirmelerine ve işlevlerine erişebilirsiniz.

Uygulamalarınızı Azure AD'ye geçirme hakkında daha fazla bilgi edinmek [için Uygulamalarınızı Azure Etkin Dizin beyaz kağıdına geçirme makalesine](https://aka.ms/migrateapps/whitepaper) bakın.

## <a name="architecture"></a>Mimari

Aşağıdaki diyagram, azure AD kimlik doğrulama hizmetlerinin ve Uygulama Proxy'sinin son kullanıcılara şirket içi tek oturum açma uygulamaları sağlamak için birlikte nasıl çalıştığını genel olarak göstermektedir.

![Azure AD Application Proxy kimlik doğrulama akışı](media/what-is-application-proxy/azure-ad-application-proxy-authentication-flow.png)

1. Kullanıcı uygulamanın bitiş noktası üzerinden erişiminden sonra, kullanıcı Azure AD oturum açma sayfasına yönlendirilir. Koşullu Erişim ilkelerini yapılandırmışsanız, kuruluşunuzun güvenlik gereksinimlerine uyduğunuzdan emin olmak için şu anda belirli koşullar denetlenir.
2. Başarılı bir oturum açmadan sonra Azure AD, kullanıcının istemci aygıtına bir belirteç gönderir.
3. İstemci belirteci, kullanıcı ana adı (UPN) ve güvenlik soyadı (SPN) belirteci alır Uygulama Proxy hizmetine gönderir.
4. Application Proxy, Application Proxy [bağlayıcısı](application-proxy-connectors.md)tarafından alınan isteği iletir.
5. Bağlayıcı, kullanıcı adına gerekli olan ek kimlik doğrulamasını gerçekleştirir *(Kimlik doğrulama yöntemine bağlı olarak isteğe bağlı olarak),* uygulama sunucusunun dahili bitiş noktasını ister ve isteği şirket içi uygulamaya gönderir.
6. Uygulama sunucusundan gelen yanıt bağlayıcı aracılığıyla Application Proxy hizmetine gönderilir.
7. Yanıt, Uygulama Proxy hizmetinden kullanıcıya gönderilir.

|**Bileşen**|**Açıklama**|
|:-|:-|
|Uç Nokta|Bitiş noktası bir URL veya [son kullanıcı portalıdır.](end-user-experiences.md) Kullanıcılar harici bir URL'ye erişerek ağınızın dışındayken uygulamalara ulaşabilirler. Ağınızdaki kullanıcılar uygulamaya bir URL veya son kullanıcı portalı üzerinden erişebilir. Kullanıcılar bu uç noktalardan birine gittiklerinde, Azure AD'de kimlik doğrulaması yaparlar ve daha sonra bağlayıcı aracılığıyla şirket içi uygulamaya yönlendirilirler.|
|Azure AD|Azure AD, bulutta depolanan kiracı dizinini kullanarak kimlik doğrulamasını gerçekleştirir.|
|Uygulama Proxy hizmeti|Bu Uygulama Proxy hizmeti, Azure AD'nin bir parçası olarak bulutta çalışır. Oturum açma belirteci kullanıcıdan Uygulama Proxy Bağlayıcısı'na geçer. Application Proxy, istek üzerine erişilebilen tüm üstbilgileri ileter ve üstbilgileri protokolüne göre istemci IP adresine ayarlar. Proxy'ye gelen istek zaten bu üstbilgi varsa, istemci IP adresi üstbilginin değeri olan virgülle ayrılmış listenin sonuna eklenir.|
|Uygulama Proxy konektörü|Bağlayıcı, ağınızın içindeki bir Windows Server'da çalışan hafif bir aracıdır. Bağlayıcı, buluttaki Uygulama Proxy hizmeti ile şirket içi uygulama arasındaki iletişimi yönetir. Bağlayıcı yalnızca giden bağlantıları kullanır, böylece gelen bağlantı noktalarını açmanız veya DMZ'ye herhangi bir şey koymanız gerekmez. Bağlayıcılar devletsizdir ve gerektiğinde buluttan bilgi çeker. Bağlayıcılar hakkında yük bakiyesi ve kimlik doğrulaması gibi daha fazla bilgi için Azure [AD Uygulama Proxy bağlayıcılarını anlayın.](application-proxy-connectors.md)|
|Aktif Dizin (AD)|Etkin Dizin, etki alanı hesapları için kimlik doğrulaması gerçekleştirmek için şirket içinde çalışır. Tek oturum açma yapılandırıldığında, bağlayıcı gerekli herhangi bir ek kimlik doğrulaması gerçekleştirmek için AD ile iletişim kurar.|
|Şirket içi uygulama|Son olarak, kullanıcı şirket içi bir uygulamaya erişebilir.|

Azure AD Application Proxy, bulut tabanlı Application Proxy hizmeti ve şirket içi bağlayıcıdan oluşur. Bağlayıcı, Uygulama Proxy hizmetinden gelen istekleri dinler ve iç uygulamalara olan bağlantıları işler. Tüm iletişimlerin TLS üzerinden gerçekleştiğini ve her zaman Uygulama Proxy hizmetinin bağlayıcısı olarak ortaya çıktığını unutmayın. Diğer bir s., iletişim sadece giden. Bağlayıcı, tüm aramalar için Uygulama Proxy hizmetine kimlik doğrulamak için bir istemci sertifikası kullanır. Bağlantı güvenliğinin tek istisnası, istemci sertifikasının oluşturulduğü ilk kurulum adımıdır. Daha fazla bilgi için [başlık altında](application-proxy-security.md#under-the-hood) Uygulama Proxy bakın.

### <a name="application-proxy-connectors"></a>Uygulama Proxy Konektörleri

[Uygulama Proxy bağlayıcıları,](application-proxy-connectors.md) buluttaki Application Proxy hizmetine giden bağlantıyı kolaylaştıran şirket içinde dağıtılan hafif aracılardır. Bağlayıcılar arka uç uygulamasına erişimi olan bir Windows Server'a yüklenmelidir. Kullanıcılar, aşağıda gösterildiği gibi konektörler aracılığıyla trafiklerini uygulamalara ileten App Proxy bulut hizmetine bağlanır.

![Azure AD Application Proxy ağ bağlantıları](media/what-is-application-proxy/azure-ad-application-proxy-network-connections.png)

Konektör ile App Proxy hizmeti arasında kurulum ve kayıt aşağıdaki gibi gerçekleştirilir:

1. BT yöneticisi giden trafiğe 80 ve 443 bağlantı noktalarını açar ve bağlayıcı, App Proxy hizmeti ve Azure AD tarafından gereken birkaç URL'ye erişim sağlar.
2. Yönetici Azure portalına girer ve konektörü şirket içi bir Windows sunucusuna yüklemek için çalıştırılabilir çalışır.
3. Bağlayıcı, App Proxy hizmetini "dinlemeye" başlar.
4. Yönetici, şirket içi uygulamayı Azure AD'ye ekler ve kullanıcıların uygulamalarına bağlanması için gereken URL'ler gibi ayarları yapılandırır.

Daha fazla bilgi için azure [AD Uygulama Proxy dağıtımı planı'na](application-proxy-deployment-plan.md)bakın.

Artıklık ve ölçek için her zaman birden fazla bağlayıcı dağıtmanız önerilir. Konektörler, hizmetle birlikte tüm yüksek kullanılabilirlik görevlerini halleder ve dinamik olarak eklenebilir veya kaldırılabilir. Yeni bir istek her geldiğinde, kullanılabilir bağlayıcılardan birine yönlendirilir. Bir bağlayıcı çalışırken, hizmete bağlanırken etkin kalır. Bağlayıcı geçici olarak kullanılamıyorsa, bu trafiğe yanıt vermez. Kullanılmayan bağlayıcılar etkin olmayan olarak etiketlenir ve 10 günlük hareketsizlikten sonra kaldırılır.

Bağlayıcılar ayrıca, bağlayıcının daha yeni bir sürümü olup olmadığını öğrenmek için sunucuyu yoklar. Manuel güncelleştirme yapabiliyor olsanız da, Uygulama Proxy BağlayıcıSı Güncelleyici hizmeti çalışmaya devam eden bağlayıcılar otomatik olarak güncellenir. Birden çok bağlayıcısı olan kiracılar için otomatik güncelleştirmeler, ortamınızdaki kapalı kalma sürelerini önlemek için her grupta aynı anda bir bağlayıcıyı hedeflemektedir.

**Not**: RsS akışına abone olarak güncelleştirmeler yayımlandığında bildirilecek Uygulama Proxy [sürüm geçmişi sayfasını](application-proxy-release-version-history.md) izleyebilirsiniz.

Her Uygulama Proxy bağlayıcısı bir [bağlayıcı grubuna](application-proxy-connector-groups.md)atanır. Aynı konektör grubundaki konektörler, yüksek kullanılabilirlik ve yük dengelemesi için tek bir birim olarak hareket eder. Azure portalında yeni gruplar oluşturabilir, bunlara bağlayıcılar atayabilir ve belirli uygulamalara hizmet vermek üzere belirli bağlayıcılar atayabilirsiniz. Yüksek kullanılabilirlik için her bağlayıcı grubunda en az iki bağlayıcı olması önerilir.

Bağlayıcı grupları, aşağıdaki senaryoları desteklemeniz gerektiğinde yararlıdır:

* Coğrafi uygulama yayıncılığı
* Uygulama segmentasyonu/yalıtımı
* Bulutta veya şirket içinde çalışan web uygulamalarını yayımlama

Bağlayıcılarınızı nerede yükleyebileceğinizi seçme ve ağınızı en iyi duruma getirmek hakkında daha fazla bilgi için [Azure Active Directory Application Proxy'yi kullanırken Ağ topolojisi hususlarına](application-proxy-network-topology.md)bakın.

## <a name="other-use-cases"></a>Diğer kullanım örnekleri

Bu noktaya kadar, tüm bulut ve şirket içi uygulamalarınızda tek oturum açma sağlarken şirket içi uygulamaları dışarıdan yayınlamak için Uygulama Proxy'sini kullanmaya odaklandık. Ancak, App Proxy için kayda değer başka kullanım örnekleri vardır. Şunları içerir:

* **Güvenli bir şekilde REST API'leri yayınlayın.** İş mantığınız veya API'ler şirket içinde çalışıyorsa veya buluttaki sanal makinelerde barındırıldığında, Application Proxy API erişimi için genel bir bitiş noktası sağlar. API uç nokta erişimi, gelen bağlantı noktalarını gerektirmeden kimlik doğrulamayı ve yetkilendirmeyi denetlemenize olanak tanır. Intune kullanan masaüstü bilgisayarlar, iOS, MAC ve Android cihazlar için çok faktörlü kimlik doğrulama ve aygıt tabanlı Koşullu Erişim gibi Azure AD Premium özellikleri aracılığıyla ek güvenlik sağlar. Daha fazla bilgi edinmek için, Azure Etkin Dizin ve [API Yönetimi ile OAuth 2.0'ı kullanarak](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)yerel istemci [uygulamalarının proxy uygulamalarıyla etkileşimkurmasını nasıl etkinleştireceğimiz](application-proxy-configure-native-client-application.md) ve API'yi nasıl koruyacağınızı öğrenin.
* **Uzak Masaüstü Hizmetleri** **(RDS)**. Standart RDS dağıtımları açık gelen bağlantılar gerektirir. Ancak, [Application Proxy ile RDS dağıtımı](application-proxy-integrate-with-remote-desktop-services.md) bağlayıcı hizmeti çalıştıran sunucudan kalıcı giden bir bağlantı vardır. Bu şekilde, şirket içi uygulamaları Uzak Masaüstü Hizmetleri aracılığıyla yayımlayarak son kullanıcılara daha fazla uygulama sunabilirsiniz. Ayrıca, sınırlı sayıda iki aşamalı doğrulama ve RDS'ye Koşullu Erişim denetimleri kümesiyle dağıtımın saldırı yüzeyini azaltabilirsiniz.
* **WebSockets kullanarak bağlanan uygulamaları yayımlayın.** [Qlik Sense](application-proxy-qlik.md) ile destek Genel Önizleme'de dir ve gelecekte diğer uygulamalara genişletilir.
* **Yerel istemci uygulamalarının proxy uygulamalarıyla etkileşime girmesini etkinleştirin.** Web uygulamalarını yayımlamak için Azure AD Application Proxy'yi kullanabilirsiniz, ancak Azure AD Kimlik Doğrulama Kitaplığı (ADAL) ile yapılandırılan [yerel istemci uygulamalarını](application-proxy-configure-native-client-application.md) yayımlamak için de kullanılabilir. Yerel istemci uygulamaları, bir cihaza yüklendikleri için web uygulamalarından farklıdır, web uygulamalarına ise tarayıcı üzerinden erişilir.

## <a name="conclusion"></a>Sonuç

Çalışma şeklimiz ve kullandığımız araçlar hızla değişiyor. Daha fazla çalışanın kendi cihazlarını işe getirmesi ve Hizmet Olarak Yazılım (SaaS) uygulamalarının yaygın kullanımı yla, kuruluşların verilerini yönetme ve güvence altına alma şekilleri de gelişmelidir. Şirketler artık sadece kendi duvarları içinde faaliyet göstermekle kalmadılar, sınırlarını çevreleyen bir hendekle korunuyorlar. Veriler, hem şirket içi hem de bulut ortamlarında her zamankinden daha fazla konuma seyahat ediyor. Bu evrim, kullanıcıların üretkenliğini ve işbirliği yapma yeteneklerini artırmaya yardımcı olmuştur, ancak aynı zamanda hassas verilerin korunmasını daha zor hale getirir.

İster şu anda azure ad'ı ister karma bir birlikte yaşam senaryosunda kullanıcıları yönetmek için kullanıyor olun ister buluta yolculukunuzu başlatmakla ilgileniyor sanız, Azure AD Application Proxy'yi uygulamak, uzaktan kumanda sağlayarak şirket içi ayak iznizin boyutunu azaltmaya yardımcı olabilir bir hizmet olarak erişim.

Kuruluşlar aşağıdaki avantajlardan yararlanmak için bugün App Proxy'den yararlanmaya başlamalıdır:

* Geleneksel VPN veya diğer şirket içi web yayıncılığı çözümleri ve DMZ yaklaşımıyla ilişkili ek yükü olmadan şirket içi uygulamaları dışarıdan yayımlayın
* Office 365 veya diğer SaaS uygulamaları olsun ve şirket içi uygulamalar da dahil olmak üzere tüm uygulamalarda tek oturum açma
* Azure AD'nin yetkisiz erişimi önlemek için Office 365 telemetrisini kaldırdığı bulut ölçeğinde güvenlik
* Kurumsal trafiğin doğrulanmasını sağlamak için intune entegrasyonu
* Kullanıcı hesabı yönetiminin merkezileştirilmesi
* En son güvenlik düzeltme eklerine sahip olmak için otomatik güncelleştirmeler
* Yeni özellikler piyasaya sürüldükçe; saml tek oturum açma ve uygulama çerezleri daha ayrıntılı yönetimi için en son destek olmak

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD Uygulama Proxy'yi planlama, çalıştırma ve yönetme hakkında daha fazla bilgi için azure [AD Uygulama Proxy dağıtımı planı'na](application-proxy-deployment-plan.md)bakın.
* Canlı bir demo planlamak veya değerlendirme için ücretsiz 90 günlük deneme sürümü almak için [Bkz. Enterprise Mobility + Security ile başlarken.](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial)
