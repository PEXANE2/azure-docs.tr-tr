---
title: Ölçüm Danışmanı C# hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 10/14/2020
ms.author: mbullwin
ms.openlocfilehash: 8e9b2c69344a19c027f72983a02834aee2e14ccf
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186869"
---
[Başvuru belgeleri](https://aka.ms/azsdk/net/docs/ref/metricsadvisor)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.MetricsAdvisor)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/samples/README.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, ölçüm Danışmanı <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> örneğinizi dağıtmak için Azure Portal ölçüm Danışmanı kaynağı oluşturun.  
* Zaman serisi verileriyle kendi SQL veritabanınız.
   
> [!TIP]
> * [GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/samples/README.md)'Da .net ölçüm Danışmanı örneklerini bulabilirsiniz.
> * Ölçüm Danışmanı kaynağınızın kullanabilmeniz için bir hizmet örneği dağıtması 10 ila 30 dakika sürebilir. Başarılı bir şekilde dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın. Dağıtımdan sonra, ölçüm Danışmanı örneğinizi hem Web portalı hem de REST API kullanmaya başlayabilirsiniz. 
> * Azure portal REST API URL 'sini kaynağınızın **genel bakış** bölümünde bulabilirsiniz. Şu şekilde görünür:
>    * `https://<instance-name>.cognitiveservices.azure.com/`
   
## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler 

Yeni bir proje oluşturduktan sonra, **Çözüm Gezgini** proje çözümüne sağ tıklayıp **NuGet Paketlerini Yönet** ' i seçerek istemci kitaplığını yükleyebilirsiniz. Açılan paket yöneticisinde, Seç ' i seçin, **ön sürümü dahil** **et ' i** işaretleyin ve arama yapın `Azure.AI.MetricsAdvisor` . Sürüm `1.0.0-beta.1` ' ü ve ardından **öğesini seçin** . 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `metrics-advisor-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs* . 

```console
dotnet new console -n metrics-advisor-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler 

Visual Studio dışında bir IDE kullanıyorsanız, aşağıdaki komutla .NET için ölçüm Danışmanı istemci Kitaplığı ' nı yükleyebilirsiniz:

```console
dotnet add package Azure.AI.MetricsAdvisor --version 1.0.0-beta.1
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.MetricsAdvisor_1.0.0-beta.1/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/samples)'da bulabilirsiniz.

Proje dizininden *program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Azure.AI.MetricsAdvisor.Administration;
using Azure.AI.MetricsAdvisor.Models;
```

Uygulamanın `Main()` yönteminde, bu hızlı başlangıçta kullanılan yöntemlere çağrılar ekleyin. Bunları daha sonra oluşturacaksınız.

```csharp
static void Main(string[] args){
    // You will create the below methods later in the quickstart
    exampleTask1();
}
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar, ölçüm Danışmanı C# SDK 'SıNıN bazı önemli özelliklerini işler.

|Ad|Açıklama|
|---|---|
| [MetricsAdvisorClient](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/MetricsAdvisorClient.cs) | **Kullanım için** : <br> -Olayları listeleme <br> -Olayların temel nedenini listeleme <br> -Özgün zaman serisi verileri ve zaman serisi verilerinin hizmet tarafından zenginleştirerek elde alınması. <br> -Uyarılar listeleniyor <br> -Modelinizi ayarlamak için geri bildirim ekleme |
| [MetricsAdvisorAdministrationClient](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/MetricsAdvisorAdministrationClient.cs)| **Şunları yapmanıza olanak sağlar:** <br> -Veri akışlarını yönetme <br> -Anomali algılama yapılandırmasını yapılandırma <br> -Anomali uyarı yapılandırmasını yapılandırın <br> -Kancaları Yönet  |
| [Veri akışı](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/Models/DataFeed/DataFeed.cs)| **Veri kaynağından alınan ölçüm danışmanı. Şu `DataFeed` satırları içerir:** <br> -Zaman damgaları <br> -Sıfır veya daha fazla boyut <br> -Bir veya daha fazla ölçü  |
| [Ölçüm](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/Models/DataFeedMetric.cs)| , `Metric` Belirli bir iş sürecinin durumunu izlemek ve değerlendirmek için kullanılan, ölçülebilir bir ölçüdür. Bu, boyutlara bölünen birden çok zaman serisi değerinin bir birleşimi olabilir. Örneğin, bir Web durumu ölçümü Kullanıcı sayısı ve en-US pazarında boyut içerebilir. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için ölçüm Danışmanı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Veri akışı Ekle](#add-a-data-feed)
* [Alma durumunu denetleme](#check-the-ingestion-status)
* [Anomali algılamayı yapılandırma](#configure-anomaly-detection)
* [Kanca oluşturma](#create-a-hook)
* [Uyarı yapılandırması oluşturma](#create-an-alert-configuration)
* [Uyarıyı sorgulama](#query-the-alert)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Uygulamanın `Program` sınıfında, kaynağınızın anahtarları ve uç noktası için değişkenler oluşturun.

> [!IMPORTANT]
> Azure portala gidin. **Önkoşullar** bölümünde oluşturduğunuz ölçüm Danışmanı kaynağı başarıyla dağıtılırsa, **sonraki adımlar** altında **Kaynağa Git** düğmesine tıklayın. Kaynak **yönetimi** altında, kaynağın **anahtar ve uç** nokta sayfasında abonelik Anahtarlarınızı ve uç noktanızı bulabilirsiniz. <br><br>API anahtarınızı almak için adresine gitmeniz gerekir [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net) . Kaynağınız için uygun: **Dizin** , **abonelikler** ve **çalışma alanını** seçin ve **kullanmaya** başlayın ' ı seçin. Bundan sonra API anahtarlarınızı adresinden alabilirsiniz [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key) .   
>
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Daha fazla bilgi için bilişsel Hizmetler [güvenlik](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) makalesine bakın.

Abonelik ve API anahtarlarına sahip olduktan sonra bir MetricsAdvisorKeyCredential oluşturun. Uç nokta ve anahtar kimlik bilgileri ile şunu oluşturabilirsiniz [`MetricsAdvisorClient`](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/MetricsAdvisorClient.cs) :

```csharp
string endpoint = "<endpoint>";
string subscriptionKey = "<subscriptionKey>";
string apiKey = "<apiKey>";
var credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);
var client = new MetricsAdvisorClient(new Uri(endpoint), credential);
```

Ayrıca, [`MetricsAdvisorAdministrationClient`](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor/src/MetricsAdvisorAdministrationClient.cs) yönetim işlemlerini gerçekleştirmek için bir oluşturabilirsiniz:

```csharp
string endpoint = "<endpoint>";
string subscriptionKey = "<subscriptionKey>";
string apiKey = "<apiKey>";
var credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);
var adminClient = new MetricsAdvisorAdministrationClient(new Uri(endpoint), credential);
```

## <a name="add-a-data-feed"></a>Veri akışı Ekle

Ölçüm Danışmanı birden çok veri kaynağı türünü destekler. Bu örnekte, bir `DataFeed` SQL Server 'dan veri çıkaran bir oluşturma işlemini göstereceğiz. `connection_String`Kendı SQL Server bağlantı dizeniz ile değiştirin ve `query` tek bir zaman damgasında verilerinizi döndüren bir sorgu ile değiştirin. Ayrıca, `metric` ve `dimension` değerlerini özel verilerinize göre ayarlamanız gerekir.


```csharp
string sqlServerConnectionString = "<connection_String>";
string sqlServerQuery = "<query>";

