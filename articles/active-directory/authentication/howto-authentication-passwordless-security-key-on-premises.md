---
title: Şirket içi kaynaklarda parolasız güvenlik anahtarı oturum açma (önizleme) - Azure Etkin Dizini
description: Azure Active Directory 'i kullanarak şirket içi kaynaklarda parolasız güvenlik anahtarı oturum açmayı nasıl etkinleştirin (önizleme) öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 03/09/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 181e8192170cd7394d6817edd655f4e8257b48a4
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654035"
---
# <a name="enable-passwordless-security-key-sign-in-to-on-premises-resources-with-azure-active-directory-preview"></a>Azure Active Directory (önizleme) ile şirket içi kaynaklarda parolasız güvenlik anahtarı oturum açma olanağı sağlama

Bu belge, hem **Azure AD'nin katıldığı** hem de karma **Azure AD'nin** Windows 10 aygıtlarına katıldığı ortamlar için şirket içi kaynaklara parolasız kimlik doğrulamayı etkinleştirmeye odaklanır. Bu işlevsellik, Microsoft uyumlu güvenlik anahtarlarını kullanarak şirket içi kaynaklara sorunsuz tek oturum açma (SSO) sağlar.

|     |
| --- |
| FIDO2 güvenlik anahtarları, Azure Etkin Dizini'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

## <a name="sso-to-on-premises-resources-using-fido2-keys"></a>FIDO2 tuşlarını kullanarak şirket içi kaynaklara SSO

Azure Active Directory (AD), Aktif Dizin etki alanlarınızın bir veya birkaçı için Kerberos Bilet Verme Biletleri (TGTs) verebilir. Bu işlevsellik, kullanıcıların FIDO2 güvenlik anahtarları gibi modern kimlik bilgileriyle Windows'da oturum açmalarına ve geleneksel Active Directory tabanlı kaynaklara erişmelerine olanak tanır. Kerberos Servis Biletleri ve yetkilendirme, şirket içi Active Directory etki alanı denetleyicileriniz tarafından kontrol edilmeye devam etmektedir.

Bir Azure AD Kerberos Server nesnesi şirket içi Active Directory'nizde oluşturulur ve ardından Azure Active Directory'de güvenli bir şekilde yayımlanır. Nesne herhangi bir fiziksel sunucuile ilişkili değildir. Bu, Azure Active Directory tarafından Active Directory Etki Alanınız için Kerberos TGT'leri oluşturmak için kullanılabilecek bir kaynaktır.

