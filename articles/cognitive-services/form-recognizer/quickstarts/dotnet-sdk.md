---
title: 'Quickstart: .NET için Form Recognizer istemci kitaplığı'
description: Bu hızlı başlangıçla .NET'in eğitmek, ayıklamak, analiz etmek ve yapılandırılmış veri çıktısı almak için Form Recognizer istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: b9db9daf06b59e1a26a9b03a93aff63984841862
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77118354"
---
# <a name="quickstart-form-recognizer-client-library-for-net"></a>Quickstart: .NET için Form Recognizer istemci kitaplığı

.NET için Form Recognizer istemci kitaplığı ile başlayın. Form Recognizer, form belgelerinden anahtar/değer çiftleri ve tablo verilerini tanımlamak ve ayıklamak için makine öğrenimi teknolojisini kullanan bir Bilişsel Hizmettir. Ardından, asıl dosyadaki ilişkileri de içeren yapılandırılmış verinin çıktısını verir. SDK paketini yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

.NET için Form Recognizer istemci kitaplığını kullanın:

* [Özel bir Form Tanıyıcı modeli eğitin](#train-a-custom-model)
* [Çıkarılan anahtarların listesini alma](#get-a-list-of-extracted-keys)
* [Formları özel bir modelle analiz edin](#analyze-forms-with-a-custom-model)
* [Özel modellerin listesini alma](#get-a-list-of-custom-models)
* [Özel bir modeli silme](#delete-a-custom-model)

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/formrecognizer?view=azure-dotnet-preview) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/FormRecognizer) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.FormRecognizer/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/)
* Bir dizi eğitim verisi içeren bir Azure Depolama blob'u. Bkz. Eğitim verilerinizi bir araya getirmek için ipuçları ve seçenekler için [özel bir model için bir eğitim veri kümesi oluşturun.](../build-training-data-set.md) Bu hızlı başlangıç için, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **Tren** klasörünün altındaki dosyaları kullanabilirsiniz.
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-form-recognizer-azure-resource"></a>Form Tanıyıcı Azure kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve `FORM_RECOGNIZER_KEY` sırasıyla anahtar ve `FORM_RECOGNIZER_ENDPOINT`bitiş noktası için [ortam değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `formrecognizer-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: _Program.cs_. 

```console
dotnet new console -n formrecognizer-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Ardından uygulamayı aşağıdakilerle oluşturun:

```console
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

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki _Program.cs_ dosyasını açın. Aşağıdaki `using` deyimlerini ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_using)]

Ardından uygulamanın **Ana** yöntemine aşağıdaki kodu ekleyin. Bu eşzamanlı görevi daha sonra tanımlarsınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_main)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Form Recognizer istemci kitaplığını aşağıdaki komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.FormRecognizer --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar Form Tanıyır SDK'nın ana işlevini işler.

|Adı|Açıklama|
|---|---|
|[FormRecognizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.formrecognizerclient?view=azure-dotnet-preview)|Bu sınıf tüm Form Tanıyıcı işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[Trenİstek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainrequest?view=azure-dotnet-preview)| Kendi eğitim giriş verilerinizi kullanarak özel bir Form Recognizer modelini eğitmek için bu sınıfı kullanırsınız. |
|[Tren Sonucu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainresult?view=azure-dotnet-preview)| Bu sınıf, daha sonra formları çözümlemek için kullanabileceğiniz model kimliği de dahil olmak üzere özel bir model Tren işleminin sonuçlarını sunar. |
|[Analiz Sonucu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview)| Bu sınıf, özel bir model Çözümleme işleminin sonuçlarını sunar. **Ayıklanmış Sayfa** örneklerinin bir listesini içerir. |
|[ExtractedPage](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.extractedpage?view=azure-dotnet-preview)| Bu sınıf, tek bir form belgesinden çıkarılan tüm verileri temsil eder.|

## <a name="code-examples"></a>Kod örnekleri

<!--
    Include code snippets and short descriptions for each task you list in the the bulleted list. Briefly explain each operation, but include enough clarity to explain complex or otherwise tricky operations.

    Include links to the service's reference content when introducing a class for the first time
-->

