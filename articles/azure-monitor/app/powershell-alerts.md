---
title: Application Insights uyarıları ayarlamak için PowerShell 'i kullanma | Microsoft Docs
description: Ölçüm değişiklikleriyle ilgili e-posta almak için Application Insights yapılandırmasını otomatikleştirin.
ms.topic: conceptual
ms.date: 07/23/2016
ms.openlocfilehash: 74d477b6660c0f7ec2ee32b34169bb85886936e5
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322474"
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Application Insights uyarıları ayarlamak için PowerShell kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Application Insights](./app-insights-overview.md) [Uyarı](../platform/alerts-log.md) yapılandırmasını otomatikleştirebilir.

Ayrıca, [Web kancalarını bir uyarıya yönelik yanıtınızı otomatikleştirmek için ayarlayabilirsiniz](../platform/alerts-webhooks.md).

> [!NOTE]
> Aynı anda kaynak ve uyarı oluşturmak istiyorsanız, [bir Azure Resource Manager şablonu](powershell.md)kullanmayı göz önünde bulundurun.

## <a name="one-time-setup"></a>Tek seferlik kurulum
Daha önce Azure aboneliğinizle PowerShell kullanmadıysanız:

Azure PowerShell modülünü, betikleri çalıştırmak istediğiniz makineye yükler.

