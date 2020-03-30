---
title: Otomatik makine öğrenmesinde model birlikte çalışabilirliği
titleSuffix: Azure Machine Learning
description: Azure Machine Learning SDK'yı kullanırken otomatik ML modelinizin özelliğin önemini nasıl belirlediği ve öngörülerde nasıl öngörüde bulunabildiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: e6fa3b9705fcd718f802b73560e7703b008af2b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064003"
---
# <a name="model-interpretability-in-automated-machine-learning"></a>Otomatik makine öğrenmesinde model birlikte çalışabilirliği

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'de otomatik makine öğrenimi (ML) için yorumlanabilirlik özelliklerini nasıl etkinleştirdiğinizi öğreneceksiniz. Otomatik ML, tasarlanmış özelliğin önemini anlamanıza yardımcı olur. 

Varsayılan olarak 1.0.85'ten `model_explainability=True` sonra tüm SDK sürümleri. SDK sürüm 1.0.85 ve önceki sürümlerde `AutoMLConfig` kullanıcıların model yorumlanabilirliğini kullanabilmek için nesneyi ayarlamaları `model_explainability=True` gerekir. 

Bu makalede şunları öğreneceksiniz:

- En iyi model veya herhangi bir model için eğitim sırasında yorumlanabilirlik gerçekleştirin.
- Verilerdeki ve açıklamalartaki desenleri görmenize yardımcı olmak için görselleştirmeleri etkinleştirin.
- Çıkarım veya puanlama sırasında yorumlanabilirliği uygulayın.

## <a name="prerequisites"></a>Ön koşullar

- Yorumlanabilirlik özellikleri. Gerekli `pip install azureml-interpret azureml-contrib-interpret` paketleri almak için çalıştırın.
- Otomatik ML deneyleri oluşturma bilgisi. Azure Machine Learning SDK'nın nasıl kullanılacağı hakkında daha fazla bilgi için bu [regresyon modeli eğitimini](tutorial-auto-train-models.md) tamamlayın veya otomatik ML denemelerini nasıl [yapılandıracağımız](how-to-configure-auto-train.md)hakkında bilgi alın.

## <a name="interpretability-during-training-for-the-best-model"></a>En iyi model için eğitim sırasında yorumlanabilirlik

Mühendislik özellikleriiçin `best_run`açıklamalar içeren açıklamayı alın.

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Yapı mağazasından mühendislik özelliğini indirin

Tasarlanan `ExplanationClient` özellik açıklamalarını yapı nın artifakı `best_run`deposundan indirmek için kullanabilirsiniz. 

```python
from azureml.explain.model._internal.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Herhangi bir model için eğitim sırasında yorumlanabilirlik 

Model açıklamalarını hesaplayıp görselleştirdiğinizde, otomatik bir ML modeli için varolan bir model açıklamasıyla sınırlı değildir. Ayrıca farklı test verileri ile modeliniz için bir açıklama alabilirsiniz. Bu bölümdeki adımlar, test verilerinize dayalı olarak mühendislik özelliğinin önemini nasıl hesaplayacağınızı ve görselleştirdiğinizi gösterir.

### <a name="retrieve-any-other-automl-model-from-training"></a>Eğitimden başka bir AutoML modeli alma

```python
automl_run, fitted_model = local_run.get_output(metric='accuracy')
```

### <a name="set-up-the-model-explanations"></a>Model açıklamalarını ayarlama

Tasarlanmış `automl_setup_model_explanations` açıklamaları almak için kullanın. Aşağıdaki `fitted_model` öğeleri oluşturabilirsiniz:

- Eğitimli veya test örneklerinden öne çıkan veriler
- Tasarlanmış özellik ad listeleri
- Sınıflandırma senaryolarında etiketli sütununuzda bulunabilen sınıflar

Yukarıdaki `automl_explainer_setup_obj` listedeki tüm yapıları içerir.

```python
from azureml.train.automl.runtime.automl_explain_utilities import automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```

### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Özelliğin önemi için Mimic Explainer'ı başlatma

AutoML modelleri için bir açıklama `MimicWrapper` oluşturmak için sınıfı kullanın. MimicWrapper'ı şu parametrelerle başolarak alabilirsiniz:

- Açıklayıcı kurulum nesnesi
- Çalışma alanınız
- `fitted_model` Otomatik ML modeline vekil olarak hareket eden LightGBM modeli

MimicWrapper ayrıca, `automl_run` tasarlanmış açıklamaların yüklendiği nesneyi de alır.

```python
from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.explain.model.mimic_wrapper import MimicWrapper

