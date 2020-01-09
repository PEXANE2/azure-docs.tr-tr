---
title: Azure Storage hizmetlerindeki verilere erişme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile eğitim sırasında Azure depolama hizmetlerine güvenli bir şekilde bağlanmak için veri depolarını nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ylxiong
author: YLXiong1125
ms.reviewer: nibaccam
ms.date: 12/10/2019
ms.custom: seodec18
ms.openlocfilehash: ac6ef6341013ca13d5a9f27be8897365c1c2155d
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75540951"
---
# <a name="access-data-in-azure-storage-services"></a>Azure Storage hizmetlerindeki verilere erişme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Storage hizmetlerindeki verilerinize Azure Machine Learning veri depoları aracılığıyla kolayca nasıl erişebileceğinizi öğrenin. Veri depoları, abonelik KIMLIĞINIZ ve belirteç yetkilendirmesi gibi bağlantı bilgilerini depolamak için kullanılır. Veri depolarını kullandığınızda, betiklerinizde, bağlantı bilgilerini sabit koda eklemek zorunda kalmadan, depolama verilerinize erişebilirsiniz. 

[Bu Azure depolama çözümlerinden](#matrix)veri depoları oluşturabilirsiniz. Desteklenmeyen depolama çözümleri için ve Machine Learning denemeleri sırasında veri çıkış maliyetini kaydetmek için, verilerinizi desteklenen Azure depolama çözümlerine [taşımanızı](#move) öneririz. 

## <a name="prerequisites"></a>Ön koşullar
Gerekenler:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- Azure [BLOB kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) veya [Azure dosya paylaşımıyla](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)Azure depolama hesabı.

- [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da Python SDK 'sı aracılığıyla mevcut bir tane kullanın:

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Bir Azure depolama çözümünü bir veri deposu olarak kaydettiğinizde, bu veri deposunu otomatik olarak belirli bir çalışma alanında oluşturursunuz. Python SDK veya Azure Machine Learning Studio kullanarak bir çalışma alanına veri depoları oluşturabilir ve kaydedebilirsiniz.

### <a name="python-sdk"></a>Python SDK

Tüm yazmaç yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıftır ve formun `register_azure_*`vardır.

`register()` yöntemi [Azure Portal](https://portal.azure.com)kullanarak doldurmanız için ihtiyacınız olan bilgileri bulabilirsiniz:

1. Sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. 
2. Hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler için **genel bakış** sayfasına gidin. Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama bilgileri için, **Ayarlar** bölmesinde **erişim tuşları** ' na gidin. 

> [!IMPORTANT]
> Depolama hesabınız bir sanal ağda ise, yalnızca bir Azure blob veri deposu oluşturulması desteklenir. Çalışma alanınızın depolama hesabınıza erişimine izin vermek için `grant_workspace_access` parametresi `True`olarak ayarlayın.

Aşağıdaki örneklerde bir Azure Blob kapsayıcısının, Azure dosya paylaşımının ve Azure SQL verilerinin bir veri deposu olarak nasıl kaydedileceği gösterilmektedir.

#### <a name="blob-container"></a>Blob kapsayıcı

Bir Azure Blob kapsayıcısını bir veri deposu olarak kaydetmek için [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)kullanın.

Aşağıdaki kod, `blob_datastore_name` veri deposunu oluşturup `ws` çalışma alanına kaydeder. Bu veri deposu, belirtilen hesap anahtarını kullanarak `my-account-name` depolama hesabındaki `my-container-name` blob kapsayıcısına erişir.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

#### <a name="file-share"></a>Dosya paylaşımı

Azure dosya paylaşımının kaydını bir veri deposu olarak kaydetmek için [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)kullanın. 

Aşağıdaki kod, `file_datastore_name` veri deposunu oluşturup `ws` çalışma alanına kaydeder. Bu veri deposu, belirtilen hesap anahtarını kullanarak `my-account-name` depolama hesabındaki `my-fileshare-name` dosya paylaşımına erişir.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                 datastore_name=file_datastore_name, 
                                                 file_share_name=file_share_name, 
                                                 account_name=account_name,
                                                 account_key=account_key)
```

#### <a name="sql-data"></a>SQL verileri

Azure SQL veri deposu için, SQL kimlik doğrulaması veya hizmet sorumlusu izinleriyle bir Azure SQL veritabanına bağlı bir kimlik bilgisi veri deposunu kaydetmek için [register_azure_sql_database ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-sql-database-workspace--datastore-name--server-name--database-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--endpoint-none--overwrite-false--username-none--password-none-) kullanın. 

SQL kimlik doğrulaması üzerinden kaydolmak için:

```python
sql_datastore_name="azsqlsdksql"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the Azure SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the Azure SQL database
username=os.getenv("SQL_USER_NAME", "<my-sql-user-name>") # Username of the database user to access the database
password=os.getenv("SQL_USER_PASSWORD", "<my-sql-user-password>") # Password of the database user to access the database

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                  datastore_name=sql_datastore_name,
                                                  server_name=server_name,
                                                  database_name=database_name,
                                                  username=username,
                                                  password=password)

```

Hizmet sorumlusu üzerinden kaydolmak için:

```python 
sql_datastore_name="azsqlsdksp"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the SQL database
client_id=os.getenv("SQL_CLIENTNAME", "<my-client-id>") # Client ID of the service principal with permissions to access the database
client_secret=os.getenv("SQL_CLIENTSECRET", "<my-client-secret>") # Secret of the service principal
tenant_id=os.getenv("SQL_TENANTID", "<my-tenant-id>") # Tenant ID of the service principal

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                      datastore_name=sql_datastore_name,
                                                      server_name=server_name,
                                                      database_name=database_name,
                                                      client_id=client_id,
                                                      client_secret=client_secret,
                                                      tenant_id=tenant_id)
