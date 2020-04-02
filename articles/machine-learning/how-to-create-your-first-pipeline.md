---
title: ML boru hatları oluşturma, çalıştırma & izleme
titleSuffix: Azure Machine Learning
description: Python için Azure Machine Learning SDK ile bir makine öğrenme boru hattı oluşturun ve çalıştırın. Makine öğrenimi (ML) aşamalarını birbirine diken iş akışlarını oluşturmak ve yönetmek için ML boru hatlarını kullanın. Bu aşamalar veri hazırlama, model eğitimi, model dağıtımı ve çıkarım/puanlamayı içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/05/2019
ms.custom: seodec18
ms.openlocfilehash: fa0a5bfe921687ad964e9321e3874de37ccf9b98
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549308"
---
# <a name="create-and-run-machine-learning-pipelines-with-azure-machine-learning-sdk"></a>Azure Machine Learning SDK ile makine öğrenimi boru hatları oluşturma ve çalıştırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)kullanarak [bir makine öğrenimi ardışık hattını](concept-ml-pipelines.md) nasıl oluşturabileceğinizi, yayımlayacağınızı, çalıştırabileceğinizi ve izleyeceğinizi öğreneceksiniz.  Çeşitli ML aşamalarını birleştiren bir iş akışı oluşturmak için **ML ardışık hatlarını** kullanın ve daha sonra erişmek veya başkalarıyla paylaşmak için bu ardışık ardışık ardışık ardışık hattı Azure Machine Learning çalışma alanınızda yayımlayın.  ML ardışık hatları, çeşitli hesaplamaları kullanarak, adımları yeniden çalıştırmak yerine yeniden kullanmanın yanı sıra ML iş akışlarını başkalarıyla paylaşarak toplu puanlama senaryoları için idealdir.

