---
title: ML Studio (klasik)-ekip veri bilimi Işlemi için veri hazırlığı
description: Önceden işleme ve machine learning için etkili bir şekilde kullanılmasını hazırlamak için verileri temizledik.
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
ms.openlocfilehash: caedcf313ab809e9607907545f26ca1b62bbeca7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720053"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Gelişmiş machine learning için verileri hazırlama görevleri
Ön işleme ve temizleme verileri, model eğitimi için bir veri kümesinin kullanılabilmesi için önce gerçekleştirilmesi gereken önemli görevlerdir. Ham veriler genellikle gürültülü ve güvenilmeyen ve değerleri eksik olabilir. Modelleme için bu verileri kullanarak, yanıltıcı sonuçlara neden olabilir. Bu görevler Team Data Science işlem (TDSP) bir parçasıdır ve genellikle ilk İnceleme bulmak ve gerekli ön işleme planlamak için kullanılan bir veri kümesinin izleyin. Özetlenen adımları daha ayrıntılı TDSP işlemi hakkında yönergeler için bkz: [Team Data Science Process](overview.md).

Veri araştırma görevi gibi önceden işleme ve temizleme görevleri, SQL veya Hive veya Azure Machine Learning Studio (klasik) gibi çok çeşitli ortamlarda ve verilerinizin bulunduğu yere bağlı olarak R veya Python gibi çeşitli araçlar ve dillerle gerçekleştirilebilir. depolanır ve nasıl biçimlendirilir. TDSP doğası gereği yinelemeli olduğundan, bu görevleri iş akışı işleminin çeşitli adımları yere alabilir.

Bu makalede, verileri Azure Machine Learning Studio (klasik) uygulamasına geri almadan önce veya sonra alınabilecek çeşitli veri işleme kavramları ve görevleri açıklanır.

Veri keşif ve ön işleme bir örnek Azure Machine Learning Studio (klasik) içinde yapılan bir örnek için, bkz. [önceden işleme veri](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) videosu.

## <a name="why-pre-process-and-clean-data"></a>Neden önceden işleme ve veri temizleme?
Gerçek veriler çeşitli kaynaklardan veri toplandıktan ve işlemleri ve sürdürmenin veya bozuk veri kümesinin kaliteden ödün içerebilir. Ortaya çıkan tipik veri kalite sorunları şunlardır:

* **Tamamlanmamış**: öznitelikler veya eksik değerler içeren veri eksik.
* **Gürültülü**: veri hatalı kayıt veya aykırı değerler içeriyor.
* **Tutarsız**: veri çakışan kayıtları veya tutarsızlıklar içeriyor.

Kalite verileri, kalite Tahmine dayalı modelleri için bir önkoşuldur. "Çöp içinde çöp" kaçının, veri kalitesini artırmak ve performans bu nedenle modellemek için veri sorunları erkenden saptayın ve karşılık gelen veri işleme ve temizleme adımları karar vermek için bir veri sistem durumu ekran yürütmek için zorunludur.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Çalışan bazı tipik veri sistem durumu ekranlar nelerdir?
Biz, veri genel kalitesini denetleyerek denetleyebilirsiniz:

* Sayısını **kayıtları**.
* Sayısını **öznitelikleri** (veya **özellikleri**).
* Öznitelik **veri türleri** (nominal, sıralı veya sürekli).
* Sayısını **eksik değerleri**.
* **İyi biçimlendirilmiş** veriler.
  * TSV veya CSV veri olması durumunda sütun ayırıcıları ve satır ayıraçlarını sütunları ve satırları her zaman doğru ayrı kontrol edin.
  * HTML veya XML biçiminde veriler ise verileri kendi ilgili standartlarına göre biçimlendirilmemiş olup olmadığını denetleyin.
  * Ayrıştırma de bu kadar yapılandırılmış bilgiler yarı yapılandırılmış veya yapılandırılmamış verileri ayıklamak için gerekli olabilir.
* **Tutarsız veri kayıtlarının**. Onay değerleri aralığı izin verilir. Örneğin, veriler öğrenci GPA (sınıf noktası ortalaması) içeriyorsa, GPA 'in belirlenen aralıkta olup olmadığını kontrol edin, 0 ~ 4 deyin.

Verilerle ilgili sorunları bulduğunuzda, **işleme adımları** gereklidir; bu, genellikle eksik değerleri Temizleme, veri normalleştirme, ayırma, metin işleme ve veri hizalamasını etkileyebilecek gömülü karakterleri, ortak alanlardaki karışık veri türlerini ve diğerlerini temizleyen katıştırılmış karakterlerin değiştirilmesini içerir.

**Azure Machine Learning kullanan iyi biçimlendirilmiş bir tablo veri**.  Veriler zaten tablosal formundadır, veri ön işleme Machine Learning doğrudan Azure Machine Learning Studio (klasik) ile gerçekleştirilebilir.  Veri tablosal biçimde, XML'de olduğu say değilse ayrıştırma verileri tablo biçimine dönüştürmek için gerekli olabilir.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Veri ön işleme önemli görevleri bazıları nelerdir?
* **Veri temizleme**: eksik değerleri girin, gürültülü verileri ve aykırı değerleri algılayın ve kaldırın.
* **Veri dönüştürme**: boyutları ve gürültüsünü azaltmak için veri Normalleştir.
* **Veri azaltma**: örnek verileri kayıtları ya da daha kolay veri işleme için öznitelikler.
* **Veri ayrılma**: Dönüştür sürekli özniteliklerin kullanım kolaylığı için kategorik öznitelikleri belirli machine learning yöntemleriyle.
* **Metin Temizleme**: Örneğin, sekmeyle ayrılmış bir veri dosyasındaki katıştırılmış sekmeler, kayıtları kesintiye uğralabilen ekli yeni satırlar gibi, verilerin hizalanmasına neden olabilecek gömülü karakterleri kaldırın.

