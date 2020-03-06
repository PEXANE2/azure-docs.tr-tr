---
title: Azure Analysis Services için tanılama günlüğü | Microsoft Docs
description: Azure Analysis Services sunucunuzu izlemek için Azure Kaynak tanılama günlüğü 'nün nasıl ayarlanacağını açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0f13f297facedceb50920c0f6afca63fe1df0b48
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78375149"
---
# <a name="setup-diagnostic-logging"></a>Tanılama günlüğüne kaydetmeyi ayarlama

Herhangi bir Analysis Services çözümü önemli bir parçası sunucularınızı performansını izler. [Azure Kaynak günlükleri](../azure-monitor/platform/platform-logs-overview.md)Ile [Azure depolama](https://azure.microsoft.com/services/storage/)'ya günlükleri izleyip gönderebilir, [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)akışa alabilir ve [Azure izleyici günlüklerine](../azure-monitor/azure-monitor-log-hub.md)aktarabilirsiniz.

![Depolama, Event Hubs veya Azure Izleyici günlüklerine tanılama günlüğü](./media/analysis-services-logging/aas-logging-overview.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="whats-logged"></a>Günlüğe kaydedilenler?

**Motor**, **hizmet**ve **ölçüm** kategorilerini seçebilirsiniz.

### <a name="engine"></a>Altyapısı

**Motor** seçildiğinde tüm [XEvents](https://docs.microsoft.com/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events)günlüğe kaydedilir. Olayları tek tek seçemezsiniz. 

|XEvent kategorileri |Olay adı  |
|---------|---------|
|Güvenlik Denetimi    |   Denetim oturum açma      |
|Güvenlik Denetimi    |   Denetim oturumunu kapatma      |
|Güvenlik Denetimi    |   Denetim sunucusu başlatır ve durdurur      |
|İlerleme raporları     |   İlerleme raporu başlangıç      |
|İlerleme raporları     |   İlerleme raporu bitiş olayı      |
|İlerleme raporları     |   İlerleme raporu geçerli      |
|Sorgular     |  Sorgu başlangıç       |
|Sorgular     |   Sorgu bitiş olayı      |
|Komutlar     |  Komut başlangıç       |
|Komutlar     |  Komut bitiş olayı       |
|Hatalar ve uyarılar     |   Hata      |
|Keşif     |   Son keşfedin      |
|Bildirim     |    Bildirim     |
|Oturum     |  Oturum başlatma       |
|Kilitler    |  Çıkmaz       |
|Sorgu işleme     |   VertiPaq SE sorgusu başla      |
|Sorgu işleme     |   VertiPaq SE sorgusu bitiş      |
|Sorgu işleme     |   VertiPaq SE sorgusu önbellek eşleştir      |
|Sorgu işleme     |   Doğrudan sorgu başlangıç      |
|Sorgu işleme     |  Doğrudan sorgu bitiş olayı       |

### <a name="service"></a>Hizmet

|İşlem adı  |Gerçekleşir,  |
|---------|---------|
|ResumeServer     |    Bir sunucuyu devam ettirin     |
|SuspendServer    |   Bir sunucu pause      |
|DeleteServer     |    Bir sunucu silme     |
|RestartServer    |     Kullanıcı SSMS veya PowerShell aracılığıyla bir sunucu yeniden başlatılır.    |
|GetServerLogFiles    |    Sunucu günlüğü PowerShell aracılığıyla kullanıcı verir     |
|ExportModel     |   Visual Studio'da Aç kullanarak Kullanıcı Portalı'nda bir modeli dışarı aktarır     |

### <a name="all-metrics"></a>Tüm ölçümleri

Ölçüm kategorisi, AzureMetrics tablosuna aynı [Sunucu ölçümlerini](analysis-services-monitor.md#server-metrics) günlüğe kaydeder. Sorgu [ölçeği](analysis-services-scale-out.md) genişletme kullanıyorsanız ve her bir okuma çoğaltması için ölçümleri ayırmanız gerekiyorsa, bunun yerine AzureDiagnostics tablosunu kullanın; burada, **OperationName** , **logmetric**öğesine eşittir.

## <a name="setup-diagnostics-logging"></a>Tanılama günlüğünü ayarlama

### <a name="azure-portal"></a>Azure portalı

1. [Azure portal](https://portal.azure.com) > sunucusu ' nda, sol gezinti bölmesinde **tanılama günlükleri** ' ne ve ardından **tanılamayı aç**' a tıklayın.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını açma](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. **Tanılama ayarları**' nda, aşağıdaki seçenekleri belirtin: 

    * **Ad**. Oluşturmak günlükleri için bir ad girin.

    * **Bir depolama hesabına arşivleme**. Bu seçeneği kullanmak için bağlanmak için mevcut bir depolama hesabı gerekir. Bkz. [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Bir kaynak yöneticisi, genel amaçlı hesap oluşturmak için yönergeleri izleyin, sonra Portalı'nda bu sayfaya dönerek, depolama hesabınızı seçin. Bu, yeni oluşturulan depolama hesapları, aşağı açılan menüsünün görünmesi birkaç dakika sürebilir.
    * **Bir olay hub 'ına akış**. Bu seçeneği kullanmak için bağlanmak için mevcut bir olay hub'ı ad alanı ve olay hub'gerekir. Daha fazla bilgi için bkz. [Azure Portal kullanarak Event Hubs ad alanı ve Olay Hub 'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından olay hub'ı ad alanı ve ilke adını seçmek için Portalı'nda bu sayfaya dönün.
    * **Azure izleyici 'ye (Log Analytics çalışma alanı) gönderin**. Bu seçeneği kullanmak için, var olan bir çalışma alanını kullanın ya da portalda [Yeni bir çalışma alanı kaynağı oluşturun](../azure-monitor/learn/quick-create-workspace.md) . Günlüklerinizi görüntüleme hakkında daha fazla bilgi için bu makaledeki [Log Analytics çalışma alanındaki günlükleri görüntüle](#view-logs-in-log-analytics-workspace) bölümüne bakın.

    * **Altyapı**. Xevent'ler yazmak için bu seçeneği seçin. Bir depolama hesabına arşivleme tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.
    * **Hizmeti**. Hizmet düzeyi olayları günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.
    * **Ölçümler**. Ayrıntılı verileri [ölçümlerde](analysis-services-monitor.md#server-metrics)depolamak için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.

3. **Kaydet** düğmesine tıklayın.

    "\<çalışma alanı adı için tanılama güncellenemedi > bildiren bir hata alırsanız. Abonelik \<abonelik kimliği > Microsoft. Insights 'ı kullanmak için kayıtlı değil. " hesabı kaydetmek için [sorun giderme Azure tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergeleri izleyin ve bu yordamı yeniden deneyin.

    Nasıl Tanılama günlüklerinizi herhangi bir noktada gelecekte kaydedilir değiştirmek istiyorsanız, ayarları değiştirmek için bu sayfaya geri dönebilirsiniz.

### <a name="powershell"></a>PowerShell

Koyulmanız için gerekli olan temel komutlar aşağıdadır. PowerShell kullanarak bir depolama hesabına günlük ayarlamaya ilişkin adım adım yardım istiyorsanız, bu makalenin ilerleyen bölümlerinde bkz.

Ölçümleri ve tanılama PowerShell kullanarak etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı kimliği kaynak depolama hesabı için günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanınızın kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

[Azure izleyici REST API kullanarak tanılama ayarlarını değiştirmeyi](https://docs.microsoft.com/rest/api/monitor/)öğrenin. 

### <a name="resource-manager-template"></a>Resource Manager şablonu

[Kaynak Yöneticisi şablonu kullanarak, kaynak oluşturma sırasında tanılama ayarlarını nasıl etkinleştireceğinizi](../azure-monitor/platform/diagnostic-settings-template.md)öğrenin. 

## <a name="manage-your-logs"></a>Günlüklerinizi yönetme

Günlükleri günlüğünü ayarlama, birkaç saat içinde genellikle kullanılabilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Bir bekletme dönemi için eski günlükleri depolama hesabınızdan silinir şekilde ayarladığınızdan emin olun.

## <a name="view-logs-in-log-analytics-workspace"></a>Log Analytics çalışma alanındaki günlükleri görüntüleme

Ölçümler ve sunucu olayları, yan yana analize yönelik Log Analytics çalışma alanı kaynağınızın xEvents ile tümleşiktir. Log Analytics çalışma alanı, mimariniz genelinde tanılama günlüğü verilerinin bir bütünsel görünümünü sağlayan diğer Azure hizmetlerinden olayları alacak şekilde de yapılandırılabilir.

Tanılama verilerinizi görüntülemek için, Log Analytics çalışma alanında, sol menüden **Günlükler** ' i açın.

![Azure portalında günlük arama seçenekleri](./media/analysis-services-logging/aas-logging-open-log-search.png)

Sorgu tasarımcısında, **Logmanagement** > **AzureDiagnostics**' ı genişletin. AzureDiagnostics altyapısı ve hizmet olaylarını içerir. Bir sorgunun açık bir şekilde oluşturulduğunu fark edin. EventClass\_s alanı xEvent adlarını içerir, bu da şirket içi günlüğe kaydetme için xEvents kullandıysanız tanıdık görünebilir. **EventClass\_s** ' e veya olay adlarından birine tıklayın ve Log Analytics çalışma alanı bir sorgu oluşturma devam eder. Sorgularınızı daha sonra kullanmak üzere kaydettiğinizden emin olun.

### <a name="example-queries"></a>Örnek sorgular

#### <a name="example-1"></a>Örnek 1

Aşağıdaki sorgu, bir model veritabanı ve sunucusu için her sorgu bitiş/yenileme bitiş olayı için süreleri döndürür. Ölçeği ölçeklenirse, çoğaltma numarası ServerName_s eklendiğinden sonuçlar çoğaltma tarafından bölünür. RootActivityId_g gruplama, Azure Tanılama REST API alınan satır sayısını azaltır ve [Log Analytics hız sınırları](https://dev.loganalytics.io/documentation/Using-the-API/Limits)bölümünde açıklandığı gibi limitlerin içinde kalmalarına yardımcı olur.

```Kusto
let window = AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName" and DatabaseName_s =~ "MyDatabaseName" ;
window
| where OperationName has "QueryEnd" or (OperationName has "CommandEnd" and EventSubclass_s == 38)
| where extract(@"([^,]*)", 1,Duration_s, typeof(long)) > 0
| extend DurationMs=extract(@"([^,]*)", 1,Duration_s, typeof(long))
| project  StartTime_t,EndTime_t,ServerName_s,OperationName,RootActivityId_g,TextData_s,DatabaseName_s,ApplicationName_s,Duration_s,EffectiveUsername_s,User_s,EventSubclass_s,DurationMs
| order by StartTime_t asc
```

#### <a name="example-2"></a>Örnek 2

Aşağıdaki sorgu, bir sunucu için bellek ve QPU kullanımını döndürür. Ölçeği ölçeklenirse, çoğaltma numarası ServerName_s eklendiğinden sonuçlar çoğaltma tarafından bölünür.

```Kusto
let window = AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName";
window
| where OperationName == "LogMetric" 
| where name_s == "memory_metric" or name_s == "qpu_metric"
| project ServerName_s, TimeGenerated, name_s, value_s
| summarize avg(todecimal(value_s)) by ServerName_s, name_s, bin(TimeGenerated, 1m)
| order by TimeGenerated asc 
```

#### <a name="example-3"></a>Örnek 3

Aşağıdaki sorgu, bir sunucu için Okuma/sn Analysis Services altyapısı performans sayaçlarını döndürür.

```Kusto
let window =  AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName";
window
| where OperationName == "LogMetric" 
| where parse_json(tostring(parse_json(perfobject_s).counters))[0].name == "Rows read/sec" 
| extend Value = tostring(parse_json(tostring(parse_json(perfobject_s).counters))[0].value) 
| project ServerName_s, TimeGenerated, Value
| summarize avg(todecimal(Value)) by ServerName_s, bin(TimeGenerated, 1m)
| order by TimeGenerated asc 
```

Sorguları kullanabileceğiniz yüzlerce vardır. Sorgular hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlük sorgularını kullanmaya başlama](../azure-monitor/log-query/get-started-queries.md).


## <a name="turn-on-logging-by-using-powershell"></a>PowerShell kullanarak günlük özelliğini açar

Bu hızlı öğreticiyi analiz sunucunuz olarak bir depolama hesabı aynı abonelikte ve kaynak grubu oluşturun. Ardından, yeni depolama hesabına çıkış göndererek tanılama günlüğünü açmak için set-AzDiagnosticSetting ' i kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara sahip olmalıdır:

* Mevcut bir Azure Analysis Services sunucusu. Sunucu kaynağı oluşturma hakkında yönergeler için, bkz. [Azure Portal sunucu oluşturma](analysis-services-create-server.md)veya [PowerShell kullanarak Azure Analysis Services sunucusu oluşturma](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a>aboneliklerinize bağlanmak </a>

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```powershell
Get-AzSubscription
```

Sonra oturum açtığınız Azure Analysis Services hesabıyla ilişkili aboneliği belirtmek için şunu yazın:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden fazla aboneliğiniz varsa, aboneliği belirtmek önemlidir.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma

Sunucunuz ile aynı abonelikte olması koşuluyla günlükleriniz için var olan bir depolama hesabı kullanabilirsiniz. Bu öğreticide, Analysis Services günlüklerine adanmış yeni bir depolama hesabı oluşturursunuz. Daha kolay hale getirmek için, depolama hesabı ayrıntılarını **sa**adlı bir değişkende depolarsınız.

Ayrıca Analysis Services sunucunuz içeren kümeyle aynı kaynak grubunu kullanın. `awsales_resgroup`, `awsaleslogs`ve `West Central US` değerlerini kendi değerlerinizle değiştirin:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Günlükleriniz için sunucu hesabı belirle

Hesap adını hesap adlı bir değişkene **ayarlayın, burada**resourceName hesabın adıdır.

```powershell
$account = Get-AzResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Günlüğü etkinleştirme

Günlüğe kaydetmeyi etkinleştirmek için, set-AzDiagnosticSetting cmdlet 'ini yeni depolama hesabı, sunucu hesabı ve kategori değişkenleriyle birlikte kullanın. Aşağıdaki komutu çalıştırın, **-Enabled** bayrağını **$true**olarak ayarlar:

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

Çıkış şu örneğe benzer olmalıdır:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Bu çıktı, günlüğe kaydetme işleminin artık sunucu için etkinleştirildiğini onaylar ve depolama hesabına bilgi kaydedilir.

Eski günlüklerin otomatik olarak silinir şekilde günlükleriniz için bekletme ilkesi de ayarlayabilirsiniz. Örneğin, **$true**için **-RetentionEnabled** bayrağını kullanarak bekletme ilkesi ayarlayın ve **-retentionındays** parametresini **90**olarak ayarlayın. 90 günden eski olan günlükler otomatik olarak silinir.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Kaynak tanılama günlüğü](../azure-monitor/platform/platform-logs-overview.md)hakkında daha fazla bilgi edinin.

PowerShell Yardımı 'nda [set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) bölümüne bakın.
