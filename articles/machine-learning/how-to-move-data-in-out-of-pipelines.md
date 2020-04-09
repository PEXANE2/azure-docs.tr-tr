---
title: ML boru hatlarından giriş ve çıkış verileri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ardışık hatlarında veri hazırlama, tüketme ve oluşturma
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 04/01/2020
ms.openlocfilehash: 3dd1a82bf7fad1f201f5c0f52af944ef44a3fdf9
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879771"
---
# <a name="moving-data-into-and-between-ml-pipeline-steps-python"></a>Verileri ML ardışık satır adımlarına ve arasına taşıma (Python)

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Veriler, makine öğrenimi boru hatlarının merkezinde yer almaktadır. Bu makalede, bir Azure Machine Learning ardışık işlem işakisi için veri alma, dönüştürme ve taşıma için kod sağlar. Azure Machine Learning'de verilerin nasıl çalıştığına genel bakış için Azure [depolama hizmetlerinde Erişim verilerine](how-to-access-data.md)bakın. Azure Machine Learning ardışık hatlarının avantajları ve yapısı [için](concept-ml-pipelines.md)bkz.

Bu makalede, nasıl gösterecektir:

- Önceden `Dataset` varolan veriler için nesneleri kullanma
- Adımlarınızda verilere erişin
- Verileri `Dataset` eğitim ve doğrulama alt kümeleri gibi alt kümelere bölme
- Verileri `PipelineData` bir sonraki ardışık aşamaya aktarmak için nesneler oluşturma
- Nesneleri, ardışık hat lar adımlarına giriş olarak kullanma `PipelineData`
- Devam `Dataset` etmek istediğiniz `PipelineData` yeni nesneler oluşturun

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya Azure Machine Learning [stüdyosuna](https://ml.azure.com/)erişim.

- Azure Machine Learning çalışma alanı.
  
  [Bir Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md) veya Python SDK aracılığıyla varolan bir çalışma alanı kullanın. Ve `Workspace` `Datastore` sınıfı içe aktarın ve abonelik `config.json` bilgilerinizi işlevi `from_config()`kullanarak dosyadan yükleyin. Bu işlev varsayılan olarak geçerli dizinde JSON dosyasını arar, ancak dosyayı kullanarak `from_config(path="your/file/path")`işaret etmek için bir yol parametresi de belirtebilirsiniz.

   ```python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

- Bazı önceden varolan veriler. Bu makalede kısaca bir [Azure blob kapsayıcı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)kullanımını gösterir.

- İsteğe bağlı: [Azure Machine Learning SDK ile makine öğrenimi ardışık hatlar oluştur ve çalıştır'da](how-to-create-your-first-pipeline.md)açıklanan gibi varolan bir makine öğrenme ardışık

## <a name="use-dataset-objects-for-pre-existing-data"></a>Önceden `Dataset` varolan veriler için nesneleri kullanma 

Veri bir ardışık içine almak için tercih edilen yolu bir [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) nesnesi kullanmaktır. `Dataset`nesneler, çalışma alanı boyunca kullanılabilen kalıcı verileri temsil eder.

Nesneleri oluşturmanın ve kaydetmenin `Dataset` birçok yolu vardır. Tabular veri kümeleri, bir veya daha fazla dosyada bulunan sınırlı veri içindir. Dosya veri kümeleri ikili veriler (görüntüler gibi) veya ayrıştıracağınız veriler içindir. Nesneleri oluşturmanın `Dataset` en basit programatik yolu, çalışma alanı depolamasında veya genel URL'lerde varolan lekeleri kullanmaktır:

```python
datastore = Datastore.get(workspace, 'training_data')
iris_dataset = Dataset.Tabular.from_delimited_files(DataPath(datastore, 'iris.csv'))

cats_dogs_dataset = Dataset.File.from_files(
    paths='https://download.microsoft.com/download/3/E/1/3E1C3F21-ECDB-4869-8368-6DEBA77B919F/kagglecatsanddogs_3367a.zip',
    archive_options=ArchiveOptions(archive_type=ArchiveType.ZIP, entry_glob='**/*.jpg')
)
```

Farklı seçeneklerle ve farklı kaynaklardan veri kümeleri oluşturma, bunları kaydetme ve Azure Machine Learning UI'de gözden geçirme, veri boyutunun bilgi işlem kapasitesiyle nasıl etkileşimde olduğunu anlama ve bunları sürüm leme hakkında daha fazla seçenek [için](how-to-create-register-datasets.md)bkz. 

### <a name="pass-datasets-to-your-script"></a>Veri kümelerini komut dosyanıza aktarma

Veri kümesinin komut dosyasına giden yolunu `Dataset` geçmek için `as_named_input()` nesnenin yöntemini kullanın. Ortaya çıkan `DatasetConsumptionConfig` nesneyi bağımsız değişken olarak komut dosyanıza geçirebilir `inputs` veya bağımsız değişkeni boru hattı komut `Run.get_context().input_datasets[]`dosyanıza kullanarak veri kümesini kullanarak alabilirsiniz.

Adlandırılmış bir giriş oluşturduktan sonra, erişim modunu `as_mount()` `as_download()`seçebilirsiniz: veya . Komut dosyanız veri setinizdeki tüm dosyaları işlerse ve bilgi işlem kaynağınızdaki disk veri kümesi için yeterince büyükse, indirme erişim modu daha iyi bir seçimdir. İndirme erişim modu, çalışma zamanında veri akışı yükü nden kaçınır. Komut dosyanız veri kümesinin bir alt kümesine erişimiyorsa veya bilgi işlem için çok büyükse, montaj erişim modunu kullanın. Daha fazla bilgi için [Mount vs. Download'u](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets#mount-vs-download) okuyun

Veri kümesini ardışık aşamaya geçirmek için:

1. Bir `TabularDataset.as_named_inputs()` `DatasetConsumptionConfig` `FileDataset.as_named_input()` nesne oluşturmak için kullanmak veya (sonunda 's' yok)
1. Erişim `as_mount()` `as_download()` modunu kullanmak veya ayarlamak için
1. Veri kümelerini, bağımsız değişkeni `arguments` kullanarak `inputs` veri aktarımlarını ardışık adımlara geçirin

Aşağıdaki parçacık, bu adımları `PythonScriptStep` oluşturucu içinde birleştirme ortak deseni gösterir: 

```python

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[iris_dataset.as_named_inputs('iris').as_mount()]
)
```

Birden çok girdi oluşturmak `random_split()` `take_sample()` veya ardışık hatadımA aktarılan veri miktarını azaltmak gibi yöntemleri de kullanabilirsiniz:

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

### <a name="access-datasets-within-your-script"></a>Komut dosyanızda veri kümelerini erişin

Pipeline adım komut dosyasına adlandırılmış girişler `Run` nesne içinde sözlük olarak kullanılabilir. Etkin `Run` nesneyi `Run.get_context()` kullanarak alın ve ardından adlandırılmış `input_datasets`girişlerin öttresini kullanarak alın. Nesneyi `DatasetConsumptionConfig` bağımsız değişken `arguments` yerine bağımsız değişkeni kullanarak geçtiyseniz, kodu kullanarak `ArgParser` verilere erişin. `inputs` Her iki teknik de aşağıdaki snippet gösterilmiştir.

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

Geçirilen değer, veri kümesi dosyasına giden yol olacaktır.

Doğrudan kayıtlı bir kayda `Dataset` erişmek de mümkündür. Kayıtlı veri kümeleri kalıcı olduğundan ve bir çalışma alanı boyunca paylaşıldığından, bunları doğrudan alabilirsiniz:

```python
run = Run.get_context()
ws = run.experiment.workspace
ds = Dataset.get_by_name(workspace=ws, name='mnist_opendataset')
```

## <a name="use-pipelinedata-for-intermediate-data"></a>Ara `PipelineData` veriler için kullanın

Nesneler `Dataset` kalıcı verileri temsil ederken, [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesneleri, ardışık hat adımlarından çıktı olan geçici veriler için kullanılır. Bir `PipelineData` nesnenin ömrü tek bir ardışık basamaktan daha uzun olduğundan, bunları boru hattı tanımı komut dosyasında tanımlarsınız. Bir `PipelineData` nesne oluşturduğunuzda, bir ad ve verilerin bulunacağı bir veri deposu sağlamanız gerekir. Nesne(ler) `PipelineData` hem `PythonScriptStep` _de_ `arguments` `outputs` bağımsız değişkenleri kullanarak geçirin:

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

Nesnenizi `PipelineData` hemen yükleme sağlayan bir erişim modunu kullanarak oluşturmayı seçebilirsiniz. Bu `PipelineData`durumda, , ", verileri `upload_mode` `"upload"` yazacağınız `output_path_on_compute` yolu belirtmek için bağımsız değişkeni ayarlayın ve kullanın:

```python
PipelineData("clean_data", datastore=def_blob_store, output_mode="upload", output_path_on_compute="clean_data_output/")
```

### <a name="use-pipelinedata-as-outputs-of-a-training-step"></a>Bir `PipelineData` eğitim adımının çıktıları olarak kullanın

Ardışık alan `PythonScriptStep`ınızın içinde, programın bağımsız değişkenlerini kullanarak kullanılabilir çıktı yollarını alabilirsiniz. Bu adım ilkse ve çıktı verilerini açacaksa, belirtilen yolda dizin oluşturmanız gerekir. Daha sonra içinde yer almak istediğiniz dosyaları `PipelineData`yazabilirsiniz.

```python
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()

