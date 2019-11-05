---
title: Otomatik ML 'de model yorumlenebilirliği
titleSuffix: Azure Machine Learning
description: Otomatik ML modelinizin Azure Machine Learning SDK kullanarak nasıl tahmin sağladığını açıklayacağınızı öğrenin. Modelin Özellik önemini nasıl belirlediğini ve tahmin yaptığını anlamak için eğitim ve çıkarım sırasında kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 2c9df55eb319dd45281eca4684c79d83dc6ef933
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73515336"
---
# <a name="model-interpretability-for-automated-ml-models"></a>Otomatik ML modelleri için model yorumlenebilirliği

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu nasıl yapılır, Azure Machine Learning hizmetini kullanarak otomatik makine öğreniminde yorumlu işlevselliği nasıl etkinleştireceğinizi öğrenirsiniz. Otomatikleştirilen ML, hem ham hem de mühendislik uygulanmış özellik önemini anlamanıza olanak tanır. Model yorumlenebilirliğini kullanmak için `AutoMLConfig` nesnesinde `model_explainability=True` ayarlayın.  

Bu makalede, aşağıdaki görevleri öğreneceksiniz:

* En iyi model veya herhangi bir model için eğitim sırasında yorumlenebilirlik
* Veri ve açıklamaları içindeki desenlerin bulunmasına yardımcı olmak için görselleştirmeleri etkinleştirme
* Çıkarım veya Puanlama sırasında yorumlenebilirlik

## <a name="prerequisites"></a>Önkoşullar

* Yorumlenebilirlik özelliklerine yönelik gerekli paketleri almak için `pip install azureml-interpret azureml-contrib-interpret` çalıştırın.
* Bu makalede otomatik ML denemeleri oluşturma hakkında bilgi sahibi olduğunuz varsayılır. SDK 'da otomatik ML 'yi nasıl kullanacağınızı öğrenmek için [öğreticiye](tutorial-auto-train-models.md) veya [nasıl yapılır](how-to-configure-auto-train.md) bölümüne bakın.
 
## <a name="interpretability-during-training-for-the-best-model"></a>En iyi model için eğitim sırasında yorumlenebilirlik 

Mühendislik uygulanmış Özellikler ve ham özellikler için açıklamaları içeren `best_run`açıklamayı alın. 

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Yapıt deposundan uygulanan özellik önem derecesini indirin

Best_run 'in yapıt deposundan uygulanan özellik açıklamalarını indirmek için `ExplanationClient` kullanabilirsiniz. Ham özellikler kümesi `raw=True`için açıklama almak üzere. 

```python
from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Herhangi bir model için eğitim sırasında yorumlenebilirlik 

Bu bölümde, model açıklamalarını nasıl hesaplacağınızı ve açıklamaları görselleştirmeyi öğreneceksiniz. Bir otomatik ML modeli için mevcut model açıklaması almanın yanı sıra modelinize farklı test verileriyle de açıklama ekleyebilirsiniz. Aşağıdaki adımlar, test verilerinize göre uygulanan özellik önem ve ham Özellik önem derecesini hesaplamanıza ve görselleştirmenize olanak tanır.

### <a name="retrieve-any-other-automl-model-from-training"></a>Eğitimin diğer tüm oto modellerini al

```python
automl_run, fitted_model = local_run.get_output(metric='r2_score')
```

### <a name="setup-the-model-explanations"></a>Model açıklamalarını ayarlama

Fitted_model, automl_setup_model_explanations kullanarak uygulanan ve ham özellik açıklamalarını almak için kullanılacak aşağıdaki öğeleri oluşturabilir:

* Eğitim örnekleri/test örnekleri
* Mühendislik uygulanan ve ham Özellik adı listelerini toplayın
* Sınıflandırma senaryolarında etiketli sütunlarınızda sınıfları bulma

Automl_explainer_setup_obj, yukarıdaki listeden tüm yapıları içerir.

```python
from azureml.train.automl.automl_explain_utilities import AutoMLExplainerSetupClass, automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```
### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Özellik önemi için benzeme açıklama 'ı başlatın

Oto ml modellerini açıklayan `MimicWrapper` sınıfını kullanın. Mımısarmalayıcı, açıklama kurulum nesnesi, çalışma alanınız ve otomatik ML modelini açıklamak için yedek model görevi gören bir LightGBM modeli ile başlatılabilir (burada fitted_model). Mımısarmalayıcı Ayrıca ham ve mühendislik uygulanan açıklamaları karşıya yükleyeceğiniz automl_run nesnesini de alır.

```python
from azureml.interpret.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.interpret.mimic_wrapper import MimicWrapper

explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel, 
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names, 
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Uygulanan özellik önemini hesaplamak ve görselleştirmek için MimicExplainer kullanın

