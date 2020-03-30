---
title: Azure Cosmos DB'de dizin oluşturma ilkelerini yönetme
description: Dizin oluşturma ilkelerini nasıl yönetacağınızı, bir özelliği dizin oluşturmayı nasıl dahil edinacağınızı veya hariç tutacağınızı, farklı Azure Cosmos DB SB SK'larını kullanarak dizin oluşturmayı nasıl tanımlarız öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: thweiss
ms.openlocfilehash: 58a1ee13afa76b152723cb71d4037f9c31cc8d4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252083"
---
# <a name="manage-indexing-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin oluşturma ilkelerini yönetme

Azure Cosmos DB'de veriler, her kapsayıcı için tanımlanan [dizin oluşturma ilkelerine](index-policy.md) göre dizine eklenir. Yeni oluşturulan kapsayıcıların varsayılan dizin oluşturma ilkesi tüm dizeler veya sayılar için aralık dizinlerini zorunlu tutar. Bu ilke kendi özel dizin oluşturma ilkenizle geçersiz kılınabilir.

## <a name="indexing-policy-examples"></a>Dizin oluşturma ilkesi örnekleri

Azure portalında nasıl açıkta kaldıklarını [Gösteren JSON biçimlerinde](index-policy.md#include-exclude-paths)gösterilen dizin oluşturma ilkelerine ilişkin bazı örnekler aşağıda verilmiştir. Aynı parametreler Azure CLI veya herhangi bir SDK aracılığıyla ayarlanabilir.

### <a name="opt-out-policy-to-selectively-exclude-some-property-paths"></a>Bazı özellik yollarını seçikartla dışlamak için devre dışı bırakma ilkesi

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*"
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

Bu dizin oluşturma ilkesi, aşağıdakine , ```kind``` ```dataType```" ```precision``` ve varsayılan değerlerini el ile ayarlayana eşdeğerdir. Bu özellikleri açıkça ayarlamak için artık gerekli değildir ve bunları tamamen dizin oluşturma ilkesinden atlayabilirsiniz (yukarıdaki örnekte gösterildiği gibi).

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number",
                        "precision": -1
                    },
                    {
                        "kind": "Range",
                        "dataType": "String",
                        "precision": -1
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

### <a name="opt-in-policy-to-selectively-include-some-property-paths"></a>Bazı özellik yollarını seçişekilde eklemek için kabul etme ilkesi

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*"
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

Bu dizin oluşturma ilkesi, aşağıdakine , ```kind``` ```dataType```" ```precision``` ve varsayılan değerlerini el ile ayarlayana eşdeğerdir. Bu özellikleri açıkça ayarlamak için artık gerekli değildir ve bunları tamamen dizin oluşturma ilkesinden atlayabilirsiniz (yukarıdaki örnekte gösterildiği gibi).

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

> [!NOTE] 
> Azure Cosmos DB'nin modelinize eklenebilecek yeni mülkleri proaktif olarak dizine eklemesine izin vermek için bir **devre dışı bırakma** dizin oluşturma ilkesi kullanmanız genellikle önerilir.

### <a name="using-a-spatial-index-on-a-specific-property-path-only"></a>Yalnızca belirli bir özellik yolunda uzamsal dizini kullanma

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/_etag/?"
        }
    ],
    "spatialIndexes": [
        {
            "path": "/path/to/geojson/property/?",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ]
}
```

## <a name="composite-indexing-policy-examples"></a>Bileşik dizini oluşturma ilkesi örnekleri

Tek tek özellikler için yollar eklemeveya hariç almanın yanı sıra, bileşik dizini de belirtebilirsiniz. Birden çok özellik için yan tümcesi olan bir `ORDER BY` sorgu gerçekleştirmek istiyorsanız, bu özellikler üzerinde bileşik [dizin](index-policy.md#composite-indexes) gereklidir. Ayrıca, bileşik dizinler bir filtre ve farklı özellikleri bir ORDER BY yan tümcesi olan sorgular için bir performans yararı olacaktır.

### <a name="composite-index-defined-for-name-asc-age-desc"></a>Bileşik indeks için tanımlanan (ad asc, yaş desc):

```json
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

#2 Sorgu #1 ve Sorgu için ad ve yaş üzerine yukarıdaki bileşik dizini gereklidir:

Sorgu #1:

