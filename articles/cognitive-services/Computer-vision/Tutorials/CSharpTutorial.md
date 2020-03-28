---
title: "Örnek: C'de bir görüntü işleme uygulamasını keşfedin #"
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'de Computer Vision API kullanan temel bir Windows uygulamasını keşfedin. OCR gerçekleştirin, küçük resimler oluşturun ve bir görüntüdeki görsel özelliklerle çalışın.
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76168873"
---
# <a name="sample-explore-an-image-processing-app-with-c"></a>Örnek: C ile görüntü işleme uygulamasını keşfedin #

Optik karakter tanıma (OCR) gerçekleştirmek için Computer Vision kullanan, akıllı kırpılmış küçük resimler oluşturmak ve bir görüntüde yüzler de dahil olmak üzere görsel özellikleri algılamak, kategorilere ayırmak, etiketlemek ve tanımlamak için bilgisayar görüşü kullanan temel bir Windows uygulamasını keşfedin. Aşağıdaki örnek bir görüntü URL'si veya yerel ortamda depolanan dosya göndermenizi sağlar. Bu açık kaynak örneğini, .NET Framework'ün bir parçası olan Computer Vision API ve Windows Presentation Foundation 'ı (WPF) kullanarak Windows için kendi uygulamanızı oluşturmak için şablon olarak kullanabilirsiniz.

> [!div class="checklist"]
> * Örnek uygulamayı GitHub'dan alın
> * Visual Studio'da örnek uygulamayı açın ve oluşturun
> * Örnek uygulamayı çalıştırın ve çeşitli senaryolar gerçekleştirmek için uygulamayla etkileşimkurun
> * Örnek uygulamayla birlikte verilen çeşitli senaryoları keşfedin

## <a name="prerequisites"></a>Ön koşullar

Örnek uygulamayı keşfetmeden önce aşağıdaki ön koşulları yerine getirebildiğinizden emin olun:

* [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) veya üzerine sahip olmanız gerekir.
* Görüntü İşleme için bir abonelik anahtarınız olması gerekir. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)ücretsiz bir deneme anahtarı alabilirsiniz. Veya Computer Vision'a abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin. Hizmet bitiş noktası URL'sini de not alın.

## <a name="get-the-sample-app"></a>Örnek uygulamayı alma

Computer Vision örnek uygulaması GitHub'da `Microsoft/Cognitive-Vision-Windows` depodan edinilebilir. Bu depo aynı zamanda `Microsoft/Cognitive-Common-Windows` Git alt modülü olarak depoyu da içerir. Alt modül de dahil olmak üzere bu depoyu komut satırındaki `git clone --recurse-submodules` komutu kullanarak veya GitHub Desktop'ı kullanarak tekrartekrar klonlayabilirsiniz.

Örneğin, Bilgisayar Görüşü örnek uygulamasının deposunu bir komut isteminden yinelemeli olarak klonlamak için aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Vision-Windows.git
```

> [!IMPORTANT]
> Bu depoyu ZIP olarak indirmeyin. Git, ZIP olarak bir depo indirirken alt modülleri içermez.

### <a name="get-optional-sample-images"></a>İsteğe bağlı örnek görüntüler alın

İsteğe bağlı olarak, GitHub'da bulunan [Face](../../Face/Overview.md) örnek uygulamasıyla `Microsoft/Cognitive-Face-Windows` birlikte verilen örnek görüntüleri depodan kullanabilirsiniz. Bu örnek uygulama, `/Data`birden çok kişinin görüntülerini içeren bir klasör içerir. Computer Vision örnek uygulaması için açıklanan yöntemlerle bu depoyu da tekrartekrar klonlayabilirsiniz.

Örneğin, Face örnek uygulamasının deposunu bir komut isteminden yinelemek için aşağıdaki komutu çalıştırın:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Face-Windows.git
```

## <a name="open-and-build-the-sample-app-in-visual-studio"></a>Visual Studio'da örnek uygulamayı açın ve oluşturun

