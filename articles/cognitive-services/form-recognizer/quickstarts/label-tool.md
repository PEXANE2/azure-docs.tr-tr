---
title: 'Hızlı başlangıç: formları etiketleme, model eğitme ve örnek etiketleme araç formu tanıyıcıyı kullanarak form çözümleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, form belgelerinin el ile etiketlenmesi için form tanıyıcı örnek etiketleme aracını kullanacaksınız. Ardından, etiketli belgelerle özel bir model eğtireceksiniz ve anahtar/değer çiftlerini çıkarmak için modeli kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 13c54b548a507043fda7ff230cf7641c26f471d1
ms.sourcegitcommit: 5b6acff3d1d0603904929cc529ecbcfcde90d88b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88724086"
---
# <a name="train-a-form-recognizer-model-with-labels-using-the-sample-labeling-tool"></a>Örnek etiketleme aracını kullanarak form tanıyıcı modelini etiketlerle eğitme

Bu hızlı başlangıçta, el ile etiketlenmiş verileri içeren özel bir modeli eğitebilmeniz için örnek etiketleme aracı ile REST API tanıyıcı formunu kullanacaksınız. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:

- Aynı türde en az altı biçim kümesi. Bu verileri modeli eğitme ve bir formu test etmek için kullanacaksınız. Bu hızlı başlangıç için [örnek bir veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını, standart performanslı katmanlı bir Azure depolama hesabındaki bir BLOB depolama kapsayıcısının köküne yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-the-sample-labeling-tool"></a>Örnek etiketleme aracını ayarlama

Örnek etiketleme aracını çalıştırmak için Docker altyapısını kullanacaksınız. Docker kapsayıcısını ayarlamak için bu adımları izleyin. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.

> [!TIP]
> OCR formu etiketleme Aracı, GitHub 'da açık kaynak proje olarak da kullanılabilir. Araç, tepki verme + Redux kullanılarak oluşturulan bir TypeScript Web uygulamasıdır. Daha fazla bilgi edinmek veya katkıda bulunmak için bkz. [OCR formu etiketleme aracı](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md#run-as-web-application) deposu. Aracı çevrimiçi olarak denemek için [Fott Web sitesine](https://fott.azurewebsites.net/)gidin.   

1. İlk olarak, bir ana bilgisayara Docker 'ı yüklemeniz gerekir. Bu kılavuzda, yerel bilgisayarın bir konak olarak nasıl kullanılacağı gösterilir. Azure 'da bir Docker barındırma hizmeti kullanmak istiyorsanız, [örnek etiketleme aracı](../deploy-label-tool.md) nasıl yapılır Kılavuzu ' na bakın. 

   Ana bilgisayar aşağıdaki donanım gereksinimlerini karşılamalıdır:

    | Kapsayıcı | Minimum | Önerilen|
    |:--|:--|:--|
    |Örnek etiketleme aracı|2 çekirdek, 4 GB bellek|4 çekirdek, 8 GB bellek|

   İşletim sisteminiz için uygun talimatları izleyerek makinenize Docker 'yi yüklemeyin: 
   * [Windows](https://docs.docker.com/docker-for-windows/)
   * [macOS](https://docs.docker.com/docker-for-mac/)
   * [Linux](https://docs.docker.com/install/)





1. Komutuyla örnek etiketleme araç kapsayıcısını alın `docker pull` .

    # <a name="v20"></a>[v2.0](#tab/v2-0)    
    ```
    docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool
    ```
    # <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
    ```
    docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:2.1.012970002-amd64-preview
    ```

    ---

1. Artık kapsayıcıyı ile çalıştırmaya hazırsınız `docker run` .

    # <a name="v20"></a>[v2.0](#tab/v2-0)    
    ```
    docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool eula=accept
    ```
    # <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
    ```
    docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:2.1.012970002-amd64-preview    
    ```

    --- 

   Bu komut, örnek etiketleme aracını bir Web tarayıcısı üzerinden kullanılabilir hale getirir. `http://localhost:3000` öğesine gidin.

> [!NOTE]
> Ayrıca, REST API form tanıyıcı kullanarak belgeleri etiketleyebilir ve modellerle eğitebilirsiniz. REST API eğitme ve analiz etmek için bkz. [REST API ve Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="set-up-input-data"></a>Giriş verilerini ayarlama

İlk olarak, tüm eğitim belgelerinin aynı biçimde olduğundan emin olun. Birden çok biçimdeki formlara sahipseniz, bunları ortak biçime göre alt klasörlerde düzenleyin. Eğitedığınızda, API 'yi bir alt klasöre yönlendirmeniz gerekir.

### <a name="configure-cross-domain-resource-sharing-cors"></a>Etki alanları arası kaynak paylaşımını (CORS) yapılandırma

Depolama hesabınızda CORS 'yi etkinleştirin. Azure portal depolama hesabınızı seçin ve sol bölmedeki **CORS** sekmesine tıklayın. Alt satırda aşağıdaki değerleri girin. Ardından en üstteki **Kaydet** ' e tıklayın.

* İzin verilen çıkış noktaları = * 
* İzin verilen yöntemler = \[ Tümünü Seç\]
* İzin verilen üstbilgiler = *
* Gösterilen üstbilgiler = * 
* En fazla yaş = 200

> [!div class="mx-imgBorder"]
> ![Azure portal CORS kurulumu](../media/label-tool/cors-setup.png)

## <a name="connect-to-the-sample-labeling-tool"></a>Örnek etiketleme aracına bağlanma

Örnek etiketleme aracı bir kaynağa (özgün formlarınızın bulunduğu yer) ve bir hedefe (oluşturulan etiketlerin ve çıkış verilerinin dışarı aktardığı yere) bağlanır.

Bağlantılar, projeler arasında ayarlanabilir ve paylaşılabilir. Bunlar bir Genişletilebilir sağlayıcı modeli kullandıkları için kolayca yeni kaynak/hedef sağlayıcılar ekleyebilirsiniz.

Yeni bir bağlantı oluşturmak için, sol gezinti çubuğundaki **yeni bağlantılar** (Tak) simgesine tıklayın.

Alanları aşağıdaki değerlerle girin:

* **Görünen ad** -bağlantı görünen adı.
* **Açıklama** -proje tanımlarınız.
* **SAS URL 'si** -Azure Blob depolama kapsayıcının paylaşılan erişim IMZASı (SAS) URL 'si. SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Hizmeti kullandıktan sonra sona erme süresini bir süre olarak ayarlayın. **Okuma**, **yazma**, **silme**ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .

:::image type="content" source="../media/label-tool/connections.png" alt-text="Örnek etiketleme aracının bağlantı ayarları.":::


## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Örnek etiketleme aracında, projeler yapılandırma ve ayarlarınızı depolar. Yeni bir proje oluşturun ve alanları aşağıdaki değerlerle girin:

* **Görünen ad** -proje görünen adı
* **Güvenlik belirteci** -bazı proje ayarları, API anahtarları veya diğer paylaşılan gizli diziler gibi hassas değerler içerebilir. Her proje, hassas proje ayarlarını şifrelemek/şifrelerini çözmek için kullanılabilen bir güvenlik belirteci oluşturur. Sol gezinti çubuğunun altındaki dişli simgesine tıklayarak uygulama ayarlarında güvenlik belirteçlerini bulabilirsiniz.
* **Kaynak bağlantısı** -bu proje için kullanmak istediğiniz önceki adımda oluşturduğunuz Azure Blob depolama bağlantısı.
* **Klasör yolu** -isteğe bağlı-kaynak formlarınız blob kapsayıcısındaki bir klasörde yer alıyorsa, burada klasör adını belirtin
* **Form tanıyıcı hizmeti URI 'si** -form tanıyıcı uç nokta URL 'si.
* **API anahtarı** -form tanıyıcı abonelik anahtarınız.
* **Açıklama** -isteğe bağlı-proje açıklaması

:::image type="content" source="../media/label-tool/new-project.png" alt-text="Örnek etiketleme aracında yeni proje sayfası.":::

## <a name="label-your-forms"></a>Formlarınızı etiketleme

Bir proje oluşturduğunuzda veya açtığınızda, ana Etiket Düzenleyicisi penceresi açılır. Etiket Düzenleyicisi üç bölümden oluşur:

* Kaynak bağlantıdan bir form listesinin kaydırılabilir listesini içeren bir yeniden boyutlandırılabilir Önizleme bölmesi.
* Etiket uygulamanıza izin veren ana Düzenleyici bölmesi.
* Kullanıcıların etiketleri değiştirmesine, kilitlemesine, yeniden sıralayabilir ve silmesine izin veren Etiketler Düzenleyicisi bölmesi. 

### <a name="identify-text-elements"></a>Metin öğelerini tanımla

Her belge için metin düzeni bilgilerini almak için sol bölmedeki **tüm dosyalarda OCR Çalıştır** ' a tıklayın. Etiketleme aracı her metin öğesinin etrafında sınırlayıcı kutular çizer.

Ayrıca, hangi tabloların otomatik olarak ayıklandığını gösterir. Ayıklanan tabloyu görmek için belgenin sol tarafındaki tablo/kılavuz simgesine tıklayın. Bu hızlı başlangıçta, tablo içeriği otomatik olarak ayıklandığından tablo içeriğini etiketlendirilecektir, bunun yerine otomatik ayıklamanın olması gerekir.

:::image type="content" source="../media/label-tool/table-extraction.png" alt-text="Örnek etiketleme aracında tablo görselleştirme.":::

### <a name="apply-labels-to-text"></a>Metne Etiketler uygulama

Ardından, Etiketler (Etiketler) oluşturacak ve bunları modelin tanımasını istediğiniz metin öğelerine uygulayacaksınız.

# <a name="v20"></a>[v2.0](#tab/v2-0)  
1. İlk olarak, tanımlamak istediğiniz etiketleri oluşturmak için Etiketler Düzenleyicisi bölmesini kullanın.
   1. **+** Yeni bir etiket oluşturmak için tıklayın.
   1. Etiket adını girin.
   1. Etiketi kaydetmek için ENTER tuşuna basın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden sözcükler seçmek için tıklayın.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Sayı tuşları ilk 10 etiketi için kısayol tuşu olarak atanır. Etiket Düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden düzenleyebilirsiniz.
    > [!Tip]
    > Formlarınızı etiketleyerek aşağıdaki ipuçlarını göz önünde bulundurun.
    > * Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz.
    > * Her etiket, sayfa başına yalnızca bir kez uygulanabilir. Bir değer aynı formda birden çok kez görünürse, her örnek için farklı etiketler oluşturun. Örneğin: "Invoice # 1", "Invoice # 2" vb.
    > * Etiketler sayfalara yayılamaz.
    > * Değerleri formda göründükleri şekilde etiketleyin; iki farklı etikete sahip iki parçaya bir değeri bölmeye çalışmayın. Örneğin, bir adres alanının birden çok satıra yayılsa bile tek bir etiketle etiketlenmesi gerekir.
    > * Etiketli alanlarınıza anahtar eklemeyin &mdash; yalnızca değerleri.
    > * Tablo verileri otomatik olarak algılanmalı ve son çıktı JSON dosyasında kullanılabilir olacaktır. Ancak, model tüm tablo verilerinizi algılayamazsa, bu alanları da el ile etiketleyebilirsiniz. Tablodaki her hücreyi farklı bir etiketle etiketleyin. Formlarınızın farklı sayıda satır içeren tabloları varsa, en büyük olası tabloyla en az bir form etiketlediğinizden emin olun.
    > * **+** Etiketlerinizi aramak, yeniden adlandırmak, yeniden sıralamak ve silmek için sağ tarafındaki düğmeleri kullanın.
    > * Etiket kendisini silmeden uygulanan bir etiketi kaldırmak için belge görünümünde etiketli dikdörtgeni seçin ve DELETE tuşuna basın.


# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1) 
1. İlk olarak, tanımlamak istediğiniz etiketleri oluşturmak için Etiketler Düzenleyicisi bölmesini kullanın.
   1. **+** Yeni bir etiket oluşturmak için tıklayın.
   1. Etiket adını girin.
   1. Etiketi kaydetmek için ENTER tuşuna basın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden sözcükler seçmek için tıklayın. _V 2.1 önizlemesinde_ , anahtar değer çiftleri olarak radyo düğmeleri ve onay kutuları gibi _seçim işaretlerini_ seçmek için de tıklayabilirsiniz. Form tanıyıcı, değer olarak seçim işaretinin "seçili" veya "seçilmemiş" olup olmadığını belirler.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Sayı tuşları ilk 10 etiketi için kısayol tuşu olarak atanır. Etiket Düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden düzenleyebilirsiniz.
    > [!Tip]
    > Formlarınızı etiketleyerek aşağıdaki ipuçlarını göz önünde bulundurun.
    > * Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz.
    > * Her etiket, sayfa başına yalnızca bir kez uygulanabilir. Bir değer aynı formda birden çok kez görünürse, her örnek için farklı etiketler oluşturun. Örneğin: "Invoice # 1", "Invoice # 2" vb.
    > * Etiketler sayfalara yayılamaz.
    > * Değerleri formda göründükleri şekilde etiketleyin; iki farklı etikete sahip iki parçaya bir değeri bölmeye çalışmayın. Örneğin, bir adres alanının birden çok satıra yayılsa bile tek bir etiketle etiketlenmesi gerekir.
    > * Etiketli alanlarınıza anahtar eklemeyin &mdash; yalnızca değerleri.
    > * Tablo verileri otomatik olarak algılanmalı ve son çıktı JSON dosyasında kullanılabilir olacaktır. Ancak, model tüm tablo verilerinizi algılayamazsa, bu alanları da el ile etiketleyebilirsiniz. Tablodaki her hücreyi farklı bir etiketle etiketleyin. Formlarınızın farklı sayıda satır içeren tabloları varsa, en büyük olası tabloyla en az bir form etiketlediğinizden emin olun.
    > * **+** Etiketlerinizi aramak, yeniden adlandırmak, yeniden sıralamak ve silmek için sağ tarafındaki düğmeleri kullanın.
    > * Etiket kendisini silmeden uygulanan bir etiketi kaldırmak için belge görünümünde etiketli dikdörtgeni seçin ve DELETE tuşuna basın.


---

:::image type="content" source="../media/label-tool/main-editor-2-1.png" alt-text="Örnek etiketleme aracının ana düzenleyici penceresi.":::


Formlarınızın en az beş kısmını etiketlemek için yukarıdaki adımları izleyin.

### <a name="specify-tag-value-types"></a>Etiket değer türlerini belirtin

İsteğe bağlı olarak, her etiket için beklenen veri türünü ayarlayabilirsiniz. Etiketin sağ tarafındaki bağlam menüsünü açın ve menüden bir tür seçin. Bu özellik, algılama algoritmasının metin algılama doğruluğunu iyileştirecek bazı varsayımlar yapmasına olanak sağlar. Ayrıca, algılanan değerlerin son JSON çıkışında standartlaştırılmış bir biçimde döndürülmesini de sağlar. 

> [!div class="mx-imgBorder"]
> ![Örnek etiketleme aracıyla değer türü seçimi](../media/whats-new/formre-value-type.png)

Aşağıdaki değer türleri ve Çeşitlemeler Şu anda destekleniyor:
* `string`
    * Varsayılan, `no-whitespaces` , `alphanumeric`
* `number`
    * varsayılanını `currency`
* `date` 
    * Varsayılan, `dmy` , `mdy` , `ymd`
* `time`
* `integer`
* `selectionMark` – _Yeni v 2.1-Önizleme. 1!_

> [!NOTE]
> Tarih biçimlendirme için şu kurallara bakın:
> 
> Aşağıdaki karakterler DMY Tarih sınırlayıcıları olarak kullanılabilir: `, - / . \` . Boşluk, sınırlayıcı olarak kullanılamaz. Örneğin:
> * 01, 01, 2020
> * 01-01-2020
> * 01/01/2020
>
> Gün ve ay her biri bir veya iki rakam olarak yazılabilir ve yıl iki veya dört basamaklı olabilir:
> * 1-1-2020
> * 1-01-20
>
> Bir DMY Tarih dizesinde sekiz basamak varsa sınırlayıcı isteğe bağlıdır:
> * 01012020
> * 01 01 2020
>
> Ay, tam veya kısa ad olarak da yazılabilir. Ad kullanılırsa, sınırlayıcı karakterler isteğe bağlıdır:
> * 01/Jan/2020
> * 01Oc2020
> * 01 Ocak 2020

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Sağ bölmedeki eğit simgesine tıklayarak Eğitim sayfasını açın. Ardından, modele eğitime başlamak için **eğitme** düğmesine tıklayın. Eğitim işlemi tamamlandıktan sonra, aşağıdaki bilgileri görürsünüz:

* **Model kimliği** -oluşturulan ve EĞITILEN modelin kimliği. Her eğitim çağrısı kendi KIMLIĞINE sahip yeni bir model oluşturur. Bu dizeyi güvenli bir konuma kopyalayın; [REST API](./curl-train-extract.md) veya [istemci kitaplığı](./client-library.md)aracılığıyla tahmin görüşmeleri yapmak istiyorsanız buna ihtiyacınız olur.
* **Ortalama doğruluk** -modelin ortalama doğruluğu. Yeni bir model oluşturmak için ek formları ve eğitimi yeniden etiketleyerek model doğruluğunu artırabilirsiniz. Beş formu etiketleyerek ve gerektiğinde daha fazla form ekleyerek başlamasını öneririz.
* Etiketlerin listesi ve etiket başına tahmini doğruluk.


:::image type="content" source="../media/label-tool/train-screen.png" alt-text="Eğitim görünümü.":::

Eğitim bittikten sonra, **Ortalama doğruluk** değerini inceleyin. Düşükse, daha fazla giriş belgesi eklemeli ve yukarıdaki adımları tekrarlamalısınız. Daha önce etiketlediğiniz belgeler proje dizininde kalacak.

> [!TIP]
> Eğitim sürecini bir REST API çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="compose-trained-models"></a>Eğitilen modeller oluşturma

# <a name="v20"></a>[v2.0](#tab/v2-0)  

Bu özellik şu anda v 2.1 sürümünde kullanılabilir. Önizleme. 

# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1) 

Model oluşturma ile, tek bir model KIMLIĞIYLE en fazla 100 model oluşturabilirsiniz. Bu oluşturulmuş model KIMLIĞIYLE çözümle çağırdığınızda, form tanıyıcı önce gönderdiğiniz formu sınıflandırır, en iyi eşleşen modeliyle eşleştirir ve ardından bu modelin sonuçlarını döndürür. Bu, gelen formlar çeşitli şablonlardan birine ait olduğunda faydalıdır.

Örnek etiketleme aracında modeller oluşturmak için sol taraftaki model oluşturma (iki ok) simgesine tıklayın. Sol tarafta, birlikte oluşturmak istediğiniz modelleri seçin. Oklar simgesi olan modeller zaten oluşturulmuş modellerdir. "Oluştur" düğmesine tıklayın. Açılan pencerede yeni oluşturulan modelinizi adlandırın ve "Oluştur" a tıklayın. İşlem tamamlandığında, yeni oluşturulan modeliniz listede görünmelidir. 

:::image type="content" source="../media/label-tool/model-compose.png" alt-text="Model oluşturma UX görünümü.":::

---

## <a name="analyze-a-form"></a>Formu analiz etme 

Modelinizi test etmek için sol taraftaki tahmin (ampul) simgesine tıklayın. Eğitim sürecinde kullanmadığınız bir form belgesini karşıya yükleyin. Ardından, form için anahtar/değer tahminleri elde etmek için sağdaki **tahmin** düğmesine tıklayın. Araç etiketleri sınırlayıcı kutulara uygular ve her bir etiketin güvenirliği rapor eder.

> [!TIP]
> Çözümle API 'sini bir REST çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="improve-results"></a>Sonuçları geliştirme

Bildirilen doğrulukla bağlı olarak, modeli geliştirmek için daha fazla eğitim yapmak isteyebilirsiniz. Tahmin tamamladıktan sonra, uygulanan etiketlerin her biri için güvenirlik değerlerini inceleyin. Ortalama doğruluk eğitim değeri yüksekse, ancak güven puanları düşükse (veya sonuçlar yanlış olursa), tahmin için kullanılan dosyayı eğitim kümesine eklemeli, etiketleyip yeniden eğitmelisiniz.

Çözümlenen belgeler eğitiminde kullanılanlardan farklıysa, bildirilen ortalama doğruluk, güvenilirlik ve gerçek doğruluk tutarsız olabilir. Bazı belgelerin kişiler tarafından görüntülendiklerinde benzer olduğunu ancak AI modeline ayrı görünebileceğini aklınızda bulundurun. Örneğin, iki çeşitleme içeren bir form türüyle eğitebilirsiniz; burada eğitim kümesi %20 değişim A ve %80 değişim B ' den oluşur. Tahmin sırasında, A varyasyonuna ait belgelerin güvenilirlik puanları büyük olasılıkla daha düşüktür.

## <a name="save-a-project-and-resume-later"></a>Projeyi kaydetme ve daha sonra yeniden başlatma

Projenizi başka bir zamanda veya başka bir tarayıcıda duraklatmak için, projenizin güvenlik belirtecini kaydetmeniz ve daha sonra yeniden girmeniz gerekir. 

### <a name="get-project-credentials"></a>Proje kimlik bilgilerini al
Proje ayarları sayfanıza gidin (kaydırıcı simgesi) ve güvenlik belirteci adını göz önünde alın. Ardından, geçerli tarayıcı örneğinizdeki tüm güvenlik belirteçlerini gösteren uygulama ayarlarınıza (dişli simgesi) gidin. Projenizin güvenlik belirtecini bulun ve adını ve anahtar değerini güvenli bir konuma kopyalayın.

### <a name="restore-project-credentials"></a>Proje kimlik bilgilerini geri yükle
Projenizi yeniden başlatmak istediğinizde, önce aynı BLOB depolama kapsayıcısına bir bağlantı oluşturmanız gerekir. Bunu yapmak için yukarıdaki adımları tekrarlayın. Ardından, uygulama ayarları sayfasına (dişli simgesi) gidin ve projenizin güvenlik belirtecinin orada olup olmadığını görün. Değilse, yeni bir güvenlik belirteci ekleyin ve önceki adımdan belirteç adınızın ve anahtarınızın üzerine kopyalayın. Sonra Ayarları Kaydet ' e tıklayın. 

### <a name="resume-a-project"></a>Projeyi sürdürür

Son olarak ana sayfaya (kuruluş simgesi) gidin ve bulut projesini aç ' a tıklayın. Sonra blob Storage bağlantısını seçin ve projenizin *. vott* dosyasını seçin. Uygulama, güvenlik belirtecine sahip olduğundan projenin tüm ayarlarını yükler.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilerle bir modeli eğtirecek şekilde form tanıyıcı örnek etiketleme aracının nasıl kullanılacağını öğrendiniz. Etiketleme aracını kendi uygulamanızla tümleştirmek isterseniz etiketli veri eğitimi ile ilgilenen REST API 'Lerini kullanın.

> [!div class="nextstepaction"]
> [Python kullanarak etiketlerle eğitme](./python-labeled-data.md)
