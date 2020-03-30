---
title: Video Dizinleyici ile animasyonlu karakter algılama
titleSuffix: Azure Media Services
description: Bu konu, Video Indexer ile animasyonlu karakter algılamanın nasıl kullanılacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/19/2019
ms.author: juliako
ms.openlocfilehash: af608dcfbb5d98cf3116de4e14dc12bf6facb97b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989918"
---
# <a name="animated-character-detection-preview"></a>Animasyonlu karakter algılama (önizleme)

Azure Medya Hizmetleri Video Dizinleyici, [Bilişsel Hizmetler özel vizyonuyla](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)tümleştirme yoluyla animasyonlu içerikteki karakterlerin algılanmasını, gruplanmasını ve tanınmasını destekler. Bu işlevsellik hem portal hem de API aracılığıyla kullanılabilir.

Belirli bir animasyon modeliyle animasyonlu bir video yükledikten sonra, Video Indexer anahtar kareleri ayıklar, bu karelerde animasyonlu karakterleri algılar, benzer karakteri gruplandırZ ve en iyi örneği seçer. Daha sonra, gruplandırılmış karakterleri, eğitildiği modellere göre karakterleri tanımlayan Özel Vizyon'a gönderir. 

Modelinizi eğitmeye başlamadan önce, karakterler adsız olarak algılanır. Adları eklediğinizde ve modeli eğitirken Video Dizinleyici karakterleri tanır ve buna göre adlandıracaktır.

## <a name="flow-diagram"></a>Akış diyagramı

Aşağıdaki diyagram animasyonlu karakter algılama işleminin akışını gösterir.

![Akış diyagramı](./media/animated-characters-recognition/flow.png)

## <a name="accounts"></a>Hesaplar

Video Indexer hesabınızın türüne bağlı olarak farklı özellik kümeleri kullanılabilir. Hesabınızı Azure'a nasıl bağlayacaklarınız hakkında bilgi [için](connect-to-azure.md)bkz.

* Deneme hesabı: Video Indexer modeli oluşturmak ve Video Indexer hesabınıza bağlamak için dahili bir Custom Vision hesabı kullanır. 
* Ücretli hesap: Özel Vizyon hesabınızı Video Indexer hesabınıza bağlarsınız (zaten hesabınız yoksa, önce bir hesap oluşturmanız gerekir).

### <a name="trial-vs-paid"></a>Deneme vs ödenen

|İşlev|Deneme|Ödenen|
|---|---|---|
|Özel Vizyon hesabı|Video Indexer tarafından sahne arkasında yönetildi. |Özel Vizyon hesabınız Video Dizinleyici'ye bağlıdır.|
|Animasyon modelleri sayısı|Bir|Hesap başına en fazla 100 model (Özel Vizyon sınırlaması).|
|Modeli eğitme|Video Indexer, yeni karakterler için modeli varolan karakterlerin ek örneklerini eğitir.|Hesap sahibi, değişiklik yapmaya hazır olduklarında modeli eğitir.|
|Custom Vision'da gelişmiş seçenekler|Özel Vizyon portalına erişim yok.|Modelleri Custom Vision portalında kendiniz ayarlayabilirsiniz.|

## <a name="use-the-animated-character-detection-with-portal"></a>Portal ile animasyonlu karakter algılamasını kullanma 

Bu bölümde, animasyonlu karakter algılama modelini kullanmaya başlamak için atmanız gereken adımlar açıklanmaktadır. 

Deneme hesaplarında Özel Vizyon tümleştirmesi Video Indexer tarafından yönetildiği için, animasyonlu karakterler modelini oluşturmaya ve kullanmaya başlayabilir ve aşağıdaki bölümü atlayabilirsiniz ("Özel Vizyon hesabınızı bağlayın").

### <a name="connect-your-custom-vision-account-paid-accounts-only"></a>Özel Vizyon hesabınızı bağlayın (yalnızca ücretli hesaplar)

Video Indexer ücretli hesabınız varsa, önce bir Özel Vizyon hesabı bağlamanız gerekir. Zaten bir Özel Vizyon hesabınız yoksa, lütfen bir tane oluşturun. Daha fazla bilgi için [Bkz. Özel Vizyon.](../../cognitive-services/custom-vision-service/home.md)

> [!NOTE]
> Her iki hesabın da aynı bölgede olması gerekir. Custom Vision tümleştirmesi şu anda Japonya bölgesinde desteklenmez.

#### <a name="connect-a-custom-vision-account-with-api"></a>ÖZEL Görme hesabını API ile bağlama 

Özel Vizyon hesabınızı Video Indexer'a bağlamak veya şu anda Video Dizinleyici'ye bağlı olan Özel Vizyon hesabını değiştirmek için aşağıdaki adımları izleyin:

1. [www.customvision.ai](https://www.customvision.ai) ve giriş için göz atın.
1. Aşağıdaki tuşları kopyalayın: 

    * Eğitim anahtarı (eğitim kaynağı için)
    * Tahmin anahtarı (tahmin kaynağı için)
    * Uç Nokta 
    * Tahmin kaynağı kimliği
    
    > [!NOTE]
    > Tüm anahtarları sağlamak için Özel Vizyon'da biri eğitim, diğeri tahmin için olmak üzere iki ayrı kaynağa sahip olmanız gerekir.
1. [Video Dizinleyici'ye](https://vi.microsoft.com/)göz atın ve oturum açın.
1. Sayfanın sağ üst köşesindeki soru işaretine tıklayın ve **API Başvurusu'nu**seçin.
1. **Ürünler** sekmesini tıklatarak API Yönetimi'ne abone olduğunuzdan emin olun. Bağlı bir API'niz varsa, bir sonraki adıma devam edebilirsiniz, aksi takdirde abone olabilirsiniz. 
1. Geliştirici portalında, **Tam API Başvurusu'nu** tıklatın ve **İşlemler'e**göz atın.  
1. **Özel Vizyon Hesabı Bağla'yı (PREVIEW)** seçin ve **Ted'i**tıklatın.
1. Gerekli alanları ve erişim belirteci doldurun ve **Gönder'i**tıklatın. 

    Video Indexer erişim jetonunun nasıl alınılabildiğini öğrenmek için [geliştirici portalına](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token?)gidin ve [ilgili belgeleri](video-indexer-use-apis.md#obtain-access-token-using-the-authorization-api)görün.  
1. Arama 200 Tamam yanıtını döndüründen hesabınız bağlanır.
1. [Video Dizinleyici](https://vi.microsoft.com/)) portalına göz atarak bağlantınızı doğrulamak için:
1. Sağ üst köşedeki **İçerik modeli özelleştirme** düğmesine tıklayın.
1. **Animasyonkarakterleri** sekmesine gidin.
1. Custom Vision'da Modelleri Yönet'e tıkladığınızda"**, yeni bağladığınız Özel Vizyon hesabına aktarılırsınız.

> [!NOTE]
> Şu anda yalnızca Video Indexer aracılığıyla oluşturulan modeller desteklenir. Özel Vizyon aracılığıyla oluşturulan modeller kullanılamaz. Buna ek olarak, en iyi uygulama, Özel Vizyon aracılığıyla yapılan değişiklikler istenmeyen sonuçlara neden olabileceğinden, Video Dizinleyici aracılığıyla oluşturulan modelleri yalnızca Video Dizinleyici platformu üzerinden yeniden düzenlendirmektir.

### <a name="create-an-animated-characters-model"></a>Animasyonlu karakterler modeli oluşturma

1. [Video Indexer](https://vi.microsoft.com/) web sitesine gidip oturum açın.
1. Sayfanın sağ üst köşesindeki içerik modeli özelleştirme düğmesine tıklayın.

    ![İçerik modeli özelleştirme](./media/animated-characters-recognition/content-model-customization.png)
1. Model özelleştirme bölümündeki **Animasyonkarakterleri** sekmesine gidin.
1. Model **Ekle'ye**tıklayın.
1. Model adınızı girin ve adı kaydetmek için enter'u tıklatın.

> [!NOTE]
> En iyi uygulama her animasyon serisi için bir özel vizyon modeli olmasıdır. 

### <a name="index-a-video-with-an-animated-model"></a>Animasyonlu bir modelle videoyu dizine dizine

1. Üst menüden **Yükle** düğmesine tıklayın.
1. Yüklemek için bir video seçin (bir dosyadan veya URL'den).
1. Gelişmiş **seçeneklere**tıklayın.
1. **Altında Kişi / Animasyon karakterleri** Animasyon **modelleri**seçin.
1. Bir modeliniz varsa otomatik olarak seçilir ve birden çok modeliniz varsa açılır menüden ilgili modeli seçebilirsiniz.
1. Yükleme üzerine tıklayın.
1. Video dizine alındıktan sonra, Algılanan karakterleri **Insights** bölmesinde **Animasyon karakterleri** bölümünde görürsünüz.

> [!NOTE] 
> Modeli etiketlemeden ve iliştirmeden önce, tüm animasyon karakterleri "Bilinmeyen #X" olarak adlandırılır. Modeli eğittmeniz inden sonra onlar da tanınacaktır.

### <a name="customize-the-animated-characters-models"></a>Animasyonkarakterleri modellerini özelleştirme

1. Modeli etiketle ve eğit.

    1. Algılanan karakteri adını düzenleyerek etiketle. Bir karakter modele eğitildikten sonra, bu modelle birlikte dizine eklenmiş bir sonraki video olarak kabul edilecektir. 
    1. Videonuzdaki animasyonlu karakteri etiketlemek için **Öngörüler** sekmesine gidin ve pencerenin sağ üst köşesindeki **Edit** düğmesine tıklayın.
    1. **Öngörüler** bölmesinde, algılanan animasyonkarakterlerin herhangi birini tıklatın ve adlarını "Bilinmeyen #X" (veya daha önce karaktere atanmış ad) olarak değiştirin.
    1. Yeni adı yazdıktan sonra, yeni adın yanındaki denetim simgesine tıklayın. Bu, Video Dizinleyici'deki modeldeki yeni adı kaydeder.
    1. İstediğiniz tüm adları düzenlemeyi bitirdikten sonra, modeli eğitmenniz gerekir.

        Özelleştirme sayfasını açın ve **Animasyonlu karakterler** sekmesine tıklayın ve ardından modelinizi eğitmek için **Tren** düğmesine tıklayın.
         
        Ücretli bir hesabınız varsa, Müşteri **Görüşü bağlantısındaki Modelleri Yönet'i** tıklatabilirsiniz (aşağıda gösterildiği gibi). Daha sonra **Özel Vizyon**modelinin sayfasına iletilir.
 
        ![İçerik modeli özelleştirme](./media/animated-characters-recognition/content-model-customization-tab.png)

     1. Eğitildikten sonra, bu modelle dizinlenecek veya yeniden dizine dizinlenecek herhangi bir video, eğitilen karakterleri tanıyacaktır. 
    Özel Vizyon hesaplarına erişimi olan ücretli hesaplar, modelleri ve etiketli resimleri görebilir. [Özel Vizyon'da sınıflandırıcınızı geliştirme](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier)hakkında daha fazla bilgi edinin.

1. Animasyonlu karakteri silin.

    1. Video istatistiklerinizdeki animasyonlu karakteri silmek **için, İstatistikler** sekmesine gidin ve pencerenin sağ üst köşesindeki **Düzenle** düğmesine tıklayın.
    1. Animasyonlu karakteri seçin ve ardından adlarının altındaki **Sil** düğmesine tıklayın.

    > [!NOTE]
    > Bu, bu videodaki içgörüyü siler, ancak modeli etkilemez.

1. Bir modeli silin.

    1. Üst menüdeki **İçerik modeli özelleştirme** düğmesine tıklayın ve **Animasyonkarakterleri** sekmesine gidin.
    1. Silmek istediğiniz modelin sağındaki elips simgesine ve ardından sil düğmesine tıklayın.
    
    * Ücretli hesap: modelvideo indexer bağlantısı kesilir ve yeniden bağlamak mümkün olmayacaktır.
    * Deneme hesabı: model gümrük vizyonundan da silinecektir. 
    
        > [!NOTE]
        > Deneme hesabında kullanabileceğiniz yalnızca bir model var. Sildikten sonra, diğer modelleri eğitemezsiniz.

## <a name="use-the-animated-character-detection-with-api"></a>API ile animasyonlu karakter algılamasını kullanma 

1. Özel Vizyon hesabı bağlayın.

    Video Indexer ücretli hesabınız varsa, önce bir Özel Vizyon hesabı bağlamanız gerekir. <br/>
    Zaten bir Özel Vizyon hesabınız yoksa, lütfen bir tane oluşturun. Daha fazla bilgi için [Bkz. Özel Vizyon.](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/home)

    [ÖZEL Görme hesabınızı API'yi kullanarak bağlayın.](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Connect-Custom-Vision-Account?tags=&pattern=&groupBy=tag)
1. Animasyonlu karakterler modeli oluşturun.

    Create [animasyon modeli](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Animation-Model?&groupBy=tag) API'yi kullanın.
1. Bir videoyu dizine dizine dizin veya yeniden dizine dizin.

    Yeniden [dizine ekleyen](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) API'yi kullanın. 
1. Animasyonkarakterleri modellerini özelleştirin.

    Tren [animasyon modeli](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Train-Animation-Model?&groupBy=tag) API'yi kullanın.

### <a name="view-the-output"></a>Çıktıyı görüntüleme

Oluşturulan JSON dosyasındaki animasyonkarakterleri görün.

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

* Şu anda, "animasyon tanımlama" özelliği Doğu Asya bölgesinde desteklenmemektedir.
* Videonun kalitesi düşükse, videoda küçük veya uzak görünen karakterler düzgün tanımlanamayabilir.
* Öneri, animasyonlu karakter kümesi başına bir model kullanmaktır (örneğin animasyonlu seri başına).

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)
