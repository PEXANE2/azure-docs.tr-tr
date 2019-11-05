---
title: Model hiper parametrelerini ayarla
titleSuffix: Azure Machine Learning service
description: En uygun parametre ayarlarını belirleyebilmek için bir modelde parametre süpürme gerçekleştirmek üzere Azure Machine Learning hizmetinde ayarlama modeli hiper parametreleri modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 06adfe66bfe894d7b3c95e3d416da866c7d103b3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73515661"
---
# <a name="tune-model-hyperparameters"></a>Model hiper parametrelerini ayarla

Bu makalede, belirli bir makine öğrenimi modeli için en uygun hiper parametreleri belirleme Azure Machine Learning tasarımcısında (Önizleme) bulunan [model hiper parametreleri](tune-model-hyperparameters.md) modülünün nasıl kullanılacağı açıklanır. Modül, farklı ayar bileşimleri kullanarak birden çok modeli oluşturur ve sınar ve ayarların birleşimini almak için ölçümleri tüm modeller üzerinde karşılaştırır. 

Terms *parametresi* ve *hyperparameter* kafa karıştırıcı olabilir. Modelin *parametreleri* Özellikler bölmesinde ayarladığınız şeydir. Temel olarak, bu modül belirtilen parametre ayarları üzerinde bir *parametre tarama* gerçekleştirir ve belirli bir karar ağacı, veri kümesi veya gerileme yöntemi için farklı olabilecek en iyi _hiper parametre_kümesini öğrenir. En iyi yapılandırmayı bulma işlemi bazen *ayarlama*olarak adlandırılır. 

Modül, bir modelin en iyi ayarlarını bulmak için aşağıdaki yöntemi destekler 

**Tümleşik eğitim ve ayarlama**: kullanılacak parametre kümesini yapılandırır ve ardından modülün birden fazla kombinasyon üzerinde yineleme yapmasına izin verir ve bu da "en iyi" modeli bulana kadar doğruluğu ölçmenize olanak tanır. En öğrenner modülleri sayesinde, eğitim süreci sırasında hangi parametrelerin değiştirilmesi gerektiğini ve ne sabit kalması gerektiğini seçebilirsiniz.

    Depending on how long you want the tuning process to run, you might decide to exhaustively test all combinations, or you could shorten the process by establishing a grid of parameter combinations and testing a randomized subset of the parameter grid.

 Bu yöntem, yeniden kullanım için kaydedebilmeniz gereken eğitilen bir model oluşturur.  

### <a name="related-tasks"></a>İlgili görevler

+ Ayarlamadan önce, en yüksek bilgi değerine sahip sütunları veya değişkenleri ayarlamak için özellik seçimini uygulayın.

## <a name="how-to-configure-tune-model-hyperparameters"></a>Ayarlama modeli hiper parametrelerini yapılandırma  

Genellikle, belirli bir makine öğrenimi modelinin en uygun hiper parametrelerini öğrenirken önemli miktarda sorumlu olması gerekir.

### <a name="train-a-model-using-a-parameter-sweep"></a>Parametre süpürme kullanarak bir modeli eğitme  

Bu bölümde, model [hiper parametreleri ayarla](tune-model-hyperparameters.md) modülünü kullanarak bir modeli gösteren temel bir parametre süpürme gerçekleştirme açıklanmaktadır.

1.  Tasarımcı 'daki işlem hattınızı [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) modülünü ekleyin.

2.  Eğitimli olmayan bir modeli en soldaki girişe bağlayın. 

3. Değişken Oluşturucu **Oluştur** seçeneğini **parametre aralığına** ayarlayın ve parametre süpürme içinde kullanılacak bir değer aralığı belirtmek için **Aralık oluşturucusunu** kullanın.  

    Neredeyse tüm sınıflandırma ve regresyon modülleri tümleşik bir parametre süpürmesini destekler. Bir parametre aralığı yapılandırmayı desteklemeyen öğrenenlere yönelik olarak yalnızca kullanılabilir parametre değerleri test edilebilir.

    Bir veya daha fazla parametre için değeri el ile ayarlayabilir ve ardından kalan parametrelerin üzerinde tarama yapabilirsiniz. Bu işlem biraz zaman kazandırabilir.

4.  Eğitim için kullanmak istediğiniz veri kümesini ekleyin ve bunu [ayarlama modeli hiper parametrelerinin](tune-model-hyperparameters.md)orta girişine bağlayın.  

    İsteğe bağlı olarak, etiketli bir veri kümeniz varsa, bunu en sağdaki giriş bağlantı noktasına (**Isteğe bağlı doğrulama veri kümesi**) bağlayabilirsiniz. Bu, eğitim ve ayarlama sırasında doğruluğu ölçmenize olanak tanır.

