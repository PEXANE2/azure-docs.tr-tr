---
title: Sanal makinelerin içeriğini denetleme hakkında bilgi edinin
description: Azure Ilkesi 'nin sanal makineler içindeki ayarları denetlemek için konuk yapılandırma aracısını nasıl kullandığını öğrenin.
ms.date: 05/20/2020
ms.topic: conceptual
ms.openlocfilehash: 81c8c642eb8b5da1e45e4d9a703685acf219ca5a
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362637"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi’nin Konuk Yapılandırmasını anlama

Azure Ilkesi, hem Azure 'da hem de [yay bağlantılı makinelerde](https://docs.microsoft.com/azure/azure-arc/servers/overview)çalışan makineler için bir makine içindeki ayarları denetleyebilir.
Doğrulama, Konuk Yapılandırması uzantısı ve istemcisi tarafından gerçekleştirilir. Uzantı, istemci aracılığıyla şunun gibi ayarları doğrular:

- İşletim sisteminin yapılandırması
- Uygulama yapılandırması veya varlığı
- Ortam ayarları

Şu anda, çoğu Azure Ilkesi Konuk yapılandırma ilkesi yalnızca makinenin içindeki ayarları denetler.
Yapılandırma uygulamamaları. Özel durum [aşağıda başvurulan](#applying-configurations-using-guest-configuration)bir yerleşik ilkedir.

## <a name="enable-guest-configuration"></a>Konuk yapılandırmasını etkinleştir

Azure 'daki makineler ve yay bağlantılı makineler dahil ortamınızdaki makinelerin durumunu denetlemek için aşağıdaki ayrıntıları gözden geçirin.

## <a name="resource-provider"></a>Kaynak sağlayıcısı

Konuk yapılandırması 'nı kullanabilmeniz için önce kaynak sağlayıcısını kaydetmeniz gerekir. Bir konuk yapılandırma ilkesinin atanması Portal üzerinden yapıldığında kaynak sağlayıcı otomatik olarak kaydedilir. [Portal](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure POWERSHELL](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell)veya [Azure CLI](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli)aracılığıyla el ile kayıt yapabilirsiniz.

## <a name="deploy-requirements-for-azure-virtual-machines"></a>Azure sanal makineleri için gereksinimleri dağıtma

Bir makine içindeki ayarları denetlemek için, bir [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir ve makinede sistem tarafından yönetilen bir kimlik olması gerekir. Uzantı, uygulanabilir ilke atamasını ve ilgili yapılandırma tanımını indirir. Kimlik, Konuk yapılandırma hizmetine okuduğu ve yazdığı makinenin kimliğini doğrulamak için kullanılır. Arc bağlantılı makine aracısına eklendiğinden, bu uzantı, Arc bağlantılı makineler için gerekli değildir.

> [!IMPORTANT]
> Azure sanal makinelerini denetlemek için konuk yapılandırma uzantısı ve yönetilen bir kimlik gereklidir. Uzantıyı ölçekli olarak dağıtmak için aşağıdaki ilke girişim atamasını yapın: 
>  - [Sanal makinelerde Konuk yapılandırma ilkelerini etkinleştirmek için önkoşulları dağıtın](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8)

### <a name="limits-set-on-the-extension"></a>Uzantı üzerinde ayarlanan sınırlar

Uzantının makinede çalışan etkileyen uygulamalarla sınırlandırılacağından, Konuk yapılandırmasının CPU 'nun %5 ' inden fazlasını aşmasına izin verilmez. Bu sınırlama hem yerleşik hem de özel tanımlar için vardır. Aynı değer, yay bağlantılı makine aracısındaki Konuk yapılandırma hizmeti için de geçerlidir.

### <a name="validation-tools"></a>Doğrulama araçları

Makinenin içinde, Konuk yapılandırma istemcisi denetimi çalıştırmak için yerel araçları kullanır.

Aşağıdaki tabloda, desteklenen her bir işletim sisteminde kullanılan yerel araçların bir listesi gösterilmektedir. Yerleşik içerikte, Konuk yapılandırma bu araçları otomatik olarak yüklemeyi işler.

|İşletim sistemi|Doğrulama Aracı|Notlar|
|-|-|-|
|Windows|[PowerShell Istenen durum yapılandırması](/powershell/scripting/dsc/overview/overview) v2| Yan yana yalnızca Azure Ilkesi tarafından kullanılan bir klasöre yüklenir. Windows PowerShell DSC ile çakışmaz. PowerShell Core, sistem yoluna eklenmedi.|
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Chef InSpec Version 2.2.61 'i varsayılan konuma yükleyip sistem yoluna eklenir. Ruby ve Python dahil InSpec paketine yönelik bağımlılıklar da yüklenir. |

### <a name="validation-frequency"></a>Doğrulama sıklığı

Konuk yapılandırması istemcisi, her 5 dakikada bir yeni içerik denetler. Konuk ataması alındıktan sonra, bu yapılandırmanın ayarları 15 dakikalık bir aralıkta yeniden denetlenir. Sonuçlar, denetim tamamlandığında Konuk yapılandırma kaynak sağlayıcısına gönderilir. Bir ilke [değerlendirme tetikleyicisi](../how-to/get-compliance-data.md#evaluation-triggers) gerçekleştiğinde, makinenin durumu Konuk yapılandırma kaynak sağlayıcısına yazılır. Bu güncelleştirme, Azure Ilkesinin Azure Resource Manager özelliklerini değerlendirmesini sağlar. İsteğe bağlı bir Azure Ilke değerlendirmesi, Konuk yapılandırma kaynak sağlayıcısından en son değeri alır. Ancak, makine içindeki yapılandırmanın yeni bir denetimini tetiklemez.

## <a name="supported-client-types"></a>Desteklenen istemci türleri

Konuk yapılandırma ilkelerine yeni sürümler dahildir. Konuk yapılandırma Aracısı uyumlu değilse, Azure Marketi 'nde bulunan daha eski işletim sistemi sürümleri hariç tutulur.
Aşağıdaki tabloda, Azure görüntülerinde desteklenen işletim sistemlerinin bir listesi gösterilmektedir:

|Publisher|Name|Sürümler|
|-|-|-|
|Canonical|Ubuntu Server|14,04 ve üzeri|
|Credavtiv|Debian|8 ve üzeri|
|Microsoft|Windows Server|2012 ve üzeri|
|Microsoft|Windows İstemcisi|Windows 10|
|OpenLogic|CentOS|7,3 ve üzeri|
|Red Hat|Red Hat Enterprise Linux|7,4 ve üzeri|
|SUSE|SLES|12 SP3 ve üzeri|

Özel sanal makine görüntüleri, Konuk yapılandırma ilkeleri tarafından, yukarıdaki tablodaki işletim sistemlerinden biri oldukları sürece desteklenir.

## <a name="guest-configuration-extension-network-requirements"></a>Konuk yapılandırma uzantısı ağ gereksinimleri

Azure 'daki Konuk yapılandırma kaynak sağlayıcısıyla iletişim kurmak için makineler **443**numaralı bağlantı noktasında Azure veri merkezlerine giden erişim gerektirir. Azure 'daki bir ağ giden trafiğe izin vermezse, [ağ güvenlik grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kuralları ile özel durumlar yapılandırın. "Guestandhybridmanagement" [hizmet etiketi](../../../virtual-network/service-tags-overview.md) , Konuk yapılandırma hizmetine başvurmak için kullanılabilir.

## <a name="managed-identity-requirements"></a>Yönetilen kimlik gereksinimleri

Girişim [dağıtım önkoşulları sanal makinelerde Konuk yapılandırma ilkelerini etkinleştirmek için](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8) ilke yoksa, sistem tarafından atanan bir yönetilen kimliği etkinleştirir. Girişimde kimlik oluşturmayı yöneten iki ilke tanımı vardır. İlke tanımlarındaki koşullar, Azure 'daki makine kaynağının geçerli durumuna bağlı olarak doğru davranışı güvence altına alır.

Makinenin Şu anda herhangi bir yönetilen kimliği yoksa, etkin ilke şu şekilde olacaktır: [ \[ Önizleme \] : kimliği olmayan sanal makinelerde Konuk yapılandırma atamalarını etkinleştirmek için sistem tarafından atanan yönetilen kimlik ekleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3cf2ab00-13f1-4d0c-8971-2ac904541a7e)

Makinenin Şu anda Kullanıcı tarafından atanan bir sistem kimliği varsa, etkin ilke şu şekilde olacaktır: [ \[ Önizleme \] : Kullanıcı tarafından atanan bir kimliğe sahip sanal makinelerde Konuk yapılandırma atamalarını etkinleştirmek için sistem tarafından atanan yönetilen kimlik ekleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F497dff13-db2a-4c0f-8603-28fa3b331ab6)

## <a name="guest-configuration-definition-requirements"></a>Konuk yapılandırma tanımı gereksinimleri

Konuk yapılandırması tarafından çalıştırılan her denetim, bir **Deployifnotexists** tanımı ve bir **auditınotexists** tanımı olmak üzere iki ilke tanımı gerektirir. **Deployifnotexists** ilke tanımları, her makinede denetim gerçekleştirmeye yönelik bağımlılıkları yönetir.

**Deployifnotexists** ilke tanımı, aşağıdaki öğeleri doğrular ve düzeltir:

- Makineye değerlendirmek için bir yapılandırma atandığını doğrulayın. Şu anda bir atama yoksa, atamayı alın ve makineyi şu şekilde hazırlayın:
  - [Yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) kullanarak makinede kimlik doğrulama
  - **Microsoft. GuestConfiguration** uzantısının en son sürümünü yükleme
  - Gerekirse [doğrulama araçlarını](#validation-tools) ve bağımlılıklarını yükleme

**Deployifnotexists** ataması uyumlu değilse, bir [Düzeltme görevi](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

**Deployifnotexists** ataması uyumluysa, **auditınotexists** ilke ataması, Konuk atamasının uyumlu veya uyumsuz olduğunu belirler. Doğrulama Aracı, sonuçları Konuk yapılandırma istemcisine sağlar. İstemci, sonuçları Konuk yapılandırması kaynak sağlayıcısı aracılığıyla kullanılabilir hale getiren konuk uzantısına iletir.

Azure Ilkesi, **Uyumluluk** düğümündeki uyumluluğu raporlamak Için Konuk yapılandırma kaynak sağlayıcıları **karmaşık ancestatus** özelliğini kullanır. Daha fazla bilgi için bkz. [uyumluluk verilerini alma](../how-to/get-compliance-data.md).

> [!NOTE]
> **Uıınotexists** ilkesi, sonuçları döndürmek için bu **ilke için** gereklidir. **Deployifnotexists**olmadan, **auditınotexists** ilkesi "0/0" kaynağını durum olarak gösterir.

Konuk yapılandırması için tüm yerleşik ilkeler, atamalarda kullanılmak üzere tanımları gruplamak için bir girişimde bulunur. Preview adlı yerleşik girişim _ \[ \] : Linux ve Windows makineler içindeki denetim parolası güvenliği_ 18 ilke içerir. Windows için altı **Deployifnotexists** ve **Auditınotexists** çiftleri ve Linux için üç çift vardır. [İlke tanımı](definition-structure.md#policy-rule) mantığı yalnızca hedef işletim sisteminin değerlendirildiğini doğrular.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Sektör temellerini izleyen işletim sistemi ayarlarını denetleme

Azure Ilkesindeki bir girişim, "taban çizgisi" ile işletim sistemi ayarlarını denetleme olanağı sağlar. Tanım, _ \[ Önizleme \] : Azure Güvenlik taban çizgisi ayarlarıyla eşleşmeyen Windows sanal makinelerini denetleme_ , Active Directory Grup İlkesi dayalı bir kurallar kümesi içerir.

Ayarların çoğu parametre olarak kullanılabilir. Parametreler, denetlendiğini özelleştirmenizi sağlar.
İlkeyi gereksinimlerinize göre hizalayın veya ilkeyi sektör mevzuatı standartları gibi üçüncü taraf bilgilerle eşleyin.

Bazı parametreler bir tamsayı değer aralığını destekler. Örneğin, en fazla parola yaşı ayarı geçerli grup ilkesi ayarını denetleyebilir. "1, 70" bir Aralık, kullanıcıların parolalarını en az 70 günde bir, ancak bir günden az değiştirmeme gerektiğini doğrulayacağından emin olur.

İlkeyi bir Azure Resource Manager dağıtım şablonu kullanarak atarsanız, özel durumları yönetmek için bir parametre dosyası kullanın. Dosyaları git gibi bir sürüm denetim sistemine iade edin. Dosya değişiklikleriyle ilgili açıklamalar, bir atamanın beklenen değer için bir özel durum olduğunu kanıtlamadır.

#### <a name="applying-configurations-using-guest-configuration"></a>Konuk yapılandırması kullanılarak yapılandırmaları uygulama

Azure Ilkesinin en son özelliği makineler içindeki ayarları yapılandırır. _Windows makinelerinde saat dilimini yapılandırma_ tanımı, saat dilimini yapılandırarak makinede değişiklikler yapar.

_Yapılandırma_ile başlayan tanımları atarken, _Windows VM 'Lerde Konuk yapılandırma ilkesini etkinleştirmek için tanım dağıtma ön koşullarını_da atamanız gerekir. Seçeneğini belirlerseniz bu tanımları bir girişim içinde birleştirebilirsiniz.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Azure dışındaki makinelere ilke atama

Konuk yapılandırması için kullanılabilen denetim ilkeleri **Microsoft. HybridCompute/machines** kaynak türünü içerir. İlke atamasının kapsamındaki [sunucular Için Azure yaya](../../../azure-arc/servers/overview.md) eklendi tüm makineler otomatik olarak eklenir.

### <a name="multiple-assignments"></a>Birden çok atama

Konuk yapılandırma ilkeleri Şu anda yalnızca makine başına aynı Konuk atamasının atanmasını, Ilke ataması farklı parametreler kullanıyor olsa bile destekler.

## <a name="client-log-files"></a>İstemci günlük dosyaları

Konuk yapılandırma uzantısı, günlük dosyalarını aşağıdaki konumlara Yazar:

Pencerelerin`C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

'Un`/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

`<version>`, Geçerli sürüm numarasını belirtir.

### <a name="collecting-logs-remotely"></a>Günlükleri uzaktan toplama

Konuk yapılandırma yapılandırmalarının veya modüllerinin sorunlarını gidermenin ilk adımı, `Test-GuestConfigurationPackage` [Windows Için özel konuk yapılandırma denetim ilkesi oluşturma](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows)adımlarını izleyerek cmdlet 'i kullanmalıdır.
Bu başarılı olmazsa, istemci günlüklerinin toplanması sorunları tanılamanıza yardımcı olabilir.

#### <a name="windows"></a>Windows

[Azure VM Run komutunu](../../../virtual-machines/windows/run-command.md)kullanarak günlük dosyalarından bilgi yakala, aşağıdaki örnek PowerShell betiği yararlı olabilir.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

[Azure VM Run komutunu](../../../virtual-machines/linux/run-command.md)kullanarak günlük dosyalarından bilgi yakala, aşağıdaki örnek Bash betiği yararlı olabilir.

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Konuk yapılandırma örnekleri

Konuk yapılandırması yerleşik ilke örnekleri aşağıdaki konumlarda bulunur:

- [Yerleşik ilke tanımları-Konuk yapılandırma](../samples/built-in-policies.md#guest-configuration)
- [Yerleşik girişimler-Konuk yapılandırma](../samples/built-in-initiatives.md#guest-configuration)
- [Azure Ilke örnekleri GitHub deposu](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk yapılandırması uyumluluk görünümünden](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration) her ayarın ayrıntılarını görüntülemeyi öğrenin
- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
