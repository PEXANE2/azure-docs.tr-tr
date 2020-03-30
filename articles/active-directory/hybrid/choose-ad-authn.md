---
title: Azure AD karma kimlik çözümleri için kimlik doğrulama
titleSuffix: Active Directory
description: Bu kılavuz, ORTA ve büyük kuruluşlarda Azure AD karma kimlik çözümlerini seçmekle görevli CEO'lara, CEO'lara, CISOs'lara, Baş Kimlik Mimarlarına, Kurumsal Mimarlara ve Güvenlik ve BT karar üreticilerine yardımcı olur.
keywords: ''
author: martincoetzer
ms.author: martinco
ms.date: 10/30/2019
ms.topic: article
ms.service: security
ms.subservice: security-fundamentals
ms.workload: identity
ms.openlocfilehash: e263ecde532a8aaed420932bf355910da201723e
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365842"
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Azure Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin

Doğru kimlik doğrulama yöntemini seçmek, uygulamalarını buluta taşımak isteyen kuruluşlar için ilk sorundur. Aşağıdaki nedenlerden dolayı bu kararı hafife almayın:

1. Buluta geçmek isteyen bir kuruluş için ilk karar.

2. Kimlik doğrulama yöntemi, bir kuruluşun buluttaki varlığının önemli bir bileşenidir. Tüm bulut verilerine ve kaynaklarına erişimi denetler.

3. Azure AD'deki diğer tüm gelişmiş güvenlik ve kullanıcı deneyimi özelliklerinin temelidir.

Kimlik BT güvenliğinin yeni denetim düzlemidir, bu nedenle kimlik doğrulama, kuruluşun yeni bulut dünyasına erişim korumasıdır. Kuruluşların güvenliklerini güçlendiren ve bulut uygulamalarını davetsiz misafirlere karşı güvende tutan bir kimlik kontrol düzlemine ihtiyacı vardır.

> [!NOTE]
> Kimlik doğrulama yönteminizi değiştirmek için planlama, sınama ve olası kapalı kalma süresi gerekir. [Aşamalı kullanıma sunulması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-staged-rollout) test etmek ve yavaş yavaş federasyondan bulut kimlik doğrulamasına geçirmek için harika bir yoldur.

### <a name="out-of-scope"></a>Kapsam dışı
Varolan bir şirket içi dizin ayak izi olmayan kuruluşlar bu makalenin odak noktası değildir. Genellikle, bu işletmeler yalnızca bulutta kimlik oluştururve bu da karma kimlik çözümü gerektirmez. Yalnızca bulutkimlikleri yalnızca bulutta bulunur ve şirket içi kimliklerle ilişkili değildir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Azure AD karma kimlik çözümü yeni denetim düzleminiz olduğunda, kimlik doğrulama bulut erişiminin temelidir. Doğru kimlik doğrulama yöntemini seçmek, Azure AD karma kimlik çözümü oluşturmada çok önemli bir ilk karardır. Buluttaki kullanıcıları da sağlayan Azure AD Connect kullanılarak yapılandırılan kimlik doğrulama yöntemini uygulayın.

Kimlik doğrulama yöntemini seçmek için, seçiminizi uygulamanın zamanını, varolan altyapıyı, karmaşıklığı ve maliyetini göz önünde bulundurmanız gerekir. Bu etkenler her kuruluş için farklıdır ve zaman içinde değişebilir.

