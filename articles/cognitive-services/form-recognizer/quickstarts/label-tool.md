---
title: 'Hızlı başlangıç: formları etiketleme, model eğitme ve örnek etiketleme araç formu tanıyıcıyı kullanarak form çözümleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, form belgelerinin el ile etiketlenmesi için form tanıyıcı örnek etiketleme aracını kullanacaksınız. Ardından, etiketli belgelerle özel bir model eğtireceksiniz ve anahtar/değer çiftlerini çıkarmak için modeli kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 11/14/2019
ms.author: pafarley
ms.openlocfilehash: 8ab673c1a268f5ab663e8f423dd9b60cdfde14ab
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77118379"
---
# <a name="train-a-form-recognizer-model-with-labels-using-the-sample-labeling-tool"></a>Örnek etiketleme aracını kullanarak form tanıyıcı modelini etiketlerle eğitme

Bu hızlı başlangıçta, el ile etiketlenmiş verileri içeren özel bir modeli eğitebilmeniz için örnek etiketleme aracı ile REST API tanıyıcı formunu kullanacaksınız. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:

- Aynı türde en az altı biçim kümesi. Bu verileri modeli eğitme ve bir formu test etmek için kullanacaksınız. Bu hızlı başlangıç için [örnek bir veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure depolama hesabındaki BLOB depolama kapsayıcısının köküne yükleyin.

## <a name="set-up-the-sample-labeling-tool"></a>Örnek etiketleme aracını ayarlama

Örnek etiketleme aracını çalıştırmak için Docker altyapısını kullanacaksınız. Docker kapsayıcısını ayarlamak için bu adımları izleyin. Docker ve kapsayıcı temelleri hakkında bilgi için bkz. [Docker genel bakış](https://docs.docker.com/engine/docker-overview/).
1. İlk olarak, bir ana bilgisayara Docker 'ı yüklemeniz gerekir. Ana bilgisayar yerel bilgisayarınız ([Windows](https://docs.docker.com/docker-for-windows/), [MacOS](https://docs.docker.com/docker-for-mac/)veya [Linux](https://docs.docker.com/install/)) olabilir. Ya da Azure 'da [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/index), [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/index)veya bir [Azure Stack dağıtılan](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-kubernetes-deploy?view=azs-1910)bir Kubernetes kümesi gibi Docker barındırma hizmeti kullanabilirsiniz. Ana bilgisayar aşağıdaki donanım gereksinimlerini karşılamalıdır:

    | Kapsayıcı | Minimum | Önerilen|
    |:--|:--|:--|
    |Örnek etiketleme aracı|2 çekirdek, 4 GB bellek|4 çekirdek, 8 GB bellek|
    
1. Örnek etiketleme araç kapsayıcısını `docker pull` komutuyla alın.
    ```
    docker pull mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool
    ```
1. Artık kapsayıcıyı `docker run`ile çalıştırmaya hazır olursunuz.
    ```
    docker run -it -p 3000:80 mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool eula=accept
    ```

   Bu komut, örnek etiketleme aracını bir Web tarayıcısı üzerinden kullanılabilir hale getirir. [http://localhost:3000](http://localhost:3000) kısmına gidin.

> [!NOTE]
> Ayrıca, REST API form tanıyıcı kullanarak belgeleri etiketleyebilir ve modellerle eğitebilirsiniz. REST API eğitme ve analiz etmek için bkz. [REST API ve Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="set-up-input-data"></a>Giriş verilerini ayarlama

İlk olarak, tüm eğitim belgelerinin aynı biçimde olduğundan emin olun. Birden çok biçimdeki formlara sahipseniz, bunları ortak biçime göre alt klasörlerde düzenleyin. Eğitedığınızda, API 'yi bir alt klasöre yönlendirmeniz gerekir.

### <a name="configure-cross-domain-resource-sharing-cors"></a>Etki alanları arası kaynak paylaşımını (CORS) yapılandırma

Depolama hesabınızda CORS 'yi etkinleştirin. Azure portal depolama hesabınızı seçin ve sol bölmedeki **CORS** sekmesine tıklayın. Alt satırda aşağıdaki değerleri girin. Ardından en üstteki **Kaydet** ' e tıklayın.

* İzin verilen çıkış noktaları = * 
* İzin verilen Yöntemler = \[tüm\] Seç
* İzin verilen üstbilgiler = *
* Gösterilen üstbilgiler = * 
* En fazla yaş = 200

> [!div class="mx-imgBorder"]
> ![CORS kurulumunu Azure portal](../media/label-tool/cors-setup.png)

## <a name="connect-to-the-sample-labeling-tool"></a>Örnek etiketleme aracına bağlanma

Örnek etiketleme aracı bir kaynağa (özgün formlarınızın bulunduğu yer) ve bir hedefe (oluşturulan etiketlerin ve çıkış verilerinin dışarı aktardığı konum) bağlanır.

Bağlantılar, projeler arasında ayarlanabilir ve paylaşılabilir. Bunlar bir Genişletilebilir sağlayıcı modeli kullandıkları için kolayca yeni kaynak/hedef sağlayıcılar ekleyebilirsiniz.

Yeni bir bağlantı oluşturmak için, sol gezinti çubuğundaki **yeni bağlantılar** (Tak) simgesine tıklayın.

Alanları aşağıdaki değerlerle girin:

* **Görünen ad** -bağlantı görünen adı.
* **Açıklama** -proje tanımlarınız.
* **SAS URL 'si** -Azure Blob depolama kapsayıcının paylaşılan erişim IMZASı (SAS) URL 'si. SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Hizmeti kullandıktan sonra sona erme süresini bir süre olarak ayarlayın. **Okuma**, **yazma**, **silme**ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

![Örnek etiketleme aracının bağlantı ayarları](../media/label-tool/connections.png)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Örnek etiketleme aracında, projeler yapılandırma ve ayarlarınızı depolar. Yeni bir proje oluşturun ve alanları aşağıdaki değerlerle girin:

* **Görünen ad** -proje görünen adı
* **Güvenlik belirteci** -bazı proje ayarları, API anahtarları veya diğer paylaşılan gizli diziler gibi hassas değerler içerebilir. Her proje, hassas proje ayarlarını şifrelemek/şifrelerini çözmek için kullanılabilen bir güvenlik belirteci oluşturur. Güvenlik belirteçleri, sol gezinti çubuğunun alt köşesindeki dişli simgesine tıklayarak uygulama ayarları ' nda bulunabilir.
* **Kaynak bağlantısı** -bu proje için kullanmak istediğiniz önceki adımda oluşturduğunuz Azure Blob depolama bağlantısı.
* **Klasör yolu** -isteğe bağlı-kaynak formlarınız blob kapsayıcısındaki bir klasörde yer alıyorsa, burada klasör adını belirtin
* **Form tanıyıcı hizmeti URI 'si** -form tanıyıcı uç nokta URL 'si.
* **API anahtarı** -form tanıyıcı abonelik anahtarınız.
* **Açıklama** -isteğe bağlı-proje açıklaması

![Örnek etiketleme aracında yeni proje sayfası](../media/label-tool/new-project.png)

## <a name="label-your-forms"></a>Formlarınızı etiketleme

Bir proje oluşturduğunuzda veya açtığınızda, ana Etiket Düzenleyicisi penceresi açılır. Etiket Düzenleyicisi üç bölümden oluşur:

* Kaynak bağlantıdan bir form listesinin kaydırılabilir listesini içeren bir yeniden boyutlandırılabilir Önizleme bölmesi.
* Etiket uygulamanıza izin veren ana Düzenleyici bölmesi.
* Kullanıcıların etiketleri değiştirmesine, kilitlemesine, yeniden sıralayabilir ve silmesine izin veren Etiketler Düzenleyicisi bölmesi. 

### <a name="identify-text-elements"></a>Metin öğelerini tanımla

Her belge için metin düzeni bilgilerini almak için sol bölmedeki **tüm dosyalarda OCR Çalıştır** ' a tıklayın. Etiketleme aracı her metin öğesinin etrafında sınırlayıcı kutular çizer.

### <a name="apply-labels-to-text"></a>Metne Etiketler uygulama

Ardından, Etiketler oluşturacak ve bunları modelin tanımasını istediğiniz metin öğelerine uygulayacaksınız.

1. İlk olarak, tanımlamak istediğiniz etiketleri (Etiketler) oluşturmak için Etiketler Düzenleyicisi bölmesini kullanın.
1. Ana düzenleyicide, vurgulanan metin öğelerinden bir veya birden çok sözcük seçmek için tıklayın ve sürükleyin.

    > [!NOTE]
    > Şu anda birden çok sayfa arasında yayılan metin seçemezsiniz.
1. Uygulamak istediğiniz etikete tıklayın veya ilgili klavye tuşuna basın. Seçili her metin öğesine yalnızca bir etiket uygulayabilirsiniz ve her etiket sayfa başına yalnızca bir kez uygulanabilir.

    > [!TIP]
    > Sayı tuşları ilk on etiket için kısayol olarak atanır. Etiket Düzenleyicisi bölmesindeki yukarı ve aşağı ok simgelerini kullanarak etiketlerinizi yeniden düzenleyebilirsiniz.

Formlarınızın beş kısmını etiketlemek için yukarıdaki adımları izleyin ve sonra bir sonraki adıma geçin.

![Örnek etiketleme aracının ana düzenleyici penceresi](../media/label-tool/main-editor.png)


## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Eğitim sayfasını açmak için sol bölmedeki eğitme simgesine (eğitme arabası) tıklayın. Ardından, modele eğitime başlamak için **eğitme** düğmesine tıklayın. Eğitim işlemi tamamlandıktan sonra, aşağıdaki bilgileri görürsünüz:

* **Model kimliği** -oluşturulan ve EĞITILEN modelin kimliği. Her eğitim çağrısı kendi KIMLIĞINE sahip yeni bir model oluşturur. Bu dizeyi güvenli bir konuma kopyalayın; REST API aracılığıyla tahmin görüşmeleri yapmak istiyorsanız buna ihtiyacınız olur.
* **Ortalama doğruluk** -modelin ortalama doğruluğu. Yeni bir model oluşturmak için ek formları ve eğitimi yeniden etiketleyerek model doğruluğunu artırabilirsiniz. Beş formu etiketleyerek ve gerektiğinde daha fazla form ekleyerek başlamasını öneririz.
* Etiketlerin listesi ve etiket başına tahmini doğruluk.

![Eğitim görünümü](../media/label-tool/train-screen.png)

Eğitim bittikten sonra, **Ortalama doğruluk** değerini inceleyin. Düşükse, daha fazla giriş belgesi eklemeli ve yukarıdaki adımları tekrarlamalısınız. Daha önce etiketlediğiniz belgeler proje dizininde kalacak.

> [!TIP]
> Eğitim sürecini bir REST API çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="analyze-a-form"></a>Formu analiz etme

Modelinizi test etmek için sol taraftaki tahmin (dikdörtgenler) simgesine tıklayın. Eğitim sürecinde kullandığınız bir form belgesini karşıya yükleyin. Ardından, form için anahtar/değer tahminleri elde etmek için sağdaki **tahmin** düğmesine tıklayın. Araç etiketleri sınırlayıcı kutulara uygular ve her bir etiketin güvenirliği rapor eder.

> [!TIP]
> Çözümle API 'sini bir REST çağrısıyla de çalıştırabilirsiniz. Bunun nasıl yapılacağını öğrenmek için bkz. [Python kullanarak etiketlerle eğitme](./python-labeled-data.md).

## <a name="improve-results"></a>Sonuçları geliştirme

Bildirilen doğrulukla bağlı olarak, modeli geliştirmek için daha fazla eğitim yapmak isteyebilirsiniz. Tahmin tamamladıktan sonra, uygulanan etiketlerin her biri için güvenirlik değerlerini inceleyin. Ortalama doğruluk eğitim değeri yüksekse, ancak güven puanları düşükse (veya sonuçlar yanlış olursa), tahmin için kullanılan dosyayı eğitim kümesine eklemeli, etiketleyip yeniden eğitmelisiniz.

Analiz edilen belgeler eğitiminde kullanılanlardan farklıysa, bildirilen ortalama doğruluk, güven puanı ve gerçek doğruluk tutarsız olabilir. Bazı belgelerin kişiler tarafından görüntülendiklerinde benzer olduğunu ancak AI modeline ayrı görünebileceğini aklınızda bulundurun. Örneğin, iki çeşitleme içeren bir form türüyle eğitebilirsiniz; burada eğitim kümesi %20 değişim A ve %80 değişim B ' den oluşur. Tahmin sırasında, A varyasyonuna ait belgelerin güvenilirlik puanları büyük olasılıkla daha düşüktür.

## <a name="save-a-project-and-resume-later"></a>Projeyi kaydetme ve daha sonra yeniden başlatma

Projenizi başka bir zamanda veya başka bir tarayıcıda duraklatmak için, projenizin güvenlik belirtecini kaydetmeniz ve daha sonra yeniden girmeniz gerekir. 

### <a name="get-project-credentials"></a>Proje kimlik bilgilerini al
Proje ayarları sayfanıza gidin (kaydırıcı simgesi) ve güvenlik belirteci adını göz önünde alın. Ardından, geçerli tarayıcı örneğinizdeki tüm güvenlik belirteçlerini gösteren uygulama ayarlarınıza (dişli simgesi) gidin. Projenizin güvenlik belirtecini bulun ve adını ve anahtar değerini güvenli bir konuma kopyalayın.

### <a name="restore-project-credentials"></a>Proje kimlik bilgilerini geri yükle
Projenizi yeniden başlatmak istediğinizde, önce aynı BLOB depolama kapsayıcısına bir bağlantı oluşturmanız gerekir. Bunu yapmak için yukarıdaki adımları izleyin. Ardından, uygulama ayarları sayfasına (dişli simgesi) gidin ve projenizin güvenlik belirtecinin orada olup olmadığını görün. Değilse, yeni bir güvenlik belirteci ekleyin ve önceki adımdan belirteç adınızın ve anahtarınızın üzerine kopyalayın. Sonra Ayarları Kaydet ' e tıklayın. 

### <a name="resume-a-project"></a>Projeyi sürdürür
Son olarak ana sayfaya (kuruluş simgesi) gidin ve bulut projesini aç ' a tıklayın. Sonra blob Storage bağlantısını seçin ve projenizin *. vott* dosyasını seçin. Uygulama, güvenlik belirtecine sahip olduğundan projenin tüm ayarlarını yükler.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilerle bir modeli eğtirecek şekilde form tanıyıcı örnek etiketleme aracının nasıl kullanılacağını öğrendiniz. Etiketleme aracını kendi uygulamanızla tümleştirmek isterseniz etiketli veri eğitimi ile ilgilenen REST API 'Lerini kullanın.

> [!div class="nextstepaction"]
> [Python kullanarak etiketlerle eğitme](./python-labeled-data.md)