5.  [Model hiper parametrelerinin ayarla](tune-model-hyperparameters.md)' nın **Özellikler** bölmesinde, **parametre swemodu**için bir değer seçin. Bu seçenek parametrelerin nasıl seçili olduğunu denetler.

    - **Tüm ızgara**: Bu seçeneği belirlediğinizde, modül, sistem tarafından önceden tanımlanmış bir kılavuz üzerinde döngü gerçekleştirerek farklı birleşimler deneyebilir ve en iyi öğrenme kimliğini belirler. Bu seçenek, en iyi parametre ayarlarının ne olabileceğini ve tüm olası değer birleşimini denemek istediğinizi bilmeyen durumlar için yararlıdır.

    - **Rastgele tarama**: Bu seçeneği belirlediğinizde modül, sistem tarafından tanımlanan bir aralıktaki parametre değerlerini rastgele seçer. Modülün yürütmesini istediğiniz en fazla çalıştırma sayısını belirtmeniz gerekir. Bu seçenek, tercih ettiğiniz ölçümleri kullanarak model performansını artırmak istediğiniz, ancak bilgi işlem kaynaklarını sürdürmeye devam eden durumlar için yararlıdır.

    Rastgele bir tarama seçerseniz, rastgele **tarama üzerinde en fazla çalıştırma sayısını**belirtebilirsiniz. Bu, bir dizi değerin rastgele bir bileşimini kullanarak, modelin kaç kez eğitilecek olması gerektiğini gösterir.

6.  **Etiket sütunu**için, tek bir etiket sütunu seçmek üzere sütun seçiciyi başlatın.

7.  **Rastgele tarama sırasında en fazla çalışma sayısı**: rastgele bir tarama seçerseniz, parametre değerlerinin rastgele bir birleşimini kullanarak modelin kaç kez eğitilmeli olduğunu belirtebilirsiniz.

    **Rastgele kılavuzda en fazla çalışma sayısı**: Bu seçenek ayrıca parametre değerlerinin rastgele bir örneklemede yineleme sayısını denetler, ancak değerler belirtilen aralıktan rastgele oluşturulmaz; Bunun yerine, tüm olası parametre değeri birleşimlerinin bir matrisi oluşturulur ve bir rastgele örnekleme, matris üzerinden alınır. Bu yöntem, bölgesel fazla örnekleme veya yetersiz örnekleme konusunda daha etkilidir ve daha az açıktır.

8.  Modelleri **derecelendirerek** kullanılacak tek bir ölçüm seçin.

    Bir parametre süpürme çalıştırdığınızda model türü için geçerli tüm ölçümler hesaplanır ve **tarama sonuçları** raporunda döndürülür. Ayrı ölçümler, regresyon ve sınıflandırma modelleri için kullanılır.

    Ancak, seçtiğiniz ölçüm modellerin derecelendirme şeklini belirler. Yalnızca, seçili ölçüm tarafından derecelendirilen en üst model, Puanlama için kullanılacak eğitilen bir model olarak çıktı.

9.  **Rastgele çekirdek**için, parametre süpürme başlatılırken kullanılacak bir sayı yazın. 

10. İşlem hattını çalıştırma.

## <a name="results-of-hyperparameter-tuning"></a>Hiper parametre ayarlama sonuçları

Eğitim tamamlandığında:

+ En iyi modelin doğruluk ölçümleri kümesini görüntülemek için modüle sağ tıklayın, **tarama sonuçları**' nı seçin ve ardından **Görselleştir**' i seçin.

    Model türü için geçerli olan tüm doğruluk ölçümleri çıkışlardır, ancak sıralama için seçtiğiniz ölçüm hangi modelin "en iyi" kabul edileceğini belirler.

+ Diğer işlem hatlarında Puanlama için modeli kullanmak üzere, ayarlama işlemini tekrarlamanız gerekmeden model çıktısına sağ tıklayıp **eğitilen model olarak kaydet**' i seçin. 


## <a name="technical-notes"></a>Teknik notlar

Bu bölümde, sık sorulan soruların uygulama ayrıntıları, ipuçları ve yanıtları yer almaktadır.

### <a name="how-a-parameter-sweep-works"></a>Bir parametre süpürme nasıl kullanılır

Bu bölümde, parametre süpür'nin genel olarak nasıl çalıştığı ve bu modüldeki seçeneklerin nasıl etkileşime gireceğini açıklanmaktadır.

Bir parametre süpürme ayarladığınızda, en çok sınırlı sayıda parametresi veya tanımladığınız bir parametre alanı üzerinde ayrıntılı bir arama kullanmak için aramanızın kapsamını tanımlarsınız.

