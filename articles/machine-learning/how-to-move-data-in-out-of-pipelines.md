---
title: ML işlem hatlarında verileri taşıma
titleSuffix: Azure Machine Learning
description: Verilerin Azure Machine Learning işlem hatlarında giriş & çıkışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 04/01/2020
ms.custom: contperfq4
ms.openlocfilehash: 233361fb238342cde3c692174e85fb57f69979b1
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858460"
---
# <a name="moving-data-into-and-between-ml-pipeline-steps-python"></a>ML işlem hattı adımlarına ve adımlar arasında veri taşıma (Python)

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir Azure Machine Learning işlem hattındaki adımlar arasında veri alma, dönüştürme ve taşıma kodu sağlanmaktadır. Verilerin Azure Machine Learning nasıl çalıştığına ilişkin genel bakış için bkz. [Azure Storage hizmetlerindeki verilere erişme](how-to-access-data.md). Azure Machine Learning işlem hatlarının avantajları ve yapısı için bkz. [Azure Machine Learning işlem hatları nedir?](concept-ml-pipelines.md).

Bu makalede nasıl yapılacağı gösterilmektedir:

- Önceden `Dataset` var olan veriler için nesneleri kullanma
- Adımlarınız içindeki verilere erişin
- Verileri `Dataset` , eğitim ve doğrulama alt kümeleri gibi alt kümelere bölme
- Sonraki `PipelineData` işlem hattı adımına veri aktarmak için nesne oluşturma
- İşlem `PipelineData` hattı adımlarına girdi olarak nesneleri kullanma
- Kalıcı hale `Dataset` getirmek istediğiniz `PipelineData` yeni nesneler oluşturun

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da Python SDK 'sı aracılığıyla mevcut bir tane kullanın. `Workspace` Ve `Datastore` sınıfını içeri aktarın ve işlevini `config.json` `from_config()`kullanarak dosyadaki abonelik bilgilerinizi yükleyin. Bu işlev, varsayılan olarak geçerli dizindeki JSON dosyasını arar, ancak kullanarak `from_config(path="your/file/path")`dosyayı işaret etmek için bir yol parametresi de belirtebilirsiniz.

   ```python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

- Önceden varolan bazı veriler. Bu makalede bir [Azure Blob kapsayıcısının](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)kullanımı kısaca gösterilmektedir.

- İsteğe bağlı: [Azure MACHINE LEARNING SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)bölümünde açıklananlar gibi mevcut bir Machine Learning işlem hattı.

## <a name="use-dataset-objects-for-pre-existing-data"></a>Önceden `Dataset` var olan veriler için nesneleri kullanma 

Bir işlem hattına veri almanın tercih edilen yolu bir [veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) nesnesi kullanmaktır. `Dataset`nesneler, çalışma alanı genelinde kullanılabilir kalıcı verileri temsil eder.

Nesneleri oluşturmak ve kaydettirmek `Dataset` için birçok yol vardır. Tablo veri kümeleri, bir veya daha fazla dosyada kullanılabilen sınırlandırılmış verilere yöneliktir. Dosya veri kümeleri, ikili veriler (örneğin, görüntüler) veya ayrıştırılacak veriler içindir. Nesneleri oluşturmanın `Dataset` en basit programlama yolları, çalışma alanı depolamada veya genel URL 'lerde Mevcut blobları kullanmaktır:

```python
datastore = Datastore.get(workspace, 'training_data')
iris_dataset = Dataset.Tabular.from_delimited_files(DataPath(datastore, 'iris.csv'))

