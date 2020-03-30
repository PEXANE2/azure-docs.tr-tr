---
title: Azure AD Connect eşitlemeile parola karma senkronizasyonu uygulayın | Microsoft Dokümanlar
description: Parola karma eşitlemenin nasıl çalıştığı ve nasıl ayarlanış yaptığı hakkında bilgi sağlar.
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
ms.openlocfilehash: c41b11ab65f5710d338ce0041579e1eb4678ec42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331362"
---
# <a name="implement-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama
Bu makalede, kullanıcı parolalarınızı şirket içi Active Directory örneğinden bulut tabanlı azure Etkin Dizin (Azure AD) örneğine eşitlemeniz gereken bilgiler sağlanır.

## <a name="how-password-hash-synchronization-works"></a>Parola karması eşitleme nasıl çalışır?
Active Directory etki alanı hizmeti, parolaları gerçek kullanıcı parolasının karma değer gösterimi biçiminde depolar. Karma değer, tek yönlü matematiksel fonksiyonun *(karma algoritma)* bir sonucudur. Tek yönlü işlevin sonucunu, parolanın düz metin sürümüne döndürmek mümkün değildir. 

Parolanızı eşitlemek için Azure AD Connect eşitleme, parola karmanızı şirket içi Active Directory örneğinden ayıklar. Azure Active Directory kimlik doğrulama hizmetine eşitlenmeden önce parola karmasına ekstra güvenlik işlemeuygulanır. Parolalar kullanıcı başına ve kronolojik sırada senkronize edilir.

Parola karma eşitleme işleminin gerçek veri akışı, kullanıcı verilerinin eşitlenmesine benzer. Ancak, parolalar diğer öznitelikler için standart dizin eşitleme penceresinden daha sık eşitlenir. Parola karma eşitleme işlemi her 2 dakikada bir çalışır. Bu işlemin sıklığını değiştiremezsiniz. Parolayı eşitlediğinizde, varolan bulut parolasının üzerine yazar.

Parola karma eşitleme özelliğini ilk kez etkinleştirdiğinizde, kapsam içerisindeki tüm kullanıcıların parolalarının ilk eşitlemesini gerçekleştirir. Eşitlemek istediğiniz kullanıcı parolalarının bir alt kümesini açıkça tanımlayamazsınız. Ancak, birden çok bağlayıcı varsa, bazı bağlayıcılar için parola karma eşitlemedevre dışı bırakmak mümkündür, ancak [diğerlerini Set-ADSyncAADSyncSyncConfiguration](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant) cmdlet kullanarak.

Şirket içi parolayı değiştirdiğinizde, güncelleştirilmiş parola genellikle birkaç dakika içinde eşitlenir.
Parola karma eşitleme özelliği otomatik olarak başarısız eşitleme denemelerini yeniden dener. Parola eşitleme girişimi sırasında bir hata oluşursa, olay görüntüleyicisinizde bir hata günlüğe kaydedilir.

Parolanın eşitlenmesinin, şu anda oturum açmış olan kullanıcı üzerinde hiçbir etkisi yoktur.
Geçerli bulut hizmeti oturumunuz, siz oturum açmışken bir bulut hizmetinde gerçekleşen eşitlenmiş parola değişikliğinden hemen etkilenmez. Ancak, bulut hizmeti yeniden kimlik doğrulamanızı gerektiriyorsa, yeni parolanızı sağlamanız gerekir.

Bir kullanıcı, kurumsal ağında oturum açıp imzalamadığına bakılmaksızın Azure AD'ye kimlik doğrulaması yapmak için ikinci kez şirket kimlik bilgilerini girmelidir. Ancak, kullanıcı oturum açma sırasında beni oturum aç (KMSI) onay kutusunu seçerse, bu desen en aza indirilebilir. Bu seçim, kimlik doğrulamayı 180 gün boyunca atlayan bir oturum çerezi ayarlar. KMSI davranışı Azure AD yöneticisi tarafından etkinleştirilebilir veya devre dışı bırakılmış olabilir. Ayrıca, kullanıcıların kurumsal ağınıza bağlı kurumsal cihazlarında olduklarında otomatik olarak imzalayan [Seamless SSO'yı](how-to-connect-sso.md)açarak parola istemlerini azaltabilirsiniz.

> [!NOTE]
> Parola eşitleme yalnızca Active Directory'deki nesne türü kullanıcısı için desteklenir. iNetOrgPerson nesne türü için desteklenmez.

