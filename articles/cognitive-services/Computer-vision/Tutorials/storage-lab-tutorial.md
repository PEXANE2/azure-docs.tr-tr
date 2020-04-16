---
title: 'Öğretici: Azure görüntüleri için meta veri oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, görüntüler için meta veri oluşturmak için Azure Computer Vision hizmetini bir web uygulamasına nasıl entegre acağınızı öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 43172cb08bb1e31c8cff891628ca6ef85cb8c864
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81404421"
---
# <a name="tutorial-use-computer-vision-to-generate-image-metadata-in-azure-storage"></a>Öğretici: Azure Depolama'da görüntü meta verileri oluşturmak için Computer Vision'ı kullanın

Bu eğitimde, yüklenen resimler için meta veri oluşturmak için Azure Computer Vision hizmetini bir web uygulamasına nasıl entegre acağınızı öğreneceksiniz. Bu, bir şirketin tüm resimleri için hızlı bir şekilde açıklayıcı altyazılar veya aranabilir anahtar kelimeler oluşturmak istemesi gibi [dijital varlık yönetimi (DAM)](../Home.md#computer-vision-for-digital-asset-management) senaryoları için yararlıdır.

Tam bir uygulama kılavuzu GitHub'daki [Azure Depolama ve Bilişsel Hizmetler Laboratuvarı'nda](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md) bulunabilir ve bu öğretici aslında laboratuvarın Egzersiz 5'ini kapsar. Her adımı izleyerek tam uygulamayı oluşturmak isteyebilirsiniz, ancak computer vision'ı mevcut bir web uygulamasına nasıl entegre edacağınızı öğrenmek istiyorsanız, buradan okuyabilirsiniz.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Azure'da Bir Bilgisayar Vizyonu kaynağı oluşturma
> * Azure Depolama görüntülerinde görüntü analizi yapma
> * Azure Depolama resimlerine meta veri ekleme
> * Azure Depolama Gezgini'ni kullanarak görüntü meta verilerini denetleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2017 Topluluk sürümü](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) veya üstü, "ASP.NET ve web geliştirme" ve "Azure geliştirme" iş yükleri yüklü.
- Görüntü depolama için ayarlanmış bir blob kapsayıcısı olan bir Azure Depolama hesabı (bu adımda yardıma ihtiyacınız varsa [Azure Depolama Laboratuvarı'nın Alıştırmalar](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise1) 1'ini izleyin).
- Azure Depolama Gezgini aracı (bu adımda yardıma ihtiyacınız varsa [Azure Depolama Laboratuvarı'nın Alıştırma 2'sini](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise2) izleyin).
- Azure Depolama'ya erişimi olan ASP.NET bir web uygulaması (böyle bir uygulamayı hızlı bir şekilde oluşturmak için [Azure Depolama Laboratuvarı'nın Alıştırma](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3) 3'üne uyun).

## <a name="create-a-computer-vision-resource"></a>Bilgisayarlı Vizyon kaynağı oluşturma

Azure hesabınız için bir Bilgisayar Vizyonu kaynağı oluşturmanız gerekir; bu kaynak, Azure'un Computer Vision hizmetine erişiminizi yönetir. 

