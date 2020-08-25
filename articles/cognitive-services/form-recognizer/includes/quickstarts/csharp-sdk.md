---
title: 'Hızlı başlangıç: .NET için form tanıyıcı istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için form tanıyıcı istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 08/17/2020
ms.author: pafarley
ms.openlocfilehash: 520c42b4ff75328bad67e18a6723aafc60e6c19c
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752975"
---
[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/ai.formrecognizer-readme-pre)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/src)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.FormRecognizer)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/README.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services).
* Eğitim verileri kümesi içeren bir Azure Depolama Blobu. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../../build-training-data-set.md) . Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **eğitme** klasörü altındaki dosyaları kullanabilirsiniz.
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-form-recognizer-azure-resource"></a>Form tanıyıcı Azure kaynağı oluşturma

[!INCLUDE [create resource](../create-resource.md)]

### <a name="create-environment-variables"></a>Ortam değişkenlerini oluşturma

[!INCLUDE [environment-variables](../environment-variables.md)]

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `formrecognizer-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: _program.cs_. 

```console
dotnet new console -n formrecognizer-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Ardından uygulamayı ile derleyin:

```console
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

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki yönergeleri ekleyin `using` :

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_using)]

Sonra uygulamanın **Main** yöntemine aşağıdaki kodu ekleyin. Bu zaman uyumsuz görevi üzerinde daha sonra tanımlayacaksınız. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_main)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için form tanıyıcı istemci Kitaplığı ' nı yükleyeceksiniz:

```console
dotnet add package Azure.AI.FormRecognizer --version 3.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.


<!-- Objet model TBD -->



## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için form tanıyıcı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Yönteminin altında `Main` , ' de başvurulan görevi tanımlayın `Main` . Burada, yukarıda tanımladığınız abonelik değişkenlerini kullanarak iki istemci nesnesinin kimlik doğrulamasını yapacaksınız. Gerektiğinde, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilmeniz için bir **AzureKeyCredential** nesnesi kullanacaksınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_auth)]


### <a name="call-client-specific-methods"></a>İstemciye özgü yöntemleri çağırma

Sonraki kod bloğu, istemci nesnelerini form tanıyıcı SDK 'sindeki ana görevlerin her biri için metotları çağırmak üzere kullanır. Bu yöntemleri daha sonra daha sonra tanımlayacaksınız.

Ayrıca, eğitim ve test verileriniz için URL 'lere başvurular eklemeniz gerekecektir. 
* Özel model eğitim verilerinize yönelik SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .
* Sınanacak bir formun URL 'sini almak için yukarıdaki adımları kullanarak blob depolamada tek bir belgenin SAS URL 'sini alabilirsiniz. Ya da başka bir yerde bulunan bir belgenin URL 'sini alın.
* Bir makbuz görüntüsünün URL 'sini de almak için yukarıdaki yöntemi kullanın veya belirtilen örnek görüntü URL 'sini kullanın.

> [!NOTE]
> Bu kılavuzdaki kod parçacıkları, URL 'Ler tarafından erişilen uzak formları kullanır. Bunun yerine yerel form belgelerini işlemek istiyorsanız, [başvuru belgelerindeki](https://docs.microsoft.com/dotnet/api/overview/azure/ai.formrecognizer-readme-pre)ilgili yöntemlere bakın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_calls)]

## <a name="recognize-form-content"></a>Form içeriğini tanı

Bir modeli eğmenize gerek kalmadan belgeler içindeki tabloları, çizgileri ve sözcükleri tanımak için form tanıyıcıyı kullanabilirsiniz.

Belirli bir URI 'deki bir dosyanın içeriğini tanımak için **Startrecognizecontentfromuri** metodunu kullanın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_getcontent_call)]


Döndürülen değer bir **Formpage** nesneleri koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod bu nesneler boyunca yinelenir ve ayıklanan anahtar/değer çiftlerini ve tablo verilerini yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_getcontent_print)]


## <a name="recognize-receipts"></a>Alındıları tanı

Bu bölümde, önceden eğitilen bir makbuz modeli kullanılarak ABD makbuzlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URI 'den alındıları tanımak için **StartRecognizeReceiptsFromUri** yöntemini kullanın. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan bir **Recognizedmakbuz** nesneleri koleksiyonudur: Aşağıdaki kod, belirtilen URI 'ye bir alındısı işler ve ana alanları ve değerleri konsola yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_receipt_call)]


Sonraki kod bloğu, alış irsaliyesinde algılanan bireysel öğeler arasında yinelenir ve ayrıntılarını konsola yazdırır.
[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_receipt_item_print)]

Son olarak kod bloğu, Makbuzdaki toplam değeri yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_receipt_total_print)]

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Bu bölümde, bir modelin kendi verilerinize nasıl eğulacağı gösterilmektedir. Eğitilen bir model, özgün form belgesindeki anahtar/değer ilişkilerini içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

> [!NOTE]
> Modelleri, [form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md)gibi bir grafik kullanıcı arabirimi ile de eğitebilirsiniz.

### <a name="train-a-model-without-labels"></a>Etiketler olmadan bir modeli eğitme

Eğitim belgelerini el ile etiketlemeden özel modellerinizde bulunan tüm alanları ve değerleri tanımak için özel modeller eğitme.

Aşağıdaki yöntem, belirli bir belge kümesi üzerinde bir model TRAIN ve modelin durumunu konsola yazdırır. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_train)]

Döndürülen **Customformmodel** nesnesi, modelin tanıyabileceği form türleri ve her form türünden ayıklayabileceği alanlar hakkında bilgiler içerir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_train_response)]

