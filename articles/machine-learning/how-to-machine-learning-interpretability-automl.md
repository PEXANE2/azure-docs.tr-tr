---
title: Otomatik makine öğreniminde model yorumlenebilirliği
titleSuffix: Azure Machine Learning
description: Otomatikleştirilmiş ML modelinizin Özellik önemini nasıl belirlediğini ve Azure Machine Learning SDK kullanırken tahmine dayalı hale getirmek için nasıl açıklamalar alabileceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: b2c7825b10feab45df9cb89dbe2b82da1c143866
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129756"
---
# <a name="model-interpretability-in-automated-machine-learning"></a>Otomatik makine öğreniminde model yorumlenebilirliği

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning ' de otomatik makine öğrenimi (ML) için yorumlanabilirliğini özelliklerini nasıl etkinleştireceğinizi öğreneceksiniz. Otomatikleştirilmiş ML, uygulanan özellik önemini anlamanıza yardımcı olur. 

1\.0.85 kümesinden sonra tüm SDK sürümleri varsayılan olarak `model_explainability=True`. SDK sürüm 1.0.85 ve önceki sürümlerinde, kullanıcıların model yorumlenebilirliğini kullanabilmesi için `AutoMLConfig` nesnesinde `model_explainability=True` ayarlaması gerekir. 

Bu makalede şunları öğreneceksiniz:

- En iyi model veya herhangi bir model için eğitim sırasında yorumlenebilirlik gerçekleştirin.
- Veri ve açıklamaları desenlerdeki desenleri görmenizi sağlayacak görselleştirmeleri etkinleştirin.
- Çıkarım veya Puanlama sırasında yorumlenebilirlik uygulayın.

## <a name="prerequisites"></a>Önkoşullar

- Yorumlenebilirlik özellikleri. Gerekli paketleri almak için `pip install azureml-interpret azureml-contrib-interpret` çalıştırın.
- Otomatik ML denemeleri oluşturma hakkında bilgi. Azure Machine Learning SDK 'sını kullanma hakkında daha fazla bilgi için, bu [regresyon modeli öğreticisini](tutorial-auto-train-models.md) doldurun veya [Otomatik ml denemeleri](how-to-configure-auto-train.md)'yi nasıl yapılandıracağınızı öğrenin.

## <a name="interpretability-during-training-for-the-best-model"></a>En iyi model için eğitim sırasında yorumlenebilirlik

Mühendislik uygulanmış özellikler için açıklamaları içeren `best_run`açıklamayı alın.

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Yapıt deposundan uygulanan özellik önem derecesini indirin

`best_run`yapıt deposundan uygulanan özellik açıklamalarını indirmek için `ExplanationClient` kullanabilirsiniz. 

