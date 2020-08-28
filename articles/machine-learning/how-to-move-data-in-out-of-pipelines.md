---
title: ML işlem hatlarında verileri taşıma
titleSuffix: Azure Machine Learning
description: Verilerin Azure Machine Learning işlem hatlarında giriş & çıkışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 08/20/2020
ms.topic: conceptual
ms.custom: how-to, contperfq4, devx-track-python
ms.openlocfilehash: f870f90ede4465bf9ebf5c886e1ebb7aa76acaaa
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88997906"
---
# <a name="moving-data-into-and-between-ml-pipeline-steps-python"></a>ML işlem hattı adımlarına ve adımlar arasında veri taşıma (Python)

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir Azure Machine Learning işlem hattındaki adımlar arasında veri alma, dönüştürme ve taşıma kodu sağlanmaktadır. Verilerin Azure Machine Learning nasıl çalıştığına ilişkin genel bakış için bkz. [Azure Storage hizmetlerindeki verilere erişme](how-to-access-data.md). Azure Machine Learning işlem hatlarının avantajları ve yapısı için bkz. [Azure Machine Learning işlem hatları nedir?](concept-ml-pipelines.md).

Bu makalede nasıl yapılacağı gösterilmektedir:

- `Dataset`Önceden var olan veriler için nesneleri kullanma
- Adımlarınız içindeki verilere erişin
- `Dataset`Verileri, eğitim ve doğrulama alt kümeleri gibi alt kümelere bölme
- `PipelineData`Sonraki işlem hattı adımına veri aktarmak için nesne oluşturma
- İşlem `PipelineData` hattı adımlarına girdi olarak nesneleri kullanma
- `Dataset`Kalıcı hale getirmek istediğiniz yeni nesneler oluşturun `PipelineData`

> [!TIP]
> Ardışık düzen adımları arasında geçici verileri geçirmek ve işlem hattı çalıştırmalarından sonra verilerinizi sürdürmek için geliştirilmiş bir deneyim, genel önizleme sınıflarında ve ' de bulunur  `OutputFileDatasetConfig` `OutputTabularDatasetConfig` .  Bu sınıflar deneysel önizleme özelliklerine sahiptir ve herhangi bir zamanda değişebilir.
> 
>Deneysel özellikler hakkında daha fazla bilgi için bkz https://aka.ms/azuremlexperimental ..

## <a name="prerequisites"></a>Önkoşullar

