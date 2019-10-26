---
title: Veri kümesi sürümü oluşturma
titleSuffix: Azure Machine Learning service
description: Veri kümelerinizi sürümüne en iyi uygulamayı ve sürüm oluşturma ile makine öğrenimi ardışık düzenleri ile nasıl çalıştığını öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: sihhu
ms.reviewer: nibaccam
ms.date: 10/25/2019
ms.custom: ''
ms.openlocfilehash: a361800623796cb3bc26a47c4f8f532503836124
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72902008"
---
# <a name="version-and-track-datasets-in-experiments"></a>Denemeleri içinde veri kümelerini sürüm ve izleme

Bu nasıl yapılır, reproducibility için Azure Machine Learning veri kümelerini nasıl kullanacağınızı ve izleyeceğinizi öğrenirsiniz. Veri kümesi sürümü oluşturma, verilerinizin durumuna yer işaretinin bir yoludur, böylece gelecekteki denemeleri için veri kümesinin belirli bir sürümünü uygulayabilirsiniz.

Sürüm oluşturma için göz önünde bulundurulması gereken tipik senaryolar:

* Yeni veriler yeniden eğitimine uygun olduğunda.
* Farklı veri hazırlama veya özellik Mühendisliği yaklaşımları uyguladığınızda.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır için şunları yapmanız gerekir:

- Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), [azureml veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset?view=azure-ml-py) paketini içerir.
    
- [Azure Machine Learning çalışma alanı](concept-workspace.md). Aşağıdaki kodla mevcut bir tane alın veya [Yeni bir çalışma alanı oluşturun](how-to-manage-workspace.md).

    ```Python
    import azureml.core
    from azureml.core import Workspace
    
    ws = Workspace.from_config()
    ```
- [Azure Machine Learning veri kümesi](how-to-create-register-datasets.md).

<a name="register"></a>

## <a name="register-and-retrieve-dataset-versions"></a>Veri kümesi sürümlerini kaydetme ve alma

Bir veri kümesini kaydetmek, denemeleri ve iş arkadaşlarınızla sürüm, yeniden kullanma ve paylaşma olanağı sağlar. Aynı ada sahip birden çok veri kümesini kaydedebilir ve adı ve sürüm numarasına göre belirli bir sürümü alabilirsiniz.

### <a name="register-a-dataset-version"></a>Veri kümesi sürümünü kaydetme

Aşağıdaki kod, `create_new_version` parametresini `True`ayarlayarak `titanic_ds`veri kümesinin yeni bir sürümünü kaydeder. Çalışma alanına kayıtlı mevcut `titanic_ds` yoksa, adı `titanic_ds` yeni bir veri kümesi oluşturur ve sürümünü 1 olarak ayarlar.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data',
                                 create_new_version = True)
```

### <a name="retrieve-a-dataset-by-name"></a>Bir veri kümesini ada göre al

Varsayılan olarak, `Dataset` sınıfındaki [get_by_name ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) yöntemi, çalışma alanına kayıtlı veri kümesinin en son sürümünü döndürür. 

Aşağıdaki kod `titanic_ds` veri kümesinin 1. sürümünü alır.

```Python
from azureml.core import Dataset
# Get a dataset by name and version number
titanic_ds = Dataset.get_by_name(workspace = workspace,
                                 name = 'titanic_ds', 
                                 version = 1)
