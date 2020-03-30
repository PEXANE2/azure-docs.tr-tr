---
title: 'Öğretici: Tasarımcı ile otomobil fiyatı tahmin'
titleSuffix: Azure Machine Learning
description: Sürükle ve bırak arabirimini kullanarak bir makine öğrenme modelini nasıl eğitecek, puanlandırın ve dağıtın. Bu öğretici doğrusal regresyon kullanarak otomobil fiyatları tahmin iki bölümlük bir dizi parçasıdır.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 03/12/2020
ms.openlocfilehash: 0488002352d222abb0166737f9a042060b1a1bb1
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389435"
---
# <a name="tutorial-predict-automobile-price-with-the-designer-preview"></a>Öğretici: Tasarımcı ile otomobil fiyatı tahmin (önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu iki bölümlü eğitimde, herhangi bir otomobilin fiyatını tahmin eden bir makine öğrenimi modelini eğitmek ve dağıtmak için Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğrenirsiniz. Tasarımcı, tek bir kod satırı olmadan makine öğrenme modelleri oluşturmanıza olanak tanıyan bir sürükle ve bırak aracıdır.

Öğreticinin birinci bölümünde, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir ardışık hat lar oluşturun.
> * Veri aktarın.
> * Veri hazırlayın.
> * Bir makine öğrenme modeli eğitin.
> * Bir makine öğrenme modelini değerlendirin.

Öğreticinin [ikinci bölümünde,](tutorial-designer-automobile-price-deploy.md) gönderdiğiniz teknik özelliklere göre herhangi bir aracın fiyatını tahmin etmek için modelinizi gerçek zamanlı bir çıkarım bitiş noktası olarak dağıtacaksınız. 

> [!NOTE]
>Bu öğreticinin tamamlanmış bir sürümü örnek bir ardışık işlem olarak kullanılabilir.
>
>Bulmak için çalışma alanınızdaki tasarımcıya gidin. Yeni **boru hattı** bölümünde Örnek **1 - Regresyon: Otomobil Fiyat Tahmini(Temel)** seçeneğini belirleyin.

## <a name="create-a-new-pipeline"></a>Yeni bir ardışık hat lar oluşturma

Azure Machine Learning ardışık hatları, tek bir kaynakta birden çok makine öğrenimi ve veri işleme adımı düzenler. Ardışık işler, projeler ve kullanıcılar arasında karmaşık makine öğrenimi iş akışlarını düzenlemenize, yönetmenize ve yeniden kullanmanıza izin verir.

Azure Machine Learning ardışık hattı oluşturmak için bir Azure Machine Learning çalışma alanına ihtiyacınız vardır. Bu bölümde, bu kaynakların her ikisini de nasıl oluşturabileceğinizi öğrenirsiniz.

### <a name="create-a-new-workspace"></a>Yeni çalışma alanı oluşturma

Tasarımcıyı kullanmak için öncelikle bir Azure Machine Learning çalışma alanına ihtiyacınız vardır. Çalışma alanı, Azure Machine Learning için en üst düzey kaynaktır ve Azure Machine Learning'de oluşturduğunuz tüm yapılarla çalışmak için merkezi bir yer sağlar.

Kurumsal sürümü olan bir Azure Machine Learning çalışma alanınız varsa, [bir sonraki bölüme atlayın.](#create-the-pipeline)

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal-enterprise.md)]

### <a name="create-the-pipeline"></a>İşlem hattını oluşturma

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a>oturum açın ve çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı'yı**seçin.

    ![Tasarımcıya nasıl erişilir gösteren görsel çalışma alanının ekran görüntüsü](./media/tutorial-designer-automobile-price-train-score/launch-designer.png)

1. **Kullanımı kolay önceden önceden modülleri**seçin.

1. Tuvalin üst kısmında, varsayılan ardışık **ardışık ad Pipeline-Created-on'u**seçin. Otomobil fiyat *tahmini*için yeniden adlandırın. Adın benzersiz olması gerekmez.

## <a name="set-the-default-compute-target"></a>Varsayılan işlem hedefini ayarlama

Bir ardışık işlem hedefi üzerinde çalışır, bu da çalışma alanınıza bağlı bir bilgi işlem kaynağıdır. Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için yeniden kullanabilirsiniz.

