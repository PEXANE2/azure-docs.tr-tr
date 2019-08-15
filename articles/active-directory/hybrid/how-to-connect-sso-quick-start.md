---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma-hızlı başlangıç | Microsoft Docs'
description: Bu makalede Azure Active Directory sorunsuz çoklu oturum açma ile çalışmaya başlama açıklanmaktadır
services: active-directory
keywords: Azure AD Connect nedir, yükler Active Directory, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87764ed30182dc548fc3a260582174f121e27e24
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965224"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Kesintisiz çoklu oturum açma Azure Active Directory: Hızlı başlangıç

## <a name="deploy-seamless-single-sign-on"></a>Kesintisiz çoklu oturum açma dağıtın

Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma (sorunsuz SSO), şirket ağınıza bağlı olan kurumsal masaüstlerinde olduklarında kullanıcılar tarafından otomatik olarak oturum açar. Sorunsuz SSO, kullanıcılarınıza ek şirket içi bileşenlere gerek duymadan bulut tabanlı uygulamalarınıza kolay erişim sağlar.

Sorunsuz SSO dağıtmak için aşağıdaki adımları izleyin.

## <a name="step-1-check-the-prerequisites"></a>1\. adım: Önkoşulları denetleme

Aşağıdaki önkoşulların yerinde olduğundan emin olun:

* **Azure AD Connect sunucunuzu ayarlayın**: [Geçiş kimlik doğrulamasını](how-to-connect-pta.md) oturum açma yönteminiz olarak kullanırsanız, ek önkoşul denetimi gerekli değildir. Oturum açma yönteminiz olarak [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md) kullanıyorsanız ve Azure AD Connect Ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri doğrulayın:
   - Azure AD Connect sürüm 1.1.644.0 veya üstünü kullanıyorsunuz. 
   - Güvenlik duvarınız veya proxy DNS beyaz listeye izin veriyorsa, bağlantı noktası 443 üzerinden  **\*. msappproxy.net** URL 'lerine yönelik bağlantıları beyaz listeye ekleyin. Aksi takdirde, haftalık olarak güncellenen [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verin. Bu önkoşul yalnızca özelliği etkinleştirdiğinizde geçerlidir. Bu, gerçek Kullanıcı oturum açma işlemleri için gerekli değildir.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0, Parola karması eşitlemeyle ilgili bir sorun var. Parola karması eşitlemesini doğrudan kimlik doğrulamasıyla birlikte kullanmayı düşünmüyorsanız, daha fazla bilgi edinmek için [Azure AD Connect sürüm notlarını](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) okuyun.

* **Desteklenen bir Azure AD Connect topolojisi kullanın**: [Burada](plan-connect-topologies.md)açıklanan Azure AD Connect desteklenen topolojilerden birini kullandığınızdan emin olun.

    >[!NOTE]
    >Sorunsuz SSO, aralarında AD güvenleri olup olmadığı gibi birden çok AD ormanını destekler.

* **Etki alanı yöneticisi kimlik bilgilerini ayarlama**: Her bir Active Directory ormanı için etki alanı yöneticisi kimlik bilgilerine sahip olmanız gerekir:
    * Azure AD Connect aracılığıyla Azure AD 'ye eşitliyorsanız.
    * Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.
    
* **Modern kimlik doğrulamayı etkinleştir**: Bu özelliğin çalışması için kiracınızda [modern kimlik doğrulamayı](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) etkinleştirmeniz gerekir.

* **Office 365 istemcilerinin en son sürümlerini kullanın**: Office 365 istemcileri (Outlook, Word, Excel ve diğerleri) ile sessiz oturum açma deneyimi almak için, kullanıcılarınızın 16.0.8730. xxxx veya üzeri sürümlerini kullanması gerekir.

## <a name="step-2-enable-the-feature"></a>2\. adım: Özelliği etkinleştirin

