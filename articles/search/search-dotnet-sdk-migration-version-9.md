---
title: Azure Arama .NET SDK sürümü9'a yükseltme
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Arama .NET SDK sürüm 9'a geçirin. Yeniliklerin ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fcc70267754f7e66f29dd1b855d3efb8b814e78b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793013"
---
# <a name="upgrade-to-azure-search-net-sdk-version-9"></a>Azure Arama .NET SDK sürümü9'a yükseltme

[Azure Arama .NET SDK](https://aka.ms/search-sdk)sürümü 7.0 önizlemesini veya daha eski sürümünü kullanıyorsanız, bu makale, uygulamanızı sürüm 9'u kullanmak üzere yükseltmenize yardımcı olur.

> [!NOTE]
> Sürüm 8.0-önizlemeyi, genel olarak henüz kullanılamayan özellikleri değerlendirmek için kullanmak isterseniz, önceki sürümlerden 8.0 önizlemeye yükseltmek için bu makaledeki yönergeleri de izleyebilirsiniz.

Örnekler de dahil olmak üzere SDK'nın daha genel bir gözden geçirme için [,.NET Uygulamasından Azure Arama'nın nasıl kullanılacağına](search-howto-dotnet-sdk.md)bakın.

Azure Arama .NET SDK sürümü 9 önceki sürümlerden birçok değişiklik içerir. Bunlardan bazıları değişiklikleri bozuyor, ancak kodunuzda yalnızca nispeten küçük değişiklikler gerektirmelidir. Yeni SDK sürümünü kullanmak için kodunuzu nasıl değiştireceğinize ilişkin yönergeler için [yükseltme adımları'na](#UpgradeSteps) bakın.

> [!NOTE]
> Sürüm 4.0 önizleme veya daha eski kullanıyorsanız, önce sürüm 5'e yükseltmeniz ve ardından sürüm 9'a yükseltmeniz gerekir. Talimatlar için [Azure Arama .NET SDK sürüm 5'e yükseltme](search-dotnet-sdk-migration-version-5.md) ye bakın.
>
> Azure Arama hizmeti örneğiniz, en sonuncusu da dahil olmak üzere birkaç REST API sürümü destekler. Bir sürümü artık en son sürüm olmadığında kullanmaya devam edebilirsiniz, ancak en yeni sürümü kullanmak için kodunuzu geçirmenizi öneririz. REST API'sini kullanırken, api sürüm parametresi aracılığıyla her istekteki API sürümünü belirtmeniz gerekir. .NET SDK'yı kullanırken, kullandığınız SDK sürümü REST API'nin ilgili sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü desteklemek üzere yükseltilse bile bu kodu hiçbir değişiklik olmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-9"></a>Sürüm 9'daki yenilikler
Azure Arama .NET SDK sürümü 9, Azure Arama REST API'sinin özellikle 2019-05-06 olmak üzere genel kullanıma açık en son sürümünü hedefler. Bu, bir .NET uygulamasından Azure Arama'nın yeni özelliklerini aşağıdakiler de dahil olmak üzere kullanmayı mümkün kılar:

* [AI zenginleştirme,](cognitive-search-concept-intro.md) görüntülerden, lekelerden ve diğer yapılandırılmamış veri kaynaklarından metin ayıklama yeteneğidir ve içeriği azure arama dizininde daha fazla aranabilir hale getirmek için zenginleştirir.
* Karmaşık [türler](search-howto-complex-data-types.md) için destek, bir Azure Arama dizininde iç içe olan hemen hemen her JSON yapısını modellemenize olanak tanır.
* [Otomatik tamamlama,](search-autocomplete-tutorial.md) tür olarak arama davranışı uygulamak için **API Öner'e** bir alternatif sağlar. Otomatik tamamlama "bitirir" sözcüğü veya ötmeyi kullanıcının şu anda yazmakta olduğu.
* Azure Blob dizini ninin bir parçası olan [JsonLines ayrıştırma modu,](search-howto-index-json-blobs.md)JSON varlığı başına yeni bir satırla ayrılmış bir arama belgesi oluşturur.

### <a name="new-preview-features-in-version-80-preview"></a>Sürüm 8.0-önizlemedeki yeni önizleme özellikleri
Azure Arama .NET SDK sürümü 8.0-önizleme, API sürümünü hedefler 2017-11-11-Önizleme. Bu sürüm, sürüm 9'un tüm özelliklerini ve aşağıdakileri içerir:

* Hizmet tarafında şifreleme-at-rest için [müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) yeni bir önizleme özelliğidir. Microsoft tarafından yönetilen yerleşik şifreleme-at-rest ek olarak, anahtarların tek sahibi olduğunuz ek bir şifreleme katmanı uygulayabilirsiniz.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet `Microsoft.Azure.Search` Paket Yöneticisi Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun. Kodunuzu nasıl yapılandırıldığına bağlı olarak, başarılı bir şekilde yeniden oluşturulabilir. Eğer öyleyse, gitmeye hazırsın!

Yapınız başarısız olursa, her yapı hatasını düzeltmeniz gerekir. Her olası yapı hatasının nasıl çözüleceği yle ilgili ayrıntılar için [sürüm 9'daki son dakika değişikliklerine](#ListOfChanges) bakın.

Eski yöntemler veya özelliklerle ilgili ek yapı uyarıları görebilirsiniz. Uyarılar, amortismana uymak yerine ne kullanılacağına ilişkin yönergeleri içerir. Örneğin, uygulamanız özelliği `DataSourceType.DocumentDb` kullanıyorsa, "Bu üye amortismana uğradı. Bunun yerine CosmosDb kullanın".

Herhangi bir yapı hatasını veya uyarıyı düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK'daki yeni özellikler [9.](#WhatsNew)

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-9"></a>Sürüm 9'daki değişiklikleri kesme

Sürüm 9'da, uygulamanızı yeniden oluşturmaya ek olarak kod değişiklikleri gerektirebilecek birkaç kesme değişikliği vardır.

> [!NOTE]
> Aşağıdaki değişikliklerin listesi ayrıntılı değildir. Bazı değişiklikler büyük olasılıkla yapı hatalarına neden olmaz, ancak Azure Search .NET SDK derlemelerinin önceki sürümlerine bağlı olan derlemelerle ikili uyumluluğu bozdukları için teknik olarak bozulur. Bu tür değişiklikler aşağıda listelenmemiştir. İkili uyumluluk sorunlarını önlemek için lütfen sürüm 9'a yükseltirken uygulamanızı yeniden yeniden oluşturun.

### <a name="immutable-properties"></a>Değişmez özellikler

Çeşitli model sınıflarının ortak özellikleri artık değişmez. Sınama için bu sınıfların özel örneklerini oluşturmanız gerekiyorsa, yeni parametrelendirilmiş oluşturucuları kullanabilirsiniz:

  - `AutocompleteItem`
  - `DocumentSearchResult`
  - `DocumentSuggestResult`
  - `FacetResult`
  - `SearchResult`
  - `SuggestResult`

### <a name="changes-to-field"></a>Alandeğişiklikleri

Sınıf, `Field` karmaşık alanları da temsil edebileceği için değişti.

Aşağıdaki `bool` özellikler artık nullable vardır:

  - `IsFilterable`
  - `IsFacetable`
  - `IsSearchable`
  - `IsSortable`
  - `IsRetrievable`
  - `IsKey`

Bunun nedeni, bu özelliklerin artık karmaşık alanlar söz konusu olması `null` gerektiğidir. Bu özellikleri okuyan kodunuz varsa, işlemek `null`için hazır olması gerekir. Diğer tüm özelliklerinin `Field` her zaman geçersiz olduğunu ve olmaya devam ettiğini `null` ve bunlardan bazılarının karmaşık alanlar söz konusu olduğunda da - özellikle aşağıdakiler:

  - `Analyzer`
  - `SearchAnalyzer`
  - `IndexAnalyzer`
  - `SynonymMaps`

Parametresiz yapıcı `Field` yapılmıştır. `internal` Şu andan `Field` itibaren, her inşaat sırasında açık bir ad ve veri türü gerektirir.

### <a name="simplified-batch-and-results-types"></a>Basitleştirilmiş toplu iş ve sonuç türleri

Sürüm 7.0-preview ve daha önce, belge gruplarını kapsülleyen çeşitli sınıflar paralel sınıf hiyerarşileri halinde yapılandırılmıştır:

  -  `DocumentSearchResult`ve `DocumentSearchResult<T>` devralınan`DocumentSearchResultBase`
  -  `DocumentSuggestResult`ve `DocumentSuggestResult<T>` devralınan`DocumentSuggestResultBase`
  -  `IndexAction`ve `IndexAction<T>` devralınan`IndexActionBase`
  -  `IndexBatch`ve `IndexBatch<T>` devralınan`IndexBatchBase`
  -  `SearchResult`ve `SearchResult<T>` devralınan`SearchResultBase`
  -  `SuggestResult`ve `SuggestResult<T>` devralınan`SuggestResultBase`

Genel bir tür parametresi olmayan türlerden türemiş türleri "dinamik olarak yazılmış" `Document` senaryolarda ve türün kullanımı varsayılmıştır kullanılmak üzere amaçlandı.

Sürüm 8.0-preview ile başlayarak, temel sınıfların ve genel olmayan türemiş sınıfların tümü kaldırıldı. Dinamik olarak yazılan senaryolar için , `IndexBatch<Document>` `DocumentSearchResult<Document>`, ve benzeri kullanabilirsiniz.
 
### <a name="removed-extensibleenum"></a>Kaldırıldı EkstansibleEnum

`ExtensibleEnum` Taban sınıf kaldırıldı. Bundan türetilen tüm sınıflar artık , , `AnalyzerName` `DataType`ve `DataSourceType` örneğin gibi structs vardır. Yöntemleri `Create` de kaldırıldı. Bu tür dizeleri `Create` örtülü olarak dönüştürülebilir olduğundan aramaları kaldırabilirsiniz. Bu derleyici hatalarıyla sonuçlanırsa, disambiguate türlerine döküm yoluyla dönüşüm işlecinin çağrılmasını açıkça çağırabilirsiniz. Örneğin, kodu şu şekilde değiştirebilirsiniz:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", AnalyzerName.Create("my_email_analyzer")) { IsSearchable = true }
    },
    ...
}
```

şu şekilde:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", (AnalyzerName)"my_email_analyzer") { IsSearchable = true }
    },
    ...
}
```

Bu tür isteğe bağlı değerleri tutan özellikler artık açıkça nullable olarak yazılır, böylece isteğe bağlı olmaya devam ederler.

### <a name="removed-facetresults-and-hithighlights"></a>Kaldırıldı FacetResults ve HitHighlights

Ve `FacetResults` `HitHighlights` sınıflar kaldırıldı. Fason sonuçlar şimdi olarak `IDictionary<string, IList<FacetResult>>` yazılır `IDictionary<string, IList<string>>`ve hit vurgular olarak . Bu değişikliğin getirdiği yapı hatalarını çözmenin hızlı `using` bir yolu, kaldırılan türleri kullanan her dosyanın en üstüne takma adlar eklemektir. Örnek:

```csharp
using FacetResults = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<Models.FacetResult>>;
using HitHighlights = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<string>>;
```

### <a name="change-to-synonymmap"></a>Eş Anlamlı Haritaya Geçiş 

Oluşturucu `SynonymMap` artık bir `enum` parametreye `SynonymMapFormat`sahip değil. Bu enum un tek bir değeri vardı ve bu nedenle gereksiz oldu. Bunun sonucu olarak yapı hataları görürseniz, parametreye yapılan başvuruları kaldırmanız `SynonymMapFormat` yeterlidir.

### <a name="miscellaneous-model-class-changes"></a>Çeşitli model sınıfı değişiklikleri

Özelliği `AutocompleteMode` artık `AutocompleteParameters` geçersiz değildir. Bu özelliği atayan kodunuz `null`varsa, yalnızca kaldırabilirsiniz ve özellik otomatik olarak varsayılan değere başolarak başlatacaktır.

Bu oluşturucu otomatik `IndexAction` olarak oluşturulduğundan, parametrelerin oluşturucuya sırası değişti. Bunun yerine yapıcı kullanarak, biz fabrika `IndexAction.Upload`yöntemleri `IndexAction.Merge`kullanmanızı öneririz , , ve benzeri.

### <a name="removed-preview-features"></a>Kaldırılan önizleme özellikleri

Sürüm 8.0-önizlemeden sürüm 9'a yükseltiyorsanız, bu özellik hala önizlemede olduğundan müşteri tarafından yönetilen anahtarlarla şifrelemenin kaldırıldığını unutmayın. Özellikle, `EncryptionKey` özellikleri `Index` ve `SynonymMap` kaldırılmıştır.

Uygulamanız bu özelliğe sıkı bir bağımlılık içeriyorsa, Azure Arama .NET SDK'nın 9 sürümüne yükseltilemezsiniz. Sürüm 8.0-önizleme'yi kullanmaya devam edebilirsiniz. Ancak, **üretim uygulamalarında önizleme SDK'larını kullanmanızı önermediğimizi**lütfen unutmayın. Önizleme özellikleri yalnızca değerlendirme amaçlıdır ve değişebilir.

> [!NOTE]
> SDK'nın 8.0-önizleme sürümünü kullanarak şifreli dizinler veya eşanlamlı haritalar oluşturduysanız, şifreleme durumlarını olumsuz etkilemeden bunları kullanmaya ve SDK'nın 9 sürümünü kullanarak tanımlarını değiştirebilirsiniz. SDK'nın 9 sürümü `encryptionKey` özelliği REST API'ye göndermez ve sonuç olarak REST API kaynağın şifreleme durumunu değiştirmez. 

### <a name="behavioral-change-in-data-retrieval"></a>Veri almada davranış değişikliği

`Search`"Dinamik olarak yazılan" , `Suggest`veya `Get` tür `Document`örneklerini döndüren API'leri kullanıyorsanız, artık boş JSON dizilerini `string[]` `object[]` 'yerine ' e dizileştirdiklerini unutmayın.

## <a name="conclusion"></a>Sonuç
Azure Arama .NET SDK'yı kullanma hakkında daha fazla ayrıntıya ihtiyacınız [varsa,.NET Nasıl Sunulur'a](search-howto-dotnet-sdk.md)bakın.

SDK hakkındaki görüşlerinizi bekliyoruz. Sorunlarla karşılaşırsanız, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)konusunda bizden yardım istemekte çekinmeyin. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun dosyalayabilirsiniz. Sorun başlığınızı "[Azure Arama]" ile önek yaptığınızdan emin olun.

Azure Arama'yı kullandığınız için teşekkür ederiz!
