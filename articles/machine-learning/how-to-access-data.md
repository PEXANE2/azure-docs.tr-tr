---
title: Azure Storage Services 'a bağlanma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile eğitim sırasında Azure depolama hizmetlerine güvenli bir şekilde bağlanmak için veri depolarını nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 03/24/2020
ms.custom: seodec18
ms.openlocfilehash: f5a7605a1fa68c3a600c77ded762722990d7a514
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231212"
---
# <a name="connect-to-azure-storage-services"></a>Azure Storage Services 'a bağlanma
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning veri depoları aracılığıyla Azure Storage Services 'a nasıl bağlanacağınızı öğrenin. Veri deposu, çalışma alanıyla ilişkili [Key Vault](https://azure.microsoft.com/services/key-vault/) abonelik kimliğiniz ve belirteç yetkilendirmesi gibi mağaza bağlantı bilgilerini, betiklerinizde kalıcı olarak kodlamadan depolamaya güvenle erişebilmek için depolar. Datamağazaların Azure Machine Learning genel veri erişimi iş akışına uygun olduğunu anlamak için, [güvenli erişim verileri](concept-data.md#data-workflow) makalesine bakın.

[Bu Azure depolama çözümlerinden](#matrix)veri depoları oluşturabilirsiniz. Desteklenmeyen depolama çözümleri için ve Machine Learning denemeleri sırasında veri çıkış maliyetini kaydetmek için, verilerinizi desteklenen Azure depolama çözümlerine [taşımanızı](#move) öneririz. 

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- Azure [BLOB kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) veya [Azure dosya paylaşımıyla](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)Azure depolama hesabı.

- [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da Python SDK 'sı aracılığıyla mevcut bir tane kullanın. `Workspace` Ve `Datastore` sınıfını içeri aktarın ve işlevini `config.json` `from_config()`kullanarak dosyadaki abonelik bilgilerinizi yükleyin. Bu, varsayılan olarak geçerli dizindeki JSON dosyasını arar, ancak kullanarak `from_config(path="your/file/path")`dosyayı işaret etmek için bir yol parametresi de belirtebilirsiniz.

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```
<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>Desteklenen veri depolama hizmeti türleri

Datamağazaların Şu anda aşağıdaki matriste listelenen depolama hizmetlerine bağlantı bilgilerini depolamayı desteklemektedir.

| Depolama&nbsp;türü | Kimlik&nbsp;doğrulama türü | [Azure&nbsp;Machine&nbsp;Learning Studio](https://ml.azure.com/) | [Azure&nbsp;Machine&nbsp;Learning&nbsp; Python SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) |  [Azure&nbsp;MACHINE&nbsp;Learning CLI](reference-azure-machine-learning-cli.md) | [Azure&nbsp;Machine&nbsp;Learning&nbsp; REST API 'si](https://docs.microsoft.com/rest/api/azureml/)
---|---|---|---|---|---
[Azure&nbsp;blob&nbsp;depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;dosya&nbsp;paylaşma](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;depolama gen&nbsp;1](https://docs.microsoft.com/azure/data-lake-store/)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;Data Lake&nbsp;depolama gen&nbsp;2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;SQL&nbsp;veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)| SQL kimlik doğrulaması <br>Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;PostgreSQL](https://docs.microsoft.com/azure/postgresql/overview) | SQL kimlik doğrulaması| ✓ | ✓ | ✓ |✓
[MySQL&nbsp;için&nbsp;&nbsp;Azure veritabanı](https://docs.microsoft.com/azure/mysql/overview) | SQL kimlik doğrulaması|  | ✓* | ✓* |✓*
[Databricks&nbsp;dosya&nbsp;sistemi](https://docs.microsoft.com/azure/databricks/data/databricks-file-system)| Kimlik doğrulaması yok | | ✓** | ✓ ** |✓** 

* MySQL yalnızca işlem hattı [Datatransferstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py)için desteklenir. <br>
* * Databricks yalnızca işlem hattı [Databricksstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) için destekleniyor

### <a name="storage-guidance"></a>Depolama yönergeleri

[Azure Blob kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)için bir veri deposu oluşturmanızı öneririz. Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Premium Depolama daha pahalı olsa da, daha hızlı üretilen iş hızları, özellikle büyük bir veri kümesiyle eğeceğiniz durumlarda eğitim çalışmalarınızın hızını iyileştirebilir. Depolama hesaplarının maliyeti hakkında daha fazla bilgi için bkz. [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

[Azure Data Lake Storage 2.](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction?toc=/azure/storage/blobs/toc.json) , Azure Blob depolama alanının üzerine kurulmuştur ve kurumsal büyük veri analizi için tasarlanmıştır. Data Lake Storage 2. temel bir parçası, blob depolamaya [hiyerarşik bir ad alanının](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) eklenmesinin bir parçasıdır. Hiyerarşik ad alanı, etkili veri erişimi için nesneleri/dosyaları bir dizin hiyerarşisi halinde düzenler.

Bir çalışma alanı oluşturduğunuzda, bir Azure Blob kapsayıcısı ve bir Azure dosya paylaşımında otomatik olarak çalışma alanı kaydedilir. Bunlar sırasıyla ve `workspaceblobstore` `workspacefilestore`olarak adlandırılır. `workspaceblobstore`, çalışma alanı yapıtlarını ve makine öğrenimi deneme günlüklerinizi depolamak için kullanılır. `workspacefilestore`[işlem örneği](https://docs.microsoft.com/azure/machine-learning/concept-compute-instance#accessing-files)aracılığıyla yetkilendirilmiş not defterlerini ve R betiklerini depolamak için kullanılır. `workspaceblobstore` Kapsayıcı varsayılan veri deposu olarak ayarlanır.

> [!IMPORTANT]
> Azure Machine Learning tasarımcı (Önizleme), tasarımcı giriş sayfasında bir örnek açtığınızda otomatik olarak **azureml_globaldatasets** adlı bir veri deposu oluşturacak. Bu veri deposu yalnızca örnek veri kümelerini içerir. Gizli veri erişimi **için lütfen bu veri deposunu kullanmayın.**
> ![Tasarımcı örnek veri kümeleri için otomatik olarak oluşturulan veri deposu](media/how-to-access-data/datastore-designer-sample.png)

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Bir Azure depolama çözümünü bir veri deposu olarak kaydettiğinizde, bu veri deposunu otomatik olarak oluşturur ve belirli bir çalışma alanına kaydedersiniz. [Python SDK](#python-sdk) veya [Azure Machine Learning Studio](#azure-machine-learning-studio)kullanarak bir çalışma alanına veri depoları oluşturabilir ve kaydedebilirsiniz.

>[!IMPORTANT]
> İlk veri deposu oluşturma ve kaydetme işlemi kapsamında, Azure Machine Learning temel alınan depolama hizmetinin olduğunu ve kullanıcının (Kullanıcı adı, hizmet sorumlusu veya SAS belirteci) Bu depolama alanına erişimi olduğunu doğrular. Ancak, Azure Data Lake Storage Gen 1 ve 2 veri depoları için bu doğrulama daha sonra olur, veya [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py) [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-parquet-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) gibi veri erişimi yöntemleri çağrılır. 
<br><br>
Veri deposu oluşturulduktan sonra, bu doğrulama yalnızca, veri deposu nesneleri her alınışında **değil** , temeldeki depolama kapsayıcısına erişim gerektiren yöntemler için gerçekleştirilir. Örneğin, veri deposundan dosyaları indirmek isterseniz doğrulama gerçekleşir; ancak yalnızca varsayılan veri deposundan değişiklik yapmak istiyorsanız doğrulama gerçekleşmez.

### <a name="python-sdk"></a>Python SDK'sı

Tüm yazmaç yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıfta bulunur ve formundadır `register_azure_*`.
> [!IMPORTANT]
> Depolama hesabınız bir sanal ağda ise, yalnızca **SDK aracılığıyla** veri depolarının oluşturulması desteklenir.

`register_azure_*()` Yöntemi [Azure Portal](https://portal.azure.com)doldurmanız için gereken bilgileri bulabilirsiniz.

* Kimlik doğrulaması için bir hesap anahtarı veya SAS belirteci kullanmayı planlıyorsanız, sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. 
  * **Genel bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler sağlar. 
      1. Hesap anahtarları için, **Ayarlar** bölmesinde **erişim tuşları** ' na gidin. 
      1. SAS belirteçleri için, **Ayarlar** bölmesinde **paylaşılan erişim imzaları** ' na gidin.

* Kimlik doğrulaması için bir hizmet ilkesi kullanmayı planlıyorsanız, **uygulama kayıtları** gidin ve kullanmak istediğiniz uygulamayı seçin. 
    * Kendisine karşılık gelen **genel bakış** sayfası, Kiracı kimliği ve istemci kimliği gibi gerekli bilgileri içerir.

Aşağıdaki örneklerde bir Azure Blob kapsayıcısının, Azure dosya paylaşımının ve Azure Data Lake Storage nesil 2 ' nin bir veri deposu olarak nasıl kaydedileceği gösterilmektedir. Bu örneklerde belirtilen parametreler, bir veri deposu oluşturmak ve kaydettirmek için **gereken parametrelerdir** . 

Diğer depolama hizmetleri için veri depoları oluşturmak ve bu yöntemler için isteğe bağlı parametreleri görmek üzere, lütfen [ilgili `register_azure_*` yöntemler için başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#methods)bakın.

#### <a name="blob-container"></a>Blob kapsayıcı

Bir Azure Blob kapsayıcısını bir veri deposu olarak kaydetmek için kullanın [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

Aşağıdaki kod, `blob_datastore_name` veri deposunu oluşturur ve `ws` çalışma alanına kaydeder. Bu veri deposu, `my-container-name` belirtilen hesap erişim anahtarını `my-account-name` kullanarak depolama hesabındaki blob kapsayıcısına erişir.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account access key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```
Blob Kapsayıcınız sanal ağda ise, yöntecunuza `skip_validation=True` [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-) parametresini ekleyin. 

#### <a name="file-share"></a>Dosya paylaşımı

Bir Azure dosya paylaşımının kaydını bir veri deposu olarak kaydetmek için [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)kullanın. 

Aşağıdaki kod, `file_datastore_name` veri deposunu oluşturur ve `ws` çalışma alanına kaydeder. Bu veri deposu, `my-fileshare-name` belirtilen hesap erişim anahtarını `my-account-name` kullanarak depolama hesabındaki dosya paylaşımına erişir.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account access key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                     datastore_name=file_datastore_name, 
                                                     file_share_name=file_share_name, 
                                                     account_name=account_name,
                                                     account_key=account_key)
```
Dosya paylaşımınız sanal ağda ise, `skip_validation=True` [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-) yöntemi parametresini ekleyin. 

#### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage 2. nesil

Bir Azure Data Lake Storage 2. nesil (ADLS Gen 2) veri deposu için, [hizmet sorumlusu izinleriyle](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)bir Azure Data Lake Gen 2 depolamasına bağlı bir kimlik bilgisi veri deposunu kaydetmek için [register_azure_data_lake_gen2 ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) kullanın. Hizmet sorumlunuzu kullanabilmeniz için, [uygulamanızı kaydetmeniz](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) ve *Depolama Blobu veri sahibi* erişimine sahip hizmet sorumlusu 'na vermeniz gerekir. [ADLS Gen 2 için erişim denetimi ayarlama](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)hakkında daha fazla bilgi edinin. 

Hizmet sorumlunuzu kullanabilmeniz için [uygulamanızı kaydetmeniz](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) ve hizmet sorumlusuna doğru veri erişimi vermeniz gerekir. [ADLS Gen 2 için erişim denetimi ayarlama](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)hakkında daha fazla bilgi edinin. 

Aşağıdaki kod, `adlsgen2_datastore_name` veri deposunu oluşturur ve `ws` çalışma alanına kaydeder. Bu veri deposu, belirtilen hizmet `test` sorumlusu kimlik `account_name` bilgilerini kullanarak depolama hesabındaki dosya sistemine erişir.

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

> [!IMPORTANT]
> Depolama hesabınız bir sanal ağda ise, yalnızca [SDK aracılığıyla](#python-sdk) veri depolarının oluşturulması desteklenir. 

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da oturum açın.
1. **Yönet**altında sol bölmedeki **veri depoları** ' nı seçin.
1. **+ Yeni veri deposu**seçin.
1. Yeni bir veri deposu için formu doldurun. Form, Azure depolama türü ve kimlik doğrulama türü seçimlerinize göre kendisini akıllıca güncelleştirir.
  
Formu [Azure Portal](https://portal.azure.com)doldurmanız için gereken bilgileri bulabilirsiniz. Sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. **Genel bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler sağlar. 

* Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama öğeleri için, **Ayarlar** bölmesinde **erişim tuşları** ' na gidin. 

* Kiracı KIMLIĞI ve istemci KIMLIĞI gibi hizmet sorumlusu öğeleri için **uygulama kayıtları** gidin ve kullanmak istediğiniz uygulamayı seçin. Buna karşılık gelen **genel bakış** sayfası, bu öğeleri içerir. 

Aşağıdaki örnek, bir Azure blob veri deposu oluştururken formun nasıl göründüğünü göstermektedir: 
    
![Yeni veri deposu için form](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Çalışma alanınızdan veri depoları alın

Geçerli çalışma alanında kayıtlı belirli bir veri deposunu almak için, [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) `Datastore` sınıfında statik yöntemi kullanın:

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Belirli bir çalışma alanıyla kaydedilen veri depolarının listesini almak için, [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) özelliği bir çalışma alanı nesnesi üzerinde kullanabilirsiniz:

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
Varsayılan veri deposunu aşağıdaki kodla da değiştirebilirsiniz. Bu özellik yalnızca SDK aracılığıyla desteklenir. 

```Python
 ws.set_default_datastore(new_default_datastore)
```
<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>Verileri karşıya yükleme ve indirme

Aşağıdaki [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) örneklerde [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) açıklanan ve yöntemleri, [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) ve [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) sınıfları için de aynı şekilde çalışır.

> [!NOTE]
> AzureDataLakeGen2 datamağazalara yükleme şu anda desteklenmiyor.

### <a name="upload"></a>Karşıya Yükle

Python SDK kullanarak veri deposuna bir dizin veya tek bir dosya yükleyin:

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

`target_path` Parametresi dosya paylaşımında (veya blob kapsayıcısında) karşıya yüklenecek konumu belirtir. Varsayılan olarak `None`, verileri köke yüklenir. Varsa `overwrite=True`, üzerindeki `target_path` mevcut verilerin üzerine yazılır.

Ayrıca, `upload_files()` yöntemi aracılığıyla tek tek dosyaların bir listesini veri deposuna yükleyebilirsiniz.

### <a name="download"></a>İndirme

Veri deposundaki verileri yerel dosya sisteminize indirin:

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

`target_path` Parametresi, verilerin indirileceği yerel dizinin konumudur. İndirilecek dosya paylaşımında (veya blob kapsayıcısında) klasöre bir yol belirtmek için, bu yolu sağlayın `prefix`. `prefix` İse `None`, dosya paylaşımınızın (veya blob kapsayıcısının) tüm içerikleri indirilir.

<a name="train"></a>

## <a name="access-your-data-during-training"></a>Eğitim sırasında verilerinize erişin

Veri mağazalarınızla verilerle etkileşim kurmak veya verilerinizi makine öğrenimi görevleri için eğitim gibi tüketilebilir bir nesneye paketlemek için [Azure Machine Learning bir veri kümesi oluşturun](how-to-create-register-datasets.md). Veri kümeleri, Pandas veya Spark DataFrame 'e tablo verileri yükleyen işlevler sağlar. Veri kümeleri ayrıca Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı aracılığıyla herhangi bir biçimde dosya indirme veya bağlama olanağı sağlar. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](how-to-train-with-datasets.md).

### <a name="accessing-source-code-during-training"></a>Eğitim sırasında kaynak koduna erişme

Azure Blob depolama, bir Azure dosya paylaşımından daha yüksek işleme hızına sahiptir ve paralel olarak başlatılan çok sayıda işi ölçeklendirecektir. Bu nedenle, çalıştırmaların kaynak kodu dosyalarını aktarmak için blob depolamayı kullanacak şekilde yapılandırılmasını öneririz.

Aşağıdaki kod örneği, kaynak kodu aktarımları için kullanılacak blob veri deposuna ait çalıştırma yapılandırması ' nda belirtir.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Puanlama sırasında verilere erişin

Azure Machine Learning, modellerinizi Puanlama için kullanmanın birkaç yolunu sağlar. Bu yöntemlerin bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına erişmenize izin veren yöntemleri anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Veri deposu erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](how-to-use-parallel-run-step.md) | ✔ | Büyük miktarlarda verileri zaman uyumsuz olarak tahmin edin. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri bir Web hizmeti olarak dağıtın. |
| [Azure IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge cihazlara dağıtın. |

SDK 'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK 'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python) , bloblarda veya dosyalarda depolanan verilere erişmek için kullanabileceğiniz bir istemci kitaplığıdır.

<a name="move"></a>

## <a name="move-data-to-supported-azure-storage-solutions"></a>Verileri desteklenen Azure depolama çözümlerine taşıma

Azure Machine Learning, Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı 'ndan veri erişimini destekler. Desteklenmeyen depolama kullanıyorsanız, [Azure Data Factory ve bu adımları](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool)kullanarak verilerinizi desteklenen Azure depolama çözümlerine taşımanızı öneririz. Verileri desteklenen depolamaya taşımak, Machine Learning denemeleri sırasında veri çıkış maliyetlerini kaydetmenize yardımcı olabilir. 

Azure Data Factory, ek ücret ödemeden 80 taneden fazla yerleşik bağlayıcı ile verimli ve dayanıklı veri aktarımı sağlar. Bu bağlayıcılar Azure veri Hizmetleri, şirket içi veri kaynakları, Amazon S3 ve Redshift ve Google BigQuery içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Modeli eğitme](how-to-train-ml-models.md)
* [Model dağıtma](how-to-deploy-and-where.md)
