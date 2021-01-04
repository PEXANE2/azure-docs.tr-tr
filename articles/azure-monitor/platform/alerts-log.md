---
title: Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme | Microsoft Docs
description: Azure Izleyici 'yi kullanarak günlük uyarı kuralları oluşturma, görüntüleme ve yönetme
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 07/29/2019
ms.subservice: alerts
ms.openlocfilehash: a8dbadd3af9dc21ced54af151e4de705f854e011
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696062"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme

## <a name="overview"></a>Genel Bakış

Günlük uyarıları, kullanıcıların kaynak günlüklerini her ayarlama sıklığını değerlendirmek ve sonuçlara göre bir uyarı tetiklemesi için [Log Analytics](../log-query/log-analytics-tutorial.md) bir sorgu kullanmasına izin verir. Kurallar, [eylem gruplarını](./action-groups.md)kullanarak bir veya daha fazla eylemi tetikleyebilir. [Günlük uyarılarının işlevselliği ve terminolojisi hakkında daha fazla bilgi edinin](alerts-unified-log.md).

Bu makalede, Azure Izleyici kullanarak günlük uyarılarını oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir. Uyarı kuralları üç bileşen tarafından tanımlanır:
- Hedef: izlenecek belirli bir Azure kaynağı.
- Ölçüt: değerlendirilecek mantık. Karşılanırsa, uyarı ateşlenir.  
- Eylem: bildirimler veya Otomasyon-e-posta, SMS, Web kancası vb.

Ayrıca, [ayrı bir makalede](alerts-log-create-templates.md)açıklanan Azure Resource Manager şablonlarını kullanarak günlük uyarı kuralları oluşturabilirsiniz.

> [!NOTE]
> [Log Analytics çalışma alanındaki](../log-query/log-analytics-tutorial.md) günlük verileri Azure izleyici ölçüm deposuna gönderilebilir. Ölçüm uyarıları [farklı davranışa](alerts-metric-overview.md)sahiptir ve bu işlem, çalıştığınız verilere bağlı olarak daha çok tercih edilebilir. Günlükleri ölçümlere yönlendirme hakkında bilgi için bkz. [Günlükler Için ölçüm uyarısı](alerts-metric-logs.md).

## <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Azure portal bir günlük uyarı kuralı oluşturun

Uyarılar için sorgular yazmaya başlama adımları burada verilmiştir:

1. Uyarı vermek istediğiniz kaynağa gidin.
1. **İzleyici** altında **Günlükler**' i seçin.
1. Soruna işaret edebilecek günlük verilerini sorgulayın. [Kendi sorgunuzu yazmaya](../log-query/log-analytics-tutorial.md)ne bulabileceğinizi veya başladığınızı anlamak için [Uyarı sorgusu örnekleri konusunu](../log-query/example-queries.md) kullanabilirsiniz. [İyileştirilmiş uyarı sorguları oluşturmayı öğrenin](alerts-log-query.md).
1. Uyarı oluşturma akışını başlatmak için ' + yeni uyarı kuralı ' düğmesine basın.

    ![Log Analytics-uyarı ayarla](media/alerts-log/AlertsAnalyticsCreate.png)