Şunlara ihtiyacınız var:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da Python SDK 'sı aracılığıyla mevcut bir tane kullanın. `Workspace`Ve sınıfını içeri aktarın `Datastore` ve işlevini kullanarak dosyadaki abonelik bilgilerinizi yükleyin `config.json` `from_config()` . Bu işlev, varsayılan olarak geçerli dizindeki JSON dosyasını arar, ancak kullanarak dosyayı işaret etmek için bir yol parametresi de belirtebilirsiniz `from_config(path="your/file/path")` .

   ```python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

- Önceden varolan bazı veriler. Bu makalede bir [Azure Blob kapsayıcısının](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)kullanımı kısaca gösterilmektedir.

- İsteğe bağlı: [Azure MACHINE LEARNING SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)bölümünde açıklananlar gibi mevcut bir Machine Learning işlem hattı.

## <a name="use-dataset-objects-for-pre-existing-data"></a>`Dataset`Önceden var olan veriler için nesneleri kullanma 

Bir işlem hattına veri almanın tercih edilen yolu bir [veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) nesnesi kullanmaktır. `Dataset` nesneler, çalışma alanı genelinde kullanılabilir kalıcı verileri temsil eder.

Nesneleri oluşturmak ve kaydettirmek için birçok yol vardır `Dataset` . Tablo veri kümeleri, bir veya daha fazla dosyada kullanılabilen sınırlandırılmış verilere yöneliktir. Dosya veri kümeleri, ikili veriler (örneğin, görüntüler) veya ayrıştırılacak veriler içindir. Nesneleri oluşturmanın en basit programlama yolları, `Dataset` çalışma alanı depolamada veya genel URL 'lerde Mevcut blobları kullanmaktır:

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

Veri kümesinin yolunu betiğe geçirmek için `Dataset` nesnenin `as_named_input()` metodunu kullanın. Sonuç `DatasetConsumptionConfig` nesnesini betiğe bir bağımsız değişken olarak geçirebilir ya da işlem `inputs` hattı betiğinizin bağımsız değişkenini kullanarak veri kümesini kullanarak alabilirsiniz `Run.get_context().input_datasets[]` .

Adlandırılmış bir giriş oluşturduktan sonra, erişim modunu seçebilirsiniz: `as_mount()` veya `as_download()` . Betik, veri kümenizdeki tüm dosyaları işliyorsa ve işlem kaynağınızın disk veri kümesi için yeterince büyükse, indirme erişimi modu daha iyi bir seçimdir. İndirme erişimi modu, çalışma zamanında veri akışı yükünü ortadan kaldırır. Betiğinizin bir veri kümesinin alt kümesine erişmesi veya işlem için çok büyük olması durumunda bağlama erişim modunu kullanın. Daha fazla bilgi için [bağlama ve indirme](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets#mount-vs-download) bölümünü okuyun

Bir veri kümesini ardışık düzen adımınızda geçirmek için:

1. `TabularDataset.as_named_inputs()` `FileDataset.as_named_input()` Bir nesne oluşturmak için veya (sonunda ' sonunda yok) kullanın `DatasetConsumptionConfig`
1. `as_mount()` `as_download()` Erişim modunu ayarlamak için veya kullanın
1. `arguments`Ya da bağımsız değişkenini kullanarak veri kümelerini ardışık düzen adımlarınızı geçirin `inputs`

Aşağıdaki kod parçacığında, bu adımları oluşturucunun içinde birleştirmenin yaygın bir deseninin gösterildiği gösterilmektedir `PythonScriptStep` : 

```python

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[iris_dataset.as_named_inputs('iris').as_mount()]
)
```

Ayrıca, ve gibi yöntemleri kullanarak `random_split()` `take_sample()` birden çok giriş oluşturabilir veya ardışık düzen adımınıza geçirilen veri miktarını azaltabilirsiniz:

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

İşlem hattı adım betiğinizin adlandırılmış girdileri, nesne içindeki bir sözlük olarak mevcuttur `Run` . Kullanarak etkin `Run` nesneyi alın `Run.get_context()` ve kullanarak adlandırılmış girişlerin sözlüğünü alın `input_datasets` . Nesneyi bağımsız değişken yerine `DatasetConsumptionConfig` bağımsız değişkenini kullanarak geçirtiniz `arguments` `inputs` kodu kullanarak verilere erişin `ArgParser` . Her iki teknik de aşağıdaki kod parçacığında gösterilmiştir.

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

Ayrıca kayıtlı bir doğrudan erişim de mümkündür `Dataset` . Kayıtlı veri kümeleri kalıcı olduğundan ve bir çalışma alanı genelinde paylaşıldığından, bunları doğrudan alabilirsiniz:

```python
run = Run.get_context()
ws = run.experiment.workspace
ds = Dataset.get_by_name(workspace=ws, name='mnist_opendataset')
```

## <a name="use-pipelinedata-for-intermediate-data"></a>`PipelineData`Ara veriler için kullanın

`Dataset`Nesneler kalıcı verileri temsil ederken, ardışık düzen adımlarından çıktı olan geçici veriler Için [pipelinedata](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesneleri kullanılır. Bir nesnenin kullanım ömrü `PipelineData` tek bir işlem hattı adımından daha uzun olduğundan, bunları ardışık düzen tanımı betiğine tanımlarsınız. Bir `PipelineData` nesne oluşturduğunuzda, verilerin bulunacağı bir ad ve veri deposu sağlamanız gerekir. `PipelineData`Nesne (ler) `PythonScriptStep` _both_ `arguments` i ve bağımsız değişkenlerini kullanarak kendi uygulamanıza geçirin `outputs` :

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

`PipelineData`Nesnesini, anında karşıya yükleme sağlayan bir erişim modu kullanarak oluşturmayı seçebilirsiniz. Bu durumda, oluşturduğunuz zaman `PipelineData` öğesini `upload_mode` olarak ayarlayın `"upload"` ve `output_path_on_compute` verileri yazmak istediğiniz yolu belirtmek için bağımsız değişkenini kullanın:

```python
PipelineData("clean_data", datastore=def_blob_store, output_mode="upload", output_path_on_compute="clean_data_output/")
```

> [!TIP]
> İşlem hattı adımları arasında ara verileri geçirmek için geliştirilmiş bir deneyim, genel önizleme sınıfı ile sunulmaktadır `OutputFileDatasetConfig` . `OutputFileDatasetConfig` [SDK başvuru belgelerindeki](https://docs.microsoft.com/python/api/azureml-core/azureml.data.outputfiledatasetconfig?view=azure-ml-py)tasarım desenleri ve yöntemleri hakkında daha fazla bilgi edinin.

### <a name="use-pipelinedata-as-outputs-of-a-training-step"></a>`PipelineData`Eğitim adımının çıkışları olarak kullanın
Ardışık `PythonScriptStep` yollarınızın içinde, programın bağımsız değişkenlerini kullanarak kullanılabilir çıkış yollarını alabilirsiniz. Bu adım ilk ise ve çıktı verilerini başlatacaktır, belirtilen yolda dizini oluşturmanız gerekir. Daha sonra içine dahil etmek istediğiniz dosyaları yazabilirsiniz `PipelineData` .

```python
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()
# Make directory for file
os.makedirs(os.path.dirname(args.output_path), exist_ok=True)
with open(args.output_path, 'w') as f:
    f.write("Step 1's output")
