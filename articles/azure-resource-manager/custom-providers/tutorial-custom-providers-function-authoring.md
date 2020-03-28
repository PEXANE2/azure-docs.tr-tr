---
title: Bir RESTful uç noktası yazma
description: Bu öğretici, özel sağlayıcılar için yeniden bir bitiş noktasının nasıl yazılabildiğini gösterir. Desteklenen RESTful HTTP yöntemleri için istek ve yanıtların nasıl işleyeceğini ayrıntıları.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: d7f6c51211ce0572797ade659b9316003502da1f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650026"
---
# <a name="author-a-restful-endpoint-for-custom-providers"></a>Özel sağlayıcılar için yeniden bir bitiş noktası yazma

Özel sağlayıcı, Azure ile bitiş noktası arasındaki bir sözleşmedir. Özel sağlayıcılarla Azure'da iş akışlarını özelleştirebilirsiniz. Bu öğretici, özel bir sağlayıcının yeniden yazma bitiş noktasının nasıl yazılabildiğini gösterir. Azure Özel Sağlayıcıları'nı bilmiyorsanız, [özel kaynak sağlayıcılarına genel bakışa](overview.md)bakın.

> [!NOTE]
> Bu öğretici, Azure [Özel Sağlayıcıları için Azure İşlevlerini Ayarlama](./tutorial-custom-providers-function-setup.md)öğreticisini temel alır. Bu öğreticideki adımlardan bazıları yalnızca özel sağlayıcılarla çalışacak bir Azure işlev uygulaması ayarlanmışsa çalışır.

## <a name="work-with-custom-actions-and-custom-resources"></a>Özel eylemler ve özel kaynaklarla çalışma

Bu öğreticide, özel sağlayıcınız için yeniden bir bitiş noktası olarak çalışacak şekilde işlev uygulamasını güncellersiniz. Azure'daki kaynaklar ve eylemler aşağıdaki temel RESTful belirtimi nden sonra modellenir:

- **PUT**: Yeni bir kaynak oluşturma
- **GET (örnek)**: Varolan bir kaynağı alma
- **DELETE**: Varolan bir kaynağı kaldırma
- **POST**: Bir eylemi tetikleme
- **GET (collection)**: Varolan tüm kaynakları listele

 Bu öğretici için Azure Tablo depolama alanını kullanırsınız. Ancak herhangi bir veritabanı veya depolama hizmeti çalışabilir.

## <a name="partition-custom-resources-in-storage"></a>Depolamada özel kaynakları bölme

Yeniden bir hizmet oluşturduğunuzdan, oluşturulan kaynakları depolamanız gerekir. Azure Tablo depolama için, verileriniz için bölüm ve satır anahtarları oluşturmanız gerekir. Özel sağlayıcılar için, verilerin özel sağlayıcıya bölünmesi gerekir. Gelen bir istek özel sağlayıcıya gönderildiğinde, özel `x-ms-customproviders-requestpath` sağlayıcı giden isteklere bitiş noktasına üstbilgi ekler.

Aşağıdaki örnek, `x-ms-customproviders-requestpath` özel bir kaynak için bir üstbilgi gösterir:

```
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{myResourceType}/{myResourceName}
```

Örneğin `x-ms-customproviders-requestpath` üstbilgisini temel alan, aşağıdaki tabloda gösterildiği gibi depolama alanınız için *partitionKey* ve *rowKey* parametrelerini oluşturabilirsiniz:

Parametre | Şablon | Açıklama
---|---|---
*partitionKey* | `{subscriptionId}:{resourceGroupName}:{resourceProviderName}` | *PartitionKey* parametresi verilerin nasıl bölümleniyi belirtir. Genellikle veriler özel sağlayıcı örneği tarafından bölümlenir.
*rowKey* | `{myResourceType}:{myResourceName}` | *rowKey* parametresi, verilerin tek tek tanımlayıcısını belirtir. Genellikle tanımlayıcı kaynağın adıdır.

Ayrıca, özel kaynağınızı modellemek için yeni bir sınıf oluşturmanız gerekir. Bu öğreticide, işlev uygulamanıza aşağıdaki **CustomResource** sınıfını eklersiniz:

```csharp
// Custom Resource Table Entity
public class CustomResource : TableEntity
{
    public string Data { get; set; }
}
```
**CustomResource,** herhangi bir giriş verisini kabul eden basit, genel bir sınıftır. Verileri depolamak için kullanılan **TableEntity'e**dayanır. **CustomResource** sınıfı **TableEntity'den**iki özellik devralır: **partitionKey** ve **rowKey**.

## <a name="support-custom-provider-restful-methods"></a>Özel sağlayıcı nın yeniden değerleme yöntemlerini destekleme

