---
title: Passwordless güvenlik anahtarı şirket içi kaynaklarda oturum açma (Önizleme)-Azure Active Directory
description: Azure Active Directory (Önizleme) kullanarak şirket içi kaynaklarda passwordless güvenlik anahtarı oturumunu nasıl etkinleştirebileceğinizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e28403d905a25e9e792b3b1f31b79c39cd7728b
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77522109"
---
# <a name="enable-passwordless-security-key-sign-in-to-on-premises-resources-with-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) ile şirket içi kaynaklarda passwordless güvenlik anahtarı oturum açma özelliğini etkinleştirin

Bu belge, **Azure AD 'ye katılmış** ve **hibrit Azure AD 'ye katılmış** Windows 10 cihazlarındaki ortamlar için, şirket içi kaynaklarda parolasız kimlik doğrulamanın etkinleştirilmesinde odaklanır. Bu işlevsellik, Microsoft uyumlu güvenlik anahtarlarını kullanarak şirket içi kaynaklara sorunsuz çoklu oturum açma (SSO) sağlar.

|     |
| --- |
| FIDO2 güvenlik anahtarları Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="sso-to-on-premises-resources-using-fido2-keys"></a>FIDO2 anahtarlarını kullanarak şirket içi kaynaklara SSO

Azure Active Directory (AD) bir veya daha fazla Active Directory etki alanı için Kerberos Anahtar verme biletleri (TGT 'ler) verebilir. Bu işlevsellik, kullanıcıların Windows 'da FIDO2 güvenlik anahtarları gibi modern kimlik bilgileriyle oturum açmasına ve geleneksel Active Directory tabanlı kaynaklara erişmesine olanak sağlar. Kerberos hizmeti biletleri ve yetkilendirme, şirket içi Active Directory etki alanı denetleyicileriniz tarafından denetlenmeye devam eder.

Şirket içi Active Directory bir Azure AD Kerberos sunucu nesnesi oluşturulur ve Azure Active Directory güvenli bir şekilde yayımlanır. Nesne herhangi bir fiziksel sunucu ile ilişkilendirilmemiş. Yalnızca, Active Directory Etki Alanı için Kerberos TGT 'leri oluşturmak üzere Azure Active Directory tarafından kullanılabilecek bir kaynaktır.

![Azure AD ve AD DS bir bilet verme bileti (TGT) alma](./media/howto-authentication-passwordless-on-premises/fido2-ticket-granting-ticket-exchange-process.png)

1. Kullanıcı Windows 10 cihazlarında bir FIDO2 güvenlik anahtarıyla oturum açar ve Azure AD kimlik doğrulamasını yapar.
1. Azure AD, kullanıcının şirket içi AD etki alanıyla eşleşen bir Kerberos sunucu anahtarı dizinini denetler.
   1. Azure AD, kullanıcının şirket içi AD etki alanı için bir Kerberos TGT 'si oluşturur. TGT yalnızca kullanıcının SID 'sini içerir. TGT 'ye yetkilendirme verisi eklenmez.
1. TGT, Azure AD birincil yenileme belirteci (PRT) ile birlikte istemciye döndürülür.
1. İstemci makinesi, şirket içi AD etki alanı denetleyicisi ile iletişim kurar ve tamamen oluşturulmuş bir TGT için kısmi TGT 'yi alır.
1. İstemci makinede artık Azure AD PRT ve tam Active Directory TGT bulunur ve hem buluta hem de şirket içi kaynaklara erişebilir.

## <a name="requirements"></a>Gereksinimler

Kuruluşlar, bu makaledeki adımları tamamlamadan önce [Windows 10 cihazlarında (Önizleme) passwordless güvenlik anahtarı Işaretini etkinleştirme](howto-authentication-passwordless-security-key.md) adımlarını tamamlamalıdır.

Kuruluşların Ayrıca aşağıdaki yazılım gereksinimlerini karşılaması gerekir.

- Cihazların Windows 10 Insider Build 18945 veya daha yeni bir sürümü çalıştırması gerekir.
- [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect)sürüm 1.4.32.0 veya sonraki bir sürüme sahip olmanız gerekir.
- Windows Server etki alanı denetleyicileriniz aşağıdaki düzeltme eklerini yüklemiş olmalıdır:
    - Windows Server 2016 için https://support.microsoft.com/help/4534307/windows-10-update-kb4534307
    - Windows Server 2019 için https://support.microsoft.com/help/4534321/windows-10-update-kb4534321

### <a name="supported-scenarios"></a>Desteklenen senaryolar

Senaryo, aşağıdaki senaryolardan her ikisinde de çoklu oturum açmayı (SSO) destekler:

- Office 365 ve diğer SAML özellikli uygulamalar gibi bulut kaynakları için.
- Şirket içi kaynaklar ve Web sitelerine Windows tümleşik kimlik doğrulaması için. Kaynaklar, IIS kimlik doğrulaması gerektiren Web siteleri ve SharePoint siteleri ve/veya NTLM kimlik doğrulaması kullanan kaynaklar içerebilir.

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar desteklenmez:

- Windows Server Active Directory Domain Services (AD DS) etki alanına katılmış (Şirket içi aygıtlar) dağıtımı.
- Güvenlik anahtarı kullanan RDP, VDı ve Citrix senaryoları.
- Bir güvenlik anahtarı kullanan S/MIME.
- Güvenlik anahtarı kullanarak "farklı çalıştır".
- Güvenlik anahtarını kullanarak bir sunucuda oturum açın.

## <a name="create-kerberos-server-object"></a>Kerberos sunucu nesnesi oluştur

Yöneticiler, şirket içi dizininizde bir Azure AD Kerberos sunucu nesnesi oluşturmak için Azure AD Connect sunucusundan PowerShell araçlarını kullanır. Kuruluşunuzda Azure AD kullanıcıları içeren her etki alanı ve orman için aşağıdaki adımları çalıştırın:

1. Azure AD Connect en son sürümüne yükseltin. Yönergeler, karma ortamınızı desteklemek için Azure AD Connect zaten yapılandırmış olduğunuz varsayılır.
1. Azure AD Connect sunucusunda, yükseltilmiş bir PowerShell istemi açın ve `C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\` gidin.
1. Şirket içi Active Directory etki alanı ve Azure Active Directory kiracınızda yeni bir Azure AD Kerberos sunucu nesnesi oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın.

> [!NOTE]
> Aşağıdaki örnekteki `contoso.corp.com`, şirket içi Active Directory etki alanı adınızla değiştirin.

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

### <a name="viewing-and-verifying-the-azure-ad-kerberos-server"></a>Azure AD Kerberos sunucusunu görüntüleme ve doğrulama

Aşağıdaki komutu kullanarak yeni oluşturulan Azure AD Kerberos sunucusunu görüntüleyebilir ve doğrulayabilirsiniz:

```powerShell
Get-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

Bu komut, Azure AD Kerberos sunucusunun özelliklerini verir. Her şeyin iyi sırada olduğunu doğrulamak için özellikleri gözden geçirebilirsiniz.

| Özellik | Açıklama |
| --- | --- |
| Kimlik | AD DS DC nesnesinin benzersiz KIMLIĞI. Bu KIMLIK bazen "yuva" veya "dal KIMLIĞI" olarak adlandırılır. |
| DomainDnsName | Active Directory Etki Alanı DNS etki alanı adı. |
| Bilgisayar hesabı | Azure AD Kerberos Sunucusu nesnesinin (DC) bilgisayar hesabı nesnesi. |
| UserAccount | Azure AD Kerberos Sunucusu TGT şifreleme anahtarını tutan devre dışı Kullanıcı hesabı nesnesi. Bu hesabın DN 'si `CN=krbtgt_AzureAD,CN=Users,<Domain-DN>` |
| KeyVersion | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının anahtar sürümü. Anahtar oluşturulduğunda sürüm atanır. Anahtar her döndürüldüğünde sürüm artar. Artış, çoğaltma meta verilerine ve muhtemelen büyük olasılıkla fazlasına dayanır. Örneğin, ilk *Keyversion* *192272*olabilir. Anahtar ilk kez döndürüldüğünde, sürüm *212621*' e kadar ilerleyebilirsiniz. Doğrulamanız gereken önemli şey, şirket içi nesne için *keyversion* ve bulut nesnesine ait *cloudkeyversion* 'un aynı olmasıdır. |
| KeyUpdatedOn | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının güncelleştirildiği veya oluşturulduğu tarih ve saat. |
| KeyUpdatedFrom | Azure AD Kerberos Sunucusu TGT şifreleme anahtarının en son güncelleştirildiği DC. |
| Cloudıd | Azure AD nesnesinden KIMLIĞI. Yukarıdaki KIMLIKLE eşleşmelidir. |
| CloudDomainDnsName | Azure AD nesnesinden *Domaindnsname* . Yukarıdaki *Domaindnsname* ile aynı olmalıdır. |
| CloudKeyVersion | Azure AD nesnesinden *Keyversion* . Yukarıdaki *Keyversion* ile aynı olmalıdır. |
| CloudKeyUpdatedOn | Azure AD nesnesinden *Keyupdatedon* . Yukarıdaki *Keyupdatedile* eşleşmelidir. |

### <a name="rotating-the-azure-ad-kerberos-server-key"></a>Azure AD Kerberos sunucu anahtarını döndürme

Azure AD Kerberos sunucu şifrelemesi krbtgt anahtarları düzenli olarak döndürülmelidir. Diğer tüm Active Directory Etki Alanı Denetleyicisi krbtgt anahtarlarını döndürmek için kullandığınız zamanlamanın aynısını izlemeniz önerilir.

> [!WARNING]
> Ancak, KRBTGT anahtarlarını döndürebileceğiniz başka araçlar vardır, ancak Azure AD Kerberos sunucunuzun krbtgt anahtarlarını döndürmek için bu belgede bahsedilen araçları kullanmanız gerekir. Bu, anahtarların hem şirket içi AD hem de Azure AD 'de güncelleştirilmesini sağlar.

```powerShell
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred -RotateServerKey
```

### <a name="removing-the-azure-ad-kerberos-server"></a>Azure AD Kerberos Sunucusu kaldırılıyor

Senaryoya dönmek ve Azure AD Kerberos sunucusunu hem şirket içi Active Directory hem de Azure Active Directory kaldırmak istiyorsanız aşağıdaki komutu çalıştırın:

```powerShell
Remove-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

### <a name="multi-forest-and-multi-domain-scenarios"></a>Çok ormanlı ve çoklu etki alanı senaryoları

Azure AD Kerberos Sunucusu nesnesi, Azure AD 'de bir *KerberosDomain* nesnesi olarak temsil edilir. Her şirket içi Active Directory etki alanı, Azure AD 'de tek bir *KerberosDomain* nesnesi olarak temsil edilir.

Örneğin, kuruluşunuzun iki etki alanı olan bir Active Directory ormanı vardır `contoso.com` ve `fabrikam.com`. Azure AD 'nin tüm orman için Kerberos TGT 'leri vermesine izin vermeyi seçerseniz, Azure AD 'de iki *KerberosDomain* nesnesi vardır. `contoso.com`için bir *KerberosDomain* nesnesi ve diğeri `fabrikam.com`. Birden çok Active Directory ormanlarınız varsa her ormandaki her etki alanı için bir *KerberosDomain* nesnesi vardır.

Kuruluşunuzda Azure AD kullanıcıları içeren her etki alanı ve ormanda [Kerberos sunucu nesnesi oluşturmak](#create-kerberos-server-object) için bu adımları çalıştırmanız gerekir.

## <a name="known-behavior"></a>Bilinen davranış

Parolanızın süresi dolmuşsa FIDO ile oturum açma engellenmiştir. Beklentisi Kullanıcı, FIDO kullanarak oturum açmadan önce kullanıcının parolasını sıfırlamasına yöneliktir.

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliği önizlemede geri bildirimde bulunmak veya sorun yaşıyorsanız, aşağıdaki adımları kullanarak Windows geri bildirim Merkezi uygulaması aracılığıyla paylaşabilirsiniz:

1. **Geri Bildirim Hub 'ını** başlatın ve oturum açtığınızdan emin olun.
1. Aşağıdaki kategoriye göre geri bildirim gönderin:
   - Kategori: güvenlik ve Gizlilik
   - Alt Kategori: FıDO
1. Günlükleri yakalamak için, **sorunu yeniden oluşturmak** için seçeneğini kullanın

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-this-work-in-my-on-premises-environment"></a>Bu, şirket içi ortammda mi çalışıyor?

Bu özellik, saf şirket içi Active Directory Domain Services (AD DS) ortamları için çalışmaz.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>Kuruluşumun kaynaklara erişmesi için iki öğeli kimlik doğrulaması gerekir. Bu gereksinimi desteklemek için ne yapabilirim?

Güvenlik anahtarları çeşitli form faktörleri halinde gelir. Cihazların bir PIN veya biyometri ile ikinci bir faktör ile nasıl etkinleştirilenebileceği hakkında tartışmak için cihaz üreticisine başvurun.

### <a name="can-admins-set-up-security-keys"></a>Yöneticiler güvenlik anahtarlarını ayarlayabilir mi?

Bu özelliğin genel kullanılabilirliği (GA) için bu özellik üzerinde çalışıyoruz.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>Uyumlu güvenlik anahtarlarını bulmak için nereye gidebilirim?

[FIDO2 güvenlik anahtarları](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>Güvenlik Anahtarımı kaybedersem ne yapmam gerekiyor?

**Güvenlik bilgileri** sayfasına giderek ve güvenlik anahtarını kaldırarak Azure Portal anahtarları kaldırabilirsiniz.

### <a name="im-not-able-to-use-fido-immediately-after-i-create-a-hybrid-azure-ad-joined-machine"></a>Karma Azure AD 'ye katılmış bir makine oluşturduktan sonra FIDO 'ı hemen kullanmıyorum

Karma Azure AD 'ye katılmış bir makineyi temiz yükleme, etki alanına katılma ve yeniden başlatma işleminden sonra bir parolayla oturum açmanız ve oturum açmak için FıDO 'ı kullanabilmeniz için ilkenin eşitlenmesini beklemeniz gerekir.

- Bir komut penceresine `dsregcmd /status` yazarak geçerli durumunuzu denetleyin ve hem *Azureadkatılmış* hem de *Domainkatılmış* ' in *Evet*olarak gösterilmediğini denetleyin.
- Bu gecikme, etki alanına katılmış cihazlar için bilinen bir sınırlamadır ve FIDO 'a özgü değildir.

### <a name="im-unable-to-get-sso-to-my-ntlm-network-resource-after-signing-in-with-fido-and-get-a-credential-prompt"></a>FIDO ile oturum açtıktan ve kimlik bilgisi istemi almaya çalıştıktan sonra NTLM ağ kaynaklarım için SSO alamıyorum

Kaynak isteğinize hizmet vermek için, yeterli sayıda etki alanı denetleyicisinin yanıt vermesi için düzeltme eki uygulanmış olduğundan emin olun. Özelliği çalıştıran bir etki alanı denetleyicisi görüp görbir şekilde bakmak için `nltest /dsgetdc:contoso /keylist /kdc`çıkışını gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Passwordless hakkında daha fazla bilgi edinin](concept-authentication-passwordless.md)