> [!NOTE]
> Bir kaynak grubu veya abonelik kapsamı kullanarak birden çok kaynakta çalıştırılan Günlükler için kaynak erişim modunu kullanırken, uyarılar üzerinde uyarı oluşturmanız önerilir. Ölçekte uyarı verme kural Yönetimi ek yükünü azaltır. Kaynakları hedefleyebilir olması için lütfen sonuçlarda kaynak KIMLIĞI sütununu ekleyin. [Uyarıları boyutlara göre bölme hakkında daha fazla bilgi edinin](alerts-unified-log.md#split-by-alert-dimensions).

### <a name="log-alert-for-log-analytics-and-application-insights"></a>Log Analytics ve Application Insights için günlük uyarısı

1. Sorgu söz dizimi doğruysa, sorgunun geçmiş verileri, son altı saatten geçen haftaya kadar grafik dönemini ince ayar seçeneği ile bir grafik olarak görüntülenir.
 
    Sorgu sonuçlarınız zaman sütunu olmayan özetlenmiş veriler veya [projeye](/azure/kusto/query/projectoperator) özel sütunlar içeriyorsa, grafik tek bir değer gösterir.

    ![Uyarı kuralını Yapılandır](media/alerts-log/AlertsPreviewAlertLog.png)

1. Belirtilen koşulun değerlendirecek zaman aralığını, [**period**](alerts-unified-log.md#query-time-range) seçeneğini kullanarak seçin.

1. Günlük uyarıları, iki [**Ölçü**](alerts-unified-log.md#measure)türüne göre olabilir:
    1. **Sonuç sayısı** -sorgu tarafından döndürülen kayıt sayısı.
    1. **Ölçüm ölçümü**  -  Seçilen ifade ve [bin ()](/azure/kusto/query/binfunction) seçimine göre gruplanmış özetleme kullanılarak hesaplanan *Toplam değer* . Örnek:

    ```Kusto
    // Reported errors
    union Event, Syslog // Event table stores Windows event records, Syslog stores Linux records
    | where EventLevelName == "Error" // EventLevelName is used in the Event (Windows) records
    or SeverityLevel== "err" // SeverityLevel is used in Syslog (Linux) records
    | summarize AggregatedValue = count() by Computer, bin(TimeGenerated, 15m)
    ```

1. Ölçüm ölçümleri uyarı mantığı için, isteğe bağlı olarak, **toplama** seçeneğini kullanarak [uyarıların boyutlara göre nasıl bölüneceği](alerts-unified-log.md#split-by-alert-dimensions) belirtebilirsiniz. Satır gruplandırma ifadesi benzersiz olmalı ve sıralanmalıdır.

    > [!NOTE]
    > As [bin ()](/azure/kusto/query/binfunction) , düzensiz zaman aralıklarına yol açabilir. Uyarı hizmeti, [bin ()](/azure/kusto/query/binfunction) işlevini otomatik olarak [bin_at ()](/azure/kusto/query/binatfunction) işlevine, çalışma zamanında, sabit bir nokta ile sonuçları sağlamak üzere dönüştürür.

    > [!NOTE]
    > Uyarı boyutlarına göre bölme yalnızca geçerli scheduledQueryRules API 'SI için kullanılabilir. Eski [Log Analytics uyarı API](api-alerts.md)'sini kullanırsanız, anahtarını değiştirmeniz gerekir. [Geçiş hakkında daha fazla bilgi edinin](./alerts-log-api-switch.md). Ölçekteki kaynak merkezli uyarı yalnızca API sürümünde `2020-05-01-preview` ve üzerinde desteklenir.

    ![toplama açık seçeneği](media/alerts-log/aggregate-on.png)

1. İleri, önizleme verilerine bağlı olarak [ **işleci**, **eşik değeri**](alerts-unified-log.md#threshold-and-operator)ve [**sıklığı**](alerts-unified-log.md#frequency)ayarlar.

1. Ayrıca, isteğe bağlı olarak, **Toplam veya ardışık ihlaller** kullanarak [bir uyarının tetiklenmesi için ihlallerin sayısını](alerts-unified-log.md#number-of-violations-to-trigger-alert) ayarlayabilirsiniz.

1. **Bitti**’yi seçin. 

1. **Uyarı kuralı adını**, **açıklamasını** tanımlayın ve uyarı **önem derecesini** seçin. Bu ayrıntılar tüm uyarı eylemlerinde kullanılır. Ayrıca, oluşturma **sırasında kuralı etkinleştir**' i seçerek, oluşturma sırasında uyarı kuralını etkinleştirememenizi de seçebilirsiniz.

1. Bir uyarı tetiklendikten sonra kural eylemlerini bir süre gizlemek istiyorsanız, [**uyarıları bastır**](alerts-unified-log.md#state-and-resolving-alerts) seçeneğini kullanın. Kural çalışmaya devam eder ve uyarı oluşturur, ancak gürültü engellemek için Eylemler tetiklenmeyecektir. Sessiz eylem değeri, uyarı sıklığından daha büyük olmalıdır.

    ![Günlük uyarıları uyarılarını gösterme](media/alerts-log/AlertsPreviewSuppress.png)

1. Uyarı koşulu karşılandığında uyarı kuralının bir veya daha fazla [**Eylem grubunu**](action-groups.md#webhook) tetiklemeniz gerekip gerekmediğini belirtin.

    > [!NOTE]
    > Gerçekleştirilebilecek eylemlerdeki sınırlamalar için [Azure abonelik hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md) bakın.  

1. Günlük uyarı kurallarında, isteğe bağlı olarak eylemleri özelleştirebilirsiniz:

    - **Özel e-posta konusu**: e-posta eylemlerinin *e-posta konusunu* geçersiz kılar Postanın gövdesini değiştiremezsiniz ve bu alan **e-posta adresleri için değildir**.
    - **Özel JSON yükünü dahil et**: eylem grubunun bir Web kancası eylemi içerdiğini varsayarak, eylem grupları tarafından kullanılan Web kancası JSON 'ı geçersiz kılar. [Günlük uyarıları için Web kancası eylemi](./alerts-log-webhook.md)hakkında daha fazla bilgi edinin.

    ![Günlük uyarıları için eylem geçersiz kılmaları](media/alerts-log/AlertsPreviewOverrideLog.png)

1. Tüm alanlar doğru ayarlandıysa, **Uyarı kuralı oluştur** düğmesine tıklanmış olabilir ve bir uyarı oluşturulur.

    Birkaç dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

    ![Kural oluşturma](media/alerts-log/AlertsPreviewCreate.png)

#### <a name="creating-log-alert-for-log-analytics-and-application-insights-from-the-alerts-management"></a>Uyarı yönetiminden Log Analytics ve Application Insights için günlük uyarısı oluşturma

> [!NOTE]
> Uyarı yönetiminden oluşturma işlemi şu anda kaynak merkezli günlüklerde desteklenmez

1. [Portalda](https://portal.azure.com/) **İzle** ' yi ve ardından **Uyarılar**' ı seçin.

    ![İzleme](media/alerts-log/AlertsPreviewMenu.png)

1. **Yeni uyarı kuralı**' nı seçin. 

    ![Uyarı Ekle](media/alerts-log/AlertsPreviewOption.png)

1. **Uyarı oluştur** bölmesi görüntülenir. Dört bölümden oluşur: 
    - Uyarının uygulandığı kaynak.
    - Denetlenecek koşul.
    - Koşul doğru ise gerçekleştirilecek eylemler.
    - Uyarının adı ve açıklaması hakkında ayrıntılar. 

    ![Kural Oluştur](media/alerts-log/AlertsPreviewAdd.png)

1. **Kaynak Seç** düğmesine basın. *Abonelik*, *kaynak türü* seçerek ve bir kaynak seçerek filtre uygulayın. Kaynağın kullanılabilir günlüklere sahip olduğundan emin olun.

   ![Kaynak seçin](media/alerts-log/Alert-SelectResourceLog.png)

1. Sonra, kaynak için kullanılabilen sinyal seçeneklerinin listesini görüntülemek için **koşul** Ekle düğmesini kullanın. **Özel günlük araması** seçeneğini belirleyin.

   ![Kaynak seçin-özel günlük araması](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > Uyarılar portalı Log Analytics ve Application Insights Kaydedilmiş sorguları listeler ve şablon uyarı sorguları olarak kullanılabilirler.

1. Seçildikten sonra, **arama sorgusu** alanında uyarı sorgusunu yazın, yapıştırın veya düzenleyin.

1. [Son bölümde](#log-alert-for-log-analytics-and-application-insights)açıklanan sonraki adımlara devam edin.

### <a name="log-alert-for-all-other-resource-types"></a>Diğer tüm kaynak türleri için günlük uyarısı

> [!NOTE]
> Şu anda API sürümü `2020-05-01-preview` ve kaynak merkezli günlük uyarıları için ek ücret alınmaz.  Önizlemede olan özellikler için fiyatlandırma, gelecekte duyurulacak ve faturalandırma başlamadan önce bir bildirim sunulacaktır. Bildirim döneminden sonra yeni API sürümü ve kaynak merkezli günlük uyarılarını kullanmaya devam etmeyi tercih etmeniz gerekir, ilgili ücret üzerinden faturalandırılırsınız.

1. **Koşul** sekmesinden başlatın:

    1. [**Ölçü**](alerts-unified-log.md#measure), [**toplama türü**](alerts-unified-log.md#aggregation-type)ve [**toplama ayrıntı**](alerts-unified-log.md#aggregation-granularity) düzeyinin doğru olduğundan emin olun. 
        1. Varsayılan olarak, kural son 5 dakika içindeki sonuçların sayısını sayar.
        1. Özetlenen sorgu sonuçlarını tespit etmemiz durumunda, kural birkaç saniye içinde otomatik olarak güncelleştirilerek bu durum, bunu yakalar.

    1. Gerekirse, [boyutlara göre bölmek için uyarı](alerts-unified-log.md#split-by-alert-dimensions)seçin: 
       - Algılanırsa **kaynak kimliği sütunu** otomatik olarak seçilir ve tetiklenen uyarının bağlamını kaydın kaynağına değiştirir. 
       - **Kaynak kimliği sütunu** , abonelik veya kaynak gruplarında uyarı vermek için devre dışı olabilir. Sorgu sonuçları, çapraz kaynakları temel alarak, devre dışı bırakma yararlı olur. Örneğin, kaynak grubunun sanal makinelerinin %80 ' ünü yüksek CPU kullanımı yaşamadığını kontrol eden bir sorgu.
       - Boyutlar tablosu kullanılarak herhangi bir sayı veya metin sütunu türü için en fazla altı ek SPIN de seçilebilir.
       - Uyarılar, benzersiz kombinasyonlara ve uyarı yüküne göre bölünme göre ayrı olarak tetiklenir ve bu bilgileri içerir.
    
        ![Toplama parametrelerini seçin ve bölme](media/alerts-log/select-aggregation-parameters-and-splitting.png)

    1. **Önizleme** grafiği zaman içinde sorgu değerlendirmeleri sonuçlarını gösterir. Grafik dönemini değiştirebilir veya boyutlara göre bölerek benzersiz uyarıdan kaynaklanan farklı zaman serisini seçebilirsiniz.

        ![Önizleme grafiği](media/alerts-log/preview-chart.png)

    1. Ardından, önizleme verilerine göre **Uyarı mantığını** ayarlayın; [ **İşleç**, **eşik değeri**](alerts-unified-log.md#threshold-and-operator)ve [**Sıklık**](alerts-unified-log.md#frequency).

        ![Eşik ve uyarı mantığı içeren Önizleme grafiği](media/alerts-log/chart-and-alert-logic.png)

    1. İsteğe bağlı olarak [**, uyarıyı tetiklemek için,**](alerts-unified-log.md#number-of-violations-to-trigger-alert) **Gelişmiş Seçenekler** bölümünde ihlal sayısı ayarlayabilirsiniz.
    
        ![Gelişmiş seçenekler](media/alerts-log/advanced-options.png)

1. **Eylemler** sekmesinde, gerekli [eylem gruplarını](action-groups.md)seçin veya oluşturun.

    ![Eylemler sekmesi](media/alerts-log/actions-tab.png)

1. **Ayrıntılar** sekmesinde, **Uyarı kuralı ayrıntılarını** ve **proje ayrıntılarını** tanımlayın. İsteğe bağlı olarak, **Şimdi çalıştırmayı başlatma** veya uyarı kuralı başladıktan sonra bir süre Için [**Eylemler sesini kapatma**](alerts-unified-log.md#state-and-resolving-alerts) seçeneğini belirleyebilirsiniz.

    > [!NOTE]
    > Günlük uyarı kuralları şu anda durum bilgisiz değildir ve bir uyarı her oluşturulduğunda bir uyarı oluşmadığında bir eylem harekete geçirilir.

    ![Ayrıntılar sekmesi](media/alerts-log/details-tab.png)

1. **Etiketler** sekmesinde, uyarı kuralı kaynağında gerekli etiketleri ayarlayın.

    ![Etiketler sekmesi](media/alerts-log/tags-tab.png)

1. **Gözden geçir + oluştur** sekmelerinde, bir doğrulama çalışır ve herhangi bir sorunu bilgilendirecektir. Kural tanımını gözden geçirin ve onaylayın.
1. Tüm alanlar doğruysa, **Oluştur** düğmesini seçin ve uyarı kuralı oluşturma ' yı doldurun. Tüm uyarılar uyarı yönetiminden görüntülenebilir.
 
    ![İnceleme ve sekme oluşturma](media/alerts-log/review-and-create-tab.png)

## <a name="view--manage-log-alerts-in-azure-portal"></a>Azure portal 'da günlük uyarılarını görüntüleme & yönetme

1. [Portalda](https://portal.azure.com/)ilgili kaynağı veya **izleme** hizmetini seçin. Ardından Izleyici bölümünde **Uyarılar** ' ı seçin.

1. Uyarı yönetimi, tetiklenen tüm uyarıları görüntüler. [Uyarı yönetimi hakkında daha fazla bilgi edinin](alerts-managing-alert-instances.md).

    > [!NOTE]
    > Günlük uyarı kuralları şu anda [durum bilgisiz ve çözümlenmiyor](alerts-unified-log.md#state-and-resolving-alerts).

1. Kuralları düzenlemek için üstteki çubukta **Uyarı kurallarını yönet** düğmesini seçin:

    ![ Uyarı kurallarını yönet](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-powershell"></a>PowerShell kullanarak günlük uyarılarını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!NOTE]
> PowerShell Şu anda API sürümünde desteklenmiyor `2020-05-01-preview`

Aşağıda listelenen PowerShell cmdlet 'leri [Zamanlanmış sorgu KURALLARı API](/rest/api/monitor/scheduledqueryrules/)'siyle kuralları yönetmek için kullanılabilir.

- New [-AzScheduledQueryRule](/powershell/module/az.monitor/new-azscheduledqueryrule) : PowerShell cmdlet 'i yeni bir günlük uyarı kuralı oluşturmak için.
- [Set-AzScheduledQueryRule](/powershell/module/az.monitor/set-azscheduledqueryrule) : PowerShell cmdlet 'i var olan bir günlük uyarısı kuralını güncelleştirmek için.
- [New-AzScheduledQueryRuleSource](/powershell/module/az.monitor/new-azscheduledqueryrulesource) : bir günlük uyarısı için kaynak parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleSchedule](/powershell/module/az.monitor/new-azscheduledqueryruleschedule): bir günlük uyarısı için zamanlama parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) : bir günlük uyarısı için eylem parametreleri belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleAznsActionGroup](/powershell/module/az.monitor/new-azscheduledqueryruleaznsactiongroup) : bir günlük uyarısı için eylem grupları parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleTriggerCondition](/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) : günlük uyarısı için tetikleyici koşulu parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleLogMetricTrigger](/powershell/module/az.monitor/new-azscheduledqueryrulelogmetrictrigger) : ölçüm [Ölçüm türü günlük uyarısı](./alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value)için ölçüm tetikleyicisi koşul parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleTriggerCondition](/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) cmdlet 'i tarafından giriş olarak kullanılır.
- [Get-AzScheduledQueryRule](/powershell/module/az.monitor/get-azscheduledqueryrule) : var olan günlük uyarısı kurallarını veya belirli bir günlük uyarısı kuralını listelemek için PowerShell cmdlet 'i
- [Update-AzScheduledQueryRule](/powershell/module/az.monitor/update-azscheduledqueryrule) : PowerShell cmdlet 'i, günlük uyarı kuralını etkinleştirmek veya devre dışı bırakmak için
- [Remove-AzScheduledQueryRule](/powershell/module/az.monitor/remove-azscheduledqueryrule): PowerShell cmdlet 'i var olan bir günlük uyarı kuralını siler

> [!NOTE]
> ScheduledQueryRules PowerShell cmdlet 'leri, yalnızca geçerli [Zamanlanmış sorgu KURALLARı API](/rest/api/monitor/scheduledqueryrules/)'sinde oluşturulan kuralları yönetebilir. Eski [Log Analytics uyarı API 'si](api-alerts.md) kullanılarak oluşturulan günlük uyarı kuralları yalnızca, [ZAMANLANMıŞ sorgu kuralları API 'sine geçildiğinde](alerts-log-api-switch.md)PowerShell kullanılarak yönetilebilir.

PowerShell kullanarak bir günlük uyarı kuralı oluşturmak için örnek adımlar aşağıda verilmiştir:

```powershell
$source = New-AzScheduledQueryRuleSource -Query 'Heartbeat | summarize AggregatedValue = count() by bin(TimeGenerated, 5m), _ResourceId' -DataSourceId "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews"

$schedule = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 15 -TimeWindowInMinutes 30

$metricTrigger = New-AzScheduledQueryRuleLogMetricTrigger -ThresholdOperator "GreaterThan" -Threshold 2 -MetricTriggerType "Consecutive" -MetricColumn "_ResourceId"

$triggerCondition = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator "LessThan" -Threshold 5 -MetricTrigger $metricTrigger

$aznsActionGroup = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG" -EmailSubject "Custom email subject" -CustomWebhookPayload "{ `"alert`":`"#alertrulename`", `"IncludeSearchResults`":true }"

$alertingAction = New-AzScheduledQueryRuleAlertingAction -AznsAction $aznsActionGroup -Severity "3" -Trigger $triggerCondition

New-AzScheduledQueryRule -ResourceGroupName "contosoRG" -Location "Region Name for your Application Insights App or Log Analytics Workspace" -Action $alertingAction -Enabled $true -Description "Alert description" -Schedule $schedule -Source $source -Name "Alert Name"
```

Ayrıca, PowerShell kullanarak bir [şablon ve parametreler](./alerts-log-create-templates.md) dosyası kullanarak günlük uyarısı oluşturabilirsiniz:

```powershell
Connect-AzAccount

Select-AzSubscription -SubscriptionName <yourSubscriptionName>

New-AzResourceGroupDeployment -Name AlertDeployment -ResourceGroupName ResourceGroupofTargetResource `
  -TemplateFile mylogalerttemplate.json -TemplateParameterFile mylogalerttemplate.parameters.json
```

## <a name="managing-log-alerts-using-cli"></a>CLı kullanarak günlük uyarılarını yönetme

> [!NOTE]
> Azure CLı desteği yalnızca scheduledQueryRules API sürümü ve üzeri için kullanılabilir `2020-05-01-preview` . Sanal API sürümü, aşağıda açıklandığı gibi şablonlarla Azure Resource Manager CLı kullanabilir. Eski [Log Analytics uyarı API](api-alerts.md)'sini KULLANıYORSANıZ, CLI kullan ' a geçmeniz gerekir. [Geçiş hakkında daha fazla bilgi edinin](./alerts-log-api-switch.md).

Önceki bölümlerde Azure portal kullanarak günlük uyarı kurallarının nasıl oluşturulacağı, görüntüleneceği ve yönetileceği açıklanmaktadır. Bu bölümde, platformlar arası [Azure CLI](/cli/azure/get-started-with-azure-cli)kullanarak nasıl yapılacağı açıklanır. Azure CLı 'yı kullanmaya başlamanın en hızlı yolu [Azure Cloud Shell](../../cloud-shell/overview.md)kullanmaktır. Bu makalede Cloud Shell kullanacağız.

1. Azure portal git **Cloud Shell**' i seçin.

1. Komut isteminde komutlar ``--help`` ve kullanma hakkında daha fazla bilgi edinmek için seçeneğini kullanabilirsiniz. Örneğin, aşağıdaki komut, günlük uyarılarını oluşturmak, görüntülemek ve yönetmek için kullanabileceğiniz komutların listesini gösterir:

    ```azurecli
    az monitor scheduled-query --help
    ```

1. Sistem olay hatalarının sayısını izleyen bir günlük uyarı kuralı oluşturabilirsiniz:

    ```azurecli
    az monitor scheduled-query create -g {ResourceGroup} -n {nameofthealert} --scopes {vm_id} --condition "count \'union Event, Syslog | where TimeGenerated > ago(1h) | where EventLevelName == \"Error\" or SeverityLevel== \"err\"\' > 2" --description {descriptionofthealert}
    ```

1. Aşağıdaki komutu kullanarak bir kaynak grubundaki tüm günlük uyarılarını görüntüleyebilirsiniz:

    ```azurecli
    az monitor scheduled-query list -g {ResourceGroup}
    ```

1. Kuralın adını veya kaynak KIMLIĞINI kullanarak belirli bir günlük uyarısı kuralının ayrıntılarını görebilirsiniz:

    ```azurecli
    az monitor scheduled-query show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor scheduled-query show --ids {RuleResourceId}
    ```

1. Aşağıdaki komutu kullanarak bir günlük uyarı kuralını devre dışı bırakabilirsiniz:

    ```azurecli
    az monitor scheduled-query update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

1. Aşağıdaki komutu kullanarak bir günlük uyarı kuralını silebilirsiniz:

    ```azurecli
    az monitor scheduled-query delete -g {ResourceGroup} -n {AlertRuleName}
    ```

Azure Resource Manager CLı 'yi [şablon](./alerts-log-create-templates.md) dosyaları ile de kullanabilirsiniz:

```azurecli
az login

az deployment group create \
    --name AlertDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file mylogalerttemplate.json \
    --parameters @mylogalerttemplate.parameters.json
```

Oluşturma işlemi başarılı olduğunda 201 döndürülür. Güncelleştirme başarısı için 200 döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlük uyarıları](./alerts-unified-log.md)hakkında bilgi edinin.
* [Azure Resource Manager şablonları](./alerts-log-create-templates.md)kullanarak günlük uyarıları oluşturun.
* [Günlük uyarıları için Web kancası eylemlerini](./alerts-log-webhook.md)anlayın.
* [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.