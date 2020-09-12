---
title: Azure Digital Twins API’lerini ve SDK’larını kullanma
titleSuffix: Azure Digital Twins
description: Bkz. SDK ile birlikte Azure dijital TWINS API 'Leri ile çalışma.
author: baanders
ms.author: baanders
ms.date: 06/04/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f61773e693ad1276ef48b2f26ef6e3e8f59e2992
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89612376"
---
# <a name="use-the-azure-digital-twins-apis-and-sdks"></a>Azure Digital Twins API’lerini ve SDK’larını kullanma

Azure dijital TWINS, örneğinizi ve onun öğelerini yönetmek için hem **Denetim düzlemi API 'leri** hem de **veri düzlemi API 'leri** ile birlikte sunulur. Bu makale, kullanılabilir API 'Lere ve bunlarla etkileşime yönelik yöntemlere genel bir bakış sunar. REST API 'Leri doğrudan ilişkili Swaggers ile veya bir SDK aracılığıyla kullanabilirsiniz.

## <a name="overview-control-plane-apis"></a>Genel Bakış: denetim düzlemi API 'Leri

Denetim düzlemi API 'Leri, Azure dijital TWINS örneğinizi bir bütün olarak yönetmek için kullanılır, bu nedenle tüm örneğinizi oluşturma veya silme gibi işlemleri kapsar. Bunları, uç noktaları oluşturmak ve silmek için de kullanacaksınız.

Genel önizleme için en güncel Denetim düzlemi API sürümü _**2020-03-01-Preview**_' dır.

