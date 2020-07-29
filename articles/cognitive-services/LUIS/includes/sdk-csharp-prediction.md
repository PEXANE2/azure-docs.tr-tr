---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 4bf86c616420bb049e1d7a82ad0e942e6eb7b36f
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369293"
---
.NET için Language Understanding (LUSıS) tahmin istemci kitaplığını kullanın:

* Yuvaya göre tahmin al
* Sürüme göre tahmin

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Runtime)  |  [Tahmin çalışma zamanı paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/)  |  [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/LanguageUnderstanding/predict-with-sdk-3x)

## <a name="prerequisites"></a>Önkoşullar

* Language Understanding (LUSıS) portal hesabı- [ücretsiz olarak bir tane oluşturun](https://www.luis.ai)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Bir LUSıS uygulama KIMLIĞI-genel IoT uygulama KIMLIĞI ' ni kullanın `df67dcdb-c37d-46af-88e1-8b97951ca1c2` . Hızlı başlangıç kodunda kullanılan Kullanıcı sorgusu bu uygulamaya özeldir.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `new` ad ile yeni bir konsol uygulaması oluşturmak için DotNet komutunu kullanın `language-understanding-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: `Program.cs` .

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin.

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

### <a name="install-the-sdk"></a>SDK Yükleme

Uygulama dizini içinde, aşağıdaki komutla .NET için Language Understanding (LUSıS) tahmin çalışma zamanı istemci Kitaplığı ' nı yüklemelisiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime --version 3.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Language Understanding (LUSıS) tahmini çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Hazırlama veya ürün yuvasına](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getslotpredictionasync?view=azure-dotnet) göre tahmin
* [Sürüme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getversionpredictionasync?view=azure-dotnet) göre tahmin


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Language Understanding (LUSıS) tahmin çalışma zamanı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Mevcut `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

1. Anahtar, kaynak adı, uygulama KIMLIĞI ve yayımlama yuvası için değişkenler oluşturun. Uygulama KIMLIĞINI genel IoT uygulamasına ayarlayın:

    **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`**

    [!code-csharp[Create variables](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_variables)]

1. Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi oluşturun ve bir [Luisruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_create_client)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söylenişi, [PredictionRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-dotnet) nesnesinin bir parçasıdır.

**GetSlotPredictionAsync** yöntemi, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_maintask)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin sağlamak için değişkenleri ve yöntemleri birbirine bağlamak üzere aşağıdaki ana yöntemi kullanın.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_main)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` uygulama dizininizdeki komutla çalıştırın.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tahminlerinizi tamamladığınızda, program.cs dosyasını ve alt dizinlerini silerek bu hızlı başlangıçta işi temizleyin.
