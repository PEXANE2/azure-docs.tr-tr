---
title: Şirket içi Azure AD Parola Koruması Sorun Giderme
description: Şirket içi Aktif Dizin Etki Alanı Hizmetleri ortamı için Azure AD Parola Koruması sorununa nasıl giderilen öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79ebf543a3880a4f2c8ee8c0d706c268ef3f08d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263653"
---
# <a name="troubleshoot-on-premises-azure-ad-password-protection"></a>Sorun Giderme: Şirket içi Azure AD Parola Koruması

Azure AD Parola Koruması'nın dağıtımından sonra sorun giderme gerekebilir. Bu makale, bazı sık karşılaşılan sorun giderme adımlarını anlamanıza yardımcı olmak için ayrıntılara girer.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>DC aracısı dizinde bir proxy bulamıyor

Bu sorunun ana belirtisi DC aracı sı admin olay günlüğünde 30017 olayları.

Bu sorunun olağan nedeni, bir proxy henüz kaydedilmemiş olmasıdır. Bir proxy kaydedilmişse, belirli bir DC aracısı bu proxy'yi görene kadar AD çoğaltma gecikmesi nedeniyle bazı gecikmeler olabilir.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>DC aracısı bir proxy ile iletişim kuramaz

Bu sorunun ana belirtisi DC aracı sı admin olay günlüğünde 30018 olayları. Bu sorunun birkaç olası nedeni olabilir:

1. DC aracısı, kayıtlı proxy(ler) ağ bağlantısına izin vermeyen ağın yalıtılmış bir bölümünde bulunur. Bu sorun, diğer DC aracıları Azure'dan parola ilkeleri indirmek için proxy(ler) ile iletişim kurabildiği sürece iyi huylu olabilir. Bir kez indirildikten sonra, bu ilkeler daha sonra sysvol payı ilke dosyalarının çoğaltılması yoluyla yalıtılmış DC tarafından elde edilecektir.

1. Proxy ana bilgisayar makinesi RPC uç nokta mapper uç noktasına erişimi engelliyor (bağlantı noktası 135)

   Azure AD Parola Koruma Proxy yükleyicisi otomatik olarak 135 bağlantı noktasına erişimsağlayan bir Windows Güvenlik Duvarı gelen kuralı oluşturur. Bu kural daha sonra silinirse veya devre dışı bırakılırsa, DC aracıları Proxy hizmetiyle iletişim kuramaz. Yerleşik Windows Güvenlik Duvarı başka bir güvenlik duvarı ürünü yerine devre dışı bırakılmışsa, bu güvenlik duvarını bağlantı noktası 135'e erişime izin verecek şekilde yapılandırmanız gerekir.

1. Proxy ana bilgisayar makinesi, Proxy hizmeti tarafından dinlenen RPC bitiş noktasına (dinamik veya statik) erişimi engelliyor

   Azure AD Parola Koruma Proxy yükleyicisi, Azure AD Parola Koruması Proxy hizmeti tarafından dinlenen gelen bağlantı noktalarına erişime izin veren bir Windows Güvenlik Duvarı gelen kuralını otomatik olarak oluşturur. Bu kural daha sonra silinirse veya devre dışı bırakılırsa, DC aracıları Proxy hizmetiyle iletişim kuramaz. Yerleşik Windows Güvenlik Duvarı başka bir güvenlik duvarı ürünü yerine devre dışı bırakılmışsa, bu güvenlik duvarını Azure AD Parola Koruması Proxy hizmeti tarafından dinlenen gelen bağlantı noktalarına erişime izin verecek şekilde yapılandırmanız gerekir. Proxy hizmeti belirli bir statik RPC bağlantı noktasında `Set-AzureADPasswordProtectionProxyConfiguration` (cmdlet kullanılarak) dinlenecek şekilde yapılandırıldıysa, bu yapılandırma daha spesifik hale getirilebilir.

