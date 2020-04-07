---
title: Azure Kullanıcı İlkesi (UPN) değişikliklerini planlama ve sorun giderme
description: UPN değişiklikleri için bilinen sorunları ve azaltıcı etkenleri anlama
services: active-directory
ms.service: active-directory
ms.subservice: hybrid
ms.topic: how-to
ms.date: 03/13/2020
ms.author: baselden
author: barbaraselden
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: d11be1d971922095d4a1ace1c81c763134b4e58c
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743326"
---
# <a name="plan-and-troubleshoot-user-principal-name-changes-in-azure-active-directory"></a>Azure Etkin Dizini'nde Kullanıcı Adı değişikliklerini planlama ve sorun giderme

Kullanıcı Anaadı (UPN), kullanıcı hesapları için bir internet iletişim standardı olan bir özniteliktir. UPN, UPN öneki (kullanıcı hesabı adı) ve UPN sonekinden (DNS etki alanı adı) oluşur. Önek "@" simgesini kullanarak sonek birleştirir. Örneğin, someone@example.com. UPN, dizin ormanı içindeki tüm güvenlik temel nesneleri arasında benzersiz olmalıdır. 

> [!NOTE]
> Geliştiriciler için, kullanıcı objectID'yi UPN yerine değişmez tanımlayıcı olarak kullanmanızı öneririz. Uygulamalarınız şu anda UPN kullanıyorsa, deneyimlerini geliştirmek için UPN'yi kullanıcının birincil e-posta adresiyle eşleşecek şekilde ayarlamanızı öneririz.<br> **Karma bir ortamda, bir kullanıcı için UPN'nin şirket içi dizinde ve Azure Etkin Dizini'nde aynı olması önemlidir.**

**Bu makalede, kullanıcı tanımlayıcısı olarak UPN kullandığınızı varsayar. UPN değişiklikleri için planlama yı ve UPN değişikliklerinden kaynaklanabilir sorunları kurtarmayı ele adaz.**

## <a name="learn-about-upns-and-upn-changes"></a>UPN'ler ve UPN değişiklikleri hakkında bilgi edinin
Oturum açma sayfaları genellikle gerekli değer aslında upn olduğunda kullanıcıların e-posta adreslerini girmelerini ister. Bu nedenle, birincil e-posta adresleri değiştiğinde kullanıcıların UPN'ini değiştirdiğinden emin olmalısınız.

Kullanıcıların birincil e-posta adresleri birçok nedenden dolayı değişebilir:

* şirket rebranding

* farklı şirket bölümlerine taşınan çalışanlar 

* birleşme ve devralmalar

* çalışan adı değişiklikleri

### <a name="types-of-upn-changes"></a>UPN değişiklik türleri

Önek, sonek veya her ikisini değiştirerek BIR UPN'yi değiştirebilirsiniz.

* **Önek değiştirme.**

   *  Örneğin, bir kişinin adı değiştiyse, hesap adını değiştirebilirsiniz:  
BSimon@contoso.com içinBJohnson@contoso.com

   * Önekler için şirket standardını da değiştirebilirsiniz:  
Bsimon@contoso.com içinBritta.Simon@contoso.com

* **Sonek değiştirme.** <br>

    Örneğin, bir kişi bölümleri değiştirdiyse, etki alanını değiştirebilirsiniz: 

   * Britta.Simon@contoso.comHedefBritta.Simon@contosolabs.com <br>
     Veya<br>
    * Britta.Simon@corp.contoso.comHedefBritta.Simon@labs.contoso.com 

Bir kullanıcının birincil e-posta adresi her güncelleştirinde kullanıcının UPN'sini değiştirin. E-posta değişikliğinin nedeni ne olursa olsun, UPN her zaman eşleşecek şekilde güncelleştirilmelidir.

Active Directory'den Azure AD'ye ilk eşitleme sırasında, kullanıcıların e-postalarının UPN'leriyle aynı olduğundan emin olun.

### <a name="upns-in-active-directory"></a>Active Directory'de UPN'ler

Etkin Dizin'de varsayılan UPN soneki, kullanıcı hesabını oluşturduğunuz etki alanının DNS adıdır. Çoğu durumda, bu, internet üzerinde kurumsal etki alanı olarak kaydettiğiniz etki alanı adıdır. Kullanıcı hesabını contoso.com etki alanında oluşturursanız, varsayılan UPN

