---
title: 'Filtre tabanlı özellik seçimi: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: En büyük tahmin gücüne sahip bir veri kümesindeki özellikleri belirlemek için Azure Machine Learning hizmetinde filtre tabanlı özellik seçimi modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: ac1421c93f1a4ca42d7f1d94bb898c423c380a57
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73517975"
---
# <a name="filter-based-feature-selection"></a>Filtre tabanlı özellik seçimi

Bu makalede, giriş veri kümenizdeki en büyük tahmin gücüne sahip sütunları belirlemek için Azure Machine Learning tasarımcısında [filtre tabanlı özellik seçimi](filter-based-feature-selection.md) modülünün nasıl kullanılacağı açıklanır. 

Genel olarak, *Özellik seçimi* , belirli bir çıktı verildiğinde, girişlerin hangi sütunların daha tahmine sahip olduğunu anlamak için girişlere istatistiksel testlerin uygulanması işlemini ifade eder. [Filtre tabanlı özellik seçimi](filter-based-feature-selection.md) modülü, Pearson bağıntı ve çi-kare değerleri gibi bağıntı yöntemleri dahil olmak üzere, aralarından seçim yapmak için birden çok özellik seçimi algoritması sağlar. 

[Filtre tabanlı özellik seçimi](filter-based-feature-selection.md) modülünü kullandığınızda bir veri kümesi sağlarsınız, etiketi veya bağımlı değişkeni içeren sütunu tanımlayabilir ve sonra özellik önem derecesine göre kullanılacak tek bir yöntem belirtirsiniz.

Modül, tahmine dayalı güce göre derecelendirilen en iyi özellik sütunlarını içeren bir veri kümesi çıkarır. Ayrıca, özelliklerin adlarını ve bunların puanlarını seçili ölçüden çıkartır.  

### <a name="what-is-filter-based-feature-selection-and-why-use-it"></a>Filtre tabanlı özellik seçimi nedir ve neden kullanılır?  

Bu özellik seçimine yönelik bu modül, ilgisiz öznitelikleri belirlemek için seçili ölçümü kullandığınız ve modelinizdeki gereksiz sütunları filtreleyecek şekilde "filtre tabanlı" olarak adlandırılır.  Verilerinize uyan tek bir istatistiksel ölçü seçersiniz ve modül her özellik sütunu için bir puan hesaplar. Sütunlar, özellik puanlarına göre derecelendirilen döndürülür. 

Doğru özellikleri seçerek sınıflandırmanın doğruluğunu ve verimliliğini de artırabilirsiniz. 

Tahmine dayalı modelinizi oluşturmak için genellikle en iyi puanları içeren sütunları kullanırsınız. Özellik seçimi puanlarını kötü olan sütunlar veri kümesinde bırakılabilir ve bir model oluşturduğunuzda yok sayılır.  

### <a name="how-to-choose-a-feature-selection-metric"></a>Özellik seçimi ölçümü seçme

**Filtre tabanlı özellik seçimi** , her sütunda bilgi değerini değerlendirmek için çeşitli ölçümler sağlar.  Bu bölüm, her ölçüm için genel bir açıklama ve nasıl uygulandığını sağlar. Her bir ölçümü kullanmaya yönelik ek gereksinimler, [Teknik notlar](#technical-notes) bölümünde ve her modülün yapılandırılması için [yönergeler](#how-to-configure-filter-based-feature-selection) bölümünde belirtilmiştir.

-   **Pearson bağıntı**  

     Pearson 'un bağıntı istatistiği veya Pearson 'un bağıntı katsayısı, `r` değeri olarak istatistiksel modellerle de bilinir. Her iki değişken için de bağıntı gücünü gösteren bir değer döndürür

     Pearson 'un bağıntı katsayısı, iki değişkenin kovaryansını alarak ve standart sapmalarının çarpımı ile ayırarak hesaplanır. Katsayı, iki değişkendeki ölçek değişikliklerinden etkilenmez.  

-   **Çi kare**  

     İki yönlü kikare test, beklenen değerleri kapatma değerinin gerçek sonuçlara nasıl olduğunu ölçtüğünden oluşan istatistiksel bir yöntemdir. Yöntemi, değişkenlerin rastgele olduğunu varsayar ve bağımsız değişkenlerin yeterli bir örneğinden çizilir. Elde edilen kikare istatistiği, sonuçların beklenen (rastgele) sonuçtan ne kadar olduğunu gösterir.  


> [!TIP]
> Özel özellik seçimi yöntemi için farklı bir seçeneğe ihtiyaç duyuyorsanız, [R betiği Yürüt](execute-r-script.md) modülünü kullanın. 
##  <a name="how-to-configure-filter-based-feature-selection"></a>Filtre tabanlı özellik seçimini yapılandırma

Standart istatistiksel bir ölçüm seçersiniz ve modül bir sütun çifti arasındaki bağıntıyı hesaplar: etiket sütunu ve bir özellik sütunu

1.  İşlem hattınızla **filtre tabanlı özellik seçimi** modülünü ekleyin. Tasarımcı 'daki **Özellik seçimi** kategorisinde bulabilirsiniz.

2. Olası özellikler olan en az iki sütun içeren bir giriş veri kümesini bağlayın.  

    Bir sütunun çözümlenmesi ve bir özellik puanı oluşturulması gerektiğini sağlamak için, **ısfeature** özniteliğini ayarlamak Için [meta verileri Düzenle](edit-metadata.md) modülünü kullanın. 

    > [!IMPORTANT]
    > Giriş olarak sağlayabilmeniz gereken sütunların olası özellikler olduğundan emin olun. Örneğin, tek bir değer içeren bir sütun hiçbir bilgi değeri içermez.
    >
    > Hatalı Özellikler oluşturacak sütunları biliyorsanız, bunları sütun seçiminden kaldırabilirsiniz. Ayrıca, bunları **kategorik**olarak Işaretlemek Için [meta verileri Düzenle](edit-metadata.md) modülünü de kullanabilirsiniz. 
3.  **Özellik Puanlama yöntemi**için, puanları hesaplamada kullanmak üzere aşağıdaki belirlenen istatistiksel yöntemlerden birini seçin.  

    | Yöntem              | Gereksinimler                             |
    | ------------------- | ---------------------------------------- |
    | Pearson bağıntı | Etiket metin veya sayısal olabilir. Özellikler sayısal olmalıdır. |
    Çi kare| Etiketler ve Özellikler metin veya sayısal olabilir. İki kategorik sütunun özellik önemini hesaplamak için bu yöntemi kullanın.|

    > [!TIP]
    > Seçili ölçüyü değiştirirseniz, diğer tüm seçimler sıfırlanır, bu nedenle ilk olarak bu seçeneği ayarladığınızdan emin olun!)