1. Proxy ana bilgisayar makinesi etki alanı denetleyicileri makinede oturum açma yeteneği sağlayacak şekilde yapılandırılmamıştır. Bu davranış, "Bu bilgisayara ağdan eriş" kullanıcı ayrıcalığı ataması ile denetlenir. Ormandaki tüm etki alanlarındaki tüm etki alanı denetleyicilerine bu ayrıcalık verilmelidir. Bu ayar genellikle daha büyük bir ağ sertleştirme çabasının bir parçası olarak kısıtlanır.

## <a name="proxy-service-is-unable-to-communicate-with-azure"></a>Proxy hizmeti Azure ile iletişim kuramıyor

1. Proxy makinesinin [dağıtım gereksinimlerinde](howto-password-ban-bad-on-premises-deploy.md)listelenen uç noktalara bağlantı sağladığından emin olun.

1. Ormanın ve tüm proxy sunucularının aynı Azure kiracısına karşı kayıtlı olduğundan emin olun.

   Bu gereksinimi `Get-AzureADPasswordProtectionProxy` `Get-AzureADPasswordProtectionDCAgent` PowerShell cmdlets'i çalıştırarak denetleyebilir ve döndürülen her öğenin özelliğini `AzureTenant` karşılaştırabilirsiniz. Doğru çalışma için, bildirilen kiracı adı tüm DC aracıları ve proxy sunucuları arasında aynı olmalıdır.

   Bir Azure kiracı kaydı uyuşmazlığı koşulu varsa, bu sorun, tüm `Register-AzureADPasswordProtectionForest` kayıtlar için aynı Azure kiracısının kimlik bilgilerini kullandığınızdan emin olmak için gerektiğinde `Register-AzureADPasswordProtectionProxy` powershell cmdlets çalıştırılarak giderilebilir.

## <a name="dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files"></a>DC aracısı parola ilkesi dosyalarını şifreleyemiyor veya şifresini çözemiyor

Azure AD Parola Koruması, Microsoft Anahtar Dağıtım Hizmeti tarafından sağlanan şifreleme ve şifre çözme işlevlerine kritik bir bağımlılık sağlar. Şifreleme veya şifre çözme hataları çeşitli belirtilerle ortaya çıkabilir ve çeşitli olası nedenleri vardır.

1. KDS hizmetinin bir etki alanında tüm Windows Server 2012 ve daha sonra etki alanı denetleyicilerinde etkin ve işlevsel olduğundan emin olun.

   Varsayılan olarak KDS hizmetinin hizmet başlangıç modu El Ile (Tetikleyici Başlat) olarak yapılandırılır. Bu yapılandırma, istemci nin hizmeti ilk kez kullanmaya çalıştığı anda isteğe bağlı olarak başlatıldırı anlamına gelir. Bu varsayılan hizmet başlangıç modu, Azure AD Parola Koruması'nın çalışması için kabul edilebilir.

   KDS hizmet başlangıç modu Devre Dışı Bırakılmış olarak yapılandırıldıysa, Azure AD Parola Koruması düzgün çalışmadan önce bu yapılandırmanın düzeltilmesi gerekir.

   Bu sorun için basit bir test, Hizmet yönetimi MMC konsolu üzerinden veya diğer yönetim araçlarını kullanarak KDS hizmetini el ile başlatmaktır (örneğin, komut istemi konsolundan "net başlangıç kdssvc" çalıştırın). KDS hizmetinin başarılı bir şekilde başlaması ve çalışmaya devam etmesi bekleniyor.

   KDS hizmetinin başlatılamamasının en yaygın temel nedeni, Active Directory etki alanı denetleyicisi nesnesinin varsayılan Etki Alanı Denetleyicileri OU'nun dışında bulunmasıdır. Bu yapılandırma KDS hizmeti tarafından desteklenmez ve Azure AD Parola Koruması tarafından uygulanan bir sınırlama değildir. Bu koşulun düzeltmesi, etki alanı denetleyicisi nesnesini varsayılan Etki Alanı Denetleyicileri OU altında bir konuma taşımaktır.

