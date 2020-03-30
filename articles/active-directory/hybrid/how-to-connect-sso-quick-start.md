---
title: 'Azure AD Connect: Sorunsuz Tek Oturum Açma - hızlı başlangıç | Microsoft Dokümanlar'
description: Bu makalede, Azure Active Directory Seamless Tek Oturum Açma ile nasıl başlanır
services: active-directory
keywords: Azure AD Connect nedir, Active Directory'yi yükler, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenler
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
ms.openlocfilehash: 3ec56d37ca2c0a199968707b3d93f4797be2beca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261209"
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory Seamless Tek Oturum Açma: Hızlı başlangıç

## <a name="deploy-seamless-single-sign-on"></a>Kesintisiz Tek Oturum Açma'yı Dağıt

Azure Active Directory (Azure AD) Sorunsuz Tek Oturum Açma (Sorunsuz SSO), kurumsal ağınıza bağlı kurumsal masaüstü bilgisayarlarda kullanıcılarda otomatik olarak oturum açmaktadır. Sorunsuz SSO, kullanıcılarınıza şirket içi bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolay erişim sağlar.

Sorunsuz SSO'ya dağıtmak için aşağıdaki adımları izleyin.

## <a name="step-1-check-the-prerequisites"></a>Adım 1: Ön koşulları kontrol edin

Aşağıdaki ön koşulların yerinde olduğundan emin olun:

* **Azure AD Connect sunucunuzu ayarlama**: Oturum açma yönteminiz olarak [Geçiş Kimlik Doğrulaması](how-to-connect-pta.md) kullanıyorsanız, ek ön koşul denetimi gerekmez. Oturum açma yönteminiz olarak [parola karma senkronizasyonunu](how-to-connect-password-hash-synchronization.md) kullanıyorsanız ve Azure AD Connect ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri sağlayın:
   - Sürüm 1.1.644.0 veya daha sonra Azure AD Connect'i kullanırsınız. 
   - Güvenlik duvarınız veya proxy'niz DNS'nin beyaz listeye alınmasına izin veriyorsa, ** \*bağlantı** noktası 443 üzerinden .msappproxy.net URL'lerine bağlantıları beyaz listele. Değilse, haftalık olarak güncelleştirilen [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişin. Bu ön koşul yalnızca özelliği etkinleştirdiğinizde geçerlidir. Gerçek kullanıcı oturum açmaları için gerekli değildir.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 sürümlerinde parola karma eşitlemeyle ilgili bir sorun vardır. Parola _don't_ karma senkronizasyonunu Pass-through Authentication ile birlikte kullanmak istemiyorsanız, daha fazla bilgi edinmek için [Azure AD Connect sürüm notlarını](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) okuyun.

* **Desteklenen bir Azure AD Connect topolojisini kullanın**: Azure AD Connect'in desteklenen topolojilerinden birini [burada](plan-connect-topologies.md)kullandığınızdan emin olun.

    >[!NOTE]
    >Sorunsuz SSO, aralarında AD güvenleri olsa da olmasa da birden fazla AD ormanLarını destekler.

* **Etki alanı yöneticisi kimlik bilgilerini ayarlama**: Her Active Directory ormanı için etki alanı yöneticisi kimlik bilgilerine sahip olmanız gerekir:
    * Azure AD Connect aracılığıyla Azure AD'ye senkronize olursunuz.
    * Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.
    
* **Modern kimlik doğrulamayı etkinleştir**: Bu özelliğin çalışması için kiracınızda [modern kimlik doğrulamasını](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) etkinleştirmeniz gerekir.

* **Office 365 istemcilerinin en son sürümlerini kullanın**: Office 365 istemcileriyle (Outlook, Word, Excel ve diğerleri) sessiz oturum açma deneyimi elde etmek için kullanıcılarınızın 16.0.8730.xxxx veya üzeri sürümleri kullanması gerekir.

## <a name="step-2-enable-the-feature"></a>Adım 2: Özelliği etkinleştirme

[Azure AD Connect](whatis-hybrid-identity.md)ile Sorunsuz SSO'ya olanak sağlar.

