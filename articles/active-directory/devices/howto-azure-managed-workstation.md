---
title: Azure tarafından yönetilen iş istasyonlarını dağıtma-Azure Active Directory
description: Yanlış yapılandırma veya uzlaşmadan dolayı ihlal riskini azaltmak için güvenli, Azure tarafından yönetilen iş istasyonları dağıtmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5fe1bf294c34afc2f7e0e0aa911dc05597ab9df
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85252789"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Güvenli, Azure tarafından yönetilen bir iş istasyonu dağıtma

Artık [güvenli iş istasyonlarını anladığınıza](concept-azure-managed-workstation.md)göre, dağıtım işleminin başlaması zaman atalım. Bu kılavuzda, başlangıçtan daha güvenli bir iş istasyonu oluşturmak için tanımlı profilleri kullanın.

![Güvenli bir iş istasyonunun dağıtımı](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Çözümü dağıtabilmeniz için önce bir profil seçin. Birden çok profili dağıtımda eşzamanlı olarak kullanabilir ve Etiketler ya da gruplarla atayabilirsiniz.

> [!NOTE]
> Gereksinimlerinizin gerektirdiği her türlü profili uygulayın. Microsoft Intune atayarak başka bir profile geçebilirsiniz.

| Profil | Düşük | Gelişmiş | Yüksek | Özelleştirilmiş | Korunmalıdır | Yalıtılmış |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Azure AD 'de Kullanıcı | Yes | Yes | Yes | Yes | Yes | Yes |
| Intune tarafından yönetilen | Yes | Yes | Yes | Yes | Yes | Yes |
| Cihaz-Azure AD kayıtlı | Yes |  |  |  |  | |   |
| Cihaz-Azure AD 'ye katılmış |   | Yes | Yes | Yes | Yes | Yes |
| Intune güvenlik temeli uygulandı |   | Yes <br> Leştirilmiş | Yes <br> (HighSecurity) | Yes <br> (NCSC) | Yes <br> Korunmalıdır | NA |
| Donanım, güvenli Windows 10 standartlarına karşılıyor |   | Yes | Yes | Yes | Yes | Yes |
| Microsoft Defender ATP etkin |   | Yes  | Yes | Yes | Yes | Yes |
| Yönetici haklarının kaldırılması |   |   | Yes  | Yes | Yes | Yes |
| Microsoft Autopilot kullanarak dağıtım |   |   | Yes  | Yes | Yes | Yes |
| Yalnızca Intune tarafından yüklenen uygulamalar |   |   |   | Yes | Yes |Yes |
| Onaylanan listeyle kısıtlanan URL 'Ler |   |   |   | Yes | Yes |Yes |
| Internet engellendi (gelen/giden) |   |   |   |  |  |Yes |

> [!NOTE]
> Güvenli iş istasyonu Kılavuzu **cihazlarında** profiller ve ilkeler atanır. Kullanıcılara doğrudan ilke uygulanmayacak, cihaz paylaşımının (paylaşılan cihazlar) etkin olmasını sağlar. Güvenli bir iş istasyonu dağıtımınızda paylaşılmadığı veya ayrı kullanıcı ilkeleri gerekliyse, Kullanıcı ilkesi profillerinin atanması Kullanıcı ve cihaza atanabilir. 

## <a name="license-requirements"></a>Lisans gereksinimleri

Bu kılavuzda ele alınan kavramlar, Microsoft 365 Kurumsal E5 veya denk bir SKU olduğunu varsayar. Bu kılavuzdaki önerilerden bazıları, daha düşük SKU 'Lar ile uygulanabilir. Daha fazla bilgi için bkz. [Microsoft 365 Kurumsal lisanslama](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Lisans sağlamayı otomatik hale getirmek için kullanıcılarınız için [grup tabanlı lisanslamayı](../users-groups-roles/licensing-groups-assign.md) göz önünde bulundurun.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory yapılandırması

Azure Active Directory (Azure AD), yönetici iş istasyonlarınız için kullanıcıları, grupları ve aygıtları yönetir. [Yönetici hesabıyla](../users-groups-roles/directory-assign-admin-roles.md)kimlik hizmetlerini ve özellikleri etkinleştirin.

Güvenli iş istasyonu yönetici hesabını oluşturduğunuzda, hesabı geçerli iş istasyonunuzda kullanıma sunun. Bu ilk yapılandırmayı ve tüm genel yapılandırmayı yapmak için bilinen bir güvenli cihaz kullandığınızdan emin olun. İlk seferlik deneyimin saldırıya maruz kalma olasılığını azaltmak için, [kötü amaçlı yazılımdan bulaşmaları önleme kılavuzunu](/windows/security/threat-protection/intelligence/prevent-malware-infection)takip etmeyi göz önünde bulundurun.

En azından yöneticileriniz için çok faktörlü kimlik doğrulaması gerektir. Bkz. Uygulama Kılavuzu için [bulut tabanlı MFA dağıtma](../authentication/howto-mfa-getstarted.md) .

### <a name="azure-ad-users-and-groups"></a>Azure AD kullanıcıları ve grupları

1. Azure Portal, **Azure Active Directory**  >  **kullanıcılara**  >  **Yeni Kullanıcı**Azure Active Directory gidin.
1. [Kullanıcı oluşturma öğreticisindeki](/Intune/quickstart-create-user)adımları izleyerek cihaz yöneticinizi oluşturun.
1. Şunları girin:

   * **Ad** -güvenli Iş istasyonu Yöneticisi
   * **Kullanıcı adı** - `secure-ws-admin@identityitpro.com`
   * **Dizin rolü**  -  **Sınırlı yönetici** ve **Intune yönetici** rolünü seçin.

1. **Oluştur**'u seçin.

Sonra iki grup oluşturursunuz: iş istasyonu kullanıcıları ve iş istasyonu cihazları.

Azure Portal **Azure Active Directory**  >  **gruplar**  >  **Yeni Grup**' a gidin.

1. İş istasyonu kullanıcıları grubu için, kullanıcılara lisansların sağlanması otomatik hale getirmek üzere [grup tabanlı lisanslama](../users-groups-roles/licensing-groups-assign.md) yapılandırmak isteyebilirsiniz.
1. İş istasyonu kullanıcıları grubu için şunu girin:

   * **Grup türü** -güvenlik
   * **Grup adı** -güvenli Iş istasyonu kullanıcıları
   * **Üyelik türü** -atandı

1. Güvenli iş istasyonu yönetici kullanıcısını ekleyin:`secure-ws-admin@identityitpro.com`
1. Güvenli iş istasyonlarını yönetmek için diğer kullanıcıları ekleyebilirsiniz.
1. **Oluştur**'u seçin.
1. İş istasyonu cihazları grubu için şunu girin:

   * **Grup türü** -güvenlik
   * **Grup adı** -güvenli iş istasyonları
   * **Üyelik türü** -atandı

1. **Oluştur**'u seçin.

### <a name="azure-ad-device-configuration"></a>Azure AD cihaz yapılandırması

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Cihazları Azure AD 'ye kimlerin katılabileceği belirtin

Yönetim güvenlik grubunuzun cihazları etki alanınıza katılmasına izin vermek için Active Directory cihaz ayarınızı yapılandırın. Bu ayarı Azure portal yapılandırmak için:

1. **Azure Active Directory**  >  **cihazlar**  >  **cihaz ayarları**' na gidin.
1. **Kullanıcılar cihazları Azure AD 'ye katabilir**ve ardından "güvenli Iş istasyonu kullanıcıları" grubunu seçerek **Seçili** ' ı seçin.

#### <a name="removal-of-local-admin-rights"></a>Yerel yönetici haklarının kaldırılması

Bu yöntem VIP, DevOps ve güvenli düzey iş istasyonlarının kullanıcılarının makinelerinde yönetici haklarına sahip olmasını gerektirir. Bu ayarı Azure portal yapılandırmak için:

1. **Azure Active Directory**  >  **cihazlar**  >  **cihaz ayarları**' na gidin.
1. **Azure AD 'ye katılmış cihazlarda ek yerel Yöneticiler**altında **hiçbiri** ' ni seçin.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Cihazlara katılması için çok faktörlü kimlik doğrulaması gerektir

Cihazları Azure AD 'ye katma işlemini daha da güçlendirin:

1. **Azure Active Directory**  >  **cihazlar**  >  **cihaz ayarları**' na gidin.
1. **Cihazlara katmak Için Multi-Factor auth iste**altında **Evet** ' i seçin.
1. **Kaydet**’i seçin.

#### <a name="configure-mobile-device-management"></a>Mobil cihaz yönetimini yapılandırma

Azure portal:

1. **Azure Active Directory**  >  **Mobility (MDM ve MAM)**  >  **Microsoft Intune**gidin.
1. **MDM Kullanıcı kapsamı** ayarını **Tümü**olarak değiştirin.
1. **Kaydet**’i seçin.

Bu adımlar, Intune ile herhangi bir cihazı yönetmenizi sağlar. Daha fazla bilgi için bkz. [Intune hızlı başlangıç: Windows 10 cihazları için otomatik kayıt ayarlama](/Intune/quickstart-setup-auto-enrollment). Intune yapılandırma ve uyumluluk ilkelerini gelecekteki bir adımda oluşturursunuz.

#### <a name="azure-ad-conditional-access"></a>Azure AD Koşullu Erişim

Azure AD koşullu erişimi, ayrıcalıklı yönetim görevlerinin uyumlu cihazlarla kısıtlanmasını sağlamaya yardımcı olabilir. **Güvenli Iş Istasyonu kullanıcıları** grubunun önceden tanımlanmış üyeleri, bulut uygulamalarında oturum açarken Multi-Factor Authentication gerçekleştirmek için gereklidir. En iyi uygulama, acil durum erişim hesaplarını ilkeden dışlayamazsınız. Daha fazla bilgi için bkz. [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md).

## <a name="intune-configuration"></a>Intune yapılandırması

### <a name="configure-enrollment-status"></a>Kayıt durumunu yapılandırma

Güvenli iş istasyonunuzun güvenilir bir temiz cihaz olduğundan emin olmak önemlidir. Yeni cihazlar satın alırken, Microsoft 'un, sağlama zinciri yönetimi sırasında güvenlik açıklarına maruz kalma oranını sınırlayan [Windows 10 Pro 'Nun Windows 10 Pro](/Windows/deployment/Windows-10-pro-in-s-mode)'ya ayarlı olduğunu insist sağlayabilirsiniz. Tedarikçinizden bir cihaz aldıktan sonra, Autopilot kullanarak bunları S modundan değiştirebilirsiniz. Aşağıdaki kılavuz, dönüşüm işlemini uygulama hakkında ayrıntılar sağlar.

