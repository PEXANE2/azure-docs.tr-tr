---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 53e6382cf8d046b2c9818b906890bc64642fd2ed
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77372031"
---
.NET için dil bilgisi (LUIS) yazarlığını kullanın:

* Uygulama oluşturma
* Niyetler, varlıklar ve örnek söz ekleme
* İfade listesi gibi özellikler ekleme
* Eğitim ve yayın uygulaması

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Authoring) | [Yazma Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/) | [C# Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/LUIS/LUIS.cs)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama (LUIS) portal hesabı - [Ücretsiz bir tane oluşturun](https://www.luis.ai)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.


## <a name="setting-up"></a>Ayarlama

### <a name="get-your-language-understanding-luis-starter-key"></a>Dil Anlayışınızı (LUIS) başlangıç anahtarınızı alın

Bir LUIS yazma kaynağı oluşturarak [başlangıç anahtarınızı](../luis-how-to-azure-subscription.md#starter-key) alın. Anahtarınızı ve anahtarın bulunduğu bölgeyi bir sonraki adım için saklayın.

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

Anahtarınızı ve anahtar için bölgeyi kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:

* `COGNITIVESERVICE_AUTHORING_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `COGNITIVESERVICE_REGION`- Anahtarınızla ilişkili bölge. Örneğin, `westus`.

İşletim sisteminiziçin yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx COGNITIVESERVICE_AUTHORING_KEY <replace-with-your-authoring-key>
setx COGNITIVESERVICE_REGION <replace-with-your-authoring-region>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export COGNITIVESERVICE_AUTHORING_KEY=<replace-with-your-authoring-key>
export COGNITIVESERVICE_REGION=<replace-with-your-authoring-region>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export COGNITIVESERVICE_AUTHORING_KEY=<replace-with-your-authoring-key>
export COGNITIVESERVICE_REGION=<replace-with-your-authoring-region>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), adında `new` `language-understanding-quickstart`yeni bir konsol uygulaması oluşturmak için dotnet komutunu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir `Program.cs`"Hello World" C# projesi oluşturur: .

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin.

1. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

    ```dotnetcli
    dotnet build
    ```

    Yapı çıktısı hiçbir uyarı veya hata içermemelidir.

    ```console
    ...
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ...
    ```


### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizininde ,.NET için dil bilgisi (LUIS) yazan istemci kitaplığını aşağıdaki komutla yükleyin:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring --version 3.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.


## <a name="object-model"></a>Nesne modeli

Dil Anlama (LUIS) yazma istemcisi, Azure'a doğrulayan ve yazar anahtarınızı içeren bir [LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* Uygulamalar - [oluşturmak](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet), [silmek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet), [yayınlamak](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet)
* Örnek söyleyinçler - [toplu iş ekle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet), [ID'ye göre sil](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.deleteasync?view=azure-dotnet)
* Özellikler - [ifade listelerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.featuresextensions.addphraselistasync?view=azure-dotnet) yönetme
* Model - [niyetleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) ve varlıkları yönetme
* Desen - [desenleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.patternextensions?view=azure-dotnet) yönetme
* Tren - [eğitim durumu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet) için uygulamayı ve anketi [eğitin](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.trainversionasync?view=azure-dotnet)
* [sürümler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.versionsextensions?view=azure-dotnet) - klonla yönetin, dışa aktarma ve silme


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için istemci kitaplığı yazan Dil Anlayışı (LUIS) ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Uygulama oluşturma](#create-a-luis-app)
* [Varlık ekleme](#create-entities-for-the-app)
* [Hedef ekleme](#create-intent-for-the-app)
* [Örnek konuşmalar ekleme](#add-example-utterance-to-intent)
* [Uygulamayı eğitme](#train-the-app)
* [Uygulamayı yayımlama](#publish-a-language-understanding-app)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *Program.cs* dosyasını açın. Varolan `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

1. Yazarlık anahtarınızı yönetmek için bir değişken oluşturun. `COGNITIVESERVICES_AUTHORING_KEY` Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyicinin, IDE'nin veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacaktır.

1. Yazma bölgenizi ve bitiş noktanızı tutmak için değişkenler oluşturun. Yazar anahtarınızın bulunduğu bölge, nerede yazdığınıza bağlıdır. [Üç yazma bölgeleri](../luis-reference-regions.md) şunlardır:

    * Avustralya -`australiaeast`
    * Avrupa -`westeurope`
    * ABD ve diğer bölgeler `westus` - (Varsayılan)

    [!code-csharp[Authorization to resource key](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=Variables)]

1. Anahtarınızla bir [ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.apikeyserviceclientcredentials?view=azure-dotnet) oluşturun ve [bir LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

    [!code-csharp[Create LUIS client object](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. Amaçları, varlıkları ve örnek söyleyiyi tutan doğal dil işleme (NLP) modelini içeren bir LUIS uygulaması oluşturun.

1. [ApplicationCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.applicationcreateobject?view=azure-dotnet)oluşturun. Ad ve dil kültürü gerekli özellikleri vardır.

1. [Apps.AddAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet) yöntemini arayın. Yanıt uygulama kimliğidir.

    [!code-csharp[Create a LUIS app](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringCreateApplication)]

## <a name="create-intent-for-the-app"></a>Uygulama için niyet oluşturma
LUIS uygulamasının modelindeki birincil nesne amaçtır. Niyet kullanıcı söyleyiş _niyetleri_bir gruplama ile hizalar. Bir kullanıcı bir soru sorabilir veya bir bottan (veya başka bir istemci uygulamasından) belirli bir _yanıt_ arayan bir açıklama yapabilir. Bir uçuş rezervasyonu, varış şehrindeki hava durumu hakkında soru sormak ve müşteri hizmetleri için iletişim bilgilerini sormak gibi niyetörnekleri ne verilebilir:

Benzersiz amaç adına bir [ModelCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modelcreateobject?view=azure-dotnet) oluşturun ve ardından uygulama kimliğini, sürüm kimliğini ve ModelCreateObject'i [Model.AddIntentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addintentasync?view=azure-dotnet) yöntemine geçirin. Yanıt, niyet kimliğidir.

[!code-csharp[Create intent](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringAddIntents)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlıklar oluşturma

Varlıklar gerekli olmasa da, çoğu uygulamada bulunur. Varlık, kullanıcının niyetini tamolarak dosyalamak için gerekli olan kullanıcı sözcüklerinden bilgi ayıklar. Önceden [oluşturulmuş](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addprebuiltasync?view=azure-dotnet) ve özel varlıklar, her biri kendi veri dönüştürme nesnesi (DTO) modelleri ile çeşitli türleri vardır.  Uygulamanıza eklemek için ortak önceden oluşturulmuş varlıklar [numarası,](../luis-reference-prebuilt-number.md) [datetimeV2,](../luis-reference-prebuilt-datetimev2.md) [geographyV2](../luis-reference-prebuilt-geographyv2.md), [ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Bu **AddNtities** yöntemi `Location` iki rol, basit `Class` bir varlık, `Flight` bileşik varlık ve birkaç önceden oluşturulmuş varlıklar ekler basit bir varlık oluşturdu.

Varlıkların bir niyetle işaretlenmediğini bilmek önemlidir. Onlar ve genellikle birçok niyet için geçerli dir. Yalnızca örnek kullanıcı sözcükleri belirli, tek bir amaç için işaretlenir.

Varlıklar için oluşturma yöntemleri [Model](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) sınıfının bir parçasıdır. Her varlık türünün, genellikle `model` [Modeller](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models?view=azure-dotnet) ad alanında sözcüğü içeren kendi veri dönüştürme nesnesi (DTO) modeli vardır.

[!code-csharp[Create entities](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringAddEntities)]

## <a name="add-example-utterance-to-intent"></a>Niyete örnek söyleyiş ekleme

Bir söyleyiş niyetini belirlemek ve varlıkları ayıklamak için, uygulamanın söyleyiş örneklerine ihtiyacı vardır. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlemesi gerekmez.

[Örnek Etiket Nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-dotnet) nesnelerinin bir listesini oluşturarak örnek söyleyerek, her örnek için bir nesne ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğü ile tüm varlıkları işaretlemelidir. Varlık değeri tam olarak örnek söyleyiş metninde göründüğü gibi olmalıdır.

[Arama Examples.BatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ExamplesExtensions_BatchAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_IExamples_System_Guid_System_String_System_Collections_Generic_IList_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_Models_ExampleLabelObject__System_Threading_CancellationToken_) uygulama kimliği, sürüm kimliği ve örneklerin listesi ile. Arama, bir sonuç listesiyle yanıt verir. Modele başarıyla eklenmiştir emin olmak için her örneğin sonucunu denetlemeniz gerekir.

[!code-csharp[Add example utterances to a specific intent](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringBatchAddUtterancesForIntent)]

**CreateUtterance** ve **CreateLabel** yöntemleri, nesneler oluşturmanıza yardımcı olacak yardımcı yöntemlerdir.

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, LUIS uygulamasının modelin bu sürümü için eğitilmesi gerekir. Eğitimli bir model bir [kapta](../luis-container-howto.md)kullanılabilir veya evreleme veya ürün yuvalarında [yayınlanabilir.](../luis-how-to-publish-app.md)

[Train.TrainVersionAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions?view=azure-dotnet) yöntemi nin uygulama kimliği ve sürüm kimliği ne kadar önemli olduğu.

Çok küçük bir model, bu quickstart gösterir gibi, çok hızlı bir şekilde tren olacaktır. Üretim düzeyindeki uygulamalar için, uygulamanın eğitimi, eğitimin ne zaman veya ne zaman başarılı olduğunu belirlemek için [GetStatusAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_TrainExtensions_GetStatusAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ITrain_System_Guid_System_String_System_Threading_CancellationToken_) yöntemine bir yoklama çağrısı içermelidir. Yanıt, her nesne için ayrı bir durum olan [ModelTrainingInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-dotnet) nesnelerinin listesidir. Eğitimin tamamlanmış sayılması için tüm nesnelerin başarılı olması gerekir.

[!code-csharp[Train the app's version](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringTrainVersion)]

## <a name="publish-a-language-understanding-app"></a>Dil Anlama uygulaması yayınlama

[PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet) yöntemini kullanarak LUIS uygulamasını yayımlayın. Bu, geçerli eğitilmiş sürümü bitiş noktasında belirtilen yuvaya yayımlar. İstemci uygulamanız, amaç ve varlık çıkarma tahmini için kullanıcı yla ilgili söyleyişler göndermek için bu bitiş noktasını kullanır.

[!code-csharp[Create entities](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/LUIS/LUIS.cs?name=AuthoringPublishVersionAndSlot)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `dotnet run` komutu yla uygulamayı çalıştırın.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Temizlemek isterseniz, LUIS uygulamasını silebilirsiniz. Uygulamanın silinme [uygulaması Apps.DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet) yöntemi ile yapılır. Ayrıca luis [portalından](https://www.luis.ai)uygulamayı silebilirsiniz.