```

#### <a name="storage-guidance"></a>Depolama yönergeleri

Azure Blob kapsayıcısı önerilir. Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Premium Depolama daha pahalı olsa da, daha hızlı üretilen iş hızları, özellikle büyük bir veri kümesiyle eğeceğiniz durumlarda eğitim çalışmalarınızın hızını iyileştirebilir. Depolama hesaplarının maliyeti hakkında daha fazla bilgi için bkz. [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio 

Azure Machine Learning Studio 'da birkaç adımda yeni bir veri deposu oluşturun:

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da oturum açın.
1. **Yönet**altında sol bölmedeki **veri depoları** ' nı seçin.
1. **+ Yeni veri deposu**seçin.
1. Yeni bir veri deposu için formu doldurun. Form, Azure depolama türü ve kimlik doğrulama türü seçimlerinize göre kendisini akıllıca güncelleştirir.
  
Formu [Azure Portal](https://portal.azure.com)doldurmanız için gereken bilgileri bulabilirsiniz. Sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. **Genel bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler sağlar. Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama öğeleri için, **Ayarlar** bölmesinde **Hesap anahtarları** ' na gidin.

Aşağıdaki örnek, bir Azure blob veri deposu oluştururken formun nasıl göründüğünü göstermektedir: 
    
![Yeni veri deposu için form](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Çalışma alanınızdan veri depoları alın

Geçerli çalışma alanında kayıtlı belirli bir veri deposunu almak için `Datastore` sınıfında [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) static metodunu kullanın:

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Belirli bir çalışma alanıyla kaydedilen veri depolarının listesini almak için, bir çalışma alanı nesnesi üzerinde [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) özelliğini kullanabilirsiniz:

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Bir çalışma alanı oluşturduğunuzda, bir Azure Blob kapsayıcısı ve bir Azure dosya paylaşımında otomatik olarak çalışma alanı kaydedilir. Bunlar sırasıyla `workspaceblobstore` ve `workspacefilestore`olarak adlandırılır. Blob kapsayıcısı ve çalışma alanına bağlı depolama hesabında sağlanan dosya paylaşımının bağlantı bilgilerini depolar. `workspaceblobstore` kapsayıcısı, varsayılan veri deposu olarak ayarlanır.

Çalışma alanının varsayılan veri deposunu almak için şu satırı kullanın:

```Python
datastore = ws.get_default_datastore()
```

Geçerli çalışma alanı için farklı bir varsayılan veri deposu tanımlamak üzere çalışma alanı nesnesi üzerinde [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) yöntemini kullanın:

```Python
# Define the default datastore for the current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>Karşıya yükleme ve verileri indirme

Aşağıdaki örneklerde açıklanan [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) ve [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) yöntemleri, [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) ve [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) sınıfları için de aynıdır ve aynı şekilde çalışır.

### <a name="upload"></a>Karşıya Yükleme

Python SDK kullanarak veri deposuna bir dizin veya tek bir dosya yükleyin:

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

`target_path` parametresi, karşıya yüklenecek dosya paylaşımında (veya blob kapsayıcısında) konumu belirtir. `None`varsayılan olarak, veriler köke yüklenir. `overwrite=True`, `target_path` üzerindeki mevcut verilerin üzerine yazılır.