```sql
    SELECT *
    FROM c
    ORDER BY c.name ASC, c.age DESC
```

Sorgu #2:

```sql
    SELECT *
    FROM c
    ORDER BY c.name DESC, c.age ASC
```

Bu bileşik dizini Sorgu #3 ve Sorgu #4 yararlanacak ve filtreleri optimize edecektir:

Sorgu #3:

```sql
SELECT *
FROM c
WHERE c.name = "Tim"
ORDER BY c.name DESC, c.age ASC
```

Sorgu #4:

```sql
SELECT *
FROM c
WHERE c.name = "Tim" AND c.age > 18
```

### <a name="composite-index-defined-for-name-asc-age-asc-and-name-asc-age-desc"></a>Bileşik indeks için tanımlanan (adı ASC, yaş ASC) ve (adı ASC, yaş DESC):

Aynı dizin oluşturma ilkesi içinde birden çok farklı bileşik dizin tanımlayabilirsiniz.

```json
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"ascending"
                }
            ],
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

### <a name="composite-index-defined-for-name-asc-age-asc"></a>Bileşik indeks için tanımlanan (adı ASC, yaş ASC):

Siparişi belirtmek isteğe bağlıdır. Belirtilmemişse, sıra yükseliyor.

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                },
                {  
                    "path":"/age",
                }
            ]
        ]
}
```

### <a name="excluding-all-property-paths-but-keeping-indexing-active"></a>Tüm özellik yollarını hariç tutmak, ancak dizin oluşturmayı etkin tutmak

