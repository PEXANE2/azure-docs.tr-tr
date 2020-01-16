---
title: Azure Storage hizmetlerindeki verilere erişme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile eğitim sırasında Azure depolama hizmetlerine güvenli bir şekilde bağlanmak için veri depolarını nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 01/15/2020
ms.custom: seodec18
ms.openlocfilehash: f8bad8be3c02228fee9dded729e22c5a406da178
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76025047"
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
<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>Desteklenen veri depolama hizmeti türleri

Datamağazaların Şu anda aşağıdaki matriste listelenen depolama hizmetlerine bağlantı bilgilerini depolamayı desteklemektedir.

| Depolama&nbsp;türü | Kimlik doğrulama&nbsp;türü | [Azure&nbsp;Machine&nbsp;öğrenme Studio](https://ml.azure.com/) | [Azure&nbsp;Machine&nbsp;öğrenme&nbsp; Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) |  [Azure&nbsp;Machine&nbsp;öğrenme CLı](reference-azure-machine-learning-cli.md) | [Azure&nbsp;Machine&nbsp;öğrenme&nbsp; REST API 'SI](https://docs.microsoft.com/rest/api/azureml/)
---|---|---|---|---|---
[Azure&nbsp;blob&nbsp;depolaması](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;dosya&nbsp;paylaşma]((https://docs.microsoft.com/azure/storage/files/storage-files-introduction))| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;depolama gen&nbsp;1](https://docs.microsoft.com/azure/data-lake-store/)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;depolama gen&nbsp;2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
Azure&nbsp;SQL&nbsp;veritabanı| SQL kimlik doğrulaması <br>Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
Azure&nbsp;PostgreSQL | SQL kimlik doğrulaması| ✓ | ✓ | ✓ |✓
&nbsp;MySQL için Azure&nbsp;veritabanı&nbsp; | SQL kimlik doğrulaması|  | ✓ | ✓ |✓
Databricks&nbsp;dosya&nbsp;sistemi| Kimlik doğrulaması yok | | ✓* | ✓ * |✓* 

\* yalnızca yerel işlem hedefi senaryolarında desteklenir

### <a name="storage-guidance"></a>Depolama yönergeleri

Azure Blob kapsayıcısı için bir veri deposu oluşturmanızı öneririz.  
Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Premium Depolama daha pahalı olsa da, daha hızlı üretilen iş hızları, özellikle büyük bir veri kümesiyle eğeceğiniz durumlarda eğitim çalışmalarınızın hızını iyileştirebilir. Depolama hesaplarının maliyeti hakkında daha fazla bilgi için bkz. [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

Bir çalışma alanı oluşturduğunuzda, bir Azure Blob kapsayıcısı ve bir Azure dosya paylaşımında otomatik olarak çalışma alanı kaydedilir. Bunlar sırasıyla `workspaceblobstore` ve `workspacefilestore`olarak adlandırılır. Blob kapsayıcısı ve çalışma alanına bağlı depolama hesabında sağlanan dosya paylaşımının bağlantı bilgilerini depolar. `workspaceblobstore` kapsayıcısı, varsayılan veri deposu olarak ayarlanır.

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Bir Azure depolama çözümünü bir veri deposu olarak kaydettiğinizde, bu veri deposunu otomatik olarak oluşturur ve belirli bir çalışma alanına kaydedersiniz. Python SDK veya Azure Machine Learning Studio kullanarak bir çalışma alanına veri depoları oluşturabilir ve kaydedebilirsiniz.

>[!IMPORTANT]
> Geçerli veri deposu oluşturma ve kaydetme işleminin bir parçası olarak Azure Machine Learning, kullanıcının (Kullanıcı adı, hizmet sorumlusu veya SAS belirteci) temel alınan depolama hizmetine erişimi olduğunu doğrular. 
<br>
Ancak, Azure Data Lake Storage Gen 1 ve 2 veri depoları için, bu doğrulama daha sonra [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py) veya [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-parquet-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) gibi veri erişim yöntemleri çağrıldığında oluşur. 

### <a name="python-sdk"></a>Python SDK

Tüm yazmaç yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıftır ve formun `register_azure_*`vardır.

`register()` yöntemi [Azure Portal](https://portal.azure.com)kullanarak doldurmanız için ihtiyacınız olan bilgileri bulabilirsiniz:

1. Sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. 
2. Hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler için **genel bakış** sayfasına gidin. Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama bilgileri için, **Ayarlar** bölmesinde **erişim tuşları** ' na gidin. 

> [!IMPORTANT]
> Depolama hesabınız bir sanal ağda ise, yalnızca bir Azure blob veri deposu oluşturulması desteklenir. Çalışma alanınızın depolama hesabınıza erişimine izin vermek için `grant_workspace_access` parametresi `True`olarak ayarlayın.

Aşağıdaki örneklerde bir Azure Blob kapsayıcısının, Azure dosya paylaşımının ve Azure Data Lake Storage nesil 2 ' nin bir veri deposu olarak nasıl kaydedileceği gösterilmektedir. Diğer depolama hizmetleri için lütfen [`register_azure_*` yöntemler için başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#methods)bakın.

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

#### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage 2. nesil

Bir Azure Data Lake Storage 2. nesil (ADLS Gen 2) veri deposu için, hizmet sorumlusu izinleriyle bir Azure Data Lake Gen 2 depolamasına bağlı bir kimlik bilgisi veri deposunu kaydetmek için [register_azure_data_lake_gen2 ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) kullanın. [ADLS Gen 2 için erişim denetimi ayarlama](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)hakkında daha fazla bilgi edinin. 

Aşağıdaki kod, `adlsgen2_datastore_name` veri deposunu oluşturup `ws` çalışma alanına kaydeder. Bu veri deposu, belirtilen hizmet sorumlusu kimlik bilgilerini kullanarak `account_name` depolama hesabındaki dosya sistemine `test` erişir.

```python 
adlsgen2_datastore_name = 'adlsgen2datastore'

subscription_id=os.getenv("ADL_SUBSCRIPTION", "<my_subscription_id>") # subscription id of ADLS account
resource_group=os.getenv("ADL_RESOURCE_GROUP", "<my_resource_group>") # resource group of ADLS account

account_name=os.getenv("ADLSGEN2_ACCOUNTNAME", "<my_account_name>") # ADLS Gen2 account name
tenant_id=os.getenv("ADLSGEN2_TENANT", "<my_tenant_id>") # tenant id of service principal
client_id=os.getenv("ADLSGEN2_CLIENTID", "<my_client_id>") # client id of service principal
client_secret=os.getenv("ADLSGEN2_CLIENT_SECRET", "<my_client_secret>") # the secret of service principal

adlsgen2_datastore = Datastore.register_azure_data_lake_gen2(workspace=ws,
                                                             datastore_name=adlsgen2_datastore_name,
                                                             account_name=account_name, # ADLS Gen2 account name
                                                             filesystem='test', # ADLS Gen2 filesystem
                                                             tenant_id=tenant_id, # tenant id of service principal
                                                             client_id=client_id, # client id of service principal
                                                             client_secret=client_secret) # the secret of service principal
```

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

Veri mağazalarınızla verilerle etkileşim kurmak veya verilerinizi makine öğrenimi görevleri için eğitim gibi tüketilebilir bir nesneye paketlemek için [Azure Machine Learning bir veri kümesi oluşturun](how-to-create-register-datasets.md). Veri kümeleri, Pandas veya Spark DataFrame 'e tablo verileri yükleyen işlevler sağlar. Veri kümeleri ayrıca Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı aracılığıyla herhangi bir biçimde dosya indirme veya bağlama olanağı sağlar. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](how-to-train-with-datasets.md).

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

Azure Machine Learning, Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı 'ndan veri erişimini destekler. Desteklenmeyen depolama kullanıyorsanız, [Azure Data Factory ve bu adımları]((https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool))kullanarak verilerinizi desteklenen Azure depolama çözümlerine taşımanızı öneririz. Verileri desteklenen depolamaya taşımak, Machine Learning denemeleri sırasında veri çıkış maliyetlerini kaydetmenize yardımcı olabilir. 

Azure Data Factory, ek ücret ödemeden 80 taneden fazla yerleşik bağlayıcı ile verimli ve dayanıklı veri aktarımı sağlar. Bu bağlayıcılar Azure veri Hizmetleri, şirket içi veri kaynakları, Amazon S3 ve Redshift ve Google BigQuery içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Bir model eğitip](how-to-train-ml-models.md)
* [Model dağıtma](how-to-deploy-and-where.md)
