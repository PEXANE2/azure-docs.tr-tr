---
title: Ölçüm Danışmanı JavaScript hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 11/09/2020
ms.author: mbullwin
ms.openlocfilehash: 9222098f719241a6b3d6e50d4181106101c6d912
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96356384"
---
[Başvuru belgeleri](/javascript/api/overview/azure/ai-metrics-advisor-readme-pre?preserve-view=true&view=azure-node-preview)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/metricsadvisor/ai-metrics-advisor/README.md)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/ai-metrics-advisor)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Node.js](https://nodejs.org/) geçerli sürümü
* Azure aboneliğiniz olduktan sonra, ölçüm Danışmanı <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> örneğinizi dağıtmak için Azure Portal ölçüm Danışmanı kaynağı oluşturun.  
* Zaman serisi verileriyle kendi SQL veritabanınız.
  
> [!TIP]
> * [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples)'Da JavaScript ölçüm Danışmanı örneklerini bulabilirsiniz.
> * Ölçüm Danışmanı kaynağınızın kullanabilmeniz için bir hizmet örneği dağıtması 10 ila 30 dakika sürebilir. Başarılı bir şekilde dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın. Dağıtımdan sonra, ölçüm Danışmanı örneğinizi hem Web portalı hem de REST API kullanmaya başlayabilirsiniz. 
> * Azure portal REST API URL 'sini kaynağınızın **genel bakış** bölümünde bulabilirsiniz. Şu şekilde görünür:
>    * `https://<instance-name>.cognitiveservices.azure.com/`
    
## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`npm init`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` . 

```console
npm init
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`@azure/ai-metrics-advisor`NPM paketini yüklerken:

```console
npm install @azure/ai-metrics-advisor@1.0.0-beta.2
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

Adlı bir dosya oluşturun `index.js` ve aşağıdaki kitaplıkları içeri aktarın:

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples/javascript)'da bulabilirsiniz.

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. 

> [!IMPORTANT]
> Azure portala gidin. **Önkoşullar** bölümünde oluşturduğunuz ölçüm Danışmanı kaynağı başarıyla dağıtılırsa, **sonraki adımlar** altında **Kaynağa Git** düğmesine tıklayın. Kaynak **yönetimi** altında, kaynağın **anahtar ve uç** nokta sayfasında abonelik Anahtarlarınızı ve uç noktanızı bulabilirsiniz. <br><br>API anahtarınızı almak için adresine gitmeniz gerekir [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net) . Kaynağınız için uygun: **Dizin**, **abonelikler** ve **çalışma alanını** seçin ve **kullanmaya** başlayın ' ı seçin. Bundan sonra API anahtarlarınızı adresinden alabilirsiniz [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key) .   
>
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Daha fazla bilgi için bilişsel Hizmetler [güvenlik](../../../cognitive-services-security.md) makalesine bakın.

```javascript
subscriptionKey = "<paste-your-metrics-advisor-key-here>";
apiKey ="<paste-your-metrics-advisor-api-key-here>";
endpoint = "<paste-your-metrics-advisor-endpoint-here>";
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, ölçüm Danışmanı JavaScript SDK 'sının önemli özelliklerinden bazılarını işler.

|Ad|Açıklama|
|---|---|
| MetricsAdvisorClient | **Kullanım için**: <br> -Olayları listeleme <br> -Olayların temel nedenini listeleme <br> -Özgün zaman serisi verileri ve zaman serisi verilerinin hizmet tarafından zenginleştirerek elde alınması. <br> -Uyarılar listeleniyor <br> -Modelinizi ayarlamak için geri bildirim ekleme |
| MetricsAdvisorAdministrationClient | **Şunları yapmanıza olanak sağlar:** <br> -Veri akışlarını yönetme <br> -Anomali uyarı yapılandırması oluşturma, yapılandırma, alma, listeleme ve silme <br> -Kancaları Yönet  |
| Veri akışı | **Veri kaynağından alınan ölçüm danışmanı. Şu `DataFeed` satırları içerir:** <br> -Zaman damgaları <br> -Sıfır veya daha fazla boyut <br> -Bir veya daha fazla ölçü  |
| DataFeedMetric | , `DataFeedMetric` Belirli bir iş sürecinin durumunu izlemek ve değerlendirmek için kullanılan, ölçülebilir bir ölçüdür. Bu, boyutlara bölünen birden çok zaman serisi değerinin bir birleşimi olabilir. Örneğin, bir Web durumu ölçümü Kullanıcı sayısı ve en-US pazarında boyut içerebilir. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, JavaScript için ölçüm Danışmanı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Veri akışı Ekle](#add-a-data-feed)
* [Alma durumunu denetle](#check-ingestion-status)
* [Anomali algılamayı yapılandırma](#configure-anomaly-detection)
* [Kanca oluşturma](#create-a-hook)
* [Uyarı yapılandırması oluşturma](#create-an-alert-configuration)
* [Uyarıyı sorgulama](#query-the-alert)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

İki anahtara ve uç nokta adresine sahip olduktan sonra, istemcilerin kimliğini doğrulamak için MetricsAdvisorKeyCredential sınıfını aşağıdaki şekilde kullanabilirsiniz:

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorClient,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);
const client = new MetricsAdvisorClient(endpoint, credential);
const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
```

