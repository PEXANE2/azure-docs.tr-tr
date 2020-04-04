---
title: Şirket içinde Azure AD Parola Koruması dağıtma
description: Şirket içi Aktif Dizin Etki Alanı Hizmetleri ortamında Azure AD Parola Koruması'nı nasıl planlayıp dağıtabileceğinizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 03/05/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ac9b76dd8d3c950b14f6d7b331f15647427ac89
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652731"
---
# <a name="plan-and-deploy-on-premises-azure-active-directory-password-protection"></a>Şirket içi Azure Etkin Dizin Parola Koruması planlama ve dağıtma

Kullanıcılar genellikle okul, spor takımı veya ünlü kişi gibi yaygın yerel sözcükleri kullanan parolalar oluşturur. Bu parolaları tahmin etmek kolaydır ve sözlük tabanlı saldırılara karşı zayıftır. Kuruluşunuzdaki güçlü parolaları zorlamak için Azure Active Directory (Azure AD) Parola Koruması, genel ve özel olarak yasaklanmış bir parola listesi sağlar. Bu yasaklı parola listesinde eşleşme varsa parola değiştirme isteği başarısız olur.

Şirket içi Active Directory Domain Services (AD DS) ortamınızı korumak için, ön lisans DC'nizle çalışmak üzere Azure AD Parola Koruması'nı yükleyebilir ve yapılandırabilirsiniz. Bu makalede, Azure AD Parola Koruması proxy hizmetini ve Azure AD Parola Koruması DC aracısını şirket içi ortamınızda nasıl yükleyip kaydedebilirsiniz.

Azure AD Parola Koruması'nın şirket içi ortamda nasıl çalıştığı hakkında daha fazla bilgi için [Windows Server Etkin Dizin için Azure AD Parola Koruması'nı nasıl uygulayacağınızı](concept-password-ban-bad-on-premises.md)öğrenin.

## <a name="deployment-strategy"></a>Dağıtım stratejisi

Aşağıdaki diyagram, Azure AD Parola Koruması'nın temel bileşenlerinin şirket içi Etkin Dizin ortamında nasıl birlikte çalıştığını gösterir:

