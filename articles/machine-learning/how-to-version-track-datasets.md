---
title: Dataset sürüm
titleSuffix: Azure Machine Learning
description: Veri kümelerinizi en iyi şekilde nasıl sürümedize edin ve makine öğrenimi boru hatlarıyla sürüm nasıl çalışır öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 03/09/2020
ms.custom: ''
ms.openlocfilehash: acbd2e3ba756255cbc69ae8a7b7ad62d7a1c1c5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528481"
---
# <a name="version-and-track-datasets-in-experiments"></a>Denemelerdeki veri kümelerini sürüm ve izleme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, yeniden çoğaltılabilmek için Azure Machine Learning veri kümelerini nasıl sürümleyip izleyeceğinizi öğreneceksiniz. Dataset sürüm, gelecekteki denemeler için veri kümesinin belirli bir sürümünü uygulayabilmeniz için verilerinizin durumunu yer imi belirlemenin bir yoludur.

Tipik sürüm senaryoları:

* Yeniden eğitim için yeni veriler kullanılabilir olduğunda
* Farklı veri hazırlama veya özellik mühendisliği yaklaşımları uygularken

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için şunları yapmanız gerekir:

- [Python için Azure Machine Learning SDK yüklendi.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) Bu SDK [azureml-datasets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset?view=azure-ml-py) paketini içerir.
    
- [Azure Makine Öğrenimi çalışma alanı.](concept-workspace.md) Aşağıdaki kodu çalıştırarak varolan bir tane alın veya [yeni bir çalışma alanı oluşturun.](how-to-manage-workspace.md)

    ```Python
    import azureml.core
    from azureml.core import Workspace
    
    ws = Workspace.from_config()
    ```
- [Azure Machine Learning veri kümesi.](how-to-create-register-datasets.md)

<a name="register"></a>

## <a name="register-and-retrieve-dataset-versions"></a>Veri kümesi sürümlerini kaydetme ve alma

Bir veri kümesini kaydederek, bunları denemeler arasında ve iş arkadaşlarınızla sürümleyebilir, yeniden kullanabilir ve paylaşabilirsiniz. Aynı ad altında birden çok veri kümesi kaydedebilir ve belirli bir sürümü ada ve sürüm numarasına göre alabilirsiniz.

### <a name="register-a-dataset-version"></a>Veri kümesi sürümünü kaydetme

Aşağıdaki kod, parametreyi `titanic_ds` `create_new_version` . `True` Çalışma alanına kayıtlı `titanic_ds` varolan bir veri kümesi yoksa, kod adında `titanic_ds` yeni bir veri kümesi oluşturur ve sürümünü 1 olarak ayarlar.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data',
                                 create_new_version = True)
```
Ayrıca, bir veri kümesinin yeni bir sürümünü 

### <a name="retrieve-a-dataset-by-name"></a>Ada göre bir veri kümesi alma

Varsayılan olarak, sınıftaki `Dataset` [get_by_name()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) yöntemi, çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür. 

Aşağıdaki kod, veri kümesinin sürüm 1'ini `titanic_ds` alır.

```Python
from azureml.core import Dataset
# Get a dataset by name and version number
titanic_ds = Dataset.get_by_name(workspace = workspace,
                                 name = 'titanic_ds', 
                                 version = 1)
```

<a name="best-practice"></a>

## <a name="versioning-best-practice"></a>En iyi uygulama sürüm

Bir veri kümesi sürümü oluşturduğunuzda, çalışma alanıyla birlikte fazladan bir veri kopyası *oluşturmuyorsunuz.* Veri kümeleri depolama hizmetinizdeki verilere yapılan başvurular olduğundan, depolama hizmetiniz tarafından yönetilen tek bir doğruluk kaynağınız vardır.

>[!IMPORTANT]
> Veri kümeniz tarafından başvurulan veriler üzerine yazılmış veya silinmişse, veri kümesinin belirli bir sürümünü aramak değişikliği *geri* almaz.

Bir veri kümesinden veri yüklediğinizde, veri kümesi tarafından başvurulan geçerli veri içeriği her zaman yüklenir. Her veri kümesi sürümünün tekrarlanabilir olduğundan emin olmak istiyorsanız, veri kümesi sürümütarafından başvurulan veri içeriğini değiştirmemenizi öneririz. Yeni veriler geldiğinde, yeni veri dosyalarını ayrı bir veri klasörüne kaydedin ve ardından bu yeni klasörden veri eklemek için yeni bir veri kümesi sürümü oluşturun.

Aşağıdaki resim ve örnek kod, veri klasörlerinizi yapılandırmanın ve bu klasörlere başvuran veri kümesi sürümleri oluşturmanın önerilen yolunu gösterir:

![Klasör yapısı](./media/how-to-version-track-datasets/folder-image.png)

```Python
from azureml.core import Dataset

