---
title: "Örnek: ' de bir görüntü işleme uygulamasını keşfetC#"
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'de Görüntü İşleme API'si kullanan temel bir Windows uygulamasını keşfedebilir. OCR gerçekleştirin, küçük resimler oluşturun ve bir görüntüdeki görsel özelliklerle çalışın.
services: cognitive-services
author: PatrickFarley
manager: nolachar
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: b492d8e3bdcf6d9a41df3eb79ef159985cc715cf
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76168873"
---
# <a name="sample-explore-an-image-processing-app-with-c"></a>Örnek: ile bir görüntü işleme uygulamasını araştırmaC#

Optik karakter tanıma (OCR) gerçekleştirmek, akıllı kırpılan küçük resimler oluşturmak, ayrıca bir görüntüdeki yüzler dahil olmak üzere görsel özellikleri algılamak, sınıflandırmak, etiketlemek ve açıklama eklemek için Görüntü İşleme kullanan temel bir Windows uygulamasını keşfedebilir. Aşağıdaki örnek bir görüntü URL'si veya yerel ortamda depolanan dosya göndermenizi sağlar. Bu açık kaynak örneğini, .NET Framework bir parçası olan Görüntü İşleme API'si ve Windows Presentation Foundation (WPF) kullanarak Windows için kendi uygulamanızı oluşturmak üzere bir şablon olarak kullanabilirsiniz.

> [!div class="checklist"]
> * GitHub 'dan örnek uygulamayı edinme
> * Visual Studio 'da örnek uygulamayı açın ve oluşturun
> * Örnek uygulamayı çalıştırın ve çeşitli senaryolar gerçekleştirmek için onunla etkileşime geçin
> * Örnek uygulamayla birlikte sunulan çeşitli senaryoları inceleyin

## <a name="prerequisites"></a>Ön koşullar

Örnek uygulamayı araştırmadan önce, aşağıdaki önkoşulları karşılatığınızdan emin olun:

* [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) veya üzerine sahip olmanız gerekir.
* Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)'den ücretsiz bir deneme anahtarı edinebilirsiniz. Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Hizmet uç noktası URL 'sini de göz atın.

## <a name="get-the-sample-app"></a>Örnek uygulamayı alma

Görüntü İşleme örnek uygulama, `Microsoft/Cognitive-Vision-Windows` deposundan GitHub 'da kullanılabilir. Bu depo Ayrıca, git alt modülü olarak `Microsoft/Cognitive-Common-Windows` deposunu içerir. Bu depoyu, alt modül dahil, komut satırından `git clone --recurse-submodules` komutunu kullanarak veya GitHub Desktop kullanarak yinelemeli olarak kopyalayabilirsiniz.

Örneğin, Görüntü İşleme örnek uygulaması için depoyu bir komut isteminden yinelemeli olarak kopyalamak için aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Vision-Windows.git
```

> [!IMPORTANT]
> Bu depoyu bir ZIP olarak indirmeyin. Git, ZIP olarak bir depoyu indirirken alt modüller içermez.

### <a name="get-optional-sample-images"></a>İsteğe bağlı örnek görüntüleri al

İsteğe bağlı olarak, `Microsoft/Cognitive-Face-Windows` deposundan GitHub 'da bulunan [yüz](../../Face/Overview.md) örnek uygulamasıyla birlikte bulunan örnek görüntüleri kullanabilirsiniz. Bu örnek uygulama, birden çok kişinin görüntüsünü içeren `/Data`bir klasör içerir. Bu depoyu Ayrıca, Görüntü İşleme örnek uygulama için açıklanan yöntemlerle yinelemeli olarak kopyalayabilirsiniz.

Örneğin, yüz örnek uygulaması için depoyu bir komut isteminden yinelemeli olarak kopyalamak için aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Face-Windows.git
```

## <a name="open-and-build-the-sample-app-in-visual-studio"></a>Visual Studio 'da örnek uygulamayı açın ve oluşturun

