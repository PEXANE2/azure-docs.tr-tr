---
title: Model yorumlanabilirliği
titleSuffix: Azure Machine Learning
description: Modelinizin Azure Machine Learning SDK 'Yı kullanarak neden tahmin sağladığını açıklayacağınızı öğrenin. Bu, modelinizin tahminleri nasıl yaptığını anlamak için eğitim ve çıkarım sırasında kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 06/21/2019
ms.openlocfilehash: 703416d2df103c1ac8db06cbef43600058c250ef
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350475"
---
# <a name="model-interpretability-with-azure-machine-learning"></a>Azure Machine Learning ile model yorumlenebilirliği

Bu makalede, modelinizin, Azure Machine Learning Python SDK 'sının çeşitli yorumlamalar paketleriyle neden olduğunu nasıl anlatacağınızı öğreneceksiniz.

SDK 'daki sınıfları ve yöntemleri kullanarak şunları edinebilirsiniz:
+ Hem ham hem de mühendislik uygulanmış özellikler için özellik önem değerleri
+ Eğitim ve çıkarım sırasında gerçek dünyada veri kümelerinde ölçeklendirerek uyumluluk.
+ Eğitim sırasında veri ve açıklamaları içindeki desenlerin bulunmasına yardımcı olacak etkileşimli görselleştirmeler

Geliştirme döngüsünün eğitim aşamasında, model tasarımcıları ve değerlendiricileri, hipotezleri ve paydaşlarla derlemeyi doğrulamak için bir modelin yorum çıktısını kullanabilir.  Ayrıca, hata ayıklama için modellerle ilgili öngörüleri, model davranışının amaçlarını eşleştirirken doğrulamak ve farkı denetlemek için kullanır.

Machine Learning 'de **Özellikler** , hedef veri noktasını tahmin etmek için kullanılan veri alanlarıdır. Örneğin, kredi riskini tahmin etmek için yaş, hesap boyutu ve hesap yaşı için veri alanları kullanılıyor olabilir. Bu durumda, yaş, hesap boyutu ve hesap yaşı **özelliklerdir**. Özellik önemi, her bir veri alanının modelin tahminlerini nasıl etkilediğine bildirir. Örneğin, yaş, hesap boyutu ve yaş açısından tahmin doğruluğunu önemli ölçüde etkilemediğinden, tahminde yoğun bir şekilde kullanılıyor olabilir. Bu işlem, veri bilimcilerinin ortaya çıkan tahminleri açıklamasına olanak tanır. bu sayede, hissedarların modelde hangi veri noktalarının en önemlileri olduğuna ilişkin görünürlüğü vardır.

Bu araçları kullanarak, makine öğrenimi modellerini **tüm verilerde küresel**olarak veya **belirli bir veri noktasında yerel olarak** , kullanımı kolay ve ölçeklenebilir bir şekilde, son teknoloji olan teknolojileri kullanarak tanımlayabilirsiniz.

Yorumlenebilirlik sınıfları birden çok SDK paketi aracılığıyla kullanılabilir hale getirilir. [Azure Machine Learning IÇIN SDK paketlerini yüklemeyi](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)öğrenin.

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), Microsoft tarafından desteklenen işlevleri içeren ana paket.

* `azureml.contrib.explain.model`, önizleme ve deneyebilecek deneysel işlevleri.

* `azureml.train.automl.automlexplainer`otomatik makine öğrenimi modellerini yorumlama paketi.

> [!IMPORTANT]
> `contrib` Ad alanındaki içerik tam olarak desteklenmez. Deneysel işlevler, diğer bir deyişle, bu, yavaş olarak ana ad alanına taşınır.

## <a name="how-to-interpret-your-model"></a>Modelinizi yorumlama

Modelin genel davranışını veya belirli tahminleri anlamak için yorumlamalar sınıflarını ve yöntemlerini uygulayabilirsiniz. İlki genel açıklama olarak adlandırılır ve ikincisi yerel açıklama olarak adlandırılır.

Yöntemler Ayrıca, yöntemin model belirsiz veya modele özgü olup olmadığına göre kategorize edilebilir. Bazı yöntemler belirli model türlerini hedeflemelidir. Örneğin, SHAP 'nin Tree açıklama yalnızca ağaç tabanlı modeller için geçerlidir. Bazı yöntemler, modeli taklit açıklama veya SHAP 'nin çekirdek açıklama gibi siyah bir kutu olarak değerlendirir. `explain` Paket, veri kümelerine, model türlerine ve kullanım örneklerine göre bu farklı yaklaşımlar kullanır.