# get the default datastore of the workspace
datastore = workspace.get_default_datastore()

# create & register weather_ds version 1 pointing to all files in the folder of week 27
datastore_path1 = [(datastore, 'Weather/week 27')]
dataset1 = Dataset.File.from_files(path=datastore_path1)
dataset1.register(workspace = workspace,
                  name = 'weather_ds',
                  description = 'weather data in week 27',
                  create_new_version = True)

# create & register weather_ds version 2 pointing to all files in the folder of week 27 and 28
datastore_path2 = [(datastore, 'Weather/week 27'), (datastore, 'Weather/week 28')]
dataset2 = Dataset.File.from_files(path = datastore_path2)
dataset2.register(workspace = workspace,
                  name = 'weather_ds',
                  description = 'weather data in week 27, 28',
                  create_new_version = True)

```

<a name="pipeline"></a>

## <a name="version-a-pipeline-output-dataset"></a>Sürüm bir boru hattı çıktı veri kümesi

Her Machine Learning boru hattı adımının giriş ve çıktısı olarak bir veri kümesi kullanabilirsiniz. Ardışık hatları yeniden çalıştırdığınızda, her ardışık iş adımın çıktısı yeni bir veri kümesi sürümü olarak kaydedilir.

Machine Learning ardışık hatları, her adımın çıktısını her ardışık ardışık çalışma sürümü yeniden çalıştığında yeni bir klasöre doldurduğundan, sürümlenmiş çıktı veri kümeleri yeniden üretilebilir. [Ardışık hatlar'daki veri kümeleri](how-to-create-your-first-pipeline.md#steps)hakkında daha fazla bilgi edinin.

```Python
from azureml.core import Dataset
from azureml.pipeline.steps import PythonScriptStep
from azureml.pipeline.core import Pipeline, PipelineData
from azureml.core. runconfig import CondaDependencies, RunConfiguration

# get input dataset 
input_ds = Dataset.get_by_name(workspace, 'weather_ds')

# register pipeline output as dataset
output_ds = PipelineData('prepared_weather_ds', datastore=datastore).as_dataset()
output_ds = output_ds.register(name='prepared_weather_ds', create_new_version=True)

conda = CondaDependencies.create(
    pip_packages=['azureml-defaults', 'azureml-dataprep[fuse,pandas]'], 
    pin_sdk_version=False)

run_config = RunConfiguration()
run_config.environment.docker.enabled = True
run_config.environment.python.conda_dependencies = conda

# configure pipeline step to use dataset as the input and output
prep_step = PythonScriptStep(script_name="prepare.py",
                             inputs=[input_ds.as_named_input('weather_ds')],
                             outputs=[output_ds],
                             runconfig=run_config,
                             compute_target=compute_target,
                             source_directory=project_folder)
```

<a name="track"></a>

## <a name="track-datasets-in-experiments"></a>Denemelerdeki veri kümelerini izleme

Her Machine Learning denemesi için, deneme `Run` nesnesi aracılığıyla giriş olarak kullanılan veri kümelerini kolayca izleyebilirsiniz.

Aşağıdaki kod, [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-details--) deneme çalışmasında hangi giriş veri kümelerinin kullanıldığını izlemek için yöntemi kullanır:

```Python
# get input datasets
inputs = run.get_details()['inputDatasets']
input_dataset = inputs[0]['dataset']

# list the files referenced by input_dataset
input_dataset.to_path()
```

Ayrıca kullanarak `input_datasets` https://ml.azure.com/deneylerden bulabilirsiniz. 

Aşağıdaki resim, Azure Machine Learning stüdyosundaki bir denemenin giriş veri kümesinin nerede bulunacağını gösterir. Bu örnekiçin, **Denemeler** bölmenize gidin ve denemenizin belirli bir `keras-mnist`çalışması için **Özellikler** sekmesini açın.

![Giriş veri kümeleri](./media/how-to-version-track-datasets/input-datasets.png)

Modelleri veri kümelerine kaydetmek için aşağıdaki kodu kullanın:

```Python
model = run.register_model(model_name='keras-mlp-mnist',
                           model_path=model_path,
                           datasets =[('training data',train_dataset)])
```

Kayıt tan sonra Python'u kullanarak veri kümesine kayıtlı modellerin https://ml.azure.com/listesini görebilir veya .'ye gidebilirsiniz.

Aşağıdaki görünüm **Varlıklar**altındaki **Veri Kümeleri** bölmesinden dir. Veri kümesini seçin ve ardından veri kümesine kayıtlı modellerin listesi için **Modeller** sekmesini seçin. 

![Giriş veri setleri modelleri](./media/how-to-version-track-datasets/dataset-models.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümeleriyle eğitme](how-to-train-with-datasets.md)
* [Daha fazla örnek veri seti dizüstü bilgisayarlar](https://aka.ms/dataset-tutorial)
