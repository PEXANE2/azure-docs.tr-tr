---
title: Azure Otomasyonu iş verilerini Azure İzleyici günlüklerine iletme
description: Bu makalede, ek öngörü ve yönetim sunmak için Iş durumu ve runbook iş akışlarının Azure Monitor günlüklerine nasıl gönderilen gösterilmektedir.
services: automation
ms.subservice: process-automation
ms.date: 02/05/2019
ms.topic: conceptual
ms.openlocfilehash: a9f4e641e60d6cf1c481c445767422e8b4df683b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457697"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-azure-monitor-logs"></a>Otomasyondan Azure Monitör günlüklerine iş durumu ve iş akışları iletme

Otomasyon, runbook iş durumunu ve iş akışlarını Log Analytics çalışma alanınıza gönderebilir. Bu işlem çalışma alanı bağlama içermez ve tamamen bağımsızdır. İş günlükleri ve iş akışları Azure portalında veya PowerShell ile tek tek işler için görünür ve bu da basit incelemeler gerçekleştirmenize olanak tanır. Azure Monitor günlükleri ile artık şunları yapabilirsiniz:

* Otomasyon işlerinizin durumu hakkında bilgi edinin.
* Runbook iş durumunuza (örneğin, başarısız veya askıya alınmış) bağlı olarak bir e-posta veya uyarı tetikle.
* İş akışlarınızda gelişmiş sorgular yazın.
* Otomasyon hesapları daki işleri ilişkilendirin.
* Runbook sonuçlarınızı, runbook iş durumunuzu ve diğer ilgili önemli göstergeleri veya ölçümleri görselleştirmek için özel görünümler ve arama sorguları kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites-and-deployment-considerations"></a>Ön koşullar ve dağıtım konuları

Otomasyon günlüklerinizi Azure Monitor günlüklerine göndermeye başlamak için şunları yapmanız gerekir:

