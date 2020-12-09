---
title: Video Indexer ile animasyonlu karakter algılama
titleSuffix: Azure Media Services
description: Bu, Video Indexer ile animasyonlu karakter algılamayı nasıl kullanacağınızı gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.custom: references_regions
ms.topic: how-to
ms.date: 12/07/2020
ms.author: juliako
ms.openlocfilehash: 9effac182acdea6fcb41ed26faf6c2f6535a5cbf
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96906174"
---
# <a name="use-the-animated-character-detection-preview-with-portal-and-api"></a>Portal ve API ile animasyonlu karakter algılamayı (Önizleme) kullanma 

Azure Media Services Video Indexer, animasyonlu içeriklerde karakterlerin algılanmasını, gruplandırılmasını ve tanınmasını destekler, bu işlevsellik Azure portal ve API aracılığıyla kullanılabilir. [Bu genel bakış](animated-characters-recognition.md) konusunu gözden geçirin.

Bu makalede, Azure portal ve Video Indexer API 'SI ile animasyonlu karakter algılamayı nasıl kullanabileceğiniz gösterilmektedir.

## <a name="use-the-animated-character-detection-with-portal"></a>Portal ile animasyonlu karakter algılamayı kullanma 

Bu bölümde, animasyonlu karakter algılama modelini kullanmaya başlamak için gerçekleştirmeniz gereken adımlar açıklanmaktadır. 

Özel Görüntü İşleme tümleştirme, deneme hesaplarında Video Indexer tarafından yönetildiğinden, animasyonlu karakterler modelini oluşturmaya ve kullanmaya başlayabilir ve aşağıdaki bölümü atlayabilirsiniz ("Özel Görüntü İşleme hesabınızı bağlama").

### <a name="connect-your-custom-vision-account-paid-accounts-only"></a>Özel Görüntü İşleme hesabınızı bağlama (yalnızca ücretli hesaplar)

Video Indexer ücretli bir hesabınız varsa, önce bir Özel Görüntü İşleme hesabı bağlamanız gerekir. Zaten bir Özel Görüntü İşleme hesabınız yoksa lütfen bir tane oluşturun. Daha fazla bilgi için bkz. [özel görüntü işleme](../../cognitive-services/custom-vision-service/overview.md).

> [!NOTE]
> Her iki hesabın de aynı bölgede olması gerekir. Özel Görüntü İşleme tümleştirme Şu anda Japonya bölgesinde desteklenmiyor.

#### <a name="connect-a-custom-vision-account-with-api"></a>Özel Görüntü İşleme bir hesabı API ile bağlama 

Özel Görüntü İşleme hesabınızı Video Indexer bağlamak veya şu anda Video Indexer 'e bağlı olan Özel Görüntü İşleme hesabı değiştirmek için aşağıdaki adımları izleyin:

1. [Www.customvision.ai](https://www.customvision.ai) ve oturum açma bilgilerine gidin.
1. Aşağıdaki anahtarları kopyalayın: 

    * Eğitim anahtarı (eğitim kaynağı için)
    * Tahmin anahtarı (tahmin kaynağı için)
    * Uç Nokta 
    * Tahmin kaynak KIMLIĞI
    
    > [!NOTE]
    > Tek bir eğitim ve diğeri tahmin için Özel Görüntü İşleme iki ayrı kaynağa sahip olmanız için ihtiyacınız olan tüm anahtarları sağlamak için.
1. [Video Indexer](https://vi.microsoft.com/)gidin ve oturum açın.
1. Sayfanın sağ üst köşesindeki soru işaretine tıklayın ve **API başvurusu**' nu seçin.
1. **Ürünler** sekmesi ' ne tıklayarak API Management abone olduğunuzdan emin olun. Bir API bağlantısı varsa, bir sonraki adıma devam edebilir, aksi takdirde abone olabilirsiniz. 
1. Geliştirici portalında, **tüm API başvurusuna** tıklayın ve **işlemlere** gidin.  
1. **Özel görüntü işleme hesabı Bağlan ' ı (Önizleme)** seçin ve **dene**' ye tıklayın.
1. Gerekli alanların yanı sıra erişim belirtecini de girin ve **Gönder**' e tıklayın. 

    Video Indexer erişim belirtecini alma hakkında daha fazla bilgi için [Geliştirici portalına](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token?)gidin ve [ilgili belgelere](video-indexer-use-apis.md#obtain-access-token-using-the-authorization-api)bakın.  
1. Çağrı 200 Tamam yanıtı döndürtikten sonra hesabınız bağlanır.
1. Bağlantınızı doğrulamak için [video Indexer](https://vi.microsoft.com/)) portalına gidin:
1. Sağ üst köşedeki **içerik modeli özelleştirmesi** düğmesine tıklayın.
1. **Animasyonlu karakterler** sekmesine gidin.
1. "* * Özel Görüntü İşleme modelleri Yönet ' e tıkladığınızda, yeni bağladığınız Özel Görüntü İşleme hesabına aktarılacaktır.

> [!NOTE]
> Şu anda yalnızca Video Indexer aracılığıyla oluşturulan modeller desteklenir. Özel Görüntü İşleme üzerinden oluşturulan modeller kullanılamayacak. Ayrıca, Özel Görüntü İşleme aracılığıyla yapılan değişiklikler istenmeden sonuçlara neden olabileceğinden, Video Indexer aracılığıyla oluşturulan modelleri yalnızca Video Indexer platformu aracılığıyla düzenlemek en iyi uygulamadır.

### <a name="create-an-animated-characters-model"></a>Animasyonlu karakterler modeli oluşturma

1. [Video Indexer](https://vi.microsoft.com/) web sitesine gidip oturum açın.
1. Sayfanın sağ üst köşesindeki içerik modeli özelleştirmesi düğmesine tıklayın.

    ![Sağ üst köşede "içerik modeli özelleştirmesi" düğmesinin seçili olduğu "Video Indexer" sayfasını gösteren ekran görüntüsü.](./media/animated-characters-recognition/content-model-customization.png)
1. Model özelleştirme bölümündeki **animasyonlu karakterler** sekmesine gidin.
1. **Model Ekle**' ye tıklayın.
1. Modelleyin ve adı kaydetmek için ENTER 'a tıklayın.

> [!NOTE]
> En iyi uygulama, her bir animasyon serisi için bir özel görüntü işleme modelidir. 

### <a name="index-a-video-with-an-animated-model"></a>Animasyonlu modelle video dizini oluşturma

1. Üstteki menüden **karşıya yükle** düğmesine tıklayın.
1. Karşıya yüklenecek bir video seçin (bir dosyadan veya URL 'den).
1. **Gelişmiş Seçenekler**' e tıklayın.
1. **Kişiler/animasyonlu karakterler** altında **animasyon modeller**' ı seçin.
1. Tek bir modeliniz varsa, otomatik olarak seçilir ve birden çok modelleriniz varsa, açılan menüden ilgili olanı seçebilirsiniz.
1. Karşıya yükle ' ye tıklayın.
1. Videonun dizini oluşturulduktan sonra, **Öngörüler** bölmesindeki **animasyonlu karakterler** bölümünde Algılanan karakterleri görürsünüz.

> [!NOTE] 
> Modeli etiketleyerek ve eğitimi yapmadan önce tüm animasyonlu karakterler "bilinmeyen #X" olarak adlandırılır. Modeli eğdikten sonra da tanınacaktır.

### <a name="customize-the-animated-characters-models"></a>Animasyonlu karakterler modellerini özelleştirme

1. Modeli etiketleyin ve eğitme.

    1. Algılanan karakteri, adını düzenleyerek etiketleyin. Bir karakter modele eğitilirken, bu modelle birlikte dizine alınmış bir sonraki video tanınacaktır. 
    1. Videonuzdaki bir animasyonlu karakteri etiketlemek için **Öngörüler** sekmesine gidin ve pencerenin sağ üst köşesindeki **Düzenle** düğmesine tıklayın.
    1. **Öngörüler** bölmesinde, algılanan animasyonlu karakterlerden herhangi birine tıklayın ve adlarını "bilinmeyen #X" (veya daha önce karaktere atanmış olan ad) olarak değiştirin.
    1. Yeni adı yazdıktan sonra, yeni adın yanındaki onay simgesine tıklayın. Bu, yeni adı modelde Video Indexer kaydeder.
    1. İstediğiniz tüm adları düzenledikten sonra modeli eğmeniz gerekir.

        Özelleştirme sayfasını açın ve **animasyon karakterleri** sekmesine tıklayın ve ardından modelinizi eğitebilmeniz Için **eğitme** düğmesine tıklayın.
         
        Ücretli bir hesabınız varsa, **Müşteri vizyonlarındaki modelleri Yönet** bağlantısını (aşağıda gösterildiği gibi) tıklayabilirsiniz. Daha sonra **özel görüntü işleme** modelin sayfasına iletilecektir.
 
        ![İçerik modeli özelleştirmesi](./media/animated-characters-recognition/content-model-customization-tab.png)

     1. Eğitim olduktan sonra, bu modelle Dizinlenecek veya yeniden Dizinlenecek tüm videolar eğitilen karakterleri tanır. 
    Özel Görüntü İşleme hesabına erişimi olan ücretli hesaplar, modelleri ve etiketli görüntüleri burada görebilir. [Özel görüntü işleme sınıflandırıcınızı iyileştirme](../../cognitive-services/custom-vision-service/getting-started-improving-your-classifier.md)hakkında daha fazla bilgi edinin.

1. Animasyonlu bir karakteri silin.

    1. Video öngörülerinizi animasyonlu bir karakter silmek için **Öngörüler** sekmesine gidin ve pencerenin sağ üst köşesindeki **Düzenle** düğmesine tıklayın.
    1. Animasyonlu karakteri seçin ve ardından ad altındaki **Sil** düğmesine tıklayın.

    > [!NOTE]
    > Bu, bu videodan öngörüyü siler, ancak modeli etkilemez.

1. Bir modeli silin.

    1. Üstteki menüdeki **içerik modeli özelleştirmesi** düğmesine tıklayın ve **animasyonlu karakterler** sekmesine gidin.
    1. Silmek istediğiniz modelin sağ tarafındaki üç nokta simgesine ve ardından Sil düğmesine tıklayın.
    
    * Ücretli hesap: modelin bağlantısı Video Indexer kesilecek ve yeniden bağlanamayacaksınız.
    * Deneme hesabı: model gümrük Vision 'dan da silinecek. 
    
        > [!NOTE]
        > Deneme hesabında yalnızca bir modelleyebilirsiniz. Sildikten sonra, diğer modelleri eğitebilirsiniz.

## <a name="use-the-animated-character-detection-with-api"></a>API ile animasyonlu karakter algılamayı kullanma 

1. Özel Görüntü İşleme bir hesabı bağlayın.

    Video Indexer ücretli bir hesabınız varsa, önce bir Özel Görüntü İşleme hesabı bağlamanız gerekir. <br/>
    Zaten bir Özel Görüntü İşleme hesabınız yoksa lütfen bir tane oluşturun. Daha fazla bilgi için bkz. [özel görüntü işleme](../../cognitive-services/custom-vision-service/overview.md).

    [API kullanarak özel görüntü işleme hesabınızı bağlayın](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Connect-Custom-Vision-Account?tags=&pattern=&groupBy=tag).
1. Bir animasyonlu karakter modeli oluşturun.

    [Animasyon modeli oluşturma](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Animation-Model?&groupBy=tag) API 'sini kullanın.
1. Videoyu dizine koyun veya yeniden dizine koyun.

    [Yeniden dizin oluşturma](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) API 'sini kullanın. 
1. Animasyonlu karakterler modellerini özelleştirin.

    [Eğitme animasyon modeli](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Train-Animation-Model?&groupBy=tag) API 'sini kullanın.

### <a name="view-the-output"></a>Çıktıyı görüntüleme

Oluşturulan JSON dosyasındaki animasyonlu karakterleri görüntüleyin.

```json
"animatedCharacters": [
    {
    "videoId": "e867214582",
    "confidence": 0,
    "thumbnailId": "00000000-0000-0000-0000-000000000000",
    "seenDuration": 201.5,
    "seenDurationRatio": 0.3175,
    "isKnownCharacter": true,
    "id": 4,
    "name": "Bunny",
    "appearances": [
        {
            "startTime": "0:00:52.333",
            "endTime": "0:02:02.6",
            "startSeconds": 52.3,
            "endSeconds": 122.6
        },
        {
            "startTime": "0:02:40.633",
            "endTime": "0:03:16.6",
            "startSeconds": 160.6,
            "endSeconds": 196.6
        },
    ]
    },
]
```

## <a name="limitations"></a>Sınırlamalar

* Şu anda, "animasyon kimliği" özelliği East-Asia bölgede desteklenmez.
* Videonun kalitesi zayıflarsa videoda küçük veya uzak görünen karakterler düzgün şekilde tanımlanmayabilir.
* Öneri, her bir animasyon karakter kümesi için (örneğin, bir animasyon serisi) bir model kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)
