---
title: Azure Bilişsel Arama .NET SDK sürümü10'a yükseltin
titleSuffix: Azure Cognitive Search
description: Kodu azure bilişsel arama .NET SDK sürüm 10'a eski sürümlerden geçirin. Yeniliklerin ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ad912eb0b26354d40a654a1c8782dfcb960235e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847514"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-10"></a>Azure Bilişsel Arama .NET SDK sürümü10'a yükseltin

[Azure Arama .NET SDK](https://aka.ms/search-sdk)sürümü 9.0 veya daha eski kullanıyorsanız, bu makale, sürüm 10'u kullanmak üzere uygulamanızı yükseltmenize yardımcı olur.

Azure Arama, sürüm 10'da Azure Bilişsel Arama olarak yeniden adlandırılmıştır, ancak ad alanları ve paket adları değişmez. SDK'nın önceki sürümleri (9.0 ve önceki) eski adı kullanmaya devam eder. Örnekler de dahil olmak üzere SDK'yı kullanma hakkında daha fazla bilgi için [,.NET Uygulamasından Azure Bilişsel Arama'nın nasıl kullanılacağına](search-howto-dotnet-sdk.md)bakın.

Sürüm 10 çeşitli özellikler ve hata düzeltmeleri ekler, REST API sürümü `2019-05-06`en son sürümü olarak aynı işlevsel seviyeye getirerek. Değişikliğin varolan kodu bozduğu durumlarda, [sorunu çözmek için gereken adımları](#UpgradeSteps)size ileteceğiz.

> [!NOTE]
> Sürüm 8.0 önizleme veya daha eski kullanıyorsanız, önce sürüm 9'a yükseltmeniz ve ardından sürüm 10'a yükseltmeniz gerekir. Talimatlar için [Azure Arama .NET SDK sürüm 9'a yükseltme](search-dotnet-sdk-migration-version-9.md) ye bakın.
>
> Arama hizmeti örneğiniz, en sonuncusu da dahil olmak üzere birkaç REST API sürümlerini destekler. Bir sürümü artık en son sürüm olmadığında kullanmaya devam edebilirsiniz, ancak en yeni sürümü kullanmak için kodunuzu geçirmenizi öneririz. REST API'sini kullanırken, api sürüm parametresi aracılığıyla her istekteki API sürümünü belirtmeniz gerekir. .NET SDK'yı kullanırken, kullandığınız SDK sürümü REST API'nin ilgili sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü desteklemek üzere yükseltilse bile bu kodu hiçbir değişiklik olmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-10"></a>Sürüm 10'daki yenilikler
Azure Bilişsel Arama .NET SDK sürümü 10, BU güncelleştirmelerle birlikte`2019-05-06`REST API'nin () en son genel olarak kullanılabilir sürümünü hedefler:

* İki yeni becerinin tanıtılması - [Koşullu beceri](cognitive-search-skill-conditional.md) ve [Metin Çeviri becerisi](cognitive-search-skill-text-translation.md).
* [Şekillendirici beceri](cognitive-search-skill-shaper.md) girişleri iç içe geçmiş bağlamlardan konsolidasyon alabilmek için yeniden yapılandırılmıştır. Daha fazla bilgi için bu [örnek JSON tanımına](https://docs.microsoft.com/azure/search/cognitive-search-skill-shaper#scenario-3-input-consolidation-from-nested-contexts)bakın.
* İki yeni [alan haritalama fonksiyonunun](search-indexer-field-mappings.md)eklenmesi:
    - [urlEncode](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#urlencode-function)
    - [urlDecode](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#urldecode-function)
* Bazı durumlarda, [dizinleyici yürütme durumunda](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) görünen hatalar ve uyarılar hata ayıklama yardımcı ek ayrıntılar olabilir. `IndexerExecutionResult`bu davranışı yansıtacak şekilde güncelleştirildi.
* Bir [beceri](cognitive-search-defining-skillset.md) içinde tanımlanan bireysel beceriler isteğe bağlı `name` olarak bir özellik belirterek tanımlanabilir.
* `ServiceLimits`[karmaşık türleri](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) için `IndexerExecutionInfo` sınırları gösterir ve ilgili dizinleyici sınırları/kotaları gösterir.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları

1. NuGet Paket Yöneticisi `Microsoft.Azure.Search` Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

2. NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun. 

3. Yapınız başarısız olursa, her yapı hatasını düzeltmeniz gerekir. Her olası yapı hatasının nasıl çözüleceği yle ilgili ayrıntılar için [sürüm 10'daki son dakika değişikliklerine](#ListOfChanges) bakın.

4. Herhangi bir yapı hatasını veya uyarıyı düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK'daki yeni özellikler [10.](#WhatsNew)

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-10"></a>Sürüm 10'daki değişiklikleri kesme

Sürüm 10'da, uygulamanızı yeniden oluşturmaya ek olarak kod değişiklikleri gerektirebilecek birkaç kesme değişikliği vardır.

> [!NOTE]
> Aşağıdaki değişikliklerin listesi ayrıntılı değildir. Bazı değişiklikler büyük olasılıkla yapı hatalarına neden olmaz, ancak Azure Bilişsel Arama .NET SDK derlemelerinin önceki sürümlerine bağlı olan derlemelerle ikili uyumluluğu bozdukları için teknik olarak bozulur. Bu kategoriye giren önemli değişiklikler de önerilerle birlikte listelenir. İkili uyumluluk sorunlarını önlemek için lütfen sürüm 10'a yükseltirken uygulamanızı yeniden yeniden oluşturun.

### <a name="custom-web-api-skill-definition"></a>Özel Web API beceri tanımı

Özel Web [API becerisinin](cognitive-search-custom-skill-web-api.md) tanımı sürüm 9 ve daha büyük yanlış olarak belirtilmiştir. 

Sözlük _içeren_ `HttpHeaders` bir nesne özelliği olarak `WebApiSkill` belirtilen model. REST API isteği `WebApiSkill` kötü biçimlendirilmiş dikkate alacağını, çünkü bu şekilde inşa edilmiş bir beceri oluşturmak bir istisna ile sonuçlanır. Bu sorun, modelin `HttpHeaders` `WebApiSkill` kendisi üzerinde bir üst düzey sözlük **özelliği** yaparak düzeltilmiştir - rest API geçerli bir istek olarak kabul edilir.

Örneğin, daha önce aşağıdaki gibi bir `WebApiSkill` anlık girişiminde bulunduysanız:

```csharp

var webApiSkill = new WebApiSkill(
            inputs, 
            outputs,
            uri: "https://contoso.example.org")
{
    HttpHeaders = new WebApiHttpHeaders()
    {
        Headers = new Dictionary<string, string>()
        {
            ["header"] = "value"
        }
    }
};

```

REST API doğrulama hatası önlemek için aşağıdaki leri değiştirin:

```csharp

var webApiSkill = new WebApiSkill(
            inputs, 
            outputs,
            uri: "https://contoso.example.org")
{
    HttpHeaders = new Dictionary<string, string>()
    {
        ["header"] = "value"
    }
};

```

## <a name="shaper-skill-allows-nested-context-consolidation"></a>Şekillendirici becerisi iç içe bağlam konsolidasyonu sağlar

Şekillendirici becerisi artık iç içe geçmiş bağlamlardan giriş konsolidasyonuna izin verebilir. Bu değişikliği etkinleştirmek `InputFieldMappingEntry` için, sadece bir `Source` özellik veya hem de `SourceContext` `Inputs` özellikleri belirterek anında olabilir şekilde modifiye.

Büyük olasılıkla herhangi bir kod değişikliği yapmanız gerekmez; ancak bu iki kombinasyondan yalnızca birine izin verildiğini unutmayın. Bu şu anlama gelir:

- Yalnızca `Source` `InputFieldMappingEntry` başharflerinin başlattığı bir yer oluşturmak geçerlidir.
- Yalnızca `InputFieldMappingEntry` `SourceContext` ve `Inputs` başharflerinin başlandığı bir yer oluşturmak geçerlidir.
- Bu üç özelliği içeren diğer tüm kombinasyonlar geçersizdir.

Bu yeni yeteneği kullanmaya başlamaya karar verirseniz, bu değişikliği başlatmadan önce tüm müşterilerinizin sürüm 10'u kullanmak üzere güncelleştirdiğinden emin olun. Aksi takdirde, bir istemci (SDK eski bir sürümünü kullanarak) Shaper beceri bir güncelleştirme doğrulama hataları neden olabilir olasılığı vardır.

> [!NOTE]
> Temel `InputFieldMappingEntry` model iç içe geçmiş bağlamlardan konsolidasyona izin verecek şekilde değiştirilmiş olsa da, kullanımı yalnızca Şekillendirici becerisi nin tanımı içinde geçerlidir. Bu yeteneği başka becerilerde kullanmak, derleme zamanında geçerli olmakla birlikte, çalışma zamanında bir doğrulama hatasına neden olur.

## <a name="skills-can-be-identified-by-a-name"></a>Beceriler bir adla tanımlanabilir

Bir beceri içinde her beceri şimdi `Name`yeni bir özelliği vardır , hangi beceri belirlemenize yardımcı olmak için kodunuzda baş harfe olabilir. Bu isteğe bağlıdır - belirtilmemişse (açık bir kod değişikliği yapılmamışsa varsayılan addır), becerideki becerinin 1 tabanlı dizinini kullanarak varsayılan bir ad atanır ve '#' karakteriyle önceden belirlenmiştir. Örneğin, aşağıdaki skillset tanımında (kısaltma için atlanan çoğu başlatma):

```csharp
var skillset = new Skillset()
{
    Skills = new List<Skill>()
    {
        new SentimentSkill(),
        new WebApiSkill(),
        new ShaperSkill(),
        ...
    }
}
```

`SentimentSkill``#1`atanır, `WebApiSkill` atanır, `#2` `ShaperSkill` atanır `#3` ve böyle devam edin.

Becerileri özel bir ada göre tanımlamayı seçerseniz, müşterilerinizin tüm örneklerini önce SDK'nın 10. Aksi takdirde, SDK'nın eski bir sürümünü kullanan `null` bir `Name` istemcinin bir becerinin özelliğini çıkararak istemcinin varsayılan adlandırma düzenine geri dönmesine neden olma olasılığı vardır.

## <a name="details-about-errors-and-warnings"></a>Hatalar ve uyarılar hakkında ayrıntılar

`ItemError`ve `ItemWarning` dizinleyici yürütme sırasında ortaya çıkan hataların ve uyarıların ayrıntılarını (sırasıyla) kapsülleyen modeller, dizinleyicinin hata ayıklanmasına yardımcı olmak amacıyla üç yeni özellik içerecek şekilde değiştirilmiştir. Bu özellikler şunlardır:

- `Name`: Hatanın kaynaklandığı kaynağın adı. Örneğin, bağlı skillset belirli bir beceri anlamına gelebilir.
- `Details`: Hata veya uyarı ile ilgili ek ayrıntılı ayrıntılar.
- `DocumentationLink`: Belirli bir hata veya uyarı için sorun giderme kılavuzuna bağlantı.

> [!NOTE]
> Hatalarımızı ve uyarılarımızı mümkün olduğunca bu yararlı ayrıntıları içerecek şekilde yapılandırmaya başladık. Tüm hatalar ve uyarılar için bu ayrıntıların mevcut olduğundan emin olmak için çalışıyoruz, ancak bu devam eden bir çalışmadır ve bu ek ayrıntılar her zaman doldurulmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Şekillendirici becerisindeki değişiklikler, yeni veya varolan kod üzerinde en büyük potansiyel etkiye sahiptir. Bir sonraki adım olarak, giriş yapısını gösteren bu örneği yeniden gözden geçirin: [Shaper beceri JSON tanımı örneği](cognitive-search-skill-shaper.md)
- [AI zenginleştirme genel bakış](cognitive-search-concept-intro.md)üzerinden gidin.
- SDK hakkındaki görüşlerinizi bekliyoruz. Sorunlarla karşılaşırsanız, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)konusunda bizden yardım istemekte çekinmeyin. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun dosyalayabilirsiniz. Sorun başlığınızı "[Azure Bilişsel Arama]" ile önek yaptığınızdan emin olun.

