---
title: Otomatik ML uzaktan işlem hedefleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile Azure Machine Learning uzaktan işlem hedefi üzerinde otomatik makine öğrenimi kullanarak modeller oluşturmayı öğrenin
services: machine-learning
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4276a713e62f96cc5340fc7be0e8391939d32342
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497313"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Bulutta otomatik makine öğrenimi ile modelleri eğitme

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning, modelinizi yönettiğiniz farklı türlerde işlem kaynakları üzerinde eğitebilirsiniz. İşlem hedefi, bulutta yerel bir bilgisayar veya bir kaynak olabilir.

Azure Machine Learning Işlem (AmlCompute) gibi ek bilgi işlem hedefleri ekleyerek makine öğrenimi denemenizin kolayca ölçeğini değiştirebilir veya ölçeklendirebilirsiniz. AmlCompute, kolayca tek veya çok düğümlü bir işlem oluşturmanıza olanak sağlayan bir yönetilen işlem altyapısıdır.

Bu makalede, AmlCompute ile otomatikleştirilmiş ML kullanarak bir model oluşturmayı öğreneceksiniz.

## <a name="how-does-remote-differ-from-local"></a>Uzaktan yerel nasıl farklıdır?

"[Otomatik makine öğrenimi ile sınıflandırma modelini eğitme](tutorial-auto-train-models.md)" öğreticisi, bir MODELI otomatik ml ile eğitme konusunda yerel bir bilgisayar kullanmayı öğretir. Eğitim yerel olarak uzak hedefler de için de geçerli olduğunda iş akışı. Ancak, uzaktan işlem ile otomatikleştirilmiş ML denemesi yinelemeleri zaman uyumsuz olarak yürütülür. Bu işlevsellik, belirli bir yinelemeyi iptal etmenizi, yürütmenin durumunu izlemenize veya Jupyter not defterindeki diğer hücrelerde çalışmaya devam etmenize olanak tanır. Uzaktan eğitebilmeniz için öncelikle AmlCompute gibi bir uzak işlem hedefi oluşturursunuz. Ardından, uzak kaynağı yapılandırıp kodunuzu buraya gönderebilirsiniz.

Bu makalede, uzak bir AmlCompute hedefinde otomatik ML denemesi çalıştırmak için gereken ek adımlar gösterilmektedir. Öğreticiden `ws`çalışma alanı nesnesi, burada kod boyunca kullanılır.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Kaynak oluştur

Zaten mevcut değilse, çalışma alanınızda AmlCompute hedefini oluşturun (`ws`).

**Tahmin süresi**: AmlCompute hedefinin oluşturulması yaklaşık 5 dakika sürer.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

amlcompute_cluster_name = "automlcl"  # Name your cluster
provisioning_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                            # for GPU, use "STANDARD_NC6"
                                                            # vm_priority = 'lowpriority', # optional
                                                            max_nodes=6)
compute_target = ComputeTarget.create(
    ws, amlcompute_cluster_name, provisioning_config)

# Can poll for a minimum number of nodes and for a specific timeout.
# If no min_node_count is provided, it will use the scale settings for the cluster.
compute_target.wait_for_completion(
    show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Artık `compute_target` nesnesini uzak işlem hedefi olarak kullanabilirsiniz.

Küme adı kısıtlamaları şunları içerir:
+ 64 karakterden kısa olmalıdır.
+ Şu karakterlerden herhangi birini içeremez: `\` ~! @ # $% ^ & * () = + _ [] {} \\\\ |; : \' \\"< >/?. `

## <a name="access-data-using-tabulardataset-function"></a>TabularDataset işlevini kullanarak verilere erişme

Otomatikmlconfig içindeki otomatik ML 'ye geçirilen `TabularDataset`s olarak X ve y tanımlandı. Varsayılan olarak `from_delimited_files`, `infer_column_types` doğru olarak ayarlar, bu da sütun türünü otomatik olarak çıkaracaktır. 

Sütun türlerini el ile ayarlamak isterseniz, her bir sütunun türünü el ile ayarlamak için `set_column_types` bağımsız değişkenini ayarlayabilirsiniz. Aşağıdaki kod örneğinde, veriler sköğren paketinden gelir.

```python
# Create a project_folder if it doesn't exist
if not os.path.isdir('data'):
    os.mkdir('data')
    
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

from sklearn import datasets
from azureml.core.dataset import Dataset
from scipy import sparse
import numpy as np
import pandas as pd

data_train = datasets.load_digits()

pd.DataFrame(data_train.data[100:,:]).to_csv("data/X_train.csv", index=False)
pd.DataFrame(data_train.target[100:]).to_csv("data/y_train.csv", index=False)

ds = ws.get_default_datastore()
ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)

X = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/X_train.csv'))
y = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/y_train.csv'))

```

## <a name="create-run-configuration"></a>Çalıştırma yapılandırması oluştur

Get_Data. Kopyala betiğinin bağımlılıklarını kullanılabilir hale getirmek için tanımlı `CondaDependencies`sahip bir `RunConfiguration` nesnesi tanımlayın. Bu nesneyi `AutoMLConfig``run_configuration` parametresi için kullanın.

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config = RunConfiguration(framework="python")
run_config.target = compute_target
run_config.environment.docker.enabled = True
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

dependencies = CondaDependencies.create(
    pip_packages=["scikit-learn", "scipy", "numpy"])
run_config.environment.python.conda_dependencies = dependencies
```

Bu tasarım deseninin ek bir örneği için bu [örnek not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) bakın.

## <a name="configure-experiment"></a>Deneme yapılandırma
`AutoMLConfig`için ayarları belirtin.  ( [Parametrelerin tam listesini](how-to-configure-auto-train.md#configure-experiment) ve olası değerlerini görün.)

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             X = X,
                             y = y,
                             **automl_settings,
                             )
```

### <a name="enable-model-explanations"></a>Model açıklamalarını etkinleştir

`AutoMLConfig` oluşturucusunda isteğe bağlı `model_explainability` parametresini ayarlayın. Ayrıca, model explainability özelliğini kullanmak için bir doğrulama veri çerçevesi nesnesi bir parametre olarak geçirilmelidir `X_valid`.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             X = X,
                             y = y,
                             **automl_settings,
                             model_explainability=True,
                             X_valid=X_test
                             )