![Azure AD ve AD DS'den bilet (TGT) sağlayan bilet alma](./media/howto-authentication-passwordless-on-premises/fido2-ticket-granting-ticket-exchange-process.png)

1. Kullanıcı, Windows 10 aygıtında FIDO2 güvenlik anahtarıyla giriş yapar ve Azure AD'ye kimlik doğrular.
1. Azure AD, kullanıcının şirket içi AD etki alanıyla eşleşen bir Kerberos sunucu anahtarının dizinini denetler.
   1. Azure AD, kullanıcının şirket içi AD etki alanı için bir Kerberos TGT oluşturur. TGT yalnızca kullanıcının SID'sini içerir. TGT'ye yetkilendirme verisi dahil değildir.
1. TGT, Azure AD Birincil Yenileme Belirteci (PRT) ile birlikte istemciye döndürülür.
1. İstemci makinesi şirket içi bir AD etki alanı denetleyicisi ile bağlantı kurlar ve kısmi TGT'yi tam olarak oluşturulmuş bir TGT karşılığında satlar.
1. İstemci makinesi artık bir Azure AD PRT'ye ve tam bir Active Directory TGT'ye sahiptir ve hem bulut hem de şirket içi kaynaklara erişebilir.

## <a name="requirements"></a>Gereksinimler

Kuruluşlar, bu makaledeki adımları tamamlamadan önce [Windows 10 aygıtlarında parolasız güvenlik anahtarı oturumunu etkinleştirme](howto-authentication-passwordless-security-key.md) adımlarını (önizleme) tamamlamalıdır.

Kuruluşlar da aşağıdaki yazılım gereksinimlerini karşılamalıdır.

- Aygıtlar Windows 10 Insider Build 18945 veya daha yeni çalışıyor olmalıdır.
- Sürüm 1.4.32.0 veya daha sonra [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect)olmalıdır.
  - Kullanılabilir Azure AD karma kimlik doğrulama seçenekleri hakkında daha fazla bilgi için [bkz.](../../security/fundamentals/choose-ad-authn.md) [Select which installation type to use for Azure AD Connect](../hybrid/how-to-connect-install-select-installation.md)
- Windows Server etki alanı denetleyicilerinizde aşağıdaki düzeltme emaları yüklü olmalıdır:
    - Windows Server 2016 için -https://support.microsoft.com/help/4534307/windows-10-update-kb4534307
    - Windows Server 2019 için -https://support.microsoft.com/help/4534321/windows-10-update-kb4534321

### <a name="supported-scenarios"></a>Desteklenen senaryolar

Senaryo, aşağıdaki senaryoların her ikisinde de tek oturum açma (SSO) destekler:

- Office 365 ve diğer SAML özellikli uygulamalar gibi bulut kaynakları için.
- Şirket içi kaynaklar ve web sitelerine Windows-Integrated kimlik doğrulaması için. Kaynaklar, IIS Kimlik Doğrulaması gerektiren web sitelerini ve SharePoint sitelerini ve/veya NTLM kimlik doğrulamasını kullanan kaynakları içerebilir.

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar desteklenmez:

- Windows Server Active Directory Domain Services (AD DS) etki alanı (şirket içi yalnızca aygıtlar) dağıtımına katıldı.
- Güvenlik anahtarı kullanarak RDP, VDI ve Citrix senaryoları.
- Güvenlik anahtarı nı kullanarak S/MIME.
- Güvenlik anahtarı nı kullanarak "Çalıştır" olarak çalıştırın.
- Güvenlik anahtarını kullanarak sunucuda oturum açın.

## <a name="create-kerberos-server-object"></a>Kerberos sunucu nesnesi oluşturma

Yöneticiler, şirket içi dizinde bir Azure AD Kerberos Server nesnesi oluşturmak için Azure AD Connect sunucularındaki PowerShell araçlarını kullanır. Kuruluşunuzdaki Azure AD kullanıcılarını içeren her etki alanı ve ormanda aşağıdaki adımları çalıştırın:

1. Azure AD Connect'in en son sürümüne yükseltin. Yönergeler, karma ortamınızı desteklemek için Azure AD Connect'i zaten yapılandırdığınızı varsayar.
1. Azure AD Connect Server'da, yükseltilmiş bir PowerShell istemi açın ve`C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\`
1. Hem şirket içi Active Directory etki alanınızda hem de Azure Active Directory kiracınızda yeni bir Azure AD Kerberos sunucu nesnesi oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın.

> [!NOTE]
> Aşağıdaki `contoso.corp.com` örnekte şirket içi Active Directory etki alanı adınız ile değiştirin.

```powerShell
Import-Module ".\AzureAdKerberos.psd1"

# Specify the on-premises Active Directory domain. A new Azure AD
# Kerberos Server object will be created in this Active Directory domain.
$domain = "contoso.corp.com"

# Enter an Azure Active Directory global administrator username and password.
$cloudCred = Get-Credential

# Enter a domain administrator username and password.
$domainCred = Get-Credential

# Create the new Azure AD Kerberos Server object in Active Directory
# and then publish it to Azure Active Directory.
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

### <a name="viewing-and-verifying-the-azure-ad-kerberos-server"></a>Azure AD Kerberos Server'ı görüntüleme ve doğrulama

Yeni oluşturulan Azure AD Kerberos Server'ı aşağıdaki komutu kullanarak görüntüleyebilir ve doğrulayabilirsiniz:

```powerShell
Get-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

Bu komut, Azure AD Kerberos Server özelliklerini çıkarır. Her şeyin iyi durumda olduğunu doğrulamak için özellikleri gözden geçirebilirsiniz.

| Özellik | Açıklama |
| --- | --- |
| Kimlik | AD DS DC nesnesinin benzersiz kimliği. Bu kimlik bazen "yuva" veya "şube kimliği" olarak adlandırılır. |
| DomainDnsName | Etkin Dizin Etki Alanı'nın DNS etki alanı adı. |
| Bilgisayar Hesabı | Azure AD Kerberos Server nesnesinin (DC) bilgisayar hesabı nesnesi. |
| Kullanıcı Hesabı | Azure AD Kerberos Server TGT şifreleme anahtarını tutan devre dışı bırakılmış kullanıcı hesabı nesnesi. Bu hesabın DN`CN=krbtgt_AzureAD,CN=Users,<Domain-DN>` |
| Tuş Versiyonu | Azure AD Kerberos Server TGT şifreleme anahtarının anahtar sürümü. Anahtar oluşturulduğunda sürüm atanır. Sürüm daha sonra anahtar döndürüldüğünde her döndürüldüğünde artımlanır. Artışlar çoğaltma meta-veri dayanmaktadır ve büyük olasılıkla birden büyüktür. Örneğin, ilk *KeyVersion* *192272*olabilir. Anahtar ilk kez döndürüldüğünde, sürüm *212621'e*kadar ilerleyebilir. Doğrulamak için önemli olan şey, şirket içi nesne için *KeyVersion* ve bulut nesnesi için *CloudKeyVersion* aynı olmasıdır. |
| TuşGüncelleme | Azure AD Kerberos Server TGT şifreleme anahtarının güncelleştirilme veya oluşturulduğu tarih ve saat. |
| KeyUpdatedFrom | Azure AD Kerberos Server TGT şifreleme anahtarının en son güncellendiği DC. |
| Bulutlu | Azure AD Nesnesi'nden kimlik. Yukarıdaki kimlikle eşleşmelidir. |
| CloudDomainDnsName | Azure AD Nesnesinden *DomainDnsName.* Yukarıdaki *DomainDnsName* ile eşleşmelidir. |
| CloudKeyVersion | Azure AD Nesnesinden *Anahtar Sürümü.* Yukarıdaki *KeyVersion* maç gerekir. |
| CloudKeyGüncelle | Azure AD Nesnesinden *KeyUpdatedOn.* Yukarıdaki *KeyUpdatedOn* maç gerekir. |

### <a name="rotating-the-azure-ad-kerberos-server-key"></a>Azure AD Kerberos Server anahtarını döndürme

Azure AD Kerberos Server şifreleme krbtgt anahtarları düzenli olarak döndürülmelidir. Diğer tüm Active Directory Domain Controller krbtgt tuşlarını döndürmek için kullandığınız aynı zamanlamaya uymanız önerilir.

> [!WARNING]
> Krbtgt tuşlarını döndürebilecek başka araçlar da vardır, ancak Azure AD Kerberos Server'ınızın krbtgt anahtarlarını döndürmek için bu belgede belirtilen araçları kullanmanız gerekir. Bu, anahtarların hem şirket içi AD hem de Azure AD'de güncelleştirilmesini sağlar.

```powerShell
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred -RotateServerKey
```

### <a name="removing-the-azure-ad-kerberos-server"></a>Azure AD Kerberos Server'ı kaldırma

Senaryoyu tersine çevirmek ve Azure AD Kerberos Server'ı şirket içi Active Directory ve Azure Active Directory'den kaldırmak istiyorsanız aşağıdaki komutu çalıştırın:

```powerShell
Remove-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

### <a name="multi-forest-and-multi-domain-scenarios"></a>Çok ormanlı ve çok etki alanı senaryoları

Azure AD Kerberos sunucu nesnesi Azure AD'de *KerberosDomain nesnesi* olarak temsil edilir. Şirket içinde her Active Directory etki alanı, Azure AD'de tek bir *KerberosDomain* nesnesi olarak temsil edilir.

Örneğin, kuruluşunuzun iki etki alanı olan bir `contoso.com` Etkin `fabrikam.com`Dizin ormanı vardır ve. Azure AD'nin tüm orman için Kerberos TGT'ler yayınlamasına izin vermeyi seçerseniz, Azure AD'de iki *KerberosDomain* nesnesi vardır. Bir *KerberosDomain* `contoso.com`nesne için `fabrikam.com`, ve bir için . Birden çok Active Directory ormanın varsa, her ormandaki her etki alanı için bir *KerberosDomain* nesnesi vardır.

Kuruluşunuzda Azure AD kullanıcıları içeren her etki alanında ve ormanda [Kerberos sunucu nesnesi oluşturmak](#create-kerberos-server-object) için adımları çalıştırmanız gerekir.

## <a name="known-behavior"></a>Bilinen davranış

Parolanızın süresi dolmuşsa FIDO ile oturum açma engellenir. Beklenti, kullanıcının FIDO kullanarak oturum açmadan önce parolasını sıfırlamasıdır.

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliği niönizlerken geri bildirim paylaşmak veya sorunlarla karşılaşmak istiyorsanız, aşağıdaki adımları kullanarak Windows Geri Bildirim Hub uygulaması üzerinden paylaşın:

1. **Geri Bildirim Hub'ı** başlatın ve oturum açmış olduğunuzdan emin olun.
1. Aşağıdaki kategoriler altında geri bildirim gönderin:
   - Kategori: Güvenlik ve Gizlilik
   - Alt kategori: FIDO
1. Günlükleri yakalamak **için, Sorunumu Yeniden Oluşturma** seçeneğini kullanın

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-this-work-in-my-on-premises-environment"></a>Bu benim şirket içi çevremde işe yarıyor mu?

Bu özellik, saf bir şirket içi Active Directory Domain Services (AD DS) ortamı için çalışmaz.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>Kuruluşum, kaynaklara erişmek için iki faktörlü kimlik doğrulaması gerektirir. Bu gereksinimi desteklemek için ne yapabilirim?

Güvenlik anahtarları çeşitli form faktörleri gelir. Cihazlarının ikinci bir faktör olarak PIN veya biyometrik ile nasıl etkinleştirilebileceğini tartışmak için ilgi çeken aygıt üreticisine başvurun.

### <a name="can-admins-set-up-security-keys"></a>Yöneticiler güvenlik anahtarlarını ayarlayabilir mi?

Bu özelliğin genel kullanılabilirliği (GA) için bu özellik üzerinde çalışıyoruz.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>Uyumlu Güvenlik Anahtarlarını bulmak için nereye gidebilirim?

[FIDO2 güvenlik anahtarları](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>Güvenlik anahtarımı kaybedersem ne yapmalıyım?

**Güvenlik bilgileri** sayfasına giderek güvenlik anahtarını kaldırarak Azure portalından anahtarları kaldırabilirsiniz.

### <a name="im-not-able-to-use-fido-immediately-after-i-create-a-hybrid-azure-ad-joined-machine"></a>Hibrit Azure AD birleştirilmiş makine oluşturduktan hemen sonra FIDO'da kullanamam

Karma azure AD'yi yükleme yi temiz yüklerseniz, etki alanı birleştirme ve yeniden başlatma işleminden sonra bir parolayla oturum açmanız ve oturum açabilmek için FIDO'yu kullanmadan önce ilkenin eşitlenmesini beklemeniz gerekir.

- Komut penceresine `dsregcmd /status` yazarak geçerli durumunuzu kontrol edin ve hem AzureAdJoined hem de *DomainJoined'In* *EVET*gösterip göstermediğini kontrol edin. *DomainJoined*
- Bu gecikme etki alanı birleştirilmiş aygıtlar için bilinen bir sınırlamadır ve FIDO'ya özgü değildir.

### <a name="im-unable-to-get-sso-to-my-ntlm-network-resource-after-signing-in-with-fido-and-get-a-credential-prompt"></a>FIDO ile oturum açtıktan sonra SSO'yu NTLM ağ kaynağıma ulaştırıyorum ve kimlik bilgisi istemi alıyorum

Kaynak isteğinize hizmet vermek için zamanında yanıt verecek yeterli etki alanı denetleyicilerinin yamalı olduğundan emin olun. Özelliği çalıştıran bir etki alanı denetleyicisi görüp görebildiğinizi denetlemek için, 'nin çıktısını `nltest /dsgetdc:contoso /keylist /kdc`gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Şifresiz ler hakkında daha fazla bilgi edinin](concept-authentication-passwordless.md)
