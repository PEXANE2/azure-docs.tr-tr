---
title: PowerShell ile Log Analytics'i & yapılandırın
description: Azure Monitor'daki Günlük Analytics çalışma alanları, şirket içi veya bulut altyapınızdaki sunuculardan veri depolar. Azure tanılama tarafından oluşturulduğunda Azure depolamasından makine verileri toplayabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/19/2019
ms.openlocfilehash: 2584cedceab1386cbab9c72bb4b510eebe2122bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054705"
---
# <a name="manage-log-analytics-workspace-in-azure-monitor-using-powershell"></a>PowerShell'i kullanarak Azure Monitör'de Günlük Analizi çalışma alanını yönetme

[Log Analytics PowerShell cmdlets'i](https://docs.microsoft.com/powershell/module/az.operationalinsights/) kullanarak Azure Monitor'daki Log Analytics çalışma alanında bir komut satırından veya komut dosyasının bir parçası olarak çeşitli işlevleri gerçekleştirebilirsiniz.  PowerShell ile gerçekleştirebileceğiniz görevlere örnek olarak şunlar verilebilir:

* Çalışma alanı oluşturma
* Bir çözüm ekleme veya kaldırma
* Kaydedilen aramaları içe aktarma ve dışa aktarma
* Bilgisayar grubu oluşturma
* Windows aracısı yüklü bilgisayarlardan IIS günlüklerinin toplanmasını etkinleştirme
* Linux ve Windows bilgisayarlarından performans sayaçları toplama
* Linux bilgisayarlarda syslog'dan etkinlik toplama
* Windows etkinlik günlüklerinden olayları toplama
* Özel etkinlik günlüklerini topla
* Azure sanal makinesine günlük analizi aracısını ekleme
* Azure tanılama kullanılarak toplanan verileri dizinlemek için günlük analitiğini yapılandırma

Bu makalede, PowerShell gerçekleştirebileceğiniz bazı işlevleri gösteren iki kod örneği verilmektedir.  Diğer işlevler için [Log Analytics PowerShell cmdlet referansına](https://docs.microsoft.com/powershell/module/az.operationalinsights/) başvurabilirsiniz.

> [!NOTE]
> Log Analytics daha önce Operasyonel Öngörüler olarak adlandırılıyordu, bu yüzden cmdlets'te kullanılan isimdir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu örnekler, Az.OperationalInsights modülünün 1.0.0 veya sonraki sürümleriyle çalışır.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Log Analytics Çalışma Alanı oluşturma ve yapılandırma
Aşağıdaki komut dosyası örneği nasıl gösterin:

1. Çalışma alanı oluşturma
2. Kullanılabilir çözümleri listele
3. Çalışma alanına çözümler ekleme
4. Kaydedilen aramaları alma
5. Kaydedilen aramaları dışa aktarma
6. Bilgisayar grubu oluşturma
7. Windows aracısı yüklü bilgisayarlardan IIS günlüklerinin toplanmasını etkinleştirme
8. Linux bilgisayarlarından Mantıksal Disk perf sayaçları toplamak (% Kullanılmış Inodes; Ücretsiz Megabayt; % Kullanılan Alan; Disk Aktarımları/sn; Disk Okuma/sn; Disk Yazma/sn)
9. Linux bilgisayarlarından syslog etkinliklerini toplayın
10. Windows bilgisayarlardan Uygulama Olay Günlüğü'nden Hata ve Uyarı olaylarını toplama
11. Windows bilgisayarlardan Bellek Kullanılabilir Mbytes performans sayacı toplama
12. Özel bir günlük toplama

```powershell

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique across all Azure subscriptions - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Perf | where ObjectName == "LogicalDisk" and CounterName == "Current Disk Queue Length" and InstanceName == "C:"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1",
    "description": "Example custom log datasource",
    "inputs": [
        {
            "location": {
            "fileSystemLocations": {
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                "linuxFileTypeLogPaths": [ "/var/logs" ]
                }
            },
        "recordDelimiter": {
            "regexDelimiter": {
                "pattern": "\\n",
                "matchIndex": 0,
                "matchIndexSpecified": true,
                "numberedGroup": null
                }
            }
        }
    ],
    "extractions": [
        {
            "extractionName": "TimeGenerated",
            "extractionType": "DateTime",
            "extractionProperties": {
                "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                    }
                }
            }
        ]
    }
"@

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json

# Create Computer Group based on a query
New-AzOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxPerformanceCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernel syslog collection"
Enable-AzOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs

New-AzOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

> [!NOTE]
> Özel bir günlük yapılandırmasını tanımlayan **CustomLogRawJson** parametresinin biçimi karmaşık olabilir. Varolan bir Özel Günlük için yapılandırmayı almak için [Get-AzOperationalInsightsDataSource'u](https://docs.microsoft.com/powershell/module/az.operationalinsights/get-azoperationalinsightsdatasource?view=azps-3.2.0) kullanın. **Özellikler** özelliği **CustomLogRawJson** parametresi için gerekli yapılandırmadır.

Yukarıdaki örnekte regexDelimiter yeni\\hat için "n" olarak tanımlanmıştır. Günlük sınırlayıcı da bir zaman damgası olabilir.  Desteklenen biçimler şunlardır:

| Biçimlendir | Json RegEx formatı standart bir RegEx her \ için iki \\ \\ kullanır, böylece bir RegEx uygulamasında test azaltmak eğer \ | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\d)|[0-9])/(([0-3]\\d)|(\\d))/((\\d{2})|(\\d{4}))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\s(AM|PM|am|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|May|Apr|Jul|Jun|Aug|Oct|Sep|Nov|Dec|jan|feb|mar|may|apr|jul|jun|aug|oct|sep|nov|dec)\\/((19|20)[0-9][0-9]))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Sept|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)).*?((?:(?:[0-2]?\\d{1})|(?:[3][01]{1})))(?![\\d]).*?((?:(?:[1]{1}\\d{1}\\d{1}\\d{1})|(?:[2]{1}\\d{3})))(?![\\d]).*?((?:(?:[0-1][0-9])|(?:[2][0-3])|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\s?(?:am|AM|pm|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]|[1][0-2])([0-2][0-9]|[3][0-1])\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]|[3][0-1])([0][1-9]|[1][0-2])[0-9]{2}\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s?([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0-1]?[0-9]|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> MMM'den sonra iki boşluk | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> nerede + is + veya a - <br> nerede zzzz zaman ofset | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\/((19|20)[0-9][0-9]):([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\s[\\+|\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> T, gerçek bir t harfidir. | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))T((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>Azure tanılama göndermek için Log Analytics'i yapılandırma
Azure kaynaklarının aracısız izlenmesi için kaynakların Azure tanılamalarını etkinleştirmesi ve Günlük Analizi çalışma alanına yazmak üzere yapılandırması gerekir. Bu yaklaşım, verileri doğrudan çalışma alanına gönderir ve verilerin bir depolama hesabına yazılmasını gerektirmez. Desteklenen kaynaklar şunlardır:

| Kaynak Türü | Günlükler | Ölçümler |
| --- | --- | --- |
| Uygulama Ağ Geçitleri    | Evet | Evet |
| Automation hesapları     | Evet | |
| Batch hesapları          | Evet | Evet |
| Veri Gölü analitiği     | Evet | |
| Veri Gölü deposu         | Evet | |
| Elastik SQL Havuzu        |     | Evet |
| Olay hub'ı ad alanı     |     | Evet |
| IoT Hub’ları                |     | Evet |
| Key Vault               | Evet | |
| Yük Dengeleyiciler          | Evet | |
| Logic Apps              | Evet | Evet |
| Ağ Güvenlik Grupları | Evet | |
| Redis için Azure Önbelleği             |     | Evet |
| Arama hizmetleri         | Evet | Evet |
| Servis Veri Günü ad alanı   |     | Evet |
| SQL (v12)               |     | Evet |
| Web Siteleri               |     | Evet |
| Web Server çiftlikleri        |     | Evet |

Kullanılabilir ölçümlerin ayrıntıları için Azure [Monitor ile desteklenen ölçümlere](../../azure-monitor/platform/metrics-supported.md)bakın.

Kullanılabilir günlüklerin ayrıntıları için, [desteklenen hizmetlere ve kaynak günlükleri için şemaya](../../azure-monitor/platform/diagnostic-logs-schema.md)bakın.

```powershell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Farklı aboneliklerde bulunan kaynaklardan günlük leri toplamak için önceki cmdlet'i de kullanabilirsiniz. Cmdlet, hem kaynak oluşturma günlüklerinin kimliğini hem de günlüklerin gönderildiği çalışma alanını sağladığınız için abonelikler arasında çalışabilir.


## <a name="configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage"></a>Günlük Analizi çalışma alanını depolama alanından Azure tanılamalarını toplamak için yapılandırma
Klasik bir bulut hizmetinin veya hizmet dokusu kümesinin çalışan bir örneğinden günlük verileri toplamak için önce verileri Azure depolamasına yazmanız gerekir. Daha sonra bir Log Analytics çalışma alanı, günlükleri depolama hesabından toplamak üzere yapılandırılır. Desteklenen kaynaklar şunlardır:

* Klasik bulut hizmetleri (web ve çalışan rolleri)
* Servis kumaş kümeleri

Aşağıdaki örnek, nasıl yapılacağını gösterir:

1. Çalışma alanının verileri dizine ekeceği varolan depolama hesaplarını ve konumlarını listele
2. Depolama hesabından okumak için yapılandırma oluşturma
3. Ek konumlardaki verileri dizinlemek için yeni oluşturulan yapılandırmayı güncelleştirme
4. Yeni oluşturulan yapılandırmayı silme

```powershell
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable"
$workspace = (Get-AzOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want the workspace to index
$storageId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight"

```

Farklı aboneliklerde depolama hesaplarından günlükleri toplamak için önceki komut dosyasını da kullanabilirsiniz. Depolama hesabı kaynak kimliği ve ilgili erişim anahtarı sağladığınız için komut dosyası abonelikler arasında çalışabilir. Erişim anahtarını değiştirdiğinizde, yeni anahtara sahip olmak için depolama içgörüsünü güncelleştirmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics'in yapılandırması için PowerShell'i kullanma hakkında ek bilgi için [Log Analytics PowerShell cmdletlerini inceleyin.](https://docs.microsoft.com/powershell/module/az.operationalinsights/)