* [Azure PowerShell'in](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)en son sürümü.
* Log Analytics çalışma alanı. Daha fazla bilgi için Azure [Monitor günlüklerini başlatın'](../log-analytics/log-analytics-get-started.md)a bakın.
* Azure Otomasyon hesabınız için kaynak kimliği.

Azure Otomasyon hesabınız için kaynak kimliğini bulmak için aşağıdaki komutu kullanın:

```powershell-interactive
# Find the ResourceId for the Automation account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Log Analytics çalışma alanınız için kaynak kimliğini bulmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Önceki komutların çıktısında birden fazla Otomasyon hesabınız veya çalışma alanınız varsa, kaynak kimliği için değeri yapılandırmanız ve kopyalamanız gereken adı bulun.

1. Azure portalında, **Otomasyon hesap** bıçağından Otomasyon hesabınızı seçin ve **Tüm ayarları**seçin. 
2. Tüm **ayarlar** bıçağından, **Hesap Ayarları**altında **Özellikler'i**seçin.  
3. **Özellikler** bıçağında, aşağıda gösterilen özelliklere dikkat edin.

    ![Otomasyon hesabı özellikleri](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="azure-monitor-log-records"></a>Azure Monitör günlük kayıtları

Azure Otomasyontanılama, Azure Monitor günlüklerinde iki tür `AzureDiagnostics`kayıt oluşturur, ". Sonraki bölümlerdeki tablolar, Azure Otomasyonu'nun oluşturduğu kayıtlara ve günlük arama sonuçlarında görünen veri türlerine örnektir.

### <a name="job-logs"></a>İş günlükleri

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatan arayan. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işini tanımlayan GUID. |
| Sonuç Türü |Runbook işinin durumudur. Olası değerler şunlardır:<br>- Yeni<br>- Oluşturuldu<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Tamamlanmış |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır. |
| ThrottledRequests | Azure'da gerçekleştirilen işlem türü. Otomasyon için değer İş'tir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Azure Monitor günlüklerinin verileri toplamak için kullandığı sistem. Azure tanılama değeri her zaman Azure tanılama için Azure'dur. |
| Sonuç Açıklaması |Runbook iş sonuç durumu. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin korelasyon GUID'i. |
| ResourceId |Çalışma kitabının Azure Otomasyon hesap kaynak kimliği. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik GUID'i. |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Kaynak sağlayıcısı. Değeri MICROSOFT'dur. Otomasyon. |
| ResourceType | Kaynak türü. Değeri AUTOMATIONACCOUNTS olduğunu. |

### <a name="job-streams"></a>İş akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatan arayan. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Arayan için kiracıyı tanımlayan GUID. |
| JobId_g |Runbook işini tanımlayan GUID. |
| Sonuç Türü |Runbook işinin durumudur. Olası değerler şunlardır:<br>- Devam Ediyor |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır. |
| ThrottledRequests | Azure'da gerçekleştirilen işlem türü. Otomasyon için değer İş'tir. |
| Kaynak | Otomasyon hesabının adı. |
| SourceSystem | Azure Monitor günlüklerinin verileri toplamak için kullandığı sistem. Azure tanılama değeri her zaman Azure tanılama için Azure'dur. |
| Sonuç Açıklaması |Runbook'taki çıktı akışını içeren açıklama. |
| CorrelationId |Runbook işinin korelasyon GUID'i. |
| ResourceId |Çalışma kitabının Azure Otomasyon hesap kaynak kimliği. |
| SubscriptionId | Otomasyon hesabı için Azure abonelik GUID'i. |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | Kaynak sağlayıcısı. Değeri MICROSOFT'dur. Otomasyon. |
| ResourceType | Kaynak türü. Değeri AUTOMATIONACCOUNTS olduğunu. |

## <a name="setting-up-integration-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle tümleştirme ayarlama

1. Bilgisayarınızda, **Başlangıç** ekranından Windows PowerShell'i başlatın.
2. Aşağıdaki PowerShell komutlarını çalıştırın ve değerleri `[your resource ID]` `[resource ID of the log analytics workspace]` önceki bölümdeki değerlerle ve bu değerlerle birlikte edin.

   ```powershell-interactive
   $workspaceId = "[resource ID of the log analytics workspace]"
   $automationAccountId = "[resource ID of your Automation account]"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Bu komut dosyasını çalıştırdıktan sonra, Azure Monitor günlüklerinde yeni `JobLogs` veya `JobStreams` yazılı kayıtları görmeye başlamanız bir saat sürebilir.

Günlükleri görmek için, günlük analizi günlük aramasında aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Otomasyon hesabınızın Log Analytics çalışma alanınıza günlük ler gönderdiğini doğrulamak için, aşağıdaki PowerShell komutunu kullanarak otomasyon hesabında tanılamanın doğru şekilde yapılandırıldığından kontrol edin.

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda, aşağıdakileri

* Altında, `Logs`değeri `Enabled` True'dur.
* `WorkspaceId`Log Analytics `ResourceId` çalışma alanınız için değere ayarlanır.

## <a name="viewing-automation-logs-in-azure-monitor-logs"></a>Azure Monitor günlüklerinde Otomasyon Günlüklerini görüntüleme

Artık Otomasyon iş günlüklerinizi Azure Monitor günlüklerine göndermeye başladığınızda, Azure Monitor günlüklerinde bu günlüklerle neler yapabileceğinizi görelim.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Runbook işi başarısız olduğunda veya askıya aldığında e-posta gönderme

En iyi müşteri sorar bir şey runbook iş yanlış gittiğinde bir e-posta veya metin göndermek için yeteneğidir.

Bir uyarı kuralı oluşturmak için, uyarıyı çağırması gereken runbook iş kayıtları için günlük araması oluşturarak başlayın. Uyarı kuralını oluşturmak ve yapılandırmak için **Uyarı** düğmesini tıklatın.

1. Günlük Analizi çalışma alanına Genel Bakış sayfasından **Günlükleri Görüntüle'yi**tıklatın.
2. Sorgu alanına aşağıdaki aramayı yazarak uyarınız için bir günlük arama sorgusu oluşturun:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")`<br><br>Ayrıca runbook adına göre aşağıdakileri kullanarak gruplatabilirsiniz:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Birden fazla Otomasyon hesabından veya aboneliğinizden çalışma alanınıza günlükler ayarlarsanız, uyarılarınızı abonelik ve Otomasyon hesabına göre gruplayabilirsiniz. Otomasyon hesap adı `Resource` arama alanında `JobLogs`bulunabilir.
3. **Oluştur kuralı** ekranını açmak için sayfanın üst kısmında Yeni **Uyarı Kuralı'nı** tıklatın. Uyarıyı yapılandırma seçenekleri hakkında daha fazla bilgi için [Azure'daki Günlük uyarılarına](../azure-monitor/platform/alerts-unified-log.md)bakın.

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlanmış tüm işleri bulma

Hatalar konusunda uyarıya ek olarak, bir runbook işinin sonlandırma hatası na sahip olduğunu da bulabilirsiniz. Bu gibi durumlarda, PowerShell bir hata akışı üretir, ancak sonlandırma hatası olmayan hatalar işinizin askıya alınmasına veya başarısız olmasına neden olmaz.

1. Günlük Analizi çalışma alanınızda **Günlükler'i**tıklatın.
2. Sorgu alanına, yazın. `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g`
3. **Ara** düğmesini tıklatın.

### <a name="view-job-streams-for-a-job"></a>Bir iş için iş akışlarını görüntüleme

Bir işi hata ayıklarken, iş akışlarına da bakmak isteyebilirsiniz. Aşağıdaki sorgu GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0 ile tek bir iş için tüm akışları gösterir:

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş iş durumunu görüntüleme

Son olarak, zaman içinde iş geçmişinizi görselleştirmek isteyebilirsiniz. Bu sorguyu, zaman içinde işlerinizin durumunu aramak için kullanabilirsiniz.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`
<br> ![Log Analytics Geçmiş İş Durum Tablosu](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="removing-diagnostic-settings"></a>Tanılama ayarlarını kaldırma

Tanılama ayarını Otomasyon hesabından kaldırmak için aşağıdaki komutu çalıştırın:

```powershell-interactive
$automationAccountId = "[resource ID of your Automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```
## <a name="next-steps"></a>Sonraki adımlar

* Günlük Analizi sorununa yardımcı olmak için, [Günlük Analitiği'nin neden artık veri topladığını](../azure-monitor/platform/manage-cost-storage.md#troubleshooting-why-log-analytics-is-no-longer-collecting-data)görme sorununa bakın.
* Azure Monitor günlükleriyle farklı arama sorguları oluşturma ve Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için [Azure Monitor günlüklerinde Günlük aramaları'na](../log-analytics/log-analytics-log-searches.md)bakın.
* Runbook'lardan çıktı ve hata iletilerinin nasıl oluşturulup alınıp alınabildiğini anlamak için [runbook çıktısı ve iletilerine](automation-runbook-output-and-messages.md)bakın.
* Runbook yürütme, runbook işleri ve diğer teknik ayrıntıları izlemek hakkında daha fazla bilgi edinmek için [bkz.](automation-runbook-execution.md)
* Azure Monitor günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için [bkz.](../azure-monitor/platform/collect-azure-metrics-logs.md)