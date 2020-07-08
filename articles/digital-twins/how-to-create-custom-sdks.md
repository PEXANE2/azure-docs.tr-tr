---
title: Oto Rest ile Azure dijital TWINS için özel SDK 'lar oluşturma
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS 'i C# dışındaki dillerde kullanmak için bkz. özel SDK 'lar oluşturma.
author: baanders
ms.author: baanders
ms.date: 4/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 261b288154dddacf91f3cb3ba6dec99e3a3534cc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84725809"
---
# <a name="create-custom-sdks-for-azure-digital-twins-with-autorest"></a>Oto Rest ile Azure dijital TWINS için özel SDK 'lar oluşturma

Şu anda, Azure Digital TWINS API 'Leri ile etkileşime yönelik yalnızca yayımlanan veri düzlemi SDK 'Sı .NET için geçerlidir (C#). .NET SDK ve genel olarak API 'Ler hakkında bilgi edinmek için bkz. [nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma](how-to-use-apis-sdks.md). Başka bir dilde çalışıyorsanız, bu makalede, oto Rest kullanarak kendi SDK 'nizi tercih ettiğiniz dilde nasıl oluşturabileceğiniz gösterilir.

## <a name="set-up-the-sdk"></a>SDK 'Yı ayarlama

Bir SDK oluşturmak için şunlar gerekir:
* [Oto Rest](https://github.com/Azure/autorest), sürüm 2.0.4413 (sürüm 3 Şu anda desteklenmiyor)
* Otomatik Rest için önkoşul olarak [Node.js](https://nodejs.org)
* [Azure Digital TWINS Openapı (Swagger) dosyası](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/preview/2020-03-01-preview/digitaltwins.json)

Node.js yüklüyse, doğru oto Rest sürümünün yüklü olduğundan emin olmak için bu komutu çalıştırabilirsiniz:
```cmd/sh
npm install -g autorest@2.0.4413
```

Azure dijital TWINS Swagger dosyasında oto Rest 'i çalıştırmak için aşağıdaki adımları izleyin:
1. Azure dijital TWINS Swagger dosyasını çalışma dizinine kopyalayın.
2. Bir komut isteminde o çalışma dizinine geçin.
3. Aşağıdaki komutla, oto REST komutunu çalıştırın.

```cmd/sh
autorest --input-file=adtApiSwagger.json --csharp --output-folder=ADTApi --add-credentials --azure-arm --namespace=ADTApi
```

Sonuç olarak, çalışma dizininizde *Adtapı* adlı yeni bir klasör görürsünüz. Oluşturulan SDK dosyaları *Adtapı*ad alanına sahip olacaktır, bu da örneklerin geri kalanı aracılığıyla kullanmaya devam edersiniz.

Oto Rest, çok çeşitli dil kodu oluşturucuları destekler.

## <a name="add-the-sdk-to-a-visual-studio-project"></a>SDK 'Yı bir Visual Studio projesine ekleme

Doğrudan bir .NET çözümüne, oto Rest tarafından oluşturulan dosyaları dahil edebilirsiniz. Ancak, Azure dijital TWINS SDK 'sına çok sayıda ayrı projede (istemci uygulamalarınız, Azure Işlevleri uygulamalarınız vb.) ihtiyacınız olacak şekilde, oluşturulan dosyalardan ayrı bir proje (.NET sınıf kitaplığı) oluşturmanızı öneririz. Daha sonra bu sınıf kitaplığı projesini diğer çözümlerinizi proje başvurusu olarak ekleyebilirsiniz.

Bu bölüm, SDK 'nın kendi projesi olan ve diğer projelere dahil olabilen bir sınıf kitaplığı olarak nasıl oluşturulacağı hakkında yönergeler sağlar. Uygulamanız gereken adımlar:

1. Sınıf kitaplığı için yeni bir Visual Studio çözümü oluşturma
2. Proje adı olarak "ADTApi" adını kullanın
3. Çözüm Gezgini ' nde, oluşturulan çözümün *Adtapi* projesini sağ seçin ve *> var olan öğe Ekle ' yi seçin...*
4. SDK 'Yı oluşturduğunuz klasörü bulun ve kök düzeyindeki dosyaları seçin
5. "Tamam" düğmesine basın
6. Projeye bir klasör ekleyin (Çözüm Gezgini içinde projeyi sağ seçin ve *> yeni klasör ekle*' yi seçin.
7. Klasörü "modeller" olarak adlandırın
8. Çözüm Gezgini ' nde *modeller* klasörünü sağ seçin ve *> var olan öğeyi Ekle ' yi seçin...*
9. Oluşturulan SDK 'nın *modeller* klasöründeki dosyaları seçin ve "Tamam" düğmesine basın

SDK 'Yı başarılı bir şekilde derlemek için, projenizin bu başvurulara ihtiyacı olacak:
* `Microsoft.Rest.ClientRuntime`
* `Microsoft.Rest.ClientRuntime.Azure`

Bunları eklemek için *NuGet paket yöneticisi > açık araçlar > çözüm Için NuGet Paketlerini Yönet...*.

1. Panelde, *tarayıcı* sekmesinin seçili olduğundan emin olun
2. *Microsoft. Rest* 'i arayın
3. *ClientRuntime* ve *ClientRuntime. Azure* paketlerini seçin ve bunları çözümünüze ekleyin

Artık projeyi oluşturabilir ve yazdığınız herhangi bir Azure dijital TWINS uygulamasına bir proje başvurusu olarak ekleyebilirsiniz.

## <a name="general-guidelines-for-generated-sdks"></a>Oluşturulan SDK 'lar için genel yönergeler

Bu bölüm, ile ilgili genel bilgileri ve oluşturulan SDK 'Yı kullanma yönergelerini içerir.

### <a name="synchronous-and-asynchronous-calls"></a>Zaman uyumlu ve zaman uyumsuz çağrılar

Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

### <a name="typed-and-untyped-data"></a>Türü belirtilmiş ve türsüz veriler

REST API çağrılar genellikle kesin türü belirtilmiş nesneleri döndürür. Ancak Azure dijital TWINS, kullanıcıların TWINS için kendi özel türlerini tanımlamasına izin vermediğinden, birçok Azure dijital TWINS çağrısı için statik dönüş verilerini önceden tanımlamanız mümkün değildir. Bunun yerine, API 'Ler uygun olduğunda kesin türü belirtilmiş sarmalayıcı türleri döndürür ve özel olarak yazılmış ikizi verileri Json.NET nesnelerinde (API imzalarında "Object" veri türünün göründüğü her yerde kullanılır). Bu nesneleri kodunuzda uygun şekilde çevirebilirsiniz.

### <a name="error-handling"></a>Hata işleme

SDK 'da bir hata oluştuğunda (404 gibi HTTP hataları dahil), SDK bir özel durum oluşturur. Bu nedenle, try/catch blokları içindeki tüm API çağrılarını kapsüllemek önemlidir.

İşte bir ikizi eklemeye ve bu işlemdeki hataları yakamaya çalışan bir kod parçacığı:

```csharp
try
{
    await client.DigitalTwins.AddAsync(id, initData);
    Console.WriteLine($"Created a twin successfully: {id}");
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
}
```

### <a name="paging"></a>Sayfalama

Oto Rest, SDK için iki tür sayfalama deseni üretir:
* Sorgu API 'SI hariç tüm API 'Ler için bir tane
* Bir sorgu API 'SI için

Sorgu olmayan sayfalama düzeninde her çağrının iki sürümü vardır:
* Başlangıç çağrısını yapmak için bir sürüm (gibi `DigitalTwins.ListEdges()` )
* Sonraki sayfaları almak için bir sürüm, sonal "Ileri" (gibi `DigitalTwins.ListEdgesNext()` )

Azure dijital TWINS 'ten gelen giden ilişkilerin disk belleğine alınmış bir listesini almayı gösteren bir kod parçacığı aşağıda verilmiştir:
```csharp
try
{
    // List to hold the results in
    List<object> relList = new List<object>();
    // Enumerate the IPage object returned to get the results
    // ListAsync will throw if an error occurs
    IPage<object> relPage = await client.DigitalTwins.ListEdgesAsync(id);
    relList.AddRange(relPage);
    // If there are more pages, the NextPageLink in the page is set
    while (relPage.NextPageLink != null)
    {
        // Get more pages...
        relPage = await client.DigitalTwins.ListEdgesNextAsync(relPage.NextPageLink);
        relList.AddRange(relPage);
    }
    Console.WriteLine($"Found {relList.Count} relationships on {id}");
    // Do something with each object found
    // As relationships are custom types, they are JSON.Net types
    foreach (JObject r in relList)
    {
        string relId = r.Value<string>("$edgeId");
        string relName = r.Value<string>("$relationship");
        Console.WriteLine($"Found relationship {relId} from {id}");
    }
}
catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error retrieving relationships on {id}: {e.Response.StatusCode}");
}
```

İkinci model yalnızca sorgu API 'SI için oluşturulur. Açıkça bir kullanır `continuationToken` .

Bu düzene sahip bir örnek aşağıda verilmiştir:

```csharp
string query = "SELECT * FROM digitaltwins";
string conToken = null; // continuation token from the query
int page = 0;
try
{
    // Repeat the query while there are pages
    do
    {
        QuerySpecification spec = new QuerySpecification(query, conToken);
        QueryResult qr = await client.Query.QueryTwinsAsync(spec);
        page++;
        Console.WriteLine($"== Query results page {page}:");
        if (qr.Items != null)
        {
            // Query returns are JObjects
            foreach(JObject o in qr.Items)
            {
                string twinId = o.Value<string>("$dtId");
                Console.WriteLine($"  Found {twinId}");
            }
        }
        Console.WriteLine($"== End query results page {page}");
        conToken = qr.ContinuationToken;
    } while (conToken != null);
} catch (ErrorResponseException e)
{
    Console.WriteLine($"*** Error in twin query: ${e.Response.StatusCode}");
}
```

## <a name="next-steps"></a>Sonraki adımlar

SDK 'nizi kullanabileceğiniz bir istemci uygulaması oluşturma adımlarını gözden geçir:
* [Öğretici: istemci uygulamasını kodlayın](tutorial-code.md)