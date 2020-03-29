---
title: Office 365 ve Azure AD kullanıcıları için sertifika yenileme | Microsoft Dokümanlar
description: Bu makalede, Office 365 kullanıcılarına, sertifika yenileme konusunda bildirimde bulunan e-postalarla ilgili sorunları nasıl çözecekleri açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d98a1aabef2de505e66b2127226b9e89cd791e20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60244813"
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365 ve Azure Etkin Diziniçin federasyon sertifikalarını yenileme
## <a name="overview"></a>Genel Bakış
Azure Etkin Dizin (Azure AD) ve Active Directory Federation Services (AD FS) arasındaki başarılı federasyon için, AD FS tarafından güvenlik belirteçlerini Azure AD'ye imzalamak için kullanılan sertifikalar Azure AD'de yapılandırılanlarla eşleşmelidir. Herhangi bir uyumsuzluk güvenin bozulmasına yol açabilir. Azure AD, AD FS ve Web Application Proxy'yi dağıttığınızda (extranet erişimi için) bu bilgilerin eşit tutulmasını sağlar.

Bu makalede, belirteç imzalama sertifikalarınızı yönetmek ve bunları Azure AD ile eşit tutmak için aşağıdaki durumlarda ek bilgiler sağlanmaktadır:

* Web Uygulama Proxy'sini dağıtmazsınız ve bu nedenle federasyon meta verileri extranet'te kullanılamaz.
* Belirteç imzalama sertifikaları için AD FS varsayılan yapılandırmasını kullanmıyorsunuz.
* Bir üçüncü taraf kimlik sağlayıcısı kullanıyorsunuz.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Belirteç imzalama sertifikaları için AD FS varsayılan yapılandırma
Belirteç imzalama ve belirteç çözme sertifikaları genellikle kendi imzalı sertifikalardır ve bir yıl için iyidir. Varsayılan olarak, AD FS **AutoCertificateRollover**adlı bir otomatik yenileme işlemi içerir. AD FS 2.0 veya daha yeni bir deyişle kullanıyorsanız, Office 365 ve Azure AD sertifikanızın süresi dolmadan önce otomatik olarak güncellenir.