cats_dogs_dataset = Dataset.File.from_files(
    paths='https://download.microsoft.com/download/3/E/1/3E1C3F21-ECDB-4869-8368-6DEBA77B919F/kagglecatsanddogs_3367a.zip',
    archive_options=ArchiveOptions(archive_type=ArchiveType.ZIP, entry_glob='**/*.jpg')
)
```

Farklı seçeneklere ve farklı kaynaklara sahip veri kümeleri oluşturma, bunları kaydetme ve Azure Machine Learning Kullanıcı arabiriminde gözden geçirme, veri boyutunun işlem kapasitesiyle nasıl etkileşime gireceğini anlama ve bunların sürümü oluşturma hakkında daha fazla seçenek için bkz. [Azure Machine Learning veri kümeleri oluşturma](how-to-create-register-datasets.md). 

### <a name="pass-datasets-to-your-script"></a>Veri kümelerini betiğe geçirin

Veri kümesinin yolunu betiğe geçirmek için `Dataset` nesnenin `as_named_input()` metodunu kullanın. Sonuç `DatasetConsumptionConfig` nesnesini betiğe bir bağımsız değişken olarak geçirebilir ya da işlem hattı betiğinizin `inputs` bağımsız değişkenini kullanarak veri kümesini kullanarak `Run.get_context().input_datasets[]`alabilirsiniz.

Adlandırılmış bir giriş oluşturduktan sonra, erişim modunu seçebilirsiniz: `as_mount()` veya. `as_download()` Betik, veri kümenizdeki tüm dosyaları işliyorsa ve işlem kaynağınızın disk veri kümesi için yeterince büyükse, indirme erişimi modu daha iyi bir seçimdir. İndirme erişimi modu, çalışma zamanında veri akışı yükünü ortadan kaldırır. Betiğinizin bir veri kümesinin alt kümesine erişmesi veya işlem için çok büyük olması durumunda bağlama erişim modunu kullanın. Daha fazla bilgi için [bağlama ve indirme](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets#mount-vs-download) bölümünü okuyun

Bir veri kümesini ardışık düzen adımınızda geçirmek için:

1. Bir `TabularDataset.as_named_inputs()` `DatasetConsumptionConfig` nesne `FileDataset.as_named_input()` oluşturmak için veya (sonunda ' sonunda yok) kullanın
1. Erişim `as_mount()` modunu `as_download()` ayarlamak için veya kullanın
1. `arguments` Ya da `inputs` bağımsız değişkenini kullanarak veri kümelerini ardışık düzen adımlarınızı geçirin

Aşağıdaki kod parçacığında, bu adımları `PythonScriptStep` oluşturucunun içinde birleştirmenin yaygın bir deseninin gösterildiği gösterilmektedir: 

```python

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[iris_dataset.as_named_inputs('iris').as_mount()]
)
```

Ayrıca, `random_split()` ve `take_sample()` gibi yöntemleri kullanarak birden çok giriş oluşturabilir veya ardışık düzen adımınıza geçirilen veri miktarını azaltabilirsiniz:

```python
seed = 42 # PRNG seed
smaller_dataset = iris_dataset.take_sample(0.1, seed=seed) # 10%
train, test = smaller_dataset.random_split(percentage=0.8, seed=seed)

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[train.as_named_inputs('train').as_download(), test.as_named_inputs('test').as_download()]
)
```

### <a name="access-datasets-within-your-script"></a>Komut dosyası içindeki veri kümelerine erişin

İşlem hattı adım betiğinizin adlandırılmış girdileri, `Run` nesne içindeki bir sözlük olarak mevcuttur. Kullanarak etkin `Run` nesneyi alın `Run.get_context()` ve kullanarak `input_datasets`adlandırılmış girişlerin sözlüğünü alın. `DatasetConsumptionConfig` `arguments` Nesneyi `inputs` bağımsız değişken yerine bağımsız değişkenini kullanarak geçirtiniz kodu kullanarak `ArgParser` verilere erişin. Her iki teknik de aşağıdaki kod parçacığında gösterilmiştir.

```python
# In pipeline definition script:
# Code for demonstration only: It would be very confusing to split datasets between `arguments` and `inputs`
train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    arguments=['--training-folder', train.as_named_inputs('train').as_download()]
    inputs=[test.as_named_inputs('test').as_download()]
)

# In pipeline script
parser = argparse.ArgumentParser()
parser.add_argument('--training-folder', type=str, dest='train_folder', help='training data folder mounting point')
args = parser.parse_args()
training_data_folder = args.train_folder

testing_data_folder = Run.get_context().input_datasets['test']
```

Geçirilen değer, veri kümesi dosyalarının yolu olacak.

Ayrıca kayıtlı `Dataset` bir doğrudan erişim de mümkündür. Kayıtlı veri kümeleri kalıcı olduğundan ve bir çalışma alanı genelinde paylaşıldığından, bunları doğrudan alabilirsiniz:

```python
run = Run.get_context()
ws = run.experiment.workspace
ds = Dataset.get_by_name(workspace=ws, name='mnist_opendataset')
```

## <a name="use-pipelinedata-for-intermediate-data"></a>Ara `PipelineData` veriler için kullanın

Nesneler `Dataset` kalıcı verileri temsil ederken, ardışık düzen adımlarından çıktı olan geçici veriler Için [pipelinedata](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesneleri kullanılır. Bir `PipelineData` nesnenin kullanım ömrü tek bir işlem hattı adımından daha uzun olduğundan, bunları ardışık düzen tanımı betiğine tanımlarsınız. Bir `PipelineData` nesne oluşturduğunuzda, verilerin bulunacağı bir ad ve veri deposu sağlamanız gerekir. `PipelineData` Nesne (ler `PythonScriptStep` `arguments` ) `outputs` _i ve bağımsız değişkenlerini kullanarak kendi_ uygulamanıza geçirin:

```python
default_datastore = workspace.get_default_datastore()
dataprep_output = PipelineData("clean_data", datastore=default_datastore)

