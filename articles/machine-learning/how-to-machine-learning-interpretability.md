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
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: b68d2a72dc18f683f2203429908a536db1b5124a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063996"
---
# <a name="model-interpretability-in-azure-machine-learning"></a>Azure Machine Learning'de model yorumlanabilirliği
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

## <a name="overview-of-model-interpretability"></a>Model yorumlanabilirliğine genel bakış

Yorumlanabilirlik, şirket politikalarına, endüstri standartlarına ve devlet yönetmeliklerine uygunluğu sağlamak için veri bilimciler ve iş karar vericileri için çok önemlidir:
+ Veri bilimciler, bulgularının değerini ve doğruluğunu anlayabilmek için modellerini yöneticilere ve paydaşlara açıklayabilme yeteneğine ihtiyaç duyarlar. 
+ İş karar vericileri, son kullanıcıların güvenlerini kazanmaları ve sürdürmeleri için şeffaflık sağlama becerisine ihtiyaç duyarlar.

Bir makine öğrenme modelini açıklama yeteneğini etkinleştirmek, model geliştirmenin iki ana aşamasında önemlidir:
+ Makine öğrenme modeli geliştirme döngüsünün eğitim aşamasında. Model tasarımcıları ve değerlendiriciler hipotezleri doğrulamak ve paydaşlarla güven oluşturmak için bir modelin yorumlanabilirlik çıktısını kullanabilirler. Ayrıca, hata ayıklama, model davranışını doğrulamak ve yanlı lık veya önemsiz özellikleri denetlemek için modele ilişkin öngörüleri de kullanırlar.
+ Çıkarıcı aşama boyunca, dağıtılan modellerin etrafında şeffaflığa sahip olmak, yöneticilerin modelin nasıl çalıştığını ve kararlarının gerçek hayatta insanlara nasıl davrandığını ve etkilediğini "dağıtıldığında" anlamalarını sağlar. 

## <a name="interpretability-with-azure-machine-learning"></a>Azure Machine Learning ile Yorumlanabilirlik

Bu makalede, Model yorumlanabilirlik kavramlarının SDK'da nasıl uygulandığını öğreniyorsunuz.

SDK'daki sınıfları ve yöntemleri kullanarak şunları elde edebilirsiniz:
+ Hem ham hem de mühendislik özellikleri için özellik önem değerleri
+ Gerçek dünya veri kümelerinde, eğitim ve çıkarım sırasında ölçekte yorumlanabilirlik.
+ Eğitim zamanında veri ve açıklamalardaki örüntülerin keşfinde size yardımcı olacak etkileşimli görselleştirmeler


Makine öğreniminde **özellikler,** hedef veri noktasını tahmin etmek için kullanılan veri alanlarıdır. Örneğin, kredi riskini tahmin etmek için yaş, hesap boyutu ve hesap yaşı için veri alanları kullanılabilir. Bu durumda, yaş, hesap boyutu ve hesap yaşı **özellikleridir.** Özellik önemi, her veri alanının modelin öngörülerini nasıl etkilediğini söyler. Örneğin, hesap boyutu ve yaşı tahmin doğruluğunu önemli ölçüde etkilemezken, yaş tahminde yoğun olarak kullanılabilir. Bu işlem, veri bilimcilerin ortaya çıkan tahminleri açıklamalarına olanak tanır, böylece paydaşlar modelde hangi veri noktalarının en önemli olduğu konusunda görünürlüğe sahiptir.

Bu araçları kullanarak, makine öğrenimi modellerini **tüm verilerüzerinde veya** **yerel olarak belirli bir veri noktasında,** kullanımı kolay ve ölçeklenebilir bir şekilde kullanarak en son teknolojiyi kullanarak açıklayabilirsiniz.

Yorumlanabilirlik sınıfları birden fazla SDK paketi aracılığıyla kullanılabilir hale getirilir. [Azure Machine Learning için SDK paketlerini](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)nasıl yükleyin öğrenin.

* `azureml.interpret`, Microsoft tarafından desteklenen işlevleri içeren ana paket.

* `azureml.contrib.interpret`, önizleme ve deneyebileceğiniz deneysel işlevler.

* `azureml.train.automl.automlexplainer`otomatik makine öğrenme modellerini yorumlamak için paket.

> [!IMPORTANT]
> `contrib` Ad alanındaki içerik tam olarak desteklenmez. Deneysel işlevler olgunlaştıkça, yavaş yavaş ana ad alanına taşınırlar.

## <a name="how-to-interpret-your-model"></a>Modelinizi yorumlama

Modelin genel davranışını veya belirli tahminlerini anlamak için yorumlanabilirlik sınıflarını ve yöntemlerini uygulayabilirsiniz. Eski küresel açıklama denir ve ikinci yerel açıklama denir.

