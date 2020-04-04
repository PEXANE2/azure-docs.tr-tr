---
title: Azure Çok Faktörlü Kimlik Doğrulamayı Dağıtma - Azure Etkin Dizini
description: Microsoft Azure Çok Faktörlü Kimlik Doğrulama dağıtım planlaması
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ae58482ced524958ffcdd6094ae57856d088eaf
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653950"
---
# <a name="planning-a-cloud-based-azure-multi-factor-authentication-deployment"></a>Bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama

İnsanlar giderek karmaşıklaşan senaryolarda örgütsel kaynaklara bağlanıyor. İnsanlar, genellikle birden çok platformda akıllı telefonlar, tabletler, bilgisayarlar ve dizüstü bilgisayarlar kullanarak şirket ağında ve şirket dışında kuruluşa ait, kişisel ve ortak aygıtlardan bağlanır. Her zaman birbirine bağlı, çok cihazlı ve çok platformlu bu dünyada, kullanıcı hesaplarının güvenliği her zamankinden daha önemlidir. Parolalar, karmaşıklıkları ne olursa olsun, aygıtlar, ağlar ve platformlar arasında kullanılan parolalar, özellikle kullanıcılar hesaplar arasında parolaları yeniden kullanma eğiliminde olduklarında, kullanıcı hesabının güvenliğini sağlamak için artık yeterli değildir. Gelişmiş kimlik avı ve diğer sosyal mühendislik saldırıları, kullanıcı adlarının ve parolaların karanlık web'de yayınlanmasına ve satılmasına neden olabilir.