```python
from azureml.explain.model._internal.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Herhangi bir model için eğitim sırasında yorumlenebilirlik 

Model açıklamalarını hesaplarken ve bunları görselleştirebileceğiniz zaman, bir otomatik ML modeli için mevcut model açıklaması ile sınırlı değilsiniz. Ayrıca, farklı test verileriyle modelinize yönelik bir açıklama alabilirsiniz. Bu bölümdeki adımlarda, test verilerinize göre uygulanan özellik önemini nasıl hesaplabileceğiniz ve görselleştirebileceğiniz gösterilmektedir.

### <a name="retrieve-any-other-automl-model-from-training"></a>Eğitimin diğer tüm oto modellerini al

```python
automl_run, fitted_model = local_run.get_output(metric='accuracy')
```

### <a name="set-up-the-model-explanations"></a>Model açıklamalarını ayarlama

Uygulanan açıklamaları almak için `automl_setup_model_explanations` kullanın. `fitted_model` aşağıdaki öğeleri oluşturabilir:

- Eğitilen veya test örneklerinden öne çıkan veriler
- Uygulanan özellik adı listeleri
- Sınıflandırma senaryolarında etiketli sütunlarınızda findable sınıfları

`automl_explainer_setup_obj`, yukarıdaki listeden tüm yapıları içerir.

```python
from azureml.train.automl.runtime.automl_explain_utilities import automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```

### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Özellik önemi için benzeme açıklama 'ı başlatın

Oto ml modelleriyle ilgili bir açıklama oluşturmak için `MimicWrapper` sınıfını kullanın. Mmıısarmalayıcı 'yi şu parametrelerle başlatabilirsiniz:

- Açıklama Kurulum nesnesi
- Çalışma alanınız
- `fitted_model` otomatikleştirilmiş ML modelinin yedeği olarak davranan bir LightGBM modeli

Mımısarmalayıcı Ayrıca, uygulanan açıklamalarının karşıya yükleneceği `automl_run` nesnesini alır.

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

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Uygulanan özellik önemini hesaplamak ve görselleştirmek için MimicExplainer kullanın

Oluşturulan mühendislik özelliklerine yönelik özellik önemini elde etmek için, dönüştürülmüş test örnekleriyle birlikte `explain()` yöntemi ' ni çağırabilirsiniz. Ayrıca, oluşturulan özelliklerin özellik önem değerlerinin Pano görselleştirmesini otomatik ML özellikleri tarafından görüntülemek için `ExplanationDashboard` kullanabilirsiniz.

```python
engineered_explanations = explainer.explain(['local', 'global'], eval_dataset=automl_explainer_setup_obj.X_test_transform)
print(engineered_explanations.get_feature_importance_dict())
```

### <a name="interpretability-during-inference"></a>Çıkarım sırasında yorumlenebilirlik

Bu bölümde, önceki bölümde yer alan açıklamaları hesaplamak için kullanılan açıklama ile otomatik bir ML modeli oluşturmayı öğreneceksiniz.

### <a name="register-the-model-and-the-scoring-explainer"></a>Modeli ve Puanlama açıklama kaydetme

' İ kullanarak, uygulanan özellik önem değerlerini, çıkarım sırasında hesaplayacağımız Puanlama açıklama oluşturmak için `TreeScoringExplainer` kullanın. Puanlama açıklama daha önce hesaplanan `feature_map` başlatırsınız. 

Puanlama Açıklama ' nı kaydedin ve sonra modeli ve Puanlama Açıklama ' ı Model Yönetimi hizmetiyle kaydedin. Aşağıdaki kodu çalıştırın:

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

Ardından, dağıtılan model için kapsayıcıda gerekli ortam bağımlılıklarını oluşturun. Lütfen, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerdiğinden, > = 1.0.45 sürümüne sahip olan azureml-varsayılan değer bir PIP bağımlılığı olarak listelenmelidir.

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

Otomatikleştirilmiş ML modelinden tahmin edilen değeri görmek için bazı test verileriyle çıkarım. Öngörülen değer için uygulanan özellik önem derecesini görüntüleyin.

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

### <a name="visualize-to-discover-patterns-in-data-and-explanations-at-training-time"></a>Eğitim sırasında veri ve açıklamaları desenlerdeki desenleri keşfetmeyi görselleştirin

Özellik önem grafiğini [Azure Machine Learning Studio](https://ml.azure.com)'daki çalışma alanınızda görselleştirebilirsiniz. Otomatik ML çalıştırıldıktan sonra, belirli bir çalışmayı görüntülemek için **model ayrıntılarını görüntüle** ' yi seçin. Açıklama görselleştirme panosunu görmek için **açıklamalar** sekmesini seçin.

[![Machine Learning Yorumlenebilirlik mimarisi](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png)](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Otomatik makine öğrenimi dışında Azure Machine Learning SDK 'sının alanlarında model açıklamalarını ve özellik önemini nasıl etkinleştirebilirim hakkında daha fazla bilgi için, bkz. [yorumda bulunan kavram makalesi](how-to-machine-learning-interpretability.md).