# Make directory for file
os.makedirs(os.path.dirname(args.output_path), exist_ok=True)
with open(args.output_path, 'w') as f:
    f.write("Step 1's output")
```

`PipelineData` Eğer `is_directory` bağımsız değişken ayarlanmış olsaydı `True`, sadece `os.makedirs()` arama gerçekleştirmek için yeterli olacaktır ve o zaman ne olursa olsun dosyaları yazmak için özgür olacak şekilde istedim. Daha fazla bilgi için [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) başvuru belgelerine bakın.

### <a name="read-pipelinedata-as-inputs-to-non-initial-steps"></a>Başlangıç `PipelineData` olmayan adımlara giriş olarak okuma

İlk ardışık hatlar adımı `PipelineData` yola bazı verileri yazdıktan ve bu ilk adımın çıktısı olduktan sonra, daha sonraki bir adıma giriş olarak kullanılabilir:

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

Bir girdinin `PipelineData` değeri önceki çıktıya giden yoldur. Daha önce gösterildiği gibi, ilk adım tek bir dosya yazdıysa, bu dosyayı tüketen aşağıdaki gibi görünebilir: 

```python
parser = argparse.ArgumentParser()
parser.add_argument('--pd', dest='pd', required=True)
args = parser.parse_args()

with open(args.pd) as f:
    print(f.read())
```

## <a name="convert-pipelinedata-objects-to-datasets"></a>Nesneleri `PipelineData` s'ye `Dataset`dönüştürme

Çalışma süresinden `PipelineData` daha uzun süre kullanılabilir hale getirmek istiyorsanız, `as_dataset()` işlevi kullanarak bir `Dataset`'e dönüştürmek için işlevini kullanın. Daha sonra, `Dataset`çalışma alanınızda birinci sınıf vatandaş yapma kaydolabilirsiniz. Nesneniz, `PipelineData` ardışık hatlar her çalıştığında farklı bir yola sahip `create_new_version` olacağıiçin, bir `Dataset` `PipelineData` nesneden oluşturulmuş bir nesneyi `True` kaydederken ayarlamanız önerilir.

```python
step1_output_ds = step1_output_data.as_dataset()
step1_output_ds.register(name="processed_data", create_new_version=True)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure makine öğrenme veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Azure Machine Learning SDK ile makine öğrenimi boru hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)