dataprep_step = PythonScriptStep(
    name="prep_data",
    script_name="dataprep.py",
    compute_target=cluster,
    arguments=["--output-path", dataprep_output]
    inputs=[Dataset.get_by_name(workspace, 'raw_data')],
    outputs=[dataprep_output]
)
```

`PipelineData` Nesnesini, anında karşıya yükleme sağlayan bir erişim modu kullanarak oluşturmayı seçebilirsiniz. Bu durumda, oluşturduğunuz `PipelineData`zaman öğesini `upload_mode` olarak `"upload"` ayarlayın ve verileri yazmak istediğiniz yolu belirtmek için `output_path_on_compute` bağımsız değişkenini kullanın:

```python
PipelineData("clean_data", datastore=def_blob_store, output_mode="upload", output_path_on_compute="clean_data_output/")
```

### <a name="use-pipelinedata-as-outputs-of-a-training-step"></a>Eğitim `PipelineData` adımının çıkışları olarak kullanın

Ardışık yollarınızın içinde `PythonScriptStep`, programın bağımsız değişkenlerini kullanarak kullanılabilir çıkış yollarını alabilirsiniz. Bu adım ilk ise ve çıktı verilerini başlatacaktır, belirtilen yolda dizini oluşturmanız gerekir. Daha sonra içine dahil etmek istediğiniz dosyaları yazabilirsiniz `PipelineData`.

```python
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()

# Make directory for file
os.makedirs(os.path.dirname(args.output_path), exist_ok=True)
with open(args.output_path, 'w') as f:
    f.write("Step 1's output")
```

' I ' a `PipelineData` ayarlanmış `is_directory` bağımsız değişkenle oluşturduysanız, yalnızca `os.makedirs()` çağrıyı gerçekleştirmek için yeterli olur ve ardından yola beklediğiniz dosyaları yazmanız ücretsizdir. `True` Daha ayrıntılı bilgi için bkz. [Pipelinedata](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) Reference belgeleri.

### <a name="read-pipelinedata-as-inputs-to-non-initial-steps"></a>İlk `PipelineData` olmayan adımlara giriş olarak oku

İlk işlem hattı adımı, `PipelineData` yola bazı veriler yazar ve bu ilk adımın çıktısı haline gelirse, sonraki bir adımda giriş olarak kullanılabilir:

```python
step1_output_data = PipelineData("processed_data", datastore=def_blob_store, output_mode="upload")

step1 = PythonScriptStep(
    name="generate_data",
    script_name="step1.py",
    runconfig = aml_run_config,
    arguments = ["--output_path", step1_output_data],
    inputs=[],
    outputs=[step1_output_data]
)

step2 = PythonScriptStep(
    name="read_pipeline_data",
    script_name="step2.py",
    compute_target=compute,
    runconfig = aml_run_config,
    arguments = ["--pd", step1_output_data],
    inputs=[step1_output_data]
)

pipeline = Pipeline(workspace=ws, steps=[step1, step2])
```

Bir `PipelineData` girdinin değeri, önceki çıktının yoludur. Daha önce gösterildiği gibi, ilk adım tek bir dosya yazdı, bu, şu şekilde görünebilir: 

```python
parser = argparse.ArgumentParser()
parser.add_argument('--pd', dest='pd', required=True)
args = parser.parse_args()

with open(args.pd) as f:
    print(f.read())
```

## <a name="convert-pipelinedata-objects-to-datasets"></a>Nesneleri `PipelineData` s öğesine `Dataset`Dönüştür

Bir çalıştırmanın süresinden daha uzun bir süre `PipelineData` için kullanılabilir hale getirmek istiyorsanız, `as_dataset()` işlevini ' a `Dataset`dönüştürmek için kullanın. Sonra, `Dataset`çalışma alanınızda birinci sınıf bir vatandaşlık yaparak öğesini kaydedebilirsiniz. İşlem hattı `PipelineData` her çalıştığında nesneniz farklı bir yola sahip olacağı `create_new_version` için, bir `True` `Dataset` `PipelineData` nesneden oluşturulan bir kayıt sırasında olarak ayarlamanız kesinlikle önerilir.

```python
step1_output_ds = step1_output_data.as_dataset()
step1_output_ds.register(name="processed_data", create_new_version=True)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Azure Machine Learning SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)
