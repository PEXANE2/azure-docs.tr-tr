---
title: Azure Monitör'de Günlük Analizi çalışma alanlarını yönetme | Microsoft Dokümanlar
description: Kaynak, çalışma alanı veya tablo düzeyinde izinleri kullanarak Azure Monitor'da Günlük Analizi çalışma alanında depolanan verilere erişimi yönetebilirsiniz. Bu makalede, nasıl tamamlanıncaya kadar ayrıntılı bilgi vereb.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/22/2019
ms.openlocfilehash: 1e559309b8e8d9768ca2f79dabfb01ec6086a961
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348716"
---
# <a name="manage-access-to-log-data-and-workspaces-in-azure-monitor"></a>Azure Monitor'da günlük verilerine ve çalışma alanlarına erişimi yönetme

Azure Monitor [günlük](data-platform-logs.md) verilerini bir Log Analytics çalışma alanında depolar. Çalışma alanı, veri ve yapılandırma bilgilerini içeren bir kapsayıcıdır. Günlük verilerine erişimi yönetmek için çalışma alanınızla ilgili çeşitli yönetim görevlerini gerçekleştirirsiniz.

Bu makalede, günlüklere erişimin nasıl yönetilenve bunları içeren çalışma alanlarını nasıl yönetiştirilen ve erişime nasıl izin verilmeye de dahil olmak üzere aşağıdakileri içeren çalışma alanlarının nasıl yönetileni açıklanmaktadır: 

* Çalışma alanı izinlerini kullanarak çalışma alanı.
* Azure rol tabanlı erişim denetimini (RBAC) kullanarak belirli kaynaklardan günlük verilerine erişmesi gereken kullanıcılar.
* Azure RBAC'ı kullanarak çalışma alanında belirli bir tabloda günlüğe kaydetmesi gereken kullanıcılar.

## <a name="configure-access-control-mode"></a>Erişim kontrol modunu yapılandırma

Azure portalından veya Azure PowerShell ile çalışma alanında yapılandırılan [erişim denetim modunu](design-logs-deployment.md) görüntüleyebilirsiniz.  Desteklenen yöntemlerden birini kullanarak bu ayarı değiştirebilirsiniz:

* Azure portalında

* Azure PowerShell

* Azure Resource Manager şablonu

### <a name="from-the-azure-portal"></a>Azure portalından

**Günlük Analizi çalışma alanı** menüsünde çalışma alanı için Genel **Bakış** sayfasında geçerli çalışma alanı erişim denetim modunu görüntüleyebilirsiniz.

![Çalışma alanı erişim denetim modunu görüntüleme](media/manage-access/view-access-control-mode.png)

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
1. Azure portalında, çalışma alanınız > Günlük Analizi çalışma alanlarını seçin.

Bu ayarı çalışma alanının **Özellikler** sayfasından değiştirebilirsiniz. Çalışma alanını yapılandırma izniniz yoksa ayarı değiştirmek devre dışı bırakılır.

![Çalışma alanı erişim modunu değiştirme](media/manage-access/change-access-control-mode.png)

### <a name="using-powershell"></a>PowerShell’i kullanma

Abonelikteki tüm çalışma alanlarıiçin erişim denetim modunu incelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions}
```

Çıktı aşağıdakilere benzemelidir:

```
DefaultWorkspace38917: True
DefaultWorkspace21532: False
```

Bir değer, çalışma alanının çalışma alanı bağlamı erişim moduyla yapılandırıldığı `False` anlamına gelir.  Bir değer, çalışma alanının kaynak bağlamı erişim moduyla yapılandırıldığı `True` anlamına gelir.

> [!NOTE]
> Bir çalışma alanı boolean değeri olmadan döndürülür ve boşsa, `False` bu da bir değerin sonuçlarıyla eşleşir.
>

Kaynak bağlamı iznine belirli bir çalışma alanı için erişim denetim modunu ayarlamak için aşağıdaki komut dosyasını kullanın:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null)
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Abonelikteki tüm çalışma alanları için kaynak bağlam iznine erişim denetim modunu ayarlamak için aşağıdaki komut dosyasını kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null)
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
}
```

### <a name="using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanma

Bir Azure Kaynak Yöneticisi şablonundaki erişim modunu yapılandırmak için, çalışma alanındaki **access'i etkinleştirenLogAccessUsingOnlyResourcePermissions** özelliğini aşağıdaki değerlerden birine ayarlayın.

