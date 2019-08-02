---
title: Eğitim için veri mağazalarındaki/bloblardaki verilere erişin
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetiyle eğitim sırasında blob veri depolamaya erişmek için veri depolarını nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/24/2019
ms.custom: seodec18
ms.openlocfilehash: 97a4bc20394553b97211763cedaa76c3711306f2
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68319319"
---
# <a name="access-data-from-your-datastores"></a>Veri mağazalarınızın verilerine erişme

 Azure Machine Learning hizmetinde, veri depoları, kaynak kodunuzda değişiklik gerektirmeden depolamaya erişmek için işlem konumlarından bağımsız mekanizmalarda bulunur. Bir yolu parametre olarak almak için eğitim kodu yazdığınızda veya doğrudan bir Estimator 'a veri deposu sağlamanıza bakılmaksızın, Azure Machine Learning iş akışları, veri deposu konumlarınızın erişilebilir olduğundan ve işlem içeriğiniz için kullanılabilir hale getirildiklerinden emin olun.

Bu nasıl yapılır, aşağıdaki görevlerin örneklerini gösterir:
* [Veri deposu seçin](#access)
* [Verileri alma](#get)
* [Verileri veri depolarına yükleme ve indirme](#up-and-down)
* [Eğitim sırasında veri deposuna erişim](#train)

## <a name="prerequisites"></a>Önkoşullar

Veri depoları kullanmak için önce ihtiyacınız bir [çalışma](concept-workspace.md).

Başlangıç ya da tarafından [yeni bir çalışma alanı oluşturma](setup-create-workspace.md#sdk) veya mevcut bir alınıyor:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="choose-a-datastore"></a>Veri deposu seçin

Varsayılan veri deposunu kullanabilir veya kendinizinkini getirebilirsiniz.

### <a name="use-the-default-datastore-in-your-workspace"></a>Çalışma alanınızda varsayılan veri deposunu kullanma

 Her çalışma alanının hemen kullanabileceğiniz kayıtlı, varsayılan bir veri deposu vardır.

Çalışma alanınızın varsayılan veri deposu almak için:

```Python
ds = ws.get_default_datastore()
```

### <a name="register-your-own-datastore-with-the-workspace"></a>Çalışma alanıyla kendi veri deposunu kaydetme

Var olan Azure Depolama'iniz varsa, çalışma alanınızda bir veri deposu olarak kaydedebilirsiniz. 

<a name="store"></a>

####  <a name="storage-guidance"></a>Depolama yönergeleri

BLOB depolama ve blob veri depoları önerilir. Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Daha pahalı olsa da, özellikle büyük bir veri kümesine göre eğiteseniz, eğitimin hızını iyileştirebilecek daha hızlı üretilen iş hızları nedeniyle Premium Depolama önerilir. Depolama hesabı maliyet bilgileri için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) ' na bakın.

>[!NOTE]
> Azure Machine Learning hizmeti, belirli senaryolar için yararlı olabilecek diğer veri deposu türlerini destekler. Örneğin, bir veritabanında depolanan verileri kullanarak eğmeniz gerekiyorsa, Azuressqldatabasedatastore veya AzurePostgreSqlDatastore kullanabilirsiniz. Kullanılabilir veri deposu türleri için [Bu tabloya](#matrix) bakın.

#### <a name="register-your-datastore"></a>Veri deposundan kaydolun
Tüm yazmaç yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıfta bulunur ve register_azure_ * biçiminde olmalıdır.

Aşağıdaki örneklerde bir Azure Blob kapsayıcısını veya Azure dosya paylaşımının bir veri deposu olarak nasıl kaydedileceği gösterilmektedir.

+ **Azure Blob kapsayıcısı veri deposu**için şunu kullanın[`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)

  ```Python
  ds = Datastore.register_azure_blob_container(workspace=ws, 
                                               datastore_name='your datastore name', 
                                               container_name='your azure blob container name',
                                               account_name='your storage account name', 
                                               account_key='your storage account key',
                                               create_if_not_exists=True)
  ```

+ **Azure dosya paylaşımında veri deposu**için kullanın [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). Örneğin: 
  ```Python
  ds = Datastore.register_azure_file_share(workspace=ws, 
                                           datastore_name='your datastore name', 
                                           file_share_name='your file share name',
                                           account_name='your storage account name', 
                                           account_key='your storage account key',
                                           create_if_not_exists=True)
  ```

<a name="get"></a>

## <a name="find--define-datastores"></a>Veri depolarını bulma & tanımlama

Geçerli çalışma alanında kayıtlı olan belirli bir veri deposunu almak için şunu [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) kullanın:

```Python
#get named datastore from current workspace
ds = Datastore.get(ws, datastore_name='your datastore name')
```

Belirli bir çalışma alanındaki tüm veri depolarının listesini almak için şu kodu kullanın:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, ds in datastores.items():
    print(name, ds.datastore_type)
```

Geçerli çalışma alanı için farklı bir varsayılan veri deposu tanımlamak için şunu [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-)kullanın:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Karşıya yükleme & indirme verileri
Aşağıdaki örneklerde [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) açıklanan [](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) [](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) ve yöntemleri, AzureBlobDatastore ve AzureFileDatastore sınıfları için de aynı şekilde çalışır. [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)

### <a name="upload"></a>Karşıya Yükle

 Python SDK'sını kullanarak bir veri deposu için bir dizin veya tek tek dosyaları karşıya yükleyin.

Bir veri deposuna bir dizine yüklenecek `ds`:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

ds.upload(src_dir='your source directory',
          target_path='your target path',
          overwrite=True,
          show_progress=True)
```

`target_path` Dosya Paylaşımı (veya blob kapsayıcısı) karşıya yüklemek için konumu belirtir. Varsayılan `None`, bu durumda veriler karşıya kök. `overwrite=True` mevcut verileri geçersiz kılacaktır `target_path`.

Veya veri deposu aracılığıyla veri deposu'nın tek tek dosyaların listesini karşıya `upload_files()` yöntemi.

### <a name="download"></a>İndirme
Benzer şekilde, verileri bir veri deposundan yerel dosya sisteminize indirin.

```Python
ds.download(target_path='your target path',
            prefix='your prefix',
            show_progress=True)
```

`target_path` Verileri yüklemek için yerel bir dizin konumdur. Dosya Paylaşımı (veya blob kapsayıcısı) indirmek için klasöre bir yol belirtmek için bu yolun sağlamak `prefix`. Varsa `prefix` olduğu `None`, dosya paylaşımı (veya blob kapsayıcısı) tüm içeriği karşıdan.

<a name="train"></a>
## <a name="access-datastores-during-training"></a>Eğitim sırasında veri depolarına erişin

Veri deposundan eğitim işlem hedefinde kullanılabilir hale getirildikten sonra, bu yolu eğitim betiğinizdeki bir parametre olarak geçirerek eğitim çalıştırmaları (örneğin, eğitim veya doğrulama verileri) sırasında erişebilirsiniz.

Aşağıdaki tabloda, işlem hedefine [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) , çalışma sırasında veri deposunu nasıl kullanacağınızı söyleyen Yöntemler listelenmiştir.

Yapmanın|Yöntem|Açıklama|
----|-----|--------
Bağlama| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--)| Veri deposunu işlem hedefine bağlamak için kullanın.
İndirme|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)|Veri deposunun içeriğini tarafından `path_on_compute`belirtilen konuma indirmek için kullanın. <br> Eğitim çalıştırma bağlamı için bu yükleme çalıştırılmadan önce olur.
Karşıya Yükle|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)| Veri deposuna tarafından `path_on_compute` belirtilen konumdan bir dosyayı karşıya yüklemek için kullanın. <br> Eğitim çalıştırma bağlamı için bu karşıya yükleme, çalıştırıldıktan sonra olur.

 ```Python
import azureml.data
from azureml.data.data_reference import DataReference

ds.as_mount()
ds.as_download(path_on_compute='your path on compute')
ds.as_upload(path_on_compute='yourfilename')
```  

Veri deposundaki belirli bir klasöre veya dosyaya başvurmak ve işlem hedefinde kullanılabilir hale getirmek için veri deposunun [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#path-path-none--data-reference-name-none-) işlevini kullanın.

```Python
#download the contents of the `./bar` directory in ds to the compute target
ds.path('./bar').as_download()
```

> [!NOTE]
> Herhangi `ds` bir `ds.path` veya nesnesi, değeri, hedef işlem üzerindeki bağlama/ `"$AZUREML_DATAREFERENCE_XXXX"` indirme yolunu temsil eden biçimdeki bir ortam değişkeni adına çözümlenir. Hedef işlem üzerindeki veri deposu yolu, eğitim betiğinin yürütme yoluyla aynı olmayabilir.

<a name="matrix"></a>
### <a name="training-compute-and-datastore-matrix"></a>Eğitim işlem ve veri deposu matrisi

Aşağıdaki matris, farklı eğitim işlem hedefleri ve veri deposu senaryoları için kullanılabilir veri erişim işlevlerini görüntüler. [Azure Machine Learning için eğitim işlem hedefleri](how-to-set-up-training-targets.md#compute-targets-for-training)hakkında daha fazla bilgi edinin.

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

### <a name="examples"></a>Örnekler 

Aşağıdaki kod örnekleri, eğitim sırasında veri deposuna [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) erişmek için sınıfına özgüdür.

Bu kod, belirtilen eğitim işlem hedefinde ' de `train.py` `script_params`tanımlı parametreleri kullanarak belirtilen kaynak dizinden eğitim betiği kullanarak bir tahmin aracı oluşturur.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': ds.as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Ayrıca, veri depolarınızı bağlamak veya veritabanından kopyalamak için bir veri depoları listesini Estimator Oluşturucu `inputs` parametresine geçirebilirsiniz. Bu kod örneği:
* Eğitim betiğinizin `train.py` çalıştırılabilmesi için `ds1` veri deposundaki tüm içerikleri işlem hedefine indirir
* Çalıştırmadan önce `'./foo'` `ds2` işlemhedefine`train.py` veri deposundaki klasörü indirir
* Komut dosyanız çalıştırıldıktan `'./bar.pkl'` sonra işlem hedefinden dosyayı veri deposuna yükler `ds3`

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[ds1.as_download(), ds2.path('./foo').as_download(), ds3.as_upload(path_on_compute='./bar.pkl')])
```

## <a name="access-datastores-during-for-scoring"></a>Puanlama için veri depolarına erişin

Azure Machine Learning hizmeti, Puanlama için modellerinizi kullanmanın birkaç yolunu sağlar. Bu yöntemlerin bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına erişmenize izin veren yöntemleri anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Veri deposu erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](how-to-run-batch-predictions.md) | ✔ | Büyük miktarlarda verileri zaman uyumsuz olarak tahmin edin. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri Web hizmeti olarak dağıtın. |
| [IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge cihazlara dağıtın. |

SDK 'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK 'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, bloblarda depolanan verilere erişmek için [Python Için Azure depolama SDK 'sını](https://github.com/Azure/azure-storage-python) kullanma.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir model eğitip](how-to-train-ml-models.md)

* [Model dağıtma](how-to-deploy-and-where.md)
