---
title: Azure Izleyici 'de Log Analytics çalışma alanlarını yönetme | Microsoft Docs
description: Kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları ile ilgili çeşitli yönetim görevlerini kullanarak Azure Izleyici 'de Log Analytics çalışma alanlarını yönetebilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: magoedte
ms.openlocfilehash: 59e5bbaf8deccdd8218e9c5590266070ed3b5ebb
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624331"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Azure Izleyici 'de günlük verilerini ve çalışma alanlarını yönetme

Azure Izleyici, [günlük](data-platform-logs.md) verilerini, aslında veri ve yapılandırma bilgilerini içeren bir kapsayıcı olan Log Analytics çalışma alanında depolar. Günlük verilerine erişimi yönetmek için, çalışma alanınız ile ilgili çeşitli yönetim görevlerini gerçekleştirirsiniz.

Bu makalede, aşağıdaki gibi, günlüklere erişimin nasıl yönetileceği ve bunları içeren çalışma alanlarının yönetimi açıklanmaktadır:

* Azure rol tabanlı erişim denetimi (RBAC) kullanarak belirli kaynaklardan günlük verilerine erişmesi gereken kullanıcılara erişim verme.

* Çalışma alanı izinlerini kullanarak çalışma alanına erişim izni verme.

* Azure RBAC kullanarak çalışma alanındaki belirli bir tablodaki günlük verilerine erişmesi gereken kullanıcılara erişim verme.

## <a name="define-access-control-mode"></a>Erişim denetimi modunu tanımlama

Bir çalışma alanında yapılandırılan erişim denetimi modunu Azure portal veya Azure PowerShell ile görüntüleyebilirsiniz.  Bu ayarı, aşağıdaki desteklenen yöntemlerden birini kullanarak değiştirebilirsiniz:

* Azure portal

* Azure PowerShell

* Azure Resource Manager şablonu

### <a name="configure-from-the-azure-portal"></a>Azure portal yapılandırma

**Log Analytics çalışma** alanı menüsündeki çalışma alanının **genel bakış** sayfasında geçerli çalışma alanı erişim denetimi modunu görüntüleyebilirsiniz.

![Çalışma alanı erişim denetimi modunu görüntüle](media/manage-access/view-access-control-mode.png)

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
1. Azure portal, çalışma alanınız > Log Analytics çalışma alanları ' nı seçin.

Bu ayarı, çalışma alanının **Özellikler** sayfasından değiştirebilirsiniz. Çalışma alanını yapılandırma izniniz yoksa ayarı değiştirmek devre dışı bırakılır.

![Çalışma alanı erişim modunu değiştir](media/manage-access/change-access-control-mode.png)

### <a name="configure-using-powershell"></a>PowerShell 'i kullanarak yapılandırma

Abonelikteki tüm çalışma alanları için erişim denetimi modunu incelemek üzere aşağıdaki komutu kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions}
```

Çıkışın aşağıdakine benzer olması gerekir:

```
DefaultWorkspace38917: True
DefaultWorkspace21532: False
```

Değeri, çalışma `False` alanının çalışma alanı bağlamı erişim moduyla yapılandırıldığı anlamına gelir.  Değeri, çalışma `True` alanının kaynak bağlamı erişim moduyla yapılandırıldığı anlamına gelir.

> [!NOTE]
> Bir çalışma alanı Boole değeri olmadan döndürülürse ve boşsa, bu da bir `False` değer sonuçlarıyla eşleşir.
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
```

### <a name="configure-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak yapılandırma

Azure Resource Manager şablonunda erişim modunu yapılandırmak için, çalışma alanında **Enablelogaccessusingonlyresourcepermissions** Özellik bayrağını aşağıdaki değerlerden birine ayarlayın.

* **yanlış**: Çalışma alanını çalışma alanı bağlamı izinlerine ayarlayın. Bayrak ayarlanmamışsa bu varsayılan ayardır.
* **doğru**: Çalışma alanını Kaynak bağlamı izinlerine ayarlayın.

## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme

Belirli bir kullanıcı için çalışma alanına uygulanan izinler, [erişim moduyla](design-logs-deployment.md#access-mode) ve çalışma alanının [erişim denetimi moduna](design-logs-deployment.md#access-control-mode) göre tanımlanır. **Çalışma alanı bağlamı**ile, bu moddaki sorgular çalışma alanındaki tüm tablolardaki tüm verilerin kapsamına alınmış olması halinde, izninizin olduğu çalışma alanındaki tüm günlükleri görüntüleyebilirsiniz. **Kaynak bağlamı**ile, erişim sahibi olduğunuz Azure Portal doğrudan kaynak üzerinde bir arama gerçekleştirirken belirli bir kaynak, kaynak grubu veya abonelik için çalışma alanındaki günlük verilerini görüntüleyebilirsiniz. Bu moddaki sorgular yalnızca söz konusu kaynakla ilişkili verilere göre kapsamlandırılır.

### <a name="workspace-permissions"></a>Çalışma alanı izinleri

Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabı birden çok çalışma alanına erişim sahibi olabilir. Erişim, [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md)kullanılarak yönetilir.

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

Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

Azure 'da Log Analytics çalışma alanları için iki yerleşik kullanıcı rolü vardır:

* Log Analytics Okuyucusu
* Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:

* Tüm izleme verilerini görüntüleme ve arama
* Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

Log Analytics okuyucusu rolü, aşağıdaki Azure eylemleri içerir:

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm Azure kaynaklarını ve kaynak yapılandırması görüntüleme olanağı. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarlarını okumasını ve verilerin üzerinde sorgu gerçekleştirmesini sağlar. Daha ayrıntılı seçeneklere bakın. |
| Action | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Kullanım dışı, kullanıcılara atanması gerekmez. |
| Action | `Microsoft.OperationalInsights/workspaces/search/action` | Kullanım dışı, kullanıcılara atanması gerekmez. |
| Action | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Çalışma alanının engeller veri koleksiyonu API'sini kullanmak ve aracıları yüklemek için gereken anahtarı. Bu kullanıcının yeni kaynaklar çalışma alanına eklemesini engeller |

*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:

* Log Analytics okuyucusu gibi tüm izleme verilerini okuma
* Otomasyon hesaplarını oluşturma ve yapılandırma
* Yönetim çözümlerini ekleme ve kaldırma

    > [!NOTE]
    > Başarıyla son iki eylemleri gerçekleştirmek için bu izin kaynak grubu veya abonelik düzeyinde verilmesi gerekir.

* Depolama hesabı anahtarlarını okuma
* Azure Depolama’dan günlüklerin toplanmasını yapılandırma
* Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  * VM'lere VM uzantısı ekleme
  * Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE]
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

Log Analytics katkıda bulunan rolü, aşağıdaki Azure eylemleri içerir:

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarını okumasına ve verilerde sorgu gerçekleştirmesine izin verir. Daha ayrıntılı seçeneklere bakın. |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırmaları ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:

* Abonelik - Abonelikteki tüm çalışma alanlarına erişim
* Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
* Kaynak - Yalnızca belirtilen çalışma alanına erişim

Doğru erişim denetimini güvence altına almak için atamaları Kaynak düzeyinde (çalışma alanı) gerçekleştirmeniz gerekir.  Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

### <a name="resource-permissions"></a>Kaynak izinleri

Kullanıcılar, kaynak bağlamı erişimi kullanarak bir çalışma alanından günlükleri sorgularsa, kaynak üzerinde aşağıdaki izinlere sahip olurlar:

| İzin | Açıklama |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Örnekler:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Kaynak için tüm günlük verilerini görüntüleme olanağı.  |
| `Microsoft.Insights/diagnosticSettings/write` | Tanılama ayarını bu kaynak için günlükleri ayarlamaya izin verecek şekilde yapılandırma özelliği. |

