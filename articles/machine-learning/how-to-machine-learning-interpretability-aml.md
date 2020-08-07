---
title: Python 'da ML modellerini yorumlama & açıklama (Önizleme)
titleSuffix: Azure Machine Learning
description: Machine Learning modelinizin Özellik önemini nasıl belirlediğini ve Azure Machine Learning SDK kullanırken tahmine dayalı hale getirmek için nasıl açıklamalar alabileceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: mithigpe
author: minthigpen
ms.reviewer: Luis.Quintanilla
ms.date: 07/09/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 8682342d23c37d527528de0b525dbdd49a52676e
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853407"
---
# <a name="use-the-interpretability-package-to-explain-ml-models--predictions-in-python-preview"></a>Python 'da (Önizleme), ML modellerini & öngörülerini açıklamak için yorumlamalar paketini kullanın

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu nasıl yapılır kılavuzunda, aşağıdaki görevleri gerçekleştirmek için Azure Machine Learning Python SDK 'sının yorumlenebilirlik paketini kullanmayı öğreneceksiniz:


* Tüm model davranışının veya kişisel makinenizde tek tek tahminlerden yerel olarak açıklayın.

* Mühendislik uygulanan özellikler için yorumlenebilirlik tekniklerini etkinleştirin.

* Azure 'da tüm modelin ve bireysel tahminlerinin davranışını açıklayın.

* Model açıklamaları ile etkileşim kurmak için bir görselleştirme panosu kullanın.

* Inkoya sınırlama sırasında açıklamaları gözlemlemek için modelinize birlikte bir Puanlama açıklama dağıtın.



Desteklenen yorumlenebilirlik teknikleri ve makine öğrenimi modelleri hakkında daha fazla bilgi için, Azure Machine Learning ve [örnek not defterlerinde](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) [model yorumlenebilirliğini](how-to-machine-learning-interpretability.md) inceleyin.

## <a name="generate-feature-importance-value-on-your-personal-machine"></a>Kişisel makinenizde Özellik önemi değeri oluştur 
Aşağıdaki örnekte, Azure hizmetleriyle iletişim kurmadan kişisel makinenizde yorumlenebilirlik paketinin nasıl kullanılacağı gösterilmektedir.

1. `azureml-interpret`Ve `azureml-contrib-interpret` paketlerini yükler.
    ```bash
    pip install azureml-interpret
    pip install azureml-contrib-interpret
    ```

2. Yerel bir Jupyter not defterinde örnek modeli eğitme.

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

3. Açıklama yerel olarak çağırın.
   * Bir açıklama nesnesini başlatmak için modelinizi ve bazı eğitim verilerinizi açıklama oluşturucusuna geçirin.
   * Açıklamaları ve görselleştirmelerinizi daha bilgilendirici hale getirmek için sınıflandırma yaparsanız özellik adlarını ve çıkış sınıfı adlarını geçirmeye seçebilirsiniz.

   Aşağıdaki kod blokları `TabularExplainer` ,, `MimicExplainer` ve yerel olarak bir açıklama nesnesinin örneğini oluşturmayı gösterir `PFIExplainer` .
   * `TabularExplainer`Alttaki üç SHAP explainers birini ( `TreeExplainer` , `DeepExplainer` , veya `KernelExplainer` ) çağırır.
   * `TabularExplainer`kullanım durumu için en uygun olanı otomatik olarak seçer, ancak her üç temel explainers doğrudan çağırabilirsiniz.

    ```python
    from interpret.ext.blackbox import TabularExplainer

    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    veya

    ```python

    from interpret.ext.blackbox import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    
    from interpret.ext.glassbox import LGBMExplainableModel
    from interpret.ext.glassbox import LinearExplainableModel
    from interpret.ext.glassbox import SGDExplainableModel
    from interpret.ext.glassbox import DecisionTreeExplainableModel

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

    veya

    ```python
    from interpret.ext.blackbox import PFIExplainer

    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model,
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

### <a name="explain-the-entire-model-behavior-global-explanation"></a>Tüm model davranışını açıklayın (genel açıklama) 

Toplam (genel) özellik önem değerlerini almanıza yardımcı olması için aşağıdaki örneğe bakın.

```python

# you can use the training data or the test data here, but test data would allow you to use Explanation Exploration
global_explanation = explainer.explain_global(x_test)