Denetim düzlemi API 'Lerini kullanmak için:
* En son [Swagger klasörüne](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins)başvurarak API 'leri doğrudan çağırabilirsiniz. Bu depo ayrıca kullanımı gösteren örneklerin bir klasörünü de içerir.
* Şu anda, içindeki denetim API 'Leri için SDK 'Lara erişebilirsiniz...
  - [.Net (C#)](https://www.nuget.org/packages/Microsoft.Azure.Management.DigitalTwins/1.0.0-preview.1) ([kaynak](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Microsoft.Azure.Management.DigitalTwins)) ([başvuru [otomatik olarak oluşturulan]](https://docs.microsoft.com/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet-preview&preserve-view=true))
  - [Java](https://search.maven.org/artifact/com.microsoft.azure.digitaltwins.v2020_03_01_preview/azure-mgmt-digitaltwins) ([kaynak](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/digitaltwins)) ([başvuru [otomatik olarak oluşturulan]](https://docs.microsoft.com/java/api/overview/azure/digitaltwins/management?view=azure-java-preview&preserve-view=true))
  - [JavaScript](https://www.npmjs.com/package/@azure/arm-digitaltwins) ([kaynak](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/digitaltwins/arm-digitaltwins))
  - [Python](https://pypi.org/project/azure-mgmt-digitaltwins/) ([kaynak](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/digitaltwins/azure-mgmt-digitaltwins))
  - [Kaynak ol](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/digitaltwins/mgmt/2020-03-01-preview/digitaltwins)

Ayrıca, [Azure Portal](https://portal.azure.com) ve [CLI](how-to-use-cli.md)aracılığıyla Azure dijital TWINS ile etkileşime girerek denetim düzlemi API 'leri uygulayabilirsiniz.

## <a name="overview-data-plane-apis"></a>Genel Bakış: veri düzlemi API 'Leri

Veri düzlemi API 'Leri, Azure dijital TWINS örneğiniz içindeki öğeleri yönetmek için kullanılır. Yollar oluşturma, model yükleme, ilişki oluşturma ve TWINS yönetimi gibi işlemleri içerirler. Bunlar, aşağıdaki kategorilere büyük ölçüde ayrılabilir:
* **Digitaltwınsmodelleriyle** -digitaltwınsmodel kategorisi, bir Azure dijital TWINS örneğindeki [modelleri](concepts-models.md) yönetmek için API 'ler içerir. Yönetim etkinlikleri, DTDL 'de yazılan modellerin karşıya yükleme, doğrulama, alma ve silme işlemlerini içerir.
* **Digitaltwins** -digitaltwıns kategorisi, geliştiricilerin bir Azure dijital TWINS örneğinde bulunan [dijital TWINS](concepts-twins-graph.md) ve ilişkilerini oluşturmalarına, değiştirmesine ve silmesine izin veren API 'leri içerir.
* **Sorgu** -sorgu kategorisi, geliştiricilerin ilişkiler genelinde [ikizi grafiğinde dijital TWINS kümeleri bulmasına](how-to-query-graph.md) olanak tanır.
* **Eventroutes** -eventroutes kategorisi, verileri sistem ve aşağı akış Hizmetleri aracılığıyla [yönlendiren](concepts-route-events.md)API 'leri içerir.

Genel önizleme için en güncel veri düzlemi API sürümü _**2020-05-31-Preview**_' dir. Veri düzlemi işlemleri için _2020-03-01-Preview_ API sürümü artık kullanım dışıdır.

Veri düzlemi API 'Lerini kullanmak için:
* API 'Leri doğrudan çağırabilirsiniz...
   - en son [Swagger klasörüne](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins)başvuruluyor. Bu depo ayrıca kullanımı gösteren örneklerin bir klasörünü de içerir. 
   - [API başvuru belgelerini](https://docs.microsoft.com/rest/api/azure-digitaltwins/)görüntüleme.
* **.Net (C#)** SDK 'sını kullanabilirsiniz. .NET SDK 'Yı kullanmak için...
   - paketi NuGet: [Azure. DigitalTwins. Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core)' dan görüntüleyebilir ve ekleyebilirsiniz. 
   - örnek bir klasör içeren SDK kaynağını GitHub 'da bulabilirsiniz: [.net Için Azure IoT dijital TWINS istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core). 
   - [SDK başvuru belgelerini](https://docs.microsoft.com/dotnet/api/overview/azure/digitaltwins?view=azure-dotnet-preview&preserve-view=true)görüntüleyebilirsiniz.
   - Bu makalenin [.net (C#) SDK (veri düzlemi)](#net-c-sdk-data-plane) bölümüne devam ederek ayrıntılı bilgi ve kullanım örnekleri görebilirsiniz.
* **JavaScript** SDK 'sını kullanabilirsiniz. JavaScript SDK 'sını kullanmak için...
   - paketi NPM 'den görüntüleyebilir ve yükleyebilirsiniz: [JavaScript Için Azure Azure dijital TWINS istemci kitaplığı](https://www.npmjs.com/package/@azure/digital-twins/v/1.0.0-preview.1)
* Diğer bir dil için, oto Rest kullanarak bir SDK oluşturabilirsiniz. [*Nasıl yapılır: Azure dijital TWINS için özel SDK 'Lar oluşturma*](how-to-create-custom-sdks.md)bölümündeki yönergeleri Izleyerek oto Rest kullanın.

[CLI](how-to-use-cli.md)aracılığıyla Azure dijital TWINS ile etkileşime girerek Tarih düzlemi API 'leri de uygulayabilirsiniz.

## <a name="net-c-sdk-data-plane"></a>.NET (C#) SDK (veri düzlemi)

Azure Digital Twins .NET (C#) SDK 'Sı, .NET için Azure SDK 'sının bir parçasıdır. Açık kaynaktır ve Azure dijital TWINS veri düzlemi API 'Lerini temel alır.

> [!NOTE]
> SDK tasarımı hakkında daha fazla bilgi için bkz. [Azure SDK 'ları için genel tasarım ilkeleri](https://azure.github.io/azure-sdk/general_introduction.html) ve belirli [.net tasarım yönergeleri](https://azure.github.io/azure-sdk/dotnet_introduction.html).

SDK 'yı kullanmak için, projenize **Azure. DigitalTwins. Core** NuGet paketini ekleyin. Ayrıca, **Azure. Identity** paketinin en son sürümüne de ihtiyacınız olacaktır.

* Visual Studio 'da, NuGet Paket Yöneticisi ( *araçlar > NuGet Paket Yöneticisi aracılığıyla erişilir > çözüm Için NuGet Paketlerini Yönet*) ile paket ekleyebilirsiniz. 
* .NET komut satırı aracını kullanarak şunları gerçekleştirebilirsiniz:

    ```cmd/sh
    dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
    dotnet add package Azure.identity
    ```

API 'Leri uygulamada kullanma hakkında ayrıntılı bilgi için bkz. [*öğretici: istemci uygulaması kodu*](tutorial-code.md). 

### <a name="net-sdk-usage-examples"></a>.NET SDK kullanım örnekleri

.NET SDK 'nın kullanımını gösteren bazı kod örnekleri aşağıda verilmiştir.

Hizmette kimlik doğrulaması yapın:

```csharp
// Authenticate against the service and create a client
var credentials = new InteractiveBrowserCredential(tenantId, clientId);
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
```

Model ve liste modellerini karşıya yükleyin:

```csharp
// Upload a model
var typeList = new List<string>();
string dtdl = File.ReadAllText("SampleModel.json");
typeList.Add(dtdl);
try {
    await client.CreateModelsAsync(typeList);
} catch (RequestFailedException rex) {
    Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
}
// Read a list of models back from the service
AsyncPageable<ModelData> modelDataList = client.GetModelsAsync();
await foreach (ModelData md in modelDataList)
{
    Console.WriteLine($"Type name: {md.DisplayName}: {md.Id}");
}
```

TWINS oluşturma ve sorgulama:

```csharp
// Initialize twin metadata
BasicDigitalTwin twinData = new BasicDigitalTwin();

twinData.Id = $"firstTwin";
twinData.Metadata.ModelId = "dtmi:com:contoso:SampleModel;1";
twinData.CustomProperties.Add("data", "Hello World!");
try {
    await client.CreateDigitalTwinAsync("firstTwin", JsonSerializer.Serialize(twinData));
} catch(RequestFailedException rex) {
    Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
}
 
// Run a query    
AsyncPageable<string> result = client.QueryAsync("Select * From DigitalTwins");
await foreach (string twin in result)
{
    // Use JSON deserialization to pretty-print
    object jsonObj = JsonSerializer.Deserialize<object>(twin);
    string prettyTwin = JsonSerializer.Serialize(jsonObj, new JsonSerializerOptions { WriteIndented = true });
    Console.WriteLine(prettyTwin);
    // Or use BasicDigitalTwin for convenient property access
    BasicDigitalTwin btwin = JsonSerializer.Deserialize<BasicDigitalTwin>(twin);
}
```

Bu örnek uygulama kodundan izlenecek yol için [*öğreticiye bakın: istemci uygulamasını kodlayın*](tutorial-code.md) . 

[.Net (C#) SDK 'sı Için GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core/samples)deposunda ek örnekler de bulabilirsiniz.

#### <a name="serialization-helpers"></a>Serileştirme yardımcıları

Serileştirme yardımcıları, temel bilgilere erişim için hızlı bir şekilde ikizi verileri oluşturmak veya seri durumdan çıkarmak amacıyla SDK içinde kullanılabilen yardımcı işlevlerdir. Core SDK yöntemleri varsayılan olarak JSON olarak ikizi verisi döndürdüğü için, ikizi verilerini daha fazla bölmek üzere bu yardımcı sınıfları kullanmak yararlı olabilir.

Kullanılabilir yardımcı sınıfları şunlardır:
* `BasicDigitalTwin`: Bir Digital ikizi 'ın temel verilerini temsil eder
* `BasicRelationship`: Bir ilişkinin temel verilerini temsil eder
* `UpdateOperationUtility`: Güncelleştirme çağrılarında kullanılan JSON Patch bilgilerini temsil eder
* `WriteableProperty`: Özellik meta verilerini temsil eder

##### <a name="deserialize-a-digital-twin"></a>Dijital ikizi serisini kaldırma

Ya da gibi tercih ettiğiniz JSON kitaplığını kullanarak ikizi verilerinin serisini her zaman kaldırabilirsiniz `System.Test.Json` `Newtonsoft.Json` . Bir ikizi temel erişimi için yardımcı sınıflar bunu biraz daha uygun hale getirir.

```csharp
Response<string> res = client.GetDigitalTwin(twin_id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
```

`BasicDigitalTwin`Yardımcı sınıfı ayrıca, ile ikizi üzerinde tanımlanan özelliklere erişmenizi sağlar `Dictionary<string, object>` . İkizi özelliklerini listelemek için şunu kullanabilirsiniz:

```csharp
Response<string> res = client.GetDigitalTwin(twin_id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

##### <a name="create-a-digital-twin"></a>Dijital ikizi oluşturma

Sınıfını kullanarak `BasicDigitalTwin` , bir ikizi örneği oluşturmak için veri hazırlayacaksınız:

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
twin.CustomProperties = props;

client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<BasicDigitalTwin>(twin));
```

Yukarıdaki kod, aşağıdaki "el ile" varyanta eşdeğerdir:

```csharp
Dictionary<string, object> meta = new Dictionary<string, object>()
{
    { "$model", "dtmi:example:Room;1"}
};
Dictionary<string, object> twin = new Dictionary<string, object>()
{
    { "$metadata", meta },
    { "Temperature", 25.0 }
};
client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<Dictionary<string, object>>(twin));
```

##### <a name="deserialize-a-relationship"></a>İlişki serisini kaldırma

Her zaman, veya gibi tercih ettiğiniz JSON kitaplığını kullanarak ilişki verilerinin serisini kaldırabilirsiniz `System.Test.Json` `Newtonsoft.Json` . Bir ilişkiye temel erişim için yardımcı sınıflar bunu biraz daha uygun hale getirir.

```csharp
Response<string> res = client.GetRelationship(twin_id, rel_id);
BasicRelationship rel = JsonSerializer.Deserialize<BasicRelationship>(res.Value);
Console.WriteLine($"Relationship Name: {rel.Name}");
```

`BasicRelationship`Yardımcı sınıfı ayrıca, bir ile ilişkisi üzerinde tanımlanan özelliklere erişmenizi sağlar `Dictionary<string, object>` . Özellikleri listelemek için şunları kullanabilirsiniz:

```csharp
Response<string> res = client.GetRelationship(twin_id, rel_id);
BasicRelationship rel = JsonSerializer.Deserialize<BasicRelationship>(res.Value);
Console.WriteLine($"Relationship Name: {rel.Name}");
foreach (string prop in rel.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

##### <a name="create-a-relationship"></a>Bir ilişki oluşturma

Sınıfını kullanarak `BasicRelationship` , bir ikizi örneğinde ilişki oluşturmaya yönelik verileri de hazırlayacaksınız:

```csharp
BasicRelationship rel = new BasicRelationship();
rel.TargetId = "myTargetTwin";
rel.Name = "contains"; // a relationship with this name must be defined in the model
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("active", true);
rel.CustomProperties = props;
client.CreateRelationship("mySourceTwin", "rel001", JsonSerializer.Serialize<BasicRelationship>(rel));
```

##### <a name="create-a-patch-for-twin-update"></a>İkizi güncelleştirmesi için bir düzeltme eki oluşturma

TWINS ve ilişkiler için yapılan çağrıları güncelleştirme, [JSON Patch](http://jsonpatch.com/) yapısını kullanır. JSON yama işlemleri listesi oluşturmak için, `UpdateOperationsUtility` sınıfını aşağıda gösterildiği gibi kullanabilirsiniz.

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", 25.0);
uou.AppendAddOp("/myComponent/Property", "Hello");
// Un-set a property
uou.AppendRemoveOp("/Humidity");
client.UpdateDigitalTwin("myTwin", uou.Serialize());
```

## <a name="general-apisdk-usage-notes"></a>Genel API/SDK kullanım notları

> [!NOTE]
> Önizleme sırasında Azure Digital TWINS 'in, **çıkış noktaları arası kaynak paylaşımını (CORS)** desteklemediğini lütfen unutmayın. Sonuç olarak, bir tarayıcı uygulamasından, bir [API Management (APıM)](../api-management/api-management-key-concepts.md) arabiriminden veya bir [Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview) bağlayıcısından REST API arıyorsanız bir ilke hatası görebilirsiniz.
> Bu hatayı çözmek için aşağıdakilerden birini yapabilirsiniz:
> * İletibir bilgisayardan CORS üst bilgisini şerit `Access-Control-Allow-Origin` . Bu üstbilgi yanıtın paylaşılıp paylaşılamayacağını gösterir. 
> * Alternatif olarak, bir CORS proxy 'si oluşturun ve Azure dijital TWINS REST API isteği üzerinden isteyin. 

Aşağıdaki listede, API 'Leri ve SDK 'ları kullanmaya yönelik ek ayrıntılar ve genel yönergeler verilmiştir.

* SDK 'yı kullanmak için, sınıfın örneğini oluşturun `DigitalTwinsClient` . Oluşturucu, paketteki çeşitli kimlik doğrulama yöntemleriyle elde edilebilir kimlik bilgileri gerektirir `Azure.Identity` . Daha fazla bilgi için `Azure.Identity` bkz. [ad alanı belgeleri](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet&preserve-view=true). 
* Başlarken faydalı olduğunu fark edebilirsiniz `InteractiveBrowserCredential` , ancak [yönetilen kimliğin](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet&preserve-view=true)kimlik bilgileri de dahil olmak üzere, Azure Digital TWINS ['e karşı MSI ile ayarlanan Azure işlevleri](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet) kimlik doğrulaması için kullanacağınız diğer birkaç seçenek vardır. Hakkında daha fazla bilgi için `InteractiveBrowserCredential` bkz. [sınıf belgeleri](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet&preserve-view=true).
* Tüm hizmet API çağrıları, sınıf üzerinde üye işlevleri olarak gösterilir `DigitalTwinsClient` .
* Tüm hizmet işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde bulunur.
* Tüm hizmet işlevleri, 400 veya üzeri bir dönüş durumu için bir özel durum oluşturur. Çağrıları bir bölüme sardığınızdan `try` ve en azından yakaladığınızdan emin olun `RequestFailedExceptions` . Bu tür özel durum hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/dotnet/api/azure.requestfailedexception?view=azure-dotnet&preserve-view=true)bakın.
* Çoğu hizmet yöntemi döndürür `Response<T>` veya ( `Task<Response<T>>` zaman uyumsuz çağrılar için), burada `T` hizmet çağrısının dönüş nesnesinin sınıfıdır. [`Response`](https://docs.microsoft.com/dotnet/api/azure.response-1?view=azure-dotnet&preserve-view=true)Sınıfı, hizmet döndürmesini kapsüller ve dönüş değerlerini kendi `Value` alanına sunar.  
* Disk belleğine alınmış sonuçları olan hizmet yöntemleri, `Pageable<T>` veya `AsyncPageable<T>` sonuçlarını döndürür. Sınıfı hakkında daha fazla bilgi için buraya bakın `Pageable<T>` . hakkında daha fazla bilgi için [here](https://docs.microsoft.com/dotnet/api/azure.pageable-1?view=azure-dotnet-preview&preserve-view=true) `AsyncPageable<T>` [buraya](https://docs.microsoft.com/dotnet/api/azure.asyncpageable-1?view=azure-dotnet-preview&preserve-view=true)bakın.
* Bir döngüsü kullanarak, disk belleğine alınmış sonuçları yineleyebilirsiniz `await foreach` . Bu süreç hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8)bakın.
* Temel alınan SDK `Azure.Core` . SDK altyapısı ve türleri hakkında başvuru için bkz. [Azure ad alanı belgeleri](https://docs.microsoft.com/dotnet/api/azure?view=azure-dotnet-preview&preserve-view=true) .

Hizmet yöntemleri mümkün olduğunda kesin türü belirtilmiş nesneler döndürür. Ancak, Azure dijital TWINS, çalışma zamanında Kullanıcı tarafından özel olarak yapılandırılmış modelleri temel aldığı için (hizmete yüklenen DTDL modelleri aracılığıyla), çoğu hizmet API 'Leri JSON biçiminde ikizi verileri alır ve döndürür.

## <a name="monitor-api-metrics"></a>API ölçümlerini izleme

İstekler, gecikme ve başarısızlık oranı gibi API ölçümleri [Azure Portal](https://portal.azure.com/)görüntülenebilir. 

Portal giriş sayfasında, ayrıntılarını almak için Azure dijital TWINS örneğinizi arayın. *Ölçümler* sayfasını açmak Için Azure Digital TWINS örneğinin menüsündeki **ölçümler** seçeneğini belirleyin.

:::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

Buradan, örneğiniz için ölçümleri görüntüleyebilir ve özel görünümler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. bir Azure dijital TWINS örneği ve kimlik doğrulaması kurmak için API 'Leri kullanma:
* [*Nasıl yapılır: örnek ve kimlik doğrulaması ayarlama*](how-to-set-up-instance-scripted.md)

Ya da, bu nasıl yapılır: ile aynı şekilde kullanılan bir istemci uygulaması oluşturma adımlarını gözden geçir:
* [*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)
