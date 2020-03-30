---
title: Algoritmaları optimize edin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) bir algoritma için en uygun parametre kümesinin nasıl seçileceğini açıklar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 04148b482cb07665f43df5bd86a77175cbbaf08b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218262"
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da algoritmalarınızı optimize etmek için parametreleri seçin (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu konu, Azure Machine Learning Studio'da (klasik) bir algoritma için doğru hiperparametre kümesinin nasıl seçilebildiğini açıklar. Çoğu makine öğrenme algoritması ayarlamak için parametreleri vardır. Bir modeli eğitirken, bu parametreler için değerler sağlamanız gerekir. Eğitilen modelin etkinliği seçtiğiniz model parametrelerine bağlıdır. En uygun parametreler kümesini bulma işlemi *model seçimi*olarak bilinir.



Model seçimi yapmanın çeşitli yolları vardır. Makine öğreniminde çapraz doğrulama, model seçiminde en yaygın kullanılan yöntemlerden biridir ve Azure Machine Learning Studio'daki varsayılan model seçim mekanizmasıdır (klasik). Azure Machine Learning Studio (klasik) hem R hem de Python'u desteklediği nden, R veya Python'u kullanarak her zaman kendi model seçim mekanizmalarını uygulayabilirsiniz.

En iyi parametre kümesini bulma işleminde dört adım vardır:

1. **Parametre alanını tanımlayın**: Algoritma için öncelikle göz önünde bulundurmak istediğiniz parametre değerlerinin tam olarak karar verin.
2. **Çapraz doğrulama ayarlarını tanımlayın**: Veri kümesi için çapraz doğrulama kıvrımlarının nasıl seçilene karar verin.
3. **Ölçümü tanımla**: Doğruluk, kök ortalama kare hata, kesinlik, geri çağırma veya f-skoru gibi en iyi parametre kümesini belirlemek için hangi ölçümü kullanacağına karar verin.
4. **Train, evaluate ve karşılaştır**: Parametre değerlerinin her benzersiz birleşimi için, çapraz doğrulama tanımladığınız hata ölçümü tarafından ve temel alınarak gerçekleştirilir. Değerlendirme ve karşılaştırmadan sonra, en iyi performans gösteren modeli seçebilirsiniz.

Aşağıdaki resim, Azure Machine Learning Studio'da (klasik) bunun nasıl sağlanabileceğini göstermektedir.

![En iyi parametre kümesini bulun](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametre alanını tanımlama
Model başlatma adımında parametre kümesini tanımlayabilirsiniz. Tüm makine öğrenme algoritmalarının parametre bölmesinin iki eğitmen modu vardır: *Tek Parametre* ve *Parametre Aralığı.* Parametre Aralığı modunu seçin. Parametre Aralığı modunda, her parametre için birden çok değer girebilirsiniz. Metin kutusuna virgülle ayrılmış değerleri girebilirsiniz.

![İki sınıf artırılmış karar ağacı, tek parametre](./media/algorithm-parameters-optimize/fig2.png)

 Alternatif olarak, Kılavuz'un maksimum ve minimum noktalarını ve Kullanım Aralığı **Oluşturucusu**ile oluşturulacak toplam nokta sayısını tanımlayabilirsiniz. Varsayılan olarak, parametre değerleri doğrusal bir ölçekte oluşturulur. Ancak **Günlük Ölçeği** işaretlenirse, değerler günlük ölçeğinde oluşturulur (diğer bir deyişle, bitişik noktaların oranı farkları yerine sabittir). Tamsayı parametreleri için tire kullanarak bir aralık tanımlayabilirsiniz. For example, "1-10" means that all integers between 1 and 10 (both inclusive) form the parameter set. Karışık mod da desteklenir. Örneğin, "1-10, 20, 50" parametresi kümesi 1-10, 20 ve 50'yi içeren bir arayı içerir.

![İki sınıf artırılmış karar ağacı, parametre aralığı](./media/algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Çapraz doğrulama kıvrımlarını tanımlama
[Bölüm ve Örnek][partition-and-sample] modülü verilere rasgele kıvrımatamak için kullanılabilir. Modül için aşağıdaki örnek yapılandırmada, beş kıvrım tanımlar ve örnek örneklerine rasgele bir kat numarası atayabiliriz.

![Bölüm ve örnek](./media/algorithm-parameters-optimize/fig4.png)

## <a name="define-the-metric"></a>Metrik tanımlayın
[Tune Model Hiperparametreler][tune-model-hyperparameters] modülü, belirli bir algoritma ve veri kümesi için en iyi parametre kümesini ampirik olarak seçmek için destek sağlar. Modelin eğitimiyle ilgili diğer bilgilere ek olarak, bu modülün **Özellikler** bölmesi en iyi parametre kümesini belirlemek için metrik içerir. Sınıflandırma ve regresyon algoritmaları için sırasıyla iki farklı açılır liste kutusu vardır. Düşünülen algoritma bir sınıflandırma algoritması ise, regresyon ölçümü göz ardı edilir ve tam tersi. Bu özel örnekte, metrik **Doğruluk'** dur.   

![Süpürme parametreleri](./media/algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Eğitin, değerlendirin ve karşılaştırın
Aynı [Tune Model Hyperparameters][tune-model-hyperparameters] modülü, parametre kümesine karşılık gelen tüm modelleri eğitiyor, çeşitli ölçümleri değerlendiriyor ve ardından seçtiğiniz ölçüğe göre en iyi eğitilmiş modeli oluşturur. Bu modülün iki zorunlu girişi vardır:

* Eğitimsiz öğrenci
* Veri kümesi

Modül de isteğe bağlı bir veri kümesi girişi vardır. Veri kümesini, zorunlu veri kümesi girişine kat bilgileriyle bağlayın. Veri kümesine herhangi bir kat bilgisi atanmamışsa, varsayılan olarak 10 kat çapraz doğrulama otomatik olarak yürütülür. Kat ataması yapılmazsa ve isteğe bağlı veri kümesi bağlantı noktasında bir doğrulama veri kümesi sağlanmışsa, bir tren testi modu seçilir ve her parametre kombinasyonu için modeli eğitmek için ilk veri kümesi kullanılır.

![Artırılmış karar ağacı sınıflandırıcı](./media/algorithm-parameters-optimize/fig6a.png)

Model daha sonra doğrulama veri kümesi nde değerlendirilir. Modülün sol çıkış bağlantı noktası parametre değerlerinin işlevleri olarak farklı ölçümleri gösterir. Doğru çıkış bağlantı noktası seçilen metrik (Bu durumda**doğruluk)** göre en iyi performans modeline karşılık gelen eğitimli modeli verir.  

![Doğrulama veri kümesi](./media/algorithm-parameters-optimize/fig6b.png)

Doğru çıkış bağlantı noktasını görselleştirerek seçilen tam parametreleri görebilirsiniz. Bu model, bir test kümesi puanlama veya eğitimli bir model olarak kaydolduktan sonra operasyonel web hizmeti kullanılabilir.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
