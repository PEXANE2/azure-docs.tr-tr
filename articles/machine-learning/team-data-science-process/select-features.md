---
title: Team Data Science Process içinde özellik seçimi
description: Özellik Seçimi amacını açıklar ve makine öğrenimi veri geliştirme sürecinin içindeki rollerine örnekler sağlar.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1127a470a48660ffffa892d24c9f2991ec64c8e6
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76716674"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Team Data Science Process’te (TDSP) özellik seçimi
Bu makalede, özellik seçimi amaçları açıklanır ve machine Learning veri geliştirme sürecinde rolü örnekleri sağlar. Bu örnekler, Azure Machine Learning Studio'dan çizilir.

Özellik Seçimi ve mühendislik bir parçası olan Team Data Science işlem (makalesinde özetlenen TDSP) olduğu [Team Data Science Process nedir?](overview.md). Özellik Mühendisliği ve seçimi olan bölümleri **geliştirme özellikleri** TDSP. adımı.

* **özellik Mühendisliği**: Bu işlem verilerinde mevcut ham özelliklerinden ilgili ek özellikler oluşturma ve Tahmine dayalı öğrenme algoritmasını güç artırmak için çalışır.
* **Özellik Seçimi**: Bu işlem özgün veri özellikleri anahtar kısmını eğitim sorunun işlenemez azaltmak girişimi seçer.

Normalde **özellik Mühendisliği** ek özellikler oluşturmak için öncelikle uygulanır ve ardından **özellik seçimi** adım, ilgisiz, yedekli ya da son derece bağıntılı özellikleri ortadan kaldırmak için gerçekleştirilir.

## <a name="filter-features-from-your-data---feature-selection"></a>Filtre özellikleri verilerinizi - özellik seçimi
Özellik seçimi, sınıflandırma veya regresyon görevleri için kullanılabilir. Hedef veri varyans en uzun süreyi temsil etmek için en az bir özellik kümesi kullanarak boyutlar azaltılmasına özgün veri kümesinden alınan özelliklerinin bir alt kümesi seçmektir. Bu alt özellikler kümesini modeli eğitmek için kullanılır. Özellik Seçimi iki ana amaca hizmet eder.

* İlk olarak, özellik seçimi sıklıkla ortadan kaldırarak ilgisiz, yedekli sınıflandırma doğruluğu artırır veya özellikleri son derece bağıntılı.
* İkinci olarak, model eğitim işleminden daha verimli hale getirir özellikleri sayısını azaltır. Verimlilik, destek vektör makineleri gibi tren açısından pahalı olan öğrenenler için önemlidir.

Özellik Seçimi modeli eğitmek için kullanılan veri kümesi özellikleri sayısını azaltmak arama olsa da, bunun için "boyut düzeyi azaltma" terimi tarafından başvurmaz. Özellik Seçimi yöntemleri veri özgün özelliklerinin bir alt kümesi, değişiklik yapmadan ayıklayın.  Boyut düzeyi azaltma yöntemleri orijinal özellikler dönüştürebilir ve bu nedenle bunlarda değişiklik mühendislik uygulanan özellikleri kullanın. Asıl bileşen analizi, kurallı bağıntı analiz ve tekil değer ayrıştırma boyut düzeyi azaltma yöntemleri örnekleridir.

Diğerleriyle birlikte, denetlenen bir bağlamda özellik seçimi yöntemleri yaygın olarak uygulanan bir kategorisi "filtre tabanlı özellik seçimi" adı verilir. Her özellik ve hedef öznitelik arasındaki bağıntıyı değerlendirerek bu yöntemler bir puan her bir özellik atamak için istatistiksel bir ölçü uygulayın. Özellikler, ardından tutma veya belirli bir özelliği ortadan eşiği ayarlamanıza yardımcı olması için kullanılabilir puana göre sıralanır. Bu yöntemleri kullanılan istatistiksel ölçüleri kişi bağıntı, karşılıklı bilgileri ve Chi karesini test örneklerindendir.

Azure Machine Learning Studio modülleri için özellik seçimi sağlanan vardır. Aşağıdaki şekilde gösterildiği gibi, bu modüller [filtre tabanlı özellik seçimi][filter-based-feature-selection] ve [Fisher doğrusal ayrılmış minant analizini][fisher-linear-discriminant-analysis]içerir.

![Özellik Seçimi modülleri](./media/select-features/feature-Selection.png)

Örneğin, [filtre tabanlı özellik seçimi][filter-based-feature-selection] modülünün kullanımını göz önünde bulundurun. Kolaylık olması için metin madenciliği örneği kullanarak devam edin. [Özellik karma][feature-hashing] modülü aracılığıyla bir dizi 256 Özellik oluşturulduktan sonra bir regresyon modeli oluşturmak istediğinizi ve yanıt değişkeninin 1 ile 5 arasında defter İnceleme derecelendirmeleri Içeren "Sütun1" olduğunu varsayalım. "Puanlama yöntemi özelliği" ayarlayarak "Pearson bağıntı", "hedef sütun" olacak "Col1" ve "sayısı istenen özellikleri" 50 olarak. Ardından modül [filtresi tabanlı özellik seçimi][filter-based-feature-selection] , "Sütun1" hedef özniteliğiyle birlikte 50 özellikleri içeren bir veri kümesi oluşturur. Aşağıdaki şekil bu deneme ve giriş parametrelerini akışı gösterilmektedir:

![Filtre tabanlı özellik seçimi modül özellikleri](./media/select-features/feature-Selection1.png)

Aşağıdaki şekil, oluşturulan veri kümelerini gösterir:

![Sonuç veri kümesini filtre temel özellik seçimi Modülü](./media/select-features/feature-Selection2.png)

Her bir özellik kendisi ve hedef öznitelik "Col1" arasında Pearson bağıntı göre puanlanır. Üst puanları özelliklerle tutulur.

Seçili özellikleri karşılık gelen puanları, aşağıdaki şekilde gösterilmiştir:

![Filtre temel özellik seçimi modülü için puanları](./media/select-features/feature-Selection3.png)

Bu [filtre tabanlı özellik seçimi][filter-based-feature-selection] modülünü uygulayarak, "Pearson bağıntı" Puanlama metoduna bağlı olarak "Sütun1" hedef değişkeni ile en çok bağıntılı özelliklere sahip olduklarından 50 'den 256 tanesi seçilidir.

## <a name="conclusion"></a>Sonuç
Özellik Mühendisliği ve Özellik seçimi, yaygın olarak oluşturulan iki özelliktir ve seçilen özellikler, veride bulunan anahtar bilgilerini çıkarmaya deneyen Eğitim sürecinin verimliliğini artırır. Bunlar ayrıca giriş verileri doğru bir şekilde sınıflandırmak için ve ilgilendiğiniz sonuçları yerine daha tahmin etmek üzere bu modellerinin gücünden geliştirin. Öğrenme hesaplama açısından daha tractable yapmak için özellik Mühendisliği ve seçimi birleştirebilirsiniz. Bunu geliştirmek ve ardından ayarlamak veya bir model eğitip için gereken özellikleri sayısını azaltmayı yapar. Matematiksel anlamda, modeli eğitmek için seçilen en az sayıda bağımsız değişkenlere, veri desenlerinde açıklamak ve sonuçlar'ı başarıyla tahmin özellikleridir.

Her zaman mutlaka özellik Mühendisliği veya özellik seçimi gerçekleştirmek için değil. Toplanan veriler, seçili algoritması ve deneyde amaç veya gerekli olup olmadığını bağlıdır.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