```

## <a name="submit-training-experiment"></a>Eğitim deneme deneyimini gönder

Şimdi, algoritmayı otomatik olarak seçmek için yapılandırmayı, hiper parametreleri ve modeli eğiteye gönderebilirsiniz.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Aşağıdaki örneğe benzer bir çıktı görürsünüz:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************

     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Sonuçları keşfet

Bir grafik ve sonuç tablosu görmek için [eğitim öğreticisinde](tutorial-auto-train-models.md#explore-the-results) gösterildiği gibi aynı [jupi pencere öğesini](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) kullanabilirsiniz.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```

Burada pencere öğesinin statik bir görüntüsü yer alır.  Not defterinde, söz konusu çalıştırmaya yönelik çalıştırma özelliklerini ve çıkış günlüklerini görmek için tablodaki herhangi bir satıra tıklayabilirsiniz.   Her yineleme için kullanılabilir her ölçümün grafiğini görüntülemek için grafiğin üzerindeki açılan menüyü de kullanabilirsiniz.

![pencere öğesi tablosu](./media/how-to-auto-train-remote/table.png)
![pencere öğesi çizimi](./media/how-to-auto-train-remote/plot.png)

Pencere öğesi, bireysel çalıştırma ayrıntılarını görmek ve araştırmak için kullanabileceğiniz bir URL görüntüler.  

Jupyter not defterinde değilseniz, bu URL 'YI çalıştırmanın içinden görüntüleyebilirsiniz:

```
remote_run.get_portal_url()
```

Çalışma alanınızda aynı bilgiler bulunur.  Bu sonuçlar hakkında daha fazla bilgi edinmek için bkz. [otomatik makine öğrenimi sonuçlarını anlama](how-to-understand-automated-ml.md).

### <a name="view-logs"></a>Günlükleri görüntüleme

DSVM 'de `/tmp/azureml_run/{iterationid}/azureml-logs`altında günlükleri bulun.

## <a name="explain"></a>En iyi model açıklaması

Model açıklama verilerinin alınması, arka uçta nelerin çalıştığını artırmak için modeller hakkındaki ayrıntılı bilgileri görmenizi sağlar. Bu örnekte, model açıklamalarını yalnızca en uygun model için çalıştırırsınız. İşlem hattındaki tüm modeller için çalıştırırsanız, bu önemli bir çalışma süresine neden olur. Model açıklama bilgileri şunları içerir:

* shap_values: Shap lib tarafından oluşturulan açıklama bilgileri.
* expected_values: X_train verisi kümesine uygulanan modelin beklenen değeri.
* overall_summary: model düzeyi özellik önem değerleri azalan düzende sıralanır.
* overall_imp: Özellik adları overall_summary ile aynı sırada sıralanır.
* per_class_summary: sınıf düzeyi özellik önem değerleri azalan düzende sıralanır. Yalnızca sınıflandırma durumu için kullanılabilir.
* per_class_imp: Özellik adları per_class_summary ile aynı sırada sıralanır. Yalnızca sınıflandırma durumu için kullanılabilir.

Yinelemelerinizin en iyi işlem hattını seçmek için aşağıdaki kodu kullanın. `get_output` yöntemi, en iyi çalışmayı ve en son çağrıya yönelik olarak uygun modeli döndürür.

```python
best_run, fitted_model = remote_run.get_output()
```

`retrieve_model_explanation` işlevini içeri aktarın ve en iyi modelde çalıştırın.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Görüntülemek istediğiniz `best_run` açıklama değişkenlerinin sonuçlarını yazdırın.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

`best_run` açıklaması Özet değişkenlerinin yazdırılması aşağıdaki çıkışa neden olur.

![Model explainability konsol çıkışı](./media/how-to-auto-train-remote/expl-print.png)

Ayrıca, özellik önemini pencere öğesi kullanıcı arabiriminden veya [Azure Machine Learning Studio](https://ml.azure.com)'daki çalışma alanınızda görselleştirebilirsiniz. 

![Model explainability Kullanıcı arabirimi](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Örnek

[How-to-Use-azureml/Automated-Machine-Learning/Remote-amlcompute/Auto-ml-Remote-amlcompute. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) Not defteri, bu makaledeki kavramları gösterir.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Otomatik eğitime yönelik ayarları yapılandırmayı](how-to-configure-auto-train.md)öğrenin.
