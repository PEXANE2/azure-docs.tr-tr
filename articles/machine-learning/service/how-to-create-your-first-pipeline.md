---
title: ML işlem hatlarını oluşturun, çalıştırın, & izleyin
titleSuffix: Azure Machine Learning service
description: Oluşturun ve Python için Azure Machine Learning SDK'sı ile işlem hattı öğrenme bir makine çalıştırın. Makine öğrenimi (ML) aşamalarını birlikte barındıran iş akışlarını oluşturmak ve yönetmek için işlem hatlarını kullanırsınız. Bu aşamalar veri hazırlama, model eğitimi, model dağıtımı ve çıkarım/Puanlama içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 3f0b764b16c1b550c9afa4107449c1b02815e8d1
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668496"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Azure Machine Learning SDK kullanarak makine öğrenimi işlem hattı oluşturma ve çalıştırma

Bu makalede, [Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk)kullanarak [makine öğrenimi ardışık düzeni](concept-ml-pipelines.md) oluşturmayı, yayımlamayı, çalıştırmayı ve izlemeyi öğreneceksiniz.  Bu komut zincirleri oluşturmak ve çeşitli makine öğrenme aşamaları birleştirmek iş akışlarını yönetme yardımcı olur. Veri hazırlama ve modeli eğitimi gibi bir Ardışık düzenin her aşaması, bir veya daha fazla adım ekleyebilirsiniz.

Oluşturduğunuz işlem hattı, Azure Machine Learning hizmeti üyelerine görünür [çalışma](how-to-manage-workspace.md). 

