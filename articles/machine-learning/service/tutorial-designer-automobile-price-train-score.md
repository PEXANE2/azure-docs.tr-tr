---
title: 'Öğretici: tasarımcı ile otomobil fiyatlarını tahmin etme'
titleSuffix: Azure Machine Learning
description: Bir sürükle ve bırak arabirimi kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma hakkında bilgi edinin. Bu öğretici, doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etmeye yönelik iki bölümlü bir serinin bir parçasıdır.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 8aafc1caf44e6a937fc53801f2fa34157dc9eab5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73501592"
---
# <a name="tutorial-predict-automobile-price-with-the-designer-preview"></a>Öğretici: tasarımcı ile otomobil fiyatını tahmin etme (Önizleme)
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Bu iki bölümden oluşan öğreticide, her bir otomobil fiyatını tahmin eden tahmine dayalı bir analiz çözümü geliştirmek ve dağıtmak için Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğreneceksiniz. 

Birinci bölümde, ortamınızı ayarlayın, modülleri etkileşimli bir tuvale sürükleyip bırakın ve bir Azure Machine Learning işlem hattı oluşturmak için bunları birbirine bağlayın.

Öğreticinin bir parçası olarak şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Yeni işlem hattı oluşturma
> * Veri içeri aktarma
> * Verileri hazırlama
> * Makine öğrenimi modelini eğitme
> * Machine Learning modelini değerlendirme

Öğreticinin [ikinci bölümünde](tutorial-designer-automobile-price-deploy.md) , size gönderilen teknik belirtimlere bağlı olarak herhangi bir arabasının fiyatını tahmin etmek için tahmine dayalı modelinizi gerçek zamanlı bir zaman aşımı noktası olarak dağıtmayı öğreneceksiniz. 

> [!Note]
>Bu öğreticinin tamamlanmış sürümü örnek bir işlem hattı olarak sunulmaktadır.
>
>Bulmak için **çalışma alanınızdaki tasarımcıya**gidin. Yeni işlem **hattı** bölümünde **Örnek 1-gerileme: otomobil fiyat tahmini (temel)** öğesini seçin.

## <a name="create-a-new-pipeline"></a>Yeni işlem hattı oluşturma

Azure Machine Learning işlem hatları, birden çok, bağımlı makine öğrenimi ve veri işleme adımlarını tek bir kaynakta düzenler. İşlem hatları, projeler ve kullanıcılar arasında karmaşık makine öğrenimi iş akışlarını düzenlemenize, yönetmenize ve yeniden kullanmanıza yardımcı olur. Azure Machine Learning bir işlem hattı oluşturmak için bir Azure Machine Learning çalışma alanına ihtiyacınız vardır. Bu bölümde, bu kaynakların her ikisini de oluşturmayı öğreneceksiniz.

### <a name="create-a-new-workspace"></a>Yeni çalışma alanı oluştur