Bu kod parçacıkları, .NET için Form Recognizer istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Özel bir Form Tanıyıcı modeli eğitin](#train-a-custom-model)
* [Çıkarılan anahtarların listesini alma](#get-a-list-of-extracted-keys)
* [Formları özel bir modelle analiz edin](#analyze-forms-with-a-custom-model)
* [Özel modellerin listesini alma](#get-a-list-of-custom-models)
* [Özel bir modeli silme](#delete-a-custom-model)

## <a name="define-variables"></a>Değişkenleri tanımlama

Herhangi bir yöntem tanımlamadan önce, **Program** sınıfınızın en üstüne aşağıdaki değişken tanımlarını ekleyin. Bazı değişkenleri kendiniz doldurmanız gerekir. 

* Eğitim verileriniz için SAS URL'sini almak için Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim imzasını al'ı**seçin. **Okuma** ve **Liste** izinlerinin denetlendiğinden emin olun ve **Oluştur'** u tıklatın. Ardından **URL** bölümündeki değeri kopyalayın. Bu formu olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
* Çözümlemek için örnek bir forma ihtiyacınız varsa, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **Test** klasörünün altındaki dosyalardan birini kullanabilirsiniz. Bu kılavuz da yalnızca PDF formlarını kullanır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_variables)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Yöntemin `Main` altında, 'de `Main`başvurulan görevi tanımlayın. Burada, yukarıda tanımladığınız abonelik değişkenlerini kullanarak istemci nesnesinin kimliğini doğrularsınız. Diğer yöntemleri daha sonra tanımlarsınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_maintask)]

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Aşağıdaki yöntem, Azure blob kapsayıcınızda depolanan belgelerde yeni bir tanıma modeli eğitmek için Form Recognizer istemci nesnenizi kullanır. Yeni eğitilen model [(ModelResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelresult?view=azure-dotnet-preview) nesnesi tarafından temsil edilen) hakkında bilgi görüntülemek için bir yardımcı yöntemi kullanır ve model kimliğini döndürür.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_train)]

Aşağıdaki yardımcı yöntem, Form Tanıyıcısı modeli yle ilgili bilgileri görüntüler.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displaymodel)]

## <a name="get-a-list-of-extracted-keys"></a>Çıkarılan anahtarların listesini alma

Eğitim tamamlandıktan sonra, özel model eğitim belgelerinden çıkardığı anahtarların listesini tutar. Gelecekteki form belgelerinin bu anahtarları içermesini bekler ve Çözümleme işleminde karşılık gelen değerlerini ayıklar. Çıkarılan anahtarların listesini almak ve konsola yazdırmak için aşağıdaki yöntemi kullanın. Bu, eğitim sürecinin etkili olduğunu doğrulamak için iyi bir yoldur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_getkeys)]

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir modelle analiz edin

Bu yöntem, PDF form belgesini çözümlemek ve anahtar/değer verilerini ayıklamak için Form Recognizer istemcisini ve model kimliğini kullanır. Sonuçları görüntülemek için bir yardımcı yöntemi kullanır [(AnalyzeResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview) nesnesi tarafından temsil edilir).

> [!NOTE]
> Aşağıdaki yöntem PDF formunu analiz eder. JPEG ve PNG formlarını analiz eden benzer yöntemler için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/FormRecognizer)tam örnek koduna bakın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_analyzepdf)]

Aşağıdaki yardımcı yöntem, bir Çözümle işlemi yle ilgili bilgileri görüntüler.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displayanalyze)]

## <a name="get-a-list-of-custom-models"></a>Özel modellerin listesini alma

Hesabınıza ait tüm eğitimli modellerin listesini döndürebilir ve ne zaman oluşturuldukları hakkında bilgi alabilirsiniz. Model listesi bir [ModelsResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelsresult?view=azure-dotnet-preview) nesnesi tarafından temsil edilir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_getmodellist)]

## <a name="delete-a-custom-model"></a>Özel bir modeli silme

Özel modeli hesabınızdan silmek istiyorsanız, aşağıdaki yöntemi kullanın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_deletemodel)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki `dotnet run` komutu arayarak uygulamayı çalıştırın.

```console
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Ayrıca, hesabınızdan silmek istediğiniz özel bir model eğittiyseniz, yöntemi [Özel bir modeli Sil'de](#delete-a-custom-model)çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, özel bir model eğitmek ve formları çözümlemek için Form Recognizer .NET istemci kitaplığını kullandınız. Ardından, daha iyi bir eğitim veri seti oluşturmak ve daha doğru modeller üretmek için ipuçları öğrenin.

> [!div class="nextstepaction"]
>[Eğitim verileri kümesi oluşturma](../build-training-data-set.md)

* [Form Tanıma nedir?](../overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/FormRecognizer)bulunabilir.
