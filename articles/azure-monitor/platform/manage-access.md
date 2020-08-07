---
title: Azure Izleyici 'de Log Analytics çalışma alanlarını yönetme | Microsoft Docs
description: Kaynak, çalışma alanı veya tablo düzeyi izinleri kullanarak Azure Izleyici 'deki bir Log Analytics çalışma alanında depolanan verilere erişimi yönetebilirsiniz. Bu makalede nasıl tamamlanacağı açıklanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/10/2019
ms.openlocfilehash: 5b450254648cb253d6631397d703430401009f14
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87925644"
---
# <a name="manage-access-to-log-data-and-workspaces-in-azure-monitor"></a>Azure İzleyici'deki günlük verilerine ve çalışma alanlarına erişimi yönetme

Azure Izleyici, [günlük](data-platform-logs.md) verilerini bir Log Analytics çalışma alanında depolar. Çalışma alanı, veri ve yapılandırma bilgilerini içeren bir kapsayıcıdır. Günlük verilerine erişimi yönetmek için, çalışma alanınız ile ilgili çeşitli yönetim görevlerini gerçekleştirirsiniz.

Bu makalede, ' ye erişim verme dahil olmak üzere günlüklere erişimi yönetme ve bunları içeren çalışma alanlarını yönetme konuları açıklanmaktadır: 

