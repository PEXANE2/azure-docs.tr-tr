---
title: Yerel ve uzak çalıştırmalar için model yorumlenebilirliği
titleSuffix: Azure Machine Learning
description: Modelinizin Azure Machine Learning SDK 'Yı kullanarak neden tahmin sağladığını açıklayacağınızı öğrenin. Modelin Özellik önemini nasıl belirlediğini ve tahmin yaptığını anlamak için eğitim ve çıkarım sırasında kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: a2b71a10606b7cd20f06b2497515b758426833a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73515310"
---
# <a name="model-interpretability-for-local-and-remote-runs"></a>Yerel ve uzak çalıştırmalar için model yorumlenebilirliği

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, modelinizin, Azure Machine Learning Python SDK 'nın yorumyapılanmış paketiyle neden olduğunu nasıl anlatacağınızı öğreneceksiniz. Aşağıdaki görevleri öğrenirsiniz:

* Hem yerel olarak hem de uzaktan işlem kaynaklarında eğitilen makine öğrenimi modellerini yorumlama
* Azure çalışma geçmişinde yerel ve genel açıklamaları depolama
* [Azure Machine Learning Studio](https://ml.azure.com) 'da yorumlenebilirlik görselleştirmelerini görüntüleyin
* Modelinize bir Puanlama açıklama dağıtma

Model yorumlenebilirliği hakkında daha fazla bilgi edinmek için bkz. [kavram makalesi](how-to-machine-learning-interpretability.md).

## <a name="local-interpretability"></a>Yerel yorumlenebilirlik

Aşağıdaki örnek, Azure hizmetleriyle iletişim kurmadan yorumlama paketinin yerel olarak nasıl kullanılacağını gösterir. Yorumlenebilirlik paketini almak için `pip install azureml-interpret` çalıştırın.

1. Yerel bir Jupyter not defterinde örnek modeli eğitme.

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

2. Açıklama yerel olarak çağırın: bir açıklama nesnesi başlatmak Için modelinize ve bazı eğitim verilerinizi açıklama oluşturucusuna geçirmeniz gerekir. Ayrıca, açıklamaları ve görselleştirmelerinizi daha bilgilendirici hale getirmek için kullanılacak özellik adlarını ve çıkış sınıfı adlarını (sınıflandırma yapıyorsanız) isteğe bağlı olarak geçirebilirsiniz. `TabularExplainer`, `MimicExplainer`ve `PFIExplainer` yerel olarak kullanarak bir açıklama nesnesinin örneğini oluşturma işlemi aşağıda verilmiştir. `TabularExplainer`, alttaki üç SHAP explainers birini (`TreeExplainer`, `DeepExplainer`veya `KernelExplainer`) çağırır ve kullanım örneği için en uygun olanı otomatik olarak seçiyoruz. Ancak, temel alınan üç explainers her birini doğrudan çağırabilirsiniz.

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

### <a name="overall-global-feature-importance-values"></a>Genel (genel) özellik önem değerleri

Genel özellik önem değerlerini alın.
    
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

### <a name="instance-level-local-feature-importance-values"></a>Örnek düzeyi (yerel) özelliği önem değerleri

Yerel Özellik önem değerlerini al: tek bir örneği veya örnek grubunu açıklamak için aşağıdaki işlev çağrılarını kullanın. PFIExplainer yerel açıklamaları desteklemediğini lütfen unutmayın.

```python
# explain the first data point in the test set
local_explanation = explainer.explain_local(x_test[0:5])

# sorted feature importance values and feature names
sorted_local_importance_names = local_explanation.get_ranked_local_names()
sorted_local_importance_values = local_explanation.get_ranked_local_values()
```

## <a name="interpretability-for-remote-runs"></a>Uzaktan çalıştırmalar için yorumlenebilirlik

Bu örnek, uzak çalıştırmalar için model yorumlenebilirliğini etkinleştirmek üzere `ExplanationClient` sınıfının nasıl kullanılacağını gösterir. Kavram önceki bölüme benzer, ancak bir uzak çalıştırmada `ExplanationClient`, yorumlenebilirlik bağlamını karşıya yüklerken kullanır ve ardından bağlamı daha sonra yerel bir ortamda indirebilirsiniz. Gerekli paketi almak için `pip install azureml-contrib-interpret` kullanın.

1. Yerel bir Jupyter not defterinde eğitim betiği oluşturun (örneğin, train_explain. Kopyala).

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

1. İşlem hedefiniz olarak bir Azure Machine Learning işlem ayarlamayı ve eğitim çalıştırmanızı göndermeyi öğrenmek için [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md#amlcompute) yönergelerini izleyin. [Örnek not defterlerini](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/azure-integration/remote-explanation)da görebilirsiniz.

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

## <a name="raw-feature-transformations"></a>Ham Özellik dönüştürmeleri

İsteğe bağlı olarak, dönüşümden önceki ham Özellikler (mühendislik uygulanmış özelliklerden değil) açısından açıklamaları almak için özellik dönüştürme işlem hattınızı açıklama (train_explain. Kopyala) geçirebilirsiniz. Bunu atlarsanız açıklama, uygulanan özellikler açısından açıklamaları sağlar.

Desteklenen dönüşümlerin biçimi, [sköğren-Pandas](https://github.com/scikit-learn-contrib/sklearn-pandas)bölümünde açıklananla aynıdır. Genel olarak, Tüm dönüştürmeler tek bir sütunda çalıştıkları sürece desteklenir ve bu nedenle açıkça bir tane olmak üzere çok daha vardır. 

Ham özellikleri bir `sklearn.compose.ColumnTransformer` veya bir veya bir veya bir veya bir veya bir veya bir bir bir transformatör tanımlama grubu Aşağıdaki kod `sklearn.compose.ColumnTransformer`kullanır. 


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

Örneği, birlikte bulunan transformatör tanımlama grupları listesiyle çalıştırmak istiyorsanız aşağıdaki kodu kullanın.

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

Yerel Jupyter Not defterinize açıklamaları indirdikten sonra, modelinizi anlamak ve yorumlamak için görselleştirme panosunu kullanabilirsiniz.

### <a name="global-visualizations"></a>Küresel görselleştirmeler

Aşağıdaki çizimler, eğitilen modelin, tahmine dayalı ve açıklamalarıyla birlikte küresel bir görünümünü sağlar.

|ZF|Açıklama|
|----|-----------|
|Veri araştırması| Tahmin değerleriyle birlikte veri kümesine genel bakış.|
|Küresel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, temel alınan modelin genel davranışını anlamak için kullanışlıdır.|
|Açıklama araştırması|Modelin tahmin değerlerinde (veya tahmin değerleri açısından) bir değişiklik yapmaktan bir özelliğin nasıl sorumlu olduğunu gösterir. Ayrıca, tahmine dayalı olarak iki özelliğin nasıl etkileşime gireceğini gösterir.|
|Özet önem derecesi| Her bir özelliğin tahmin değerinde olduğu etkinin dağılımını göstermek için tüm veri noktalarında imzalı yerel özellik önem değerlerini kullanır.|

[![görselleştirme panosu genel](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Yerel görselleştirmeler

Verilen veri noktası için yerel özellik önem grafiğini yüklemek üzere yukarıdaki çizim sırasında herhangi bir zamanda herhangi bir veri noktasına tıklayın.

|ZF|Açıklama|
|----|-----------|
|Yerel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, belirli bir veri noktasındaki temeldeki modelin yerel davranışını anlamak için kullanışlıdır.|
|Perturbation araştırması|Seçili veri noktasının özellik değerlerini değiştirmenize ve bu değişikliklerin tahmin değerini nasıl etkileyeceğini gözlemlemeye olanak tanır.|
|Bireysel koşullu beklenti (buz)| Bir özellik değiştiğinde veri noktasının tahmin değişikliğini görmek için bir özellik değerini minimum değerden maksimum değere değiştirmenize izin verir.|

[![görselleştirme Panosu yerel özellik önemi](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![görselleştirme Pano özelliği Perturbation](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![görselleştirme Pano buz çizimleri](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Bir görselleştirme panosunun, Jupyıter çekirdeği başlamadan önce etkinleştirilmiş pencere öğesi uzantılarına sahip olmanız gerekir.

* Jupyter notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.interpret.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.interpret.visualize
    ```



* Jupyıter Labs

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```

Görselleştirme panosunu yüklemek için aşağıdaki kodu kullanın.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

### <a name="visualization-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'da görselleştirme

[Uzaktan yorumlanbilirlik](how-to-machine-learning-interpretability-aml.md#interpretability-for-remote-runs) bölümündeki adımları tamamlayarak, [Azure Machine Learning Studio](https://ml.azure.com)'daki görselleştirme panosunu kontrol edebilirsiniz. Azure Machine Learning Studio 'da gösterilen Pano, yukarıda açıklanan görselleştirme panosunun daha basit bir sürümüdür ve yalnızca aşağıdaki iki sekmeyi destekler.

|ZF|Açıklama|
|----|-----------|
|Küresel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, temel alınan modelin genel davranışını anlamak için kullanışlıdır.|
|Özet önem derecesi| Her bir özelliğin tahmin değerinde olduğu etkinin dağılımını göstermek için tüm veri noktalarında imzalı yerel özellik önem değerlerini kullanır.|

Hem genel hem de yerel açıklamalar kullanılabiliyorsa, her iki sekme de verilerle doldurulur. Yalnızca genel açıklama kullanılabiliyorsa ikinci sekme devre dışı bırakılır.

Azure Machine Learning Studio 'daki görselleştirme panosuna erişmek için aşağıdaki yollardan birini izleyebilirsiniz:

1. Denemeleri sekmesi (Önizleme): "denemeleri" sekmesine tıklayarak Azure Machine Learning hizmetinde çalıştırdığınız denemeleri listesini görürsünüz. Bu listeden, seçilen deneme adı altındaki tüm çalıştırmaların bulunduğu bir sayfaya yeniden yönlendirilmek üzere belirli bir deneme seçebilirsiniz. Her bir çalıştırmaya ve "açıklamalar" sekmesine tıklayarak açıklama görselleştirme panosunu görürsünüz.


[![görselleştirme Panosu yerel özellik önemi](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png)](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png#lightbox)


2. Modeller sekmesi: özgün modelinizi [Azure Machine Learning modelleri dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where)bölümündeki adımları kullanarak kaydettirdiğiniz durumlarda modelinize "modeller" sekmesi listesinde görünür. Her modele ve "açıklamalar" sekmesine tıklayarak açıklama görselleştirme panosunu görürsünüz.

## <a name="interpretability-at-inference-time"></a>Çıkarımı zamanında yorumlenebilirlik

Açıklama özgün modelle birlikte dağıtılabilir ve yerel açıklama bilgilerini sağlamak için çıkarım zamanında kullanılabilir. Ayrıca, çıkarım sırasında yorumlamaya yönelik performansı artırmak için daha hafif Puanlama explainers sunuyoruz. Daha hafif bir Puanlama açıklama dağıtma işlemi, model dağıtmaya benzer ve aşağıdaki adımları içerir:


1. Açıklama nesnesi oluşturun (örneğin, tablo Rexplainer kullanarak):

   ```python
    from interpret.ext.blackbox import TabularExplainer


   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Açıklama nesnesini kullanarak bir Puanlama açıklama oluşturma:

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import KernelScoringExplainer, save

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

   1. G + + yüklenmiş özel bir dockerfile oluşturun

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Oluşturulan görüntüyü Dağıt (tahmini süre: 5 dakika)

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

1. Temizle: dağıtılan bir Web hizmetini silmek Için `service.delete()`kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Model yorumlenebilirliği hakkında daha fazla bilgi edinmek için [kavramsal makaleye](how-to-machine-learning-interpretability.md)bakın.