>[!VIDEO https://www.youtube.com/embed/YtW2cmVqSEw]

Azure AD, karma kimlik çözümleri için aşağıdaki kimlik doğrulama yöntemlerini destekler.

### <a name="cloud-authentication"></a>Bulut kimlik doğrulaması
Bu kimlik doğrulama yöntemini seçtiğinizde, Azure AD kullanıcıların oturum açma işlemini işler. Sorunsuz tek oturum açma (SSO) ile birleştiğinde, kullanıcılar kimlik bilgilerini yeniden girmek zorunda kalmadan bulut uygulamalarında oturum açabilirler. Bulut kimlik doğrulaması ile iki seçenek arasından seçim yapabilirsiniz:

**Azure AD parolası karma senkronizasyonu.** Azure AD'deki şirket içi dizin nesneleri için kimlik doğrulamasını etkinleştirmenin en basit yolu. Kullanıcılar, ek bir altyapı dağıtmak zorunda kalmadan şirket içinde kullandıkları aynı kullanıcı adını ve parolayı kullanabilir. Kimlik Koruması ve [Azure AD Etki Alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)gibi Azure AD'nin bazı premium özellikleri, hangi kimlik doğrulama yöntemini seçerseniz seçin parola karma eşitleme gerektirir.

> [!NOTE]
> Parolalar hiçbir zaman açık metinde depolanamaz veya Azure AD'de geri döndürülebilir bir algoritmayla şifrelenmez. Gerçek parola karma eşitleme işlemi hakkında daha fazla bilgi için [bkz.](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)

**Azure AD Pass-through Kimlik Doğrulaması.** Bir veya daha fazla şirket içi sunucuda çalışan bir yazılım aracısı kullanarak Azure AD kimlik doğrulama hizmetleri için basit bir parola doğrulama sağlar. Sunucular kullanıcıları doğrudan şirket içi Active Dizininizle doğrular ve bu da parola doğrulamanın bulutta gerçekleşmemesini sağlar.

Şirket içi kullanıcı hesabı durumlarını, parola ilkelerini ve oturum açma saatlerini anında uygulamak için güvenlik gereksinimi olan şirketler bu kimlik doğrulama yöntemini kullanabilir. Gerçek geçiş kimlik doğrulama işlemi hakkında daha fazla bilgi için Azure [AD geçiş kimlik doğrulaması ile Kullanıcı oturum açma'ya](../../active-directory/hybrid/how-to-connect-pta.md)bakın.

### <a name="federated-authentication"></a>Federe kimlik doğrulama
Bu kimlik doğrulama yöntemini seçtiğinizde, Azure AD, kullanıcının parolasını doğrulamak için kimlik doğrulama işlemini şirket içi Active Directory Federation Services (AD FS) gibi ayrı bir güvenilir kimlik doğrulama sistemine teslim eder.

Kimlik doğrulama sistemi ek gelişmiş kimlik doğrulama gereksinimleri sağlayabilir. Örnekler akıllı kart tabanlı kimlik doğrulama veya üçüncü taraf çok faktörlü kimlik doğrulamavardır. Daha fazla bilgi için bkz: [Active Directory Federation Services dağıtma.](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide)

Aşağıdaki bölüm, bir karar ağacı kullanarak hangi kimlik doğrulama yönteminin sizin için doğru olduğuna karar vermenize yardımcı olur. Azure AD karma kimlik çözümünüz için bulut veya federe kimlik doğrulama dağıtılıp dağıtılmayacağını belirlemenize yardımcı olur.

## <a name="decision-tree"></a>Karar ağacı

![Azure AD kimlik doğrulama karar ağacı](./media/choose-ad-authn/azure-ad-authn-image1.png)

Karar soruları yla ilgili ayrıntılar:

1. Azure AD, parolaları doğrulamak için şirket içi bileşenlere güvenmeden kullanıcılar için oturum açma işlemlerini işleyebilir.
2. Azure AD, kullanıcı oturum açma oturumunu Microsoft'un AD FS'si gibi güvenilir bir kimlik doğrulama sağlayıcısına devredebilir.
3. Hesabın süresi dolmuş, devre dışı bırakılmış hesap, parola süresi dolmuş, hesap kilitlenmiş ve oturum açma saatleri gibi kullanıcı düzeyinde Etkin Dizin güvenlik ilkeleri uygulamanız gerekiyorsa, Azure AD bazı şirket içi bileşenler gerektirir.
4. Azure AD tarafından yerel olarak desteklenmeyen oturum açma özellikleri:
   * Akıllı kartları veya sertifikaları kullanarak oturum açın.
   * Şirket içi MFA Server'ı kullanarak oturum açın.
   * Üçüncü taraf kimlik doğrulama çözümlerini kullanarak oturum açma.
   * Çok yerinde şirket içi kimlik doğrulama çözümü.
5. Azure AD Kimlik Koruması, *Kullanıcılara sızdırılmış kimlik bilgileri* raporunu sağlamak için hangi oturum açma yöntemini seçerseniz seçin Parola Karma Eşitleme gerektirir. Birincil oturum açma yöntemi başarısız olursa ve hata olayından önce yapılandırılırsa, kuruluşlar Parola Hash Eşitlemesi'nde başarısız olabilir.

> [!NOTE]
> Azure AD Kimlik Koruması için [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/) lisansları gerekir.

## <a name="detailed-considerations"></a>Ayrıntılı hususlar

### <a name="cloud-authentication-password-hash-synchronization"></a>Bulut kimlik doğrulama: Parola karma senkronizasyonu

* **Çaba**. Parola karma eşitleme dağıtım, bakım ve altyapı ile ilgili en az çaba gerektirir.  Bu çaba düzeyi genellikle kullanıcılarının Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklarda oturum açmasını gerektiren kuruluşlar için geçerlidir. Etkinleştirildiğinde, parola karma eşitleme, Azure AD Connect eşitleme işleminin bir parçasıdır ve her iki dakikada bir çalışır.

* **Kullanıcı deneyimi.** Kullanıcıların oturum açma deneyimini geliştirmek için, sorunsuz SSO'ya parola karma senkronizasyonu ile dağıtın. Sorunsuz SSO, kullanıcılar oturum açtığında gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryolar**. Kuruluşlar isterse, Azure AD Kimlik Koruması raporlarıyla Azure AD Premium P2 ile kimlikbilgileri kullanmak mümkündür. Bir örnek, sızdırılan kimlik bilgileri raporudur. Windows Hello for [Business, parola karma senkronizasyonunu kullandığınızda belirli gereksinimlere](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)sahiptir. [Azure AD Etki Alanı Hizmetleri,](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) yönetilen etki alanında kullanıcıların kurumsal kimlik bilgilerini sağlamak için parola karma senkronizasyonu gerektirir.

    Parola karma eşitleme ile çok faktörlü kimlik doğrulaması gerektiren kuruluşlar Azure AD çok faktörlü kimlik doğrulaması veya [Koşullu Erişim özel denetimlerini](../../active-directory/conditional-access/controls.md#custom-controls-preview)kullanmalıdır. Bu kuruluşlar, federasyona dayanan üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemlerikullanamaz.

> [!NOTE]
> Azure AD Koşullu Erişim için [Azure AD Premium P1](https://azure.microsoft.com/pricing/details/active-directory/) lisansları gerekir.

* **İş sürekliliği.** Bulut kimlik doğrulamasıyla parola karma senkronizasyonu kullanmak, tüm Microsoft veri merkezlerine ölçeklenen bir bulut hizmeti olarak son derece kullanılabilir. Parola karma eşitlemenin uzun süre ler boyunca inmediğinden emin olmak için, bekleme yapılandırmasında hazırlama modunda ikinci bir Azure AD Connect sunucusu dağıtın.

* **Dikkat edilecek hususlar**. Şu anda, parola karma eşitleme şirket içi hesap durumlarında değişiklikleri hemen zorlamaz. Bu durumda, kullanıcı hesabı durumu Azure AD ile senkronize edilene kadar bir kullanıcı bulut uygulamalarına erişebilir. Kuruluşlar, yöneticilerin şirket içi kullanıcı hesabı durumlarında toplu güncelleştirmeler yaptıktan sonra yeni bir eşitleme döngüsü çalıştırarak bu sınırlamayı aşmak isteyebilir. Bir örnek hesapları devre dışı bırakmaktır.

> [!NOTE]
> Parolanın süresi doldu ve hesap kilitlenen durumlar şu anda Azure AD Connect ile Azure AD ile eşitlenmedi. Bir kullanıcının parolasını değiştirdiğinizde ve *kullanıcının bir sonraki oturum açma bayrağında parolayı değiştirmesi gerektiğini* ayarladığınızda, kullanıcı parolasını değiştirene kadar parola karması Azure AD Connect ile Azure AD ile eşitlenmez.

Dağıtım adımları için [parola karma eşitleme uygulamasına](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md) bakın.

### <a name="cloud-authentication-pass-through-authentication"></a>Bulut kimlik doğrulaması: Geçiş Kimlik Doğrulaması  

* **Çaba**. Geçiş kimlik doğrulaması için, varolan sunucularda yüklü bir veya daha fazla (üç tane tavsiye) hafif aracıya ihtiyacınız vardır. Bu aracılar, şirket içi AD etki alanı denetleyicileriniz de dahil olmak üzere şirket içi Active Directory Domain Services'ınıza erişebilmeli. Internet'e ve etki alanı denetleyicilerinize giden erişime ihtiyaçları vardır. Bu nedenle, aracıları bir çevre ağına yerleştirmek desteklenmez.

    Geçiş Kimlik Doğrulaması, etki alanı denetleyicilerine sınırlandırılmamış ağ erişimi gerektirir. Tüm ağ trafiği şifrelenir ve kimlik doğrulama istekleriyle sınırlıdır. Bu işlem hakkında daha fazla bilgi için, geçiş kimlik doğrulama güvenliği [derin dalış](../../active-directory/hybrid/how-to-connect-pta-security-deep-dive.md) bakın.

* **Kullanıcı deneyimi.** Kullanıcıların oturum açma deneyimini geliştirmek için, Geçiş Kimlik Doğrulaması ile sorunsuz SSO dağıtın. Sorunsuz SSO, kullanıcılar oturum açtıktan sonra gereksiz istemleri ortadan kaldırır.

* **Gelişmiş senaryolar**. Geçiş Kimlik Doğrulaması, oturum açma sırasında şirket içi hesap ilkesini zorlar. Örneğin, şirket içi bir kullanıcının hesap durumu devre dışı bırakıldığında, kilitlendiğinde veya [parolalarının süresi dolduğunda](../../active-directory/hybrid/how-to-connect-pta-faq.md#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) veya oturum açma denemesi kullanıcının oturum açmasına izin verildiği saatlerin dışına düştüğünde erişim reddedilir.

    Geçiş kimlik doğrulaması ile çok faktörlü kimlik doğrulaması gerektiren kuruluşların Azure Çok Faktörlü Kimlik Doğrulaması (MFA) veya [Koşullu Erişim özel denetimlerini](../../active-directory/conditional-access/controls.md#custom-controls-preview)kullanması gerekir. Bu kuruluşlar, federasyona dayanan bir üçüncü taraf veya şirket içi çok faktörlü kimlik doğrulama yöntemi kullanamaz. Gelişmiş özellikler, geçiş kimlik doğrulaması seçseniz de seçmeseniz de parola karma eşitlemenin dağıtılmasını gerektirir. Kimlik Koruması'nın sızdırılan kimlik bilgileri raporu, bir örnektir.

* **İş sürekliliği.** İki ekstra geçiş kimlik doğrulama aracısı dağıtmanızı öneririz. Bu ekstralar, Azure AD Connect sunucusundaki ilk temsilciye ek olarak verilir. Bu ek dağıtım, kimlik doğrulama isteklerinin yüksek kullanılabilirliğini sağlar. Üç aracı dağıtıldığında, başka bir aracı bakım için olmadığında bir aracı yine de başarısız olabilir.

    Geçiş kimlik doğrulamasına ek olarak parola karma eşitlemesi dağıtmanın başka bir yararı daha vardır. Birincil kimlik doğrulama yöntemi artık kullanılamıyorsa yedek kimlik doğrulama yöntemi gibi davranır.

* **Dikkat edilecek hususlar**. Aracılar önemli bir şirket içi hata nedeniyle kullanıcının kimlik bilgilerini doğrulayamadığında, parola karma eşitlemesini geçiş kimlik doğrulaması için yedek kimlik doğrulama yöntemi olarak kullanabilirsiniz. Parola karma senkronizasyonu nda başarısız olun otomatik olarak gerçekleşmez ve oturum açma yöntemini el ile değiştirmek için Azure AD Connect'i kullanmanız gerekir.

    Alternatif Kimlik desteği de dahil olmak üzere Geçiş Kimlik Doğrulama ile ilgili diğer hususlar için [sık sorulan sorulara](../../active-directory/hybrid/how-to-connect-pta-faq.md)bakın.

Dağıtım adımları için [geçiş kimlik doğrulaması uygulamasına](../../active-directory/hybrid/how-to-connect-pta.md) bakın.

### <a name="federated-authentication"></a>Federe kimlik doğrulama

* **Çaba**. Federe kimlik doğrulama sistemi, kullanıcıların kimliğini doğrulamak için harici bir güvenilen sisteme dayanır. Bazı şirketler, Azure AD karma kimlik çözümleriyle mevcut federe sistem yatırımlarını yeniden kullanmak ister. Federe sistemin bakımı ve yönetimi Azure AD'nin denetiminin dışında kalır. Güvenli bir şekilde dağıtıldıklarından ve kimlik doğrulama yükünü kaldırabilmek için federe sistemi kullanarak kuruluşa kalmış.

* **Kullanıcı deneyimi.** Federe kimlik doğrulama kullanıcı deneyimi özellikleri, topoloji ve federasyon çiftliğiyapılandırma uygulanmasına bağlıdır. Bazı kuruluşların, federasyon çiftliğine erişimi güvenlik gereksinimlerine uyacak şekilde uyarlamak ve yapılandırmak için bu esnekliğe ihtiyacı vardır. Örneğin, dahili olarak bağlanan kullanıcıları ve aygıtları, kimlik bilgileri istenmeden kullanıcılarda otomatik olarak oturum açabilmeleri için yapılandırmak mümkündür. Bu yapılandırma, aygıtlarında zaten oturum açmış oldukları için çalışır. Gerekirse, bazı gelişmiş güvenlik özellikleri kullanıcıların oturum açma işlemini zorlaştırır.

* **Gelişmiş senaryolar**. Müşteriler Azure AD'nin yerel olarak desteklemediği bir kimlik doğrulama gereksinimi olduğunda genellikle federe kimlik doğrulama çözümü gereklidir. Doğru oturum açma [seçeneğini seçmenize](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/)yardımcı olacak ayrıntılı bilgilere bakın. Aşağıdaki ortak gereksinimleri göz önünde bulundurun:

  * Akıllı kart veya sertifika gerektiren kimlik doğrulaması.
  * Şirket içi MFA sunucuları veya federe kimlik sağlayıcısı gerektiren üçüncü taraf çok faktörlü sağlayıcılar.
  * Üçüncü taraf kimlik doğrulama çözümleri kullanılarak kimlik doğrulama. Azure [AD federasyonu uyumluluk listesine](../../active-directory/hybrid/how-to-connect-fed-compatibility.md)bakın.
  * Örneğin, Kullanıcı Adı (UPN) yerine BIR SAMAccountName(örneğin, DOMAIN\username) user@domain.comgerektiren oturum açın.

* **İş sürekliliği.** Federe sistemler genellikle bir çiftlik olarak bilinen bir yük dengeli sunucular dizisi gerektirir. Bu çiftlik, kimlik doğrulama istekleri için yüksek kullanılabilirlik sağlamak için bir iç ağ ve çevre ağı topolojisi olarak yapılandırılır.

    Birincil kimlik doğrulama yöntemi artık kullanılamadığında, federe kimlik doğrulamayöntemiyle birlikte parola karma eşitlemesi dağıtın. Buna örnek olarak, şirket içi sunucular kullanılamadığında da buna bir örnektir. Bazı büyük kurumsal kuruluşlar, düşük gecikmeli kimlik doğrulama istekleri için coğrafi DNS ile yapılandırılan birden çok Internet giriş noktasını desteklemek için bir federasyon çözümü gerektirir.

* **Dikkat edilecek hususlar**. Federe sistemler genellikle şirket içi altyapıya daha önemli bir yatırım gerektirir. Çoğu kuruluş, şirket içi federasyon yatırımı varsa bu seçeneği seçer. Ve tek bir kimlik sağlayıcı kullanmak için güçlü bir iş gereksinimi ise. Federasyon, bulut kimlik doğrulama çözümlerine kıyasla çalışması ve sorun giderilmesi için daha karmaşıktır.

Azure AD'de doğrulanamayan bir etki alanı için, kullanıcı kimliği oturum açma yı uygulamak için ek yapılandırmagerekir. Bu gereksinim, Alternatif giriş kimliği desteği olarak bilinir. Sınırlamalar ve gereksinimler için [Alternatif Giriş Kimliğini Yapılandırma'ya](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) bakın. Federasyonlu bir üçüncü taraf çok faktörlü kimlik doğrulama sağlayıcısı kullanmayı seçerseniz, sağlayıcıların aygıtların Azure AD'ye katılmasına izin vermek için WS-Trust'ı desteklediğinden emin olun.

Dağıtım adımları için [Federasyon Sunucularını Dağıtmaya](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) bakın.

> [!NOTE]
> Azure AD karma kimlik çözümünüzü dağıttığınızda, Azure AD Connect'in desteklenen topolojilerinden birini uygulamanız gerekir. [Azure AD Connect için Topologies'de](../../active-directory/hybrid/plan-connect-topologies.md)desteklenen ve desteklenmeyen yapılandırmalar hakkında daha fazla bilgi edinin.

## <a name="architecture-diagrams"></a>Mimari diyagramları

Aşağıdaki diyagramlar, Azure AD karma kimlik çözümünüzle kullanabileceğiniz her kimlik doğrulama yöntemi için gereken üst düzey mimari bileşenlerini ana hatlar. Çözümler arasındaki farkları karşılaştırmanıza yardımcı olacak bir genel bakış sağlarlar.

* Parola karma senkronizasyon çözümünün basitliği:

    ![Parola karma senkronizasyonu ile Azure AD karma kimliği](./media/choose-ad-authn/azure-ad-authn-image2.png)

* Artıklık için iki aracı kullanarak, geçiş kimlik doğrulaması aracıgereksinimleri:

    ![Geçiş Kimlik Doğrulaması ile Azure AD karma kimliği](./media/choose-ad-authn/azure-ad-authn-image3.png)

* Kuruluşunuzun çevre ve iç ağında federasyon için gerekli bileşenler:

    ![Federe kimlik doğrulamaile Azure AD karma kimliği](./media/choose-ad-authn/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Yöntemleri karşılaştırma

|Değerlendirme|Şifre karma senkronizasyonu + Sorunsuz SSO|Geçiş Kimlik Doğrulama + Sorunsuz SSO|AD FS ile Federasyon|
|:-----|:-----|:-----|:-----|
|Kimlik doğrulama nerede gerçekleşir?|Bulutta|Şirket içi kimlik doğrulama aracısı ile güvenli bir parola doğrulama değişiminden sonra bulutta|Şirket içi|
|Sağlama sisteminin ötesindeki şirket içi sunucu gereksinimleri nelerdir: Azure AD Connect?|None|Her ek kimlik doğrulama aracısı için bir sunucu|İki veya daha fazla AD FS sunucusu<br><br>Çevre/DMZ ağında iki veya daha fazla WAP sunucusu|
|Sağlama sisteminin ötesinde şirket içi Internet ve ağ için gereksinimler nelerdir?|None|Kimlik doğrulama aracılarını çalıştıran sunuculardan [giden Internet erişimi](../../active-directory/hybrid/how-to-connect-pta-quick-start.md)|Çevredeki WAP sunucularına [gelen Internet erişimi](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-requirements)<br><br>Çevredeki WAP sunucularından AD FS sunucularına gelen ağ erişimi<br><br>Ağ yük dengeleme|
|TLS/SSL sertifika gereksinimi var mı?|Hayır|Hayır|Evet|
|Bir sağlık izleme çözümü var mı?|Gerekli değil|Azure Active [Directory yönetici merkezi](../../active-directory/hybrid/tshoot-connect-pass-through-authentication.md) tarafından sağlanan temsilci durumu|[Azure AD Connect Health](../../active-directory/hybrid/how-to-connect-health-adfs.md)|
|Kullanıcılar, şirket ağındaki etki alanına bağlı aygıtlardan bulut kaynaklarına tek oturum açma hakkı elde mi edin?|[Dikişsiz SSO](../../active-directory/hybrid/how-to-connect-sso.md) ile Evet|[Dikişsiz SSO](../../active-directory/hybrid/how-to-connect-sso.md) ile Evet|Evet|
|Hangi oturum açma türleri desteklenir?|UserPrincipalName + şifre<br><br>[Sorunsuz SSO](../../active-directory/hybrid/how-to-connect-sso.md) kullanarak Windows-Integrated Kimlik Doğrulama<br><br>[Alternatif giriş kimliği](../../active-directory/hybrid/how-to-connect-install-custom.md)|UserPrincipalName + şifre<br><br>[Sorunsuz SSO](../../active-directory/hybrid/how-to-connect-sso.md) kullanarak Windows-Integrated Kimlik Doğrulama<br><br>[Alternatif giriş kimliği](../../active-directory/hybrid/how-to-connect-pta-faq.md)|UserPrincipalName + şifre<br><br>sAMAccountName + şifre<br><br>Windows-Tümleşik Kimlik Doğrulama<br><br>[Sertifika ve akıllı kart kimlik doğrulaması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternatif giriş kimliği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Windows Hello for Business desteklendi mi?|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>*Windows Server 2016 Etki Alanı işlevsel düzeyi gerektirir*|[Anahtar güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Sertifika güven modeli](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Çok faktörlü kimlik doğrulama seçenekleri nelerdir?|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Koşullu Erişimli Özel Denetimler*](../../active-directory/conditional-access/controls.md)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Koşullu Erişimli Özel Denetimler*](../../active-directory/conditional-access/controls.md)|[Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/)<br><br>[Azure MFA sunucusu](../../active-directory/authentication/howto-mfaserver-deploy.md)<br><br>[Üçüncü taraf MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)<br><br>[Koşullu Erişimli Özel Denetimler*](../../active-directory/conditional-access/controls.md)|
|Hangi kullanıcı hesabı durumları desteklenir?|Devre dışı bırakılan hesaplar<br>(30 dakikaya kadar gecikme)|Devre dışı bırakılan hesaplar<br><br>Hesap kilitlendi<br><br>Hesabın süresi doldu<br><br>Parolanın süresi doldu<br><br>Oturum açma saatleri|Devre dışı bırakılan hesaplar<br><br>Hesap kilitlendi<br><br>Hesabın süresi doldu<br><br>Parolanın süresi doldu<br><br>Oturum açma saatleri|
|Koşullu Erişim seçenekleri nelerdir?|[Azure AD Premium ile Azure AD Koşullu Erişim](../../active-directory/conditional-access/overview.md)|[Azure AD Premium ile Azure AD Koşullu Erişim](../../active-directory/conditional-access/overview.md)|[Azure AD Premium ile Azure AD Koşullu Erişim](../../active-directory/conditional-access/overview.md)<br><br>[AD FS talep kuralları](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Eski protokolleri engellemek desteklendi mi?|[Evet](../../active-directory/conditional-access/overview.md)|[Evet](../../active-directory/conditional-access/overview.md)|[Evet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Oturum açma sayfalarındaki logoyu, resmi ve açıklamayı özelleştirebilir misiniz?|[Evet, Azure AD Premium ile](../../active-directory/fundamentals/customize-branding.md)|[Evet, Azure AD Premium ile](../../active-directory/fundamentals/customize-branding.md)|[Evet](../../active-directory/hybrid/how-to-connect-fed-management.md)|
|Hangi gelişmiş senaryolar desteklenir?|[Akıllı parola kilitleme](../../active-directory/authentication/howto-password-smart-lockout.md)<br><br>[Azure AD Premium P2 ile sızdırılan kimlik bilgileri raporları](../../active-directory/reports-monitoring/concept-risk-events.md)|[Akıllı parola kilitleme](../../active-directory/authentication/howto-password-smart-lockout.md)|Çok siteli düşük gecikmeli kimlik doğrulama sistemi<br><br>[AD FS extranet lokavt](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)<br><br>[Üçüncü taraf kimlik sistemleriyle entegrasyon](../../active-directory/hybrid/how-to-connect-fed-compatibility.md)|

> [!NOTE]
> Azure AD Koşullu Erişim'deki özel denetimler şu anda aygıt kaydını desteklemiyor.

## <a name="recommendations"></a>Öneriler
Kimlik sisteminiz, kullanıcılarınızın bulut uygulamalarına ve bulutta geçirip kullanıma sunabileceğiniz iş yeri uygulamalarına erişimini sağlar. Yetkili kullanıcıların üretken ve kötü aktörleri kuruluşunuzun hassas verilerinden uzak tutmak için kimlik doğrulama, uygulamalara erişimi denetler.

Aşağıdaki nedenlerle hangi kimlik doğrulama yöntemini seçerseniz seçin veya parola karma eşitlemesini etkinleştirin:

1. **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma.** Geçiş Kimlik Doğrulaması ve federasyon şirket içi altyapıya güvenir. Geçiş kimlik doğrulaması için, şirket içi ayak izi sunucu donanımını ve Geçiş Kimlik Doğrulama aracılarını gerektiren ağ ağlarını içerir. Federasyon için, şirket içi ayak izi daha da büyüktür. Kimlik doğrulama isteklerini ve dahili federasyon sunucularını proxy etmek için çevre ağınızdaki sunucular gerektirir.

    Tek bir hata noktasını önlemek için gereksiz sunucular dağıtın. Daha sonra herhangi bir bileşen başarısız olursa kimlik doğrulama istekleri her zaman hizmet edilecektir. Hem geçiş kimlik doğrulaması hem de federasyon, kimlik doğrulama isteklerini yanıtlamak için etki alanı denetleyicilerine güvenir ve bu da başarısız olabilir. Bu bileşenlerin çoğu sağlıklı kalmak için bakıma ihtiyaç duyar. Bakım planlanmadığında ve doğru şekilde uygulanmadığında kesintiler daha olasıdır. Microsoft Azure AD bulut kimlik doğrulama hizmeti genel olarak ölçeklendirildiği ve her zaman kullanılabilir olduğundan parola karma eşitlemesini kullanarak kesintilerden kaçının.

2. **Şirket içi kesinti hayatta kalma**.  Bir siber saldırı veya felaket nedeniyle şirket içi kesintinin sonuçları önemli olabilir, itibar marka hasar arasında değişen felçli bir kuruluş saldırı ile başa çıkamayan. Son zamanlarda, birçok kuruluş, hedefli ransomware de dahil olmak üzere kötü amaçlı yazılım saldırılarının kurbanı oldu, hangi onların şirket içi sunucular aşağı gitmek için neden oldu. Microsoft, müşterilerin bu tür saldırılarla başa çıkmada yardımcı olduğunda, iki kuruluş kategorisi görür:

   * Daha önce parola karma eşitlemesi açık kuruluşlar parola karma eşitleme kullanmak için kimlik doğrulama yöntemini değiştirdi. Birkaç saat içinde tekrar devreye girdiler. Office 365 üzerinden e-postaya erişimi kullanarak sorunları gidermek ve bulut tabanlı diğer iş yüklerine erişmek için çalıştılar.

   * Daha önce parola karma eşitlemesini etkinleştirmeyen kuruluşlar, sorunları çözmek için iletişim için güvenilmeyen dış tüketici e-posta sistemlerine başvurmak zorunda kaldı. Bu gibi durumlarda, kullanıcıların bulut tabanlı uygulamalarda yeniden oturum açabilmesi şirket içi kimlik altyapılarını geri yüklemeleri haftalar aldı.

3. **Kimlik koruması.** Buluttaki kullanıcıları korumanın en iyi yollarından biri, Azure AD Premium P2 ile Azure AD Kimlik Korumasıdır. Microsoft, kötü aktörlerin karanlık web'de sattığı ve kullanıma sundığı kullanıcı ve parola listeleri için Internet'i sürekli olarak tarar. Azure AD, kuruluşunuzdaki kullanıcı adlarından ve parolalardan herhangi birinin gizliliğinin ihlal edilip edilelmeyişa da geçerli olup olmadığını doğrulamak için bu bilgileri kullanabilir. Bu nedenle, ister federe ister geçiş kimlik doğrulaması olsun, hangi kimlik doğrulama yöntemini kullanırsanız kullanın parola karma senkronizasyonunu etkinleştirmek çok önemlidir. Sızdırılan kimlik bilgileri rapor olarak sunulur. Bu bilgileri, kullanıcıları sızdırılan parolalarla oturum açmaya çalıştıklarında parolalarını değiştirmeye zorlamak için kullanın.

## <a name="conclusion"></a>Sonuç

Bu makalede, kuruluşların bulut uygulamalarına erişimi desteklemek için yapılandırabileceği ve dağıtabileceği çeşitli kimlik doğrulama seçenekleri özetlenebilir. Çeşitli iş, güvenlik ve teknik gereksinimleri karşılamak için, kuruluşlar parola karma eşitleme, Geçiş Kimlik Doğrulama ve federasyon arasında seçim yapabilir.

Her kimlik doğrulama yöntemini göz önünde bulundurun. Çözümü dağıtma çabası ve kullanıcının oturum açma işlemi deneyimi iş gereksinimlerinizi karşılıyor mu? Kuruluşunuzun her kimlik doğrulama yönteminin gelişmiş senaryolarına ve iş sürekliliği özelliklerine ihtiyacı olup olmadığını değerlendirin. Son olarak, her kimlik doğrulama yönteminin dikkate değerlerini değerlendirin. Bunlardan herhangi biri seçiminizi uygulamanızı engelliyor mu?

## <a name="next-steps"></a>Sonraki adımlar

Günümüz dünyasında, tehditler günün 24 saati mevcut ve her yerden geliyor. Doğru kimlik doğrulama yöntemini uygularsanız, güvenlik risklerinizi azaltacak ve kimliklerinizi koruyacaktır.

Azure AD ile [başlayın](../../active-directory/fundamentals/get-started-azure-ad.md) ve kuruluşunuz için doğru kimlik doğrulama çözümlerini dağıtın.

Federeden bulut kimlik doğrulamasına geçiş yapmayı düşünüyorsanız, [oturum açma yöntemini değiştirme](../../active-directory/hybrid/plan-connect-user-signin.md)hakkında daha fazla bilgi edinin. Geçişi planlamanıza ve uygulamanıza yardımcı olmak [için, bu proje dağıtım planlarını](https://aka.ms/deploymentplans) kullanın veya federal kullanıcıları aşamalı bir yaklaşımla bulut kimlik doğrulaması kullanmaya geçirmek için yeni Aşamalı Kullanıma [Alma](../../active-directory/hybrid/how-to-connect-staged-rollout.md) özelliğini kullanmayı düşünün.
