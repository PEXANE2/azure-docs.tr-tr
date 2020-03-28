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
ms.openlocfilehash: 3bb0b9fb8b268c3e036ffb365eb8d5b80a241269
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77372138"
---
.NET için Dil Anlayışı (LUIS) tahmin istemci kitaplığını kullanın:

* Yuvaya göre tahmin alma

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Runtime) | [Tahmin çalışma zamanı Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) | [C# Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/LanguageUnderstanding/predict-with-sdk-3x)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama (LUIS) portal hesabı - [Ücretsiz bir tane oluşturun](https://www.luis.ai)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

Daha fazla belge mi arıyorsunuz?

 * [SDK referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

Anahtarınızı ve kaynağınızın adını kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:

* `LUIS_PREDICTION_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `LUIS_ENDPOINT_NAME`- Anahtarınızla ilişkili kaynak adı.

İşletim sisteminiziçin yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_PREDICTION_KEY <replace-with-your-resource-key>
setx LUIS_ENDPOINT_NAME <replace-with-your-resource-name>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_PREDICTION_KEY=<replace-with-your-resource-key>
export LUIS_ENDPOINT_NAME=<replace-with-your-resource-name>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export LUIS_PREDICTION_KEY=<replace-with-your-resource-key>
export LUIS_ENDPOINT_NAME=<replace-with-your-resource-name>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

---

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

Uygulama dizininde, .NET için Dil Anlayışı (LUIS) tahmin çalışma zamanı istemci kitaplığını aşağıdaki komutla yükleyin:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime --version 3.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Dil Bilgisi (LUIS) tahmin çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure'a kimlik doğrulayan bir [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* [Evreleme veya ürün yuvası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getslotpredictionasync?view=azure-dotnet) na göre tahmin
* [Sürüme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getversionpredictionasync?view=azure-dotnet) göre tahmin


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Dil Anlama (LUIS) tahmin çalışma zamanı istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *Program.cs* dosyasını açın. Varolan `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

1. Anahtar, ad ve uygulama kimliği için değişkenler oluşturun:

    Tahmin anahtarınızı yönetmek için bir değişken adlı `LUIS_PREDICTION_KEY`bir ortam değişkeninden çekilir. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyicinin, IDE'nin veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacaktır.

    Kaynak adınızı `LUIS_ENDPOINT_NAME`tutmak için bir değişken oluşturun.

    Uygulama kimliği için bir değişken oluşturun. `LUIS_APP_ID` Ortam değişkenini genel IoT uygulamasına ayarlayın:

    **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`**

    [!code-csharp[Create variables](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_variables)]

1. Anahtarınızla bir [ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.apikeyserviceclientcredentials?view=azure-dotnet) oluşturun ve bir [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_create_client)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin alma

İstek tahmin çalışma süresi oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söyleyiş, [PredictionRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-dotnet) nesnesinin bir parçasıdır.

**GetSlotPredictionAsync** yöntemi, isteği yerine getirmek için uygulama kimliği, yuva adı, tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyar. Ayrıntılı, tüm amaçları gösterir ve günlük isteğe bağlıdır.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_maintask)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin almak için değişkenleri ve yöntemleri birbirine bağlamak için aşağıdaki ana yöntemi kullanın.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_main)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `dotnet run` komutu yla uygulamayı çalıştırın.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öngörüleriniz bittiğinde, program.cs dosyasını ve alt dizinlerini silerek bu hızlı başlangıçtan çalışmayı temizleyin.
