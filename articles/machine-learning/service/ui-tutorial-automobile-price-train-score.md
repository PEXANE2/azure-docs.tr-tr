---
title: 'Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin'
titleSuffix: Azure Machine Learning service
description: Bir sürükle ve bırak görsel arabirimi kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma hakkında bilgi edinin. Bu öğretici, doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etmeye yönelik iki bölümlü bir serinin bir parçasıdır.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/21/2019
ms.openlocfilehash: 09d81e281b92b662572cefc220f2227651b69838
ms.sourcegitcommit: 83a89c45253b0d432ce8dcd70084c18e9930b1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371728"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin

Bu iki bölümden oluşan öğreticide, her bir otomobil fiyatını tahmin eden tahmine dayalı analitik çözüm geliştirmek ve dağıtmak için Azure Machine Learning hizmeti görsel arabirimini nasıl kullanacağınızı öğreneceksiniz. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

Birinci bölümde, ortamınızı ayarlayacaksınız, veri kümelerini ve analiz modüllerini etkileşimli bir tuvale sürükleyip bırakır ve bir deneme oluşturmak için bunları birbirine bağlayabilirsiniz. 

Öğreticinin bir parçası olarak şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Veri alma ve Temizleme
> * Makine öğrenimi modelini eğitme
> * Bir modeli Puanlama ve değerlendirme

Öğreticinin [ikinci kısmında](ui-tutorial-automobile-price-deploy.md) , tahmine dayalı modelinizi bir Azure Web hizmeti olarak dağıtmayı öğrenirsiniz ve bu sayede, size gönderilen teknik belirtimlere göre herhangi bir arabasının fiyatını tahmin edebilirsiniz. 

Bu öğreticinin tamamlanmış sürümü örnek bir deneme olarak sunulmaktadır.

**Denemeleri sayfasında**, **Yeni Ekle**' yi seçin ve **ardından örnek 1 gerileme ' yı seçin: Otomobil fiyat tahmini (temel)** denemesi.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanınız varsa, sonraki bölüme atlayın.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-new-experiment"></a>Yeni deneme oluştur