Cihazların kullanılmadan önce tam olarak yapılandırıldığından emin olmak için Intune, **tüm uygulamalar ve profiller yüklenene kadar cihaz kullanımını engellemek**için bir yol sağlar.

**Azure Portal**:

1. **Microsoft Intune**  >  **cihaz kaydı**  >  **Windows kayıt**  >  **kayıt durumu sayfası**  >  **varsayılan**  >  **ayarları**' na gidin.
1. **Uygulama profili yükleme Ilerlemesini göster** ' i **Evet**olarak ayarlayın.
1. **Tüm uygulamalar ve profiller Evet 'e yüklenene kadar cihaz kullanımını engelle** ' **Yes**ye ayarlayın.

### <a name="create-an-autopilot-deployment-profile"></a>Bir Autopilot dağıtım profili oluşturma

Bir cihaz grubu oluşturduktan sonra, Autopilot cihazlarını yapılandırmak için bir dağıtım profili oluşturmanız gerekir.

Azure portal Intune 'da:

1. **Cihaz kaydı**  >  **Windows kayıt**  >  **dağıtım profilleri**  >  **Profil oluştur**' u seçin.
1. Şunları girin:

   * Ad- **güvenli iş istasyonu dağıtım profili**.
   * Açıklama- **güvenli iş Istasyonlarının dağıtımı**.
   * **Tüm hedeflenen cihazları Autopilot’a dönüştür** ayarını **Evet** olarak seçin. Bu ayar, listedeki tüm cihazların Autopilot dağıtım hizmetine kaydolmasını sağlar. Kaydın işlenmesi için 48 saat kadar bekleyin.