# if you used the PFIExplainer in the previous step, use the next line of code instead
# global_explanation = explainer.explain_global(x_train, true_labels=y_train)

# sorted feature importance values and feature names
sorted_global_importance_values = global_explanation.get_ranked_global_values()
sorted_global_importance_names = global_explanation.get_ranked_global_names()
dict(zip(sorted_global_importance_names, sorted_global_importance_values))

# alternatively, you can print out a dictionary that holds the top K feature names and values
global_explanation.get_feature_importance_dict()
```

### <a name="explain-an-individual-prediction-local-explanation"></a>Tek bir tahmin açıklayın (yerel açıklama)
Tek bir örneğin veya örnek grubunun açıklamalarını çağırarak farklı veri noktalarının bireysel Özellik önem değerlerini alın.
> [!NOTE]
> `PFIExplainer`Yerel açıklamaları desteklemez.

```python
# get explanation for the first data point in the test set
local_explanation = explainer.explain_local(x_test[0:5])

# sorted feature importance values and feature names
sorted_local_importance_names = local_explanation.get_ranked_local_names()
sorted_local_importance_values = local_explanation.get_ranked_local_values()
```

### <a name="raw-feature-transformations"></a>Ham Özellik dönüştürmeleri

Mühendislik uygulanmış özellikler yerine ham, dönüştürülmemiş özellikler açısından açıklamaları almayı tercih edebilirsiniz. Bu seçenek için, özellik dönüştürme işlem hattınızı içindeki açıklama geçitirsiniz `train_explain.py` . Aksi takdirde açıklama, mühendislik uygulanmış özellikler açısından açıklamaları sağlar.

Desteklenen dönüşümlerin biçimi, [sköğren-Pandas](https://github.com/scikit-learn-contrib/sklearn-pandas)bölümünde açıklananla aynıdır. Genel olarak, Tüm dönüştürmeler tek bir sütunda çalıştıkları sürece desteklenir, böylece bir çok-çok olacak şekilde net bir şekilde çalışır.

Bir `sklearn.compose.ColumnTransformer` veya bir birlikte bulunan transformatör tanımlama gruplarının listesi ile ham özellikler için bir açıklama alın. Aşağıdaki örnek kullanılmıştır `sklearn.compose.ColumnTransformer` .

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

## <a name="generate-feature-importance-values-via-remote-runs"></a>Uzak çalıştırmalar aracılığıyla Özellik önem değerleri oluşturma

Aşağıdaki örnek, `ExplanationClient` uzak çalıştırmalar için model yorumlenebilirliğini etkinleştirmek üzere sınıfını nasıl kullanabileceğinizi gösterir. Yerel işlemle kavramsal olarak benzerdir, ancak şunları yapabilirsiniz:

* `ExplanationClient`Yorumlenebilirlik bağlamını karşıya yüklemek için uzak çalıştırmada öğesini kullanın.
* Bağlamı daha sonra yerel bir ortamda indirin.

1. `azureml-interpret`Ve `azureml-contrib-interpret` paketlerini yükler.
    ```bash
    pip install azureml-interpret
    pip install azureml-contrib-interpret
    ```
1. Yerel bir Jupyter not defterinde eğitim betiği oluşturun. Örneğin, `train_explain.py`.

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run
    from interpret.ext.blackbox import TabularExplainer

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

1. İşlem hedefi olarak bir Azure Machine Learning Işlem kurun ve eğitim çalıştırmanıza iletin. Yönergeler için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md#amlcompute) . [Örnek Not defterlerinin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/azure-integration/remote-explanation) yararlı olduğunu da görebilirsiniz.

1. Yerel Jupyter Not defterinize açıklamayı indirin.

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    
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

Yerel Jupyter Not defterinize açıklamaları indirdikten sonra, modelinizi anlamak ve yorumlamak için görselleştirme panosunu kullanabilirsiniz.

### <a name="understand-entire-model-behavior-global-explanation"></a>Tüm model davranışını anlama (genel açıklama) 

Aşağıdaki çizimler, eğitilen modelin, tahminlerinin ve açıklamalarıyla birlikte genel görünümünü sağlar.

|ZF|Açıklama|
|----|-----------|
|Veri araştırması| Tahmin değerleriyle birlikte veri kümesine genel bir bakış görüntüler.|
|Küresel önem derecesi|, Modelin genel üst K (yapılandırılabilir K) önemli özelliklerini göstermek için tek tek veri noktalarının Özellik önem değerlerini toplar. Temel alınan modelin genel davranışının anlaşılmasına yardımcı olur.|
|Açıklama araştırması|Bir özelliğin modelin tahmin değerlerinde bir değişikliği nasıl etkilediğini veya tahmin değerlerinin olasılığını gösterir. Özellik etkileşiminin etkisini gösterir.|
|Özet önem derecesi|Her bir özelliğin tahmin değeri üzerindeki etkisinin dağıtımını göstermek için tüm veri noktalarında bireysel Özellik önem değerlerini kullanır. Bu diyagramı kullanarak, özellik değerlerinin tahmin değerlerini etkilediği yönü inceleyebilirsiniz.
|

[![Görselleştirme panosu genel](./media/how-to-machine-learning-interpretability-aml/global-charts.png)](./media/how-to-machine-learning-interpretability-aml/global-charts.png#lightbox)

### <a name="understand-individual-predictions-local-explanation"></a>Ayrı tahminleri anlama (yerel açıklama) 

Tüm veri noktaları için tek tek özellik önem kümesini, genel çizbir veri noktasına tıklayarak yükleyebilirsiniz.

|ZF|Açıklama|
|----|-----------|
|Yerel önem derecesi|Tek bir tahmin için en üstteki K (yapılandırılabilir K) önemli özellikleri gösterir. Belirli bir veri noktasındaki temeldeki modelin yerel davranışını göstermeye yardımcı olur.|
|Perturbation Araştırması (Analize)|Seçili veri noktasının özellik değerlerinde değişikliklere izin verir ve sonuç olarak tahmini değişiklikleri gözlemleyin.|
|Bireysel koşullu beklenti (buz)| Özellik değeri değişikliklerinin en küçük değerden en büyük değere değiştirilmesine izin verir. Bir özellik değiştiğinde veri noktasının öngörme şeklini göstermeye yardımcı olur.|

[![Görselleştirme Panosu yerel özellik önemi](./media/how-to-machine-learning-interpretability-aml/local-charts.png)](./media/how-to-machine-learning-interpretability-aml/local-charts.png#lightbox)


[![Görselleştirme panosu özelliği Perturbation](./media/how-to-machine-learning-interpretability-aml/perturbation.gif)](./media/how-to-machine-learning-interpretability-aml/perturbation.gif#lightbox)


[![Görselleştirme panosu buz çizimleri](./media/how-to-machine-learning-interpretability-aml/ice-plot.png)](./media/how-to-machine-learning-interpretability-aml/ice-plot.png#lightbox)

> [!NOTE]
> Jupyıter çekirdeği başlamadan önce görselleştirme panosu için pencere öğesi uzantılarını etkinleştirdiğinizden emin olun.

* Jupyter Notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.interpret.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.interpret.visualize
    ```