### <a name="detailed-description-of-how-password-hash-synchronization-works"></a>Parola karma senkronizasyonunun nasıl çalıştığının ayrıntılı açıklaması

Aşağıdaki bölümde, Active Directory ve Azure AD arasında parola karma eşitlemenin nasıl çalıştığı ayrıntılı olarak açıklanmaktadır.

![Ayrıntılı parola akışı](./media/how-to-connect-password-hash-synchronization/arch3b.png)

1. Her iki dakikada bir, AD Connect sunucusundaki parola karma eşitleme aracısı, bir DC'den depolanmış parola karmalarını (unicodePwd özniteliği) ister.  Bu istek, DC'ler arasında veri eşitlemek için kullanılan standart [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) çoğaltma protokolü aracılığıyla yapılır. Hizmet hesabı, parola çözümlerini elde etmek için Tüm AD izinlerini (yüklemede varsayılan olarak verilen) Çoğaltma Dizin Değişikliklerine sahip olmalıdır.
2. Göndermeden önce DC, RPC oturum anahtarının [MD5](https://www.rfc-editor.org/rfc/rfc1321.txt) karma anahtarı ve tuz olan bir anahtar kullanarak MD4 şifre sini şifreler. Daha sonra rpc üzerinden şifre karma eşitleme aracısı sonucu gönderir. DC ayrıca DC çoğaltma protokolünü kullanarak tuzu senkronizasyon aracısına aktarıyor, böylece ajan zarfın şifresini çözebilecek.
3. Şifre karma senkronizasyon aracısı şifreli zarfı bulduktan sonra, alınan verilerin orijinal MD4 biçimine geri dönmek için bir anahtar oluşturmak için [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) ve tuzu kullanır. Parola karma eşitleme aracısı hiçbir zaman açık metin parolasına erişememiştir. Parola karma eşitleme aracısının MD5 kullanımı kesinlikle DC ile çoğaltma protokolü uyumluluğu içindir ve yalnızca DC ile parola karma eşitleme aracısı arasındaki yerinde kullanılır.
4. Parola karma eşitleme aracısı, önce karma32 bayt hexadecimal dize dönüştürerek, daha sonra UTF-16 kodlama ile ikili içine bu dize dönüştürerek 64 bayt için 16 bayt ikili şifre karma genişletir.
5. Parola karma senkronizasyon aracısı, orijinal karmayı daha fazla korumak için 64 baytlık ikiliye 10 bayt uzunluğunda tuzdan oluşan kullanıcı başına bir tuz ekler.
6. Parola karma eşitleme aracısı daha sonra MD4 karma artı kullanıcı başına tuz birleştirir ve [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) işlevine girer. [HMAC-SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) anahtarlı karma algoritmasının 1000 yinelemesi kullanılır. 
7. Parola karma eşitleme aracısı, elde edilen 32 baytlık karmayı alır, hem kullanıcı başına salt hem de SHA256 yineleme sayısını (Azure AD tarafından kullanılmak üzere) birleştirir ve string'i Azure AD Connect'ten Azure AD'ye TLS üzerinden iletir.</br> 
8. Bir kullanıcı Azure AD'de oturum açmaya çalıştığında ve parolasını girdiğinde, parola aynı MD4+salt+PBKDF2+HMAC-SHA256 işleminden çalıştırılır. Ortaya çıkan karma, Azure AD'de depolanan karmayla eşleşirse, kullanıcı doğru parolayı girmiş ve kimliği doğrulanmıştır.

> [!NOTE]
> Orijinal MD4 karmaSı Azure AD'ye aktarılmez. Bunun yerine, orijinal MD4 karma SHA256 karma iletilir. Sonuç olarak, Azure AD'de depolanan karma elde edilirse, şirket içi geçici bir karma saldırıda kullanılamaz.

### <a name="security-considerations"></a>Güvenlik konuları

Parolaları eşitlerken, parolanızın düz metin sürümü parola karma eşitleme özelliğine, Azure AD'ye veya ilişkili hizmetlerden herhangi biriyle açıklanmaz.

Kullanıcı kimlik doğrulaması, kuruluşun kendi Etkin Dizin örneğine karşı değil, Azure AD'ye karşı gerçekleşir. Orijinal MD4 karmasının bir karması olan Azure AD'de depolanan SHA256 parola verileri, Active Directory'de depolanandan daha güvenlidir. Ayrıca, bu SHA256 karmasının şifresi çözülemediğinden, kuruluşun Active Directory ortamına geri getirilemez ve karma bir geçiş saldırısında geçerli bir kullanıcı parolası olarak sunulamaz.

### <a name="password-policy-considerations"></a>Parola ilkesi yle ilgili hususlar

Parola karma eşitlemesini etkinleştirmeden etkilenen iki tür parola ilkesi vardır:

* Parola karmaşıklığı ilkesi
* Parola sona erme ilkesi

#### <a name="password-complexity-policy"></a>Parola karmaşıklığı ilkesi

Parola karma eşitleme etkinleştirildiğinde, şirket içi Active Directory örneğinizdeki parola karmaşıklığı ilkeleri, eşitlenmiş kullanıcılar için buluttaki karmaşıklık ilkelerini geçersiz kılar. Azure AD hizmetlerine erişmek için şirket içi Active Directory örneğinizin tüm geçerli parolalarını kullanabilirsiniz.

> [!NOTE]
> Doğrudan bulutta oluşturulan kullanıcıların parolaları, bulutta tanımlandığı şekilde parola ilkelerine tabi olmaya devam eder.

#### <a name="password-expiration-policy"></a>Parola sona erme ilkesi

Bir kullanıcı parola karma senkronizasyonu kapsamındaysa, varsayılan olarak bulut hesabı parolası *asla sona ermeyecek*şekilde ayarlanır.

Şirket içi ortamınızda süresi dolmuş eşitlenmiş bir parola kullanarak bulut hizmetlerinde oturum açmaya devam edebilirsiniz. Bulut parolanız, şirket içi ortamda parolayı bir sonraki kez değiştirişinize göre güncelleştirilir.

##### <a name="public-preview-of-the-enforcecloudpasswordpolicyforpasswordsyncedusers-feature"></a>*EnforceCloudPasswordPolicyForPasswordSyncedUsers* özelliğinin genel önizlemesi

Yalnızca Azure AD tümleşik hizmetleriyle etkileşimde bulunan ve parola son kullanma ilkesine uyması gereken senkronize kullanıcılar varsa, *Bunları EnforceCloudPasswordPolicyForPasswordSyncedUsers* özelliğini etkinleştirerek Azure AD parola son kullanma politikanıza uymaya zorlayabilirsiniz.

*EnforceCloudPasswordPolicyForPasswordSyncedUsers* devre dışı bırakıldığında (varsayılan ayar budur), Azure AD Connect senkronize edilmiş kullanıcıların Parola İlkeleri özniteliğini "Devre Dışı BırakmaParola Süresi" olarak ayarlar. Bu, bir kullanıcının parolası her eşitleninde yapılır ve Azure AD'ye bu kullanıcının bulut parola son kullanma ilkesini yoksaymasını bildirir. Azure AD PowerShell modüllerini kullanarak özniteliğin değerini aşağıdaki komutla denetleyebilirsiniz:

`(Get-AzureADUser -objectID <User Object ID>).passwordpolicies`


EnforceCloudPasswordPolicyForPasswordSyncedUsers özelliğini etkinleştirmek için, aşağıda gösterildiği gibi MSOnline PowerShell modüllerini kullanarak aşağıdaki komutu çalıştırın. Aşağıda gösterildiği gibi Etkinleştir parametresi için evet yazmanız gerekir:

```
Set-MsolDirSyncFeature -Feature EnforceCloudPasswordPolicyForPasswordSyncedUsers
cmdlet Set-MsolDirSyncFeature at command pipeline position 1
Supply values for the following parameters:
Enable: yes
Confirm
Continue with this operation?
[Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): y
```

Azure AD etkinleştirildikten sonra, `DisablePasswordExpiration` parola ilkeleri özniteliğinden değeri kaldırmak için senkronize edilmiş her kullanıcıya gitmez. Bunun yerine, değer, `None` her kullanıcı için bir sonraki parola eşitleme sırasında şirket içi AD'de parolalarını değiştirdiğinde ayarlanır.  

Parola karma eşitlemesini etkinleştirmeden önce, parola karma eşitlemelerinin kullanıcılar için Parola Politikaları özniteliğine `DisablePasswordExpiration` değer katmayabilmesi için, EnforceCloudPasswordPolicyForPasswordSyncedUsers'ı etkinleştirmeniz önerilir.

Varsayılan Azure AD parola ilkesi, kullanıcıların parolalarını her 90 günde bir değiştirmelerini gerektirir. AD'deki politikanız da 90 günse, iki ilke eşleşecektir. Ancak, REKLAM ilkesi 90 gün değilse, Set-MsolPasswordPolicy PowerShell komutunu kullanarak Azure AD parola ilkesini eşleşecek şekilde güncelleştirebilirsiniz.

Azure AD, kayıtlı etki alanı başına ayrı bir parola son kullanma ilkesini destekler.

Uyarı: Azure AD'de süresi dolmayan parolalara sahip olması gereken eşitlenmiş hesaplar varsa, `DisablePasswordExpiration` Azure AD'deki kullanıcı nesnesinin Parola İlkeleri özelliğine açıkça değer eklemeniz gerekir.  Bunu aşağıdaki komutu çalıştırarak yapabilirsiniz.

`Set-AzureADUser -ObjectID <User Object ID> -PasswordPolicies "DisablePasswordExpiration"`

> [!NOTE]
> Bu özellik şu anda Genel Önizleme'de yer alan bir özelliktir.
> Set-MsolPasswordPolicy PowerShell komutu federe etki alanlarında çalışmaz. 

#### <a name="public-preview-of-synchronizing-temporary-passwords-and-force-password-change-on-next-logon"></a>Geçici parolaları eşitleme nin genel önizlemesi ve "Sonraki Oturumaçmada Parola Değişikliğini Zorlama"

Bir kullanıcıyı, özellikle yönetici parola sıfırlama oluştuktan sonra, ilk oturum açma sırasında parolasını değiştirmeye zorlamak tipiktir.  Genellikle "geçici" parola ayarlama olarak bilinir ve Active Directory (AD) bir kullanıcı nesnesi üzerinde "Kullanıcı sonraki oturumaçmada parolayı değiştirmeli" bayrağı işaretleyerek tamamlanır.
  
Geçici parola işlevi, birden fazla kişinin bu kimlik bilgisine sahip olduğu süreyi en aza indirmek için kimlik bilgisinin sahipliğinin ilk kullanımda tamamlanmasını sağlamaya yardımcı olur.

Senkronize kullanıcılar için Azure AD'deki geçici parolaları desteklemek için, Azure AD Connect sunucunuzda aşağıdaki komutu çalıştırarak *ForcePasswordChangeOnLogOn* özelliğini etkinleştirebilirsiniz:

`Set-ADSyncAADCompanyFeature  -ForcePasswordChangeOnLogOn $true`

> [!NOTE]
> Bir kullanıcıyı bir sonraki oturum açmada parolasını değiştirmeye zorlamak, aynı anda parola değişikliği gerektirir.  Azure AD Connect, kuvvet parola değişikliği bayrağını tek başına almaz; parola karma eşitleme sırasında oluşan algılanan parola değişikliğine ektir.

> [!CAUTION]
> Bu özelliği yalnızca kiracıda SSPR ve Password Writeback etkinleştirildiğinde kullanmalısınız.  Bu, bir kullanıcının Parolasını SSPR aracılığıyla değiştirmesi durumunda Active Directory ile senkronize edilmesidir.

> [!NOTE]
> Bu özellik şu anda genel önizlemede.

#### <a name="account-expiration"></a>Hesap sona ermesi

Kuruluşunuz hesap Süresi özniteliğini kullanıcı hesabı yönetiminin bir parçası olarak kullanıyorsa, bu öznitelik Azure AD ile eşitlenmez. Sonuç olarak, parola karma eşitleme için yapılandırılan bir ortamda süresi dolmuş bir Active Directory hesabı Azure AD'de etkin olmaya devam eder. Hesabın süresi dolmuşsa, bir iş akışı eyleminin kullanıcının Azure AD hesabını devre dışı bırakan bir PowerShell komut dosyası tetiklemesi öneririz [(Set-AzureADUser](https://docs.microsoft.com/powershell/module/azuread/set-azureaduser?view=azureadps-2.0) cmdlet'ini kullanın). Tam tersine, hesap açık olduğunda, Azure REKLAM örneğinin açık olması gerekir.

