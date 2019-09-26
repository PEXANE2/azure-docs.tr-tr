---
title: Azure Storage hizmetlerindeki verilere erişme
titleSuffix: Azure Machine Learning
description: Veri depolarını kullanarak eğitim sırasında Azure depolama hizmetlerine erişme hakkında bilgi edinin Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 08/2/2019
ms.custom: seodec18
ms.openlocfilehash: 9de3232bcd7908f775dadff4dc584f2a687b0c68
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299770"
---
# <a name="access-data-in-azure-storage-services"></a>Azure Storage hizmetlerindeki verilere erişme

Bu makalede, Azure Storage hizmetlerindeki verilerinize Azure Machine Learning veri depoları aracılığıyla kolayca nasıl erişebileceğinizi öğrenin. Veri depoları, abonelik KIMLIĞINIZ ve belirteç yetkilendirmesi gibi bağlantı bilgilerini depolamak için kullanılır. Datamağazaların kullanılması, betiklerinizde bulunan bağlantı bilgilerini sabit bir şekilde kodlamadan depolamaya erişmenizi sağlar.

Bu nasıl yapılır, aşağıdaki görevlerin örneklerini gösterir:
* [Veri depolarını Kaydet](#access)
* [Çalışma alanından veri depoları al](#get)
* [Veri depolarını kullanarak verileri karşıya yükleme ve indirme](#up-and-down)
* [Eğitim sırasında verilere erişin](#train)

## <a name="prerequisites"></a>Önkoşullar

Veri depoları kullanmak için önce ihtiyacınız bir [çalışma](concept-workspace.md).

Başlangıç ya da tarafından [yeni bir çalışma alanı oluşturma](how-to-manage-workspace.md) veya mevcut bir alınıyor:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="register-datastores"></a>Veri depolarını Kaydet

Tüm yazmaç yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıfta bulunur ve register_azure_ * biçiminde olmalıdır.

Aşağıdaki örneklerde bir Azure Blob kapsayıcısını veya Azure dosya paylaşımının bir veri deposu olarak nasıl kaydedileceği gösterilmektedir.

+ **Azure Blob kapsayıcısı veri deposu**için şunu kullanın[`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)

  ```Python
  datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                      datastore_name='your datastore name', 
                                                      container_name='your azure blob container name',
                                                      account_name='your storage account name', 
                                                      account_key='your storage account key',
                                                      create_if_not_exists=True)
  ```

+ **Azure dosya paylaşımında veri deposu**için kullanın [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Örneğin: 
  ```Python
  datastore = Datastore.register_azure_file_share(workspace=ws, 
                                                  datastore_name='your datastore name', 
                                                  file_share_name='your file share name',
                                                  account_name='your storage account name', 
                                                  account_key='your storage account key',
                                                  create_if_not_exists=True)
  ```

####  <a name="storage-guidance"></a>Depolama yönergeleri

Azure Blob kapsayıcısını öneririz. Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Daha pahalı olsa da, özellikle büyük bir veri kümesine göre eğiteseniz, eğitimin hızını iyileştirebilecek daha hızlı üretilen iş hızları nedeniyle Premium Depolama önerilir. Depolama hesabı maliyet bilgileri için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) ' na bakın.

<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Çalışma alanınızdan veri depoları alın

Geçerli çalışma alanında kayıtlı belirli bir veri deposunu almak için, veri deposu [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) sınıfında statik yöntemini kullanın:

```Python
#get named datastore from current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Belirli bir çalışma alanıyla kaydedilen veri depolarının listesini almak için, [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) özelliği bir çalışma alanı nesnesi üzerinde kullanabilirsiniz:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Bir çalışma alanı oluşturduğunuzda, bir Azure Blob kapsayıcısı ve bir Azure dosya paylaşımının `workspaceblobstore` `workspacefilestore` sırasıyla adlı çalışma alanına kaydedilir. Blob kapsayıcısının bağlantı bilgilerini ve çalışma alanına bağlı depolama hesabında sağlanan dosya paylaşımının depolarlar. `workspaceblobstore` Varsayılan veri deposu olarak ayarlanır.

Çalışma alanınızın varsayılan veri deposu almak için:

```Python
datastore = ws.get_default_datastore()
```

Geçerli çalışma alanı için farklı bir varsayılan veri deposu tanımlamak üzere çalışma [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) alanı nesnesi üzerinde yöntemi kullanın:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Karşıya yükleme & indirme verileri
Aşağıdaki örneklerde [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) açıklanan [](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) [](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) ve yöntemleri, AzureBlobDatastore ve AzureFileDatastore sınıfları için de aynı şekilde çalışır. [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-)

### <a name="upload"></a>Karşıya Yükle

 Python SDK'sını kullanarak bir veri deposu için bir dizin veya tek tek dosyaları karşıya yükleyin.

Bir veri deposuna bir dizine yüklenecek `datastore`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

`target_path` Parametresi dosya paylaşımında (veya blob kapsayıcısında) karşıya yüklenecek konumu belirtir. Varsayılan `None`, bu durumda veriler karşıya kök. `overwrite=True` Konumundaki`target_path` mevcut verilerin üzerine yazıldığında.

Veya bağımsız bir dosya listesini, `upload_files()` yöntemi aracılığıyla veri deposuna yükleyin.

### <a name="download"></a>İndirme

Benzer şekilde, verileri bir veri deposundan yerel dosya sisteminize indirin.

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

`target_path` Parametresi, verilerin indirileceği yerel dizinin konumudur. Dosya Paylaşımı (veya blob kapsayıcısı) indirmek için klasöre bir yol belirtmek için bu yolun sağlamak `prefix`. Varsa `prefix` olduğu `None`, dosya paylaşımı (veya blob kapsayıcısı) tüm içeriği karşıdan.

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Eğitim sırasında verilerinize erişin

> [!IMPORTANT]
> [Azure Machine Learning veri kümeleri (Önizleme)](how-to-create-register-datasets.md) kullanmak, eğitiminde verilerinize erişmenin yeni bir yoludur. Veri kümeleri, Pandas veya Spark DataFrame 'e tablo verileri yükleyen ve Azure blob, Azure dosyası, Azure Data Lake Gen 1, Azure Data Lake Gen 2, Azure SQL, Azure PostgreSQL ' den herhangi bir biçimde dosya indirme veya bağlama olanağı sunan işlevler sağlar. [Veri kümeleriyle eğitme](how-to-train-with-datasets.md)hakkında daha fazla bilgi edinin.

Aşağıdaki tabloda, işlem hedefine çalışma sırasında veri depolarını nasıl kullanacağınızı söyleyen Yöntemler listelenmiştir. 

Yapmanın|Yöntem|Açıklama|
----|-----|--------
Bağla| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Veri deposunu işlem hedefine bağlamak için kullanın.
İndirme|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Veri deposunun içeriğini tarafından `path_on_compute`belirtilen konuma indirmek için kullanın. <br><br> Bu indirme, çalıştırmadan önce oluşur.
Karşıya Yükle|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| Veri deposuna tarafından `path_on_compute` belirtilen konumdan bir dosyayı karşıya yüklemek için kullanın. <br><br> Bu karşıya yükleme, çalıştırıldıktan sonra olur.

Veri deposundaki belirli bir klasöre veya dosyaya başvurmak ve işlem hedefinde kullanılabilir hale getirmek için veri deposu [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) yöntemini kullanın.

```Python
#to mount the full contents in your storage to the compute target
datastore.as_mount()

#to download the contents of the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Belirtilen `datastore` `"$AZUREML_DATAREFERENCE_XXXX"`veya `datastore.path` nesne, değeri, hedef işlem üzerindeki bağlama/indirme yolunu temsil eden biçimdeki bir ortam değişkeni adı olarak çözümlenir. Hedef işlem üzerindeki veri deposu yolu, eğitim betiğinin yürütme yoluyla aynı olmayabilir.

### <a name="examples"></a>Örnekler 

Aşağıdaki kod örnekleri, eğitim sırasında verilere erişim [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) sınıfına özgüdür. 

`script_params`, entry_script için parametreler içeren bir sözlüktür. Bunu kullanarak veri deposuna geçiş yapın ve verilerin işlem hedefi üzerinde nasıl kullanılabilir yapıldığını tanımlayabilirsiniz. Uçtan uca [öğreticimizden](tutorial-train-models-with-aml.md)daha fazla bilgi edinin.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Ayrıca veri depolarınızı/veritabanından veri bağlamak veya veri kopyalamak için Estimator Oluşturucu `inputs` parametresine bir veri depoları listesi geçirebilirsiniz. Bu kod örneği:
* Eğitim betiğinizin `train.py` çalıştırılabilmesi `datastore1` için içindeki tüm içeriği işlem hedefine indirir
* Çalıştırmadan önce `'./foo'` `datastore2`,içindeki klasörünü işlem hedefine indirir `train.py`
* Komut dosyası çalıştırıldıktan `'./bar.pkl'` `datastore3` sonra işlem hedefinden dosyayı öğesine yükler

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```

### <a name="compute-and-datastore-matrix"></a>İşlem ve veri deposu matrisi

Datamağazaların Şu anda aşağıdaki matriste listelenen depolama hizmetlerine bağlantı bilgilerini depolamayı desteklemektedir. Bu matris, farklı işlem hedefleri ve veri deposu senaryoları için kullanılabilir veri erişim işlevlerini görüntüler. [Azure Machine Learning için işlem hedefleri](how-to-set-up-training-targets.md#compute-targets-for-training)hakkında daha fazla bilgi edinin.

|İşlem|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Yerel|[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|Yok         |Yok                                                                         |
| Azure Machine Learning işlem |[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ml&nbsp;işlem hatları](concept-ml-pipelines.md)|[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ml&nbsp;işlem hatları](concept-ml-pipelines.md)|Yok         |Yok                                                                         |
| Sanal makineler               |[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |Yok         |Yok                                                                         |
| HDInsight                      |[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |Yok         |Yok                                                                         |
| Veri aktarımı                  |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)                                               |Yok                                           |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)            |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)                                                                            |
| Databricks                     |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)                                              |Yok                                           |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)             |Yok                                                                         |
| Azure Batch                    |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)                                               |Yok                                           |Yok         |Yok                                                                         |
| Azure Data Lake Analytics       |Yok                                           |Yok                                           |[ML&nbsp;işlem hatları](concept-ml-pipelines.md)             |Yok                                                                         |

> [!NOTE]
> Son derece yinelemeli, büyük veri süreçlerinin `as_download()` `as_mount()`yerine kullanarak daha hızlı çalıştığı senaryolar olabilir; bu, experimentally doğrulanabilir.

### <a name="accessing-source-code-during-training"></a>Eğitim sırasında kaynak koduna erişme

Azure Blob depolama, Azure dosya paylaşımından daha yüksek aktarım hızına sahiptir ve paralel olarak başlatılan çok sayıda işi ölçeklendirecektir. Bu nedenle, çalıştırmaların kaynak kodu dosyalarını aktarmak için blob depolamayı kullanacak şekilde yapılandırılmasını öneririz.

Aşağıdaki kod örneği, kaynak kodu aktarımları için kullanılacak blob veri deposuna ait çalıştırma yapılandırması ' nda belirtir.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Puanlama sırasında verilere erişin

Azure Machine Learning, modellerinizi Puanlama için kullanmanın birkaç yolunu sağlar. Bu yöntemlerin bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına erişmenize izin veren yöntemleri anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Veri deposu erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](how-to-run-batch-predictions.md) | ✔ | Büyük miktarlarda verileri zaman uyumsuz olarak tahmin edin. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri Web hizmeti olarak dağıtın. |
| [IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge cihazlara dağıtın. |

SDK 'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK 'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python) , bloblarda veya dosyalarda depolanan verilere erişmek için kullanabileceğiniz bir istemci kitaplığıdır.


## <a name="next-steps"></a>Sonraki adımlar

* [Bir model eğitip](how-to-train-ml-models.md)

* [Model dağıtma](how-to-deploy-and-where.md)