* Jupyıterlab

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```

Görselleştirme panosunu yüklemek için aşağıdaki kodu kullanın.

```python
from interpret_community.widget import ExplanationDashboard

ExplanationDashboard(global_explanation, model, dataset=x_test)
```

### <a name="visualization-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'da görselleştirme

[Uzaktan YORUMSİZ](how-to-machine-learning-interpretability-aml.md#generate-feature-importance-values-via-remote-runs) adımları tamamlarınız (üretilen açıklama Azure Machine Learning çalıştırma geçmişine yüklenirken), görselleştirme panosunu [Azure Machine Learning Studio](https://ml.azure.com)'da görüntüleyebilirsiniz. Bu Pano, yukarıda açıklanan görselleştirme panosunun daha basit bir sürümüdür (.net 'te gerçek zamanlı hesaplamalar gerçekleştirebilecek bir etkin işlem olmadığından, açıklama araştırması ve buz çizimleri devre dışı bırakılır).

Veri kümesi, genel ve yerel açıklamalar kullanılabiliyorsa, veriler sekmelerin tamamını (Perturbation araştırması ve buz hariç) doldurur. Yalnızca genel bir açıklama varsa, Özet önem derecesi sekmesi ve tüm yerel açıklama sekmeleri devre dışıdır.

Azure Machine Learning Studio 'daki görselleştirme panosuna erişmek için bu yollardan birini izleyin:

* **Denemeleri** bölmesi (Önizleme)
  1. Azure Machine Learning üzerinde çalıştırdığınız denemeleri listesini görmek için sol bölmedeki **denemeleri** öğesini seçin.
  1. Bu deneyteki tüm çalıştırmaları görüntülemek için belirli bir deneme seçin.
  1. Bir çalıştırma seçin ve ardından **açıklamalar** sekmesini açıklama görselleştirme panosu ' na tıklayın.

   [![Görselleştirme Panosu yerel özellik önemi](./media/how-to-machine-learning-interpretability-aml/amlstudio-experiments.png)](./media/how-to-machine-learning-interpretability-aml/amlstudio-experiments.png#lightbox)

* **Modeller** bölmesi
  1. [Azure Machine Learning ile modelleri dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)bölümündeki adımları izleyerek orijinal modelinizi kaydettiniz, görüntülemek için sol bölmedeki **modeller** ' ı seçebilirsiniz.
  1. Açıklama görselleştirme panosunu görüntülemek için bir model ve ardından **açıklamalar** sekmesini seçin.

## <a name="interpretability-at-inference-time"></a>Çıkarımı zamanında yorumlenebilirlik

Açıklama 'yi özgün modelle birlikte dağıtabilir ve yeni DataPoint için bireysel Özellik önem değerlerini (yerel açıklama) sağlamak üzere çıkarımı zaman içinde kullanabilirsiniz. Ayrıca, çıkarım performansını artırmak için daha hafif Puanlama explainers sunuyoruz. Daha hafif bir Puanlama açıklama dağıtma işlemi, model dağıtmaya benzer ve aşağıdaki adımları içerir:

1. Bir açıklama nesnesi oluşturun. Örneğin, şunu kullanabilirsiniz `TabularExplainer` :

   ```python
    from interpret.ext.blackbox import TabularExplainer


   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Açıklama nesnesiyle bir Puanlama açıklama oluşturun.

   ```python
   from azureml.interpret.scoring.scoring_explainer import KernelScoringExplainer, save

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

1. İsteğe bağlı bir adım olarak, Puanlama açıklama buluttan alabilir ve açıklamaları test edebilirsiniz.

   ```python
   from azureml.interpret.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Aşağıdaki adımları izleyerek görüntüyü bir işlem hedefine dağıtın:

   1. Gerekirse, [modelleri Azure Machine Learning Ile dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)' daki adımları izleyerek orijinal tahmin modelinizi kaydedin.

   1. Puanlama dosyası oluşturma.

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
   1. Dağıtım yapılandırmasını tanımlayın.

         Bu yapılandırma, modelinizin gereksinimlerine bağlıdır. Aşağıdaki örnekte, bir CPU çekirdeği ve bir GB bellek kullanan bir yapılandırma tanımlanmaktadır.

         ```python
         from azureml.core.webservice import AciWebservice

          aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                    memory_gb=1,
                                                    tags={"data": "NAME_OF_THE_DATASET",
                                                          "method" : "local_explanation"},
                                                    description='Get local explanations for NAME_OF_THE_PROBLEM')
         ```

   1. Ortam bağımlılıklarıyla bir dosya oluşturun.

         ```python
         from azureml.core.conda_dependencies import CondaDependencies

         # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

         azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-interpret', 'azureml-core', 'azureml-telemetry', 'azureml-interpret']
 

         # specify CondaDependencies obj
         myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                          pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                          pin_sdk_version=False)


         with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

         with open("myenv.yml","r") as f:
            print(f.read())
         ```

   1. G + + yüklenmiş özel bir dockerfile oluşturun.

         ```python
         %%writefile dockerfile
         RUN apt-get update && apt-get install -y g++
         ```

   1. Oluşturulan görüntüyü dağıtın.
   
         Bu işlem yaklaşık beş dakika sürer.

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

1. Dağıtımı test edin.

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

1. Temizleyin.

   Dağıtılmış bir Web hizmetini silmek için kullanın `service.delete()` .

## <a name="next-steps"></a>Sonraki adımlar

[Model yorumlenebilirliği hakkında daha fazla bilgi edinin](how-to-machine-learning-interpretability.md)

[Gözden geçirin Azure Machine Learning Yorumlenebilirliği örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model)