4.  Yalnızca daha önce özellik olarak işaretlenen sütunlar için bir puan oluşturmak üzere **Özellik sütunları üzerinde** çalışma seçeneğini belirleyin. 

    Bu seçeneğin işaretini kaldırırsanız, modül, **istenen özellikler sayısında**belirtilen sütun sayısına kadar, diğer bir deyişle ölçütü karşılayan herhangi bir sütun için bir puan oluşturur.  

5.  **Hedef sütun**için, etiket sütununu ada veya dizine göre (dizinler tek tabanlı) seçmek için **sütun seçiciyi Başlat** ' a tıklayın.  

     İstatistiksel bağıntı içeren tüm yöntemler için bir etiket sütunu gereklidir. Etiket sütunu veya birden çok etiket sütunu seçerseniz modül bir tasarım zamanı hatası döndürür. 

6.  **İstenen özellik sayısı**için, sonuç olarak döndürülmesini istediğiniz özellik sütunlarının sayısını yazın.  

     - Belirtebileceğiniz özellik sayısı alt sınırı 1 ' dir, ancak bu değeri artırmanız önerilir.  

     - İstenen özelliklerin sayısı veri kümesindeki sütun sayısından büyükse, tüm özellikler, sıfır puanlarla birlikte döndürülür.  

    - Özellik sütunlarından daha az sonuç sütunu belirtirseniz, Özellikler azalan puana göre sıralanır ve yalnızca en üstteki Özellikler döndürülür. 

7.  İşlem hattını çalıştırın veya [filtre tabanlı özellik seçimi](filter-based-feature-selection.md) modülünü seçin ve ardından **Seçileni Çalıştır**' a tıklayın.


## <a name="results"></a>Sonuçlar

İşlem tamamlandıktan sonra:

+ Analiz edilen Özellik sütunlarının tam listesini ve puanlarını görmek için, modüle sağ tıklayın, **Özellikler**' i seçin ve **Görselleştir**' e tıklayın.  

+ Özellik seçim ölçütleriniz temel alınarak oluşturulan veri kümesini görüntülemek için, modüle sağ tıklayın, **veri kümesi**' ni seçin ve **Görselleştir**' e tıklayın. 

Veri kümesi beklediğinizden daha az sütun içeriyorsa, modül ayarlarını ve girdi olarak belirtilen sütunların veri türlerini denetleyin. Örneğin, **istenen özelliklerin sayısını** 1 olarak ayarlarsanız, çıkış veri kümesi yalnızca iki sütun içerir: etiket sütunu ve en yüksek dereceli özellik sütunu.


##  <a name="technical-notes"></a>Teknik notlar  

### <a name="implementation-details"></a>Uygulama ayrıntıları

Bir sayısal özellikte ve kategorik bir etikette Pearson bağıntı kullanırsanız, özellik puanı aşağıdaki gibi hesaplanır:  

1.  Kategorik sütunundaki her bir düzey için sayısal sütunun koşullu ortalaması olarak hesaplama yapın.  

2.  Koşullu sütunu, sayısal sütunla bağıntılı.  

### <a name="requirements"></a>Gereksinimler  

-   **Etiket** veya **puan** sütunu olarak belirlenmiş herhangi bir sütun için bir özellik seçimi puanı üretilemez.  

-   Yöntemi tarafından desteklenmeyen bir veri türü sütunuyla bir Puanlama yöntemi kullanmaya çalışırsanız, modül bir hata oluşturacak ya da sütuna sıfır puanı atanır.  

-   Bir sütun mantıksal (true/false) değerler içeriyorsa, bunlar doğru = 1 ve yanlış = 0 olarak işlenir.  

-   Bir sütun **etiket** veya **puan**olarak belirlendiyse bir özellik olamaz.  

### <a name="how-missing-values-are-handled"></a>Eksik değerler nasıl işlenir  

-   Tüm eksik değerlere sahip herhangi bir sütun hedef (etiket) sütunu olarak belirtemezsiniz.  

-   Bir sütun eksik değerler içeriyorsa, sütun puanı hesaplanırken bunlar yok sayılır.  

-   Özellik sütunu olarak belirlenmiş bir sütunda tüm eksik değerler varsa, sıfır puanı atanır.   


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 

