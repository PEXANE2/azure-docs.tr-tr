---
title: 'Filtre Tabanlı Özellik Seçimi: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: En büyük tahmin gücüne sahip bir veri kümesindeki özellikleri tanımlamak için Azure Machine Learning'deki Filtre Tabanlı Özellik Seçimi modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: c009a98931240e92527035e51fdce3f1c92f5212
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477604"
---
# <a name="filter-based-feature-selection"></a>Filtre Tabanlı Özellik Seçimi

Bu makalede, Azure Machine Learning tasarımcısında Filtre Tabanlı Özellik Seçimi modülünün nasıl kullanılacağı açıklanmaktadır (önizleme). Bu modül, giriş veri kümenizdeki en büyük tahmin gücüne sahip sütunları belirlemenize yardımcı olur. 

Genel olarak, *özellik seçimi,* belirli bir çıktı verilen girdilere istatistiksel testler uygulama işlemini ifade eder. Amaç, hangi sütunların çıktıyı daha tahmin edici olduğunu belirlemektir. Filtre Tabanlı Özellik Seçimi modülü, aralarından seçim yapabileceğiniz birden çok özellik seçimi algoritması sağlar. Modül, Pearson korelasyon ve ki-kare değerleri gibi korelasyon yöntemlerini içerir. 

Filtre Tabanlı Özellik Seçimi modüllerini kullandığınızda, bir veri kümesi sağlar ve etiket veya bağımlı değişkeniçeren sütunu tanımlarsınız. Daha sonra özelliğin önemini ölçmede kullanılacak tek bir yöntem belirtirsiniz.

Modül, tahmin gücüne göre sıralanan en iyi özellik sütunlarını içeren bir veri kümesi çıkar. Ayrıca, seçilen metrikten özelliklerin adlarını ve puanlarını da çıkar.  

## <a name="what-filter-based-feature-selection-is"></a>Filtre tabanlı özellik seçimi nedir  

Alakasız öznitelikleri bulmak için seçili ölçüt kullandığınızdan, özellik seçimi için bu modüle "filtre tabanlı" adı verilir. Ardından modelinizdeki yedek sütunları filtrelersiniz. Verilerinize uygun tek bir istatistiksel ölçü seçersiniz ve modül her özellik sütunu için bir puan hesaplar. Sütunlar, özellik puanlarına göre sıralanır şekilde döndürülür. 

Doğru özellikleri seçerek, sınıflandırmanın doğruluğunu ve verimliliğini potansiyel olarak artırabilirsiniz. 

Genellikle tahmine dayalı modelinizi oluşturmak için yalnızca en iyi puanlara sahip sütunları kullanırsınız. Düşük özellik seçim puanlarına sahip sütunlar veri kümesinde bırakılabilir ve bir model oluşturduğunuzda yoksayılabilir.  

## <a name="how-to-choose-a-feature-selection-metric"></a>Özellik seçimi ölçümü nasıl seçilir?

