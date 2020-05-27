---
title: Azure Otomasyonu iş verilerini Azure İzleyici günlüklerine iletme
description: Bu makalede, Azure Izleyici günlüklerine iş durumu ve runbook iş akışlarının nasıl gönderileceği açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 05/22/2020
ms.topic: conceptual
ms.openlocfilehash: ba498fe9f70664a801172a6ff3705ac41a6371ef
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835265"
---
# <a name="forward-azure-automation-job-data-to-azure-monitor-logs"></a>Azure Otomasyonu iş verilerini Azure İzleyici günlüklerine iletme

Azure Otomasyonu, Log Analytics çalışma alanınıza runbook iş durumu ve iş akışları gönderebilir. Bu işlem, çalışma alanı bağlamayı içermez ve tamamen bağımsızdır. İş günlükleri ve iş akışları, bireysel işler için Azure portal veya PowerShell ile görünür ve bu, basit araştırmalar gerçekleştirmenize olanak tanır. Artık Azure Izleyici günlükleri ile şunları yapabilirsiniz:

* Otomasyon işlerinizin durumuyla ilgili Öngörüler elde edin.
* Runbook iş durumunuz (örneğin, başarısız veya askıya alındı) temelinde bir e-posta veya uyarı tetikleyin.
* İş akışlarınız genelinde gelişmiş sorgular yazın.
* Otomasyon hesaplarında işlerin ilişkilendirilmesi.
* Runbook sonuçlarınızı, runbook iş durumunu ve diğer ilgili anahtar göstergelerini veya ölçümlerini görselleştirmek için özel görünümleri ve arama sorgularını kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-deployment-considerations"></a>Önkoşullar ve dağıtım konuları

Otomasyon günlüklerinizi Azure Izleyici günlüklerine göndermeye başlamak için şunlar gerekir:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)en son sürümü.
* Log Analytics çalışma alanı. Daha fazla bilgi için bkz. [Azure izleyici günlüklerini kullanmaya başlama](../log-analytics/log-analytics-get-started.md).
* Azure Otomasyonu hesabınızın kaynak KIMLIĞI.

Azure Otomasyonu hesabınızın kaynak KIMLIĞINI bulmak için aşağıdaki komutu kullanın:

```powershell-interactive
# Find the ResourceId for the Automation account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Log Analytics çalışma alanınızın kaynak KIMLIĞINI bulmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Yukarıdaki komutların çıktısında birden fazla otomasyon hesabınız veya çalışma alanınız varsa, aşağıdaki işlemi gerçekleştirerek Otomasyon hesabınızın tam kaynak KIMLIĞININ bir parçası olan adı ve diğer ilgili özellikleri bulabilirsiniz:

1. Azure portal **Otomasyon hesapları** sayfasından Otomasyon hesabınızı seçin. 
2. Seçilen Otomasyon hesabının sayfasında, **Hesap ayarları**altında **Özellikler**' i seçin.  
3. **Özellikler** sayfasında aşağıda gösterilen ayrıntıları göz önünde bulabilirsiniz.

    ![Otomasyon hesabı özellikleri](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="azure-monitor-log-records"></a>Azure Izleyici günlük kayıtları

Azure Otomasyonu tanılama, Azure Izleyici günlüklerinde olarak etiketlenen iki tür kayıt oluşturur `AzureDiagnostics` . Sonraki bölümlerdeki tablolar, Azure Otomasyonu tarafından oluşturulan kayıt örnekleri ve günlük araması sonuçlarında görünen veri türleridir.

### <a name="job-logs"></a>İş günlükleri

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatan arayan. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Çağıran için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işini tanımlayan GUID. |
| 'I |Runbook işinin durumudur. Olası değerler şunlardır:<br>-Yeni<br>-Oluşturuldu<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>-Tamamlandı |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır. |
| ThrottledRequests | Azure 'da gerçekleştirilen işlem türü. Otomasyon için değer Iş olur. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Azure Izleyici günlüklerinin verileri toplamak için kullandığı sistem. Azure Tanılama için değer her zaman Azure ' dır. |
| ResultDescription |Runbook işi sonuç durumu. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin bağıntı GUID 'SI. |
| ResourceId |Runbook 'un Azure Otomasyonu hesabı kaynak KIMLIĞI. |
| SubscriptionId | Otomasyon hesabı için Azure aboneliği GUID 'ı. |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Kaynak sağlayıcısı. Değer MICROSOFT. Otomasyon. |
| ResourceType | Kaynak türü. Değer AUTOMATIONACCOUNTS değeridir. |

### <a name="job-streams"></a>İş akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatan arayan. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Çağıran için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işini tanımlayan GUID. |
| 'I |Runbook işinin durumudur. Olası değerler şunlardır:<br>-Devam ediyor |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır. |
| ThrottledRequests | Azure 'da gerçekleştirilen işlem türü. Otomasyon için değer Iş olur. |
| Kaynak | Otomasyon hesabının adı. |
| SourceSystem | Azure Izleyici günlüklerinin verileri toplamak için kullandığı sistem. Azure Tanılama için değer her zaman Azure ' dır. |
| ResultDescription |Runbook 'tan çıkış akışını içeren açıklama. |
| CorrelationId |Runbook işinin bağıntı GUID 'SI. |
| ResourceId |Runbook 'un Azure Otomasyonu hesabı kaynak KIMLIĞI. |
| SubscriptionId | Otomasyon hesabı için Azure aboneliği GUID 'ı. |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Kaynak sağlayıcısı. Değer MICROSOFT. Otomasyon. |
| ResourceType | Kaynak türü. Değer AUTOMATIONACCOUNTS değeridir. |

## <a name="set-up-integration-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile tümleştirmeyi ayarlama

1. Bilgisayarınızda, **Başlangıç** ekranından Windows PowerShell ' i başlatın.
2. Aşağıdaki PowerShell komutlarını çalıştırın, ve değerlerini `$automationAccountId` `$workspaceId` önceki bölümdeki değerlerle düzenleyin.

   ```powershell-interactive
   $workspaceId = "resource ID of the log analytics workspace"
   $automationAccountId = "resource ID of your Automation account"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Bu betiği çalıştırdıktan sonra, yeni veya yazılan Azure Izleyici günlüklerinde kayıtları görmek için başlamadan önce bir saat sürebilir `JobLogs` `JobStreams` .

