---
title: 'Hızlı başlangıç: formları etiketleme, model eğitme ve örnek etiketleme araç formu tanıyıcıyı kullanarak formları çözümleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, form belgelerinin el ile etiketlenmesi için form tanıyıcı örnek etiketleme aracını kullanacaksınız. Ardından, etiketli belgelerle özel bir belge işleme modeli eğtireceksiniz ve anahtar/değer çiftlerini çıkarmak için modeli kullanacaksınız.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/29/2021
ms.author: lajanuar
ms.custom: cog-serv-seo-aug-2020
keywords: Belge işleme
ms.openlocfilehash: 3814eb9d71f38406533d0bcecf594bbdcd42d5b7
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101095802"
---
<!-- markdownlint-disable MD001 -->
<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD034 -->
# <a name="train-a-form-recognizer-model-with-labels-using-the-sample-labeling-tool"></a>Örnek etiketleme aracını kullanarak form tanıyıcı modelini etiketlerle eğitme

Bu hızlı başlangıçta, el ile etiketlenmiş verilerle özel bir belge işleme modelini eğitebilmeniz için örnek etiketleme aracı ile REST API tanıyıcı formunu kullanacaksınız. Form tanıyıcı ile denetimli öğrenme hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Azure-Form-Recognizer/player]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title=" bir form tanıyıcı kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtarınızı ve uç noktanızı almak için bir form tanıyıcı kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
  * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
  * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Aynı türde en az altı biçim kümesi. Bu verileri modeli eğitme ve bir formu test etmek için kullanacaksınız. Bu hızlı başlangıç için [örnek veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) ( *sample_data.zip* indir ve Ayıkla) kullanabilirsiniz. Eğitim dosyalarını, standart performanslı katmanlı bir Azure depolama hesabındaki bir BLOB depolama kapsayıcısının köküne yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="try-it-out"></a>Deneyin

