---
title: Azure Machine Learning'de model yorumlanabilirliği
titleSuffix: Azure Machine Learning
description: Azure Machine Learning SDK'yı kullanarak modelinizin neden öngörülerde bulunarak tahminde bulunarak nasıl açıklayacağınızı öğrenin. Modelinizin nasıl tahminlerde bulundugunu anlamak için eğitim ve çıkarım sırasında kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: Luis.Quintanilla
ms.date: 04/02/2020
ms.openlocfilehash: fcb837af85a54102e8c9eafc33249af9dba6b5ce
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631407"
---
# <a name="model-interpretability-in-azure-machine-learning"></a>Azure Machine Learning'de model yorumlanabilirliği
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

## <a name="overview-of-model-interpretability"></a>Model yorumlanabilirliğine genel bakış

Yorumlanabilirlik, şirket politikalarına, endüstri standartlarına ve devlet yönetmeliklerine uygunluğu sağlamak için veri bilimciler, denetçiler ve iş karar vericileri için çok önemlidir:

+ Veri bilimcilerin modellerini yöneticilere ve paydaşlara açıklayabilmeleri gerekir, böylece bulgularının değerini ve doğruluğunu anlayabilirler. Ayrıca, modellerini hata ayıklamak ve bunları nasıl geliştireceklerine dair bilinçli kararlar almak için yorumlanabilirlik gerektirirler. 

+ Yasal denetçiler, mevzuata uygunluk la ilgili modelleri doğrulamak ve modellerin kararlarının insanları nasıl etkilediğini izlemek için araçlara ihtiyaç duyarlar. 

+ İş karar vericileri, son kullanıcılara şeffaflık sağlama yeteneğine sahip olarak huzur alabilmeye ihtiyaç duyarlar. Bu onları kazanmak ve güven korumak için izin verir.


Bir makine öğrenme modelini açıklama yeteneğini etkinleştirmek, model geliştirmenin iki ana aşamasında önemlidir:
+ Eğitim aşamasında, model tasarımcıları ve değerlendiriciler hipotezleri doğrulamak ve paydaşlarla güven oluşturmak için bir modelin yorumlanabilirlik çıktısını kullanabilirler. Ayrıca hata ayıklama, model davranışını doğrulamak ve model haksızlığını veya önemsiz özellikleri denetlemek için modele ilişkin öngörüleri de kullanırlar.

+ Çıkarıcı aşama boyunca, dağıtılan modellerin etrafında şeffaflığa sahip olmak, yöneticilerin modelin nasıl çalıştığını ve kararlarının gerçek hayatta insanlara nasıl davrandığını ve etkilediğini "dağıtıldığında" anlamalarını sağlar. 

## <a name="interpretability-with-azure-machine-learning"></a>Azure Machine Learning ile Yorumlanabilirlik

Yorumlanabilirlik sınıfları birden fazla SDK paketi aracılığıyla kullanılabilir: [(Azure Machine Learning için SDK paketlerinin](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)nasıl yüklenir öğrenin )

* `azureml.interpret`, Microsoft tarafından desteklenen işlevleri içeren ana paket.

* `azureml.contrib.interpret`, önizleme ve deneyebileceğiniz deneysel işlevler.

* `azureml.train.automl.automlexplainer`otomatik makine öğrenme modellerini yorumlamak için paket.

Yorumlanabilirlik `pip install azureml-interpret` paketlerini almak için kullanmak ve `pip install azureml-interpret-contrib` genel kullanım için ve `pip install azureml-interpret-contrib` AutoML kullanımı için.


> [!IMPORTANT]
> `contrib` Ad alanındaki içerik tam olarak desteklenmez. Deneysel işlevler olgunlaştıkça, yavaş yavaş ana ad alanına taşınırlar.
.



## <a name="how-to-interpret-your-model"></a>Modelinizi yorumlama

SDK'daki sınıfları ve yöntemleri kullanarak şunları yapabilirsiniz:
+ Tüm model ve/veya tek tek veri noktaları için özellik önem değerleri oluşturarak model tahminini açıklayın. 
+ Eğitim ve çıkarım sırasında, ölçekte gerçek dünya veri kümelerinde model yorumlanabilirliği elde edin.
+ Eğitim zamanında veri ve açıklamalardaki desenleri keşfetmek için etkileşimli görselleştirme panosu kullanın