[Azure Çok Faktörlü Kimlik Doğrulama (MFA),](concept-mfa-howitworks.md) verilere ve uygulamalara erişimi korumaya yardımcı olur. İkinci bir kimlik doğrulama biçimini kullanarak ek bir güvenlik katmanı sağlar. Kuruluşlar, çözümü kendi özel gereksinimlerine uygun hale getirmek için [Koşullu Erişim'i](../conditional-access/overview.md) kullanabilir.

## <a name="prerequisites"></a>Ön koşullar

Azure Çok Faktörlü Kimlik Doğrulama'yı dağıtıma başlamadan önce göz önünde bulundurulması gereken ön koşul öğeleri vardır.

| Senaryo | Önkoşul |
| --- | --- |
| Modern kimlik doğrulama ile **yalnızca buluta özel** kimlik ortamı | **Ek önkoşul görevi yok** |
| **Karma** kimlik senaryoları | [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) dağıtılır ve kullanıcı kimlikleri Azure Active Directory ile şirket içi Active Directory Etki Alanı Hizmetleri ile senkronize edilir veya federe edilir. |
| Bulut erişimi için yayınlanan şirket içi eski uygulamalar | Azure AD [Application Proxy](../manage-apps/application-proxy.md) dağıtıldı. |
| RADIUS Kimlik Doğrulaması ile Azure MFA'yı kullanma | Bir [Ağ İlkesi Sunucusu (NPS)](howto-mfa-nps-extension.md) dağıtıldı. |
| Kullanıcılar microsoft Office 2010 veya daha önce veya iOS 11 veya daha önceki için Apple Mail'e sahiptir | iOS 12 veya daha sonra için [Microsoft Office 2013 veya sonraki](https://support.microsoft.com/help/4041439/modern-authentication-configuration-requirements-for-transition-from-o) adresine ve Apple postasına yükseltin. Koşullu Erişim, eski kimlik doğrulama protokolleri tarafından desteklenmez. |

## <a name="plan-user-rollout"></a>Kullanıcı kullanıma sunulmasını planlama

MFA kullanıma alma planınız, destek kapasiteniz dahilinde olan dağıtım dalgalarını izleyen bir pilot dağıtım içermelidir. Koşullu Erişim ilkelerinizi küçük bir pilot kullanıcı grubuna uygulayarak kullanıma başlayın. Pilot kullanıcılar, kullanılan işlem ve kayıt davranışları üzerindeki etkisini değerlendirdikten sonra, ilkeye daha fazla grup ekleyebilir veya varolan gruplara daha fazla kullanıcı ekleyebilirsiniz.

### <a name="user-communications"></a>Kullanıcı iletişimi

Kullanıcıları, planlanan iletişimlerde, yaklaşan değişiklikler, Azure MFA kayıt gereksinimleri ve gerekli kullanıcı eylemleri hakkında bilgilendirmek çok önemlidir. İletişimin, iletişim, Değişim Yönetimi veya İnsan Kaynakları departmanları gibi kuruluşunuz içinden temsilcilerle birlikte geliştirilmesini öneririz.

Microsoft, iletişimlerinizi hazırlamanıza yardımcı olmak için [iletişim şablonları](https://aka.ms/mfatemplates) ve [son kullanıcı belgeleri](../user-help/security-info-setup-signin.md) sağlar. Bu sayfadaki [https://myprofile.microsoft.com](https://myprofile.microsoft.com) **Güvenlik Bilgileri** bağlantılarını seçerek kullanıcıları doğrudan kaydolmaya gönderebilirsiniz.

## <a name="deployment-considerations"></a>Dağıtma konuları

Azure Çok Faktörlü Kimlik Doğrulama, Koşullu Erişim ile ilkeleri zorlayarak dağıtılır. [Koşullu Erişim ilkesi,](../conditional-access/overview.md) kullanıcıların şu gibi belirli ölçütler karşılandığında çok faktörlü kimlik doğrulaması gerçekleştirmelerini gerektirebilir:

* Tüm kullanıcılar, belirli bir kullanıcı, bir grubun üyesi veya atanmış rol
* Belirli bulut uygulamasına erişiliyor
* Cihaz platformu
* Cihazın durumu
* Ağ konumu veya coğrafi konumdaki IP adresi
* İstemci uygulamaları
* Oturum açma riski (Kimlik Koruması Gerektirir)
* Uyumlu cihaz
* Karma Azure AD birleştirilmiş aygıt
* Onaylı istemci başvurusu

Kuruluşunuza çok faktörlü kimlik doğrulamayı kullanıma çıkarmak için [çok faktörlü kimlik doğrulama maddelerinde](https://www.microsoft.com/download/details.aspx?id=57600&WT.mc_id=rss_alldownloads_all) özelleştirilebilir posterleri ve e-posta şablonlarını kullanın.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Koşullu Erişimile Çok Faktörlü Kimlik Doğrulamayı Etkinleştirme

Koşullu Erişim ilkeleri, kayıt dışı kullanıcıların önemli bir güvenlik göz önünde bulundurulması gereken ilk oturum açma sırasında kaydı tamamlamalarını gerektiren kaydı zorunlu kılmaktadır.

[Azure AD Kimlik Koruması,](../identity-protection/howto-configure-risk-policies.md) Azure Çok Faktörlü Kimlik Doğrulama hikayesine hem bir kayıt ilkesi hem de otomatik risk algılama ve düzeltme ilkeleri sağlar. Kimlikten ödün verilen bir tehdit olduğunda parola değişikliklerini zorlamak veya aşağıdaki [olaylar](../reports-monitoring/concept-risk-events.md)tarafından riskli kabul edildiğinde MFA'yı gerektiren ilkeler oluşturulabilir:

* Sızdırılan kimlik bilgileri
* Anonim IP adreslerinden oturum açma işlemleri
* Alışılmadık konumlara imkansız seyahat
* Alışılmadık konumlardan oturum açma işlemleri
* Bulaşma olan cihazlardan oturum açma işlemleri
* Şüpheli etkinliklerle IP adreslerinden oturum açma

Azure Active Directory Identity Protection tarafından algılanan risk algılamalarından bazıları gerçek zamanlı olarak gerçekleşir ve bazıları çevrimdışı işleme gerektirir. Yöneticiler, riskli davranışlar sergileyen ve el ile düzelten, parola değişikliği gerektiren veya Koşullu Erişim ilkelerinin bir parçası olarak çok faktörlü kimlik doğrulaması gerektiren kullanıcıları engellemeyi seçebilir.

## <a name="define-network-locations"></a>Ağ konumlarını tanımlama

Kuruluşların [adlandırılmış konumları](../conditional-access/location-condition.md#named-locations)kullanarak ağlarını tanımlamak için Koşullu Erişim'i kullanmalarını öneririz. Kuruluşunuz Kimlik Koruması kullanıyorsa, adlandırılmış konumlar yerine risk tabanlı ilkeler kullanmayı düşünün.

### <a name="configuring-a-named-location"></a>Adlandırılmış konumu yapılandırma

1. Azure portalında **Azure Etkin Dizini'ni** açın
2. **Güvenlik'i** Seçin
3. **Yönet**altında, **Adlandırılmış Konumları** seçin
4. **Yeni Konum** Seçin
5. **Ad** alanında anlamlı bir ad sağlayın
6. *IP aralıklarını* veya *Ülkeler/Bölgeler'i* kullanarak konumu tanımlayıp tanımlamadığınızı seçin
   1. IP *Aralıkları* kullanıyorsanız
      1. *Güvenilir konum olarak işaretleyip işaretlememeye*karar verin. Güvenilir bir konumdan oturum açılması, kullanıcının oturum açma riskini azaltır. Bu konumu yalnızca girilen IP aralıklarının kuruluşunuzda kurulu ve güvenilir olduğunu biliyorsanız güvenilir olarak işaretleyin.
      2. IP Aralıklarını Belirtin
   2. *Ülkeler/Bölgeler* kullanıyorsanız
      1. Açılan menüyü genişletin ve bu adlandırılmış konum için tanımlamak istediğiniz ülkeleri veya bölgeleri seçin.
      2. Bilinmeyen alanları dahil edip *etmemeye*karar verin. Bilinmeyen alanlar, bir ülkeye/bölgeye eşlenemez IP adresleridir.
7. **Oluştur**’u seçin

## <a name="plan-authentication-methods"></a>Kimlik doğrulama yöntemlerini planla

Yöneticiler, kullanıcılar için kullanılabilir hale getirmek istedikleri [kimlik doğrulama yöntemlerini](../authentication/concept-authentication-methods.md) seçebilirler. Kullanıcıların birincil metodlarının kullanılamaması durumunda bir yedekleme yöntemine sahip olmaları için tek bir kimlik doğrulama yönteminden daha fazlasına izin vermek önemlidir. Yöneticilerin etkinleştirmek için aşağıdaki yöntemler kullanılabilir:

### <a name="notification-through-mobile-app"></a>Mobil uygulama üzerinden bildirim

Mobil cihazınızdaki Microsoft Authenticator uygulamasına bir anında iletme bildirimi gönderilir. Kullanıcı bildirimi görüntüler ve doğrulamayı tamamlamak için **Onay'ı** seçer. Mobil uygulama üzerinden anında iletme bildirimleri, kullanıcılar için en az müdahaleci seçeneği sağlar. Telefon yerine veri bağlantısı kullandıkları için aynı zamanda en güvenilir ve güvenli seçenektir.

> [!NOTE]
> Kuruluşunuzun Çin'de çalışan veya Çin'e seyahat eden personeli varsa, **Android cihazlardaki** **mobil uygulama yöntemiyle bildirim** o ülkede çalışmaz. Bu kullanıcılar için alternatif yöntemler kullanılabilir hale getirilmelidir.

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadan doğrulama kodu

Microsoft Authenticator uygulaması gibi bir mobil uygulama her 30 saniyede bir yeni bir OATH doğrulama kodu oluşturur. Kullanıcı doğrulama kodunu oturum açma arabirimine girer. Telefonun veri veya hücresel sinyale sahip olup olmadığı mobil uygulama seçeneği kullanılabilir.

### <a name="call-to-phone"></a>Telefona çağrı

Kullanıcıya otomatik sesli arama yerleştirilir. Kullanıcı aramayı yanıtlar ve **#** kimlik doğrulamasını onaylamak için telefon tuş takımıüzerinde basar. Telefonla arama, bir mobil uygulamadan bildirim veya doğrulama kodu için harika bir yedekleme yöntemidir.

### <a name="text-message-to-phone"></a>Telefona kısa mesaj

Kullanıcıya doğrulama kodu içeren bir kısa mesaj gönderilir, kullanıcıdan oturum açma arabirimine doğrulama kodunu girmesi istenir.

### <a name="choose-verification-options"></a>Doğrulama seçeneklerini seçin

1. **Azure Active Directory,** **Kullanıcılar**, **Çok Faktörlü Kimlik Doğrulama'ya**göz atın.

   ![Azure portalında Azure AD Kullanıcıları blade'inden Çok Faktörlü Kimlik Doğrulama portalına erişim](media/howto-mfa-getstarted/users-mfa.png)

1. Hizmet **ayarlarına**göz atın açılır yeni sekmesinde.
1. **Doğrulama seçenekleri**altında, kullanıcıların kullanabileceği yöntemler için tüm kutuları denetleyin.

   ![Çok Faktörlü Kimlik Doğrulama hizmet ayarları sekmesinde doğrulama yöntemlerini yapılandırma](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

1. **Kaydet**'e tıklayın.
1. Hizmet **ayarları** sekmesini kapatın.

## <a name="plan-registration-policy"></a>Kayıt ilkesini planla

Yöneticiler, kullanıcıların yöntemlerini nasıl kaydedeceğini belirlemelidir. Kuruluşlar, Azure MFA ve self servis parola sıfırlama (SSPR) için [yeni birleştirilmiş kayıt deneyimini etkinleştirmelidir.](howto-registration-mfa-sspr-combined.md) SSPR, kullanıcıların çok faktörlü kimlik doğrulama için kullandıkları yöntemleri kullanarak parolalarını güvenli bir şekilde sıfırlamalarına olanak tanır. Şu anda genel önizlemede bulunan bu birleşik kaydı öneririz, çünkü kullanıcılar için harika bir deneyimdir ve her iki hizmet için de bir kez kayıt olabilir. SSPR ve Azure MFA için aynı yöntemleri etkinleştirmek, kullanıcılarınızın her iki özelliği de kullanmak üzere kaydolmasına olanak tanır.

### <a name="registration-with-identity-protection"></a>Kimlik Koruması ile Kayıt

Kuruluşunuz Azure Active Directory Identity Protection kullanıyorsa, [mfa kayıt ilkesini kullanıcılarınızın](../identity-protection/howto-mfa-policy.md) etkileşimli olarak bir sonraki oturum açmalarında kaydolmalarını istenecek şekilde yapılandırın.

### <a name="registration-without-identity-protection"></a>Kimlik Koruması Olmadan Kayıt

Kuruluşunuzun Kimlik Koruması'nı etkinleştiren lisansları yoksa, kullanıcılardan oturum açma sırasında MFA'nın bir sonraki kez kaydolması istenir. Kullanıcılar MFA ile korunan uygulamaları kullanmıyorlarsa MFA'ya kaydolmayabilir. Kötü aktörlerin bir kullanıcının parolasını tahmin edememesi ve hesabın denetimini etkin bir şekilde alarak mfa'ya kaydolması için tüm kullanıcıların kaydedilmesi önemlidir.

#### <a name="enforcing-registration"></a>Kayıt zorlama

Koşullu Erişim ilkesi aşağıdaki adımları kullanarak kullanıcıları Çok Faktörlü Kimlik Doğrulama için kaydolmaya zorlayabilir

1. Bir grup oluşturun, şu anda kayıtlı olmayan tüm kullanıcıları ekleyin.
2. Koşullu Erişim'i kullanarak, tüm kaynaklara erişmek için bu grup için çok faktörlü kimlik doğrulamasını zorlar.
3. Düzenli aralıklarla grup üyeliğini yeniden değerlendirin ve gruptan kaydolan kullanıcıları kaldırın.

[MSOnline PowerShell modülüne](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0)dayanan PowerShell komutlarına sahip kayıtlı ve kayıtlı olmayan Azure MFA kullanıcılarını tanımlayabilirsiniz.

#### <a name="identify-registered-users"></a>Kayıtlı kullanıcıları belirleme

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

#### <a name="identify-non-registered-users"></a>Kayıtlı olmayan kullanıcıları belirleme

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcıları kullanıcı başına MFA'dan Koşullu Erişim tabanlı MFA'ya dönüştürme

Kullanıcılarınız kullanıcı başına etkin leştirilmiş ve zorunlu Azure Çok Faktörlü Kimlik Doğrulaması kullanılarak etkinleştirildiyse, aşağıdaki PowerShell Koşullu Erişim tabanlı Azure Çok Faktörlü Kimlik Doğrulamasına dönüştürme de size yardımcı olabilir.

Bu PowerShell'i Bir İmKB penceresinde çalıştırın veya bir . PS1 dosyası yerel olarak çalışacak.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> Biz son zamanlarda buna göre yukarıdaki davranış ve PowerShell komut değişti. Daha önce, komut dosyası MFA yöntemlerini kaydedin, MFA'yı devre dışı bıraktı ve yöntemleri geri yükledi. Devre dışı kalma için varsayılan davranış yöntemleri temizlemediği için artık bu artık gerekli değildir.

## <a name="plan-conditional-access-policies"></a>Koşullu Erişim ilkelerini planla

MFA ve diğer denetimlerin ne zaman gerekli olduğunu belirleyecek olan Koşullu Erişim ilke stratejinizi planlamak için [Azure Etkin Dizinde Koşullu Erişim nedir?](../conditional-access/overview.md).

Azure AD kiracınızın yanlışlıkla kilitlenmesini önlemeniz önemlidir. [Kiracınızda iki veya daha fazla acil durum erişim hesabı oluşturarak](../users-groups-roles/directory-emergency-access.md) ve bunları Koşullu Erişim politikanızdan hariç tutarak bu yanlışlıkla yönetim erişimi eksikliğinin etkisini azaltabilirsiniz.

### <a name="create-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

1. Genel bir yönetici hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
   ![Pilot gruptaki Azure portal kullanıcıları için MFA'yı etkinleştirmek için Koşullu Erişim ilkesi oluşturma](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)
1. İlkiniz için anlamlı bir ad sağlayın.
1. **Kullanıcılar ve gruplar**altında:
   * **Ekle** sekmesinde, **Tüm kullanıcılar** radyo düğmesini seçin
   * **Dışla** sekmesinde, Kullanıcılar **ve gruplar** için kutuyu işaretleyin ve acil durum erişim hesaplarınızı seçin.
   * **Bitti**’ye tıklayın.
1. **Bulut uygulamaları** **altında, Tüm bulut uygulamaları** radyo düğmesini seçin.
   * İstEĞE **BAĞLI: Dışla** sekmesinde, kuruluşunuzun MFA gerektirmediği bulut uygulamalarını seçin.
   * **Bitti**’ye tıklayın.
1. **Koşullar** altında bölümü:
   * İsteğE BAĞLI: Azure Kimlik Koruması'nı etkinleştirdiyseniz, oturum açma riskini ilkenin bir parçası olarak değerlendirmeyi seçebilirsiniz.
   * İstEĞE BAĞLI: Güvenilen konumlar veya adlandırılmış konumlar yapılandırıldıysanız, bu konumları ilkeden dahil etmek veya hariç tutmak için belirtebilirsiniz.
1. **Grant**altında, **Grant erişim** radyo düğmesinin seçildiğinden emin olun.
    * **Çok faktörlü kimlik doğrulaması gerekte**kutusunu işaretleyin.
    * **Seç'i**tıklatın.
1. **Oturum** bölümünü atlayın.
1. Etkinleştir **ilkesini** **A'ya**ayarlama
1. **Oluştur'u**tıklatın.

## <a name="plan-integration-with-on-premises-systems"></a>Şirket içi sistemlerle tümleştirme yi planlama

Azure AD'ye karşı doğrudan kimlik doğrulaması yapmayan bazı eski ve şirket içi uygulamalar, şu lar dahil olmak üzere MFA'yı kullanmak için ek adımlar gerektirir:

* Uygulama proxy'sini kullanması gereken eski şirket içi uygulamalar.
* NPS sunucusu ile MFA bağdaştırıcısı kullanması gereken şirket içi RADIUS uygulamaları.
* AD FS 2016 veya daha yeni ile MFA adaptör kullanmak gerekir şirket içi AD FS uygulamaları.

Doğrudan Azure AD ile kimlik doğrulaması yapan ve modern kimlik doğrulamaya (WS-Fed, SAML, OAuth, OpenID Connect) sahip uygulamalar Koşullu Erişim ilkelerini doğrudan kullanabilir.

### <a name="use-azure-mfa-with-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy ile Azure MFA'yı kullanma

Şirket içinde ikamet eden uygulamalar Azure AD Uygulama Proxy'si aracılığıyla [Azure AD](../manage-apps/application-proxy.md) kiracınıza yayınlanabilir ve Azure AD kimlik doğrulamasını kullanacak şekilde yapılandırılırsa Azure Çok Faktörlü Kimlik Doğrulaması'ndan yararlanabilir.

Bu uygulamalar, diğer Azure AD tümleşik uygulamaları gibi Azure Çok Faktörlü Kimlik Doğrulaması'nı uygulayan Koşullu Erişim ilkelerine tabidir.

Aynı şekilde, tüm kullanıcı oturum açma ları için Azure Çok Faktörlü Kimlik Doğrulaması zorlanırsa, Azure AD Application Proxy ile yayınlanan şirket içi uygulamalar korunur.

### <a name="integrating-azure-multi-factor-authentication-with-network-policy-server"></a>Azure Çok Faktörlü Kimlik Doğrulamayı Ağ İlkesi Sunucusuyla Tümleştirme

Azure MFA için Ağ İlkesi Sunucusu (NPS) uzantısı, varolan sunucularınızı kullanarak kimlik doğrulama altyapınıza bulut tabanlı MFA özellikleri ekler. NPS uzantısı ile, mevcut kimlik doğrulama akışınıza telefon görüşmesi, kısa mesaj veya telefon uygulaması doğrulaması ekleyebilirsiniz. Bu tümleştirmenin aşağıdaki sınırlamaları vardır:

* CHAPv2 protokolü ile yalnızca kimlik doğrulama uygulaması push bildirimleri ve sesli arama desteklenir.
* Koşullu Erişim ilkeleri uygulanamaz.

NPS uzantısı, [VPN,](howto-mfa-nps-extension-vpn.md) [Uzak Masaüstü Ağ Geçidi bağlantılarını](howto-mfa-nps-extension-rdg.md)veya diğer RADIUS özellikli uygulamaları korumak için ikinci bir kimlik doğrulama faktörü sağlamak için RADIUS ve bulut tabanlı Azure MFA arasında bir bağdaştırıcı görevi görür. Bu ortamda Azure MFA'ya kaydolan kullanıcılara tüm kimlik doğrulama girişimleri için meydan okunur, Koşullu Erişim ilkelerinin olmaması, MFA'nın her zaman gerekli olduğu anlamına gelir.

#### <a name="implementing-your-nps-server"></a>NPS sunucunuzu uygulama

Dağıtılmış ve halihazırda kullanılmakta olan bir NPS örneğiniz varsa, geçerli [NPS Altyapınızı Azure Çok Faktörlü Kimlik Doğrulaması ile tümleştir'e](howto-mfa-nps-extension.md)başvurun. NPS'yi ilk kez ayarlıyorsanız, yönergeler için [Network Policy Server'a (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) başvurun. Sorun giderme kılavuzu, [Azure Çok Faktörlü Kimlik Doğrulaması için NPS uzantısından gelen hata iletilerini çözümle](howto-mfa-nps-extension-errors.md)makalesinde bulunabilir.

#### <a name="prepare-nps-for-users-that-arent-enrolled-for-mfa"></a>MFA'ya kaydolmamış kullanıcılar için NPS hazırlama

MFA'ya kayıtlı olmayan kullanıcıların kimlik doğrulaması yapmaya çalıştıklarında ne olacağını seçin. Özellik davranışını `REQUIRE_USER_MATCH` denetlemek için kayıt `HKLM\Software\Microsoft\AzureMFA` defteri ayarını kayıt defteri yolunda kullanın. Bu ayarın tek bir yapılandırma seçeneği vardır.

| Anahtar | Değer | Varsayılan |
| --- | --- | --- |
| `REQUIRE_USER_MATCH` | DOĞRU / YANLIŞ | Ayarlanmıyor (TRUE'ya eşdeğer) |

Bu ayarın amacı, bir kullanıcı MFA'ya kaydolmadığında ne yapacağını belirlemektir. Bu ayarı değiştirmenin etkileri aşağıdaki tabloda listelenmiştir.

| Ayarlar | Kullanıcı MFA Durumu | Etkiler |
| --- | --- | --- |
| Anahtar yok | Kayıtlı değil | MFA mücadelesi başarısız oldu |
| True / set'e ayarlanmış değer | Kayıtlı değil | MFA mücadelesi başarısız oldu |
| Anahtar False olarak ayarlandı | Kayıtlı değil | MFA olmadan kimlik doğrulama |
| Yanlış veya Doğru olarak ayarlanan anahtar | Kaydedildi | MFA ile kimlik doğrulaması yapılmalıdır |

### <a name="integrate-with-active-directory-federation-services"></a>Aktif Dizin Federasyon Hizmetleri ile entegre

Kuruluşunuz Azure AD ile bıkmışsa, Hem şirket içinde hem de bulutta [AD FS kaynaklarını güvence altına almak için Azure Çok Faktörlü Kimlik Doğrulaması'nı](multi-factor-authentication-get-started-adfs.md)kullanabilirsiniz. Azure MFA, parolaları azaltmanızı ve kimlik doğrulaması için daha güvenli bir yol sağlamanızı sağlar. Windows Server 2016'dan başlayarak, artık birincil kimlik doğrulaması için Azure MFA'yı yapılandırabilirsiniz.

Windows Server 2012 R2'deki AD FS'nin aksine, AD FS 2016 Azure MFA bağdaştırıcısı doğrudan Azure AD ile tümleşir ve şirket içi Azure MFA sunucusu gerektirmez. Azure MFA bağdaştırıcısı Windows Server 2016'da yerleşiktir ve ek yüklemeye gerek yoktur.

AD FS 2016 ile Azure MFA kullanırken ve hedef uygulama Koşullu Erişim ilkesine tabiyken, başka hususlar da vardır:

* Koşullu Erişim, uygulama Azure AD'ye güvenen bir taraf olduğunda, AD FS 2016 veya daha yenisiyle befek olarak kullanılabilir.
* Koşullu Erişim, uygulama AD FS 2016 veya AD FS 2019'un bağlı bir tarafı olduğunda ve AD FS 2016 veya AD FS 2019 ile yönetildiğinde veya federated olduğunda kullanılamaz.
* AD FS 2016 veya AD FS 2019 birincil kimlik doğrulama yöntemi olarak Azure MFA'yı kullanacak şekilde yapılandırıldığında Koşullu Erişim de kullanılamaz.

#### <a name="ad-fs-logging"></a>AD FS günlüğü

Standart AD FS 2016 ve 2019 oturum açma hem Windows Güvenlik Günlüğü ve AD FS Yönetici günlüğü, kimlik doğrulama istekleri ve bunların başarı veya başarısızlık hakkında bilgi içerir. Bu olaylardaki olay günlüğü verileri Azure MFA kullanılıp kullanılmadığını gösterir. Örneğin, bir AD FS Auditing Event ID 1200 şunları içerebilir:

```
<MfaPerformed>true</MfaPerformed>
<MfaMethod>MFA</MfaMethod>
```

#### <a name="renew-and-manage-certificates"></a>Sertifikaları yenileme ve yönetme

Her AD FS sunucusunda, yerel bilgisayarı My Store'da, sertifikanın son kullanma tarihini içeren OU=Microsoft AD FS Azure MFA adlı kendi imzalanmış azure MFA sertifikası olacaktır. Son kullanma tarihini belirlemek için her AD FS sunucusunda bu sertifikanın geçerlilik süresini denetleyin.

Sertifikalarınızın geçerlilik süresi sona ermek üzereyse, [her AD FS sunucusunda yeni bir MFA sertifikası oluşturun ve doğrulayın.](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa#configure-the-ad-fs-servers)

Aşağıdaki kılavuz, AD FS sunucularınızdaki Azure MFA sertifikalarını nasıl yöneteceğimize ayrıntılı olarak açıklanmaktadır. AD FS'yi Azure MFA ile yapılandırdığınızda, `New-AdfsAzureMfaTenantCertificate` PowerShell cmdlet üzerinden oluşturulan sertifikalar 2 yıl süreyle geçerlidir. Yenilenen sertifikaları, MFA hizmetindeki geçersiz kesintiler için sona ermeden önce yenileyin ve kurutun.

## <a name="implement-your-plan"></a>Planınızı uygulayın

Çözümünüzü planladığınıza göre, aşağıdaki adımları izleyerek uygulayabilirsiniz:

1. Gerekli ön koşulları yerine getirin
   1. Tüm karma senaryolar için [Azure AD Connect'i](../hybrid/whatis-hybrid-identity.md) dağıtma
   1. Bulut erişimi için yayınlanan şirket içi uygulamalar için [Azure AD Uygulama Proxy'yi](../manage-apps/application-proxy.md) dağıtma
   1. Herhangi bir RADIUS kimlik doğrulaması için [NPS'yi](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) dağıtma
   1. Kullanıcıların, modern kimlik doğrulama özelliği ne olacaksa Microsoft Office'in desteklenen sürümlerine yükselttiklerinden emin olun
1. Seçilen [kimlik doğrulama yöntemlerini](#choose-verification-options) yapılandırma
1. [Adlandırılmış ağ konumlarınızı](../conditional-access/location-condition.md#named-locations) tanımlama
1. MFA'yı kullanıma başlatmak için grupları seçin.
1. [Koşullu Erişim ilkelerinizi](#create-conditional-access-policy) yapılandırma
1. MFA kayıt politikanızı yapılandırın
   1. [Kombine MFA ve SSPR](howto-registration-mfa-sspr-combined.md)
   1. [Kimlik Koruması](../identity-protection/howto-mfa-policy.md) ile
1. Kullanıcı iletişimi gönderin ve kullanıcıların[https://aka.ms/mfasetup](https://aka.ms/mfasetup)
1. [Kimlerin kaydolduğunu takip edin](#identify-non-registered-users)

> [!TIP]
> Devlet bulutu kullanıcıları[https://aka.ms/GovtMFASetup](https://aka.ms/GovtMFASetup)

## <a name="manage-your-solution"></a>Çözümünüzü yönetin

Azure MFA için raporlar

Azure Çok Faktörlü Kimlik Doğrulama, Azure portalı üzerinden raporlar sağlar:

| Rapor | Konum | Açıklama |
| --- | --- | --- |
| Kullanım ve dolandırıcılık uyarıları | Azure REKLAM > Oturum Açma | Genel kullanım, kullanıcı özeti ve kullanıcı ayrıntıları hakkında bilgi sağlar; belirtilen tarih aralığında gönderilen dolandırıcılık uyarılarının geçmişi. |

## <a name="troubleshoot-mfa-issues"></a>Sorun giderme MFA sorunları

Microsoft Destek Merkezi'ndeki [Sorun Giderme Azure Çok Faktörlü Kimlik Doğrulama makalesinde](https://support.microsoft.com/help/2937344/troubleshooting-azure-multi-factor-authentication-issues) Azure MFA ile ilgili sık karşılaşılan sorunlara yönelik çözümler bulun.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik doğrulaması yöntemleri nelerdir?](concept-authentication-methods.md)
* [Azure Çok Faktörlü Kimlik Doğrulama ve Azure AD self servis parola sıfırlama için yakınsama kaydını etkinleştirme](concept-registration-mfa-sspr-converged.md)
* Neden bir kullanıcıdan MFA gerçekleştirmesi istendi veya istenmedi? [Azure Çok Faktörlü Kimlik Doğrulama belgesindeki Raporlar'daki Azure AD oturum açma raporuna](howto-mfa-reporting.md#azure-ad-sign-ins-report)bakın.