## <a name="add-a-data-feed"></a>Veri akışı Ekle

Ölçüm Danışmanı, farklı türlerde veri kaynaklarına bağlanmayı destekler. SQL Server verileri almak için bir örnek aşağıda verilmiştir.

`sql_server_connection_string`Kendı SQL Server bağlantı dizeniz ile değiştirin ve `query` tek bir zaman damgasında verilerinizi döndüren bir sorgu ile değiştirin. Ayrıca, `metric` ve `dimension` değerlerini özel verilerinize göre ayarlamanız gerekir.

> [!IMPORTANT]
> Sorgu her bir zaman damgasında her bir boyut birleşimi için en çok bir kayıt döndürmelidir. Ve sorgu tarafından döndürülen tüm kayıtlar aynı zaman damgalarına sahip olmalıdır. Ölçüm Danışmanı, verilerinizi almak için bu sorguyu her zaman damgası için çalıştırır. Daha fazla bilgi ve örnek için [sorgular hakkında SSS bölümüne](../../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) bakın. 

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  subscriptionKey = "<paste-your-metrics-advisor-key-here>";
  apiKey ="<paste-your-metrics-advisor-api-key-here>";
  endpoint = "<paste-your-metrics-advisor-endpoint-here>";
  const sqlServerConnectionString ="<sql_server_connection_string>";
  const sqlServerQuery ="<query>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);

  const created = await createDataFeed(adminClient, sqlServerConnectionString, sqlServerQuery);
  console.log(`Data feed created: ${created.id}`);
}

async function createDataFeed(adminClient, sqlServerConnectionString, sqlServerQuery) {
  console.log("Creating Datafeed...");
  const dataFeed = {
    name: "test_datafeed_" + new Date().getTime().toString(),
    source: {
      dataSourceType: "SqlServer",
      dataSourceParameter: {
        connectionString: sqlServerConnectionString,
        query: sqlServerQuery
      }
    },
    granularity: {
      granularityType: "Daily"
    },
    schema: {
      metrics: [
        {
          name: "revenue",
          displayName: "revenue",
          description: "Metric1 description"
        },
        {
          name: "cost",
          displayName: "cost",
          description: "Metric2 description"
        }
      ],
      dimensions: [
        { name: "city", displayName: "city display" },
        { name: "category", displayName: "category display" }
      ],
      timestampColumn: null
    },
    ingestionSettings: {
      ingestionStartTime: new Date(Date.UTC(2020, 5, 1)),
      ingestionStartOffsetInSeconds: 0,
      dataSourceRequestConcurrency: -1,
      ingestionRetryDelayInSeconds: -1,
      stopRetryAfterInSeconds: -1
    },
    rollupSettings: {
      rollupType: "AutoRollup",
      rollupMethod: "Sum",
      rollupIdentificationValue: "__CUSTOM_SUM__"
    },
    missingDataPointFillSettings: {
      fillType: "SmartFilling"
    },
    accessMode: "Private",
    adminEmails: ["xyz@example.com"]
  };
  const result = await adminClient.createDataFeed(dataFeed);

  return result;
}
```

## <a name="check-ingestion-status"></a>Alma durumunu denetle

Veri alımı başladıktan sonra, alma durumunu kontrol edebilirsiniz.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const dataFeedId = "<data feed id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  await checkIngestionStatus(
    adminClient,
    dataFeedId,
    new Date(Date.UTC(2020, 8, 1)),
    new Date(Date.UTC(2020, 8, 12))
  );
}

async function checkIngestionStatus(adminClient, datafeedId, startTime, endTime) {
  // This shows how to use for-await-of syntax to list status
  console.log("Checking ingestion status...");
  const iterator = adminClient.listDataFeedIngestionStatus(datafeedId, startTime, endTime);
  for await (const status of iterator) {
    console.log(`  [${status.timestamp}] ${status.status} - ${status.message}`);
  }
}
```

## <a name="configure-anomaly-detection"></a>Anomali algılamayı yapılandırma