![Azure AD Parola Koruması bileşenleri birlikte nasıl çalışır?](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Yazılımı dağıtmadan önce yazılımın nasıl çalıştığını gözden geçirmek iyi bir fikirdir. Daha fazla bilgi için Azure [AD Parola Koruması'na kavramsal genel bakış](concept-password-ban-bad-on-premises.md)bölümüne bakın.

Dağıtımlara *denetim* modunda başlamanızı öneririz. Denetim modu, parolaların ayarlanmaya devam edebileceği varsayılan ilk ayardır. Engellenecek parolalar olay günlüğüne kaydedilir. Proxy sunucularını ve DC aracılarını denetim modunda dağıttıktan sonra, ilke uygulandığında parola ilkesinin kullanıcılar üzerindeki etkisini izleyin.

Denetim aşamasında, birçok kuruluş aşağıdaki durumların geçerli olduğunu bulur:

* Daha güvenli parolalar kullanmak için varolan operasyonel süreçleri geliştirmeleri gerekir.
* Kullanıcılar genellikle güvenli olmayan parolalar kullanır.
* Kullanıcıları güvenlik zorlamada yapılacak değişiklik, bunlar üzerindeki olası etkisi ve daha güvenli parolaları nasıl seçecekleri konusunda bilgilendirmeleri gerekir.

Daha güçlü parola doğrulamasının varolan Active Directory etki alanı denetleyicisi dağıtım otomasyonunuzu etkilemesi de mümkündür. Bu tür sorunların ortaya çıkarılamaya yardımcı olmak için denetim dönemi değerlendirmesi sırasında en az bir DC promosyonu ve bir DC indirgeme gerçekleşmesini öneririz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Ntdsutil.exe zayıf bir Dizin Hizmetleri Onarım Modu parolası ayarlayamıyor](howto-password-ban-bad-on-premises-troubleshoot.md#ntdsutilexe-fails-to-set-a-weak-dsrm-password)
* [Etki alanı denetleyicisi çoğaltma promosyonzayıf Dizin Hizmetleri Onarım Modu parolası nedeniyle başarısız olur](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-replica-promotion-fails-because-of-a-weak-dsrm-password)
* [Etki alanı denetleyicisi indirgeme zayıf bir yerel Yönetici parolası nedeniyle başarısız olur](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-demotion-fails-due-to-a-weak-local-administrator-password)

Özellik denetim modunda makul bir süre çalıştırıldıktan sonra, daha güvenli parolalar gerektirmek için yapılandırmayı *Denetimden* *Uygula'ya* geçirebilirsiniz. Bu süre içinde ek izleme iyi bir fikirdir.

### <a name="multiple-forest-considerations"></a>Birden fazla orman dikkat

Azure AD Parola Koruması'nı birden çok ormana dağıtmak için ek gereksinim yoktur.

Her orman, aşağıdaki bölümde açıklandığı gibi, [önm Azure AD Parola Koruması'nı dağıtmak](#download-required-software)için bağımsız olarak yapılandırılmıştır. Her Azure AD Parola Koruması proxy'si yalnızca katıldığı ormandaki etki alanı denetleyicilerini destekleyebilir.

Herhangi bir ormandaki Azure AD Parola Koruma yazılımı, Active Directory güven yapılandırmalarından bağımsız olarak diğer ormanlarda dağıtılan parola koruma yazılımından habersizdir.

### <a name="read-only-domain-controller-considerations"></a>Salt okunur etki alanı denetleyicisi hususları

Parola değişikliği veya ayarolayları işlenmez ve salt okunur etki alanı denetleyicilerinde (RODCs) devam eder. Bunun yerine, yazılabilir etki alanı denetleyicilerine iletilirler. RODC'lere Azure AD Password Protection DC aracı yazılımını yüklemeniz gerekmez.

Ayrıca, Salt AD Parola Koruması proxy hizmetini salt okunur etki alanı denetleyicisi üzerinde çalıştırmak için desteklenmez.

### <a name="high-availability-considerations"></a>Yüksek kullanılabilirlik hususları

Parola koruması için temel endişe, ormandaki DC'ler Azure'dan yeni ilkeler veya başka veriler indirmeye çalıştıklarında Azure AD Parola Koruması proxy sunucularının kullanılabilirliğidir. Her Azure AD Parola Koruma DC aracısı, hangi proxy sunucusunun arayacağına karar verirken basit bir yuvarlak robin tarzı algoritma kullanır. Aracı, yanıt vermeyen proxy sunucularını atlar.

Hem dizin hem de sysvol klasör durumunun sağlıklı çoğaltılmasına sahip tam olarak bağlı olan çoğu Etkin Dizin dağıtımları için, iki Azure AD Parola Koruması proxy sunucusu kullanılabilirliği sağlamak için yeterlidir. Bu yapılandırma, yeni ilkelerin ve diğer verilerin zamanında indirilebilen sonuçlar. İsterseniz ek Azure AD Parola Koruması proxy sunucuları dağıtabilirsiniz.

Azure AD Password Protection DC aracı yazılımının tasarımı, yüksek kullanılabilirlikle ilişkili olağan sorunları azaltır. Azure AD Parola Koruma DC aracısı, en son indirilen parola ilkesinin yerel önbelleğini tutar. Tüm kayıtlı proxy sunucuları kullanılamamış olsa bile, Azure AD Parola Koruması DC aracıları önbelleğe alınmış parola ilkelerini zorlamaya devam ederler.

Büyük bir dağıtımda parola ilkeleri için makul bir güncelleştirme sıklığı genellikle saatler veya daha az değil, günlerdir. Bu nedenle, proxy sunucularının kısa kesintileri Azure AD Parola Koruması'nı önemli ölçüde etkilemez.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri

Lisanslama hakkında daha fazla bilgi için Azure [AD Parola Koruması lisanslama gereksinimlerine](concept-password-ban-bad.md#license-requirements)bakın.

Aşağıdaki temel gereksinimler geçerlidir:

* Azure AD Parola Koruması bileşenleriyüklü etki alanı denetleyicileri de dahil olmak üzere tüm makinelerin Evrensel C Çalışma Zamanı yüklü olması gerekir.
    * Windows Update'teki tüm güncelleştirmeleri aldığınızdan emin olarak çalışma süresini alabilirsiniz. Veya işletim sistemi özel bir güncelleme paketinde alabilirsiniz. Daha fazla bilgi [için Windows'da Evrensel C Çalışma Zamanı için Güncelleştirme'ye](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows)bakın.
* Windows Server Active Directory ormanını Azure AD'ye kaydetmek için orman kökü etki alanında Etkin Dizin etki alanı yöneticisi ayrıcalıklarına sahip bir hesaba ihtiyacınız vardır.
* Anahtar Dağıtım Hizmeti, Windows Server 2012'yi çalıştıran etki alanındaki tüm etki alanı denetleyicilerinde etkinleştirilmelidir. Varsayılan olarak, bu hizmet el ile tetikleme başlatma yoluyla etkinleştirilir.
* Ağ bağlantısı, her etki alanında en az bir etki alanı denetleyicisi ile Azure AD Parola Koruması için proxy hizmetini barındıran en az bir sunucu arasında olmalıdır. Bu bağlantı, etki alanı denetleyicisinin RPC endpoint mapper bağlantı noktası 135 ve proxy hizmetindeki RPC sunucu bağlantı noktasına erişmesine izin vermelidir.
    * Varsayılan olarak, RPC sunucu bağlantı noktası dinamik bir RPC bağlantı noktasıdır, ancak [statik bir bağlantı noktası kullanacak](#static)şekilde yapılandırılabilir.
* Azure AD Parola Koruma Proxy hizmetinin yüklendiği tüm makinelerin aşağıdaki uç noktalara ağ erişimi olmalıdır:

    |**Uç Nokta**|**Amaç**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Kimlik doğrulama istekleri|
    |`https://enterpriseregistration.windows.net`|Azure AD Parola Koruması işlevi|

### <a name="azure-ad-password-protection-dc-agent"></a>Azure AD Şifre Koruma DC aracısı

Aşağıdaki gereksinimler Azure AD Parola Koruması DC aracısı için geçerlidir:

* Azure AD Password Protection DC aracı yazılımının yüklendiği tüm makineler Windows Server 2012 veya sonraki bir tarihte çalıştırılmalıdır.
    * Etkin Dizin etki alanı veya orman Windows Server 2012 etki alanı işlevsel düzeyi (DFL) veya orman işlevsel düzeyinde (FFL) olması gerekmez. [Tasarım İlkeleri](concept-password-ban-bad-on-premises.md#design-principles)belirtildiği gibi, dc ajan veya proxy yazılımı çalıştırmak için gerekli minimum DFL veya FFL var.
* Azure AD Parola Koruma DC aracısını çalıştıran tüm makinelerin .NET 4.5 yüklü olması gerekir.
* Azure AD Parola Koruması DC aracı hizmetini çalıştıran herhangi bir Etkin Dizin etki alanı, sysvol çoğaltma için Dağıtılmış Dosya Sistemi Çoğaltma (DFSR) kullanmalıdır.
   * Etki alanınız zaten DFSR kullanmıyorsa, Azure AD Parola Koruması'nı yüklemeden önce geçiş yapmanız gerekir. Daha fazla bilgi için [Bkz. SYSVOL Çoğaltma Geçiş Kılavuzu: FRS-DFS Çoğaltma](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019(v=ws.10))

    > [!WARNING]
    > Azure AD Password Protection DC aracı yazılımı şu anda sysvol çoğaltma için FRS (DFSR için selefi teknoloji) kullanmaya devam eden etki alanlarında etki alanı denetleyicileri üzerine yüklenir, ancak yazılım bu ortamda düzgün çalışmaz.
    >
    > Ek olumsuz yan etkiler, tek tek dosyaların çoğaltılamaz ve sysvol geri yükleme yordamları başarılı görünüyor ama sessizce tüm dosyaları çoğaltmak için başarısız içerir.
    >
    > Hem DFSR'nin doğal yararları hem de Azure AD Parola Koruması dağıtımının engelini kaldırmak için etki alanınızı mümkün olan en kısa sürede DFSR'yi kullanmak üzere geçirin. Yazılımın gelecekteki sürümleri, hala FRS kullanan bir etki alanında çalışırken otomatik olarak devre dışı bırakılır.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD Şifre Koruma proxy hizmeti

Aşağıdaki gereksinimler Azure AD Parola Koruması proxy hizmeti için geçerlidir:

* Azure AD Parola Koruması proxy hizmetinin yüklendiği tüm makineler Windows Server 2012 R2 veya sonraki lerini çalıştırmalıdır.

    > [!NOTE]
    > Azure AD Parola Koruması proxy hizmeti dağıtımı, etki alanı denetleyicisi giden doğrudan internet bağlantısına sahip olsa bile Azure AD Parola Koruması'nı dağıtmak için zorunlu bir gereksinimdir.

* Azure AD Parola Koruması proxy hizmetinin yüklendiği tüm makinelerde .NET 4.7 yüklü olmalıdır.
    * .NET 4.7 zaten tam olarak güncellenmiş bir Windows Server yüklü olmalıdır. Gerekirse, [Windows için .NET Framework 4.7 çevrimdışı yükleyicide bulunan yükleyiciyi](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows)indirin ve çalıştırın.
* Azure AD Parola Koruması proxy hizmetini barındıran tüm makineler, etki alanı denetleyicilerine proxy hizmetinde oturum açma olanağı verecek şekilde yapılandırılmalıdır. Bu yetenek, "Bu bilgisayara ağdan eriş" ayrıcalığı ataması ile denetlenir.
* Azure AD Parola Koruması proxy hizmetini barındıran tüm makineler, giden TLS 1.2 HTTP trafiğine izin verecek şekilde yapılandırılmalıdır.
* Azure AD Parola Koruması proxy hizmetini kaydetmek için *bir Global Administrator* hesabı ve orman Azure AD ile.
* [Uygulama Proxy ortamı kurulum yordamlarında](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#prepare-your-on-premises-environment)belirtilen bağlantı noktaları ve URL'ler kümesi için ağ erişimi etkinleştirilmelidir.

### <a name="microsoft-azure-ad-connect-agent-updater-prerequisites"></a>Microsoft Azure AD Connect Aracısı Güncelleyici ön koşulları

Microsoft Azure AD Connect Agent Updater hizmeti, Azure AD Parola Koruma Proxy hizmetiyle yan yana yüklenir. Microsoft Azure AD Connect Agent Updater hizmetinin çalışabilmesi için ek yapılandırma gereklidir:

* Ortamınız bir HTTP proxy sunucusu kullanıyorsa, [varolan şirket içi proxy sunucularıyla Work'te](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-connectors-with-proxy-servers)belirtilen yönergeleri izleyin.
* Microsoft Azure AD Connect Agent Updater hizmeti, [TLS gereksinimlerinde](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#tls-requirements)belirtilen TLS 1.2 adımlarını da gerektirir.

> [!WARNING]
> Azure AD Parola Koruması proxy'si ve Azure AD Application Proxy, Microsoft Azure AD Connect Agent Updater hizmetinin farklı sürümlerini yükler, bu nedenle talimatlar Uygulama Proxy içeriğine başvurur. Bu farklı sürümler yan yana yüklendiğinde uyumsuzdur, bu nedenle aynı makineye Azure AD Parola Koruma Proxy'si ve Uygulama Proxy'si yüklenmesi önerilmez.

## <a name="download-required-software"></a>Gerekli yazılımı indirin

Şirket içi Azure AD Parola Koruması dağıtımı için iki gerekli yükleyici vardır:

* Azure AD Şifre Koruma DC aracısı (*AzureADPasswordProtectionDCAgentSetup.msi*)
* Azure AD Şifre Koruma proxy (*AzureADPasswordProtectionProxySetup.exe*)

Microsoft [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=57071)her iki yükleyicilerindir.

## <a name="install-and-configure-the-proxy-service"></a>Proxy hizmetini yükleme ve yapılandırma

Azure AD Parola Koruması proxy hizmeti genellikle şirket içi AD DS ortamınızdaki bir üye sunucuda dır. Azure AD Parola Koruması proxy hizmeti yüklendikten sonra, Azure AD kiracınız için genel ve müşteri yasaklı parola listelerinin bir kopyasını korumak için Azure AD ile iletişim kurar.

Sonraki bölümde, şirket içi AD DS ortamınızdaki etki alanı denetleyicilerine Azure AD Parola Koruması DC aracılarını yüklersiniz. Bu DC aracıları, etki alanı içindeki parola değişikliği olaylarını işlerken kullanılmak üzere en son yasaklı parola listelerini almak için proxy hizmetiyle iletişim kurar.

Azure AD Parola Koruması proxy hizmetini barındırmak için bir veya daha fazla sunucu seçin. Sunucu(lar) için aşağıdaki hususlar geçerlidir:

* Bu tür her hizmet yalnızca tek bir orman için parola ilkeleri sağlayabilir. Ana bilgisayar makinesi, o ormandaki bir etki alanına katılmalıdır. Kök ve alt etki alanlarının her ikisi de desteklenir. Ormanın her alanında en az bir DC ile parola koruma makinesi arasında ağ bağlantısı gerekir.
* Azure AD Parola Koruması proxy hizmetini sınama için bir etki alanı denetleyicisi üzerinde çalıştırabilirsiniz, ancak bu etki alanı denetleyicisi daha sonra internet bağlantısı gerektirir. Bu bağlantı bir güvenlik sorunu olabilir. Bu yapılandırmayı yalnızca sınama için öneririz.
* Yüksek [kullanılabilirlik hususları](#high-availability-considerations)önceki bölümde belirtildiği gibi, fazlalık için en az iki Azure AD Parola Koruması proxy sunucusu öneririz.
* Salt okunur etki alanı denetleyicisi üzerinde Azure AD Parola Koruması proxy hizmetini çalıştırmak için desteklenmez.

Azure AD Parola Koruması proxy hizmetini yüklemek için aşağıdaki adımları tamamlayın:

1. Azure AD Parola Koruması proxy hizmetini `AzureADPasswordProtectionProxySetup.exe` yüklemek için yazılım yükleyicisi çalıştırın.

    Yazılım yüklemesi yeniden başlatma gerektirmez ve aşağıdaki örnekte olduğu gibi standart MSI yordamları kullanılarak otomatikleştirilmiş olabilir:
    
    ```console
    AzureADPasswordProtectionProxySetup.exe /quiet
    ```
    
    > [!NOTE]
    > Yükleme hatasını önlemek için paketi yüklemeden `AzureADPasswordProtectionProxySetup.exe` önce Windows Güvenlik Duvarı hizmeti çalışıyor olmalıdır.
    >
    > Windows Güvenlik Duvarı çalışmamak üzere yapılandırılırsa, geçici çözüm yükleme sırasında Güvenlik Duvarı hizmetini geçici olarak etkinleştirmek ve çalıştırmaktır. Proxy yazılımının yüklemeden sonra Windows Güvenlik Duvarı'na özel bir bağımlılığı yoktur.
    >
    > Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, dağıtım gereksinimlerini karşılayabilmek için yine de yapılandırılmalıdır. Bunlar, bağlantı noktası 135'e ve proxy RPC sunucu bağlantı noktasına gelen erişime izin vermeyi içerir. Daha fazla bilgi için [dağıtım gereksinimleriyle](#deployment-requirements)ilgili önceki bölüme bakın.

1. Azure AD Şifre Koruma proxy yazılımı yeni `AzureADPasswordProtection`bir PowerShell modülü içerir. Aşağıdaki adımlar bu PowerShell modülünden çeşitli cmdlets çalıştırın.

    Bu modülü kullanmak için bir PowerShell penceresini yönetici olarak açın ve yeni modülü aşağıdaki gibi aktarın:
    
    ```powershell
    Import-Module AzureADPasswordProtection
    ```

1. Azure AD Parola Koruması proxy hizmetinin çalıştığını denetlemek için aşağıdaki PowerShell komutunu kullanın:

    ```powershell
    Get-Service AzureADPasswordProtectionProxy | fl
    ```

    Sonuç, *Bir Çalışma* **Durumu** göstermelidir.

1. Proxy hizmeti makinede çalışıyor, ancak Azure AD ile iletişim kurmak için kimlik bilgileri yok. Cmdlet'i kullanarak Azure AD Parola `Register-AzureADPasswordProtectionProxy` Koruması proxy sunucusunu Azure AD ile kaydedin.

    Bu cmdlet, Azure kiracınız için genel yönetici kimlik bilgileri gerektirir. Ayrıca, orman kökü etki alanında şirket içi Active Directory etki alanı yöneticisi ayrıcalıklarına da ihtiyacınız vardır. Bu cmdlet ayrıca yerel yönetici ayrıcalıklarına sahip bir hesap kullanılarak çalıştırılmalıdır:

    Bu komut, Azure AD Parola Koruması proxy hizmeti için bir kez başarılı olduktan sonra, ek çağrıları başarılı olur, ancak gereksizdir.

    Cmdlet `Register-AzureADPasswordProtectionProxy` aşağıdaki üç kimlik doğrulama modunu destekler. İlk iki mod Azure Çok Faktörlü Kimlik Doğrulamasını destekler, ancak üçüncü mod desteklemez.

    > [!TIP]
    > Bu cmdlet'in belirli bir Azure kiracısı için çalıştırılması ilk kez tamamlanmadan önce gözle görülür bir gecikme olabilir. Bir hata bildirilmedikçe, bu gecikme hakkında endişelenmeyin.

     * Etkileşimli kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Bu mod Server Core işletim sistemlerinde çalışmaz. Bunun yerine, aşağıdaki kimlik doğrulama modlarından birini kullanın. Ayrıca, Internet Explorer Gelişmiş Güvenlik Yapılandırması etkinse bu mod başarısız olabilir. Geçici çözüm, yapılandırmayı devre dışı bırakıp proxy'yi kaydetmek ve sonra yeniden etkinleştirmektir.

     * Aygıt kodu kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        ```

        İstendiğinde, bir web tarayıcısı açmak ve kimlik doğrulama kodunu girmek için bağlantıyı izleyin.

     * Sessiz (parola tabanlı) kimlik doğrulama modu:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Hesabınız için Azure Çok Faktörlü Kimlik Doğrulama gerekliyse, bu mod başarısız olur. Bu durumda, önceki iki kimlik doğrulama modundan birini kullanın veya bunun yerine MFA gerektirmeyen farklı bir hesap kullanın.
        >
        > Azure Aygıt Kaydı (Azure AD Parola Koruması tarafından kapakların altında kullanılan) genel olarak MFA gerektirecek şekilde yapılandırıldıysa, MFA'nın gerekli olduğunu da görebilirsiniz. Bu gereksinimi çözüme getirmek için önceki iki kimlik doğrulama modundan biriyle MFA'yı destekleyen farklı bir hesap kullanabilirsiniz veya Azure Aygıt Kaydı MFA gereksinimini geçici olarak gevşetebilirsiniz.
        >
        > Bu değişikliği yapmak için Azure portalında **Azure Etkin Dizini'ni** arayın ve seçin ve ardından **Aygıt ayarları > aygıtları**seçin. **Set Multi-Factor Auth gerektiren cihazlara katılmak için** *Hayır*. Kayıt tamamlandıktan sonra bu ayarı *Evet* olarak yeniden yapılandırdığından emin olun.
        >
        > MFA gereksinimlerinin yalnızca test amacıyla atlatılmasını öneririz.

    Şu anda gelecekteki işlevler için ayrılmış *-ForestCredential* parametresini belirtmeniz gerekmez.

    Azure AD Parola Koruması proxy hizmetinin kaydı, hizmet süresi boyunca yalnızca bir kez gereklidir. Bundan sonra, Azure AD Parola Koruması proxy hizmeti gerekli diğer tüm hizmetleri otomatik olarak gerçekleştirir.

1. Şimdi, PowerShell cmdlet'i kullanarak Azure ile iletişim kurmak `Register-AzureADPasswordProtectionForest` için şirket içi Active Directory ormanını gerekli kimlik bilgileriyle kaydedin.

    > [!NOTE]
    > Ortamınızda birden çok Azure AD Parola Koruması proxy sunucusu yüklüyse, ormanı kaydetmek için hangi proxy sunucusunu kullandığınız önemli değildir.

    Cmdlet, Azure kiracınız için genel yönetici kimlik bilgileri gerektirir. Ayrıca bu cmdlet yerel yönetici ayrıcalıkları olan bir hesap kullanarak çalıştırmalısınız. Ayrıca şirket içi Active Directory Enterprise Administrator ayrıcalıkları gerektirir. Bu adım orman başına bir kez çalıştırılır.

    Cmdlet `Register-AzureADPasswordProtectionForest` aşağıdaki üç kimlik doğrulama modunu destekler. İlk iki mod Azure Çok Faktörlü Kimlik Doğrulamasını destekler, ancak üçüncü mod desteklemez.

    > [!TIP]
    > Bu cmdlet'in belirli bir Azure kiracısı için çalıştırılması ilk kez tamamlanmadan önce gözle görülür bir gecikme olabilir. Bir hata bildirilmedikçe, bu gecikme hakkında endişelenmeyin.

     * Etkileşimli kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Bu mod Server Core işletim sistemlerinde çalışmaz. Bunun yerine aşağıdaki iki kimlik doğrulama modundan birini kullanın. Ayrıca, Internet Explorer Gelişmiş Güvenlik Yapılandırması etkinse bu mod başarısız olabilir. Geçici çözüm, Yapılandırmayı devre dışı bırakıp, ormanı kaydetmek ve sonra yeniden etkinleştirmektir.  

     * Aygıt kodu kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        ```

        İstendiğinde, bir web tarayıcısı açmak ve kimlik doğrulama kodunu girmek için bağlantıyı izleyin.

     * Sessiz (parola tabanlı) kimlik doğrulama modu:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Hesabınız için Azure Çok Faktörlü Kimlik Doğrulama gerekliyse, bu mod başarısız olur. Bu durumda, önceki iki kimlik doğrulama modundan birini kullanın veya bunun yerine MFA gerektirmeyen farklı bir hesap kullanın.
        >
        > Azure Aygıt Kaydı (Azure AD Parola Koruması tarafından kapakların altında kullanılan) genel olarak MFA gerektirecek şekilde yapılandırıldıysa, MFA'nın gerekli olduğunu da görebilirsiniz. Bu gereksinimi çözüme getirmek için önceki iki kimlik doğrulama modundan biriyle MFA'yı destekleyen farklı bir hesap kullanabilirsiniz veya Azure Aygıt Kaydı MFA gereksinimini geçici olarak gevşetebilirsiniz.
        >
        > Bu değişikliği yapmak için Azure portalında **Azure Etkin Dizini'ni** arayın ve seçin ve ardından **Aygıt ayarları > aygıtları**seçin. **Set Multi-Factor Auth gerektiren cihazlara katılmak için** *Hayır*. Kayıt tamamlandıktan sonra bu ayarı *Evet* olarak yeniden yapılandırdığından emin olun.
        >
        > MFA gereksinimlerinin yalnızca test amacıyla atlatılmasını öneririz.

       Bu örnekler yalnızca şu anda oturum açmış kullanıcı da kök etki alanı için Etkin Dizin etki alanı yöneticisiyse başarılı olur. Bu durumda, *-ForestCredential* parametresi üzerinden alternatif etki alanı kimlik bilgileri sağlayabilirsiniz.

    Aktif Dizin ormanının kaydı, orman ömrü boyunca sadece bir kez gereklidir. Bundan sonra, ormandaki Azure AD Parola Koruma DC aracıları gerekli diğer tüm hizmetleri otomatik olarak gerçekleştirir. Bir `Register-AzureADPasswordProtectionForest` orman için başarılı bir şekilde çalıştırdıktan sonra, cmdlet ek çağrıları başarılı, ancak gereksizdir.
    
    Başarılı `Register-AzureADPasswordProtectionForest` olmak için, Windows Server 2012 veya sonraki en az bir DC çalıştıran Azure AD Parola Koruması proxy sunucusunun etki alanında kullanılabilir olmalıdır. Azure AD Password Protection DC aracı yazılımının bu adımdan önce hiçbir etki alanı denetleyicisine yüklenmesi gerekmez.

### <a name="configure-the-proxy-service-to-communicate-through-an-http-proxy"></a>Bir HTTP proxy üzerinden iletişim kurmak için proxy hizmetini yapılandırma

Ortamınız Azure ile iletişim kurmak için belirli bir HTTP proxy'sinin kullanılmasını gerektiriyorsa, Azure AD Parola Koruması hizmetini yapılandırmak için aşağıdaki adımları kullanın.

`%ProgramFiles%\Azure AD Password Protection Proxy\Service` Klasörde bir *AzureADPasswordProtectionProxy.exe.config* dosyası oluşturun. Aşağıdaki içeriği ekleyin:

   ```xml
   <configuration>
      <system.net>
         <defaultProxy enabled="true">
         <proxy bypassonlocal="true"
            proxyaddress="http://yourhttpproxy.com:8080" />
         </defaultProxy>
      </system.net>
   </configuration>
   ```

HTTP proxy'niz kimlik doğrulaması gerektiriyorsa, *useDefault Credentials* etiketini ekleyin:

   ```xml
   <configuration>
      <system.net>
         <defaultProxy enabled="true" useDefaultCredentials="true">
         <proxy bypassonlocal="true"
            proxyaddress="http://yourhttpproxy.com:8080" />
         </defaultProxy>
      </system.net>
   </configuration>
   ```

Her iki durumda `http://yourhttpproxy.com:8080` da, belirli HTTP proxy sunucunuzun adresi ve bağlantı noktasıyla değiştirin.

HTTP proxy'niz bir yetkilendirme ilkesi kullanacak şekilde yapılandırılırsa, parola koruması için proxy hizmetini barındıran makinenin Active Directory bilgisayar hesabına erişim izni vermelisiniz.

*AzureADPasswordProtectionProxy.exe.config* dosyasını oluşturduktan veya güncelledikten sonra Azure AD Parola Koruması proxy hizmetini durdurmanızı ve yeniden başlatmanızı öneririz.

Proxy hizmeti, bir HTTP proxy'ye bağlanmak için belirli kimlik bilgilerinin kullanımını desteklemez.

### <a name="configure-the-proxy-service-to-listen-on-a-specific-port"></a>Proxy hizmetini belirli bir bağlantı noktasında dinleyecek şekilde yapılandırın

Azure AD Password Protection DC aracısı, proxy hizmetiyle iletişim kurmak için TCP üzerinden RPC kullanır. Varsayılan olarak, Azure AD Parola Koruması proxy hizmeti kullanılabilir dinamik RPC bitiş noktasını dinler. Ortamınızdaki ağ topolojisi veya güvenlik duvarı gereksinimleri nedeniyle gerekirse hizmeti belirli bir TCP bağlantı noktasında dinleyecek şekilde yapılandırabilirsiniz.

<a id="static" /></a>Hizmeti statik bir bağlantı noktası altında çalışacak `Set-AzureADPasswordProtectionProxyConfiguration` şekilde yapılandırmak için cmdlet'i aşağıdaki gibi kullanın:

```powershell
Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
```

> [!WARNING]
> Bu değişikliklerin etkili olması için Azure AD Parola Koruması proxy hizmetini durdurmanız ve yeniden başlatmanız gerekir.

Hizmeti dinamik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için aynı yordamı kullanın, ancak *StaticPort'u* sıfıra ayarlayın:

```powershell
Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
```

> [!WARNING]
> Bu değişikliklerin etkili olması için Azure AD Parola Koruması proxy hizmetini durdurmanız ve yeniden başlatmanız gerekir.

Azure AD Parola Koruması proxy hizmeti, bağlantı noktası yapılandırmasındaki herhangi bir değişiklikten sonra el ile yeniden başlatmayı gerektirir. Bu yapılandırma değişikliklerini yaptıktan sonra etki alanı denetleyicilerinde Azure AD Parola Koruması DC aracı hizmetini yeniden başlatmanız gerekmez.

Hizmetin geçerli yapılandırmasını sorgulamak için `Get-AzureADPasswordProtectionProxyConfiguration` aşağıdaki örnekte gösterildiği gibi cmdlet'i kullanın

```powershell
Get-AzureADPasswordProtectionProxyConfiguration | fl
```

Aşağıdaki örnek çıktı, Azure AD Parola Koruması proxy hizmetinin dinamik bir bağlantı noktası kullandığını gösterir:

```output
ServiceName : AzureADPasswordProtectionProxy
DisplayName : Azure AD password protection Proxy
StaticPort  : 0
```

## <a name="install-the-dc-agent-service"></a>DC aracı hizmetini yükleme

Azure AD Password Protection DC aracı hizmetini `AzureADPasswordProtectionDCAgentSetup.msi` yüklemek için paketi çalıştırın.

Aşağıdaki örnekte gösterildiği gibi standart MSI yordamları kullanarak yazılım yüklemesini otomatikleştirebilirsiniz:

```console
msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart
```

Yükleyicinin `/norestart` makineyi otomatik olarak yeniden başlatmasını tercih ederseniz, bayrak atlanabilir.

Yazılım yüklemesi veya yüklemeyi kaldırma, yeniden başlatma gerektirir. Parola filtresi DL'lerin yalnızca yeniden başlatılarak yüklenmesi veya boşaltılması bu gereksinimdir.

Önm Azure AD Parola Koruması'nın yüklenmesi, DC aracı yazılımı bir etki alanı denetleyicisi üzerine yüklendikten ve bu bilgisayar yeniden başlatıldıktan sonra tamamlanır. Başka bir yapılandırma gerekli veya mümkün değildir. Ön kullanıma açık DC'lere karşı parola değiştirme olayları, Azure AD'deki yapılandırılmış yasaklı parola listelerini kullanır.

Azure portalından ön m Azure AD Parola Koruması'nı etkinleştirmek veya özel yasaklı parolaları yapılandırmak için şirket [içinde Azure AD Parola Korumasını Etkinleştir'e](howto-password-ban-bad-on-premises-operations.md)bakın.

> [!TIP]
> Azure AD Parola Koruma DC aracısını henüz etki alanı denetleyicisi olmayan bir makineye yükleyebilirsiniz. Bu durumda, hizmet başlar ve çalışır, ancak makine etki alanı denetleyicisi olarak yükseltilene kadar etkin kalır.

## <a name="upgrading-the-proxy-service"></a>Proxy hizmetini yükseltme

Azure AD Parola Koruması proxy hizmeti otomatik yükseltmeyi destekler. Otomatik yükseltme, proxy hizmetiyle yan yana yüklenen Microsoft Azure AD Connect Agent Updater hizmetini kullanır. Otomatik yükseltme varsayılan olarak açıktır ve `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet kullanılarak etkinleştirilebilir veya devre dışı bırakılabilir.

Geçerli ayar `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet kullanılarak sorgulanabilir. Otomatik yükseltme ayarının her zaman etkin olmasını öneririz.

Cmdlet, `Get-AzureADPasswordProtectionProxy` bir ormanda şu anda yüklü olan tüm Azure AD Parola Koruması proxy sunucularının yazılım sürümünü sorgulamak için kullanılabilir.

### <a name="manual-upgrade-process"></a>Manuel yükseltme işlemi

El ile yükseltme, `AzureADPasswordProtectionProxySetup.exe` yazılım yükleyicisinin en son sürümünü çalıştırarak gerçekleştirilir. Yazılımın en son sürümü [Microsoft Download Center'da](https://www.microsoft.com/download/details.aspx?id=57071)mevcuttur.

Azure AD Parola Koruması proxy hizmetinin geçerli sürümünü kaldırmak gerekmez - yükleyici yerinde yükseltme yapar. Proxy hizmetini yükseltirken yeniden başlatma gerekmez. Yazılım yükseltmesi, `AzureADPasswordProtectionProxySetup.exe /quiet`standart MSI yordamları kullanılarak otomatikleştirilmiş olabilir.

## <a name="upgrading-the-dc-agent"></a>DC aracısını yükseltme

Azure AD Password Protection DC aracı yazılımının daha yeni bir sürümü kullanılabilir olduğunda, yükseltme `AzureADPasswordProtectionDCAgentSetup.msi` yazılım paketinin en son sürümünü çalıştırarak gerçekleştirilir. Yazılımın en son sürümü [Microsoft Download Center'da](https://www.microsoft.com/download/details.aspx?id=57071)mevcuttur.

DC aracı yazılımının geçerli sürümünü kaldırmak gerekmez - yükleyici yerinde bir yükseltme gerçekleştirir. DC aracı yazılımını yükseltirken her zaman yeniden başlatma gerekir - bu gereksinim temel Windows davranışından kaynaklanır.

Yazılım yükseltmesi, `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart`standart MSI yordamları kullanılarak otomatikleştirilmiş olabilir.

Yükleyicinin makineyi `/norestart` otomatik olarak yeniden başlatmasını isterseniz bayrağı atlaabilirsiniz.

Cmdlet, `Get-AzureADPasswordProtectionDCAgent` şu anda yüklü olan tüm Azure AD Parola Koruması DC aracılarının bir ormanda yazılım sürümünü sorgulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık şirket içi sunucularınızda Azure AD Parola Koruması için ihtiyacınız olan hizmetleri yüklediğinize göre, yüklemenizi tamamlamak için [Azure portalında ön yükleme Azure AD Parola Koruması'nı etkinleştirin.](howto-password-ban-bad-on-premises-operations.md)
