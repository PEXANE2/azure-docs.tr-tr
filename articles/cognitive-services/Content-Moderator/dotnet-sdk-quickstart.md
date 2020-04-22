---
title: 'Quickstart: .NET için İçerik Moderatör istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçla .NET için İçerik Moderatör istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: e93cfd6d44e6a59b4b3d94a49bef766fadf4381c
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770616"
---
# <a name="quickstart-content-moderator-client-library-for-net"></a>Quickstart: .NET için İçerik Moderatör istemci kitaplığı

.NET için İçerik Moderatör istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. İçerik Moderatör potansiyel saldırgan, riskli veya başka bir istenmeyen malzeme için metin, görüntü ve video içeriğini kontrol eden bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız daha sonra, yönetmeliklere uymak veya kullanıcılar için amaçlanan ortamı korumak için işaretlenmiş içeriği işleyebilir.

.NET için İçerik Moderatör istemci kitaplığını kullanın:

* [Orta metin](#moderate-text)
* [Orta görüntüler](#moderate-images)
* [İnceleme oluşturma](#create-a-review)

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ContentModerator) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) | [Örnekleri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/samples-dotnet)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-content-moderator-azure-resource"></a>İçerik ModeratörÜ Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak İçerik Moderatörü için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/) kaynağınızı görüntüleme

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `CONTENT_MODERATOR_ENDPOINT`anahtar `CONTENT_MODERATOR_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz metin düzenleyicisinde veya IDE'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `content-moderator-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*.

```console
dotnet new console -n content-moderator-quickstart
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

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *Program.cs* dosyasını açın. Aşağıdaki `using` deyimlerini ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_using)]

**Program** sınıfında, kaynağınızın bitiş noktası konumu için değişkenler ve ortam değişkenleri olarak anahtar oluşturun.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_creds)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenlerini oluşturduysanız, değişkenlere erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Content Moderator istemci kitaplığını aşağıdaki komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.ContentModerator --version 2.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar İçerik Moderatör .NET SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[İçerikModeratorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.contentmoderatorclient?view=azure-dotnet)|Bu sınıf tüm İçerik Moderatör işlevselliği için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[ImageModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation?view=azure-dotnet)|Bu sınıf, yetişkinlere uygun içerik, kişisel bilgiler veya insan yüzleri için görüntüleri çözümleme işlevini sağlar.|
|[TextModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.textmoderation?view=azure-dotnet)|Bu sınıf, dil, küfür, hatalar ve kişisel bilgiler için metni çözümleme işlevini sağlar.|
|[İncelemeler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.reviews?view=azure-dotnet)|Bu sınıf, iş oluşturma yöntemleri, özel iş akışları ve insan incelemeleri de dahil olmak üzere Gözden Geçirme API'lerinin işlevselliğini sağlar.|

## <a name="code-examples"></a>Kod örnekleri


Bu kod parçacıkları, .NET için İçerik Moderatörü istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Orta metin](#moderate-text)
* [Orta görüntüler](#moderate-images)
* [İnceleme oluşturma](#create-a-review)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Yeni bir yöntemde, istemci nesnelerini bitiş noktanız ve anahtarınızla anında anons edin. Her senaryo için farklı bir istemciye ihtiyacınız yoktur, ancak kodunuzu düzenli tutmaya yardımcı olabilir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_client)]

## <a name="moderate-text"></a>Orta metin

Aşağıdaki kod, bir metin gövdesini çözümlemek ve sonuçları konsola yazdırmak için İçerik Moderatöristemcisi kullanır. **Program** sınıfınızın kökünde, giriş ve çıktı dosyalarını tanımlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_text_vars)]

Daha sonra projenizin köküne bir *TextFile.txt* dosyası ekleyin. Bu dosyaya kendi metninizi ekleyin veya aşağıdaki örnek metni kullanın:

```
Is this a grabage email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.
Crap is the profanity here. Is this information PII? phone 4255550111
```

Yönteminize `Main` aşağıdaki yöntem çağrısını ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_textmod_call)]

Ardından, **Program** sınıfınızda bir yerde metin moderasyon yöntemini tanımlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_textmod)]

## <a name="moderate-images"></a>Orta görüntüler

Aşağıdaki kod, yetişkinlere uygun ve müstehcen içerik için uzak görüntüleri çözümlemek için bir [ImageModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation?view=azure-dotnet) nesnesi ile birlikte bir İçerik Moderatör istemcisi kullanır.

> [!NOTE]
> Yerel bir resmin içeriğini de analiz edebilirsiniz. Yerel görüntülerle çalışan yöntemler ve işlemler için [başvuru belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation.evaluatefileinputwithhttpmessagesasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_ContentModerator_ImageModeration_EvaluateFileInputWithHttpMessagesAsync_System_IO_Stream_System_Nullable_System_Boolean__System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) bakın.

### <a name="get-sample-images"></a>Örnek görüntüler alın

Giriş ve çıktı dosyalarınızı tanımlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_image_vars)]

Ardından projenizin kökünde *ImageFiles.txt*giriş dosyasını oluşturun. Bu dosyada, her satırda bir URL&mdash;çözümlemek için görüntülerin URL'lerini eklersiniz. Aşağıdaki örnek görüntüleri kullanabilirsiniz:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

Giriş ve çıktı dosyalarınızı yöntemde aşağıdaki `Main` yöntem çağrısına geçirin. Bu yöntemi daha sonraki bir adımda tanımlarsınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_textmod_call)]

### <a name="define-helper-class"></a>Yardımcı sınıfını tanıt

**Program** sınıfına aşağıdaki sınıf tanımını ekleyin. Bu iç sınıf görüntü ılımlılık sonuçlarını işleyecek.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_dataclass)]

### <a name="define-the-image-moderation-method"></a>Görüntü ılımlılık yöntemini tanımlama

Aşağıdaki yöntem, metin dosyasındaki görüntü URL'lerini yineler, bir **Değerlendirme Verileri** örneği oluşturur ve görüntüyü yetişkinlere uygun/müstehcen içerik, metin ve insan yüzleri için analiz eder. Daha sonra son **EvaluationData** örneğini bir listeye ekler ve döndürülen verilerin tam listesini konsola yazar.

#### <a name="iterate-through-image-urls"></a>Görüntü URL'leri aracılığıyla yineleyin

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_imagemod_iterate)]

#### <a name="analyze-content"></a>İçeriği analiz etme

İçerik Moderatör'ün ekrana verdiği görüntü öznitelikleri hakkında daha fazla bilgi için [Görüntü Denetleme Kavramları](./image-moderation-api.md) kılavuzuna bakın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_imagemod_analyze)]

#### <a name="write-moderation-results-to-file"></a>Moderasyon sonuçlarını dosyaya yazma

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_imagemod_save)]

## <a name="create-a-review"></a>İnceleme oluşturma

İçerik Moderatör .NET SDK'yı kullanarak içeriği [Gözden Geçirme aracına](https://contentmoderator.cognitive.microsoft.com) yedirebilir, böylece insan moderatörler içeriği inceleyebilir. İnceleme aracı hakkında daha fazla bilgi edinmek için [Gözden Geçirme aracı kavramsal kılavuzuna](./review-tool-user-guide/human-in-the-loop.md)bakın.

Bu bölümdeki yöntem, gözden geçirme oluşturmak, kimliğini almak ve Gözden Geçirme aracının web portalı üzerinden insan girdisi aldıktan sonra ayrıntılarını denetlemek için [İncelemeler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.reviews?view=azure-dotnet) sınıfını kullanır. Tüm bu bilgileri bir çıktı metin dosyasında kaydeder. Yönteminizi yönteminizden `Main` arayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_review_call)]

### <a name="get-sample-images"></a>Örnek görüntüler alın

**Program** sınıfının kökünde aşağıdaki diziyi bildirin. Bu değişken, incelemeyi oluşturmak için kullanılacak örnek bir görüntüye başvurur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_review_urls)]

### <a name="get-review-credentials"></a>İnceleme kimlik bilgilerini alın

Gözden Geçirme [aracında](https://contentmoderator.cognitive.microsoft.com) oturum açın ve takım adınızı alın. Ardından **program** sınıfındaki uygun değişkene atayın. İsteğe bağlı olarak, incelemenin etkinliğinde güncelleştirmeleri almak için bir geri arama bitiş noktası ayarlayabilirsiniz.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_review_vars)]

### <a name="define-helper-class"></a>Yardımcı sınıfını tanıt

**Program** sınıfınızda aşağıdaki sınıf tanımını ekleyin. Bu sınıf, Gözden Geçirme aracına gönderilen tek bir gözden geçirme örneğini temsil etmek için kullanılır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_review_item)]

### <a name="define-helper-method"></a>Yardımcı yöntemini tanımlama

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, gözden geçirme sorgularının sonuçlarını çıktı metin dosyasına yazar.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_writeline)]

### <a name="define-the-review-creation-method"></a>Gözden geçirme oluşturma yöntemini tanımlama

Artık gözden geçirme oluşturma ve sorgulama yı işleyecek yöntemi tanımlamaya hazırsınız. Yeni bir yöntem ekleyin, **CreateReviews**ve aşağıdaki yerel değişkenleri tanımlayın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_createreview_fields)]

#### <a name="post-reviews-to-the-review-tool"></a>İnceleme aracına yorum gönderme

Ardından, verilen örnek görüntüler aracılığıyla yinelemek için aşağıdaki kodu ekleyin, meta veriler ekleyin ve bunları tek bir toplu iş halinde Gözden Geçirme aracına gönderin. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_createreview_create)]

API çağrısından döndürülen nesne, yüklenen her resim için benzersiz kimlik değerleri içerir. Aşağıdaki kod bu teşekklü leri ayrışdırır ve daha sonra toplu işteki her görüntünün durumu için İçerik Moderatör'ü sorgulamak için bunları kullanır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_createreview_ids)]

### <a name="get-review-details"></a>İnceleme ayrıntılarını alın

Aşağıdaki kod, programın kullanıcı girişi için beklemesine neden olur. Çalışma zamanında bu adıma geldiğinizde, Gözden [Geçirme aracına](https://contentmoderator.cognitive.microsoft.com) kendiniz gidebilir, örnek resmin yüklendiğini doğrulayabilir ve onunla etkileşimkurabilirsiniz. Bir incelemeyle nasıl etkileşimde bulundurunuz hakkında bilgi [için, İncelemeler nasıl yapılacağını kılavuzuna](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/review-moderated-images)bakın. Bitirdikten sonra, programa devam etmek ve inceleme sürecinin sonuçlarını almak için herhangi bir tuşa basabilirsiniz.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ContentModerator/Program.cs?name=snippet_createreview_results)]

Bu senaryoda bir geri arama bitiş noktası kullandıysanız, bu biçimde bir olay almalısınız:

```console
{'callback_endpoint': 'https://requestb.in/qmsakwqm',
 'content': '',
 'content_id': '3ebe16cb-31ed-4292-8b71-1dfe9b0e821f',
 'created_by': 'cspythonsdk',
 'metadata': [{'key': 'sc', 'value': 'True'}],
 'review_id': '201901i14682e2afe624fee95ebb248643139e7',
 'reviewer_result_tags': [{'key': 'a', 'value': 'True'},
                          {'key': 'r', 'value': 'True'}],
 'status': 'Complete',
 'sub_team': 'public',
 'type': 'Image'}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki uygulamayı komutla `dotnet run` çalıştırın.

```dotnet
dotnet run 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Moderasyon görevleri yapmak için Content Moderator .NET kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kavramsal bir kılavuz okuyarak görüntülerin veya diğer medyanın moderasyonu hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Görüntü ılımlılığı kavramları](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Azure Content Moderator nedir?](./overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ContentModerator/Program.cs)bulunabilir.
