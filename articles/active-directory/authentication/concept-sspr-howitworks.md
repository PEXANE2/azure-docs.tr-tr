---
title: Self servis parola sıfırlama derin dalış - Azure Active Directory
description: Self servis şifre sıfırlama nasıl çalışır?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b19c80378aa40a7f791a3eb61130b013217ddee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848587"
---
# <a name="how-it-works-azure-ad-self-service-password-reset"></a>Nasıl çalışır: Azure AD self servis parola sıfırlama

Self servis şifre sıfırlama (SSPR) nasıl çalışır? Bu seçenek arabirimde ne anlama geliyor? Azure Etkin Dizin (Azure AD) SSPR hakkında daha fazla bilgi edinmek için okumaya devam edin.

## <a name="how-does-the-password-reset-portal-work"></a>Parola sıfırlama portalı nasıl çalışır?

Bir kullanıcı parola sıfırlama portalına gittiğinde, aşağıdakileri belirlemek için bir iş akışı önerilir:

   * Sayfa nasıl yerelleştirilmelidir?
   * Kullanıcı hesabı geçerli mi?
   * Kullanıcı hangi kuruluşa aittir?
   * Kullanıcının şifresi nerede yönetilir?
   * Kullanıcı bu özelliği kullanma lisansına sahip mi?

Parola sıfırlama sayfasının arkasındaki mantık hakkında bilgi edinmek için aşağıdaki adımları okuyun:

1. Kullanıcı **hesap bağlantınıza erişemez'i** seçer veya [https://aka.ms/sspr](https://passwordreset.microsoftonline.com)doğrudan .
   * Tarayıcı yerel durumuna bağlı olarak, deneyim uygun dilde işlenir. Parola sıfırlama deneyimi, Office 365'in desteklediği dillerde yerelleştirilmiştir.
   * Farklı bir yerelleştirilmiş dilde parola sıfırlama portalını görüntülemek için parola sıfırlama URL'sinin sonuna "?mkt=" ekinde yerelleştirmeyi takip eden örnekle birlikte . [https://passwordreset.microsoftonline.com/?mkt=es-us](https://passwordreset.microsoftonline.com/?mkt=es-us)
