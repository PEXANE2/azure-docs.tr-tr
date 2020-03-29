---
title: Yerel ve uzak çalıştırmalar için model yorumlanabilirliği
titleSuffix: Azure Machine Learning
description: Azure Machine Learning SDK'yı kullanırken makine öğrenimi modelinizin özelliğin önemini nasıl belirlediği ve öngörülerde nasıl öngörülerde bulunabildiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: a479982eeac325c9774e3858ec51643e8ba699c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064050"
---
# <a name="model-interpretability-for-local-and-remote-runs"></a>Yerel ve uzak çalıştırmalar için model yorumlanabilirliği

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, modelinizin neden tahminlerini yaptığını anlamak için Azure Machine Learning Python SDK'nın yorumlanabilirlik paketini kullanmayı öğrenirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

* Hem yerel hem de uzaktan bilgi işlem kaynakları üzerinde eğitilmiş makine öğrenimi modellerini yorumlayın.
* Azure Run Geçmişi'nde yerel ve genel açıklamaları depolayın.
* [Azure Machine Learning stüdyosunda](https://ml.azure.com)yorumlanabilirlik görselleştirmelerini görüntüleyin.
* Modelinize bir puanlama açıklayıcısı dağıtın.

Daha fazla bilgi için Azure [Machine Learning'de Model yorumlanabilirliği'ne](how-to-machine-learning-interpretability.md)bakın.

## <a name="local-interpretability"></a>Yerel yorumlanabilirlik

Aşağıdaki örnek, Azure hizmetlerine başvurmadan yerel olarak yorumlanabilirlik paketinin nasıl kullanılacağını gösterir.

1. Gerekirse, yorumlanabilirlik paketi almak için kullanın. `pip install azureml-interpret`

1. Yerel bir Jupyter dizüstü bilgisayarda örnek bir model eğitin.

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

1. Açıklamayı yerel olarak arayın.
   * Bir açıklayıcı nesneyi başlatmayı sağlamak için, modelinizi ve bazı eğitim verilerinizi açıklayıcının oluşturucusuna iletin.
   * Açıklamalarınızı ve görselleştirmelerinizi daha bilgilendirici hale getirmek için, sınıflandırma yapıyorsanız özellik adları ve çıktı sınıf adlarını geçirmeyi seçebilirsiniz.

   Aşağıdaki kod blokları, bir açıklayıcı nesnenin `TabularExplainer`,, `MimicExplainer`ve `PFIExplainer` yerel olarak anlık olarak nasıl anlık olarak nasıl anlayacağını gösterir.
   * `TabularExplainer`altında üç SHAP açıklayıcılarından birini`TreeExplainer`çağırır `DeepExplainer`( `KernelExplainer`, , veya ).
   * `TabularExplainer`kullanım örneğiniz için en uygun olanı otomatik olarak seçer, ancak her üç temel açıklayıcının her birini doğrudan arayabilirsiniz.

    ```python
    from interpret.ext.blackbox import TabularExplainer

    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    or

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

    or

    ```python
    from interpret.ext.blackbox import PFIExplainer

    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model,
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

### <a name="overall-global-feature-importance-values"></a>Genel, genel özellik önem değerleri

Genel özellik önem değerlerini elde etmeye yardımcı olmak için aşağıdaki örneğe bakın.

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

### <a name="instance-level-local-feature-importance-values"></a>Örnek düzeyi, yerel özellik önem değerleri

Tek bir örnek veya bir örnek grubu için açıklamalar çağırarak yerel özellik önem değerlerini alın.
> [!NOTE]
> `PFIExplainer`yerel açıklamaları desteklemez.

```python
# get explanation for the first data point in the test set
local_explanation = explainer.explain_local(x_test[0:5])

# sorted feature importance values and feature names
sorted_local_importance_names = local_explanation.get_ranked_local_names()
sorted_local_importance_values = local_explanation.get_ranked_local_values()
```

## <a name="interpretability-for-remote-runs"></a>Uzaktan çalıştırmalar için yorumlanabilirlik

Aşağıdaki örnek, uzaktan çalıştırmalar `ExplanationClient` için model yorumlanabilirliğini etkinleştirmek için sınıfı nasıl kullanabileceğinizi gösterir. Kavramsal olarak yerel sürece benzer, sizin dışınız dışında:

* Yorumlanabilirlik bağlamını `ExplanationClient` yüklemek için uzaktan çalıştırmada kullanın.
* Bağlamı daha sonra yerel bir ortamda karşıdan yükleyin.

1. Gerekirse, gerekli `pip install azureml-contrib-interpret` paketi almak için kullanın.

1. Yerel bir Jupyter dizüstü bilgisayarında bir eğitim komut dosyası oluşturun. Örneğin, `train_explain.py`.

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

1. Bilgi işlem hedefiniz olarak bir Azure Machine Learning Compute ayarlayın ve eğitim çalışmanızı gönderin. Talimatlar [için model eğitimi için bilgi işlem hedefleri ayarlama](how-to-set-up-training-targets.md#amlcompute) ya da bkz. [Örnek not defterlerini](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/azure-integration/remote-explanation) de yararlı bulabilirsiniz.

1. Açıklamayı yerel Jupyter not defterinize indirin.

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

## <a name="raw-feature-transformations"></a>Ham özellik dönüşümleri

Tasarlanmış özellikler yerine ham, dönüştürülmemiş özellikler açısından açıklama almayı tercih edebilirsiniz. Bu seçenek için, özellik dönüştürme ardışık işlem `train_explain.py`inizi ' deki açıklayıcıya geçirirsiniz. Aksi takdirde, açıklayıcı mühendislik özellikleri açısından açıklamalar sağlar.

Desteklenen dönüşümlerin biçimi [sklearn-pandalarda](https://github.com/scikit-learn-contrib/sklearn-pandas)açıklandığı gibi aynıdır. Genel olarak, dönüşümler tek bir sütunda çalıştıkları sürece desteklenir, böylece bire bir oldukları açıktır.

Bir `sklearn.compose.ColumnTransformer` kullanarak veya monte transformatör tuples bir listesini kullanarak ham özellikleri için bir açıklama alın. Aşağıdaki örnekkullanır. `sklearn.compose.ColumnTransformer`

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

Eğer monte transformatör tuples listesi ile örnek çalıştırmak istiyorsanız, aşağıdaki kodu kullanın:

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

## <a name="visualizations"></a>Görsel öğeler

Açıklamaları yerel Jupyter dizüstü defterinize indirdikten sonra, modelinizi anlamak ve yorumlamak için görselleştirme panosunu kullanabilirsiniz.

### <a name="global-visualizations"></a>Genel görselleştirmeler

Aşağıdaki çizimler, öngörüleri ve açıklamaları ile birlikte eğitilmiş modelin küresel bir görünümünü sağlar.

|Arsa|Açıklama|
|----|-----------|
|Veri Arama| Tahmin değerleriyle birlikte veri kümesine genel bir bakış görüntüler.|
|Küresel Önem|Genel olarak en iyi K (yapılandırılabilir K) önemli özelliklerini gösterir. Altta yatan modelin genel davranışının anlaşılmasına yardımcı olur.|
|Açıklama Arama|Bir özelliğin modelin tahmin değerlerindeki bir değişikliği veya tahmin değerlerinin olasılığını nasıl etkilediğini gösterir. Özellik etkileşiminin etkisini gösterir.|
|Özet Önemi|Her özelliğin tahmin değeri üzerindeki etkisinin dağılımını göstermek için tüm veri noktalarında yerel, özellik önem değerlerini kullanır.|

[![Görselleştirme Panosu Genel](./media/how-to-machine-learning-interpretability-aml/global-charts.png)](./media/how-to-machine-learning-interpretability-aml/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Yerel görselleştirmeler

Çizimdeki tek tek veri noktasını seçerek, herhangi bir veri noktası için yerel, özellik önem çizimini yükleyebilirsiniz.

|Arsa|Açıklama|
|----|-----------|
|Yerel Önem|Üst K (yapılandırılabilir K) önemli özelliklerini genel olarak gösterir. Belirli bir veri noktasında temel modelin yerel davranışını göstermeye yardımcı olur.|
|Perturbation Exploration|Değişikliklerin seçili veri noktasının özellik değerlerine sahip olmasını ve tahmin değerinde ortaya çıkan değişiklikleri gözlemlemesine izin verir.|
|Bireysel Koşullu Beklenti (ICE)| Özellik değeri değişikliklerinin en düşük değerden maksimum değere kadar olmasını sağlar. Bir özellik değiştiğinde veri noktasının tahmininin nasıl değiştiğini göstermeye yardımcı olur.|

[![Görselleştirme Panosu Yerel Özellik Önemi](./media/how-to-machine-learning-interpretability-aml/local-charts.png)](./media/how-to-machine-learning-interpretability-aml/local-charts.png#lightbox)


[![Görselleştirme Panosu Özelliği Perturbation](./media/how-to-machine-learning-interpretability-aml/perturbation.gif)](./media/how-to-machine-learning-interpretability-aml/perturbation.gif#lightbox)


[![Görselleştirme Panosu ICE Çizimler](./media/how-to-machine-learning-interpretability-aml/ice-plot.png)](./media/how-to-machine-learning-interpretability-aml/ice-plot.png#lightbox)

> [!NOTE]
> Jupyter çekirdeği başlamadan önce, görselleştirme panosu için widget uzantılarını etkinleştirdiğinizden emin olun.

* Jupyter notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.interpret.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.interpret.visualize
    ```

* JupyterLab

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```

Görselleştirme panosunu yüklemek için aşağıdaki kodu kullanın.

```python
from interpret_community.widget import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

### <a name="visualization-in-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosunda görselleştirme

[Uzaktan yorumlanabilirlik](#interpretability-for-remote-runs) adımlarını tamamlarsanız, [Azure Machine Learning stüdyosunda](https://ml.azure.com)görselleştirme panosunu görüntüleyebilirsiniz. Bu pano, yukarıda açıklanan görselleştirme panosunun daha basit bir sürümüdür. Yalnızca iki sekmeyi destekler:

|Arsa|Açıklama|
|----|-----------|
|Küresel Önem|Genel olarak en iyi K (yapılandırılabilir K) önemli özelliklerini gösterir. Altta yatan modelin genel davranışının anlaşılmasına yardımcı olur.|
|Özet Önemi|Her özelliğin tahmin değeri üzerindeki etkisinin dağılımını göstermek için tüm veri noktalarında yerel, özellik önem değerlerini kullanır.|

Hem genel hem de yerel açıklamalar varsa, veriler her iki sekmeyi de doldurulur. Yalnızca genel bir açıklama varsa, Özet Önem sekmesi devre dışı bırakılır.

Azure Machine Learning stüdyosundaki görselleştirme panosuna erişmek için aşağıdaki yollardan birini izleyin:

* **Denemeler** bölmesi (Önizleme)
  1. Azure Machine Learning'de çalıştırdığınız denemelerin listesini görmek için sol bölmedeki **Denemeler'i** seçin.
  1. Bu denemedeki tüm çalıştırmaları görüntülemek için belirli bir deneme seçin.
  1. Bir çalıştırma seçin ve ardından açıklama görselleştirme panosuna **Açıklamalar** sekmesini seçin.

   [![Görselleştirme Panosu Yerel Özellik Önemi](./media/how-to-machine-learning-interpretability-aml/amlstudio-experiments.png)](./media/how-to-machine-learning-interpretability-aml/amlstudio-experiments.png#lightbox)

* **Modeller** bölmesi
  1. [Azure Machine Learning ile Dağıt'taki modelleri](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)izleyerek orijinal modelinizi kaydettiyseniz, görüntülemek için sol bölmedeki **Modelleri** seçebilirsiniz.
  1. Açıklama görselleştirme panosunu görüntülemek için bir model ve ardından **Açıklamalar** sekmesini seçin.

## <a name="interpretability-at-inference-time"></a>Çıkarım zamanında yorumlanabilirlik

Açıklayıcıyı özgün modelle birlikte dağıtabilir ve yerel açıklama bilgilerini sağlamak için çıkarım zamanında kullanabilirsiniz. Ayrıca çıkarım zamanında yorumlanabilirlik performansını artırmak için daha hafif puanlama açıklayıcıları sunuyoruz. Daha hafif bir puanlama açıklayıcısı dağıtma işlemi, bir modeli dağıtmaya benzer ve aşağıdaki adımları içerir:

1. Bir açıklama nesnesi oluşturun. Örneğin, şunları kullanabilirsiniz: `TabularExplainer`

   ```python
    from interpret.ext.blackbox import TabularExplainer


   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Açıklama nesnesi ile bir puanlama açıklayıcısı oluşturun.

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Puanlama açıklayıcımodelini kullanan bir görüntüyü yapılandırın ve kaydedin.

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. İsteğe bağlı bir adım olarak, puanlama açıklayıcısını buluttan alabilir ve açıklamaları test edebilirsiniz.

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import load

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

   1. Gerekirse, Azure Machine Learning ile Dağıt [modellerindeki](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)adımları izleyerek orijinal tahmin modelinizi kaydedin.

   1. Bir puanlama dosyası oluşturun.

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

         Bu yapılandırma, modelinizin gereksinimlerine bağlıdır. Aşağıdaki örnekte, bir CPU çekirdeği ve bir GB bellek kullanan bir yapılandırma tanımlanır.

         ```python
         from azureml.core.webservice import AciWebservice

          aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                    memory_gb=1,
                                                    tags={"data": "NAME_OF_THE_DATASET",
                                                          "method" : "local_explanation"},
                                                    description='Get local explanations for NAME_OF_THE_PROBLEM')
         ```

   1. Ortam bağımlılıkları içeren bir dosya oluşturun.

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

   1. g++ yüklü özel bir dockerfile oluşturun.

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

1. Temizlemek.

   Dağıtılan bir web hizmetini silmek için `service.delete()`.

## <a name="next-steps"></a>Sonraki adımlar

[Model yorumlanabilirliği hakkında daha fazla bilgi edinin](how-to-machine-learning-interpretability.md)