Tüm ardışık hatlar için varsayılan **bilgi işlem hedefi** ayarlayabilirsiniz, bu da her modüle varsayılan olarak aynı işlem hedefini kullanmasını söyler. Ancak, modül başına işlem hedeflerini belirtebilirsiniz.

1. Ardışık alan adının yanında, **Ayarlar** bölmesini](./media/tutorial-designer-automobile-price-train-score/gear-icon.png) açmak için tuvalin üst kısmındaki dişli simgesinin Dişli **simgesiekran** ![görüntüsünü seçin.

1. Tuvalin sağındaki **Ayarlar** bölmesinde, **bilgi işlem hedefini seçin'i**seçin.

    Zaten kullanılabilir bir işlem hedefiniz varsa, bu ardışık alanı çalıştırmak için bu hedefi seçebilirsiniz.

    > [!NOTE]
    > Tasarımcı yalnızca Azure Machine Learning Compute hedefleri üzerinde denemeler çalıştırabilir. Diğer hesaplama hedefleri gösterilmez.

1. Bilgi işlem kaynağı için bir ad girin.

1. **Kaydet'i**seçin.

    > [!NOTE]
    > Bir bilgi işlem kaynağı oluşturmak yaklaşık beş dakika sürer. Kaynak oluşturulduktan sonra, kaynağı yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > İşlem kaynağı, maliyetten tasarruf etmek için boşta yken sıfır düğüme otomatik ölçeklendirir. Bir gecikmeden sonra tekrar kullandığınızda, yeniden ölçeklendirilirken yaklaşık beş dakikalık bekleme süresi yaşayabilirsiniz.

## <a name="import-data"></a>Veri içeri aktarma

Denemeniz için tasarımcıda bulunan birkaç örnek veri kümesi vardır. Bu öğretici için **Otomobil fiyat verilerini (Raw)** kullanın. 

1. Boru hattı tuvalinin solunda veri kümeleri ve modüllerden oluşan bir palet vardır. **Veri kümelerini**seçin ve ardından kullanılabilir örnek veri kümelerini görüntülemek için **Örnekler** bölümünü görüntüleyin.

1. Veri kümesi **Otomobil fiyat verilerini (Raw)** seçin ve tuval üzerine sürükleyin.

   ![Verileri tuvale sürükleyin](./media/tutorial-designer-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Verileri görselleştirme

Kullanacağınız veri kümesini anlamak için verileri görselleştirebilirsiniz.

1. Otomobil **fiyat verileri (Raw)** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde **Çıktılar + log'u**seçin.

1. Verileri görselleştirmek için grafik simgesini seçin.

    ![Verileri görselleştirme](./media/tutorial-designer-automobile-price-train-score/visualize-data.png)

1. Her biri hakkındaki bilgileri görüntülemek için veri penceresindeki farklı sütunları seçin.

    Her satır bir otomobili temsil eder ve her otomobille ilişkili değişkenler sütun olarak görünür. Bu veri kümesinde 205 satır ve 26 sütun vardır.

## <a name="prepare-data"></a>Verileri hazırlama

Veri kümeleri genellikle çözümlemeden önce bazı ön işleme gerektirir. Veri kümesini incelerken bazı eksik değerleri fark etmiş olabilirsiniz. Bu eksik değerler, modelin verileri doğru şekilde çözümleyebilmeleri için temizlenmelidir.

### <a name="remove-a-column"></a>Sütun kaldırma

Bir model eğitdiğinizde, eksik olan veriler le ilgili bir şeyler yapmanız gerekir. Bu veri kümesinde, **normalleştirilmiş kayıplar** sütunu birçok değer eksikolduğundan, bu sütunu modelden tamamen dışlarsınız.

1. Tuvalin solundaki modül paletinde **Veri Dönüşümü** bölümünü genişletin ve **Dataset modülündeki Sütunları Seç'i** bulun.

1. **Dataset modülündeki Sütunları Seç'i** tuvale sürükleyin. Modülü veri kümesi modülünün altına bırakın.

1. Otomobil **fiyat verilerini (Raw)** veri kümesini **Dataset modülündeki Sütunları Seç** modülüne bağlayın. Kanvas üzerindeki veri kümesinin altındaki küçük daire olan veri kümesinin çıkış bağlantı noktasından, modülün üst kısmındaki küçük daire olan **Dataset'teki Sütunları Seç**giriş noktasına sürükleyin.

    > [!TIP]
    > Bir modülün çıkış bağlantı noktasını başka bir modülün giriş bağlantı noktasına bağladığınızda, ardışık noktanız aracılığıyla bir veri akışı oluşturursunuz.
    >

    ![Modülleri bağlama](./media/tutorial-designer-automobile-price-train-score/connect-modules.gif)

1. **Dataset modülünde Sütunları Seç'i** seçin.

1. Tuvalin sağındaki modül ayrıntıları **bölmesinde, Sütunu Edit'i**seçin.

1. Sütun **adlarını** **Ekle'nin**yanına bırak ve **Tüm sütunları**seçin.

1. Yeni **+** bir kural eklemek için seçin.

1. Açılan menülerden **Dışla** ve **Sütun adlarını**seçin.
    
1. Metin kutusuna *normalleştirilmiş kayıpları* girin.

1. Sağ altta, sütun seçiciyi kapatmak için **Kaydet'i** seçin.

    ![Bir sütunu hariç tut](./media/tutorial-designer-automobile-price-train-score/exclude-column.png)

1. **Dataset modülünde Sütunları Seç'i** seçin. 

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, **Yorum** metin kutusunu seçin ve *normalleştirilmiş kayıpları dışla*girin.

    Ardışık hattınızı düzenlemenize yardımcı olmak için grafikte yorumlar görünür.

### <a name="clean-missing-data"></a>Eksik verileri temizleme

**Normalleştirilmiş kayıplar** sütununu kaldırdıktan sonra veri kümenizhala eksik değerlere sahiptir. **Eksik Verileri Temizle** modüllerini kullanarak kalan eksik verileri kaldırabilirsiniz.

> [!TIP]
> Giriş verilerinden eksik değerleri temizlemek, modüllerin çoğunu tasarımcıda kullanmak için bir ön koşuldur.

1. Tuvalin solundaki modül paletinde Veri **Dönüşümü**bölümünü genişletin ve **Temiz Eksik Veri** modüllerini bulun.

1. Temiz **Eksik Veri** modüllerini boru hattı tuvaline sürükleyin. Veri kümesi **modülündeki Sütunları Seç'e** bağlayın. 

1. Eksik **Verileri Temizle** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde **Sütunu Edit'i**seçin.

1. Görünen **temizlenecek** sütunlarda açılan menüyü **Ekle'ye**genişletin. Seçin, **Tüm sütunlar**

1. **Kaydet**’i seçin

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, Temizleme modu altında tüm satırı **kaldır'ı**seçin. **Remove entire row**

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, **Yorum** kutusunu seçin ve *eksik değer satırlarını kaldır'ı*girin. 

    Boru hattınız şimdi şuna benzer:
    
    ![Sütun seçin](./media/tutorial-designer-automobile-price-train-score/pipeline-clean.png)

## <a name="train-a-machine-learning-model"></a>Bir makine öğrenme modeli eğitin

Artık verileri işlemek için modülleri yerleştirdiğinize göre, eğitim modüllerini ayarlayabilirsiniz.

Bir sayı olan fiyatı tahmin etmek istediğinizden, bir regresyon algoritması kullanabilirsiniz. Bu örnekiçin, doğrusal bir regresyon modeli kullanın.

### <a name="split-the-data"></a>Verileri bölme

Verileri bölme, makine öğreniminde yaygın bir görevdir. Verilerinizi iki ayrı veri kümesine bölersiniz. Bir veri kümesi modeli eğitecek ve diğeri modelin ne kadar iyi performans gösterdiğini test edecektir.

1. Modül paletinde, Veri **Dönüşümü** bölümünü genişletin ve **Bölünmüş Veri** modüllerini bulun.

1. Bölünmüş **Veri** modüllerini boru hattı tuvaline sürükleyin.

1. **Temiz Eksik Veri** modülünün sol bağlantı noktasını Bölünmüş **Veri** modülüne bağlayın.

    > [!IMPORTANT]
    > **Temiz Eksik Verilerin** sol çıktı bağlantı noktalarının Split **Data'ya**bağlandığından emin olun. Sol bağlantı noktası temizlenen verileri içerir. Sağ bağlantı noktası, diske edilmiş verileri içerir.

1. Verileri **Böl** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları **bölmesinde, ilk çıktı veri kümesindeki satırların** Fraksiyonu'nu 0,7 olarak ayarlayın.

    Bu seçenek, modeli eğitmek için verilerin yüzde 70'ini ve test etmek için yüzde 30'unu böler. Yüzde 70 veri kümesine sol çıkış bağlantı noktasından erişilebilir. Kalan veriler doğru çıkış bağlantı noktasından kullanılabilir.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, **Yorum** kutusunu seçin ve *veri kümesini eğitim kümesine (0,7) ve test kümesine (0,3) bölün'ü*girin.

### <a name="train-the-model"></a>Modeli eğitme

Modeli fiyat içeren bir veri seti vererek eğitin. Algoritma, eğitim verilerinin sunduğu özellikler ve fiyat arasındaki ilişkiyi açıklayan bir model oluşturuyor.

1. Modül paletinde, **Machine Learning Algoritmaları'nı**genişletin.
    
    Bu seçenek, öğrenme algoritmalarını başlatmayı kullanmak için kullanabileceğiniz çeşitli modül kategorileri görüntüler.

1. **Regresyon** > **Lineer Regresyon'u**seçin ve boru hattı tuvaline sürükleyin.

1. **Lineer Regresyon** modülünün çıktısını **Tren Modeli** modülünün sol girişine bağlayın.

1. Modül paletinde, bölüm **Modülü eğitimini**genişletin ve **Tren Modeli** modüllerini tuvale sürükleyin.

1. Tren **Modeli** modülünü seçin ve boru hattı tuvaline sürükleyin.

1. **Bölünmüş Veri** modülündeki eğitim veri çıktısını (sol bağlantı noktası) **Tren Modeli** modülünün sağ girişine bağlayın.
    
    > [!IMPORTANT]
    > **Split Data'nın** sol çıkış bağlantı noktalarının **Train Model'e**bağlandığından emin olun. Sol bağlantı noktası eğitim kümesini içerir. Sağ bağlantı noktası test kümesini içerir.

    ![Tren Modeli modülünün doğru yapılandırmasını gösteren ekran görüntüsü. Lineer Regresyon modülü Tren Modeli modülünün sol portuna, Bölünmüş Veri modülü ise Tren Modeli'nin sağ limanına bağlanır](./media/tutorial-designer-automobile-price-train-score/pipeline-train-model.png)

1. Tren **Modeli** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde sütun seçiciyi **edin'i** seçin.

1. Etiket **sütunu** iletişim kutusunda, açılır menüyü genişletin ve **Sütun adlarını**seçin. 

1. Metin kutusuna, modelinizin tahmin edeceğini belirtmek için *fiyat* girin.

    >[!IMPORTANT]
    > Sütun adını tam olarak girdiğinizi unutmayın. **Fiyatı**sermayelendirmeyin. 

    Boru hattınız şu na benzemelidir:

    ![Tren Modeli modülünün eklenmesinden sonra boru hattının doğru yapılandırmasını gösteren ekran görüntüsü.](./media/tutorial-designer-automobile-price-train-score/pipeline-train-graph.png)

### <a name="add-the-score-model-module"></a>Puan Modeli modülekle

Verilerinizin yüzde 70'ini kullanarak modelinizi eğittinden sonra, modelinizin ne kadar iyi çalıştığını görmek için diğer yüzde 30'unu puanlamak için bu modeli kullanabilirsiniz.

1. **Puan Modeli** modüllerini bulmak için arama kutusuna *puan modelini* girin. Modülü boru hattı tuvaline sürükleyin. 

1. **Modeli Eğitme** modülünün çıkışını **Model Puanlama** modülünün sol giriş bağlantı noktasına bağlayın. **Verileri Bölme** modülünün test verileri çıkışını (sağ bağlantı noktası) **Model Puanlama** modülünün sağ giriş bağlantı noktasına bağlayın.

### <a name="add-the-evaluate-model-module"></a>Model'i Değerlendir modülekle

Modelinizin test veri kümesini ne kadar iyi puanladığını değerlendirmek için **Model'i Değerlendir** modüllerini kullanın.

1. **Modeli Değerlendir** modüllerini bulmak için arama kutusuna *değerlendir'i* girin. Modülü boru hattı tuvaline sürükleyin. 

1. **Puan Modeli** modülünün çıktısını **Değerlendir Modeli'nin**sol girişine bağlayın. 

    Son boru hattı şuna benzer:

    ![Ardışık yapının doğru yapılandırmasını gösteren ekran görüntüsü.](./media/tutorial-designer-automobile-price-train-score/pipeline-final-graph.png)

## <a name="submit-the-pipeline"></a>Boru hattını gönderme

Artık boru hattınız tamamen kurulum olduğuna göre, makine öğrenimi modelinizi eğitmek için bir boru hattı çalıştırabilirsiniz. Geliştirme sırasında ardışık hatlar için yapılan değişiklikleri gözden geçirmek için kullanılabilecek geçerli bir ardışık hatlar hattı çalıştırabilirsiniz.

1. Tuvalin üst kısmında **Gönder'i**seçin.

1. **Ardışık hatlar dizini çalıştır** iletişim kutusunu ayarla'da **yeni oluştur'u**seçin.

    > [!NOTE]
    > Denemeler grubu benzer boru hattı birlikte çalışır. Bir ardışık ardışık çalıştırma için aynı denemeyi birden çok kez çalıştırın.

    1. **Yeni deneme adı**için açıklayıcı bir ad girin.

    1. **Gönder**’i seçin.
    
    Tuvalin sağ üst kısmında ki çalışma durumunu ve ayrıntılarını görüntüleyebilirsiniz.
    
    İlk çalıştırma ysa, ardışık alanınızın çalışmasını tamamlaması 20 dakika kadar sürebilir. Varsayılan işlem ayarlarının en az düğüm boyutu 0'dır, bu da tasarımcının boşta kaldıktan sonra kaynak ayırması gerektiği anlamına gelir. Yinelenen ardışık işlem çalışır işlem kaynakları zaten tahsis olduğundan daha az zaman alacaktır. Ayrıca, tasarımcı verimliliği daha da artırmak için her modül için önbelleğe alınmış sonuçlar kullanır.

### <a name="view-scored-labels"></a>Puanlı etiketleri görüntüleme

Çalıştırma tamamlandıktan sonra, ardışık hatlar çalışmasının sonuçlarını görüntüleyebilirsiniz. İlk olarak, regresyon modeli tarafından oluşturulan tahminlere bakın.

1. Çıktısını görüntülemek için **Puan Modeli** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, sonuçları görüntülemek için **Çıktılar + günlükler** > grafik simgesini ![görselleştir'i](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) seçin.

    Burada tahmin edilen fiyatları ve test verilerinden gerçek fiyatları görebilirsiniz.

    ![Puanlı Etiket sütununu vurgulayan çıktı görselleştirmesinin ekran görüntüsü](./media/tutorial-designer-automobile-price-train-score/score-result.png)

### <a name="evaluate-models"></a>Modelleri değerlendirin

Test veri kümesinde eğitimli modelin ne kadar iyi performans gösterdiğini görmek için **Modeli Değerlendir'i** kullanın.

1. Çıktısını görüntülemek için **Modeli Değerlendir** modülünü seçin.

1. Tuvalin sağındaki modül ayrıntıları bölmesinde, sonuçları görüntülemek için **Çıktılar + günlükler** > grafik simgesini ![görselleştir'i](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) seçin.

Modeliniz için aşağıdaki istatistikler gösterilir:

* **Ortalama Mutlak Hata (MAE)**: Mutlak hataların ortalaması. Hata, öngörülen değer le gerçek değer arasındaki farktır.
* **Kök Ortalama Kareli Hata (RMSE)**: Test veri kümesinde yapılan tahminlerin kareli hatalarının ortalamasının kare köküdür.
* **Relative Absolute Error (Göreli Mutlak Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak hataların mutlak farka göreli ortalaması.
* **Relative Squared Error (Göreli Karesi Alınmış Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış hataların karesi alınmış farka göreli ortalaması.
* **Belirleme Katsayısı**: R kare değeri olarak da bilinen bu istatistiksel ölçüm, bir modelin verilere ne kadar uyduğunu gösterir.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük bir değer, öngörülerin gerçek değerlere daha yakın olduğunu gösterir. Kararlılık katsayısı için, değeri bire (1,0) ne kadar yakınsa, tahminler o kadar iyi olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticinin [2.](tutorial-designer-automobile-price-deploy.md)

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

İkinci bölümde, modelinizi gerçek zamanlı bitiş noktası olarak nasıl dağıtacağınız öğrenilir.

> [!div class="nextstepaction"]
> [Modelleri dağıtmaya devam etme](tutorial-designer-automobile-price-deploy.md)