Günlükleri görmek için Log Analytics günlük aramasında aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı Doğrula

Otomasyon hesabınızın Log Analytics çalışma alanınıza günlük gönderdiğini doğrulamak için, aşağıdaki PowerShell komutu kullanılarak tanılamaların Otomasyon hesabında doğru şekilde yapılandırılıp yapılandırılmadığını denetleyin.

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda şunları doğrulayın:

* Altında `Logs` , değeri true 'dur `Enabled` .
* `WorkspaceId`, `ResourceId` Log Analytics çalışma alanınız için değere ayarlanır.

## <a name="view-automation-logs-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Otomasyon günlüklerini görüntüleme

Artık Otomasyon iş günlüklerinizi Azure Izleyici günlüklerine göndermeye başladığınıza göre, Azure Izleyici günlükleri içinde bu günlüklerle neler yapabileceğinizi görelim.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Bir runbook işi başarısız olduğunda veya askıya aldığında e-posta gönder

Aşağıdaki adımlarda, bir runbook işiyle ilgili bir sorun olduğunda sizi bilgilendirmek üzere Azure Izleyici 'de uyarıların nasıl ayarlanacağı gösterilmektedir.

Uyarı kuralı oluşturmak için, uyarıyı çağırması gereken runbook iş kayıtları için bir günlük araması oluşturarak başlayın. Uyarı kuralını oluşturmak ve yapılandırmak için **Uyarı** düğmesine tıklayın.

1. Log Analytics çalışma alanına genel bakış sayfasında **günlükleri görüntüle**' ye tıklayın.

2. Sorgu alanına aşağıdaki aramayı yazarak Uyarınız için bir günlük arama sorgusu oluşturun:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")`<br><br>Ayrıca, kullanarak runbook adına göre gruplandırabilirsiniz:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Çalışma alanınıza birden fazla otomasyon hesabından veya aboneliğine ait Günlükler ayarlarsanız, uyarılarınızı abonelik ve otomasyon hesabına göre gruplandırabilirsiniz. Otomasyon hesabı adı `Resource` , arama içindeki alanda bulunabilir `JobLogs` .

3. **Kural oluştur** ekranını açmak için sayfanın üst kısmındaki **Yeni uyarı kuralı** ' na tıklayın. Uyarıyı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [Azure 'Da günlük uyarıları](../azure-monitor/platform/alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlanan tüm işleri bul

Hatalarda uyarı verme işleminin yanı sıra, bir runbook işinin Sonlandırılmamış bir hata olduğunu fark edebilirsiniz. Bu durumlarda, PowerShell bir hata akışı üretir, ancak Sonlandırılmamış hatalar işinizin askıya alınmasına veya başarısız olmasına neden olmaz.

1. Log Analytics çalışma alanınızda **Günlükler**' e tıklayın.

2. Sorgu alanına, yazın `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` .

3. **Ara** düğmesine tıklayın.

### <a name="view-job-streams-for-a-job"></a>İş akışlarını görüntüleme

Bir iş hata ayıklaması yaparken iş akışlarına da bakmak isteyebilirsiniz. Aşağıdaki sorgu, GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0 ile tek bir işin tüm akışlarını gösterir:

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş iş durumunu görüntüle

Son olarak, zaman içinde iş geçmişinizi görselleştirmek isteyebilirsiniz. Bu sorguyu, zaman içinde işlerinizin durumunu aramak için kullanabilirsiniz.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`
<br> ![Log Analytics geçmiş Iş durumu grafiği](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Tanılama ayarlarını kaldır

Otomasyon hesabından tanılama ayarını kaldırmak için aşağıdaki komutu çalıştırın:

```powershell-interactive
$automationAccountId = "[resource ID of your Automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="next-steps"></a>Sonraki adımlar

* Arama sorguları oluşturma ve Azure Izleyici günlükleriyle Otomasyon iş günlüklerini İnceleme hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde günlük aramaları](../log-analytics/log-analytics-log-searches.md).
* Runbook 'lardan çıkış ve hata iletilerinin oluşturulmasını ve alınmasını anlamak için bkz. [runbook çıkışını izleme](automation-runbook-output-and-messages.md).
* Runbook yürütmesi, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'Nda runbook yürütme](automation-runbook-execution.md).
* Azure Izleyici günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için bkz. Azure [izleyici günlüklerine Azure depolama verilerini toplama genel bakış](../azure-monitor/platform/collect-azure-metrics-logs.md).
* Log Analytics sorun giderme konusunda yardım için bkz. [Log Analytics artık veri toplamamasının neden olduğu sorun giderme](../azure-monitor/platform/manage-cost-storage.md#troubleshooting-why-log-analytics-is-no-longer-collecting-data).