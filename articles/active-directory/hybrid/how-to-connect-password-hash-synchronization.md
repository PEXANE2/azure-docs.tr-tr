---
title: Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama | Microsoft Docs
description: Parola Karması eşitleme nasıl çalıştığı ve nasıl ayarlandığı hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
search.appverid:
- MET150
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6451507eb5a25f432c73468d0da0db1838c8c9a
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79261391"
---
# <a name="implement-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama
Bu makalede, şirket içi Active Directory örneğinden bulut tabanlı bir Azure Active Directory (Azure AD) örneği, kullanıcı parolalarını eşitlemek için gereken bilgileri sağlar.

## <a name="how-password-hash-synchronization-works"></a>Parola karması eşitleme nasıl çalışır?
Active Directory etki alanı hizmeti parolaları bir karma değer gösterimini gerçek kullanıcı parolasının biçiminde depolar. Karma değeri, tek yönlü matematik işlevinin ( *karma algoritma*) bir sonucudur. Tek yönlü işlevin sonucunu, parolanın düz metin sürümüne döndürmek mümkün değildir. 

Parolanızı eşitlemek için şirket içi Active Directory örneğinden, parola karmasını Azure AD Connect eşitleme ayıklar. Azure Active Directory kimlik doğrulama hizmeti eşitlenmeden önce ek güvenlik işleme için parola karması uygulanır. Parolalar, kullanıcı başına temelinde ve kronolojik sırayla eşitlenir.

Parola Karması eşitleme işlemini gerçek veri akışını verilerinin eşitlemesine ilişkin kullanıcı için benzerdir. Ancak, parolaları diğer öznitelikler için standart dizin eşitleme penceresinde daha sık eşitlenir. Parola Karması eşitleme işlemi 2 dakikada bir çalışır. Bu işlemin sıklığını değiştiremezsiniz. Parola Eşitleme sırasında mevcut bulut parolayı üzerine yazar.

İlk kez parola karması eşitleme özelliğini etkinleştirdiğinizde tüm kapsamındaki kullanıcıların parolalarının ilk eşitlemeyi gerçekleştirir. Bir alt kümesini eşitlemek istediğiniz kullanıcı parolalarını açıkça tanımlayamazsınız. Ancak, birden çok bağlayıcı varsa, bazı bağlayıcılar için parola karma eşitlemesini devre dışı bırakmak mümkündür ancak [set-ADSyncAADPasswordSyncConfiguration](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant) cmdlet 'ini kullanarak diğerlerine uygulanmaz.

Bir şirket içi parolayı değiştirdiğinizde, güncelleştirilmiş parolayı, genellikle birkaç dakika içinde eşitlenir.
Parola Karması eşitleme özelliği eşitleme başarısız girişimleri otomatik olarak yeniden dener. Bir hata, bir parola eşitleme girişimi sırasında bir hata meydana gelirse, Olay Görüntüleyicisi'nde günlüğe kaydedilir.

Parola Eşitleme, şu anda oturum açan kullanıcının herhangi bir etkisi yoktur.
Geçerli bulut hizmeti oturumunuzu, bir bulut hizmeti için oturum açtığınız sırada gerçekleşen, eşitlenmiş parola değişikliği hemen etkilenmez. Ancak, bulut hizmetine yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı vermeniz gerekir.

Bir kullanıcı şirket kimlik bilgilerini, olup, şirket ağlarına oturum açmadıysanız bağımsız olarak Azure AD'ye kimlik doğrulaması için ikinci bir kez girmeniz gerekir. Bu düzen, ancak kullanıcı oturum açma sırasında (KMSI) onay kutusu Oturumumu açık bırak seçerse indirgenebilir. Bu seçim, 180 gün için kimlik doğrulamasını atlar bir oturum tanımlama bilgisini ayarlar. KMSI'yi davranışı, etkin veya Azure AD Yöneticisi tarafından devre dışı. Ayrıca, şirket ağınıza bağlı şirket cihazlarındaki kullanıcıları otomatik olarak imzalayan [sorunsuz SSO](how-to-connect-sso.md)'yu etkinleştirerek parola istemlerini azaltabilirsiniz.

> [!NOTE]
> Parola Eşitleme, yalnızca Active Directory nesne türü kullanıcı için desteklenir. InetOrgPerson nesnesi türü için desteklenmiyor.

### <a name="detailed-description-of-how-password-hash-synchronization-works"></a>Parola Karması eşitleme nasıl çalıştığına ilişkin ayrıntılı bir açıklaması