1. Bir Bilgisayar Vizyonu kaynağı oluşturmak için [Azure Bilişsel Hizmetler kaynağı oluştur'daki](../../cognitive-services-apis-create-account.md) yönergeleri izleyin.

1. Ardından kaynak grubunuzun menüsüne gidin ve yeni oluşturduğunuz Computer Vision API aboneliğini tıklatın. Url'yi **Endpoint'in** altındaki URL'yi bir anda kolayca alabileceğiniz bir yere kopyalayın. Ardından **Erişim tuşlarını göster'i**tıklatın.

    ![Bitiş noktası URL'si ve erişim anahtarları bağlantısı özetlenen Azure portal sayfası](../Images/copy-vision-endpoint.png)
    
    [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]


1. Sonraki **pencerede, KEY 1** değerini panoya kopyalayın.

    ![Anahtarlar iletişim kutusunu yönet, kopyalama düğmesi ana hatlarıyla](../Images/copy-vision-key.png)

## <a name="add-computer-vision-credentials"></a>Bilgisayar Lı Vizyon kimlik bilgileri ekleme

Ardından, Uygulamanızın Bilgisayar Lı Vizyon kaynaklarına erişebilmeleri için gerekli kimlik bilgilerini eklersiniz

Visual Studio'da ASP.NET web uygulamanızı açın ve projenin kökündeki **Web.config** dosyasına gidin. Önceki adımda kopyaladığınız anahtarı ve `<appSettings>` `VISION_KEY` `VISION_ENDPOINT` daha önce adımda kaydettiğiniz URL'yi değiştirerek dosyanın bölümüne aşağıdaki ifadeleri ekleyin.

```xml
<add key="SubscriptionKey" value="VISION_KEY" />
<add key="VisionEndpoint" value="VISION_ENDPOINT" />
```

Ardından Solution Explorer'da projeyi sağ tıklatın ve **Microsoft.Azure.CognitiveServices.Vision.ComputerVision**paketini yüklemek için **NuGet Paketlerini Yönet** komutunu kullanın. Bu paket, BilgisayarLı Vizyon API'sini aramak için gereken türleri içerir.

## <a name="add-metadata-generation-code"></a>Meta veri oluşturma kodu ekleme

Ardından, görüntüler için meta veri oluşturmak için Computer Vision hizmetinden gerçekten yararlanan kodu eklersiniz. Bu adımlar, laboratuvardaki ASP.NET uygulaması için geçerliolacaktır, ancak bunları kendi uygulamanıza uyarlayabilirsiniz. Önemli olan, bu noktada görüntüleri bir Azure Depolama kapsayıcısına yükleyebilen, ondan görüntüleri okuyabilen ve görünümde görüntüleyebilen ASP.NET bir web uygulamanız olmasıdır. Bu adımdan emin değilseniz, En iyisi [Azure Depolama Laboratuvarı'nın Egzersiz 3'ü izlemenizdir.](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise3) 

1. Projenin **Denetleyicileri** klasöründeki *HomeController.cs* dosyasını açın `using` ve dosyanın üst kısmında aşağıdaki ifadeleri ekleyin:

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
    ```

1. Ardından, **Upload** yöntemine gidin; bu yöntem görüntüleri blob depolamaya dönüştürür ve yükler. Aşağıdaki kodu, (veya görüntü-blob oluşturma işleminizin sonunda) ile başlayan `// Generate a thumbnail` bloktan hemen sonra ekleyin. Bu kod, görüntüyü içeren blob alır (`photo`), ve bu görüntü için bir açıklama oluşturmak için Computer Vision kullanır. Computer Vision API ayrıca görüntüiçin geçerli anahtar kelimelerin bir listesini oluşturur. Oluşturulan açıklama ve anahtar kelimeler, daha sonra alınabilmeleri için blob'un meta verilerinde depolanır.

    ```csharp
    // Submit the image to Azure's Computer Vision API
    ComputerVisionClient vision = new ComputerVisionClient(
        new ApiKeyServiceClientCredentials(ConfigurationManager.AppSettings["SubscriptionKey"]),
        new System.Net.Http.DelegatingHandler[] { });
    vision.Endpoint = ConfigurationManager.AppSettings["VisionEndpoint"];

    VisualFeatureTypes[] features = new VisualFeatureTypes[] { VisualFeatureTypes.Description };
    var result = await vision.AnalyzeImageAsync(photo.Uri.ToString(), features);

    // Record the image description and tags in blob metadata
    photo.Metadata.Add("Caption", result.Description.Captions[0].Text);

    for (int i = 0; i < result.Description.Tags.Count; i++)
    {
        string key = String.Format("Tag{0}", i);
        photo.Metadata.Add(key, result.Description.Tags[i]);
    }

    await photo.SetMetadataAsync();
    ```

1. Ardından, aynı dosyadaki **Dizin** yöntemine gidin. Bu yöntem, hedeflenen blob kapsayıcısındaki **(IListBlobItem** örnekleri gibi) depolanan görüntü lekelerini sıralar ve bunları uygulama görünümüne geçirir. Bu `foreach` yöntemdeki bloğu aşağıdaki kodla değiştirin. Bu kod, her blob'un bağlı meta verilerini almak için **CloudBlockBlob.FetchAttributes** çağırır. Bilgisayar tarafından oluşturulan açıklamayı meta`caption`verilerden ayıklar ve görünüme geçirilen **BlobInfo** nesnesine ekler.
    
    ```csharp
    foreach (IListBlobItem item in container.ListBlobs())
    {
        var blob = item as CloudBlockBlob;
    
        if (blob != null)
        {
            blob.FetchAttributes(); // Get blob metadata
            var caption = blob.Metadata.ContainsKey("Caption") ? blob.Metadata["Caption"] : blob.Name;
    
            blobs.Add(new BlobInfo()
            {
                ImageUri = blob.Uri.ToString(),
                ThumbnailUri = blob.Uri.ToString().Replace("/photos/", "/thumbnails/"),
                Caption = caption
            });
        }
    }
    ```

## <a name="test-the-app"></a>Uygulamayı test edin

Visual Studio'daki değişikliklerinizi kaydedin ve uygulamayı tarayıcınızda başlatmak için **Ctrl+F5** tuşuna basın. Uygulamayı, laboratuvarın kaynaklarındaki "fotoğraflar" klasöründen veya kendi klasörünüzden birkaç resim yüklemek için kullanın. İmleci görünümdeki resimlerden birinin üzerine gontttettiğinizde, bir araç ipucu penceresi görünmeli ve görüntü için bilgisayar tarafından oluşturulan başlığı görüntülemelidir.

![Bilgisayar tarafından oluşturulan resim yazısı](../Images/thumbnail-with-tooltip.png)

Ekli meta verilerin tümünün görüntülenmesini sağlamak için, resimler için kullandığınız depolama kapsayıcısını görüntülemek için Azure Depolama Gezgini'ni kullanın. Kapsayıcıdaki lekelerden herhangi birini sağ tıklatın ve **Özellikler'i**seçin. İletişim kutusunda, anahtar değeri çiftleri listesi görürsünüz. Bilgisayar tarafından oluşturulan resim açıklaması "Resim Yazısı" öğesinde, arama anahtar kelimeleri ise "Tag0", "Tag1" ve benzeri yerlerde depolanır. İşiniz bittiğinde, iletişim kutusunu kapatmak için **İptal'i** tıklatın.

![Resim özellikleri iletişim penceresi, meta veri etiketleri listelenen](../Images/blob-metadata.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Web uygulamanızda çalışmaya devam etmek istiyorsanız, [Sonraki adımlar](#next-steps) bölümüne bakın. Bu uygulamayı kullanmaya devam etmeyi düşünmüyorsanız, uygulamaya özel tüm kaynakları silmeniz gerekir. Kaynakları silyapmak için Azure Depolama aboneliğinizi ve Computer Vision kaynağınızı içeren kaynak grubunu silebilirsiniz. Bu, depolama hesabını, yüklenen lekeleri ve ASP.NET web uygulamasına bağlanmak için gereken Uygulama Hizmeti kaynağını kaldırır. 

Kaynak grubunu silmek için portaldaki **Kaynak grupları** sekmesini açın, bu proje için kullandığınız kaynak grubuna gidin ve görünümün üst **kısmındaki kaynak grubunu sil'i** tıklatın. Silmek istediğinizi doğrulamak için kaynak grubunun adını yazmanız istenir, çünkü silindikten sonra bir kaynak grubu kurtarılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure'un Computer Vision hizmetini, yüklenen ler gibi blob görüntüleri için altyazılar ve anahtar kelimeler oluşturmak üzere varolan bir web uygulamasında ayarlarsınız. Ardından, web uygulamanıza arama işlevselliği eklemeyi öğrenmek için Azure Depolama Laboratuvarı, Egzersiz 6'ya bakın. Bu, Computer Vision hizmetinin oluşturduğu arama anahtar kelimelerinden yararlanır.

> [!div class="nextstepaction"]
> [Uygulamanıza arama ekleme](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise6)