### <a name="overwrite-synchronized-passwords"></a>Eşitlenmiş parolaların üzerine yazma

Bir yönetici, Windows PowerShell'i kullanarak parolanızı el ile sıfırlayabilir.

Bu durumda, yeni parola eşitlenmiş parolanızı geçersiz kılar ve bulutta tanımlanan tüm parola ilkeleri yeni parolaya uygulanır.

Şirket içi parolanızı yeniden değiştirirseniz, yeni parola buluta eşitlenir ve el ile güncelleştirilen parolayı geçersiz kılar.

Parolanın eşitlenmesinin oturum açmış olan Azure kullanıcısı üzerinde hiçbir etkisi yoktur. Geçerli bulut hizmeti oturumunuz, siz bir bulut hizmetinde oturum açtığınızda gerçekleşen eşitlenmiş parola değişikliğinden hemen etkilenmez. KMSI bu farkın süresini uzatür. Bulut hizmeti yeniden kimlik doğrulamanızı gerektiriyorsa, yeni parolanızı sağlamanız gerekir.

### <a name="additional-advantages"></a>Ek avantajlar

- Genellikle, parola karma eşitleme bir federasyon hizmeti daha uygulamak için basittir. Herhangi bir ek sunucu gerektirmez ve kullanıcıların kimliğini doğrulamak için yüksek kullanılabilirlikte bir federasyon hizmetine olan bağımlılığı ortadan kaldırır.
- Parola karma eşitleme de federasyona ek olarak etkinleştirilebilir. Federasyon hizmetiniz bir kesinti yle karşılanırsa geri dönüş olarak kullanılabilir.