Makine öğreniminde **özellikler,** hedef veri noktasını tahmin etmek için kullanılan veri alanlarıdır. Örneğin, kredi riskini tahmin etmek için yaş, hesap boyutu ve hesap yaşı için veri alanları kullanılabilir. Bu durumda, yaş, hesap boyutu ve hesap yaşı **özellikleridir.** Özellik önemi, her veri alanının modelin öngörülerini nasıl etkilediğini söyler. Örneğin, hesap boyutu ve yaşı tahmin değerlerini önemli ölçüde etkilemezken, tahminde yaş büyük ölçüde kullanılabilir. Bu işlem, veri bilimcilerin ortaya çıkan tahminleri açıklamalarına olanak tanır, böylece paydaşlar modelde hangi özelliklerin en önemli olduğu konusunda görünürlüğe sahiptir.

Desteklenen yorumlanabilirlik teknikleri, desteklenen makine öğrenimi modelleri ve desteklenen çalışma ortamları hakkında bilgi edinin.


## <a name="supported-interpretability-techniques"></a>Desteklenen yorumlanabilirlik teknikleri

 `azureml-interpret`Yorumlanabilir modelleri eğitmek ve karakutu AI sistemlerinin açıklanmasına yardımcı olmak için açık kaynak kodlu bir python paketi olan [Interpret-Community'de](https://github.com/interpretml/interpret-community/)geliştirilen yorumlanabilirlik tekniklerini kullanır. [Interpret-Community,](https://github.com/interpretml/interpret-community/) bu SDK'nın desteklenen açıklayıcılarına ev sahipliği yapmaktadır ve şu anda aşağıdaki yorumlanabilirlik tekniklerini desteklemektedir:

|Yorumlanabilirlik Tekniği|Açıklama|Tür|
|--|--|--------------------|
|1. SHAP Ağacı Açıklama| [SHAP](https://github.com/slundberg/shap)'s ağaç açıklayıcı, hangi polinom zaman hızlı SHAP değer tahmin algoritması **ağaçlar ve ağaçların toplulukları**özgü odaklanır .|Modele özel|
|2. SHAP Derin Açıklama| [SHAP](https://github.com/slundberg/shap)gelen açıklamaya dayanarak , Deep Explainer ["SHAP NIPS kağıt](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions)açıklanan DeepLIFT ile bir bağlantı üzerine inşa derin öğrenme modellerinde SHAP değerleri için yüksek hızlı bir yaklaşım algoritması . **TensorFlow** arka uç kullanan TensorFlow modelleri ve **Keras** modelleri desteklenmektedir (PyTorch için ön destek de vardır)".|Modele özel|
|3. SHAP Lineer Açıklama| [SHAP'ın](https://github.com/slundberg/shap)Lineer açıklayıcısı, opsiyonel olarak özellikler arası korelasyonları hesaba katmak üzere **doğrusal**bir model için SHAP değerlerini hesaplar.|Modele özel|
|4. SHAP Kernel Açıklayıcı| [SHAP](https://github.com/slundberg/shap)'s Kernel açıklayıcı herhangi bir **model**için SHAP değerlerini tahmin etmek için özel ağırlıklı yerel doğrusal regresyon kullanır.|Model-agnostik|
|5. Mimic Explainer (Küresel Vekil)| Mimic explainer karakutu modelleri taklit etmek için [küresel vekil modelleri](https://christophm.github.io/interpretable-ml-book/global.html) eğitim fikrine dayanmaktadır. Küresel bir vekil modeli mümkün olduğunca doğru **olarak herhangi bir kara kutu modelinin** tahminleri yaklaşık eğitimli bir özünde yorumlanabilir bir modeldir. Veri bilim adamları kara kutu modeli hakkında sonuçlar alabilirsiniz vekil modeli yorumlayabilirsiniz. Aşağıdaki yorumlanabilir modellerden birini vekil modeli olarak kullanabilirsiniz: LightGBM (LGBMExplainableModel), Lineer Regresyon (LinearExplainableModel), Stochastic Gradient Descent açıklanabilir model (SGDExplainableModel) ve Karar Ağacı (DecisionTreeExplainableModel).|Model-agnostik|
|6. Permütasyon Özelliği Önem Açıklayıcı (PFI)| Permütasyon Özelliği Önemi, [Breiman'ın Rastgele Ormanlar makalesine](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf) göre sınıflandırma ve regresyon modellerini açıklamak için kullanılan bir tekniktir (bkz. bölüm 10). Yüksek düzeyde, çalışma şekli, tüm veri kümesi için bir anda verileri rasgele bir özelliği karıştırmave faiz ölçümünün ne kadar değiştiğini hesaplamaktır. Değişiklik ne kadar büyükse, bu özellik o kadar önemlidir. PFI, altta yatan **herhangi bir modelin** genel davranışını açıklayabilir, ancak bireysel öngörüleri açıklamaz. |Model-agnostik|




Yukarıda açıklanan yorumlanabilirlik tekniklerinin yanı sıra, başka bir `TabularExplainer` [SHAP tabanlı açıklayıcıyı](https://github.com/slundberg/shap)destekliyoruz. Modele bağlı olarak, `TabularExplainer` desteklenen SHAP açıklamalarından birini kullanır:

* Tüm ağaç tabanlı modeller için TreeExplainer
* DNN modelleri için DeepExplainer
* Lineer modeller için LinearExplainer
* Diğer tüm modeller için KernelExplainer

`TabularExplainer`ayrıca doğrudan SHAP Açıklayıcıları üzerinde önemli özellik ve performans geliştirmeleri yapmıştır:

* **Başlatma veri kümesinin özeti.** Açıklama hızının en önemli olduğu durumlarda, başlatma veri kümesini özetler ve genel ve bireysel özellik önem değerlerinin oluşturulmasını hızlandıran küçük bir temsili örnek kümesi oluştururuz.
* **Değerlendirme veri kümesini örnekleme.** Kullanıcı büyük bir değerlendirme örneği kümesini geçerse, ancak bunların tamamının değerlendirilmesi gerekmezse, genel model açıklamalarının hesaplanmasını hızlandırmak için örnekleme parametresi doğru olarak ayarlanabilir.

Aşağıdaki diyagram, desteklenen açıklayıcıların geçerli yapısını gösterir.

[![Makine Öğrenimi Yorumlanabilirlik Mimarisi](./media/how-to-machine-learning-interpretability/interpretability-architecture.png)](./media/how-to-machine-learning-interpretability/interpretability-architecture.png#lightbox)


## <a name="supported-machine-learning-models"></a>Desteklenen makine öğrenme modelleri

SDK `azureml.interpret` paketi aşağıdaki veri seti biçimleri ile eğitilmiş modelleri destekler:
- `numpy.array`
- `pandas.DataFrame`
- `iml.datatypes.DenseData`
- `scipy.sparse.csr_matrix`

Açıklama işlevleri hem modelleri hem de ardışık hatları girdi olarak kabul eder. Bir model sağlanırsa, modelin tahmin `predict` `predict_proba` işlevini veya Scikit kuralına uygun şekilde uygulaması gerekir. Modeliniz bunu desteklemiyorsa, modelinizi Scikit'te `predict` veya `predict_proba` Scikit'te aynı sonucu oluşturan bir işleve sarıbilir ve bu sarıcı işlevini seçili açıklamayla kullanabilirsiniz. Bir ardışık hatlar sağlanmışsa, açıklama işlevi çalışan ardışık komut dosyasının bir öngörü döndürür varsayar. Bu sarma tekniğini kullanarak, `azureml.interpret` PyTorch, TensorFlow ve Keras derin öğrenme çerçeveleri yanı sıra klasik makine öğrenme modelleri ile eğitilmiş modelleri destekleyebilir.

## <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

Paket `azureml.interpret` hem yerel hem de uzaktan işlem hedefleri ile çalışacak şekilde tasarlanmıştır. Yerel olarak çalıştırılırsa, SDK işlevleri hiçbir Azure hizmetine başvurmaz. 

Açıklamayı Azure Machine Learning Compute'da uzaktan çalıştırabilir ve açıklama bilgilerini Azure Machine Learning Run History Service'de kaydedebilirsiniz. Bu bilgiler günlüğe kaydedildikten sonra, kullanıcı çözümlemesi için Azure Machine Learning stüdyosunda açıklamadan gelen raporlar ve görselleştirmeler kolayca kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Hem yerel hem de Azure Machine Learning uzaktan bilgi işlem kaynaklarında eğitim veren modeller için yorumlanabilirliği etkinleştirmek için [nasıl olunan](how-to-machine-learning-interpretability-aml.md) yapıya bakın. Ek senaryolar için [örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) bakın.
