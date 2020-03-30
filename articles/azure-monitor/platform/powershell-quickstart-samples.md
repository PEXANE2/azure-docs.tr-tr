---
title: Azure İzleyici PowerShell hızlı başlangıç örnekleri
description: Otomatik ölçeklendirme, uyarılar, web hook'lar ve arama Etkinliği günlükleri gibi Azure Monitor özelliklerine erişmek için PowerShell'i kullanın.
ms.subservice: ''
ms.topic: conceptual
ms.date: 2/14/2018
ms.openlocfilehash: 9f039f71954998ef561d1efd1e559318740c86ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274326"
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure İzleyici PowerShell hızlı başlangıç örnekleri
Bu makalede, Azure Monitor özelliklerine erişmenize yardımcı olmak için PowerShell komutları örnekleniyor.

> [!NOTE]
> Azure Monitor, 25 Eylül 2016'ya kadar "Azure Öngörüleri" adı verilen yeni adıdır. Ancak, ad boşlukları ve böylece aşağıdaki komutlar hala "öngörüler" sözcüğü içerir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="set-up-powershell"></a>PowerShell'i ayarlama
Henüz yapmadıysanız, PowerShell'i bilgisayarınızda çalışacak şekilde ayarlayın. Daha fazla bilgi için [PowerShell'in nasıl yüklenir ve yapılandırılın.](/powershell/azure/overview)

## <a name="examples-in-this-article"></a>Bu makaledeki örnekler
Makaledeki örnekler, Azure Monitor cmdlets'i nasıl kullanabileceğinizi göstermektedir. [Azure Monitor (Insights) Cmdlets](https://docs.microsoft.com/powershell/module/az.applicationinsights)adresindeki Azure Monitor PowerShell cmdlet'lerinin tüm listesini de inceleyebilirsiniz.

## <a name="sign-in-and-use-subscriptions"></a>Oturum açma ve abonelikleri kullanma
İlk olarak, Azure aboneliğinizde oturum açın.

```powershell
Connect-AzAccount
```

Ekranda bir işaret görürsünüz. Hesabınızı oturum aladıktan sonra Kiracı Kimliği ve varsayılan Abonelik Kimliği görüntülenir. Tüm Azure cmdlet'leri varsayılan aboneliğiniz bağlamında çalışır. Erişiminiz olan aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

```powershell
Get-AzSubscription
```

Çalışma bağlamınızı (komutlarınızın hangi aboneye karşı çalıştırıldığı) görmek için aşağıdaki komutu kullanın:

```powershell
Get-AzContext
```
Çalışma bağlamınızı farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın:

```powershell
Set-AzContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Abonelik için Etkinlik günlüğünü alın
[Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet'i kullanın.  Aşağıda bazı yaygın örnekler verilmiştir. Etkinlik Günlüğü, operasyonların son 90 gününü tutar. Bu saatten önce tarihlerin kullanılması bir hata iletisi ile sonuçlanır.  

Aşağıdaki komutlarda hangi saatlerde kullanılacağını doğrulamak için geçerli tarih/saatin ne olduğunu görün:
```powershell
Get-Date
```

Bu zaman/tarihten günümüze günlük girişlerini alın:

```powershell
Get-AzLog -StartTime 2019-03-01T10:30
```

Saat/tarih aralığı arasında günlük girişleri alın:

```powershell
Get-AzLog -StartTime 2019-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir kaynak grubundan günlük girişleri alın:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Saat/tarih aralığı arasında belirli bir kaynak sağlayıcıdan günlük girişleri alın:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir arayanla tüm günlük girişlerini alın:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Aşağıdaki komut, etkinlik günlüğünden son 1000 olayı alır:

```powershell
Get-AzLog -MaxRecord 10
```

`Get-AzLog`diğer birçok parametreyi destekler. Daha `Get-AzLog` fazla bilgi için başvuruya bakın.