>[!NOTE]
> Azure AD Connect gereksinimlerinizi karşılamazsa [PowerShell'i kullanarak Kesintisiz SSO'ya da etkinleştirebilirsiniz.](tshoot-connect-sso.md#manual-reset-of-the-feature) Active Directory ormanı başına birden fazla etki alanınız varsa ve Kesintisiz SSO'yu etkinleştirmek istediğiniz etki alanı hakkında daha fazla hedeflenmiş olmak istiyorsanız bu seçeneği kullanın.

Azure AD Connect'in yeni bir yüklemesini yapıyorsanız, [özel yükleme yolunu](how-to-connect-install-custom.md)seçin. Kullanıcı **oturum açma** sayfasında, **tek oturum** açma seçeneğini etkinleştir seçeneğini belirleyin.

>[!NOTE]
> Seçenek, yalnızca Oturum Açma yöntemi **Parola Karma Senkronizasyonu** veya **Geçiş Kimlik Doğrulaması**ise seçim için kullanılabilir.

![Azure AD Connect: Kullanıcı oturum açma](./media/how-to-connect-sso-quick-start/sso8.png)

Azure AD Connect yüklemeniz zaten varsa, Azure AD Connect'te **kullanıcı oturum aç'ını değiştir'i** seçin ve **ardından İleri'yi**seçin. Azure AD Connect sürümlerini 1.1.880.0 veya üzerinde kullanıyorsanız, **tek oturum açma seçeneğini etkinleştir** seçeneği varsayılan olarak seçilir. Azure AD Connect'in eski sürümlerini kullanıyorsanız, **tek oturum açma** seçeneğini seçin.

![Azure AD Connect: Kullanıcı oturum açma](./media/how-to-connect-sso-quick-start/changeusersignin.png)

Sayfada **tek işareti etkinleştir'e** gelene kadar sihirbazaracılığıyla devam edin. Her Etkin Dizin ormanı için etki alanı yöneticisi kimlik bilgilerini sağlayın:

* Azure AD Connect aracılığıyla Azure AD'ye senkronize olursunuz.
* Sorunsuz SSO için etkinleştirmek istediğiniz kullanıcıları içerir.

Sihirbaz tamamlandıktan sonra, Sorunsuz SSO kiracıüzerinde etkinleştirilir.

>[!NOTE]
> Etki alanı yöneticisi kimlik bilgileri Azure AD Connect'te veya Azure AD'de depolanmaz. Yalnızca özelliği etkinleştirmek için kullanılırlar.

Sorunsuz SSO'yu doğru şekilde etkinleştirdiğinizi doğrulamak için aşağıdaki yönergeleri izleyin:

1. Kiracınız için genel yönetici kimlik bilgileriyle [Azure Active Directory yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Sol bölmede **Azure Etkin Dizini'ni** seçin.
3. **Azure AD Connect'i**seçin.
4. **Kesintisiz tek oturum açma** özelliğinin **Etkin**olarak göründüğünü doğrulayın.

![Azure portalı: Azure AD Connect bölmesi](./media/how-to-connect-sso-quick-start/sso10.png)

>[!IMPORTANT]
> Sorunsuz SSO, her AD `AZUREADSSOACC` ormanında şirket içi Active Directory (AD) adlı bir bilgisayar hesabı oluşturur. Bilgisayar `AZUREADSSOACC` hesabının güvenlik nedenleriyle güçlü bir şekilde korunması gerekir. Bilgisayar hesabını yalnızca Etki Alanı Yöneticileri yönetebilir. Bilgisayar hesabındaki Kerberos delegasyonunun devre dışı bırakıldığından ve Active Directory'deki başka hiçbir hesabın `AZUREADSSOACC` bilgisayar hesabında delegasyon izinleri olmadığından emin olun. Bilgisayar hesabını, yanlışlıkla silmelere karşı güvende oldukları ve yalnızca Etki Alanı Yöneticilerinin erişebildiği bir Kuruluş Birimi'nde (OU) depolayın.

>[!NOTE]
> Şirket içi ortamınızda Pass-the-Hash ve Kimlik Bilgisi Hırsızlığı Azaltma mimarilerini kullanıyorsanız, bilgisayar hesabının `AZUREADSSOACC` Karantina kabına konmaması için uygun değişiklikleri yapın. 

## <a name="step-3-roll-out-the-feature"></a>Adım 3: Özelliği kullanıma alma

Aşağıda verilen talimatları kullanarak Kesintisiz SSO'yu kullanıcılarınıza kademeli olarak dağıtabilirsiniz. Active Directory'de Grup İlkesi'ni kullanarak tüm veya seçili kullanıcıların Intranet bölge ayarlarına aşağıdaki Azure REKLAM URL'sini ekleyerek işe başlarsınız:

- `https://autologon.microsoftazuread-sso.com`

Buna ek olarak, Grup İlkesi **aracılığıyla komut dosyası aracılığıyla durum çubuğuna güncelleştirmelere izin ver** adlı bir Intranet bölgesi ilkesi ayarını etkinleştirmeniz gerekir. 

>[!NOTE]
> Aşağıdaki talimatlar yalnızca Windows'da Internet Explorer ve Google Chrome için çalışır (internet explorer ile güvenilir bir dizi site URL'sini paylaşıyorsa). MacOS'ta Mozilla Firefox ve Google Chrome'un nasıl kurulacağına ilişkin talimatlar için sonraki bölümü okuyun.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Neden kullanıcıların Intranet bölge ayarlarını değiştirmeniz gerekiyor?

Varsayılan olarak, tarayıcı belirli bir URL'den Internet veya Intranet gibi doğru bölgeyi otomatik olarak hesaplar. Örneğin, `http://contoso/` Intranet bölgesine eşler, `http://intranet.contoso.com/` Internet bölgesine eşler ise (URL bir dönem içerdiğinden). Tarayıcının Intranet bölgesine URL'yi açıkça eklemediğiniz sürece tarayıcılar Kerberos biletlerini Azure REKLAM URL'si gibi bir bulut bitiş noktasına göndermez.

Kullanıcıların Intranet bölge ayarlarını değiştirmenin iki yolu vardır:

| Seçenek | Yönetici değerlendirmesi | Kullanıcı deneyimleri |
| --- | --- | --- |
| Grup ilkesi | Yönetici Intranet bölge ayarlarının düzenlemekilitlerini kilitler | Kullanıcılar kendi ayarlarını değiştiremez |
| Grup ilkesi tercihi |  Yönetici Intranet bölge ayarlarında düzenleme sağlar | Kullanıcılar kendi ayarlarını değiştirebilir |

### <a name="group-policy-option---detailed-steps"></a>"Grup ilkesi" seçeneği - Ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi aracını açın.
2. Bazı veya tüm kullanıcılarınız için uygulanan grup ilkesini edin. Bu örnekte **Varsayılan Etki Alanı İlkesi**kullanır.
3. **Kullanıcı Yapılandırma** > **İlke** > **Yönetim Şablonları** > Windows**Bileşenleri** > **Internet Explorer** > **Internet Control Panel** > **Güvenlik Sayfası**göz atın. Ardından **Site'den Bölgeye Atama Listesi'ni**seçin.
    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso6.png)
