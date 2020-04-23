---
title: Sanal makinelerin içeriğini denetlemeyi öğrenin
description: Azure İlke'nin sanal makinelerdeki ayarları denetlemek için Konuk Yapılandırma aracısını nasıl kullandığını öğrenin.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: 89f7cc3931971d70b441490f77b67ace89434c2b
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025229"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi'nin Konuk Yapılandırması'nı anlayın

Azure İlkesi, Azure kaynaklarını denetlemenin ve [düzeltmenin](../how-to/remediate-resources.md) ötesinde, bir makinenin içindeki ayarları denetleyebilir. Doğrulama, Konuk Yapılandırması uzantısı ve istemcisi tarafından gerçekleştirilir. Uzantı, istemci aracılığıyla şunun gibi ayarları doğrular:

- İşletim sisteminin yapılandırması
- Uygulama yapılandırması veya varlığı
- Ortam ayarları

Şu anda, çoğu Azure İlkesi Konuk Yapılandırma ilkesi yalnızca makine içindeki ayarları denetler. Yapılandırmaları uygulamıyorlar. Özel [durum, aşağıda başvurulan](#applying-configurations-using-guest-configuration)yerleşik bir ilkedir.

## <a name="resource-provider"></a>Kaynak sağlayıcısı

Konuk Yapılandırmasını kullanabileniz için kaynak sağlayıcısını kaydetmeniz gerekir. Konuk Yapılandırma ilkesinin atanması portal üzerinden yapılırsa kaynak sağlayıcısı otomatik olarak kaydedilir. [Portal](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure PowerShell](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell)veya [Azure CLI](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli)üzerinden el ile kaydolabilirsiniz.

## <a name="extension-and-client"></a>Uzantı ve istemci

Makine içindeki ayarları denetlemek için [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir. Uzantı, geçerli ilke atamasını ve ilgili yapılandırma tanımını karşıdan yükler.

> [!Important]
> Konuk Yapılandırma uzantısı, Azure sanal makinelerinde denetim ler gerçekleştirmek için gereklidir.
> Uzantıyı ölçekte dağıtmak için aşağıdaki ilke tanımlarını atayın:
>   - [Windows VM'lerde Konuk Yapılandırma İlkesi'ni etkinleştirmek için ön koşulları dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ecd903d-91e7-4726-83d3-a229d7f2e293)
>   - [Linux VM'lerde Konuk Yapılandırma Politikası'nı etkinleştirmek için ön koşulları dağıtın.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb27e9e0-526e-4ae1-89f2-a2a0bf0f8a50)

### <a name="limits-set-on-the-extension"></a>Uzantıda belirlenen sınırlar

Uzantının makine içinde çalışan uygulamaları etkilemesini sınırlamak için, Konuk Yapılandırması'nın CPU'nun %5'inden fazlasını aşmasına izin verilmez. Bu sınırlama hem yerleşik hem de özel tanımlar için vardır.

### <a name="validation-tools"></a>Doğrulama araçları

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

Konuk Yapılandırma ilkeleri yeni sürümleri kapsar. Konuk Yapılandırma aracısı uyumlu değilse, Azure pazarda kullanılabilen işletim sistemlerinin eski sürümleri hariç tutulur. Aşağıdaki tablo, Azure görüntülerinde desteklenen işletim sistemlerinin bir listesini gösterir:

|Yayımcı|Adı|Sürümler|
|-|-|-|
|Canonical|Ubuntu Server|14.04 ve sonrası|
|Credativ|Debian|8 ve sonrası|
|Microsoft|Windows Server|2012 ve sonrası|
|Microsoft|Windows İstemcisi|Windows 10|
|OpenLogic|CentOS|7.3 ve sonrası|
|Red Hat|Red Hat Enterprise Linux|7.4 ve sonrası|
|Suse|SLES|12 SP3 ve sonrası|

Özel sanal makine görüntüleri, yukarıdaki tablodaki işletim sistemlerinden biri oldukları sürece Konuk Yapılandırma ilkeleri tarafından desteklenir.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Windows Server Nano Server hiçbir sürümde desteklenmez.

## <a name="guest-configuration-extension-network-requirements"></a>Konuk Yapılandırma Uzantısı ağ gereksinimleri

Azure'daki Konuk Yapılandırma kaynak sağlayıcısıyla iletişim kurmak için **makineler, 443**bağlantı noktasındaki Azure veri merkezlerine giden erişim gerektirir. Azure'daki bir ağ giden trafiğe izin vermiyorsa, özel durumları [Ağ Güvenlik Grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kurallarıyla yapılandırın.
"GuestAndHybridManagement" [hizmet etiketi](../../../virtual-network/service-tags-overview.md) Konuk Yapılandırma hizmetine başvurmak için kullanılabilir.

## <a name="azure-managed-identity-requirements"></a>Azure yönetilen kimlik gereksinimleri

Uzantıyı sanal makinelere ekleyen **DeployIfNotExists** ilkeleri, yoksa yönetilen kimlik atanan bir sistemi de etkinleştirir.

> [!WARNING]
> Sistem yönetilen kimlik atanan etkinleştirmek ilkeleri için kapsamda sanal makinelere kullanıcı atanan yönetilen kimlik etkinleştirme kaçının. Atanan kullanıcı kimliği değiştirilir ve makine yanıt vermemiş olabilir.

## <a name="guest-configuration-definition-requirements"></a>Konuk Yapılandırma tanımı gereksinimleri

Konuk Yapılandırma tarafından çalıştırılanan her denetim iki ilke tanımı, bir **DeployIfNotExists** tanımı ve bir **AuditIfNotExists** tanımı gerektirir. 

**DeployIfNotExists** ilke tanımı aşağıdaki öğeleri doğrular ve düzeltir:

- Makineyi doğrulamak değerlendirmek için bir yapılandırma atandı. Şu anda atama yoksa, atamayı alın ve makineyi şu şekilde hazırlayın:
  - Yönetilen bir [kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) kullanarak makineye kimlik doğrulama
  - **Microsoft.GuestConfiguration** uzantısının en son sürümünü yükleme
  - Gerekirse [doğrulama araçlarının](#validation-tools) ve bağımlılıkların yüklenmesi

**DeployIfNotExists** ataması uyumlu değilse, bir [düzeltme görevi](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

**DeployIfNotExists** ataması Uyumlu olduğunda, **AuditIfNotExists** ilke ataması konuk atamanın Uyumlu olup olmadığını belirler. Doğrulama aracı, sonuçları Konuk Yapılandırma istemcisine sağlar. İstemci sonuçları Konuk Uzantısı'na iletir ve bu da onları Konuk Yapılandırma kaynak sağlayıcısı aracılığıyla kullanılabilir hale getirir.

Azure İlkesi, **Uyumluluk** düğümündeki uyumluluğu bildirmek için Konuk Yapılandırma kaynak sağlayıcıları **nasiasıdurumunu** kullanır. Daha fazla bilgi için [uyumluluk verilerini alma](../how-to/get-compliance-data.md)bilgisine bakın.

> [!NOTE]
> **AuditIfNotExists** ilkesinin sonuçları döndürmesi için **DeployIfNotExists** ilkesi gereklidir. **DeployIfNotExists**olmadan, **AuditIfNotExists** ilkesi "0/0" kaynaklarını durum olarak gösterir.

Konuk Yapılandırması için tüm yerleşik ilkeler, atamalarda kullanılacak tanımları gruplandırma girişimine dahildir. Yerleşik girişim _ \[\]Preview_ : Audit Password security in Linux ve Windows makineleri 18 ilke içerir. Windows için altı **DeployIfNotExists** ve **AuditIfNotExists** çifti ve Linux için üç çift vardır. [İlke tanımı](definition-structure.md#policy-rule) mantığı, yalnızca hedef işletim sisteminin değerlendirildiğini doğrular.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Endüstri taban çizgilerini izleyerek işletim sistemi ayarlarını denetleme

Azure İlkesi'ndeki bir girişim, işletim sistemi ayarlarını bir "temel" izleyerek denetleme olanağı sağlar. Tanım, _ \[Önizleme\]: Azure güvenlik taban çizgisi ayarlarıyla eşleşmeyan Denetim Windows VM'leri,_ Active Directory Group İlke'yi temel alan bir kural kümesi içerir.

Ayarların çoğu parametre olarak kullanılabilir. Parametreler denetlenenleri özelleştirmenize olanak sağlar. Politikayı gereksinimlerinizle hizalayın veya politikayı endüstri düzenleyici standartları gibi üçüncü taraf bilgileriyle eşleyin.

Bazı parametreler bir karşıcı değer aralığını destekler. Örneğin, Maksimum Parola Yaşı ayarı etkili Grup İlkesi ayarını denetleyebilir. "1,70" aralığı, kullanıcıların parolalarını en az 70 günde bir, ancak en az bir günde bir değiştirmeleri gerektiğini doğrular.

İlkeyi Azure Kaynak Yöneticisi dağıtım şablonu kullanarak atarsanız, özel durumları yönetmek için bir parametre dosyası kullanın. Dosyaları Git gibi bir sürüm kontrol sistemine iade edin. Dosya değişiklikleri yle ilgili yorumlar, bir atamanın neden beklenen değerin bir istisnası olduğuna kanıt sağlar.

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

[Azure VM Run Komutu'nu](../../../virtual-machines/windows/run-command.md)kullanarak günlük dosyalarından bilgi alın , aşağıdaki örnek PowerShell komut dosyası yararlı olabilir.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

[Azure VM Run Komutu'nu](../../../virtual-machines/linux/run-command.md)kullanarak günlük dosyalarından bilgi yakalayın , aşağıdaki örnek Bash komut dosyası yararlı olabilir.

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Konuk Yapılandırma örnekleri

Konuk Yapılandırma yerleşik ilke örnekleri aşağıdaki konumlarda mevcuttur:

- [Yerleşik ilke tanımları - Konuk Yapılandırması](../samples/built-in-policies.md#guest-configuration)
- [Dahili girişimler - Konuk Yapılandırması](../samples/built-in-initiatives.md#guest-configuration)
- [Azure İlkesi GitHub repo'yu örnekle](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk Yapılandırmauyumluluk görünümünden](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration) her ayarın ayrıntılarını nasıl görüntüleyin
- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
