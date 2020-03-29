---
title: Ekip Veri Bilimi Sürecinde Özellik Seçimi
description: Özellik seçiminin amacını açıklar ve makine öğreniminin veri geliştirme sürecindeki rollerinden örnekler sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76716674"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Team Data Science Process’te (TDSP) özellik seçimi
Bu makalede, özellik seçiminin amaçları açıklanmaktadır ve makine öğreniminin veri geliştirme sürecindeki rolüne örnekler verilmiştir. Bu örnekler Azure Machine Learning Studio'dan alınmıştır.

Özelliklerin mühendislik ve seçimi, makalede özetlenen Team Data Science Process 'in (TDSP) bir [parçasıdır.](overview.md) Özellik mühendisliği ve seçimi, TDSP'nin **Özellikler Geliştirme** adımının bir parçasıdır.

* **özellik mühendisliği**: Bu işlem, verilerdeki mevcut ham özelliklerden ek ilgili özellikler oluşturmaya ve öğrenme algoritmasına tahmin gücünü artırmaya çalışır.
* **özellik seçimi**: Bu işlem, eğitim sorununun boyutlarını azaltmak amacıyla özgün veri özelliklerinin temel alt kümesini seçer.

Normalde **özellik mühendisliği** ek özellikler oluşturmak için ilk uygulanır ve daha sonra **özellik seçimi** adımı alakasız, gereksiz veya yüksek korelasyon özellikleri ortadan kaldırmak için gerçekleştirilir.

## <a name="filter-features-from-your-data---feature-selection"></a>Verilerinizdeki özellikleri filtreleyin - özellik seçimi
Özellik seçimi sınıflandırma veya regresyon görevleri için kullanılabilir. Amaç, verilerdeki maksimum varyans miktarını temsil etmek için en az özellik kümesini kullanarak boyutlarını azaltan orijinal veri kümesinden özelliklerin bir alt kümesini seçmektir. Bu özellik alt kümesi modeli eğitmek için kullanılır. Özellik seçimi iki ana amac için kullanılır.

* İlk olarak, özellik seçimi genellikle alakasız, gereksiz veya son derece ilişkili özellikleri ortadan kaldırarak sınıflandırma doğruluğunu artırır.
* İkinci olarak, model eğitim sürecini daha verimli hale getiren özellik sayısını azaltır. Verimlilik, destek vektör makineleri gibi eğitmek pahalı olan öğrenciler için önemlidir.

Özellik seçimi modeli eğitmek için kullanılan veri kümesindeki özelliklerin sayısını azaltmaya çalışsa da, "boyutsallık azaltma" terimiyle anılacaktır. Özellik seçimi yöntemleri, verilerdeki orijinal özelliklerin bir alt kümesini değiştirmeden ayıklar.  Boyutsallık azaltma yöntemleri, özgün özellikleri dönüştürebilen ve böylece değiştirebilen tasarlanmış özellikleri kullanır. Boyutsallık azaltma yöntemlerine örnek olarak Temel Bileşen Analizi, kanonik korelasyon analizi ve Tekil Değer Ayrıştırma verilebilir.

Diğerlerinin yanı sıra, denetlenen bir bağlamda yaygın olarak uygulanan özellik seçimi yöntemlerikategorisine "filtre tabanlı özellik seçimi" adı verilir. Bu yöntemler, her özellik ve hedef öznitelik arasındaki korelasyonu değerlendirerek, her özelliğe bir puan atamak için istatistiksel bir ölçü uygular. Özellikler daha sonra, belirli bir özelliği tutmak veya ortadan kaldırmak için eşiğin ayarlanmasına yardımcı olmak için kullanılabilecek puana göre sıralanır. Bu yöntemlerde kullanılan istatistiksel ölçütler arasında Kişi korelasyon, karşılıklı bilgi ve Ki kare testi verilebilir.

Azure Machine Learning Studio'da özellik seçimi için sağlanan modüller vardır. Aşağıdaki şekilde gösterildiği gibi, bu modüller [Filtre Tabanlı Özellik Seçimi][filter-based-feature-selection] ve Fisher [Lineer Ayrımcı Analizi][fisher-linear-discriminant-analysis]içerir.

![Özellik Seçimi modülleri](./media/select-features/feature-Selection.png)

Örneğin, Filtre Tabanlı Özellik [Seçimi][filter-based-feature-selection] modülünün kullanımını göz önünde bulundurun. Kolaylık sağlamak için metin madenciliği örneğini kullanmaya devam edin. [Özellik Karma][feature-hashing] modülü aracılığıyla 256 özellik kümesi oluşturulduktan sonra bir regresyon modeli oluşturmak istediğinizi ve yanıt değişkeninin 1 ile 5 arasında değişen kitap inceleme derecelendirmeleri içeren "Col1" olduğunu varsayalım. "Özellik puanlama yöntemi"ni "Pearson Korelasyon" olarak, "Hedef sütun"u "Col1" ve "İstenilen özellik sayısı" 50 olarak ayarlayarak. Daha sonra Filtre [Tabanlı Özellik Seçimi][filter-based-feature-selection] modülü, hedef özniteliği "Col1" ile birlikte 50 özellik içeren bir veri kümesi üretir. Aşağıdaki şekil, bu deneyin akışını ve giriş parametrelerini gösterir:

![Filtre Tabanlı Özellik Seçimi modülü özellikleri](./media/select-features/feature-Selection1.png)

Aşağıdaki şekil, elde edilen veri kümelerini gösterir:

![Filtre Tabanlı Özellik Seçimi modülü için ortaya çıkan veri kümesi](./media/select-features/feature-Selection2.png)

Her özellik, kendisi ve hedef özniteliği "Col1" arasındaki Pearson Korelasyon'a göre puanlandırılır. En yüksek puana sahip özellikler korunur.

Seçili özelliklerin karşılık gelen puanları aşağıdaki şekilde gösterilmiştir:

![Filtre Tabanlı Özellik Seçimi modülü için puanlar](./media/select-features/feature-Selection3.png)

Bu Filtre [Tabanlı Özellik Seçimi][filter-based-feature-selection] modülü uygulanarak, "Pearson Korelasyon" puanlama yöntemine dayalı hedef değişken "Col1" ile en ilişkili özelliklere sahip oldukları için 256 özellikten 50'si seçilir.

## <a name="conclusion"></a>Sonuç
Özellik mühendisliği ve özellik seçimi, verilerde yer alan temel bilgileri ayıklamagirişiminde bulunan eğitim sürecinin verimliliğini artıran, yaygın olarak tasarlanmış ve seçili iki özelliktir. Ayrıca, giriş verilerini doğru bir şekilde sınıflandırmak ve ilginin sonuçlarını daha sağlam bir şekilde tahmin etmek için bu modellerin gücünü artırırlar. Özellik mühendisliği ve seçimi de öğrenme daha hesaplamalı olarak traktör yapmak için birleştirebilir. Bunu, bir modeli kalibre etmek veya eğitmek için gereken özelliklerin sayısını artırarak ve azaltarak yapar. Matematiksel olarak konuşursak, modeli eğitmek için seçilen özellikler, verilerdeki desenleri açıklayan ve sonuçları başarılı bir şekilde tahmin eden en az bağımsız değişken kümesidir.

Her zaman mutlaka özellik mühendisliği veya özellik seçimi gerçekleştirmek değildir. Gerekip gerekmediği toplanan verilere, seçilen algoritmaya ve denemenin amacına bağlıdır.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