1. Uyumsuz KDS şifreli arabellek biçimi Değişikliği Windows Server 2012 R2'den Windows Server 2016'ya

   Windows Server 2016'da KDS şifreli arabelleklerin biçimini değiştiren bir KDS güvenlik düzeltmesi sunuldu; bu arabellekler bazen Windows Server 2012 ve Windows Server 2012 R2'de şifreyi çözemez. Ters yön tamam - Windows Server 2012 ve Windows Server 2012 R2'de KDS şifreli arabellekler, Windows Server 2016 ve sonraki nde her zaman başarılı bir şekilde şifresini çözecektir. Active Directory etki alanlarınızdaki etki alanı denetleyicileri bu işletim sistemlerinin bir karışımını çalıştırıyorsa, bazen Azure AD Parola Koruması şifre çözme hataları bildirilebilir. Güvenlik düzeltmesinin doğası göz önüne alındığında ve hangi etki alanı denetleyicisinin belirli bir anda verileri şifreleyecek olan Azure AD Password Protection DC Aracısı'nın belirleyici olmadığı göz önüne alındığında, bu hataların zamanlamasını veya belirtilerini doğru bir şekilde tahmin etmek mümkün değildir.

   Microsoft bu sorun için bir düzeltme araştırıyor, ancak henüz kullanılabilir bir ETA yok. Bu arada, Active Directory etki alanı (lar) bu uyumsuz işletim sistemlerinin bir karışımını çalıştırmak için başka bir geçici çözüm yoktur. Başka bir deyişle, yalnızca Windows Server 2012 ve Windows Server 2012 R2 etki alanı denetleyicilerini çalıştırmanız veya yalnızca Windows Server 2016 ve üzeri etki alanı denetleyicilerini çalıştırmanız gerekir.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Zayıf parolalar kabul ediliyor, ancak

Bu sorunun çeşitli nedenleri olabilir.