> [!NOTE]
> `Get-AzLog`sadece 15 günlük bir tarih sağlar. **-MaxRecords** parametresini kullanmak, son N olaylarını 15 günden fazla sorgulamanızı sağlar. 15 günden eski olaylara erişmek için REST API veya SDK (SDK'yı kullanan C# örneğini) kullanın. **StartTime'ı**dahil etmiyorsanız, varsayılan değer **Bitiş Saati** eksi bir saattir. **EndTime'ı**içermiyorsanız, varsayılan değer geçerli saattir. Her zaman UTC bulunmaktadır.
> 
> 

## <a name="retrieve-alerts-history"></a>Uyarıları geçmişle ele alma
Tüm uyarı olaylarını görüntülemek için aşağıdaki örnekleri kullanarak Azure Kaynak Yöneticisi günlüklerini sorgulayabilirsiniz.

```powershell
Get-AzLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Belirli bir uyarı kuralının geçmişini görüntülemek için, uyarı kuralının `Get-AzAlertHistory` kaynak kimliğini geçerek cmdlet'i kullanabilirsiniz.

```powershell
Get-AzAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Cmdlet `Get-AzAlertHistory` çeşitli parametreleri destekler. Daha fazla bilgi için [Bkz. Uyarı Geçmişi Alın.](https://msdn.microsoft.com/library/mt282453.aspx)

## <a name="retrieve-information-on-alert-rules"></a>Uyarı kuralları yla ilgili bilgileri alma
Aşağıdaki komutların tümü "montest" adlı bir Kaynak Grubu üzerinde hareket eder.

Uyarı kuralının tüm özelliklerini görüntüleyin:

```powershell
Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Kaynak grubundaki tüm uyarıları alın:

```powershell
Get-AzAlertRule -ResourceGroup montest
```

Hedef kaynak için ayarlanan tüm uyarı kurallarını alın. Örneğin, vm'de ayarlanan tüm uyarı kuralları.

```powershell
Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzAlertRule`diğer parametreleri destekler. Daha fazla bilgi için [Get-AlertRule'ye](https://msdn.microsoft.com/library/mt282459.aspx) bakın.

## <a name="create-metric-alerts"></a>Ölçüm uyarıları oluşturma
Bir uyarı `Add-AlertRule` kuralı oluşturmak, güncelleştirmek veya devre dışı bırakmak için cmdlet'i kullanabilirsiniz.

Sırasıyla ve sırasıyla e-posta ve `New-AzAlertRuleEmail` `New-AzAlertRuleWebhook`webhook özellikleri oluşturabilirsiniz. Uyarı kuralı cmdlet'inde, bu özellikleri Uyarı Kuralı'nın **Eylemler** özelliğine eylem olarak atayın.

Aşağıdaki tabloda, bir metrik kullanarak bir uyarı oluşturmak için kullanılan parametreler ve değerler açıklanmaktadır.

| parametre | value |
| --- | --- |
| Adı |simpletestdiskwrite |
| Bu uyarı kuralının konumu |Doğu ABD |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
| Oluşturulan uyarının MetricName'si |\PhysicalDisk(_Total)\Disk Yazma/sn. Tam `Get-MetricDefinitions` metrik adlar nasıl alınacağını anlatan cmdlet'e bakın |
| operator |GreaterThan |
| Eşik değeri (bu metrik için sayım/sn) |1 |
| WindowSize (hh:mm:ss biçimi) |00:05:00 |
| toplayıcı (bu durumda ortalama sayısı kullanan metrik, istatistik) |Ortalama |
| özel e-postalar (dize dizisi) |'foo@example.com','bar@example.com' |
| sahiplerine, katkıda bulunanlara ve okuyuculara e-posta gönderme |-SendtoServiceOwners |

E-posta eylemi oluşturma

```powershell
$actionEmail = New-AzAlertRuleEmail -CustomEmail myname@company.com
```

Webhook eylemi oluşturma

```powershell
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Klasik bir VM'de CPU% ölçümünde uyarı kuralını oluşturun

```powershell
Add-AzMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Action $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Uyarı kuralını alma

```powershell
Get-AzAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Uyarı ekle cmdlet, verilen özellikler için zaten bir uyarı kuralı varsa kuralı da güncelleştirir. Bir uyarı kuralını devre dışı kalmak için parametre **-DisableRule'i**ekleyin.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Uyarılar için kullanılabilir ölçümlerin listesini alma
Belirli bir `Get-AzMetricDefinition` kaynak için tüm ölçümlerin listesini görüntülemek için cmdlet kullanabilirsiniz.

```powershell
Get-AzMetricDefinition -ResourceId <resource_id>
```

Aşağıdaki örnek, metrik Adı ve Birimi içeren bir tablo oluşturur.

```powershell
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Kullanılabilir `Get-AzMetricDefinition` seçeneklerin tam listesi [Get-MetricDefinitions'te](https://msdn.microsoft.com/library/mt282458.aspx)mevcuttur.

## <a name="create-and-manage-activity-log-alerts"></a>Etkinlik Günlüğü uyarıları oluşturma ve yönetme
Etkinlik Günlüğü `Set-AzActivityLogAlert` uyarısı ayarlamak için cmdlet'i kullanabilirsiniz. Etkinlik Günlüğü uyarısı, önce koşullarınızı koşullar sözlüğü olarak tanımlamanızı, ardından bu koşulları kullanan bir uyarı oluşturmanızı gerektirir.

```powershell

$condition1 = New-AzActivityLogAlertCondition -Field 'category' -Equal 'Administrative'
$condition2 = New-AzActivityLogAlertCondition -Field 'operationName' -Equal 'Microsoft.Compute/virtualMachines/write'
$additionalWebhookProperties = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
$additionalWebhookProperties.Add('customProperty', 'someValue')
$actionGrp1 = New-AzActionGroup -ActionGroupId '/subscriptions/<subid>/providers/Microsoft.Insights/actiongr1' -WebhookProperty $additionalWebhookProperties
Set-AzActivityLogAlert -Location 'Global' -Name 'alert on VM create' -ResourceGroupName 'myResourceGroup' -Scope '/subscriptions/<subid>' -Action $actionGrp1 -Condition $condition1, $condition2

```

Ek webhook özellikleri isteğe bağlıdır. Bir Etkinlik Günlüğü Uyarısı'nın içeriğini `Get-AzActivityLogAlert`kullanarak geri alabilirsiniz.

## <a name="create-and-manage-autoscale-settings"></a>Otomatik Ölçek ayarlarını oluşturma ve yönetme
Bir kaynağın (Web uygulaması, VM, Bulut Hizmeti veya Sanal Makine Ölçeği Kümesi) yalnızca bir otomatik ölçek ayarı bunun için yapılandırılabilir.
Ancak, her otomatik ölçek ayarı birden çok profil eolabilir. Örneğin, biri performans tabanlı ölçek profili için, ikincisi de zamanlama tabanlı profil için. Her profilüzerinde yapılandırılan birden çok kural olabilir. Otomatik Ölçeklendirme hakkında daha fazla bilgi için, [uygulamanın otomatik ölçeklendirmesine bakın.](../../cloud-services/cloud-services-how-to-scale-portal.md)

Kullanılacak adımlar şunlardır:

1. Kural(lar) oluşturun.
2. Profil(ler) daha önce oluşturduğunuz profillerin eşlenesini oluşturun.
3. İsteğe bağlı: Webhook ve e-posta özelliklerini yapılandırarak otomatik ölçeklendirme için bildirimler oluşturun.
4. Önceki adımlarda oluşturduğunuz profilleri ve bildirimleri eşleyerek hedef kaynağın üzerinde ad içeren bir otomatik ölçek ayarı oluşturun.

Aşağıdaki örnekler, CPU kullanım metrikünü kullanarak bir Windows işletim sistemi için Sanal Makine Ölçeği Kümesi için otomatik ölçek ayarını nasıl oluşturabileceğinizi gösterir.

İlk olarak, örnek sayısı artarak ölçeklendirecek bir kural oluşturun.

```powershell
$rule1 = New-AzAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Ardından, örnek sayısı azaldığı ölçeklendirilen bir kural oluşturun.

```powershell
$rule2 = New-AzAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Ardından, kurallar için bir profil oluşturun.

```powershell
$profile1 = New-AzAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Bir webhook özelliği oluşturun.

```powershell
$webhook_scale = New-AzAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

E-posta ve daha önce oluşturduğunuz webhook dahil olmak üzere otomatik ölçeklendirme ayarı için bildirim özelliğini oluşturun.

```powershell
$notification1= New-AzAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Son olarak, daha önce oluşturduğunuz profili eklemek için otomatik ölçek ayarını oluşturun. 

```powershell
Add-AzAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Otomatik Ölçek ayarlarını yönetme hakkında daha fazla bilgi [için](https://msdn.microsoft.com/library/mt282461.aspx)Bkz.

## <a name="autoscale-history"></a>Otomatik ölçeklendirme geçmişi
Aşağıdaki örnek, en son otomatik ölçeklendirme ve uyarı olaylarını nasıl görüntülediğinizi gösterir. Otomatik ölçek geçmişini görüntülemek için etkinlik günlüğü aramasını kullanın.

```powershell
Get-AzLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

AutoScale geçmişini `Get-AzAutoScaleHistory` almak için cmdlet kullanabilirsiniz.

```powershell
Get-AzAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Daha fazla bilgi için Bkz. [Get-AutoscaleHistory.](https://msdn.microsoft.com/library/mt282464.aspx)

### <a name="view-details-for-an-autoscale-setting"></a>Otomatik ölçek lendirme ayarı için ayrıntıları görüntüleme
Otomatik ölçek `Get-Autoscalesetting` ayarı hakkında daha fazla bilgi almak için cmdlet'i kullanabilirsiniz.

Aşağıdaki örnekte, 'myrg1' kaynak grubundaki tüm otomatik ölçeklendirme ayarlarıyla ilgili ayrıntılar gösterilmektedir.

```powershell
Get-AzAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Aşağıdaki örnekte, kaynak grubundaki tüm otomatik ölçek ayarları ve özellikle 'MyScaleVMSSSetting' adlı otomatik ölçek ayarı ile ilgili ayrıntılar gösterilmektedir.

```powershell
Get-AzAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Otomatik ölçek ayarını kaldırma
Otomatik ölçek `Remove-Autoscalesetting` ayarını silmek için cmdlet'i kullanabilirsiniz.

```powershell
Remove-AzAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Etkinlik günlüğü için günlük profillerini yönetme
Etkinlik *günlüğünden* bir depolama hesabına bir günlük profili oluşturabilir ve veri dışa aktarabilirsiniz ve bunun için veri saklamayı yapılandırabilirsiniz. İsteğe bağlı olarak, verileri Olay Hub'ınıza da aktarabilirsiniz. Bu özellik şu anda Önizleme'de dir ve abonelik başına yalnızca bir günlük profili oluşturabilirsiniz. Günlük profilleri oluşturmak ve yönetmek için geçerli aboneliğiniz ile aşağıdaki cmdletleri kullanabilirsiniz. Belirli bir aboneliği de seçebilirsiniz. PowerShell varsayılan olarak geçerli abonelik için olsa da, bunu her zaman kullanarak `Set-AzContext`değiştirebilirsiniz. Etkinlik günlüğünü, verileri bu abonelik içindeki herhangi bir depolama hesabına veya Olay Hub'ına yönlendirmek için yapılandırabilirsiniz. Veriler JSON formatında blob dosyaları olarak yazılır.

### <a name="get-a-log-profile"></a>Günlük profili oluşturma
Varolan günlük profillerinizi getirmek `Get-AzLogProfile` için cmdlet'i kullanın.

### <a name="add-a-log-profile-without-data-retention"></a>Veri saklama olmadan günlük profili ekleme
```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Location global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Günlük profilini kaldırma
```powershell
Remove-AzLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Veri saklama ile günlük profili ekleme
**-RetentionInDays** özelliğini, verilerin tutulduğu pozitif tamsayı olarak gün sayısıyla belirtebilirsiniz.

```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Location global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Bekletme ve EventHub içeren günlük profili ekleme
Verilerinizi depolama hesabına yönlendirmenin yanı sıra, verilerinizi bir Olay Hub'ına da aktarabilirsiniz. Bu önizleme sürümünde depolama hesabı yapılandırması zorunludur ancak Olay Hub yapılandırması isteğe bağlıdır.

```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Tanılama günlüklerini yapılandırma
Birçok Azure hizmeti, aşağıdakilerden birini veya daha fazlasını yapabilen ek günlükler ve telemetriler sağlar: 
 - Azure Depolama hesabınızda veri kaydetmek için yapılandırılabilir
 - Olay Hub'larına gönderildi
 - bir Log Analytics çalışma alanına gönderilir. 

İşlem yalnızca kaynak düzeyinde gerçekleştirilebilir. Depolama hesabı veya olay hub'ı, tanılama ayarının yapılandırıldığı hedef kaynakla aynı bölgede bulunmalıdır.

### <a name="get-diagnostic-setting"></a>Tanılama ayarını alın
```powershell
Get-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Tanı ayarını devre dışı

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Bekletme olmadan tanılama ayarını etkinleştirme

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Bekletme ile tanılama ayarını etkinleştirme

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Belirli bir günlük kategorisi için bekletme ile tanılama ayarını etkinleştirme

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Olay Hub'ları için tanı ayarını etkinleştirme

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Log Analytics için tanı ayarını etkinleştirme

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId /subscriptions/s1/resourceGroups/insights-integration/providers/providers/microsoft.operationalinsights/workspaces/myWorkspace -Enabled $true

```

WorkspaceId özelliğinin çalışma alanının *kaynak kimliğini* aldığını unutmayın. Log Analytics çalışma alanınızın kaynak kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

```powershell
(Get-AzOperationalInsightsWorkspace).ResourceId

```

Bu komutlar, birden çok hedefe veri göndermek için birleştirilebilir.