* **false**: Çalışma alanını çalışma alanı bağlamı izinlerine ayarlayın. Bayrak ayarlı değilse, varsayılan ayar budur.
* **true**: Çalışma alanını kaynak bağlam izinlerine ayarlayın.

## <a name="manage-access-using-workspace-permissions"></a>Çalışma alanı izinlerini kullanarak erişimi yönetme

Her çalışma alanıyla ilişkili birden çok hesap olabilir ve her hesabın birden çok çalışma alanına erişimi olabilir. Erişim, [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md)kullanılarak yönetilir.

Şu etkinlikler de Azure izinleri gerektirir:

|Eylem |Gereken Azure İzni |Notlar |
|-------|-------------------------|------|
| İzleme çözümleri ekleme ve kaldırma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||
| Çalışma alanı temel özelliklerini görüntüleyin ve portala çalışma alanı bıçağını girin | `Microsoft.OperationalInsights/workspaces/read` ||
| Herhangi bir arabirimi kullanarak günlükleri sorgula | `Microsoft.OperationalInsights/workspaces/query/read` ||
| Sorguları kullanarak tüm günlük türlerine erişin | `Microsoft.OperationalInsights/workspaces/query/*/read` ||
| Belirli bir günlük tablosuna erişin | `Microsoft.OperationalInsights/workspaces/query/<table_name>/read` ||
| Bu çalışma alanına günlük gönderme izni vermek için çalışma alanı anahtarlarını okuyun | `Microsoft.OperationalInsights/workspaces/sharedKeys/action` ||

## <a name="manage-access-using-azure-permissions"></a>Azure izinlerini kullanarak erişimi yönetme

Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin. Örneğin özel roller, [bkz.](#custom-role-examples)

Azure'un Log Analytics çalışma alanları için iki yerleşik kullanıcı rolü vardır:

* Log Analytics Okuyucusu
* Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:

* Tüm izleme verilerini görüntüleme ve arama
* Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

Log Analytics Reader rolü aşağıdaki Azure eylemlerini içerir:

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm Azure kaynaklarını ve kaynak yapılandırmasını görüntüleme yeteneği. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, çalışma alanı ayarlarını okumak ve verilerüzerinde sorgu gerçekleştirmek için tam sınırsız izinler sağlar. Yukarıdaki daha ayrıntılı seçeneklere bakın. |
| Eylem | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Amortismana uğradı, kullanıcılara atamaya gerek yok. |
| Eylem | `Microsoft.OperationalInsights/workspaces/search/action` | Amortismana uğradı, kullanıcılara atamaya gerek yok. |
| Eylem | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Veri toplama API'sini kullanmak ve aracıları yüklemek için gereken çalışma alanı anahtarının okunmasını engeller. Bu, kullanıcının çalışma alanına yeni kaynaklar eklemesini engeller |

*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:

* *Log Analytics Reader rolünün*tüm ayrıcalıklarını içerir ve kullanıcının tüm izleme verilerini okumasına olanak sağlar
* Otomasyon hesapları oluşturma ve yapılandırma
* Yönetim çözümlerini ekleme ve kaldırma

    > [!NOTE]
    > Son iki eylemin başarıyla gerçekleştirilebilmesi için, bu iznin kaynak grubunda veya abonelik düzeyinde verilmesi gerekir.

* Depolama hesabı anahtarlarını okuma
* Azure Depolama'daki günlük koleksiyonunu yapılandırma
* Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  * VM'lere VM uzantısı ekleme
  * Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE]
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

Log Analytics Katılımcısı rolü aşağıdaki Azure eylemlerini içerir:

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, çalışma alanı ayarını okumak ve verilerüzerinde sorgu gerçekleştirmek için tam sınırsız izinler sağlar. Yukarıdaki daha ayrıntılı seçeneklere bakın. |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırma ekleme, güncelleştirme ve kaldırma. Çalışma alanı gelişmiş ayarlarını yeniden `Microsoft.OperationalInsights/workspaces/write`sağlamak için kullanıcının ihtiyacı vardır. |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:

* Abonelik - Abonelikteki tüm çalışma alanlarına erişim
* Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
* Kaynak - Yalnızca belirtilen çalışma alanına erişim

Doğru erişim denetimi sağlamak için atamaları kaynak düzeyinde (çalışma alanı) gerçekleştirmenizi öneririz. Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

### <a name="resource-permissions"></a>Kaynak izinleri

Kullanıcılar kaynak bağlamı erişimini kullanarak bir çalışma alanından günlükleri sorguladığında, kaynak üzerinde aşağıdaki izinlere sahip olurlar:

