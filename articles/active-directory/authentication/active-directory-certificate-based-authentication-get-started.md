---
title: Sertifika tabanlı kimlik doğrulama - Azure Etkin Dizini
description: Ortamınızda sertifika tabanlı kimlik doğrulamayı nasıl yapılandırıştırmayı öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a6c44a8253c81b44d02351b2df9c943d9f358f8
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654349"
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Azure Active Directory’de sertifika tabanlı kimlik doğrulamayı kullanmaya başlama

Sertifika tabanlı kimlik doğrulama, Exchange çevrimiçi hesabınızı şu şekilde bağlarken Windows, Android veya iOS aygıtındaki istemci sertifikasıyla Azure Active Directory tarafından kimlik doğrulaması yapmanızı sağlar:

- Microsoft Outlook ve Microsoft Word gibi Microsoft mobil uygulamaları
- Exchange ActiveSync (EAS) istemcileri

Bu özelliğin yapılandırılması, mobil cihazınızdaki belirli postalara ve Microsoft Office uygulamalarına bir kullanıcı adı ve parola kombinasyonu girme gereksinimini ortadan kaldırır.

Bu konu:

- Office 365 Enterprise, Business, Education ve ABD Hükümeti planlarında kiracı kullanıcıları için sertifika tabanlı kimlik doğrulamayı yapılandırmanız ve kullanmanız için gereken adımları sağlar. Bu özellik, Office 365 Çin, ABD Hükümeti Savunması ve ABD Hükümeti Federal planlarında önizlemede kullanılabilir.
- Zaten ortak [anahtar altyapı (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) ve [AD FS](../hybrid/how-to-connect-fed-whatis.md) yapılandırılmış olduğunu varsayar.

## <a name="requirements"></a>Gereksinimler

Sertifika tabanlı kimlik doğrulamasını yapılandırmak için aşağıdaki ifadelerin doğru olması gerekir:

- Sertifika tabanlı kimlik doğrulama (CBA), yalnızca tarayıcı uygulamaları, modern kimlik doğrulaması (ADAL) kullanan yerel istemciler veya MSAL kitaplıkları için Federe ortamlar için desteklenir. Bunun tek istisnası, federe ve yönetilen hesaplar için kullanılabilen Exchange Online (EXO) için Exchange Active Sync (EAS) 'dir.
- Kök sertifika yetkilisi ve ara sertifika yetkilileri Azure Active Directory'de yapılandırılmalıdır.
- Her sertifika yetkilisinin, internete bakan bir URL üzerinden başvurulan bir sertifika iptal listesi (CRL) olması gerekir.
- Azure Etkin Dizin'de en az bir sertifika yetkiniz yapılandırılmış olmalıdır. Sertifika [yetkilileri yapılandırılması](#step-2-configure-the-certificate-authorities) bölümünde ilgili adımları bulabilirsiniz.
- Exchange ActiveSync istemcileri için, istemci sertifikasının Kullanıcının çevrimiçi olarak Ana Adı veya Konu Alternatif Ad alanının RFC822 Adı değerinde ki e-posta adresine sahip olması gerekir. Azure Active Directory, RFC822 değerini dizindeki Proxy Adresi özniteliğiyle eşler.
- İstemci aygıtınızın istemci sertifikaları veren en az bir sertifika yetkilisine erişimi olmalıdır.
- İstemci kimlik doğrulaması için bir istemci sertifikası istemcinize verilmiş olmalıdır.

>[!IMPORTANT]
>Azure Etkin Dizini'nin başarıyla indirip önbelleğe alması için bir CRL'nin maksimum boyutu 20MB'dır ve CRL'yi indirmek için gereken süre 10 saniyeyi geçmemelidir.  Azure Etkin Dizini CRL indiremezse, ilgili CA tarafından verilen sertifikaları kullanarak sertifika tabanlı kimlik doğrulamaları başarısız olur. CRL dosyalarının boyut kısıtlamaları içinde olduğundan emin olmak için en iyi uygulamalar, sertifika yaşam sürelerini makul sınırlar içinde tutmak ve süresi dolmuş sertifikaları temizlemektir. 

## <a name="step-1-select-your-device-platform"></a>Adım 1: Cihaz platformunuzu seçin

İlk adım olarak, önemsediğiniz cihaz platformu için aşağıdakileri gözden geçirmeniz gerekir:

- Office mobil uygulamaları desteği
- Belirli uygulama gereksinimleri

İlgili bilgiler aşağıdaki aygıt platformları için vardır:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)