[Azure AD Connect](whatis-hybrid-identity.md)aracılığıyla sorunsuz SSO 'yu etkinleştirin.

>[!NOTE]
> Gereksinimlerinizi karşılamıyorsa, [PowerShell kullanarak sorunsuz SSO](tshoot-connect-sso.md#manual-reset-of-the-feature) 'yu de etkinleştirebilirsiniz Azure AD Connect. Active Directory ormanı başına birden fazla etki alanınız varsa ve için sorunsuz SSO 'yu etkinleştirmek istediğiniz etki alanı hakkında daha fazla bilgi almak istiyorsanız bu seçeneği kullanın.

Azure AD Connect yeni bir yüklemesini gerçekleştiriyorsanız, [özel yükleme yolunu](how-to-connect-install-custom.md)seçin. **Kullanıcı oturum açma** sayfasında **Çoklu oturum açmayı etkinleştir** seçeneğini belirleyin.

>[!NOTE]
> Seçenek yalnızca oturum açma yöntemi **Parola karması eşitleme** veya **geçişli kimlik doğrulama**olduğunda seçilebilir.

![Azure AD Connect: Kullanıcı oturumu açma](./media/how-to-connect-sso-quick-start/sso8.png)

Zaten bir Azure AD Connect kurulumunuz varsa, Azure AD Connect **Kullanıcı oturum açma sayfasını Değiştir** ' i seçin ve ardından **İleri**' yi seçin. Azure AD Connect sürümleri 1.1.880.0 veya üzerini kullanıyorsanız, **Çoklu oturum açmayı etkinleştir** seçeneği varsayılan olarak seçilir. Azure AD Connect eski sürümlerini kullanıyorsanız **Çoklu oturum açmayı etkinleştir** seçeneğini belirleyin.

![Azure AD Connect: Kullanıcı oturumunu değiştirme](./media/how-to-connect-sso-quick-start/changeusersignin.png)

**Çoklu oturum açmayı etkinleştir** sayfasına ulaşana kadar sihirbaza devam edin. Her bir Active Directory ormanı için etki alanı yöneticisi kimlik bilgilerini sağlayın:

* Azure AD Connect aracılığıyla Azure AD 'ye eşitliyorsanız.
* Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.

Sihirbazın tamamlanmasından sonra, kiracınızda sorunsuz SSO etkinleştirilmiştir.

>[!NOTE]
> Etki alanı yöneticisi kimlik bilgileri Azure AD Connect veya Azure AD 'de depolanmaz. Yalnızca özelliği etkinleştirmek için kullanılır.

Sorunsuz SSO 'yu doğru şekilde etkinleştirdiğinizi doğrulamak için aşağıdaki yönergeleri izleyin:

1. [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgileriyle oturum açın.
2. Sol bölmedeki **Azure Active Directory** seçin.
3. **Azure AD Connect**seçin.
4. **Sorunsuz çoklu oturum açma** özelliğinin **etkin**olarak göründüğünü doğrulayın.

![Azure portal: Azure AD Connect bölmesi](./media/how-to-connect-sso-quick-start/sso10.png)

>[!IMPORTANT]
> Sorunsuz SSO, her ad ormanında Şirket `AZUREADSSOACC` içi Active Directory (ad) adında bir bilgisayar hesabı oluşturur. Güvenlik `AZUREADSSOACC` nedenleriyle bilgisayar hesabının güçlü korunması gerekir. Bilgisayar hesabını yalnızca etki alanı yöneticileri yönetebilmelidir. Bilgisayar hesabında Kerberos temsilcisinin devre dışı bırakıldığından ve Active Directory ' deki başka hiçbir hesabın `AZUREADSSOACC` bilgisayar hesabında temsilciliizin izinlerine sahip olmadığından emin olun. Bilgisayar hesabını, yanlışlıkla silinmelerden güvenli oldukları ve yalnızca etki alanı yöneticilerinin erişimi olan bir kuruluş biriminde (OU) depolayın.

>[!NOTE]
> Şirket içi ortamınızda, karma ve kimlik bilgisi hırsızlığı azaltma mimarilerini kullanıyorsanız, `AZUREADSSOACC` bilgisayar hesabının karantina kapsayıcısında bitmediğinden emin olmak için uygun değişiklikleri yapın. 

## <a name="step-3-roll-out-the-feature"></a>3\. adım: Özelliği kullanıma al

Aşağıda belirtilen yönergeleri kullanarak kullanıcılarınıza sorunsuz SSO 'yu yavaş bir şekilde dağıtabilirsiniz. Aşağıdaki Azure AD URL 'sini, Active Directory grup ilkesi kullanarak, tüm kullanıcıların Intranet bölgesi ayarlarına ekleyerek başlayabilirsiniz:

- `https://autologon.microsoftazuread-sso.com`

Ayrıca, grup ilkesi aracılığıyla **betik aracılığıyla durum çubuğuna güncelleştirmelere Izin ver** adlı bir Intranet bölgesi ilkesi ayarını etkinleştirmeniz gerekir. 

>[!NOTE]
> Aşağıdaki yönergeler yalnızca Windows 'da Internet Explorer ve Google Chrome için geçerlidir (Internet Explorer ile bir güvenilen site URL 'si kümesini paylaşıyorsa). MacOS 'ta Mozilla Firefox ve Google Chrome ayarlama yönergeleri için sonraki bölümü okuyun.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Kullanıcıların Intranet bölgesi ayarlarını neden değiştirmeniz gerekiyor?

Varsayılan olarak tarayıcı, doğru bölgeyi Internet veya Intranet ' i belirli bir URL 'den otomatik olarak hesaplar. Örneğin, `http://contoso/` Intranet bölgesine eşlenir, öte yandan `http://intranet.contoso.com/` Internet bölgesiyle eşlenir (URL bir nokta içereceğinden). URL 'YI tarayıcının Intranet bölgesine açıkça eklemediğiniz takdirde, tarayıcılar Azure AD URL 'SI gibi bir bulut uç noktasına Kerberos bileti göndermez.

Kullanıcıların Intranet bölgesi ayarlarını değiştirmek için iki yol vardır:

| Seçenek | Yönetici değerlendirmesi | Kullanıcı deneyimi |
| --- | --- | --- |
| Grup ilkesi | Yönetici, Intranet bölgesi ayarlarının düzenlemesini kilitler | Kullanıcılar kendi ayarlarını değiştiremezler |
| Grup İlkesi tercihi |  Yönetici, Intranet bölgesi ayarlarında düzenlenmesine izin veriyor | Kullanıcılar kendi ayarlarını değiştirebilir |

### <a name="group-policy-option---detailed-steps"></a>"Grup İlkesi" seçeneği-ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi aracını açın.
2. Kullanıcılarınıza veya tümüne uygulanan Grup ilkesini düzenleyin. Bu örnek **varsayılan etki alanı ilkesi**kullanır.
3.  > **Windows bileşenleri** > **Internet Explorer Internet Yönetim Şablonları** **Kullanıcı yapılandırma ilkesine** > gidin >  >  **Denetim Masası** > **Güvenlik sayfası**. Ardından **siteden bölgeye atama listesi**' ni seçin.
    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso6.png)
