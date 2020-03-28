---
title: 'Öğretici 2: Tren kredi riski modelleri'
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümünü nasıl oluşturacağımı gösteren ayrıntılı bir öğretici. Bu öğretici, üç bölümlük bir öğretici serisinin ikinci bölümüdür. Modellerin nasıl eğitilip değerlendirileceklerini gösterir.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 8feca17f10bb891f0ca5577b2363f95901da4a46
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79217866"
---
# <a name="tutorial-2-train-credit-risk-models---azure-machine-learning-studio-classic"></a>Öğretici 2: Kredi risk modellerini eğitin - Azure Machine Learning Studio (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu eğitimde, tahmine dayalı bir analitik çözüm geliştirme sürecine uzun bir göz atabilirsiniz. Machine Learning Studio (klasik) basit bir model geliştirmek.  Ardından modeli Azure Machine Learning web hizmeti olarak dağıtırsınız.  Bu dağıtılan model, yeni verileri kullanarak öngörülerde bulunabilir. Bu öğretici **üç bölümlük öğretici serisinin ikinci bölümüdür.**

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi risk değerlendirmesi karmaşık bir sorundur, ancak bu öğretici biraz basitleştirir. Microsoft Azure Machine Learning Studio 'yı (klasik) kullanarak nasıl tahmine dayalı bir analiz çözümü oluşturabileceğinize bir örnek olarak kullanırsınız. Bu çözüm için Azure Machine Learning Studio (klasik) ve Machine Learning web hizmetini kullanırsınız.  

Bu üç bölümlü eğitimde, herkese açık kredi riski verileriyle başlarsınız.  Daha sonra geliştirmek ve bir tahmin modeli eğitmek.  Son olarak modeli bir web hizmeti olarak dağıtın.

[Öğreticinin birinci bölümünde,](tutorial-part1-credit-risk.md)bir Machine Learning Studio (klasik) çalışma alanı oluşturdunuz, veri yükledin ve bir deneme oluşturdunuz.

Öğretici bu bölümünde size:
 
> [!div class="checklist"]
> * Birden fazla model eğitin
> * Modelleri puanla ve değerlendirin


[Öğreticinin üçüncü bölümünde,](tutorial-part3-credit-risk-deploy.md)modeli bir web hizmeti olarak dağıtacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Öğretici tam [bir parçası](tutorial-part1-credit-risk.md).

## <a name="train-multiple-models"></a><a name="train"></a>Birden fazla model eğitin

Makine öğrenimi modelleri oluşturmak için Azure Machine Learning Studio'yu (klasik) kullanmanın avantajlarından biri, tek bir denemede aynı anda birden fazla model türünü deneme ve sonuçları karşılaştırma yeteneğidir. Bu tür denemeler, sorununuz için en iyi çözümü bulmanıza yardımcı olur.

Bu öğreticide geliştirdiğimiz denemede, iki farklı model türü oluşturacak ve son denememizde hangi algoritmayı kullanmak istediğinize karar vermek için puanlama sonuçlarını karşılaştıracaksınız.  

