---
title: Algoritmaları iyileştirme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) ' de bir algoritma için en uygun parametre kümesini nasıl seçebileceğinizi açıklar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 04148b482cb07665f43df5bd86a77175cbbaf08b
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79218262"
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio algoritmalarınızı iyileştirmek için parametreler seçin (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu konuda, Azure Machine Learning Studio (klasik) ' de bir algoritma için doğru hiper parametre kümesinin nasıl oluşturulacağı açıklanmaktadır. Çoğu makine öğrenimi algoritmaları ayarlanacak parametrelere sahip. Bir model eğitip yaparken bu parametrelerin değerlerini sağlamasını gerekir. Eğitilen modelin çalışıp çalışmadığını seçtiğiniz model parametreleri bağlıdır. En iyi parametre kümesini bulma işlemi *model seçimi*olarak bilinir.



Seçimi modellemek için çeşitli yollar vardır. Machine Learning 'de, çapraz doğrulama, model seçimi için en yaygın olarak kullanılan yöntemlerden biridir ve Azure Machine Learning Studio (klasik) ' de varsayılan model seçme mekanizmasından biridir. Azure Machine Learning Studio (klasik) hem R hem de Python desteklediğinden, R veya Python kullanarak her zaman kendi model seçim mekanizmalarının uygulanmasını sağlayabilirsiniz.

En iyi parametre kümesi bulma işlemi dört adım vardır:

1. **Parametre alanını tanımlayın**: algoritma için önce dikkate almak istediğiniz parametre değerlerini kesin olarak belirleyin.
2. **Çapraz doğrulama ayarlarını tanımlayın**: veri kümesi için çapraz doğrulama katlarının nasıl seçileceğine karar verin.
3. **Ölçüyü tanımlayın**: doğruluk, kök ortalama kare hatası, duyarlık, geri çağırma veya f puanı gibi en iyi parametre kümesini belirlemek için hangi metriğin kullanılacağına karar verin.
4. **Eğitme, değerlendirme ve karşılaştırma**: parametre değerlerinin her benzersiz birleşimi için, çapraz doğrulama tarafından ve tanımladığınız hata ölçümüne göre yürütülür. Değerlendirme ve karşılaştırma sonra en yüksek performansa modeli seçebilirsiniz.

Aşağıdaki görüntüde bunun Azure Machine Learning Studio (klasik) ' de nasıl elde edilebileceği gösterilmektedir.

![En iyi parametre kümesi bulma](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametre alanı tanımlayın
Model başlatma adım kümesi parametresi tanımlayabilirsiniz. Tüm makine öğrenimi algoritmalarının parametre bölmesinde iki farklı mod vardır: *tek parametre* ve *parametre aralığı*. Parametre aralık modu seçin. Parametre aralık modunda her parametre için birden çok değer girebilirsiniz. Metin kutusuna, virgülle ayrılmış değerler girebilirsiniz.

![İki sınıflı artırmalı karar ağacı, tek bir parametre](./media/algorithm-parameters-optimize/fig2.png)

 Alternatif olarak, kılavuzun maksimum ve en düşük noktalarını ve **kullanım aralığı Oluşturucu**ile oluşturulacak toplam punto sayısını tanımlayabilirsiniz. Varsayılan olarak, parametre değerlerini bir doğrusal ölçek üzerinde oluşturulur. Ancak, **günlük ölçeği** işaretlenirse, değerler günlük ölçeğinde oluşturulur (yani, bitişik noktaların oranı, aralarındaki fark yerine sabittir). Tamsayı parametre için bir kısa çizgi kullanarak bir aralığı tanımlayabilirsiniz. Örneğin, "1-10", 1 ile 10 arasındaki (ikisi de dahil) tüm tamsayılar parametre kümesini oluşturur. Karma mod da desteklenir. Örneğin, "1-10, 20, 50" parametre kümesi, 1-10, 20 ve 50 tamsayıları içerir.

![İki sınıflı artırmalı karar ağacı, parametre aralık](./media/algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Çapraz doğrulama hatları tanımlayın
[Bölüm ve örnek][partition-and-sample] modül, verileri rastgele atamak için kullanılabilir. Aşağıdaki örnek yapılandırmada modülü için beş hatları tanımlayın ve Katlama birkaç örnek örneklerine rastgele atayın.

![Bölüm ve örnek](./media/algorithm-parameters-optimize/fig4.png)

## <a name="define-the-metric"></a>Ölçüm tanımlama
Set [model Hyperparameters][tune-model-hyperparameters] modülü, belirli bir algoritma ve veri kümesi için en iyi parametre kümesini seçme için destek sağlar. Modele eğitim ile ilgili diğer bilgilere ek olarak, Bu modülün **Özellikler** bölmesi en iyi parametre kümesini belirlemek için ölçüm içerir. Sınıflandırma ve regresyon algoritmalar için iki farklı aşağı açılan liste kutuları sırasıyla sahiptir. Algoritma odaklanılan bir sınıflandırma algoritmasıdır ise, regresyon ölçüm göz ardı edilir ve bunun tersi de geçerlidir. Bu belirli örnekte, ölçüm **doğruluk**olur.   

![Gözden geçirme parametreleri](./media/algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Eğitim, değerlendirin ve karşılaştırın
Aynı [ayar modeli Hyperparameters][tune-model-hyperparameters] modülü, parametre kümesine karşılık gelen tüm modelleri de hesaplar, çeşitli ölçümleri değerlendirir ve seçtiğiniz ölçümü temel alarak en iyi eğitilen modeli oluşturur. Bu modül, iki zorunlu giriş vardır:

* Deneyimsiz öğrenici
* Veri kümesi

Modül giriş isteğe bağlı bir veri kümesi de vardır. Zorunlu veri kümesi giriş Katlama bilgilerle veri kümesine bağlanın. Veri kümesi herhangi bir Katlama bilgi uygulanmamışsa 10-fold bir çapraz doğrulama otomatik olarak varsayılan olarak yürütülür. Katlama atama yapılmaz ve isteğe bağlı bir veri kümesi bağlantı noktalarından sağlanan doğrulama veri kümesi, eğitin ve test modu seçilir ve ilk veri kümesi için her parametre bileşimi modeli eğitmek için kullanılır.

![Artırmalı karar ağacı Sınıflandırıcısı](./media/algorithm-parameters-optimize/fig6a.png)

Model doğrulama veri kümesinde ardından değerlendirilir. Modülünün sol çıkış bağlantı noktasına parametre değerlerinin işlevleri farklı ölçümlerini gösterir. Doğru çıkış bağlantı noktası, seçilen ölçüye (Bu durumda**doğruluk** ) göre en iyi şekilde uygulanan modele karşılık gelen eğitilen modeli sağlar.  

![Doğrulama veri kümesi](./media/algorithm-parameters-optimize/fig6b.png)

Doğru çıkış bağlantı noktasına görselleştirerek seçilen tam parametreleri görebilirsiniz. Bu model, bir sınama kümesi Puanlama veya çalışır hale getirilen web hizmeti olarak eğitilen bir modelin kaydettikten sonra kullanılabilir.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
