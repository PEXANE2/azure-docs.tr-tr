---
title: 'Öğretici: Azure hizmetlerini tanımak için özel logo dedektörünü kullanın - Özel Vizyon'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, logo algılama senaryosunun bir parçası olarak Custom Vision kullanan örnek bir uygulama dan geçeceksiniz. Özel Görme'nin uçdan uca bir uygulama sunmak için diğer bileşenlerle nasıl kullanıldığını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 51fa6d4859eb4b7f059b499ba73d84d9fc65e6f6
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78398983"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Öğretici: Kamera resimlerinde Azure hizmet logolarını tanıyın

Bu eğitimde, daha büyük bir senaryonun parçası olarak Özel Görme'yi kullanan örnek bir uygulamayı keşfedeceksiniz. Mobil platformlar için bir Xamarin.Forms uygulaması olan AI Visual Provision uygulaması, Azure hizmet logolarının kamera resimlerini analiz eder ve ardından gerçek hizmetleri kullanıcının Azure hesabına dağır. Burada, kullanışlı bir uç-uç uygulama sunmak için diğer bileşenlerle eşgüdüm içinde Özel Görme'yi nasıl kullandığını öğreneceksiniz. Tüm uygulama senaryosunu kendiniz çalıştırabilir veya kurulumun yalnızca Özel Görme bölümünü tamamlayabilir ve uygulamanın nasıl kullandığını keşfedebilirsiniz.

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:

> [!div class="checklist"]
> - Azure hizmet logolarını tanımak için özel bir nesne detektörü oluşturun.
> - Uygulamanızı Azure Bilgisayar Vizyonu ve Özel Görme'ye bağlayın.
> - Uygulamadan Azure hizmetlerini dağıtmak için bir Azure hizmet ana hesabı oluşturun.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2017 veya sonrası](https://www.visualstudio.com/downloads/)
- Visual Studio için Xamarin iş yükü [(Bkz. Xamarin Yükleme](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- Visual Studio için bir iOS veya Android emülatörü
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (isteğe bağlı)

## <a name="get-the-source-code"></a>Kaynak kodunu alma

Sağlanan web uygulamasını kullanmak istiyorsanız, github'daki [AI Görsel Hüküm](https://github.com/Microsoft/AIVisualProvision) deposundan uygulamanın kaynak kodunu kopyalayın veya indirin. Visual Studio'da *Kaynak/VisualProvision.sln* dosyasını açın. Daha sonra, uygulamayı çalıştırabilmek için proje dosyalarından bazılarını edinebilirsiniz.

## <a name="create-an-object-detector"></a>Nesne dedektörü oluşturma

Custom Vision [web sitesinde](https://customvision.ai/) oturum açın ve yeni bir proje oluşturun. Nesne Algılama projesi belirtin ve Logo etki alanını kullanın; bu, hizmetin logo algılama için optimize edilmiş bir algoritma kullanmasına izin verir. 

![Chrome tarayıcısında Custom Vision web sitesinde yeni proje penceresi](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Ardından, Azure hizmet logolarının görüntülerini yükleyerek ve bunları el ile etiketleyerek logo algılama algoritmasını eğitin. AIVisualProvision deposu, kullanabileceğiniz bir dizi eğitim görüntüsü içerir. Web sitesinde, **Eğitim Görselleri** sekmesinde **resim ekle** düğmesini seçin. Ardından deponun **Belgeler/Görüntüler/Training_DataSet** klasörüne gidin. Her resimdeki logoları el ile etiketlemeniz gerekir, bu nedenle yalnızca bu projeyi test ediyorsanız, resimlerin yalnızca bir alt kümesini yüklemek isteyebilirsiniz. Kullanmayı planladığınız her etiketin en az 15 örneğini yükleyin.

Eğitim resimlerini yükledikten sonra, ekranda ilkini seçin. Etiketleme penceresi görüntülenir. Her resimdeki her logo için kutular çizin ve etiket atayın. 

![Custom Vision web sitesinde logo etiketleme](media/azure-logo-tutorial/tag-logos.png)

Uygulama belirli etiket dizeleri ile çalışacak şekilde yapılandırılmıştır. Tanımları *Kaynak\VisualProvision\Services\Recognition\RecognitionService.cs* dosyasında bulabilirsiniz:

[!code-csharp[Tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?name=snippet_constants)]

Bir görüntüyü etiketledikten sonra, bir sonraki görüntüyü etiketlemek için sağa gidin. Bitirdiğinde etiketleme penceresini kapatın.

## <a name="train-the-object-detector"></a>Nesne dedektörünü eğitin

Sol bölmede, resimlerinizi görüntülemek için **Etiketler** anahtarını **Etiketle'ye** ayarlayın. Ardından, modeli eğitmek için sayfanın üst kısmındaki yeşil düğmeyi seçin. Algoritma, yeni görüntülerde aynı etiketleri tanımak için eğitecektir. Ayrıca, doğruluk puanları oluşturmak için modeli mevcut görüntülerin bazılarında test edecektir.

![Özel Vizyon web sitesi, Eğitim Görselleri sekmesinde. Bu ekran görüntüsünde, Tren düğmesi özetlenmiştir](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Tahmin URL'sini alın

Modeliniz eğitildikten sonra, bu modeli uygulamanıza entegre etmeye hazırsınız. Bitiş noktası URL'sini (uygulamanın sorgulayacak modelinizin adresini) ve tahmin anahtarını (uygulamanın tahmin isteklerine erişimini sağlamak için) almanız gerekir. **Performans** sekmesinde, sayfanın üst kısmındaki **Tahmin URL** düğmesini seçin.

![BIR URL adresi ve API anahtarı görüntüleyen Bir Tahmin API penceresi gösteren Özel Vizyon web sitesi](media/azure-logo-tutorial/cusvis-endpoint.png)

*Kaynak\VisualProvision\AppSettings.cs* dosyasındaki ilgili alanlara bitiş noktası URL'sini ve **Tahmin Anahtarı** değerini kopyalayın:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_cusvis_keys)]

## <a name="examine-custom-vision-usage"></a>Özel Görme kullanımını inceleyin

Uygulamanın Custom Vision anahtarınızı ve bitiş noktası URL'nizi nasıl kullandığını görmek için *Kaynak/VisualProvision/Services/Recognition/CustomVisionService.cs* dosyasını açın. **PredictImageContentsAsync** yöntemi, bir iptal belirteciyle birlikte bir görüntü dosyasının bayt akışını (eşzamanlı görev yönetimi için) alır, Özel Görüş tahmin API'sini çağırır ve tahmin sonucunu döndürür. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?name=snippet_prediction)]

Bu sonuç, kendisi **Tahmin** örneklerinin bir listesini içeren bir **Tahmin Sonucu** örneği biçimindedir. **Tahmin,** görüntüde algılanan bir etiket ve sınırlayıcı kutusu konumunu içerir.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?name=snippet_prediction_class)]

Uygulamanın bu verileri nasıl işlediği hakkında daha fazla bilgi edinmek için **GetResourcesAsync** yöntemiyle başlayın. Bu yöntem *Kaynak/VisualProvision/Services/Recognition/RecognitionService.cs* dosyasında tanımlanmıştır.  

## <a name="add-computer-vision"></a>Bilgisayar Görüşü Ekle

Öğreticinin Özel Vizyon bölümü tamamlandı. Uygulamayı çalıştırmak istiyorsanız, Computer Vision hizmetini de entegre etmeniz gerekir. Uygulama, logo algılama işlemini tamamlamak için Computer Vision metin tanıma özelliğini kullanır. Azure logosu görünümüyle *veya* yakınına yazdırılan metinle tanınabilir. Custom Vision modellerinin aksine, Computer Vision resimler veya videolar üzerinde belirli işlemleri gerçekleştirmek için önceden eğitilir.

Anahtar ve bitiş noktası URL'si almak için Computer Vision hizmetine abone olun. Bu adımda yardım için [abonelik anahtarlarını nasıl elde edebilirsiniz'a](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe)bakın.

![Quickstart menüsü yle Azure portalındaki Computer Vision hizmeti. API uç noktası URL'si gibi tuşlar için bir bağlantı özetlenmiştir](media/azure-logo-tutorial/comvis-keys.png)

Ardından, *Kaynak\VisualProvision\AppSettings.cs* dosyasını açın `ComputerVisionEndpoint` ve `ComputerVisionKey` değişkenleri doğru değerlerle doldurun.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_comvis_keys)]

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Uygulama, Azure aboneliğinize hizmet dağıtmak için bir Azure hizmet ana hesabı gerektirir. Hizmet sorumlusu, rol tabanlı erişim denetimini kullanarak bir uygulamaya belirli izinleri temsilciolarak vermenizi sağlar. Daha fazla bilgi için [hizmet ilkeleri kılavuzuna](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals)bakın.