1. **İleri**’yi seçin.

   * **Dağıtım modu**Için, **kendi kendine dağıtma (Önizleme)** öğesini seçin. Bu profile sahip cihazlar, cihazı kaydeden kullanıcıyla ilişkilendirilir. Cihazı kaydetmek için kullanıcı kimlik bilgileri gerekir. Bir cihazı **kendi kendine** dağıtma modunda dağıtmak, paylaşılan bir modele dizüstü bilgisayar dağıtmanıza imkan tanımak için gereklidir. Cihaz bir kullanıcıya ilk kez atanana kadar hiçbir Kullanıcı Ataması gerçekleşmeyecektir. Sonuç olarak, bir kullanıcı ataması tamamlanana kadar BitLocker gibi tüm kullanıcı ilkeleri etkinleştirilmez. Güvenli bir cihazda oturum açma hakkında daha fazla bilgi için bkz. [Seçili profiller](/intune/device-profile-assign).
   * **Azure AD 'ye katılım** kutusu, **Azure AD 'ye katılmış** ve gri renkte olmalıdır.
   * Dilinizi (bölge), Kullanıcı hesabı türü **standardını**seçin. 

1. **İleri**’yi seçin.

   * Önceden yapılandırdıysanız bir kapsam etiketi seçin.

1. **İleri**’yi seçin.
1. **Assignments**  >  **Assign to**  >  **Seçili gruplara**atanan atamalar ' ı seçin. **Dahil edilecek grupları seçin**' de, **güvenli iş istasyonları**' nı seçin.
1. **İleri**’yi seçin.
1. Profili oluşturmak için **Oluştur**’u seçin. Autopilot dağıtım profili artık cihazlara atanmak üzere hazırdır.

Autopilot ' deki cihaz kaydı, cihaz türü ve rolüne göre farklı bir kullanıcı deneyimi sağlar. Dağıtım örneğimizde, güvenli cihazların toplu olarak dağıtıldığı ve paylaşılabileceği, ancak ilk kez kullanıldığı, cihazın bir kullanıcıya atandığı bir model gösterilmektedir. Daha fazla bilgi için bkz. [Intune Autopilot cihaz kaydı](/intune/device-enrollment).

### <a name="configure-windows-update"></a>Windows Update Yapılandır

Windows 10 ' un güncel tutulması, yapabileceğiniz en önemli şeylerden biridir. Windows 'u güvenli bir durumda tutmak için güncelleştirmelerin iş istasyonlarına uygulanma hızını yönetmek üzere bir güncelleştirme halkası dağıtırsınız. 

Bu kılavuzda, yeni bir güncelleştirme halkası oluşturmanız ve aşağıdaki varsayılan ayarları değiştirmeniz önerilir:

Azure portalında:

1. **Microsoft Intune**  >  **yazılım güncelleştirmeleri**  >  **Windows 10 güncelleştirme halkaları**' na gidin.
1. Şunları girin:

   * Ad- **Azure tarafından yönetilen iş istasyonu güncelleştirmeleri**
   * Bakım kanalı- **Windows Insider-Fast**
   * Kalite güncelleştirmesi erteleme (gün)- **3**
   * Özellik Güncelleştirmesi erteleme süresi (gün)- **3**
   * Otomatik Güncelleştirme davranışı- **Son Kullanıcı denetimi olmadan otomatik yükleme ve yeniden başlatma**
   * Kullanıcının Windows güncelleştirmelerini duraklatmasını Engelle- **Engelle**
   * İş saatleri dışında yeniden başlatma için kullanıcının onayını iste- **gerekli**
   * Kullanıcının yeniden başlatılmasına izin ver (bağlı yeniden başlatma)- **gerekli**
   * Otomatik yeniden başlatmadan sonra kullanıcıların kullanıcı tarafından yeniden başlatılmasını sağlamak için geçiş (gün)- **3**
   * Yeniden başlatma için yeniden başlatma anımsatıcısı (gün)- **3**
   * Bekleyen yeniden başlatmalar için son tarihi ayarla (gün)- **3**