Çıktı, belirli bir modelin tahminini nasıl yaptığı hakkında bilgi kümesidir, örneğin:
* Küresel/yerel göreli Özellik önemi

* Küresel/yerel özellik ve tahmin ilişkisi

### <a name="explainers"></a>Explainers

İki explainers kümesi vardır: SDK 'da doğrudan explainers ve meta explainers.

__Doğrudan explainers__ , tümleşik kitaplıklardan gelir. SDK, ortak bir API ve çıkış biçimi kullanıma sunabilmeleri için tüm explainers sarmalanır. Bu explainers doğrudan kullanmaya daha rahat bir şekilde yararlandıysanız, ortak API ve çıkış biçimini kullanmak yerine doğrudan bunları çağırabilirsiniz. SDK 'da bulunan doğrudan explainers listesi aşağıda verilmiştir:

* **Shap ağacı açıklama**: Ağaç ve Kümelemeler ağaçlarına özgü polinom zaman hızlı SHAP değer tahmini algoritmasına odaklanan SHAP 'nin Tree açıklama.
* **Shap derin açıklama**: SHAP 'nin açıklamasına göre, derin açıklama ", Shap NıP 'lerde açıklanan ayrıntılı bir bağlantı üzerinde bağlantı kurulan derin öğrenme modellerindeki SHAP değerleri için yüksek hızda bir yaklaşık algoritmadır. TensorFlow arka ucunu kullanan TensorFlow modelleri ve keras modelleri desteklenir (Ayrıca, PyTorch için de ön destek vardır) ".
* **Shap çekirdek açıklama**: SHAP 'nin Kernel açıklama, herhangi bir model için SHAP değerlerini tahmin etmek üzere özel ağırlıklı bir yerel doğrusal regresyon kullanır.
* **Benzeme açıklama**: Benzeme açıklama, genel yedek modellerin fikrini temel alır. Genel yedek modeli, siyah bir kutu modelinin tahmin edilebilmesini mümkün olduğunca doğru şekilde tahmin etmek için eğitilen bir doğası gereği ınterbox modelidir. Veri bilimconu, & kara kutu modeli hakkında ekibinizle çizmek için yedek modeli yorumlayabilir. Aşağıdaki yoruma modellerden birini, yedek modeliniz olarak kullanabilirsiniz: LightGBM (LinearExplainableModel), doğrusal regresyon (LinearExplainableModel), Stochastic gradyan Descent explainable model (SGDExplainableModel) ve karar ağacı (DecisionTreeExplainableModel).


