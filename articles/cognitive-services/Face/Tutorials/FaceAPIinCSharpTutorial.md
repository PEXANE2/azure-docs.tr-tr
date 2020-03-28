---
title: "Öğretici: .NET SDK'yı kullanarak görüntüdeki yüz verilerini algılama ve görüntüleme"
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, görüntüdeki yüzleri algılamak ve çerçevelemek için Yüz hizmetini kullanan bir Windows uygulaması oluşturursunuz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: a5cf3c59c94134e1d0751c1467cd324a95c366eb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78898812"
---
# <a name="tutorial-create-a-windows-presentation-framework-wpf-app-to-display-face-data-in-an-image"></a>Öğretici: Görüntüdeki yüz verilerini görüntülemek için bir Windows Sunu Çerçevesi (WPF) uygulaması oluşturun

Bu eğitimde, bir görüntüdeki yüzleri algılamak ve bu verileri UI'de sunmak için .NET istemcisi SDK aracılığıyla Azure Face hizmetini nasıl kullanacağınızı öğreneceksiniz. Yüzleri algılayan, her yüzün etrafına bir çerçeve çizen ve durum çubuğunda yüzün açıklamasını görüntüleyen bir WPF uygulaması oluşturursunuz. 

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - WPF uygulaması oluşturma
> - Yüz istemci kitaplığını yükleme
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme
> - Durum çubuğunda vurgulanan yüzün açıklamasını görüntüleme

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)

Tam örnek kodu GitHub'daki [Bilişsel Yüz CSharp örnek](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) deposunda mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 


## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api)ücretsiz deneme abonelik anahtarı alabilirsiniz. Veya Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin. Ardından, sırasıyla ve sırasıyla anahtar `FACE_SUBSCRIPTION_KEY` ve `FACE_ENDPOINT`hizmet bitiş noktası dizesi için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

Yeni bir WPF uygulama projesi oluşturmak için aşağıdaki adımları izleyin.

1. Visual Studio'da Yeni Proje iletişim kutusunu açın. **Yükle,** sonra **Visual C#** seçeneğini belirleyin, ardından **WPF App'ı (.NET Framework)** seçin.
1. Uygulamaya **FaceTutorial** adını verin ve **Tamam**'a tıklayın.
1. Gereken NuGet paketlerini alın. Solution Explorer'da projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin; ardından aşağıdaki paketi bulun ve yükleyin:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.5.0-önizleme.1](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1)

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

Bu bölümde, uygulamanın temel çerçevesini, yüze özgü özellikleri olmadan eklersiniz.

### <a name="create-the-ui"></a>UI oluşturma

*MainWindow.xaml'ı* açın ve içeriği&mdash;aşağıdaki kodla değiştirin, bu kod UI penceresini oluşturur. Ve `FacePhoto_MouseMove` `BrowseButton_Click` yöntemler, daha sonra tanımladığınız olay işleyicileridir.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?name=snippet_xaml)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

*MainWindow.xaml.cs* açın ve istemci kitaplığı ad alanlarını ve diğer gerekli ad alanlarını ekleyin. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_using)]

Ardından, **MainWindow** sınıfına aşağıdaki kodu ekleyin. Bu kod, abonelik anahtarı nı ve bitiş noktasını kullanarak bir **FaceClient** örneği oluşturur.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mainwindow_fields)]

Sonraki **Ana Pencere** oluşturucu ekleyin. Uç nokta URL dizenizi denetler ve istemci nesnesiyle ilişkilendirer.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mainwindow_constructor)]

Son olarak, **BrowseButton_Click** ve **FacePhoto_MouseMove** yöntemlerini sınıfa ekleyin. Bu yöntemler *MainWindow.xaml'da*bildirilen olay işleyicilerine karşılık gelir. BrowseButton_Click **BrowseButton_Click** yöntemi, kullanıcının bir .jpg görüntüsü seçmesini sağlayan bir **OpenFileDialog**oluşturur. Daha sonra görüntüyü ana pencerede görüntüler. Daha sonraki adımlarda **BrowseButton_Click** ve **FacePhoto_MouseMove** için kalan kodu eklersiniz. Ayrıca `faceList` başvurunun&mdash; **AlgıYüz** nesnelerinin bir listesini de not edin. Bu başvuru, uygulamanızın gerçek yüz verilerini depoladığı ve çağıracağı yerdir.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_start)]

<!-- [!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_end)] -->

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_start)]

<!-- [!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_end)] -->

### <a name="try-the-app"></a>Uygulamayı deneyin

Uygulamanızı test etmek için menüde **Başlat**'a basın. Uygulama penceresi açıldığında, sol alt köşede **Gözat'ı** tıklatın. **Dosya Aç** iletişim kutusu görünmelidir. Dosya sisteminizden bir resim seçin ve pencerede görüntüleninip görüntülenindiğini doğrulayın. Ardından uygulamayı kapatın ve bir sonraki adıma geçin.

![Yüzlerin değiştirilmemiş resmini gösteren ekran görüntüsü](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Görüntü yükleme ve yüzleri algılama

Uygulamanız, yerel bir resim yüklemek için REST API'sini [algılayan](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) **FaceClient.Face.DetectWithStreamAsync** yöntemini arayarak yüzleri algılar.

**MainWindow** sınıfına aşağıdaki yöntemi **FacePhoto_MouseMove** yönteminin altına ekleyin. Bu yöntem, alınacak yüz öznitelikleri listesini tanımlar ve gönderilen resim dosyasını **akışa**okur. Daha sonra bu nesnelerin her ikisini de **DetectWithStreamAsync** yöntemi çağrısına geçirir.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_uploaddetect)]

## <a name="draw-rectangles-around-faces"></a>Yüzlerin etrafına dikdörtgenler çizin

Ardından, görüntüde algılanan her yüzün etrafına bir dikdörtgen çizmek için kodu eklersiniz. **MainWindow** sınıfına, `FacePhoto.Source = bitmapSource` satırdan sonra **BrowseButton_Click** yönteminin sonuna aşağıdaki kodu ekleyin. Bu **kod, YüklemeAndDetectFaces**için çağrı algılanan yüzlerin bir listesini doldurur. Daha sonra her yüzün etrafına bir dikdörtgen çizer ve değiştirilmiş görüntüyü ana pencerede görüntüler.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_browsebuttonclick_mid)]

## <a name="describe-the-faces"></a>Yüzleri açıklayın

**UploadAndDetectFaces** yönteminin altındaki **MainWindow** sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem, alınan yüz özniteliklerini yüzü açıklayan bir dize dönüştürür.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_facedesc)]

## <a name="display-the-face-description"></a>Yüz açıklamasını görüntüleme

**FacePhoto_MouseMove** yöntemine aşağıdaki kodu ekleyin. Bu olay işleyicisi, `faceDescriptionStatusBar` imleç algılanan bir yüz dikdörtgeninin üzerinde gezinirken yüz açıklaması dizesini görüntüler.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?name=snippet_mousemove_mid)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüz içeren bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Görüntüdeki yüzlerin her birinde kırmızı bir dikdörtgen görmeniz gerekir. Fareyi bir yüz dikdörtgeninin üzerinde hareket ettirirseniz, durum çubuğunda bu yüzün açıklaması görünmelidir.

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Yüz hizmetini .NET SDK'yı kullanmak için temel işlemi öğrendiniz ve görüntüdeki yüzleri algılamak ve çerçevelemek için bir uygulama oluşturdunuz. Ardından, yüz algılama ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
