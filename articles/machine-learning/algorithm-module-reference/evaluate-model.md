---
title: 'Modeli Değerlendirin: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Eğitimli bir modelin doğruluğunu ölçmek için Azure Machine Learning'de Modeli Değerlendir modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/24/2020
ms.openlocfilehash: c1bcbb6a368c9c80f968c48c1a6e0bc6c95133d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456413"
---
# <a name="evaluate-model-module"></a>Model modüllerini değerlendirin

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Eğitimli bir modelin doğruluğunu ölçmek için bu modülü kullanın. Bir modelden oluşturulan puanları içeren bir veri kümesi sağlarsınız ve **Model Değerlendir** modülü endüstri standardı değerlendirme ölçümleri kümesini hesaplar.
  
 **Değerlendirme Modeli** tarafından döndürülen ölçümler, değerlendirdiğiniz modeltürüne bağlıdır:  
  
-   **Sınıflandırma Modelleri**    
-   **Regresyon Modelleri**  
-   **Kümeleme Modelleri**  


> [!TIP]
> Eğer modelleme değerlendirme için yeni iseniz, Biz EdX [makine öğrenme kursunun](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) bir parçası olarak, Dr Stephen Elston tarafından video serisi öneririz. 


**Modeli Değerlendir** modülünü kullanmanın üç yolu vardır:

+ Eğitim verileriniz üzerinden puanlar oluşturun ve modeli bu puanlar üzerinden değerlendirin
+ Modelde puanlar oluşturun, ancak bu puanları ayrılmış bir test kümesindeki puanlarla karşılaştırın
+ Aynı veri kümesini kullanarak iki farklı ama ilgili modelin puanlarını karşılaştırın

## <a name="use-the-training-data"></a>Eğitim verilerini kullanma

Bir modeli değerlendirmek için, bir giriş sütunları ve puanları kümesi içeren bir veri kümesi bağlamanız gerekir.  Başka veri yoksa, özgün veri kümenizi kullanabilirsiniz.