* **Permütasyon özelliği önem açıklama**: Permütasyon özelliği önem derecesi, [Breiman 'Nın rastgele ormanlar kağıdına](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) göre önemli olan sınıflandırma ve regresyon modellerini açıklamak için kullanılan bir tekniktir (bkz. Bölüm 10). Yüksek düzeyde, çalışma şekli, veri kümesinin tamamı için bir seferde bir özelliği rastgele karıştırarak ve ilgi çekici performans ölçüsünün ne kadarının azaldığından hesaplama. Değişiklik ne kadar büyükse, bu özellik o kadar önemli olur.

* **Limon açıklama** (`contrib`): Limon SARıSı tabanlı açıklama, yerel yedek modeller oluşturmak için son teknoloji yerel yorumlanmış açıklamalar (LIME) algoritmasını kullanır. Genel yedek modellerinden farklı olarak, açık bir şekilde öngörülere yönelik olarak yerel yedek modellerine yönelik olarak, açık bir şekilde
* **Han metin açıklama** (`contrib`): HAN Text açıklama, belirli bir siyah kutu metin modelinin metin verilerinden model açıklamalarını almak için hiyerarşik bir dikkat ağı kullanır. HAN yedek modelini belirli bir öğretmen modelinin öngörülen çıkışları üzerinde eğiyoruz. Corpus metin genelinde eğitimin üzerinde küresel bir şekilde eğitim ettikten sonra, açıklamalarının doğruluğunu artırmak için belirli bir belge için ince ayar adımı ekledik. HAN, tümce ve sözcük ilgilenilmesi için iki dikkat katmanıyla birlikte çift yönlü bir RNN kullanır. DNN, öğretmen modeli üzerinde eğitilirken ve belirli bir belge üzerinde ince ayar yaptıktan sonra, önemli katmanlardan önemli sözcükleri ayıklayabiliriz. HAN 'ın, metin verileri için LIME veya SHAP 'tan daha doğru olduğunu, ancak eğitim süresi bakımından daha pahalı olduğunu bulduk. Ancak, kullanıcıya yine de yavaş olsa da, bir yandan da bir yandan da bir sözcük katıştırmasını kullanarak ağı başlatma seçeneği sunarak eğitim sırasında geliştirmeler yaptık. Eğitim süresi, uzak bir Azure GPU VM 'sinde HAN çalıştırılarak önemli ölçüde artırılabilir. HAN 'in uygulanması, [' belge sınıflandırması Için hiyerarşik dikkat ağları (Yang et al., 2016) '](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification)bölümünde açıklanmaktadır.


__Meta explainers__ otomatik olarak uygun bir doğrudan açıklama seçer ve verilen model ve veri kümelerine göre en iyi açıklama bilgilerini oluşturur. Meta explainers, tümleştirdiğimiz veya geliştirdiğimiz tüm kitaplıkların (SHAP, LIME, benzeme vb.) faydalanır. SDK 'da kullanılabilen meta explainers şunlardır:

* **Tablo açıklama**: Tablosal veri kümeleri ile kullanılır.
* **Metin açıklama**: Metin veri kümeleri ile kullanılır.
* **Görüntü açıklama**: Görüntü veri kümeleri ile kullanılır.

Doğrudan explainers 'in meta seçimine ek olarak, meta explainers temel kitaplıkların üzerine ek özellikler geliştirin ve doğrudan explainers üzerinden hız ve ölçeklenebilirliği geliştirir.

Şu `TabularExplainer` anda doğrudan Shap explainers çağırmak için aşağıdaki mantığı kullanır:

1. Ağaç tabanlı bir modelde varsa, başka bir model uygulayın `TreeExplainer`
2. DNN modeli ise, başka bir model uygulayın `DeepExplainer`
3. Bunu bir siyah kutu modeli olarak değerlendirin ve SHAP uygulayın`KernelExplainer`

Daha fazla kitaplık SDK `TabularExplainer` ile tümleştirildiği ve her bir açıklama için olumlu ve olumsuz yönleri öğrendiğimiz için yerleşik zeka, daha karmaşık hale gelir.

`TabularExplainer`Ayrıca, doğrudan explainers üzerinde önemli özellikler ve performans iyileştirmeleri de yaptı:

* **Başlatma veri kümesinin Özeti**. Açıklama hızının en önemli olduğu durumlarda, başlatma veri kümesini özetliyoruz ve genel ve yerel açıklamayı hızlandıran küçük bir temsilci örnekleri kümesi oluşturur.
* **Değerlendirme verisi kümesini örnekleme**. Kullanıcı büyük bir değerlendirme örnekleri kümesinde geçerse ancak bunların tümünün değerlendirilmemesi gerekmiyorsa, genel açıklamayı hızlandırmak için örnekleme parametresi true olarak ayarlanabilir.

Aşağıdaki diyagramda, doğrudan ve meta explainers 'in geçerli yapısı gösterilmektedir.

[![Machine Learning Yorumlenebilirlik mimarisi](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Desteklenen modeller

Python `numpy.array`, `pandas.DataFrame` `explain` , veya`iml.datatypes.DenseData`biçimdeki verikümelerindeeğitilentümmodellerSDK'nınyorumlenebilirlikpaketitarafındandesteklenir.`scipy.sparse.csr_matrix`

Açıklama işlevleri, giriş olarak hem modelleri hem de işlem hatlarını kabul eder. Bir model sağlanmışsa, modelin tahmin işlevini `predict` uygulaması veya `predict_proba` scikit kuralına uygun olması gerekir. Bir işlem hattı (ardışık düzen betiğinin adı) sağlanmışsa, açıklama işlevi çalışan ardışık düzen betiğinin bir tahmin döndürdüğünü varsayar. PyTorch, TensorFlow ve keras derin öğrenme çerçeveleri aracılığıyla eğitilen modelleri destekliyoruz.

### <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

`explain` Paket hem yerel hem de uzak işlem hedefleri ile çalışacak şekilde tasarlanmıştır. Yerel olarak çalıştırırsanız, SDK işlevleri hiçbir Azure hizmetine başvurmaz. Azure Machine Learning Işlem sırasında açıklamayı Uzaktan çalıştırabilir ve açıklama bilgilerini Azure Machine Learning çalıştırma geçmişi Hizmetleri ' ne kaydedebilirsiniz. Bu bilgiler günlüğe kaydedildikten sonra, açıklamada bulunan raporlar ve görselleştirmeler Kullanıcı analizine yönelik Azure Machine Learning Çalışma Alanı portalında kullanıma hazırdır.

## <a name="interpretability-in-training"></a>Eğitimin yorumlanmasını

### <a name="train-and-explain-locally"></a>Yerel olarak eğitme ve açıklama

1. Modelinizi yerel bir Jupyter not defterinde eğitme.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Açıklama 'i çağırın: Bir açıklama nesnesini başlatmak için modelinize ve bazı eğitim verilerinizi açıklama oluşturucusuna geçirmeniz gerekir. Ayrıca, açıklamaları ve görselleştirmelerinizi daha bilgilendirici hale getirmek için kullanılacak özellik adlarını ve çıkış sınıfı adlarını (sınıflandırma yapıyorsanız) isteğe bağlı olarak geçirebilirsiniz. Aşağıda, açıklama nesnesinin [tablo](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py)ve [PFIExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.permutation.permutation_importance.pfiexplainer?view=azure-ml-py) kullanarak yerel olarak nasıl örneklendirilecek. `TabularExplainer`, alttaki üç Shap explainers (`TreeExplainer`, `DeepExplainer`, veya `KernelExplainer`) öğesini çağırır ve kullanım durumu için en uygun olanı otomatik olarak seçiyoruz. Ancak, temel alınan üç explainers her birini doğrudan çağırabilirsiniz.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    or

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    from azureml.explain.model.mimic.models.linear_model import LinearExplainableModel,SGDExplainableModel
    from azureml.explain.model.mimic.models.tree_model import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns. 
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```
   or

    ```python
    from azureml.explain.model.permutation.permutation_importance import PFIExplainer 
    
    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model, 
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

3. Genel özellik önem değerlerini alın.

    ```python
    # you can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    
    # if you used the PFIExplainer in the previous step, use the next line of code instead
    # global_explanation = explainer.explain_global(x_train, true_labels=y_test)

    # sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))

    # alternatively, you can print out a dictionary that holds the top K feature names and values
    global_explanation.get_feature_importance_dict()
    ```

4. Yerel Özellik önem değerlerini al: tek bir örneği veya örnek grubunu açıklamak için aşağıdaki işlev çağrılarını kullanın. PFIExplainer yerel açıklamaları desteklemediğini lütfen unutmayın.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

    or

    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:5])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

### <a name="train-and-explain-remotely"></a>Uzaktan eğitme ve açıklama

Azure Machine Learning tarafından desteklenen çeşitli işlem hedefleri üzerinde eğitebilirsiniz, bu bölümdeki örnek, Azure Machine Learning Işlem hedefi kullanarak bunun nasıl yapılacağını gösterir.

1. Yerel bir Jupyter not defterinde eğitim betiği oluşturun (örneğin, run_explainer. Kopyala).

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                x_train, 
                                features=feature_names, 
                                classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. İşlem hedefiniz olarak bir Azure Machine Learning Işlem ayarlama ve eğitim çalıştırmayı gönderme hakkında bilgi edinmek için [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md#amlcompute) konusundaki yönergeleri izleyin.

3. Yerel Jupyter Not defterinize açıklamayı indirin.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Görsel öğeler

Modelinizi anlamak ve yorumlamak için görselleştirme panosunu kullanın:

### <a name="global-visualizations"></a>Küresel görselleştirmeler

Aşağıdaki çizimler, eğitilen modelin, tahmine dayalı ve açıklamalarıyla birlikte küresel bir görünümünü sağlar.

|Çizim|Açıklama|
|----|-----------|
|Veri araştırması| Tahmin değerleriyle birlikte veri kümesine genel bakış.|
|Küresel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, temel alınan modelin genel davranışını anlamak için kullanışlıdır.|
|Açıklama araştırması|Modelin tahmin değerlerinde (veya tahmin değerleri açısından) bir değişiklik yapmaktan bir özelliğin nasıl sorumlu olduğunu gösterir. |
|Özet| Her bir özelliğin tahmin değerinde olduğu etkinin dağılımını göstermek için tüm veri noktalarında imzalı yerel özellik önem değerlerini kullanır.|

[![Görselleştirme panosu genel](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Yerel görselleştirmeler

Verilen veri noktası için yerel özellik önem grafiğini yüklemek üzere, yukarıdaki çizim sırasında herhangi bir zamanda herhangi bir veri noktasına tıklayabilirsiniz.

|Çizim|Açıklama|
|----|-----------|
|Yerel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, belirli bir veri noktasındaki temeldeki modelin yerel davranışını anlamak için kullanışlıdır.|
|Perturbation araştırması|Seçili veri noktasının özellik değerlerini değiştirmenize ve bu değişikliklerin tahmin değerini nasıl etkileyeceğini gözlemlemeye olanak tanır.|
|Bireysel koşullu beklenti (buz)| Bir özellik değiştiğinde veri noktasının tahmin değişikliğini görmek için bir özellik değerini minimum değerden maksimum değere değiştirmenize izin verir.|

[![Görselleştirme Panosu yerel özellik önemi](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Görselleştirme panosu özelliği Perturbation](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Görselleştirme panosu buz çizimleri](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Bir görselleştirme panosunun, Jupyıter çekirdeği başlamadan önce etkinleştirilmiş pencere öğesi uzantılarına sahip olmanız gerekir.

* Jupyter notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.explain.model.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.explain.model.visualize
    ```



* Jupyıter Labs

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```
Görselleştirme panosunu yüklemek için aşağıdaki kodu kullanın:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>Ham Özellik dönüştürmeleri

İsteğe bağlı olarak, dönüşümden önceki ham Özellikler (mühendislik uygulanmış özelliklerden değil) açısından açıklamaları almak için özellik dönüştürme işlem hattınızı açıklama 'e geçirebilirsiniz. Bunu atlarsanız açıklama, uygulanan özellikler açısından açıklamaları sağlar.

Desteklenen dönüşümlerin biçimi, [sköğren-Pandas](https://github.com/scikit-learn-contrib/sklearn-pandas)bölümünde açıklananla aynıdır. Genel olarak, Tüm dönüştürmeler tek bir sütunda çalıştıkları sürece desteklenir ve bu nedenle açıkça bir tane olmak üzere çok daha vardır. 

Ham özellikleri bir `sklearn.compose.ColumnTransformer` veya bir veya bir veya bir veya bir veya bir veya bir veya daha fazla transformatör tanımlama listesi Aşağıdaki hücre kullanır `sklearn.compose.ColumnTransformer`. 

```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=preprocessor)
```

Örneği, birlikte bulunan transformatör tanımlama grupları listesiyle çalıştırmak istiyorsanız aşağıdaki kodu kullanın: 
```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(
    strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(
    handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                      ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=transformations)
```

## <a name="interpretability-at-inferencing-time"></a>Inlekrime sırasında yorumlenebilirlik

Açıklama, özgün modelle birlikte dağıtılabilir ve yerel açıklama bilgilerini sağlamak için Puanlama zamanında kullanılabilir. Ayrıca, aşınma sırasında daha fazla performans sağlamak için daha hafif Puanlama explainers sunuyoruz. Daha hafif bir Puanlama açıklama dağıtma işlemi, model dağıtmaya benzer ve aşağıdaki adımları içerir:




1. Açıklama nesnesi oluşturun (örneğin, tablo Rexplainer kullanarak):

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Açıklama nesnesini kullanarak bir Puanlama açıklama oluşturma:

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Puanlama açıklama modelini kullanan bir görüntüyü yapılandırın ve kaydedin.

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. Seçim Buluttan Puanlama açıklama alın ve açıklamaları test edin

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Görüntüyü bir işlem hedefine dağıtın:

   1. Puanlama dosyası oluşturma (Bu adımdan önce, ilk tahmin modelinizi kaydetmek için [Azure Machine Learning modelleri dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) ' daki adımları izleyin)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import pandas as pd
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Dağıtım yapılandırmasını tanımlayın (Bu yapılandırma, modelinizin gereksinimlerine bağlıdır. Aşağıdaki örnek, bir CPU çekirdeği ve 1 GB bellek kullanan bir yapılandırmayı tanımlar.

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "NAME_OF_THE_DATASET",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for NAME_OF_THE_PROBLEM')
        ```

   1. Ortam bağımlılıklarıyla dosya oluşturma

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

        azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-explain-model', 'azureml-core', 'azureml-telemetry', 'azureml-explain-model']
 

        # specify CondaDependencies obj
        myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                         pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                         pin_sdk_version=False)


        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. G + + yüklenmiş özel bir dockerfile oluşturun

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Oluşturulan görüntüyü Dağıt (süre tahmini: 5 dakika)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ```

1. Dağıtımı test etme

    ```python
    import requests

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Temizle: Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.




## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki yönergeleri gösteren Jupyıter Not defterlerinin bir koleksiyonunu görmek için, [Azure Machine Learning yorumsuz örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model)bakın.