+ **Rastgele tarama**: Bu seçenek, bir dizi yinelemenin sayısını kullanarak bir modeli ister. 

     Yinelemek için bir Aralık değeri belirtirsiniz ve modül bu değerlerin rastgele seçilmiş bir alt kümesini kullanır.  Değerler değiştirme ile seçilir, yani önceden rastgele seçilmiş olan sayıların kullanılabilir sayı havuzundan kaldırılmadığı anlamına gelir. Bu nedenle, seçilen herhangi bir değer tüm geçişlerde aynı kalır.  

+ **Tüm ızgara**: kılavuzun tamamını kullanma seçeneği, her bir ve her birleşimin test edileceği anlamına gelir. Bu seçenek en kapsamlı olarak düşünülebilir, ancak en fazla zaman gerektirir. 

### <a name="controlling-the-length-and-complexity-of-training"></a>Eğitimin uzunluğunu ve karmaşıklığını denetleme

Birçok ayar kombinasyonu üzerinde yineleme zaman alabilir, bu nedenle modül işlemi kısıtlamak için çeşitli yollar sağlar:

+ Bir modeli test etmek için kullanılan yineleme sayısını sınırlayın
+ Parametre alanını sınırlandırma
+ Yineleme sayısını ve parametre alanını sınırla

Belirli bir veri kümesi ve model için en verimli eğitim yöntemini belirleme ayarlarına sahip işlem hattı yapmanızı öneririz.

### <a name="choosing-an-evaluation-metric"></a>Değerlendirme ölçümü seçme

Ölçüm sonuçlarını gözden geçirebilmeniz için her bir modelin doğruluğunu içeren bir rapor, sonda sunulur. Tüm ikili sınıflandırma modelleri için tek bir ölçüm kümesi kullanılır, tüm çok sınıf sınıflandırma modelleri için doğruluk kullanılır ve gerileme modelleri için farklı bir ölçüm kümesi kullanılır. Ancak eğitim sırasında, ayarlama işlemi sırasında oluşturulan modelleri derecelendirerek kullanılacak **tek** bir ölçüm seçmeniz gerekir. En iyi ölçümün, iş sorununuza ve hatalı pozitif sonuçlar ve yanlış negatifler maliyetlerine bağlı olarak değiştiğini fark edebilirsiniz.

#### <a name="metrics-used-for-binary-classification"></a>İkili sınıflandırma için kullanılan ölçümler

-   **Doğruluk** Toplam durum için gerçek sonuç oranı.  

-   **Duyarlık** Gerçek sonuçların pozitif sonuç oranı.  

-   **Geri çek** Tüm sonuçlar üzerinde doğru sonuçların kesiri.  

-   **F puanı** Duyarlık ve geri çekmeyi dengeleyen bir ölçü.  

-   **AUC** X ekseninde yanlış pozitif değerler çizilse ve y ekseninde gerçek pozitif değerler çizilse, eğri altındaki alanı temsil eden bir değer.  

-   **Ortalama günlük kaybı** İki olasılık dağıtımı arasındaki fark: gerçek bir, ve modeldeki bir.  

#### <a name="metrics-used-for-regression"></a>Gerileme için kullanılan ölçümler

-   **Mutlak ortalama hata** modeldeki tüm hatanın ortalamasını alır, burada hata tahmin edilen değerin gerçek değerden uzaklığı anlamına gelir. Genellikle **Mae**olarak kısaltılır.  

-   **Ortalanan kare hatasının kökü** , hataların karelerinin ortalamasını ölçer ve bu değerin kökünü alır. Genellikle **rmo olarak kısaltıcı**  

-   **Göreli mutlak hata** , hatayı doğru değerin yüzdesi olarak gösterir.  

-   **Göreli kare oluşan hata** toplam kare hata sayısını normalleştirir ve tahmin edilen değerlerin toplam kare hatası ile bölünür.  

-   **Belirleme katsayısı** Verilerin modele ne kadar iyi uyduğunu gösteren tek bir sayı. Bir değeri, modelin verilerle tam olarak eşleştiği anlamına gelir; sıfır değeri, verilerin rastgele olması veya başka türlü modele sığamayacak olması anlamına gelir. Genellikle **r<sup>2</sup>** , **r<sup>2</sup>** veya **r-kare**olarak adlandırılır.  

### <a name="modules-that-do-not-support-a-parameter-sweep"></a>Parametre süpürme desteği olmayan modüller

Azure Machine Learning neredeyse tüm öğrenenler, tümleşik bir parametre süpürme ile çapraz doğrulamayı destekler, bu da işlem hattının parametrelerini seçmenizi sağlar. Learner, bir değer aralığı ayarlamayı desteklemiyorsa, bunu çapraz doğrulamada kullanmaya devam edebilirsiniz. Bu durumda, tarama için bazı izin verilen değerler aralığı seçilir. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 