Aşağıdaki bölümde açıklanmaktadır, ayrıntılı, Active Directory ve Azure AD arasında parola karması eşitleme nasıl çalışır.

![Ayrıntılı parola akış](./media/how-to-connect-password-hash-synchronization/arch3b.png)

1. Her iki dakikada bir DC gelen parola karmalarını (unicodePwd özniteliğini) AD Connect sunucusu isteklerde parola karması eşitleme Aracısı depolanır.  Bu istek, DC 'Ler arasında verileri eşzamanlı hale getirmek için kullanılan standart [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) çoğaltma protokolü aracılığıyla yapılır. Hizmet hesabı, parola karmaları almak için dizin değişikliklerini çoğaltma ve (varsayılan olarak yükleme izni) dizin değişikliklerini tümüne çoğaltma AD izinleri olmalıdır.
2. Göndermeden önce, DC, RPC oturum anahtarının [MD5](https://www.rfc-editor.org/rfc/rfc1321.txt) karması ve bir güvenlik alanı olan bir anahtar kullanarak MD4 parola karmasını şifreler. Bunu ardından sonuç parola karması eşitleme Aracısı ile RPC üzerinden gönderir. Eşitleme aracısına ayrıca geçirir salt aracı Zarf şifresini olacak şekilde DC çoğaltma protokolü kullanarak etki alanı denetleyicisi.
3. Parola karması eşitleme aracısının şifreli zarfı olduktan sonra, alınan verilerin şifresinin çözülmesi için bir anahtar oluşturmak üzere [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) kullanır ve bu ANAHTARıN özgün MD4 biçimine geri dönmesi için anahtarı oluşturur. Parola Karması eşitleme Aracısı düz metin parolası hiç erişebilir. Parola Karması eşitleme aracısının MD5 kesinlikle DC ile çoğaltma protokol uyumluluk için kullanılır ve yalnızca şirket içi etki alanı denetleyicisi ve parola karması eşitleme aracısı arasında kullanılır.
4. Parola Karması eşitleme Aracısı'nı, ilk UTF-16 kodlamalı ikili ardından bu dize dönüştürme 32 bayt onaltılık dize, karma geri dönüştürerek 64 bayt ile 16 bayt ikili parola karması genişletir.
5. Parola Karması eşitleme Aracısı ekler bir kullanıcı güvenlik değeri, özgün karma daha iyi korumak için 64-bayt ikili için 10 bayt uzunlukta bir güvenlik değeri oluşan başına.
6. Parola karması eşitleme Aracısı daha sonra MD4 karmasını ve Kullanıcı başına anahtar olarak birleştirir ve [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) işlevine giriş yapın. [HMAC-SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) anahtarlı karma algoritmasının 1000 yinelemesi kullanılır. 
7. Parola Karması eşitleme Aracısı elde edilen 32 bayt karma alır, her ikisini birleştirerek kullanıcı salt ve SHA256 sayısı ona yinelemeler (tarafından kullanılacak Azure AD), ardından iletir Azure AD Connect dizeden Azure AD'ye SSL üzerinden.</br> 
8. Bir kullanıcının Azure AD'de oturum dener ve parolasını girer, parola aynı MD4 + salt + PBKDF2 + HMAC SHA256 işlemi çalıştırılır. Sonuçta elde edilen karma Azure AD'de depolanan karma eşleşirse, kullanıcı doğru parolayı geçtiğini ve doğrulanır.

> [!NOTE]
> Özgün MD4 karma Azure AD'ye aktarılan değil. Bunun yerine, özgün MD4 karma SHA256 karma iletilir. Sonuç olarak, Azure AD'de depolanan karma aldıysanız, bir şirket içi pass--hash saldırısında kullanılamaz.

### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Parola eşitleme yaparken parolanızı düz metin sürümünü Azure AD'ye parola karması eşitleme özelliğini veya tüm ilişkili hizmetlerin gösterilmez.

Kullanıcı kimlik doğrulamasını Azure AD'ye yönelik yerine kuruluşun kendi Active Directory örneğine karşı gerçekleşir. Azure AD'de--depolanan SHA256 parola verileri özgün MD4 karma--karma Active Directory'de depolanan değerinden daha güvenlidir. Ayrıca, bu SHA256 karma şifresi çözülemiyor olduğundan, bu olamaz kuruluşun Active Directory ortamına geri getirildi ve pass--hash saldırısı bir geçerli kullanıcı parolası olarak sunulan.

### <a name="password-policy-considerations"></a>Parola ilke konuları

Parola Karması eşitlemeyi etkinleştirerek, etkilenen parola ilkelerini iki tür vardır:

* Parola karmaşıklığı İlkesi
* Parola süre Dolum İlkesi

#### <a name="password-complexity-policy"></a>Parola karmaşıklığı İlkesi

Parola Karması eşitleme etkinleştirildiğinde, şirket içi Active Directory Örneğinizde parola karmaşıklık ilkeleri eşitlenmiş kullanıcılar için bulutta karmaşıklığı ilkeleri geçersiz kılar. Azure AD Hizmetleri erişmek için şirket içi Active Directory örneğinden tüm geçerli parolaların kullanabilirsiniz.

> [!NOTE]
> Bulutta tanımlandığı gibi doğrudan bulutta oluşturulan sanal kullanıcıların parolalarını yine de parola ilkelerine sahiptir.

#### <a name="password-expiration-policy"></a>Parola süre Dolum İlkesi

Bir kullanıcı parola karması eşitleme kapsamınsa, varsayılan olarak, bulut hesabı parolası *hiç kullanım*dışı olarak ayarlanır.

Şirket içi ortamınızda süresi dolmuş bir eşitlenmiş parola kullanarak bulut hizmetlerinizde oturum açmak devam edebilirsiniz. Bulut parolanızı, parola şirket içi ortamda bir sonraki değiştirdiğinizde güncelleştirilir.

##### <a name="public-preview-of-the-enforcecloudpasswordpolicyforpasswordsyncedusers-feature"></a>*Enforcechoparlör Passwordpolicyforpasswordsyncedusers* özelliğinin genel önizlemesi

Yalnızca Azure AD ile tümleşik hizmetler ile etkileşime geçen eşitlenmiş kullanıcılar varsa ve bir parola süre sonu ilkesiyle uyumlu olması gerekiyorsa, *Enforcechoparlör Passwordpolicyforpasswordsyncedusers* özelliğini etkinleştirerek BUNLARı Azure AD parola süre sonu ilkenize uygun hale getirebilirsiniz.

*Enforcecıpasswordpolicyforpasswordsyncedusers* devre dışı bırakıldığında (varsayılan ayar), Azure AD Connect eşitlenen kullanıcıların passwordpolicies özniteliğini "Disablepasswordexpidıma" olarak ayarlar. Bu, bir kullanıcının parolasının her eşitlenilişinde yapılır ve Azure AD 'yi bu kullanıcı için bulut parolası süre sonu ilkesini yoksayacak şekilde yönlendirir. Aşağıdaki komutla Azure AD PowerShell modülünü kullanarak özniteliğin değerini kontrol edebilirsiniz:

`(Get-AzureADUser -objectID <User Object ID>).passwordpolicies`


Enforcechoparlör Passwordpolicyforpasswordsyncedusers özelliğini etkinleştirmek için aşağıda gösterildiği gibi MSOnline PowerShell modülünü kullanarak aşağıdaki komutu çalıştırın. Enable parametresi için aşağıda gösterildiği gibi Yes yazmanız gerekir:

```
Set-MsolDirSyncFeature -Feature EnforceCloudPasswordPolicyForPasswordSyncedUsers
cmdlet Set-MsolDirSyncFeature at command pipeline position 1
Supply values for the following parameters:
Enable: yes
Confirm
Continue with this operation?
[Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): y
```

Azure AD etkinleştirildikten sonra, `DisablePasswordExpiration` değeri PasswordPolicies özniteliğinden kaldırmak için eşitlenmiş her kullanıcıya gitmez. Bunun yerine, daha sonra şirket içi AD 'de parolasını değiştirdiklerinde, her kullanıcı için bir sonraki parola eşitleme sırasında değer `None` olarak ayarlanır.  

Parola karma eşitlemesi etkinleştirilmeden önce Enforcecizpasswordpolicyforpasswordsyncedusers etkinleştirilmesi önerilir, böylece parola karmalarının ilk eşitlenmesi, kullanıcılar için PasswordPolicies özniteliğine `DisablePasswordExpiration` değerini eklemez.

Varsayılan Azure AD parola ilkesi, kullanıcıların parolalarının her 90 günde bir değiştirilmesini gerektirir. AD 'deki ilkeniz da 90 gün ise, iki ilke eşleşmelidir. Ancak, AD ilkesi 90 gün değilse, Set-MsolPasswordPolicy PowerShell komutunu kullanarak Azure AD parola ilkesini eşleşecek şekilde güncelleştirebilirsiniz.

Azure AD, kayıtlı etki alanı başına ayrı bir parola süre sonu ilkesini destekler.

Desteklenmediği uyarısıyla: Azure AD 'de süresi dolmayan parolalara sahip olması gereken eşitlenmiş hesaplar varsa, `DisablePasswordExpiration` değerini Azure AD 'deki Kullanıcı nesnesinin PasswordPolicies özniteliğine açıkça eklemeniz gerekir.  Bunu, aşağıdaki komutu çalıştırarak yapabilirsiniz.

`Set-AzureADUser -ObjectID <User Object ID> -PasswordPolicies "DisablePasswordExpiration"`

> [!NOTE]
> Bu özellik şu anda genel önizlemede.
> Set-MsolPasswordPolicy PowerShell komutu, Federasyon etki alanlarında çalışmayacaktır. 

#### <a name="public-preview-of-synchronizing-temporary-passwords-and-force-password-change-on-next-logon"></a>Geçici parolaların eşitlenmesinin genel önizlemesi ve "bir sonraki oturumda parola değişikliğini zorla"

Bir kullanıcıyı ilk oturum açma sırasında, özellikle de yönetici parolası sıfırlama oluştuktan sonra parolalarını değiştirmeye zorlamak normaldir.  Genellikle "geçici" bir parola ayarı olarak bilinir ve Active Directory (AD) içindeki bir kullanıcı nesnesi üzerinde "Kullanıcı bir sonraki oturum açışında parolayı değiştirmeli" bayrağıyla tamamlanır.
  
Geçici parola işlevi, kimlik bilgisinin sahipliğinin bu kimlik bilgisi ile ilgili bilgi sahibi olduğu süreyi en aza indirmek için ilk kullanımda, kimlik bilgisinin sahipliğinin aktarılışında emin olmaya yardımcı olur.

Azure AD 'de eşitlenen kullanıcılar için geçici parolaları desteklemek üzere Azure AD Connect sunucunuzda aşağıdaki komutu çalıştırarak *Forcepasswordchangeonlogon* özelliğini etkinleştirebilirsiniz:

`Set-ADSyncAADCompanyFeature  -ForcePasswordChangeOnLogOn $true`

> [!NOTE]
> Bir kullanıcının bir sonraki oturum açışında parolasını değiştirmesine zorlamak için aynı anda bir parola değişikliği yapılması gerekir.  Azure AD Connect, parolayı zorla değiştirme bayrağını kendisi seçmeyecektir; Parola karması eşitleme sırasında oluşan algılanan parola değişikliğine ek olarak belirlenir.

> [!CAUTION]
> Bu özelliği yalnızca, kiracı üzerinde SSPR ve parola geri yazma etkinleştirildiğinde kullanmanız gerekir.  Bu, bir Kullanıcı parolasını SSPR ile değiştirirse, Active Directory eşitlenecek.

> [!NOTE]
> Bu özellik şu anda genel önizlemede.

#### <a name="account-expiration"></a>Hesap süresi

Kuruluşunuzun kullanıcı hesabı Yönetimi işleminin bir parçası olarak accountExpires öznitelik kullanıyorsa, bu öznitelik Azure AD'ye eşitlenmez. Sonuç olarak, süresi dolmuş bir Active Directory hesabı için parola karması eşitlemeyi yapılandırılmış bir ortamda hala Azure AD'de etkin olacaktır. Hesabın kullanım zamanı dolmuşsa, bir iş akışı eyleminin kullanıcının Azure AD hesabını devre dışı bırakan bir PowerShell betiğini tetiklemesi gerektiğini öneririz ( [set-AzureADUser](https://docs.microsoft.com/powershell/module/azuread/set-azureaduser?view=azureadps-2.0) cmdlet 'ini kullanın). Hesap açık olduğunda, buna karşılık, Azure AD örneği açık olması.

### <a name="overwrite-synchronized-passwords"></a>Eşitlenmiş parolalar üzerine yaz

Bir yönetici, el ile Windows PowerShell kullanarak parolanızı sıfırlayabilirsiniz.

Bu durumda, yeni parola eşitlenmiş parolanızı geçersiz kılar ve bulutta tanımlanan tüm parola ilkelerini yeni parola uygulanır.

Şirket içi parolanızı değiştirirseniz yeniden, yeni parola, buluta eşitlenebilir ve el ile güncelleştirilmiş parolayı geçersiz kılar.

Parola Eşitleme, oturum açmış kullanıcının Azure üzerinde hiçbir etkisi yoktur. Geçerli bulut hizmeti oturumunuzu, bir bulut hizmetine oturumunuz sırasında oluşan eşitlenmiş parola değişikliği hemen etkilenmez. KMSI'yi bu fark süresini uzatır. Bulut hizmetine yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı vermeniz gerekir.

### <a name="additional-advantages"></a>Ek avantajlar

- Genellikle, parola karması eşitleme bir Federasyon Hizmeti uygulamak daha kolaydır. Diğer sunucular gerektirmez ve kullanıcıların kimliğini doğrulamak için bir yüksek oranda kullanılabilir bir Federasyon Hizmeti bağımlılığa ortadan kaldırır.
- Parola Karması eşitleme, Federasyon yanı sıra da etkinleştirilebilir. Federasyon hizmetinize bir kesinti oluşursa bir geri dönüş olarak kullanılabilir.

## <a name="password-hash-sync-process-for-azure-ad-domain-services"></a>Azure AD Domain Services için Parola karması eşitleme işlemi

Kerberos, LDAP veya NTLM kullanması gereken uygulamalar ve hizmetler için eski kimlik doğrulama sağlamak üzere Azure AD Domain Services kullanırsanız, bazı ek süreçler Parola karması eşitleme akışının bir parçasıdır. Azure AD Connect, Azure AD Domain Services kullanım için parola karmalarını Azure AD ile eşitlemeye yönelik aşağıdaki ek işlemleri kullanır:

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamları ile eşitleme için yüklenmeli ve yapılandırılmalıdır. Nesneleri Azure AD 'ye geri eşitlemeniz için Azure AD DS tarafından yönetilen bir etki alanına Azure AD Connect yüklemek desteklenmez.
>
> Azure AD Connect yalnızca Azure AD kiracınız için Azure AD DS etkinleştirdiğinizde eski parola karmalarını eşitler. Yalnızca Azure AD ile şirket içi AD DS ortamı eşitlemesini Azure AD Connect kullanıyorsanız aşağıdaki adımlar kullanılmaz.
>
> Eski uygulamalarınız NTLM kimlik doğrulaması veya LDAP basit bağlamalar kullanmıyorsanız, Azure AD DS için NTLM parola karma eşitlemesini devre dışı bırakmanızı öneririz. Daha fazla bilgi için bkz. [zayıf şifre paketlerini ve NTLM kimlik bilgisi karma eşitlemesini devre dışı bırakma](../../active-directory-domain-services/secure-your-domain.md).

1. Azure AD Connect kiracının Azure AD Domain Services örneğinin ortak anahtarını alır.
1. Kullanıcı parolasını değiştirdiğinde, şirket içi etki alanı denetleyicisi parola değişikliğinin (karma) sonucunu iki özniteliğe depolar:
    * NTLM parola karması için *unicodePwd* .
    * , Kerberos Parola karması için *kimlik bilgileri* .
1. Azure AD Connect, Dizin çoğaltma kanalı aracılığıyla parola değişikliklerini algılar (öznitelik değişiklikleri diğer etki alanı denetleyicilerine çoğaltılmaya gerek yoktur).
1. Parolası değişmiş olan her kullanıcı için, Azure AD Connect aşağıdaki adımları gerçekleştirir:
    * Rastgele bir AES 256 bit simetrik anahtar üretir.
    * Şifrelemenin ilk yuvarlaklaştırmak için gereken rastgele bir başlatma vektörü üretir.
    * , *Mentalcredentials* özniteliklerinden Kerberos parola karmalarını ayıklar.
    * Azure AD Domain Services Güvenlik Yapılandırması *Syncntlmpasswords* ayarını denetler.
        * Bu ayar devre dışı bırakılırsa, rastgele, yüksek entropi bir NTLM karması (kullanıcının parolasından farklı) oluşturur. Bu karma daha sonra, exacted Kerberos parola karmalarıyla birlikte, *Mentalcrendetials* özniteliğinden bir veri yapısına birleştirilir.
        * Etkinleştirilirse, *unicodePwd* özniteliğinin değerini, *dtalcredentials* özniteliğinden ayıklanan Kerberos parola karmalarıyla tek bir veri yapısına birleştirir.
    * AES Simetrik anahtarını kullanarak tek veri yapısını şifreler.
    * Kiracının Azure AD Domain Services ortak anahtarını kullanarak AES Simetrik anahtarını şifreler.
1. Azure AD Connect şifreli AES Simetrik anahtarını, parola karmalarını içeren şifreli veri yapısını ve Azure AD 'ye başlatma vektörünü iletir.
1. Azure AD şifreli AES Simetrik anahtarını, şifreli veri yapısını ve Kullanıcı için başlatma vektörünü depolar.
1. Azure AD, şifreli AES Simetrik anahtarını, şifrelenmiş veri yapısını ve Azure AD Domain Services için şifrelenmiş bir HTTP oturumunda iç eşitleme mekanizması kullanarak başlatma vektörünü gönderir.
1. Azure AD Domain Services kiracının Azure Anahtar Kasası 'na ait örneğinin özel anahtarını alır.
1. Her şifreli veri kümesi (tek bir kullanıcının parola değişikliğini temsil eden) için Azure AD Domain Services aşağıdaki adımları gerçekleştirir:
    * , AES Simetrik anahtarının şifresini çözmek için özel anahtarını kullanır.
    * Parola karmalarını içeren şifreli veri yapısının şifresini çözmek için başlatma vektörü ile AES Simetrik anahtarını kullanır.
    * Aldığı Kerberos parola karmalarını Azure AD Domain Services etki alanı denetleyicisine yazar. Karmalar, Azure AD Domain Services etki alanı denetleyicisinin ortak anahtarına şifrelenmiş Kullanıcı nesnesinin *Mentalcredentials* özniteliğine kaydedilir.
    * Azure AD Domain Services, Azure AD Domain Services etki alanı denetleyicisine aldığı NTLM parola karmasını yazar. Karma, Azure AD Domain Services etki alanı denetleyicisinin ortak anahtarına şifrelenmiş Kullanıcı nesnesinin *unicodePwd* özniteliğine kaydedilir.

## <a name="enable-password-hash-synchronization"></a>Parola karması eşitlemeyi etkinleştirme

>[!IMPORTANT]
>AD FS (veya başka bir Federasyon teknolojileri) ile parola karması eşitlemeye geçiş yapıyorsanız, [burada](https://aka.ms/adfstophsdpdownload)yayımlanan ayrıntılı dağıtım kılavuzumuzu izlemenizi kesinlikle öneririz.

Azure AD Connect **Express ayarları** seçeneğini kullanarak yüklediğinizde, Parola karması eşitleme otomatik olarak etkinleştirilir. Daha fazla bilgi için bkz. [Express ayarlarını kullanarak Azure AD Connect](how-to-connect-install-express.md)kullanmaya başlama.

Azure AD Connect yükleme sırasında özel ayarları kullanırsanız, parola karması eşitleme, kullanıcı oturum açma sayfasında kullanılabilir. Daha fazla bilgi için bkz. [özel Azure AD Connect yüklemesi](how-to-connect-install-custom.md).

![Parola karmasını eşitlemeyi etkinleştirme](./media/how-to-connect-password-hash-synchronization/usersignin2.png)

### <a name="password-hash-synchronization-and-fips"></a>Parola karma eşitlemesi ve FIPS
Sunucunuz Federal Bilgi İşleme Standardı (FIPS göre) kilitli, MD5 devre dışı bırakıldı.

**Parola karması eşitlemesi için MD5 etkinleştirmek üzere aşağıdaki adımları uygulayın:**

1. İçin %ProgramFiles%\Azure AD Sync\Bin gidin.
2. Miiserver.exe.config açın.
3. Dosyanın sonunda configuration/çalışma zamanı düğümüne gidin.
4. Şu düğümü ekleyin: `<enforceFIPSPolicy enabled="false"/>`
5. Yaptığınız değişiklikleri kaydedin.

Başvuru için bu kod parçacığı, ne gibi görünmelidir gösterilmiştir:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Güvenlik ve FIPS hakkında daha fazla bilgi için bkz. [Azure AD Parola karması eşitleme, şifreleme ve FIPS uyumluluğu](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-hash-synchronization"></a>Parola Karması eşitleme sorunlarını giderme
Parola karması eşitlemeyle ilgili sorun yaşıyorsanız, bkz. [Parola karması eşitleme sorunlarını giderme](tshoot-connect-password-hash-synchronization.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect eşitleme: eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [ADFS 'den Parola karması eşitlemesine geçiş için adım adım bir dağıtım planı alın](https://aka.ms/authenticationDeploymentPlan)