Tek tek dosyaların listesini `upload_files()` yöntemi aracılığıyla veri deposuna da yükleyebilirsiniz.

### <a name="download"></a>İndirin

Veri deposundaki verileri yerel dosya sisteminize indirin:

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

`target_path` parametresi, verilerin indirileceği yerel dizinin konumudur. Dosya Paylaşımı (veya blob kapsayıcısı) indirmek için klasöre bir yol belirtmek için bu yolun sağlamak `prefix`. `prefix` `None`, dosya paylaşımınızın (veya blob kapsayıcısının) tüm içerikleri indirilir.

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Eğitim sırasında verilerinize erişin

> [!IMPORTANT]
> Artık, eğitiminde verilerinize erişmek için [Azure Machine Learning veri kümeleri](how-to-create-register-datasets.md) kullanmanızı öneririz. Veri kümeleri, Pandas veya Spark DataFrame 'e tablo verileri yükleyen işlevler sağlar. Veri kümeleri ayrıca Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı aracılığıyla herhangi bir biçimde dosya indirme veya bağlama olanağı sağlar. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](how-to-train-with-datasets.md).

Aşağıdaki tabloda, işlem hedefine çalışma sırasında veri depolarını nasıl kullanacağınızı söyleyen Yöntemler listelenmektedir: 

Yapmanın|Yöntem|Açıklama|
----|-----|--------
Bağlama| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Veri deposunu işlem hedefine bağlamak için kullanın. Veri deposu bağlandığında, veri deposundaki tüm dosyalar işlem Hedefinizdeki erişilebilir hale getirilir.
İndirin|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Veri deposunun içeriğini `path_on_compute`tarafından belirtilen konuma indirmek için kullanın. <br><br> Bu indirme, çalıştırmadan önce oluşur.
Karşıya Yükleme|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| `path_on_compute` tarafından belirtilen konumdan bir dosyayı veri deposuna yüklemek için kullanın. <br><br> Bu karşıya yükleme, çalıştırıldıktan sonra olur.

Veri deposundaki belirli bir klasöre veya dosyaya başvurmak ve işlem hedefinde kullanılabilir hale getirmek için, veri deposu [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) yöntemini kullanın:

```Python
# To mount the full contents in your storage to the compute target
datastore.as_mount()

# To download the contents of only the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Belirtilen `datastore` veya `datastore.path` nesnesi, `"$AZUREML_DATAREFERENCE_XXXX"`biçim ortam değişkeni adına çözümlenir. Bu adın değeri, işlem hedefinin bağlama/indirme yolunu temsil eder. İşlem hedefi üzerindeki veri deposu yolu, eğitim betiğinin yürütme yoluyla aynı olmayabilir.

### <a name="examples"></a>Örnekler 

Eğitim sırasında verilere erişmek için [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) sınıfını kullanmanızı öneririz. 

`script_params` değişkeni, `entry_script`parametreleri içeren bir sözlüktür. Bunu kullanarak veri deposuna geçiş yapın ve verilerin işlem hedefi üzerinde nasıl kullanılabilir yapıldığını tanımlayabilirsiniz. [Uçtan uca öğreticiden](tutorial-train-models-with-aml.md)daha fazla bilgi edinin.

```Python
from azureml.train.estimator import Estimator

# Notice that '/' is in front, which indicates the absolute path
script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Ayrıca, veri deposuna/veritabanından veri bağlamak veya veri kopyalamak için `Estimator` oluşturucunun `inputs` parametresine bir veri depoları listesi geçirebilirsiniz. Bu kod örneği:
* `train.py` eğitim betiği çalıştırılmadan önce `datastore1` tüm içeriğini işlem hedefine indirir.
* `train.py` çalıştırılmadan önce `datastore2` `'./foo'` klasörünü işlem hedefine indirir.
* Komut dosyanız çalıştıktan sonra, `'./bar.pkl'` dosyasını işlem hedefinden `datastore3` yükler.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
Eğitim için bir `RunConfig` nesnesi kullanmayı tercih ediyorsanız, bir [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) nesnesi ayarlamanız gerekir. 

Aşağıdaki kod, bir tahmin ardışık düzeninde `DataReference` nesnesiyle nasıl çalışalınacağını gösterir. Tam örnek için [Bu not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-estimatorstep.ipynb)' ne bakın.