4. İlkeyi etkinleştirin ve ardından iletişim kutusuna aşağıdaki değerleri girin:
   - **Değer adı**: Kerberos biletlerinin iletildiği Azure REKLAM URL'si.
   - **Değer** (Veri): **1** Intranet bölgesini gösterir.

     Sonuç şu na benzer:

     Değer adı:`https://autologon.microsoftazuread-sso.com`
  
     Değer (Veri): 1

   >[!NOTE]
   > Bazı kullanıcıların Seamless SSO kullanmasına izin vermek istiyorsanız (örneğin, bu kullanıcılar paylaşılan kiosklarda oturum açıyorsa), önceki değerleri **4**olarak ayarlayın. Bu eylem, Azure REKLAM URL'sini Kısıtlı bölgeye ekler ve Kesintisiz SSO'yu her zaman başarısız alar.
   >

5. **Tamam**’ı ve ardından tekrar **Tamam**’ı seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso7.png)

6. **Kullanıcı Yapılandırma** > **İlke** > **Yönetim Şablonları** > **Windows Bileşenleri** > **Internet Explorer** > **Internet Control Panel** > **Güvenlik Sayfası** > **Intranet Zone**göz atın. Ardından **komut dosyası aracılığıyla durum çubuğuna güncelleştirmelere izin ver'i**seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso11.png)

7. İlke ayarını etkinleştirin ve ardından **Tamam'ı**seçin.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso12.png)

### <a name="group-policy-preference-option---detailed-steps"></a>"Grup ilkesi tercihi" seçeneği - Ayrıntılı adımlar

