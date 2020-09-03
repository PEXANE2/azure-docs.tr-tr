---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 09/01/2020
ms.topic: include
ms.custom: include file, devx-track-dotnet, cog-serv-seo-aug-2020
ms.author: diberry
ms.openlocfilehash: fff4e8c43263dfcc49be6cb6269078643118e8df
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323267"
---
.NET için Language Understanding (LUSıS) istemci kitaplıklarını kullanın:
* Uygulama oluşturun
* Makine tarafından öğrenilen bir varlık (örneğin, örnek) ile bir amaç ekleyin
* Uygulamayı eğitme ve yayımlama
* Sorgu tahmini çalışma zamanı

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)  |  [Yazma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Authoring) ve [tahmin](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Runtime) kitaplığı kaynak kodu | [Yazma](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/) ve [tahmin](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) NuGet | [C# örneği](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/LanguageUnderstanding/sdk-3x//Program.cs)

## <a name="prerequisites"></a>Önkoşullar

* Geçerli [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) sürümü ve [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/).
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portal [bir Language Understanding yazma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) . Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı Language Understanding yazmak üzere bağlamak için [oluşturduğunuz](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz. Hizmeti denemek için ücretsiz fiyatlandırma katmanını ( `F0` ) kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `new` ad ile yeni bir konsol uygulaması oluşturmak için DotNet komutunu kullanın `language-understanding-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: `Program.cs` .

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin.

    ```dotnetcli
    cd language-understanding-quickstart
    ```

1. Uygulamayı ile oluşturabilirsiniz:

    ```dotnetcli
    dotnet build
    ```

    Derleme çıktısı hiçbir uyarı veya hata içermemelidir.

    ```console
    ...
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ...
    ```

### <a name="install-the-nuget-libraries"></a>NuGet kitaplıklarını yükler

Uygulama dizini içinde, aşağıdaki komutlarla .NET için Language Understanding (LUSıS) istemci kitaplıklarını yükleyeceksiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring --version 3.2.0-preview.3
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime --version 3.1.0-preview.1
```

## <a name="authoring-object-model"></a>Nesne modeli yazma

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) nesnesidir.

## <a name="code-examples-for-authoring"></a>Yazma için kod örnekleri

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* Uygulamalar- [oluşturma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet), [silme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet), [Yayımlama](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet)
* Örnek utterer- [ekleme](https://docs.microsoft.com//dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.addasync?view=azure-dotnet), [kimliğe göre silme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.deleteasync?view=azure-dotnet)
* Özellikler- [tümcecik listelerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.featuresextensions.addphraselistasync?view=azure-dotnet) yönetme
* Model- [amaçları](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) ve varlıkları yönetme
* Desen yönetme [desenleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.patternextensions?view=azure-dotnet)
* Eğitim- [uygulamayı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.trainversionasync?view=azure-dotnet) ve yoklama [durumunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet) eğitme
* [Sürümler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.versionsextensions?view=azure-dotnet) -kopyalama, dışarı aktarma ve silme ile yönetme

## <a name="prediction-object-model"></a>Tahmin nesnesi modeli

Language Understanding (LUSıS) tahmini çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesidir.

## <a name="code-examples-for-prediction-runtime"></a>Tahmin çalışma zamanı için kod örnekleri

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Hazırlama veya üretim yuvasına](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getslotpredictionasync?view=azure-dotnet) göre tahmin
* [Sürüme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getversionpredictionasync?view=azure-dotnet) göre tahmin


[!INCLUDE [Bookmark links to same article](sdk-code-examples.md)]

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Mevcut `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Add NuGet libraries to code file](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=Dependencies)]

## <a name="add-boilerplate-code"></a>Ortak kod ekleme

1. `Main`Zaman uyumsuz çağrılara izin vermek için yönteminin imzasını değiştirin:

    ```csharp
    public static async Task Main()
    ```

1. Aksi belirtilmedikçe kodun geri kalanını `Main` sınıfın yöntemine ekleyin `Program` .

## <a name="create-variables-for-the-app"></a>Uygulama için değişkenler oluşturma

İki değişken kümesi oluşturun: değiştirdiğiniz ilk küme, ikinci küme, kod örneğinde göründükleri gibi bırakır. 

1. Yazma anahtarınızı ve kaynak adlarınızı tutmak için değişkenler oluşturun.

    [!code-csharp[Variables you need to change](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=VariablesYouChange)]

1. Uç noktalarınızı, uygulama adınızı, sürümünüzü ve amaç adınızı tutmak için değişkenler oluşturun.

    [!code-csharp[Variables you don't need to change](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=VariablesYouDontNeedToChangeChange)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi oluşturun ve bir [Luisauthoringclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-csharp[Authenticate the client](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

Bir LUSıS uygulaması, amaçlar, varlıklar ve örnek işleme dahil olmak üzere doğal dil işleme (NLP) modelini içerir.

Bir [Applicationcreateobject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.applicationcreateobject?view=azure-dotnet)oluşturun. Ad ve dil kültürü gerekli özelliklerdir. [Apps. Addadsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet) yöntemini çağırın. Yanıt, uygulama KIMLIĞIDIR.

[!code-csharp[Create a LUIS app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AuthoringCreateApplication)]

## <a name="create-intent-for-the-app"></a>Uygulama için amaç oluştur
Bir LUıN uygulamasının modelindeki birincil nesne, amaç ' dır. Amaç, Kullanıcı utterlerinin bir gruplandırması ile _hizalanır._ Bir Kullanıcı soru sorabilir veya bir bot 'tan (veya başka bir istemci uygulamasından) belirli bir _amaçlanan_ yanıtı bulmak için bir ifade oluşturabilir. Bir uçuşmaya örnek olarak, hedef şehirdeki hava durumu hakkında bilgi isteyerek ve müşteri hizmetleri için iletişim bilgilerini soruyor.

Benzersiz bir amaç adıyla bir [modelcreateobject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modelcreateobject?view=azure-dotnet) oluşturun ve ardından uygulama kimliği, sürüm kimliği ve Modelcreateobject 'yi [model. addi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addintentasync?view=azure-dotnet) metoduna geçirin. Yanıt, amaç KIMLIĞIDIR.

`intentName`Değer, `OrderPizzaIntent` [uygulama Için değişkenlerinizi oluşturma](#create-variables-for-the-app) bölümünde değişkenlerin bir parçası olarak için sabit olarak kodlanır.

[!code-csharp[Create intent for the app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AddIntent)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlık oluşturma

Varlıklar gerekli olmasa da, çoğu uygulama içinde bulunur. Varlık, kullanıcının amaç bilgisini almak için gerekli olan bilgileri Kullanıcı aracılığıyla ayıklar. Her biri kendi veri dönüştürme nesnesi (DTO) modelleriyle birlikte, çok sayıda [önceden oluşturulmuş](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addprebuiltasync?view=azure-dotnet) ve özel varlık türü vardır.  Uygulamanıza eklenecek ortak önceden oluşturulmuş varlıklar [Number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md), [Ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Varlıkların bir amaç ile işaretlenmediğini bilmek önemlidir. Bunlar, genellikle birçok amaç için uygulanabilir. Yalnızca belirli bir amaç için örnek Kullanıcı utbotları işaretlenir.

Varlıklar için oluşturma yöntemleri [model](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) sınıfının bir parçasıdır. Her varlık türü, genellikle `model` [modeller](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models?view=azure-dotnet) ad alanındaki sözcüğü içeren kendi veri dönüştürme nesnesi (DTO) modeline sahiptir.

Varlık oluşturma kodu, alt varlıklara uygulanan alt varlıklar ve özelliklerle makine öğrenimi varlığı oluşturur `Quantity` .

:::image type="content" source="../media/quickstart-sdk/machine-learned-entity.png" alt-text="' Miktar ' alt varlıklarına uygulanan alt varlıklar ve özellikler ile makine öğrenimi varlığı, oluşturulan varlığı gösteren portaldan kısmi ekran görüntüsü.":::

[!code-csharp[Create entities for the app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AuthoringAddEntities)]

Bu alt varlığa Özellikler atamak için alt varlığın KIMLIĞINI bulmak üzere sınıfına aşağıdaki yöntemi kullanın.

[!code-csharp[Find subentity id](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AuthoringSortModelObject)]

## <a name="add-example-utterance-to-intent"></a>Amaca göre örnek ekleme

Bir utterance 'in amacı ve ayıklama varlıklarını tespit etmek için, uygulama için örneklere örnek gerekir. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlenmesi gerekmez.

Her örnek için tek bir nesne olan [Examplelabelobject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-dotnet) nesnelerinin bir listesini oluşturarak örnek bir parametre ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğüne sahip tüm varlıkları işaretlemelidir. Varlık değeri, örnek utterine 'nın metninde göründüğünden tam olarak olmalıdır.

:::image type="content" source="../media/quickstart-sdk/labeled-example-machine-learned-entity.png" alt-text="Portalda etiketlenmiş örneği gösteren kısmi ekran görüntüsü. ":::

[Örnekleri çağırın. Addadsync](https://docs.microsoft.com//dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.addasync?view=azure-dotnet) Ile uygulama kimliği, sürüm kimliği ve örnek.

[!code-csharp[Add example utterance to intent](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=AuthoringAddLabeledExamples)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, bu modelin bu sürümü için LUıN uygulamasının eğitililmesi gerekir. Eğitilen bir model [kapsayıcıda](../luis-container-howto.md)kullanılabilir veya hazırlama veya ürün yuvalarında [yayımlanabilir](../luis-how-to-publish-app.md) .

[Tren. Traınversionasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions?view=azure-dotnet) yöntemi, uygulama kimliği ve sürüm kimliği gerektirir.

Bu hızlı başlangıç gibi çok küçük bir model, çok hızlı bir şekilde eğitecektir. Üretim düzeyinde uygulamalar için, uygulamanın eğitim ne zaman veya ne zaman başarılı olduğunu öğrenmek için [Getstatusasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_TrainExtensions_GetStatusAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ITrain_System_Guid_System_String_System_Threading_CancellationToken_) yöntemine bir yoklama çağrısı içermesi gerekir. Yanıt, her bir nesne için ayrı bir durum içeren [Modeltrainingınfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-dotnet) nesnelerinin bir listesidir. Eğitimin tamamlandı olarak kabul edilmesi için tüm nesnelerin başarılı olması gerekir.

[!code-csharp[Train the app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=TrainAppVersion)]

## <a name="publish-app-to-production-slot"></a>Uygulamayı üretim yuvasına Yayımla

[Publishasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet) YÖNTEMINI kullanarak Luo uygulamasını yayımlayın. Bu, geçerli eğitilen sürümü uç noktada belirtilen yuvaya yayımlar. İstemci uygulamanız bu uç noktayı, amaç ve varlık ayıklama amacıyla Kullanıcı utbotları göndermek için kullanır.

[!code-csharp[Publish app to production slot](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=PublishVersion)]

## <a name="authenticate-the-prediction-runtime-client"></a>Tahmin çalışma zamanı istemcisinin kimliğini doğrulama

Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi kullanın ve bir [Luisruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!INCLUDE [Caution about using authoring key](caution-authoring-key.md)]

[!code-csharp[Authenticate the prediction runtime client](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=PredictionCreateClient)]


## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki kodu ekleyin.

Kullanıcı söylenişi, [PredictionRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-dotnet) nesnesinin bir parçasıdır.

**GetSlotPredictionAsync** yöntemi, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır.

[!code-csharp[Get prediction from runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/sdk-3x//Program.cs?name=QueryPredictionEndpoint)]

[!INCLUDE [Prediction JSON response](sdk-json.md)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` uygulama dizininizdeki komutla çalıştırın.

```dotnetcli
dotnet run
```