var dataFeedName = "Sample data feed";
var dataFeedSource = new MySqlDataFeedSource(sqlServerConnectionString, sqlServerQuery);
var dataFeedGranularity = new DataFeedGranularity(DataFeedGranularityType.Daily);

var dataFeedMetrics = new List<DataFeedMetric>()
{
    new DataFeedMetric("cost"),
    new DataFeedMetric("revenue")
};
var dataFeedDimensions = new List<MetricDimension>()
{
    new MetricDimension("category"),
    new MetricDimension("city")
};
var dataFeedSchema = new DataFeedSchema(dataFeedMetrics)
{
    DimensionColumns = dataFeedDimensions
};

var ingestionStartTime = DateTimeOffset.Parse("2020-01-01T00:00:00Z");
var dataFeedIngestionSettings = new DataFeedIngestionSettings(ingestionStartTime);

Response<DataFeed> response = await adminClient.CreateDataFeedAsync(dataFeedName, dataFeedSource,
    dataFeedGranularity, dataFeedSchema, dataFeedIngestionSettings);

DataFeed dataFeed = response.Value;

Console.WriteLine($"Data feed ID: {dataFeed.Id}");

// Only the ID of the data feed is known at this point. You can perform another service
// call to get more information, such as status, created time, the list of administrators,
// or the metric IDs.