Burada gösterildiği gibi Azure Bulut Shell veya Azure CLI'yi kullanarak bir hizmet ilkesi oluşturabilirsiniz. Başlamak için oturum açın ve kullanmak istediğiniz aboneliği seçin.

```azurecli
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Ardından hizmet prensiinizi oluşturun. (Bu işlemin tamamlanması biraz zaman alabilir.)

```azurecli
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

Başarılı bir şekilde tamamlandıktan sonra, gerekli kimlik bilgileri de dahil olmak üzere aşağıdaki JSON çıktısını görmeniz gerekir.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```

Ve `tenantId` değerlere `clientId` dikkat edin. *Kaynak\VisualProvision\AppSettings.cs* dosyasındaki uygun alanlara ekleyin.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_serviceprincipal)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bu noktada, uygulamaya aşağıdakilere erişim izni vermiş siniz:

- Eğitimli bir Custom Vision modeli
- Bilgisayarlı Vizyon hizmeti
- Hizmet ana hesabı

Uygulamayı çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Solution Explorer'da **VisualProvision.Android** projesini veya **VisualProvision.iOS** projesini seçin. Ana araç çubuğundaki açılır menüden karşılık gelen bir emülatör veya bağlı bir mobil cihaz seçin. Sonra uygulamayı çalıştırın.

    > [!NOTE]
    > iOS emülatörü çalıştırmak için bir MacOS aygıtına ihtiyacınız olacaktır.

1. İlk ekranda, hizmetin asıl müşteri kimliğini, kiracı kimliğini ve parolasını girin. **Giriş** düğmesini seçin.

    > [!NOTE]
    > Bazı emülatörlerde **Giriş** düğmesi bu adımda etkinleştirilmeyebilir. Bu durumda uygulamayı durdurun, *Kaynak/VisualProvision/Pages/LoginPage.xaml* dosyasını `Button` açın, **Gİrİş BUTONU**etiketli öğeyi bulun, aşağıdaki satırı kaldırın ve uygulamayı yeniden çalıştırın.
    >  ```xaml
    >  IsEnabled="{Binding IsValid}"
    >  ```
    
    ![Hizmet temel kimlik bilgileri için alanları gösteren uygulama ekranı](media/azure-logo-tutorial/app-credentials.png)

1. Bir sonraki ekranda, açılan menüden Azure aboneliğinizi seçin. (Bu menü, hizmet müdürünün erişesahip olduğu tüm abonelikleri içermelidir.) Devam **et** düğmesini seçin. Bu noktada, uygulama aygıtın kamera ve fotoğraf depolama erişimi vermek isteyebilir. Erişim izinlerini ver.

    ![Hedef Azure aboneliği için açılır alan gösteren uygulama ekranı](media/azure-logo-tutorial/app-az-subscription.png)


1. Cihazınızdaki kamera etkinleştirilir. Eğittiğiniz Azure hizmet logolarından birinin fotoğrafını çekin. Dağıtım penceresi, yeni hizmetler için bir bölge ve kaynak grubu seçmeni istenmelidir (bunları Azure portalından dağıtıyorsanız yapacağınız gibi). 

    ![Azure logolarının iki kağıt kesimine odaklanan bir akıllı telefon kamera ekranı](media/azure-logo-tutorial/app-camera-capture.png)

    ![Dağıtım bölgesi ve kaynak grubu için alanları gösteren bir uygulama ekranı](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu senaryonun tüm adımlarını izlediyseniz ve uygulamayı Azure hizmetlerini hesabınıza dağıtmak için kullandıysanız, [Azure portalına](https://ms.portal.azure.com/)gidin. İşte, kullanmak istemediğiniz hizmetleri iptal edin.

Özel Görme ile kendi nesne algılama projenizi oluşturmayı planlıyorsanız, bu öğreticide oluşturduğunuz logo algılama projesini silmek isteyebilirsiniz. Custom Vision için ücretsiz deneme sadece iki proje sağlar. Logo algılama projesini silmek için, [Custom Vision web sitesinde,](https://customvision.ai) **Projeleri** açın ve ardından My **New Project'in**altındaki çöp simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, mobil kamera görüntülerindeki logoları algılamak için Özel Vizyon hizmetini kullanan tam özellikli bir Xamarin.Forms uygulamasını kurdunuz ve keşfettiniz. Ardından, kendi uygulamanız için bir model oluşturduğunuzda, onu güçlü ve doğru hale getirebilmeniz için Özel Vizyon modeli oluşturmak için en iyi uygulamaları öğrenin.

> [!div class="nextstepaction"]
> [Sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md)