Zaman serisinde bir noktanın bir anomali olup olmadığını saptamak için anomali algılama yapılandırmasına ihtiyacımız var. Varsayılan algılama yapılandırması her ölçüm için otomatik olarak uygulanırken, özelleştirilmiş bir anomali algılama yapılandırması oluşturarak verilerinizde kullanılan algılama modlarını ayarlayabilirsiniz.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const metricId =  "<metric id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);

  const detectionConfig = await configureAnomalyDetectionConfiguration(adminClient, metricId);
  console.log(`Detection configuration created: ${detectionConfig.id}`);
}

async function configureAnomalyDetectionConfiguration(adminClient, metricId) {
  console.log(`Creating an anomaly detection configuration on metric '${metricId}'...`);
  const detectionConfig = {
    name: "test_detection_configuration" + new Date().getTime().toString(),
    metricId,
    wholeSeriesDetectionCondition: {
      smartDetectionCondition: {
        sensitivity: 100,
        anomalyDetectorDirection: "Both",
        suppressCondition: {
          minNumber: 1,
          minRatio: 1
        }
      }
    },
    description: "Detection configuration description"
  };
  return await adminClient.createDetectionConfig(detectionConfig);
}
```

### <a name="create-a-hook"></a>Kanca oluşturma

Gerçek zamanlı Uyarılara abone olmak için kancalar kullanıyoruz. Bu örnekte, uyarının NAKLEDILECEĞI ölçüm Danışmanı hizmeti için bir Web kancası oluşturacağız.

```javascript

const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  const hook = await createWebhookHook(adminClient);
  console.log(`Webhook hook created: ${hook.id}`);
}

async function createWebhookHook(adminClient) {
  console.log("Creating a webhook hook");
  const hook = {
    hookType: "Webhook",
    name: "web hook " + new Date().getTime().toFixed(),
    description: "description",
    hookParameter: {
      endpoint: "https://example.com/handleAlerts", // you must enter a valid webhook url to post the alert payload
      username: "username",
      password: "password"
      // certificateKey: "certificate key",
      // certificatePassword: "certificate password"
    }
  };

  return await adminClient.createHook(hook);
}
```

##  <a name="create-an-alert-configuration"></a>Uyarı yapılandırması oluşturma

Bu örnek, bir uyarının tetiklenmesi gerektiğini ve bir uyarı için hedef olarak hangi kancaların kullanılacağını nasıl yapılandıracağınızı gösterir.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const detectionConfigId = "<detection id>";
  const hookId = "<hook id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  const alertConfig = await configureAlertConfiguration(adminClient, detectionConfigId, [hookId]);
  console.log(`Alert configuration created: ${alertConfig.id}`);
}

async function configureAlertConfiguration(adminClient, detectionConfigId, hookIds) {
  console.log("Creating a new alerting configuration...");
  const anomalyAlertConfig = {
    name: "test_alert_config_" + new Date().getTime().toString(),
    crossMetricsOperator: "AND",
    metricAlertConfigurations: [
      {
        detectionConfigurationId: detectionConfigId,
        alertScope: {
          scopeType: "All"
        },
        alertConditions: {
          severityCondition: { minAlertSeverity: "Medium", maxAlertSeverity: "High" }
        },
        snoozeCondition: {
          autoSnooze: 0,
          snoozeScope: "Metric",
          onlyForSuccessive: true
        }
      }
    ],
    hookIds,
    description: "Alerting config description"
  };
  return await adminClient.createAlertConfig(anomalyAlertConfig);
}
```

## <a name="query-the-alert"></a>Uyarıyı sorgulama

```javascript
const { MetricsAdvisorKeyCredential, MetricsAdvisorClient } = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const alertConfigId = "<alert config id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const client = new MetricsAdvisorClient(endpoint, credential);

  const alertIds = await queryAlerts(
    client,
    alertConfigId,
    new Date(Date.UTC(2020, 8, 1)),
    new Date(Date.UTC(2020, 8, 12))
  );

  if (alertIds.length > 1) {
    // query anomalies using an alert id.
    await queryAnomaliesByAlert(client, alertConfigId, alertIds[0]);
  } else {
    console.log("No alerts during the time period");
  }
}

async function queryAlerts(client, alertConfigId, startTime, endTime) {
  let alerts = [];
  const iterator = client.listAlerts(alertConfigId, startTime, endTime, "AnomalyTime");
  for await (const alert of iterator) {
    alerts.push(alert);
  }

  return alerts;
}

async function queryAnomaliesByAlert(client, alert) {
  console.log(
    `Listing anomalies for alert configuration '${alert.alertConfigId}' and alert '${alert.id}'`
  );
  const iterator = client.listAnomalies(alert);
  for await (const anomaly of iterator) {
    console.log(
      `  Anomaly ${anomaly.severity} ${anomaly.status} ${anomaly.seriesKey.dimension} ${anomaly.timestamp}`
    );
  }
}
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node index.js
```