1. Çalışma alanınızı [Azure Portal](https://portal.azure.com/)açın.

1. Çalışma alanınızda **görsel arabirim**' i seçin. Ardından **görsel arabirimi Başlat**' ı seçin. 

    ![Machine Learning hizmeti çalışma alanından görsel arabirime nasıl erişediğinin gösterildiği Azure portal ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

1. Görsel arabirim penceresinin alt kısmındaki **+ Yeni** ' yi seçerek yeni bir deneme oluşturun.

1. **Boş denemeler**' i seçin.

1. Tuvalin en üstündeki **"deneme sürümü oluşturuldu...** " varsayılan deneme adını seçin ve anlamlı bir şekilde yeniden adlandırın. Örneğin, **"otomobil fiyat tahmini"** . Adın benzersiz olması gerekmez.

## <a name="specify-data"></a>Veri belirt

Machine Learning verilere bağlıdır. Luckily, bu arabirime dahil edilen birkaç örnek veri kümesi vardır ve bunu denemeniz için kullanılabilir. Bu öğretici için, **otomobil veri kümesi (ham)** örnek veri kümesini kullanın. 

1. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Kaydedilen veri kümelerini görüntülemek için **kayıtlı veri kümelerini** seçin ve **örnekler** ' i seçin.

1. Veri kümesini, **otomobil fiyat verilerini (ham)** seçin ve tuvale sürükleyin.

   ![Verileri tuvale sürükleyin](./media/ui-tutorial-automobile-price-train-score/drag-data.png)

1. Hangi veri sütunlarının birlikte çalışabileceği seçin. **Veri kümesi modülünde sütunları seçme** modülünü bulmak için paletin en üstündeki arama kutusuna **seçin** yazın.

1. **Veri kümesindeki sütunları seçme** modülü ' ne tıklayın ve tuval üzerine sürükleyin. Modülün veri kümesi modülünün altına bırakın.

1. Daha önce eklediğiniz veri kümesini, tıklayıp sürükleyerek **veri kümesindeki sütunları seçme** modülüne bağlayın. Veri kümesinin çıkış bağlantı noktasından, tuvaldeki veri kümesinin en altında bulunan küçük bir daire olan ve modülün en üstünde bulunan küçük bir daire olan **veri kümesindeki sütunları seçme**giriş bağlantı noktasına kadar sürükleyin.

    > [!TIP]
    > Bir modülün çıkış bağlantı noktasını başka bir giriş bağlantı noktasına bağladığınızda, denemenize karşı bir veri akışı oluşturursunuz.
    >

    ![Modülleri bağlama](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    Kırmızı ünlem işareti, modülün özelliklerini henüz belirlemediğiniz anlamına gelir.

1. **Veri kümesi modülünde sütunları seç '** i seçin.

1. Tuvalin sağındaki **Özellikler** bölmesinde **Sütunları Düzenle**' yi seçin.

    **Sütunları seçin** iletişim kutusunda, **tüm sütunlar** ' ı seçin ve **tüm özellikleri**ekleyin. İletişim kutusu şöyle görünmelidir:

     ![Sütun seçici](./media/ui-tutorial-automobile-price-train-score/select-all.png)

1. Sütun seçiciyi kapatmak için sağ alt köşedeki **Tamam** ' ı seçin.

## <a name="run-the-experiment"></a>Denemeyi çalıştırma

İstediğiniz zaman, veri akışındaki bu noktada verilerin nasıl göründüğünü görmek için bir veri kümesinin veya modülün çıkış bağlantı noktasına tıklayın. **Görselleştir** seçeneği devre dışıysa, önce denemeyi çalıştırmanız gerekir.

Bir deneme, çalışma alanınıza bağlı bir işlem kaynağı olan işlem hedefi üzerinde çalışır. Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

İşlem hedefi kullanılabilir olduktan sonra, deneme çalışır. Çalıştırma tamamlandığında, her modülde yeşil bir onay işareti görünür.


## <a name="visualize-the-data"></a>Verileri görselleştirme

İlk denemenizi çalıştırdığınıza göre, sahip olduğunuz veri kümesi hakkında daha fazla bilgi edinmek için verileri görselleştirebilirsiniz.

1. **Veri kümesindeki sütunları seç '** in altındaki çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

1. Bu sütun hakkındaki bilgileri görüntülemek için veri penceresinde farklı sütunlara tıklayın.

    Bu veri kümesinde, her satır bir otomobil ve her bir otomobil ile ilişkili değişkenler sütun olarak görünür. Bu veri kümesinde 205 satır ve 26 sütun vardır.

     Bir veri sütununa her tıkladığınızda, söz konusu sütunun **istatistik** bilgileri ve **görselleştirme** görüntüsü solda görünür. Örneğin, kapılara tıkladığınızda, iki  benzersiz değere ve iki eksik değere sahip olduğunu görürsünüz. Değerleri görmek için aşağı kaydırın: iki ve dört kapı.

     ![Verilerin önizlemesini yapın](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)

1. Veri kümeniz hakkında daha fazla bilgi edinmek için her sütuna tıklayın ve bu sütunların bir otomobil fiyatını tahmin etmek için yararlı olup olmayacağını düşünün.

## <a name="prepare-data"></a>Verileri hazırlama

Genellikle, bir veri kümesinin çözümlenmesi için önce bazı ön işleme gerekir. Veri kümesini görselleştirirken bazı eksik değerleri fark etmiş olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Eksik değerleri olan satırları kaldırırsınız. Ayrıca, **normalleştirilmiş kayıplar** sütununda eksik değerlerin büyük bir oranı bulunur, bu nedenle bu sütunu modelden tamamen dışlayabilirsiniz.

> [!TIP]
> Giriş verilerinden eksik değerleri temizleme, çoğu modülü kullanmanın bir önkoşuludur.

### <a name="remove-column"></a>Sütunu kaldırma

İlk olarak, **normalleştirilmiş zararlar** sütununu tamamen kaldırın.

1. **Veri kümesi modülünde sütunları seç '** i seçin.

1. Tuvalin sağındaki **Özellikler** bölmesinde **Sütunları Düzenle**' yi seçin.

    * Kurallar ve **tüm sütunlar** seçili **olarak** bırakın.

    * Açılan menülerden **Hariç Tut** ve **sütun adlarını** seçerek metin kutusuna tıklayın. **Normalleştirilmiş zararlar**yazın.

    * Sütun seçiciyi kapatmak için sağ alt köşedeki **Tamam** ' ı seçin.

    ![Sütun dışlama](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Artık veri kümesindeki Select sütunları için Özellikler bölmesi, **normalleştirilmiş zararlar**hariç veri kümesindeki tüm sütunları geçidiyi gösterir.
        
    Özellikler bölmesi, **normalleştirilmiş kayıplar** sütununun dışlanacağını gösterir.
        
    ![Özellik bölmesi](./media/ui-tutorial-automobile-price-train-score/property-pane.png)
        
    Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. 

1. **Veri kümesi modülünde sütunları seç** ' e çift tıklayın ve "normalleştirilmiş zararları hariç tut" açıklamasını yazın. 
    
    Yorumu yazdıktan sonra modülün dışına tıklayın. Modülün bir yorum içerdiğini gösteren bir aşağı ok belirir.

1. Yorumu göstermek için aşağı oka tıklayın.

    Modül artık yorumu gizlemek için bir yukarı ok gösterir.
        
    ![Açıklamalar](./media/ui-tutorial-automobile-price-train-score/comments.png)

### <a name="clean-missing-data"></a>Eksik verileri temizleme

Bir modeli eğitedığınızda, eksik olan veriler hakkında bir şey yapmanız gerekir. Bu durumda, eksik verileri olan kalan satırları kaldırmak için bir modül ekleyeceksiniz.

1. **Eksik verileri temizle** modülünü bulmak için arama kutusuna **Clean** yazın.

1. **Eksik verileri temizle** modülünü deneme tuvaline sürükleyin ve **veri kümesindeki sütunları seçme** modülüne bağlayın. 

1. Özellikler bölmesinde, **temizleme modu**' nun altındaki **tüm satırı Kaldır** ' ı seçin.

1. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.
 
    ![Satırları kaldır](./media/ui-tutorial-automobile-price-train-score/remove-rows.png)

    Denemeniz artık şuna benzer görünmelidir:
    
    ![Select-Column](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="train-the-model"></a>Modeli eğitme

Artık veriler hazır olduğuna göre, tahmine dayalı bir model oluşturabilirsiniz. Modeli eğitebilmeniz için verilerinizi kullanacaksınız. Daha sonra, fiyatları tahmin etmeye ne kadar yakından başlayacaklarını görmek için modeli test edersiniz.

**Sınıflandırma** ve **regresyon**, denetimli makine öğrenimi algoritmasının iki türüdür. **Sınıflandırma** , renk (kırmızı, mavi veya yeşil) gibi tanımlı kategori kümesinden bir yanıtı tahmin eder. **Gerileme** , bir sayıyı tahmin etmek için kullanılır.

Bir sayı olan fiyatı tahmin etmek istediğiniz için, regresyon algoritmasını kullanabilirsiniz. Bu örnekte, doğrusal regresyon modeli kullanacaksınız.

Fiyatı içeren bir veri kümesi vererek modeli eğitme. Model, verileri tarar ve bir araba 'nin özellikleri ile fiyatı arasında bağıntılar arar.

Verileri ayrı eğitim ve test veri kümelerine bölerek, hem modeli eğitmek hem de test etmek için verilerinizi kullanın.

1. **Verileri Böl** modülünü bulmak için arama kutusuna **bölünmüş** verileri yazın ve **eksik verileri temizleme** modülünün sol bağlantı noktasına bağlayın.

1. **Bölünmüş veri** modülünü seçin. Özellikler bölmesinde, ilk çıktı veri kümesindeki satır kesirini 0,7 olarak ayarlayın. Bu şekilde, modeli eğitmek için verilerin yüzde 70 ' ünü kullanacağız ve test için yüzde 30 ' unu geri alacaksınız.

    ![Özellikler bölmesinin doğru yapılandırmasını gösteren ekran görüntüsü. "Verileri bölme" değerleri "satırları Böl", 0,7, rastgele bölünen, 0, false olmalıdır.](./media/ui-tutorial-automobile-price-train-score/split-data.png)

1. **Bölünmüş verilere** çift tıklayın ve "veri kümesini eğitim kümesine Böl (0.7) ve test kümesi (0,3)" açıklamasını yazın

1. Öğrenme algoritmasını seçmek için modül paleti arama kutusunu temizleyin.

1. **Machine Learning** genişletin ve sonra **modeli Başlat**' ı genişletin. Böylece makine öğrenimi algoritmalarını başlatmak için kullanılabilecek çeşitli modül kategorileri görüntülenir.

1. Bu deneme için, **regresyon** > **Doğrusal regresyon** ' i seçin ve deneme tuvaline sürükleyin.

    ![Özellikler bölmesinin doğru yapılandırmasını gösteren ekran görüntüsü. "Verileri bölme" değerleri "satırları Böl", 0,7, rastgele bölünen, 0, false olmalıdır.](./media/ui-tutorial-automobile-price-train-score/linear-regression-module.png)

1. **Eğitim modeli** modülünü bulun ve deneme tuvaline sürükleyin. Doğrusal regresyon modülünün çıkışını tren modeli modülünün sol girişine bağlayın ve **bölünmüş veri** modülünün eğitim verileri çıkışını (sol bağlantı noktası) **tren modeli** modülünün doğru girişine bağlayın.

    ![Eğitim modeli modülünün doğru yapılandırmasını gösteren ekran görüntüsü. Doğrusal regresyon modülü, model eğitimi modülünün sol bağlantı noktasına bağlanır ve bölünmüş veri modülü, tren modelinin sağ bağlantı noktasına bağlanır](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. **Model eğitme** modülünü seçin. Özellikler bölmesinde, sütun seçiciyi Başlat ' ı seçin ve ardından **sütun adlarını içer**' in yanındaki **Fiyat** yazın. Fiyatlandırma, modelinizin tahmin edilecek değerdir

    ![Sütun seçici modülünün doğru yapılandırmasını gösteren ekran görüntüsü. Kurallarla > sütun adlarını Içerme > "Price"](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    Denemenizin şöyle görünmesi gerekir:

    ![Eğitim modeli modülü eklendikten sonra denemenin doğru yapılandırmasını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

## <a name="score-and-evaluate-the-model"></a>Modeli Puanlama ve değerlendirme

Artık, bu modeli verilerinizin yüzde 70 ' ünü kullanarak eğitildiniz, modelin ne kadar iyi olduğunu görmek için, verilerin yüzde 30 ' unu almak üzere onu kullanabilirsiniz.

1. **Puan modeli** modülünü bulmak için arama kutusuna **puan modeli** yazın ve modülü deneme tuvaline sürükleyin. **Eğitim modeli** modülünün çıkışını, **puan modelinin**sol giriş bağlantı noktasına bağlayın. **Veri ayırma** modülünün test verileri çıkışını (sağ bağlantı noktası), **puan modelinin**sağ giriş bağlantı noktasına bağlayın.

1. **Modeli değerlendir** ' i bulmak için arama kutusuna **değerlendir** yazın ve modülü deneme tuvaline sürükleyin. **Puan modeli** modülünün çıkışını **modeli değerlendir**modülünün sol girişine bağlayın. Son deneme şuna benzer şekilde görünecektir:

    ![Denemenin son doğru yapılandırmasını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Daha önce oluşturduğunuz işlem kaynağını kullanarak denemeyi çalıştırın.

1. **Puan** modelinin çıkış bağlantı noktasını seçip **Görselleştir**' i seçerek **puan modeli** modülünün çıktısını görüntüleyin. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.

    !["Puanlanmış etiket" sütununu vurgulayan çıkış görselleştirmesinin ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. **Modeli değerlendir** modülünün çıkışını görüntülemek için çıkış bağlantı noktasını seçin ve ardından **Görselleştir**' i seçin.

    ![Son deneme için değerlendirme sonuçlarını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

Modeliniz için aşağıdaki istatistikler gösterilmektedir:

* **Ortalama mutlak hata (MAE)** : Mutlak hataların ortalaması (bir hata, tahmin edilen değer ile gerçek değer arasındaki farktır).
* **Kök ortalama kare hatası (rmo)** : Test veri kümesinde yapılan tahmine dayalı olarak kare içinde oluşan ortalama hata sayısının kare kökü.
* **Göreli mutlak hata**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak farka göre ortalama mutlak hata sayısı.
* **Göreli kare hatası**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki kare farklılığa göre ortalama kare dışı hataların ortalaması.
* **Belirleme katsayısı**: R kare değeri olarak da bilinen bu, modelin verilere ne kadar iyi uyduğunu gösteren istatistiksel bir ölçümdür.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük olan bir değer, tahminlerin gerçek değerlerle daha yakından eşleştiğini gösterir. Değer belirleme katsayısı için, değeri bir (1,0), daha iyi tahmine sahip olur.

## <a name="manage-experiments-in-azure-machine-learning-service-workspace"></a>Azure Machine Learning hizmeti çalışma alanında denemeleri yönetme

Görsel arabirimde oluşturduğunuz denemeleri Azure Machine Learning hizmeti çalışma alanından yönetilebilir. Bireyler çalıştırma, tanılama günlükleri, yürütme grafikleri ve daha fazlası gibi daha ayrıntılı bilgileri görmek için çalışma alanını kullanın.

1. Çalışma alanınızı [Azure Portal](https://portal.azure.com/)açın.  

1. Çalışma alanınızda **denemeleri**' yi seçin. Sonra oluşturduğunuz denemeyi seçin.

    ![Azure portal 'de denemeleri 'e nasıl gidebileceğiniz gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/portal-experiments.png)

    Bu sayfada, denemeye ve en son çalışmasına ilişkin bir genel bakış görürsünüz.

    ![Azure portal deneme istatistiklerinin genel bakışını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/experiment-overview.png)

1. Belirli bir yürütme hakkında daha fazla ayrıntı görmek için bir çalıştırma numarası seçin.

    ![Ekran görüntüsü ayrıntılı çalıştırma raporu](./media/ui-tutorial-automobile-price-train-score/run-details.png)

    Çalıştırma raporu gerçek zamanlı olarak güncelleştirilir. Denemenize bir **Python betiği Yürüt** veya **R betiği Yürüt** modülü kullandıysanız, **Günlükler** sekmesinde çıkış yapmak için betik günlüklerini belirtebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticinin bir parçası olarak şu adımları tamamladınız:

* Deneme oluşturma
* Verileri hazırlama
* Modeli eğitme
* Modeli Puanlama ve değerlendirme

İkinci bölümde, modelinizi bir Azure Web hizmeti olarak dağıtmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Modelleri dağıtmaya devam et](ui-tutorial-automobile-price-deploy.md)