**Enterprise sürümü**olan bir Azure Machine Learning çalışma alanınız varsa, [sonraki bölüme atlayın](#create-the-pipeline).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal-enterprise.md)]

### <a name="create-the-pipeline"></a>İşlem hattını oluşturma

1. [Ml.Azure.com](https://ml.azure.com) içinde oturum açın ve birlikte çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı**' yı seçin.

    ![Tasarımcıya nasıl erişediğinin gösterildiği görsel çalışma alanının ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/launch-visual-interface.png)

1. Kullanımı **kolay önceden oluşturulmuş modüller**' i seçin.

1. Tuvalin en üstündeki "işlem **hattı-oluşturuldu-on..."** varsayılan işlem hattı adını seçin ve anlamlı bir şekilde yeniden adlandırın. Örneğin, **"otomobil fiyat tahmini"** . Adın benzersiz olması gerekmez.

## <a name="import-data"></a>Veri içeri aktarma

Üzerinde denemeler yapmanız için tasarımcıya dahil edilen birkaç örnek veri kümesi vardır. Bu öğretici için, **otomobil veri kümesi (ham)** örnek veri kümesini kullanın. 

1. Ardışık düzen tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Kullanılabilir örnek veri kümelerini görüntülemek için **veri kümeleri** ' ni seçin ve **örnekler** bölümünü görüntüleyin.

1. Veri kümesini, **otomobil fiyat verilerini (ham)** seçin ve tuvale sürükleyin.

   ![Verileri tuvale sürükleyin](./media/ui-tutorial-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Verileri görselleştirme

Kullanacağınız veri kümesini anlamak için verileri görselleştirebilirsiniz.

1. **Otomobil fiyat verileri (ham)** modülünü seçin.

1. Tuvalin sağındaki **Özellikler** bölmesinde, **çıktılar**' i seçin.

1. Verileri görselleştirmek için grafik simgesini seçin.

    ![Verileri görselleştirme](./media/ui-tutorial-automobile-price-train-score/visualize-data.png)

1. Her biri hakkındaki bilgileri görüntülemek için veri penceresinde farklı sütunları seçin.

    Her satır bir otomobil ve her bir otomobil ile ilişkili değişkenler sütun olarak görünür. Bu veri kümesinde 205 satır ve 26 sütun vardır.

## <a name="prepare-data"></a>Verileri hazırlama

Veri kümeleri, analiz etmeden önce genellikle bazı ön işleme gerektirir. Veri kümesini görselleştirirken bazı eksik değerleri fark etmiş olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Birçok eksik değeri olan sütunları kaldırır ve eksik değerleri olan tüm satırları kaldırabilirsiniz.

### <a name="remove-a-column"></a>Sütun kaldırma

Bir modeli eğitedığınızda, eksik olan veriler hakkında bir şey yapmanız gerekir. Bu veri kümesinde, **normalleştirilmiş kayıplar** sütununda çok sayıda değer yok, bu nedenle bu sütunu modelden tamamen hariç tutacağız.

1. Hangi veri sütunlarının birlikte çalışabileceği seçin. **Veri kümesi modülünde sütunları seçme** modülünü bulmak için paletin en üstündeki arama kutusuna **seçin** yazın.

1. **Veri kümesindeki sütunları seçme** modülü ' ne tıklayın ve tuval üzerine sürükleyin. Modülün veri kümesi modülünün altına bırakın.

1. Daha önce eklediğiniz veri kümesini, tıklayıp sürükleyerek **veri kümesindeki sütunları seçme** modülüne bağlayın. Veri kümesinin çıkış bağlantı noktasından, tuvaldeki veri kümesinin en altında bulunan küçük daire, modülün en üstünde bulunan küçük bir daire olan **veri kümesindeki sütun seçme**giriş bağlantı noktasına sürükleyin.

    > [!TIP]
    > Bir modülün çıkış bağlantı noktasını başka bir giriş bağlantı noktasına bağladığınızda işlem hattınızla veri akışı oluşturursunuz.
    >

    ![Modülleri bağlama](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. **Veri kümesi modülünde sütunları seç '** i seçin.

1. Tuvalin sağ tarafındaki **Özellikler** bölmesinde **Parametreler** > **sütunu Düzenle**' yi seçin.

1. Yeni bir kural eklemek için **+** seçin.

1. Açılan menüden **Dışla** ve **sütun adlarını**seçin.
    
1. Metin kutusuna **normalleştirilmiş zararlar** girin.

1. Sütun seçiciyi kapatmak için sağ alt köşedeki **Kaydet** ' i seçin.

    ![Sütun dışlama](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Özellikler bölmesi, **normalleştirilmiş kayıplar** sütununun dışlanacağını gösterir.

1. **Veri kümesi modülünde sütunları seç '** i seçin. 

1. **Özellikler**' de, **Parametreler** > **Açıklama** ' yı seçin ve "normalleştirilmiş zararları hariç tut" yazın.

### <a name="clean-missing-data"></a>Eksik verileri temizle

Veri kümeniz, **normalleştirilmiş zararlar** sütununu kaldırdıktan sonra yine de eksik değerler içeriyor. **Eksik veri silme** modülünü kullanarak kalan eksik verileri kaldırabilirsiniz.

> [!TIP]
> Giriş verilerinden eksik değerleri Temizleme, tasarımcıda birçok modülün kullanılmasına yönelik bir önkoşuldur.

1. **Eksik verileri temizle** modülünü bulmak için arama kutusuna **Clean** yazın.

1. **Eksik verileri temizle** modülünü ardışık düzen tuvaline sürükleyin ve **veri kümesindeki sütunları seçme** modülüne bağlayın. 

1. Özellikler bölmesinde, **temizleme modu**' nun altındaki **tüm satırı Kaldır** ' ı seçin.

1. Özellikler bölmesinde "eksik değer satırlarını kaldır" yazın. **Açıklama** kutusunda.  

    İşlem hatlarınız şuna benzer şekilde görünmelidir:
    
    ![Select-Column](./media/ui-tutorial-automobile-price-train-score/pipeline-clean.png)

## <a name="train-a-machine-learning-model"></a>Makine öğrenimi modelini eğitme

Artık veriler önceden işlense de, tahmine dayalı bir model oluşturabilirsiniz. Modeli eğitebilmeniz için verilerinizi kullanacaksınız. Daha sonra, fiyatları tahmin etmeye ne kadar yakından başlayacaklarını görmek için modeli test edersiniz.

### <a name="select-an-algorithm"></a>Algoritmayı seçme

**Sınıflandırma** ve **regresyon**, denetimli makine öğrenimi algoritmasının iki türüdür. **Sınıflandırma** , renk (kırmızı, mavi veya yeşil) gibi tanımlı kategori kümesinden bir yanıtı tahmin eder. **Gerileme** , bir sayıyı tahmin etmek için kullanılır.

Bir sayı olan fiyatı tahmin etmek istediğiniz için, regresyon algoritmasını kullanabilirsiniz. Bu örnekte, doğrusal regresyon modeli kullanacaksınız.

### <a name="split-the-data"></a>Verileri bölme

Verileri iki ayrı veri kümesine bölerek modeli eğitim ve test etme için verilerinizi kullanın.

1. **Verileri Böl** modülünü bulmak için arama kutusuna **bölünmüş** verileri yazın ve **eksik verileri temizleme** modülünün sol bağlantı noktasına bağlayın.

1. **Bölünmüş veri** modülünü seçin.

1. Özellikler bölmesinde, **ilk çıktı veri kümesindeki satır kesirini** 0,7 olarak ayarlayın.

    Bu, modeli eğitmek için verilerin yüzde 70 ' ünü böler ve test için yüzde 30 ' u geri tutar.

1. Özellikler bölmesinde, "veri kümesini eğitim kümesine Böl (0.7) ve test kümesi (0,3)." yazın. **Açıklama** kutusunda.

### <a name="train-the-model"></a>Modeli eğitme

Fiyatı içeren bir veri kümesi vererek modeli eğitme. Model, verileri tarar ve bir araba 'nin özellikleri ile fiyatı arasında bağıntılar arar.

1. Öğrenme algoritmasını seçmek için modül paleti arama kutusunu temizleyin.

1. **Machine Learning algoritmaları**genişletin.
    
    Böylece makine öğrenimi algoritmalarını başlatmak için kullanılabilecek çeşitli modül kategorileri görüntülenir.

1. Bu işlem hattı için, **Doğrusal regresyon** > **gerileme** ' yı seçin ve işlem hattı tuvaline sürükleyin.

1. **Eğitim modeli** modülünü bulup ardışık düzen tuvaline sürükleyin. 

1. Doğrusal regresyon modülünün çıkışını eğitme modeli modülünün sol girişine bağlayın.

1. **Veri ayırma** modülünün eğitim verileri çıkışını (sol bağlantı noktası), **eğitme modeli** modülünün sağ girişine bağlayın.

    ![Eğitim modeli modülünün doğru yapılandırmasını gösteren ekran görüntüsü. Doğrusal regresyon modülü, model eğitimi modülünün sol bağlantı noktasına bağlanır ve bölünmüş veri modülü, tren modelinin sağ bağlantı noktasına bağlanır](./media/ui-tutorial-automobile-price-train-score/pipeline-train-model.png)

1. **Model eğitme** modülünü seçin.

1. Özellikler bölmesinde, **sütun seçiciyi Düzenle** ' yi seçin.

1. **Etiket sütunu** iletişim kutusunda, açılan menüyü genişletin ve **sütun adları**' nı seçin. Metin kutusuna **Price**yazın. Fiyat, modelinizin tahmin edilecek değerdir.

    İşlem hatlarınız şöyle görünmelidir:

    ![Eğitim modeli modülü eklendikten sonra işlem hattının doğru yapılandırmasını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/pipeline-train-graph.png)

## <a name="evaluate-a-machine-learning-model"></a>Machine Learning modelini değerlendirme

Artık, bu modeli verilerinizin yüzde 70 ' ünü kullanarak eğitildiniz, modelin ne kadar iyi olduğunu görmek için, verilerin yüzde 30 ' unu almak üzere onu kullanabilirsiniz.

1. **Puan modeli** modülünü bulmak için arama kutusuna **puan modeli** yazın ve modülü işlem hattı tuvaline sürükleyin. 

1. **Eğitim modeli** modülünün çıkışını, **puan modelinin**sol giriş bağlantı noktasına bağlayın. **Veri ayırma** modülünün test verileri çıkışını (sağ bağlantı noktası), **puan modelinin**sağ giriş bağlantı noktasına bağlayın.

1. **Modeli değerlendir** ' i bulmak için arama kutusuna **değerlendir** yazın ve modülü işlem hattı tuvaline sürükleyin. 

1. **Puan modeli** modülünün çıkışını **modeli değerlendir**modülünün sol girişine bağlayın. 

    Son işlem hattı şuna benzer görünmelidir:

    ![İşlem hattının doğru yapılandırmasını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/pipeline-final-graph.png)

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

### <a name="view-results"></a>Sonuçları görüntüleme

Çalıştırma tamamlandıktan sonra, işlem hattı çalıştırmasının sonuçlarını görüntüleyebilirsiniz. 

1. **Puan** modeli modülünü seçerek, puan **modeli** modülünün çıktısını görüntüleyin.

1. **Özellikler** bölmesinde, **çıktılar** > **görselleştirin**' ı seçin. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.

    !["Puanlanmış etiket" sütununu vurgulayan çıkış görselleştirmesinin ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. **Puan modeli** modülünü seçerek **modeli değerlendir** modülünün çıktısını görüntüleyin.

1. **Özellikler** bölmesinde **Çıkış** > **Görselleştir**' i seçin ve ardından **Görselleştir**' i seçin.

Modeliniz için aşağıdaki istatistikler gösterilmektedir:

* **Ortalama mutlak hata (MAE)** : mutlak hataların ortalaması (bir hata, tahmin edilen değer ile gerçek değer arasındaki farktır).
* **Kök ortalama kare hatası (rmo)** : test veri kümesinde yapılan tahmine dayalı ortalama kare şeklindeki hata sayısının kare kökü.
* **Relative Absolute Error (Göreli Mutlak Hata)** : Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak hataların mutlak farka göreli ortalaması.
* **Relative Squared Error (Göreli Karesi Alınmış Hata)** : Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış hataların karesi alınmış farka göreli ortalaması.
* **Belirleme katsayısı**: R kare değeri olarak da bilinen bu, modelin verilere ne kadar iyi uyduğunu gösteren istatistiksel bir ölçümdür.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük olan bir değer, tahminlerin gerçek değerlerle daha yakından eşleştiğini gösterir. Değer belirleme katsayısı için, değeri bir (1,0), daha iyi tahmine sahip olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticinin bir parçası olarak şu adımları tamamladınız:

* İşlem hattı oluşturma
* Veriler hazırlandı
* Model eğitimi
* Model puanlanmış ve model değerlendirildi

İkinci bölümde, modelinizi gerçek zamanlı bir uç nokta olarak dağıtmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Modelleri dağıtmaya devam et](tutorial-designer-automobile-price-deploy.md)
