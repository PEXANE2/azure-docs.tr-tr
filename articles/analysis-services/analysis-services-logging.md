---
title: Azure Analiz Hizmetleri için tanılama günlüğü | Microsoft Dokümanlar
description: Azure Çözümleme Hizmetleri sunucunuzu izlemek için Azure kaynak tanılama günlüğe kaydetmeyi nasıl ayarlayabileceğinizi açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0f13f297facedceb50920c0f6afca63fe1df0b48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266188"
---
# <a name="setup-diagnostic-logging"></a>Tanılama günlüğüne kaydetmeyi ayarlama

Analiz Hizmetleri çözümlerinin önemli bir parçası sunucularınızın nasıl performans gösterdiğini izlemektir. [Azure kaynak günlükleri](../azure-monitor/platform/platform-logs-overview.md)ile günlükleri izleyebilir ve [Azure Depolama'ya](https://azure.microsoft.com/services/storage/)gönderebilir, Azure [Etkinlik Hub'larına](https://azure.microsoft.com/services/event-hubs/)aktarabilir ve [Azure Monitor günlüklerine](../azure-monitor/azure-monitor-log-hub.md)aktarabilirsiniz.

![Depolama, Olay Hub'ları veya Azure Monitör günlüklerine tanılama günlüğü](./media/analysis-services-logging/aas-logging-overview.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="whats-logged"></a>Günlüğe neler kaydedilir?

**Motor,** **Servis**ve **Ölçümler** kategorilerini seçebilirsiniz.

### <a name="engine"></a>Altyapı

**Motor'u** seçmek tüm [xEvents](https://docs.microsoft.com/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events)günlüklerini . Tek tek olayları seçemezsiniz. 

|XEvent kategorileri |Olay adı  |
|---------|---------|
|Güvenlik Denetimi    |   Audit Login      |
|Güvenlik Denetimi    |   Audit Logout      |
|Güvenlik Denetimi    |   Denetim Sunucusu Başlar ve Durur      |
|İlerleme Raporları     |   İlerleme Raporu Başlıyor      |
|İlerleme Raporları     |   İlerleme Raporu Sonu      |
|İlerleme Raporları     |   İlerleme Raporu Güncel      |
|Sorgular     |  Query Begin       |
|Sorgular     |   Query End      |
|Komutlar     |  Komut Başla       |
|Komutlar     |  Komut Sonu       |
|Hatalar & Uyarılar     |   Hata      |
|Bul     |   Sonu Keşfet      |
|Bildirim     |    Bildirim     |
|Oturum     |  Session Initialize       |
|Kilitler    |  Çıkmaz       |
|Sorgu İşleme     |   VertiPaq SE Sorgusu Başla      |
|Sorgu İşleme     |   VertiPaq SE Sorgu Sonu      |
|Sorgu İşleme     |   VertiPaq SE Sorgu Önbellek Eşleştirme      |
|Sorgu İşleme     |   Doğrudan Sorgu Başla      |
|Sorgu İşleme     |  Doğrudan Sorgu Sonu       |

### <a name="service"></a>Hizmet

|İşlem adı  |Oluşur  |
|---------|---------|
|ResumeServer     |    Sunucuya devam edin     |
|Askıya Alma Sunucusu    |   Sunucumu duraklatma      |
|Silme Sunucusu     |    Sunucu silme     |
|Sunucu'yı Yeniden Başlat    |     Kullanıcı SSMS veya PowerShell aracılığıyla bir sunucuyeniden başlatın    |
|GetServerLogFiles    |    Kullanıcı PowerShell üzerinden sunucu günlüğü dışa aktarın     |
|İhracat Modeli     |   Kullanıcı Visual Studio'da Açık'ı kullanarak portalda bir model dışa aktarmaz     |

### <a name="all-metrics"></a>Tüm ölçümler

Ölçümler kategorisi, aynı [Sunucu ölçümlerini](analysis-services-monitor.md#server-metrics) AzureMetrics tablosuna kaydeder. Sorgu [ölçeğini](analysis-services-scale-out.md) kullanıyorsanız ve her okuma yinelemesi için ölçümleri ayırmanız gerekiyorsa, **Bunun** yerine İşlem Adı'nın **LogMetric'e**eşit olduğu Azure Diagnostics tablosunu kullanın.

## <a name="setup-diagnostics-logging"></a>Tanılama günlüğünü ayarlama

### <a name="azure-portal"></a>Azure portalında

1. [Azure portalı](https://portal.azure.com) > sunucusunda, sol gezintide **Tanılama günlüklerini** tıklatın ve ardından **tanılamayı aç'ı**tıklatın.

    ![Azure portalında Azure Cosmos DB için tanılama günlüğe kaydetmeyi açın](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. **Tanılama ayarları** sayfasında aşağıdaki seçenekleri belirtin: 

    * **Adı**. Oluşturmak için günlükleri için bir ad girin.

    * **Bir depolama hesabına arşivleyin.** Bu seçeneği kullanmak için, bağlanmak için varolan bir depolama hesabına ihtiyacınız var. Bkz. [Bir depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Bir Kaynak Yöneticisi, genel amaçlı hesap oluşturmak için yönergeleri izleyin ve ardından portaldaki bu sayfaya dönerek depolama hesabınızı seçin. Yeni oluşturulan depolama hesaplarının açılan menüde görünmesi birkaç dakika sürebilir.
    * **Bir olay hub'ına akış**. Bu seçeneği kullanmak için, bağlanmak için varolan bir Olay Hub'ı ad alanı ve olay hub'ına ihtiyacınız vardır. Daha fazla bilgi için bkz. [Azure portalı kullanarak Event Hubs ad alanı ve olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından Olay Hub'ı ad alanını ve ilke adını seçmek için portaldaki bu sayfaya dönün.
    * **Azure Monitörü'ne (Log Analytics çalışma alanı) gönderin.** Bu seçeneği kullanmak için, varolan bir çalışma alanı kullanın veya portalda yeni bir çalışma alanı kaynağı [oluşturun.](../azure-monitor/learn/quick-create-workspace.md) Günlüklerinizi görüntüleme hakkında daha fazla bilgi için bu makalede [Log Analytics çalışma alanında günlükleri](#view-logs-in-log-analytics-workspace) görüntüle'ye bakın.

    * **Motor**. xEvents'i günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme ediyorsanız, tanılama günlükleri için bekletme süresini seçebilirsiniz. Günlükler bekletme süresi dolduktan sonra otomatik olarak silinir.
    * **Hizmet**. Hizmet düzeyi olaylarını günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme yapıyorsanız tanılama günlüklerinin saklama süresini belirleyebilirsiniz. Günlükler bekletme süresi dolduktan sonra otomatik olarak silinir.
    * **Ölçümler**. Ayrıntılı verileri Ölçümler'de depolamak için bu seçeneği [belirleyin.](analysis-services-monitor.md#server-metrics) Bir depolama hesabına arşivleme yapıyorsanız tanılama günlüklerinin saklama süresini belirleyebilirsiniz. Günlükler bekletme süresi dolduktan sonra otomatik olarak silinir.

3. **Kaydet**'e tıklayın.

    "Çalışma alanı adı> için \<tanılama güncelleştirmek için başarısız olduğunu belirten bir hata alırsanız. abonelik \<kimliği> microsoft.insights kullanmak için kayıtlı değildir." Hesabı kaydetmek için [Sorun Giderme Azure Tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergelerini izleyin ve bu yordamı yeniden deneyin.

    Tanılama günlüklerinizin gelecekte herhangi bir noktada nasıl kaydedilip kaydedilebildiğini değiştirmek isterseniz, ayarları değiştirmek için bu sayfaya dönebilirsiniz.

### <a name="powershell"></a>PowerShell

İşte size gitmek için temel komutları vardır. PowerShell'i kullanarak bir depolama hesabına günlüğe kaydetme konusunda adım adım yardım almak istiyorsanız, bu makalenin ilerleyen yerlerine bakın.

Ölçümleri ve tanılama günlüğüne kaydetmeyi PowerShell kullanarak etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerinin bir depolama hesabına kaydedilmesini sağlamak için şu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı kimliği, günlükleri göndermek istediğiniz depolama hesabının kaynak kimliğidir.

- Tanılama günlüklerinin akışını bir olay hub'ına yapmak için şu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği, şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Tanılama günlüklerini Log Analytics çalışma alanına göndermek için şu komutu kullanın:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanının kaynak kimliğini almak için aşağıdaki komutu kullanabilirsiniz:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

Bu parametreleri bir arada kullanarak birden fazla çıkış seçeneği oluşturabilirsiniz.

### <a name="rest-api"></a>REST API

[Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirmeyi](https://docs.microsoft.com/rest/api/monitor/) öğrenin. 

### <a name="resource-manager-template"></a>Resource Manager şablonu

[Resource Manager şablonu kullanarak kaynak oluşturma sırasında tanılama ayarını etkinleştirmeyi](../azure-monitor/platform/diagnostic-settings-template.md) öğrenin. 

## <a name="manage-your-logs"></a>Günlüklerinizi yönetme

Günlükler genellikle günlük kurulumu ndan sonra birkaç saat içinde kullanılabilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Eski günlüklerin depolama hesabınızdan silinmesi için bir bekletme süresi ayarladıktan emin olun.

## <a name="view-logs-in-log-analytics-workspace"></a>Günlük Analytics çalışma alanında günlükleri görüntüleme

Ölçümler ve sunucu olayları, yan yana analiz için Log Analytics çalışma alanı kaynağınızdaki xEvents ile tümleştirilir. Log Analytics çalışma alanı, mimarinizde tanısal günlük verilerinin bütünsel bir görünümünü sağlayan diğer Azure hizmetlerinden etkinlikler alacak şekilde de yapılandırılabilir.

Tanılama verilerinizi görüntülemek için Log Analytics çalışma alanında, sol menüden **Günlükler'i** açın.

![Azure portalında Günlük Arama seçenekleri](./media/analysis-services-logging/aas-logging-open-log-search.png)

Sorgu oluşturucuda, **LogManagement** > **AzureDiagnostics'i**genişletin. AzureDiagnostics, Motor ve Servis olaylarını içerir. Bir sorgunun anında oluşturulduğunu fark edin. EventClass'ın\_alanı, şirket içi günlüğe kaydetme için xEvents kullandıysanız tanıdık gelen xEvent adlarını içerir. **EventClass'ı\_** veya olay adlarından birini tıklatın ve Log Analytics çalışma alanı sorgu oluşturmaya devam ediyor. Sorgularınızı daha sonra kullanmak üzere kaydetmeyi unutmayın.

### <a name="example-queries"></a>Örnek sorgular

#### <a name="example-1"></a>Örnek 1

Aşağıdaki sorgu, bir model veritabanı ve sunucu için her sorgu sonu/yenileme son olayı için süreleri döndürür. Ölçeklendirilirse, çoğaltma numarası ServerName_s dahil olduğundan, sonuçlar yineleme tarafından bölünur. RootActivityId_g göre gruplandırma, Azure Diagnostics REST API'den alınan satır sayısını azaltır ve [Log Analytics Rate sınırlarında](https://dev.loganalytics.io/documentation/Using-the-API/Limits)açıklandığı gibi sınırlar içinde kalmaya yardımcı olur.

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

Aşağıdaki sorgu, bir sunucu için bellek ve QPU tüketimini döndürür. Ölçeklendirilirse, çoğaltma numarası ServerName_s dahil olduğundan, sonuçlar yineleme tarafından bölünur.

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

Aşağıdaki sorgu, bir sunucu için Satır okuma/sn Analiz Hizmetleri motor performans sayaçlarını döndürür.

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

Kullanabileceğiniz yüzlerce sorgu vardır. Sorgular hakkında daha fazla bilgi edinmek için [Azure Monitor günlük sorgularına başlayın'](../azure-monitor/log-query/get-started-queries.md)a bakın.


## <a name="turn-on-logging-by-using-powershell"></a>PowerShell'i kullanarak günlüğe kaydetmeyi açın

Bu hızlı öğreticide, Çözümleme Hizmeti sunucunuzla aynı abonelik ve kaynak grubunda bir depolama hesabı oluşturursunuz. Daha sonra tanılama günlüğe kaydetmeyi açmak ve çıktıyı yeni depolama hesabına göndermek için Set-AzDiagnosticSetting'i kullanırsınız.

### <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara sahip olmalısınız:

* Varolan bir Azure Çözümleme Hizmetleri sunucusu. Sunucu kaynağı oluşturma yla ilgili talimatlar için azure [portalında sunucu oluşturma](analysis-services-create-server.md)veya [PowerShell'i kullanarak bir Azure Analiz Hizmetleri sunucusu oluşturma](analysis-services-create-powershell.md)'na bakın.

### <a name="aconnect-to-your-subscriptions"></a></a>Aboneliklerinize bağlanma

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```powershell
Get-AzSubscription
```

Ardından, günlüğe kaydettiğiniz Azure Analiz Hizmetleri hesabıyla ilişkili aboneliği belirtmek için şunları yazın:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden çok aboneliğiniz varsa, aboneliği belirtmeniz önemlidir.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma

Sunucunuzla aynı abonelikte olması koşuluyla, günlükleriniz için varolan bir depolama hesabı kullanabilirsiniz. Bu öğretici için, Çözüm hizmetleri günlüklerine adanmış yeni bir depolama hesabı oluşturursunuz. Bunu kolaylaştırmak için, depolama hesabı ayrıntılarını **sa**adlı bir değişkende depolıyorsun.

Ayrıca, Çözümleme Hizmetleri sunucunuzu içeren kaynak grubuyla aynı kaynak grubunu da kullanırsınız. `awsales_resgroup`'nin `awsaleslogs`yerine geçen `West Central US` değerler ve kendi değerlerinizle:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Günlükleriniz için sunucu hesabını belirleme

Hesap adını, Kaynak Adı'nın hesabın adı olduğu bir değişken adlı **hesaba**ayarlayın.

```powershell
$account = Get-AzResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Günlü kaydını etkinleştir

Günlüğe kaydetmeyi etkinleştirmek için, set-AzDiagnosticSetting cmdlet'i yeni depolama hesabı, sunucu hesabı ve kategori değişkenleriyle birlikte kullanın. **-Etkin** bayrağı **$true**ayarlayarak aşağıdaki komutu çalıştırın:

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

Bu çıktı, artık sunucu için günlük kullanımının etkin olduğunu onaylar ve bilgileri depolama hesabına kaydeder.

Günlükleriniz için bekletme ilkesini, eski günlüklerin otomatik olarak silinmesi için de ayarlayabilirsiniz. Örneğin, **-BekletmeEtkin** bayrağı kullanarak **$true**ve **-BekletmeGün parametresini** **90'a**ayarlayın. 90 günden eski günlükler otomatik olarak silinir.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynak tanılama günlüğe kaydetme](../azure-monitor/platform/platform-logs-overview.md)hakkında daha fazla bilgi edinin.

PowerShell yardım [Set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) bakın.
