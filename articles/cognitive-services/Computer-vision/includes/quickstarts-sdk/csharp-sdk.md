---
title: 'Quickstart: .NET için Computer Vision istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için Computer Vision istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: ee4b6b4247ac1a5c988d8b455fd1d8a2bff889a9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80136003"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ComputerVision) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-computer-vision-azure-resource"></a>Bilgisayar Vizyonu Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Computer Vision için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `COMPUTER_VISION_ENDPOINT`anahtar `COMPUTER_VISION_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `computer-vision-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*.

```console
dotnet new console -n computer-vision-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

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

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *Program.cs* dosyasını açın. Aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_using)]

Uygulamanın **Program** sınıfında, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_vars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Computer Vision istemci kitaplığını aşağıdaki komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 5.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler Computer Vision .NET SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet) | Bu sınıf tüm Computer Vision işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve çoğu görüntü işlemlerini yapmak için kullanırsınız.|
|[ComputerVisionClientExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions?view=azure-dotnet)| Bu sınıf **ComputerVisionClient**için ek yöntemler içerir.|
|[GörselÖzellik Türleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)| Bu enum, standart bir Çözümleme işleminde yapılabilecek farklı görüntü çözümleme türlerini tanımlar. Gereksinimlerinize bağlı olarak visualfeaturetypes değerleri kümesi ni belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Computer Vision istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Basılı ve el yazısıyla yazılmış metni okuma](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Sırasıyla ve sırasıyla `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` Computer Vision anahtarı nız ve bitiş noktanız için ortam [değişkenleri oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Yeni bir yöntemde, son noktanız ve anahtarınızla istemciyi anında anons edin. Anahtarınızla bir **[ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.apikeyserviceclientcredentials?view=azure-dotnet)** oluşturun ve **[computerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet)** nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_auth)]

Bu yöntemi `Main` yöntemde aramak isteyebilirsiniz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Aşağıdaki kod, `AnalyzeImageUrl`uzak bir görüntüyü çözümlemek ve sonuçları yazdırmak için istemci nesnesini kullanan bir yöntem tanımlar. Yöntem, metin açıklaması, kategoriler, etiket listesi, algılanan yüzler, yetişkinlere uygun içerik bayrakları, ana renkler ve görüntü türünü döndürür.

Yönteme `Main` yöntem çağrısını ekleyin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_analyzeinmain)]

### <a name="set-up-test-image"></a>Test görüntüsünü ayarlama

**Program** sınıfınızda, çözümlemek istediğiniz resmin URL'sine bir başvuru kaydedin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_analyze_url)]

> [!NOTE]
> Yerel bir görüntüyü de analiz edebilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/ComputerVision/Program.cs) örnek kodu görün.

### <a name="specify-visual-features"></a>Görsel özellikleri belirtin

Görüntü analizi için yeni yönteminizi tanımlayın. Analizlerinizde ayıklamak istediğiniz görsel özellikleri belirten kodu aşağıya ekleyin. Tam bir liste için **[VisualFeatureTypes](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)** tamuna bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_visualfeatures)]

### <a name="analyze"></a>Çözümleme

**AnalyzeImageAsync** yöntemi, tüm çıkarılan bilgileri içeren bir **ImageAnalysis** nesnesini döndürür.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_analyze_call)]

Aşağıdaki bölümlerde bu bilgilerin nasıl ayrıştırılalı ştırış edilebildiğini göstereceğimiz.

### <a name="get-image-description"></a>Resim açıklamasını alın

Aşağıdaki kod, görüntü için oluşturulan altyazıların listesini alır. Daha fazla ayrıntı için [görüntüleri açıkla'ya](../../concept-describing-images.md) bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_describe)]

### <a name="get-image-category"></a>Resim kategorisi alın

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla ayrıntı için [görüntüleri kategorilere ayırın.](../../concept-categorizing-images.md)

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_categorize)]

### <a name="get-image-tags"></a>Resim etiketlerini alın

Aşağıdaki kod, görüntüde algılanan etiketler kümesini alır. Daha fazla ayrıntı için [İçerik etiketlerini](../../concept-tagging-images.md) görün.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_tags)]

### <a name="detect-objects"></a>Nesneleri algılama

Aşağıdaki kod görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla ayrıntı için [Nesne algılama](../../concept-object-detection.md) konusuna bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_objects)]

### <a name="detect-brands"></a>Markaları algılama

Aşağıdaki kod, görüntüdeki kurumsal markaları ve logoları algılar ve konsola yazdırır. Daha fazla ayrıntı için [Marka algılama](../../concept-brand-detection.md) ya da bkz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod, resimde algılanan yüzleri dikdörtgen koordinatlarıyla döndürür ve yüz özniteliklerini seçer. Daha fazla ayrıntı için [Yüz algılama](../../concept-detecting-faces.md) ya da yüz algılama bilgisine bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkinlere uygun, müstehcen veya kanlı içeriği algılama

Aşağıdaki kod, görüntüde algılanan yetişkinlere uygun içeriğin varlığını yazdırır. Daha fazla bilgi için [Yetişkin, müstehcen, kanlı içeriğe](../../concept-detecting-adult-content.md) bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzeni alın

Aşağıdaki kod, baskın renkler ve vurgu rengi gibi görüntüde algılanan renk özniteliklerini yazdırır. Daha fazla ayrıntı için [Renk şemaları'na](../../concept-detecting-color-schemes.md) bakın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Etki alanına özel içerik alın

Computer Vision, görüntüler üzerinde daha fazla analiz yapmak için özel modeller kullanabilir. Daha fazla ayrıntı için [Etki Alanına özgü içeriğe](../../concept-detecting-domain-content.md) bakın. 

Aşağıdaki kod, görüntüde algılanan ünlüler hakkındaki verileri ayrışdırır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_celebs)]

Aşağıdaki kod, görüntüde algılanan yer işaretleri yle ilgili verileri ayrışdırır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü alma

Aşağıdaki kod, küçük resim veya&mdash;çizgi çizimi olsun görüntünün türü hakkında bilgi yazdırır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Basılı ve el yazısıyla yazılmış metni okuma

Computer Vision görüntüdeki görünür metni okuyabilir ve karakter akışına dönüştürebilir. Bu bölümdeki kod, `ExtractTextUrl`görüntüdeki yazdırılan veya el yazısıyla yazılmış metni algılamak ve ayıklamak için istemci nesnesini kullanan bir yöntem tanımlar.

Yönteme `Main` yöntem çağrısını ekleyin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_extracttextinmain)]

### <a name="set-up-test-image"></a>Test görüntüsünü ayarlama

**Program** sınıfınızda, metin çıkarmak istediğiniz resmin URL'sine bir başvuru kaydedin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_extracttext_url)]

> [!NOTE]
> Yerel bir resimden metin ayıklayabilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/ComputerVision/Program.cs) örnek kodu görün.

### <a name="call-the-read-api"></a>Okuma API'sini arayın

Metni okumak için yeni yöntemi tanımlayın. Verilen görüntü için **BatchReadFileAsync** yöntemini çağıran aşağıdaki kodu ekleyin. Bu, bir işlem kimliği döndürür ve görüntünün içeriğini okumak için bir eşzamanlı işlem başlatır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_extract_call)]

### <a name="get-read-results"></a>Okuma sonuçları alın

Ardından, işlem kimliğini **BatchReadFileAsync** çağrısından döndürün ve hizmetin çalışma sonuçları için sorgulanması için kullanın. Aşağıdaki kod, sonuçlar döndürülene kadar işlemi bir saniye aralıklarla denetler. Daha sonra çıkarılan metin verilerini konsola yazdırır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_extract_response)]

### <a name="display-read-results"></a>Okuma sonuçlarını görüntüleme

Alınan metin verilerini ayrıştmak ve görüntülemek için aşağıdaki kodu ekleyin ve yöntem tanımını tamamlayın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ComputerVision/Program.cs?name=snippet_extract_display)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki uygulamayı komutla `dotnet run` çalıştırın.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Bilgisayarlı Vizyon API başvurusu (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/ComputerVision/Program.cs)bulunabilir.