Aşağıdaki bölümlerde bu veri işleme adımlardan bazıları ayrıntılı olarak açıklanmaktadır.

## <a name="how-to-deal-with-missing-values"></a>Eksik değerleri ile nasıl?
Eksik değerleri ile dağıtmak için öncelikle sorunu daha iyi işleyebilmek için eksik değerleri nedeni tanımlamak idealdir. Tipik eksik değer işleme yöntemler şunlardır:

* **Silme**: eksik değerleri ile kayıt kaldırma
* **Kukla değiştirme**: eksik değerleri bir kukla değer ile değiştirin: Örneğin, *bilinmeyen* kategorik veya 0 için sayısal değerler için.
* **Değiştirme anlamına**: eksik veri sayısal ise, eksik değerlerin ortalaması ile değiştirin.
* **Sık kullanılan değiştirme**: eksik veri kategorik ise, eksik değerleri en sık rastlanan öğeyle değiştirin.
* **Regresyon değiştirme**: eksik değerleri gerileyen değerlerle değiştirmek için bir regresyon yöntemi kullanın.  

## <a name="how-to-normalize-data"></a>Veri'leri normalleştirmek nasıl?
Veri normalleştirme, sayısal değerleri belirtilen bir aralığa yeniden ayırır. Popüler veri normalleştirme yöntemler şunlardır:

* **Min-Maks normalleştirme**: doğrusal olarak bir aralıkta verileri dönüştürme, 0 ve 1 en küçük değer burada ölçeği 0 ve en büyük değeri 1 arasında varsayalım.
* **Z-puanı normalleştirme**: ortalama ve standart sapma göre verileri ölçeklendirmenize: veri ortalaması arasındaki farkı tarafından standart sapma bölün.
* **Ondalık ölçeklendirme**: öznitelik değeri ondalık taşıyarak veri ölçeklendirin.  

## <a name="how-to-discretize-data"></a>Veri ayırmak nasıl?
Veri nominal öznitelikleri veya aralıklarla sürekli değer dönüştürerek ayrılmış. Bu bazı yollar şunlardır:

* **Eşit genişlik gruplama**: bir özniteliğin tüm olası değerler aralığı aynı boyutta N gruplara ayırın ve depo sayı ile denk gelen bir depo değerler atayın.
* **Eşit yükseklik gruplama**: N gruplara, her örneği aynı sayısını içeren bir özniteliğin tüm olası değerler aralığı ayırın ve ardından bin sayı ile denk gelen bir depo değerler atayın.  

## <a name="how-to-reduce-data"></a>Verileri azaltmak nasıl?
Daha kolay veri işleme için veri boyutunu düşürmek için çeşitli yöntemler vardır. Veri boyutu ve etki alanına bağlı olarak, aşağıdaki yöntemleri uygulanabilir:

* **Kayıt örnekleme**: veri kayıtları örnek ve yalnızca temsili bir alt kümesi verileri seçin.
* **Öznitelik örnekleme**: yalnızca bir öznitelik alt kümesinden en önemli verileri seçin.  
* **Toplama**: veri gruplara ayırın ve her grup için sayıları depolayın. Örneğin, günlük gelir rakamları son 20 yılda bir restoran zinciri verilerin boyutunu azaltmak için aylık gelir toplanabilir.  

## <a name="how-to-clean-text-data"></a>Metin verilerini temizlemek nasıl?
**Tablo verilerinde metin alanları** , sütunları hizalamayı ve/veya kayıt sınırlarını etkileyen karakterler içerebilir. Örneğin, sekmeyle ayrılmış bir dosyadaki katıştırılmış sekmeler sütun hizalanmasına neden olur ve katıştırılmış yeni satır karakterleri kayıt satırlarını keser. Metin yazma veya okuma sırasında metin kodlama işleme, yanlış okunamaz karakterlerin (null değerleri gibi) yanlışlıkla kullanıma sunulmasıyla ve metin ayrıştırmayı da etkileyebilecek şekilde hatalı şekilde. Dikkatli ayrıştırma ve düzenleme metin alanları için uygun hizalama ve/veya metin yapılandırılmamış veya yarı yapılandırılmış verilerden ayıklama yapılandırılmış verilere temizlemek için gerekli olabilir.

**Veri keşfi** verileri erken bir görünüm sunar. Bu adım sırasında birkaç veri sorun yazdığı ortaya çıkarıldı olabilir ve karşılık gelen yöntemlere bu sorunları gidermeye yönelik uygulanabilir.  Sorunun kaynağı nedir ve nasıl sorun sunulmuş gibi sorular sormak önemlidir. Bu işlem Ayrıca, bunları çözmek için yapılması gereken veri işleme adımlarına karar vermenize yardımcı olur. Son kullanım durumlarını ve kişiler 'yi tanımlamak, veri işleme çabalarının önceliklerini belirlemek için de kullanılabilir.

## <a name="references"></a>Başvurular
> *Veri madenciliği: Kavramları ve teknikleri*, üçüncü baskı, Morgan Kaufmann Jiawei Han Micheline Kamber ve Jian Pei 2011
> 
> 