response = await adminClient.GetDataFeedAsync(dataFeed.Id);

dataFeed = response.Value;

Console.WriteLine($"Data feed status: {dataFeed.Status.Value}");
Console.WriteLine($"Data feed created time: {dataFeed.CreatedTime.Value}");

Console.WriteLine($"Data feed administrators:");
foreach (string admin in dataFeed.Options.Administrators)
{
    Console.WriteLine($" - {admin}");
}

Console.WriteLine($"Metric IDs:");
foreach (DataFeedMetric metric in dataFeed.Schema.MetricColumns)
{
    Console.WriteLine($" - {metric.MetricName}: {metric.MetricId}");
}
```

## <a name="check-the-ingestion-status"></a>Alma durumunu denetleme

Daha önce oluşturulan bir öğesinin giriş durumunu denetleme `DataFeed`

```csharp
string dataFeedId = "<dataFeedId>";

var startTime = DateTimeOffset.Parse("2020-01-01T00:00:00Z");
var endTime = DateTimeOffset.Parse("2020-09-09T00:00:00Z");
var options = new GetDataFeedIngestionStatusesOptions(startTime, endTime);

Console.WriteLine("Ingestion statuses:");
Console.WriteLine();

int statusCount = 0;

await foreach (DataFeedIngestionStatus ingestionStatus in adminClient.GetDataFeedIngestionStatusesAsync(dataFeedId, options))
{
    Console.WriteLine($"Timestamp: {ingestionStatus.Timestamp}");
    Console.WriteLine($"Status: {ingestionStatus.Status.Value}");
    Console.WriteLine($"Service message: {ingestionStatus.Message}");
    Console.WriteLine();

    // Print at most 10 statuses.
    if (++statusCount >= 10)
    {
        break;
    }
}
```

## <a name="configure-anomaly-detection"></a>Anomali algılamayı yapılandırma 

Hizmete hangi veri noktalarının anormal düşünüldüler olduğunu bildirmek için bir anomali algılama yapılandırması oluşturun.

```csharp
string metricId = "<metricId>";
string configurationName = "Sample anomaly detection configuration";

var hardThresholdSuppressCondition = new SuppressCondition(1, 100);
var hardThresholdCondition = new HardThresholdCondition(AnomalyDetectorDirection.Down, hardThresholdSuppressCondition)
{
    LowerBound = 5.0
};

var smartDetectionSuppressCondition = new SuppressCondition(4, 50);
var smartDetectionCondition = new SmartDetectionCondition(10.0, AnomalyDetectorDirection.Up, smartDetectionSuppressCondition);

var detectionCondition = new MetricWholeSeriesDetectionCondition()
{
    HardThresholdCondition = hardThresholdCondition,
    SmartDetectionCondition = smartDetectionCondition,
    CrossConditionsOperator = DetectionConditionsOperator.Or
};

var detectionConfiguration = new AnomalyDetectionConfiguration(metricId, configurationName, detectionCondition);

Response<AnomalyDetectionConfiguration> response = await adminClient.CreateMetricAnomalyDetectionConfigurationAsync(detectionConfiguration);

detectionConfiguration = response.Value;