* [Microsoft Web Platformu Yükleyicisi (V5 veya üzeri)](https://www.microsoft.com/web/downloads/platform.aspx)yükler.
* Microsoft Azure PowerShell 'i yüklemek için kullanın

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Azure PowerShell başlatın ve [aboneliğinize bağlanın](/powershell/azure/):

```azurepowershell
Add-AzAccount
```


## <a name="get-alerts"></a>Uyarıları al

```azurepowershell
Get-AzAlertRule -ResourceGroup "Fabrikam" `
  [-Name "My rule"] `
  [-DetailedOutput]
```

## <a name="add-alert"></a>Uyarı Ekle

```azurepowershell
Add-AzMetricAlertRule -Name "{ALERT NAME}" `
  -Description "{TEXT}" `
  -ResourceGroup "{GROUP NAME}" `
  -TargetResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
  -MetricName "{METRIC NAME}" `
  -Operator GreaterThan `
  -Threshold {NUMBER}  `
  -WindowSize {HH:MM:SS} `
  [-SendEmailToServiceOwners] `
  [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM"] `
  -Location "East US" // must be East US at present `
  -RuleType Metric
```


## <a name="example-1"></a>Örnek 1
Sunucunun HTTP isteklerine yanıtı 5 dakikadan fazla olursa, bana e-posta gönderin 1 saniyeden daha yavaştır. Application Insights kaynağıdır ıecreamwebapp olarak adlandırılır ve bu, Fabrikam kaynak grubunda yer alan. Azure aboneliğinin sahibiyim.

GUID, abonelik KIMLIĞIDIR (uygulamanın izleme anahtarı değildir).

```azurepowershell
Add-AzMetricAlertRule -Name "slow responses" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "request.duration" `
  -Operator GreaterThan `
  -Threshold 1 `
  -WindowSize 00:05:00 `
  -SendEmailToServiceOwners `
  -Location "East US" `
  -RuleType Metric
```

## <a name="example-2"></a>Örnek 2
"SalesPerHour" adlı bir ölçümü raporlamak için [Trackmetric ()](./api-custom-events-metrics.md#trackmetric) kullandığım bir uygulamam var. "SalesPerHour", 24 saatten fazla ortalama olan 100 altına düşerse iş arkadaşlarınıza e-posta gönderin.

```azurepowershell
Add-AzMetricAlertRule -Name "poor sales" `
  -Description "slow sales alert" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "salesPerHour" `
  -Operator LessThan `
  -Threshold 100 `
  -WindowSize 24:00:00 `
  -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
  -Location "East US" `
  -RuleType Metric
```

Aynı kural, TrackEvent veya trackPageView gibi başka bir izleme çağrısının [ölçüm parametresi](./api-custom-events-metrics.md#properties) kullanılarak bildirilen ölçüm için de kullanılabilir.

## <a name="metric-names"></a>Ölçüm adları
| Ölçüm adı | Ekran adı | Açıklama |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Tarayıcı özel durumları |Tarayıcıda oluşturulan yakalanamayan özel durumların sayısı. |
| `basicExceptionServer.count` |Sunucu özel durumları |Uygulama tarafından oluşturulan işlenmemiş özel durumların sayısı |
| `clientPerformance.clientProcess.value` |İstemci işlem süresi |DOM yükleninceye kadar belgenin son baytını alma arasındaki süre. Zaman uyumsuz istekler hala işlenmeye devam edebilir. |
| `clientPerformance.networkConnection.value` |Sayfa yükleme ağ bağlantı süresi |Tarayıcının ağa bağlanması için geçen süre. Önbelleğe alınmışsa 0 olabilir. |
| `clientPerformance.receiveRequest.value` |Yanıt süresini alma |Tarayıcı gönderme isteği ile yanıt almaya başlamak için geçen süre. |
| `clientPerformance.sendRequest.value` |İstek gönderme süresi |İsteğin gönderilmesi için tarayıcıdan geçen süre. |
| `clientPerformance.total.value` |Tarayıcı sayfa yükleme süresi |DOM, stil sayfaları, betikler ve görüntüler yükleninceye kadar Kullanıcı isteğinden geçen süre. |
| `performanceCounter.available_bytes.value` |Kullanılabilir bellek |Fiziksel bellek bir işlem veya sistem kullanımı için hemen kullanılabilir. |
| `performanceCounter.io_data_bytes_per_sec.value` |İşlem GÇ oranı |Dakikada, ağda ve cihazlarda okunan ve yazılan toplam bayt/saniye. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |özel durum oranı |Saniye başına oluşturulan özel durumlar. |
| `performanceCounter.percentage_processor_time.value` |İşlem CPU 'SU |İşlemci tarafından uygulama işleminin yönergelerini yürütmek için kullanılan tüm işlem iş parçacıklarının geçen sürenin yüzdesi. |
| `performanceCounter.percentage_processor_total.value` |İşlemci zamanı |İşlemcinin boş olmayan iş parçacıklarında harcadığı sürenin yüzdesi. |
| `performanceCounter.process_private_bytes.value` |İşlem özel baytları |İzlenen uygulamanın işlemlerine özel olarak atanan bellek. |
| `performanceCounter.request_execution_time.value` |ASP.NET isteği yürütme süresi |En son isteğin yürütme süresi. |
| `performanceCounter.requests_in_application_queue.value` |Yürütme kuyruğundaki ASP.NET istekleri |Uygulama isteği sırasının uzunluğu. |
| `performanceCounter.requests_per_sec.value` |ASP.NET istek hızı |ASP.NET ' den saniyede uygulamaya yapılan tüm isteklerin oranı. |
| `remoteDependencyFailed.durationMetric.count` |Bağımlılık sorunları |Sunucu uygulaması tarafından dış kaynaklara yapılan başarısız çağrıların sayısı. |
| `request.duration` |Sunucu yanıt süresi |Bir HTTP isteği alma ve yanıtın gönderilmesini tamamlama arasındaki süre. |
| `request.rate` |İstek hızı |Uygulamaya saniye başına yapılan tüm isteklerin oranı. |
| `requestFailed.count` |Başarısız istekler |Yanıt koduyla sonuçlanan HTTP isteklerinin sayısı >= 400 |
| `view.count` |Sayfa görünümleri |Bir Web sayfasına yönelik istemci kullanıcı isteklerinin sayısı. Yapay trafik filtrelendi. |
| {Özel Ölçüm adınız} |{Ölçüm adınız} |Ölçüm değeri, [trackmetric](./api-custom-events-metrics.md#trackmetric) tarafından veya [bir izleme çağrısının ölçümler parametresinde](./api-custom-events-metrics.md#properties)raporlanır. |

Ölçümler farklı telemetri modülleri tarafından gönderilir:

| Ölçüm grubu | Toplayıcı modülü |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>görüntüle |[Tarayıcı JavaScript 'ı](./javascript.md) |
| performanceCounter |[Performans](./configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Bağımlılık](./configuration-with-applicationinsights-config.md) |
| isteyen<br/>requestFailed |[Sunucu isteği](./configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Web Kancaları
[Bir uyarıya yanıtınızı otomatikleştirebilir](../platform/alerts-webhooks.md). Bir uyarı ortaya çıktığında Azure, seçtiğiniz bir Web adresini çağırır.

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights yapılandırılacak komut dosyası](./create-new-resource.md#creating-a-resource-automatically)
* [Şablonlardan Application Insights ve Web testi kaynakları oluşturma](powershell.md)
* [Application Insights için kupMicrosoft Azure tanılama otomatikleştirin](powershell-azure-diagnostics.md)
* [bir uyarıya yanıtınızı otomatikleştirin](../platform/alerts-webhooks.md)

