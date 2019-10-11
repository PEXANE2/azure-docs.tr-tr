---
title: Sanal makinelerin içeriğini denetleme hakkında bilgi edinin
description: Azure Ilkesi 'nin bir Azure makinesi içindeki ayarları denetlemek için konuk yapılandırmasını nasıl kullandığını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/20/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 82279e6937fccfbbef13f9580f76cd344593b0df
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255838"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure Ilkesinin Konuk yapılandırmasını anlama

Azure Ilkesi, Azure kaynaklarını denetlemeye ve yeniden [düzeltmelere](../how-to/remediate-resources.md) göre, bir makine içindeki ayarları denetleyebilir. Doğrulama, Konuk yapılandırma uzantısı ve istemcisi tarafından gerçekleştirilir. İstemci aracılığıyla uzantı, şu gibi ayarları doğrular:

- İşletim sisteminin yapılandırması
- Uygulama yapılandırması veya varlığı
- Ortam ayarları

Azure Ilkesi Konuk yapılandırması şu anda yalnızca makinenin içindeki ayarları denetler. Yapılandırma uygulamaz.

## <a name="extension-and-client"></a>Uzantı ve istemci

Bir makine içindeki ayarları denetlemek için, bir [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkindir. Uzantı, uygulanabilir ilke atamasını ve ilgili yapılandırma tanımını indirir.

### <a name="limits-set-on-the-extension"></a>Uzantı üzerinde ayarlanan sınırlar

Uzantının makinede çalışan etkileyen uygulamalarla sınırlandırılacağından, Konuk yapılandırmasının CPU kullanımının% 5 ' inden fazla olmasına izin verilmez. Bu sınırlama hem yerleşik hem de özel tanımlar için vardır.

## <a name="register-guest-configuration-resource-provider"></a>Konuk yapılandırma kaynak sağlayıcısını Kaydet

Konuk yapılandırması 'nı kullanabilmeniz için önce kaynak sağlayıcısını kaydetmeniz gerekir. Portala veya PowerShell aracılığıyla kaydolabilirsiniz. Bir konuk yapılandırma ilkesinin atanması Portal üzerinden yapıldığında kaynak sağlayıcı otomatik olarak kaydedilir.

### <a name="registration---portal"></a>Kayıt-Portal

Azure portal aracılığıyla konuk yapılandırma kaynak sağlayıcısını kaydetmek için aşağıdaki adımları izleyin:

1. Azure portal başlatın ve **tüm hizmetler**' e tıklayın. **Abonelik**arayın ve seçin.

1. Konuk yapılandırmasını etkinleştirmek istediğiniz aboneliği bulun ve tıklatın.

1. **Abonelik** sayfasının sol menüsünde **kaynak sağlayıcıları**' na tıklayın.

1. **Microsoft. GuestConfiguration**öğesini bulana kadar filtre uygulayın veya kaydırın, ardından aynı satırda **Kaydet** ' e tıklayın.

### <a name="registration---powershell"></a>Kayıt-PowerShell

PowerShell aracılığıyla konuk yapılandırma kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

## <a name="validation-tools"></a>Doğrulama araçları

Makinenin içinde, Konuk yapılandırma istemcisi denetimi çalıştırmak için yerel araçları kullanır.

Aşağıdaki tabloda, desteklenen her bir işletim sisteminde kullanılan yerel araçların bir listesi gösterilmektedir:

|İşletim sistemi|Doğrulama Aracı|Notlar|
|-|-|-|
|Windows|[Microsoft Istenen durum yapılandırması](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby ve Python, Konuk yapılandırma uzantısı tarafından yüklenir. |

### <a name="validation-frequency"></a>Doğrulama sıklığı

Konuk yapılandırması istemcisi, her 5 dakikada bir yeni içerik denetler. Konuk ataması alındıktan sonra ayarlar 15 dakikalık bir aralıkta denetlenir. Sonuçlar, Denetim tamamlandıktan hemen sonra Konuk yapılandırma kaynak sağlayıcısına gönderilir. Bir ilke [değerlendirme tetikleyicisi](../how-to/get-compliance-data.md#evaluation-triggers) gerçekleştiğinde, makinenin durumu Konuk yapılandırma kaynak sağlayıcısına yazılır. Bu güncelleştirme, Azure Ilkesinin Azure Resource Manager özelliklerini değerlendirmesini sağlar. İsteğe bağlı bir Azure Ilke değerlendirmesi, Konuk yapılandırma kaynak sağlayıcısından en son değeri alır. Ancak, makine içindeki yapılandırmanın yeni bir denetimini tetiklemez.

## <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, Azure görüntülerinde desteklenen işletim sisteminin bir listesi gösterilmektedir:

|Publisher|Name|Sürümler|
|-|-|-|
|Kurallı|Ubuntu Server|14,04, 16,04, 18,04|
|credavtiv|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Windows Istemcisi|Windows 10|
|OpenLogic|CentOS|7,3, 7,4, 7,5|
|Red Hat|Red Hat Enterprise Linux|7,4, 7,5|
|SUSE|SLES|12 SP3|

> [!IMPORTANT]
> Konuk yapılandırması, desteklenen bir işletim sistemi çalıştıran düğümleri denetleyebilir. Özel bir görüntü kullanan sanal makineleri denetlemek isterseniz, **Deployifnotexists** tanımını çoğaltmanız ve resim özelliklerinizi dahil etmek için **IF** bölümünü değiştirmeniz gerekir.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Windows Server nano Server hiçbir sürümde desteklenmez.

## <a name="guest-configuration-extension-network-requirements"></a>Konuk yapılandırma uzantısı ağ gereksinimleri

Azure 'daki Konuk yapılandırma kaynak sağlayıcısıyla iletişim kurmak için makineler **443**numaralı bağlantı noktasında Azure veri merkezlerine giden erişim gerektirir. Azure 'da giden trafiğe izin veren özel bir sanal ağ kullanıyorsanız, [ağ güvenlik grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kuralları ile özel durumlar yapılandırın. Azure Ilke Konuk yapılandırması için şu anda bir hizmet etiketi yok.

IP adresi listelerinde [Microsoft Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıklar ve IP aralıklarında yaklaşan değişiklikler vardır. Yalnızca sanal makinelerinizin dağıtıldığı bölgelerdeki IP 'lere giden erişime izin vermeniz gerekir.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. Dosya işlem, SQL ve depolama aralıklarını içerir. Güncelleştirilmiş bir dosya haftalık olarak gönderilir. Dosya, şu anda dağıtılmış aralıkları ve IP aralıklarında yaklaşan değişiklikleri yansıtır. Dosyada görüntülenen yeni aralıklar, en az bir hafta için veri merkezlerinde kullanılmıyor. Her hafta yeni XML dosyasını indirmek iyi bir fikirdir. Daha sonra, sitenizi Azure 'da çalışan hizmetleri doğru şekilde belirlemek için güncelleştirin. Azure ExpressRoute kullanıcıları bu dosyanın, her ayın ilk haftasında Azure Space Sınır Ağ Geçidi Protokolü (BGP) tanıtımını güncelleştirmek için kullanıldığını unutmayın.

## <a name="guest-configuration-definition-requirements"></a>Konuk yapılandırma tanımı gereksinimleri

Konuk yapılandırması tarafından çalıştırılan her denetim, bir **Deployifnotexists** tanımı ve bir **auditınotexists** tanımı olmak üzere iki ilke tanımı gerektirir. **Deployifnotexists** tanımı, makineyi Konuk yapılandırma Aracısı ve diğer bileşenleri [doğrulama araçlarını](#validation-tools)destekleyecek şekilde hazırlamak için kullanılır.

**Deployifnotexists** ilke tanımı, aşağıdaki öğeleri doğrular ve düzeltir:

- Makineye değerlendirmek için bir yapılandırma atandığını doğrulayın. Şu anda bir atama yoksa, atamayı alın ve makineyi şu şekilde hazırlayın:
  - [Yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) kullanarak makinede kimlik doğrulama
  - **Microsoft. GuestConfiguration** uzantısının en son sürümünü yükleme
  - Gerekirse [doğrulama araçlarını](#validation-tools) ve bağımlılıklarını yükleme

**Deployifnotexists** ataması uyumlu değilse, bir [Düzeltme görevi](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

**Deployifnotexists** ataması uyumlu olduğunda, **auditınotexists** ilke ataması, yapılandırma atamasının uyumlu veya uyumsuz olduğunu anlamak için yerel doğrulama araçlarını kullanır. Doğrulama Aracı, sonuçları Konuk yapılandırma istemcisine sağlar. İstemci, sonuçları Konuk yapılandırması kaynak sağlayıcısı aracılığıyla kullanılabilir hale getiren konuk uzantısına iletir.

Azure Ilkesi, **Uyumluluk** düğümündeki uyumluluğu raporlamak Için Konuk yapılandırma kaynak sağlayıcıları **karmaşık ancestatus** özelliğini kullanır. Daha fazla bilgi için bkz. [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).

> [!NOTE]
> **Uıınotexists** ilkesi, sonuçları döndürmek için bu **ilke için** gereklidir. **Deployifnotexists**olmadan, **auditınotexists** ilkesi "0/0" kaynağını durum olarak gösterir.

Konuk yapılandırması için tüm yerleşik ilkeler, atamalarda kullanılmak üzere tanımları gruplamak için bir girişimde bulunur. [Önizleme] adlı yerleşik girişim *: Linux ve Windows makineler Içindeki denetim parolası güvenlik ayarları* 18 ilke içerir. Windows için altı **Deployifnotexists** ve **Auditınotexists** çiftleri ve Linux için üç çift vardır. [İlke tanımı](definition-structure.md#policy-rule) mantığı yalnızca hedef işletim sisteminin değerlendirildiğini doğrular.

### <a name="multiple-assignments"></a>Birden çok atama

Konuk yapılandırma ilkeleri Şu anda yalnızca makine başına aynı Konuk atamasının atanmasını, Ilke ataması farklı parametreler kullanıyor olsa bile destekler.

## <a name="built-in-resource-modules"></a>Yerleşik kaynak modülleri

Konuk yapılandırma uzantısı yüklenirken, ' GuestConfiguration ' PowerShell modülü DSC kaynak modüllerinin en son sürümüne dahildir. Modül sayfası [Guestconfiguration](https://www.powershellgallery.com/packages/GuestConfiguration/)' dan ' el ile indirme ' bağlantısı kullanılarak bu modül PowerShell Galerisi indirilebilir.
'. Nupkg ' dosya biçimi, sıkıştırmayı açmak ve gözden geçirmek için '. zip ' olarak yeniden adlandırılabilir.

## <a name="client-log-files"></a>İstemci günlük dosyaları

Konuk yapılandırma uzantısı, günlük dosyalarını aşağıdaki konumlara Yazar:

Windows: `C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\<version>\dsc\logs\dsc.log`

Linux: `/var/lib/waagent/Microsoft.GuestConfiguration.ConfigurationforLinux-<version>/GCAgent/logs/dsc.log`

@No__t-0 geçerli sürüm numarasını gösterir.

### <a name="collecting-logs-remotely"></a>Günlükleri uzaktan toplama

Konuk yapılandırma yapılandırmalarının veya modülleriyle ilgili sorunları gidermeye yönelik ilk adım, [Konuk yapılandırma paketini test](../how-to/guest-configuration-create.md#test-a-guest-configuration-package)etme bölümündeki adımları izleyerek `Test-GuestConfigurationPackage` cmdlet 'ini kullanmalıdır.
Bu başarılı olmazsa, istemci günlüklerinin toplanması sorunları tanılamanıza yardımcı olabilir.

#### <a name="windows"></a>Windows

Windows makinelerdeki günlük dosyalarından bilgi yakalamak üzere Azure VM çalıştırma komutu özelliğini kullanmak için aşağıdaki örnek PowerShell betiği yararlı olabilir. Daha fazla bilgi için bkz. [Run komutuyla WINDOWS sanal makinenizde PowerShell betikleri çalıştırma](../../../virtual-machines/windows/run-command.md).

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$latestVersion = Get-ChildItem -Path 'C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\' | ForEach-Object {$_.FullName} | Sort-Object -Descending | Select-Object -First 1
Select-String -Path "$latestVersion\dsc\logs\dsc.log" -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Linux makinelerdeki günlük dosyalarından bilgi yakalamak için Azure VM çalıştırma komutunu kullanmak üzere aşağıdaki örnek Bash betiği yararlı olabilir. Daha fazla bilgi için bkz. [Run komutuyla LINUX sanal makinenizde Shell betikleri çalıştırma](../../../virtual-machines/linux/run-command.md)

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
latestVersion=$(find /var/lib/waagent/ -type d -name "Microsoft.GuestConfiguration.ConfigurationforLinux-*" -maxdepth 1 -print | sort -z | sed -n 1p)
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' "$latestVersion/GCAgent/logs/dsc.log" | tail
```

## <a name="guest-configuration-samples"></a>Konuk yapılandırma örnekleri

Ilke Konuk yapılandırması için örnekler aşağıdaki konumlarda kullanılabilir:

- [Örnek dizini-Konuk yapılandırması](../samples/index.md#guest-configuration)
- [Azure Ilke örnekleri GitHub deposu](https://github.com/Azure/azure-policy/tree/master/samples/GuestConfiguration)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure ilke tanımı yapısını](definition-structure.md)gözden geçirin.
- [İlke efektlerini anlamak](effects.md)için gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/getting-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.