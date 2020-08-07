---
title: 'Öğretici: .NET SDK kullanarak görüntüdeki yüz verileri algılama ve görüntüleme'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir görüntüdeki yüzeyleri algılamak ve çerçeveye eklemek için yüz hizmetini kullanan bir Windows uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 1decd57c8389cb826c370d47793b793c24f9e6b8
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87833683"
---
# <a name="tutorial-create-a-windows-presentation-framework-wpf-app-to-display-face-data-in-an-image"></a>Öğretici: bir görüntüde yüz verileri göstermek için bir Windows Presentation Framework (WPF) uygulaması oluşturma

Bu öğreticide, bir görüntüdeki yüzeyleri tespit etmek ve ardından bu verileri Kullanıcı arabiriminde sunmak için .NET istemci SDK 'Sı aracılığıyla Azure yüz hizmeti 'ni kullanmayı öğreneceksiniz. Yüzleri algılayan, her yüz etrafında bir çerçeve çizen ve durum çubuğunda yüzün açıklamasını görüntüleyen bir WPF uygulaması oluşturacaksınız. 

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - WPF uygulaması oluşturma
> - Yüz istemci kitaplığını yükler
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme
> - Durum çubuğunda vurgulanan yüzün açıklamasını görüntüle

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)

Örnek kodun tamamı GitHub 'daki bilişsel [yüz CSharp örnek](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) deposunda mevcuttur.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun. 


## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Yüz Tanıma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve hizmet uç noktası dizesi için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT` .
- Herhangi bir [Visual Studio](https://www.visualstudio.com/downloads/)sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Yeni bir WPF uygulaması projesi oluşturmak için aşağıdaki adımları izleyin.

1. Visual Studio 'da yeni proje iletişim kutusunu açın. **Yüklü**, sonra **Visual C#** ve **WPF uygulaması (.NET Framework)** öğesini seçin.
1. Uygulamaya **FaceTutorial** adını verin ve **Tamam**'a tıklayın.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. ardından, aşağıdaki paketi bulun ve yükledikten sonra:
    - [Microsoft. Azure. Biliveservices. Vision. Face 2.5.0-Preview. 1](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1)

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

Bu bölümde, uygulamanın temel çerçevesini, yüz özgü özellikleri olmadan ekleyeceksiniz.

### <a name="create-the-ui"></a>Kullanıcı arabirimini oluşturma

*MainWindow. xaml* ' i açın ve içeriği aşağıdaki kodla değiştirin &mdash; . Bu kod, UI penceresini oluşturur. `FacePhoto_MouseMove`Ve `BrowseButton_Click` yöntemleri, daha sonra tanımlayacaksınız olay işleyicileridir.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?name=snippet_xaml)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

*MainWindow.xaml.cs* 'i açın ve diğer gerekli ad alanlarıyla birlikte istemci kitaplığı ad alanlarını ekleyin. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_using)]

Sonra, **MainWindow** sınıfına aşağıdaki kodu ekleyin. Bu kod, abonelik anahtarını ve uç noktayı kullanarak bir **Faceclient** örneği oluşturur.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mainwindow_fields)]

Ardından **MainWindow** oluşturucuyu ekleyin. Uç nokta URL dizenizi denetleyip istemci nesnesiyle ilişkilendirir.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mainwindow_constructor)]

Son olarak, **BrowseButton_Click** ve **FacePhoto_MouseMove** yöntemlerini sınıfına ekleyin. Bu yöntemler, *MainWindow. xaml*içinde belirtilen olay işleyicilerine karşılık gelir. **BrowseButton_Click** yöntemi, kullanıcının bir. jpg görüntüsü seçmesini sağlayan bir **OpenFileDialog**oluşturur. Sonra, görüntüyü ana pencerede görüntüler. Daha sonraki adımlarda **BrowseButton_Click** ve **FacePhoto_MouseMove** için kalan kodu eklersiniz. Ayrıca, `faceList` başvuruya &mdash; yönelik bir dizi nesnenin **DetectedFace** listesine başvurun. Bu başvuru, uygulamanızın gerçek yüz verilerini depolayacağı ve çağıracağı yerdir.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_start)]

<!-- [!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_end)] -->

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_start)]

<!-- [!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_end)] -->

### <a name="try-the-app"></a>Uygulamayı deneyin

Uygulamanızı test etmek için menüde **Başlat**'a basın. Uygulama penceresi açıldığında sol alt köşedeki **Araştır** ' a tıklayın. **Dosya açma** iletişim kutusu görünmelidir. Dosya sisteminden bir görüntü seçin ve pencerede görüntülendiğini doğrulayın. Ardından, uygulamayı kapatın ve sonraki adıma ilerleyin.

![Yüzlerin değiştirilmemiş resmini gösteren ekran görüntüsü](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Görüntüyü karşıya yükleme ve yüzeyleri algılama

Uygulamanız, yerel bir görüntünün karşıya yüklenmesi için [algılama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API sarmalayan **Faceclient. Face. DetectWithStreamAsync** yöntemini çağırarak yüzeyleri algılar.

Aşağıdaki yöntemi, **FacePhoto_MouseMove** yönteminin altında **MainWindow** sınıfına ekleyin. Bu yöntem, gönderilen resim dosyasını almak ve bir **akışa**göndermek için yüz özniteliklerinin bir listesini tanımlar. Daha sonra bu nesnelerin her ikisini de **Detectwithstreamasync** yöntem çağrısına geçirir.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_uploaddetect)]

## <a name="draw-rectangles-around-faces"></a>Yüzler etrafında dikdörtgenler çizme

Sonra, görüntüde algılanan her bir yüzey etrafında bir dikdörtgen çizmek için kodu ekleyeceksiniz. **MainWindow** sınıfında, **BrowseButton_Click** yönteminin sonuna, satırdan sonra aşağıdaki kodu ekleyin `FacePhoto.Source = bitmapSource` . Bu kod, algılanan yüzlerin bir listesini **UploadAndDetectFaces**çağrısından doldurur. Ardından, her bir yüzey etrafında bir dikdörtgen çizer ve değiştirilen görüntüyü ana pencerede görüntüler.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_mid)]

## <a name="describe-the-faces"></a>Yüzeyleri açıkla

Aşağıdaki yöntemi **MainWindow** sınıfına, **UploadAndDetectFaces** yönteminin altına ekleyin. Bu yöntem, alınan yüz özniteliklerini, yüzü açıklayan bir dizeye dönüştürür.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_facedesc)]

## <a name="display-the-face-description"></a>Yüz açıklamasını görüntüleme

**FacePhoto_MouseMove** yöntemine aşağıdaki kodu ekleyin. Bu olay işleyicisi `faceDescriptionStatusBar` , imleç algılanan bir yüz dikdörtgeninin üzerine geldiğinde içindeki yüz açıklaması dizesini görüntüler.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_mid)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüz içeren bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Görüntüdeki her yüzey üzerinde kırmızı bir dikdörtgen görmeniz gerekir. Fareyi bir yüz dikdörtgeninin üzerine getirdiğinizde, bu yüzün açıklaması durum çubuğunda görünmelidir.

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yüz hizmeti .NET SDK 'sını kullanmaya yönelik temel süreci öğrenmiş ve bir görüntüdeki yüzeyleri algılayıp çerçeveli bir uygulama oluşturdunuz. Sonra, yüz algılama 'nın ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