| İzin | Açıklama |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Örnekler:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Kaynak için tüm günlük verilerini görüntüleme yeteneği.  |
| `Microsoft.Insights/diagnosticSettings/write` | Bu kaynak için günlükleri ayarlamaya izin verecek tanılama ayarını yapılandırma yeteneği. |

`/read`izin genellikle _ \*/read veya_ _\*_ yerleşik [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) ve [Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) rolleri gibi izinleri içeren bir rolden verilir. Belirli eylemleri veya özel yerleşik rolleri içeren özel roller bu izni içermeyebilir.

Farklı tablolar için farklı erişim denetimi oluşturmak istiyorsanız aşağıdaki [tablo başına erişim denetimini tanımlama](#table-level-rbac) konusuna bakın.

## <a name="custom-role-examples"></a>Özel rol örnekleri

1. Bir kullanıcıya kaynaklarından günlük verilerine erişim sağlamak için aşağıdakileri gerçekleştirin:

    * Çalışma alanı veya **kaynak izinlerini kullanmak** için çalışma alanı erişim denetim modunu yapılandırma

    * Kullanıcılara `*/read` `Microsoft.Insights/logs/*/read` veya kaynaklarına izin ver. Çalışma alanında Log [Analytics Reader](../../role-based-access-control/built-in-roles.md#reader) rolüne zaten atanmışsa, bu yeterlidir.

2. Bir kullanıcıya kaynaklarından günlük verilerine erişim sağlamak ve kaynaklarını çalışma alanına günlük gönderecek şekilde yapılandırmak için aşağıdakileri gerçekleştirin:

    * Çalışma alanı veya **kaynak izinlerini kullanmak** için çalışma alanı erişim denetim modunu yapılandırma

    * Kullanıcılara çalışma alanında aşağıdaki izinleri `Microsoft.OperationalInsights/workspaces/read` `Microsoft.OperationalInsights/workspaces/sharedKeys/action`ver: ve . Bu izinlerle, kullanıcılar çalışma alanı düzeyinde sorgu lar gerçekleştiremez. Yalnızca çalışma alanını sayısala alabilir ve tanılama ayarları veya aracı yapılandırması için bir hedef olarak kullanabilirler.

    * Kullanıcılara kaynaklarına aşağıdaki izinleri `Microsoft.Insights/logs/*/read` `Microsoft.Insights/diagnosticSettings/write`verir: ve . [Log Analytics Katılımcısı](../../role-based-access-control/built-in-roles.md#contributor) rolüne zaten atanmışsa, Okuyucu `*/read` rolü atanmışsa veya bu kaynakta izin verilmişse, bu yeterlidir.

3. Bir kullanıcıya güvenlik olaylarını okuyamadan ve veri gönderemeden kaynaklarından günlük verilerine erişim sağlamak için aşağıdakileri gerçekleştirin:

    * Çalışma alanı veya **kaynak izinlerini kullanmak** için çalışma alanı erişim denetim modunu yapılandırma

    * Kullanıcılara kaynaklarına aşağıdaki izinleri `Microsoft.Insights/logs/*/read`verir: .

    * Kullanıcıların SecurityEvent türünü okumasını engellemek için aşağıdaki `Microsoft.Insights/logs/SecurityEvent/read`Eylemsizlik'i ekleyin: . NonAction, okuma izni () sağlayan eylemle aynı özel`Microsoft.Insights/logs/*/read`rolde olacaktır. Kullanıcı, bu kaynağa veya abonelik veya kaynak grubuna atanan başka bir rolden okuma eyleminin doğasında varsa, tüm günlük türlerini okuyabilir. Bu, örneğin Okuyucu `*/read`veya Katkıda Bulunan rolüyle birlikte var olan , devraldıkları nda da geçerlidir.

4. Bir kullanıcıya kaynaklarından günlük verilerine erişim sağlamak ve tüm Azure AD oturum açma oturumlarını okumak ve çalışma alanından Güncelleştirme Yönetimi çözüm günlüğü verilerini okumak için aşağıdakileri gerçekleştirin:

    * Çalışma alanı veya **kaynak izinlerini kullanmak** için çalışma alanı erişim denetim modunu yapılandırma

    * Kullanıcılara çalışma alanında aşağıdaki izinleri ver: 

        * `Microsoft.OperationalInsights/workspaces/read`– kullanımının çalışma alanını sayısala göre ataması ve Azure portalındaki çalışma alanı bıçaklarını açması için gereklidir
        * `Microsoft.OperationalInsights/workspaces/query/read`– sorguları yürütebilen her kullanıcı için gereklidir
        * `Microsoft.OperationalInsights/workspaces/query/SigninLogs/read`– Azure AD oturum açma günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/Update/read`– Update Management çözüm günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read`– Update Management çözüm günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read`– Güncelleme yönetim günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/Heartbeat/read`– Update Management çözümlerini kullanabilmek için gerekli
        * `Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read`– Update Management çözümlerini kullanabilmek için gerekli

    * Kullanıcılara kaynaklarına aşağıdaki izinleri `*/read`ver: Reader rolüne `Microsoft.Insights/logs/*/read`atanmış veya . 

## <a name="table-level-rbac"></a>Tablo seviyesi RBAC

**Tablo düzeyi RBAC,** diğer izinlere ek olarak Bir Log Analytics çalışma alanındaki verilere daha ayrıntılı denetim tanımlamanıza olanak tanır. Bu denetim, yalnızca belirli bir kullanıcı kümesi tarafından erişilebilen belirli veri türlerini tanımlamanıza olanak tanır.

Çalışma alanındaki belirli [tablolara](../log-query/logs-structure.md) erişim izni vermek için [Azure özel rolleri](../../role-based-access-control/custom-roles.md) ile tablo erişim denetimini uygularsınız. Bu roller, kullanıcının [erişim modundan](design-logs-deployment.md#access-mode)bağımsız olarak çalışma alanı bağlamı veya kaynak [bağlamı erişim denetim modlarına](design-logs-deployment.md#access-control-mode) sahip çalışma alanlarına uygulanır.

Tablo erişim denetimine erişimi tanımlamak için aşağıdaki eylemlerle özel bir [rol](../../role-based-access-control/custom-roles.md) oluşturun.

* Bir tabloya erişim sağlamak için, onu rol tanımının **Eylemler** bölümüne ekleyin. İzin verilen **Eylemler'den**erişimi çıkarmak için, **NotActions** bölümüne ekleyin.
* Tüm tabloları belirtmek için Microsoft.OperationalInsights/workspaces/query/* kullanın.

Örneğin, _Kalp Atışı_ ve _Azure Etkinliği_ tablolarına erişerek bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
    "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Yalnızca _SecurityBaseline_ tablosuna erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
],
```

### <a name="custom-logs"></a>Özel günlükler

 Özel günlükler, özel günlükler ve HTTP Veri Toplayıcı API'sı gibi veri kaynaklarından oluşturulur. Günlük türünü belirlemenin en kolay yolu, [günlük şemasında Özel Günlükler](../log-query/get-started-portal.md#understand-the-schema)altında listelenen tabloları denetlemektir.

 Şu anda tek tek özel günlüklere erişim izni veremezsin, ancak tüm özel günlüklere erişim izni verebilirsiniz. Tüm özel günlüklere erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
],
```

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Bir kullanıcıya _ \*/read_ eylemini içeren standart Reader veya Contributor rolleriyle genel okuma izni verilirse, bu işlem tablo başına erişim denetimini geçersiz kılar ve tüm günlük verilerine erişim sağlar.
* Bir kullanıcıya tablo başına erişim izni verilirse, ancak başka izin verilmezse, oturum günlüğü verilerine API'den erişebilir, ancak Azure portalından erişemez. Azure portalından erişim sağlamak için temel rolü olarak Log Analytics Reader'ı kullanın.
* Aboneliğin yöneticileri, diğer izin ayarlarına bakılmaksızın tüm veri türlerine erişebilir.
* Çalışma alanı sahipleri, tablo başına erişim denetimi için diğer tüm kullanıcı gibi değerlendirilir.
* Atama sayısını azaltmak için tek tek kullanıcılar yerine güvenlik gruplarına rol atamanızı öneririz. Bu, erişimi yapılandırmak ve doğrulamak için varolan grup yönetim araçlarını kullanmanıza da yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

* Veri merkezinizdeki veya diğer bulut ortamınızdaki bilgisayarlardan veri toplamak için [Günlük Analizi temsilcisine genel bakış](../../azure-monitor/platform/log-analytics-agent.md) bölümüne bakın.

* Bkz. Azure SANAL M'lerinden veri toplamayapılandırmak için [Azure sanal makineleri hakkında veri topla.](../../azure-monitor/learn/quick-collect-azurevm.md)
