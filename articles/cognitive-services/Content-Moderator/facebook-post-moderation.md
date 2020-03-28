---
title: 'Öğretici: Orta Facebook içeriği - İçerik Moderatör'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, Facebook gönderilerini ve yorumlarını ılımlı olarak kullanmak için makine öğrenimi tabanlı İçerik Moderatörü'ü nasıl kullanacağınızı öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 9f25c845302d62e3bc9e230b4a6f8f2669f4ac35
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774280"
---
# <a name="tutorial-moderate-facebook-posts-and-commands-with-azure-content-moderator"></a>Öğretici: Azure İçerik Moderatörü ile Orta Facebook gönderileri ve komutları

Bu eğitimde, Bir Facebook sayfasındaki gönderilerin ve yorumların ortasına yardımcı olmak için Azure İçerik Moderatörü'nün nasıl kullanılacağını öğreneceksiniz. Facebook, ziyaretçiler tarafından gönderilen içeriği İçerik Moderatör hizmetine gönderir. Ardından İçerik Moderatör'ü iş akışlarınız, içerik puanlarına ve eşiklere bağlı olarak içeriği yayımlar veya İnceleme aracı içinde incelemeler oluşturur. Bu senaryonun çalışan bir örneği için [Build 2017 demo videosuna](https://channel9.msdn.com/Events/Build/2017/T6033) bakın.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Content Moderator takımı oluşturma.
> * Content Moderator'dan ve Facebook'tan HTTP olaylarını dinleyen Azure İşlevleri oluşturma.
> * Bir Facebook uygulamasını kullanarak Bir Facebook sayfasını İçerik Moderatör'üne bağla.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

Bu diyagram, bu senaryonun her bileşenini gösterir:

![İçerik Moderatör Diyagramı "FBListener" üzerinden Facebook'tan bilgi alma ve "CMListener" üzerinden bilgi gönderme](images/tutorial-facebook-moderation.png)

> [!IMPORTANT]
> 2018 yılında Facebook, Facebook Apps için daha sıkı bir inceleme politikası uyguladı. Uygulamanız Facebook inceleme ekibi tarafından incelenip onaylanmamışsa, bu öğreticinin adımlarını tamamlayamayacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Content Moderator abonelik anahtarı. İçerik Moderatör hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin.
- Bir [Facebook hesabı](https://www.facebook.com/).

## <a name="create-a-review-team"></a>İnceleme ekibi oluşturma

[İçerik Moderatör İnceleme aracına](https://contentmoderator.cognitive.microsoft.com/) nasıl kaydolunacağına ve bir inceleme ekibi oluşturacağınıza ilişkin talimatlar için [web'deki İçeriği Deneyin](quick-start.md) Moderatörü'ne bakın. **Kimlik Bilgileri** sayfasındaki **Takım Kimliği** değerine dikkat edin.

## <a name="configure-image-moderation-workflow"></a>Görüntü Moderasyon iş akışını yapılandırma

Özel bir görüntü iş akışı oluşturmak için [İş Akışlarını Tanımla, sına ve kullan](review-tool-user-guide/workflows.md) kılavuzuna bakın. İçerik Moderatörotomatik olarak Facebook'taki görüntüleri kontrol etmek ve gözden geçirme aracına bazı göndermek için bu iş akışını kullanır. İş akışı **adını**not alın.

## <a name="configure-text-moderation-workflow"></a>Metin moderasyon iş akışını yapılandırma

Yine, iş [akışlarını tanımla, sına ve kullan](review-tool-user-guide/workflows.md) kılavuzuna bakın; bu kez, özel bir metin iş akışı oluşturun. İçerik Moderatörotomatik metin içeriğini kontrol etmek için bu iş akışını kullanır. İş akışı **adını**not alın.

![Metin İş Akışını Yapılandırma](images/text-workflow-configure.PNG)

**İş Akışını Yürüt** düğmesini kullanarak iş akışınızı test edin.

![Metin İş Akışını Test Etme](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure İşlevleri oluşturma

[Azure portalında](https://portal.azure.com/) oturum açın ve aşağıdaki adımları izleyin:

1. [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasında gösterildiği gibi bir Azure İşlev Uygulaması oluşturun.
1. Yeni oluşturulan İşlev Uygulaması'na gidin.
1. Uygulama içinde, Platform **özellikleri** sekmesine gidin ve **Yapılandırma'yı**seçin. Bir sonraki sayfanın **Uygulama ayarları** bölümünde, aşağıdaki anahtar/değer çiftleri eklemek için Yeni **uygulama ayarını** seçin:
    
    | Uygulama Ayarı adı | value   | 
    | -------------------- |-------------|
    | cm:TeamId   | Content Moderator Takım Kimliğiniz  | 
    | cm:SubscriptionKey | Content Moderator abonelik anahtarınız. Bkz. [Kimlik Bilgileri](review-tool-user-guide/credentials.md) |
    | cm:Region | Content Moderator bölge adınız (boşluk içermez). Bunu Azure kaynağınızın **Genel Bakış** sekmesinin **Konum** alanında bulabilirsiniz.|
    | cm:ImageWorkflow | Görüntüler üzerinde çalıştırılacak iş akışının adı |
    | cm:TextWorkflow | Metinler üzerinde çalıştırılacak iş akışının adı |
    | cm:CallbackEndpoint | Daha sonra bu kılavuzda oluşturacağınız CMListener İşlev Uygulaması için url |
    | fb:VerificationToken | Facebook özet akışı etkinliklerine abone olmak için kullandığınız gizli bir belirteç |
    | fb:PageAccessToken | Facebook graph api'si erişim belirtecinin süresi dolmaz ve işlevin sizin adınıza gönderileri Gizlemesini/Silmesini sağlar. Bu belirteci daha sonraki bir adımda alırsınız. |

    Sayfanın üst kısmındaki **Kaydet** düğmesini tıklatın.

1. **Platform özellikleri** sekmesine geri dön. Yeni **+** **işlev** bölmesini açmak için sol bölmedeki düğmeyi kullanın. Oluşturmak üzere olduğunuz işlev Facebook'tan etkinlik alır.

    ![İşlev Ekle düğmesi vurgulanmış olan Azure İşlevler bölmesi.](images/new-function.png)

    1. **Http tetikleme**diyor kiremit tıklayın.
    1. **FBListener** adını girin. **Yetkilendirme Düzeyi** alanı **İşlev** olarak ayarlanmalıdır.
    1. **Oluştur'u**tıklatın.
    1. **run.csx** içeriğini **FbListener/run.csx** içeriğiile değiştirin

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/FbListener/run.csx?range=1-154)]

1. **CMListener**adlı yeni bir **Http tetikleyici** işlevi oluşturun. Bu işlev Content Moderator'dan olayları alır. **run.csx** içeriğini **CMListener/run.csx** içeriğiyle değiştirin

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/CmListener/run.csx?range=1-110)]

---

## <a name="configure-the-facebook-page-and-app"></a>Facebook sayfasını ve Uygulamasını yapılandırma

1. Facebook Uygulaması oluşturun.

    ![facebook geliştirici sayfası](images/facebook-developer-app.png)

    1. [Facebook geliştirici sitesine](https://developers.facebook.com/) gidin
    1. **My Apps** (Uygulamalarım) öğesine tıklayın.
    1. Yeni Uygulama ekleyin.
    1. bir şey adı
    1. **Webhooks 'u** seçin -> Kurulumu
    1. Açılan menüde **Sayfa'yı** seçin ve **bu nesneye Abone Ol'u** seçin
    1. Geri Arama URL'si olarak **FBListener Url'sini** sağlayın ve **İşlevi Uygulaması Ayarları** altında yapılandırdığınız **Belirteci Doğrulayın**
    1. Abone olduktan sonra, akışı aşağı kaydırıp **subscribe** (abone ol) öğesini seçin.
    1. FBListener Azure İşlevinize bir test iletisi göndermek için **özet akışı** satırının **Test** düğmesine tıklayın ve ardından **Sunucuma Gönder düğmesine** basın. FBListener'ınızda alınan isteği görmelisiniz.

1. Facebook Sayfası oluşturun.

    > [!IMPORTANT]
    > 2018 yılında Facebook, Facebook uygulamalarını daha sıkı bir şekilde incelemeyi uygulamaya koymuştur. Uygulamanız Facebook inceleme ekibi tarafından incelenip onaylanmamışsa bölüm 2, 3 ve 4'ün uygulamalarını uygulayamazsınız.

    1. [Facebook](https://www.facebook.com/bookmarks/pages)'a gidin ve **yeni bir Facebook Sayfası** oluşturun.
    1. Şu adımları izleyerek Facebook Uygulamasının bu sayfaya erişmesine izin verin:
        1. [Graph API Explorer](https://developers.facebook.com/tools/explorer/)'a gidin.
        1. **Uygulama**'yı seçin.
        1. **Sayfa Erişim Belirteci**'ni seçin. Bir **Get** isteği gönderin.
        1. Yanıtta **Sayfa Kimliği**'ne tıklayın.
        1. Şimdi **/subscribed_apps** bölümünü URL'ye ekleyin ve bir **Get** (boş yanıt) isteği gönderin.
        1. **Post** isteği gönderin. **success: true** gibi bir yanıt alırsınız.

3. Süresi dolmayan bir Graph API erişim belirteci oluşturun.

    1. [Graph API Explorer](https://developers.facebook.com/tools/explorer/)'a gidin.
    2. **Uygulama** seçeneğini belirtin.
    3. **Kullanıcı Erişim Belirteci Al** seçeneğini belirtin.
    4. **İzin Seç** bölümünde **manage_pages** ve **publish_pages** seçeneklerini belirtin.
    5. **Erişim belirtecini** (Kısa Ömürlü Belirteç) sonraki adımda kullanacağız.

4. Önümüzdeki birkaç adımda Postman kullanıyoruz.

    1. **Postman**'ı açın (veya [buradan](https://www.getpostman.com/) alın).
    2. Şu iki dosyayı içeri aktarın:
        1. [Postman Collection](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman Environment](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Şu ortam değişkenlerini güncelleştirin:
    
        | Anahtar | Değer   | 
        | -------------------- |-------------|
        | appId   | Buraya Facebook Uygulama Tanımlayıcınızı ekleyin  | 
        | appSecret | Buraya Facebook Uygulamanızın gizli dizisini ekleyin | 
        | short_lived_token | Önceki adımda oluşturduğunuz kısa ömürlü kullanıcı erişim belirtecini ekleyin |
    4. Şimdi koleksiyonda listelenen 3 API'yi çalıştırın: 
        1. **Uzun Ömürlü Erişim Belirteci Oluştur**'u seçin ve **Gönder**'e tıklayın.
        2. **Kullanıcı Kimliğini Al**'ı seçin ve **Gönder**'e tıklayın.
        3. **Kalıcı Sayfa Erişim Belirtecini Al**'ı seçin ve **Gönder**'e tıklayın.
    5. Yanıttan **access_token** değerini kopyalayın ve bunu **fb:PageAccessToken** Uygulama ayarına atayın.

Çözüm, Facebook sayfanıza gönderilen tüm resimleri ve metinleri Content Moderator'a gönderir. Daha önce yapılandırdığınız iş akışları çağrılır. İş akışlarında tanımlanan ölçütlerinizi aşmayan içerik, inceleme aracı içindeki incelemelere aktarılır. İçeriğin geri kalanı otomatik olarak yayınlanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ürün görüntülerini ürün türüne göre etiketlemek ve bir inceleme ekibinin içerik denetleme hakkında bilinçli kararlar almasına izin vermek amacıyla çözümlemek üzere bir program ayarlarsınız. Ardından, görüntü moderasyonunun ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Görüntü denetimi](./image-moderation-api.md)