## <a name="password-hash-sync-process-for-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri için parola karma eşitleme işlemi

Kerberos, LDAP veya NTLM kullanması gereken uygulamalar ve hizmetler için eski kimlik doğrulaması sağlamak için Azure AD Etki Alanı Hizmetlerini kullanıyorsanız, bazı ek işlemler parola karma eşitleme akışının bir parçasıdır. Azure AD Connect, parola açıklarını Azure AD Etki Alanı Hizmetlerinde kullanılmak üzere Azure AD ile eşitlemek için aşağıdaki ek işlemi kullanır:

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamlarıyla senkronizasyon için yüklenmeli ve yapılandırılmalıdır. Azure AD Connect'i Azure AD DS yönetilen bir etki alanına yüklemek için desteklenmez.
>
> Azure AD Connect, eski parola bağlantı larını yalnızca Azure AD kiracınız için Azure AD DS'yi etkinleştirdiğinizde eşitler. Azure AD Connect'i yalnızca şirket içi BIR AD DS ortamını Azure AD ile eşitlemek için kullanıyorsanız, aşağıdaki adımlar kullanılmaz.
>
> Eski uygulamalarınız NTLM kimlik doğrulaması veya LDAP basit bağlamaları kullanmıyorsa, Azure AD DS için NTLM şifre karma eşitlemesi devre dışı bırakmanızı öneririz. Daha fazla bilgi için zayıf [şifreleme paketlerini ve NTLM kimlik bilgisi karma senkronizasyonunu devre dışı sayıl.](../../active-directory-domain-services/secure-your-domain.md)