Yöntemler, yöntemin model agnostik mi yoksa modele özgü mü olduğuna göre de kategorize edilebilir. Bazı yöntemler belirli türde modelleri hedeflenebilmiştir. Örneğin, SHAP'ın ağaç açıklayıcısı yalnızca ağaç tabanlı modeller için geçerlidir. Bazı yöntemler modeli taklit açıklayıcı veya SHAP'ın çekirdek açıklayıcısı gibi bir kara kutu olarak ele alabilirsiniz. Paket, `interpret` veri kümelerini, model türlerini ve kullanım örneklerini temel alan bu farklı yaklaşımlardan yararlanır.

Çıktı, belirli bir modelin tahminini nasıl yaptığına ilişkin bir bilgi kümesidir:
* Genel/yerel göreli özellik önemi
* Genel/yerel özellik ve tahmin ilişkisi

### <a name="explainers"></a>Açıklamalayıcılar

Bu paket, yorumlanabilir modelleri eğitmek ve kara kutu AI sistemlerini açıklamaya yardımcı olmak için açık kaynak kodlu bir python paketi olan [Interpret-Community'de](https://github.com/interpretml/interpret-community/)geliştirilen yorumlanabilirlik tekniklerini kullanır. [Interpret-Community,](https://github.com/interpretml/interpret-community/) bu SDK'nın desteklenen açıklayıcılarına ev sahipliği yapmaktadır ve şu anda aşağıdaki yorumlanabilirlik tekniklerini desteklemektedir:

* **SHAP Ağacı Açıklayıcısı**: [SHAP](https://github.com/slundberg/shap)'ın ağaç açıklayıcısı, ağaçlara ve ağaç topluluklarına özgü polinom zamanı hızlı SHAP değer tahmin algoritmasına odaklanır.
* **SHAP Deep Explainer**: [SHAP'ın](https://github.com/slundberg/shap)açıklamasına dayanarak, Deep Explainer ["SHAP NIPS kağıtta](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions)açıklanan DeepLIFT ile bağlantı kuran derin öğrenme modellerinde SHAP değerleri için yüksek hızlı bir yaklaşım algoritmasıdır. TensorFlow arka uç kullanan TensorFlow modelleri ve Keras modelleri desteklenmektedir (PyTorch için ön destek de vardır)".
* **SHAP Lineer Açıklayıcı**: [SHAP](https://github.com/slundberg/shap)'ın Lineer açıklayıcısı, opsiyonel olarak özellikler arası korelasyonları hesaba katmak üzere, doğrusal bir model için SHAP değerlerini hesaplar.

* **SHAP Kernel Explainer**: [SHAP](https://github.com/slundberg/shap)'s Kernel açıklayıcı herhangi bir model için SHAP değerlerini tahmin etmek için özel ağırlıklı yerel lineer regresyon kullanır.
* **Mimic Explainer**: Mimic explainer karakutu modelleri taklit etmek için [küresel vekil modelleri](https://christophm.github.io/interpretable-ml-book/global.html) eğitim fikrine dayanmaktadır. Küresel bir vekil modeli mümkün olduğunca doğru bir kara kutu modelinin tahminleri yaklaşık eğitilmiş bir özünde yorumlanabilir bir modeldir. Veri bilim adamı kara kutu modeli hakkında sonuçlar alabilirsiniz vekil modeli yorumlayabilirsiniz. Aşağıdaki yorumlanabilir modellerden birini vekil modeli olarak kullanabilirsiniz: LightGBM (LGBMExplainableModel), Lineer Regresyon (LinearExplainableModel), Stochastic Gradyan Descent açıklanabilir model (SGDExplainableModel) ve Karar Ağacı ( KararTreeExplainableModel).


* **Permütasyon Özelliği Önem Açıklayıcı**: Permütasyon Özelliği [Önemi, Breiman'ın Rastgele Ormanlar makalesine](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf) göre sınıflandırma ve regresyon modellerini açıklamak için kullanılan bir tekniktir (bkz. bölüm 10). Yüksek düzeyde, çalışma şekli, tüm veri kümesi için bir anda verileri rasgele bir özelliği karıştırmave faiz ölçümünün ne kadar değiştiğini hesaplamaktır. Değişiklik ne kadar büyükse, bu özellik o kadar önemlidir.

* **LIME Explainer** (`contrib`): LIME dayalı , [LIME](https://github.com/marcotcr/lime)Explainer state-of-the-art Yerel yorumlanabilir model-agnostik açıklamalar (LIME) algoritması yerel vekil modelleri oluşturmak için kullanır. Küresel vekil modelleri aksine, LIME bireysel tahminleri açıklamak için yerel vekil modelleri eğitim odaklanır.
* **HAN Metin Açıklayıcısı** (`contrib`): HAN Text Explainer, belirli bir kara kutu metin modeli için metin verilerinden model açıklamaları almak için Hiyerarşik Dikkat Ağı kullanır. Bu belirli bir kara kutu modelinin öngörülen çıkışları HAN vekil modeli trenler. Metin corpus genelinde küresel eğitim den sonra, açıklamaların doğruluğunu artırmak için belirli bir belge için bir ince ayar adımı ekler. HAN, cümle ve kelime dikkati için iki dikkat katmanı içeren çift yönlü bir RNN kullanır. DNN kara kutu modeli üzerinde eğitildikten ve belirli bir belge üzerinde ince ayar yaptıktan sonra, kullanıcı dikkat katmanlarından sözcük önemlerini ayıklayabilir. HAN metin verileri için LIME veya SHAP daha doğru olduğu gösterilmiştir ama eğitim süresi açısından da daha pahalı. Kullanıcıya, eğitim süresini kısaltmak için GloVe kelime katıştırmalarıyla ağı başlatma seçeneği vermek için iyileştirmeler yapılmıştır. Uzak bir Azure GPU VM üzerinde HAN çalıştırılarak eğitim süresi önemli ölçüde artırılabilir. HAN'ın uygulanması ['Belge Sınıflandırması için Hiyerarşik Dikkat Ağları (Yang vd., 2016)'](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification)'' olarak açıklanmıştır.


* **Tabular Açıklayıcı** `TabularExplainer` : Doğrudan [SHAP](https://github.com/slundberg/shap) Açıklayıcıları çağırmak için aşağıdaki mantığı kullanır:

    1. Ağaç tabanlı bir modelse, SHAP `TreeExplainer`uygulayın, başka
    2. Bir DNN modeli ise, SHAP `DeepExplainer`uygulayın, başka
    3. Doğrusal bir modelse, `LinearExplainer`SHAP uygulayın, başka
    3. Bir kara kutu modeli olarak davranın ve SHAP uygulayın`KernelExplainer`


`TabularExplainer`ayrıca doğrudan SHAP Açıklayıcıları üzerinde önemli özellik ve performans geliştirmeleri yapmıştır:

* **Başlatma veri kümesinin özeti.** Açıklama hızının en önemli olduğu durumlarda, başlatma veri kümesini özetler ve hem genel hem de yerel açıklamayı hızlandıran küçük bir temsilci örnekleri kümesi oluştururuz.
* **Değerlendirme veri kümesini örnekleme.** Kullanıcı büyük bir değerlendirme örneği kümesini geçerse, ancak bunların tamamının değerlendirilmesi gerekmezse, genel açıklamayı hızlandırmak için örnekleme parametresi doğru olarak ayarlanabilir.

Aşağıdaki diyagram, doğrudan ve meta açıklayıcıların geçerli yapısını gösterir.

[![Makine Öğrenimi Yorumlanabilirlik Mimarisi](./media/how-to-machine-learning-interpretability/interpretability-architecture.png)](./media/how-to-machine-learning-interpretability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Desteklenen modeller

Python `numpy.array` `pandas.DataFrame`, `iml.datatypes.DenseData`veya `scipy.sparse.csr_matrix` biçimde veri kümeleri üzerinde eğitilmiş tüm modeller `explain` SDK'nın yorumlanabilirlik paketi tarafından desteklenir.

Açıklama işlevleri hem modelleri hem de ardışık hatları girdi olarak kabul eder. Bir model sağlanırsa, modelin tahmin `predict` `predict_proba` işlevini veya Scikit kuralına uygun şekilde uygulaması gerekir. Bir ardışık hatlar (ardışık komut dosyasının adı) sağlanırsa, açıklama işlevi çalışan ardışık komut dosyasının bir öngörü döndürür varsayar. PyTorch, TensorFlow ve Keras derin öğrenme çerçeveleri üzerinden eğitilmiş modelleri destekliyoruz.

### <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

Paket `explain` hem yerel hem de uzaktan işlem hedefleri ile çalışacak şekilde tasarlanmıştır. Yerel olarak çalıştırılırsa, SDK işlevleri hiçbir Azure hizmetine başvurmaz. Açıklamayı Azure Machine Learning Compute'da uzaktan çalıştırabilir ve açıklama bilgilerini Azure Machine Learning Run History Services'da oturum açabilirsiniz. Bu bilgiler günlüğe kaydedildikten sonra, kullanıcı çözümlemesi için Azure Machine Learning çalışma alanında açıklamadan gelen raporlar ve görselleştirmeler kolayca kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Hem yerel hem de Azure Machine Learning uzaktan bilgi işlem kaynaklarında eğitim veren modeller için yorumlanabilirliği etkinleştirmek için [nasıl olunan](how-to-machine-learning-interpretability-aml.md) yapıya bakın. Ek senaryolar için [örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) bakın.