1. DC aracınızın süresi dolmuş genel bir önizleme yazılımı sürümü çalıştırıyor. Bkz. [Genel önizleme DC aracı yazılımının süresi doldu.](howto-password-ban-bad-on-premises-troubleshoot.md#public-preview-dc-agent-software-has-expired)

1. DC aracınız bir ilke indiremez veya varolan ilkelerin şifresini çözemez. Yukarıdaki konularda olası nedenleri kontrol edin.

1. Parola ilkesi Zorlama modu hala Denetim olarak ayarlanır. Bu yapılandırma etkinse, Azure AD Parola Koruması portalını kullanarak zorlama olarak yeniden yapılandırın. Daha fazla bilgi için [bkz.](howto-password-ban-bad-on-premises-operations.md#modes-of-operation)

1. Parola ilkesi devre dışı bırakıldı. Bu yapılandırma etkinse, Azure AD Parola Koruması portalını kullanarak etkinleştirilen şekilde yeniden yapılandırın. Daha fazla bilgi için [bkz.](howto-password-ban-bad-on-premises-operations.md#modes-of-operation)

1. Dc aracı yazılımını etki alanındaki tüm etki alanı denetleyicilerine yüklemedin. Bu durumda, uzak Windows istemcilerinin parola değiştirme işlemi sırasında belirli bir etki alanı denetleyicisini hedeflediğinden emin olmak zordur. DC aracı yazılımının yüklü olduğu belirli bir DC'yi başarıyla hedefaldığınızı düşünüyorsanız, DC aracısı yönetici olay günlüğünü iki kez kontrol ederek doğrulayabilirsiniz: sonuç ne olursa olsun, parolanın sonucunu belgelemek için en az bir olay olacaktır Doğrulama. Parolası değiştirilen kullanıcı için olay yoksa, parola değişikliği büyük olasılıkla farklı bir etki alanı denetleyicisi tarafından işlenmiştir.

   Alternatif bir sınama olarak, dc aracı yazılımının yüklü olduğu dc'de oturum açtıkça parolaları ayarlamayı\değiştirmeyi deneyin. Bu teknik üretim Active Directory etki alanları için önerilmez.

   DC aracı yazılımının artımlı dağıtımı bu sınırlamalara bağlı olarak desteklenirken, Microsoft DC aracı yazılımının bir etki alanında en kısa sürede tüm etki alanı denetleyicilerine yüklenmesini şiddetle önerir.

1. Parola doğrulama algoritması aslında beklendiği gibi çalışıyor olabilir. Bkz. [Parolalar nasıl değerlendirilir.](concept-password-ban-bad.md#how-are-passwords-evaluated)

## <a name="ntdsutilexe-fails-to-set-a-weak-dsrm-password"></a>Ntdsutil.exe zayıf bir DSRM şifresini ayarlamak için başarısız olur

Etkin Dizin, etki alanının parola karmaşıklığı gereksinimlerini karşıladığından emin olmak için her zaman yeni bir Dizin Hizmetleri Onarım Modu parolasını doğrular; bu doğrulama, Azure AD Parola Koruması gibi parola filtresi dll'lerine de çağrıda bulunur. Yeni DSRM parolası reddedilirse, aşağıdaki hata iletisi sonuçları:

```text
C:\>ntdsutil.exe
ntdsutil: set dsrm password
Reset DSRM Administrator Password: reset password on server null
Please type password for DS Restore Mode Administrator Account: ********
Please confirm new password: ********
Setting password failed.
        WIN32 Error Code: 0xa91
        Error Message: Password doesn't meet the requirements of the filter dll's
```

Azure AD Parola Koruması, Active Directory DSRM parolası için parola doğrulama olay günlüğü olay(ları) günlüğe kaydettiğinde, olay günlüğü iletilerinin bir kullanıcı adı içermemesi beklenir. DSRM hesabı gerçek Active Directory etki alanının bir parçası olmayan yerel bir hesap olduğundan bu davranış oluşur.  

## <a name="domain-controller-replica-promotion-fails-because-of-a-weak-dsrm-password"></a>Etki alanı denetleyicisi çoğaltma promosyonu zayıf bir DSRM parolası nedeniyle başarısız olur

DC promosyon işlemi sırasında, yeni Dizin Hizmetleri Onarım Modu parolası doğrulama için etki alanında mevcut bir DC'ye gönderilecektir. Yeni DSRM parolası reddedilirse, aşağıdaki hata iletisi sonuçları:

```powershell
Install-ADDSDomainController : Verification of prerequisites for Domain Controller promotion failed. The Directory Services Restore Mode password does not meet a requirement of the password filter(s). Supply a suitable password.
```

Yukarıdaki sorunda olduğu gibi, herhangi bir Azure AD Parola Koruması parola doğrulama sonucu olayı bu senaryo için boş kullanıcı adlarını olacaktır.

## <a name="domain-controller-demotion-fails-due-to-a-weak-local-administrator-password"></a>Etki alanı denetleyicisi indirgeme zayıf bir yerel Yönetici parolası nedeniyle başarısız olur

Dc aracı yazılımını hala çalıştıran bir etki alanı denetleyicisini düşürmek için desteklenir. Ancak yöneticiler, DC aracı yazılımının indirgeme işlemi sırasında geçerli parola ilkesini zorlamaya devam ettiğini bilmelidir. Yeni yerel Yönetici hesap parolası (indirgeme işleminin bir parçası olarak belirtilir) diğer parolalar gibi doğrulanır. Microsoft, DC indirgeme yordamının bir parçası olarak yerel Yönetici hesapları için güvenli parolaların seçilmesini önerir.

Kaldırma başarılı olduktan ve etki alanı denetleyicisi yeniden başlatıldıktan ve normal bir üye sunucu olarak yeniden çalıştırıldıktan sonra, DC aracı yazılımı pasif modda çalışmaya geri döner. Daha sonra herhangi bir zamanda kaldırılabilir.

## <a name="booting-into-directory-services-repair-mode"></a>Dizin Hizmetleri Onarım Moduna Önyükleme

Etki alanı denetleyicisi Dizin Hizmetleri Onarım Moduna önyüklenirse, DC aracısı parola filtresi dll bu durumu algılar ve şu anda etkin olan ilke den bağımsız olarak tüm parola doğrulama veya zorlama etkinliklerinin devre dışı edilmesine neden olur Yapılandırma. DC aracısı parola filtresi dll, örneğin Yönetici olay günlüğüne 10023 uyarı olayını kaydeder:

```text
The password filter dll is loaded but the machine appears to be a domain controller that has been booted into Directory Services Repair Mode. All password change and set requests will be automatically approved. No further messages will be logged until after the next reboot.
```
## <a name="public-preview-dc-agent-software-has-expired"></a>Genel önizleme DC aracı yazılımının süresi doldu

Azure AD Parola Koruması genel önizleme döneminde, DC aracı yazılımı aşağıdaki tarihlerde parola doğrulama isteklerini işlemeyi durdurmak için sabit kodlanmıştır:

* Sürüm 1.2.65.0, 1 Eylül 2019 tarihinde parola doğrulama isteklerini işlemeyi durdurur.
* Sürüm 1.2.25.0 ve önceki sürüm 1 Temmuz 2019'da şifre doğrulama isteklerini işleme durduruldu.

Son tarih yaklaştıkça, tüm zaman sınırlı DC aracı sürümleri, önyükleme zamanında DC aracısı Admin olay günlüğüne 10021 olay yatacak ve bu gibi görünür:

```text
The password filter dll has successfully loaded and initialized.

The allowable trial period is nearing expiration. Once the trial period has expired, the password filter dll will no longer process passwords. Please contact Microsoft for an newer supported version of the software.

Expiration date:  9/01/2019 0:00:00 AM

This message will not be repeated until the next reboot.
```

Son tarih geçtikten sonra, tüm zaman sınırlı DC aracı sürümleri aşağıdaki gibi görünen önyükleme zamanında DC aracı Yöneticisi olay günlüğüne 10022 olay yatacak:

```text
The password filter dll is loaded but the allowable trial period has expired. All password change and set requests will be automatically approved. Please contact Microsoft for a newer supported version of the software.

No further messages will be logged until after the next reboot.
```

Son tarih yalnızca ilk önyüklemede denetlendi, takvim son tarihi geçtikten çok sonraya kadar bu olayları göremeyebilirsiniz. Son tarih tanındıktan sonra, etki alanı denetleyicisi veya tüm parolalar dışında daha büyük bir ortam üzerinde hiçbir olumsuz etki otomatik olarak onaylanacaktır.

> [!IMPORTANT]
> Microsoft, süresi dolan genel önizleme DC aracılarının hemen en son sürüme yükseltilmesini önerir.

Yükseltmesi gereken ortamınızda DC aracılarını keşfetmenin kolay `Get-AzureADPasswordProtectionDCAgent` bir yolu, örneğin cmdlet'i çalıştırmaktır:

```powershell
PS C:\> Get-AzureADPasswordProtectionDCAgent

ServerFQDN            : bpl1.bpl.com
SoftwareVersion       : 1.2.125.0
Domain                : bpl.com
Forest                : bpl.com
PasswordPolicyDateUTC : 8/1/2019 9:18:05 PM
HeartbeatUTC          : 8/1/2019 10:00:00 PM
AzureTenant           : bpltest.onmicrosoft.com
```

Bu konu için, SoftwareVersion alanı açıkça bakmak için önemli bir özelliktir. PowerShell filtrelemesini, örneğin, gerekli temel sürümde veya üzerinde olan DC aracılarını filtrelemek için de kullanabilirsiniz:

```powershell
PS C:\> $LatestAzureADPasswordProtectionVersion = "1.2.125.0"
PS C:\> Get-AzureADPasswordProtectionDCAgent | Where-Object {$_.SoftwareVersion -lt $LatestAzureADPasswordProtectionVersion}
```

Azure AD Parola Koruma Proxy yazılımı hiçbir sürümde zaman sınırlı değildir. Microsoft, hem DC hem de proxy aracılarının yayımlandıkça en son sürümlere yükseltilmesini önerir. `Get-AzureADPasswordProtectionProxy` Cmdlet, dc aracıları için yukarıdaki örneğe benzer şekilde yükseltme gerektiren Proxy aracılarını bulmak için kullanılabilir.

Dc [aracısını yükseltme](howto-password-ban-bad-on-premises-deploy.md#upgrading-the-dc-agent) ve belirli yükseltme yordamları hakkında daha fazla bilgi için [Proxy hizmetini yükseltmeye](howto-password-ban-bad-on-premises-deploy.md#upgrading-the-proxy-service) bakın.

## <a name="emergency-remediation"></a>Acil durum düzeltmesi

DC aracı hizmetinin sorunlara neden olduğu bir durum oluşursa, DC aracı hizmeti hemen kapatılabilir. DC aracısı parola filtresi dll hala çalışmayan hizmeti aramaya çalışır ve uyarı olaylarını günlüğe kaydeder (10012, 10013), ancak bu süre içinde gelen tüm parolalar kabul edilir. DC aracı hizmeti daha sonra Windows Service Control Manager aracılığıyla gerektiğinde "Devre Dışı" başlangıç türüyle yapılandırılabilir.

Bir diğer düzeltme önlemi de Azure AD Parola Koruması portalında Etkinleştir modunu Hayır olarak ayarlamak olacaktır. Güncelleştirilmiş ilke indirildikten sonra, her DC aracı sıcağına tüm parolaların olduğu gibi kabul edildiği bir moda geçecektir. Daha fazla bilgi için [bkz.](howto-password-ban-bad-on-premises-operations.md#modes-of-operation)

## <a name="removal"></a>Kaldırma

Azure AD parola koruma yazılımını kaldırmaya ve ilgili tüm durumu etki alanı(lar) ve ormandan temizlemeye karar verenden, bu görev aşağıdaki adımları kullanarak gerçekleştirilebilir:

> [!IMPORTANT]
> Bu adımları sırayla gerçekleştirmek önemlidir. Proxy hizmetinin herhangi bir örneği çalışmaya bırakılırsa, hizmetini düzenli olarak yeniden oluştururConnectionPoint nesnesi. DC aracı hizmetinin herhangi bir örneği çalışmaya bırakılırsa, hizmetini aralıklarla yeniden oluştururConnectionPoint nesnesi ve sysvol durumu.

1. Proxy yazılımını tüm makinelerden kaldırın. Bu adım yeniden başlatma **gerektirmez.**
2. Tüm etki alanı denetleyicilerinden DC Agent yazılımını kaldırın. Bu adım yeniden **başlatmayı gerektirir.**
3. Her etki alanı adlandırma bağlamında tüm Proxy hizmet bağlantı noktalarını el ile kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory PowerShell komutu ile bulunabilir:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   $keywords değişken değerinin sonundaki yıldız işaretini ("*") atlamayın.

   `Get-ADObject` Komut uçağrısıyla bulunan `Remove-ADObject`ortaya çıkan nesne(ler) daha sonra el ile ile ile türlenebilir veya silinebilir.

4. Her etki alanı adlandırma bağlamında tüm DC aracı bağlantı noktalarını el ile kaldırın. Yazılımın ne kadar yaygın olarak dağıtıldığına bağlı olarak, ormanda etki alanı denetleyicisi başına bu nesnelerden biri olabilir. Bu nesnenin konumu aşağıdaki Active Directory PowerShell komutu yla bulunabilir:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   `Get-ADObject` Komut uçağrısıyla bulunan `Remove-ADObject`ortaya çıkan nesne(ler) daha sonra el ile ile ile türlenebilir veya silinebilir.

   $keywords değişken değerinin sonundaki yıldız işaretini ("*") atlamayın.

5. Orman düzeyindeyapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Etkin Dizin yapılandırması adlandırma bağlamında bir kapsayıcıda tutulur. Aşağıdaki gibi keşfedilebilir ve silinebilir:

   ```powershell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Aşağıdaki klasörü ve tüm içeriğini el ile silerek sysvol ile ilgili tüm durumu el ile kaldırın:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Gerekirse, bu yola belirli bir etki alanı denetleyicisi üzerinde yerel olarak da erişilebilir; varsayılan konum aşağıdaki yol gibi bir şey olacaktır:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Sysvol payı varsayılan olmayan bir konumda yapılandırıldıysa, bu yol farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Parola Koruması için sık sorulan sorular](howto-password-ban-bad-on-premises-faq.md)

Genel ve özel yasaklı parola listeleri hakkında daha fazla bilgi için, makaleyi [Ban kötü parolalar](concept-password-ban-bad.md) bakın