Örnek uygulamayı çalıştırmadan veya keşfetmeden önce Visual Studio'nun bağımlılıkları çözebilmeleri için önce örnek uygulamayı oluşturmanız gerekir. Örnek uygulamayı açmak ve oluşturmak için aşağıdaki adımları yapın:

1. Visual Studio çözüm dosyasını Visual `/Sample-WPF/VisionAPI-WPF-Samples.sln`Studio'da açın.
1. Visual Studio çözümlü iki proje içerdiğinden emin olun:  

   * SampleUserControlLibrary
   * VisionAPI-WPF Örnekleri  

   SampleUserControlLibrary projesi kullanılamıyorsa, `Microsoft/Cognitive-Vision-Windows` depoyu tekrartekrar klonladığınızı doğrulayın.
1. Visual Studio'da Ctrl+Shift+B tuşuna basın veya şerit menüsünden **Oluştur'u** seçin ve ardından çözümü oluşturmak için **Çözüm Oluştur'u** seçin.

## <a name="run-and-interact-with-the-sample-app"></a>Örnek uygulamayı çalıştırın ve etkileşimde verin

Küçük resim oluşturma veya resim etiketleme gibi çeşitli görevleri gerçekleştirirken sizinle ve Computer Vision istemci kitaplığıyla nasıl etkileşimde olduğunu görmek için örnek uygulamayı çalıştırabilirsiniz. Örnek uygulamayı çalıştırmak ve bunlarla etkileşimde kalmak için aşağıdaki adımları yapın:

1. Yapı tamamlandıktan sonra, **F5** tuşuna basın veya şerit menüsünden **Hata Ayıklama'yı** seçin ve ardından örnek uygulamayı çalıştırmak için **hata ayıklama** başlat'ı seçin.
1. Örnek uygulama görüntülendiğinde, Abonelik Anahtarı Yönetimi sayfasını görüntülemek için gezinti bölmesinden **Abonelik Anahtarı Yönetimi'ni** seçin.
   ![Abonelik Anahtar Yönetimi sayfası](../Images/Vision_UI_Subscription.PNG)  