Örnek uygulamayı çalıştırmadan veya araştırmadan önce, Visual Studio 'Nun bağımlılıkları çözebilmesi için öncelikle örnek uygulamayı oluşturmanız gerekir. Örnek uygulamayı açmak ve derlemek için aşağıdaki adımları uygulayın:

1. Visual Studio 'da `/Sample-WPF/VisionAPI-WPF-Samples.sln`Visual Studio çözüm dosyasını açın.
1. Visual Studio çözümünün iki proje içerdiğinden emin olun:  

   * SampleUserControlLibrary
   * VisionAPI-WPF-örnekler  

   SampleUserControlLibrary projesi kullanılamıyorsa, `Microsoft/Cognitive-Vision-Windows` depoyu özyinelemeli olarak Klonladığınız onaylayın.
1. Visual Studio 'da, CTRL + SHIFT + B tuşlarına basın ya da şerit menüsünden **Oluştur** ' u seçin ve çözümü derlemek Için **çözüm oluştur** ' u seçin.

## <a name="run-and-interact-with-the-sample-app"></a>Çalıştırma ve örnek uygulamayla etkileşim kurma

Örnek uygulamayı, küçük resim oluşturma veya görüntü etiketleme gibi çeşitli görevleri gerçekleştirirken Görüntü İşleme istemci kitaplığıyla nasıl etkileşime gireceğini görmek için çalıştırabilirsiniz. Çalıştırmak ve örnek uygulamayla etkileşim kurmak için aşağıdaki adımları uygulayın:

1. Derleme tamamlandıktan sonra, **F5** tuşuna basın veya şerit menüsünde **Hata Ayıkla** ' yı seçin ve ardından örnek uygulamayı çalıştırmak için **hata ayıklamayı Başlat** ' ı seçin.
1. Örnek uygulama görüntülenirken, abonelik anahtar yönetimi sayfasını göstermek için gezinti bölmesinden **abonelik anahtar yönetimi** ' ni seçin.
   ![abonelik anahtarı yönetim sayfası](../Images/Vision_UI_Subscription.PNG)  