1. Grup İlkesi Yönetimi Düzenleyicisi aracını açın.
2. Bazı veya tüm kullanıcılarınız için uygulanan grup ilkesini edin. Bu örnekte **Varsayılan Etki Alanı İlkesi**kullanır.
3. **Kullanıcı Yapılandırma** > **Tercihleri** > **Windows Ayarlar** > **Kayıt Defteri** > **Yeni** > **Kayıt Defteri öğesine**göz atın.

    ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso15.png)

4. Uygun alanlara aşağıdaki değerleri girin ve **Tamam'ı**tıklatın.
   - **Anahtar Yol**: ***Yazılım\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\microsoftazuread-sso.com\autologon***
   - **Değer adı**: ***https***.
   - **Değer türü**: ***REG_DWORD***.
   - **Değer verileri**: ***00000001***.
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso16.png)
 
     ![Çoklu oturum açma](./media/how-to-connect-sso-quick-start/sso17.png)

### <a name="browser-considerations"></a>Tarayıcı da dikkat edilmesi gerekenler

#### <a name="mozilla-firefox-all-platforms"></a>Mozilla Firefox (tüm platformlar)

Mozilla Firefox otomatik olarak Kerberos kimlik doğrulaması kullanmaz. Her kullanıcı aşağıdaki adımları kullanarak Azure REKLAM URL'sini Firefox ayarlarına el ile eklemelidir:
1. Firefox'u `about:config` çalıştırın ve adres çubuğuna girin. Gördüğünüz bildirimleri kapatın.
2. **Network.negotiate-auth.trusted-uris** tercihini arayın. Bu tercih, Firefox'un Kerberos kimlik doğrulaması için güvendiği siteleri listeler.
3. Sağ tıklayın ve **Değiştir'i**seçin.
4. Sahaya girin. `https://autologon.microsoftazuread-sso.com`
5. **Tamam'ı** seçin ve ardından tarayıcıyı yeniden açın.

#### <a name="safari-macos"></a>Safari (macOS)

macOS'u çalıştıran makinenin AD'ye katıldığından emin olun. macOS aygıtınıza AD katılma yönergeleri bu makalenin kapsamı dışındadır.

#### <a name="microsoft-edge-based-on-chromium-all-platforms"></a>Microsoft Edge Krom (tüm platformlar) dayalı

