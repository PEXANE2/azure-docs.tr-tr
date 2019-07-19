---
title: Bir sanal makinenin içeriğini nasıl denetleyeceğinizi anlayın
description: Konuk yapılandırma Azure İlkesi içinde bir Azure sanal makine ayarlarını denetlemek için nasıl kullandığını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/18/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 74e36d944450e1ce2c61481b2cb7e345860212af
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326891"
---
# <a name="understand-azure-policys-guest-configuration"></a>Azure İlkesi'nin Konuk yapılandırma anlama

Azure Ilkesi, Azure kaynaklarını denetlemeye ve [düzeltmelere](../how-to/remediate-resources.md) ek olarak, bir sanal makine içindeki ayarları denetleyebilir. Doğrulama Konuk yapılandırma uzantısı ve istemci tarafından gerçekleştirilir. İstemcisi aracılığıyla uzantısı gibi işletim sistemi yapılandırması, uygulama yapılandırması veya varlığı, ortam ayarlarını ve diğer ayarlarını doğrular.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="extension-and-client"></a>Uzantı ve istemci

Bir sanal makine içinde ayarlarını denetlemek için bir [sanal makine uzantısı](../../../virtual-machines/extensions/overview.md) etkinleştirilir. Uzantı geçerli ilke ataması ve karşılık gelen yapılandırma tanımını indirir.

### <a name="register-guest-configuration-resource-provider"></a>Konuk yapılandırma kaynak sağlayıcısını kaydetme

Konuk yapılandırma kullanabilmeniz için kaynak sağlayıcısını kaydetmeniz gerekir. Portal veya PowerShell aracılığıyla kaydedebilirsiniz. Bir konuk yapılandırma ilkesinin atanması Portal üzerinden yapıldığında kaynak sağlayıcı otomatik olarak kaydedilir.

#### <a name="registration---portal"></a>Kayıt - Portal

Azure portalı üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki adımları izleyin:

1. Azure portalını başlatma ve tıklayarak **tüm hizmetleri**. Arayın ve seçin **abonelikleri**.

1. Bulun ve Konuk yapılandırma için etkinleştirmek istediğiniz aboneliğe tıklayın.

1. Soldaki menüde **abonelik** sayfasında **kaynak sağlayıcıları**.

1. Filtre uygulamak veya bulduktan kadar kaydırın **Microsoft.GuestConfiguration**, ardından **kaydetme** aynı satırda.

#### <a name="registration---powershell"></a>Kayıt - PowerShell

PowerShell üzerinden Konuk yapılandırması için kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Doğrulama Araçları

Sanal makinenin içinde denetim çalıştırmak için yerel Araçlar Konuk yapılandırma istemcisi kullanır.

Aşağıdaki tabloda, desteklenen her işletim sisteminde kullanılan yerel Araçlar listesi gösterilmektedir:

|İşletim sistemi|Doğrulama Aracı|Notlar|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby ve Python Konuk Configuration uzantısı tarafından yüklenir. |

### <a name="validation-frequency"></a>Doğrulama sıklığı

