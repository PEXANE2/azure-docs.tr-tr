---
title: Sanal makinelerin içeriğini denetlemeyi öğrenin
description: Azure İlke'nin sanal makinelerdeki ayarları denetlemek için Konuk Yapılandırma aracısını nasıl kullandığını öğrenin.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: 889e99e94b2c81a6654fcbe7851e93c40163a0c6
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985329"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi'nin Konuk Yapılandırması'nı anlayın

Azure İlkesi, Azure kaynaklarını denetlemenin ve [düzeltmenin](../how-to/remediate-resources.md) ötesinde, bir makinenin içindeki ayarları denetleyebilir. Doğrulama, Konuk Yapılandırması uzantısı ve istemcisi tarafından gerçekleştirilir. Uzantı, istemci aracılığıyla şunun gibi ayarları doğrular:

- İşletim sisteminin yapılandırması
- Uygulama yapılandırması veya varlığı
- Ortam ayarları

Şu anda, çoğu Azure İlkesi Konuk Yapılandırma ilkesi yalnızca makine içindeki ayarları denetler. Yapılandırmaları uygulamıyorlar. Özel [durum, aşağıda başvurulan](#applying-configurations-using-guest-configuration)yerleşik bir ilkedir.

## <a name="extension-and-client"></a>Uzantı ve istemci

Makine içindeki ayarları denetlemek için [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir. Uzantı, geçerli ilke atamasını ve ilgili yapılandırma tanımını karşıdan yükler.

### <a name="limits-set-on-the-extension"></a>Uzantıda belirlenen sınırlar

Uzantının makine içinde çalışan uygulamaları etkilemesini sınırlamak için, Konuk Yapılandırması'nın CPU kullanımının %5'inden fazlasını aşmasına izin verilmez. Bu sınırlama hem yerleşik hem de özel tanımlar için vardır.

## <a name="register-guest-configuration-resource-provider"></a>Konuk Yapılandırma kaynak sağlayıcısını kaydedin

Konuk Yapılandırmasını kullanabileniz için kaynak sağlayıcısını kaydetmeniz gerekir. Portal dan veya PowerShell üzerinden kayıt olabilirsiniz. Konuk Yapılandırma ilkesinin atanması portal üzerinden yapılırsa kaynak sağlayıcısı otomatik olarak kaydedilir.

### <a name="registration---portal"></a>Kayıt - Portal

Kaynak sağlayıcısını Azure portalı üzerinden Konuk Yapılandırması için kaydetmek için aşağıdaki adımları izleyin:

1. Azure portalını başlatın ve **Tüm hizmetlere**tıklayın. **Abonelikleri**arayın ve seçin.

1. Konuk Yapılandırması'nı etkinleştirmek istediğiniz aboneliği bulun ve tıklayın.

1. **Abonelik** sayfasının sol menüsünde **Kaynak sağlayıcıları**tıklatın.

1. **Microsoft.GuestConfiguration'ı**bulana kadar filtre uygulayın veya kaydırın, ardından aynı satırda **Kaydol'u** tıklatın.

### <a name="registration---powershell"></a>Kayıt - PowerShell

PowerShell aracılığıyla Konuk Yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

## <a name="validation-tools"></a>Doğrulama araçları

Makinenin içinde, Konuk Yapılandırma istemcisi denetimi çalıştırmak için yerel araçları kullanır.

Aşağıdaki tablo, desteklenen her işletim sisteminde kullanılan yerel araçların listesini gösterir:

|İşletim sistemi|Doğrulama aracı|Notlar|
|-|-|-|
|Windows|[Windows PowerShell İstenilen Durum Yapılandırması](/powershell/scripting/dsc/overview/overview) v2| |
|Linux|[Şef InSpec](https://www.chef.io/inspec/)| Ruby ve Python makinede değilse, Konuk Yapılandırma uzantısı tarafından yüklenir. |

### <a name="validation-frequency"></a>Doğrulama sıklığı

Konuk Yapılandırma istemcisi her 5 dakikada bir yeni içerik için kontrol eder. Konuk atama alındıktan sonra, bu yapılandırmanın ayarları 15 dakikalık bir aralıkla yeniden denetlenir.
Denetim tamamlandığında sonuçlar Konuk Yapılandırma kaynak sağlayıcısına gönderilir. İlke [değerlendirme tetikleyicisi](../how-to/get-compliance-data.md#evaluation-triggers) oluştuğunda, makinenin durumu Konuk Yapılandırma kaynak sağlayıcısına yazılır. Bu güncelleştirme, Azure İlkesi'nin Azure Kaynak Yöneticisi özelliklerini değerlendirmesine neden olur. İsteğe bağlı Azure İlkesi değerlendirmesi, Konuk Yapılandırma kaynak sağlayıcısından en son değeri alır. Ancak, makine içindeki yapılandırmanın yeni bir denetimini tetiklemiyor.

## <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tablo, Azure görüntülerinde desteklenen işletim sisteminin listesini gösterir:

|Yayımcı|Adı|Sürümler|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|Credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Windows İstemcisi|Windows 10|
|OpenLogic|CentOS|7.3, 7.4, 7.5, 7.6, 7.7|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5, 7.6, 7.7|
|Suse|SLES|12 SP3|

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Windows Server Nano Server hiçbir sürümde desteklenmez.

## <a name="guest-configuration-extension-network-requirements"></a>Konuk Yapılandırma Uzantısı ağ gereksinimleri

Azure'daki Konuk Yapılandırma kaynak sağlayıcısıyla iletişim kurmak için **makineler, 443**bağlantı noktasındaki Azure veri merkezlerine giden erişim gerektirir. Azure'da giden trafiğe izin vermeyen özel bir sanal ağ kullanıyorsanız, özel durumları [Ağ Güvenliği Grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kurallarıyla yapılandırın.
"GuestAndHybridManagement" [hizmet etiketi](../../../virtual-network/service-tags-overview.md) Konuk Yapılandırma hizmetine başvurmak için kullanılabilir.

## <a name="guest-configuration-definition-requirements"></a>Konuk Yapılandırma tanımı gereksinimleri

Konuk Yapılandırma tarafından çalıştırılanan her denetim iki ilke tanımı, bir **DeployIfNotExists** tanımı ve bir **AuditIfNotExists** tanımı gerektirir. **DeployIfNotExists** tanımı, [doğrulama araçlarını](#validation-tools)desteklemek için makineyi Konuk Yapılandırma aracısı ve diğer bileşenlerle birlikte hazırlamak için kullanılır.

**DeployIfNotExists** ilke tanımı aşağıdaki öğeleri doğrular ve düzeltir:

- Makineyi doğrulamak değerlendirmek için bir yapılandırma atandı. Şu anda atama yoksa, atamayı alın ve makineyi şu şekilde hazırlayın:
  - Yönetilen bir [kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) kullanarak makineye kimlik doğrulama
  - **Microsoft.GuestConfiguration** uzantısının en son sürümünü yükleme
  - Gerekirse [doğrulama araçlarının](#validation-tools) ve bağımlılıkların yüklenmesi

**DeployIfNotExists** ataması uyumlu değilse, bir [düzeltme görevi](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

**DeployIfNotExists** ataması Uyumlu olduğunda, **AuditIfNotExists** ilkesi ataması yapılandırma atamasının Uyumlu olup olmadığını veya uyumlu olmadığını belirlemek için yerel doğrulama araçlarını kullanır. Doğrulama aracı, sonuçları Konuk Yapılandırma istemcisine sağlar. İstemci sonuçları Konuk Uzantısı'na iletir ve bu da onları Konuk Yapılandırma kaynak sağlayıcısı aracılığıyla kullanılabilir hale getirir.

Azure İlkesi, **Uyumluluk** düğümündeki uyumluluğu bildirmek için Konuk Yapılandırma kaynak sağlayıcıları **nasiasıdurumunu** kullanır. Daha fazla bilgi için [uyumluluk verilerini alma](../how-to/get-compliance-data.md)bilgisine bakın.

> [!NOTE]
> **AuditIfNotExists** ilkesinin sonuçları döndürmesi için **DeployIfNotExists** ilkesi gereklidir. **DeployIfNotExists**olmadan, **AuditIfNotExists** ilkesi "0/0" kaynaklarını durum olarak gösterir.

Konuk Yapılandırması için tüm yerleşik ilkeler, atamalarda kullanılacak tanımları gruplandırma girişimine dahildir. Yerleşik girişim _ \[Preview\]_ : Audit Password güvenlik ayarları linux ve Windows makineleri içinde 18 ilkeler içerir. Windows için altı **DeployIfNotExists** ve **AuditIfNotExists** çifti ve Linux için üç çift vardır. [İlke tanımı](definition-structure.md#policy-rule) mantığı, yalnızca hedef işletim sisteminin değerlendirildiğini doğrular.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Endüstri taban çizgilerini izleyerek işletim sistemi ayarlarını denetleme

Azure İlkesi'nde kullanılabilen girişimlerden biri, Microsoft'un "taban çizgisini" takiben sanal makinelerdeki işletim sistemi ayarlarını denetleme olanağı sağlar. Tanım, _ \[Önizleme\]: Azure güvenlik temel ayarlarıyla eşleşmeyan Denetim Windows VM'leri,_ Active Directory Group İlke'deki ayarlara dayalı tam bir denetim kuralları kümesi içerir.

Ayarların çoğu parametre olarak kullanılabilir. Bu işlev, ilkeyi kuruluş gereksinimlerinizle uyumlu hale getirmek veya politikayı endüstri düzenleyici standartları gibi üçüncü taraf bilgileriyle eşlemek için denetlenenleri özelleştirmenize olanak tanır.

Bazı parametreler bir karşıcı değer aralığını destekler. Örneğin, Maksimum Parola Yaşı parametresi, makine sahiplerine esneklik sağlamak için bir aralık işleci kullanılarak ayarlanabilir. Kullanıcıların parolalarını değiştirmelerini gerektiren etkili Grup İlkesi ayarının 70 günden fazla olmaması, ancak bir günden az olmaması gerektiğini denetleyebilirsiniz. Parametre için bilgi balonu açıklandığı gibi, bu iş ilkesi etkin denetim değeri yapmak için, "1,70" değerini ayarlayın.

İlkeyi Azure Kaynak Yöneticisi dağıtım şablonu kullanarak atarsanız, bu ayarları kaynak denetiminden yönetmek için bir parametre dosyası kullanabilirsiniz. Her iade belgelerinde yapılan açıklamalarla Denetim ilkelerinde yapılan değişiklikleri yönetmek için Git gibi bir araç kullanmak, bir atamanın neden beklenen değerin bir istisnası olması gerektiğine ilişkin kanıtlar.

#### <a name="applying-configurations-using-guest-configuration"></a>Konuk Yapılandırması'nı kullanarak yapılandırmaları uygulama

Azure İlkesi'nin en son özelliği, makinelerin içindeki ayarları yapılandırır. Tanım _Windows makinelerde saat dilimini yapılandırma,_ saat dilimini yapılandırarak makinede değişiklik yapar.

_Yapılandırma_ile başlayan tanımları atarken, _Windows VM'lerde Konuk Yapılandırma İlkesi'ni etkinleştirmek için ön koşulları dağıt_tanımını da atamanız gerekir. İsterseniz bu tanımları bir girişimde birleştirebilirsiniz.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Azure dışındaki makinelere ilkeler atama

Konuk Yapılandırma sı için kullanılabilir Denetim ilkeleri **Microsoft.HybridCompute/machines** kaynak türünü içerir. İlke ataması kapsamında olan [Sunucular için Azure Arc'a](../../../azure-arc/servers/overview.md) binen tüm makineler otomatik olarak dahil edilir.

### <a name="multiple-assignments"></a>Birden çok atama

Konuk Yapılandırma ilkeleri şu anda, İlke ataması farklı parametreler kullansa bile, makine başına yalnızca bir kez aynı Konuk Atamasını atamayı destekler.

## <a name="client-log-files"></a>İstemci günlük dosyaları

Konuk Yapılandırma uzantısı günlük dosyalarını aşağıdaki konumlara yazar:

Windows:`C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux:`/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

Nerede `<version>` geçerli sürüm numarası anlamına gelir.

### <a name="collecting-logs-remotely"></a>Günlükleri uzaktan toplama

Sorun giderme Konuk Yapılandırma yapılandırmaları veya modülleri ilk `Test-GuestConfigurationPackage` adım [Windows için özel bir Konuk Yapılandırma denetim ilkesi oluşturmak](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows)için nasıl adımları aşağıdaki cmdlet kullanmak olmalıdır.
Bu başarılı olmazsa, istemci günlüklerini toplamak sorunları tanılamaya yardımcı olabilir.

#### <a name="windows"></a>Windows

Windows makinelerindeki günlük dosyalarından bilgi almak için Azure VM Run Komutu özelliğini kullanmak için aşağıdaki örnek PowerShell komut dosyası yararlı olabilir. Daha fazla bilgi için Bkz. [Windows VM'nizde Çalıştır Komutu ile PowerShell komut dosyalarını çalıştırın.](../../../virtual-machines/windows/run-command.md)

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Linux makinelerindeki günlük dosyalarından bilgi almak için Azure VM Run Komutu özelliğini kullanmak için aşağıdaki örnek Bash komut dosyası yararlı olabilir. Daha fazla bilgi için, [Run Komutu ile Linux VM'nizde Shell komutlarını çalıştır](../../../virtual-machines/linux/run-command.md)

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Konuk Yapılandırma örnekleri

İlke Konuk Yapılandırması yerleşik girişimleri için kaynak aşağıdaki konumlarda mevcuttur:

- [Yerleşik ilke tanımları - Konuk Yapılandırması](../samples/built-in-policies.md#guest-configuration)
- [Dahili girişimler - Konuk Yapılandırması](../samples/built-in-initiatives.md#guest-configuration)
- [Azure İlkesi GitHub repo'yu örnekle](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