> [!NOTE]
> Kodu doğrudan bu öğreticiden kopyalamıyorsanız, yanıt içeriği üstbilgiyi `Content-Type` `application/json`'ye ayarlar geçerli Bir JSON olmalıdır.

Artık veri bölümleme ayarladığınız için, temel CRUD'yi oluşturun ve özel kaynaklar ve özel eylemler için tetikleme yöntemlerini oluşturun. Özel sağlayıcılar vekiller gibi davrandığından, RESTful bitiş noktasının isteği ve yanıtı modellemesi ve işlemesi gerekir. Aşağıdaki kod parçacıkları, temel RESTful işlemlerinin nasıl işleyeceğini gösterir.

### <a name="trigger-a-custom-action"></a>Özel bir eylemi tetikleme

Özel sağlayıcılar için, POST istekleri aracılığıyla özel bir eylem tetiklenir. Özel bir eylem isteğe bağlı olarak giriş parametreleri kümesi içeren bir istek gövdesi kabul edebilir. Eylem daha sonra, eylemin sonucunu ve başarılı olup olmadığını gösteren bir yanıt verir.

İşlev uygulamanıza aşağıdaki **TriggerCustomAction** yöntemini ekleyin:

```csharp
/// <summary>
/// Triggers a custom action with some side effects.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <returns>The HTTP response result of the custom action.</returns>
public static async Task<HttpResponseMessage> TriggerCustomAction(HttpRequestMessage requestMessage)
{
    var myCustomActionRequest = await requestMessage.Content.ReadAsStringAsync();

    var actionResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    actionResponse.Content = myCustomActionRequest != string.Empty ? 
        new StringContent(JObject.Parse(myCustomActionRequest).ToString(), System.Text.Encoding.UTF8, "application/json") :
        null;
    return actionResponse;
}
```

**TriggerCustomAction** yöntemi gelen bir isteği kabul eder ve yalnızca yanıtı bir durum koduyla geri yankılanır.

### <a name="create-a-custom-resource"></a>Özel kaynak oluşturma

Özel sağlayıcılar için PUT istekleri aracılığıyla özel bir kaynak oluşturulur. Özel sağlayıcı, özel kaynak için bir dizi özellik içeren bir JSON istek gövdesini kabul eder. Azure'daki kaynaklar, yeniden bir model izler. Bir kaynak oluşturmak, almak veya silmek için aynı istek URL'sini kullanabilirsiniz.

Yeni kaynaklar oluşturmak için aşağıdaki **CreateCustomResource** yöntemini ekleyin:

```csharp
/// <summary>
/// Creates a custom resource and saves it to table storage.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <param name="tableStorage">The Azure Table storage account.</param>
/// <param name="azureResourceId">The parsed Azure resource ID.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider ID.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The HTTP response containing the created custom resource.</returns>
public static async Task<HttpResponseMessage> CreateCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, ResourceId azureResourceId, string partitionKey, string rowKey)
{
    // Adds the Azure top-level properties.
    var myCustomResource = JObject.Parse(await requestMessage.Content.ReadAsStringAsync());
    myCustomResource["name"] = azureResourceId.Name;
    myCustomResource["type"] = azureResourceId.FullResourceType;
    myCustomResource["id"] = azureResourceId.Id;

    // Save the resource into storage.
    var insertOperation = TableOperation.InsertOrReplace(
        new CustomResource
        {
            PartitionKey = partitionKey,
            RowKey = rowKey,
            Data = myCustomResource.ToString(),
        });
    await tableStorage.ExecuteAsync(insertOperation);

    var createResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    createResponse.Content = new StringContent(myCustomResource.ToString(), System.Text.Encoding.UTF8, "application/json");
    return createResponse;
}
```

**CreateCustomResource** yöntemi, gelen isteği Azure'a özgü alanlar **id,** **ad**ve **türü**içerecek şekilde güncelleştirir. Bu alanlar, Azure genelindeki hizmetler tarafından kullanılan üst düzey özelliklerdir. Özel sağlayıcının Azure İlkesi, Azure Kaynak Yöneticisi Şablonları ve Azure Etkinlik Günlüğü gibi diğer hizmetlerle birlikte çalışmasına izin verirler.

Özellik | Örnek | Açıklama
---|---|---
**Adı** | {myCustomResourceName} | Özel kaynağın adı
**Türü** | Microsoft.CustomProviders/resourceProviders/{resourceTypeName} | Kaynak türü ad alanı
**Kimliği** | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>{resourceTypeName}/{myCustomResourceName} | Kaynak kimliği

Özellikleri eklemenin yanı sıra, JSON belgesini Azure Tablo depolama alanına da kaydettiniz.