Konuk yapılandırması istemcisi, her 5 dakikada bir yeni içerik denetler. Konuk ataması alındıktan sonra ayarlar 15 dakikalık bir aralıkta denetlenir. Sonuçlar, Denetim tamamlandıktan hemen sonra Konuk yapılandırma kaynak sağlayıcısına gönderilir. Bir ilke [değerlendirme tetikleyicisi](../how-to/get-compliance-data.md#evaluation-triggers) gerçekleştiğinde, makinenin durumu Konuk yapılandırma kaynak sağlayıcısına yazılır. Bu, Azure Ilkesinin Azure Resource Manager özelliklerini değerlendirmesini sağlar. İsteğe bağlı bir Azure Ilke değerlendirmesi, Konuk yapılandırma kaynak sağlayıcısından en son değeri alır. Ancak, sanal makine içindeki yapılandırmanın yeni bir denetimini tetiklemez.

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, desteklenen işletim sistemi listesini Azure görüntülerinde gösterilmektedir:

|Yayımcı|Ad|Sürümler|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04 18.04|
|credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Windows İstemcisi|Windows 10|
|OpenLogic|CentOS|7.3, 7.4 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SuSE|SLES|12 SP3|

> [!IMPORTANT]
> Konuk yapılandırması, desteklenen bir işletim sistemi çalıştıran düğümleri denetleyebilir. Özel bir görüntü kullanan sanal makineleri denetlemek isterseniz, **Deployifnotexists** tanımını çoğaltmanız ve resim özelliklerinizi dahil etmek için **IF** bölümünü değiştirmeniz gerekir.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Windows Server nano Server hiçbir sürümde desteklenmez.

### <a name="guest-configuration-extension-network-requirements"></a>Konuk yapılandırma uzantısı ağ gereksinimleri

Azure 'daki Konuk yapılandırma kaynak sağlayıcısıyla iletişim kurmak için, sanal makinelerin **443**numaralı bağlantı noktasında Azure veri merkezlerine giden erişimi olması gerekir. Azure 'da özel bir sanal ağ kullanıyorsanız ve giden trafiğe izin vermezseniz, özel durumların [ağ güvenlik grubu](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) kuralları kullanılarak yapılandırılması gerekir. Şu anda Azure Ilke Konuk yapılandırması için bir hizmet etiketi yok.

IP adresi listelerinde [Microsoft Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıklar ve IP aralıklarında yaklaşan değişiklikler vardır. Yalnızca sanal makinelerinizin dağıtıldığı bölgelerdeki IP 'lere giden erişime izin vermeniz gerekir.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. Dosya işlem, SQL ve depolama aralıklarını içerir. Güncelleştirilmiş bir dosya haftalık olarak gönderilir. Dosya, şu anda dağıtılmış aralıkları ve IP aralıklarında yaklaşan değişiklikleri yansıtır. Dosyada görüntülenen yeni aralıklar, en az bir hafta için veri merkezlerinde kullanılmıyor. Her hafta yeni XML dosyasını indirmek iyi bir fikirdir. Daha sonra, sitenizi Azure 'da çalışan hizmetleri doğru şekilde belirlemek için güncelleştirin. Azure ExpressRoute kullanıcıları bu dosyanın, her ayın ilk haftasında Azure Space Sınır Ağ Geçidi Protokolü (BGP) tanıtımını güncelleştirmek için kullanıldığını unutmayın.

## <a name="guest-configuration-definition-requirements"></a>Konuk yapılandırma tanımı gereksinimleri

Konuk yapılandırması tarafından çalıştırılan her denetim, bir **Deployifnotexists** tanımı ve bir **Denetim** tanımı olmak üzere iki ilke tanımı gerektirir. **Deployifnotexists** tanımı, sanal makineyi Konuk yapılandırma Aracısı ve diğer bileşenleriyle [doğrulama araçlarını](#validation-tools)destekleyecek şekilde hazırlamak için kullanılır.

**Deployıfnotexists** ilke tanımı doğrular ve düzeltir aşağıdaki öğeleri:

- Doğrulama sanal makine değerlendirilecek bir yapılandırmayı atanmıştır. Hiçbir atama şu anda mevcutsa ataması Al ve sanal makine tarafından hazırlayın:
  - Sanal makineyi kullanarak kimlik doğrulaması bir [yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md)
  - En son sürümünü yükleme **Microsoft.GuestConfiguration** uzantısı
  - Yükleme [Doğrulama Araçları](#validation-tools) ve gerekirse bağımlılıkları

**Deployifnotexists** ataması uyumlu değilse, bir [Düzeltme görevi](../how-to/remediate-resources.md#create-a-remediation-task) kullanılabilir.

**Deployifnotexists** ataması uyumlu olduktan sonra **Denetim** ilkesi ataması, yapılandırma atamasının uyumlu veya uyumsuz olup olmadığını anlamak için yerel doğrulama araçlarını kullanır.
Doğrulama Aracı sonuçları Konuk yapılandırma istemciye sağlar. İstemci, Konuk yapılandırma kaynak sağlayıcısı kullanılabilir hale getirir Konuk uzantısına sonuçları iletir.

Azure İlkesi kullanan Konuk yapılandırma kaynak sağlayıcıları **complianceStatus** rapor uyumluluk özelliğini **Uyumluluk** düğümü. Daha fazla bilgi için [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).

> [!NOTE]
> **Denetim** ilkesinin sonuçları döndürmesi Için **Deployifnotexists** ilkesi gerekir.
> **Deployifnotexists**olmadan, **Denetim** ilkesi "0/0" kaynağını durum olarak gösterir.

Tüm yerleşik ilkeleri Konuk yapılandırması için girişim atamaları tanımlarında kullanın grubuna dahil edilmiştir. [Önizleme] adlı *yerleşik girişim: Linux ve Windows sanal makineleri* içindeki denetim parolası güvenlik ayarları 18 ilke içerir. Windows için altı **Deployifnotexists** ve **Denetim** çiftleri ve Linux için üç çift vardır. Her durumda, yalnızca hedef mantıksal tanımındaki doğrular işletim sistemine göre değerlendirilir [ilke kuralı](definition-structure.md#policy-rule) tanımı.

## <a name="multiple-assignments"></a>Birden çok atama

Konuk yapılandırma ilkeleri Şu anda yalnızca sanal makine başına aynı Konuk atamasının atanmasını, Ilke ataması farklı parametreler kullanıyor olsa bile destekler.

## <a name="client-log-files"></a>İstemci günlük dosyaları

Konuk yapılandırma uzantısı, günlük dosyalarını aşağıdaki konumlara Yazar:

Windows: `C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\<version>\dsc\logs\dsc.log`

Linux: `/var/lib/waagent/Microsoft.GuestConfiguration.ConfigurationforLinux-<version>/GCAgent/logs/dsc.log`

, Geçerli sürüm numarasını belirtir.`<version>`

## <a name="guest-configuration-samples"></a>Konuk yapılandırma örnekleri

Ilke Konuk yapılandırması için örnekler aşağıdaki konumlarda kullanılabilir:

- [Örnek dizini-Konuk yapılandırması](../samples/index.md#guest-configuration)
- [Azure ilke örnekleri GitHub deposu](https://github.com/Azure/azure-policy/tree/master/samples/GuestConfiguration).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/getting-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteceğinizi öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/index.md)ile yönetim grubunun ne olduğunu inceleyin.
