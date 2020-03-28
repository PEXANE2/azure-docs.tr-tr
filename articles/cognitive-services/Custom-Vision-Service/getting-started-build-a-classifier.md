---
title: Quickstart Bir sınıflandırıcı oluşturun - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir resim sınıflandırma modeli oluşturmak için Custom Vision web sitesini nasıl kullanacağınızı öğreneceksiniz.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: anroth
ms.openlocfilehash: b664a586398e297a00ea9cd8fe68dc65e6ade5c8
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170023"
---
# <a name="quickstart-how-to-build-a-classifier-with-custom-vision"></a>Quickstart: Özel Vizyon ile bir sınıflandırıcı nasıl inşa edile

Bu hızlı başlangıçta, Custom Vision web sitesi aracılığıyla nasıl bir sınıflandırıcı oluşturabileceğinizi öğreneceksiniz. Bir sınıflandırıcı modeli oluşturduğunuzda, görüntü sınıflandırması için Özel Görme hizmetini kullanabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Sınıflandırıcınızı eğitmek için bir dizi görüntü. Resim seçimiyle ilgili ipuçları için aşağıya bakın.

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Azure portalında Özel Vizyon kaynakları oluşturun

[!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Web tarayıcınızda, Özel [Vizyon web sayfasına](https://customvision.ai) gidin ve __Oturum Aç'ı__seçin. Azure portalında oturum açarken kullandığınız hesapla oturum açın.

![Oturum açma sayfasının görüntüsü](./media/browser-home.png)


1. İlk projenizi oluşturmak için **Yeni Proje'yi**seçin. **Yeni proje** oluştur iletişim kutusu görüntülenir.

    ![Yeni proje iletişim kutusunda ad, açıklama ve etki alanları vardır.](./media/getting-started-build-a-classifier/new-project.png)

1. Proje için bir ad ve açıklama girin. Ardından bir Kaynak Grubu seçin. Oturum açmış hesabınız bir Azure hesabıyla ilişkiliyse, Kaynak Grubu açılır bölümü Özel Vizyon Hizmeti Kaynağı içeren tüm Azure Kaynak Gruplarınızı görüntüler. 

   > [!NOTE]
   > Kullanılabilir kaynak grubu yoksa, lütfen [Azure portalında](https://portal.azure.com/)oturum açmak için kullandığınız hesapla aynı hesapla [customvision.ai](https://customvision.ai) oturum açtığınızı onaylayın. Ayrıca, lütfen Özel Görme portalında, Özel Görme kaynaklarınızın bulunduğu Azure portalındaki dizin le aynı "Dizin"i seçtiğinizi onaylayın. Her iki sitede de, dizininizin ekranın sağ üst köşesindeki açılan hesap menüsünden seçebilirsiniz. 

1. Proje __Türlerine__ Göre Sınıflandırma __Seçin.__ Daha sonra, __Sınıflandırma Türleri__altında, kullanım durumunuza bağlı olarak **Çok Etiketli** veya **Çok Sınıflı'yı**seçin. Çok etiketli sınıflandırma, bir görüntüye (sıfır veya daha fazla) etiketlerinizi herhangi bir sayıda uygularken, çok sınıflı sınıflandırma görüntüleri tek bir kategoriye ayırır (gönderdiğiniz her resim en olası etikete sıralanır). İsterseniz sınıflandırma türünü daha sonra değiştirebilirsiniz.

1. Ardından, kullanılabilir etki alanlarından birini seçin. Her etki alanı, aşağıdaki tabloda açıklandığı gibi, belirli görüntü türleri için sınıflandırıcıyı optimize eder. İsterseniz etki alanını daha sonra değiştirebilirsiniz.

    |Domain|Amaç|
    |---|---|
    |__Genel__| Çok çeşitli görüntü sınıflandırma görevleri için optimize edin. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanını seçeceğiniz den emin değilseniz, Genel etki alanını seçin. |
    |__Gıda__|Bir restoran menüsünde görmek gibi yemeklerin fotoğrafları için optimize edin. Tek tek meyve veya sebzelerin fotoğraflarını sınıflandırmak istiyorsanız, Gıda alan adını kullanın.|
    |__Yer işaretleri__|Hem doğal hem de yapay olarak tanınabilir yerler için optimize edin. Bu etki alanı en iyi, simge sel fotoğrafta açıkça görüldüğünde çalışır. Bu etki alanı, önündeki kişiler tarafından biraz engellenmiş olsa bile çalışır.|
    |__Perakende__|Bir alışveriş kataloğunda veya alışveriş web sitesinde bulunan resimler için optimize edin. Elbiseler, pantolonlar ve gömlekler arasında yüksek hassasiyetle sınıflandırmak istiyorsanız, bu alanı kullanın.|
    |__Kompakt etki alanları__| Mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için optimize edin. Kompakt etki alanları tarafından oluşturulan modeller yerel olarak çalıştırmak için dışa aktarılabilir.|

1. Son olarak, __proje oluştur'u__seçin.

## <a name="choose-training-images"></a>Eğitim resimlerini seçin

[!INCLUDE [choose training images](includes/choose-training-images.md)]

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu bölümde, sınıflandırıcıyı eğitmeye yardımcı olmak için görüntüleri yükler ve el ile etiketlersiniz. 

1. Resim eklemek için __resim ekle__ düğmesini tıklatın ve ardından __yerel dosyalara gözat'ı__seçin. Etiketlemeye geçmek için __Aç'ı__ seçin. Etiket seçiminiz, yüklemeyi seçtiğiniz tüm resim grubuna uygulanır, böylece görüntüleri istedikleri etiketlere göre ayrı gruplara yüklemek daha kolaydır. Tek tek resimlerin etiketlerini yüklendikten sonra da değiştirebilirsiniz.

    ![Görüntü ekleme denetimi sol üstte ve sol altta bir düğme olarak gösterilir.](./media/getting-started-build-a-classifier/add-images01.png)


1. Etiket oluşturmak için __Etiketlerim__ alanına metin girin ve Enter tuşuna basın. Etiket zaten varsa, açılır menüde görünür. Çok etiketli bir projede, resimlerinize birden fazla etiket ekleyebilirsiniz, ancak çok sınıflı bir projede yalnızca bir etiket ekleyebilirsiniz. Resimleri yüklemeyi bitirmek için __Yükle [numara] dosya__ larını kullanın. 

    ![Etiket ve yükleme sayfasının görüntüsü](./media/getting-started-build-a-classifier/add-images03.png)

1. Resimler yüklendikten sonra __Bitti'yi__ seçin.

    ![İlerleme çubuğu tamamlanan tüm görevleri gösterir.](./media/getting-started-build-a-classifier/add-images04.png)

Başka bir resim kümesi yüklemek için bu bölümün üst kısmına dönün ve adımları yineleyin.

## <a name="train-the-classifier"></a>Sınıflandırıcıyı eğitme

Sınıflandırıcıyı eğitmek için **Tren** düğmesini seçin. Sınıflandırıcı, her etiketin görsel niteliklerini tanımlayan bir model oluşturmak için geçerli görüntülerin tümlerini kullanır.

![Web sayfasının üstbilgi araç çubuğunun sağ üst kısmındaki tren düğmesi](./media/getting-started-build-a-classifier/train01.png)

Eğitim süreci sadece birkaç dakika sürer. Bu süre zarfında, eğitim süreci yle ilgili bilgiler **Performans** sekmesinde görüntülenir.

![Ana bölümde bir eğitim iletişim kutusu bulunan tarayıcı penceresi](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>Sınıflandırıcıyı değerlendirin

Eğitim tamamlandıktan sonra, modelin performansı tahmin edilir ve görüntülenir. Özel Görme Hizmeti, [k-fold çapraz doğrulama](https://en.wikipedia.org/wiki/Cross-validation_(statistics))adı verilen bir işlem kullanarak, hassaslık ve hatırlama hesaplamak için eğitim için gönderdiğiniz görüntüleri kullanır. Hassaslık ve geri çağırma, bir sınıflandırıcının etkinliğinin iki farklı ölçümütür:

- **Kesinlik,** tanımlanan sınıflandırmaların doğru olan kısmını gösterir. Örneğin, model köpek olarak 100 görüntü tespit ve bunların 99 aslında köpekler, o zaman hassas% 99 olacaktır.
- **Geri çağırma,** doğru tanımlanmış gerçek sınıflandırmaların kesirlerini gösterir. Örneğin, aslında 100 adet elma görüntüsü olsaydı ve model 80'i elma olarak tanımlasaydı, geri çağırma %80 olurdu.

![Eğitim sonuçları, sınıflandırıcıdaki her etiket için genel hassasiyeti ve geri çağırmayı ve hassasiyeti ve geri çağırmayı gösterir.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>Olasılık Eşiği

[!INCLUDE [probability threshold](includes/probability-threshold.md)]

## <a name="manage-training-iterations"></a>Eğitim yinelemelerini yönetme

Sınıflandırıcınızı her eğittiğınızda, kendi güncelleştirilmiş performans ölçümleriyle yeni bir _yineleme_ oluşturursunuz. Tüm yinelemelerinizi **Performans** sekmesinin sol bölmesinde görüntüleyebilirsiniz. Ayrıca, eskimişse bir yinelemeyi silmek için kullanabileceğiniz **Sil** düğmesini de bulabilirsiniz. Bir yinelemeyi sildiğinizde, onunla benzersiz olarak ilişkili tüm görüntüleri silersiniz.

Bkz. Eğitimli modellerinize programlı olarak nasıl eriştiğinizi öğrenmek için [modelinizi tahmin API'sıyla kullanın.](./use-prediction-api.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Custom Vision web sitesini kullanarak bir görüntü sınıflandırma modeli oluşturmayı ve nasıl eğitinip eğitin. Ardından, modelinizi geliştirme işlemi hakkında daha fazla bilgi alın.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)