2. Kullanıcı bir kullanıcı kimliği girer ve bir captcha geçer.
3. Azure AD, kullanıcının aşağıdaki denetimleri yaparak bu özelliği kullanabileceğini doğrular:
   * Kullanıcının bu özelliği etkinleştirip etkinleştirdiğini ve bir Azure AD lisansı atanmış olduğundan denetler.
     * Kullanıcı bu özelliği etkinleştirmediyse veya bir lisans atanmışsa, kullanıcıdan parolasını sıfırlamak için yöneticisiyle iletişime geçmesi istenir.
   * Kullanıcının hesabında yönetici ilkesine uygun olarak tanımlanan doğru kimlik doğrulama yöntemlerine sahip olduğunu denetler.
     * İlke yalnızca bir yöntem gerektiriyorsa, kullanıcının yönetici ilkesi tarafından etkinleştirilen kimlik doğrulama yöntemlerinden en az biri için tanımlanan uygun veriye sahip olmasını sağlar.
       * Kimlik doğrulama yöntemleri yapılandırılmamışsa, kullanıcının parolasını sıfırlamak için yöneticisiyle iletişime geçmesi önerilir.
     * İlke iki yöntem gerektiriyorsa, kullanıcının yönetici ilkesi tarafından etkinleştirilen kimlik doğrulama yöntemlerinden en az ikisi için tanımlanan uygun veriye sahip olmasını sağlar.
       * Kimlik doğrulama yöntemleri yapılandırılmamışsa, kullanıcının parolasını sıfırlamak için yöneticisiyle iletişime geçmesi önerilir.
     * Kullanıcıya bir Azure yöneticisi rolü atanmışsa, güçlü iki geçitli parola ilkesi zorlanır. Bu ilke hakkında daha fazla bilgi [bölümü Yönetici iyon farklılıkları sıfırlamak](concept-sspr-policy.md#administrator-reset-policy-differences)bulunabilir.
   * Kullanıcının parolasının şirket içinde yönetilip yönetilmediğini (federe, geçiş kimlik doğrulaması veya parola karma senkronize) denetler.
     * Writeback dağıtılırsa ve kullanıcının parolası şirket içinde yönetilirse, kullanıcının kimliğini doğrulamasına ve parolasını sıfırlamasına izin verilir.
     * Writeback dağıtılmazsa ve kullanıcının parolası şirket içinde yönetiliyorsa, kullanıcıdan parolasını sıfırlamak için yöneticisiyle iletişime geçmesi istenir.
4. Kullanıcının parolasını başarıyla sıfırlayabildiği tespit edilirse, kullanıcı sıfırlama işlemi boyunca yönlendirilir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinse, kimlik doğrulama yöntemleri için aşağıdaki seçeneklerden en az birini seçmeniz gerekir. Bazen "kapılar" olarak adlandırılan bu seçenekleri duyarsınız. Kullanıcılarınızın ihtiyaç duyduklarında bir tanesine erişememesi durumunda daha fazla esnekliğe sahip olmaları için **iki veya daha fazla kimlik doğrulama yöntemi seçmenizi** öneririz. Aşağıda listelenen yöntemler le ilgili ek ayrıntılar makalede kimlik [doğrulama yöntemleri nelerdir?](concept-authentication-methods.md)

* Mobil uygulama bildirimi
* Mobil uygulama kodu
* Email
* Cep telefonu
* Ofis telefonu
* Güvenlik soruları

Kullanıcılar parolalarını yalnızca yöneticinin etkinleştirdiği kimlik doğrulama yöntemlerinde veri varsa sıfırlayabilirler.

> [!IMPORTANT]
> Mart 2019'dan itibaren telefon görüşmesi seçenekleri, ücretsiz/deneme Azure AD kiracılarında MFA ve SSPR kullanıcıları tarafından kullanılamaz. SMS mesajları bu değişiklikle etkilenmez. Telefon görüşmesi, ücretli Azure AD kiracılarında bulunan kullanıcılar tarafından kullanılabilir olmaya devam edecektir. Bu değişiklik yalnızca ücretsiz/deneme Azure AD kiracılarını etkiler.

> [!WARNING]
> Azure Yöneticisi rolleri atanan [hesapların, Yönetici ilke farklılıklarını sıfırlama](concept-sspr-policy.md#administrator-reset-policy-differences)bölümünde tanımlandığı şekilde yöntemleri kullanması gerekir.

![Azure portalında kimlik doğrulama yöntemleri seçimi][Authentication]

### <a name="number-of-authentication-methods-required"></a>Gerekli kimlik doğrulama yöntemlerinin sayısı

Bu seçenek, bir kullanıcının parolasını sıfırlamak veya kilidini açmak için geçmesi gereken kullanılabilir kimlik doğrulama yöntemlerinin veya kapıların en az sayısını belirler. Bir veya iki olarak ayarlanabilir.

Yönetici bu kimlik doğrulama yöntemini etkinleştirilerse, kullanıcılar daha fazla kimlik doğrulama yöntemi sağlamayı seçebilir.

Bir kullanıcı kayıtlı en az gerekli yöntemleri yoksa, onları bir yöneticinin parolasını sıfırlamasını istemeye yönlendiren bir hata sayfası görür.

#### <a name="mobile-app-and-sspr"></a>Mobil uygulama ve SSPR

Parola sıfırlama yöntemi olarak Microsoft Authenticator uygulaması gibi bir mobil uygulamayı kullanırken aşağıdaki uyarılara dikkat edmelisiniz:

* Yöneticiler parolayı sıfırlamak için bir yöntem kullanılmasını gerektirdiğinde, kullanılabilir tek seçenek doğrulama kodudur.
* Yöneticiler parolayı sıfırlamak için iki yöntem inkullanılmasını gerektirdiğinde, kullanıcılar diğer etkin yöntemlere ek olarak **bildirim** **veya** doğrulama kodunu kullanabilir.

| Sıfırlamak için gereken yöntem sayısı | Bir | İki |
| :---: | :---: | :---: |
| Mobil uygulama özellikleri kullanılabilir | Kod | Kod veya Bildirim |

Kullanıcılar self servis şifre sıfırlama için kayıt olurken mobil uygulamalarını kaydetme [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)seçeneğine sahip değildir. Kullanıcılar mobil uygulamalarını [https://aka.ms/mfasetup](https://aka.ms/mfasetup)yeni güvenlik bilgileri kaydı önizlemesinde [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)veya yeni güvenlik bilgileri kaydı önizlemesinde kaydedebilirler.

> [!WARNING]
> Kullanıcıların yeni deneyime erişebilmesi [için self servis parola sıfırlama ve Azure Çok Faktörlü Kimlik Doğrulama (Genel Önizleme) için Yakınsama kaydını](concept-registration-mfa-sspr-converged.md) etkinleştirmeniz [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)gerekir.

> [!IMPORTANT]
> Kimlik doğrulayıcı uygulaması, 1-gate ilkesini yapılandırırken tek kimlik doğrulama yöntemi olarak seçilemez. Benzer şekilde, 2 kapılı bir ilke yapılandırırken kimlik doğrulayıcı uygulaması ve yalnızca bir ek yöntem seçilemez.
> Daha sonra, kimlik doğrulayıcı uygulamasını bir yöntem olarak içeren SSPR ilkelerini yapılandırırken, 1 kapılı ilke yapılandırırken en az ek bir yöntem seçilmelidir ve 2 kapılı ilke yapılandırırken en az iki ek yöntem seçilmelidir.
> Bu gereksinimin nedeni, geçerli SSPR kayıt deneyiminin kimlik doğrulayıcı uygulamasını kaydetme seçeneğini içermemesidir. Kimlik doğrulayıcı uygulamasını kaydetme seçeneği, [self servis parola sıfırlama ve Azure Çok Faktörlü Kimlik Doğrulama (Genel Önizleme) için yeni Yakınsama kaydına](concept-registration-mfa-sspr-converged.md)dahildir.
> Yalnızca kimlik doğrulayıcı uygulamasını (1 kapılı ilkeler için) veya kimlik doğrulayıcı uygulamasını ve yalnızca bir ek yöntemi (2 kapılı ilkeler için) kullanan ilkelere izin vermek, kullanıcıların yeni uygulamayı kullanacak şekilde yapılandırılına kadar SSPR'ye kaydolmalarının engellenmesine neden olabilir kayıt deneyimi.

### <a name="change-authentication-methods"></a>Kimlik doğrulama yöntemlerini değiştirme

Sıfırlama veya kayıtlı kilidini açmak için yalnızca bir gerekli kimlik doğrulama yöntemi olan bir ilkeyle başlarsanız ve bunu iki yöntemle değiştirirseniz, ne olur?

| Kayıtlı yöntem sayısı | Gerekli yöntem sayısı | Sonuç |
| :---: | :---: | :---: |
| 1 veya daha fazla | 1 | Sıfırlama veya kilidini **açabilme** |
| 1 | 2 | Sıfırlayamıyor veya kilidini **açamıyor** |
| 2 veya daha fazla | 2 | Sıfırlama veya kilidini **açabilme** |

Bir kullanıcının kullanabileceği kimlik doğrulama yöntemleritürlerini değiştirirseniz, kullanıcıların kullanılabilir minimum veri miktarına sahip değillerse yanlışlıkla SSPR'yi kullanabilmelerini engelleyebilirsiniz.

Örnek:
1. Özgün ilke, gerekli iki kimlik doğrulama yöntemiyle yapılandırılır. Sadece ofis telefon numarasını ve güvenlik sorularını kullanır. 
2. Yönetici, güvenlik sorularını artık kullanmamak için ilkeyi değiştirir, ancak bir cep telefonu ve alternatif bir e-posta kullanımına izin verir.
3. Cep telefonu veya alternatif e-posta alanları olmayan kullanıcılar parolalarını sıfırlayamamaktadır.

## <a name="registration"></a>Kayıt

### <a name="require-users-to-register-when-they-sign-in"></a>Kullanıcıların oturum verirken kaydolmalarını zorunlu kılmasını zorunlu kılmaktadır

Bu seçeneği etkinleştirmek için, kullanıcının Azure AD kullanarak herhangi bir uygulamada oturum açmaları durumunda parola sıfırlama kaydını tamamlaması gerekmektedir. Bu iş akışı aşağıdaki uygulamaları içerir:

* Office 365
* Azure portalında
* Erişim Paneli
* Federe uygulamalar
* Azure AD'yi kullanarak özel uygulamalar

Kayıt gerektiren devre dışı bırakıldığında, kullanıcılar el ile kaydolabilir. Erişim Paneli'ndeki **Profil** sekmesi nin altındaki **parola sıfırlama** bağlantısını ziyaret [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) edebilir veya seçebilirler.

> [!NOTE]
> Kullanıcılar **iptal** seçeneğini seçerek veya pencereyi kapatarak parola sıfırlama kayıt portalını kapatabilirler. Ancak, kayıt işlemlerini tamamlayana kadar her oturum açtıklarında kayıt yaptırmaları istenir.
>
> Bu kesinti, zaten oturum açmışsa kullanıcının bağlantısını kesmez.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Kullanıcılardan kimlik doğrulama bilgilerini yeniden onaylamaları istenmeden önceki gün sayısını ayarlama

Bu seçenek, kimlik doğrulama bilgilerini ayarlama ve yeniden onaylama arasındaki süreyi belirler ve yalnızca oturum **açma seçeneğinde kullanıcıların kaydolmasını etkinleştirdiğinizde** kullanılabilir.

Geçerli değerler 0 ile 730 gün arasındadır ve "0" kullanıcıların kimlik doğrulama bilgilerini yeniden onaylamaları istenmez.

## <a name="notifications"></a>Bildirimler

### <a name="notify-users-on-password-resets"></a>Parola sıfırlamayı kullanıcılara bildir

Bu seçenek **Evet**olarak ayarlanırsa, parolalarını sıfırlayan kullanıcılar, parolalarının değiştirildiğini bildiren bir e-posta alır. E-posta, SSPR portalı aracılığıyla Azure AD'de dosyada bulunan birincil ve alternatif e-posta adreslerine gönderilir. Sıfırlama olayı başka kimseye bildirilmez.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Diğer yöneticiler parolalarını sıyrtığında tüm yöneticileri bilgilendirin

Bu seçenek **Evet**olarak ayarlanırsa, *tüm yöneticiler* Azure AD'deki dosyadaki birincil e-posta adreslerine bir e-posta alır. E-posta, başka bir yöneticinin SSPR kullanarak parolalarını değiştirdiğini belirtir.

Örnek: Bir ortamda dört yönetici vardır. Yönetici A, SSPR kullanarak parolalarını sıfırlar. B, C ve D yöneticileri parola sıfırlama konusunda onları uyaran bir e-posta alır.

## <a name="on-premises-integration"></a>Şirket içi tümleştirme

Azure AD Connect'i yükler, yapılandırmak ve etkinleştirmek için şirket içi tümleştirmeler için aşağıdaki ek seçeneklere sahip olursunuz. Bu seçenekler gri renkteyse, yazma düzgün şekilde yapılandırılmamıştır. Daha fazla bilgi için [bkz.](howto-sspr-writeback.md)

![Parola yazma nın doğrulanması etkin ve çalışıyor][Writeback]

Bu sayfa, şirket içi geri yazma istemcisinin hızlı bir durumunu sağlar, aşağıdaki iletilerden biri geçerli yapılandırmaya göre görüntülenir:

* Şirket içi yazma istemciniz çalışır durumda.
* Azure AD çevrimiçidir ve şirket içi geri yazma istemcinize bağlıdır. Ancak, Azure AD Connect'in yüklenen sürümü güncel değil gibi görünüyor. En son bağlantı özelliklerine ve önemli hata düzeltmelerine sahip olduğundan emin olmak için [Azure AD Bağlantısını Yükseltmeyi](../hybrid/how-to-upgrade-previous-version.md) düşünün.
* Ne yazık ki, Azure AD Connect'in yüklü sürümü güncel olmadığından şirket içi geri yazma istemci durumunuzu kontrol edemeyiz. Bağlantı durumunuzu kontrol edebilmek için [Azure AD Connect'i yükseltin.](../hybrid/how-to-upgrade-previous-version.md)
* Ne yazık ki, şu anda şirket içi yazma istemcinle bağlantı kuramıyoruz. Bağlantıyı geri yüklemek için [Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) sorun giderin.
* Ne yazık ki, parola yazma düzgün yapılandırılmamışolduğundan, şirket içi yazma istemcinize bağlanabiliyoruz. Bağlantıyı geri yüklemek için [parola yazmayı yeniden yapılandırın.](howto-sspr-writeback.md)
* Ne yazık ki, şu anda şirket içi yazma istemcinle bağlantı kuramıyoruz. Bunun nedeni bizim tarafımızdaki geçici sorunlar olabilir. Sorun devam ederse, bağlantıyı geri yüklemek için [Azure AD Connect sorun giderin.](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity)

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Şirket içi dizininize parolaları geri yazma

Bu denetim, bu dizini için parola yazma etkin olup olmadığını belirler. Writeback üzerindeyse, şirket içi geri yazma hizmetinin durumunu gösterir. Bu denetim, Azure AD Connect'i yeniden yapılandırmak zorunda kalmadan parola yazma işlemlerini geçici olarak devre dışı kılmış sayılsa yararlıdır.

* Anahtar **Evet**olarak ayarlanırsa, yazma geri etkinleştirilir ve federe, geçiş kimlik doğrulaması veya parola karma senkronize kullanıcılar parolalarını sıfırlayabiliyor.
* Anahtar **Hayır**olarak ayarlanmışsa, yazma geri devre dışı bırakılır ve federe, geçiş kimlik doğrulaması veya parola karma senkronize kullanıcılar parolalarını sıfırlayamaz.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Kullanıcıların parolalarını sıfırlamadan hesapların kilidini açmalarına izin verme

Bu denetim, parola sıfırlama portalını ziyaret eden kullanıcılara parolalarını sıfırlamak zorunda kalmadan şirket içi Active Directory hesaplarının kilidini açma seçeneğinin verilip verilmemesi gerektiğini belirtir. Varsayılan olarak, Azure AD parola sıfırlama gerçekleştirirken hesapların kilidini açar. Bu iki işlemi ayırmak için bu ayarı kullanırsınız.

* **Evet**olarak ayarlanırsa, kullanıcılara parolalarını sıfırlama ve hesabın kilidini açma veya parolayı sıfırlamak zorunda kalmadan hesaplarının kilidini açma seçeneği verilir.
* **Hayır**olarak ayarlanırsa, kullanıcılar yalnızca birleştirilmiş parola sıfırlama ve hesap kilidini açma işlemi gerçekleştirebilir.

### <a name="on-premises-active-directory-password-filters"></a>Şirket içi Active Directory parola filtreleri

Azure AD self servis parola sıfırlama, Etkin Diziliş'te yönetici tarafından başlatılan parola sıfırlama eşdeğerini gerçekleştirir. Özel parola kurallarını uygulamak için bir üçüncü taraf parola filtresi kullanıyorsanız ve bu parola filtresinin Azure AD self servis parola sıfırlama sırasında denetlenmesini istiyorsanız, üçüncü taraf parola filtresi çözümmetninde uygulanacak şekilde yapılandırıldığından emin olun admin şifre sıfırlama senaryosu. [Windows Server Active Directory için Azure AD parola koruması](concept-password-ban-bad-on-premises.md) varsayılan olarak desteklenir.

## <a name="password-reset-for-b2b-users"></a>B2B kullanıcıları için parola sıfırlama

Parola sıfırlama ve değiştirme, tüm işletmelerarası (B2B) yapılandırmalarında tam olarak desteklenir. B2B kullanıcı parolası sıfırlama aşağıdaki üç durumda desteklenir:

* **Varolan bir Azure AD kiracısına sahip bir iş ortağı kuruluşunun kullanıcıları**: İş ortaklığı yaptığınız kuruluşun varolan bir Azure AD kiracısı varsa, *bu kiracıda etkinleştirilen parola sıfırlama ilkelerine saygı*duyarız. Parola sıfırlamanın çalışması için iş ortağı kuruluşun Azure AD SSPR'sinin etkin olduğundan emin olması yeterlidir. Office 365 müşterileri için ek ücret alınmaz ve [parola yönetimi kılavuzuyla](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) başlat'ımızdaki adımları izleyerek etkinleştirilebilir.
* Self servis kayıt **yoluyla kaydolan kullanıcılar:** Ortak olduğunuz kuruluş kiracıya girmek için [self servis kayıt](../users-groups-roles/directory-self-service-signup.md) özelliğini kullandıysa, kaydettikleri e-postayla parolayı sıfırlamalarına izin veririz.
* **B2B kullanıcıları**: Yeni [Azure AD B2B özelliklerini](../active-directory-b2b-what-is-azure-ad-b2b.md) kullanarak oluşturulan yeni B2B kullanıcıları, davet işlemi sırasında kaydettikleri e-postayla parolalarını sıfırlayabiletir.

Bu senaryoyu sınamak https://passwordreset.microsoftonline.com için bu ortak kullanıcılardan birine gidin. Alternatif bir e-posta veya kimlik doğrulama e-postası tanımlanmışsa, parola sıfırlama beklendiği gibi çalışır.

> [!NOTE]
> Azure AD kiracınıza (Hotmail.com, Outlook.com veya diğer kişisel e-posta adresleri gibi konuk erişimi verilen Microsoft hesapları Azure AD SSPR'sini kullanamaz. [Microsoft hesap makalenizde oturum açamadığınız zaman'da](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) bulunan bilgileri kullanarak parolalarını sıfırlamaları gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, Azure AD aracılığıyla parola sıfırlama konusunda ek bilgiler sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Authentication]: ./media/concept-sspr-howitworks/manage-authentication-methods-for-password-reset.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
[Writeback]: ./media/concept-sspr-howitworks/troubleshoot-on-premises-integration-writeback.png "Şirket içi tümleştirme parolası yazma yapılandırması ve sorun giderme bilgileri"
