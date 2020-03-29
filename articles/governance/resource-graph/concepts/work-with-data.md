---
title: Büyük veri kümeleriyle çalışma
description: Azure Kaynak Grafiği ile çalışırken büyük veri kümelerinde kayıtları nasıl alacağı, biçimlendirme, sayfa ve atlama konusunda anlayın.
ms.date: 03/20/2020
ms.topic: conceptual
ms.openlocfilehash: be15a6234935627ca748276e6330c50c3ee5a775
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064735"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Büyük Azure kaynak veri kümeleriyle çalışma

Azure Kaynak Grafiği, Azure ortamınızdaki kaynaklarla çalışmak ve kaynaklar hakkında bilgi almak için tasarlanmıştır. Kaynak Grafiği, binlerce kaydı sorgularken bile bu verileri hızlı bir şekilde elde etme lerini sağlar. Kaynak Grafiği'nin bu büyük veri kümeleriyle çalışmak için birkaç seçeneği vardır.

Yüksek frekanstaki sorgularla çalışma kılavuzu [için, daraltılmış istekler için Kılavuz'a](./guidance-for-throttled-requests.md)bakın.

## <a name="data-set-result-size"></a>Veri kümesi sonuç boyutu

Varsayılan olarak, Kaynak Grafiği herhangi bir sorguyalnızca **100** kayıt döndürerek sınırlar. Bu denetim, hem kullanıcıyı hem de hizmeti büyük veri kümelerine neden olacak istenmeyen sorgulara karşı korur. Bu olay genellikle, bir müşteri kendi özel ihtiyaçlarına uygun şekilde kaynakları bulmak ve filtrelemek için sorguları deneme olarak olur. Bu denetim, sonuçları sınırlamak için [üst](/azure/kusto/query/topoperator) veya [sınırAzure](/azure/kusto/query/limitoperator) Veri Gezgini dil operatörleri kullanarak farklıdır.

> [!NOTE]
> **İlk**kullanırken, en az bir sütun `asc` veya `desc`. Sıralama yapılmadan, döndürülen sonuçlar rasgeledir ve yinelenebilir değildir.

Varsayılan sınır, Kaynak Grafiği ile etkileşim tüm yöntemlerle geçersiz kılınabilir. Aşağıdaki örnekler, veri kümesi boyut sınırının _200_ile nasıl değiştirilebildiğini gösterir:

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -First 200
```

REST [API'de,](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources)denetim **$top** ve **QueryRequestOptions'un**bir parçasıdır.

_En kısıtlayıcı_ denetim kazanacak. Örneğin, sorgunuz **üst** veya **sınır** işleçleri kullanıyorsa ve **İlk'ten**daha fazla kayıtla sonuçlanırsa, döndürülen en büyük kayıtlar **İlk'e**eşit olur. Aynı şekilde, **üst** veya **limit** **İlk'ten**küçükse, döndürülen kayıt kümesi **üst** veya **sınırtarafından**yapılandırılan daha küçük değer olacaktır.

**İlk** şu anda _5000_maksimum izin değeri vardır.

## <a name="skipping-records"></a>Kayıtları atlama

Büyük veri kümeleriyle çalışmak için bir sonraki seçenek **Atla** denetimidir. Bu denetim, sorgunuzun sonuçları döndürmeden önce tanımlanan kayıt sayısını atlamasına veya atlamasına olanak tanır. **Atlama,** sonucukümesinin ortasında bir yerde kayıtlara ulaşmak için amacın anlamlı bir şekilde sonuç veren sorgular için yararlıdır. Gerekli sonuçlar döndürülen veri kümesinin sonundaysa, farklı bir sıralama yapılandırması kullanmak ve sonuçları veri kümesinin üstünden almak daha verimliolur.

> [!NOTE]
> **Skip**kullanırken, sonuçları en az bir sütunla `asc` veya `desc`. Sıralama yapılmadan, döndürülen sonuçlar rasgeledir ve yinelenebilir değildir.

Aşağıdaki örnekler, bir sorgunun neden olacağı ilk _10_ kaydı nasıl atlayacağını, bunun yerine 11.

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -Skip 10
```

REST [API'de,](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources)denetim **$skip** ve **QueryRequestOptions'un**bir parçasıdır.

## <a name="paging-results"></a>Disk belleği sonuçları

İşleme için daha küçük kayıt kümelerine ayarlanmış bir sonucu kırmak gerektiğinde veya bir sonuç kümesi izin verilen _maksimum 1000_ kayıt değerini aşacağı için, sayfalama yı kullanın. [REST API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources) **QueryResponse,** bir sonuç kümesinin ayrıldığını belirtmek için değerler sağlar: **sonuçTruncated** ve **$skipToken.**
**sonuçTruncated** yanıt döndürülen ek kayıtlar varsa tüketici bilgilendirir bir boolean değeridir. Bu durum, **sayım** özelliği **totalRecords** özelliğinden daha az olduğunda da tanımlanabilir. **totalRecords** sorguyla eşleşen kaç kayıt tanımlar.

 **sonuçTruncated,** sayfalama devre dışı bırakıldığında veya `id` sütun olmadığı için mümkün olmadığında veya sorgu nun istendiğinden daha az kaynak olduğunda **doğrudur.** **Sonuç Truncated** **doğru**olduğunda, **$skipToken** özelliği ayarlanmaz.

Aşağıdaki örnekler, azure CLI ve Azure PowerShell ile bu kayıtlar atlandıktan sonra ilk 3000 kaydı nasıl **atlayış** ve **ilk** 1000 kaydı niçin döndüreceğimi gösterir:

```azurecli-interactive
az graph query -q "Resources | project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> Pagination çalışması için sorgu **id** alanını **yansıtmalıdır.** Sorguda eksikse, yanıt **$skipToken**içermez.

Örneğin, REST API dokümanlarında [Sonraki sayfa sorgusuna](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources#next-page-query) bakın.

## <a name="formatting-results"></a>Sonuçları biçimlendirme

Kaynak Grafiği sorgusunun sonuçları _Tablo_ ve _ObjectArray_olmak üzere iki biçimde sağlanır. Biçim, istek seçeneklerinin bir parçası olarak **sonuçFormat** parametresi ile yapılandırılır. _Tablo_ **biçimi, resultFormat**için varsayılan değerdir.

Azure CLI sonuçları varsayılan olarak JSON'da sağlanır. Azure PowerShell'deki sonuçlar varsayılan olarak **bir PSCustomObject'tir,** ancak `ConvertTo-Json` cmdlet kullanılarak hızla JSON'a dönüştürülebilir. Diğer SDK'lar için sorgu sonuçları _ObjectArray_ biçiminde nisbi olarak yapılandırılabilir.

### <a name="format---table"></a>Biçim - Tablo

Varsayılan biçim, _Tablo,_ sorgu tarafından döndürülen özelliklerin sütun tasarımı ve satır değerlerini vurgulamak için tasarlanmış bir JSON biçiminde sonuçları döndürür. Bu biçim, önce tanımlanan sütunlar ve daha sonra bu sütunlara hizalanmış verileri temsil eden her satırla yapılandırılmış bir tabloda veya elektronik tabloda tanımlanan verilere yakından benzer.

Burada _Tablo_ biçimlendirme ile bir sorgu sonucu nun bir örneği:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": {
        "columns": [{
                "name": "name",
                "type": "string"
            },
            {
                "name": "type",
                "type": "string"
            },
            {
                "name": "location",
                "type": "string"
            },
            {
                "name": "subscriptionId",
                "type": "string"
            }
        ],
        "rows": [
            [
                "veryscaryvm2-nsg",
                "microsoft.network/networksecuritygroups",
                "eastus",
                "11111111-1111-1111-1111-111111111111"
            ]
        ]
    },
    "facets": [],
    "resultTruncated": "true"
}
```

### <a name="format---objectarray"></a>Biçim - ObjectArray

_ObjectArray_ biçimi de json biçiminde sonuç döndürür. Ancak, bu tasarım, sütun ve satır verilerinin dizi gruplarında eşleştiği JSON'da ortak olan anahtar/değer çifti ilişkisine hizalanır.

Burada _ObjectArray_ biçimlendirme ile bir sorgu sonucu nun bir örneği:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": [{
        "name": "veryscaryvm2-nsg",
        "type": "microsoft.network/networksecuritygroups",
        "location": "eastus",
        "subscriptionId": "11111111-1111-1111-1111-111111111111"
    }],
    "facets": [],
    "resultTruncated": "true"
}
```

_ObjectArray_ biçimini kullanmak için **sonuçBiçimini** ayarlamasına bazı örnekler aşağıda verilmiştir:

```csharp
var requestOptions = new QueryRequestOptions( resultFormat: ResultFormat.ObjectArray);
var request = new QueryRequest(subscriptions, "Resources | limit 1", options: requestOptions);
```

```python
request_options = QueryRequestOptions(
    result_format=ResultFormat.object_array
)
request = QueryRequest(query="Resources | limit 1", subscriptions=subs_list, options=request_options)
response = client.resources(request)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlangıç sorgularında](../samples/starter.md)kullanılan dile bakın.
- [Gelişmiş sorgularda](../samples/advanced.md)gelişmiş kullanımlara bakın.
- Kaynakları nasıl [keşfedebilirsiniz](explore-resources.md)hakkında daha fazla bilgi edinin.