4. İlkeyi etkinleştirin ve iletişim kutusuna aşağıdaki değerleri girin:
   - **Değer adı**: Kerberos biletleri iletileceği Azure AD URL 'SI.
   - **Değer** (Veri): **1** , Intranet bölgesini gösterir.

     Sonuç şöyle görünür:

     Değer adı:`https://autologon.microsoftazuread-sso.com`
  
     Değer (veri): 1.

   >[!NOTE]
   > Bazı kullanıcıların sorunsuz SSO 'yu kullanmasını engellemek istiyorsanız (örneğin, bu kullanıcılar paylaşılan kiler 'de oturum açtığında), önceki değerleri **4**olarak ayarlayın. Bu eylem, Azure AD URL 'sini kısıtlanmış bölgeye ekler ve her seferinde sorunsuz SSO başarısız olur.
   >

5. **Tamam**' ı ve ardından yeniden **Tamam** ' ı seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso7.png)

6.  > **Windows bileşenleri** > **Internet Explorer Internet Yönetim Şablonları** **Kullanıcı yapılandırma ilkesine** > gidin >  >  **Denetim Masası** > **Güvenlik sayfası** > **Intranet bölgesi**. Ardından **betik aracılığıyla durum çubuğunda güncelleştirmelere Izin ver**' i seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso11.png)

