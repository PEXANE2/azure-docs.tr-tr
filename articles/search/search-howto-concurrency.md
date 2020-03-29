---
title: Kaynaklara eşzamanlı yazmaları yönetme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinleri, dizinleyiciler ve veri kaynaklarında güncellemeler veya silmelerde havada çarpışmaları önlemek için iyimser eşzamanlılık kullanın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: edfb2fe5cc37a00335ca7b5be851a88825b03eb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792210"
---
# <a name="how-to-manage-concurrency-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da eşzamanlılık nasıl yönetilir?

Dizinler ve veri kaynakları gibi Azure Bilişsel Arama kaynaklarını yönetirken, özellikle de kaynaklara uygulamanızın farklı bileşenleri tarafından eş zamanlı olarak erişiliyorsa, kaynakları güvenli bir şekilde güncelleştirmek önemlidir. İki istemci eş zamanlı olarak koordinasyon olmadan bir kaynağı güncelleştirdiğinizde, yarış koşulları mümkündür. Bunu önlemek için Azure Bilişsel Arama iyimser bir *eşzamanlılık modeli*sunar. Kaynaküzerinde kilit yok. Bunun yerine, kaynak sürümünü tanımlayan her kaynak için bir ETag vardır, böylece yanlışlıkla üzerine yazılmasından kaçınan istekler hazırlayabilirsiniz.

> [!Tip]
> Örnek bir [C# çözümündeki](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) kavramsal kod, Azure Bilişsel Arama'da eşzamanlılık denetiminin nasıl çalıştığını açıklar. Kod, eşzamanlılık denetimini çağıran koşullar oluşturur. Aşağıdaki [kod parçasını](#samplecode) okumak büyük olasılıkla çoğu geliştirici için yeterlidir, ancak çalıştırmak istiyorsanız, hizmet adını ve yönetici api anahtarını eklemek için appsettings.json'ı edin. Bir hizmet URL'si `http://myservice.search.windows.net`verilen `myservice`, hizmet adı .

## <a name="how-it-works"></a>Nasıl çalışır?

İyimser eşzamanlılık, API çağrılarındaki erişim koşulu denetimleri aracılığıyla dizinlere, dizin oluşturanlara, veri kaynaklarına ve eş anlamlı Map kaynaklarına yazılır.

Tüm kaynakların nesne sürümü bilgilerini sağlayan bir [*varlık etiketi (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) vardır. Önce ETag'ı işaretleyerek, kaynağın ETag'inin yerel kopyanızla eşleştiğinden emin olarak, tipik bir iş akışında eşzamanlı güncelleştirmeleri (yerel olarak alın, değiştirin, güncelleştirmeyi) önleyebilirsiniz.

+ REST API istek üstbilgisi üzerinde bir [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) kullanır.
+ .NET SDK, ETag'ı accessCondition nesnesi üzerinden ayarlar ve [If-Match | Kaynakta If-Match-None üstbilgisi.](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) [IResourceWithETag'dan (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) devralınan herhangi bir nesnenin accessCondition nesnesi vardır.

Bir kaynağı her güncellediğinizde, eetiketi otomatik olarak değişir. Eşzamanlılık yönetimini uyguladığınız zaman, yaptığınız tek şey, uzak kaynağın istemcide değiştirdiğiniz kaynağın kopyasıyla aynı ETag'a sahip olmasını gerektiren güncelleştirme isteğine bir ön koşul koymaktır. Eşzamanlı bir işlem uzak kaynağı zaten değiştirmişse, ETag ön koşulla eşleşmez ve istek HTTP 412 ile başarısız olur. .NET SDK kullanıyorsanız, bu uzantı yönteminin `CloudException` `IsAccessConditionFailed()` doğru döndüğü bir durum olarak gösterir.

> [!Note]
> Eşzamanlılık için tek bir mekanizma vardır. Kaynak güncelleştirmeleri için hangi API'nin kullanıldığına bakılmaksızın her zaman kullanılır.

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Kullanım örnekleri ve örnek kod

Aşağıdaki kod, anahtar güncelleştirme işlemleri için accessCondition denetimlerini gösterir:

+ Kaynak artık yoksa güncelleştirmeyi başarısız
+ Kaynak sürümü değişirse güncelleştirmeyi başarısız

### <a name="sample-code-from-dotnetetagsexplainer-program"></a>[DotNetEEtiketlerExplainer programından](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) örnek kod

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Cognitive Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Cognitive Search has an associated ETag that keeps track of which version
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Cognitive Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Cognitive Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Cognitive Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a>Tasarım deseni

İyimser eşzamanlılık uygulamak için bir tasarım deseni, erişim durumu denetimini yeniden deneyen bir döngü, erişim koşulu için bir test ve değişiklikleri yeniden uygulamaya çalışmadan önce isteğe bağlı olarak güncelleştirilmiş bir kaynak alır.

Bu kod parçacığı zaten var olan bir dizin eşanlamlısı Map eklenmesini göstermektedir. Bu kod, [Azure Bilişsel Arama için Eş anlamlı C# örneğinden](search-synonyms-tutorial-sdk.md)dir.

Parçacık "oteller" dizini alır, bir güncelleştirme işleminde nesne sürümünü denetler, koşul başarısız olursa özel bir durum atar ve en son sürümü almak için sunucudan dizin alımıyla başlayarak işlemi (en fazla üç kez) yeniden dener.

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a>Sonraki adımlar

Varolan bir dizinin güvenli bir şekilde nasıl güncelleştirileceklerine ilişkin daha fazla bağlam için [eş anlamlı C# örneğini](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) gözden geçirin.

Aşağıdaki örneklerden birini ETags veya AccessCondition nesnelerini içerecek şekilde değiştirmeyi deneyin.

+ [GitHub'da REST API örneği](https://github.com/Azure-Samples/search-rest-api-getting-started)
+ [GitHub'da .NET SDK örneği.](https://github.com/Azure-Samples/search-dotnet-getting-started) Bu çözüm, bu makalede sunulan kodu içeren "DotNetEtagsExplainer" projesini içerir.

## <a name="see-also"></a>Ayrıca bkz.

[Ortak HTTP istek ve yanıt başlıkları](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)
[HTTP durum kodları](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)
[Dizin işlemleri (REST API)](https://docs.microsoft.com/rest/api/searchservice/index-operations)