1. [Puan Modeli'nin](./score-model.md) **Puanlı veri kümesi** çıktısını Değerlendir **Modeli'nin**girişine bağlayın. 
2. **Model'i Değerlendir** modül'üne tıklayın ve değerlendirme puanlarını oluşturmak için ardışık hattı çalıştırın.

## <a name="use-testing-data"></a>Test verilerini kullanma

Makine öğreniminde yaygın bir senaryo, orijinal veri kümenizi [Split](./split-data.md) modüllerini veya [Bölüm ve Örnek](./partition-and-sample.md) modüllerini kullanarak veri kümelerini eğitim ve test e kümelerine ayırmaktır. 

1. [Puan Modeli'nin](score-model.md) **Puanlı veri kümesi** çıktısını Değerlendir **Modeli'nin**girişine bağlayın. 
2. Test verilerini içeren Split Data modülünün çıktısını **Model'i Değerlendir'in**sağ girişine bağlayın.
2. **Modeli Değerlendir** modülünü tıklatın ve değerlendirme puanlarını oluşturmak için **seçilen Çalıştır'ı** seçin.

## <a name="compare-scores-from-two-models"></a>İki modelden gelen puanları karşılaştırın

Ayrıca, **Modeli Değerlendirmek**için ikinci bir puan kümesi bağlayabilirsiniz.  Puanlar, bilinen sonuçları olan paylaşılan bir değerlendirme kümesi veya aynı veriler için farklı bir modelden bir dizi sonuç olabilir.

Aynı verilerdeki iki farklı modelin sonuçlarını kolayca karşılaştırabildiğiniziçin bu özellik yararlıdır. Veya, aynı veriler üzerinde iki farklı çalıştırmadan gelen puanları farklı parametrelerle karşılaştırabilirsiniz.

1. [Puan Modeli'nin](score-model.md) **Puanlı veri kümesi** çıktısını Değerlendir **Modeli'nin**girişine bağlayın. 
2. İkinci model için Skor Modeli modülünün çıktısını **Değerlendir Modeli'nin**sağ girişine bağlayın.
3. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

**Modeli Değerlendir'i**çalıştırdıktan sonra, modülü sağ tıklatın ve sonuçları görmek için **Değerlendirme sonuçlarını Görselleştir'i** seçin.

Veri kümelerini **Model'i Değerlendir'in**her iki girişine de bağlarsanız, sonuçlar her iki veri kümesi veya her iki model için ölçümler içerir.
Sol bağlantı noktasına iliştirilen model veya veriler önce raporda, ardından veri kümesinin ölçümleri veya sağ bağlantı noktasına iliştirilen model sunulur.  

Örneğin, aşağıdaki resim, aynı veriler üzerinde oluşturulmuş ancak farklı parametrelere sahip iki kümeleme modelinin sonuçlarının karşılaştırmasını temsil eder.  

![Karşılaştırma2Modeller](media/module/evaluate-2-models.png)  

Bu bir kümeleme modeli olduğundan, değerlendirme sonuçları iki regresyon modelinin puanlarını karşılaştırdığınızda veya iki sınıflandırma modelini karşılaştırdığınızdan farklıdır. Ancak, genel sunu aynıdır. 

## <a name="metrics"></a>Ölçümler

Bu bölümde, **Model'i Değerlendir**ile kullanılmak üzere desteklenen belirli model türleri için döndürülen ölçümler açıklanmaktadır:

+ [sınıflandırma modelleri](#metrics-for-classification-models)
+ [regresyon modelleri](#metrics-for-regression-models)
+ [kümeleme modelleri](#metrics-for-clustering-models)

### <a name="metrics-for-classification-models"></a>Sınıflandırma modelleri için ölçümler

Sınıflandırma modelleri değerlendirilirken aşağıdaki ölçümler raporlanır.
  
-   **Doğruluk,** bir sınıflandırma modelinin iyiliğini, doğru sonuçların toplam olgulara oranı olarak ölçer.  
  
-   **Kesinlik,** tüm olumlu sonuçlara göre doğru sonuçların oranıdır.  
  
-   **Geri çağırma,** model tarafından döndürülen tüm doğru sonuçların bir kısmıdır.  
  
-   **F-skoru, ideal F-skor** değerinin 1 olduğu 0 ile 1 arasındaki hassasiyet ve hatırlamanın ağırlıklı ortalaması olarak hesaplanır.  
  
-   **AUC,** y ekseninde doğru pozitifler ve x ekseninde yanlış pozitiflerle çizilen eğrinin altındaki alanı ölçer. Bu metrik, farklı türdeki modelleri karşılaştırmanızı sağlayan tek bir sayı sağladığından yararlıdır.  
  
- **Ortalama günlük kaybı,** yanlış sonuçların cezasını ifade etmek için kullanılan tek bir puandır. Bu iki olasılık dağılımları arasındaki fark olarak hesaplanır - gerçek bir ve modelde bir.  
  
- **Eğitim günlüğü kaybı** rasgele bir tahmin üzerinde sınıflandırıcı avantajı nı temsil eden tek bir puandır. Günlük kaybı, etiketlerdeki bilinen değerlerle (zemin gerçeği) çıktıaldığı olasılıkları karşılaştırarak modelinizin belirsizliğini ölçer. Bir bütün olarak model için günlük kaybını en aza indirmek istiyorsunuz.

### <a name="metrics-for-regression-models"></a>Regresyon modelleri için ölçümler
 
Regresyon modelleri için döndürülen ölçümler hata miktarını tahmin etmek üzere tasarlanmıştır.  Gözlenen ve öngörülen değerler arasındaki fark küçükse, bir modelin verilere iyi uyduğunu kabul edilir. Ancak, artıkların desenine (herhangi bir tahmin noktası ile buna karşılık gelen gerçek değeri arasındaki fark) bakarak, modeldeki potansiyel önyargı hakkında çok şey söyleyebilir.  
  
 Regresyon modellerinin değerlendirilmesi için aşağıdaki ölçümler bildirilmiştir.
  
- **Ortalama mutlak hata (MAE)** tahminlerin gerçek sonuçlara ne kadar yakın olduğunu ölçer; böylece, daha düşük bir puan daha iyidir.  
  
- **Kök ortalama kare hata (RMSE)** modelinde hata özetleyen tek bir değer oluşturur. Farkı squaring olarak, metrik aşırı tahmin ve yetersiz tahmin arasındaki farkı göz ardı eder.  
  
- **Göreli mutlak hata (RAE)** beklenen ve gerçek değerler arasındaki göreli mutlak farktır; göreli, çünkü ortalama farkı aritmetik ortalamaya bölünür.  
  
- **Göreli kare hata (RSE)** benzer şekilde, gerçek değerlerin toplam kare hatasına bölünerek öngörülen değerlerin toplam kare hatasını normalleştirir.  
  

  
- Genellikle R<sup>2</sup>olarak adlandırılan **belirleme katsayısı,** modelin tahmin gücünü 0 ile 1 arasındaki bir değer olarak temsil eder. Sıfır, modelin rastgele olduğu anlamına gelir (hiçbir şey açıklaşmadı); 1 mükemmel bir uyum olduğu anlamına gelir. Ancak, düşük değerler tamamen normal olabileceğinden ve yüksek değerler şüpheli olabileceğinden, R<sup>2</sup> değerlerinin yorumlanmasında dikkatli olunmalıdır.

###  <a name="metrics-for-clustering-models"></a>Kümeleme modelleri için ölçümler

Kümeleme modelleri birçok açıdan sınıflandırma ve regresyon modellerinden önemli ölçüde farklı olduğundan, [Değerlendirme Modeli](evaluate-model.md) de kümeleme modelleri için farklı bir istatistik kümesi döndürür.  
  
 Kümeleme modeli için döndürülen istatistikler, her kümeye kaç veri noktası atandığını, kümeler arasındaki ayrım miktarını ve her küme içinde veri noktalarının ne kadar sıkı bir şekilde toplandığını açıklar.  
  
 Kümeleme modelinin istatistikleri, küme başına istatistikleri içeren ek satırlarla tüm veri kümesi boyunca ortalamaya göre yapılır.  
  
Kümeleme modellerini değerlendirmek için aşağıdaki ölçümler bildirilmiştir.
    
-   Sütundaki puanlar, **Diğer Merkeze Ortalama Uzaklık,** kümedeki her noktanın diğer tüm kümelerin merkezlerine ne kadar yakın olduğunu gösterir.   

-   Sütundaki puanlar, **Küme Merkezine Ortalama Uzaklık**, kümedeki tüm noktaların o kümenin merkezcisine olan yakınlığını temsil edir.  
  
-   **Puan Sayısı** sütunu, her kümeye kaç veri noktası nın atandığını ve herhangi bir kümedeki toplam veri noktası sayısını gösterir.  
  
     Kümelere atanan veri noktalarının sayısı, kullanılabilir toplam veri noktası sayısından azsa, bu veri noktalarının kümeye atanamadığını anlamına gelir.  
  
-   Sütundaki puanlar, **Küme Merkezine Maksimal Uzaklık,** her nokta ile o noktanın kümesinin merkeztaşı arasındaki mesafelerin toplamını temsil eder.  
  
     Bu sayı yüksekse, kümenin yaygın olarak dağıldığı anlamına gelebilir. Kümenin yayılmasını belirlemek için bu istatistiği **Küme Merkezine Ortalama Uzaklık** ile birlikte gözden geçirmelisiniz.   

-   Sonuçların her bölümünün altındaki **Birleşik Değerlendirme** puanı, o modelde oluşturulan kümelerin ortalama puanlarını listeler.  
  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 