7. İlke ayarını etkinleştirin ve ardından **Tamam**' ı seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso12.png)

### <a name="group-policy-preference-option---detailed-steps"></a>"Grup İlkesi tercihi" seçeneği-ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi aracını açın.
2. Kullanıcılarınıza veya tümüne uygulanan Grup ilkesini düzenleyin. Bu örnek **varsayılan etki alanı ilkesi**kullanır.
3. **Kullanıcı yapılandırma** > **tercihleri** **Windows ayarları** **kayıt defteri**yeni kayıt defteri öğesine gidin. >  >  >  > 

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso15.png)

4. Uygun alanlara aşağıdaki değerleri girin ve **Tamam**' a tıklayın.
   - **Anahtar yolu**: ***Software\microsoft\windows\currentversion\ınternet Settings\ZoneMap\Domains\microsoftazuread-sso.com\autologon***
   - **Değer adı**: ***https***.
   - **Değer türü**: ***REG_DWORD***.
   - **Değer verisi**: ***00000001***.
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso16.png)
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso17.png)

### <a name="browser-considerations"></a>Tarayıcı konuları

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (tüm platformlar)

Mozilla Firefox, Kerberos kimlik doğrulamasını otomatik olarak kullanmaz. Her kullanıcının aşağıdaki adımları kullanarak, Azure AD URL 'sini Firefox ayarlarına el ile eklemesi gerekir:
1. Firefox 'u çalıştırın ve `about:config` adres çubuğuna girin. Gördüğünüz tüm bildirimleri kapatın.
2. **Network. Negotiate-Auth. Trusted-uris** tercihini arayın. Bu tercih edilecek Kerberos kimlik doğrulaması için Firefox 'un güvenilen siteleri listelenir.
3. Sağ tıklayın ve **Değiştir**' i seçin.
4. Alana `https://autologon.microsoftazuread-sso.com` girin.
5. **Tamam** ' ı seçin ve ardından tarayıcıyı yeniden açın.

#### <a name="safari-macos"></a>Safari (macOS)

MacOS çalıştıran makinenin AD 'ye katılmış olduğundan emin olun. MacOS cihazınızı AD ile birleştirme yönergeleri Bu makalenin kapsamı dışındadır.

#### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar)

Ortamınızdaki [authnegotiatedelegatewhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) veya [authserverwhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) ilke ayarlarını geçersiz kıldıysanız, Azure AD 'nin URL 'sini (`https://autologon.microsoftazuread-sso.com`) da eklemediğinizden emin olun.

#### <a name="google-chrome-macos-and-other-non-windows-platforms"></a>Google Chrome (macOS ve diğer Windows dışı platformlar)

Mac OS ve diğer Windows dışı platformlar için Google Chrome için, tümleşik kimlik doğrulaması için Azure AD URL 'sini beyaz listeye alma hakkında bilgi için, [Kmıum proje Ilkesi listesine](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) bakın.

Mac kullanıcıları için Azure AD URL 'sini Firefox ve Google Chrome 'a aktarmak için üçüncü taraf Active Directory grup ilkesi uzantılarının kullanımı Bu makalenin kapsamı dışındadır.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz SSO, Firefox ve Microsoft Edge tarayıcılarında özel göz atma modunda çalışmaz. Tarayıcı Gelişmiş korumalı modda çalışıyorsa Internet Explorer 'da da çalışmaz.