Form tanıyıcı örnek etiketleme aracını çevrimiçi olarak denemek için [Fott Web sitesine](https://fott-preview.azurewebsites.net/)gidin.

### <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri deneyin](https://fott-preview.azurewebsites.net/)

### <a name="v20"></a>[v2.0](#tab/v2-0)

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri deneyin](https://fott.azurewebsites.net/)

---

Form tanıyıcı hizmetini denemek için bir Azure aboneliğine ([ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)) ve bir [form tanıyıcı kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) uç noktası ve anahtarına ihtiyacınız olacaktır.

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

### <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)

```console
 docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest-preview
```

### <a name="v20"></a>[v2.0](#tab/v2-0)

```console
docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool
```

---
</br>
  3. Artık kapsayıcıyı ile çalıştırmaya hazırsınız `docker run` .

### <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)

```console
 docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest-preview eula=accept
```

### <a name="v20"></a>[v2.0](#tab/v2-0)

```console
docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool eula=accept
```

---

   Bu komut, örnek etiketleme aracını bir Web tarayıcısı üzerinden kullanılabilir hale getirir. `http://localhost:3000` öğesine gidin.

> [!NOTE]
> Ayrıca, REST API form tanıyıcı kullanarak belgeleri etiketleyebilir ve modellerle eğitebilirsiniz. REST API eğitme ve analiz etmek için bkz. [REST API ve Python kullanarak etiketlerle eğitme](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md).

## <a name="set-up-input-data"></a>Giriş verilerini ayarlama

İlk olarak, tüm eğitim belgelerinin aynı biçimde olduğundan emin olun. Farklı biçimlere sahip belgeleriniz varsa bunları biçime göre klasörlere ayırın. Eğitim sırasında API'yi bir alt klasöre yönlendirmeniz gerekecektir.

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
* **SAS URL 'si** -Azure Blob depolama kapsayıcının paylaşılan erişim IMZASı (SAS) URL 'si. [!INCLUDE [get SAS URL](../includes/sas-instructions.md)]

   :::image type="content" source="../media/quickstarts/get-sas-url.png" alt-text="SAS URL 'SI alımı":::

:::image type="content" source="../media/label-tool/connections.png" alt-text="Örnek etiketleme aracının bağlantı ayarları.":::


## <a name="create-a-new-project"></a>Yeni proje oluşturma

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

Ardından, Etiketler (Etiketler) oluşturacak ve bunları modelin analiz etmesini istediğiniz metin öğelerine uygulayacaksınız.

### <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)

1. İlk olarak, tanımlamak istediğiniz etiketleri oluşturmak için Etiketler Düzenleyicisi bölmesini kullanın:
   * **+** Yeni bir etiket oluşturmak için tıklayın.
   * Etiket adını girin.
   * Etiketi kaydetmek için ENTER tuşuna basın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden sözcükler seçmek için tıklayın. _V 2.1 Preview. 2_ API 'sinde, anahtar değer çiftleri olarak radyo düğmeleri ve onay kutuları gibi _seçim işaretlerini_ seçmek için de tıklayabilirsiniz. Form tanıyıcı, değer olarak seçim işaretinin "seçili" veya "seçilmemiş" olup olmadığını belirler.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Sayı tuşları ilk 10 etiketi için kısayol tuşu olarak atanır. Etiket Düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden düzenleyebilirsiniz.
    > [!Tip]
    > Formlarınızı etiketleyerek aşağıdaki ipuçlarını göz önünde bulundurun:
    >
    > * Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz.
    > * Her etiket, sayfa başına yalnızca bir kez uygulanabilir. Bir değer aynı formda birden çok kez görünürse, her örnek için farklı etiketler oluşturun. Örneğin: "Invoice # 1", "Invoice # 2" vb.
    > * Etiketler sayfalara yayılamaz.
    > * Değerleri formda göründükleri şekilde etiketleyin; iki farklı etikete sahip iki parçaya bir değeri bölmeye çalışmayın. Örneğin, bir adres alanının birden çok satıra yayılsa bile tek bir etiketle etiketlenmesi gerekir.
    > * Etiketli alanlarınıza anahtar eklemeyin &mdash; yalnızca değerleri.
    > * Tablo verileri otomatik olarak algılanmalı ve son çıktı JSON dosyasında kullanılabilir olacaktır. Ancak, model tüm tablo verilerinizi algılayamazsa, bu alanları da el ile etiketleyebilirsiniz. Tablodaki her hücreyi farklı bir etiketle etiketleyin. Formlarınızın farklı sayıda satır içeren tabloları varsa, en büyük olası tabloyla en az bir form etiketlediğinizden emin olun.
    > * **+** Etiketlerinizi aramak, yeniden adlandırmak, yeniden sıralamak ve silmek için sağ tarafındaki düğmeleri kullanın.
    > * Etiket kendisini silmeden uygulanan bir etiketi kaldırmak için belge görünümünde etiketli dikdörtgeni seçin ve DELETE tuşuna basın.
    >

### <a name="v20"></a>[v2.0](#tab/v2-0)

1. İlk olarak, tanımlamak istediğiniz etiketleri oluşturmak için Etiketler Düzenleyicisi bölmesini kullanın.
   1. **+** Yeni bir etiket oluşturmak için tıklayın.
   1. Etiket adını girin.
   1. Etiketi kaydetmek için ENTER tuşuna basın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden sözcükler seçmek için tıklayın.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Sayı tuşları ilk 10 etiketi için kısayol tuşu olarak atanır. Etiket Düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden düzenleyebilirsiniz.
    > [!Tip]
    > Formlarınızı etiketleyerek aşağıdaki ipuçlarını göz önünde bulundurun:
    >
    > * Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz.
    > * Her etiket, sayfa başına yalnızca bir kez uygulanabilir. Bir değer aynı formda birden çok kez görünürse, her örnek için farklı etiketler oluşturun. Örneğin: "Invoice # 1", "Invoice # 2" vb.
    > * Etiketler sayfalara yayılamaz.
    > * Değerleri formda göründükleri şekilde etiketleyin; iki farklı etikete sahip iki parçaya bir değeri bölmeye çalışmayın. Örneğin, bir adres alanının birden çok satıra yayılsa bile tek bir etiketle etiketlenmesi gerekir.
    > * Etiketli alanlarınıza anahtar eklemeyin &mdash; yalnızca değerleri.
    > * Tablo verileri otomatik olarak algılanmalı ve son çıktı JSON dosyasında kullanılabilir olacaktır. Ancak, model tüm tablo verilerinizi algılayamazsa, bu alanları da el ile etiketleyebilirsiniz. Tablodaki her hücreyi farklı bir etiketle etiketleyin. Formlarınızın farklı sayıda satır içeren tabloları varsa, en büyük olası tabloyla en az bir form etiketlediğinizden emin olun.
    > * **+** Etiketlerinizi aramak, yeniden adlandırmak, yeniden sıralamak ve silmek için sağ tarafındaki düğmeleri kullanın.
    > * Etiket kendisini silmeden uygulanan bir etiketi kaldırmak için belge görünümünde etiketli dikdörtgeni seçin ve DELETE tuşuna basın.
>

---

:::image type="content" source="../media/label-tool/main-editor-2-1.png" alt-text="Örnek etiketleme aracının ana düzenleyici penceresi.":::

Formlarınızın en az beş kısmını etiketlemek için yukarıdaki adımları izleyin.

### <a name="specify-tag-value-types"></a>Etiket değer türlerini belirtin

İsteğe bağlı olarak, her etiket için beklenen veri türünü ayarlayabilirsiniz. Etiketin sağ tarafındaki bağlam menüsünü açın ve menüden bir tür seçin. Bu özellik, algılama algoritmasının metin algılama doğruluğunu iyileştirecek bazı varsayımlar yapmasına olanak sağlar. Ayrıca, algılanan değerlerin son JSON çıkışında standartlaştırılmış bir biçimde döndürülmesini de sağlar. Değer türü bilgileri etiket dosyalarınızda aynı yoldaki *fields.js* dosyasına kaydedilir.

> [!div class="mx-imgBorder"]
> ![Örnek etiketleme aracıyla değer türü seçimi](../media/whats-new/value-type.png)

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
> `dmy` `mdy` Tarih biçimlendirme çalışması için bir biçim (,,) belirtmeniz gerekir `ymd` .
>
> Aşağıdaki karakterler Tarih sınırlayıcıları olarak kullanılabilir: `, - / . \` . Boşluk, sınırlayıcı olarak kullanılamaz. Örneğin:
>
> * 01, 01, 2020
> * 01-01-2020
> * 01/01/2020
>
> Gün ve ay her biri bir veya iki rakam olarak yazılabilir ve yıl iki veya dört basamaklı olabilir:
>
> * 1-1-2020
> * 1-01-20
>
> Bir tarih dizesinde sekiz basamak varsa sınırlayıcı isteğe bağlıdır:
>
> * 01012020
> * 01 01 2020
>
> Ay, tam veya kısa ad olarak da yazılabilir. Ad kullanılırsa, sınırlayıcı karakterler isteğe bağlıdır. Ancak, bu biçim diğerlerinden daha az tanınmış olabilir.
>
> * 01/Jan/2020
> * 01Oc2020
> * 01 Ocak 2020

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Sağ bölmedeki eğit simgesine tıklayarak Eğitim sayfasını açın. Ardından, modele eğitime başlamak için **eğitme** düğmesine tıklayın. Eğitim işlemi tamamlandıktan sonra, aşağıdaki bilgileri görürsünüz:

* **Model kimliği** -oluşturulan ve EĞITILEN modelin kimliği. Her eğitim çağrısı kendi KIMLIĞINE sahip yeni bir model oluşturur. Bu dizeyi güvenli bir konuma kopyalayın; [REST API](./client-library.md?pivots=programming-language-rest-api) veya [istemci kitaplığı](./client-library.md)aracılığıyla tahmin görüşmeleri yapmak istiyorsanız buna ihtiyacınız olur.
* **Ortalama doğruluk** -modelin ortalama doğruluğu. Yeni bir model oluşturmak için ek formları ve eğitimi yeniden etiketleyerek model doğruluğunu artırabilirsiniz. Beş formu etiketleyerek ve gerektiğinde daha fazla form ekleyerek başlamasını öneririz.
* Etiketlerin listesi ve etiket başına tahmini doğruluk.


:::image type="content" source="../media/label-tool/train-screen.png" alt-text="Eğitim görünümü.":::

Eğitim bittikten sonra, **Ortalama doğruluk** değerini inceleyin. Düşükse, daha fazla giriş belgesi eklemeli ve yukarıdaki adımları tekrarlamalısınız. Daha önce etiketlediğiniz belgeler proje dizininde kalacak.

> [!TIP]
> Eğitim sürecini bir REST API çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md).

## <a name="compose-trained-models"></a>Eğitilen modeller oluşturma

### <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)

Model Oluşturma özelliği sayesinde tek bir model kimliği altında 100 adede kadar model oluşturabilirsiniz. Oluşturduğunuz model kimliğiyle analiz gerçekleştirdiğinizde Form Tanıma hizmeti öncelikle gönderdiğiniz formu sınıflandırır, en uygun modelle eşleştirir ve ardından modele ait sonuçları döndürür. Bu, gelen formların birden çok şablondan birine ait olabileceği durumlarda faydalıdır.

Örnek etiketleme aracında modeller oluşturmak için sol taraftaki model oluşturma (birleştirme oku) simgesine tıklayın. Sol tarafta, birlikte oluşturmak istediğiniz modelleri seçin. Oklar simgesi olan modeller zaten oluşturulmuş modellerdir.
"Oluştur" düğmesine tıklayın. Açılır pencerede yeni oluşturulan modelinizi adlandırın ve "Oluştur" a tıklayın. İşlem tamamlandığında, yeni oluşturulan modeliniz listede görünmelidir.

:::image type="content" source="../media/label-tool/model-compose.png" alt-text="Model oluşturma UX görünümü.":::

### <a name="v20"></a>[v2.0](#tab/v2-0)

Bu özellik şu anda v 2.1 sürümünde kullanılabilir. Önizleme.

---

## <a name="analyze-a-form"></a>Formu analiz etme

Modelinizi test etmek için sol taraftaki tahmin (ampul) simgesine tıklayın. Eğitim sürecinde kullanmadığınız bir form belgesini karşıya yükleyin. Ardından, form için anahtar/değer tahminleri elde etmek için sağdaki **tahmin** düğmesine tıklayın. Araç etiketleri sınırlayıcı kutulara uygular ve her bir etiketin güvenirliği rapor eder.

> [!TIP]
> Çözümle API 'sini bir REST çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md).

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

Son olarak ana sayfaya (kuruluş simgesi) gidin ve bulut projesini aç ' a tıklayın. Sonra blob Storage bağlantısını seçin ve projenizin *. fott* dosyasını seçin. Uygulama, güvenlik belirtecine sahip olduğundan projenin tüm ayarlarını yükler.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilerle bir modeli eğtirecek şekilde form tanıyıcı örnek etiketleme aracının nasıl kullanılacağını öğrendiniz. Eğitim verilerini etiketlemek için kendi yardımcı programını derlemek isterseniz, etiketli veri eğitimi ile ilgilenen REST API 'Lerini kullanın.

> [!div class="nextstepaction"]
> [Python kullanarak etiketlerle eğitme](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md)

* [Form Tanıma nedir?](../overview.md)
* [Form tanıyıcı hızlı başlangıç](client-library.md)