1. **Oluştur**'u seçin.
1. **Atamalar** sekmesinde, **güvenli iş istasyonları** grubunu ekleyin.

Windows Update ilkeleri hakkında daha fazla bilgi için bkz. [Ilke CSP-güncelleştirme](/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP Intune tümleştirmesi

Windows Defender ATP ve Microsoft Intune, güvenlik ihlallerini önlemeye yardımcı olmak için birlikte çalışır. Ayrıca, ihlallerinin etkisini de sınırlayabilir. ATP özellikleri, gerçek zamanlı tehdit algılama ve uç nokta cihazlarının kapsamlı şekilde denetlenmesini ve günlüğe kaydedilmesini olanaklı kılmaktadır.

Windows Defender ATP ve Intune tümleştirmesini yapılandırmak için Azure portal gidin.

1. **Microsoft Intune**  >  **Cihaz uyumluluğu**  >  **Windows Defender ATP**Microsoft Intune gidin.
1. Windows Defender ATP 'yi **yapılandırma**altındaki adım 1 ' de, Windows Defender **güvenlik merkezi 'Nde Microsoft Intune IÇIN Windows Defender ATP 'yi bağla**' yı seçin.
1. Windows Defender Güvenlik Merkezi’nde:

   1. **Ayarlar**  >  **Gelişmiş Özellikler**' i seçin.
   1. **Microsoft Intune bağlantı**için **Açık**' ı seçin.
   1. **Tercihleri kaydet**’i seçin.

1. Bir bağlantı kurulduktan sonra Intune 'a dönün ve en üstte **Yenile** ' yi seçin.
1. **10.0.15063 ve üstü sürümü Windows cihazları Windows Defender ATP bağlayıcısına bağla** ayarını **Açık** olarak ayarlayın.
1. **Kaydet**’i seçin.

Daha fazla bilgi için bkz. [Windows Defender Gelişmiş tehdit koruması](/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>İş istasyonu profili sağlamlaştırma

Çözümün sağlamlaştırılmasını başarıyla tamamlaması için uygun betiği indirip yürütün. İstediğiniz **profil düzeyiniz**için indirme bağlantılarını bulun:

| Profil | İndirme konumu | Kısaltın |
| --- | --- | --- |
| Düşük güvenlik | Yok | Yok |
| Gelişmiş güvenlik | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809). ps1 |
| Yüksek güvenlik | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809). ps1 |
| Özelleştirilmiş | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC-Windows10 (1803) SecurityBaseline.ps1 |
| Özelleştirilmiş uyumluluk * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10 (1803). ps1 |
| Korunmalıdır | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809) -SecurityBaseline.ps1 |

\*Özelleştirilmiş uyumluluk, NCSC Windows10 SecurityBaseline içinde sunulan özelleştirilmiş yapılandırmayı zorlayan bir betiktir.

Betik başarıyla yürütüldükten sonra, Intune 'da profiller ve ilkeler için güncelleştirmeler yapabilirsiniz. Gelişmiş ve güvenli profiller için betikler, sizin için ilkeler ve profiller oluşturur, ancak ilkeyi **güvenli Iş istasyonları** cihaz grubunuza atamanız gerekir.

* Betikler tarafından oluşturulan Intune cihaz yapılandırma profillerini burada bulabilirsiniz: **Azure Portal**  >  **Microsoft Intune**  >  **cihaz yapılandırma**  >  **profilleri**.
* Betikler tarafından oluşturulan Intune cihaz uyumluluk ilkelerini burada bulabilirsiniz: **Azure Portal**  >  **Microsoft Intune**  >  **cihaz uyumluluk**  >  **ilkeleri**.

Betikler tarafından yapılan değişiklikleri gözden geçirmek için profilleri dışarı aktarabilirsiniz. Bu şekilde, [Seccon belgelerinde](/windows/security/threat-protection/windows-security-configuration-framework/windows-security-configuration-framework)özetlendiği gibi gerekebilecek ek sağlamlaştırma belirleyebilirsiniz.

`DeviceConfiguration_Export.ps1`Geçerli tüm Intune profillerini dışarı aktarmak Için [Deviceconfiguration GiuHub deposundan](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) Intune veri dışa aktarma betiğini çalıştırın.

## <a name="additional-configurations-and-hardening-to-consider"></a>Göz önünde bulundurulması gereken ek yapılandırma ve sağlamlaştırma

Buradaki kılavuzu izleyerek güvenli bir iş istasyonu dağıttınız. Bununla birlikte, ek denetimler de göz önünde bulundurmanız gerekir. Örneğin:

* farklı tarayıcılara erişimi sınırlandırma
* giden HTTP 'ye izin ver
* select Web sitelerini engelle
* Özel PowerShell betikleri çalıştırmak için izinleri ayarla

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Güvenlik Duvarı yapılandırma hizmeti sağlayıcısında (CSP) kuralları ayarlama

