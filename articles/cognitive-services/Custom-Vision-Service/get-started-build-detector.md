---
title: Quickstart Nesne dedektörü oluşturma - Özel Görüş Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir resim sınıflandırma modeli oluşturmak için Custom Vision web sitesini nasıl kullanacağınızı öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: ba121400368f37c4a562a9c34e209c59d15b173c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81404103"
---
# <a name="quickstart-how-to-build-an-object-detector-with-custom-vision"></a>Quickstart: Özel Vizyon ile nesne dedektörü nasıl inşa edilebilen

Bu hızlı başlangıçta, Custom Vision web sitesi aracılığıyla nesne dedektörü oluşturmayı öğreneceksiniz. Bir dedektör modeli oluşturduğunuzda, nesne algılama için Özel Görme hizmetini kullanabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Dedektör modelinizi eğitmek için bir dizi görüntü. [Örnek resim](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/vision/images) kümesini GitHub'da kullanabilirsiniz. Veya aşağıdaki ipuçlarını kullanarak kendi resimlerinizi seçebilirsiniz.

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Azure portalında Özel Vizyon kaynakları oluşturun

[!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Web tarayıcınızda, Özel [Vizyon web sayfasına](https://customvision.ai) gidin ve __Oturum Aç'ı__seçin. Azure portalında oturum açarken kullandığınız hesapla oturum açın.

![Oturum açma sayfasının görüntüsü](./media/browser-home.png)


1. İlk projenizi oluşturmak için **Yeni Proje'yi**seçin. **Yeni proje** oluştur iletişim kutusu görüntülenir.

    ![Yeni proje iletişim kutusunda ad, açıklama ve etki alanları vardır.](./media/get-started-build-detector/new-project.png)

1. Proje için bir ad ve açıklama girin. Ardından bir Kaynak Grubu seçin. Oturum açmış hesabınız bir Azure hesabıyla ilişkiliyse, Kaynak Grubu açılır bölümü Özel Vizyon Hizmeti Kaynağı içeren tüm Azure Kaynak Gruplarınızı görüntüler. 

   > [!NOTE]
   > Kullanılabilir kaynak grubu yoksa, lütfen [Azure portalında](https://portal.azure.com/)oturum açmak için kullandığınız hesapla aynı hesapla [customvision.ai](https://customvision.ai) oturum açtığınızı onaylayın. Ayrıca, lütfen Özel Görme portalında, Özel Görme kaynaklarınızın bulunduğu Azure portalındaki dizin le aynı "Dizin"i seçtiğinizi onaylayın. Her iki sitede de, dizininizin ekranın sağ üst köşesindeki açılan hesap menüsünden seçebilirsiniz. 

1. Proje Türleri altında __Nesne__ __Algılama'yı__seçin.

1. Ardından, kullanılabilir etki alanlarından birini seçin. Her etki alanı, aşağıdaki tabloda açıklandığı gibi, belirli görüntü türleri için dedektörü optimize eder. İsterseniz etki alanını daha sonra değiştirebilirsiniz.

    |Domain|Amaç|
    |---|---|
    |__Genel__| Çok çeşitli nesne algılama görevleri için optimize edin. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanını seçeceğiniz den emin değilseniz, Genel etki alanını seçin. |
    |__Logo__|Görüntülerde marka logolarını bulmak için optimize edin.|
    |__Kompakt etki alanları__| Mobil cihazlarda gerçek zamanlı nesne algılama kısıtlamaları için optimize edin. Kompakt etki alanları tarafından oluşturulan modeller yerel olarak çalıştırmak için dışa aktarılabilir.|

1. Son olarak, __proje oluştur'u__seçin.

## <a name="choose-training-images"></a>Eğitim resimlerini seçin

[!INCLUDE [choose training images](includes/choose-training-images.md)]

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu bölümde dedektörü eğitmeye yardımcı olmak için görüntüleri yükleyecek ve elle etiketleyeceksiniz. 

1. Resim eklemek için __resim ekle__ düğmesini tıklatın ve ardından __yerel dosyalara gözat'ı__seçin. Resimleri yüklemek için __Aç'ı__ seçin.

    ![Görüntü ekleme denetimi sol üstte ve sol altta bir düğme olarak gösterilir.](./media/get-started-build-detector/add-images.png)

1. Yüklenen resimlerinizi UI'nin **Etiketlenmemiş** bölümünde görürsünüz. Bir sonraki adım, dedektörün tanımayı öğrenmesini istediğiniz nesneleri el ile etiketlemektir. Etiketleme iletişim penceresini açmak için ilk resmi tıklatın. 

    ![Etiketlenmemiş bölümde yüklenen resimler](./media/get-started-build-detector/images-untagged.png)

1. Resminizdeki nesnenin etrafındaki dikdörtgeni tıklatın ve sürükleyin. Ardından, **+** düğmeyle yeni bir etiket adı girin veya açılır listeden varolan bir etiket seçin. Dedektör eğitimde olumsuz bir örnek olarak etiketlenmemiş arka plan alanını kullandığından, algılamak istediğiniz nesnenin her örneğini etiketlemek çok önemlidir. Etiketlemeyi bitirdiğinizde, etiketlerinizi kaydetmek ve bir sonraki resme geçmek için sağdaki oku tıklatın.

    ![Nesneyi dikdörtgen seçimle etiketleme](./media/get-started-build-detector/image-tagging.png)

Başka bir resim kümesi yüklemek için bu bölümün üst kısmına dönün ve adımları yineleyin.

## <a name="train-the-detector"></a>Dedektörü eğitin

Dedektör modelini eğitmek için **Tren** düğmesini seçin. Dedektör, etiketlenen her nesneyi tanımlayan bir model oluşturmak için geçerli görüntülerin ve etiketlerinin tümünü kullanır.

![Web sayfasının üstbilgi araç çubuğunun sağ üst kısmındaki tren düğmesi](./media/getting-started-build-a-classifier/train01.png)

Eğitim süreci sadece birkaç dakika sürer. Bu süre zarfında, eğitim süreci yle ilgili bilgiler **Performans** sekmesinde görüntülenir.

![Ana bölümde bir eğitim iletişim kutusu bulunan tarayıcı penceresi](./media/get-started-build-detector/training.png)

## <a name="evaluate-the-detector"></a>Dedektörü değerlendirin

Eğitim tamamlandıktan sonra, modelin performansı hesaplanır ve görüntülenir. Custom Vision hizmeti, hassaslık, hatırlama ve ortalama ortalama hassasiyeti hesaplamak için eğitim için gönderdiğiniz görüntüleri kullanır. Hassaslık ve geri çağırma, dedektörün etkinliğinin iki farklı ölçümütür:

- **Kesinlik,** tanımlanan sınıflandırmaların doğru olan kısmını gösterir. Örneğin, model köpek olarak 100 görüntü tespit ve bunların 99 aslında köpekler, o zaman hassas% 99 olacaktır.
- **Geri çağırma,** doğru tanımlanmış gerçek sınıflandırmaların kesirlerini gösterir. Örneğin, aslında 100 adet elma görüntüsü olsaydı ve model 80'i elma olarak tanımlasaydı, geri çağırma %80 olurdu.

![Eğitim sonuçları genel hassasiyeti ve geri çağırmayı ve ortalama ortalama hassasiyeti gösterir.](./media/get-started-build-detector/trained-performance.png)

### <a name="probability-threshold"></a>Olasılık Eşiği

[!INCLUDE [probability threshold](includes/probability-threshold.md)]

## <a name="manage-training-iterations"></a>Eğitim yinelemelerini yönetme

Dedektörünüzü her eğittiğınızda, kendi güncelleştirilmiş performans ölçümleriyle yeni bir _yineleme_ oluşturursunuz. Tüm yinelemelerinizi **Performans** sekmesinin sol bölmesinde görüntüleyebilirsiniz. Sol bölmede, eskimişse bir yinelemeyi silmek için kullanabileceğiniz **Sil** düğmesini de bulacaksınız. Bir yinelemeyi sildiğinizde, onunla benzersiz olarak ilişkili tüm görüntüleri silersiniz.

Bkz. Eğitimli modellerinize programlı olarak nasıl eriştiğinizi öğrenmek için [modelinizi tahmin API'sıyla kullanın.](./use-prediction-api.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Custom Vision web sitesini kullanarak bir nesne dedektörü modeli oluşturmayı ve nasıl eğitileceğinizi öğrendiniz. Ardından, modelinizi geliştirme işlemi hakkında daha fazla bilgi alın.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)