Filtre Tabanlı Özellik Seçimi modülü, her sütundaki bilgi değerini değerlendirmek için çeşitli ölçümler sağlar. Bu bölümde, her metnin genel bir açıklaması ve nasıl uygulandığı sağlanmıştır. Teknik [notlarda](#technical-notes) ve her modülü yapılandırma [kullanabilirsiniz yönergelerinde](#how-to-configure-filter-based-feature-selection) her metrik kullanmak için ek gereksinimleri bulabilirsiniz.

-   **Pearson korelasyon**  

    Pearson korelasyon istatistik, ya da Pearson korelasyon katsayısı, `r` aynı zamanda değer olarak istatistiksel modellerde bilinir. Herhangi iki değişken için, bağıntıgücünü gösteren bir değer döndürür.

    Pearson'ın korelasyon katsayısı, iki değişkenin covariance alarak ve standart sapmaların çarpımı ile bölünerek hesaplanır. İki değişkendeki ölçek değişiklikleri katsayıyı etkilemez.  

-   **Chi kare**  

    İki yönlü ki-kare testi, beklenen değerlerin gerçek sonuçlara ne kadar yakın olduğunu ölçen istatistiksel bir yöntemdir. Yöntem, değişkenlerin rasgele olduğunu ve bağımsız değişkenlerin yeterli bir örneğinden çekildiğini varsayar. Elde edilen ki-kare istatistik, sonuçların beklenen (rastgele) sonuçtan ne kadar uzakta olduğunu gösterir.  


> [!TIP]
> Özel özellik seçimi yöntemi için farklı bir seçeneğe ihtiyacınız varsa, [Yürüt R Script](execute-r-script.md) modüllerini kullanın. 

## <a name="how-to-configure-filter-based-feature-selection"></a>Filtre Tabanlı Özellik Seçimi nasıl yapılandırılır?

Standart bir istatistiksel metrik seçin. Modül, bir sütun çifti arasındaki ilişkiyi hesaplar: etiket sütunu ve özellik sütunu.

1.  Filtre Tabanlı Özellik Seçimi modüllerini ardınıza ekleyin. Bunu tasarımcıda **Özellik Seçimi** kategorisinde bulabilirsiniz.

2. Olası özellikler içeren en az iki sütun içeren bir giriş veri kümesi bağlayın.  

    Bir sütunun analiz edildiğinden ve bir özellik puanı oluşturulduğundan emin olmak **için, IsFeature** özniteliğini ayarlamak için [Meta Verileri Edit](edit-metadata.md) modüllerini kullanın. 

    > [!IMPORTANT]
    > Girdi olarak sağladığınız sütunların potansiyel özellikler olduğundan emin olun. Örneğin, tek bir değer içeren bir sütunun bilgi değeri yoktur.
    >
    > Bazı sütunların kötü özellikler yapacağını biliyorsanız, bunları sütun seçiminden kaldırabilirsiniz. Ayrıca, Bunları **Kategorik**olarak işaretlemek için [Meta Verileri Edit](edit-metadata.md) modüllerini de kullanabilirsiniz. 
3.  **Özellik puanlama yöntemi**için, puanların hesaplanmasında kullanmak üzere aşağıdaki yerleşik istatistiksel yöntemlerden birini seçin.  

    | Yöntem              | Gereksinimler                             |
    | ------------------- | ---------------------------------------- |
    | Pearson korelasyon | Etiket metin veya sayısal olabilir. Özellikler sayısal olmalıdır. |
    Chi kare| Etiketler ve özellikler metin veya sayısal olabilir. İki kategorik sütun için özellik önemini hesaplamak için bu yöntemi kullanın.|

    > [!TIP]
    > Seçili ölçümü değiştirirseniz, diğer tüm seçimler sıfırlanacaktır. Bu yüzden önce bu seçeneği ayarladıklısın.
4.  Yalnızca daha önce özellik olarak işaretlenmiş sütunlar için puan oluşturmak için **özellik sütunlarında Çalıştır** seçeneğini seçin. 

    Bu seçeneği temizlerseniz, modül aksi takdirde kriterleri karşılayan herhangi bir sütun için bir puan oluşturacak, **istenilen özelliklerin sayısı**belirtilen sütun sayısına kadar .  

5.  **Hedef sütun için,** etiket sütununu ada veya dizinine göre seçmek için **Başlat sütun seçicisini** seçin. (Dizinler tek tabanlıdır.)  
    İstatistiksel korelasyon içeren tüm yöntemler için etiket sütunu gereklidir. Etiket sütunu veya birden çok etiket sütunu seçmezseniz, modül bir tasarım zamanı hatası döndürür. 

6.  **İstenilen özellik sayısı**için, sonuç olarak döndürülen istediğiniz özellik sütunlarının sayısını girin:  

    - Belirtebileceğiniz en az özellik sayısı birdir, ancak bu değeri artırmanızı öneririz.  

    - İstenilen özelliklerin belirtilen sayısı veri kümesindeki sütun sayısından büyükse, tüm özellikler döndürülür. Sıfır puanlı özellikler bile döndürülür.  

    - Özellik sütunları olduğundan daha az sonuç sütunu belirtirseniz, özellikler azalan puana göre sıralanır. Yalnızca en iyi özellikler döndürülür. 

7.  Ardışık hatlar dosyasını gönderin veya Filtre Tabanlı Özellik Seçimi modülünü seçin ve ardından **seçili Çalıştır'ı**seçin.


## <a name="results"></a>Sonuçlar

İşlem tamamlandıktan sonra:

+ Analiz edilen özellik sütunlarının ve puanlarının tam listesini görmek için modüle sağ tıklayın ve **Visualize'ı**seçin.  

+ Özellik seçim ölçütlerinize göre veri kümesini görüntülemek için modüle sağ tıklayın ve **Visualize'ı**seçin. 

Veri kümesi beklediğinden daha az sütun içeriyorsa, modül ayarlarını kontrol edin. Ayrıca giriş olarak sağlanan sütunların veri türlerini denetleyin. Örneğin, **istenen özellik sayısını** 1 olarak ayarlarsanız, çıktı veri kümesi yalnızca iki sütun içerir: etiket sütunu ve en yüksek dereceli özellik sütunu.


##  <a name="technical-notes"></a>Teknik notlar  

### <a name="implementation-details"></a>Uygulama ayrıntıları

Sayısal bir özellik ve kategorik etiket üzerinde Pearson korelasyonunu kullanırsanız, özellik puanı aşağıdaki gibi hesaplanır:  

1.  Kategorik sütundaki her düzey için sayısal sütunun koşullu ortalamasını hesaplayın.  

2.  Koşullu araç sütununu sayısal sütunla ilişkilendirin.  

### <a name="requirements"></a>Gereksinimler  

-   **Etiket** veya **Puan** sütunu olarak atanan sütun lar için özellik seçimi puanı oluşturulamaz.  

-   Yöntemin desteklemediği veri türünden bir sütuniçeren bir puanlama yöntemi kullanmaya çalışırsanız, modül bir hata yükseltir. Veya, sütuna sıfır puan atanır.  

-   Bir sütun mantıksal (doğru/yanlış) değerler içeriyorsa, `True = 1` `False = 0`bunlar .  

-   Bir **sütun, Etiket** veya **Puan**olarak atanmışsa bir özellik olamaz.  

### <a name="how-missing-values-are-handled"></a>Eksik değerler nasıl işlenir?  

-   Tüm eksik değerlere sahip herhangi bir sütunu hedef (etiket) sütunu olarak belirtemezsiniz.  

-   Bir sütun eksik değerler içeriyorsa, modül sütunun puanını hesaplarken bunları yok sayar.  

-   Özellik sütunu olarak atanan bir sütunun tüm eksik değerleri varsa, modül sıfır puan atar.   


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