1. Azure AD Connect, kiracının Azure AD Etki Alanı Hizmetleri örneği için ortak anahtarı alır.
1. Bir kullanıcı parolasını değiştirdiğinde, şirket içi etki alanı denetleyicisi parola değişikliğinin (iş ortaklarını) sonucunu iki öznitelikte saklar:
    * ntlm şifre karma için *unicodePwd.*
    * Kerberos şifre karma için *ek Kimlik Bilgileri.*
1. Azure AD Connect, dizin çoğaltma kanalı (diğer etki alanı denetleyicilerine çoğaltılması gereken öznitelik değişiklikleri) aracılığıyla parola değişikliklerini algılar.
1. Parolası değiştirilen her kullanıcı için Azure AD Connect aşağıdaki adımları gerçekleştirir:
    * Rastgele bir AES 256-bit simetrik anahtar üretir.
    * Şifrelemenin ilk turu için gereken rasgele bir başlatma vektörü oluşturur.
    * Ek *Kimlik Bilgileri* özniteliklerinden Kerberos parola sıcalarını ayıklar.
    * Azure AD Etki Alanı Hizmetleri güvenlik yapılandırması *SyncNtlmPasswords* ayarını denetler.
        * Bu ayar devre dışı bırakılırsa, rasgele, yüksek entropi NTLM karma (kullanıcının parolasından farklı) oluşturur. Bu karma daha sonra bir veri yapısı içine *ekCrendetials* öznitelik tam Kerberos şifre karmaları ile birleştirilir.
        * Etkinse, *tek kodluPwd* özniteliğinin *değerini, ek Kimlik Bilgileri* özniteliğinden çıkarılan Kerberos parola sıyrıklarıyla tek bir veri yapısında birleştirir.
    * AES simetrik anahtarı kullanarak tek veri yapısını şifreler.
    * Kiracının Azure AD Etki Alanı Hizmetleri ortak anahtarını kullanarak AES simetrik anahtarını şifreler.