```

`PipelineData` `is_directory` ' I ' a ayarlanmış bağımsız değişkenle oluşturduysanız `True` , yalnızca çağrıyı gerçekleştirmek için yeterli olur `os.makedirs()` ve ardından yola beklediğiniz dosyaları yazmanız ücretsizdir. Daha ayrıntılı bilgi için bkz. [Pipelinedata](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) Reference belgeleri.


### <a name="read-pipelinedata-as-inputs-to-non-initial-steps"></a>`PipelineData`İlk olmayan adımlara giriş olarak oku

İlk işlem hattı adımı, yola bazı veriler yazar `PipelineData` ve bu ilk adımın çıktısı haline gelirse, sonraki bir adımda giriş olarak kullanılabilir:

```python
step1_output_data = PipelineData("processed_data", datastore=def_blob_store, output_mode="upload")
# get adls gen 2 datastore already registered with the workspace
datastore = workspace.datastores['my_adlsgen2']

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

Bir girdinin değeri, `PipelineData` önceki çıktının yoludur. 

> [!TIP]
> İşlem hattı adımları arasında ara verileri geçirmek için geliştirilmiş bir deneyim, genel önizleme sınıfı ile sunulmaktadır `OutputFileDatasetConfig` . `OutputFileDatasetConfig` [SDK başvuru belgelerindeki](https://docs.microsoft.com/python/api/azureml-core/azureml.data.outputfiledatasetconfig?view=azure-ml-py)tasarım desenleri ve yöntemleri hakkında daha fazla bilgi edinin.

Daha önce gösterildiği gibi, ilk adım tek bir dosya yazdı, bu, şu şekilde görünebilir: 

```python
parser = argparse.ArgumentParser()
parser.add_argument('--pd', dest='pd', required=True)
args = parser.parse_args()
with open(args.pd) as f:
    print(f.read())
```

## <a name="convert-pipelinedata-objects-to-datasets"></a>`PipelineData`Nesneleri s öğesine `Dataset` Dönüştür

`PipelineData`Bir çalıştırmanın süresinden daha uzun bir süre için kullanılabilir hale getirmek istiyorsanız, `as_dataset()` işlevini ' a dönüştürmek için kullanın `Dataset` . Sonra, `Dataset` çalışma alanınızda birinci sınıf bir vatandaşlık yaparak öğesini kaydedebilirsiniz. `PipelineData`İşlem hattı her çalıştığında nesneniz farklı bir yola sahip olacağı `create_new_version` için, `True` `Dataset` bir nesneden oluşturulan bir kayıt sırasında olarak ayarlamanız kesinlikle önerilir `PipelineData` .

```python
step1_output_ds = step1_output_data.as_dataset()
step1_output_ds.register(name="processed_data", create_new_version=True)

```
> [!TIP]
> İşlem hattı çalıştırmalarınızın dışındaki ara verilerinizi kalıcı hale getiren gelişmiş bir deneyim, genel önizleme sınıfı ile sunulmaktadır `OutputFileDatasetConfig` . `OutputFileDatasetConfig` [SDK başvuru belgelerindeki](https://docs.microsoft.com/python/api/azureml-core/azureml.data.outputfiledatasetconfig?view=azure-ml-py)tasarım desenleri ve yöntemleri hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Azure Machine Learning SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)