```

<a name="best-practice"></a>

## <a name="versioning-best-practice"></a>Sürüm oluşturma en iyi uygulaması

Bir veri kümesi sürümü oluştururken, çalışma alanıyla birlikte verilerin ek bir kopyasını **oluşturuyoruz.** Veri kümeleri depolama hizmetinizdeki verilere başvurulardır, bu nedenle depolama hizmetiniz tarafından yalnızca tek bir gerçeği kaynağı yönetilir. 

>[!IMPORTANT]
> Veri kümeniz tarafından başvurulan verilerin üzerine yazılırsa veya silinirse, veri kümesinin belirli bir sürümünü çağırmak değişikliği döndürmez. 

Veri kümesinden verileri yüklerken, veri kümesi tarafından başvurulan geçerli veri içeriğini her zaman yükler. Her bir veri kümesi sürümünün reproducibility sağlamak istiyorsanız, veri kümesi sürümü tarafından başvurulan veri içeriğini değiştirmenizi öneririz. Yeni veriler geldiğinde, yeni veri dosyalarını ayrı bir veri klasörüne kaydedin ve bu yeni veri klasöründeki verileri dahil etmek için yeni bir veri kümesi sürümü oluşturun.

Aşağıdaki görüntü ve örnek kod, veri klasörlerinizi yapılandırmak için önerilen yolu gösterir ve bu klasörlere başvuran veri kümesi sürümlerini oluşturur.

![Klasör yapısı](media/how-to-version-datasets/folder-image.png)

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

## <a name="version-a-pipeline-output-dataset"></a>İşlem hattı çıkış veri kümesi sürümü

Veri kümesini her makine öğrenimi (ML) ardışık düzen adımının giriş ve çıkışı olarak kullanabilirsiniz. İşlem hatlarını yeniden çalıştırdığınızda, her bir ardışık düzen adımının çıktısı yeni bir veri kümesi sürümü olarak kaydedilir. 

Bu işlem hattı her yeniden çalıştırıldığında, ML ardışık düzenleri her adımın çıkışını yeni bir klasöre doldurduğundan, sürümlü çıkış veri kümeleri tekrarlanabilir.

```Python
from azureml.core import Dataset
from azureml.pipeline.steps import PythonScriptStep
from azureml.pipeline.core import Pipeline, PipelineData

# get input dataset 
input_ds = Dataset.get_by_name(workspace, 'weather_ds')

# register pipeline output as dataset
output_ds = PipelineData('prepared_weather_ds', datastore=datastore).as_dataset()
output_ds = output_ds.register(name='prepared_weather_ds', create_new_version=True)

# configure pipeline step to use dataset as the input and output
prep_step = PythonScriptStep(script_name="prepare.py",
                             inputs=[input_ds.as_named_input('weather_ds')],
                             outputs=[output_ds],
                             compute_target=compute_target,
                             source_directory=project_folder)
```

<a name="track"></a>

## <a name="track-datasets-in-experiments"></a>Denemeleri içinde veri kümelerini izleme

Her makine öğrenimi denemesi için, girdi olarak kullanılan veri kümelerini, kayıtlı modelin `Run` nesnesi aracılığıyla kolayca izleyebilirsiniz.

Modelleri veri kümelerine kaydetmek için aşağıdaki kodu kullanın.

```Python
model = run.register_model(model_name='keras-mlp-mnist',
                           model_path=model_path,
                           datasets =[('training data',train_dataset)])
```

Kayıttan sonra, Python veya [çalışma alanı giriş sayfası](https://ml.azure.com/)kullanılarak veri kümesiyle kayıtlı modellerin listesini görebilirsiniz.

Aşağıdaki kod, bu deneme çalıştırması ile hangi giriş veri kümelerinin kullanıldığını izlemek için [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-details--) yöntemini kullanır.

```Python
# get input datasets
inputs = run.get_details()['inputDatasets']
train_dataset = inputs[0]['dataset']

# list the files referenced by train_dataset
train_dataset.to_path()
```

Ayrıca, [çalışma alanı giriş sayfasını (Önizleme)](https://ml.azure.com/)kullanarak denemeleri 'tan `input_datasets` bulabilirsiniz. 

Aşağıdaki görüntüde, çalışma alanı giriş sayfasında bir deneyin giriş veri kümesinin nerede bulunacağı gösterilmektedir. Bu örnekte, **denemeleri** bölmenize gidin ve denemenizin belirli bir çalışması için **özellikler** sekmesini açın `keras-mnist`. 

![Giriş veri kümeleri](media/how-to-version-datasets/input-datasets.png)

Ayrıca, çalışma alanı giriş sayfası ile veri kümenizi kullanan modelleri de bulabilirsiniz. Aşağıdaki görünüm, varlıklar altındaki veri kümeleri dikey penceresinden yapılır. Veri kümesini seçin ve bu veri kümesini kullanan modellerin listesi için modeller sekmesine gidin. 

![Giriş veri kümesi modelleri](media/how-to-version-datasets/dataset-models.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümeleriyle eğitme](how-to-train-with-datasets.md).
* [Veri kümelerinde daha fazla örnek Not defteri](https://aka.ms/dataset-tutorial).