```Python
from azureml.core import Datastore
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

def_blob_store = Datastore(ws, "workspaceblobstore")

input_data = DataReference(
       datastore=def_blob_store,
       data_reference_name="input_data",
       path_on_datastore="20newsgroups/20news.pkl")

output = PipelineData("output", datastore=def_blob_store)
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>İşlem ve veri deposu matrisi

Datamağazaların Şu anda aşağıdaki matriste listelenen depolama hizmetlerine bağlantı bilgilerini depolamayı desteklemektedir. Bu matris, farklı işlem hedefleri ve veri deposu senaryoları için kullanılabilir veri erişim işlevlerini görüntüler. [Azure Machine Learning için işlem hedefleri hakkında daha fazla bilgi edinin](how-to-set-up-training-targets.md#compute-targets-for-training).

|Bilgi İşlem|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Yerel|[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|Yok         |Yok                                                                         |
| Azure Machine Learning işlem |[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning işlem hatları](concept-ml-pipelines.md)|[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning işlem hatları](concept-ml-pipelines.md)|Yok         |Yok                                                                         |
| Sanal makineler               |[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |Yok         |Yok                                                                         |
| Azure HDInsight                      |[as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |Yok         |Yok                                                                         |
| Veri aktarımı                  |[Machine Learning işlem hatları](concept-ml-pipelines.md)                                               |Yok                                           |[Machine Learning işlem hatları](concept-ml-pipelines.md)            |[Machine Learning işlem hatları](concept-ml-pipelines.md)                                                                            |
| Azure Databricks                     |[Machine Learning işlem hatları](concept-ml-pipelines.md)                                              |Yok                                           |[Machine Learning işlem hatları](concept-ml-pipelines.md)             |Yok                                                                         |
| Azure Batch                    |[Machine Learning işlem hatları](concept-ml-pipelines.md)                                               |Yok                                           |Yok         |Yok                                                                         |
| Azure Data Lake Analytics       |Yok                                           |Yok                                           |[Machine Learning işlem hatları](concept-ml-pipelines.md)             |Yok                                                                         |

> [!NOTE]
> Son derece yinelemeli, büyük veri işlemlerinin `as_mount()`yerine `as_download()` kullanarak daha hızlı çalıştığı senaryolar olabilir. Bu experimentally doğrulayabilirsiniz.

### <a name="accessing-source-code-during-training"></a>Eğitim sırasında kaynak koduna erişme

Azure Blob depolama, bir Azure dosya paylaşımından daha yüksek işleme hızına sahiptir ve paralel olarak başlatılan çok sayıda işi ölçeklendirecektir. Bu nedenle, çalıştırmaların kaynak kodu dosyalarını aktarmak için blob depolamayı kullanacak şekilde yapılandırılmasını öneririz.

Aşağıdaki kod örneği, kaynak kodu aktarımları için kullanılacak blob veri deposunun hangi çalıştırma yapılandırmasında kullanılacağını belirtir:

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Puanlama sırasında verilere erişin

Azure Machine Learning, modellerinizi Puanlama için kullanmanın birkaç yolunu sağlar. Bu yöntemlerin bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına erişmenize izin veren yöntemleri anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Veri deposu erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](how-to-run-batch-predictions.md) | ✔ | Büyük miktarlarda verileri zaman uyumsuz olarak tahmin edin. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri bir Web hizmeti olarak dağıtın. |
| [Azure IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge cihazlara dağıtın. |

SDK 'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK 'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python) , bloblarda veya dosyalarda depolanan verilere erişmek için kullanabileceğiniz bir istemci kitaplığıdır.

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>Verileri desteklenen Azure depolama çözümlerine taşıma

Azure Machine Learning, Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı 'ndan veri erişimini destekler. Desteklenmeyen depolama kullanıyorsanız, Azure Data Factory kullanarak verilerinizi desteklenen Azure Storage çözümlerine taşımanızı öneririz. Verileri desteklenen depolamaya taşımak, Machine Learning denemeleri sırasında veri çıkış maliyetlerini kaydetmenize yardımcı olabilir. 

Azure Data Factory, ek ücret ödemeden 80 taneden fazla yerleşik bağlayıcı ile verimli ve dayanıklı veri aktarımı sağlar. Bu bağlayıcılar Azure veri Hizmetleri, şirket içi veri kaynakları, Amazon S3 ve Redshift ve Google BigQuery içerir. [Azure Data Factory kullanarak verilerinizi taşımak için adım adım kılavuzu izleyin](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir model eğitip](how-to-train-ml-models.md)
* [Model dağıtma](how-to-deploy-and-where.md)