* Çalışma alanı izinlerini kullanarak çalışma alanı.
* Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak belirli kaynaklardan günlük verilerine erişmesi gereken kullanıcılar ( [kaynak bağlamı](design-logs-deployment.md#access-mode) olarak da bilinir)
* Azure RBAC kullanarak çalışma alanındaki belirli bir tablodaki günlük verilerine erişmesi gereken kullanıcılar.

RBAC ve erişim stratejilerinin çevresindeki günlük kavramlarını anlamak için, [Azure Izleyici günlükleri dağıtımınızı tasarlama](design-logs-deployment.md) konusunu okuyun

## <a name="configure-access-control-mode"></a>Erişim denetimi modunu yapılandırma

Bir çalışma alanında yapılandırılan [erişim denetimi modunu](design-logs-deployment.md) Azure portal veya Azure PowerShell ile görüntüleyebilirsiniz.  Bu ayarı, aşağıdaki desteklenen yöntemlerden birini kullanarak değiştirebilirsiniz:

* Azure portal

* Azure PowerShell

* Azure Resource Manager şablonu

### <a name="from-the-azure-portal"></a>Azure portalından

**Log Analytics çalışma** alanı menüsündeki çalışma alanının **genel bakış** sayfasında geçerli çalışma alanı erişim denetimi modunu görüntüleyebilirsiniz.

![Çalışma alanı erişim denetimi modunu görüntüle](media/manage-access/view-access-control-mode.png)

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
1. Azure portal, çalışma alanınız > Log Analytics çalışma alanları ' nı seçin.

Bu ayarı, çalışma alanının **Özellikler** sayfasından değiştirebilirsiniz. Çalışma alanını yapılandırma izniniz yoksa ayarı değiştirmek devre dışı bırakılır.

![Çalışma alanı erişim modunu değiştir](media/manage-access/change-access-control-mode.png)

### <a name="using-powershell"></a>PowerShell’i kullanma

Abonelikteki tüm çalışma alanları için erişim denetimi modunu incelemek üzere aşağıdaki komutu kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions}
```

Çıkışın aşağıdakine benzer olması gerekir:

```
DefaultWorkspace38917: True
DefaultWorkspace21532: False
```

Değeri, çalışma alanının `False` çalışma alanı bağlamı erişim moduyla yapılandırıldığı anlamına gelir.  Değeri, `True` çalışma alanının kaynak bağlamı erişim moduyla yapılandırıldığı anlamına gelir.

> [!NOTE]
> Bir çalışma alanı Boole değeri olmadan döndürülürse ve boşsa, bu da bir değer sonuçlarıyla eşleşir `False` .
>

Belirli bir çalışma alanı için erişim denetimi modunu kaynak bağlamı iznine ayarlamak için aşağıdaki betiği kullanın:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null)
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Abonelikteki tüm çalışma alanları için erişim denetimi modunu kaynak bağlamı iznine ayarlamak için aşağıdaki betiği kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null)
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
}
```

### <a name="using-a-resource-manager-template"></a>Resource Manager şablonu kullanma

Azure Resource Manager şablonunda erişim modunu yapılandırmak için, çalışma alanında **Enablelogaccessusingonlyresourcepermissions** Özellik bayrağını aşağıdaki değerlerden birine ayarlayın.

* **yanlış**: çalışma alanını çalışma alanı bağlamı izinlerine ayarlayın. Bayrak ayarlanmamışsa bu varsayılan ayardır.
* **doğru**: çalışma alanını Kaynak bağlamı izinlerine ayarlayın.

## <a name="manage-access-using-workspace-permissions"></a>Çalışma alanı izinlerini kullanarak erişimi yönetme

Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabın birden fazla çalışma alanına erişimi olabilir. Erişim, [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md)kullanılarak yönetilir.

Şu etkinlikler de Azure izinleri gerektirir:

|Eylem |Gereken Azure İzni |Notlar |
|-------|-------------------------|------|
| İzleme çözümlerini ekleme ve kaldırma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||
| Çalışma alanı temel özelliklerini görüntüleyin ve portalda çalışma alanı Dikey penceresini girin | `Microsoft.OperationalInsights/workspaces/read` ||
| Tüm arabirimleri kullanarak günlükleri sorgula | `Microsoft.OperationalInsights/workspaces/query/read` ||
| Sorguları kullanarak tüm günlük türlerine erişin | `Microsoft.OperationalInsights/workspaces/query/*/read` ||
| Belirli bir günlük tablosuna erişme | `Microsoft.OperationalInsights/workspaces/query/<table_name>/read` ||
| Bu çalışma alanına Günlükler gönderilmesine izin vermek için çalışma alanı anahtarlarını okuyun | `Microsoft.OperationalInsights/workspaces/sharedKeys/action` ||

## <a name="manage-access-using-azure-permissions"></a>Azure izinlerini kullanarak erişimi yönetme

Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin. Örneğin özel roller için bkz. [özel roller örneği](#custom-role-examples)

Azure 'da Log Analytics çalışma alanları için iki yerleşik kullanıcı rolü vardır:

* Log Analytics Okuyucusu
* Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:

* Tüm izleme verilerini görüntüleme ve arama
* Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

Log Analytics okuyucu rolü aşağıdaki Azure eylemlerini içerir:

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm Azure kaynaklarını ve kaynak yapılandırmalarını görüntüleyebilme. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarlarını okumasını ve verilerin üzerinde sorgu gerçekleştirmesini sağlar. Daha ayrıntılı seçeneklere bakın. |
| Eylem | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Kullanım dışı, kullanıcılara atanması gerekmez. |
| Eylem | `Microsoft.OperationalInsights/workspaces/search/action` | Kullanım dışı, kullanıcılara atanması gerekmez. |
| Eylem | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Veri toplama API 'sini kullanmak ve aracıları yüklemek için gerekli çalışma alanı anahtarını okumayı engeller. Bu, kullanıcının çalışma alanına yeni kaynak eklemesini önler |

*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:

* *Log Analytics okuyucu rolünün*tüm ayrıcalıklarını içerir ve bu da kullanıcının tüm izleme verilerini okumasına izin verir
* Otomasyon hesapları oluşturma ve yapılandırma
* Yönetim çözümlerini ekleme ve kaldırma

    > [!NOTE]
    > Son iki eylemi başarılı bir şekilde gerçekleştirmek için bu iznin kaynak grubu veya abonelik düzeyinde verilmesi gerekir.

* Depolama hesabı anahtarlarını oku
* Azure depolama 'daki günlüklerin koleksiyonunu yapılandırma
* Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  * VM'lere VM uzantısı ekleme
  * Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE]
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

Log Analytics katkıda bulunan rolü aşağıdaki Azure eylemlerini içerir:

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarını okumasına ve verilerde sorgu gerçekleştirmesine izin verir. Daha ayrıntılı seçeneklere bakın. |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırma ekleyin, güncelleştirin ve kaldırın. Çalışma alanı gelişmiş ayarlarını düzenlemek için, Kullanıcı ihtiyaçları `Microsoft.OperationalInsights/workspaces/write` . |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:

* Abonelik - Abonelikteki tüm çalışma alanlarına erişim
* Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
* Kaynak - Yalnızca belirtilen çalışma alanına erişim

Doğru erişim denetimini güvence altına almak için kaynak düzeyinde (çalışma alanı) atamalar gerçekleştirmenizi öneririz. Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

### <a name="resource-permissions"></a>Kaynak izinleri

Kullanıcılar, kaynak bağlamı erişimi kullanarak bir çalışma alanından günlükleri sorgularsa, kaynak üzerinde aşağıdaki izinlere sahip olurlar:

| İzin | Açıklama |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Örnekler:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Kaynak için tüm günlük verilerini görüntüleme olanağı.  |
| `Microsoft.Insights/diagnosticSettings/write` | Tanılama ayarını bu kaynak için günlükleri ayarlamaya izin verecek şekilde yapılandırma özelliği. |

`/read`izin genellikle, yerleşik okuyucu ve katkıda bulunan rolleri gibi _ \* /Read veya_ Permissions içeren bir rolden verilir _\*_ . [Reader](../../role-based-access-control/built-in-roles.md#reader) [Contributor](../../role-based-access-control/built-in-roles.md#contributor) Belirli eylemler veya adanmış yerleşik roller içeren özel roller bu izni içermeyebilir.

Farklı tablolar için farklı erişim denetimi oluşturmak isterseniz, aşağıdaki [tablo başına erişim denetimi tanımlama](#table-level-rbac) bölümüne bakın.

## <a name="custom-role-examples"></a>Özel rol örnekleri

1. Bir kullanıcıya, kaynaklarından günlük verileri erişimi vermek için aşağıdakileri yapın:

    * Çalışma alanı erişim denetimi modunu **çalışma alanı veya kaynak izinleri kullanacak** şekilde yapılandırma

    * Kullanıcılara `*/read` veya `Microsoft.Insights/logs/*/read` izinlerini kaynaklarına verin. Çalışma alanında [Log Analytics okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü zaten atanırsa, bu yeterli olur.

2. Bir kullanıcıya, kaynaklarından günlük verilerine erişim vermek ve kaynaklarını çalışma alanına Günlükler gönderecek şekilde yapılandırmak için aşağıdakileri yapın:

    * Çalışma alanı erişim denetimi modunu **çalışma alanı veya kaynak izinleri kullanacak** şekilde yapılandırma

    * Çalışma alanında kullanıcılara aşağıdaki izinleri verin: `Microsoft.OperationalInsights/workspaces/read` ve `Microsoft.OperationalInsights/workspaces/sharedKeys/action` . Kullanıcılar, bu izinlerle hiçbir çalışma alanı düzeyi sorgu gerçekleştiremez. Bunlar yalnızca çalışma alanını numaralandırırlar ve Tanılama ayarları veya aracı yapılandırması için bir hedef olarak kullanabilir.

    * Kullanıcılara kaynakları için aşağıdaki izinleri verin: `Microsoft.Insights/logs/*/read` ve `Microsoft.Insights/diagnosticSettings/write` . [Log Analytics katkıda](../../role-based-access-control/built-in-roles.md#contributor) bulunan rolü zaten atanırsa, okuyucu rolüne atanmış veya `*/read` Bu kaynakta izin verilen izinler varsa, bu yeterli olur.

3. Güvenlik olaylarını okuyup veri gönderebilmek zorunda kalmadan, kaynaklarından günlük verilerine erişim vermek için aşağıdakileri yapın:

    * Çalışma alanı erişim denetimi modunu **çalışma alanı veya kaynak izinleri kullanacak** şekilde yapılandırma

    * Kullanıcılara kaynakları için aşağıdaki izinleri verin: `Microsoft.Insights/logs/*/read` .

    * Kullanıcıların SecurityEvent türünü okumasını engellemek için aşağıdaki eylemi ekleyin: `Microsoft.Insights/logs/SecurityEvent/read` . Olmayan eylem, okuma izni () sağlayan eylem ile aynı özel rolde yer almaktadır `Microsoft.Insights/logs/*/read` . Kullanıcı bu kaynağa veya abonelik ya da kaynak grubuna atanmış başka bir rolden okuma eylemini içeriyorsa, tüm günlük türlerini okuyabilirler. Bu, `*/read` Örneğin, okuyucu veya katkıda bulunan rolüyle, varsa de geçerlidir.

4. Bir kullanıcıya, kaynaklarından günlük verileri için erişim izni vermek ve tüm Azure AD oturum açma ve Güncelleştirme Yönetimi çözüm günlüğü verilerini çalışma alanından okumak için aşağıdakileri yapın:

    * Çalışma alanı erişim denetimi modunu **çalışma alanı veya kaynak izinleri kullanacak** şekilde yapılandırma

    * Çalışma alanında kullanıcılara aşağıdaki izinleri verin: 

        * `Microsoft.OperationalInsights/workspaces/read`– Kullanıcı çalışma alanını numaralandırabilmek ve Azure portal çalışma alanı Dikey penceresini açmak için gereklidir
        * `Microsoft.OperationalInsights/workspaces/query/read`– sorguları yürütebilmesi için her kullanıcı için gereklidir
        * `Microsoft.OperationalInsights/workspaces/query/SigninLogs/read`– Azure AD oturum açma günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/Update/read`– Güncelleştirme Yönetimi çözüm günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read`– Güncelleştirme Yönetimi çözüm günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read`– Güncelleştirme yönetimi günlüklerini okuyabilmek için
        * `Microsoft.OperationalInsights/workspaces/query/Heartbeat/read`– Güncelleştirme Yönetimi çözümü kullanabilmek için gereklidir
        * `Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read`– Güncelleştirme Yönetimi çözümü kullanabilmek için gereklidir

    * Kullanıcılara kaynakları için aşağıdaki izinleri verin: `*/read` , okuyucu rolüne atanır veya `Microsoft.Insights/logs/*/read` . 

## <a name="table-level-rbac"></a>Tablo düzeyi RBAC

**Tablo DÜZEYI RBAC** , diğer izinlerin yanı sıra bir Log Analytics çalışma alanındaki verilere daha ayrıntılı denetim tanımlamanızı sağlar. Bu denetim, yalnızca belirli bir kullanıcı kümesi için erişilebilen belirli veri türlerini tanımlamanızı sağlar.

Çalışma alanındaki belirli [tablolara](../log-query/logs-structure.md) erişim sağlamak için [Azure özel rolleriyle](../../role-based-access-control/custom-roles.md) tablo erişim denetimi uygulayabilirsiniz. Bu roller, kullanıcının [erişim modundan](design-logs-deployment.md#access-mode)bağımsız olarak, çalışma alanı bağlamı veya kaynak bağlamı [erişim denetimi modlarıyla](design-logs-deployment.md#access-control-mode) çalışma alanlarına uygulanır.

Tablo erişim denetimine erişimi tanımlamak için aşağıdaki eylemlerle [özel bir rol](../../role-based-access-control/custom-roles.md) oluşturun.

* Bir tabloya erişim vermek için, rol tanımının **Eylemler** bölümüne ekleyin. İzin verilen **eylemlerden**erişimi çıkarmak Için, **NotActions** bölümüne dahil edin.
* Tüm tabloları belirtmek için Microsoft. Operationalınsights/Workspaces/Query/* kullanın.

Örneğin, _sinyal_ ve _AzureActivity_ tablolarına erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
    "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Yalnızca _Securitybaseline_ tablosuna erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
],
```
Yukarıdaki örnekler, izin verilen tabloların beyaz bir listesini tanımlar. Bu örnek, bir Kullanıcı tüm tablolara erişebil, ancak _Securityalert_ tablosuna erişebilme kara liste tanımını gösterir:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/*/read"
],
"notActions":  [
    "Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read"
],
```

### <a name="custom-logs"></a>Özel günlükler

 Özel Günlükler ve HTTP Veri Toplayıcı API 'SI gibi veri kaynaklarından özel günlükler oluşturulur. Günlük türünü belirlemenin en kolay yolu, [günlük şemasında özel Günlükler](../log-query/get-started-portal.md#understand-the-schema)altında listelenen tabloları denetlemede olur.

 Tek tek özel günlüklere erişim izni verilemez, ancak tüm özel günlüklere erişim izni verebilirsiniz. Tüm özel günlüklere erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
    "Microsoft.OperationalInsights/workspaces/read",
    "Microsoft.OperationalInsights/workspaces/query/read",
    "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
],
```
Özel günlüklere erişimi yönetmeye yönelik alternatif bir yaklaşım, bunları bir Azure kaynağına atamak ve kaynak bağlamı paradigması kullanarak erişimi yönetmeleri. Bu yöntemi kullanmak için, [http veri toplayıcı API 'si](data-collector-api.md)aracılığıyla Log Analytics veriler istendiğinde [x-MS-azureresourceıd](data-collector-api.md#request-headers) üst bilgisinde belirterek kaynak kimliğini dahil etmeniz gerekir. Kaynak KIMLIĞI geçerli olmalı ve erişim kuralları uygulanmış olmalıdır. Günlükler alındıktan sonra, burada açıklandığı gibi kaynağa okuma erişimi olan kullanıcılar tarafından erişilebilir.

Bazen özel Günlükler, belirli bir kaynakla doğrudan ilişkili olmayan kaynaklardan gelir. Bu durumda, yalnızca bu günlüklere erişimi yönetmek için bir kaynak grubu oluşturun. Kaynak grubu hiçbir ücret vermez, ancak özel günlüklere erişimi denetlemek için geçerli bir kaynak KIMLIĞI sağlar. Örneğin, belirli bir güvenlik duvarı özel Günlükler gönderiyorsa, "MyFireWallLogs" adlı bir kaynak grubu oluşturun ve API isteklerinin "MyFireWallLogs" kaynak KIMLIĞINI içerdiğinden emin olun. Daha sonra güvenlik duvarı günlük kayıtları, yalnızca MyFireWallLogs veya tam çalışma alanı erişimiyle erişim izni verilen kullanıcılar için erişilebilir.          

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Bir kullanıcıya, _ \* /Read_ eylemini içeren standart okuyucu veya katkıda bulunan rollerle genel okuma izni verildiyse, tablo başına erişim denetimini geçersiz kılar ve tüm günlük verilerine erişim sağlar.
* Bir kullanıcıya tablo başına erişim verildiyse ancak başka izinler yoksa, API 'den günlük verilerine erişebilecek ancak Azure portal. Azure portal erişim sağlamak için, temel rolü olarak Log Analytics okuyucu kullanın.
* Aboneliğin yöneticileri ve sahipleri, diğer izin ayarlarından bağımsız olarak tüm veri türlerine erişebilir.
* Çalışma alanı sahipleri, tablo başına erişim denetimi için diğer kullanıcılar gibi davranır.
* Bireysel kullanıcılar yerine, atamaların sayısını azaltmak için güvenlik gruplarına roller atanmasını öneririz. Bu Ayrıca, erişimi yapılandırmak ve doğrulamak için mevcut Grup Yönetimi araçlarını kullanmanıza yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

* Veri merkezinizdeki veya diğer bulut Ortamınızdaki bilgisayarlardan veri toplamak için [Log Analytics aracısına genel bakış](./log-analytics-agent.md) konusuna bakın.

* Azure VM 'lerinden veri toplamayı yapılandırmak için bkz. [Azure sanal makineler hakkında veri toplama](../learn/quick-collect-azurevm.md) .