### <a name="retrieve-a-custom-resource"></a>Özel bir kaynak alma

Özel sağlayıcılar için, GET istekleri aracılığıyla özel bir kaynak alınır. Özel bir sağlayıcı JSON istek gövdesini kabul *etmez.* GET istekleri için uç nokta, zaten oluşturulmuş kaynağı döndürmek için `x-ms-customproviders-requestpath` üstbilgi kullanır.

Varolan kaynakları almak için aşağıdaki **RetrieveCustomResource** yöntemini ekleyin:

```csharp
/// <summary>
/// Retrieves a custom resource.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <param name="tableStorage">The Azure Table storage account.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider ID.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The HTTP response containing the existing custom resource.</returns>
public static async Task<HttpResponseMessage> RetrieveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    var retrieveResponse = requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NotFound);

    retrieveResponse.Content = existingCustomResource != null ?
            new StringContent(existingCustomResource.Data, System.Text.Encoding.UTF8, "application/json"):
            null;
    return retrieveResponse;
}
```

Azure'da kaynaklar yeniden bir model izler. Kaynak oluşturan istek URL'si, GET isteği gerçekleştirilirse kaynağı da döndürür.

### <a name="remove-a-custom-resource"></a>Özel bir kaynağı kaldırma

Özel sağlayıcılar için DELETE istekleri aracılığıyla özel bir kaynak kaldırılır. Özel bir sağlayıcı JSON istek gövdesini kabul *etmez.* DELETE isteği için uç nokta, `x-ms-customproviders-requestpath` zaten oluşturulmuş kaynağı silmek için üstbilgi kullanır.

Varolan kaynakları kaldırmak için aşağıdaki **RemoveCustomResource** yöntemini ekleyin:

```csharp
/// <summary>
/// Removes an existing custom resource.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <param name="tableStorage">The Azure storage account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider ID.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The HTTP response containing the result of the deletion.</returns>
public static async Task<HttpResponseMessage> RemoveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    if (existingCustomResource != null) {
        var deleteOperation = TableOperation.Delete(existingCustomResource);
        await tableStorage.ExecuteAsync(deleteOperation);
    }

    return requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NoContent);
}
```

Azure'da kaynaklar yeniden bir model izler. Kaynak oluşturan istek URL'si, DELETE isteği gerçekleştirilirse kaynağı da siler.

### <a name="list-all-custom-resources"></a>Tüm özel kaynakları listele

Özel sağlayıcılar için, toplama GET isteklerini kullanarak varolan özel kaynakların listesini listeleyebilirsiniz. Özel bir sağlayıcı JSON istek gövdesini kabul *etmez.* GET istekleri koleksiyonu için uç nokta, `x-ms-customproviders-requestpath` zaten oluşturulmuş kaynakları sayısallandırmak için üstbilgi kullanır.

Varolan kaynakları sayısallandırmak için aşağıdaki **EnumerateAllCustomResources** yöntemini ekleyin:

```csharp
/// <summary>
/// Enumerates all the stored custom resources for a given type.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <param name="tableStorage">The Azure Table storage account.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider ID.</param>
/// <param name="resourceType">The resource type of the enumeration.</param>
/// <returns>The HTTP response containing a list of resources stored under 'value'.</returns>
public static async Task<HttpResponseMessage> EnumerateAllCustomResources(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string resourceType)
{
    // Generate upper bound of the query.
    var rowKeyUpperBound = new StringBuilder(resourceType);
    rowKeyUpperBound[rowKeyUpperBound.Length - 1]++;

    // Create the enumeration query.
    var enumerationQuery = new TableQuery<CustomResource>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, partitionKey),
            TableOperators.And,
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, resourceType),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, rowKeyUpperBound.ToString()))));
    
    var customResources = (await tableStorage.ExecuteQuerySegmentedAsync(enumerationQuery, null))
        .ToList().Select(customResource => JToken.Parse(customResource.Data));

    var enumerationResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    enumerationResponse.Content = new StringContent(new JObject(new JProperty("value", customResources)).ToString(), System.Text.Encoding.UTF8, "application/json");
    return enumerationResponse;
}
```

> [!NOTE]
> RowKey QueryComparisons.GreaterThan ve QueryComparisons.LessThan dizeleri için bir "startswith" sorgusu gerçekleştirmek için Azure Tablo depolama sözdizimidir.

Varolan tüm kaynakları listelemek için, özel sağlayıcınız bölümüaltında kaynakların bulunmasını sağlayan bir Azure Tablosu depolama sorgusu oluşturun. Sorgu daha sonra satır anahtarının aynı `{myResourceType}` değerle başlayıp başlamadığını denetler.

