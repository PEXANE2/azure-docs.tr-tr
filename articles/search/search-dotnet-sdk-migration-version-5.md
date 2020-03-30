---
title: Azure Arama .NET SDK sürümü 5'e yükseltin
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Arama .NET SDK sürüm 5'e geçirin. Yeniliklerin ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bb0cd191ba7e5939c55d11b484ed7a2c422f8c6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793032"
---
# <a name="upgrade-to-azure-search-net-sdk-version-5"></a>Azure Arama .NET SDK sürümü 5'e yükseltin

[Azure Arama .NET SDK](https://aka.ms/search-sdk)sürümü 4.0 önizlemesini veya daha eski sürümünü kullanıyorsanız, bu makale, sürüm 5'i kullanmak üzere uygulamanızı yükseltmenize yardımcı olur.

Örnekler de dahil olmak üzere SDK'nın daha genel bir gözden geçirme için [,.NET Uygulamasından Azure Arama'nın nasıl kullanılacağına](search-howto-dotnet-sdk.md)bakın.

Azure Arama .NET SDK sürümü 5 önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla küçüktür, bu nedenle kodunuzu değiştirmek yalnızca en az çaba yı gerektirmelidir. Yeni SDK sürümünü kullanmak için kodunuzu nasıl değiştireceğinize ilişkin yönergeler için [yükseltme adımları'na](#UpgradeSteps) bakın.

> [!NOTE]
> Sürüm 2.0 önizleme veya daha eski kullanıyorsanız, önce sürüm 3'e yükseltmeniz ve ardından sürüm 5'e yükseltmeniz gerekir. Talimatlar için [Azure Arama .NET SDK sürüm 3'e yükseltme](search-dotnet-sdk-migration.md) ye bakın.
>
> Azure Arama hizmeti örneğiniz, en sonuncusu da dahil olmak üzere birkaç REST API sürümü destekler. Bir sürümü artık en son sürüm olmadığında kullanmaya devam edebilirsiniz, ancak en yeni sürümü kullanmak için kodunuzu geçirmenizi öneririz. REST API'sini kullanırken, api sürüm parametresi aracılığıyla her istekteki API sürümünü belirtmeniz gerekir. .NET SDK'yı kullanırken, kullandığınız SDK sürümü REST API'nin ilgili sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü desteklemek üzere yükseltilse bile bu kodu hiçbir değişiklik olmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Sürüm 5'teki yenilikler
Azure Arama .NET SDK sürümü 5, Azure Arama REST API'sinin özellikle 2017-11-11 sürümü yle genel olarak kullanılabilir olan en son sürümünü hedefler. Bu, bir .NET uygulamasından Azure Arama'nın yeni özelliklerini aşağıdakiler de dahil olmak üzere kullanmayı mümkün kılar:

* [Eş anlamlılar](search-synonyms.md).
* Artık dizinleyici yürütme geçmişindeki uyarılara programlı `Warning` olarak `IndexerExecutionResult` erişebilirsiniz (daha fazla ayrıntı için [.NET başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) özelliğine bakın).
* .NET Core 2 desteği.
* Yeni paket yapısı, SDK'nın yalnızca ihtiyacınız olan bölümlerini kullanmayı destekler (ayrıntılar için [sürüm 5'teki son dakika değişikliklerine](#ListOfChanges) bakın).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet `Microsoft.Azure.Search` Paket Yöneticisi Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun. Kodunuzu nasıl yapılandırıldığına bağlı olarak, başarılı bir şekilde yeniden oluşturulabilir. Eğer öyleyse, gitmeye hazırsın!

Yapınız başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

    The name 'SuggesterSearchMode' does not exist in the current context

Bir sonraki adım, bu yapı hatasını düzeltmektir. Hataya neyin neden olduğu ve nasıl düzeltilenle ilgili ayrıntılar için [sürüm 5'teki değişiklikleri](#ListOfChanges) kesme ye bakın.

Azure Search .NET SDK'nın ambalajındaki değişiklikler nedeniyle, sürüm 5'i kullanmak için uygulamanızı yeniden oluşturmanız gerektiğini lütfen unutmayın. Bu değişiklikler [sürüm 5'teki Breaking değişikliklerinde](#ListOfChanges)ayrıntılı olarak açıklanmaktadır.

Eski yöntemler veya özelliklerle ilgili ek yapı uyarıları görebilirsiniz. Uyarılar, amortismana uymak yerine ne kullanılacağına ilişkin yönergeleri içerir. Örneğin, uygulamanız yöntemi `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` kullanıyorsa, "Bu davranış artık varsayılan olarak etkinleştirildi, bu nedenle bu yöntemi aramak artık gerekli değildir" yazan bir uyarı almalısınız.

Herhangi bir yapı hatasını veya uyarıyı düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK'daki yeni özellikler [sürüm 5'teki yeniliklerde](#WhatsNew)ayrıntılı olarak açıklanmaktadır.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Sürüm 5'teki değişiklikleri kesme

### <a name="new-package-structure"></a>Yeni Paket Yapısı

Sürüm 5'teki en önemli kırılma `Microsoft.Azure.Search` değişimi, derlemenin ve içeriğinin şimdi dört ayrı NuGet paketi olarak dağıtılan dört ayrı derlemeye ayrılmış olmasıdır:

 - `Microsoft.Azure.Search`: Bu, diğer tüm Azure Arama paketlerini bağımlılık olarak içeren bir meta paketidir. SDK'nın önceki bir sürümünden yükseltme alıyorsanız, bu paketi yükseltmeniz ve yeniden oluşturmanız yeni sürümü kullanmaya başlamak için yeterli olacaktır.
 - `Microsoft.Azure.Search.Data`: Azure Arama'yı kullanarak bir .NET uygulaması geliştiriyorsanız ve yalnızca dizinlerinizdeki belgeleri sorgulamanız veya güncelleştirmeniz gerekiyorsa bu paketi kullanın. Dizinler, eşanlamlı haritalar veya diğer hizmet düzeyi kaynakları oluşturmanız veya güncelleştirmeniz gerekiyorsa, `Microsoft.Azure.Search` bunun yerine paketi kullanın.
 - `Microsoft.Azure.Search.Service`: Azure Arama dizinlerini, eşanlamlı haritaları, dizin leyicileri, veri kaynaklarını veya diğer hizmet düzeyi kaynakları yönetmek için .NET'te otomasyon geliştiriyorsanız bu paketi kullanın. Yalnızca dizinlerinizdeki belgeleri sorgulamanız veya güncelleştirmeniz `Microsoft.Azure.Search.Data` gerekiyorsa, bunun yerine paketi kullanın. Azure Arama'nın tüm işlevlerine ihtiyacınız `Microsoft.Azure.Search` varsa, bunun yerine paketi kullanın.
 - `Microsoft.Azure.Search.Common`: Azure Arama .NET kitaplıkları tarafından gereken yaygın türler. Bu paketi doğrudan uygulamanızda kullanmanız gerekmez; Sadece bir bağımlılık olarak kullanılmak üzere dir.
 
Birçok tür derlemeler arasında taşındığından, bu değişiklik teknik olarak kırılıyor. Bu nedenle, SDK'nın sürüm 5'ine yükseltmek için uygulamanızı yeniden oluşturmanız gereklidir.

Sürüm 5'te, uygulamanızı yeniden oluşturmaya ek olarak kod değişiklikleri gerektirebilecek az sayıda başka kesme değişikliği vardır.

### <a name="change-to-suggesters"></a>Öneriyi Zekçilere Değiştir 

Oluşturucu `Suggester` artık bir `enum` parametreye `SuggesterSearchMode`sahip değil. Bu enum un tek bir değeri vardı ve bu nedenle gereksiz oldu. Bunun sonucu olarak yapı hataları görürseniz, parametreye yapılan başvuruları kaldırmanız `SuggesterSearchMode` yeterlidir.

### <a name="removed-obsolete-members"></a>Eski üyeler kaldırıldı

Önceki sürümlerde geçersiz olarak işaretlenmiş ve daha sonra sürüm 5'te kaldırılan yöntemler veya özelliklerle ilgili yapı hataları görebilirsiniz. Bu tür hatalarla karşılaşırsanız, bunları şu şekilde çözebilirsiniz:

- `IndexingParametersExtensions.IndexStorageMetadataOnly` Yöntemi kullanıyorsanız, bunun `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` yerine kullanın.
- `IndexingParametersExtensions.SkipContent` Yöntemi kullanıyorsanız, bunun `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` yerine kullanın.

### <a name="removed-preview-features"></a>Kaldırılan önizleme özellikleri

Sürüm 4.0-önizlemeden sürüm 5'e yükseltiyorsanız, Bu özellikler hala önizlemede olduğundan, Blob Indexers için JSON dizi ve CSV ayrıştırma desteğinin kaldırıldığını unutmayın. Özellikle, `IndexingParametersExtensions` sınıfın aşağıdaki yöntemleri kaldırıldı:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanız bu özelliklere sıkı bir bağımlılık içeriyorsa, Azure Arama .NET SDK sürümüne yükseltilemezsiniz. Sürüm 4.0-önizleme'yi kullanmaya devam edebilirsiniz. Ancak, **üretim uygulamalarında önizleme SDK'larını kullanmanızı önermediğimizi**lütfen unutmayın. Önizleme özellikleri yalnızca değerlendirme amaçlıdır ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Arama .NET SDK'yı kullanma hakkında daha fazla ayrıntıya ihtiyacınız [varsa,.NET Nasıl Sunulur'a](search-howto-dotnet-sdk.md)bakın.

SDK hakkındaki görüşlerinizi bekliyoruz. Sorunlarla karşılaşırsanız, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)konusunda bizden yardım istemekte çekinmeyin. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun dosyalayabilirsiniz. Sorun başlığınızı "[Azure Arama]" ile önek yaptığınızdan emin olun.

Azure Arama'yı kullandığınız için teşekkür ederiz!
