---
title: Azure Otomasyonu iş verilerini Azure İzleyici günlüklerine iletme
description: Bu makalede, ek öngörü ve yönetim sunmak için Iş durumu ve runbook iş akışlarının Azure Monitor günlüklerine nasıl gönderilen gösterilmektedir.
services: automation
ms.subservice: process-automation
ms.date: 02/05/2019
ms.topic: conceptual
ms.openlocfilehash: beb69edc57b5a13db0f6d2e5e1536804f3472aff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75421910"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-azure-monitor-logs"></a>Otomasyondan Azure Monitör günlüklerine iş durumu ve iş akışları iletme

Otomasyon, runbook iş durumunu ve iş akışlarını Log Analytics çalışma alanınıza gönderebilir. Bu işlem çalışma alanı bağlama içermez ve tamamen bağımsızdır. İş günlükleri ve iş akışları Azure portalında veya PowerShell ile tek tek işler için görünür ve bu da basit incelemeler gerçekleştirmenize olanak tanır. Azure Monitor günlükleri ile artık şunları yapabilirsiniz:

* Otomasyon işleriniz hakkında bilgi edinin.
* Runbook iş durumunuza (örneğin, başarısız veya askıya alınmış) bağlı olarak bir e-posta veya uyarı tetikle.
* İş akışlarınızda gelişmiş sorgular yazın.
* Otomasyon hesapları daki işleri ilişkilendirin.
* Zaman içinde iş geçmişinizi görselleştirin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-deployment-considerations"></a>Ön koşullar ve dağıtım konuları

Otomasyon günlüklerinizi Azure Monitor günlüklerine göndermeye başlamak için şunları yapmanız gerekir:

* [Azure PowerShell'in](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)en son sürümü.
* Log Analytics çalışma alanı. Daha fazla bilgi için Azure [Monitor günlüklerini başlatın'](../log-analytics/log-analytics-get-started.md)a bakın.
* Azure Otomasyon hesabınız için ResourceId.

Azure Otomasyon hesabınız için ResourceId'yi bulmak için:

```powershell-interactive
# Find the ResourceId for the Automation Account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Log Analytics çalışma alanınız için ResourceId'yi bulmak için aşağıdaki PowerShell'i çalıştırın:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Önceki komutların çıktısında birden fazla Otomasyon hesabınız veya çalışma alanLarınız varsa, *Kaynak Kimliği'nin*değerini yapılandırmanız ve kopyalamanız gereken *Adı* bulun.

Otomasyon hesabınızın *Adını* bulmanız gerekiyorsa, Azure portalında **Otomasyon hesabınızı Otomasyon hesap** bıçağından seçin ve Tüm **ayarları**seçin. **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br> ![Otomasyon Hesabı](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png)özellikleri .

## <a name="set-up-integration-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle tümleştirme ayarlama

1. Bilgisayarınızda, **Başlangıç** ekranından **Windows PowerShell'i** başlatın.
2. Aşağıdaki PowerShell'i çalıştırın ve değeri `[your resource id]` `[resource id of the log analytics workspace]` önceki adımdaki değerlerle birlikte edin.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Bu komut dosyasını çalıştırdıktan sonra, Azure Monitor günlüklerinde yeni İş Günlükleri veya İş Akışlarının yazıldığını görmeye başlamanız bir saat sürebilir.

Günlükleri görmek için, günlük analizi günlük aramasında aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Otomasyon hesabınızın Log Analytics çalışma alanınıza günlük ler gönderdiğini doğrulamak için, aşağıdaki PowerShell'i kullanarak Tanılamanın Otomasyon hesabında doğru şekilde yapılandırıldığından kontrol edin:

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda aşağıdakileri sağlamak:

* *Günlükler*altında, *Etkin'in* değeri *Doğrudur.*
* *WorkspaceId* değeri, Log Analytics çalışma alanınızın ResourceId'ine ayarlanır.

## <a name="azure-monitor-log-records"></a>Azure Monitör günlük kayıtları

Azure Otomasyonu'ndan gelen tanılama, Azure Monitor günlüklerinde iki tür kayıt oluşturur ve **Azure Diagnostics**olarak etiketlenir. Aşağıdaki sorgular, Azure Monitor günlüklerine yükseltilen sorgu dilini kullanır. Eski sorgu dili ile yeni Azure Kusto sorgu dili arasındaki yaygın sorgular hakkında bilgi için [Legacy'yi yeni Azure Kusto Sorgu Dili hile sayfasına](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language) ziyaret edin

### <a name="job-logs"></a>İş Günlükleri

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| Sonuç Türü |Runbook işinin durumudur. Olası değerler şunlardır:<br>- Yeni<br>- Oluşturuldu<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Tamamlanmış |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır. |
| ThrottledRequests | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Azure Monitor günlüklerinin verileri nasıl topladığı. Azure tanılama için her zaman *Azure* tanılama. |
| Sonuç Açıklaması |Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesap kaynak kimliğini belirtir. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Microsoft. Otomasyon |
| ResourceType | OTOMASYON HESAPLARI |


### <a name="job-streams"></a>İş Akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| Sonuç Türü |Runbook işinin durumudur. Olası değerler şunlardır:<br>- Devam Ediyor |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır. |
| ThrottledRequests | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Azure Monitor günlüklerinin verileri nasıl topladığı. Azure tanılama için her zaman *Azure* tanılama. |
| Sonuç Açıklaması |Runbook’un çıktı akışını içerir. |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesap kaynak kimliğini belirtir. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Microsoft. Otomasyon |
| ResourceType | OTOMASYON HESAPLARI |

## <a name="viewing-automation-logs-in-azure-monitor-logs"></a>Azure Monitor günlüklerinde Otomasyon Günlüklerini görüntüleme

Artık Otomasyon iş günlüklerinizi Azure Monitor günlüklerine göndermeye başladığınızda, Azure Monitor günlüklerinde bu günlüklerle neler yapabileceğinizi görelim.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Runbook işi başarısız olduğunda veya askıya aldığında e-posta gönderme
En iyi müşteri sorar bir şey runbook iş yanlış gittiğinde bir e-posta veya metin göndermek için yeteneğidir.

Bir uyarı kuralı oluşturmak için, uyarıyı çağırması gereken runbook iş kayıtları için günlük araması oluşturarak başlarsınız. Uyarı kuralını oluşturmak ve yapılandırmak için **Uyarı** düğmesini tıklatın.

1. Günlük Analizi çalışma alanına Genel Bakış sayfasından **Günlükleri Görüntüle'yi**tıklatın.
2. Sorgu alanına aşağıdaki aramayı yazarak uyarınız için bir `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` günlük arama sorgusu oluşturun: RunbookName'ye göre aşağıdakileri kullanarak da gruplayabilirsiniz:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Birden fazla Otomasyon hesabından veya aboneliğinizden çalışma alanınıza günlükler ayarlarsanız, uyarılarınızı abonelik ve Otomasyon hesabına göre gruplayabilirsiniz. Otomasyon hesap adı, İş Günlükleri'nde Kaynak alanında bulunabilir.
3. **Oluştur kuralı** ekranını açmak için sayfanın üst kısmında + Yeni **Uyarı Kuralı'nı** tıklatın. Uyarıyı yapılandırma seçenekleri hakkında daha fazla bilgi için [Azure'daki Günlük uyarılarına](../azure-monitor/platform/alerts-unified-log.md)bakın.

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlanmış tüm işleri bulma
Hatalar konusunda uyarıya ek olarak, bir runbook işinin sonlandırma hatası na sahip olduğunu da bulabilirsiniz. Bu gibi durumlarda PowerShell bir hata akışı üretir, ancak sonlandırma hatası olmayan hatalar işinizin askıya alınmasına veya başarısız olmasına neden olmaz.

1. Günlük Analizi çalışma alanınızda **Günlükler'i**tıklatın.
2. Sorgu alanına, yazın `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` ve ardından **Arama** düğmesini tıklatın.

### <a name="view-job-streams-for-a-job"></a>Bir iş için iş akışlarını görüntüleme
Bir işi hata ayıklarken, iş akışlarına da bakmak isteyebilirsiniz. Aşağıdaki sorgu GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0 ile tek bir iş için tüm akışları gösterir:

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş iş durumunu görüntüleme
Son olarak, zaman içinde iş geçmişinizi görselleştirmek isteyebilirsiniz. Bu sorguyu, zaman içinde işlerinizin durumunu aramak için kullanabilirsiniz.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`
<br> ![Log Analytics Geçmiş İş Durum Tablosu](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Tanılama ayarlarını kaldırma

Otomasyon Hesabından tanılama ayarını kaldırmak için aşağıdaki komutları çalıştırın:

```powershell-interactive
$automationAccountId = "[resource id of your automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="summary"></a>Özet

Otomasyon iş durumunuzu ve verilerinizi Azure Monitor günlüklerine aktararak, Otomasyon işlerinizin durumu hakkında daha iyi bilgi edinebilirsiniz:
+ Bir sorun olduğunda sizi bilgilendirmek için uyarılar ayarlama.
+ Runbook sonuçlarınızı, runbook iş durumunuzu ve diğer ilgili önemli göstergeleri veya ölçümleri görselleştirmek için özel görünümleri ve arama sorgularını kullanma.

Azure Monitor günlükleri, Otomasyon işlerinizde daha fazla operasyonel görünürlük sağlar ve olayları daha hızlı ele almanıza yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Günlük Analizi sorununa yardımcı olmak için, [Günlük Analitiği'nin neden artık veri topladığını](../azure-monitor/platform/manage-cost-storage.md#troubleshooting-why-log-analytics-is-no-longer-collecting-data)görme sorununa bakın.
* Azure Monitor günlükleriyle farklı arama sorguları oluşturma ve Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için [Azure Monitor günlüklerinde Günlük aramaları'na](../log-analytics/log-analytics-log-searches.md)bakın.
* Runbook'lardan çıktı ve hata iletilerinin nasıl oluşturulup alınıp alınabildiğini anlamak için [runbook çıktısı ve iletilerine](automation-runbook-output-and-messages.md)bakın.
* Runbook yürütme, runbook işleri ve diğer teknik ayrıntıları izlemek hakkında daha fazla bilgi edinmek için [bkz.](automation-runbook-execution.md)
* Azure Monitor günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için [bkz.](../azure-monitor/platform/collect-azure-metrics-logs.md)

