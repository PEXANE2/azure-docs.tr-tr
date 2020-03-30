---
title: Azure Depolama kaynaklarını C++ istemci kitaplığıyla listele
description: Kapsayıcıları, blob'ları, kuyrukları, tabloları ve varlıkları sıralamak için C++ için Microsoft Azure Depolama İstemci Kitaplığı'ndaki giriş API'lerini nasıl kullanacağınızı öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/23/2017
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: dineshm
ms.openlocfilehash: 0f9e80aff20c1b2663491f6d6ceb99aaec58230f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74269442"
---
# <a name="list-azure-storage-resources-in-c"></a>Azure Depolama Kaynaklarını C++ dilinde listeleme

Listeleme işlemleri, Azure Depolama ile birçok geliştirme senaryosunun anahtarıdır. Bu makalede, C++için Microsoft Azure Depolama İstemci Kitaplığı'nda sağlanan listeleme API'lerini kullanarak Azure Depolama'daki nesneleri en verimli şekilde nasıl sıralayacaiçiz açıklanmaktadır.

> [!NOTE]
> Bu kılavuz, [NuGet](https://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp)üzerinden kullanılabilen C++ sürüm 2.x için Azure Depolama İstemci Kitaplığını hedeflemektedir.

Depolama İstemci Kitaplığı, Azure Depolama'daki nesneleri listelemek veya sorgulamak için çeşitli yöntemler sağlar. Bu makalede, aşağıdaki senaryoları giderme:

* Hesaptaki kapsayıcıları listele
* Bir kapveya sanal blob dizinindeki lekeleri listele
* Hesaptaki kuyrukları listele
* Hesaptaki tabloları listele
* Tablodaki varlıkları sorgula

Bu yöntemlerin her biri farklı senaryolar için farklı aşırı yükler kullanılarak gösterilir.

## <a name="asynchronous-versus-synchronous"></a>Senkron karşı senkron

C++ için Depolama İstemci Kitaplığı [C++ REST kitaplığı](https://github.com/Microsoft/cpprestsdk)üzerine inşa edildiklerinden, doğal olarak [pplx:task](https://microsoft.github.io/cpprestsdk/classpplx_1_1task.html)kullanarak eşzamanlı işlemleri destekliyoruz. Örnek:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Senkron işlemler ilgili eşzamanlı işlemleri sarar:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Birden çok iş parçacığı uygulaması veya hizmetiyle çalışıyorsanız, performansınızı önemli ölçüde etkileyen eşitleme API'lerini çağırmak için bir iş parçacığı oluşturmak yerine doğrudan async API'lerini kullanmanızı öneririz.

## <a name="segmented-listing"></a>Segmente edilmiş giriş

Bulut depolama ölçeği segmentli giriş gerektirir. Örneğin, bir Azure blob kapsayıcısında bir milyondan fazla blob'unuz veya Bir Azure Tablosunda bir milyardan fazla varlığınız olabilir. Bunlar teorik sayılar değil, gerçek müşteri kullanım örnekleridir.

Bu nedenle, tüm nesneleri tek bir yanıtta listelemek pratik değildir. Bunun yerine, sayfalama kullanarak nesneleri listeleyebilirsiniz. Listeleme API'lerinin her biri *parçalı* bir aşırı yüke sahiptir.

Parçalı bir listeleme işlemi için yanıt şunları içerir:

* *_segment,* liste API'ye tek bir çağrı için döndürülen sonuç kümesini içerir.
* *continuation_token*, sonuçların bir sonraki sayfasını almak için bir sonraki çağrıya geçirilir. Döndürülecek başka sonuç olmadığında, devam belirteci null'dur.

Örneğin, bir kapsayıcıdaki tüm lekeleri listelemek için tipik bir çağrı aşağıdaki kod parçacığı gibi görünebilir. Kod [örneklerimizde](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp)mevcuttur:

```cpp
// List blobs in the blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

Bir sayfada döndürülen sonuç sayısının, örneğin, her API'nin aşırı yükündeki *max_results* parametre tarafından denetlenebileceğini unutmayın:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

*max_results* parametresini belirtmezseniz, 5000'e kadar sonuç varsayılan maksimum değeri tek bir sayfada döndürülür.

Ayrıca, Azure Tablo depolama alanına karşı yapılan bir sorgunun, devam belirteci boş olmasa bile, belirttiğiniz *max_results* parametrenin değerinden hiçbir kayıt veya daha az kayıt döndüremeyeceğini de unutmayın. Bunun nedenlerinden biri, sorgunun beş saniye içinde tamamlanaması olabilir. Devam belirteci boş olmadığı sürece, sorgu devam etmeli ve kodunuz segment sonuçlarının boyutunu varsaymamalıdır.

Çoğu senaryo için önerilen kodlama deseni, giriş veya sorguda açık ilerleme ve hizmetin her isteğe nasıl yanıt verdiğini sağlayan bölümlenmiş giriştir. Özellikle C++ uygulamaları veya hizmetleri için, listeleme ilerlemesinin alt düzey denetimi bellek ve performansı denetlemeye yardımcı olabilir.

## <a name="greedy-listing"></a>Açgözlü liste

C++ için Depolama İstemci Kitaplığı'nın önceki sürümleri (sürüm 0.5.0 Önizleme ve önceki sürümler) aşağıdaki örnekte olduğu gibi tablolar ve kuyruklar için segmentdışı listi'leri içeriyordu:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Bu yöntemler parçalı API'lerin paketleyicileri olarak uygulanmıştır. Parçalı listelemenin her yanıtı için kod, sonuçları bir vektöre ekledi ve tüm kapsayıcılar tarandıktan sonra tüm sonuçları döndürdü.

Depolama hesabı veya tablo az sayıda nesne içeriyorsa, bu yaklaşım işe yarayabilir. Ancak, nesne sayısındaki artışla, gerekli bellek sınırsız olarak artabilir, çünkü tüm sonuçlar bellekte kalır. Bir listeleme işlemi, arayan kişinin ilerlemesi hakkında hiçbir bilgisi olmadığı çok uzun zaman alabilir.

SDK'daki bu açgözlü liste API'leri C#, Java veya JavaScript Node.js ortamında bulunmaz. Bu açgözlü API'leri kullanmanın olası sorunlarını önlemek için bunları sürüm 0.6.0 Önizleme'de kaldırdık.

Kodunuz bu açgözlü API'leri çağırıyorsa:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Daha sonra, bölümlenmiş liste API'lerini kullanmak için kodunuzu değiştirmeniz gerekir:

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

Segmentin *max_results* parametresini belirterek, uygulamanızın performans hususlarını karşılamak için istek sayısı ile bellek kullanımı arasında denge leyebilirsiniz.

Ayrıca, segmentli listeleme API'leri kullanıyorsanız, ancak verileri yerel bir koleksiyonda "açgözlü" bir şekilde depolıyorsanız, verileriyerel bir koleksiyonda dikkatlice ölçekle depolamayı işlemek için kodunuzu yeniden düzenlemenizi de şiddetle öneririz.

## <a name="lazy-listing"></a>Tembel liste

Açgözlü liste potansiyel sorunları gündeme rağmen, kapsayıcıda çok fazla nesne yoksa kullanışlıdır.

Ayrıca C# veya Oracle Java SDK'ları kullanıyorsanız, belirli bir ofsetteki verilerin yalnızca gerekli olduğunda getirili tembel tarzı bir liste sunan Numaralandırılabilir programlama modeline aşina olmalısınız. C++'da, yineleyici tabanlı şablon da benzer bir yaklaşım sağlar.

Örnek olarak **list_blobs** kullanan tipik bir tembel liste API şuna benzer:

```cpp
list_blob_item_iterator list_blobs() const;
```

Tembel giriş deseni kullanan tipik bir kod parçacığı aşağıdaki gibi görünebilir:

```cpp
// List blobs in the blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

Tembel girişin yalnızca senkron modda kullanılabildiğini unutmayın.

Açgözlü listeyle karşılaştırıldığında, tembel liste yalnızca gerektiğinde veri getirir. Kapakların altında, azure depolamadan verileri yalnızca bir sonraki yineleyici bir sonraki segmente geçtiğinde getirir. Bu nedenle, bellek kullanımı sınırlı bir boyutla denetlenir ve işlem hızlıdır.

Tembel listeleme API'leri, Sürüm 2.2.0'da C++ için Depolama İstemci Kitaplığı'na dahildir.

## <a name="conclusion"></a>Sonuç

Bu makalede, C++ için Depolama İstemci Kitaplığı'nda çeşitli nesneler için API'leri listelemek için farklı aşırı yüklemeleri tartıştık. Özetlersek:

* Async API'leri, birden çok iş parçacığı senaryosu altında güçlü bir şekilde önerilir.
* Segmente edilmiş giriş çoğu senaryo için önerilir.
* Tembel liste senkron senaryolarda uygun bir sarmalayıcı olarak kitaplıkta sağlanır.
* Açgözlü giriş önerilmez ve kitaplıktan kaldırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

C++ için Azure Depolama ve İstemci Kitaplığı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [C++'dan Blob Depolama nasıl kullanılır?](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [C++'dan Tablo Depolama nasıl kullanılır?](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [C++’tan Kuyruk Depolama kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [C++ API belgeleri için Azure Depolama İstemci Kitaplığı.](https://azure.github.io/azure-storage-cpp/)
* [Azure Depolama Ekibi Blogu](https://blogs.msdn.com/b/windowsazurestorage/)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
