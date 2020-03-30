---
title: Azure Arama .NET SDK sürümü3'e yükseltin
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Arama .NET SDK sürüm 3'e geçirin. Yeniliklerin ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fcad05749892e3a652e110a7e351450bffaca6f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792975"
---
# <a name="upgrade-to-azure-search-net-sdk-version-3"></a>Azure Arama .NET SDK sürümü3'e yükseltin

<!--- DETAILS in the word doc
cosmosdb
NER v1 skill 
Indexer execution result errors no longer have status
the data source API will no longer return in the response of any REST operation, the connection string specified by the user.
--->

[Azure Arama .NET SDK](https://aka.ms/search-sdk)sürümü 2.0 önizlemesini veya daha eski sürümünü kullanıyorsanız, bu makale, uygulamanızı sürüm 3'ü kullanmak üzere yükseltmenize yardımcı olur.

Örnekler de dahil olmak üzere SDK'nın daha genel bir gözden geçirme için [,.NET Uygulamasından Azure Arama'nın nasıl kullanılacağına](search-howto-dotnet-sdk.md)bakın.

Azure Arama .NET SDK sürümü 3 önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla küçüktür, bu nedenle kodunuzu değiştirmek yalnızca en az çaba yı gerektirmelidir. Yeni SDK sürümünü kullanmak için kodunuzu nasıl değiştireceğinize ilişkin yönergeler için [yükseltme adımları'na](#UpgradeSteps) bakın.

> [!NOTE]
> Sürüm 1.0.2-önizleme veya daha eski kullanıyorsanız, önce sürüm 1.1'e yükseltmeniz ve ardından sürüm 3'e yükseltmeniz gerekir. Talimatlar için [Azure Arama .NET SDK sürüm 1.1'e yükseltme](search-dotnet-sdk-migration-version-1.md) ye bakın.
>
> Azure Arama hizmeti örneğiniz, en sonuncusu da dahil olmak üzere birkaç REST API sürümü destekler. Bir sürümü artık en son sürüm olmadığında kullanmaya devam edebilirsiniz, ancak en yeni sürümü kullanmak için kodunuzu geçirmenizi öneririz. REST API'sini kullanırken, api sürüm parametresi aracılığıyla her istekteki API sürümünü belirtmeniz gerekir. .NET SDK'yı kullanırken, kullandığınız SDK sürümü REST API'nin ilgili sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü desteklemek üzere yükseltilse bile bu kodu hiçbir değişiklik olmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Sürüm 3'teki yenilikler
Azure Arama .NET SDK sürümü 3, Azure Arama REST API'sinin özellikle 2016-09-01 olmak üzere genel olarak kullanılabilen en son sürümünü hedefler. Bu, bir .NET uygulamasından Azure Arama'nın birçok yeni özelliğinin kullanımını mümkün kılar:

* [Özel çözümleyiciler](https://aka.ms/customanalyzers)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md) ve [Azure Tablo Depolama](search-howto-indexing-azure-tables.md) dizinleyici desteği
* [Alan eşlemeleri](search-indexer-field-mappings.md) ile Dizinleyici özelleştirme
* Dizin tanımlarının, dizin oluşturma nın ve veri kaynaklarının güvenli bir şekilde eşzamanlı olarak güncellenmesini sağlamak için ETags desteği
* Model sınıfınızı süsleyerek ve yeni `FieldBuilder` sınıfı kullanarak dizin alan tanımlarını bildirimsel olarak oluşturma desteği.
* .NET Core ve .NET Taşınabilir Profil 111 desteği

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet `Microsoft.Azure.Search` Paket Yöneticisi Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun. Kodunuzu nasıl yapılandırıldığına bağlı olarak, başarılı bir şekilde yeniden oluşturulabilir. Eğer öyleyse, gitmeye hazırsın!

Yapınız başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Bir sonraki adım, bu yapı hatasını düzeltmektir. Hataya neyin neden olduğu ve nasıl düzeltilenle ilgili ayrıntılar için [sürüm 3'teki değişiklikleri kesme](#ListOfChanges) ye bakın.

Eski yöntemler veya özelliklerle ilgili ek yapı uyarıları görebilirsiniz. Uyarılar, amortismana uymak yerine ne kullanılacağına ilişkin yönergeleri içerir. Örneğin, uygulamanız özelliği `IndexingParameters.Base64EncodeKeys` kullanıyorsa,`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Herhangi bir yapı hatasını düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK'daki yeni özellikler [sürüm 3'teki yeniliklerde](#WhatsNew)ayrıntılı olarak açıklanmaktadır.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Sürüm 3'teki değişiklikleri kesme
Uygulamanızı yeniden oluşturmaya ek olarak kod değişiklikleri gerektirebilecek sürüm 3'te az sayıda son dakika değişikliği vardır.

### <a name="indexesgetclient-return-type"></a>Dizinler.GetClient iade türü
Yöntemin `Indexes.GetClient` yeni bir iade türü vardır. Daha önce, `SearchIndexClient`döndürülür , ancak `ISearchIndexClient` bu sürüm 2.0-önizleme olarak değiştirildi ve bu değişiklik sürüm 3'e taşınır. Bu, birim testleri için `GetClient` yöntemle alay etmek isteyen müşterileri, `ISearchIndexClient`sahte bir uygulama döndürerek desteklemektir.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, DataType ve diğerleri artık dolaylı olarak dizeleri dönüştürülebilir
Azure Arama .NET SDK'da ' dan `ExtensibleEnum`türetilen birçok türü vardır. Daha önce bu tür tüm örtülü `string`türüne dönüştürülebilir edildi. Ancak, bu sınıflar için `Object.Equals` uygulamada bir hata keşfedildi ve hata sabitleme bu örtülü dönüşüm devre dışı bırakılması gerekli. Açık dönüştürmeye `string` hala izin verilir.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Eski üyeler kaldırıldı

Sürüm 2.0 önizlemesinde eski olarak işaretlenmiş ve daha sonra sürüm 3'te kaldırılan yöntemler veya özelliklerle ilgili yapı hataları görebilirsiniz. Bu tür hatalarla karşılaşırsanız, bunları şu şekilde çözebilirsiniz:

- Bu oluşturucu kullanıyorsanız: `ScoringParameter(string name, string value)`, bunun yerine bu kullanın:`ScoringParameter(string name, IEnumerable<string> values)`
- `ScoringParameter.Value` Özelliği kullanıyorsanız, `ScoringParameter.Values` bunun yerine özelliği `ToString` veya yöntemi kullanın.
- `SearchRequestOptions.RequestId` Özelliği kullanıyorsanız, `ClientRequestId` bunun yerine özelliği kullanın.

### <a name="removed-preview-features"></a>Kaldırılan önizleme özellikleri

Sürüm 2.0-önizlemeden sürüm 3'e yükseltiyorsanız, Bu özellikler hala önizlemede olduğundan Blob Indexers için JSON ve CSV ayrıştırma desteğinin kaldırıldığını unutmayın. Özellikle, `IndexingParametersExtensions` sınıfın aşağıdaki yöntemleri kaldırıldı:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanız bu özelliklere sıkı bir bağımlılık içeriyorsa, Azure Arama .NET SDK'nın 3 sürümüne yükseltilemezsiniz. Sürüm 2.0-önizleme'yi kullanmaya devam edebilirsiniz. Ancak, **üretim uygulamalarında önizleme SDK'larını kullanmanızı önermediğimizi**lütfen unutmayın. Önizleme özellikleri yalnızca değerlendirme amaçlıdır ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Arama .NET SDK'yı kullanma hakkında daha fazla ayrıntıya ihtiyacınız [varsa,.NET Nasıl Sunulur'a](search-howto-dotnet-sdk.md)bakın.

SDK hakkındaki görüşlerinizi bekliyoruz. Sorunlarla karşılaşırsanız, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)konusunda bizden yardım istemekte çekinmeyin. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun dosyalayabilirsiniz. Sorun başlığınızı "[Azure Arama]" ile önek yaptığınızdan emin olun.

Azure Arama'yı kullandığınız için teşekkür ederiz!