Bu ilke, [Zaman-To-Live (TTL) özelliğinin](time-to-live.md) etkin olduğu ancak ikincil dizinin gerekli olmadığı durumlarda (Azure Cosmos DB'yi saf anahtar değer deposu olarak kullanmak için) kullanılabilir.

```json
    {
        "indexingMode": "consistent",
        "includedPaths": [],
        "excludedPaths": [{
            "path": "/*"
        }]
    }
```

### <a name="no-indexing"></a>Dizin oluşturma yok

Bu ilke dizin oluşturmayı kapatır. `indexingMode` Ayarlanmışsa, kapsayıcıya TTL ayarlayamazsınız. `none`

```json
    {
        "indexingMode": "none"
    }
```

## <a name="updating-indexing-policy"></a>Dizin oluşturma ilkesini güncelleştirme

Azure Cosmos DB'de dizin oluşturma ilkesi aşağıdaki yöntemlerden herhangi birini kullanarak güncellenebilir:

- Azure portalından
- Azure CLI'yi kullanma
- PowerShell'i kullanma
- SDK'lardan birini kullanarak

[Dizin oluşturma ilkesi güncelleştirmesi](index-policy.md#modifying-the-indexing-policy) dizin dönüşümlerini tetikler. Bu dönüşümün ilerlemesi SDK'lardan da izlenebilir.

> [!NOTE]
> Dizin oluşturma ilkesini güncellerken, Azure Cosmos DB'ye yazdığın bilgiler kesintisiz olacaktır. Yeniden dizin oluşturma sırasında, dizin güncelleştirilirken sorgular kısmi sonuçlar döndürebilir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure Cosmos kapsayıcıları dizin oluşturma ilkelerini, Azure portalının doğrudan değiştirmenize olanak tanıyan bir JSON belgesi olarak saklar.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Yeni bir Azure Cosmos hesabı oluşturun veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Ölçek **& Ayarları'na**tıklayın.

1. Dizin oluşturma ilkesi JSON belgesini değiştirin [(aşağıdaki](#indexing-policy-examples)örneklere bakın)

1. İşiniz bittiğinde **Kaydet**’e tıklayın.

![Azure portalını kullanarak Dizin Oluşturma'yı yönetme](./media/how-to-manage-indexing-policy/indexing-policy-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Bkz. özel dizin oluşturma ilkesine sahip bir kapsayıcı oluşturmak [için, CLI kullanarak özel dizin ilkesine sahip bir kapsayıcı oluşturma](manage-with-cli.md#create-a-container-with-a-custom-index-policy)

## <a name="use-powershell"></a>PowerShell kullanma

Bkz. özel dizin oluşturma ilkesine sahip bir kapsayıcı oluşturmak için [Powershell'i kullanarak özel dizin ilkesine sahip bir kapsayıcı oluşturun](manage-with-powershell.md#create-container-custom-index)

## <a name="use-the-net-sdk-v2"></a>.NET SDK V2'yi kullanın

`DocumentCollection` [.NET SDK v2'deki](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) `IndexingPolicy` nesne, değiştirip `IndexingMode` eklemenize veya kaldırmanıza `IncludedPaths` ve `ExcludedPaths`.

```csharp
// Retrieve the container's details
ResourceResponse<DocumentCollection> containerResponse = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"));
// Set the indexing mode to consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// Add an included path
containerResponse.Resource.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
// Add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/name/*" });
// Add a spatial index
containerResponse.Resource.IndexingPolicy.SpatialIndexes.Add(new SpatialSpec() { Path = "/locations/*", SpatialTypes = new Collection<SpatialType>() { SpatialType.Point } } );
// Add a composite index
containerResponse.Resource.IndexingPolicy.CompositeIndexes.Add(new Collection<CompositePath> {new CompositePath() { Path = "/name", Order = CompositePathSortOrder.Ascending }, new CompositePath() { Path = "/age", Order = CompositePathSortOrder.Descending }});
// Update container with changes
await client.ReplaceDocumentCollectionAsync(containerResponse.Resource);
```

Dizin dönüştürme ilerlemesini izlemek `RequestOptions` için, özelliği `PopulateQuotaInfo` 'ne `true`ayarlayan bir nesneyi geçirin

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> container = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"), new RequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = container.IndexTransformationProgress;
```

## <a name="use-the-net-sdk-v3"></a>.NET SDK V3'ü kullanın

`ContainerProperties` [.NET SDK v3'teki](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) nesne (kullanımıyla ilgili [bu Quickstart'a](create-sql-api-dotnet.md) `IndexingPolicy` bakın) `IndexingMode` değiştirip eklemenize veya kaldırmanıza `IncludedPaths` ve `ExcludedPaths`.

```csharp
// Retrieve the container's details
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Set the indexing mode to consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// Add an included path
containerResponse.Resource.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
// Add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/name/*" });
// Add a spatial index
SpatialPath spatialPath = new SpatialPath
{
    Path = "/locations/*"
};
spatialPath.SpatialTypes.Add(SpatialType.Point);
containerResponse.Resource.IndexingPolicy.SpatialIndexes.Add(spatialPath);
// Add a composite index
containerResponse.Resource.IndexingPolicy.CompositeIndexes.Add(new Collection<CompositePath> { new CompositePath() { Path = "/name", Order = CompositePathSortOrder.Ascending }, new CompositePath() { Path = "/age", Order = CompositePathSortOrder.Descending } });
// Update container with changes
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

Dizin dönüştürme ilerlemesini izlemek `RequestOptions` için, `PopulateQuotaInfo` `true`özelliği ayarlayan bir nesneyi `x-ms-documentdb-collection-index-transformation-progress` geçirin , ardından yanıt üstbilgisinden değeri alın.

```csharp
// retrieve the container's details
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync(new ContainerRequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = long.Parse(containerResponse.Headers["x-ms-documentdb-collection-index-transformation-progress"]);
```

Yeni bir kapsayıcı oluştururken özel bir dizin oluşturma ilkesi tanımlarken, SDK V3'ün akıcı API'si bu tanımı kısa ve verimli bir şekilde yazmanıza olanak tanır:

```csharp
await client.GetDatabase("database").DefineContainer(name: "container", partitionKeyPath: "/myPartitionKey")
    .WithIndexingPolicy()
        .WithIncludedPaths()
            .Path("/*")
        .Attach()
        .WithExcludedPaths()
            .Path("/name/*")
        .Attach()
        .WithSpatialIndex()
            .Path("/locations/*", SpatialType.Point)
        .Attach()
        .WithCompositeIndex()
            .Path("/name", CompositePathSortOrder.Ascending)
            .Path("/age", CompositePathSortOrder.Descending)
        .Attach()
    .Attach()
    .CreateIfNotExistsAsync();
```

## <a name="use-the-java-sdk"></a>Java SDK'yı kullanma

Java `DocumentCollection` [SDK'daki](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) nesne (kullanımıyla ilgili [bu Quickstart'a](create-sql-api-java.md) bakın) ortaya çıkar. `getIndexingPolicy()` `setIndexingPolicy()` İşledikleri nesne, `IndexingPolicy` dizin oluşturma modunu değiştirmenize ve dahil edilen ve dışlanan yolları eklemenize veya kaldırmanıza olanak tanır.

```java
// Retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), null);
containerResponse.subscribe(result -> {
DocumentCollection container = result.getResource();
IndexingPolicy indexingPolicy = container.getIndexingPolicy();

// Set the indexing mode to consistent
indexingPolicy.setIndexingMode(IndexingMode.Consistent);

// Add an included path

Collection<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Add an excluded path

Collection<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);

// Add a spatial index

Collection<SpatialSpec> spatialIndexes = new ArrayList<SpatialSpec>();
Collection<SpatialType> collectionOfSpatialTypes = new ArrayList<SpatialType>();

SpatialSpec spec = new SpatialSpec();
spec.setPath("/locations/*");
collectionOfSpatialTypes.add(SpatialType.Point);
spec.setSpatialTypes(collectionOfSpatialTypes);
spatialIndexes.add(spec);

indexingPolicy.setSpatialIndexes(spatialIndexes);

// Add a composite index

Collection<ArrayList<CompositePath>> compositeIndexes = new ArrayList<>();
ArrayList<CompositePath> compositePaths = new ArrayList<>();

CompositePath nameCompositePath = new CompositePath();
nameCompositePath.setPath("/name");
nameCompositePath.setOrder(CompositePathSortOrder.Ascending);

CompositePath ageCompositePath = new CompositePath();
ageCompositePath.setPath("/age");
ageCompositePath.setOrder(CompositePathSortOrder.Descending);

compositePaths.add(ageCompositePath);
compositePaths.add(nameCompositePath);

compositeIndexes.add(compositePaths);
indexingPolicy.setCompositeIndexes(compositeIndexes);

// Update the container with changes

 client.replaceCollection(container, null);
});
```

Bir kapsayıcıda dizin dönüşüm ilerlemesini `RequestOptions` izlemek için, kota bilgisinin doldurulmasını isteyen bir `x-ms-documentdb-collection-index-transformation-progress` nesneyi geçirin ve ardından yanıt üstbilgisinden değeri alın.

```java
// set the RequestOptions object
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPopulateQuotaInfo(true);
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), requestOptions);
containerResponse.subscribe(result -> {
    // retrieve the index transformation progress from the response headers
    String indexTransformationProgress = result.getResponseHeaders().get("x-ms-documentdb-collection-index-transformation-progress");
});
```

## <a name="use-the-nodejs-sdk"></a>Düğüm.js SDK kullanın

`ContainerDefinition` [Node.js SDK'nın](https://www.npmjs.com/package/@azure/cosmos) arabirimi (kullanımıyla ilgili [bu Quickstart'a](create-sql-api-nodejs.md) `indexingPolicy` bakın) `indexingMode` değiştirmenize, `includedPaths` `excludedPaths`eklemenize veya kaldırmanıza ve .

Konteynerin ayrıntılarını alma

```javascript
const containerResponse = await client.database('database').container('container').read();
```

Dizin oluşturma modunu tutarlı olacak şekilde ayarlama

```javascript
containerResponse.body.indexingPolicy.indexingMode = "consistent";
```

Uzamsal dizini de dahil olmak üzere dahil edilen yolu ekleme

```javascript
containerResponse.body.indexingPolicy.includedPaths.push({
    includedPaths: [
      {
        path: "/age/*",
        indexes: [
          {
            kind: cosmos.DocumentBase.IndexKind.Range,
            dataType: cosmos.DocumentBase.DataType.String
          },
          {
            kind: cosmos.DocumentBase.IndexKind.Range,
            dataType: cosmos.DocumentBase.DataType.Number
          }
        ]
      },
      {
        path: "/locations/*",
        indexes: [
          {
            kind: cosmos.DocumentBase.IndexKind.Spatial,
            dataType: cosmos.DocumentBase.DataType.Point
          }
        ]
      }
    ]
  });
```

Dışlanan yolu ekleme

```javascript
containerResponse.body.indexingPolicy.excludedPaths.push({ path: '/name/*' });
```

Kapsayıcıyı değişikliklerle güncelleştirin

```javascript
const replaceResponse = await client.database('database').container('container').replace(containerResponse.body);
```

Bir kapsayıcıüzerinde dizin dönüşüm ilerlemesini `RequestOptions` izlemek `populateQuotaInfo` için, `true`özelliği ayarlayan bir `x-ms-documentdb-collection-index-transformation-progress` nesneyi geçirin , ardından yanıt üstbilgisinden değeri alın.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read({
    populateQuotaInfo: true
});
// retrieve the index transformation progress from the response headers
const indexTransformationProgress = replaceResponse.headers['x-ms-documentdb-collection-index-transformation-progress'];
```

## <a name="use-the-python-sdk-v3"></a>Python SDK V3'ü kullanın

[Python SDK V3](https://pypi.org/project/azure-cosmos/) kullanılırken (kullanımıyla ilgili [bu Quickstart'a](create-sql-api-python.md) bakın), kapsayıcı yapılandırması sözlük olarak yönetilir. Bu sözlükten, dizin oluşturma ilkesine ve tüm özniteliklerine erişmek mümkündür.

Konteynerin ayrıntılarını alma

```python
containerPath = 'dbs/database/colls/collection'
container = client.ReadContainer(containerPath)
```

Dizin oluşturma modunu tutarlı olacak şekilde ayarlama

```python
container['indexingPolicy']['indexingMode'] = 'consistent'
```

Dahil edilmiş bir yol ve uzamsal dizin içeren bir dizin oluşturma ilkesi tanımlayın

```python
container["indexingPolicy"] = {

    "indexingMode":"consistent",
    "spatialIndexes":[
                {"path":"/location/*","types":["Point"]}
             ],
    "includedPaths":[{"path":"/age/*","indexes":[]}],
    "excludedPaths":[{"path":"/*"}]
}
```

Dışlanmış bir yol ile bir dizin oluşturma ilkesi tanımlama

```python
container["indexingPolicy"] = {
    "indexingMode":"consistent",
    "includedPaths":[{"path":"/*","indexes":[]}],
    "excludedPaths":[{"path":"/name/*"}]
}
```

Bileşik dizin ekleme

```python
container['indexingPolicy']['compositeIndexes'] = [
                [
                    {
                        "path": "/name",
                        "order": "ascending"
                    },
                    {
                        "path": "/age",
                        "order": "descending"
                    }
                ]
                ]
```

Kapsayıcıyı değişikliklerle güncelleştirin

```python
response = client.ReplaceContainer(containerPath, container)
```

## <a name="use-the-python-sdk-v4"></a>Python SDK V4'ü kullanın

[Python SDK V4](https://pypi.org/project/azure-cosmos/)kullanılırken, kapsayıcı yapılandırması sözlük olarak yönetilir. Bu sözlükten, dizin oluşturma ilkesine ve tüm özniteliklerine erişmek mümkündür.

Konteynerin ayrıntılarını alma

```python
database_client = cosmos_client.get_database_client('database')
container_client = database_client.get_container_client('container')
container = container_client.read()
```

Dizin oluşturma modunu tutarlı olacak şekilde ayarlama

```python
indexingPolicy = {
    'indexingMode': 'consistent'
}
```

Dahil edilmiş bir yol ve uzamsal dizin içeren bir dizin oluşturma ilkesi tanımlayın

```python
indexingPolicy = {
    "indexingMode":"consistent",
    "spatialIndexes":[
        {"path":"/location/*","types":["Point"]}
    ],
    "includedPaths":[{"path":"/age/*","indexes":[]}],
    "excludedPaths":[{"path":"/*"}]
}
```

Dışlanmış bir yol ile bir dizin oluşturma ilkesi tanımlama

```python
indexingPolicy = {
    "indexingMode":"consistent",
    "includedPaths":[{"path":"/*","indexes":[]}],
    "excludedPaths":[{"path":"/name/*"}]
}
```

Bileşik dizin ekleme

```python
indexingPolicy['compositeIndexes'] = [
    [
        {
            "path": "/name",
            "order": "ascending"
        },
        {
            "path": "/age",
            "order": "descending"
        }
    ]
]
```

Kapsayıcıyı değişikliklerle güncelleştirin

```python
response = database_client.replace_container(container_client, container['partitionKey'], indexingPolicy)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesi](index-policy.md)