1. Abonelik anahtarınızı **abonelik anahtarı**' na girin.
1. Uç noktada Endpoint URL 'sini **girin.**  
   Örneğin, Görüntü İşleme ücretsiz deneme sürümünden abonelik anahtarını kullanıyorsanız, şu uç nokta URL 'sini girin: `https://westcentralus.api.cognitive.microsoft.com`  
   [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
1. Örnek uygulamayı bir sonraki çalıştırışınızda abonelik anahtarınızı ve uç nokta URL 'sini girmek istemiyorsanız, abonelik anahtarını ve uç nokta URL 'sini bilgisayarınıza kaydetmek için **ayarı kaydet** ' i seçin. Önceden kaydedilmiş abonelik anahtarınızı ve uç nokta URL 'sini silmek istiyorsanız, **ayarı Sil**' i seçin.

   > [!NOTE]
   > Örnek uygulama, abonelik anahtarınızı ve uç nokta URL 'nizi depolamak için yalıtılmış depolama ve `System.IO.IsolatedStorage`kullanır.

1. Gezinti bölmesinde **bir senaryo seçin** ' in altında şu anda örnek uygulamaya dahil olan senaryolardan birini seçin:  

   | Senaryo | Açıklama |
   |----------|-------------|
   |Görüntüyü çözümle | Yerel veya uzak bir görüntüyü çözümlemek için [görüntüyü çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) işlemini kullanır. Analiz için görsel özellikleri ve dili seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz.  |
   |Görüntüyü etki alanı modeliyle çözümle | , Seçilen etki alanı modelini kullanarak yerel veya uzak bir görüntüyü çözümlemek için, seçebileceğiniz etki alanı modellerini listelemek için etki alanına özgü [modelleri](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) listeleme Işlemini ve [etki alanına özgü içeriği tanı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) işlemini kullanır. Analizin dilini de seçebilirsiniz. |
   |Görüntüyü açıkla | Yerel veya uzak görüntünün okunabilir bir açıklamasını oluşturmak için [görüntüyü açıkla](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fe) işlemini kullanır. Ayrıca, açıklama dilini de seçebilirsiniz. |
   |Etiketler oluştur | , Yerel veya uzak bir görüntünün görsel özelliklerini etiketlemek için [resim etiketi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1ff) işlemini kullanır. Etiketler için kullanılan dili de seçebilirsiniz. |
   |Metin Tanıma (OCR) | Bir görüntüden yazdırılmış metni tanımak ve ayıklamak için [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) işlemini kullanır. Kullanılacak dili seçebilir ya da dili otomatik olarak algılamaya Görüntü İşleme sağlayabilirsiniz. |
   |Metin Tanıma v2 (Ingilizce) | [Metin tanıma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) kullanır ve bir görüntüden yazdırılmış veya el ile yazılmış metinleri zaman uyumsuz olarak tanımak ve ayıklamak Için [metin tanıma Işlem sonucu işlemlerini alır](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) . |
   |Küçük resim al | Yerel veya uzak görüntü için küçük resim oluşturmak üzere [küçük resim al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) işlemini kullanır. |

   Aşağıdaki ekran görüntüsünde, örnek bir görüntüyü analiz ettikten sonra görüntüyü çözümle senaryosu için sunulan sayfa gösterilmektedir.
   görüntüyü çözümle sayfasının ekran görüntüsünü ![](../Images/Analyze_Image_Example.PNG)

## <a name="explore-the-sample-app"></a>Örnek uygulamayı keşfet

Görüntü İşleme örnek uygulaması için Visual Studio çözümü iki proje içerir:

* SampleUserControlLibrary  
  SampleUserControlLibrary projesi, birden çok bilişsel hizmet örneği tarafından paylaşılan işlevler sağlar. Proje şunları içerir:
  * Samplesenaryolar  
    Örnekler için başlık çubuğu, gezinti bölmesi ve İçerik bölmesi gibi standartlaştırılmış bir sunum sağlayan UserControl. Görüntü İşleme örnek uygulaması, senaryo sayfalarını göstermek ve abonelik anahtarı ve uç nokta URL 'SI gibi senaryolar genelinde paylaşılan bilgilere erişmek için MainWindow. xaml penceresinde bu denetimi kullanır.
  * SubscriptionKeyPage  
    Örnek uygulama için bir abonelik anahtarı ve uç nokta URL 'SI girmek üzere standartlaştırılmış bir düzen sağlayan bir sayfa. Görüntü İşleme örnek uygulama, senaryo sayfaları tarafından kullanılan abonelik anahtarını ve uç nokta URL 'sini yönetmek için bu sayfayı kullanır.
  * VideoResultControl  
    Video bilgileri için standartlaştırılmış bir sunum sağlayan bir UserControl. Görüntü İşleme örnek uygulama bu denetimi kullanmaz.
* VisionAPI-WPF-örnekler  
  Görüntü İşleme örnek uygulaması için ana proje, bu proje Görüntü İşleme yönelik tüm ilginç işlevleri içerir. Proje şunları içerir:
  * Analiz Zeindomainpage. xaml  
    Görüntüyü etki alanı modeliyle çözümle senaryosu için senaryo sayfası.
  * Analiz Zeımage. xaml  
    Görüntüyü çözümle senaryosunun senaryo sayfası.
  * DescribePage. xaml  
    Görüntüyü açıkla senaryosunun senaryo sayfası.
  * ImageScenarioPage.cs  
    Örnek uygulamadaki tüm senaryo sayfalarından türetilen ımasanariopage sınıfı. Bu sınıf, tüm senaryo sayfaları tarafından paylaşılan kimlik bilgileri ve biçimlendirme çıktısı sağlama gibi işlevleri yönetir.
  * MainWindow.xaml  
    Örnek uygulamanın ana penceresi, SubscriptionKeyPage ve senaryo sayfalarını sunmak için Samplesenaryolar denetimini kullanır.
  * OCRPage. xaml  
    Metin Tanıma (OCR) senaryosuna yönelik senaryo sayfası.
  * RecognizeLanguage.cs  
    Örnek uygulamadaki çeşitli yöntemler tarafından desteklenen diller hakkında bilgi sağlayan RecognizeLanguage sınıfı.
  * TagsPage. xaml  
    Etiketler oluştur senaryosuna yönelik senaryo sayfası.
  * TextRecognitionPage. xaml  
    Metin Tanıma v2 (Ingilizce) senaryosuna yönelik senaryo sayfası.
  * ThumbnailPage. xaml  
    Küçük resim al senaryosunun senaryo sayfası.

### <a name="explore-the-sample-code"></a>Örnek kodu keşfet

Örnek kodun önemli kısımları, örnek uygulamayı keşfetmenizi kolaylaştırmak için `KEY SAMPLE CODE STARTS HERE` ile başlayan ve `KEY SAMPLE CODE ENDS HERE`biten açıklama bloklarıyla çerçevelenmiş. Örnek kodun bu temel bölümleri, çeşitli görevleri yapmak için Görüntü İşleme API'si istemci kitaplığını kullanmayı öğrenmeyle ilgili en önemli kodu içerir. Görüntü İşleme örnek uygulamasındaki kodun en ilgili bölümleri arasında gezinmek için Visual Studio 'da `KEY SAMPLE CODE STARTS HERE` araması yapabilirsiniz. 

Örneğin, aşağıdaki ve analiz Zepage. xaml ' de yer alan `UploadAndAnalyzeImageAsync` yöntemi, `ComputerVisionClient.AnalyzeImageInStreamAsync` metodunu çağırarak yerel bir görüntüyü çözümlemek için istemci kitaplığının nasıl kullanılacağını gösterir.

```csharp
private async Task<ImageAnalysis> UploadAndAnalyzeImageAsync(string imageFilePath)
{
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE STARTS HERE
    // -----------------------------------------------------------------------

    //
    // Create Cognitive Services Vision API Service client.
    //
    using (var client = new ComputerVisionClient(Credentials) { Endpoint = Endpoint })
    {
        Log("ComputerVisionClient is created");

        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features.
            //
            Log("Calling ComputerVisionClient.AnalyzeImageInStreamAsync()...");
            VisualFeatureTypes[] visualFeatures = GetSelectedVisualFeatures();
            string language = (_language.SelectedItem as RecognizeLanguage).ShortCode;
            ImageAnalysis analysisResult = await client.AnalyzeImageInStreamAsync(imageFileStream, visualFeatures, null, language);
            return analysisResult;
        }
    }

    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE ENDS HERE
    // -----------------------------------------------------------------------
}
```

### <a name="explore-the-client-library"></a>İstemci kitaplığını keşfet

Bu örnek uygulama, Azure bilişsel hizmetler 'de Görüntü İşleme API'si C# için bir ince istemci sarmalayıcısı olan görüntü işleme API'si istemci kitaplığını kullanır. İstemci kitaplığı, [Microsoft. Azure. Biliveservices. Vision. ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) paketindeki NuGet 'ten kullanılabilir. Visual Studio uygulamasını yapılandırdığınızda, istemci kitaplığını ilgili NuGet paketinden alırsınız. Ayrıca, istemci kitaplığının kaynak kodunu `Microsoft/Cognitive-Vision-Windows` deposunun `/ClientLibrary` klasöründe görüntüleyebilirsiniz.

İstemci kitaplığının işlevselliği, `Microsoft.Azure.CognitiveServices.Vision.ComputerVision` ad alanındaki `ComputerVisionClient` sınıfını etrafında ortalar, ancak Görüntü İşleme ile etkileşim kurarken `ComputerVisionClient` sınıfı tarafından kullanılan modeller `Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models` ad alanında bulunur. Örnek uygulamayla birlikte bulunan çeşitli XAML senaryo sayfalarında, bu ad alanları için aşağıdaki `using` yönergeleri bulacaksınız:

```csharp
// -----------------------------------------------------------------------
// KEY SAMPLE CODE STARTS HERE
// Use the following namespace for ComputerVisionClient.
// -----------------------------------------------------------------------
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
// -----------------------------------------------------------------------
// KEY SAMPLE CODE ENDS HERE
// -----------------------------------------------------------------------
```

Görüntü İşleme örnek uygulamasına dahil olan senaryoları keşfederken `ComputerVisionClient` sınıfıyla birlikte sunulan çeşitli yöntemler hakkında daha fazla bilgi edineceksiniz.

## <a name="explore-the-analyze-image-scenario"></a>Görüntüyü çözümle senaryosunu keşfet

Bu senaryo, analiz Zepage. xaml sayfası tarafından yönetilir. Analiz için görsel özellikleri ve dili seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemlerden birini kullanarak bunu yapar:

* Uploadandanaliz Zeımageasync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* AnalyzeUrlAsync  
  Bu yöntem, `ComputerVisionClient.AnalyzeImageAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndAnalyzeImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra senaryo sayfasında seçilen görsel özellikleri ve dili alır. `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemini çağırarak dosya, görsel özellikler ve dil için `Stream` geçirerek sonucu bir `ImageAnalysis` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`AnalyzeUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Senaryo sayfasında seçilen görsel özellikleri ve dili alır. `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemini çağırır, görüntü URL 'sini, görsel özellikleri ve dili geçirerek bir `ImageAnalysis` örneği olarak sonucunu döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-analyze-image-with-domain-model-scenario"></a>Görüntüyü etki alanı modeliyle çözümle senaryosunu keşfet

Bu senaryo, analiz Zeindomainpage. xaml sayfası tarafından yönetilir. Görüntünün etki alanına özgü analizini gerçekleştirmek için `celebrities` veya `landmarks`ve dil gibi bir etki alanı modeli seçebilirsiniz ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* GetAvailableDomainModelsAsync  
  Bu yöntem, Görüntü İşleme ' dan kullanılabilir etki alanı modellerinin listesini alır ve `ComputerVisionClient.ListModelsAsync` yöntemini kullanarak sayfadaki `_domainModelComboBox` ComboBox denetimini doldurur.
* Uploadandanalzeındomainımageasync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* AnalyzeInDomainUrlAsync  
  Bu yöntem, `ComputerVisionClient.AnalyzeImageByDomainAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndAnalyzeInDomainImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra senaryo sayfasında seçilen dili alır. `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` yöntemini çağırarak dosya için `Stream`, etki alanı modelinin adı ve dili, sonra da sonucu bir `DomainModelResults` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`AnalyzeInDomainUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Senaryo sayfasında seçilen dili alır. `ComputerVisionClient.AnalyzeImageByDomainAsync` yöntemini çağırır, görüntü URL 'sini, görsel özellikleri ve dili geçirerek bir `DomainModelResults` örneği olarak sonucunu döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-describe-image-scenario"></a>Görüntüyü açıkla senaryosunu keşfet

Bu senaryo, DescribePage. xaml sayfası tarafından yönetilir. Görüntünün okunabilir bir açıklamasını oluşturmak için bir dil seçebilirsiniz ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* Uploadanddescribeımageasync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.DescribeImageInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* DescribeUrlAsync  
  Bu yöntem, `ComputerVisionClient.DescribeImageAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndDescribeImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra senaryo sayfasında seçilen dili alır. `ComputerVisionClient.DescribeImageInStreamAsync` yöntemini çağırarak dosya için `Stream`, en fazla aday sayısı (Bu durumda 3) ve dili, sonra da sonucu bir `ImageDescription` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`DescribeUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Senaryo sayfasında seçilen dili alır. `ComputerVisionClient.DescribeImageAsync` yöntemini çağırarak, görüntü URL 'sini, en fazla aday sayısını (Bu durumda 3) ve dili geçirerek bir `ImageDescription` örneği olarak sonucunu döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-generate-tags-scenario"></a>Etiket oluştur senaryosunu keşfet

Bu senaryo, TagsPage. xaml sayfası tarafından yönetilir. Bir görüntünün görsel özelliklerini etiketlemek için bir dil seçebilirsiniz ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* Uploadandgettagsforımageasync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.TagImageInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* GenerateTagsForUrlAsync  
  Bu yöntem, `ComputerVisionClient.TagImageAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndGetTagsForImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra senaryo sayfasında seçilen dili alır. `ComputerVisionClient.TagImageInStreamAsync` yöntemini çağırır, dosya ve dil için `Stream` geçirerek sonucu bir `TagResult` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`GenerateTagsForUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Senaryo sayfasında seçilen dili alır. `ComputerVisionClient.TagImageAsync` yöntemini çağırır, görüntü URL 'sini ve dilini geçirerek sonucu bir `TagResult` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-recognize-text-ocr-scenario"></a>Metin Tanıma (OCR) senaryosunu keşfet

Bu senaryo, OCRPage. xaml sayfası tarafından yönetilir. Bir görüntüden yazdırılmış metni tanımak ve ayıklamak için bir dil seçebilirsiniz ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* Uploadandrecognizeımageasync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.RecognizePrintedTextInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* RecognizeUrlAsync  
  Bu yöntem, `ComputerVisionClient.RecognizePrintedTextAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndRecognizeImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra senaryo sayfasında seçilen dili alır. Yönlendirmenin algılanmadığını ve dosya ve dil için `Stream` geçirmediğini belirten `ComputerVisionClient.RecognizePrintedTextInStreamAsync` yöntemini çağırır ve sonra sonucu bir `OcrResult` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`RecognizeUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Senaryo sayfasında seçilen dili alır. Yönlendirmenin algılanmadığını ve görüntü URL 'sini ve dilini geçirmediğini belirten `ComputerVisionClient.RecognizePrintedTextAsync` yöntemini çağırır ve sonra sonucu bir `OcrResult` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-recognize-text-v2-english-scenario"></a>Metin Tanıma v2 (Ingilizce) senaryosunu keşfet

Bu senaryo, TextRecognitionPage. xaml sayfası tarafından yönetilir. Bir görüntüden yazılı veya el yazısı metinleri zaman uyumsuz olarak tanıyacak ve ayıklayabileceğiniz tanıma modunu ve dilini seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* Uploadandrecognizeımageasync  
  Bu yöntem, görüntünün `Stream` olarak kodlanması ve `RecognizeAsync` yöntemini çağırarak ve `ComputerVisionClient.RecognizeTextInStreamAsync` yöntemine yönelik parametreli bir temsilci geçirerek Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* RecognizeUrlAsync  
  Bu yöntem, `RecognizeAsync` yöntemini çağırarak ve `ComputerVisionClient.RecognizeTextAsync` yöntemi için parametreli bir temsilci geçirerek, görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.
* RecognizeAsync bu yöntem hem `UploadAndRecognizeImageAsync` hem de `RecognizeUrlAsync` yöntemlerinin zaman uyumsuz çağrılmasını ve `ComputerVisionClient.GetTextOperationResultAsync` yöntemini çağırarak sonuçların yoklanmasını işler.

Görüntü İşleme örnek uygulamasına dahil edilen diğer senaryolardan farklı olarak, bu senaryo zaman uyumsuzdur, ancak bu yöntem, işlemi başlatmak için çağrılır, ancak durumu denetlemek ve bu işlemin sonuçlarını döndürmek için farklı bir yöntem çağırılır. Bu senaryodaki mantıksal akış, diğer senaryolardan biraz farklıdır.

`UploadAndRecognizeImageAsync` yöntemi `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar, sonra `RecognizeAsync` yöntemini çağırır, geçirme:

* `ComputerVisionClient.RecognizeTextInStreamAsync` yönteminin parametreli zaman uyumsuz temsilcisi için bir lambda ifadesi; dosya için `Stream` ve `GetHeadersAsyncFunc`' de parametre olarak tanıma modu.
* `GetOperationUrlFunc`içinde `Operation-Location` yanıt üst bilgisi değerini almak için bir temsilciye yönelik lambda ifadesi.

`RecognizeUrlAsync` yöntemi, geçirerek `RecognizeAsync` yöntemini çağırır:

* `ComputerVisionClient.RecognizeTextAsync` yönteminin parametreli zaman uyumsuz temsilcisi için bir lambda ifadesi, uzak görüntünün URL 'SI ve tanıma modu, `GetHeadersAsyncFunc`olarak parametreler olarak.
* `GetOperationUrlFunc`içinde `Operation-Location` yanıt üst bilgisi değerini almak için bir temsilciye yönelik lambda ifadesi.

`RecognizeAsync` yöntemi tamamlandığında, `UploadAndRecognizeImageAsync` ve `RecognizeUrlAsync` yöntemlerinin her ikisi de sonucu bir `TextOperationResult` örneği olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`RecognizeAsync` yöntemi, `GetHeadersAsyncFunc` geçirilen `ComputerVisionClient.RecognizeTextInStreamAsync` veya `ComputerVisionClient.RecognizeTextAsync` yöntemi için parametreli temsilciyi çağırır ve yanıt bekler. Daha sonra yöntemi, yanıttan `Operation-Location` yanıt üst bilgisi değerini almak için `GetOperationUrlFunc` geçirilen temsilciyi çağırır. Bu değer, Görüntü İşleme `GetHeadersAsyncFunc` geçirilen yöntemin sonuçlarını almak için kullanılan URL 'dir.

`RecognizeAsync` yöntemi daha sonra, `GetHeadersAsyncFunc`geçirilen yöntemin durumunu ve sonucunu almak için `Operation-Location` yanıt başlığından alınan URL 'YI geçirerek `ComputerVisionClient.GetTextOperationResultAsync` yöntemini çağırır. Durum, yöntemin tamamlandığını, başarıyla veya başarısız olduğunu belirtmezse `RecognizeAsync` yöntemi `ComputerVisionClient.GetTextOperationResultAsync` 3 kez çağrı yapmaz ve çağrılar arasında 3 saniye bekler. `RecognizeAsync` yöntemi, sonuçları çağıran yönteme döndürür.

## <a name="explore-the-get-thumbnail-scenario"></a>Küçük resim al senaryosunu keşfet

Bu senaryo ThumbnailPage. xaml sayfası tarafından yönetilir. Akıllı kırpma kullanıp kullanmayacağınızı belirtebilir, bir görüntüden küçük resim oluşturmak için istenen yükseklik ve genişlik belirtmenize ve hem görüntüyü hem de sonuçları görmenizi sağlayabilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndThumbnailImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlanması ve `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntemi çağırarak Görüntü İşleme gönderilmesi gereken yerel görüntüler için kullanılır.
* ThumbnailUrlAsync  
  Bu yöntem, `ComputerVisionClient.GenerateThumbnailAsync` yöntemini çağırarak görüntünün URL 'sinin Görüntü İşleme gönderildiği uzak görüntüler için kullanılır.

`UploadAndThumbnailImageAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiğinden, bu görüntünün içeriğini Görüntü İşleme göndermelidir. `Stream`olarak okumak için `imageFilePath` belirtilen yerel dosyayı açar. `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntemini çağırarak, genişliği, yüksekliği, dosyanın `Stream` ve akıllı kırpma kullanıp kullanmayacağınızı `Stream`olarak sonucunu döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

`RecognizeUrlAsync` yöntemi, belirtilen abonelik anahtarını ve uç nokta URL 'sini kullanarak yeni bir `ComputerVisionClient` örneği oluşturur. `ComputerVisionClient.GenerateThumbnailAsync` yöntemini çağırarak, genişliği, yüksekliği, resmin URL 'sini geçirerek ve akıllı kırpma kullanıp kullanmayacağınızı ve sonra sonucu bir `Stream`olarak döndürür. `ImageScenarioPage` sınıfından devralınan Yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, `Microsoft/Cognitive-Vision-Windows` deposunu Klonladığınız klasörü silin. Örnek görüntüleri kullanmayı tercih ettiyseniz, `Microsoft/Cognitive-Face-Windows` depoyu Klonladığınız klasörü de silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yüz hizmeti 'ni kullanmaya başlama](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
