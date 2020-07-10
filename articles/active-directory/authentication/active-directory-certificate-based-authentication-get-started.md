---
title: Sertifika tabanlı kimlik doğrulaması-Azure Active Directory
description: Ortamınızda sertifika tabanlı kimlik doğrulamasını yapılandırma hakkında bilgi edinin
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
ms.custom: has-adal-ref
ms.openlocfilehash: ca19ccb925721126f7e7d8495addd0794766f376
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202874"
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Azure Active Directory’de sertifika tabanlı kimlik doğrulamayı kullanmaya başlama

Sertifika tabanlı kimlik doğrulaması, Exchange Online hesabınızı uygulamasına bağlarken bir Windows, Android veya iOS cihazında istemci sertifikası ile Azure Active Directory tarafından kimlik doğrulaması yapmanızı sağlar:

- Microsoft Outlook ve Microsoft Word gibi Microsoft mobil uygulamaları
- Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandırmak, mobil cihazınızda belirli e-posta ve Microsoft Office uygulamalarına bir Kullanıcı adı ve parola birleşimi girme gereksinimini ortadan kaldırır.

Bu konu:

- Office 365 Kurumsal, Iş, eğitim ve ABD devlet planlarındaki kiracılar kullanıcıları için sertifika tabanlı kimlik doğrulamasını yapılandırma ve kullanma adımlarını sağlar. Bu özellik, Office 365 Çin, ABD kamu savunması ve ABD kamu Federal planlarında önizlemede sunulmaktadır.
- Zaten bir [ortak anahtar altyapısına (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) sahip olduğunuzu ve [AD FS](../hybrid/how-to-connect-fed-whatis.md) yapılandırıldığını varsayar.

## <a name="requirements"></a>Gereksinimler

Sertifika tabanlı kimlik doğrulamasını yapılandırmak için aşağıdaki deyimler doğru olmalıdır:

- Sertifika tabanlı kimlik doğrulaması (CBA) yalnızca tarayıcı uygulamaları için Federasyon ortamları, modern kimlik doğrulaması (ADAL) kullanan yerel istemciler veya MSAL kitaplıkları için desteklenir. Bir özel durum, Exchange Online (EXO) için Exchange Active Sync (EAS) ve Federasyon ve yönetilen hesaplar için kullanılabilir.
- Kök sertifika yetkilisi ve tüm ara sertifika yetkilileri Azure Active Directory ' de yapılandırılmış olmalıdır.
- Her sertifika yetkilisinin, internet 'e yönelik bir URL aracılığıyla başvurulabilen bir sertifika iptal listesi 'ne (CRL) sahip olması gerekir.
- Azure Active Directory en az bir sertifika yetkilinizin yapılandırılmış olması gerekir. [Sertifika yetkililerini yapılandırma](#step-2-configure-the-certificate-authorities) bölümünde ilgili adımları bulabilirsiniz.
- Exchange ActiveSync istemcileri için, istemci sertifikasının asıl ad veya konu alternatif adı alanının RFC822 adı değeri içinde Exchange Online 'da kullanıcının yönlendirilebilir e-posta adresine sahip olması gerekir. Azure Active Directory, RFC822 değerini dizindeki proxy adresi özniteliğiyle eşler.
- İstemci cihazınızın, istemci sertifikaları veren en az bir sertifika yetkilisine erişimi olmalıdır.
- İstemciye istemci kimlik doğrulaması için bir istemci sertifikası verilmiş olmalıdır.

>[!IMPORTANT]
>Azure Active Directory için CRL 'nin başarıyla indirileceği ve önbelleğe aldığı en büyük boyut 20 MB 'tır ve CRL 'YI indirmek için gereken süre 10 saniye değerini aşmamalıdır.  Azure Active Directory CRL indiremez, karşılık gelen CA tarafından verilen sertifikaları kullanarak sertifika tabanlı kimlik doğrulamaları başarısız olur. CRL dosyalarının boyut kısıtlamalarına göre olduğundan emin olmak için en iyi yöntemler, sertifika ömrünü makul sınırlar içinde tutmalarıdır ve süresi dolmuþ sertifikaları temizler.

## <a name="step-1-select-your-device-platform"></a>1. Adım: cihaz platformunuzu seçin

İlk adım olarak, ilgilendiğiniz cihaz platformu için aşağıdakileri gözden geçirmeniz gerekir:

- Office mobil uygulamaları desteği
- Belirli uygulama gereksinimleri

Aşağıdaki cihaz platformları için ilgili bilgiler bulunur:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)

## <a name="step-2-configure-the-certificate-authorities"></a>2. Adım: sertifika yetkililerini yapılandırma

Sertifika yetkilinizi Azure Active Directory ' de yapılandırmak için, her sertifika yetkilisi için aşağıdakileri karşıya yükleyin:

* Sertifikanın *. cer* biçimindeki genel bölümü
* Sertifika Iptal listelerinin (CRL 'Ler) bulunduğu internet 'e yönelik URL 'Ler

Bir sertifika yetkilisinin şeması şu şekilde görünür:

```csharp
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
```

Yapılandırma için [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0)' yi kullanabilirsiniz:

1. Windows PowerShell 'i yönetici ayrıcalıklarıyla başlatın.
2. Azure AD modülü sürüm [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) veya üstünü yükler.

```powershell
    Install-Module -Name AzureAD –RequiredVersion 2.0.0.33
```

İlk yapılandırma adımı olarak, kiracınızla bir bağlantı kurmanız gerekir. Kiracınızla bağlantı varsa, dizininizde tanımlı olan güvenilen sertifika yetkililerini gözden geçirebilir, ekleyebilir, silebilir ve değiştirebilirsiniz.

### <a name="connect"></a>Bağlanma

Kiracınızla bir bağlantı kurmak için [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet 'ini kullanın:

```azurepowershell
    Connect-AzureAD
```

### <a name="retrieve"></a>Almanın

Dizininizde tanımlı güvenilen sertifika yetkililerini almak için [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet 'ini kullanın.

```azurepowershell
    Get-AzureADTrustedCertificateAuthority
```

### <a name="add"></a>Ekle

Güvenilen bir sertifika yetkilisi oluşturmak için [New-AzureADTrustedCertificateAuthority](/azurepowershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet 'ini kullanın ve **CRLDistributionPoint** özniteliğini doğru bir değere ayarlayın:

```azurepowershell
    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint="<CRL Distribution URL>"
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca
```

### <a name="remove"></a>Kaldır

Güvenilen bir sertifika yetkilisini kaldırmak için [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet 'ini kullanın:

```azurepowershell
    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]
```

### <a name="modify"></a>Değiştir

Güvenilen bir sertifika yetkilisini değiştirmek için [set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet 'ini kullanın:

```azurepowershell
    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]
```

## <a name="step-3-configure-revocation"></a>3. Adım: iptali yapılandırma

İstemci sertifikasını iptal etmek için Azure Active Directory Sertifika yetkilisi bilgilerinin bir parçası olarak karşıya yüklenen URL 'lerden sertifika iptal listesini (CRL) getirir ve önbelleğe alır. CRL 'nin hala geçerli olduğundan emin olmak için CRL 'deki son yayımlama zaman damgası (**geçerlilik tarihi** özelliği) kullanılır. CRL, listenin bir parçası olan sertifikalara erişimi iptal etmek için düzenli olarak başvurulur.

Daha hızlı bir iptal gerekiyorsa (örneğin, bir Kullanıcı bir cihazı kaybederse), kullanıcının yetkilendirme belirteci geçersiz kılınabilir. Yetkilendirme belirtecini geçersiz kılmak için, Windows PowerShell kullanarak bu belirli kullanıcı için **StsRefreshTokenValidFrom** alanını ayarlayın. Erişimini iptal etmek istediğiniz her kullanıcı için **StsRefreshTokenValidFrom** alanını güncelleştirmeniz gerekir.

İptal etme işlemi devam ettiğinden, CRL 'nin **geçerlilik tarihini** **StsRefreshTokenValidFrom** tarafından ayarlanan değerden sonraki bir tarihe ayarlamanız ve söz konusu sertifikanın CRL 'de olduğundan emin olmanız gerekir.

Aşağıdaki adımlar, **StsRefreshTokenValidFrom** alanını ayarlayarak yetkilendirme belirtecini güncelleştirme ve geçersiz kılma sürecini özetler.

**İptali yapılandırmak için:**

1. Yönetici kimlik bilgileriyle MSOL hizmetine bağlanın:

```powershell
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
```

2. Bir kullanıcı için geçerli StsRefreshTokensValidFrom değerini Al:

```powershell
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom
```

3. Kullanıcı için geçerli zaman damgasına eşit yeni bir StsRefreshTokensValidFrom değeri yapılandırın:

```powershell
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")
```

Ayarladığınız tarih gelecekte olmalıdır. Tarih gelecekte değilse, **StsRefreshTokensValidFrom** özelliği ayarlı değildir. Tarih gelecekte ise, **StsRefreshTokensValidFrom** geçerli saate ayarlanır (set-MsolUser komutuyla gösterilen tarih değil).

## <a name="step-4-test-your-configuration"></a>4. Adım: yapılandırmanızı test etme

### <a name="testing-your-certificate"></a>Sertifikanızı test etme

İlk yapılandırma testi olarak, **cihaz içi tarayıcınızı**kullanarak [Outlook Web erişimi](https://outlook.office365.com) veya [SharePoint Online](https://microsoft.sharepoint.com) 'da oturum açmaya çalışın.

Oturum açma işlemi başarılı olursa şunları bilirsiniz:

- Kullanıcı sertifikası test cihazınıza sağlandı
- AD FS doğru şekilde yapılandırıldı

### <a name="testing-office-mobile-applications"></a>Office mobil uygulamalarını test etme

**Mobil Office uygulamanızda sertifika tabanlı kimlik doğrulamasını test etmek için:**

1. Test cihazınızda bir Office mobil uygulaması (örneğin, OneDrive) yükleyin.
3. Uygulamayı başlatın.
4. Kullanıcı adınızı girin ve ardından kullanmak istediğiniz kullanıcı sertifikasını seçin.

Oturumunuz başarıyla açıldı.

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync istemci uygulamalarını test etme

Sertifika tabanlı kimlik doğrulama aracılığıyla Exchange ActiveSync 'e (EAS) erişmek için, istemci sertifikasını içeren bir EAS profilinin uygulama için kullanılabilir olması gerekir.

EAS profilinin aşağıdaki bilgileri içermesi gerekir:

- Kimlik doğrulaması için kullanılacak kullanıcı sertifikası

- EAS uç noktası (örneğin, outlook.office365.com)

EAS profili, Intune gibi mobil cihaz yönetimi (MDM) kullanılarak veya sertifikayı cihazdaki EAS profiline el ile yerleştirerek cihaza bağlanabilir ve yerleştirilebilir.

### <a name="testing-eas-client-applications-on-android"></a>Android 'de EAS istemci uygulamalarını test etme

**Sertifika kimlik doğrulamasını test etmek için:**

1. Uygulamada, önceki bölümde yer alan gereksinimleri karşılayan bir EAS profili yapılandırın.
2. Uygulamayı açın ve postanın eşitlenip eşitlenmediğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

[Android cihazlarda sertifika tabanlı kimlik doğrulaması hakkında ek bilgiler.](active-directory-certificate-based-authentication-android.md)

[İOS cihazlarında sertifika tabanlı kimlik doğrulaması hakkında ek bilgiler.](active-directory-certificate-based-authentication-ios.md)