Son olarak, bu yöntem modelin benzersiz KIMLIĞINI döndürür.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_train_return)]

### <a name="train-a-model-with-labels"></a>Etiketler içeren bir modeli eğitme

Ayrıca, eğitim belgelerini el ile etiketleyerek özel modeller de eğitebilirsiniz. Etiketlerle eğitim, bazı senaryolarda daha iyi performansa yol açar. Etiketlerle eğitebilmeniz için, eğitim belgelerinin yanı sıra BLOB depolama kapsayıcıda özel etiket bilgi dosyalarına (* \<filename\>.pdf.labels.jsaçık*) sahip olmanız gerekir. [Form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md) , bu etiket dosyalarını oluşturmanıza yardımcı olmak için bir kullanıcı arabirimi sağlar. Bunları aldıktan sonra, *uselabels* parametresi olarak ayarlanan **Starttrainingasync** yöntemini çağırabilirsiniz `true` .

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_trainlabels)]

Döndürülen **Customformmodel** modeli, modelin ayıklayabileceğiniz alanları, her alandaki tahmini doğruluğunu gösterir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_trainlabels_response)]

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir model ile analiz etme

Bu bölümde, kendi formlarınız ile eğitilen modeller kullanılarak özel form türlerinizde anahtar/değer bilgilerinin ve diğer içeriklerin nasıl ayıklanacağı gösterilmektedir.

> [!IMPORTANT]
> Bu senaryoyu uygulamak için, KIMLIĞINI aşağıdaki yönteme geçirebilmeniz için zaten bir model eğitilmeniz gerekir. [Modeli eğitme](#train-a-model-without-labels) bölümüne bakın.

**Startrecognizecustomformsfromuri** metodunu kullanacaksınız. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan **Recognizedform** nesnelerinin bir koleksiyonudur:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_analyze)]

Aşağıdaki kod, analiz sonuçlarını konsola yazdırır. Her tanınan alanı ve karşılık gelen değeri, Güvenirlik puanı ile birlikte yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_analyze_response)]

## <a name="manage-your-custom-models"></a>Özel modellerinizi yönetin

Bu bölümde, hesabınızda depolanan özel modellerin nasıl yönetileceği gösterilmektedir. Aşağıdaki kod, örnek olarak tek bir yöntemde tüm model yönetimi görevlerini yapar. Aşağıdaki yöntem imzasını kopyalayarak başlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_manage)]

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Formtanıyıcı kaynak hesabındaki modellerin sayısını denetleyin

Aşağıdaki kod bloğu, form tanıyıcı hesabınıza kaç modelin kaydedildiğini denetler ve hesap limitine göre karşılaştırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_manage_model_count)]

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Şu anda kaynak hesapta depolanan modelleri listeleyin

Aşağıdaki kod bloğu, hesabınızdaki geçerli modelleri listeler ve ayrıntılarını konsola yazdırır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_manage_model_list)]

### <a name="get-a-specific-model-using-the-models-id"></a>Modelin KIMLIĞINI kullanarak belirli bir modeli al

Aşağıdaki kod bloğu yeni bir model ( [model eğitme](#train-a-model-without-labels) bölümünde olduğu gibi) ve kimliğini kullanarak ikinci bir başvuru alır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_manage_model_get)]

### <a name="delete-a-model-from-the-resource-account"></a>Kaynak hesabındaki bir modeli silme

Ayrıca, KIMLIĞINE başvurarak hesabınızdan bir modeli silebilirsiniz.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/FormRecognizerQuickstart.cs?name=snippet_manage_model_delete)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `dotnet run` .

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Sorun giderme

.NET SDK kullanarak bilişsel hizmetler form tanıyıcı istemci kitaplığıyla etkileşim kurarken, hizmet tarafından döndürülen hatalar bir ile sonuçlanır `RequestFailedException` . Bunlar, bir REST API isteği tarafından döndürülen HTTP durum kodunu içerir.

Örneğin, geçersiz bir URI ile bir makbuz görüntüsü gönderirseniz, `400` "bozuk istek" belirten bir hata döndürülür.

```csharp
try
{
    RecognizedReceiptCollection receipts = await client.StartRecognizeReceiptsFromUri(new Uri(receiptUri)).WaitForCompletionAsync();
}
catch (RequestFailedException e)
{
    Console.WriteLine(e.ToString());
}
```

İşlemin istemci istek KIMLIĞI gibi ek bilgilerin günlüğe kaydedileceğini görürsünüz.

```
Message:
    Azure.RequestFailedException: Service request failed.
    Status: 400 (Bad Request)

Content:
    {"error":{"code":"FailedToDownloadImage","innerError":
    {"requestId":"8ca04feb-86db-4552-857c-fde903251518"},
    "message":"Failed to download image from input URL."}}

Headers:
    Transfer-Encoding: chunked
    x-envoy-upstream-service-time: REDACTED
    apim-request-id: REDACTED
    Strict-Transport-Security: REDACTED
    X-Content-Type-Options: REDACTED
    Date: Mon, 20 Apr 2020 22:48:35 GMT
    Content-Type: application/json; charset=utf-8
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, modelleri eğitmek ve formları farklı yollarla analiz etmek için tanıyıcı .NET istemci kitaplığı formunu kullandınız. Daha sonra, daha iyi eğitim veri kümesi oluşturma ve daha doğru modeller üretme hakkında ipuçları edinin.

> [!div class="nextstepaction"]
> [Eğitim verileri kümesi oluşturma](../../build-training-data-set.md)

* [Form Tanıma nedir?](../../overview.md)
* Bu kılavuzun örnek kodu (ve daha fazla) [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/README.md)'da bulabilirsiniz.
