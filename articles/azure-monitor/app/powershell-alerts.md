---
title: Uygulama Öngörüleri'nde uyarıları ayarlamak için Powershell'i kullanın | Microsoft Dokümanlar
description: Metrik değişikliklerle ilgili e-postalar almak için Uygulama Öngörüleri yapılandırmasını otomatikleştirin.
ms.topic: conceptual
ms.date: 10/31/2016
ms.openlocfilehash: c19cb43d08b44b55c786e750e64a83e6f0c67381
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669854"
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Application Insights uyarıları ayarlamak için PowerShell kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Uygulama Öngörüleri'ndeki](../../azure-monitor/app/app-insights-overview.md) [uyarıların](../../azure-monitor/app/alerts.md) yapılandırmasını otomatikleştirebilirsiniz.

Buna ek olarak, [bir uyarıya yanıtınızı otomatikleştirmek için webhooks](../../azure-monitor/platform/alerts-webhooks.md)ayarlayabilirsiniz.

> [!NOTE]
> Aynı anda kaynak ve uyarı oluşturmak istiyorsanız, [bir Azure Kaynak Yöneticisi şablonu kullanmayı](powershell.md)düşünün.

## <a name="one-time-setup"></a>Tek seferlik kurulum
PowerShell'i azure aboneliğinizle daha önce kullanmadıysanız:

Azure Powershell modüllerini komut dosyalarını çalıştırmak istediğiniz makineye yükleyin.

* [Microsoft Web Platform Installer (v5 veya üstü)](https://www.microsoft.com/web/downloads/platform.aspx)yükleyin.
* Microsoft Azure Powershell'i yüklemek için kullanın

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Azure PowerShell'i başlatın ve [aboneliğinize bağlanın:](/powershell/azure/overview)

```powershell

    Add-AzAccount
```


## <a name="get-alerts"></a>Uyarıları alın
    Get-AzAlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Uyarı ekleme
    Add-AzMetricAlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Örnek 1
Sunucunun HTTP isteklerine verdiği yanıt ortalama 5 dakikadan fazla ysa, 1 saniyeden daha yavaşsa bana e-posta gönder. Uygulama Öngörüleri kaynağımın adı IceCreamWebApp ve kaynak grubu Fabrikam'da. Azure aboneliğinin sahibiyim.

GUID abonelik kimliğidir (uygulamanın enstrümantasyon anahtarı değildir).

    Add-AzMetricAlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Örnek 2
"salesPerHour" adlı bir metriği bildirmek için [TrackMetric()](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric) kullandığım bir uygulamam var. "SalesPerHour" 24 saat içinde ortalama 100'ün altına düşerse meslektaşlarıma bir e-posta gönderin.

    Add-AzMetricAlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Aynı kural, TrackEvent veya trackPageView gibi başka bir izleme çağrısının [ölçüm parametresi](../../azure-monitor/app/api-custom-events-metrics.md#properties) kullanılarak bildirilen metrik için de kullanılabilir.

## <a name="metric-names"></a>Metrik adlar
| Ölçüm adı | Ekran adı | Açıklama |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Tarayıcı özel durumları |Tarayıcıya atılan yakalanmamış özel durumlar sayısı. |
| `basicExceptionServer.count` |Sunucu özel durumları |Uygulama tarafından atılan işlenmemiş özel durumlar sayısı |
| `clientPerformance.clientProcess.value` |İstemci işleme süresi |DOM yüklenene kadar belgenin son baytını alma arasındaki süre. Async istekleri hala işliyor olabilir. |
| `clientPerformance.networkConnection.value` |Sayfa yükleme ağı bağlama süresi |Tarayıcının ağa bağlanmak için gereken süre. Önbelleğe alınmışsa 0 olabilir. |
| `clientPerformance.receiveRequest.value` |Yanıt süresi alma |Yanıt almaya başlamak için tarayıcı gönderme isteği arasındaki süre. |
| `clientPerformance.sendRequest.value` |İstek süresi gönder |İstek göndermek için tarayıcı tarafından alınan süre. |
| `clientPerformance.total.value` |Tarayıcı sayfası yükleme süresi |DOM, stylesheets, scripts ve görüntüler yüklenene kadar kullanıcı isteği nden zaman. |
| `performanceCounter.available_bytes.value` |Kullanılabilir bellek |Fiziksel bellek hemen bir işlem veya sistem kullanımı için kullanılabilir. |
| `performanceCounter.io_data_bytes_per_sec.value` |Proses IO Oranı |Saniyedeki toplam bayt lar dosya, ağ ve aygıtlara okunur ve yazılır. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |istisna oranı |Saniyede atılan özel durumlar. |
| `performanceCounter.percentage_processor_time.value` |İşlem CPU'su |İşlemci tarafından kullanılan tüm işlem iş parçacığının geçen süre yüzdesi, uygulama işlemi için yürütme yönergeleri. |
| `performanceCounter.percentage_processor_total.value` |İşlemci süresi |İşlemcinin Boşta Olmayan iş parçacıklarında harcadığı zaman yüzdesi. |
| `performanceCounter.process_private_bytes.value` |Özel baytları işleme |Yalnızca izlenen uygulamanın işlemlerine atanan bellek. |
| `performanceCounter.request_execution_time.value` |ASP.NET yürütme süresi talep |En son isteğin yürütme zamanı. |
| `performanceCounter.requests_in_application_queue.value` |yürütme kuyruğundaki istekleri ASP.NET |Uygulama istek sırasının uzunluğu. |
| `performanceCounter.requests_per_sec.value` |ASP.NET istek oranı |ASP.NET itibaren uygulama için tüm isteklerin oranı. |
| `remoteDependencyFailed.durationMetric.count` |Bağımlılık hataları |Sunucu uygulaması tarafından dış kaynaklara yapılan başarısız çağrıların sayısı. |
| `request.duration` |Sunucu yanıt süresi |BIR HTTP isteği alma ve yanıt gönderme bitirme arasındaki süre. |
| `request.rate` |İstek oranı |Tüm isteklerin saniyedeki uygulama oranı. |
| `requestFailed.count` |Başarısız istekler |Yanıt kodu >= 400 ile sonuçlanan HTTP isteklerinin sayısı |
| `view.count` |Sayfa görünümleri |Bir web sayfası için istemci kullanıcı isteklerinin sayısı. Sentetik trafik filtrelenir. |
| {özel metrik adınız} |{Metrik adınız} |[TrackMetric](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric) tarafından veya [izleme çağrısının ölçüm parametresi](../../azure-monitor/app/api-custom-events-metrics.md#properties)ile bildirilen metrik değeriniz. |

Ölçümler farklı telemetri modülleri tarafından gönderilir:

| Metrik grup | Kollektör modülü |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>görüntüle |[Tarayıcı JavaScript](../../azure-monitor/app/javascript.md) |
| Performancecounter |[Performans](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Bağımlılık](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |
| Istek<br/>requestFailed |[Sunucu isteği](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Web Kancaları
[Yanıtınızı bir uyarıya otomatikleştirebilirsiniz.](../../azure-monitor/platform/alerts-webhooks.md) Azure, bir uyarı yükseltildiğinde seçtiğiniz bir web adresini arar.

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama Öngörülerini yapılandırmak için komut dosyası](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically)
* [Şablonlardan Uygulama Öngörüleri ve web test kaynakları oluşturma](powershell.md)
* [Microsoft Azure Tanılama'yı Uygulama Öngörülerine Otomatikleştirme](powershell-azure-diagnostics.md)
* [Yanıtınızı bir uyarıya otomatikleştirme](../../azure-monitor/platform/alerts-webhooks.md)