İzin verilen ve engellenen uç noktalarınız için gerektiğinde hem gelen hem de giden kuralların yönetiminde ek değişiklikler yapabilirsiniz. Güvenli iş istasyonuna güvenmeye devam ederken tüm gelen ve giden trafiği reddeden kısıtlamayı gevşeyin. İzin verilen giden sitelerin ortak ve güvenilen Web sitelerini içermesini sağlayabilirsiniz. Daha fazla bilgi için bkz. [güvenlik duvarı yapılandırma hizmeti](/Windows/client-management/mdm/firewall-csp).

Kısıtlayıcı URL trafiği yönetimi şunları içerir:

* Güvenli iş istasyonu profili betiğinde tüm gelen trafiği engelle-ayarla.
* Azure Cloud Shell dahil olmak üzere seçili Azure ve Microsoft hizmetleri dışındaki tüm giden trafiği reddetme ve Azure parola sıfırlamayı olanaklı yapma özelliği.

```
[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"ProxyEnable"=dword:00000001
"ProxyServer"="127.0.0.1:80"
"ProxyOverride"="*.azure.com;*.azure.net;*.microsoft.com;*.windowsupdate.com;*.microsoftonline.com;*.microsoftonline.cn;*.windows.net;*.windowsazure.com;*.windowsazure.cn;*.azure.cn;*.loganalytics.io;*.applicationinsights.io;*.vsassets.io;*.azure-automation.net;*.visualstudio.com,portal.office.com;*.aspnetcdn.com;*.sharepointonline.com;*.msecnd.net;*.msocdn.com;*.webtrends.com"
"AutoDetect"=dword:00000000
```

