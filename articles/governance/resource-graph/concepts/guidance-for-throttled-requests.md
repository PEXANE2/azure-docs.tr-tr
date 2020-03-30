---
title: Kısıtlanan istekler için yönergeler
description: Taleplerin Azure Kaynak Grafiği tarafından daraltılmasını önlemek için paralel olarak gruplandırmayı, stagger, pajine ve sorgu yapmayı öğrenin.
ms.date: 12/02/2019
ms.topic: conceptual
ms.openlocfilehash: fbd4bec715b187bcc643fe32b8452b0e062e7713
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259857"
---
# <a name="guidance-for-throttled-requests-in-azure-resource-graph"></a>Azure Kaynak Grafiği'nde daraltılmış istekler için kılavuz

Azure Kaynak Grafiği verilerinin programatik ve sık kullanımı oluşturulurken, azaltmanın sorguların sonuçlarını nasıl etkilediğine dikkat edilmelidir. Verilerin istenme şeklini değiştirmek, sizin ve kuruluşunuzun daraltılmasından kaçınmanıza ve Azure kaynaklarınız hakkındaki zamanında veri akışını sürdürmenize yardımcı olabilir.

Bu makalede, Azure Kaynak Grafiği'nde sorguların oluşturulmasıyla ilgili dört alan ve desen yer alıyor:

- Azaltma üst bilgilerini anlama
- Sorguları gruplandırma
- Sorguları kademelendirme
- Pagination etkisi

## <a name="understand-throttling-headers"></a>Azaltma üst bilgilerini anlama

Azure Kaynak Grafiği, bir zaman penceresinden her kullanıcı için kota numarası ayırır. Örneğin, bir kullanıcı her 5 saniyelik pencerede en fazla 15 sorgu gönderebilir. Kota değeri birçok faktör tarafından belirlenir ve değişebilir.

Her sorgu yanıtında, Azure Kaynak Grafiği iki azaltma üstbilgini ekler:

- `x-ms-user-quota-remaining`(int): Kullanıcı için kalan kaynak kotası. Bu değer, sorgu sayısıiçin eşlenir.
- `x-ms-user-quota-resets-after`(hh:mm:ss): Kullanıcının kota tüketimi sıfırlanana kadar geçen süre.

Üstbilginin nasıl çalıştığını göstermek için, üstbilgi ve değerleri olan bir `x-ms-user-quota-remaining: 10` sorgu `x-ms-user-quota-resets-after: 00:00:03`yanıtına bakalım ve.

- Sonraki 3 saniye içinde, en fazla 10 sorgu daraltılmadan gönderilebilir.
- 3 saniye içinde, `x-ms-user-quota-remaining` değerleri `x-ms-user-quota-resets-after` ve `00:00:05` sırasıyla `15` sıfırlanacaktır.