# Initialize the Mimic Explainer
explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel, 
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names, 
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Engineered özelliğinin önemini hesaplamak ve görselleştirmek için MimicExplainer'ı kullanın

Oluşturulan mühendislik `explain()` özellikleri için özellik önem almak için dönüştürülmüş test örnekleri ile MimicWrapper yöntemi arayabilirsiniz. Ayrıca otomatik `ExplanationDashboard` ML featurizers tarafından oluşturulan mühendislik özelliklerinin özellik önem değerlerinin pano görselleştirme görüntülemek için kullanabilirsiniz.

```python
engineered_explanations = explainer.explain(['local', 'global'], eval_dataset=automl_explainer_setup_obj.X_test_transform)
print(engineered_explanations.get_feature_importance_dict())
```

### <a name="interpretability-during-inference"></a>Çıkarım sırasında yorumlanabilirlik

Bu bölümde, önceki bölümdeaçıklamaları hesaplamak için kullanılan açıklayıcı ile otomatik bir ML modelinin nasıl işlevselleştirilecek olduğunu öğrenirsiniz.

### <a name="register-the-model-and-the-scoring-explainer"></a>Modeli ve puanlama açıklamasını kaydedin

Çıkarım `TreeScoringExplainer` zamanında mühendislik özelliği önem değerlerini hesaplayacak puanlama açıklayıcısını oluşturmak için kullanın. Daha önce hesaplanan puanlama `feature_map` açıklayıcısını başlangıç olarak açıklarsınız. 

Puanlama açıklamasını kaydedin ve ardından modeli ve puanlama açıklamasını Model Yönetim Hizmeti'ne kaydedin. Aşağıdaki kodu çalıştırın:

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

### <a name="create-the-conda-dependencies-for-setting-up-the-service"></a>Hizmeti ayarlamak için conda bağımlılıklarını oluşturma

Ardından, dağıtılan model için kapsayıcıda gerekli ortam bağımlılıklarını oluşturun. Modeli web hizmeti olarak barındırmak için gereken işlevselliği içerdiğinden, >= 1,0,45 sürümüolan azureml varsayılanlarının pip bağımlılığı olarak listelemesi gerektiğini lütfen unutmayın.

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

Conda dosyasını ve önceki adımlardaki puanlama dosyasını kullanarak hizmeti dağıtın.

```python
from azureml.core.webservice import Webservice
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model, InferenceConfig
from azureml.core.environment import Environment

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                               memory_gb=1,
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"},
                                               description='Get local explanations for Bank marketing test data')
myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score_local_explain.py", environment=myenv)

# Use configs and models generated above
service = Model.deploy(ws,
                       'model-scoring',
                       [scoring_explainer_model, original_model],
                       inference_config,
                       aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-with-test-data"></a>Test verileriyle çıkarım

Otomatik ML modelinden öngörülen değeri görmek için bazı test verileri ile çıkarım. Öngörülen değer için tasarlanmış özellik önemini görüntüleyin.

```python
if service.state == 'Healthy':
    # Serialize the first row of the test data into json
    X_test_json = X_test[:1].to_json(orient='records')
    print(X_test_json)
    # Call the service to get the predictions and the engineered explanations
    output = service.run(X_test_json)
    # Print the predicted value
    print(output['predictions'])
    # Print the engineered feature importances for the predicted value
    print(output['engineered_local_importance_values'])
```

### <a name="visualize-to-discover-patterns-in-data-and-explanations-at-training-time"></a>Eğitim zamanında verilerdeki ve açıklamalardaki desenleri keşfetmek için görselleştirin

[Azure Machine Learning stüdyosunda](https://ml.azure.com)çalışma alanınızda özellik önem grafiğini görselleştirebilirsiniz. Otomatik ML çalıştırmanız tamamlandıktan sonra, belirli bir çalıştırmayı görüntülemek için **model ayrıntılarını görüntüle'yi** seçin. Açıklama görselleştirme panosunu görmek için **Açıklamalar** sekmesini seçin.

[![Makine Öğrenimi Yorumlanabilirlik Mimarisi](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png)](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning SDK'nın otomatik makine öğrenimi dışındaki alanlarında model açıklamalarını ve özellik önemini nasıl etkinleştirebileceğiniz hakkında daha fazla bilgi için [yorumlanabilirlik hakkındaki kavram makalesine](how-to-machine-learning-interpretability.md)bakın.
