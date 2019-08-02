---
title: Azure AD parola koruması sorunlarını giderme-Azure Active Directory
description: Azure AD parola koruması genel sorunlarını anlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d96f5bb189dfd20c65fc6fc6ddcb8fff66d52ff
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68666229"
---
# <a name="azure-ad-password-protection-troubleshooting"></a>Azure AD parola koruması sorunlarını giderme

Azure AD parola koruması 'nın dağıtımı sonrasında, sorun giderme gerekebilir. Bu makale, bazı yaygın sorun giderme adımlarını anlamanıza yardımcı olmak için ayrıntılara gider.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>DC Aracısı dizinde bir ara sunucu bulamıyor

Bu sorunun ana belirtisi, DC Aracısı Yönetici olay günlüğündeki 30017 olaydır.

Bu sorunun olağan nedeni, bir ara sunucu henüz kaydedilmemiştir. Bir ara sunucu kaydedilmişse, belirli bir DC Aracısı bu proxy 'yi görebilene kadar AD çoğaltma gecikmesi nedeniyle biraz gecikme olabilir.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>DC Aracısı bir ara sunucu ile iletişim kuramıyor

Bu sorunun ana belirtisi, DC Aracısı Yönetici olay günlüğündeki 30018 olaydır. Bu sorun birkaç olası nedeni olabilir:

1. DC Aracısı ağın yalıtılmış bir bölümünde bulunur ve bu, kayıtlı ara sunucu (lar) a ağ bağlantısına izin vermez. Bu nedenle, diğer DC aracıları Azure 'dan parola ilkelerini indirmek üzere proxy (ler) i ile iletişim kurabildiği sürece bu sorun zararsız olabilir. böylece, SYSVOL paylaşımında ilke dosyalarının çoğaltılmasıyla yalıtılmış DC tarafından elde edilir.

1. Proxy konak makinesi RPC uç nokta Eşleyici uç noktasına erişimi engelliyor (bağlantı noktası 135)

   Azure AD parola koruma proxy yükleyicisi, 135 numaralı bağlantı noktasına erişime izin veren bir Windows Güvenlik Duvarı gelen kuralı otomatik olarak oluşturur. Bu kural daha sonra silinirse veya devre dışıysa, DC aracıları ara sunucu hizmetiyle iletişim kuramaz. Yerleşik Windows Güvenlik Duvarı başka bir güvenlik duvarı ürününün yerine devre dışı bırakılmışsa, bu güvenlik duvarını 135 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırmanız gerekir.