`/read`izin genellikle, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) ve [katkıda](../../role-based-access-control/built-in-roles.md#contributor) bulunan rolleri gibi  _\*/Read veya_ _\*_ Permissions içeren bir rolden verilir. Belirli eylemleri içeren özel rollerin veya adanmış yerleşik rollerin bu izni içeremediğini unutmayın.

Farklı tablolar için farklı erişim denetimi oluşturmak isterseniz, aşağıdaki [tablo başına erişim denetimi tanımlama](#table-level-rbac) bölümüne bakın.

## <a name="table-level-rbac"></a>Tablo düzeyi RBAC

**Tablo DÜZEYI RBAC** , diğer izinlerin yanı sıra bir Log Analytics çalışma alanındaki verilere daha ayrıntılı denetim tanımlamanızı sağlar. Bu denetim, yalnızca belirli bir kullanıcı kümesi için erişilebilen belirli veri türlerini tanımlamanızı sağlar.

Çalışma alanındaki belirli [tablolara](../log-query/logs-structure.md) erişim izni vermek ya da erişimi reddetmek için [Azure özel rolleriyle](../../role-based-access-control/custom-roles.md) tablo erişim denetimi uygulayabilirsiniz. Bu roller, kullanıcının [erişim modundan](design-logs-deployment.md#access-mode)bağımsız olarak, çalışma alanı bağlamı veya kaynak bağlamı [erişim denetimi modlarıyla](design-logs-deployment.md#access-control-mode) çalışma alanlarına uygulanır.

Tablo erişim denetimine erişimi tanımlamak için aşağıdaki eylemlerle [özel bir rol](../../role-based-access-control/custom-roles.md) oluşturun.

* Bir tabloya erişim vermek için, rol tanımının **Eylemler** bölümüne ekleyin.
* Bir tabloya erişimi reddetmek için, rol tanımının **NotActions** bölümüne ekleyin.
* Tüm tabloları belirtmek için * kullanın.

Örneğin, _sinyal_ ve _AzureActivity_ tablolarına erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Yalnızca _Securitybaseline_ 'e erişimi olan ve başka tablolar olmadan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
```

### <a name="custom-logs"></a>Özel günlükler

 Özel Günlükler ve HTTP Veri Toplayıcı API 'SI gibi veri kaynaklarından özel günlükler oluşturulur. Günlük türünü belirlemenin en kolay yolu, [günlük şemasında özel Günlükler](../log-query/get-started-portal.md#understand-the-schema)altında listelenen tabloları denetlemede olur.

 Şu anda tek tek özel günlüklere erişim izni veremez veya vermeyebilirsiniz, ancak tüm özel günlüklere erişim verebilir veya erişimi reddedebilirsiniz. Tüm özel günlüklere erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Bir kullanıcıya,  _\*/Read_ eylemini içeren standart okuyucu veya katkıda bulunan rollerle genel okuma izni verildiyse, tablo başına erişim denetimini geçersiz kılar ve tüm günlük verilerine erişim sağlar.
* Bir kullanıcıya tablo başına erişim verildiyse ancak başka izinler yoksa, API 'den günlük verilerine erişebilecek ancak Azure portal. Azure portal erişim sağlamak için, temel rolü olarak Log Analytics okuyucu kullanın.
* Aboneliğin yöneticileri, diğer izin ayarlarından bağımsız olarak tüm veri türlerine erişebilir.
* Çalışma alanı sahipleri, tablo başına erişim denetimi için diğer kullanıcılar gibi davranır.
* Atama sayısını azaltmak için bireysel kullanıcılar yerine güvenlik gruplarına roller atamanız gerekir. Bu Ayrıca, erişimi yapılandırmak ve doğrulamak için mevcut Grup Yönetimi araçlarını kullanmanıza yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md) veri merkezinizde veya diğer bulut ortamında bulunan bilgisayarlardaki verileri toplamak için.

* Azure VM 'lerinden veri toplamayı yapılandırmak için bkz. [Azure sanal makineler hakkında veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md) .