Sorgu isteklerini _geri almak_ için üstbilgi kullanma örneğini görmek için, [Sorgu'daki](#query-in-parallel)örneğe Paralel olarak bakın.

## <a name="grouping-queries"></a>Sorguları gruplandırma

Sorguları abonelik, kaynak grubu veya tek tek kaynağa göre gruplandırma, sorguları paralelleştirmekten daha verimlidir. Daha büyük bir sorgunun kota maliyeti genellikle birçok küçük ve hedefli sorgunun kota maliyetinden daha düşüktür. Grup boyutu _300'den_az olması önerilir.

- Kötü optimize edilmiş bir yaklaşım örneği

  ```csharp
  // NOT RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  foreach (var subscriptionId in subscriptionIds)
  {
      var userQueryRequest = new QueryRequest(
          subscriptions: new[] { subscriptionId },
          query: "Resoures | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

  // ...
  }
  ```

- Örnek #1 en iyi gruplandırılmış gruplandırma yaklaşımı

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  const int groupSize = 100;
  for (var i = 0; i <= subscriptionIds.Count / groupSize; ++i)
  {
      var currSubscriptionGroup = subscriptionIds.Skip(i * groupSize).Take(groupSize).ToList();
      var userQueryRequest = new QueryRequest(
          subscriptions: currSubscriptionGroup,
          query: "Resources | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

- Örnek #2 bir sorguda birden çok kaynak almak için en iyi duruma getirilmiş gruplandırma yaklaşımı

  ```kusto
  Resources | where id in~ ({resourceIdGroup}) | project name, type
  ```

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var resourceIds = /* A big list of resourceIds */

  const int groupSize = 100;
  for (var i = 0; i <= resourceIds.Count / groupSize; ++i)
  {
      var resourceIdGroup = string.Join(",",
          resourceIds.Skip(i * groupSize).Take(groupSize).Select(id => string.Format("'{0}'", id)));
      var userQueryRequest = new QueryRequest(
          subscriptions: subscriptionList,
          query: $"Resources | where id in~ ({resourceIdGroup}) | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

## <a name="staggering-queries"></a>Sorguları kademelendirme

Azaltma nın zorlanma biçimi nedeniyle, sorguların şaşırtıcı olmasını öneririz. Diğer bir şey, aynı anda 60 sorgu göndermek yerine, sorguları dört 5 saniyelik pencerelere dönüştürün:

- Şaşırtıcı olmayan sorgu zamanlaması

  | Sorgu Sayısı         | 60  | 0    | 0     | 0     |
  |---------------------|-----|------|-------|-------|
  | Zaman Aralığı (sn) | 0-5 | 5-10 | 10-15 | 15-20 |

- Şaşırtıcı sorgu zamanlaması

  | Sorgu Sayısı         | 15  | 15   | 15    | 15    |
  |---------------------|-----|------|-------|-------|
  | Zaman Aralığı (sn) | 0-5 | 5-10 | 10-15 | 15-20 |

Aşağıda, Azure Kaynak Grafiği'ni sorgularken azaltma üstbilgisini kullanmanın bir örneği verilmiştir:

```csharp
while (/* Need to query more? */)
{
    var userQueryRequest = /* ... */
    // Send post request to Azure Resource Graph
    var azureOperationResponse = await this.resourceGraphClient
        .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
        .ConfigureAwait(false);

    var responseHeaders = azureOperationResponse.response.Headers;
    int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
    TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
    if (remainingQuota == 0)
    {
        // Need to wait until new quota is allocated
        await Task.Delay(resetAfter).ConfigureAwait(false);
    }
}
```

### <a name="query-in-parallel"></a>Paralel Sorgu

Paralelleştirme üzerinden gruplandırma önerilse de, sorguların kolayca gruplandırılabildiği zamanlar vardır. Bu gibi durumlarda, birden çok sorguyı paralel bir şekilde göndererek Azure Kaynak Grafiği'ni sorgulamak isteyebilirsiniz. Aşağıda, bu tür senaryolarda azaltma üstbilgilerine dayalı olarak nasıl _geri çekilene_ ilişkin bir örnek verilmiştir:

```csharp
IEnumerable<IEnumerable<string>> queryGroup = /* Groups of queries  */
// Run groups in parallel.
await Task.WhenAll(queryGroup.Select(ExecuteQueries)).ConfigureAwait(false);

async Task ExecuteQueries(IEnumerable<string> queries)
{
    foreach (var query in queries)
    {
        var userQueryRequest = new QueryRequest(
            subscriptions: subscriptionList,
            query: query);
        // Send post request to Azure Resource Graph.
        var azureOperationResponse = await this.resourceGraphClient
            .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
            .ConfigureAwait(false);
        
        var responseHeaders = azureOperationResponse.response.Headers;
        int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
        TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
        if (remainingQuota == 0)
        {
            // Delay by a random period to avoid bursting when the quota is reset.
            var delay = (new Random()).Next(1, 5) * resetAfter;
            await Task.Delay(delay).ConfigureAwait(false);
        }
    }
}
```

## <a name="pagination"></a>Sayfalandırma

Azure Kaynak Grafiği tek bir sorgu yanıtında en fazla 1000 girişte döndürdİğİnden, aradığınız tüm veri kümesini almak için sorgularınızı [ayırmanız](./work-with-data.md#paging-results) gerekebilir. Ancak, bazı Azure Kaynak Grafiği istemcileri pagination'ı diğerlerinden farklı işler.

- C# SDK’sı

  ResourceGraph SDK kullanırken, bir önceki sorgu yanıtından sonraki paginated sorguya döndürülen atlama belirteci geçerek pagination işlemek gerekir. Bu tasarım, tüm paginated aramalardan sonuç toplamanız ve sonunda bunları birleştirmeniz gerektiği anlamına gelir. Bu durumda, gönderdiğiniz her paginated sorgu bir sorgu kotası alır:

  ```csharp
  var results = new List<object>();
  var queryRequest = new QueryRequest(
      subscriptions: new[] { mySubscriptionId },
      query: "Resources | project id, name, type | top 5000");
  var azureOperationResponse = await this.resourceGraphClient
      .ResourcesWithHttpMessagesAsync(queryRequest, header)
      .ConfigureAwait(false);
  while (!string.Empty(azureOperationResponse.Body.SkipToken))
  {
      queryRequest.SkipToken = azureOperationResponse.Body.SkipToken;
      // Each post call to ResourceGraph consumes one query quota
      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(queryRequest, header)
          .ConfigureAwait(false);
      results.Add(azureOperationResponse.Body.Data.Rows);

      // Inspect throttling headers in query response and delay the next call if needed.
  }
  ```

- Azure CLI / Azure PowerShell

  Azure CLI veya Azure PowerShell kullanırken, Azure Kaynak Grafiği'ndeki sorgular en fazla 5000 giriş almak üzere otomatik olarak paginar. Sorgu sonuçları, tüm paginated aramalardan girişlerin birleşik bir listesini döndürer. Bu durumda, sorgu sonucuna giren giriş sayısına bağlı olarak, tek bir paginated sorgu birden fazla sorgu kotası tüketebilir. Örneğin, aşağıdaki örnekte, sorgunun tek bir çalışması en fazla beş sorgu kotası tüketebilir:

  ```azurecli-interactive
  az graph query -q 'Resources | project id, name, type' --first 5000
  ```

  ```azurepowershell-interactive
  Search-AzGraph -Query 'Resources | project id, name, type' -First 5000
  ```

## <a name="still-get-throttled"></a>Hala daraltılmış mı?

Yukarıdaki önerileri yaptıktan sonra daralıyorsanız, '' [resourcegraphsupport@microsoft.com](mailto:resourcegraphsupport@microsoft.com)adresinden ekibe başvurun.

Bu ayrıntıları sağlayın:

- Daha yüksek bir azaltma sınırı için özel kullanım kılıfınız ve iş sürücünüzün ihtiyacı vardır.
- Kaç kaynağa erişiminiz var? Tek bir sorgudan kaç tanesi döndürülür?
- Ne tür kaynaklarla ilgileniyorsunuz?
- Sorgu desenin nedir? Y saniyebaşına X sorguları vb.

## <a name="next-steps"></a>Sonraki adımlar

- [Başlangıç sorgularında](../samples/starter.md)kullanılan dile bakın.
- [Gelişmiş sorgularda](../samples/advanced.md)gelişmiş kullanımlara bakın.
- Kaynakları nasıl [keşfedebilirsiniz](explore-resources.md)hakkında daha fazla bilgi edinin.