## <a name="step-2-configure-the-certificate-authorities"></a>Adım 2: Sertifika yetkililerini yapılandırma

Her sertifika yetkilisi için Sertifika yetkilerinizi Azure Etkin Dizini'nde yapılandırmak için aşağıdakileri yükleyin:

* Sertifikanın ortak bölümü, *.cer* formatında
* Sertifika İptal Listelerinin (CRLs) bulunduğu internete bakan URL'ler

Sertifika yetkilisinin şeması aşağıdaki gibi görünür:

    class TrustedCAsForPasswordlessAuth
    {
       CertificateAuthorityInformation[] certificateAuthorities;
    }

    class CertificateAuthorityInformation

    {
        CertAuthorityType authorityType;
        X509Certificate trustedCertificate;
        string crlDistributionPoint;
        string deltaCrlDistributionPoint;
        string trustedIssuer;
        string trustedIssuerSKI;
    }

    enum CertAuthorityType
    {
        RootAuthority = 0,
        IntermediateAuthority = 1
    }

Yapılandırma için Azure Active [Directory PowerShell Sürüm 2'yi](/powershell/azure/install-adv2?view=azureadps-2.0)kullanabilirsiniz:

1. Windows PowerShell'i yönetici ayrıcalıklarıyla başlatın.
2. Azure AD modülü sürümünü [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) veya daha yüksek olarak yükleyin.

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

İlk yapılandırma adımı olarak, kiracınızla bir bağlantı kurmanız gerekir. Kiracınızla bağlantı olur olmaz, dizininizde tanımlanan güvenilir sertifika yetkililerini inceleyebilir, ekleyebilir, silebilir ve değiştirebilirsiniz.

### <a name="connect"></a>Bağlan

Kiracınızla bağlantı kurmak için [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet'ini kullanın:

    Connect-AzureAD

### <a name="retrieve"></a>Almak

Dizininizde tanımlanan güvenilir sertifika yetkililerini almak için [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'ini kullanın.

    Get-AzureADTrustedCertificateAuthority

### <a name="add"></a>Ekle

Güvenilir bir sertifika yetkilisi oluşturmak için [Yeni AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'ini kullanın ve **crlDistributionPoint** özniteliğini doğru değere ayarlayın:

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint="<CRL Distribution URL>"
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca

### <a name="remove"></a>Kaldır

Güvenilir bir sertifika yetkilisini kaldırmak için [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'ini kullanın:

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]

### <a name="modify"></a>Değiştir

Güvenilir bir sertifika yetkilisini değiştirmek için [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'ini kullanın:

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]

## <a name="step-3-configure-revocation"></a>Adım 3: İptal iyapılandır

Azure Etkin Dizin, bir istemci sertifikasını iptal etmek için, sertifika yetkilisi bilgilerinin bir parçası olarak yüklenen URL'lerden sertifika iptal listesini (CRL) getirir ve önbelleğe alınır. CRL'deki son yayımlama zaman damgası **(Etkili Tarih** özelliği) CRL'nin hala geçerli olduğundan emin olmak için kullanılır. CRL, listenin bir parçası olan sertifikalara erişimi iptal etmek için düzenli aralıklarla başvurulan bir durumdur.

Daha hızlı bir iptal gerekirse (örneğin, bir kullanıcı aygıtı kaybederse), kullanıcının yetkilendirme belirteci geçersiz kılınabilir. Yetkilendirme belirteci geçersiz kılınan için, Windows PowerShell'i kullanan bu kullanıcı için **StsRefreshTokenValidFrom** alanını ayarlayın. Erişimi iptal etmek istediğiniz her kullanıcı için **StsRefreshTokenValidFrom** alanını güncellemeniz gerekir.

İptalin devam etmesini sağlamak için CRL'nin **Geçerlilik Tarihini** **StsRefreshTokenValidFrom** tarafından belirlenen değerden sonraki bir tarihe ayarlamanız ve söz konusu sertifikanın CRL'de olduğundan emin olmalısınız.

Aşağıdaki adımlar, **StsRefreshTokenValidFrom** alanını ayarlayarak yetkilendirme belirteci güncelleştirme ve geçersiz çıkarma işlemini ana hatlar.

**İptal yapılandırmak için:**

1. Yönetici kimlik bilgileriyle MSOL hizmetine bağlanın:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. Bir kullanıcı için geçerli StsRefreshTokensValidFrom değerini alın:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Kullanıcı için geçerli zaman damgasına eşit yeni bir StsRefreshTokensValidFrom değerini yapılandırın:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Belirlediğiniz tarih gelecekte olmalıdır. Tarih gelecekte değilse, **StsRefreshTokensValidFrom** özelliği ayarlanmaz. Tarih ilerideyse, **StsRefreshTokensValidFrom** geçerli saate ayarlanır (Set-MsolUser komutuyla belirtilen tarih değil).

## <a name="step-4-test-your-configuration"></a>Adım 4: Yapılandırmanızı test edin

### <a name="testing-your-certificate"></a>Sertifikanızı test etme

İlk yapılandırma testi olarak, **aygıt tarayıcınızı**kullanarak [Outlook Web Access](https://outlook.office365.com) veya [SharePoint Online'da](https://microsoft.sharepoint.com) oturum açmayı denemelisiniz.

Oturum açma nız başarılı olursa, şunu biliyorsunuz:

- Kullanıcı sertifikası test cihazınıza sağlanmıştır
- AD FS doğru yapılandırılır

### <a name="testing-office-mobile-applications"></a>Office mobil uygulamalarını test etme

**Mobil Office uygulamanızda sertifika tabanlı kimlik doğrulamasını test etmek için:**

1. Test cihazınıza bir Office mobil uygulaması (örneğin, OneDrive) yükleyin.
3. Uygulamayı başlatın.
4. Kullanıcı adınızı girin ve ardından kullanmak istediğiniz kullanıcı sertifikasını seçin.

Başarılı bir şekilde oturum açmış olmalısınız.

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync istemci uygulamalarını test etme

Exchange ActiveSync'e (EAS) sertifika tabanlı kimlik doğrulama yoluyla erişmek için, istemci sertifikasını içeren bir EAS profilinin uygulama için kullanılabilir olması gerekir.

EAS profili aşağıdaki bilgileri içermelidir:

- Kimlik doğrulama için kullanılacak kullanıcı sertifikası

- EAS bitiş noktası (örneğin, outlook.office365.com)

Bir EAS profili, Intune gibi Mobil cihaz yönetimi (MDM) kullanılarak veya sertifikayı cihazdaki EAS profiline el ile yerleştirerek yapılandırılabilir ve cihaza yerleştirilebilir.

### <a name="testing-eas-client-applications-on-android"></a>Android'de EAS istemci uygulamalarını test etme

**Sertifika kimlik doğrulamasını test etmek için:**

1. Uygulamada, önceki bölümdeki gereksinimleri karşılayan bir EAS profilini yapılandırın.
2. Uygulamayı açın ve postanın eşitlediğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

[Android cihazlarda sertifika tabanlı kimlik doğrulama hakkında ek bilgiler.](active-directory-certificate-based-authentication-android.md)

[iOS aygıtlarında sertifika tabanlı kimlik doğrulama hakkında ek bilgiler.](active-directory-certificate-based-authentication-ios.md)