İşlem hatları, hesaplama ve bu işlem hattı çalıştırmasıyla ilişkili Ara ve son veri depolama için uzak işlem hedefleri kullanın. İşlem hatları, desteklenen [Azure depolama](https://docs.microsoft.com/azure/storage/) konumlarına ve bu konumlardan veri okuyup yazabilir.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning hizmetinin ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

## <a name="prerequisites"></a>Önkoşullar

* [Geliştirme ortamınızı yapılandırma](how-to-configure-environment.md) Azure Machine Learning SDK'sını yüklemek için.

* Oluşturma bir [Azure Machine Learning çalışma alanı](how-to-configure-environment.md#workspace) tüm işlem hattı kaynakları tutmak için. 

  ```python
  from azureml.core import Workspace
  
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Machine learning kaynaklarını ayarlama

Bir işlem hattını çalıştırmak için gereken kaynakları oluşturun:

* İşlem hattı adımları gerekli verilere erişmek için kullanılan bir veri deposunu ayarlayın.

* `DataReference` Bir nesne, bir veri deposu içinde bulunan veya ' de erişilebilir olan verileri işaret etmek üzere yapılandırın.

* Ayarlanan [hedefleri işlem](concept-azure-machine-learning-architecture.md#compute-targets) işlem hattı adımlarınızı çalıştırılacağı.

### <a name="set-up-a-datastore"></a>Bir veri deposu ayarlayın
Bir veri deposuna erişmek işlem hattının verileri depolar. Her bir çalışma alanı bir varsayılan veri deposu sahiptir. Ek veri depoları kaydedebilirsiniz. 

Çalışma alanınızı oluşturduğunuzda, [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) ve [Azure Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) alanı çalışma alanına eklenir. Azure Blob depolamaya bağlanmak için varsayılan bir veri deposu kaydedilir. Daha fazla bilgi edinmek için bkz. [Azure dosyaları, Azure Blob 'ları veya Azure diskleri ne zaman kullanılacağına karar verme](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure blob storage)
def_data_store = ws.get_default_datastore()

# The above call is equivalent to this
def_data_store = Datastore(ws, "workspaceblobstore")

# Get file storage associated with the workspace
def_file_store = Datastore(ws, "workspacefilestore")
```

Bunlar işlem hatlarınızı erişilebilir olması için veri deposu, veri dosyaları veya dizinleri yükleyin. Bu örnek, veri deposunun BLOB depolama sürümünü kullanır:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups",
    overwrite=True)
```

Bir işlem hattı, bir veya daha fazla adımdan oluşur. Bir adım, bir işlem hedefte çalıştırma bir birimdir. Adımlar veri kaynaklarını ve "Ara" veri üretir. Bir adım, veri modeli, modeli ve bağımlı dosyaları ile bir dizin veya geçici veri oluşturabilirsiniz. Bu veriler daha sonra ardışık düzende sonraki diğer adımları için kullanılabilir.

### <a name="configure-data-reference"></a>Veri başvurusu yapılandırma

Başvurulan veri kaynağı bir işlem hattı, bir adım bir giriş olarak oluşturduğunuz. Bir veri kaynağında bir işlem hattı tarafından temsil edilen bir [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) nesne. `DataReference` Nesne işaret kendini veya bir veri deposundan erişilebilir veriler.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Ara veriler (veya bir adımın çıkış) tarafından temsil edilen bir [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) nesne. `output_data1`bir adımın çıktısı olarak üretilir ve bir veya daha fazla sonraki adım girişi olarak kullanılır. `PipelineData`adımlar arasında bir veri bağımlılığı sunar ve ardışık düzende bir örtük yürütme sırası oluşturur.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

## <a name="set-up-compute-target"></a>İşlem Hedefi ' ayarlayın

Azure Machine Learning, __işlem__ (veya __işlem hedefi__) terimi, Machine Learning ardışık düzeninde hesaplama adımlarını gerçekleştiren makinelere veya kümelere başvurur.   İşlem hedeflerinin tam listesi için bkz. [model eğitimi için işlem hedefleri](how-to-set-up-training-targets.md) ve bunları oluşturma ve çalışma alanınıza iliştirme.  Bir işlem hedefi oluşturma ve ekleme işlemi, bir modeli eğitmek veya bir ardışık düzen adımını çalıştırmak olmanıza bakılmaksızın aynıdır. İşlem hedefini oluşturup iliştirdikten sonra işlem `ComputeTarget` [hattı adımınızda](#steps)nesnesini kullanın.

> [!IMPORTANT]
> İşlem hedeflerinde yönetim işlemleri gerçekleştirmek uzak işlerin içinden desteklenmez. Makine öğrenimi ardışık düzenleri uzak bir iş olarak gönderildiğinden, işlem hedefleri üzerinde yönetim işlemlerini işlem hattı içinden kullanmayın.

Aşağıdakiler için işlem hedefleri oluşturma ve ekleme örnekleri verilmiştir:

* Azure Machine Learning işlem
* Azure Databricks 
* Azure Data Lake Analytics

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning işlem

Adımlarınızı çalıştırmak için bir Azure Machine Learning işlem oluşturabilirsiniz.

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

### <a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutta Apache Spark tabanlı bir ortam olan. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Databricks çalışma alanı oluşturun. Bu kaynak oluşturmak için bkz [Azure Databricks'te Spark işini çalıştırma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) belge.

Azure Databricks bir işlem hedefi olarak eklemek için aşağıdaki bilgileri sağlayın:

* __Databricks işlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Databricks çalışma alanı adı__: Azure Databricks çalışma alanının adı.
* __Databricks erişim belirteci__: Azure Databricks için kimlik doğrulaması yapmak için kullanılan erişim belirteci. Bir erişim belirteci oluşturmak için bkz: [kimlik doğrulaması](https://docs.azuredatabricks.net/api/latest/authentication.html) belge.

Aşağıdaki kod, Azure Machine Learning SDK ile Azure Databricks işlem hedefi olarak nasıl ekleneceğini gösterir:

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

Daha ayrıntılı bir örnek için GitHub 'daki [örnek bir not defteri](https://aka.ms/pl-databricks) bölümüne bakın.

### <a id="adla"></a>Azure Data Lake Analytics'i

Azure Data Lake Analytics, Azure bulutta büyük veri analiz platformudur. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Data Lake Analytics hesabı oluşturun. Bu kaynak oluşturmak için bkz [Azure Data Lake Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) belge.

Data Lake Analytics işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

* __İşlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Kaynak grubu__: Data Lake Analytics hesabını içeren kaynak grubu.
* __Hesap adı__: Data Lake Analytics hesap adı.

Aşağıdaki kod, Data Lake Analytics işlem hedefi olarak eklemek gösterilmektedir:

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

Daha ayrıntılı bir örnek için GitHub 'daki [örnek bir not defteri](https://aka.ms/pl-adla) bölümüne bakın.

> [!TIP]
> Azure Machine Learning işlem hatlarını, yalnızca varsayılan Data Lake Analytics hesabı veri deposunda depolanan verilerle çalışabilirsiniz. Veri gerekirse çalışma varsayılandan farklı bir depoda, kullanabilirsiniz bir [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) eğitim önce verileri kopyalamak için.

## <a id="steps"></a>İşlem hattı adımlarınızı oluşturun

Çalışma alanınıza bir işlem hedefi oluşturup iliştirdikten sonra bir işlem hattı adımı tanımlamaya hazırsınızdır. Azure Machine Learning SDK üzerinden sunulan birçok yerleşik adım vardır. Bu adımların en temel sürümü, belirtilen bir işlem hedefinde bir Python betiği çalıştıran bir [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py).

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Gereksiz yeniden çalıştırma tekliflerinin çeviklerinin ortadan kaldırılması için, bir işbirliği ortamında işlem hatları kullanılırken önceki sonuçların yeniden kullanılması (`allow_reuse`) anahtarıdır. Bu, bir adımın script_name, girişleri ve parametreleri aynı kaldığında varsayılan davranıştır. Adımın çıkışı yeniden kullanıldığında, iş işleme gönderilmez, bunun yerine önceki çalıştırmanın sonuçları hemen sonraki adımın çalıştırmasında kullanılabilir. False olarak ayarlanırsa işlem hattı yürütme sırasında bu adım için her zaman yeni bir çalıştırma oluşturulacaktır. 

Adımlarınızı tanımladıktan sonra, bu adımların bazılarını veya tümünü kullanarak işlem hattını oluşturursunuz.

> [!NOTE]
> Adımları tanımlarken veya işlem hattını oluştururken Azure Machine Learning hizmetine dosya veya veri yüklenmedi.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

Aşağıdaki örnek, daha önce oluşturulan Azure Databricks işlem hedefini kullanır: 

```python
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

Daha fazla bilgi için bkz. [Azure-işlem hattı-adımlar paketi](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) ve işlem [hattı sınıfı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) başvurusu.

## <a name="submit-the-pipeline"></a>İşlem hattı gönderin

İşlem hattını gönderdiğinizde, Azure Machine Learning hizmeti her adımın bağımlılıklarını denetler ve belirttiğiniz kaynak dizinin bir anlık görüntüsünü yükler. Kaynak dizin belirtilirse, geçerli yerel dizine yüklenir. Anlık görüntü Ayrıca çalışma alanınızdaki denemenin bir parçası olarak depolanır.

> [!IMPORTANT]
> Dosyaların anlık görüntüye eklenmesini engellemek için, dizinde bir [. gitignore](https://git-scm.com/docs/gitignore) veya `.amlignore` dosya oluşturun ve dosyaları bu dosyaya ekleyin. Dosya,. gitignore dosyası ile aynı söz dizimini ve desenleri kullanır. [](https://git-scm.com/docs/gitignore) `.amlignore` Her iki dosya de varsa, `.amlignore` dosya önceliklidir.
>
> Daha fazla bilgi için bkz. [anlık görüntüler](concept-azure-machine-learning-architecture.md#snapshots).

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

Bir işlem hattını ilk kez çalıştırdığınızda Azure Machine Learning:

* Proje anlık görüntüsünü, çalışma alanıyla ilişkili blob depolamadan işlem hedefine indirir.
* İşlem hattının her adımına karşılık gelen bir Docker görüntüsü oluşturur.
* Her adım için Docker görüntüsünü kapsayıcı kayıt defterinden işlem hedefine indirir.
* Bir adımda bir `DataReference` nesne belirtilmişse veri deposunu takar. Bağlama desteklenmiyorsa, veri işlem hedefine bunun yerine kopyalanır.
* Adımı adım tanımında belirtilen işlem hedefi içinde çalıştırır. 
* Adım tarafından belirtilen Günlükler, STDOUT ve STDERR, ölçümler ve çıkış gibi yapıtlar oluşturur. Bu yapıtlar daha sonra karşıya yüklenir ve kullanıcının varsayılan veri deposunda tutulur.

![Bir deneme işlem hattı olarak çalıştırma diyagramı](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

Daha fazla bilgi için bkz. [deneme sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) başvurusu.

## <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Örneğin, deponun geçerli kayıt KIMLIĞI, geçmişin bir parçası olarak günlüğe kaydedilir.

## <a name="publish-a-pipeline"></a>Bir işlem hattı yayımlama

Bir işlem hattı çalıştırma için farklı girişlerle daha sonra yeniden yayımlayabilirsiniz. Önceden yayınlanmış bir işlem hattının REST uç noktası için parametreleri kabul etmek üzere, yayımlamadan önce işlem hattını parametreleştirin. 

1. Bir işlem hattı parametresi oluşturmak için bir [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) varsayılan değeri olan nesne.

   ```python
   pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
   ```

2. Bu ekleme `PipelineParameter` gibi herhangi bir işlem hattı'ndaki adımları bir parametre olarak nesne:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
   ```

3. Çağrıldığında bir parametre kabul eder Bu işlem hattı yayımlayın.

   ```python
   published_pipeline1 = pipeline1.publish(
       name="My_Published_Pipeline", 
       description="My Published Pipeline Description")
   ```

## <a name="run-a-published-pipeline"></a>Yayımlanan bir işlem hattı çalıştırma

Yayımlanan tüm işlem hatları bir REST uç noktasına sahiptir. Bu uç nokta, Python olmayan istemciler gibi dış sistemlerden işlem hattının çalıştırılmasını çağırır. Bu uç nokta, toplu Puanlama ve yeniden eğitim senaryolarında "Managed yinelenebilirlik" öğesini sunar.

Önceki işlem hattının çalıştırılmasını çağırmak için, [Azurecliauthentication sınıfında](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) açıklandığı şekilde bir Azure Active Directory kimlik doğrulama üstbilgisi belirtecine ihtiyacınız vardır veya Azure Machine Learning not defterinde [kimlik doğrulamasında](https://aka.ms/pl-restep-auth) daha fazla bilgi edinebilirsiniz.

```python
response = requests.post(published_pipeline1.endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "My_Pipeline",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="view-results"></a>Sonuçları görüntüleme

Tüm işlem hatlarınızı ve çalıştırma ayrıntıları listesine bakın:
1. [Azure Portal](https://portal.azure.com/) oturum açın.  

1. [Çalışma alanınızı görüntülemek](how-to-manage-workspace.md#view) işlem hatları listesinde bulunamadı.
 ![machine learning işlem hatlarını listesi](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Çalıştırma sonuçları görmek için belirli bir işlem hattını seçin.

## <a name="caching--reuse"></a>Önbelleğe alma & yeniden kullanım  

İşlem hatlarınızın davranışını iyileştirmek ve özelleştirmek için, önbelleğe alma ve yeniden kullanma konusunda birkaç şey yapabilirsiniz. Örneğin, şunları yapabilirsiniz:
+ Adım [tanımı](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)sırasında ayar `allow_reuse=False` **çalıştırma çıkışının varsayılan yeniden kullanımını devre dışı bırakın** . Gereksiz çalıştırmaları ortadan kaldıran çeviklik sunarak, birlikte çalışma sırasında işlem hattı kullanılırken anahtar kullanın. Ancak, bunu devre dışı bırakabilirsiniz.
+ Karma bir yolu veya source_directory ile diğer dosyalara ve dizinlere yönelik göreli yolları dahil etmek için **komut dosyasının ötesine karma genişletme**`hash_paths=['<file or directory']` 
+ İle **çalıştırılan tüm adımlarda çıkış yeniden oluşturmayı zorla**`pipeline_run = exp.submit(pipeline, regenerate_outputs=False)`

Varsayılan olarak, `allow-reuse` adımlar için etkindir ve yalnızca ana betik dosyası karma hale getirilir. Bu nedenle, belirli bir adımın betiği aynı (`script_name`, girişler ve parametreler) olarak kalırsa, önceki bir adım çalıştırmasının çıkışı yeniden kullanılır, iş işleme gönderilmez ve önceki çalıştırmanın sonuçları hemen bir sonraki adımda kullanılabilir .  

```python
step = PythonScriptStep(name="Hello World",
                        script_name="hello_world.py",
                        compute_target=aml_compute,
                        source_directory=source_directory,
                        allow_reuse=False,
                        hash_paths=['hello_world.ipynb'])
```
 

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [github'da bu Jupyter not defterlerini](https://aka.ms/aml-pipeline-readme) machine learning işlem hatlarını daha fazla araştırmak için.
- SDK başvurusu Yardım konularını okuyun [azureml işlem hatları çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) paket ve [azureml işlem hatları adımları](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) paket.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