## <a name="step-4-test-the-feature"></a>4\. Adım: Özelliği test etme

Belirli bir kullanıcı için özelliği test etmek için aşağıdaki koşulların tümünün yerinde olduğundan emin olun:
  - Kullanıcı bir kurumsal cihazda oturum açar.
  - Cihaz Active Directory etki alanına katıldı. Cihazın [Azure AD 'ye katılmış](../active-directory-azureadjoin-overview.md)olması gerekmez.
  - Cihazın, şirket kablolu veya kablosuz ağ ya da VPN bağlantısı gibi bir uzaktan erişim bağlantısı aracılığıyla etki alanı denetleyicinize (DC) doğrudan bağlantısı vardır.
  - Bu kullanıcıya grup ilkesi aracılığıyla [özelliği kullanıma](##step-3-roll-out-the-feature) sunulaştınız.

Kullanıcının yalnızca Kullanıcı adını girdiği ancak parolayı değil, senaryoyu test etmek için:
   - `https://myapps.microsoft.com/` Yeni bir özel tarayıcı oturumunda oturum açın.

Kullanıcının Kullanıcı adı veya parola girmesi gereken senaryoyu test etmek için şu adımlardan birini kullanın: 
   - `https://myapps.microsoft.com/contoso.onmicrosoft.com` Yeni bir özel tarayıcı oturumunda oturum açın. *Contoso* değerini kiracınızın adıyla değiştirin.
   - `https://myapps.microsoft.com/contoso.com` Yeni bir özel tarayıcı oturumunda oturum açın. *Contoso.com* değerini, kiracınızda doğrulanmış bir etki alanıyla (Federasyon etki alanı değil) değiştirin.

## <a name="step-5-roll-over-keys"></a>5\. Adım: Anahtarları atla

2\. adımda, Azure AD Connect sorunsuz SSO 'yu etkinleştirdiğiniz tüm Active Directory ormanlarda bilgisayar hesapları (Azure AD 'yi temsil eder) oluşturur. Daha fazla bilgi edinmek için [bkz. Azure Active Directory kesintisiz çoklu oturum açma: Teknik kapsamlı](how-to-connect-sso-how-it-works.md)bakış.

>[!IMPORTANT]
>Bir bilgisayar hesabındaki Kerberos şifre çözme anahtarı, sızmış ise, AD ormanındaki herhangi bir kullanıcı için Kerberos biletleri oluşturmak üzere kullanılabilir. Kötü amaçlı aktörler daha sonra güvenliği aşılmış kullanıcılar için Azure AD oturum açma işlemlerini taklit edebilir. Bu Kerberos şifre çözme anahtarlarını her 30 günde bir en az bir kez düzenli olarak almanızı önemle tavsiye ederiz.

Anahtarların nasıl alınacağı hakkında yönergeler için bkz [. Azure Active Directory kesintisiz çoklu oturum açma: Sık sorulan sorular](how-to-connect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account). Anahtarları otomatik olarak almak için bir özellik üzerinde çalışıyoruz.

>[!IMPORTANT]
>Özelliği etkinleştirdikten _hemen_ sonra bu adımı uygulamanız gerekmez. Kerberos şifre çözme anahtarlarını en az 30 günde bir alın.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik kapsamlı](how-to-connect-sso-how-it-works.md)bakış: Sorunsuz çoklu oturum açma özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-sso-faq.md): Sorunsuz çoklu oturum açma hakkında sık sorulan soruların yanıtlarını alın.
- [Sorun giderme](tshoot-connect-sso.md): Sorunsuz çoklu oturum açma özelliğiyle yaygın sorunları çözmeyi öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosya olarak yüklemek için Azure Active Directory forumunu kullanın.