## <a name="integrate-restful-operations"></a>RESTful işlemleri tümleştir

Tüm RESTful yöntemleri işlev uygulamasına eklendikten sonra, farklı REST isteklerini işlemek için işlevleri çağıran ana **Çalıştır** yöntemini güncelleştirin:

```csharp
/// <summary>
/// Entry point for the Azure function app webhook that acts as the service behind a custom provider.
/// </summary>
/// <param name="requestMessage">The HTTP request message.</param>
/// <param name="log">The logger.</param>
/// <param name="tableStorage">The Azure Table storage account.</param>
/// <returns>The HTTP response for the custom Azure API.</returns>
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log, CloudTable tableStorage)
{
    // Get the unique Azure request path from request headers.
    var requestPath = req.Headers.GetValues("x-ms-customproviders-requestpath").FirstOrDefault();

    if (requestPath == null)
    {
        var missingHeaderResponse = req.CreateResponse(HttpStatusCode.BadRequest);
        missingHeaderResponse.Content = new StringContent(
            new JObject(new JProperty("error", "missing 'x-ms-customproviders-requestpath' header")).ToString(),
            System.Text.Encoding.UTF8, 
            "application/json");
    }

    log.LogInformation($"The Custom Provider Function received a request '{req.Method}' for resource '{requestPath}'.");

    // Determines if it is a collection level call or action.
    var isResourceRequest = requestPath.Split('/').Length % 2 == 1;
    var azureResourceId = isResourceRequest ? 
        ResourceId.FromString(requestPath) :
        ResourceId.FromString($"{requestPath}/");

    // Create the Partition Key and Row Key
    var partitionKey = $"{azureResourceId.SubscriptionId}:{azureResourceId.ResourceGroupName}:{azureResourceId.Parent.Name}";
    var rowKey = $"{azureResourceId.FullResourceType.Replace('/', ':')}:{azureResourceId.Name}";

    switch (req.Method)
    {
        // Action request for a custom action.
        case HttpMethod m when m == HttpMethod.Post && !isResourceRequest:
            return await TriggerCustomAction(
                requestMessage: req);

        // Enumerate request for all custom resources.
        case HttpMethod m when m == HttpMethod.Get && !isResourceRequest:
            return await EnumerateAllCustomResources(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                resourceType: rowKey);

        // Retrieve request for a custom resource.
        case HttpMethod m when m == HttpMethod.Get && isResourceRequest:
            return await RetrieveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Create request for a custom resource.
        case HttpMethod m when m == HttpMethod.Put && isResourceRequest:
            return await CreateCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                azureResourceId: azureResourceId,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Remove request for a custom resource.
        case HttpMethod m when m == HttpMethod.Delete && isResourceRequest:
            return await RemoveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Invalid request received.
        default:
            return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}
```

Güncelleştirilmiş **Çalıştır** yöntemi artık Azure Tablo depolama alanı için eklediğiniz *tableStorage* giriş bağlamasını içerir. Yöntemin ilk bölümü `x-ms-customproviders-requestpath` üstbilgi okur ve `Microsoft.Azure.Management.ResourceManager.Fluent` kaynak kimliği olarak değeri ayrıştırmak için kitaplığı kullanır. Üstbilgi `x-ms-customproviders-requestpath` özel sağlayıcı tarafından gönderilir ve gelen istek yolunu belirtir.

Ayrıştırılmış kaynak kimliğini kullanarak, verilerin görünmesi veya özel kaynakları depolaması için **partitionKey** ve **rowKey** değerlerini oluşturabilirsiniz.

Yöntemleri ve sınıfları ekledikten sonra, işlev uygulaması için **kullanılan** yöntemleri güncelleştirmeniz gerekir. C# dosyasının üst bölümüne aşağıdaki kodu ekleyin:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"
#r "../bin/Microsoft.Azure.Management.ResourceManager.Fluent.dll"

using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Configuration;
using System.Text;
using System.Threading;
using System.Globalization;
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Bu öğreticinin herhangi bir noktasında kaybolursanız, [özel sağlayıcı C# RESTful endpoint reference'da](./reference-custom-providers-csharp-endpoint.md)tam kod örneğini bulabilirsiniz. İşlev uygulamasını tamamladıktan sonra işlev uygulaması URL'sini kaydedin. Daha sonraki öğreticilerde işlev uygulamasını tetiklemek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Azure Özel Sağlayıcısı bitiş noktasıyla çalışmak için yeniden bir bitiş noktası yazmanız gerekiyor. Özel bir sağlayıcı oluşturmayı öğrenmek için, Tutorial makalesine [gidin: Özel bir sağlayıcı oluşturma.](./tutorial-custom-providers-create.md)
