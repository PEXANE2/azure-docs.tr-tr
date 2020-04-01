---
title: 'Hızlı başlangıç: Formları etiketle, bir modeli eğitin ve örnek etiketleme aracını kullanarak bir formu analiz edin - Form Tanıyın'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlatmada, form belgelerini el ile etiketlemek için Form Tanıyıcısı örnek etiketleme aracını kullanırsınız. Ardından, etiketli belgelerle birlikte özel bir model eğitecek ve anahtar/değer çiftlerini ayıklamak için modeli kullanırsınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 02/19/2020
ms.author: pafarley
ms.openlocfilehash: 0cfe58ab0d161019d5f53d9135c65db7beff2bb4
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397986"
---
# <a name="train-a-form-recognizer-model-with-labels-using-the-sample-labeling-tool"></a>Örnek etiketleme aracını kullanarak etiketlerle form tanıyıcı modelini eğitin

Bu hızlı başlatmada, el ile etiketlenmiş verilere sahip özel bir modeli eğitmek için örnek etiketleme aracıyla Birlikte Form Recognizer REST API'yi kullanırsınız. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketleri ile Tren](../overview.md#train-with-labels) bölümüne bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yapmış olmalısınız:

- Aynı türden en az altı formdan oluşan bir küme. Bu verileri modeli eğitmek ve bir formu test etmek için kullanırsınız. Bu hızlı başlatma için örnek bir [veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure Depolama hesabındaki bir blob depolama kapsayıcısının köküne yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-the-sample-labeling-tool"></a>Örnek etiketleme aracını ayarlama

Örnek etiketleme aracını çalıştırmak için Docker motorını kullanırsınız. Docker kapsayıcısını ayarlamak için aşağıdaki adımları izleyin. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.

> [!TIP]
> OCR Form Etiketleme Aracı, GitHub'da açık kaynak projesi olarak da kullanılabilir. Araç React + Redux kullanılarak oluşturulmuş bir web uygulamasıdır ve TypeScript ile yazılmıştır. Daha fazla bilgi edinmek veya katkıda bulunmak için [OCR Form Etiketleme Aracı'na](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md#run-as-web-application)bakın.

1. İlk olarak, Docker'ı ana bilgisayara yükleyin. Bu kılavuz, yerel bilgisayarı ana bilgisayar olarak nasıl kullanacağınızı gösterir. Azure'da bir Docker barındırma hizmeti kullanmak istiyorsanız, [örnek etiketleme aracını](../deploy-label-tool.md) dağıtma kılavuzuna bakın. 

   Ana bilgisayar aşağıdaki donanım gereksinimlerini karşılamalıdır:

    | Kapsayıcı | Minimum | Önerilen|
    |:--|:--|:--|
    |Örnek etiketleme aracı|2 çekirdekli, 4 GB bellek|4 çekirdekli, 8 GB bellek|

   İşletim sisteminiz için uygun talimatları izleyerek Docker'ı makinenize yükleyin: 
   * [Windows](https://docs.docker.com/docker-for-windows/)
   * [Macos](https://docs.docker.com/docker-for-mac/)
   * [Linux](https://docs.docker.com/install/)

1. `docker pull` Komutu ile örnek etiketleme aracı konteyner alın.
    ```
    docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool
    ```
1. Şimdi konteyneri `docker run`.
    ```
    docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool eula=accept
    ```

   Bu komut, örnek etiketleme aracını bir web tarayıcısı aracılığıyla kullanılabilir hale getirecektir. [http://localhost:3000](http://localhost:3000)Git.

> [!NOTE]
> Ayrıca Form Recognizer REST API'yi kullanarak belgeleri ve tren modellerini etiketleyebilirsiniz. REST API'yi eğitmek ve analiz etmek için [REST API ve Python'u kullanarak etiketleri olan Train'e](./python-labeled-data.md)bakın.

## <a name="set-up-input-data"></a>Giriş verilerini ayarlama

İlk olarak, tüm eğitim belgelerinin aynı biçimde olduğundan emin olun. Birden çok biçimde formlarınız varsa, bunları ortak biçime göre alt klasörler halinde düzenleyin. Eğitim alırken, API'yi bir alt klasöre yönlendirmeniz gerekir.

### <a name="configure-cross-domain-resource-sharing-cors"></a>Etki alanları arası kaynak paylaşımını yapılandırma (CORS)

Depolama hesabınızda CORS'i etkinleştirin. Azure portalında depolama hesabınızı seçin ve sol bölmedeki **CORS** sekmesini tıklatın. Alt satırda, aşağıdaki değerleri doldurun. Ardından en üstte **Kaydet'i** tıklatın.

* İzin verilen kökenler = * 
* İzin verilen \[yöntemler = tümünü seçin\]
* İzin verilen üstbilgi = *
* Açıkta kalan üstbilgi = * 
* Maksimum yaş = 200

> [!div class="mx-imgBorder"]
> ![Azure portalında CORS kurulumu](../media/label-tool/cors-setup.png)

## <a name="connect-to-the-sample-labeling-tool"></a>Örnek etiketleme aracına bağlanma

Örnek etiketleme aracı bir kaynağa (özgün formlarınızın bulunduğu yerde) ve bir hedefe (oluşturulan etiketleri ve çıktı verilerini dışa aktardığı) bağlanır.

Bağlantılar kurulabilir ve projeler arasında paylaşılabilir. Genişletilebilir bir sağlayıcı modeli kullanırlar, böylece yeni kaynak/hedef sağlayıcılar kolayca ekleyebilirsiniz.

Yeni bir bağlantı oluşturmak için sol daki gezinti çubuğundaki **Yeni Bağlantılar** (tak) simgesini tıklatın.

Alanları aşağıdaki değerlerle doldurun:

* **Görüntü Adı** - Bağlantı görüntü adı.
* **Açıklama** - Proje açıklamanız.
* **SAS URL** - Azure Blob Depolama kapsayıcınızın paylaşılan erişim imzası (SAS) URL'si. SAS URL'sini almak için Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim imzasını al'ı**seçin. Hizmeti kullandıktan sonra son kullanma süresini bir süre olarak ayarlayın. **Oku,** **Yaz,** **Sil**ve **Liste** izinlerinin işaretli olduğundan emin olun ve **Oluştur'u**tıklatın. Ardından **URL** bölümündeki değeri kopyalayın. Bu formu olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

![Örnek etiketleme aracının bağlantı ayarları](../media/label-tool/connections.png)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Örnek etiketleme aracında, projeler yapılandırmalarınızı ve ayarlarınızı saklar. Yeni bir proje oluşturun ve alanları aşağıdaki değerlerle doldurun:

* **Görüntü Adı** - proje görüntü adı
* **Güvenlik Belirteci** - Bazı proje ayarları API anahtarları veya diğer paylaşılan sırlar gibi hassas değerleri içerebilir. Her proje, hassas proje ayarlarını şifrelemek/çözmek için kullanılabilecek bir güvenlik belirteci oluşturur. Sol gezinti çubuğunun alt köşesindeki vites simgesine tıklayarak Uygulama Ayarları'nda güvenlik belirteçlerini bulabilirsiniz.
* **Kaynak Bağlantısı** - Bu proje için kullanmak istediğiniz önceki adımda oluşturduğunuz Azure Blob Depolama bağlantısı.
* **Klasör Yolu** - İsteğe Bağlı - Kaynak formlarınız blob kapsayıcısındaki bir klasörde bulunuyorsa, klasör adını buraya belirtin
* **Form Recognizer Service Uri** - Form Tanıma cılız uç nokta URL'niz.
* **API Anahtarı** - Form Tanıyıcı abonelik anahtarı.
* **Açıklama** - İsteğe Bağlı - Proje açıklaması

![Örnek etiketleme aracında yeni proje sayfası](../media/label-tool/new-project.png)

## <a name="label-your-forms"></a>Formlarınızı etiketleme

Bir proje oluşturduğunuzda veya açtığınızda, ana etiket düzenleyicipenceresi açılır. Etiket düzenleyicisi üç bölümden oluşur:

* Kaynak bağlantıdan kaydırılabilir formların listesini içeren yeniden boyutlandırılabilir önizleme bölmesi.
* Etiketleri uygulamanızı sağlayan ana düzenleyici bölmesi.
* Kullanıcıların etiketleri değiştirmesine, kilitlemesine, yeniden sipariş etmesine ve silmesine olanak tanıyan etiketler düzenleyici bölmesi. 

### <a name="identify-text-elements"></a>Metin öğelerini tanımlama

Her belgenin metin düzeni bilgilerini almak için sol **bölmedeki tüm dosyalarda OCR'yi çalıştır'ı** tıklatın. Etiketleme aracı, her metin öğesinin etrafına sınırlayıcı kutular çizer.

### <a name="apply-labels-to-text"></a>Etiketleri metne uygulama

Ardından, etiketler (etiketler) oluşturur ve bunları modelin tanımasını istediğiniz metin öğelerine uygularsınız.

1. İlk olarak, tanımlamak istediğiniz etiketleri oluşturmak için etiketler düzenleyici bölmesini kullanın.
  1. Yeni **+** bir etiket oluşturmak için tıklatın.
  1. Etiket adını girin.
  1. Etiketi kaydetmek için Enter tuşuna basın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden bir veya birden çok sözcük seçmek için tıklatın ve sürükleyin.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Numara tuşları ilk 10 etiket için anahtarlar olarak atanır. Etiket düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden sipariş edebilirsiniz.
    > [!Tip]
    > Formlarınızı etiketlerken aşağıdaki ipuçlarını aklınızda bulundurun.
    > * Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz.
    > * Her etiket sayfa başına yalnızca bir kez uygulanabilir. Bir değer aynı formda birden çok kez görünüyorsa, her örnek için farklı etiketler oluşturun. Örneğin: "fatura# 1", "fatura# 2" ve benzeri.
    > * Etiketler sayfalar arasında yayılamaz.
    > * Değerleri formda göründükleri gibi etiketle; bir değeri iki farklı etiketle iki parçaya bölmeye çalışmayın. Örneğin, bir adres alanı, birden çok satıra yayılsa bile tek bir etiketle etiketlenmelidir.
    > * Etiketlenen alanlara&mdash;anahtarları yalnızca değerleri eklemeyin.
    > * Tablo verileri otomatik olarak algılanmalıdır ve son çıktı JSON dosyasında kullanılabilir. Ancak, model tablo verilerinizin tümlerini algılayamazsa, bu alanları da el ile etiketleyebilirsiniz. Tablodaki her hücreyi farklı bir etiketle etiketle. Formlarınızın farklı sayıda satıriçeren tabloları varsa, mümkün olan en büyük tabloyla en az bir formu etiketlediğinizden emin olun.


Formlarınızın beşini etiketlemek için yukarıdaki adımları izleyin ve ardından bir sonraki adıma geçin.

![Örnek etiketleme aracının ana düzenleyici penceresi](../media/label-tool/main-editor.png)


## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Eğitim sayfasını açmak için sol bölmedeki Tren simgesine (tren vagonu) tıklayın. Ardından modeli eğitmeye başlamak için **Tren** düğmesini tıklatın. Eğitim süreci tamamlandıktan sonra aşağıdaki bilgileri görürsünüz:

* **Model Kimliği** - Oluşturulan ve eğitilen modelin kimliği. Her eğitim çağrısı kendi kimliği ile yeni bir model oluşturur. Bu dizeyi güvenli bir konuma kopyalayın; REST API üzerinden tahmin aramaları yapmak istiyorsanız ihtiyacınız olacak.
* **Ortalama Doğruluk** - Modelin ortalama doğruluğu. Yeni bir model oluşturmak için ek formları etiketleyerek ve yeniden eğitim alarak model doğruluğunu artırabilirsiniz. Beş formu etiketleyerek ve gerektiğinde daha fazla form ekleyerek başlamanızı öneririz.
* Etiket listesi ve etiket başına tahmini doğruluk.

![eğitim görünümü](../media/label-tool/train-screen.png)

Eğitim bittikten sonra **Ortalama Doğruluk** değerini inceleyin. Düşükse, daha fazla giriş belgesi eklemeli ve yukarıdaki adımları yinelemelisiniz. Önceden etiketlediğiniz belgeler proje dizininde kalır.

> [!TIP]
> Ayrıca bir REST API çağrısı ile eğitim sürecini çalıştırabilirsiniz. Bunu nasıl yapacağınızı öğrenmek için [Python'u kullanarak etiketleri olan Train'e](./python-labeled-data.md)bakın.

## <a name="analyze-a-form"></a>Formu analiz et

Modelinizi test etmek için soldaki Tahmin (dikdörtgenler) simgesine tıklayın. Eğitim sürecinde kullanmadığınız bir form belgesi yükleyin. Ardından form için anahtar/değer tahminleri almak için sağdaki **Tahmin** düğmesini tıklatın. Araç, etiketleri sınırlayıcı kutulara uygular ve her etiketin güvenini bildirir.

> [!TIP]
> Ayrıca, Bir REST çağrısı ile Analyze API çalıştırabilirsiniz. Bunu nasıl yapacağınızı öğrenmek için [Python'u kullanarak etiketleri olan Train'e](./python-labeled-data.md)bakın.

## <a name="improve-results"></a>Sonuçları iyileştirme

Bildirilen doğruluğa bağlı olarak, modeli geliştirmek için daha fazla eğitim yapmak isteyebilirsiniz. Bir tahminde bulunduktan sonra, uygulanan etiketlerin her biri için güven değerlerini inceleyin. Ortalama doğruluk eğitim değeri yüksekse, ancak güven puanları düşükse (veya sonuçlar yanlışsa), tahmin için kullanılan dosyayı eğitim kümesine eklemeniz, etiketlemeniz ve yeniden eğitmelisiniz.

Bildirilen ortalama doğruluk, güven puanları ve gerçek doğruluk, analiz edilen belgeler eğitimde kullanılanlardan farklı olduğunda tutarsız olabilir. Bazı belgelerin insanlar tarafından görüntülendiğinde benzer göründüğünü, ancak AI modeline farklı görünebileceğini unutmayın. Örneğin, eğitim kümesinin %20 varyasyon A ve %80 varyasyon B'den oluştuğu iki varyasyonu olan bir form türüyle eğitim alabilirsiniz. Tahmin sırasında, A varyasyonu belgelerinin güven puanları nın daha düşük olması olasıdır.

## <a name="save-a-project-and-resume-later"></a>Projeyi kaydedin ve daha sonra devam edin

Projenize başka bir zamanda veya başka bir tarayıcıda devam etmek için projenizin güvenlik belirteci kaydedip daha sonra yeniden girmeniz gerekir. 

### <a name="get-project-credentials"></a>Proje kimlik bilgilerini alma
Proje ayarları sayfanıza (kaydırıcı simgesi) gidin ve güvenlik belirteci adını not alın. Ardından, geçerli tarayıcı örneğinizdeki tüm güvenlik belirteçlerini gösteren uygulama ayarlarınıza (vites simgesi) gidin. Projenizin güvenlik jetonunu bulun ve adını ve anahtar değerini güvenli bir konuma kopyalayın.

### <a name="restore-project-credentials"></a>Proje kimlik bilgilerini geri yükleme
Projenize devam etmek istediğinizde, önce aynı blob depolama kapsayıcısına bir bağlantı oluşturmanız gerekir. Bunu yapmak için yukarıdaki adımları tekrarlayın. Ardından, uygulama ayarları sayfasına (vites simgesi) gidin ve projenizin güvenlik belirteci olup olmadığını görün. Değilse, yeni bir güvenlik belirteci ekleyin ve belirteç adınızın ve anahtarınızın üzerinde önceki adımdan kopyalayın. Ardından Ayarları Kaydet'i tıklatın. 

### <a name="resume-a-project"></a>Projeye devam edin

Son olarak, ana sayfaya (ev simgesi) gidin ve Bulut Projesi Aç'ı tıklatın. Ardından blob depolama bağlantısını seçin ve projenizin *.vott* dosyasını seçin. Uygulama, güvenlik belirteci olduğundan projenin tüm ayarlarını yükler.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilere sahip bir modeli eğitmek için Form Recognizer örnek etiketleme aracını nasıl kullanacağınızı öğrendiniz. Etiketleme aracını kendi uygulamanıza entegre etmek istiyorsanız, etiketli veri eğitimiyle ilgili REST API'lerini kullanın.

> [!div class="nextstepaction"]
> [Python kullanarak etiketlerle tren](./python-labeled-data.md)