Mimkıwrapper içindeki açıkla () yöntemi, üretilen mühendislik özelliklerinin özellik önemini elde etmek için dönüştürülmüş test örnekleriyle çağrılabilir. Otomatik ML özelliklerine göre üretilen mühendislik özelliklerinin özellik önem derecesine sahip olan Dash panosu görselleştirmesini görüntülemek için ExplanationDashboard de kullanabilirsiniz.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard
engineered_explanations = explainer.explain(['local', 'global'],              
                                            eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(engineered_explanations.get_feature_importance_dict())
ExplanationDashboard(engineered_explanations, automl_explainer_setup_obj.automl_estimator, automl_explainer_setup_obj.X_test_transform)
```
### <a name="use-mimic-explainer-for-computing-and-visualizing-raw-feature-importance"></a>Ham Özellik önemini hesaplamak ve görselleştirmek için Benzeaçıklama kullanma

Msarmalayıcı içindeki açıkla () yöntemi, dönüştürülmüş test örnekleri ile birlikte çağrılabilir ve ham özelliklerin özellik önemini almak için `get_raw` true olarak ayarlanabilir. Ham özelliklerin özellik önem değerlerinin Pano görselleştirmesini görüntülemek için ExplanationDashboard de kullanabilirsiniz.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

raw_explanations = explainer.explain(['local', 'global'], get_raw=True, 
                                     raw_feature_names=automl_explainer_setup_obj.raw_feature_names,
                                     eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(raw_explanations.get_feature_importance_dict())
ExplanationDashboard(raw_explanations, automl_explainer_setup_obj.automl_pipeline, automl_explainer_setup_obj.X_test_raw)
```

### <a name="interpretability-during-inference"></a>Çıkarım sırasında yorumlenebilirlik

Bu bölümde, bir otomatik ML modelini bir önceki bölümdeki açıklamaları hesaplamak için kullanılan açıklama ile nasıl gerçekleştireceğinizi öğreneceksiniz.

### <a name="register-the-model-and-the-scoring-explainer"></a>Modeli ve Puanlama açıklama kaydetme

`TreeScoringExplainer` kullanarak, ham ve uygulanan özellik önem değerlerini, çıkarım sırasında hesaplamak için kullanılacak Puanlama açıklama oluşturun. Puanlama açıklama daha önce hesaplanan feature_map ile başlayacağını unutmayın. Feature_map, Puanlama açıklama tarafından ham Özellik önemini döndürecek şekilde kullanılacaktır.

Aşağıdaki kodda, Puanlama Açıklama kaydeder ve modeli ve Puanlama Açıklama Model Yönetimi hizmeti ile kaydedersiniz.

```python
from azureml.interpret.scoring.scoring_explainer import TreeScoringExplainer, save

# Initialize the ScoringExplainer
scoring_explainer = TreeScoringExplainer(explainer.explainer, feature_maps=[automl_explainer_setup_obj.feature_map])

# Pickle scoring explainer locally
save(scoring_explainer, exist_ok=True)

# Register trained automl model present in the 'outputs' folder in the artifacts
original_model = automl_run.register_model(model_name='automl_model', 
                                           model_path='outputs/model.pkl')

# Register scoring explainer
automl_run.upload_file('scoring_explainer.pkl', 'scoring_explainer.pkl')
scoring_explainer_model = automl_run.register_model(model_name='scoring_explainer', model_path='scoring_explainer.pkl')
```

### <a name="create-the-conda-dependencies-for-setting-up-the-service"></a>Hizmeti ayarlamak için Conda bağımlılıklarını oluşturun

Daha sonra, dağıtılan model için kapsayıcıda gereken ortam bağımlılıklarını oluşturursunuz.

```python
from azureml.core.conda_dependencies import CondaDependencies 

azureml_pip_packages = [
    'azureml-interpret', 'azureml-train-automl', 'azureml-defaults'
]

myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas', 'numpy', 'py-xgboost<=0.80'],
                                 pip_packages=azureml_pip_packages,
                                 pin_sdk_version=True)

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())

with open("myenv.yml","r") as f:
    print(f.read())

```

### <a name="deploy-the-service"></a>Hizmeti dağıtma

Önceki adımlardan Conda dosyasını ve Puanlama dosyasını kullanarak hizmeti dağıtın.

```python
from azureml.core.webservice import Webservice
from azureml.core.model import InferenceConfig
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"}, 
                                               description='Get local explanations for Bank marketing test data')

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score_local_explain.py",
                                   conda_file="myenv.yml")

# Use configs and models generated above
service = Model.deploy(ws, 'model-scoring', [scoring_explainer_model, original_model], inference_config, aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-using-test-data"></a>Test verilerini kullanarak çıkarım

Otomatik ML modelinden tahmin edilen değeri görmek için bazı test verilerini kullanarak çıkarım ve tahmin edilen değer için öngörülen değer ve ham Özellik önem derecesi için uygulanan özellik önem derecesini görüntüleyin.

```python
if service.state == 'Healthy':
    # Serialize the first row of the test data into json
    X_test_json = X_test[:1].to_json(orient='records')
    print(X_test_json)
    # Call the service to get the predictions and the engineered and raw explanations
    output = service.run(X_test_json)
    # Print the predicted value
    print(output['predictions'])
    # Print the engineered feature importances for the predicted value
    print(output['engineered_local_importance_values'])
    # Print the raw feature importances for the predicted value
    print(output['raw_local_importance_values'])
```

### <a name="visualizations-to-aid-you-in-the-discovery-of-patterns-in-data-and-explanations-at-training-time"></a>Eğitim sırasında veri ve açıklamaları desenlerinde bulmaya yardımcı olacak görselleştirmeler

Ayrıca, [Azure Machine Learning Studio](https://ml.azure.com)'daki çalışma alanınızdaki Özellik önem grafiğini de görselleştirebilirsiniz. Otomatik ML çalıştırma işlemi tamamlandıktan sonra, sizi belirli bir çalıştırmaya götürecektir "model ayrıntılarını görüntüle" seçeneğine tıklamanız gerekir. Buradan, açıklama görselleştirme panosunu görmek için "açıklamalar" sekmesine tıklayın. 

[![Machine Learning Yorumlenebilirlik mimarisi](./media/machine-learning-interpretability-explainability/automl-explainability.png)](./media/machine-learning-interpretability-explainability/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Model açıklamalarını ve özellik önemini otomatik makine öğrenimi dışında SDK 'nın diğer alanlarında nasıl etkinleştirilecekleri hakkında daha fazla bilgi için, bkz. yorumda bulunan [kavram](how-to-machine-learning-interpretability.md) makalesi.