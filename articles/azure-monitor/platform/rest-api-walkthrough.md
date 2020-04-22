---
title: Azure İzleme REST API izlenecek
description: Kullanılabilir metrik tanımları ve metrik değerleri almak için isteklerin kimliğini niçin doğrular ve Azure Monitor REST API'sini kullanma.
ms.subservice: metrics
ms.topic: conceptual
ms.date: 03/19/2018
ms.openlocfilehash: 6b0e321747e0f84be5a75ab96749311ff0071e8d
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687407"
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure İzleme REST API izlenecek

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, kodunuzu [Microsoft Azure Monitor REST API Başvuru](https://docs.microsoft.com/rest/api/monitor/)kullanabilirsiniz böylece kimlik doğrulama gerçekleştirmek için nasıl gösterir.

Azure Monitör API'si, kullanılabilir varsayılan metrik tanımları, parçalılığı ve metrik değerleri programlı bir şekilde almayı mümkün kılar. Veriler Azure SQL Veritabanı, Azure Cosmos DB veya Azure Veri Gölü gibi ayrı bir veri deposuna kaydedilebilir. Buradan gerektiğinde ek analizler yapılabilir.

Monitör API, çeşitli metrik veri noktalarıyla çalışmanın yanı sıra uyarı kurallarını listelemesini, etkinlik günlüklerini görüntülemesini ve çok daha fazlasını da mümkün kılar. Kullanılabilir işlemlerin tam listesi için [Microsoft Azure Monitor REST API Başvurusu'na](https://docs.microsoft.com/rest/api/monitor/)bakın.

## <a name="authenticating-azure-monitor-requests"></a>Azure Monitör isteklerini doğrulama

İlk adım, isteği doğrulamaktır.

Azure Monitor API'ye karşı yürütülen tüm görevler, Azure Kaynak Yöneticisi kimlik doğrulama modelini kullanır. Bu nedenle, tüm isteklerin Azure Etkin Dizin (Azure AD) ile doğrulanması gerekir. İstemci uygulamasının kimliğini doğrulamak için bir yaklaşım, bir Azure AD hizmet ilkesi oluşturmak ve kimlik doğrulama (JWT) belirteci almaktır. Aşağıdaki örnek komut dosyası, PowerShell üzerinden bir Azure AD hizmet ilkesi oluşturmayı gösterir. Daha ayrıntılı bir geçiş için, [kaynaklara erişmek için bir hizmet ilkesi oluşturmak için Azure PowerShell'i kullanma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)yla ilgili belgelere bakın. [Azure portalı üzerinden bir hizmet ilkesi oluşturmak](../../active-directory/develop/howto-create-service-principal-portal.md)da mümkündür.

```powershell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Connect-AzAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"
$secureStringPassword = ConvertTo-SecureString -String $pwd -AsPlainText -Force

# Create a new Azure AD application
$azureAdApplication = New-AzADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $secureStringPassword

# Create a new service principal associated with the designated application
New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Azure Monitor API'sini sorgulamak için istemci uygulamasının kimlik doğrulaması için önceden oluşturulmuş hizmet ilkesini kullanması gerekir. Aşağıdaki örnek PowerShell komut dosyası, JWT kimlik doğrulama belirteci elde etmek için [Active Directory Authentication Library](../../active-directory/azuread-dev/active-directory-authentication-libraries.md) (ADAL) kullanarak bir yaklaşım gösterir. JWT belirteci, Azure Monitor REST API'deki isteklerde HTTP Yetkilendirme parametresinin bir parçası olarak geçirilir.

```powershell
$azureAdApplication = Get-AzADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireTokenAsync("https://management.core.windows.net/", $cred).GetAwaiter().GetResult()

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Kimlik doğrulaması yaptıktan sonra, sorgular Azure Monitor REST API'si karşı yürütülebilir. İki yararlı sorgu vardır:

1. Kaynak için metrik tanımları listelama
2. Metrik değerleri alma

> [!NOTE]
> Azure REST API ile kimlik doğrulama hakkında daha fazla bilgi için lütfen [Azure REST API Başvurusu'na](https://docs.microsoft.com/rest/api/azure/)bakın.
>
>

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>Metrik Tanımları Al (Çok Boyutlu API)

Bir hizmet için kullanılabilen ölçümler listesine erişmek için [Azure Monitor Metrik tanımlarını REST API'yi](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) kullanın.

**Yöntem**: GET

**İstek URI**:\/\/https: https: management.azure.com/subscriptions/*{subscriptionId}*/resourceGroupsGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}*/*{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Örneğin, bir Azure Depolama hesabının metrik tanımlarını almak için istek aşağıdaki gibi görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```

> [!NOTE]
> Çok boyutlu Azure Monitor ölçümleri REST API'yi kullanarak metrik tanımları almak için API sürümü olarak "2018-01-01"i kullanın.
>
>

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır: (İkinci ölçümün boyutları olduğunu unutmayın)

```json
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "supportedAggregationTypes": [
                "Total",
                "Average",
                "Minimum",
                "Maximum"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "supportedAggregationTypes": [
                "Total"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT5M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT15M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT30M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ],
            "dimensions": [
                {
                    "value": "ResponseType",
                    "localizedValue": "Response type"
                },
                {
                    "value": "GeoType",
                    "localizedValue": "Geo type"
                },
                {
                    "value": "ApiName",
                    "localizedValue": "API name"
                }
            ]
        },
        ...
    ]
}
```

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>Boyut Değerlerini Al (Çok Boyutlu API)

Kullanılabilir metrik tanımları bilindikten sonra, boyutları olan bazı ölçümler olabilir. Metrik için sorgulamadan önce, bir boyutun değer aralığının ne olduğunu keşfetmek isteyebilirsiniz. Bu boyut değerlerine bağlı olarak, ölçümleri sorgularken ölçümleri boyut değerlerine göre filtrelemeyi veya segmente eklemeyi seçebilirsiniz.  Bunu başarmak için [Azure Monitor Ölçümleri REST API'yi](https://docs.microsoft.com/rest/api/monitor/metrics) kullanın.

Herhangi bir filtreleme isteği için ölçümün 'değer' ('yerelleştirilmiş Değer' değil) adını kullanın. Filtre belirtilmemişse, varsayılan metrik döndürülür. Bu API'nin kullanımı yalnızca bir boyutun joker karakter filtresine sahip olmasını sağlar.

> [!NOTE]
> Azure Monitor REST API'sini kullanarak boyut değerlerini almak için API sürümü olarak "2018-01-01"i kullanın.
>
>

**Yöntem**: GET

**İstek URI**\:: https //management.azure.com/subscriptions/*{abonelik-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?metricnames= {metric}&timespan=*{starttime/endtime}*&$filter=*{filtre}*&sonuçType=metadata&api-version=*{apiVersion}* *{metric}*

Örneğin, belirtilen zaman aralığında GeoType dimension = 'Primary' ifadesinin bulunduğu 'İşlemler' ölçümü için 'API Adı boyutu' için yayılan boyut değerlerinin listesini almak için istek aşağıdaki gibi olacaktır:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T00:00:00Z/2018-03-02T00:00:00Z&resultType=metadata&`$filter=${filter}&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır:

```json
{
  "timespan": "2018-03-01T00:00:00Z/2018-03-02T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "DeleteBlob"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "SetBlobProperties"
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Metrik Değerleri Al (Çok Boyutlu API)

Kullanılabilir metrik tanımlar ve olası boyut değerleri bilindikten sonra, ilgili metrik değerleri almak mümkündür.  Bunu başarmak için [Azure Monitor Ölçümleri REST API'yi](https://docs.microsoft.com/rest/api/monitor/metrics) kullanın.

Herhangi bir filtreleme isteği için ölçümün adı 'değer' ('yerelleştirilmiş Değer' değil) kullanın. Boyut filtreleri belirtilmemişse, yuvarlatılmış toplanan metrik döndürülür. Bir metrik sorgu birden çok zaman serisini döndürürse, sınırlı sıralı zaman serileri listesini döndürmek için 'Üst' ve 'OrderBy' sorgu parametrelerini kullanabilirsiniz.

> [!NOTE]
> Azure Monitor REST API'sini kullanarak çok boyutlu metrik değerleri almak için API sürümü olarak "2018-01-01"i kullanın.
>
>

**Yöntem**: GET

**İstek URI**:\/https: management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?metricnames= {metric}&timespan=*{starttime}&}*&$filter=*{filter}&* interval=*{timeGrain}*&toplama=*aggreation}* apiversion/versioni/&/*{metric}* *{apiVersion}*

Örneğin, GeotType'ın 'Birincil' olduğu 5 dk aralığındaki 'Hareketler' sayısına göre azalan değerdeki ilk 3 API'yi almak için istek aşağıdaki gibi olacaktır:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T02:00:00Z/2018-03-01T02:05:00Z&`$filter=${filter}&interval=PT1M&aggregation=Total&top=3&orderby=Total desc&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
    -Verbose
```

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır:

```json
{
  "cost": 0,
  "timespan": "2018-03-01T02:00:00Z/2018-03-01T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "GetBlobProperties"
            }
          ],
          "data": [
            {
              "timeStamp": "2017-09-19T02:00:00Z",
              "total": 2
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "total": 1
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "total": 3
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "total": 7
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "total": 2
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-definitions"></a>Metrik tanımları alma

Bir hizmet için kullanılabilen ölçümler listesine erişmek için [Azure Monitor Metrik tanımlarını REST API'yi](https://msdn.microsoft.com/library/mt743621.aspx) kullanın.

**Yöntem**: GET

**İstek URI**:\/\/https: https: management.azure.com/subscriptions/*{subscriptionId}*/resourceGroupsGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}*/*{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Örneğin, bir Azure Logic App'in metrik tanımlarını almak için istek aşağıdaki gibi görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosotweets-metricdef-results.json" `
                  -Verbose
```

> [!NOTE]
> Azure Monitor REST API'sini kullanarak metrik tanımları almak için API sürümü olarak "2016-03-01"i kullanın.
>
>

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır:

```json
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions",
  "value": [
    {
      "name": {
        "value": "RunsStarted",
        "localizedValue": "Runs Started"
      },
      "category": "AllMetrics",
      "startTime": "0001-01-01T00:00:00Z",
      "endTime": "0001-01-01T00:00:00Z",
      "unit": "Count",
      "primaryAggregationType": "Total",
      "resourceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        }
      ],
      "properties": null,
      "dimensions": null,
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions/RunsStarted",
      "supportedAggregationTypes": [ "None", "Average", "Minimum", "Maximum", "Total", "Count" ]
    }
  ]
}
```

Daha fazla bilgi için, [Azure Monitor REST API belgelerinde bir kaynağın metrik tanımlarını listele'ye](https://msdn.microsoft.com/library/azure/mt743621.aspx) bakın.

## <a name="retrieve-metric-values"></a>Metrik değerleri alma

Kullanılabilir metrik tanımları bilindikten sonra, ilgili metrik değerleri almak mümkündür. Herhangi bir filtreleme isteği için ölçümün 'değer' ('yerelleştirilmiş Değer' değil) adını kullanın (örneğin, 'CpuTime' ve 'İstekler' metrik veri noktalarını alın). Filtre belirtilmemişse, varsayılan metrik döndürülür.

> [!NOTE]
> Azure Monitor REST API'sini kullanarak metrik değerleri almak için API sürümü olarak "2016-09-01"i kullanın.
>
>

**Yöntem**: GET

**İstek URI**:`https:\//management.azure.com/subscriptions/\*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*`

Örneğin, verilen zaman aralığı için RunsSucceeded metrik veri noktalarını almak ve 1 saatlik bir zaman dilimi için istek aşağıdaki gibi olacaktır:

```powershell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-results.json" `
    -Verbose
```

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır:

```json
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T19:00:00Z",
          "total": 0.0
        },
        {
          "timeStamp": "2017-08-18T20:00:00Z",
          "total": 159.0
        },
        {
          "timeStamp": "2017-08-18T21:00:00Z",
          "total": 174.0
        },
        {
          "timeStamp": "2017-08-18T22:00:00Z",
          "total": 97.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

Birden çok veri veya toplama noktası almak için, aşağıdaki örnekte görüldüğü gibi, filtreye metrik tanım adlarını ve toplama türlerini ekleyin:

```powershell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-multiple-results.json" `
    -Verbose
```

Ortaya çıkan JSON yanıt gövdesi aşağıdaki örneğe benzer olacaktır:

```json
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/ActionsCompleted",
      "name": {
        "value": "ActionsCompleted",
        "localizedValue": "Actions Completed "
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

### <a name="use-armclient"></a>ARMClient kullanın

Ek bir yaklaşım, Windows makinenizde [ARMClient](https://github.com/projectkudu/armclient) kullanmaktır. ARMClient, Azure AD kimlik doğrulamasını (ve ortaya çıkan JWT belirteci) otomatik olarak işler. Aşağıdaki adımlar, metrik verileri almak için ARMClient kullanımını ana hatlarını verebedilir:

1. [Chocolatey](https://chocolatey.org/) ve [ARMClient](https://github.com/projectkudu/armclient)yükleyin.
2. Terminal penceresinde *armclient.exe giriş*yazın. Bunu yapmak, Azure'da oturum açmanızı ister.
3. *Armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01* yazın
4. *Armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01* yazın

Örneğin, belirli bir Mantık Uygulamasının metrik tanımlarını almak için aşağıdaki komutu verebilmektedir:

```console
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```

## <a name="retrieve-the-resource-id"></a>Kaynak kimliğini alma

REST API'yi kullanmak, kullanılabilir metrik tanımları, parçalılığı ve ilgili değerleri anlamada gerçekten yardımcı olabilir. Bu bilgiler Azure [Yönetim Kitaplığı'nı](https://msdn.microsoft.com/library/azure/mt417623.aspx)kullanırken yararlıdır.

Önceki kod için kullanılacak kaynak kimliği, istenen Azure kaynağına giden tam yoldur. Örneğin, bir Azure Web Uygulamasına karşı sorgulama yapmak için kaynak kimliği şu şekilde olacaktır:

*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*

Aşağıdaki liste, çeşitli Azure kaynakları için kaynak kimliği biçimlerine birkaç örnek içerir:

* **IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastik SQL Havuzu** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL Veritabanı (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*
* **Hizmet Veri Servisi** - /abonelikler/*{subscription-id}*/resourceGroups/*{kaynak grubu adı}*/sağlayıcılar/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*
* **Sanal makine ölçek kümeleri** - /abonelikler/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*
* **VM'ler** - /abonelikler/*{subscription-id}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **Olay Hub'ları** - /abonelikler/*{abonelik-id}*/resourceGroups/*{kaynak grubu adı}*/sağlayıcılar/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Azure Kaynak Gezgini'ni kullanmak, Azure portalında istenen kaynağı görüntülemek ve PowerShell veya Azure CLI üzerinden kaynak kimliğini almak için alternatif yaklaşımlar vardır.

### <a name="azure-resource-explorer"></a>Azure Resource Manager

İstenilen kaynak için kaynak kimliğini bulmak için yararlı bir yaklaşım, [Azure Kaynak Gezgini](https://resources.azure.com) aracını kullanmaktır. İstenilen kaynağa gidin ve aşağıdaki ekran görüntüsünde olduğu gibi gösterilen kıta bakın:

![Alt "Azure Kaynak Gezgini"](./media/rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portal

Kaynak kimliği Azure portalından da elde edilebilir. Bunu yapmak için, istenen kaynağa gidin ve ardından Özellikler'i seçin. Kaynak Kimliği, aşağıdaki ekran görüntüsünde görüldüğü gibi Özellikler bölümünde görüntülenir:

![Alt "Azure portalındaki Özellikler bıtır'da görüntülenen kaynak kimliği"](./media/rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell

Kaynak kimliği Azure PowerShell cmdlets kullanılarak da alınabilir. Örneğin, bir Azure Logic App'in kaynak kimliğini elde etmek için aşağıdaki örnekte olduğu gibi Get-AzureLogicApp cmdlet'ini çalıştırın:

```powershell
Get-AzLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Sonuç aşağıdaki örneğe benzer olmalıdır:

```output
Id             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets
Name           : ContosoTweets
Type           : Microsoft.Logic/workflows
Location       : centralus
ChangedTime    : 8/21/2017 6:58:57 PM
CreatedTime    : 8/18/2017 7:54:21 PM
AccessEndpoint : https://prod-08.centralus.logic.azure.com:443/workflows/f3a91b352fcc47e6bff989b85446c5db
State          : Enabled
Definition     : {$schema, contentVersion, parameters, triggers...}
Parameters     : {[$connections, Microsoft.Azure.Management.Logic.Models.WorkflowParameter]}
SkuName        :
AppServicePlan :
PlanType       :
PlanId         :
Version        : 08586982649483762729
```

### <a name="azure-cli"></a>Azure CLI

Azure CLI'yi kullanarak bir Azure Depolama hesabının `az storage account show` kaynak kimliğini almak için aşağıdaki örnekte gösterildiği gibi komutu uygulayın:

```azurecli
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Sonuç aşağıdaki örneğe benzer olmalıdır:

```json
{
  "accessTier": null,
  "creationTime": "2017-08-18T19:58:41.840552+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/contosotweets2017",
  "identity": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "centralus",
  "name": "contosotweets2017",
  "networkAcls": null,
  "primaryEndpoints": {
    "blob": "https://contosotweets2017.blob.core.windows.net/",
    "file": "https://contosotweets2017.file.core.windows.net/",
    "queue": "https://contosotweets2017.queue.core.windows.net/",
    "table": "https://contosotweets2017.table.core.windows.net/"
  },
  "primaryLocation": "centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "azmon-rest-api-walkthrough",
  "secondaryEndpoints": null,
  "secondaryLocation": "eastus2",
  "sku": {
    "name": "Standard_GRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

> [!NOTE]
> Azure Logic Apps henüz Azure CLI üzerinden kullanılamadığından, önceki örnekte bir Azure Depolama hesabı gösterilir.
>
>

## <a name="retrieve-activity-log-data"></a>Etkinlik günlüğü verilerini alma

Metrik tanımlara ve ilgili değerlere ek olarak, Azure kaynaklarıyla ilgili ek ilginç bilgiler almak için Azure Monitor REST API'sini kullanmak da mümkündür. Örnek olarak, etkinlik günlüğü verilerini sorgulamak [mümkündür.](https://msdn.microsoft.com/library/azure/dn931934.aspx) Aşağıdaki örnek, bir Azure aboneliği için belirli bir tarih aralığındaki etkinlik günlüğü verilerini sorgulamak için Azure Monitor REST API'sini kullandığını gösterir:

```powershell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar

* İzlemeye Genel Bakış'ı gözden [geçirin.](../../azure-monitor/overview.md)
* Desteklenen [ölçümleri Azure Monitor ile](metrics-supported.md)görüntüleyin.
* Microsoft [Azure Monitor REST API Başvuru'yu](https://msdn.microsoft.com/library/azure/dn931943.aspx)gözden geçirin.
* Azure [Yönetim Kitaplığını](https://msdn.microsoft.com/library/azure/mt417623.aspx)gözden geçirin.