1. Azure AD Connect, şifrelenmiş AES simetrik anahtarını, parola kalıplarını içeren şifreli veri yapısını ve başlatma vektörünü Azure AD'ye aktarır.
1. Azure AD, kullanıcı için şifreli AES simetrik anahtarını, şifrelenmiş veri yapısını ve başlatma vektörü depolar.
1. Azure AD, şifreli AES simetrik anahtarını, şifreli veri yapısını ve başlatma vektörü, şifreli bir HTTP oturumu üzerinden dahili bir eşitleme mekanizmasını kullanarak Azure AD Etki Alanı Hizmetleri'ne iter.
1. Azure AD Etki Alanı Hizmetleri, kiracının özel anahtarını Azure Key kasasından alır.
1. Her şifrelenmiş veri kümesi (tek bir kullanıcının parola değişikliğini temsil eden) için Azure AD Etki Alanı Hizmetleri aşağıdaki adımları gerçekleştirir:
    * AES simetrik anahtarının şifresini çözmek için özel anahtarını kullanır.
    * Parola hasırlarını içeren şifreli veri yapısının şifresini çözmek için başlatma vektörüyle AES simetrik anahtarını kullanır.
    * Aldığı Kerberos parolasını Azure AD Etki Alanı Hizmetleri etki alanı denetleyicisine yazar. Bu hasonlar, kullanıcı nesnesinin Azure AD Etki Alanı Hizmetleri etki alanı denetleyicisinin ortak anahtarına şifrelenmiş *tamamlayıcı Kimlik Bilgileri* özniteliğine kaydedilir.
    * Azure AD Etki Alanı Hizmetleri, aldığı NTLM parolasını Azure AD Etki Alanı Hizmetleri etki alanı denetleyicisine yazar. Karma, kullanıcı nesnesinin Azure AD Etki Alanı Hizmetleri etki alanı denetleyicisinin ortak anahtarına şifrelenmiş *tek kodluPwd* özelliğine kaydedilir.

## <a name="enable-password-hash-synchronization"></a>Parola karması eşitlemeyi etkinleştirme

>[!IMPORTANT]
>AD FS'den (veya diğer federasyon teknolojilerinden) Parola Karma Senkronizasyonu'na geçiş yapıyorsunuz, [burada](https://aka.ms/adfstophsdpdownload)yayınlanan ayrıntılı dağıtım kılavuzumuzu izlemenizi öneririz.

Azure AD Connect'i **Express Ayarlar** seçeneğini kullanarak yüklediğinizde, parola karma eşitlemesi otomatik olarak etkinleştirilir. Daha fazla bilgi için ekspres [ayarları kullanarak Azure AD Connect ile başlarken](how-to-connect-install-express.md)bakın.

Azure AD Connect'i yüklerken özel ayarlar kullanıyorsanız, kullanıcı oturum açma sayfasında parola karma eşitleme kullanılabilir. Daha fazla bilgi için Azure [AD Connect'in Özel yüklemesi'ne](how-to-connect-install-custom.md)bakın.

![Parola karmasını eşitlemeyi etkinleştirme](./media/how-to-connect-password-hash-synchronization/usersignin2.png)

### <a name="password-hash-synchronization-and-fips"></a>Şifre karma senkronizasyonu ve FIPS
Sunucunuz Federal Bilgi İşlem Standardına (FIPS) göre kilitlendiyse, MD5 devre dışı bırakılır.

**Parola karma eşitleme için MD5'i etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. %program dosyaları%\Azure AD Eşitleme\Bin'e gidin.
2. Açık miiserver.exe.config.
3. Dosyanın sonundaki yapılandırma/çalışma zamanı düğümüne gidin.
4. Aşağıdaki düğüm ekleyin:`<enforceFIPSPolicy enabled="false"/>`
5. Yaptığınız değişiklikleri kaydedin.

Başvuru için, bu parçacık gibi görünmesi gereken şeydir:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Güvenlik ve FIPS hakkında daha fazla bilgi için [Azure AD parolası karma eşitleme, şifreleme ve FIPS uyumluluğu'na](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)bakın.

## <a name="troubleshoot-password-hash-synchronization"></a>Sorun giderme parola karma senkronizasyonu
Parola karma eşitlemeyle ilgili sorunlarınız varsa, [sorun giderme parola karma senkronizasyonu'na](tshoot-connect-password-hash-synchronization.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [ADFS'den Parola Hash Senkronizasyonuna geçiş için adım adım dağıtım planı alın](https://aka.ms/authenticationDeploymentPlan)