Aralarından seçim yapabileceğiniz çeşitli modeller vardır. Mevcut modelleri görmek için, modül paletindeki **Machine Learning** düğümlerini genişletin ve ardından Model'i ve altındaki düğümleri **Initialize'i** genişletin. Bu denemenin amaçları doğrultusunda, İki [Sınıfdestek Vektör Makinesi][two-class-support-vector-machine] (SVM) ve [İki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modüllerini seçeceksiniz.

> [!TIP]
> Hangi Machine Learning algoritmasının çözmeye çalıştığınız belirli soruna en uygun olduğunu belirlemek [için Microsoft Azure Machine Learning Studio (klasik) için algoritmaları nasıl seçeceğinizi](algorithm-choice.md)öğrenin.
> 
> 

Bu denemeye hem [Iki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modüllerini hem de [İki Sınıf destek vektör makinesi modüllerini][two-class-support-vector-machine] eklersiniz.

### <a name="two-class-boosted-decision-tree"></a>İki Sınıflı Artırmalı Karar Ağacı

İlk olarak, artırılmış karar ağacı modelini ayarlayın.

1. Modül paletindeki [Iki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modüllerini bulun ve tuvale sürükleyin.

1. Tren [Modeli][train-model] modülünü bulun, tuvale sürükleyin ve ardından [Iki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modülünün çıktısını Tren [Modeli][train-model] modülünün sol giriş noktasına bağlayın.
   
   [İki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modülü genel modeli başharfletir ve Train [Model][train-model] modeli eğitmek için eğitim verilerini kullanır. 

1. Sol [daki Execute R Script][execute-r-script] modülünün sol çıktısını Tren [Modeli][train-model] modülünün sağ giriş noktasına bağlayın (bu eğitimde, eğitim için Split Data modülünün sol tarafından gelen [verileri kullandınız).](#train)
   
   > [!TIP]
   > Bu deneme için [Execute R Script][execute-r-script] modülünün iki girişine ve çıktılarından birine ihtiyacınız yoktur, böylece onları bekar bırakabilirsiniz. 
   > 
   > 

Deneyin bu bölümü şimdi şuna benzer:  

![Bir modeli eğitme](./media/tutorial-part2-credit-risk-train/experiment-with-train-model.png)

Şimdi [Tren Modeli][train-model] modülüne modelin Kredi Riski değerini tahmin etmesini istediğinizi söylemeniz gerekiyor.

1. Tren [Modeli][train-model] modülünü seçin. **Özellikler** bölmesinde, **Başlat sütun seçicisini**tıklatın.

1. Tek **bir sütun seç** iletişim kutusunda, **Kullanılabilir Sütunlar**altındaki arama alanına "kredi riski" yazın, aşağıdaki "Kredi riski"ni seçin ve "Kredi riski"ni**>** **Seçili Sütunlara**taşımak için sağ ok düğmesini ( ) tıklatın. 

    ![Tren Modeli modülü için Kredi Riski sütununa birini seçin](./media/tutorial-part2-credit-risk-train/train-model-select-column.png)

1. **Tamam** onay işaretini tıklatın.

### <a name="two-class-support-vector-machine"></a>İki Sınıflı Destek Vektör Makinesi

Sonra, SVM modelini ayarlayın.  

İlk olarak, SVM hakkında küçük bir açıklama. Artırılan karar ağaçları her türlü özellik ile iyi çalışır. Ancak, SVM modülü doğrusal bir sınıflandırıcı oluşturduğundan, oluşturduğu model, tüm sayısal özellikler aynı ölçekte olduğunda en iyi test hatasına sahiptir. Tüm sayısal özellikleri aynı ölçeğe dönüştürmek için bir "Tanh" dönüşümü [(Normalize Veri][normalize-data] modülü ile) kullanırsınız. Bu sayılarımızı [0,1] aralığına dönüştürür. SVM modülü dize özelliklerini kategorik özelliklere ve ardından ikili 0/1 özelliklerine dönüştürür, böylece dize özelliklerini el ile dönüştürmeniz gerekmez. Ayrıca, Kredi Riski sütununa (sütun 21) dönüştürmek istemezsiniz - sayısaldır, ancak modeli tahmin etmek için eğitdiğimiz değerdir, bu yüzden onu rahat bırakmanız gerekir.  

SVM modelini ayarlamak için aşağıdakileri yapın:

1. Modül paletindeki [Iki Sınıf Destek Vektör Makinesi][two-class-support-vector-machine] modüllerini bulun ve tuvalüzerine sürükleyin.

1. [Tren Modeli][train-model] modülüne sağ tıklayın, **Kopyala'yı**seçin ve ardından tuvale sağ tıklayın ve **Yapıştır'ı**seçin. [Tren Modeli][train-model] modülünün kopyası orijinalle aynı sütun seçimine sahiptir.

1. [İki Sınıf Destek Vektör Makinesi][two-class-support-vector-machine] modülünün çıktısını ikinci Tren [Modeli][train-model] modülünün sol giriş noktasına bağlayın.

1. [Normalize Veri][normalize-data] modüllerini bulun ve tuvalüzerine sürükleyin.

1. Sol [Execute R Script][execute-r-script] modülünün sol çıktısını bu modülün girişine bağlayın (bir modülün çıkış portunun birden fazla başka bir modüle bağlanabileceğini fark edin).

1. [Normalize Veri][normalize-data] modülünün sol çıkış bağlantı noktasını ikinci Tren [Modeli][train-model] modülünün sağ giriş noktasına bağlayın.

Denememizin bu kısmı şimdi şuna benzer:  

![İkinci modelin eğitimi](./media/tutorial-part2-credit-risk-train/svm-model-added.png)

Şimdi [Normalize Veri][normalize-data] modüllerini yapılandırın:

1. [Normalize Veri][normalize-data] modülünü seçmek için tıklatın. **Özellikler** bölmesinde, **Dönüşüm yöntemi** parametresi için **Tanh'ı** seçin.

1. **Başlat sütun seçicisini**tıklatın, **Başlat**için "Sütun Yok" seçeneğini belirleyin, ilk açılır listede **Ekle'yi** seçin, ikinci açılır da **sütun türünü** seçin ve üçüncü açılır listede **Sayısal'ı** seçin. Bu, tüm sayısal sütunların (ve yalnızca sayısal) dönüştürüldüğünü belirtir.

1. Bu satırın sağındaki artı işaretini (+) tıklatın - bu bir açılır bırakma sırası oluşturur. İlk açılır da **Dışla'yı** seçin, ikinci açılır da **sütun adlarını** seçin ve metin alanına "Kredi riski" girin. Bu, Kredi Riski sütununun yok sayılması gerektiğini belirtir (bu sütun sayısal olduğundan ve dışlamadıysanız dönüştürüleceği için bunu yapmanız gerekir).

1. **Tamam** onay işaretini tıklatın.  

    ![Normalize Veri modülü için sütun seçin](./media/tutorial-part2-credit-risk-train/normalize-data-select-column.png)


[Normalize Veri][normalize-data] modülü artık Kredi Riski sütunu dışında tüm sayısal sütunlarda Tanh dönüşümü gerçekleştirmek üzere ayarlanmıştır.  

## <a name="score-and-evaluate-the-models"></a>Modelleri puanla ve değerlendirin

eğitimli modellerimizi puanlamak için [Split Data][split] modülü tarafından ayrılan test verilerini kullanırsınız. daha sonra hangilerinin daha iyi sonuçlar ürettiğini görmek için iki modelin sonuçlarını karşılaştırabilirsiniz.  

### <a name="add-the-score-model-modules"></a>Puan Modeli modüllerini ekle

1. Score [Model][score-model] modüllerini bulun ve tuvalüzerine sürükleyin.

1. [İki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree] modülüne bağlı Olan Tren [Modeli][train-model] modülünü Score [Model][score-model] modülünün sol giriş noktasına bağlayın.

1. Doğru [Execute R Script][execute-r-script] modülünü (test verilerimiz) Score [Model][score-model] modülünün sağ giriş noktasına bağlayın.

    ![Skor Modeli modülü bağlı](./media/tutorial-part2-credit-risk-train/score-model-connected.png)

   
   [Puan Modeli][score-model] modülü artık test verilerinden kredi bilgilerini alabilir, model üzerinden çalıştırabilir ve modelin oluşturduğu tahminleri test verilerindeki gerçek kredi riski sütunuyla karşılayabilir.

1. İkinci bir kopya oluşturmak için [Score Model][score-model] modüllerini kopyalayıp yapıştırın.

1. SVM modelinin çıktısını (diğer bir deyişle, [İki Sınıf Destek Vektör Makinesi][two-class-support-vector-machine] modülüne bağlı Olan Tren [Modeli][train-model] modülünün çıkış portunu) ikinci [Puan Modeli][score-model] modülünün giriş noktasına bağlayın.

1. SVM modeli için, eğitim verilerinde yaptığınız gibi test verilerine de aynı dönüşümü yapmanız gerekir. Bu nedenle, ikinci bir kopya oluşturmak ve doğru Execute R [Script][execute-r-script] modülüne bağlamak için [Normalize Veri][normalize-data] modülünü kopyalayın ve yapıştırın.

1. İkinci [Normalize Veri][normalize-data] modülünün sol çıktısını ikinci [Puan Modeli][score-model] modülünün sağ giriş noktasına bağlayın.

    ![Her iki Puan Modeli modülleri bağlı](./media/tutorial-part2-credit-risk-train/both-score-models-added.png)


### <a name="add-the-evaluate-model-module"></a>Model'i Değerlendir modülekle

İki puanlama sonuçlarını değerlendirmek ve karşılaştırmak için bir [Model Değerlendir][evaluate-model] modülü kullanırsınız.  

1. [Model'i Değerlendir][evaluate-model] modüllerini bulun ve tuvale sürükleyin.

1. Artırılmış karar ağacı modeliyle ilişkili [Score Model][score-model] modülünün çıkış bağlantı noktasını [Model Değerlendir][evaluate-model] modülünün sol giriş noktasına bağlayın.

1. Diğer [Score Model][score-model] modülünü doğru giriş bağlantı noktasına bağlayın.  

    ![Bağlı Model modüllerini değerlendirin](./media/tutorial-part2-credit-risk-train/evaluate-model-added.png)


### <a name="run-the-experiment-and-check-the-results"></a>Denemeyi çalıştırın ve sonuçları kontrol edin

Denemeyi çalıştırmak için tuvalin altındaki **ÇALıŞTıR** düğmesini tıklatın. Bu işlem birkaç dakika sürebilir. Her modüldeki dönen gösterge çalıştığını gösterir ve modülün ne zaman tamamlanırolduğunu yeşil onay işareti gösterir. Tüm modüllerin bir onay işareti olduğunda, deneme çalışma tamamlandı.

Deney şimdi şuna benzer:  

![Her iki modelin de değerlendirilmesi](./media/tutorial-part2-credit-risk-train/final-experiment.png)


Sonuçları kontrol etmek [için, Model'i Değerlendir][evaluate-model] modülünün çıkış bağlantı noktasını tıklatın ve **Visualize'ı**seçin.  

[Modeli Değerlendir][evaluate-model] modülü, iki puanlı modelin sonuçlarını karşılaştırmanızı sağlayan bir çift eğri ve ölçüm üretir. Sonuçları Alıcı Operatör Karakteristik (ROC) eğrileri, Hassas/Geri Çağırma eğrileri veya Kaldırma eğrileri olarak görüntüleyebilirsiniz. Görüntülenen ek veriler bir karışıklık matrisi, eğrinin altındaki alan için kümülatif değerler (AUC) ve diğer ölçümleri içerir. Kaydırıcıyı sola veya sağa taşıyarak eşik değerini değiştirebilir ve bunun ölçüm kümesini nasıl etkilediğini görebilirsiniz.  

Grafiğin sağında, ilişkili eğriyi vurgulamak ve aşağıdaki ilişkili ölçümleri görüntülemek için karşılaştırmak için **Puanlı veri kümesini** veya **Puanlı veri kümesini** tıklatın. Eğriler için göstergede, "Puanlı veri kümesi" [Model Değerlendir][evaluate-model] modülünün sol giriş bağlantı noktasına karşılık gelir - bizim durumumuzda, bu artırılmış karar ağacı modelidir. "Karşılaştırmak için puanlı veri seti" doğru giriş bağlantı noktasına karşılık gelir - bizim durumumuzda SVM modeli. Bu etiketlerden birini tıklattığınızda, bu modelin eğrisi vurgulanır ve aşağıdaki grafikte gösterildiği gibi karşılık gelen ölçümler görüntülenir.  

![Modeller için ROC eğrileri](./media/tutorial-part2-credit-risk-train/roc-curves.png)

Bu değerleri inceleyerek, aradığınız sonuçları vermek için hangi modelin en yakın olduğuna karar verebilirsiniz. Farklı modellerdeki parametre değerlerini değiştirerek denemenizi tekrarlayabilir ve yineleyebilirsiniz. 

Bu sonuçları yorumlama ve model performansını alanın bilim ve sanat bu öğreticinin kapsamı dışındadır. Ek yardım için aşağıdaki makaleleri okuyabilirsiniz:
- [Azure Machine Learning Studio'da model performansı nasıl değerlendirilir (klasik)](evaluate-model-performance.md)
- [Azure Machine Learning Studio'da algoritmalarınızı optimize etmek için parametreleri seçin (klasik)](algorithm-parameters-optimize.md)
- [Azure Machine Learning Studio'da model sonuçlarını yorumlama (klasik)](interpret-model-results.md)

> [!TIP]
> Denemeyi her çalıştırdığınızda, bu yinelemenin kaydı Run History'de tutulur. Bu yinelemeleri görüntüleyebilir ve tuvalin altındaki **VIEW RUN HISTORY'yi** tıklatarak herhangi biri lerine geri dönebilirsiniz. **Ayrıca,** açık olandan hemen önce yinelemeye dönmek için Özellikler bölmesinde **Önceki Çalıştır'ı** tıklatabilirsiniz.
> 
> Tuvalin altındaki **SAVE AS'yi** tıklatarak denemenizin herhangi bir yinelemesinin bir kopyasını yapabilirsiniz. 
> Deneme yinelemelerinizde ne denediğinizin kaydını tutmak için denemenin **Özet** ve **Açıklama** özelliklerini kullanın.
> 
> Daha fazla bilgi için Azure [Machine Learning Studio'da (klasik) deneme yinelemelerini yönet'e](manage-experiment-iterations.md)bakın.  
> 
> 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu adımları tamamladınız: 
 
> [!div class="checklist"]
> * Deneme oluşturma
> * Birden fazla model eğitin
> * Modelleri puanla ve değerlendirin

Artık bu veriler için modelleri dağıtmaya hazırsınız.

> [!div class="nextstepaction"]
> [Öğretici 3 - Modelleri dağıtma](tutorial-part3-credit-risk-deploy.md)


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/