1. **Abonelik Anahtarı'na**abonelik anahtarınızı girin.
1. **Bitiş Noktası'na**bitiş noktası URL'sini girin.  
   Örneğin, Computer Vision ücretsiz deneme sürümünden abonelik anahtarını kullanıyorsanız, aşağıdaki uç nokta URL'sini girin:`https://westcentralus.api.cognitive.microsoft.com`  
   [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
1. Örnek uygulamayı bir sonraki çalıştırışınızda abonelik anahtarınızı ve bitiş noktası URL'nizi girmek istemiyorsanız, abonelik anahtarını ve bitiş noktası URL'sini bilgisayarınıza kaydetmek için **Ayar Kaydet'i** seçin. Önceden kaydedilmiş abonelik anahtarınızı ve bitiş noktası URL'nizi silmek **istiyorsanız, Ayar Sil'i**seçin.

   > [!NOTE]
   > Örnek uygulama, abonelik anahtarınızı `System.IO.IsolatedStorage`ve bitiş noktası URL'nizi depolamak için yalıtılmış depolama ve son nokta URL'nizi kullanır.

1. Gezinti bölmesinde **bir senaryo seçin'in** altında, örnek uygulamada şu anda bulunan senaryolardan birini seçin:  

   | Senaryo | Açıklama |
   |----------|-------------|
   |Görüntüyü Analiz Et | Yerel veya uzak bir görüntüyü çözümlemek için [Görüntü Analiz](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) et işlemini kullanır. Analiz için görsel özellikleri ve dili seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz.  |
   |Etki Alanı Modeli ile Görüntüyü Analiz Edin | Seçebileceğiniz etki alanı modellerini listelemek için [Etki Alanına Özel Modeller Listesi'ni](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) ve seçili etki alanı modelini kullanarak yerel veya uzak bir görüntüyü çözümlemek için Etki Alanına Özel İçeriği [Tanıyın](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) işlemini kullanır. Ayrıca çözümleme için dili de seçebilirsiniz. |
   |Resmi Açıkla | Yerel veya uzak bir görüntünün insan tarafından okunabilir bir açıklamasını oluşturmak için [Resmi Açıkla](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fe) işlemini kullanır. Açıklama için dili de seçebilirsiniz. |
   |Etiketler oluştur | Yerel veya uzak bir görüntünün görsel özelliklerini etiketlemek için [Etiket Görüntüsü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1ff) işlemini kullanır. Etiketler için kullanılan dili de seçebilirsiniz. |
   |Metni Tanı (OCR) | Bir görüntüden yazdırılan metni tanımak ve ayıklamak için [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) işlemini kullanır. Kullanılacak dili seçebilir veya Computer Vision'ın dili otomatik olarak algılamasına izin verebilirsiniz. |
   |Metin V2'yi Tanı (İngilizce) | [Görüntüyü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) tanıma ve [Metin İşleyişi Tanıyın işlemini](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) anlama işlemini kullanır ve bir resimden yazdırılmış veya el yazısıyla yazılmış metinleri eş senkronize olarak tanır ve ayıklar. |
   |Küçük Resmi Al | Yerel veya uzak bir görüntü için küçük resim oluşturmak için [Küçük Resmi Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) işlemini kullanır. |

   Aşağıdaki ekran görüntüsü, örnek bir görüntüyü analiz ettikten sonra Görüntü Analiz senaryosu için sağlanan sayfayı gösterir.
   ![Analiz resim sayfasının ekran görüntüsü](../Images/Analyze_Image_Example.PNG)

## <a name="explore-the-sample-app"></a>Örnek uygulamayı keşfedin

Computer Vision örnek uygulaması için Visual Studio çözümü iki proje içerir:

* SampleUserControlLibrary  
  SampleUserControlLibrary projesi, birden çok Bilişsel Hizmet örneği tarafından paylaşılan işlevsellik sağlar. Proje aşağıdakileri içerir:
  * Örnek Senaryolar  
    Örnekler için başlık çubuğu, gezinti bölmesi ve içerik bölmesi gibi standartlaştırılmış bir sunu sağlayan UserControl. Computer Vision örnek uygulaması, senaryo sayfalarını görüntülemek ve abonelik anahtarı ve bitiş noktası URL'si gibi senaryolar arasında paylaşılan bilgilere erişmek için MainWindow.xaml penceresinde bu denetimi kullanır.
  * AbonelikKeyPage  
    Örnek uygulama için abonelik anahtarı ve uç nokta URL'si girmek için standartlaştırılmış bir düzen sağlayan bir Sayfa. Computer Vision örnek uygulaması, senaryo sayfaları tarafından kullanılan abonelik anahtarı ve uç nokta URL'sini yönetmek için bu sayfayı kullanır.
  * VideoSonuç Kontrolü  
    Video bilgileri için standartlaştırılmış bir sunu sağlayan bir UserControl. Computer Vision örnek uygulaması bu denetimi kullanmaz.
* VisionAPI-WPF Örnekleri  
  Computer Vision örnek uygulaması için ana proje, bu proje Computer Vision için ilginç işlevsellik tüm içerir. Proje aşağıdakileri içerir:
  * AnalyzeInDomainPage.xaml  
    Etki Alanı Modeli senaryosu ile Görüntü Analiz için senaryo sayfası.
  * AnalyzeImage.xaml  
    Görüntü Çözümle senaryosu için senaryo sayfası.
  * AçıklamaPage.xaml  
    Resmi Açıkla senaryosu için senaryo sayfası.
  * ImageScenarioPage.cs  
    Örnek uygulamadaki tüm senaryo sayfalarının türetildiği ImageScenarioPage sınıfı. Bu sınıf, tüm senaryo sayfaları tarafından paylaşılan kimlik bilgileri sağlama ve çıktıyı biçimlendirme gibi işlevselliği yönetir.
  * MainWindow.xaml  
    Örnek uygulamanın ana penceresi, AbonelikKeyPage ve senaryo sayfalarını sunmak için Örnek Senaryolar denetimini kullanır.
  * OCRPage.xaml  
    Metni Tanı (OCR) senaryosu için senaryo sayfası.
  * RecognizeLanguage.cs  
    Örnek uygulamadaki çeşitli yöntemlerle desteklenen diller hakkında bilgi sağlayan RecognizeLanguage sınıfı.
  * EtiketlerPage.xaml  
    Etiketler oluştur senaryosu için senaryo sayfası.
  * TextRecognitionPage.xaml  
    Metin V2'yi Tanı (İngilizce) senaryosu için senaryo sayfası.
  * Küçük ResimPage.xaml  
    Küçük Resmi Al senaryosu için senaryo sayfası.

### <a name="explore-the-sample-code"></a>Örnek kodu keşfedin

Örnek kodun önemli bölümleri, örnek uygulamayı keşfetmenizi kolaylaştırmak `KEY SAMPLE CODE ENDS HERE`için, başlangıç ve bitiş ile başlayan yorum bloklarıyla `KEY SAMPLE CODE STARTS HERE` çerçevelenir. Örnek kodun bu önemli bölümleri, çeşitli görevleri yapmak için Computer Vision API istemci kitaplığını nasıl kullanılacağını öğrenmekle en alakalı kodu içerir. Computer Vision `KEY SAMPLE CODE STARTS HERE` örnek uygulamasındaki kodun en alakalı bölümleri arasında ilerlemek için Visual Studio'da arama yapabilirsiniz. 

Örneğin, aşağıdaki `UploadAndAnalyzeImageAsync` ve AnalyzePage.xaml'da yer alan yöntem, yöntemi çağırarak yerel bir görüntüyü çözümlemek `ComputerVisionClient.AnalyzeImageInStreamAsync` için istemci kitaplığınınasıl kullanacağımı gösterir.

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

### <a name="explore-the-client-library"></a>İstemci kitaplığını keşfedin

Bu örnek uygulama, Azure Bilişsel Hizmetler'deki Computer Vision API için ince bir C# istemci sarıcı olan Computer Vision API istemci kitaplığını kullanır. İstemci kitaplığı NuGet'den [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) paketinde kullanılabilir. Visual Studio uygulamasını oluştururken, istemci kitaplığını ilgili NuGet paketinden aldınız. İstemci kitaplığı için kaynak kodunu `/ClientLibrary` `Microsoft/Cognitive-Vision-Windows` deponun klasöründe de görüntüleyebilirsiniz.

İstemci kitaplığı işlevi `ComputerVisionClient` sınıfın etrafında, `Microsoft.Azure.CognitiveServices.Vision.ComputerVision` ad alanında, bilgisayar görüşü `ComputerVisionClient` ile etkileşimde sınıf tarafından kullanılan `Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models` modeller ad alanında bulunurken. Örnek uygulamayla birlikte verilen çeşitli XAML senaryo sayfalarında, bu `using` ad alanları için aşağıdaki yönergeleri bulacaksınız:

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

Computer Vision örnek uygulamasında yer alan senaryoları incelerken `ComputerVisionClient` sınıfla birlikte verilen çeşitli yöntemler hakkında daha fazla bilgi edineceksiniz.

## <a name="explore-the-analyze-image-scenario"></a>Görüntüyü Analiz Et senaryosunu keşfedin

Bu senaryo AnalyzePage.xaml sayfası tarafından yönetilir. Analiz için görsel özellikleri ve dili seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemlerden birini kullanarak bunu yapar:

* UploadAndAnalyzeImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.AnalyzeImageInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* AnalyzeUrlAsync  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.AnalyzeImageAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndAnalyzeImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Okuma `imageFilePath` için belirtilen yerel dosyayı `Stream`açar, ardından senaryo sayfasında seçilen görsel özellikleri ve dili alır. Dosya, `ComputerVisionClient.AnalyzeImageInStreamAsync` görsel özellikler `Stream` ve dil için geçen yöntemi çağırır, sonra bir `ImageAnalysis` örnek olarak sonucu döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `AnalyzeUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Senaryo sayfasında seçilen görsel özellikleri ve dili alır. Yöntem, `ComputerVisionClient.AnalyzeImageInStreamAsync` görüntü URL' si, görsel özellikleri ve dili geçirerek, `ImageAnalysis` sonucu bir örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-analyze-image-with-domain-model-scenario"></a>Etki Alanı Modeli senaryosu ile Görüntüyü Analiz Et

Bu senaryo AnalyzeInDomainPage.xaml sayfası tarafından yönetilir. Görüntünün etki alanına özgü `celebrities` bir `landmarks`çözümlemesi gerçekleştirmek için bir etki alanı modeli veya , ve dil seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* GetAvailableDomainModelsAsync  
  Bu yöntem, Kullanılabilir etki alanı modellerinin listesini `_domainModelComboBox` Computer Vision'dan alır ve `ComputerVisionClient.ListModelsAsync` yöntemi kullanarak sayfada ComboBox denetimini doldurulur.
* UploadAndAnalyzeInDomainImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* AnalyzeInDomainUrlAsync  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.AnalyzeImageByDomainAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndAnalyzeInDomainImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Okuma `imageFilePath` için belirtilen yerel dosyayı `Stream`açar, ardından senaryo sayfasında seçilen dili alır. Dosya `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` `Stream` için, etki alanı modelinin adını ve dili geçirerek yöntemi çağırır ve sonucu `DomainModelResults` örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `AnalyzeInDomainUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Senaryo sayfasında seçilen dili alır. Yöntem, `ComputerVisionClient.AnalyzeImageByDomainAsync` görüntü URL' si, görsel özellikleri ve dili geçirerek, `DomainModelResults` sonucu bir örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-describe-image-scenario"></a>Resmi Açıkla senaryosunu keşfedin

Bu senaryo DescribePage.xaml sayfası tarafından yönetilir. Görüntünün insan tarafından okunabilir bir açıklamasını oluşturmak için bir dil seçebilir ve hem görüntüyü hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndDescribeImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.DescribeImageInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* AçıklamaUrlAsync  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.DescribeImageAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndDescribeImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Okuma `imageFilePath` için belirtilen yerel dosyayı `Stream`açar, ardından senaryo sayfasında seçilen dili alır. Bu `ComputerVisionClient.DescribeImageInStreamAsync` yöntem, dosya `Stream` için geçen, aday ların maksimum sayısı (bu durumda, 3) ve dil, `ImageDescription` sonra bir örnek olarak sonucu döndürür çağırır. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `DescribeUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Senaryo sayfasında seçilen dili alır. Yöntem, `ComputerVisionClient.DescribeImageAsync` görüntü URL, aday (bu durumda, 3) ve dil maksimum sayısı geçen, sonra `ImageDescription` bir örnek olarak sonucu döndürür çağırır. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-generate-tags-scenario"></a>Etiketler oluştur senaryosunu keşfedin

Bu senaryo TagsPage.xaml sayfası tarafından yönetilir. Görüntünün görsel özelliklerini etiketlemek ve hem görüntüyü hem de sonuçları görmek için bir dil seçebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndGetEtiketlerForImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.TagImageInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* ÜretTagsForUrlAsync  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.TagImageAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndGetTagsForImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Okuma `imageFilePath` için belirtilen yerel dosyayı `Stream`açar, ardından senaryo sayfasında seçilen dili alır. Dosya ve `ComputerVisionClient.TagImageInStreamAsync` dil `Stream` için geçen yöntemi çağırır, sonra bir `TagResult` örnek olarak sonucu döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `GenerateTagsForUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Senaryo sayfasında seçilen dili alır. Yöntem çağırır, `ComputerVisionClient.TagImageAsync` görüntü URL'sini ve dili geçer, sonra `TagResult` sonucu bir örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-recognize-text-ocr-scenario"></a>Metni Tanı (OCR) senaryosunu keşfedin

Bu senaryo OCRPage.xaml sayfası tarafından yönetilir. Bir görüntüden yazdırılan metni tanımak ve ayıklamak için bir dil seçebilir ve hem resmi hem de sonuçları görebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndRecognizeImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.RecognizePrintedTextInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* UrlAsync'i Tanıyın  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.RecognizePrintedTextAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndRecognizeImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Okuma `imageFilePath` için belirtilen yerel dosyayı `Stream`açar, ardından senaryo sayfasında seçilen dili alır. Yönlendirmenin `ComputerVisionClient.RecognizePrintedTextInStreamAsync` algılanmadığını belirten ve dosya ve `Stream` dil için geçen yöntemi çağırır, sonra `OcrResult` sonucu bir örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `RecognizeUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Senaryo sayfasında seçilen dili alır. Yönlendirmenin `ComputerVisionClient.RecognizePrintedTextAsync` algılanmadığını belirten ve görüntü URL'sini ve dili geçen yöntemi çağırır `OcrResult` ve sonucu örnek olarak döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="explore-the-recognize-text-v2-english-scenario"></a>Metin V2'yi Tanı (İngilizce) senaryosunu keşfedin

Bu senaryo TextRecognitionPage.xaml sayfası tarafından yönetilir. Tanıma modunu ve bir görüntüyü eş senkronize olarak tanıyacak ve yazdırılan veya el yazısıyla yazılmış metni ayıklamak ve hem resmi hem de sonuçları görmek için bir dil seçebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndRecognizeImageAsync  
  Bu yöntem, görüntünün a `Stream` olarak kodlandığı ve `RecognizeAsync` yöntem için parametreli bir temsilci geçerek Bilgisayar Görüşü'ne `ComputerVisionClient.RecognizeTextInStreamAsync` gönderilmesi gereken yerel görüntüler için kullanılır.
* UrlAsync'i Tanıyın  
  Bu yöntem, `RecognizeAsync` yöntem çağırılarak ve `ComputerVisionClient.RecognizeTextAsync` yöntem için parametrelendirilmiş bir temsilci geçerek görüntünün URL'sinin Computer Vision'a gönderildiği uzak görüntüler için kullanılır.
* RecognizeAsync Bu yöntem, hem yöntem `UploadAndRecognizeImageAsync` hem `RecognizeUrlAsync` de yöntem için çağıran asynchronous işler, hem de `ComputerVisionClient.GetTextOperationResultAsync` yöntemi çağırarak sonuçları için yoklama.

Computer Vision örnek uygulamasında yer alan diğer senaryoların aksine, bu senaryo asynchronous' tur, bu yöntem işlemi başlatmak için çağrılır, ancak durumu denetlemek ve bu işlemin sonuçlarını döndürmek için farklı bir yöntem çağrılır. Bu senaryodaki mantıksal akış, diğer senaryolarda biraz farklıdır.

Yöntem `UploadAndRecognizeImageAsync` olarak okumak `imageFilePath` için belirtilen yerel `Stream`dosyayı açar `RecognizeAsync` , sonra yöntemi çağırır, geçen:

* Yöntemin `ComputerVisionClient.RecognizeTextInStreamAsync` parametreli bir asenkron temsilci için lambda `Stream` ifadesi, dosya için ve parametreler `GetHeadersAsyncFunc`olarak tanıma modu ile, içinde .
* Yanıt üstbilgi değerini almak `Operation-Location` için bir temsilci için `GetOperationUrlFunc`lambda ifadesi, içinde.

Yöntem, `RecognizeUrlAsync` `RecognizeAsync` geçen, yöntem çağırır:

* Yöntemin `ComputerVisionClient.RecognizeTextAsync` parametreli bir asynchronous temsilcisi için lambda ifadesi, uzak görüntünün URL'si `GetHeadersAsyncFunc`ve parametreler olarak tanıma modu ile .
* Yanıt üstbilgi değerini almak `Operation-Location` için bir temsilci için `GetOperationUrlFunc`lambda ifadesi, içinde.

`RecognizeAsync` Yöntem tamamlandığında, hem `UploadAndRecognizeImageAsync` de `RecognizeUrlAsync` yöntemler sonucu örnek `TextOperationResult` olarak döndürer. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `RecognizeAsync` geçirilen `ComputerVisionClient.RecognizeTextInStreamAsync` yöntem için `ComputerVisionClient.RecognizeTextAsync` parametrelendirilmiş temsilciyi `GetHeadersAsyncFunc` çağırır ve yanıtı bekler. Yöntem daha sonra `GetOperationUrlFunc` `Operation-Location` yanıt üstbilgi değerini yanıttan almak için geçen temsilciyi çağırır. Bu değer, Computer `GetHeadersAsyncFunc` Vision'dan geçirilen yöntemin sonuçlarını almak için kullanılan URL'dir.

Yöntem `RecognizeAsync` daha sonra, `ComputerVisionClient.GetTextOperationResultAsync` `Operation-Location` yanıt üstbilgisinden alınan URL'yi geçirerek, yöntemin durumunu ve `GetHeadersAsyncFunc`sonucunu almak için yöntemi çağırır. Durum, yöntemin tamamlandığını, başarılı veya başarısız olduğunu göstermiyorsa, `RecognizeAsync` yöntem aramalar arasında 3 saniye bekleyerek 3 kez daha çağırır. `ComputerVisionClient.GetTextOperationResultAsync` Yöntem, `RecognizeAsync` sonuçları onu çağıran yönteme döndürür.

## <a name="explore-the-get-thumbnail-scenario"></a>Küçük Resmi Al senaryosunu keşfedin

Bu senaryo ThumbnailPage.xaml sayfası tarafından yönetilir. Görüntüden küçük resim oluşturmak ve hem görüntüyü hem de sonuçları görmek için akıllı kırpma kullanıp kullanmayacağınıbelirtebilir ve istediğiniz yükseklik ve genişliği belirtebilirsiniz. Senaryo sayfası, görüntünün kaynağına bağlı olarak aşağıdaki yöntemleri kullanır:

* UploadAndThumbnailImageAsync  
  Bu yöntem, görüntünün bir `Stream` olarak kodlandığı ve `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntemi çağırarak Computer Vision'a gönderilmesi gereken yerel görüntüler için kullanılır.
* Küçük ResimUrlAsync  
  Bu yöntem, görüntünün URL'sinin `ComputerVisionClient.GenerateThumbnailAsync` yöntemi çağırarak Computer Vision'a gönderildiği uzak görüntüler için kullanılır.

Yöntem, `UploadAndThumbnailImageAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Örnek uygulama yerel bir görüntüyü analiz ettiği için, bu görüntünün içeriğini Computer Vision'a göndermesi gerekiyor. Bir `Stream`' olarak okumak `imageFilePath` için belirtilen yerel dosyayı açar. Bu `ComputerVisionClient.GenerateThumbnailInStreamAsync` yöntem çağırır, genişlik, yükseklik, dosya `Stream` için geçen ve akıllı kırpma kullanıp kullanmamak, sonra bir `Stream`sonucu döndürür. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

Yöntem, `RecognizeUrlAsync` belirtilen abonelik `ComputerVisionClient` anahtarı nı ve bitiş noktası URL'sini kullanarak yeni bir örnek oluşturur. Bu `ComputerVisionClient.GenerateThumbnailAsync` yöntem, genişlik, yükseklik, görüntü için URL geçen ve akıllı kırpma kullanmak için, `Stream`sonra bir sonucu döndürür çağırır. `ImageScenarioPage` Sınıftan devralınan yöntemler, döndürülen sonuçları senaryo sayfasında sunar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, depoyu klonladığınız klasörü `Microsoft/Cognitive-Vision-Windows` silin. Örnek görüntüleri kullanmayı seçtiyseniz, `Microsoft/Cognitive-Face-Windows` depoyu klonladığınız klasörü de silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Face hizmetine başlayın](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
