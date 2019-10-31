---
title: Passwordless güvenlik anahtarı şirket içi kaynaklarda oturum açın (Önizleme)-Azure Active Directory
description: Şirket içi kaynaklarda passwordless güvenlik anahtarı oturum açma özelliğini etkinleştir (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42c42bb1e76d854723c8a88e9c3e2c104464beb0
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164867"
---
# <a name="enable-passwordless-security-key-sign-in-to-on-premises-resources-preview"></a>Şirket içi kaynaklarda passwordless güvenlik anahtarı oturum açma özelliğini etkinleştir (Önizleme)

Bu belge, **Azure AD 'ye katılmış** ve **hibrit Azure AD 'ye katılmış** Windows 10 cihazlarındaki ortamlar için, şirket içi kaynaklarda parolasız kimlik doğrulamanın etkinleştirilmesinde odaklanır. Bu işlevsellik, Microsoft uyumlu güvenlik anahtarlarını kullanarak şirket içi kaynaklara sorunsuz çoklu oturum açma (SSO) sağlar.

|     |
| --- |
| FIDO2 güvenlik anahtarları Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="sso-to-on-premises-resources-using-fido2-keys"></a>FIDO2 anahtarlarını kullanarak şirket içi kaynaklara SSO

Azure Active Directory (AD) bir veya daha fazla Active Directory etki alanı için Kerberos Anahtar verme biletleri (TGT 'ler) verebilir. Bu işlevsellik, kullanıcıların Windows 'da FIDO2 güvenlik anahtarları gibi modern kimlik bilgileriyle oturum açmasına ve geleneksel Active Directory tabanlı kaynaklara erişmesine olanak sağlar. Kerberos hizmeti biletleri ve yetkilendirme, şirket içi Active Directory etki alanı denetleyicileriniz tarafından denetlenmeye devam edecektir.

Şirket içi Active Directory bir Azure AD Kerberos sunucu nesnesi oluşturulur ve sonra güvenli bir şekilde Azure Active Directory yayımlanır. Nesne herhangi bir fiziksel sunucu ile ilişkilendirilmemiş. Yalnızca, Active Directory Etki Alanı için Kerberos Anahtar verme biletleri (TGT 'ler) oluşturmak üzere Azure Active Directory tarafından kullanılabilecek bir kaynaktır.

![Azure AD ve AD DS ile TGT ve PRıT alma](./media/howto-authentication-passwordless-on-premises/fido2-tgt-exchange-process.png)

1. Kullanıcı Windows 10 cihazlarında bir FIDO2 güvenlik anahtarıyla oturum açar ve Azure AD kimlik doğrulamasını yapar.
1. Azure AD, kullanıcının şirket içi AD etki alanıyla eşleşen bir Kerberos sunucu anahtarı dizinini denetler.
   1. Azure AD, kullanıcının şirket içi AD etki alanı için bir Kerberos TGT 'si oluşturur. TGT yalnızca kullanıcının SID 'sini içerir. TGT 'ye yetkilendirme verisi eklenmez.
1. TGT, Azure AD birincil yenileme belirteci (PRT) ile birlikte istemciye döndürülür.
1. İstemci makinesi, şirket içi AD etki alanı denetleyicisi ile iletişim kurar ve tamamen oluşturulmuş bir TGT için kısmi TGT 'yi alır.
1. İstemci makinede artık Azure AD PRT ve tam Active Directory TGT bulunur ve hem buluta hem de şirket içi kaynaklara erişebilir.

## <a name="requirements"></a>Gereksinimler

Kuruluşlar, bu makaledeki adımları tamamlamadan önce [Windows 10 cihazlarına (Önizleme) passwordless güvenlik anahtarı Işaretini etkinleştirme](howto-authentication-passwordless-security-key.md) makalesindeki adımları tamamlamalıdır.

Kuruluşların aşağıdaki yazılım gereksinimlerini karşılaması gerekir.

- Cihazların Windows 10 Insider Build 18945 veya üstünü çalıştırıyor olması gerekir
- [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect) en son sürümüne yükseltin
- Şirket içi kimlik doğrulama isteği yükünü işleyecek Windows Server 2016/2019 etki alanı denetleyicilerini tam olarak düzeltme eki uygulanmış.

### <a name="supported-scenarios"></a>Desteklenen senaryolar

- Senaryo, her ikisine birden çoklu oturum açmayı (SSO) destekler:
   - Office 365 ve diğer SAML özellikli uygulamalar gibi bulut kaynakları.
   - Şirket içi kaynaklar ve IIS kimlik doğrulaması gerektiren Web siteleri ve SharePoint siteleri ve NTLM kimlik doğrulaması kullanan kaynaklar da dahil olmak üzere Web sitelerine yönelik Windows ile tümleşik kimlik doğrulaması.

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar desteklenmez:

- Windows Server Active Directory Domain Services (AD DS) etki alanına katılmış (Şirket içi cihazlar) dağıtımı **desteklenmez**.
- RDP, VDı ve Citrix senaryoları güvenlik anahtarı kullanılarak **desteklenmez** .
- Güvenlik anahtarı kullanılarak S/MIME **desteklenmez** .
- "Farklı çalıştır" güvenlik anahtarı kullanılarak **desteklenmez** .
- Güvenlik anahtarı kullanılarak bir sunucuda oturum açma **desteklenmez**.

## <a name="create-kerberos-server-object"></a>Kerberos sunucu nesnesi oluştur

Yöneticiler, şirket içi dizininizde bir Azure AD Kerberos sunucu nesnesi oluşturmak için Azure AD Connect sunucusundan PowerShell araçlarını kullanacaktır. Bu adımları, kuruluşunuzda Azure AD kullanıcıları içeren her etki alanı ve ormanda çalıştırmanız gerekir.

1. Azure AD Connect en son sürümüne yükseltin. Yönergeler, karma ortamınızı desteklemek için Azure AD Connect zaten yapılandırmış olduğunuz varsayılır.
1. Azure AD Connect sunucusunda, yükseltilmiş bir PowerShell istemi açın ve C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\ adresine gidin.
1. Şirket içi Active Directory etki alanı ve Azure Active Directory kiracınızda yeni bir Azure AD Kerberos sunucu nesnesi oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın.

> [!NOTE]
> Aşağıdaki örnekteki "contoso.corp.com" değerini şirket içi Active Directory etki alanı adınızla değiştirin.

```PowerShell
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

### <a name="viewing-and-verifying-the-azure-ad-kerberos-server"></a>Azure AD Kerberos sunucusunu görüntüleme ve doğrulama

Aşağıdaki komutu kullanarak yeni oluşturulan Azure AD Kerberos sunucusunu görüntüleyebilir ve doğrulayabilirsiniz. 

```PowerShell
Get-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

Bu komut, Azure AD Kerberos sunucusunun özelliklerini çıktısını alacak. Her şeyin iyi sırada olduğunu doğrulamak için özellikleri gözden geçirebilirsiniz.

| Özellik | Açıklama |
| --- | --- |
| Kimlik | AD etki alanı denetleyicisi nesnesinin benzersiz KIMLIĞI. Bu KIMLIK bazen "yuva" veya "dal KIMLIĞI" olarak adlandırılır. |
| DomainDnsName | Active Directory Etki Alanı DNS etki alanı adı. |
| Bilgisayar hesabı | Azure AD Kerberos Sunucusu nesnesinin (DC) bilgisayar hesabı nesnesi. |
| UserAccount | Azure AD Kerberos Sunucusu TGT şifreleme anahtarını tutan devre dışı Kullanıcı hesabı nesnesi. Bu hesabın DN 'si şu şekilde olacaktır: <br> CN = krbtgt_AzureAD, CN = kullanıcılar, < etki alanı-DN > |
| KeyVersion | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının anahtar sürümü. Anahtar oluşturulduğunda sürüm atanır. Anahtar her döndürüldüğünde sürüm artar. Artımlar, çoğaltma meta verilerine dayalıdır ve büyük olasılıkla bunlardan büyük olacaktır. Örneğin, ilk KeyVersion 192272 olabilir. Anahtar ilk kez döndürüldüğünde, sürüm 212621 ' e kadar ilerleyebilirsiniz. Doğrulamanız gereken önemli şey, şirket içi nesne için KeyVersion ve bulut nesnesine ait CloudKeyVersion 'un aynı olmasıdır. |
| KeyUpdatedOn | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının güncelleştirildiği/oluşturulduğu tarih ve saat. |
| KeyUpdatedFrom | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının en son güncelleştirildiği etki alanı denetleyicisi. |
| Cloudıd | Azure AD nesnesinden KIMLIĞI. Yukarıdaki KIMLIKLE eşleşmelidir. |
| CloudDomainDnsName | Azure AD nesnesinden DomainDnsName. Yukarıdaki DomainDnsName ile aynı olmalıdır. |
| CloudKeyVersion | Azure AD nesnesinden KeyVersion. Yukarıdaki KeyVersion ile aynı olmalıdır. |
| CloudKeyUpdatedOn | Azure AD nesnesinden KeyUpdatedOn. Yukarıdaki Keyupdatedile eşleşmelidir. |

### <a name="rotating-the-azure-ad-kerberos-server-key"></a>Azure AD Kerberos sunucu anahtarını döndürme

Azure AD Kerberos sunucu şifrelemesi krbtgt anahtarları düzenli olarak döndürülmelidir. Diğer tüm Active Directory Etki Alanı Denetleyicisi krbtgt anahtarlarını döndürmek için kullandığınız zamanlamanın aynısını izlemeniz önerilir.

> [!WARNING]
> Ancak, KRBTGT anahtarlarını döndürebileceğiniz başka araçlar vardır, ancak Azure AD Kerberos sunucunuzun krbtgt anahtarlarını döndürmek için bu belgede bahsedilen araçları kullanmanız gerekir. Bu, anahtarların hem şirket içi AD hem de Azure AD 'de güncelleştirilmesini sağlar.

```PowerShell
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred -RotateServerKey
```

### <a name="removing-the-azure-ad-kerberos-server"></a>Azure AD Kerberos Sunucusu kaldırılıyor

Senaryoya dönmek ve Azure AD Kerberos sunucusunu hem şirket içi Active Directory hem de Azure Active Directory kaldırmak istiyorsanız aşağıdaki komutu çalıştırın.  

```PowerShell
Remove-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

### <a name="multi-forest-and-multi-domain-scenarios"></a>Çok ormanlı ve çoklu etki alanı senaryoları

Azure AD Kerberos Sunucusu nesnesi, Azure AD 'de bir KerberosDomain nesnesi olarak temsil edilir. Her şirket içi Active Directory etki alanı, Azure AD 'de tek bir KerberosDomain nesnesi olarak temsil edilir.

Örneğin, kuruluşunuz iki etki alanı olan bir Active Directory ormanına sahiptir, contoso.com ve fabrikam.com. Azure AD 'nin tüm orman için Kerberos Anahtar verme biletlerini (TGT) vermesine izin vermeyi seçerseniz, Azure AD 'de iki KerberosDomain nesnesi olacaktır. Contoso.com için bir KerberosDomain nesnesi ve bir fabrikam.com için. Birden çok Active Directory ormanınızda, her ormandaki her etki alanı için bir KerberosDomain nesneniz olur.

Kuruluşunuzda Azure AD kullanıcıları içeren her etki alanı ve ormandaki [Kerberos sunucu nesnesi oluşturma](#create-kerberos-server-object) bölümünde yer alan adımları çalıştırmanız gerekecektir.

## <a name="known-behavior"></a>Bilinen davranış

Parolanızın süresi dolmuşsa FIDO ile oturum açma engellenecektir. Beklentisi Kullanıcı, FIDO kullanarak oturum açmadan önce kullanıcının parolasını sıfırlamasına yöneliktir.

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliğin önizlemesini yaparken geri bildirimde bulunmak veya sorun yaşarsanız, lütfen Windows Geri Bildirim Hub 'ı uygulaması aracılığıyla paylaşabilirsiniz.

1. **Geri Bildirim Hub 'ını** başlatın ve oturum açtığınızdan emin olun.
1. Aşağıdaki kategoriye göre geri bildirim gönderin:
   1. Kategori: güvenlik ve Gizlilik
   1. Alt Kategori: FıDO
1. Günlükleri yakalamak için, şu seçeneği kullanın: **sorunum yeniden oluştur**

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-this-work-in-my-on-premises-environment"></a>Bu, şirket içi ortammda mi çalışıyor?

Bu özellik, saf şirket içi Active Directory Domain Services (AD DS) ortamları için çalışmaz.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>Kuruluşumun kaynaklara erişmesi için iki öğeli kimlik doğrulaması gerekiyor, bu gereksinimi desteklemek için ne yapabilirim?

Güvenlik anahtarları çeşitli form faktörleri halinde gelir. Cihazların bir PIN veya biyometri ile ikinci bir faktör ile nasıl etkinleştirilenebileceği hakkında tartışmak için lütfen cihaz üreticisine başvurun.

### <a name="can-admins-set-up-security-keys"></a>Yöneticiler güvenlik anahtarlarını ayarlayabilir mi?

Bu özelliğin genel kullanılabilirliği (GA) için bu özellik üzerinde çalışıyoruz.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>Uyumlu güvenlik anahtarlarını bulmak için nereye gidebilirim?

[FIDO2 güvenlik anahtarları](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>Güvenlik Anahtarımı kaybedersem ne yapmam gerekiyor?

Güvenlik bilgileri sayfasına giderek ve güvenlik anahtarını kaldırarak Azure portal anahtarları kaldırabilirsiniz.

### <a name="im-not-able-to-use-fido-immediately-after-i-create-a-hybrid-azure-ad-joined-machine"></a>Karma Azure AD 'ye katılmış bir makine oluşturduktan sonra FIDO 'ı hemen kullanmıyorum

Karma bir Azure AD 'ye katılmış makine yükleme, etki alanına katılma ve yeniden başlatma için temiz bir parola ile oturum açmanız ve oturum açmak için FıDO 'ı kullanabilmeniz için ilkenin eşitlenmesini beklemeniz gerekir.

- Bir komut penceresine `dsregcmd /status` yazarak geçerli durumunuzu denetleyin ve hem `AzureAdJoined` hem de `DomainJoined` `YES`gösterilmediğini denetleyin.
- Bu gecikme, etki alanına katılmış cihazlara yönelik bilinen bir sınırlamadır ve FIDO özgü değildir.

### <a name="im-unable-to-get-sso-to-my-ntlm-network-resource-after-signing-in-with-fido-and-get-a-credential-prompt"></a>FIDO ile oturum açtıktan ve kimlik bilgisi istemi almaya çalıştıktan sonra NTLM ağ kaynaklarım için SSO alamıyorum

Kaynak isteğinize hizmet vermek için, yeterli sayıda etki alanı denetleyicisinin yanıt vermesi için düzeltme eki uygulanmış olduğundan emin olun. Özelliği çalıştıran bir etki alanı denetleyicisi görüp görbir şekilde bakmak için, NLTEST/dsgetdc: contoso/keylist/KDC. çıktısını denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Passwordless hakkında daha fazla bilgi edinin](concept-authentication-passwordless.md)