1. Proxy konak makinesi, proxy hizmeti tarafından dinlenen RPC uç noktasına (dinamik veya statik) erişimi engelliyor

   Azure AD parola koruma proxy yükleyicisi, Azure AD parola koruma proxy hizmeti tarafından listelenen tüm gelen bağlantı noktalarına erişime izin veren bir Windows Güvenlik Duvarı gelen kuralı otomatik olarak oluşturur. Bu kural daha sonra silinirse veya devre dışıysa, DC aracıları ara sunucu hizmetiyle iletişim kuramaz. Yerleşik Windows Güvenlik Duvarı başka bir güvenlik duvarı ürününün yerine devre dışı bırakılmışsa, bu güvenlik duvarını Azure AD parola koruma proxy hizmeti tarafından listelenen tüm gelen bağlantı noktalarına erişime izin verecek şekilde yapılandırmanız gerekir. Bu yapılandırma, proxy hizmeti belirli bir statik RPC bağlantı noktasını dinlemek üzere yapılandırılmışsa ( `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet 'ini kullanarak) daha belirgin hale getirilebilir.

## <a name="proxy-service-is-unable-to-communicate-with-azure"></a>Proxy hizmeti Azure ile iletişim kuramıyor

1. Proxy makinenin, [dağıtım gereksinimlerinde](howto-password-ban-bad-on-premises-deploy.md)listelenen uç noktalara bağlantısı olduğundan emin olun.

1. Ormanın ve tüm proxy sunucularının aynı Azure kiracısına karşı kaydedildiğinden emin olun.

   `Get-AzureADPasswordProtectionProxy` Ve `Get-AzureADPasswordProtectionDCAgent` PowerShell`AzureTenant` cmdlet 'lerini çalıştırarak bu gereksinimi denetleyebilir ve ardından döndürülen her bir öğenin özelliğini karşılaştırabilirsiniz. Doğru işlem için, bildirilen kiracı adı tüm DC aracıları ve proxy sunucuları genelinde aynı olmalıdır.

   Bir Azure kiracı kaydı uyuşmazlığı koşulu varsa, bu sorun `Register-AzureADPasswordProtectionProxy` ve/veya `Register-AzureADPasswordProtectionForest` PowerShell cmdlet 'leri gerektiği şekilde çalıştırılarak düzeltilebilir ve tüm kayıtlar için aynı Azure kiracısındaki kimlik bilgilerini kullandığınızdan emin olabilirsiniz.

## <a name="dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files"></a>DC Aracısı, parola ilkesi dosyalarını şifreleyemedi veya şifresini çözemedi

Bu sorun birçok belirtiyle bildirimde bulunabilir, ancak genellikle ortak bir kök nedeni vardır.

Azure AD parola koruması, Microsoft anahtar dağıtım hizmeti tarafından sağlanan şifreleme ve şifre çözme işlevlerine kritik bir bağımlılığa sahiptir ve bu, Windows Server 2012 ve üstünü çalıştıran etki alanı denetleyicilerinde kullanılabilir. KDS hizmeti, bir etki alanındaki tüm Windows Server 2012 ve üzeri etki alanı denetleyicilerinde etkinleştirilmelidir ve çalışır durumda olmalıdır.

Varsayılan olarak, KDS hizmetinin hizmet başlatma modu El Ile (tetikleyici başlatma) olarak yapılandırılmıştır. Bu yapılandırma, bir istemcinin hizmeti ilk kez kullanmaya çalıştığı, isteğe bağlı olarak başlatıldığı anlamına gelir. Bu varsayılan hizmet başlatma modu, Azure AD parola korumasının çalışması için kabul edilebilir.

KDS hizmeti başlatma modu devre dışı olarak yapılandırıldıysa, Azure AD parola koruması düzgün çalışmadan önce bu yapılandırma düzeltilmelidir.

Bu sorunun basit bir testi, hizmet yönetimi MMC konsolu aracılığıyla veya diğer yönetim araçlarını kullanarak KDS hizmetini el ile başlatmak (örneğin, bir komut istemi konsolundan "net start kdssvc" komutunu çalıştırmak). KDS hizmetinin başarıyla başlaması bekleniyordu ve çalışır durumda kalır.

KDS hizmetinin başlatılamamasına neden olan en yaygın kök nedeni, Active Directory etki alanı denetleyicisi nesnesinin varsayılan etki alanı denetleyicileri OU 'su dışında konumlandırılabilileridir. Bu yapılandırma KDS hizmeti tarafından desteklenmiyor ve Azure AD parola koruması tarafından uygulanan bir kısıtlama değil. Bu koşulun düzeltilmesi, etki alanı denetleyicisi nesnesini varsayılan etki alanı denetleyicileri OU 'su altındaki bir konuma taşımadır.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Zayıf parolalar kabul ediliyor ancak olmamalıdır

Bu sorunun çeşitli nedenleri olabilir.

1. DC aracılarınız bir ilkeyi indiremez veya mevcut ilkelerin şifresini çözemiyor. Yukarıdaki konularda olası nedenleri kontrol edin.

1. Parola ilkesi zorla modu hala denetim olarak ayarlanmıştır. Bu yapılandırma etkin ise, Azure AD parola koruma portalı 'nı kullanmaya zorlamak için yeniden yapılandırın. Bkz. [parola korumasını etkinleştirme](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Parola ilkesi devre dışı bırakıldı. Bu yapılandırma etkin ise, Azure AD parola koruma portalı 'nı kullanarak etkin olarak yeniden yapılandırın. Bkz. [parola korumasını etkinleştirme](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Etki alanındaki tüm etki alanı denetleyicilerine DC Aracısı yazılımını yüklememedi. Bu durumda, uzak Windows istemcilerinin parola değiştirme işlemi sırasında belirli bir etki alanı denetleyicisini hedeflemesini sağlamak zordur. DC Aracısı yazılımının yüklü olduğu belirli bir DC 'yi başarıyla hedeflediğinizi düşünüyorsanız, DC Aracısı Yönetici olay günlüğü ' ne iki kez göz ayırarak doğrulayabilirsiniz: sonuca bakılmaksızın, parolanın sonucunu belgelemek için en az bir olay olacaktır doğrulamasına. Parolası değiştirilen Kullanıcı için bir olay yoksa, parola değişikliği muhtemelen farklı bir etki alanı denetleyicisi tarafından işlenir.

   Alternatif bir test olarak, DC Aracısı yazılımının yüklü olduğu bir DC 'ye doğrudan oturum açarken, parolaları settingın olarak değiştirmeyi deneyin. Bu teknik, üretim Active Directory etki alanları için önerilmez.

   Bu sınırlamalara bağlı olarak, DC Aracısı yazılımının artımlı dağıtımı desteklenirken, Microsoft, DC Aracısı yazılımının bir etki alanındaki tüm etki alanı denetleyicilerine en kısa sürede yüklenmesini özellikle önerir.

1. Parola doğrulama algoritması aslında beklendiği gibi çalışıyor olabilir. [Parolaların nasıl değerlendirildiğini](concept-password-ban-bad.md#how-are-passwords-evaluated)görün.

## <a name="ntdsutilexe-fails-to-set-a-weak-dsrm-password"></a>Ntdsutil. exe zayıf bir DSRM parolası ayarlayamazsa

Active Directory, etki alanının parola karmaşıklığı gereksinimlerini karşıladığından emin olmak için her zaman yeni bir dizin hizmetleri onarım modu parolasını doğrular; Bu doğrulama Ayrıca Azure AD parola koruması gibi parola filtre dll 'lerine da çağrı yapılır. Yeni DSRM parolası reddedilirse aşağıdaki hata iletisi oluşur:

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

Azure AD parola koruması, Active Directory DSRM parolası için parola doğrulama olay günlüğü olaylarını günlüğe kaydettiği zaman, olay günlüğü iletilerinin bir Kullanıcı adı içermeyecektir. Bu durum, DSRM hesabının gerçek Active Directory etki alanının parçası olmayan bir yerel hesap olduğu için oluşur.  

## <a name="domain-controller-replica-promotion-fails-because-of-a-weak-dsrm-password"></a>Etki alanı denetleyicisi çoğaltma yükseltmesi zayıf bir DSRM parolası nedeniyle başarısız oluyor

DC yükseltme işlemi sırasında, yeni Dizin Hizmetleri onarım modu parolası, doğrulama için etki alanında var olan bir DC 'ye gönderilir. Yeni DSRM parolası reddedilirse aşağıdaki hata iletisi oluşur:

```powershell
Install-ADDSDomainController : Verification of prerequisites for Domain Controller promotion failed. The Directory Services Restore Mode password does not meet a requirement of the password filter(s). Supply a suitable password.
```

Yukarıdaki sorun içinde olduğu gibi, tüm Azure AD parola koruması parola doğrulama sonucu olayları bu senaryo için boş kullanıcı adlarına sahip olur.

## <a name="domain-controller-demotion-fails-due-to-a-weak-local-administrator-password"></a>Etki alanı denetleyicisi indirgeme zayıf bir yerel yönetici parolası nedeniyle başarısız oluyor

Hala DC Aracısı yazılımını çalıştıran bir etki alanı denetleyicisini indirgemek için desteklenir. Yöneticiler, indirgeme yordamı sırasında DC Aracısı yazılımının geçerli parola ilkesini zorlamaya devam ettiğinden haberdar edilmelidir. Yeni yerel yönetici hesabı parolası (indirgeme işleminin parçası olarak belirtilir), diğer tüm parolalar gibi onaylanır. Microsoft, DC indirgeme yordamının bir parçası olarak yerel yönetici hesapları için güvenli parolaların seçilmesini önerir.

İndirgeme başarılı olduktan ve etki alanı denetleyicisi yeniden başlatıldığında ve bir normal üye sunucu olarak çalıştırıldığında, DC Aracısı yazılımı pasif modda çalışmaya geri döner. Daha sonra herhangi bir zamanda kaldırılabilir.

## <a name="booting-into-directory-services-repair-mode"></a>Dizin Hizmetleri onarım modunda önyükleme

Etki alanı denetleyicisi dizin hizmetleri onarım modunda önbaşlatıldıysa, DC Aracısı hizmeti bu durumu algılar ve şu anda etkin olan ilke yapılandırmasından bağımsız olarak tüm parola doğrulama veya zorlama etkinliklerinin devre dışı olmasına neden olur.

## <a name="emergency-remediation"></a>Acil durum Düzeltme

DC Aracısı hizmetinin soruna neden olduğu bir durum oluşursa, DC Aracısı hizmeti hemen kapatılabilir. DC aracısı parola filtresi dll 'si hala çalışır durumda olmayan hizmeti çağırmaya çalışır ve uyarı olaylarını günlüğe kaydeder (10012, 10013), ancak tüm gelen parolalar bu süre içinde kabul edilir. Daha sonra DC Aracısı hizmeti, gerektiğinde "devre dışı" başlatma türü ile Windows hizmet denetimi Yöneticisi aracılığıyla da yapılandırılabilir.

Başka bir düzeltme ölçüsü, etkinleştirme modunu Azure AD parola koruma portalında Hayır olarak ayarlamak olacaktır. Güncelleştirilmiş ilke indirildikten sonra, her DC Aracısı hizmeti, tüm parolaların olduğu gibi kabul edildiği bir quiescent moduna geçer. Daha fazla bilgi için bkz. [zorla modu](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="removal"></a>Çıkarma

Azure AD parola koruma yazılımını kaldırmaya ve etki alanından ve ormandan ilgili tüm durumu temizlemesine karar verdiyseniz, bu görev aşağıdaki adımlar kullanılarak gerçekleştirilebilir:

> [!IMPORTANT]
> Bu adımları sırasıyla gerçekleştirmek önemlidir. Proxy hizmeti 'nin herhangi bir örneği çalışmaya ayrıldıysa, Service ConnectionPoint nesnesini düzenli olarak yeniden oluşturur. DC Aracısı hizmeti 'nin herhangi bir örneği çalışmaya ayrıldıysa, serviceConnectionPoint nesnesini ve SYSVOL durumunu düzenli olarak yeniden oluşturur.

1. Tüm makinelerden proxy yazılımını kaldırın. Bu adım için yeniden **başlatma gerekmez.**
2. DC Aracısı yazılımını tüm etki alanı denetleyicilerinden kaldırın. Bu adım **için** yeniden başlatma gerekir.
3. Her bir etki alanı adlandırma bağlamındaki tüm proxy hizmeti bağlantı noktalarını el ile kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory PowerShell komutuyla bulunabilir:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   $Keywords değişken değerinin sonundaki yıldız işaretini ("*") atlayın.

   `Get-ADObject` Komut aracılığıyla bulunan sonuç nesneleri, daha sonra `Remove-ADObject`el ile veya silinmiş olabilir.

4. Her bir etki alanı adlandırma bağlamındaki tüm DC Aracısı bağlantı noktalarını el ile kaldırın. Yazılımın ne kadar büyük ölçüde dağıtıldığını bağlı olarak, ormanda etki alanı denetleyicisi başına bu nesnelerden biri olabilir. Bu nesnenin konumu aşağıdaki Active Directory PowerShell komutuyla bulunabilir:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   `Get-ADObject` Komut aracılığıyla bulunan sonuç nesneleri, daha sonra `Remove-ADObject`el ile veya silinmiş olabilir.

   $Keywords değişken değerinin sonundaki yıldız işaretini ("*") atlayın.

5. Orman düzeyindeki yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamındaki bir kapsayıcıda tutulur. Bulunabilir ve aşağıdaki gibi silinebilir:

   ```powershell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Aşağıdaki klasörü ve tüm içeriğini el ile silerek tüm SYSVOL ilgili durumlarını el ile kaldırın:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Gerekirse, bu yola belirli bir etki alanı denetleyicisinde yerel olarak de erişilebilir. Varsayılan konum aşağıdaki yola benzer bir şeydir:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   SYSVOL paylaşımının varsayılan olmayan bir konumda yapılandırılmış olması durumunda bu yol farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola koruması hakkında sık sorulan sorular](howto-password-ban-bad-on-premises-faq.md)

Genel ve özel yasaklanmış parola listeleri hakkında daha fazla bilgi için bkz. [Hatalı parolalar](concept-password-ban-bad.md)