Ortamınızdaki [AuthNegotiateDelegateAllowlist](https://docs.microsoft.com/DeployEdge/microsoft-edge-policies#authnegotiatedelegateallowlist) veya [AuthServerAllowlist](https://docs.microsoft.com/DeployEdge/microsoft-edge-policies#authserverallowlist) ilke ayarlarını geçersiz kardıysanız, bunlara Azure`https://autologon.microsoftazuread-sso.com`AD'nin URL'sini () de eklediğinizden emin olun.

#### <a name="microsoft-edge-based-on-chromium-macos-and-other-non-windows-platforms"></a>Krom (macOS ve diğer Windows dışı platformlar) dayalı Microsoft Edge

Mac OS'deki ve Diğer Windows dışı platformlarda Krom'u temel alan Microsoft Edge için, izin listenize tümleşik kimlik doğrulama için Azure AD URL'sinin nasıl eklenmeyene ilişkin bilgi için [Krom İlke Listesi'ne göre Microsoft Edge'e](https://docs.microsoft.com/DeployEdge/microsoft-edge-policies#authserverallowlist) bakın.

#### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar)

Ortamınızdaki [AuthNegotiateDelegateWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthNegotiateDelegateWhitelist) veya [AuthServerWhitelist](https://www.chromium.org/administrators/policy-list-3#AuthServerWhitelist) ilke ayarlarını geçersiz kaktınızsa, bunlara`https://autologon.microsoftazuread-sso.com`Azure AD'nin URL'sini () de eklediğinizden emin olun.

#### <a name="google-chrome-macos-and-other-non-windows-platforms"></a>Google Chrome (macOS ve diğer Windows dışı platformlar)

Mac OS ve Diğer Windows dışı platformlarda Google Chrome için, tümleşik kimlik doğrulaması için Azure AD URL'sini nasıl beyaz listeye alacağıhakkında bilgi almak için [Krom Proje İlke Listesi'ne](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) bakın.

Azure REKLAM URL'sini Mac kullanıcılarında Firefox ve Google Chrome'a dağıtmak için üçüncü taraf Active Directory Group İlkesi uzantılarının kullanımı bu makalenin kapsamı dışındadır.

#### <a name="known-browser-limitations"></a>Bilinen tarayıcı sınırlamaları

Sorunsuz SSO Firefox ve Microsoft Edge tarayıcılarında özel tarama modunda çalışmaz. Tarayıcı Gelişmiş Korumalı modda çalışıyorsa, Internet Explorer'da da çalışmaz. Microsoft Edge'in Krom tabanlı bir sonraki sürümü için, tasarım gereği InPrivate ve Guest modunda çalışmaz.

## <a name="step-4-test-the-feature"></a>Adım 4: Özelliği test edin

Özelliği belirli bir kullanıcı için sınamak için, aşağıdaki tüm koşulların yerinde olduğundan emin olun:
  - Kullanıcı bir şirket aygıtında giriş yaptı.
  - Aygıt Active Directory etki alanınıza katıldı. _Aygıtın_ [Azure AD Joined](../active-directory-azureadjoin-overview.md)olması gerekmez.
  - Aygıtın etki alanı denetleyicinize (DC) kurumsal kablolu veya kablosuz ağ üzerinden veya VPN bağlantısı gibi bir uzaktan erişim bağlantısı üzerinden doğrudan bağlantısı vardır.
  - Bu [özelliği](#step-3-roll-out-the-feature) Grup İlkesi aracılığıyla bu kullanıcıya kullanıma çıkardınız.

Kullanıcının parolayı değil, yalnızca kullanıcı adını girdiği senaryoyu sınamak için:
   - `https://myapps.microsoft.com/` Yeni bir özel tarayıcı oturumunda oturum açın.

Kullanıcının kullanıcı adını veya parolayı girmek zorunda olmadığı senaryoyu sınamak için aşağıdaki adımlardan birini kullanın: 
   - `https://myapps.microsoft.com/contoso.onmicrosoft.com` Yeni bir özel tarayıcı oturumunda oturum açın. *Contoso'yu* kiracınızın adı ile değiştirin.
   - `https://myapps.microsoft.com/contoso.com` Yeni bir özel tarayıcı oturumunda oturum açın. *contoso.com* kiracınızda doğrulanmış bir etki alanıyla (federe etki alanı yla değil) değiştirin.

## <a name="step-5-roll-over-keys"></a>Adım 5: Tuşları n için devir

Adım 2'de Azure AD Connect, Sorunsuz SSO'yu etkinleştirdiğiniz tüm Etkin Dizin ormanlarında bilgisayar hesapları (Azure AD'yi temsil eden) oluşturur. Daha fazla bilgi için Azure [Active Directory Seamless Tek Oturum Açma: Teknik derin dalış](how-to-connect-sso-how-it-works.md).

>[!IMPORTANT]
>Bir bilgisayar hesabındaKerberos şifre çözme anahtarı, sızdırılmış ise, kendi AD ormanherhangi bir kullanıcı için Kerberos bilet oluşturmak için kullanılabilir. Kötü niyetli aktörler daha sonra tehlikeye giren kullanıcılar için Azure AD oturum açma larını taklit edebilir. Bu Kerberos şifre çözme anahtarlarını en az 30 günde bir periyodik olarak teslim almanızı şiddetle tavsiye ediyoruz.

Tuşların nasıl devredileceklerine ilişkin talimatlar için Azure [Active Directory Seamless Tek Oturum Açma: Sık sorulan sorular.](how-to-connect-sso-faq.md) Otomatik anahtar rulosu tanıtmak için bir yetenek üzerinde çalışıyoruz.

>[!IMPORTANT]
>Özelliği etkinleştirdikten _hemen_ sonra bu adımı yapmanız gerekmez. Kerberos şifre çözme anahtarlarını en az 30 günde bir çevirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Teknik derin dalış](how-to-connect-sso-how-it-works.md): Dikişsiz Tek İşaret-On özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-sso-faq.md): Sorunsuz Tek Oturum Açma hakkında sık sorulan soruların yanıtlarını alın.
- [Sorun Giderme](tshoot-connect-sso.md): Sorunsuz Tek Oturum Açma özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosyalamak için Azure Active Directory Forum'u kullanın.