ML görevlerinin CI/CD otomasyonu için [Azure Pipeline](https://docs.microsoft.com/azure/devops/pipelines/targets/azure-machine-learning?context=azure%2Fmachine-learning%2Fservice%2Fcontext%2Fml-context&view=azure-devops&tabs=yaml) adı verilen farklı bir tür ardışık lık kullanabilirsiniz, ancak bu tür bir boru hattı çalışma alanınızda asla depolanmaz. [Bu farklı boru hatları karşılaştırın.](concept-ml-pipelines.md#which-azure-pipeline-technology-should-i-use)

Veri hazırlama ve model eğitimi gibi bir ML ardışık hattının her aşaması bir veya daha fazla adım içerebilir.

Oluşturduğunuz ML ardışık hatları Azure Machine Learning [çalışma alanınızın](how-to-manage-workspace.md)üyeleri tarafından görülebilir. 

ML boru hatları, hesaplama ve söz le ilişkili ara ve nihai verilerin depolanması için uzaktan bilgi işlem hedeflerini kullanır. Desteklenen [Azure Depolama](https://docs.microsoft.com/azure/storage/) konumlarına veri okuyup yazabilirler.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

## <a name="prerequisites"></a>Ön koşullar

* Tüm boru hattı kaynaklarınızı tutmak için bir [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md) oluşturun.

* [Geliştirme ortamınızı](how-to-configure-environment.md) Azure Machine Learning SDK'yı yükecek şekilde yapılandırın veya SDK yüklü olan [bir Azure Machine Learning bilgi işlem örneğini (önizleme)](concept-compute-instance.md) kullanın.

Çalışma alanınızı ekleyerek başlayın:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```


## <a name="set-up-machine-learning-resources"></a>Makine öğrenimi kaynaklarını ayarlama

ML ardışık bir ardışık hattı çalıştırmak için gereken kaynakları oluşturun:

* Ardışık hatlar adımlarında gereken verilere erişmek için kullanılan bir veri deposu ayarlayın.

* Bir `DataReference` nesneyi, bir veri deposunda yaşayan veya erişilebilen verilere işaret etmek için yapılandırın.

* Boru hattı adımlarınızın çalışacağı [işlem hedeflerini](concept-azure-machine-learning-architecture.md#compute-targets) ayarlayın.

### <a name="set-up-a-datastore"></a>Veri deposu ayarlama

Bir veri deposu, ardışık hattın erişebilmek için verilerini depolar. Her çalışma alanının varsayılan bir veri deposu vardır. Ek veri depolarını kaydedebilirsiniz. 

Çalışma alanınızı oluşturduğunuzda, [Azure Dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [Azure Blob depolama alanı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) çalışma alanına eklenir. Azure Blob depolama alanına bağlanmak için varsayılan bir veri deposu kaydedilir. Daha fazla bilgi edinmek için Azure [Dosyaları, Azure Blob'ları veya Azure Disklerini ne zaman kullanacağınızkonusunda karar](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)verme 'ye bakın. 

```python
# Default datastore 
def_data_store = ws.get_default_datastore()

# Get the blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")

# Get file storage associated with the workspace
def_file_store = Datastore(ws, "workspacefilestore")

```

Veri dosyalarını veya dizinlerini veri ardışık alanlarından erişebilmeleri için veri deposuna yükleyin. Bu örnek, veri deposu olarak Blob depolama kullanır:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups",
    overwrite=True)
```

Bir ardışık hat, bir veya daha fazla adımdan oluşur. Adım, bir işlem hedefi üzerinde çalışan bir birimdir. Adımlar veri kaynaklarını tüketebilir ve "ara" veri üretebilir. Bir adım, model, model ve bağımlı dosyaları olan bir dizin veya geçici veri gibi veriler oluşturabilir. Bu veriler daha sonra ardışık ardışık diğer adımlar için kullanılabilir.

Ardışık hattınızı verilerinize bağlama hakkında daha fazla bilgi edinmek için, [verilere nasıl erişilir](how-to-access-data.md) ve [Veri Kümelerini Nasıl Kaydeder'](how-to-create-register-datasets.md)makalelerine bakın. 

### <a name="configure-data-reference"></a>Veri başvurularını yapılandırma

Yalnızca bir adıma giriş olarak bir ardışık hatlar üzerinde başvurulabilen bir veri kaynağı oluşturdunuz. Bir ardışık ardışık alandaki veri kaynağı, Bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesnesi tarafından temsil edilir. Nesne, `DataReference` bir veri deposunda yaşayan veya erişilebilen verilere işaret ediyor.

```python
from azureml.data.data_reference import DataReference

blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Ara veriler (veya bir adımın çıktısı) bir [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesnesi tarafından temsil edilir. `output_data1`bir adımın çıktısı olarak üretilir ve gelecekteki bir veya daha fazla adımın girişi olarak kullanılır. `PipelineData`adımlar arasında bir veri bağımlılığı sunar ve ardışık yürütme emri oluşturur. Bu nesne daha sonra ardışık işlem adımı oluştururken kullanılacaktır.

```python
from azureml.pipeline.core import PipelineData

output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="configure-data-using-datasets"></a>Veri kümelerini kullanarak verileri yapılandırma

Bir dosyada veya dosya kümesinde depolanan tabular verileriniz varsa, [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) bir `DataReference`. `TabularDataset`nesneler sürüm, diffs ve özet istatistikleri destekler. `TabularDataset`s tembelce değerlendirilir (Python jeneratörleri gibi) ve bölme veya filtreleme tarafından alt kümelemek için verimli. Sınıf, `FileDataset` bir veya daha fazla dosyayı temsil eden benzer lazily değerlendirilmiş verileri sağlar. 

from_delimited_files gibi `TabularDataset` bir [from_delimited_files](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none--support-multi-line-false-)yöntem kullanırsınız.

```python
from azureml.data import TabularDataset

iris_tabular_dataset = Dataset.Tabular.from_delimited_files([(def_blob_store, 'train-dataset/tabular/iris.csv')])
```

 Bir `FileDataset` [from_files](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-)kullanırsınız.

 Veri kümelerini [ekle & kayıt veri kümeleri](how-to-create-register-datasets.md) veya bu örnek not [defterinden](https://aka.ms/train-datasets)veri kümeleriyle çalışma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="set-up-compute-target"></a>İşlem hedefini ayarlama

Azure Machine Learning'de, __hesaplama__ (veya __bilgi işlem hedefi)__ terimi, makine öğrenimi ardışık ardınızdaki hesaplama adımlarını gerçekleştiren makineleri veya kümeleri ifade eder.   İşlem [hedeflerinin](how-to-set-up-training-targets.md) tam listesi için model eğitimi için hesaplama hedeflerine ve bunları nasıl oluşturup çalışma alanınıza ekleyeceklerinize bakın.  Bir işlem hedefi oluşturma ve ekleme işlemi, bir modeli eğitiyor veya bir ardışık işlem adımı çalıştırıyor sanız aynıdır. İşlem hedefini oluşturup iliştirdikten `ComputeTarget` [sonra, ardışık işlem adımındaki](#steps)nesneyi kullanın.

> [!IMPORTANT]
> İşlem hedefleri üzerinde yönetim işlemlerinin gerçekleştirilmesi uzak işlerden desteklenmez. Makine öğrenimi boru hatları uzak bir iş olarak sunulduğundan, boru hattının içinden bilgi işlem hedefleri üzerinde yönetim işlemleri kullanmayın.

Aşağıda, aşağıdakiler için hesaplama hedefleri oluşturma ve ekleme örnekleri verilmiştir:

* Azure Machine Learning İşlemi
* Azure Databricks 
* Azure Data Lake Analytics

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning bilgi işlem

Adımlarınızı çalıştırmak için bir Azure Machine Learning bilgi işlem oluşturabilirsiniz.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

compute_name = "aml-compute"
vm_size = "STANDARD_NC6"
if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size=vm_size,  # STANDARD_NC6 is GPU-enabled
                                                                min_nodes=0,
                                                                max_nodes=4)
    # create the compute target
    compute_target = ComputeTarget.create(
        ws, compute_name, provisioning_config)

    # Can poll for a minimum number of nodes and for a specific timeout.
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20)

    # For a more detailed view of current cluster status, use the 'status' property
    print(compute_target.status.serialize())
```

### <a name="azure-databricks"></a><a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutundaki Apache Spark tabanlı bir ortamdır. Azure Machine Learning ardışık bir işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Databricks çalışma alanı oluşturun. Çalışma alanı kaynağı oluşturmak için [Azure Veri Tuğlaları belgesinde Bir Kıvılcım Çalıştır çalışmasına](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) bakın.

Azure Databricks'i bir bilgi işlem hedefine eklemek için aşağıdaki bilgileri sağlayın:

* __Databricks bilgi işlem adı__: Bu bilgi işlem kaynağına atamak istediğiniz ad.
* __Databricks çalışma alanı adı:__ Azure Databricks çalışma alanının adı.
* __Databricks access token__: Azure Databricks'e kimlik doğrulamak için kullanılan erişim belirteci. Erişim belirteci oluşturmak için [Kimlik Doğrulama](https://docs.azuredatabricks.net/dev-tools/api/latest/authentication.html) belgesine bakın.

Aşağıdaki kod, Azure Machine Learning SDK ile bir bilgi işlem hedefi olarak Azure Databricks'in nasıl eklenebilirliğini gösterir (__Databricks çalışma alanının AML çalışma alanınızla aynı abonelikte bulunması gerekir):__

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get(
    "AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get(
    "AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get(
    "AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get(
    "AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(
        workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group=databricks_resource_group,
                                                           workspace_name=databricks_workspace_name,
                                                           access_token=databricks_access_token)
    databricks_compute = ComputeTarget.attach(
        ws,
        databricks_compute_name,
        attach_config
    )

    databricks_compute.wait_for_completion(True)
```

Daha ayrıntılı bir örnek için GitHub'da örnek bir [not defterine](https://aka.ms/pl-databricks) bakın.

### <a name="azure-data-lake-analytics"></a><a id="adla"></a>Azure Data Lake Analytics

Azure Veri Gölü Analitiği, Azure bulutundaki büyük bir veri analizi platformudur. Azure Machine Learning ardışık bir işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Veri Gölü Analizi hesabı oluşturun. Bu kaynağı oluşturmak için [Azure Veri Gölü Analizi belgesine başla](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) belgesine bakın.

Veri Gölü Analytics'i bilgi işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanmanız ve aşağıdaki bilgileri sağlamanız gerekir:

* __İşlem adı__: Bu bilgi işlem kaynağına atamak istediğiniz ad.
* __Kaynak Grubu__: Veri Gölü Analizi hesabını içeren kaynak grubu.
* __Hesap adı__: Data Lake Analytics hesap adı.

Aşağıdaki kod, Veri Gölü Analizi'nin bir bilgi işlem hedefi olarak nasıl eklenebildiğini gösterir:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get(
    "AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get(
    "AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get(
    "AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group=adla_resource_group,
                                                     account_name=adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
        ws,
        adla_compute_name,
        attach_config
    )

    adla_compute.wait_for_completion(True)
```

Daha ayrıntılı bir örnek için GitHub'da örnek bir [not defterine](https://aka.ms/pl-adla) bakın.

> [!TIP]
> Azure Machine Learning ardışık hatları yalnızca Veri Gölü Analizi hesabının varsayılan veri deposunda depolanan verilerle çalışabilir. Üzerinde çalışmanız gereken veriler varsayılan olmayan bir depodaysa, [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) eğitimden önce verileri kopyalamak için bir veri kullanabilirsiniz.

## <a name="construct-your-pipeline-steps"></a><a id="steps"></a>Boru hattı adımlarınızı oluşturun

Çalışma alanınıza bir işlem hedefi oluşturup iliştirdikten sonra, bir ardışık işlem adımı tanımlamaya hazırsınız. Azure Machine Learning SDK aracılığıyla birçok yerleşik adım mevcuttur. Bu adımların en temeli [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)'dir , belirli bir işlem hedefinde Python komut dosyası çalıştırAn:

```python
from azureml.pipeline.steps import PythonScriptStep

trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", output_data1],
    inputs=[blob_input_data],
    outputs=[output_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Önceki sonuçların yeniden`allow_reuse`kullanımı ( ) gereksiz yeniden çalıştırmaları ortadan kaldırmak çeviklik sunduğundan, ardışık bir ortamda boru hatları kullanılırken anahtardır. Script_name, girişler ve bir adımın parametreleri aynı kaldığında yeniden kullanım varsayılan davranıştır. Adımın çıktısı yeniden kullanıldığında, iş işlem bölümüne gönderilmez, bunun yerine önceki çalıştırmanın sonuçları bir sonraki adımın çalıştırılaması için hemen kullanılabilir. Yanlış `allow_reuse` olarak ayarlanırsa, boru hattı yürütme sırasında bu adım için her zaman yeni bir çalışma oluşturulur. 

Adımlarınızı tanımladıktan sonra, bu adımların bazılarını veya tümlerini kullanarak ardışık yapıyı oluşturursunuz.

> [!NOTE]
> Adımları tanımladığınızda veya ardışık hattı oluştururken Azure Machine Learning'e hiçbir dosya veya veri yüklenmez.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

from azureml.pipeline.core import Pipeline

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

Aşağıdaki örnekte, daha önce oluşturulan Azure Veri Tuğlaları bilgi işlem hedefi kullanılır: 

```python
from azureml.pipeline.steps import DatabricksStep

dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    compute_target=databricks_compute,
    allow_reuse=False
)
# List of steps to run
steps = [dbStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=steps)
```

### <a name="use-a-dataset"></a>Veri kümesi kullanma 

A `TabularDataset` veya `FileDataset` ardışık sisteminizde kullanmak [için, as_named_input(ad)](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#as-named-input-name-)çağırarak bir [DatasetConsumptionConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_consumption_config.datasetconsumptionconfig?view=azure-ml-py) nesnesine dönüştürmeniz gerekir. Bu `DatasetConsumptionConfig` nesneyi boru hattı `inputs` adımınız olarak geçersiniz. 

Azure Blob depolama, Azure Dosyaları, Azure Veri Gölü Depolama Gen1, Azure Veri Gölü Depolama Gen2, Azure SQL Veritabanı ve PostgreSQL için Azure Veritabanı'ndan oluşturulan veri kümeleri, herhangi bir boru hattı adımına giriş olarak kullanılabilir. [Bir DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py) veya [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py)çıktı yazma dışında , çıktı verileri[(PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)) yalnızca Azure Blob ve Azure Dosya payı veri depolarına yazılabilir.

```python
dataset_consuming_step = PythonScriptStep(
    script_name="iris_train.py",
    inputs=[iris_tabular_dataset.as_named_input("iris_data")],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Daha sonra [Çalıştır.input_datasets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#input-datasets) sözlük kullanarak ardışık ardışık veri kümesini alırsınız.

```python
# iris_train.py
from azureml.core import Run, Dataset

run_context = Run.get_context()
iris_dataset = run_context.input_datasets['iris_data']
dataframe = iris_dataset.to_pandas_dataframe()
```

Daha fazla bilgi için [azure-pipeline-steps paketine](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) ve [Pipeline sınıfı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) başvurusuna bakın.

## <a name="submit-the-pipeline"></a>Boru hattını gönderme

Azure Machine Learning, ardışık hattı gönderdiğinde, her adımın bağımlılıklarını denetler ve belirttiğiniz kaynak dizinin anlık görüntüsünü yükler. Kaynak dizini belirtilmemişse, geçerli yerel dizini yüklenir. Anlık görüntü, çalışma alanınızdaki denemenin bir parçası olarak da depolanır.

> [!IMPORTANT]
> Dosyaların anlık görüntüye eklenmesini önlemek için, dizinde `.amlignore` bir [.gitignore](https://git-scm.com/docs/gitignore) veya dosya oluşturun ve dosyaları bu dosyaya ekleyin. Dosya `.amlignore` [,gitignore](https://git-scm.com/docs/gitignore) dosyasıyla aynı sözdizimini ve desenleri kullanır. Her iki dosya `.amlignore` varsa, dosya önceliklidir.
>
> Daha fazla bilgi için [Anlık Görüntüler'e](concept-azure-machine-learning-architecture.md#snapshots)bakın.

```python
from azureml.core import Experiment

# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

Bir ardışık hattı ilk çalıştırdığınızda, Azure Machine Learning:

* Proje anlık görüntüsünü çalışma alanıyla ilişkili Blob depolamasından işlem hedefine indirir.
* Boru hattındaki her adıma karşılık gelen bir Docker görüntüsü oluşturur.
* Konteyner kayıt defterinden işlem hedefine her adım için Docker görüntüsünü indirir.
* Bir nesne bir `DataReference` adımda belirtilmişse veri deposunu bağlar. Montaj desteklenmiyorsa, veriler bunun yerine bilgi işlem hedefine kopyalanır.
* Adım tanımında belirtilen işlem hedefindeki adımı çalıştırın. 
* Günlükler, stdout ve stderr, ölçümler ve adım tarafından belirtilen çıktı gibi yapılar oluşturur. Bu yapılar daha sonra yüklenir ve kullanıcının varsayılan veri deposunda tutulur.

![Bir denemeyi ardışık hat olarak çalıştırma diyagramı](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

Daha fazla bilgi için [Deneme sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) başvurusuna bakın.

### <a name="view-results-of-a-pipeline"></a>Bir ardışık boru hattının sonuçlarını görüntüleme

Stüdyodaki tüm boru hatlarınızın ve çalışma ayrıntılarının listesine bakın:

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. [Çalışma alanınızı görüntüleyin.](how-to-manage-workspace.md#view)

1. Solda, tüm **ardışık** hat lar çalışanlarınızı görmek için Pipelines'ı seçin.
 ![makine öğrenimi boru hatları listesi](./media/how-to-create-your-first-pipeline/pipelines.png)
 
1. Çalıştırma sonuçlarını görmek için belirli bir ardışık hatlar

## <a name="git-tracking-and-integration"></a>Git izleme ve entegrasyon

Kaynak dizininin yerel bir Git deposu olduğu bir eğitim çalışması başlattığınızda, depo hakkındaki bilgiler çalışma geçmişinde depolanır. Daha fazla bilgi [için Azure Machine Learning için Git tümleştirmesi'ne](concept-train-model-git-integration.md)bakın.

## <a name="publish-a-pipeline"></a>Bir ardışık hat lar yayımlama

Daha sonra farklı girişlerle çalıştırmak için bir ardışık hatlar yayımlayabilirsiniz. Parametreleri kabul etmek için zaten yayımlanmış bir ardışık boru hattının REST bitiş noktası için, yayımlamadan önce ardışık etki alanı parametrenize gerekir.

1. Bir ardışık parametre oluşturmak için varsayılan değeri olan bir [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) nesnesi kullanın.

   ```python
   from azureml.pipeline.core.graph import PipelineParameter
   
   pipeline_param = PipelineParameter(
     name="pipeline_arg",
     default_value=10)
   ```

2. Bu `PipelineParameter` nesneyi, ardışık işlem deki adımlardan herhangi biri için parametre olarak aşağıdaki gibi ekleyin:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],
     compute_target=compute_target,
     source_directory=project_folder)
   ```

3. Çağrıldığında bir parametre kabul edecek bu ardışık etki yayımlayın.

   ```python
   published_pipeline1 = pipeline_run1.publish_pipeline(
        name="My_Published_Pipeline",
        description="My Published Pipeline Description",
        version="1.0")
   ```

### <a name="run-a-published-pipeline"></a>Yayımlanmış bir ardışık hatlar

Yayınlanan tüm ardışık hatlar bir REST bitiş noktasına sahiptir. Bu uç nokta, Python olmayan istemciler gibi dış sistemlerden gelen ardışık sistemden boru hattının çalışmasını çağırır. Bu uç nokta, toplu puanlama ve yeniden eğitim senaryolarında "yönetilen tekrarlanabilirlik" sağlar.

Önceki ardışık noktanın çalışmasını çağırmak için [AzureCliAuthentication sınıf](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) referansında açıklandığı gibi bir Azure Active Directory kimlik doğrulama üstbilgisi belirtecine veya Azure Machine Learning not defterinde [Kimlik Doğrulama'da](https://aka.ms/pl-restep-auth) daha fazla ayrıntı almanız gerekir.

```python
from azureml.pipeline.core import PublishedPipeline
import requests

response = requests.post(published_pipeline1.endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "My_Pipeline",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="create-a-versioned-pipeline-endpoint"></a>Sürümlenmiş ardışık hat sonu noktası oluşturma
Arkasında birden çok yayımlanmış ardışık ardışık nokta içeren bir Pipeline Endpoint oluşturabilirsiniz. Bu, yayımlanmış bir ardışık işlem hattı gibi kullanılabilir, ancak ml ardışık hatlar üzerinde tesbit ve güncelleme olarak sabit bir REST bitiş noktası verir.

```python
from azureml.pipeline.core import PipelineEndpoint

published_pipeline = PublishedPipeline.get(workspace="ws", name="My_Published_Pipeline")
pipeline_endpoint = PipelineEndpoint.publish(workspace=ws, name="PipelineEndpointTest",
                                            pipeline=published_pipeline, description="Test description Notebook")
```

### <a name="submit-a-job-to-a-pipeline-endpoint"></a>İşi bir ardışık nokta bitiş noktasına gönderme
Bir işi bir ardışık nokta bitiş noktasının varsayılan sürümüne gönderebilirsiniz:
```python
pipeline_endpoint_by_name = PipelineEndpoint.get(workspace=ws, name="PipelineEndpointTest")
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment")
print(run_id)
```
Ayrıca, belirli bir sürüme de iş gönderebilirsiniz:
```python
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment", pipeline_version="0")
print(run_id)
```

Aynı REST API kullanılarak gerçekleştirilebilir:
```python
rest_endpoint = pipeline_endpoint_by_name.endpoint
response = requests.post(rest_endpoint, 
                         headers=aad_token, 
                         json={"ExperimentName": "PipelineEndpointExperiment",
                               "RunSource": "API",
                               "ParameterAssignments": {"1": "united", "2":"city"}})
```

### <a name="use-published-pipelines-in-the-studio"></a>Stüdyoda yayınlanmış boru hatlarını kullanma

Ayrıca stüdyodan yayımlanmış bir ardışık hat lar çalıştırabilirsiniz:

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. [Çalışma alanınızı görüntüleyin.](how-to-manage-workspace.md#view)

1. Solda, Uç **Noktaları'nı**seçin.

1. Üstte, **Pipeline uç noktalarını**seçin.
 ![makine öğrenimi listesi yayınlanan boru hatları](./media/how-to-create-your-first-pipeline/pipeline-endpoints.png)

1. Ardışık hatlar bitiş noktasının önceki çalıştırmalarının sonuçlarını çalıştırmak, tüketmek veya gözden geçirmek için belirli bir ardışık nokta seçin.


### <a name="disable-a-published-pipeline"></a>Yayımlanmış bir ardışık boru hattını devre dışı

Bir ardışık ardışık hattı yayımlanmış ardışık lık listenizden gizlemek için, bu boru hattını stüdyoda veya SDK'dan devre dışı kağınızda:

```
# Get the pipeline by using its ID from Azure Machine Learning studio
p = PublishedPipeline.get(ws, id="068f4885-7088-424b-8ce2-eeb9ba5381a6")
p.disable()
```

'ile `p.enable()`yeniden etkinleştirebilirsiniz. Daha fazla bilgi için Bkz. [PublishedPipeline sınıfı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.publishedpipeline?view=azure-ml-py) başvurusu.


## <a name="caching--reuse"></a>Önbelleğe alma & yeniden  

Ardışık hatlarınızın davranışını optimize etmek ve özelleştirmek için önbelleğe alma ve yeniden kullanma yla ilgili birkaç şey yapabilirsiniz. Örneğin, şunları seçebilirsiniz:
+ [Adım tanımı](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)sırasında ayarlayarak `allow_reuse=False` **adım çalıştırma çıktısının varsayılan yeniden kullanımını kapatın.** Gereksiz çalıştırmaları ortadan kaldırmak çeviklik sunduğundan, boru hatlarını işbirlikçi bir ortamda kullanırken yeniden kullanım önemlidir. Ancak, yeniden kullanımı devre dışı bırakmak olabilir.
+ Bir **çalışmadaki tüm adımlar için çıkış rejenerasyonuna güç**`pipeline_run = exp.submit(pipeline, regenerate_outputs=False)`

Varsayılan olarak, `allow_reuse` adımlar için `source_directory` etkinleştirilir ve adım tanımında belirtilen ler işlenir. Bu nedenle, belirli bir adımın komut`script_name`dosyası aynı kalırsa (, girişler ve` source_directory` parametreler) ve başka hiçbir şey değişmediyse, önceki bir adım çalışmasının çıktısı yeniden kullanılır, iş işlem için gönderilmez ve önceki çalıştırmanın sonuçları hemen bir sonraki adıma kullanılabilir.

```python
step = PythonScriptStep(name="Hello World",
                        script_name="hello_world.py",
                        compute_target=aml_compute,
                        source_directory=source_directory,
                        allow_reuse=False,
                        hash_paths=['hello_world.ipynb'])
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla makine öğrenme boru hatları keşfetmek için [GitHub bu Jupyter dizüstü kullanın.](https://aka.ms/aml-pipeline-readme)
- [Azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paketi ve [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) paketi için SDK başvuru yardımına bakın.
- Hata ayıklama ve sorun giderme boru hatları yla ilgili ipuçları için [nasıl yapılacağını](how-to-debug-pipelines.md) görün.

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]
