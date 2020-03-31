---
title: Makine Öğrenme boru hattı YAML
titleSuffix: Azure Machine Learning
description: YAML dosyasını kullanarak makine öğrenimi ardışık hattını nasıl tanımlayın öğrenin. YAML boru hattı tanımları, Azure CLI için makine öğrenimi uzantısı ile birlikte kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: sanpil
author: sanpil
ms.date: 11/11/2019
ms.openlocfilehash: a677aaa891e21f4c9eeda02eebcb94e9d79a55ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368834"
---
# <a name="define-machine-learning-pipelines-in-yaml"></a>YAML'de makine öğrenimi boru hatlarını tanımlayın

[YAML'de](https://yaml.org/)makine öğrenimi boru hatlarınızı nasıl tanımlayabilirsiniz öğrenin. Azure CLI için makine öğrenimi uzantısını kullanırken, ardışık hatlarla ilgili komutların çoğu, ardışık lığı tanımlayan bir YAML dosyası bekler.

Aşağıdaki tablo, YAML'de bir ardışık kaynak tanımlanırken şu anda neyin desteklenmediğini ve desteklenmediğini listeler:

| Adım türü | Destekleniyor mu? |
| ----- | :-----: |
| PythonScriptStep | Evet |
| AdlaStep | Evet |
| AzureBatchStep | Evet |
| DatabricksAdım | Evet |
| Veri Aktarımı Adımı | Evet |
| AutoMLStep | Hayır |
| HyperDriveStep | Hayır |
| ModülAdım | Evet |
| MPIStep | Hayır |
| TahminciAdım | Hayır |

## <a name="pipeline-definition"></a>Boru hattı tanımı

Bir ardışık hatlar tanımı, [Pipelines](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline.pipeline?view=azure-ml-py) sınıfına karşılık gelen aşağıdaki anahtarları kullanır:

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `name` | Boru hattının tanımı. |
| `parameters` | Parametre(ler) boru hattına. |
| `data_reference` | Bir çalıştırmada verilerin nasıl ve nerede kullanılabilir hale getirilmesi gerektiğini tanımlar. |
| `default_compute` | Varsayılan işlem hedefi, ardışık işlem deki tüm adımların çalıştığı yerde. |
| `steps` | Ardışık alanda kullanılan adımlar. |

## <a name="parameters"></a>Parametreler

Bölüm, `parameters` [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?view=azure-ml-py) sınıfına karşılık gelen aşağıdaki tuşları kullanır:

| YAML anahtarı | Açıklama |
| ---- | ---- |
| `type` | Parametrenin değer türü. Geçerli türleri `string` `int`, `float` `bool`, `datapath`, , veya . |
| `default` | Varsayılan değer. |

Her parametre adlandırılmış. Örneğin, aşağıdaki YAML snippet adlı `NumIterationsParameter`üç `DataPathParameter`parametre `NodeCountParameter`tanımlar , , ve:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        NumIterationsParameter:
            type: int
            default: 40
        DataPathParameter:
            type: datapath
            default:
                datastore: workspaceblobstore
                path_on_datastore: sample2.txt
        NodeCountParameter:
            type: int
            default: 4
```

## <a name="data-reference"></a>Veri başvurusu

Bölüm, `data_references` [DataReference'a](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)karşılık gelen aşağıdaki tuşları kullanır:

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `datastore` | Başvuru için veri deposu. |
| `path_on_datastore` | Veri başvurusu için destek depolamasındaki göreli yol. |

Her veri başvurusu bir anahtarda bulunur. Örneğin, aşağıdaki YAML snippet adlı `employee_data`anahtarda depolanan bir veri başvurusu tanımlar:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        employee_data:
            datastore: adftestadla
            path_on_datastore: "adla_sample/sample_input.csv"
```

## <a name="steps"></a>Adımlar

Adımlar, ortam üzerinde çalışacak dosyalarla birlikte bir hesaplama ortamı tanımlar. Adım türünü tanımlamak için `type` aşağıdaki leri kullanın:

| Adım türü | Açıklama |
| ----- | ----- |
| `AdlaStep` | Azure Veri Gölü Analitiği ile bir U-SQL komut dosyası çalıştırın. [AdlaStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.adlastep?view=azure-ml-py) sınıfına karşılık gelir. |
| `AzureBatchStep` | Azure Toplu İş'i kullanarak işleri çalıştırır. [AzureBatchStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.azurebatchstep?view=azure-ml-py) sınıfına karşılık gelir. |
| `DatabricsStep` | Databricks not defteri, Python komut dosyası veya JAR ekler. [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricksstep?view=azure-ml-py) sınıfına karşılık gelir. |
| `DataTransferStep` | Depolama seçenekleri arasında veri aktarır. [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py) sınıfına karşılık gelir. |
| `PythonScriptStep` | Python komut dosyası çalıştırın. [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) sınıfına karşılık gelir. |

### <a name="adla-step"></a>ADLA adımı

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `script_name` | U-SQL komut dosyasının adı `source_directory`(göreli). |
| `compute_target` | Bu adım için kullanılacak Azure Veri Gölü işlem hedefi. |
| `parameters` | Boru [hattına](#parameters) parametreler. |
| `inputs` | Girişler [GirişPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.inputportbinding?view=azure-ml-py), [DataReference](#data-reference), [PortDataReference](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.portdatareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py), [DatasetDefinition](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_definition.datasetdefinition?view=azure-ml-py), veya [PipelineDataset](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedataset?view=azure-ml-py)olabilir. |
| `outputs` | Çıktılar [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) veya [OutputPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.outputportbinding?view=azure-ml-py)olabilir. |
| `source_directory` | Komut dosyası, derlemeler, vb içeren dizin |
| `priority` | Geçerli iş için kullanılacak öncelik değeri. |
| `params` | Ad değeri çiftleri sözlüğü. |
| `degree_of_parallelism` | Bu iş için kullanılacak paralellik derecesi. |
| `runtime_version` | Data Lake Analytics altyapısının çalışma zamanı sürümü. |
| `allow_reuse` | Aynı ayarlarla yeniden çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmayacağını belirler. |

Aşağıdaki örnekbir ADLA Adım tanımı içerir:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        employee_data:
            datastore: adftestadla
            path_on_datastore: "adla_sample/sample_input.csv"
    default_compute: adlacomp
    steps:
        Step1:
            runconfig: "D:\\Yaml\\default_runconfig.yml"
            parameters:
                NUM_ITERATIONS_2:
                    source: PipelineParam1
                NUM_ITERATIONS_1: 7
            type: "AdlaStep"
            name: "MyAdlaStep"
            script_name: "sample_script.usql"
            source_directory: "D:\\scripts\\Adla"
            inputs:
                employee_data:
                    source: employee_data
            outputs:
                OutputData:
                    destination: Output4
                    datastore: adftestadla
                    bind_mode: mount
```

### <a name="azure-batch-step"></a>Azure Toplu İşlem adımı

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `compute_target` | Bu adım için kullanılacak Azure Toplu İşlem hedefi. |
| `inputs` | Girişler [GirişPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.inputportbinding?view=azure-ml-py), [DataReference](#data-reference), [PortDataReference](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.portdatareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py), [DatasetDefinition](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_definition.datasetdefinition?view=azure-ml-py), veya [PipelineDataset](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedataset?view=azure-ml-py)olabilir. |
| `outputs` | Çıktılar [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) veya [OutputPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.outputportbinding?view=azure-ml-py)olabilir. |
| `source_directory` | Modül ikilileri, çalıştırılabilir, derlemeler, vb içeren dizin |
| `executable` | Bu işin bir parçası olarak çalıştırılacak komutun/yürütülebilir in adı. |
| `create_pool` | Boolean bayrağı işi çalıştırmadan önce havuzu oluşturmak için olup olmadığını belirtmek için. |
| `delete_batch_job_after_finish` | Boolean bayrağı, iş bittikten sonra Toplu İşlem hesabından silinip silinmeyeceğini belirtmek için dir. |
| `delete_batch_pool_after_finish` | İş bittikten sonra havuzun silinip silinmeyeceğini belirtmek için Boolean bayrağı. |
| `is_positive_exit_code_failure` | Görev pozitif bir kodla çıkarsa işin başarısız olup olmadığını belirtmek için Boolean bayrağı. |
| `vm_image_urn` | Ise `create_pool` `True`ve VM `VirtualMachineConfiguration`kullanır. |
| `pool_id` | İşin yürütüleceği havuzun kimliği. |
| `allow_reuse` | Aynı ayarlarla yeniden çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmayacağını belirler. |

Aşağıdaki örnekte Azure Toplu Iş adım tanımı içerir:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        input:
            datastore: workspaceblobstore
            path_on_datastore: "input.txt"
    default_compute: testbatch
    steps:
        Step1:
            runconfig: "D:\\Yaml\\default_runconfig.yml"
            parameters:
                NUM_ITERATIONS_2:
                    source: PipelineParam1
                NUM_ITERATIONS_1: 7
            type: "AzureBatchStep"
            name: "MyAzureBatchStep"
            pool_id: "MyPoolName"
            create_pool: true
            executable: "azurebatch.cmd"
            source_directory: "D:\\scripts\\AureBatch"
            allow_reuse: false
            inputs:
                input:
                    source: input
            outputs:
                output:
                    destination: output
                    datastore: workspaceblobstore
```

### <a name="databricks-step"></a>Databricks adımı

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `compute_target` | Bu adım için kullanılacak Azure Databricks bilgi işlem hedefi. |
| `inputs` | Girişler [GirişPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.inputportbinding?view=azure-ml-py), [DataReference](#data-reference), [PortDataReference](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.portdatareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py), [DatasetDefinition](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_definition.datasetdefinition?view=azure-ml-py), veya [PipelineDataset](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedataset?view=azure-ml-py)olabilir. |
| `outputs` | Çıktılar [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) veya [OutputPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.outputportbinding?view=azure-ml-py)olabilir. |
| `run_name` | Bu çalışma için Databricks'teki ad. |
| `source_directory` | Komut dosyası ve diğer dosyaları içeren dizin. |
| `num_workers` | Databricks için statik işçi sayısı kümesi çalıştırın. |
| `runconfig` | Dosyaya giden `.runconfig` yol. Bu [dosya, RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) sınıfının YAML gösterimidir. Bu dosyanın yapısı hakkında daha fazla bilgi için [runconfigschema.json](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json)bölümüne bakın. |
| `allow_reuse` | Aynı ayarlarla yeniden çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmayacağını belirler. |

Aşağıdaki örnekte bir Databricks adımı vardır:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        adls_test_data:
            datastore: adftestadla
            path_on_datastore: "testdata"
        blob_test_data:
            datastore: workspaceblobstore
            path_on_datastore: "dbtest"
    default_compute: mydatabricks
    steps:
        Step1:
            runconfig: "D:\\Yaml\\default_runconfig.yml"
            parameters:
                NUM_ITERATIONS_2:
                    source: PipelineParam1
                NUM_ITERATIONS_1: 7
            type: "DatabricksStep"
            name: "MyDatabrickStep"
            run_name: "DatabricksRun"
            python_script_name: "train-db-local.py"
            source_directory: "D:\\scripts\\Databricks"
            num_workers: 1
            allow_reuse: true
            inputs:
                blob_test_data:
                    source: blob_test_data
            outputs:
                OutputData:
                    destination: Output4
                    datastore: workspaceblobstore
                    bind_mode: mount
```

### <a name="data-transfer-step"></a>Veri aktarım adımı

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `compute_target` | Bu adım için kullanılacak Azure Veri Fabrikası işlem hedefi. |
| `source_data_reference` | Veri aktarım işlemlerinin kaynağı olarak hizmet veren giriş bağlantısı. Desteklenen değerler [InputPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.inputportbinding?view=azure-ml-py), [DataReference](#data-reference), [PortDataReference](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.portdatareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py), [DatasetDefinition](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_definition.datasetdefinition?view=azure-ml-py), veya [PipelineDataset](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedataset?view=azure-ml-py). |
| `destination_data_reference` | Veri aktarım işlemlerinin hedefi olarak hizmet veren giriş bağlantısı. Desteklenen değerler [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) ve [OutputPortBinding'dir.](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.outputportbinding?view=azure-ml-py) |
| `allow_reuse` | Aynı ayarlarla yeniden çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmayacağını belirler. |

Aşağıdaki örnekte bir veri aktarım adımı içerir:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        adls_test_data:
            datastore: adftestadla
            path_on_datastore: "testdata"
        blob_test_data:
            datastore: workspaceblobstore
            path_on_datastore: "testdata"
    default_compute: adftest
    steps:
        Step1:
            runconfig: "D:\\Yaml\\default_runconfig.yml"
            parameters:
                NUM_ITERATIONS_2:
                    source: PipelineParam1
                NUM_ITERATIONS_1: 7
            type: "DataTransferStep"
            name: "MyDataTransferStep"
            adla_compute_name: adftest
            source_data_reference:
                adls_test_data:
                    source: adls_test_data
            destination_data_reference:
                blob_test_data:
                    source: blob_test_data
```

### <a name="python-script-step"></a>Python komut dosyası adımı

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `compute_target` | Bu adım için kullanılacak işlem hedefi. İşlem hedefi bir Azure Machine Learning Compute, Virtual Machine (Data Science VM gibi) veya HDInsight olabilir. |
| `inputs` | Girişler [GirişPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.inputportbinding?view=azure-ml-py), [DataReference](#data-reference), [PortDataReference](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.portdatareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py), [DatasetDefinition](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_definition.datasetdefinition?view=azure-ml-py), veya [PipelineDataset](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedataset?view=azure-ml-py)olabilir. |
| `outputs` | Çıktılar [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) veya [OutputPortBinding](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.outputportbinding?view=azure-ml-py)olabilir. |
| `script_name` | Python komut dosyasının adı `source_directory`(göreli). |
| `source_directory` | Komut dosyası, Conda ortamı, vb içeren dizin |
| `runconfig` | Dosyaya giden `.runconfig` yol. Bu [dosya, RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) sınıfının YAML gösterimidir. Bu dosyanın yapısı hakkında daha fazla bilgi için [runconfig.json](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json)bölümüne bakın. |
| `allow_reuse` | Aynı ayarlarla yeniden çalıştırıldığında adımın önceki sonuçları yeniden kullanıp kullanmayacağını belirler. |

Aşağıdaki örnekbir Python komut dosyası adımı içerir:

```yaml
pipeline:
    name: SamplePipelineFromYaml
    parameters:
        PipelineParam1:
            type: int
            default: 3
    data_references:
        DataReference1:
            datastore: workspaceblobstore
            path_on_datastore: testfolder/sample.txt
    default_compute: cpu-cluster
    steps:
        Step1:
            runconfig: "D:\\Yaml\\default_runconfig.yml"
            parameters:
                NUM_ITERATIONS_2:
                    source: PipelineParam1
                NUM_ITERATIONS_1: 7
            type: "PythonScriptStep"
            name: "MyPythonScriptStep"
            script_name: "train.py"
            allow_reuse: True
            source_directory: "D:\\scripts\\PythonScript"
            inputs:
                InputData:
                    source: DataReference1
            outputs:
                OutputData:
                    destination: Output4
                    datastore: workspaceblobstore
                    bind_mode: mount
```

## <a name="schedules"></a>Zamanlamalar

Bir ardışık düzenin zamanlamasını tanımlarken, bir zaman aralığına bağlı olarak datastore tarafından tetiklenebilir veya yinelenebilir. Bir zamanlama tanımlamak için kullanılan anahtarlar şunlardır:

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `description` | Programın açıklaması. |
| `recurrence` | Zamanlama yineleniyorsa yineleme ayarlarını içerir. |
| `pipeline_parameters` | Boru hattı tarafından gerekli olan parametreler. |
| `wait_for_provisioning` | Tamamlanması için zamanlamanın sağlanması için beklemek olup olmadığı. |
| `wait_timeout` | Zamanlamadan önce bekleyecek saniye sayısı. |
| `datastore_name` | Değiştirilen/eklenen lekeleri izlemek için veri deposu. |
| `polling_interval` | Modifiye/eklenen lekeler için yoklama arasında, dakikalar içinde ne kadar süre. Varsayılan değer: 5 dakika. Yalnızca veri deposu zamanlamaları için desteklenir. |
| `data_path_parameter_name` | Değiştirilen blob yolu ile ayarlanan veri yolu ardışık parametreadı. Yalnızca veri deposu zamanlamaları için desteklenir. |
| `continue_on_step_failure` | Bir adım başarısız olursa, gönderilen PipelineRun'daki diğer adımların yürütülmesine devam edilip edilemeyeceği. Sağlanırsa, ardışık `continue_on_step_failure` hattın ayarını geçersiz kılar.
| `path_on_datastore` | İsteğe bağlı. Değiştirilen/eklenen lekeleri izlemek için veri deposundaki yol. Yol veri deposu için kapsayıcının altındaolduğundan, zamanlamanın izlediği gerçek`path_on_datastore`yol kapsayıcı/ . Yoksa, veri deposu kapsayıcısı izlenir. Bir alt klasörde `path_on_datastore` yapılan eklemeler/değişiklikler izlenmez. Yalnızca veri deposu zamanlamaları için desteklenir. |

Aşağıdaki örnek, veri deposu tarafından tetiklenen zamanlamanın tanımını içerir:

```yaml
Schedule: 
      description: "Test create with datastore" 
      recurrence: ~ 
      pipeline_parameters: {} 
      wait_for_provisioning: True 
      wait_timeout: 3600 
      datastore_name: "workspaceblobstore" 
      polling_interval: 5 
      data_path_parameter_name: "input_data" 
      continue_on_step_failure: None 
      path_on_datastore: "file/path" 
```

Yinelenen bir **zamanlama**tanımlarken aşağıdaki tuşları `recurrence`kullanın:

| YAML anahtarı | Açıklama |
| ----- | ----- |
| `frequency` | Zamanlamane ne sıklıkta zaman sıksık zaman lar. Geçerli değerler `"Minute"` `"Hour"`, `"Day"` `"Week"`, `"Month"`, , veya . |
| `interval` | Program ne sıklıkta ateşediyor. Sonda değeri, zamanlama yeniden çalışana kadar bekleyecek zaman birimlerinin sayısıdır. |
| `start_time` | Zamanlamanın başlangıç saati. Değerin dize biçimi. `YYYY-MM-DDThh:mm:ss` Hiçbir başlangıç zamanı sağlanmazsa, ilk iş yükü anında çalıştırılır ve gelecekteki iş yükleri zamanlamaya göre çalıştırılır. Başlangıç saati geçmişteyse, ilk iş yükü bir sonraki hesaplanan çalışma zamanında çalıştırılır. |
| `time_zone` | Başlangıç saati için saat dilimi. Saat dilimi sağlanmadıysa, UTC kullanılır. |
| `hours` | Varsa `frequency` `"Day"` veya, `"Week"`ardışık işlemin çalışması gereken günün saatleri olarak virgülle ayrılmış 0'dan 23'e kadar bir veya daha fazla tümseci belirtebilirsiniz. Yalnızca `time_of_day` `hours` veya `minutes` ve kullanılabilir. |
| `minutes` | Varsa `frequency` `"Day"` veya, `"Week"`ardışık hattın çalışması gereken saatin dakikası olarak virgülle ayrılmış 0'dan 59'a kadar bir veya daha fazla tümsaağı belirtebilirsiniz. Yalnızca `time_of_day` `hours` veya `minutes` ve kullanılabilir. |
| `time_of_day` | Eğer `frequency` `"Day"` veya `"Week"`, zamanlama çalıştırmak için günün bir saat belirtebilirsiniz. Değerin dize biçimi. `hh:mm` Yalnızca `time_of_day` `hours` veya `minutes` ve kullanılabilir. |
| `week_days` | `frequency` Ise, `"Week"`zamanlamanın ne zaman çalışması gerektiği virgülle ayrılmış bir veya daha fazla gün belirtebilirsiniz. Geçerli değerler `"Monday"` `"Tuesday"`, `"Wednesday"` `"Thursday"`, `"Friday"` `"Saturday"`, `"Sunday"`, , , ve . |

Aşağıdaki örnekte yinelenen bir zamanlama tanımı içerir:

```yaml
Schedule: 
    description: "Test create with recurrence" 
    recurrence: 
        frequency: Week # Can be "Minute", "Hour", "Day", "Week", or "Month". 
        interval: 1 # how often fires 
        start_time: 2019-06-07T10:50:00 
        time_zone: UTC 
        hours: 
        - 1 
        minutes: 
        - 0 
        time_of_day: null 
        week_days: 
        - Friday 
    pipeline_parameters: 
        'a': 1 
    wait_for_provisioning: True 
    wait_timeout: 3600 
    datastore_name: ~ 
    polling_interval: ~ 
    data_path_parameter_name: ~ 
    continue_on_step_failure: None 
    path_on_datastore: ~ 
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning için CLI uzantısını](reference-azure-machine-learning-cli.md)nasıl kullanacağınızı öğrenin.