Engelleme kurallarınız için daha fazla ayrıntı sağlamak istiyorsanız, [etki alanı engelleme listesini (DBL)](https://www.spamhaus.org/dbl/)tutan Spamhaus projesine başvurabilirsiniz: siteleri engellemek için uygulanacak gelişmiş bir kural kümesi olarak kullanılacak iyi bir kaynaktır.

### <a name="manage-local-applications"></a>Yerel uygulamaları yönetme

Üretkenlik uygulamaları da dahil olmak üzere, yerel uygulamalar kaldırıldığında güvenli iş istasyonu gerçekten sağlamlaştırılmış duruma getirilir. Burada, Chrome 'u varsayılan tarayıcı olarak ekler ve Intune 'U kullanarak kullanıcının tarayıcıyı ve eklentilerini değiştirme yeteneğini sınırlayabilirsiniz.

#### <a name="deploy-applications-using-intune"></a>Intune kullanarak uygulama dağıtma

Bazı durumlarda, Google Chrome tarayıcısı gibi uygulamalar, güvenli iş istasyonunda gereklidir. Aşağıdaki örnek, güvenlik grubu **güvenli Iş istasyonlarındaki**cihazlara Chrome yüklemek için yönergeler sağlar.

1. [Windows 64-bit için çevrimdışı yükleyici Chrome paketi](https://cloud.google.com/chrome-enterprise/browser/download/)' ni indirin.
1. Dosyaları ayıklayın ve dosyanın konumunu aklınızda olun `GoogleChromeStandaloneEnterprise64.msi` .
1. **Azure Portal** **Microsoft Intune**  >  **Client Apps**  >  **uygulamaları**  >  **Ekle**' ye gidin.
1. **Uygulama türü**' nün altında **iş kolu**' yi seçin.
1. **Uygulama paketi dosyası**altında `GoogleChromeStandaloneEnterprise64.msi` ayıklanan konumdan dosyayı seçin ve **Tamam**' ı seçin.
1. **Uygulama bilgileri**altında bir açıklama ve yayımcı sağlayın. **Tamam**’ı seçin.
1. **Ekle**'yi seçin.
1. **Atamalar** sekmesinde, **atama türü**altında **Kayıtlı cihazlar için kullanılabilir** ' ı seçin.
1. **Dahil edilen gruplar**' ın altında, **güvenli iş istasyonları** grubunu ekleyin.
1. **Tamam**' ı ve ardından **Kaydet**' i seçin.

Chrome ayarlarını yapılandırma hakkında daha fazla bilgi için bkz. [Microsoft Intune Chrome tarayıcısını yönetme](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Özel uygulamalar için şirket portalını yapılandırma

Güvenli modda uygulama yüklemesi, Intune şirket portalı ile kısıtlıdır. Ancak, portalın yüklenmesi Microsoft Store için erişim gerektirir. Güvenli çözümünüzde, şirket portalını çevrimdışı bir mod aracılığıyla tüm cihazlarda kullanılabilir hale getirebilirsiniz.

[Şirket portalı](/Intune/store-apps-company-portal-app) Intune tarafından yönetilen bir kopyası, güvenli iş istasyonları kullanıcılarına gönderebilmeniz için kullanabileceğiniz ek araçlara isteğe bağlı erişim sağlar.

Dağıtımı özel hazırlıklar gerektiren Windows 32-bit uygulamaları veya diğer uygulamaları yüklemeniz gerekebilir. Bu gibi durumlarda, [Microsoft Win32 içerik hazırlığı aracı](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) , yükleme için kullanıma hazırlamış bir `.intunewin` Biçim dosyası sağlayabilir.

### <a name="conditional-access-only-allowing-secured-workstation-ability-to-access-azure-portal"></a>Koşullu erişim yalnızca güvenliği sağlanmış iş istasyonu erişimine izin verir Azure portal

Azure AD, Azure bulut yönetim portalınıza kimlerin erişebileceğini yönetme ve kısıtlama özelliği sunar. [Koşullu erişimin](../conditional-access/overview.md) etkinleştirilmesi yalnızca güvenli iş istasyonunuzun kaynakları yönetmesini veya değiştirebileceğini güvence altına alacak. [Acil durum erişim](../users-groups-roles/directory-emergency-access.md) işlevinin yalnızca olağanüstü durumlar ve ilke aracılığıyla yönetilen hesap için kullanılması gerekiyorsa, göz önünde bulundurmanız gereken bu özelliği dağıtmaktan çok önemlidir.

> [!NOTE]
> Bir Kullanıcı grubu oluşturmanız ve koşullu erişim ilkesini atlayabileceği acil durum kullanıcısını eklemeniz gerekir. Bizim örneğimizde **acil durum Kahcam** adlı bir güvenlik grubu sunuyoruz

1. **Azure portal**  >  **Microsoft Intune**  >  **Koşullu erişim ilkeleri**  >  **Yeni ilkeye**Microsoft Intune Azure Portal gidin.
1. İlke için bir **ad** girin.
1. **Kullanıcı ve grup**Seç  >  **Kullanıcılar ve gruplar ' ı** seçin 
1. **Include**  >  **Dizin rollerini** dahil et ' i seçin > rolleri seçin > genel yönetici, ayrıcalıklı rol yöneticisi, ayrıcalıklı kimlik doğrulama Yöneticisi, Güvenlik Yöneticisi, uyumluluk Yöneticisi, koşullu erişim Yöneticisi, uygulama Yöneticisi, bulut uygulaması Yöneticisi, Intune Hizmet Yöneticisi
1. **Hariç tut** ' u seçin > **kullanıcıları ve grupları** seçin > **Dışlanan kullanıcıları seçin** > **acil durum kahvice** grubunuzu seçin.
1. **Bulut uygulamaları veya eylemler** seçin > **tüm bulut uygulamalarını** seçin
1. **Koşullar** ' ı seçin > **cihaz platformları** seçin > Yapılandır **Evet** ' i seçin > **cihaz platformları Seç** **Windows** 'u seçin
1. **Erişim denetimlerini** seçin > **erişim ver** ' i seçin **Evet** ' i seçin > **cihazın uyumlu olarak işaretlenmesini gerektir**seçeneğini belirleyin. 
1. **İlkeyi etkinleştir**  >  **'** i seçin
 
Bu ilke kümesi, yöneticilerin Intune ve WDADU tarafından ayarlanan uyumlu bir Windows cihazı kullanmasını sağlamaktır. 

Kuruluşlar, ortamlarında eski kimlik doğrulama protokollerini engellemeyi de düşünmelidir. Bu görevi gerçekleştirmenin birden çok yolu vardır. eski kimlik doğrulama protokollerini engelleme hakkında daha fazla bilgi için bkz. [nasıl yapılır: koşullu erişim Ile Azure AD 'ye eski kimlik doğrulamasını engelleme](../conditional-access/block-legacy-authentication.md).

### <a name="use-powershell-to-create-custom-settings"></a>Özel ayarlar oluşturmak için PowerShell 'i kullanma

Ayrıca, PowerShell kullanarak konak yönetimi özelliklerini genişletebilirsiniz. Bu örnek betik, konakta varsayılan bir arka plan ayarlar. Bu, Azure portal ve profiller aracılığıyla da kullanılabilir bir özelliktir. Örnek betik yalnızca PowerShell işlevlerini göstermek için hizmet verir.

Güvenli iş istasyonlarınızda bazı özel denetimleri ve ayarları ayarlamanız gerekebilir. Bu örnek, PowerShell 'in cihazı kullanıma hazırlama ve güvenli bir iş istasyonu olarak kolayca belirlemek için PowerShell 'in özelliğini kullanarak iş istasyonunun arka planını değiştirir.

Microsoft komut dosyası merkezi 'ndeki [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) betiği, Windows 'un başlangıçta bu [ücretsiz, genel arka plan görüntüsünü](https://i.imgur.com/OAJ28zO.png) yüklemesine olanak tanır.

1. Betiği yerel bir cihaza indirin.
1. CustomerXXXX ve arka plan görüntüsünün karşıdan yükleme konumunu güncelleştirin. Örneğimizde, customerXXXX ' i arka plana değiştirirsiniz.
1. **Azure Portal**  >  **Microsoft Intune**  >  **cihaz yapılandırması**  >  **PowerShell betikleri**  >  **Ekle**' ye gidin.
1. Betik için bir **ad** girin ve **betik konumunu**belirtin.
1. **Yapılandır**'ı seçin.
   1. **Bu betiği, oturum açmış kimlik bilgilerini Evet olarak kullanarak Çalıştır '** a ayarlayın. **Yes**
   1. **Tamam**’ı seçin.
1. **Oluştur**'u seçin.
1. **Atamaları**seçin  >  **grupları**seçin.
   1. Güvenlik grubu **güvenli Iş istasyonlarını**ekleyin.
   1. **Kaydet**’i seçin.

## <a name="enroll-and-validate-your-first-device"></a>İlk cihazınızı kaydetme ve doğrulama

1. Cihazınızı kaydetmek için aşağıdaki bilgilere ihtiyacınız vardır:
   * **Seri numarası** -cihaz kasada bulunabilir.
   * **Windows ürün kimliği** - **System**  >  Windows ayarları menüsünden Sistem**hakkında** bölümünde bulunur.
   * Cihaz kaydı için gerekli tüm bilgileri içeren bir CSV karma dosyası almak için [Get-Windowsautopilotınfo](https://aka.ms/Autopilotshell) ' yı çalıştırabilirsiniz.
   
     `Get-WindowsAutoPilotInfo – outputfile device1.csv`Bilgileri Intune 'a AKTARABILECEĞINIZ CSV dosyası olarak çıktısını almak için çalıştırın.

     > [!NOTE]
     > Betik için yükseltilmiş haklar gerekir. Uzaktan imzalanmış olarak çalışır. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`Komut betiğin doğru çalışmasına izin verir.

   * Bu bilgileri, Windows 10 sürüm 1809 veya daha yüksek bir cihazda oturum açarak toplayabilirsiniz. Ayrıca, donanım satıcınız bu bilgileri de sağlayabilir.
1. **Azure Portal** **Microsoft Intune**  >  **cihaz kaydı**  >  **Windows kayıt**  >  **cihazları-Windows Autopilot cihazlarını Yönet**' e gidin.
1. **Içeri aktar** ' ı SEÇIN ve CSV dosyanızı seçin.
1. Cihazı **güvenli Iş istasyonları** güvenlik grubuna ekleyin.
1. Yapılandırmak istediğiniz Windows 10 cihazında, **Windows ayarları**  >  **güncelleştirme & güvenlik**  >  **Kurtarma**' ya gidin.
   1. **Bu bilgisayarı sıfırla**bölümünden **başlayın** ' ı seçin.
   1. Cihazı sıfırlamak ve yapılandırılmış profil ve uyumluluk ilkeleriyle yeniden yapılandırmak için istemleri izleyin.

Cihazı yapılandırdıktan sonra bir gözden geçirmeyi tamamlayıp yapılandırmayı denetleyin. Dağıtıma devam etmeden önce ilk cihazın doğru şekilde yapılandırıldığını onaylayın.

## <a name="assign-devices"></a>Cihazları ata

Cihazları ve kullanıcıları atamak için, [Seçili profilleri](/intune/device-profile-assign) güvenlik grubunuza eşlemeniz gerekir. Hizmet izinleri gerektiren tüm yeni kullanıcılar da güvenlik grubuna eklenmelidir.

## <a name="using-sentinel-and-windows-defender-atp-to-monitor-and-respond-to-security-incidents"></a>Güvenlik olaylarını izlemek ve yanıtlamak için Sentinel ve Windows Defender ATP kullanma

Güvenli iş istasyonu dağıtımını izleme, [Sentinel] etkinleştirilerek ve [tehdit ve güvenlik açığı yönetimi](/windows/security/threat-protection/microsoft-defender-atp/next-gen-threat-and-vuln-mgt) kullanılarak gerçekleştirilebilir. bu kılavuzda ayrıntılı tehdit ele alınamaz, ancak olası güvenlik olaylarını izlemek ve yanıt vermek için iyi bir yaygın algılama çabaları sağlanır.

**Azure Sentinel** 'i şu şekilde kullanacağız: 

* Kullanıcı ve cihaz izleme için Intune, Azure portal ve Azure AD 'den veri toplama
* Kullanıcı ve cihaz yapılandırması şüpheli etkinliğini araştırmak için yardım
* WDADTP kullanarak güvenlik araştırmaları araştırma özelliğini

Sentinel izleme, Azure AD gibi veri kaynaklarınıza yönelik bağlayıcıların ayarlanmasını gerektirir.

1. **Azure Portal** **Azure Sentinel (Önizleme)** seçeneğine gidin > **Ekle** ' yi seçin.
1. **Azure Sentinel 'e eklemek için bir çalışma alanı seçin** bölümünde **Yeni çalışma alanı oluştur** ' u seçin.
1. Şunları girin:
   * **Log Analytics çalışma alanı** -' güvenli Iş istasyonu izleme '
   * **Abonelik** -etkin aboneliğinizi seçin
   * **Kaynak grubu** - **Tamam** > "yeni oluştur * * > Secure Workstation RG ' i seçin
   * **Konum** -dağıtımınız için coğrafi olarak en uygun konumu seçin
   * **Fiyat katmanı** - **GB başına seçim (2018)**
1. **Tamam ' ı**seçin.

Daha sonra, kullanılabilir güvenli iş istasyonu veri kaynaklarını izlemeye bağlayacağız.

1. **Azure Portal** **Azure Sentinel çalışma** alanına gidin > **güvenli Iş istasyonu izleme** çalışma alanını seçin
1. **Veri bağlayıcıları** seçin
1. Önkoşulları inceledikten sonra bağlayıcı sayfası > **Azure Active Directory** > seçin. Yapılandırmaya ilerleyin ve Azure AD oturum açma günlüklerinin yanı sıra Azure AD denetim günlükleri için de **Bağlan** ' ı seçin.
1. Önkoşulları inceledikten sonra bağlayıcı sayfası > açmak > **Azure etkinliğini** seçin. Azure etkinlik günlüklerini yapılandırmaya devam edin > aboneliğinizi seçin > **Bağlan** ' ı seçin.

Veriler Sentinel tarafından toplandıkça, **Azure Portal**seçerek etkinliği gözlemleyebilirsiniz, **Azure Sentinel 'e genel bakış ' a** gidebilirsiniz 

**Windows Defender ATP 'yi (WDADTP)** şu şekilde kullanacağız:

* Güvenlik açıklarını ve yapılandırmalarını sürekli gözlemleyin ve izleyin
* Joker karakter içindeki dinamik tehditleri önceliklendirmek için WDADTP kullanın
* Uç nokta algılama ve yanıt (EDR) uyarılarla güvenlik açıklarının bağıntısını
* Araştırmalar sırasında makine düzeyinde güvenlik açığını belirlemek için panoyu kullanma
* Intune 'a yönelik düzeltmelere gönderim

[Defender ATP panonuzu](https://securitycenter.windows.com/machines)yapılandırın. [Tehdit & güvenlik açığı yönetimi panosuna genel bakış](/windows/security/threat-protection/microsoft-defender-atp/tvm-dashboard-insights)' da rehberlik kullanma.

## <a name="monitoring-application-activity-using-microsoft-monitoring-agent-mma"></a>Microsoft Monitoring Agent kullanarak uygulama etkinliğini izleme (MMA)
Özelleştirilmiş iş istasyonundan başlayarak, uygulama dolabı bir iş istasyonunda uygulama etkinliğinin izlenmesi için etkinleştirilir. İzlemenin Log Analytics çalışma alanınızda tümleştirilebilmesi için bir MMA Aracısı ve yapılandırması izlenmelidir. 

> [!NOTE]
> Özelleştirilmiş iş istasyonu profili, AppLocker izleme ilkelerini içerir. Bir istemcide uygulama etkinliğinin izlenmesi için ilkelerin dağıtılması gerekir

MMA aracısını Intune PowerShell betiği ile dağıtma

1. Kurulum [betiğini yerel bir cihaza](https://aka.ms/securedworkstationgit)indirin.
1. **$WorkSpaceID** ve **$WorkSpaceKey** parametrelerini güncelleştirin
1. **Azure Portal**  >  **Microsoft Intune**  >  **cihaz yapılandırması**  >  **PowerShell betikleri**  >  **Ekle**' ye gidin.
1. Betik için bir **ad** girin ve **betik konumunu**belirtin.
1. **Yapılandır**'ı seçin.
   1. **Bu betiği, oturum açmış kimlik bilgilerini Evet olarak kullanarak Çalıştır '** a ayarlayın. **Yes**
   1. **Tamam**’ı seçin.
1. **Oluştur**'u seçin.
1. **Atamaları**seçin  >  **grupları**seçin.
   1. Güvenlik grubu **güvenli Iş istasyonlarını**ekleyin.
   1. **Kaydet**’i seçin.

Sonra yeni günlükleri almak için Log Analytics ayarlamanız gerekir
1. **Azure Portal**, **Log Analytics çalışma alanına** gidin > ' güvenli iş Istasyonu izleme ' öğesini seçin
1. **Gelişmiş ayarlar**  >  **verileri**  >  **Windows olay günlüklerini** seçin
1. **Aşağıdaki olay günlüklerinden olay topla** ' da 
1. Şunları girin:
   * ' Microsoft-Windows-AppLocker/EXE ve DLL ' > **bilgi** seçimini kaldırın
   * ' Microsoft-Windows-AppLocker/MSI ve Script ' > **bilgi** seçimini kaldırın
   * ' Microsoft-Windows-AppLocker/paketlenmiş uygulama-dağıtım ' > **bilgi** seçimini kaldırın
   * ' Microsoft-Windows-AppLocker/paketlenmiş uygulama-yürütme ' > **bilgi** seçimini kaldırın
1. **Kaydet**’i seçin

Uygulama günlüğü, seçtiğiniz Log Analytics çalışma alanında kullanılabilir.

## <a name="monitoring"></a>İzleme

* [Azure Sentinel ile tehditleri algılamayı](/azure/sentinel/tutorial-detect-threats) öğrenin
* [Azure Sentinel ile olayları araştırın](/azure/sentinel/tutorial-investigate-cases)
* [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](/azure/sentinel/tutorial-respond-threats-playbook)
* [Pozlama puanınızı](/windows/security/threat-protection/microsoft-defender-atp/tvm-exposure-score) nasıl gözden geçibileceğinizi anlayın
* [Güvenlik önerisini](/windows/security/threat-protection/microsoft-defender-atp/tvm-security-recommendation) gözden geçirme
* [Güvenlik düzeltmelerinin](/windows/security/threat-protection/microsoft-defender-atp/tvm-remediation) yönetimi
* [Uç nokta algılamayı ve yanıtını](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response) yönetme
* [Intune profil izleme](/intune/device-profile-monitor)ile profilleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Intune](/intune/index)hakkında daha fazla bilgi edinin.
* [Azure AD](../index.yml)'yi anlayın.
* [Microsoft Defender Gelişmiş tehdit koruması](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) ile çalışma
* [Azure Sentinel](/azure/sentinel/) 'i bulma