### <a name="renewal-notification-from-the-microsoft-365-admin-center-or-an-email"></a>Microsoft 365 yönetici merkezinden veya bir e-postadan yenileme bildirimi
> [!NOTE]
> Office için sertifikanızı yenilemenizi isteyen bir e-posta veya portal bildirimi aldıysanız, herhangi bir işlem yapmanız gerekip gerekip gerekip gerekip gerekip gerekmeden kontrol etmek için [belirteç imzalama sertifikalarında değişiklikleri yönetme'ye](#managecerts) bakın. Microsoft, herhangi bir işlem gerekmese bile sertifika yenileme bildirimlerinin gönderilmesine yol açabilecek olası bir sorunun farkındadır.
>
>

Azure AD, federasyon meta verilerini izlemeye ve bu meta verilerde belirtildiği şekilde belirteç imzalama sertifikalarını güncelleştirmeye çalışır. Belirteç imzalama sertifikalarının sona ermesinden 30 gün önce, Azure AD, federasyon meta verilerini yoklayarak yeni sertifikaların kullanılabilir olup olmadığını denetler.

* Federasyon meta verilerini başarıyla yoklayabilir ve yeni sertifikaları alabilirse, Microsoft 365 yönetici merkezinde kullanıcıya hiçbir e-posta bildirimi veya uyarı verilmez.
* Yeni belirteç imzalama sertifikalarını alamıyorsa, federasyon meta verilerine erişilemediği veya otomatik sertifika devri etkinleştirilmediği için Azure AD, Microsoft 365 yönetici merkezinde bir e-posta bildirimi ve bir uyarı yayınlar.

![Office 365 portal bildirimi](./media/how-to-connect-fed-o365-certs/notification.png)

> [!IMPORTANT]
> İş sürekliliğini sağlamak için AD FS kullanıyorsanız, bilinen sorunlariçin kimlik doğrulama hatalarının oluşmaması için lütfen sunucularınızın aşağıdaki güncelleştirmelere sahip olduğunu doğrulayın. Bu, bu yenileme ve gelecekteki yenileme dönemleri için bilinen AD FS proxy sunucu sorunlarını azaltır:
>
> Server 2012 R2 - [Windows Server Mayıs 2014 toplama](https://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 ve 2012 - [Proxy ile kimlik doğrulama Windows Server 2012 veya Windows 2008 R2 SP1 başarısız](https://support.microsoft.com/kb/3094446)
>
>

## <a name="check-if-the-certificates-need-to-be-updated"></a>Sertifikaların güncelleştirilip güncelleştirilmemesi gerekip gerekmeden kontrol edin<a name="managecerts"></a>
### <a name="step-1-check-the-autocertificaterollover-state"></a>Adım 1: AutoCertificateRollover durumunu kontrol edin
AD FS sunucunuzda PowerShell'i açın. AutoCertificateRollover değerinin True olarak ayarlı olduğundan denetleyin.

    Get-Adfsproperties

![Otomatik SertifikaRollover](./media/how-to-connect-fed-o365-certs/autocertrollover.png)

>[!NOTE] 
>AD FS 2.0 kullanıyorsanız, ilk olarak Add-Pssnapin Microsoft.Adfs.Powershell'i çalıştırın.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Adım 2: AD FS ve Azure AD'nin eşit olduğunu doğrulayın
AD FS sunucunuzda MSOnline PowerShell istemini açın ve Azure AD'ye bağlanın.

> [!NOTE]
> MSOL-Cmdlets MSOnline PowerShell modülünün bir parçasıdır.
> MSOnline PowerShell Modül'ü doğrudan PowerShell Galerisi'nden indirebilirsiniz.
> 
>

    Install-Module MSOnline

MSOnline PowerShell-Module'i kullanarak Azure AD'ye bağlanın.

    Import-Module MSOnline
    Connect-MsolService

Belirtilen etki alanı için AD FS ve Azure AD güven özelliklerinde yapılandırılan sertifikaları denetleyin.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/how-to-connect-fed-o365-certs/certsync.png)

Her iki çıktıdaki parmak izleri eşleşirse, sertifikalarınız Azure AD ile eşleşir.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Adım 3: Sertifikanızın süresinin dolmak üzere olup olmadığını kontrol edin
Get-MsolFederationProperty veya Get-AdfsCertificate çıktısında "Sonra değil" altında tarih olup olmadığını kontrol edin. Tarihe 30 günden az bir süre kaldıysa, harekete geçmelisiniz.

| Otomatik SertifikaRollover | Azure AD ile senkronize olan sertifikalar | Federasyon meta verileri herkese açık | Geçerli -liğini | Eylem |
|:---:|:---:|:---:|:---:|:---:|
| Evet |Evet |Evet |- |Eyleme gerek yok. Bkz. [Belirteç İmza sertifikasını otomatik olarak yenile.](#autorenew) |
| Evet |Hayır |- |15 günden az |Hemen yenileyin. Bkz. [Belirteç İmza sertifikasını el ile yenile.](#manualrenew) |
| Hayır |- |- |30 günden az |Hemen yenileyin. Bkz. [Belirteç İmza sertifikasını el ile yenile.](#manualrenew) |

\[-] Önemli değil

## <a name="renew-the-token-signing-certificate-automatically-recommended"></a>Belirteç imzalama sertifikasını otomatik olarak yenileme (önerilir)<a name="autorenew"></a>
Aşağıdakilerden ikisi de doğruysa, el ile herhangi bir adım gerçekleştirmeniz gerekmez:

* Extranet'ten federasyon meta verilerine erişimi etkinleştirebilen Web Application Proxy'yi dağıttınız.
* AD FS varsayılan yapılandırmasını kullanıyorsunuz (AutoCertificateRollover etkindir).

Sertifikanın otomatik olarak güncelleştirilenebilen leri doğrulamak için aşağıdakileri kontrol edin.

**1. AD FS özelliği AutoCertificateRollover True olarak ayarlanmalıdır.** Bu, AD FS'nin eskilerinin süresi dolmadan önce otomatik olarak yeni belirteç imzalama ve şifre çözme sertifikaları oluşturacağını gösterir.

**2. AD FS federasyonu meta verilerine genel olarak erişilebilir.** Genel internet üzerindeki bir bilgisayardan (şirket ağının dışında) aşağıdaki URL'ye göz gezdirerek federasyon meta verilerinizin herkese açık olduğundan kontrol edin:

https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

nerede `(your_FS_name)` federasyon hizmet ana bilgisayar adı ile değiştirilir kuruluşunuzun kullandığı gibi fs.contoso.com.  Bu ayarların her ikisini de başarıyla doğrulayabilirseniz, başka bir şey yapmanız gerekmez.  

Örnek: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml
## <a name="renew-the-token-signing-certificate-manually"></a>Belirteç imzalama sertifikasını el ile yenileme<a name="manualrenew"></a>
Belirteç imzalama sertifikalarını el ile yenilemeyi seçebilirsiniz. Örneğin, aşağıdaki senaryolar el ile yenileme için daha iyi çalışabilir:

* Belirteç imzalama sertifikaları kendi imzalı sertifikalar değildir. Bunun en yaygın nedeni, kuruluşunuzun kuruluş sertifika yetkilisinden kaydolan AD FS sertifikalarını yönetmesidir.
* Ağ güvenliği, federasyon meta verilerinin herkese açık olmasını sağlamaz.

Bu senaryolarda, belirteç imzalama sertifikalarını her güncellediğinizde, PowerShell komutu olan Update-MsolFederatedDomain'i kullanarak Office 365 etki alanınızı da güncelleştirmeniz gerekir.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Adım 1: AD FS'nin yeni belirteç imzalama sertifikalarına sahip olduğundan emin olun
**Varsayılan olmayan yapılandırma**

AD FS'nin varsayılan olmayan yapılandırmasını kullanıyorsanız **(AutoCertificateRollover'ın** **False**olarak ayarlandığı yerde), büyük olasılıkla özel sertifikalar kullanıyorsunuzdur (kendi imzasını atmadı). AD FS belirteç imzalama sertifikalarının nasıl yenileniş olduğu hakkında daha fazla bilgi için, [AD FS kendi imzalı sertifikalarını kullanmayan müşteriler için Yönerge'ye](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)bakın.

**Federasyon meta verileri herkese açık değil**

Öte yandan, **AutoCertificateRollover** **True**olarak ayarlanmışsa, ancak federasyon meta verileriniz genel olarak erişilebilir değilse, öncelikle yeni belirteç imzalama sertifikalarının AD FS tarafından oluşturulduğundan emin olun. Aşağıdaki adımları atarak yeni belirteç imzalama sertifikalarınız olduğunu onaylayın:

1. Birincil AD FS sunucusunda oturum açtığınızı doğrulayın.
2. PowerShell komut penceresini açarak ve aşağıdaki komutu çalıştırarak AD FS'deki geçerli imzalama sertifikalarını denetleyin:

    PS C:\>Get-ADFSCertificate –CertificateType belirteç imzalama

   > [!NOTE]
   > AD FS 2.0 kullanıyorsanız, önce Add-Pssnapin Microsoft.Adfs.Powershell'i çalıştırmalısınız.
   >
   >
3. Listelenen sertifikalarda komut çıktısı bak. AD FS yeni bir sertifika oluşturmuşsa, çıktıda iki sertifika görmeniz gerekir: biri **IsPrimary** değerinin **Doğru** olduğu ve **NotAfter** tarihinin 5 gün içinde olduğu, diğeri **de IsPrimary'in** **False** ve **NotAfter** olduğu bir sertifikanın gelecekte yaklaşık bir yıl olduğu.
4. Yalnızca bir sertifika görürseniz ve **NotAfter** tarihi 5 gün içindeyse, yeni bir sertifika oluşturmanız gerekir.
5. Yeni bir sertifika oluşturmak için, powershell komut isteminde aşağıdaki komutu çalıştırın: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Aşağıdaki komutu yeniden çalıştırarak güncelleştirmeyi\>doğrulayın: PS C: Get-ADFSCertificate –CertificateType belirteç imzalama

İki sertifika şimdi listelenmelidir, bunlardan biri gelecekte yaklaşık bir yıllık not **sonrası** tarihi vardır ve bunun için **IsPrimary** değeri **False'dur.**

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Adım 2: Office 365 güveni için yeni belirteç imzalama sertifikalarını güncelleştirme
Güven için kullanılacak yeni belirteç imzalama sertifikalarıyla Office 365'i şu şekilde güncelleştirin.

1. Windows PowerShell için Microsoft Azure Active Directory Modül'üne açın.
2. Çalıştır $cred=Kimlik Bilgileri Alın. Bu cmdlet kimlik bilgilerinizi istediğinde, bulut hizmeti yöneticisi hesap kimlik bilgilerinizi yazın.
3. Connect-MsolService çalıştırın –Kimlik $cred. Bu cmdlet sizi bulut hizmetine bağlar. Araç tarafından yüklenen ek cmdlets herhangi birini çalıştırmadan önce bulut hizmetine sizi bağlayan bir bağlam oluşturmak gerekir.
4. Bu komutları AD FS birincil federasyon sunucusu olmayan bir bilgisayarda çalıştırıyorsanız, AD FS &lt;birincil sunucusunun&gt;&gt; birincil &lt;AD FS sunucusunun iç FQDN adı olduğu Set-MSOLAdfscontext -Computer AD FS birincil sunucusunu çalıştırın. Bu cmdlet, sizi AD FS'ye bağlayan bir bağlam oluşturur.
5. Run Update-MSOLFederatedDomain –DomainName &lt;etki alanı&gt;. Bu cmdlet, AD FS'deki ayarları bulut hizmetine güncelleştirir ve ikisi arasındaki güven ilişkisini yapılandırır.

> [!NOTE]
> contoso.com ve fabrikam.com gibi birden çok üst düzey etki alanını desteklemeniz gerekiyorsa, **SupportMultipleDomain** anahtarını herhangi bir cmdletle kullanmanız gerekir. Daha fazla bilgi [için, Birden Çok Üst Düzey Etki Alanı Desteği'ne](how-to-connect-install-multiple-domains.md)bakın.
>


## <a name="repair-azure-ad-trust-by-using-azure-ad-connect"></a>Azure AD Bağlantısı'nı kullanarak Azure AD güvenini düzeltme<a name="connectrenew"></a>
AD FS çiftliğinizi ve Azure AD güveninizi Azure AD Connect'i kullanarak yapılandırıyorsanız, belirteç imzalama sertifikalarınız için herhangi bir işlem yapmanız gerekip gerekolmadığını tespit etmek için Azure AD Connect'i kullanabilirsiniz. Sertifikaları yenilemeniz gerekiyorsa, bunu yapmak için Azure AD Connect'i kullanabilirsiniz.

Daha fazla bilgi için [bkz.](how-to-connect-fed-management.md)

## <a name="ad-fs-and-azure-ad-certificate-update-steps"></a>AD FS ve Azure AD sertifikası güncelleştirme adımları
Belirteç imzalama sertifikaları, federasyon sunucusunun verdiği tüm belirteçleri güvenli bir şekilde imzalamak için kullanılan standart X509 sertifikalarıdır. Belirteç şifre çözme sertifikaları, gelen belirteçlerin şifresini çözmek için kullanılan standart X509 sertifikalarıdır. 

Varsayılan olarak, AD FS belirteç imzalama ve şifre çözme sertifikaları oluşturmak için otomatik olarak, hem ilk yapılandırma zamanında hem de sertifikalar son kullanma tarihlerine yaklaşırken yapılandırılır.

Azure AD, geçerli sertifikanın sona ermesinden 30 gün önce federasyon hizmeti meta verilerinden yeni bir sertifika almaya çalışır. O anda yeni bir sertifika kullanılamaması durumunda, Azure AD meta verileri düzenli günlük aralıklarla izlemeye devam eder. Yeni sertifika meta verilerde kullanılabilir olur olmaz, etki alanının federasyon ayarları yeni sertifika bilgileriyle güncelleştirilir. NextSigningCertificate `Get-MsolDomainFederationSettings` / SigningCertificate'ta yeni sertifikayı görüp görmediğinizi doğrulamak için kullanabilirsiniz.

AD FS'deki Belirteç İmzalama sertifikaları hakkında daha fazla bilgi [için](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ts-td-certs-ad-fs) bkz.
