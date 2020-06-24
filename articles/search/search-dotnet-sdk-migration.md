---
title: Azure Search .NET SDK sürüm 3 ' e yükseltin
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Search .NET SDK sürüm 3 ' e geçirin. Nelerin yeni olduğunu ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 548cd488bc811ad16cd84950ce3819f2e1f3ddbb
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85080724"
---
# <a name="upgrade-to-azure-search-net-sdk-version-3"></a>Azure Search .NET SDK sürüm 3 ' e yükseltin

<!--- DETAILS in the word doc
cosmosdb
NER v1 skill 
Indexer execution result errors no longer have status
the data source API will no longer return in the response of any REST operation, the connection string specified by the user.
--->

[Azure Search .NET SDK 'sının](https://docs.microsoft.com/dotnet/api/overview/azure/search)sürüm 2,0-Önizleme veya daha eski bir sürümünü kullanıyorsanız, bu makale uygulamanızı sürüm 3 ' ü kullanacak şekilde yükseltmenize yardımcı olur.

Örnek içeren SDK hakkında daha genel bir anlatım için bkz. [.NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

Azure Search .NET SDK 'nın 3. sürümü önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla düşüktür, bu nedenle kodunuzun değiştirilmesi yalnızca en az çaba gerektirir. Kodunuzu yeni SDK sürümünü kullanacak şekilde değiştirme hakkında yönergeler için bkz. [yükseltme adımları](#UpgradeSteps) .

> [!NOTE]
> Sürüm 1.0.2-Preview veya daha eski bir sürümünü kullanıyorsanız, önce sürüm 1,1 ' e yükseltmeniz ve ardından sürüm 3 ' e yükseltmeniz gerekir. Yönergeler için bkz. [Azure Search .NET SDK sürüm 1,1 ' e yükseltme](search-dotnet-sdk-migration-version-1.md) .
>
> Azure Search hizmet örneğiniz, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için geçirmeniz önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Sürüm 3 ' teki yenilikler
Azure Search .NET SDK 'nın 3. sürümü, Azure Search REST API, özellikle 2016-09-01 olan en son genel kullanılabilir sürümünü hedefler. Bu, aşağıdakiler de dahil olmak üzere bir .NET uygulamasından Azure Search birçok yeni özelliği kullanmayı mümkün kılar:

* [Özel çözümleyiciler](index-add-custom-analyzers.md)
* [Azure Blob depolama](search-howto-indexing-azure-blob-storage.md) ve [Azure Tablo depolama](search-howto-indexing-azure-tables.md) Dizin Oluşturucu desteği
* [Alan eşlemeleri](search-indexer-field-mappings.md) aracılığıyla Dizin Oluşturucu özelleştirmesi
* ETags, Dizin tanımlarının, dizin oluşturucularının ve veri kaynaklarının güvenli eşzamanlı güncelleştirilmesini sağlamak için desteklenir
* Model sınıfınızı dekorasyon ve yeni sınıfı kullanarak Dizin alanı tanımlarını bildirimli olarak oluşturma desteği `FieldBuilder` .
* .NET Core ve .NET taşınabilir profili 111 için destek

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet ' i `Microsoft.Azure.Search` Paket Yöneticisi konsolu 'nu kullanarak veya proje başvurularınızı sağ tıklatıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek NuGet başvurunuz ' ı güncelleştirin. Visual Studio 'da.

NuGet yeni paketleri ve bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzun nasıl yapılandırıldığına bağlı olarak, başarıyla yeniden oluşturulabilir. Öyleyse başlamaya hazırsınız demektir!

Derlemeniz başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Sonraki adım bu derleme hatasını düzeltemedi. Hataya neden olan ve nasıl düzeltileceğini gösteren Ayrıntılar için [sürüm 3 ' teki son değişikliklere](#ListOfChanges) bakın.

Eski yöntemler veya özelliklerle ilgili ek derleme uyarıları görebilirsiniz. Uyarılar, kullanım dışı özellik yerine, nelerin kullanılacağı hakkında yönergeler içerir. Örneğin, uygulamanız `IndexingParameters.Base64EncodeKeys` özelliğini kullanıyorsa, şöyle bir uyarı alırsınız`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Herhangi bir derleme hatasını düzelttikten sonra, isterseniz yeni işlevlerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK 'daki yeni özellikler, [sürüm 3 ' teki](#WhatsNew)Yenilikler bölümünde ayrıntılı olarak açıklanmıştır.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Sürüm 3 ' teki son değişiklikler
Sürüm 3 ' te, uygulamanızı yeniden oluşturmak için ek olarak kod değişikliği gerektirebilecek az sayıda Son değişiklik vardır.

### <a name="indexesgetclient-return-type"></a>Indexes. GetClient dönüş türü
`Indexes.GetClient`Yöntemin yeni bir dönüş türü vardır. Daha önce döndürüldü, `SearchIndexClient` ancak bu, `ISearchIndexClient` sürüm 2,0-Önizleme sürümünde olarak değiştirilmiştir ve bu değişiklik 3 ' e kadar sürer. Bu, ' `GetClient` nin bir sahte uygulamasını döndürerek birim testleri için yöntemini sahte yapmak isteyen müşterileri desteklemek içindir `ISearchIndexClient` .

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>Analiz Zeradı, veri türü ve diğerleri artık dizelere örtük olarak dönüştürülebilir değildir
Azure Search .NET SDK 'sında türetilmiş birçok tür vardır `ExtensibleEnum` . Daha önce bu türlerin türüne örtük olarak dönüştürülebilir `string` . Ancak, bu sınıfların uygulamasında bir hata keşfedildi `Object.Equals` ve bu örtük dönüştürmeyi devre dışı bırakmak için gereken hata düzeltiliyor. Öğesine açık dönüştürmeye `string` hala izin verilir.

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

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

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

### <a name="removed-obsolete-members"></a>Artık kullanılmayan Üyeler kaldırıldı

Sürüm 2,0 ' de eski olarak işaretlenen ve sürüm 3 ' te kaldırılan yöntemlerle veya özelliklerle ilgili derleme hataları görebilirsiniz. Bu tür hatalarla karşılaşırsanız, bu sorunları çözmek için aşağıdaki adımları uygulayın:

- Bu oluşturucuyu kullanıyorsanız `ScoringParameter(string name, string value)` , bunun yerine kullanın:`ScoringParameter(string name, IEnumerable<string> values)`
- `ScoringParameter.Value`Özelliğini kullanıyorsanız, `ScoringParameter.Values` `ToString` bunun yerine özelliğini veya yöntemini kullanın.
- `SearchRequestOptions.RequestId`Özelliğini kullanıyorsanız, `ClientRequestId` bunun yerine özelliğini kullanın.

### <a name="removed-preview-features"></a>Önizleme özellikleri kaldırıldı

Sürüm 2,0 ' den sürüm 3 ' e yükseltiyorsanız, bu özellikler hala önizlemede olduğundan blob Dizin oluşturucular için JSON ve CSV ayrıştırma desteğinin kaldırıldığını unutmayın. Özellikle, sınıfının aşağıdaki yöntemleri `IndexingParametersExtensions` kaldırılmıştır:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanızın bu özelliklere sabit bir bağımlılığı varsa, Azure Search .NET SDK 'sının 3. sürümüne yükseltme yapamazsınız. 2,0-Preview sürümünü kullanmaya devam edebilirsiniz. Ancak, lütfen **Üretim uygulamalarında önizleme SDK 'larını kullanmanızı önermiyoruz**. Önizleme özellikleri yalnızca değerlendirme amaçlıdır ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK 'yı kullanma hakkında daha fazla ayrıntıya ihtiyacınız varsa bkz. [.NET nasıl yapılır](search-howto-dotnet-sdk.md).

SDK 'daki geri bildirimlerinize hoş geldiniz. Sorunlarla karşılaşırsanız [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)hakkında yardım almak için bize danışabilirsiniz. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun oluşturabilirsiniz. Sorun başlığınız "[Azure Search]" ile öneklediğinizden emin olun.

Azure Search kullandığınız için teşekkürler!
