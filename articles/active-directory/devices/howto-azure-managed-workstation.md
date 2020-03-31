---
title: Azure tarafından yönetilen iş istasyonlarını dağıtma - Azure Etkin Dizini
description: Yanlış yapılandırma veya uzlaşma nedeniyle ihlal riskini azaltmak için güvenli, Azure tarafından yönetilen iş istasyonlarını nasıl dağıttığınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d02b0299b6267fdd9d880d5bc0fe8c93d0edadc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672614"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Güvenli, Azure tarafından yönetilen bir iş istasyonu dağıtma

Artık [güvenli iş istasyonlarını](concept-azure-managed-workstation.md)anladığınıziçin, dağıtım işlemine başlama nın zamanı geldi. Bu kılavuzla, en başından itibaren daha güvenli bir iş istasyonu oluşturmak için tanımlı profilleri kullanırsınız.

![Güvenli bir iş istasyonunun dağıtımı](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Çözümü dağıtmadan önce bir profil seçin. Dağıtımda aynı anda birden çok profil kullanabilir ve bunları etiketlerveya gruplarla atayabilirsiniz.

> [!NOTE]
> İhtiyaçlarınızın gerektirdiği profilleri uygulayın. Microsoft Intune'a atayarak başka bir profile geçebilirsiniz.

| Profil | Düşük | Gelişmiş | Yüksek | Özelleştirilmiş | Güvenli | Yalıtılmış |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Azure AD'deki kullanıcı | Evet | Evet | Evet | Evet | Evet | Evet |
| Intune tarafından yönetilen | Evet | Evet | Evet | Evet | Evet | Evet |
| Aygıt - Azure AD kayıtlı | Evet |  |  |  |  | |   |
| Aygıt - Azure AD katıldı |   | Evet | Evet | Evet | Evet | Evet |
| Intune güvenlik taban çizgisi uygulandı |   | Evet <br> (Geliştirilmiş) | Evet <br> (Yüksek Güvenlik) | Evet <br> (NCSC) | Evet <br> (Güvenli) | NA |
| Donanım, güvenli Windows 10 Standartlarını karşılar |   | Evet | Evet | Evet | Evet | Evet |
| Microsoft Defender ATP etkin |   | Evet  | Evet | Evet | Evet | Evet |
| Yönetici haklarının kaldırılması |   |   | Evet  | Evet | Evet | Evet |
| Microsoft Otomatik Pilot'u kullanarak dağıtım |   |   | Evet  | Evet | Evet | Evet |
| Yalnızca Intune tarafından yüklenen uygulamalar |   |   |   | Evet | Evet |Evet |
| Onaylı listeyle sınırlandırılmış URL'ler |   |   |   | Evet | Evet |Evet |
| Internet engellendi (gelen/giden) |   |   |   |  |  |Evet |

> [!NOTE]
> Güvenli iş istasyonunda kılavuz **cihazlar** profiller ve ilkelerle birlikte atanacaktır. Kullanıcılar, aygıt paylaşımının (paylaşılan aygıtların) etkin olmasını sağlayan ilkelere doğrudan uygulanmaz. Dağıtımınızda güvenli bir iş istasyonu paylaşılmazsa veya tek tek kullanıcı ilkeleri gerekiyorsa, kullanıcı ilkesi profillerinin atanması kullanıcıya ve aygıta atanabilir. 

## <a name="license-requirements"></a>Lisans gereksinimleri

Bu kılavuzda kapsanan kavramlar, Microsoft 365 Enterprise E5 veya eşdeğer bir SKU'ya sahip olduğunuzu varsayar. Bu kılavuzdaki önerilerden bazıları daha düşük SNU'larla uygulanabilir. Daha fazla bilgi için [Microsoft 365 Kurumsal lisanslama'ya](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise)bakın.

Lisans sağlamayı otomatikleştirmek için, kullanıcılarınız için [grup tabanlı lisanslamayı](../users-groups-roles/licensing-groups-assign.md) göz önünde bulundurun.

## <a name="azure-active-directory-configuration"></a>Azure Etkin Dizin yapılandırması

Azure Etkin Dizin (Azure AD), yönetici iş istasyonlarınız için kullanıcıları, grupları ve aygıtları yönetir. [Yönetici hesabıyla](../users-groups-roles/directory-assign-admin-roles.md)kimlik hizmetlerini ve özelliklerini etkinleştirin.

Güvenli iş istasyonu yöneticisi hesabını oluşturduğunuzda, hesabı geçerli iş istasyonunuzun karşısına çıkarırsınız. Bu ilk yapılandırmayı ve tüm genel yapılandırmayı yapmak için bilinen güvenli bir aygıt kullandığınızdan emin olun. İlk kez deneyim için saldırı maruz ilerler, [kötü amaçlı yazılım enfeksiyonları önlemek için rehberlik](/windows/security/threat-protection/intelligence/prevent-malware-infection)aşağıdaki düşünün.

En azından yöneticileriniz için çok faktörlü kimlik doğrulaması gerektirir. Uygulama kılavuzu için [bulut tabanlı MFA](../authentication/howto-mfa-getstarted.md) dağıt'a bakın.

### <a name="azure-ad-users-and-groups"></a>Azure AD kullanıcıları ve grupları

1. Azure portalından **Azure Etkin Dizin** > **Kullanıcıları** > **Yeni kullanıcıya**göz atın.
1. Kullanıcı oluşturma öğreticisindeki adımları [create user tutorial](/Intune/quickstart-create-user)izleyerek aygıt yöneticinizi oluşturun.
1. Şunları girin:

   * **Ad** - Güvenli İş İstasyonu Yöneticisi
   * **Kullanıcı adı** - `secure-ws-admin@identityitpro.com`
   * **Dizin rolü** - **Sınırlı yönetici** ve **Intune Administrator** rolünü seçin.

1. **Oluştur'u**seçin.

Ardından, iki grup oluşturursunuz: iş istasyonu kullanıcıları ve iş istasyonu aygıtları.

Azure portalından **Azure Etkin Dizin** > **Grupları** > **Yeni grubuna**göz atın.

1. İş istasyonu kullanıcıları grubu için, kullanıcılara lisans sağlamaişlemlerini otomatikleştirmek için [grup tabanlı lisanslandırmayı](../users-groups-roles/licensing-groups-assign.md) yapılandırmak isteyebilirsiniz.
1. İş istasyonu kullanıcıları grubu için şunları girin:

   * **Grup türü** - Güvenlik
   * **Grup adı** - Güvenli İş İstasyonu Kullanıcıları
   * **Üyelik türü** - Atanmış

1. Güvenli iş istasyonu yönetici kullanıcınızı ekleyin:`secure-ws-admin@identityitpro.com`
1. Güvenli iş istasyonlarını yönetecek diğer kullanıcıları ekleyebilirsiniz.
1. **Oluştur'u**seçin.
1. İş istasyonu aygıtları grubu için şunları girin:

   * **Grup türü** - Güvenlik
   * **Grup adı** - Güvenli İş İstasyonları
   * **Üyelik türü** - Atanmış

1. **Oluştur'u**seçin.

### <a name="azure-ad-device-configuration"></a>Azure AD aygıt yapılandırması

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Azure AD'ye aygıtlara kimler katılabilir

Yönetici güvenlik grubunuzun aygıtları etki alanınıza katılmasına izin vermek için Aygıtlar ayarınızı Etkin Dizini'nde yapılandırın. Bu ayarı Azure portalından yapılandırmak için:

1. Azure **Etkin Dizin** > **Aygıtları** > **Aygıt ayarlarına**gidin.
1. Kullanıcılar altında **Seçili'yi** **seçin, aygıtları Azure AD'ye katılabilir**ve ardından "Güvenli İş İstasyonu Kullanıcıları" grubunu seçebilir.

#### <a name="removal-of-local-admin-rights"></a>Yerel yönetici haklarının kaldırılması

Bu yöntem, VIP, DevOps ve güvenli düzey iş istasyonları kullanıcılarının makinelerinde yönetici haklarına sahip olmamasını gerektirir. Bu ayarı Azure portalından yapılandırmak için:

1. Azure **Etkin Dizin** > **Aygıtları** > **Aygıt ayarlarına**gidin.
1. **Azure AD'ye katılan aygıtlarda Ek yerel yöneticilerin**altında **Yok'u** seçin.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Aygıtlara katılmak için çok faktörlü kimlik doğrulama gerektirir

Aygıtları Azure AD'ye katılma işlemini daha da güçlendirmek için:

1. Azure **Etkin Dizin** > **Aygıtları** > **Aygıt ayarlarına**gidin.
1. **Aygıtlara katılmak için Çok Faktörlü Auth'un**altında **Evet'i** seçin.
1. **Kaydet'i**seçin.

#### <a name="configure-mobile-device-management"></a>Mobil cihaz yönetimini yapılandırma

Azure portalından:

1. **Azure Active Directory** > **Mobility (MDM ve MAM)** > **Microsoft Intune'a**göz atın.
1. **MDM kullanıcı kapsamı** ayarını **Tümü**olarak değiştirin.
1. **Kaydet'i**seçin.

Bu adımlar, Intune ile herhangi bir aygıtı yönetmenize olanak tanır. Daha fazla bilgi için Bkz. [Intune Quickstart: Windows 10 aygıtları için otomatik kayıt ayarlayın.](/Intune/quickstart-setup-auto-enrollment) Gelecek adımda Intune yapılandırmave uyumluluk ilkeleri oluşturursunuz.

#### <a name="azure-ad-conditional-access"></a>Azure AD Koşullu Erişim

Azure AD Koşullu Erişim, ayrıcalıklı yönetim görevlerini uyumlu aygıtlar ile sınırlamaya yardımcı olabilir. **Güvenli İş İstasyonu Kullanıcıları** grubunun önceden tanımlanmış üyelerinin bulut uygulamalarında oturum açtıklarında çok faktörlü kimlik doğrulaması gerçekleştirmeleri gerekir. En iyi yöntem, acil erişim hesaplarını ilkeden dışlamaktır. Daha fazla bilgi için Azure [AD'deki acil erişim hesaplarını yönet'e](../users-groups-roles/directory-emergency-access.md)bakın.

## <a name="intune-configuration"></a>Intune yapılandırma

### <a name="configure-enrollment-status"></a>Kayıt durumunu yapılandırma

Güvenli iş istasyonunuzun güvenilir bir temiz cihaz olduğundan emin olmak önemlidir. Yeni aygıtlar satın alırken, tedarik zinciri yönetimi sırasında güvenlik açıklarına maruz kalmanızı sınırlayan [S modunda Windows 10 Pro'ya](/Windows/deployment/Windows-10-pro-in-s-mode)fabrika olarak ayarlandığında ısrar edebilirsiniz. Tedarikçinizden bir cihaz aldıktan sonra, Otomatik Pilot'u S modundan değiştirmek için kullanabilirsiniz. Aşağıdaki kılavuz, dönüşüm sürecinin uygulanmasıyla ilgili ayrıntıları sağlar.

Aygıtların kullanılmadan önce tam olarak yapılandırıldığından emin olmak için Intune, **tüm uygulamalar ve profiller yüklenene kadar aygıt kullanımını engellemek**için bir araç sağlar.

Azure **portalından:**

1. Microsoft **Intune** > **Device kaydı** > **Windows kayıt** > **Durum Sayfası** > **Varsayılan** > **Ayarları'na**gidin.
1. Ayar Uygulama profili yükleme ilerlemesini **Evet'e** **göster.**
1. Tüm uygulamalar ve profiller **Evet'e** **yüklenene kadar aygıtı engelleyin.**

### <a name="create-an-autopilot-deployment-profile"></a>Bir Autopilot dağıtım profili oluşturma

Bir aygıt grubu oluşturduktan sonra, Otomatik Pilot aygıtlarını yapılandırmak için bir dağıtım profili oluşturmanız gerekir.

Azure portalında Intune'da:

1. **Aygıt kaydı** > nı seçin**Windows kaydı** > **Dağıtım Profilleri** > **Profil Oluştur.**
1. Şunları girin:

   * Ad - **Güvenli iş istasyonu dağıtım profili.**
   * Açıklama - **Güvenli iş istasyonlarının dağıtımı.**
   * **Tüm hedeflenen cihazları Autopilot’a dönüştür** ayarını **Evet** olarak seçin. Bu ayar, listedeki tüm cihazların Autopilot dağıtım hizmetine kaydolmasını sağlar. Kaydın işlenmesi için 48 saat kadar bekleyin.

1. **Sonraki'ni**seçin.

   * **Dağıtım modu için**Kendi Kendine Dağıtma **(Önizleme)** seçeneğini belirleyin. Bu profile sahip aygıtlar, aygıtı kaydeden kullanıcıyla ilişkilidir. Cihazı kaydetmek için kullanıcı kimlik bilgileri gerekir. Kendi **Kendine Dağıtma** modunda bir aygıtı dağıtmanın dizüstü bilgisayarları paylaşılan bir modelde dağıtmanıza olanak sağlayacağını unutmayın. Aygıt bir kullanıcıya ilk kez atanana kadar kullanıcı ataması yapılmaz. Sonuç olarak, bitlocker gibi kullanıcı ilkeleri, kullanıcı ataması tamamlanana kadar etkinleştirilmez. Güvenli bir aygıtta nasıl oturum açAbilirsiniz hakkında daha fazla bilgi için [seçili profilleri](/intune/device-profile-assign)görün.
   * Kutu **olarak Azure AD'ye Katıl,** **Azure AD'nin katıldığını** ve gri renkte olduğunu göstermeli.
   * Dilinizi (Bölge), Kullanıcı hesabı türü **standardınızı**seçin. 

1. **Sonraki'ni**seçin.

   * Önceden yapılandırıldıysanız bir kapsam etiketi seçin.

1. **Sonraki'ni**seçin.
1.  > **Seçili** **Gruplara** > **Atama**Atamaları seçin. **Dahil etmek için Seç gruplarında,** Güvenli İş **İstasyonları'nı**seçin.
1. **Sonraki'ni**seçin.
1. Profili oluşturmak için **Oluştur**’u seçin. Autopilot dağıtım profili artık cihazlara atanmak üzere hazırdır.

Otomatik Pilot'a cihaz kaydı, aygıt türüne ve rolüne göre farklı bir kullanıcı deneyimi sağlar. Dağıtım örneğimizde, güvenli aygıtların toplu olarak dağıtıldığı ve paylaşılabildiği, ancak ilk kez kullanıldığında aygıtın bir kullanıcıya atandığı bir model gösteriyoruz. Daha fazla bilgi için [Intune Autopilot aygıt kaydına](/intune/device-enrollment)bakın.

### <a name="configure-windows-update"></a>Windows Update'i yapılandırma

Windows 10'u güncel tutmak yapabileceğiniz en önemli şeylerden biridir. Windows'u güvenli bir durumda tutmak için, güncelleştirmelerin iş istasyonlarına uygulanma hızını yönetmek için bir güncelleştirme halkası dağıtırsınız. 

Bu kılavuz, yeni bir güncelleştirme halkası oluşturmanızı ve aşağıdaki varsayılan ayarları değiştirmenizi önerir:

Azure portalında:

1. Microsoft **Intune** > **Software güncelleştirmeleri** > **Windows 10 Update Rings**gidin.
1. Şunları girin:

   * Ad - **Azure tarafından yönetilen iş istasyonu güncelleştirmeleri**
   * Servis kanalı - **Windows Insider - Hızlı**
   * Kalite güncelleştirme erteleme (gün) - **3**
   * Özellik güncelleştirme erteleme süresi (gün) - **3**
   * Otomatik güncelleştirme davranışı - **Son kullanıcı denetimi olmadan otomatik yükleme ve yeniden başlatma**
   * Kullanıcının Windows güncelleştirmelerini duraklatmalarını engelleme - **Engelleme**
   * Çalışma saatleri dışında yeniden başlatmak için kullanıcının onayını gerektirme - **Gerekli**
   * Kullanıcının yeniden başlatmasına izin ver (meşgul yeniden başlatma) - **Gerekli**
   * Kullanıcıları otomatik yeniden başlatmadan sonra devreye sokmaya geçiş (gün) - **3**
   * Meşgul yeniden başlatma anımsatıcılarını erteleme (gün) - **3**
   * Bekleyen yeniden başlatmalar için son tarihi ayarlama (gün) - **3**

1. **Oluştur'u**seçin.
1. **Atamalar** sekmesine Güvenli **İş İstasyonları** grubunu ekleyin.

Windows Update ilkeleri hakkında daha fazla bilgi için [İlke CSP - Update'e](/windows/client-management/mdm/policy-csp-update)bakın.

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP Intune tümleştirmesi

Windows Defender ATP ve Microsoft Intune, güvenlik ihlallerini önlemeye yardımcı olmak için birlikte çalışır. Ayrıca ihlallerin etkisini sınırlayabilirler. ATP yetenekleri gerçek zamanlı tehdit algılama sağlamanın yanı sıra son nokta aygıtlarının kapsamlı denetim ve günlüğe kaydedilmesini sağlar.

Windows Defender ATP ve Intune'un tümleştirmesini yapılandırmak için Azure portalına gidin.

1. **Microsoft Intune** > **Aygıt uyumluluğu** > **Windows Defender ATP'ye**göz atın.
1. Windows Defender **ATP Yapılandırma**altında adım 1, **Windows Defender GÜVENLIK Merkezi'nde Microsoft Intune Windows Defender ATP bağlayın**seçin.
1. Windows Defender Güvenlik Merkezi’nde:

   1. **Gelişmiş Ayarlar** > **özelliklerini**seçin.
   1. **Microsoft Intune bağlantısı** **için, On'u**seçin.
   1. **Tercihleri kaydet**’i seçin.

1. Bağlantı kurulduktan sonra, Intune'a dönün ve en üstte **Yenile'yi** seçin.
1. **10.0.15063 ve üstü sürümü Windows cihazları Windows Defender ATP bağlayıcısına bağla** ayarını **Açık** olarak ayarlayın.
1. **Kaydet'i**seçin.

Daha fazla bilgi için [Windows Defender Gelişmiş Tehdit Koruması'na](/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection)bakın.

### <a name="finish-workstation-profile-hardening"></a>İş istasyonu profil sertleştirmeyi bitirin

Çözümün sertlemasını başarıyla tamamlamak için, uygun komut dosyasını indirin ve çalıştırın. İstediğiniz **profil seviyesi**için indirme linklerini bulun:

| Profil | İndirme konumu | Dosyaadı |
| --- | --- | --- |
| Düşük Güvenlik | Yok | Yok |
| Gelişmiş Güvenlik | https://aka.ms/securedworkstationgit | Geliştirilmiş-İş İstasyonu-Windows10-(1809).ps1 |
| Yüksek Güvenlik | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Özelleştirilmiş | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Özel Uyumluluk* | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Güvenli | https://aka.ms/securedworkstationgit | Güvenli-İş İstasyonu-Windows10-(1809)-SecurityBaseline.ps1 |

\*Özelleştirilmiş Uyumluluk, NCSC Windows10 SecurityBaseline'da sağlanan Özelleştirilmiş yapılandırmayı zorlayan bir komut dosyasıdır.

Komut dosyası başarıyla yürütüldünden sonra, Intune'daki profiller ve ilkeler için güncelleştirmeler yapabilirsiniz. Gelişmiş ve Güvenli profiller için komut dosyaları sizin için ilkeler ve profiller oluşturur, ancak ilkeyi **Güvenli İş İstasyonları** aygıt grubunuza atamanız gerekir.

* Komut dosyaları tarafından oluşturulan Intune aygıt yapılandırma profillerini burada bulabilirsiniz: **Azure portalı** > **Microsoft Intune** > **Device yapılandırma** > **Profilleri.**
* Komut dosyaları tarafından oluşturulan Intune aygıt uyumluluk ilkelerini burada bulabilirsiniz: **Azure portalı** > **Microsoft Intune** > **Aygıt Uyumluluk** > **İlkeleri.**

Komut dosyaları tarafından yapılan değişiklikleri gözden geçirmek için profilleri dışa aktarabilirsiniz. Bu [şekilde, SECCON belgelerinde](/windows/security/threat-protection/windows-security-configuration-framework/windows-security-configuration-framework)belirtildiği gibi gerekli olabilecek ek sertleştirmeyi belirleyebilirsiniz.

Tüm geçerli Intune `DeviceConfiguration_Export.ps1` profillerini dışa aktarmak için [DeviceConfiguration GiuHub deposundan](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) Intune veri dışa aktarma komut dosyasını çalıştırın.

## <a name="additional-configurations-and-hardening-to-consider"></a>Ek yapılandırmalar ve sertleştirme dikkate

Buradaki kılavuzu izleyerek, güvenli bir iş istasyonu dağıttınız. Ancak, ek denetimleri de göz önünde bulundurmanız gerekir. Örnek:

* alternatif tarayıcılara erişimi sınırlandırın
* giden HTTP izin
* belirli web sitelerini engelleme
* özel PowerShell komut dosyalarını çalıştırmak için izinleri ayarlama

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Güvenlik duvarı yapılandırma hizmeti sağlayıcısında (CSP) kuralları ayarlama

İzin verilen ve engellenen uç noktalarınız için gerektiğinde hem gelen hem de giden kuralların yönetiminde ek değişiklikler yapabilirsiniz. Güvenli iş istasyonunu sertleştirmeye devam ettikçe, gelen ve giden tüm trafiği reddeden kısıtlamayı gevşetebilirsiniz. Ortak ve güvenilir web sitelerini eklemek için izin verilen giden siteleri ekleyebilirsiniz. Daha fazla bilgi için [Bkz. Güvenlik Duvarı yapılandırma hizmeti.](/Windows/client-management/mdm/firewall-csp)

Kısıtlayıcı URL trafik yönetimi şunları içerir:

* Tüm gelen trafiği reddet - Güvenli iş istasyonu profil komut dosyasında ayarlayın.
* Azure Bulut Shell ve Azure Parola Sıfırlama özelliği de dahil olmak üzere seçili Azure ve Microsoft hizmetleri dışında tüm giden tüm giden leri reddedin.

```
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"ProxyEnable"=dword:00000001
"ProxyServer"="127.0.0.1:80"
"ProxyOverride"="*.azure.com;*.azure.net;*.microsoft.com;*.windowsupdate.com;*.microsoftonline.com;*.microsoftonline.cn;*.windows.net;*.windowsazure.com;*.windowsazure.cn;*.azure.cn;*.loganalytics.io;*.applicationinsights.io;*.vsassets.io;*.azure-automation.net;*.visualstudio.com,portal.office.com;*.aspnetcdn.com;*.sharepointonline.com;*.msecnd.net;*.msocdn.com;*.webtrends.com"
"AutoDetect"=dword:00000000
```

Engelleme kurallarınız için daha ayrıntılı bilgi sağlamak istiyorsanız, [Etki Alanı Engelleme Listesi'ni (DBL)](https://www.spamhaus.org/dbl/)koruyan Spamhaus Projesi'ne başvurabilirsiniz: siteleri engellemek için uygulanacak gelişmiş bir kural kümesi olarak kullanmak üzere iyi bir kaynak.

### <a name="manage-local-applications"></a>Yerel uygulamaları yönetme

Güvenli iş istasyonu, üretkenlik uygulamaları da dahil olmak üzere yerel uygulamalar kaldırıldığında gerçekten sertleştirilmiş bir duruma geçer. Burada, Chrome'u varsayılan tarayıcı olarak ekler ve kullanıcının tarayıcıyı ve eklentilerini değiştirme yeteneğini sınırlamak için Intune'u kullanırsınız.

#### <a name="deploy-applications-using-intune"></a>Uygulamaları Intune'u kullanarak dağıtma

Bazı durumlarda, google chrome tarayıcısı gibi uygulamalar güvenli iş istasyonunda gereklidir. Aşağıdaki örnek, Güvenli **İş İstasyonları**güvenlik grubundaki aygıtlara Chrome yüklemek için yönergeler sağlar.

1. [Windows 64-bit için](https://cloud.google.com/chrome-enterprise/browser/download/)çevrimdışı yükleyici Chrome paketini indirin.
1. Dosyaları ayıklayın ve dosyanın konumunu `GoogleChromeStandaloneEnterprise64.msi` not edin.
1. Azure **portalında** Microsoft **Intune** > **Client uygulamalarına** > göz atın**Apps** > **Ekle**.
1. **Uygulama türü altında,** **Line-of-business'ı**seçin.
1. **Uygulama paketi dosyasının** `GoogleChromeStandaloneEnterprise64.msi` altında, çıkarılan konumdan dosyayı seçin ve **Tamam'ı**seçin.
1. **Uygulama bilgileri**altında, bir açıklama ve bir yayıncı sağlar. **Tamam'ı**seçin.
1. **Ekle'yi**seçin.
1. Atamalar sekmesinde, **Atama türü**altında kayıtlı **aygıtlar** **için Kullanılabilir'i** seçin.
1. **Dahil Edilen Gruplar**altında Güvenli İş **İstasyonları** grubunu ekleyin.
1. **Tamam'ı**seçin ve ardından **Kaydet'i**seçin.

Chrome ayarlarını yapılandırma hakkında daha fazla bilgi için Bkz. [Microsoft Intune ile Chrome Tarayıcısı'nı Yönet.](https://support.google.com/chrome/a/answer/9102677)

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Özel uygulamalar için şirket portalını yapılandırma

Güvenli bir modda, uygulama yüklemesi Intune şirket portalı ile sınırlıdır. Ancak, portalı yüklemek için Microsoft Mağazası'na erişim gerekiyor. Güvenli çözümünüzde, şirket portalını çevrimdışı mod aracılığıyla tüm aygıtlar için kullanılabilir hale getirebilirsiniz.

[Şirket Portalı'nın](/Intune/store-apps-company-portal-app) Intune tarafından yönetilen bir kopyası, güvenli iş istasyonlarının kullanıcılarına iletebileceğiniz ek araçlara isteğe bağlı erişim sağlar.

Dağıtım özel hazırlıklar gerektiren Windows 32 bit uygulamaları veya diğer uygulamaları yüklemeniz gerekebilir. Bu gibi durumlarda, [Microsoft win32 içerik hazırlama aracı](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) yükleme `.intunewin` için kullanıma hazır biçim dosyası sağlayabilir.

### <a name="conditional-access-only-allowing-secured-workstation-ability-to-access-azure-portal"></a>Koşullu Erişim yalnızca güvenli iş istasyonu olanağının Azure portalına erişmesine izin verir

Azure AD, Azure bulut yönetimi portalınıza kimlerin ve nelerin erişebileceğini yönetme ve kısıtlama olanağı sunar. [Koşullu Erişimi](../conditional-access/overview.md) etkinleştirmek, kaynakları yalnızca güvenli iş istasyonunuzun yönetebileceğini veya değiştirebileceğini garanti eder. [Acil durum erişim](../users-groups-roles/directory-emergency-access.md) işlevselliği yalnızca aşırı durumlarda ve ilke yoluyla yönetilen hesap için kullanılabiliyorsa veya kullanılması gerekiyorsa, bu özelliği dağıtırken göz önünde bulundurmanız çok önemlidir.

> [!NOTE]
> Bir kullanıcı grubu oluşturmanız ve Koşullu Erişim ilkesini atlayabilecek acil durum kullanıcınızı eklemeniz gerekir. Örneğin Acil **Durum BreakGlass** adında bir güvenlik grubumuz var.

1. **Azure portalımicrosoft** > **Intune** > **Koşullu Erişim göz** > atın - İlkeler**Yeni İlke**.
1. İlke için bir **Ad** sağlayın.
1. **Kullanıcı ve Grupları** > **Seçin Kullanıcıları ve grupları seçin** 
1. **Dizini** **Ekle** > > Genel Yönetici, Ayrıcalıklı Rol Yöneticisi, Ayrıcalıklı Kimlik Doğrulama Yöneticisi, Güvenlik Yöneticisi, Uyumluluk Yöneticisi, Koşullu Erişim Yöneticisi, Uygulama Yöneticisi, Bulut Uygulama Yöneticisi, Hizmet Yöneticisi > rolleri seçin
1. Kullanıcıları Ve **grupları** seç > Dışla'yı seçin > **Hariç Kullanıcıları Seçin** > Acil Durum **BreakGlass** grubuseçin. **Exclude**
1. **Bulut uygulamalarını veya eylemlerini** seçin > Tüm bulut **uygulamalarını** seçin
1. **Koşulları** Seçin > **Aygıt Platformlarını** Seçin > **Evet'i** seçin > Select **Aygıt platformları** **Windows'u** seçin
1. **Access denetimleri** > **Seç Ver Access** **Evet** > **Aygıtın uyumlu olarak işaretlemesi için seç.** 
1. **Politikayı Etkinleştir'i** > **On** Seçin
 
Bu ilke kümesi, Yöneticilerinizin Intune ve WDATP tarafından ayarlanan uyumlu bir Windows aygıtı kullanmasını sağlar. 

Kuruluşlar, ortamlarındaki eski kimlik doğrulama protokollerini engellemeyi de düşünmelidir. Bu görevi gerçekleştirmenin birden çok yolu vardır, eski kimlik doğrulama protokollerini engelleme hakkında daha fazla bilgi için makaleye bakın, [Nasıl Yapılır: Koşullu Erişim ile Azure AD'ye eski kimlik doğrulamasını engelleyin.](../conditional-access/block-legacy-authentication.md)

### <a name="use-powershell-to-create-custom-settings"></a>Özel ayarlar oluşturmak için PowerShell'i kullanın

PowerShell'i ev sahibi yönetim özelliklerini genişletmek için de kullanabilirsiniz. Bu örnek komut dosyası, ana bilgisayarda varsayılan bir arka plan ayarlar. Azure portalı ve profilleri aracılığıyla da kullanılabilen bir özelliktir. Örnek komut dosyası yalnızca PowerShell işlevselliğini göstermek için hizmet vermektedir.

Güvenli iş istasyonlarınızda bazı özel denetimler ve ayarlar ayarlamanız gerekebilir. Bu örnek, Powershell'in cihazı kullanıma hazır ve güvenli bir iş istasyonu olarak kolayca tanımlama yeteneğini kullanarak iş istasyonunun arka planını değiştirir.

Microsoft Scripting Center'ın [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) komut dosyası, Windows'un bu [ücretsiz genel arka plan görüntüsünü](https://i.imgur.com/OAJ28zO.png) başlangıç üzerine yüklemesine olanak tanır.

1. Komut dosyasını yerel bir aygıta indirin.
1. MüşteriXXXX'i ve arka plan görüntüsünün indirme konumunu güncelleştirin. Örneğimizde, customerXXXX'i arka planlara değiştiririz.
1. **Azure portalına** > göz atın**Microsoft Intune** > **Device yapılandırmaSı** > **PowerShell komut dosyaları** > **Ekle**.
1. Komut dosyası için bir **Ad** sağlayın ve **Komut Dosyası konumunu**belirtin.
1. **Yapılandır**'ı seçin.
   1. Ayar **Bu komut dosyasını, oturum açmış kimlik bilgilerini kullanarak** **Evet'e**çalıştırın.
   1. **Tamam'ı**seçin.
1. **Oluştur'u**seçin.
1. **Atamalar'ı** > **seçin Grupları seçin.**
   1. Güvenlik grubunu güvenli **çalışma istasyonları**ekleyin.
   1. **Kaydet'i**seçin.

## <a name="enroll-and-validate-your-first-device"></a>İlk cihazınızı kaydedin ve doğrulayın

1. Cihazınızı kaydettirmek için aşağıdaki bilgilere ihtiyacınız vardır:
   * **Seri numarası** - cihaz kasası üzerinde bulundu.
   * **Windows Ürün Kimliği** - Windows Ayarları menüsünden **System** > **About** altında bulunur.
   * Cihaz kaydı için gerekli tüm bilgileri içeren bir CSV karma dosyası almak için [Get-WindowsAutoPilotInfo'yu](https://aka.ms/Autopilotshell) çalıştırabilirsiniz.
   
     Bilgileri `Get-WindowsAutoPilotInfo – outputfile device1.csv` Intune'a aktarabileceğiniz bir CSV dosyası olarak çıktıalmak için çalıştırın.

     > [!NOTE]
     > Komut dosyası yüksek haklar gerektirir. Uzaktan imzalı olarak çalışır. Komut, `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` komut dosyasının doğru çalışmasını sağlar.

   * Bu bilgileri, Windows 10 sürüm 1809 veya daha yüksek bir aygıtta oturum oluşturarak toplayabilirsiniz. Donanım satıcınız da bu bilgileri sağlayabilir.
1. Azure **portalında,** **Microsoft Intune** > **Device kayıt** > **Windows kayıt** > **Aygıtları - Windows Otomatik Pilot aygıtlarını yönet'** e gidin.
1. **İçe Aktar'ı** seçin ve CSV dosyanızı seçin.
1. Aygıtı **Güvenli İş İstasyonları** güvenlik grubuna ekleyin.
1. Yapılandırmak istediğiniz Windows 10 aygıtında, **Windows Settings** > **Update & Security** > **Recovery'e**gidin.
   1. Bu bilgisayarı **Sıfırla'nın**altında **başlatın'ı** seçin.
   1. Aygıtı, yapılandırılan profil ve uyumluluk ilkeleriyle sıfırlamak ve yeniden yapılandırmak için istemleri izleyin.

Aygıtı yapılandırıldıktan sonra bir inceleme tamamlayın ve yapılandırmayı denetleyin. Dağıtımınıza devam etmeden önce ilk aygıtın doğru şekilde yapılandırıldığından onaylayın.

## <a name="assign-devices"></a>Aygıtları atama

Aygıtları ve kullanıcıları atamak [için, seçili profilleri](/intune/device-profile-assign) güvenlik grubunuzun eşlemi yapmanız gerekir. Hizmet için izin isteyen tüm yeni kullanıcıların güvenlik grubuna da eklenmesi gerekir.

## <a name="using-sentinel-and-windows-defender-atp-to-monitor-and-respond-to-security-incidents"></a>Güvenlik olaylarını izlemek ve yanıtlamak için Sentinel ve Windows Defender ATP'yi kullanma

Güvenli iş istasyonu dağıtımının izlenmesi [Sentinel]'i etkinleştirerek ve [Tehdit ve Güvenlik Açığı Yönetimini](/windows/security/threat-protection/microsoft-defender-atp/next-gen-threat-and-vuln-mgt) kullanarak kılavuz kapsamlı tehdit avcılığı sağlamaz, ancak olası güvenlik olaylarını izlemek ve bu olaylara yanıt vermek için sağduyulu çalışmalar sağlar.

**Azure Sentinel'i** şu şekilde kullanacağız: 

* Kullanıcı ve aygıt izleme için Intune, Azure portalı ve Azure AD'den veri toplama
* Kullanıcı ve aygıt yapılandırmaşüpheli etkinliğini araştırmaya yardımcı olun
* VE WDATP kullanarak güvenlik soruşturmaları keşfetmek için yeteneği sürücü

Sentinel izleme, Azure AD gibi veri kaynaklarınıza bağlanan ların ayarlanmasını gerektirir.

1. Azure **portalında**Azure **Sentinel (Önizleme)** > **Ekle'yi** Seç'e gidin
1. Azure **Sentinel'e eklemek için çalışma alanı** **seçin'de yeni bir çalışma alanı oluşturun**
1. Şunları girin:
   * **Log Analytics Çalışma Alanı** - 'Güvenli İş İstasyonu İzleme'
   * **Abonelik** - Etkin aboneliğinizi seçin
   * **Kaynak Grubu** - ** Yeni Oluştur** > Güvenli İş İstasyonu RG > **Ok'u** seçin
   * **Konum** - Dağıtımınız için coğrafi olarak en uygun konumu seçin
   * **Fiyat Katmanı** - GB Başına Seç **(2018)**
1. **Tamam**’ı seçin.

Daha sonra mevcut güvenli iş istasyonu veri kaynaklarını izlemeye bağlayacaktır.

1. Azure **portalında,** **Güvenli İş İstasyonu İzleme** çalışma alanını seçin > Azure Sentinel çalışma **alanına** gidin
1. **Veri Bağlayıcılarını** Seçin
1. Azure **Etkin Dizini** > Açık Bağlayıcı Sayfası > Ön koşulu inceledikten sonra seçin. Yapılandırma'ya devam edin ve hem Azure REKLAM Oturum Açma Günlükleri hem de Azure REKLAM Denetim Günlükleri için **Bağlan'ı** seçin.
1. Ön koşulu inceledikten sonra **Azure Etkinliği** > Açık Bağlayıcı Sayfasını > seçin. Azure Etkinlik Günlüklerini Yapılandırmaya devam > Aboneliğinizi seçin > **Bağlan'ı** seçin

Sentinel tarafından toplanan verilerden **Azure portalLarını**seçerek etkinliği gözlemleyebilirsiniz , **Azure Sentinel Genel Bakış'a** gidin 

**Windows Defender ATP'yi (WDATP)** şu şekilde kullanacağız:

* Güvenlik açıklarını ve yanlış yapılandırmaları sürekli gözlemlemek ve izlemek
* Vahşi doğada dinamik tehditlere öncelik vermek için WDATP'den yararlanın
* Güvenlik açıklarının uç nokta algılama ve yanıt (EDR) uyarılarıyla ilişkibirliği
* İncelemeler sırasında makine düzeyinde güvenlik açığını belirlemek için panoyu kullanın
* Düzeltmeleri Intune'a itin

[Defender ATP panonuzu](https://securitycenter.windows.com/machines)yapılandırın. [Tehdit & Güvenlik Açığı Yönetimi panosuna genel bakışkılavuzunu](/windows/security/threat-protection/microsoft-defender-atp/tvm-dashboard-insights)kullanma.

## <a name="monitoring-application-activity-using-microsoft-monitoring-agent-mma"></a>Microsoft Monitoring Agent (MMA) kullanarak Uygulama etkinliğini izleme
Özel iş istasyonundan başlayarak, uygulama dolabı bir iş istasyonundaki uygulama etkinliğinin izlenmesi için etkinleştirilir. İzlemenin Log Analytics çalışma alanınıza entegre edilebilmek için bir MMA aracısı ve yapılandırması izlenmelidir. 

> [!NOTE]
> Özelleştirilmiş iş istasyonu profili AppLocker izleme ilkelerini içerir. İlkelerin dağıtımı, bir istemcide uygulama etkinliğinin izlenmesi için gereklidir

Intune PowerShell komut dosyasıyla MMA aracısını dağıtma

1. Kurulum [komut dosyasını yerel bir aygıta](https://aka.ms/securedworkstationgit)indirin.
1. Parametreleri, **$WorkSpaceID** ve **$WorkSpaceKey** güncelleştirin
1. **Azure portalına** > göz atın**Microsoft Intune** > **Device yapılandırmaSı** > **PowerShell komut dosyaları** > **Ekle**.
1. Komut dosyası için bir **Ad** sağlayın ve **Komut Dosyası konumunu**belirtin.
1. **Yapılandır**'ı seçin.
   1. Ayar **Bu komut dosyasını, oturum açmış kimlik bilgilerini kullanarak** **Evet'e**çalıştırın.
   1. **Tamam'ı**seçin.
1. **Oluştur'u**seçin.
1. **Atamalar'ı** > **seçin Grupları seçin.**
   1. Güvenlik grubunu güvenli **çalışma istasyonları**ekleyin.
   1. **Kaydet'i**seçin.

Daha sonra yeni günlükleri almak için Log Analytics'i ayarlamanız gerekir
1. Azure **portalında,**'Güvenli İş İstasyonu İzleme' > **Log Analytics Çalışma Alanına** gidin
1. **Gelişmiş ayarlar** > **Veri** > **Windows Olay Günlükleri** seçin
1. **Aşağıdaki olay günlüklerinden olayları topla** 
1. Şunları girin:
   * 'Microsoft-Windows-AppLocker/EXE ve DLL' > Unselect **Informational**
   * 'Microsoft-Windows-AppLocker/MSI ve Script' unselect **Informational** >
   * 'Microsoft-Windows-AppLocker/Packaged app-Deployment' > Unselect **Informational**
   * 'Microsoft-Windows-AppLocker/Packaged app-Execution' > Unselect **Informational**
1. **Kaydet**’i seçin

Uygulama günlüğü seçtiğiniz Log Analytics çalışma alanında kullanılabilir.

## <a name="monitoring"></a>İzleme

* [Azure Sentinel ile tehditleri](/azure/sentinel/tutorial-detect-threats) nasıl algılayın öğrenin
* [Azure Sentinel ile ilgili olayları araştırma](/azure/sentinel/tutorial-investigate-cases)
* [Azure Sentinel'de otomatik tehdit yanıtları ayarlama](/azure/sentinel/tutorial-respond-threats-playbook)
* [Pozlama Puanınızı](/windows/security/threat-protection/microsoft-defender-atp/tvm-exposure-score) nasıl inceleyeniz gerektiğini anlama
* Güvenlik önerisini gözden [geçirin](/windows/security/threat-protection/microsoft-defender-atp/tvm-security-recommendation)
* Güvenlik [Düzeltmelerini](/windows/security/threat-protection/microsoft-defender-atp/tvm-remediation) yönetme
* [Uç nokta algılama ve yanıtı](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response) yönetme
* [Intune profil izleme](/intune/device-profile-monitor)ile profilleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Intune](/intune/index)hakkında daha fazla bilgi edinin.
* [Azure REKLAM'ı](../index.yml)anlayın.
* Microsoft [Defender Gelişmiş Tehdit Koruması](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) ile çalışma
* [Azure Sentinel'i](/azure/sentinel/) Keşfedin