Console.WriteLine($"Anomaly detection configuration ID: {detectionConfiguration.Id}");
```

### <a name="create-a-hook"></a>Kanca oluşturma

Ölçüm Danışmanı, `EmailHook` `WebHook` Uyarı bildirimlerine abone olma yöntemi olarak ve sınıflarını destekler. Bu örnekte nasıl oluşturulacağını göstereceğiz `EmailHook` . Bildirimleri almaya başlamak için bir anomali uyarı yapılandırmasına geçiş yapmanız gerekir. Daha fazla bilgi için bkz. [anomali uyarı yapılandırması oluşturma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/metricsadvisor/Azure.AI.MetricsAdvisor#create-an-anomaly-alert-configuration) örneği.

```csharp
string hookName = "Sample hook";
var emailsToAlert = new List<string>()
{
    "email1@sample.com",
    "email2@sample.com"
};

var emailHook = new EmailNotificationHook(hookName, emailsToAlert);

Response<NotificationHook> response = await adminClient.CreateHookAsync(emailHook);

NotificationHook hook = response.Value;

Console.WriteLine($"Hook ID: {hook.Id}");
```

##  <a name="create-an-alert-configuration"></a>Uyarı yapılandırması oluşturma

`AnomalyAlertConfiguration`Hizmeti, uyarıları tetikleyebilen bir hizmete bildirmek için oluşturun.

```csharp
string hookId = "<hookId>";
string anomalyDetectionConfigurationId = "<anomalyDetectionConfigurationId>";

string configurationName = "Sample anomaly alert configuration";
var idsOfHooksToAlert = new List<string>() { hookId };

var scope = MetricAnomalyAlertScope.GetScopeForWholeSeries();
var metricAlertConfigurations = new List<MetricAnomalyAlertConfiguration>()
{
    new MetricAnomalyAlertConfiguration(anomalyDetectionConfigurationId, scope)
};

AnomalyAlertConfiguration alertConfiguration = new AnomalyAlertConfiguration(configurationName, idsOfHooksToAlert, metricAlertConfigurations);

Response<AnomalyAlertConfiguration> response = await adminClient.CreateAnomalyAlertConfigurationAsync(alertConfiguration);

alertConfiguration = response.Value;

Console.WriteLine($"Alert configuration ID: {alertConfiguration.Id}");
```

### <a name="query-the-alert"></a>Uyarıyı sorgulama

Belirli bir anomali uyarı yapılandırması tarafından oluşturulan uyarılara göz atın ve bu uyarıları tetikleyen anormallikleri listeleyin.

```csharp
string anomalyAlertConfigurationId = "<anomalyAlertConfigurationId>";

var startTime = DateTimeOffset.Parse("2020-01-01T00:00:00Z");
var endTime = DateTimeOffset.UtcNow;
var options = new GetAlertsOptions(startTime, endTime, TimeMode.AnomalyTime);

int alertCount = 0;

await foreach (AnomalyAlert alert in client.GetAlertsAsync(anomalyAlertConfigurationId, options))
{
    Console.WriteLine($"Alert at timestamp: {alert.Timestamp}");
    Console.WriteLine($"Id: {alert.Id}");
    Console.WriteLine($"Anomalies that triggered this alert:");

    await foreach (DataAnomaly anomaly in client.GetAnomaliesForAlertAsync(anomalyAlertConfigurationId, alert.Id))
    {
        Console.WriteLine("  Anomaly:");
        Console.WriteLine($"    Status: {anomaly.Status.Value}");
        Console.WriteLine($"    Severity: {anomaly.Severity}");
        Console.WriteLine($"    Series key:");

        foreach (KeyValuePair<string, string> keyValuePair in anomaly.SeriesKey.AsDictionary())
        {
            Console.WriteLine($"      Dimension '{keyValuePair.Key}': {keyValuePair.Value}");
        }

        Console.WriteLine();
    }

    // Print at most 3 alerts.
    if (++alertCount >= 3)
    {
        break;
    }
}
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `dotnet run` .

```dotnet
dotnet run
```