username@contoso.com

 Ancak, Active Directory etki alanlarını ve güvenlerini kullanarak [daha fazla UPN sonekleri ekleyebilirsiniz.](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain) 

Örneğin, labs.contoso.com eklemek ve kullanıcıların UPN'lerinin ve e-postalarının bunu yansıtmasını isteyebilirsiniz. Daha sonra olacak

username@labs.contoso.com.

>[!IMPORTANT]
> Etkin dizini ve Azure Etkin Dizini'ndeki UPN'ler eşleşmiyorsa, sorunlar ortaya çıkar. [Etkin Dizin'deki sonek değiştiriyorsanız,](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)Azure AD'da eşleşen bir özel etki alanı adının [eklendiğini ve doğrulandığını](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)sağlamalısınız. 

![Doğrulanmış etki alanlarının ekran görüntüsü](./media/howto-troubleshoot-upn-changes/custom-domains.png)

### <a name="upns-in-azure-active-directory"></a>Azure Etkin Dizinde UPN'ler

Kullanıcılar, kullanıcıMüdür Adı özyünlerindeki değerle Azure AD'de oturum açarak oturum açtır. 

Azure AD'yi şirket içi Active Directory'nizle birlikte kullandığınızda, kullanıcı hesapları Azure AD Connect hizmetini kullanarak eşitlenir. Varsayılan olarak Azure AD Connect sihirbazı, şirket içi Active Directory'deki userPrincipalName özniteliğini Azure AD'de UPN olarak kullanır. Özel bir yüklemede farklı bir özniteliğe değiştirebilirsiniz.

Tek bir kullanıcının Kullanıcı Ana Adı'nı (UPN) güncellediğinizde veya tüm kuruluşunuz için tanımlanmış bir işlemin olması önemlidir. 

Bu belgede bilinen sorunları ve geçici geçici işleri görün.

Kullanıcı hesaplarını Active Directory'den Azure AD'ye eşitlerken, Active Directory'deki UPN'lerin Azure AD'deki doğrulanmış etki alanlarıyla eşlediğinden emin olun.

![Doğrulanmış etki alanlarının ekran görüntüsü](./media/howto-troubleshoot-upn-changes/verified-domains.png)

userPrincipalName özniteliğinin değeri Azure AD'de doğrulanmış bir etki alanına karşılık gelmiyorsa, eşitleme işlemi varsayılan .onmicrosoft.com değeriyle sonekin yerini alır.


### <a name="roll-out-bulk-upn-changes"></a>Kullanıma hazır toplu UPN değişiklikleri

Toplu UPN değişiklikleri [için bir pilot için en iyi uygulamaları](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans) izleyin. Ayrıca, hızlı bir şekilde çözülemeyecek sorunlar bulursanız, UPN'leri geri almak için test edilmiş bir geri alma planınız vardır. Pilotunuz çalışmaya başladıktan sonra, çeşitli kuruluş rollerine ve bunların özel uygulama veya cihaz kümelerine sahip küçük kullanıcı kümelerini hedeflemeye başlayabilirsiniz.

Kullanıcıların bu ilk alt kümesi üzerinden going kullanıcıların değişikliğin bir parçası olarak ne beklemeniz gerektiğini iyi bir fikir verecektir. Bu bilgileri kullanıcı iletişimlerinize ekleyin.

Normal işlemlerin bir parçası olarak tek tek kullanıcılarüzerinde UPN'leri değiştirmek için tanımlı bir yordam oluşturun. Bilinen sorunlar ve geçici çözümlerle ilgili belgeleri içeren sınanmış bir yordam geçirmenizi öneririz.

Aşağıdaki bölümlerde, UPN'ler değiştirildiğinde bilinen olası sorunları ve geçici geçici çözüm ayrıntılarını içerir.

## <a name="apps-known-issues-and-workarounds"></a>Bilinen uygulamalar sorunları ve geçici geçici çözüm

[Hizmet olarak yazılım (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) ve Line of Business (LoB) uygulamaları genellikle kullanıcıları bulmak ve roller de dahil olmak üzere kullanıcı profili bilgilerini depolamak için UPN'lere güvenir. Kullanıcılar uygulamada ilk kez oturum açtıklarında bir kullanıcı profili oluşturmak için [Just in Time sağlama](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning) yı kullanan uygulamalar UPN değişikliklerinden etkilenebilir.

**Bilinen sorun**<br>
Bir kullanıcının UPN'ini değiştirmek, Azure AD kullanıcısı ile uygulamada oluşturulan kullanıcı profili arasındaki ilişkiyi bozabilir. Uygulama Just [in Time sağlama kullanıyorsa,](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning)yepyeni bir kullanıcı profili oluşturabilir. Bu, uygulama yöneticisinin bu ilişkiyi düzeltmek için el ile değişiklik yapması gerekir.

**Geçi -ci çözüm**<br>
[Azure AD Otomatik Kullanıcı Sağlama,](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) desteklenen bulut uygulamalarında kullanıcı kimliklerinizi otomatik olarak oluşturmanıza, korumanıza ve kaldırmanıza olanak tanır. Uygulamalarınızda otomatik kullanıcı sağlama nın yapılandırılması, uygulamalardaki UPN'leri otomatik olarak güncelleştirir. Upn değişikliklerinden etkilenmediklerini doğrulamak için uygulamaları aşamalı kullanıma göre test edin.
Geliştiriciyseniz, Azure Active Directory'den otomatik kullanıcı sağlamayı etkinleştirmek [için uygulamanız için SCIM desteği eklemeyi](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups) düşünün. 

## <a name="managed-devices-known-issues-and-workarounds"></a>Yönetilen aygıtlar bilinen sorunlar ve geçici geçici çözüm

[Cihazlarınızı Azure AD'ye getirerek,](https://docs.microsoft.com/azure/active-directory/devices/overview)bulut ve şirket içi kaynaklarınızda tek oturum açma (SSO) aracılığıyla kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız.

### <a name="azure-ad-joined-devices"></a>Azure AD’ye katılmış cihazlar

[Azure AD'ye katılan](https://docs.microsoft.com/azure/active-directory/devices/concept-azure-ad-join) aygıtlar doğrudan Azure AD'ye katılır ve kullanıcıların kuruluşkimliklerini kullanarak aygıtta oturum açmalarına olanak tanır.

**Bilinen sorunlar** <br>
Kullanıcılar, kimlik doğrulama için Azure AD'ye bağlı uygulamalarla ilgili tek oturum açma sorunları yla karşılaşabilir.

**Geçi -ci çözüm** <br>
UPN değişikliğinin Azure AD ile eşitlemesine izin verin. Yeni UPN'nin Azure AD Portalı'na yansıtTığını doğruladıktan sonra, kullanıcıdan yeni UPN'leriyle oturum açmak için "Diğer kullanıcı" döşemesini seçmesini isteyin. PowerShell ile [PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)de doğrulayabilirsiniz. Yeni UPN'leriyle oturum açtıktan sonra, eski UPN'ye yapılan atıflar "İş veya okul" Windows ayarında görünmeye devam edebilir.

![Doğrulanmış etki alanlarının ekran görüntüsü](./media/howto-troubleshoot-upn-changes/other-user.png)

### <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD’ye katılmış cihazlar

[Karma Azure AD birleştirilmiş](https://docs.microsoft.com/azure/active-directory/devices/concept-azure-ad-join-hybrid) aygıtlar Active Directory ve Azure AD'ye katılır. Ortamınızda Etkin Dizin ayak izi varsa ve Azure AD tarafından sağlanan özelliklerden yararlanmak istiyorsanız, Karma Azure AD join'i uygulayabilirsiniz.

**Bilinen sorunlar** 

Windows 10 Karma Azure AD birleştirilmiş aygıtlar beklenmeyen yeniden başlatma ve erişim sorunlarıyla karşılaşabilir.

Kullanıcılar, yeni UPN Azure AD ile eşitlenmeden önce Windows'da oturum açtıysa veya varolan bir Windows oturumunu kullanmaya devam ederse, Koşullu Erişim kaynaklara erişmek için Karma Birleştirilmiş aygıtların kullanımını zorunlu kılması için yapılandırıldıysa, kimlik doğrulaması için Azure AD kullanan uygulamalarla ilgili tek oturum açma sorunlarıyla karşılaşabilirler. 

Ayrıca, aşağıdaki ileti görüntülenir ve bir dakika sonra yeniden başlatmayı zorlar. 

"Bilgisayarınız bir dakika içinde otomatik olarak yeniden başlatılacaktır. Windows bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Bu mesajı şimdi kapatıp çalışmanızı kurtarmalısınız" dedi.

**Geçi -ci çözüm** 

Aygıtın Azure AD'den katılmadan başlatılması ve yeniden başlatılması gerekir. Yeniden başlattıktan sonra, aygıt otomatik olarak Azure AD'ye yeniden katılır ve kullanıcının "Diğer kullanıcı" döşemesini seçerek yeni UPN'yi kullanarak oturum açması gerekir. Azure AD'den bir aygıta katılmak için aşağıdaki komutu komut isteminde çalıştırın:

**dsregcmd /bırak**

Kullanılıyorsa, kullanıcının Windows Hello for Business'a [yeniden kaydolması](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-whfb-provision) gerekir. Windows 7 ve 8.1 aygıtları UPN değişiklikleri nden sonra bu sorundan etkilenmez.

## <a name="microsoft-authenticator-known-issues-and-workarounds"></a>Microsoft Authenticator bilinen sorunlar ve geçici geçici servis

Kuruluşunuz, kuruluş uygulamalarında ve verilerinde oturum açmave erişme için [Microsoft Authenticator uygulamasının](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-overview) kullanılmasını gerektirebilir. Uygulamada bir kullanıcı adı görünse de, kullanıcı kayıt işlemini tamamlayana kadar hesap doğrulama yöntemi olarak çalışacak şekilde ayarlanmaz.

[Microsoft Authenticator uygulamasının](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-overview) dört ana işlevi vardır:

* Anında iletme bildirimi veya doğrulama kodu aracılığıyla çok faktörlü kimlik doğrulama

* [Aracılı kimlik doğrulama](https://docs.microsoft.com/azure/active-directory/develop/brokered-auth) kullanan uygulamalar için tek oturum açma sağlamak için iOS ve Android cihazlarda kimlik doğrulama aracısı olarak hareket edin

* Intune Uygulama Koruması ve Cihaz Kayıt/Yönetimi gibi diğer özellikler için bir gereklilik olan Azure AD'ye aygıt kaydı (İşyeri Birleştirme olarak da bilinir),

* MFA ve cihaz kaydı gerektiren telefon oturum açma.

### <a name="multi-factor-authentication-with-android-devices"></a>Android cihazlarla Çok Faktörlü Kimlik Doğrulama

Microsoft Authenticator uygulaması bant dışı doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik bir telefon görüşmesi veya SMS yerleştirmek yerine, [Çok Faktörlü Kimlik Doğrulama (MFA)](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) kullanıcının akıllı telefonu veya tabletindeki Microsoft Authenticator uygulamasına bir bildirim gerçekleştirir. Kullanıcı oturum açma larını tamamlamak için uygulamadaki Onay'a (veya biyometrik bir PIN veya biyometrik ve "Authenticate" dokunun) dokunur.

**Bilinen sorunlar** 

Bir kullanıcının UPN'sini değiştirdiğinizde, eski UPN hala kullanıcı hesabında görüntülenir ve bir bildirim alınmayabilir. [Doğrulama kodları](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-faq) çalışmaya devam eder.

**Geçi -ci çözüm**

Bir bildirim alınırsa, kullanıcıya bildirimi reddetmesini, Authenticator uygulamasını açmasını, "Bildirimleri kontrol et" seçeneğine dokunmasını ve MFA istemini onaylamasını bildirin. Bundan sonra, hesapta görüntülenen UPN güncelleştirilir. Güncelleştirilmiş UPN'nin yeni bir hesap olarak görüntülenebileceğini unutmayın, bunun nedeni diğer Authenticator işlevleridir. Daha fazla bilgi için bu makalede bilinen ek sorunlara bakın.

### <a name="brokered-authentication"></a>Aracılı kimlik doğrulama

Android ve Microsoft Authenticator gibi iOS brokerları şunları sağlar:

* Tek oturum açma (SSO) - Kullanıcılarınızın her uygulamada oturum açmalarına gerek yoktur.

* Cihaz tanımlama - Aracı, işyeri birleştirildiğinde cihazda oluşturulan aygıt sertifikasına erişir.

* Uygulama tanımlama doğrulaması - Bir uygulama aracıyı aradığında, yeniden yönlendirme URL'sini geçer ve aracı bunu doğrular.

Ayrıca, uygulamaların [Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/)gibi daha gelişmiş özelliklere katılmasını sağlar ve [Microsoft Intune senaryolarını](https://docs.microsoft.com/azure/active-directory/develop/msal-net-use-brokers-with-xamarin-apps)destekler.

**Bilinen sorunlar**<br>
Kullanıcı, uygulama tarafından geçirilen login_hint ile komisyoncuda depolanan UPN arasındaki uyuşmazlık nedeniyle broker destekli oturum açma kullanan yeni uygulamalarda daha etkileşimli kimlik doğrulama istemleri ile sunulur.

**Geçi -ci çözüm** <br> Kullanıcının hesabı Microsoft Authenticator'dan el ile kaldırması ve aracı destekli bir uygulamadan yeni bir oturum açma başlatması gerekir. Hesap, ilk kimlik doğrulamadan sonra otomatik olarak eklenir.

### <a name="device-registration"></a>Cihaz kaydı

Microsoft Authenticator uygulaması, aygıtı Azure AD'ye kaydettirmekten sorumludur. Aygıt kaydı, aygıtın Azure AD kimliğine doğrulanmasına olanak tanır ve aşağıdaki senaryolar için bir gerekliliktir:

* Intune Uygulama Koruması

* Intune Aygıt Kaydı

* Telefon Oturum Aç

**Bilinen sorunlar**<br>
UPN'yi değiştirdiğinizde, microsoft Authenticator uygulamasında yeni UPN içeren yeni bir hesap görünürken, eski UPN'li hesap hala listelenir. Ayrıca, eski UPN uygulama ayarlarında Cihaz Kaydı bölümünde görüntülenir. Aygıt Kaydı'nın normal işlevselliğinde veya bağlı senaryolarda değişiklik yoktur.

**Geçi -ci çözüm** <br> Microsoft Authenticator uygulamasındaki eski UPN'ye yapılan tüm başvuruları kaldırmak için, kullanıcıya eski ve yeni hesapları Microsoft Authenticator'dan el ile kaldırmasını, MFA'ya yeniden kaydolmasını ve aygıta yeniden katılmasını söyle.

### <a name="phone-sign-in"></a>Telefon oturum açma

Telefon oturum açma, kullanıcıların Parola olmadan Azure AD'de oturum açmalarına olanak tanır. Telefon oturum açmayı etkinleştirmek için, kullanıcının Authenticator uygulamasını kullanarak MFA'ya kaydolması ve ardından doğrudan Authenticator'da telefon oturum açma olanağı sağlaması gerekir. Yapılandırmanın bir parçası olarak, aygıt Azure AD'ye kaydolur.

**Bilinen sorunlar** <br>
Kullanıcılar herhangi bir bildirim almadıkları için Telefon oturum açmasını kullanamazlar. Kullanıcı Bildirimleri Denetle'ye dokunursa, bir hata alır.

**Geçi -ci çözüm**<br>
Kullanıcının Telefon oturum açma için etkin olan hesaptaki açılır menüyü seçmesi ve telefon oturum açmayı devre dışı bırakmasını seçmesi gerekir. İstenirse, Telefon oturum açma tekrar etkinleştirilebilir.

## <a name="security-key-fido2-known-issues-and-workarounds"></a>Güvenlik Anahtarı (FIDO2) bilinen sorunlar ve geçici çözüm

**Bilinen sorunlar** <br>
Aynı anahtarda birden çok kullanıcı kaydedildiğinde, oturum açma ekranında eski UPN'nin görüntülendiği bir hesap seçim sayfası gösterilir. Güvenlik Anahtarlarını kullanarak oturum açmalar UPN değişikliklerinden etkilenmez.  

**Geçi -ci çözüm**<br>
Eski UPN'lere yapılan başvuruları kaldırmak için kullanıcıların [güvenlik anahtarını sıfırlamaları ve yeniden kaydettirmeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key#known-issues)gerekir.

## <a name="onedrive-known-issues-and-workarounds"></a>OneDrive bilinen sorunlar ve geçici çözüm

OneDrive kullanıcılarının UPN değişikliklerinden sonra sorunlarla karşılaştığı bilinmektedir. Daha fazla bilgi için [UPN değişikliklerinin OneDrive URL'sini ve OneDrive özelliklerini nasıl etkilediğini](https://docs.microsoft.com/onedrive/upn-changes)görün.

## <a name="next-steps"></a>Sonraki adımlar

Şu kaynaklara bakın:
* [Azure AD Connect: Tasarım kavramları](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-design-concepts)

* [Azure AD UserPrincipalName popülasyonu](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-userprincipalname)

* [Microsoft kimlik platformu kimlik belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
