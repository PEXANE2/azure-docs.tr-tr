---
title: Azure depolama hizmetlerine bağlanma
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
ms.openlocfilehash: c5972b602d92b2e08fd70850dd1af5c1236e2b1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234469"
---
# <a name="connect-to-azure-storage-services"></a>Azure depolama hizmetlerine bağlanma
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning veri depoları aracılığıyla Azure depolama hizmetlerine nasıl bağlanabileceğinizi öğrenin. Datastores, çalışma alanıyla ilişkili [Anahtar Kasası'nızda](https://azure.microsoft.com/services/key-vault/) abonelik kimliğiniz ve belirteç yetkilendirmeniz gibi bağlantı bilgilerini depolar, böylece komut dosyalarınızda sabit kodlamayapmak zorunda kalmadan depolama alanınıza güvenli bir şekilde erişebilirsiniz.

Bu Azure depolama [çözümlerinden](#matrix)veri depoları oluşturabilirsiniz. Desteklenmeyen depolama çözümleri ve makine öğrenimi denemeleri sırasında veri çıkış maliyetini kaydetmek için verilerinizi desteklenen Azure depolama çözümlerine [taşımanızı](#move) öneririz. 

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- [Azure blob kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) veya [Azure dosya paylaşımı](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)içeren bir Azure depolama hesabı.

- [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya Azure Machine Learning [stüdyosuna](https://ml.azure.com/)erişim.

- Azure Machine Learning çalışma alanı.
  
  [Bir Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md) veya Python SDK aracılığıyla varolan bir çalışma alanı kullanın. Ve `Workspace` `Datastore` sınıfı içe aktarın ve abonelik `config.json` bilgilerinizi işlevi `from_config()`kullanarak dosyadan yükleyin. Bu, varsayılan olarak geçerli dizinde JSON dosyasını arar, ancak dosyayı kullanarak `from_config(path="your/file/path")`işaret etmek için bir yol parametresi de belirtebilirsiniz.

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```
<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>Desteklenen veri depolama hizmet türleri

Datastores şu anda bağlantı bilgilerinin aşağıdaki matriste listelenen depolama hizmetlerine depolanmasını desteklemektedir. Şu anda Azure Veri ambarı desteklenmez. 

| Depolama&nbsp;türü | Kimlik&nbsp;doğrulama türü | [Azure&nbsp;&nbsp;Machine Learning stüdyosu](https://ml.azure.com/) | [Azure&nbsp;&nbsp;Machine&nbsp; Öğrenme Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) |  [Azure&nbsp;&nbsp;Makine Öğrenme CLI](reference-azure-machine-learning-cli.md) | [Azure&nbsp;Machine&nbsp;Learning&nbsp; Rest API](https://docs.microsoft.com/rest/api/azureml/)
---|---|---|---|---|---
[Azure&nbsp;Blob&nbsp;Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;&nbsp;Dosya Paylaşımı](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓
[Azure&nbsp;Veri&nbsp;Gölü&nbsp;Depolama Gen 1](https://docs.microsoft.com/azure/data-lake-store/)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;Veri&nbsp;Gölü&nbsp;Depolama Gen 2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;&nbsp;SQL Veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)| SQL kimlik doğrulaması <br>Hizmet sorumlusu| ✓ | ✓ | ✓ |✓
[Azure&nbsp;PostgreSQL](https://docs.microsoft.com/azure/postgresql/overview) | SQL kimlik doğrulaması| ✓ | ✓ | ✓ |✓
[MySQL&nbsp;&nbsp;için Azure&nbsp;Veritabanı](https://docs.microsoft.com/azure/mysql/overview) | SQL kimlik doğrulaması|  | ✓* | ✓* |✓*
[Databricks&nbsp;&nbsp;Dosya Sistemi](https://docs.microsoft.com/azure/databricks/data/databricks-file-system)| Kimlik doğrulaması yok | | ✓** | ✓ ** |✓** 

*MySQL yalnızca veri hattı [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py)için desteklenir. <br>
**Databricks sadece boru hattı [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) için desteklenir

### <a name="storage-guidance"></a>Depolama yönergeleri

[Azure Blob kapsayıcısı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)için bir veri deposu oluşturmanızı öneririz. Hem standart hem de premium depolama lekeler için kullanılabilir. Birinci sınıf depolama daha pahalı olmasına rağmen, daha hızlı veri hızı, özellikle büyük bir veri kümesine karşı antrenman yapıyorsanız, eğitim lerinizin hızını artırabilir. Depolama hesaplarının maliyeti hakkında daha fazla bilgi için [Azure fiyatlandırma hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service)bakın.

[Azure Veri Gölü Depolama Gen2,](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction?toc=/azure/storage/blobs/toc.json) Azure Blob depolama alanının üzerine inşa edilmiştir ve kurumsal büyük veri analitiği için tasarlanmıştır. Veri Gölü Depolama Gen2'nin temel bir parçası, Blob depolamasına [hiyerarşik](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) bir ad alanı eklenmesidir. Hiyerarşik ad alanı, nesneleri/dosyaları verimli veri erişimi için dizinler hiyerarşisi içinde düzenler.

Bir çalışma alanı oluşturduğunuzda, bir Azure blob kapsayıcısı ve Azure dosya paylaşımı otomatik olarak çalışma alanına kaydedilir. Adlandırılmışlar `workspaceblobstore` ve `workspacefilestore`sırasıyla. `workspaceblobstore`çalışma alanı yapılarını ve makine öğrenimi deneme günlüklerinizi depolamak için kullanılır. `workspacefilestore`bilgi işlem örneği ile yetkilendirilen defterleri ve R komut dosyalarını [depolamak](https://docs.microsoft.com/azure/machine-learning/concept-compute-instance#accessing-files)için kullanılır. Kapsayıcı `workspaceblobstore` varsayılan veri deposu olarak ayarlanır.

> [!IMPORTANT]
> Azure Machine Learning tasarımcısı (önizleme), tasarımcı ana sayfasında bir örneği açtığınızda **azureml_globaldatasets** otomatik olarak adlı bir veri deposu oluşturur. Bu veri deposu yalnızca örnek veri kümeleri içerir. Lütfen **do not** bu veri mağazanı herhangi bir gizli veri erişimi için kullanmayın!
> ![Tasarımcı örnek veri kümeleri için otomatik oluşturulan veri deposu](media/how-to-access-data/datastore-designer-sample.png)

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Bir Azure depolama çözümlerini veri deposu olarak kaydettiğinizde, bu veri deposunun otomatik olarak oluşturulmasını ve belirli bir çalışma alanına kaydedilmesi. Python SDK veya Azure Machine Learning stüdyosunu kullanarak veri depolarını oluşturabilir ve bir çalışma alanına kaydedebilirsiniz.

>[!IMPORTANT]
> Azure Machine Learning, ilk veri deposu oluşturma ve kaydetme işleminin bir parçası olarak, temel depolama hizmetinin var olduğunu ve temel sağlanan kullanıcının (kullanıcı adı, hizmet sorumlusu veya SAS belirteci) bu depolama alanına erişimi olduğunu doğrular. Ancak Azure Veri Gölü Depolama Gen 1 ve 2 veri depoları için [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py) bu [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-parquet-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) doğrulama, veri erişim yöntemleri gibi veya çağrıldığında daha sonra gerçekleşir. 
<br><br>
Datastore oluşturulduktan sonra, bu doğrulama yalnızca veri deposu nesneleri **her** kez değil, temel depolama kapsayıcısına erişim gerektiren yöntemler için gerçekleştirilir. Örneğin, doğrulama, veri deponuzdan dosya indirmek istiyorsanız gerçekleşir; ancak yalnızca varsayılan veri deponuzu değiştirmek istiyorsanız, doğrulama gerçekleşmez.

### <a name="python-sdk"></a>Python SDK'sı

Tüm kayıt yöntemleri [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıfta dır ve `register_azure_*`forma sahiptir.

`register()` Yöntemi doldurmak için ihtiyacınız olan bilgileri [Azure portalında](https://portal.azure.com)bulabilirsiniz.
Sol bölmede **Depolama Hesapları'nı** seçin ve kaydetmek istediğiniz depolama hesabını seçin. **Genel Bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşım adı gibi bilgiler sağlar. 

* Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama öğeleri için **Ayarlar** bölmesindeki **Hesap Anahtarları'na** gidin. 

* Kiracı kimliği ve istemci kimliği gibi hizmet temel öğeleri için **Uygulama kayıtlarınıza** gidin ve hangi uygulamayı kullanmak istediğinizi seçin. İlgili **Genel Bakış** sayfası bu öğeleri içerir.

> [!IMPORTANT]
> Depolama hesabınız sanal bir ağdaysa, **Yalnızca SDK üzerinden** Blob, File share, ADLS Gen 1 ve ADLS Gen 2 veri depolarının oluşturulması desteklenir. Çalışma alanınızı depolama hesabınıza erişim sağlamak için `grant_workspace_access` `True`parametreyi ' ye göre ayarlayın

Aşağıdaki örnekler, bir Azure blob kapsayıcısının, Azure dosya paylaşımının ve Azure Veri Gölü Depolama Oluşturma Oluşturma 2'nin nasıl veri deposu olarak kaydedileni gösterir. Diğer depolama hizmetleri için lütfen [geçerli `register_azure_*` yöntemlere ilişkin başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#methods)bakın.

#### <a name="blob-container"></a>Blob kapsayıcı

Azure blob kapsayıcısını veri deposu olarak [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)kaydetmek için .

Aşağıdaki kod veri deposunu `blob_datastore_name` `ws` oluşturur ve çalışma alanına kaydeder. Bu veri deposu, `my-container-name` sağlanan hesap `my-account-name` anahtarını kullanarak depolama hesabındaki blob kapsayıcısına erişir.

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

Azure dosya paylaşımını veri deposu olarak [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)kaydetmek için . 

Aşağıdaki kod veri deposunu `file_datastore_name` `ws` oluşturur ve çalışma alanına kaydeder. Bu veri deposu, `my-fileshare-name` sağlanan hesap `my-account-name` anahtarını kullanarak depolama hesabındaki dosya paylaşımına erişir.

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

#### <a name="azure-data-lake-storage-generation-2"></a>Azure Veri Gölü Depolama Oluşturma 2

Bir Azure Veri Gölü Depolama Oluşturma 2 (ADLS Gen 2) veri deposu için, [hizmet temel izinleri](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)ile Bir Azure DataLake Gen 2 depolamasına bağlı bir kimlik bilgisi veri deposu kaydetmek için [register_azure_data_lake_gen2()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) kullanın. Hizmet müdürünüzü kullanabilmek için [başvurunuzu kaydetmeniz](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) ve servis müdürüne doğru veri erişimi vermeniz gerekir. [ADLS Gen 2 için ayarlanmış erişim kontrolü](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)hakkında daha fazla bilgi edinin. 

Aşağıdaki kod veri deposunu `adlsgen2_datastore_name` `ws` oluşturur ve çalışma alanına kaydeder. Bu veri deposu, sağlanan `test` hizmet `account_name` temel kimlik bilgilerini kullanarak depolama hesabındaki dosya sistemine erişir.

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

Azure Machine Learning stüdyosunda birkaç adımda yeni bir veri deposu oluşturun:

> [!IMPORTANT]
> Depolama hesabınız sanal bir ağdaysa, yalnızca [SDK üzerinden](#python-sdk) veri depolarının oluşturulması desteklenir. 

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın.
1. **Yönet'in**altındaki sol bölmedeki **Datastores'ı** seçin.
1. + **Yeni datastore'u**seçin.
1. Yeni bir veri deposu için formu doldurun. Form, Azure depolama türü ve kimlik doğrulama türü ne yönelik seçimlerinize bağlı olarak kendisini akıllıca güncelleştirir.
  
Formu doldurmak için ihtiyacınız olan bilgileri [Azure portalında](https://portal.azure.com)bulabilirsiniz. Sol bölmede **Depolama Hesapları'nı** seçin ve kaydetmek istediğiniz depolama hesabını seçin. **Genel Bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşım adı gibi bilgiler sağlar. 

* Hesap anahtarı veya SAS belirteci gibi kimlik doğrulama öğeleri için **Ayarlar** bölmesindeki **Hesap Anahtarları'na** gidin. 

* Kiracı kimliği ve istemci kimliği gibi hizmet temel öğeleri için **Uygulama kayıtlarınıza** gidin ve hangi uygulamayı kullanmak istediğinizi seçin. İlgili **Genel Bakış** sayfası bu öğeleri içerir. 

Aşağıdaki örnek, bir Azure blob veri deposu oluşturduğunuzda formun nasıl göründüğünü gösterir: 
    
![Yeni bir veri deposu için form](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Çalışma alanınızdan veri depoları alın

Geçerli çalışma alanında belirli bir veri deposunun [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) kaydolmasını sağlamak `Datastore` için, sınıftaki statik yöntemi kullanın:

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Belirli bir çalışma alanına kayıtlı veri depolarının listesini almak [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) için, özelliği çalışma alanı nesnesi üzerinde kullanabilirsiniz:

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

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>Veri yükleme ve indirme

Aşağıdaki [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) örneklerde açıklanan yöntemler ve yöntemler [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) ve [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) sınıflarına özgüdir ve aynı şekilde çalışır.

### <a name="upload"></a>Karşıya Yükle

Python SDK'yı kullanarak bir dizin veya tek tek dosyaları veri deposuna yükleyin:

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

Parametre, `target_path` yüklenmesi gereken dosya paylaşımındaki (veya blob kapsayıcısındaki) konumu belirtir. Varsayılan olarak `None`, böylece veriler köke yüklenir. Varsa, `overwrite=True`varolan `target_path` veriler üzerine yazılırsa.

`upload_files()` Ayrıca, yöntem aracılığıyla veri deposuna tek tek dosyaların listesini yükleyebilirsiniz.

### <a name="download"></a>İndirme

Verileri bir veri deposundan yerel dosya sisteminize indirin:

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

Parametre, `target_path` verileri indirmek için yerel dizinin konumudur. İndirmek için dosya paylaşımındaki (veya blob kapsayıcısındaki) klasöre `prefix`bir yol belirtmek için, bu yolu . `None`Varsa, `prefix` dosya paylaşımınızın (veya blob kapsayıcınızın) tüm içeriği indirilir.

<a name="train"></a>

## <a name="access-your-data-during-training"></a>Eğitim sırasında verilerinize erişin

Veri depolarınızdaki verilerle etkileşimde kalmak veya verilerinizi eğitim gibi makine öğrenimi görevleri için sarf edilebilir bir nesneye paketlemek için [bir Azure Machine Learning veri seti oluşturun.](how-to-create-register-datasets.md) Veri kümeleri, bir pandalara veya Spark DataFrame'e tabular verileri yükleyen işlevler sağlar. Veri kümeleri ayrıca Azure Blob depolama, Azure Dosyaları, Azure Veri Gölü Depolama Gen1, Azure Veri Gölü Depolama Gen2, Azure SQL Veritabanı ve PostgreSQL için Azure Veritabanı'ndan herhangi bir biçimdeki dosyaları indirme veya ekleme olanağı da sağlar. [Veri kümeleriyle nasıl eğitilenler hakkında daha fazla bilgi edinin.](how-to-train-with-datasets.md)

### <a name="accessing-source-code-during-training"></a>Eğitim sırasında kaynak koduna erişim

Azure Blob depolama, Azure dosya paylaşımından daha yüksek iş verme hızına sahiptir ve paralel olarak başlatılan çok sayıda iş sayısına ölçeklendirilecek. Bu nedenle, kaynak kodu dosyalarını aktarmak için Blob depolama alanını kullanacak şekilde çalıştırmalarınızı yapılandırmanızı öneririz.

Aşağıdaki kod örneği, kaynak kodu aktarımları için kullanılacak blob datastore çalıştır yapılandırmasında belirtir.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Puanlama sırasında verilere erişin

Azure Machine Learning, puanlama için modellerinizi kullanmanın çeşitli yollarını sağlar. Bu yöntemlerden bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına hangi yöntemlerin erişmenizi sağladığını anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Datastore erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](how-to-use-parallel-run-step.md) | ✔ | Büyük miktarlarda veri üzerinde eşzamanlı olarak öngörülerde bulunun. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri web hizmeti olarak dağıtın. |
| [Azure IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge aygıtlarına dağıtın. |

SDK'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, [Python için Azure Depolama SDK'sı,](https://github.com/Azure/azure-storage-python) blob'larda veya dosyalarda depolanan verilere erişmek için kullanabileceğiniz bir istemci kitaplığıdır.

<a name="move"></a>

## <a name="move-data-to-supported-azure-storage-solutions"></a>Verileri desteklenen Azure depolama çözümlerine taşıma

Azure Machine Learning, PostgreSQL için Azure Blob depolama, Azure Dosyaları, Azure Veri Gölü Depolama Gen1, Azure Veri Gölü Depolama Gen2, Azure SQL Veritabanı ve Azure Veritabanı'ndan verilere erişmenizi destekler. Desteklenmeyen depolama alanı kullanıyorsanız, [Azure Veri Fabrikası'nı ve bu adımları](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool)kullanarak verilerinizi desteklenen Azure depolama çözümlerine taşımanızı öneririz. Desteklenen depolama ya da desteklenen depolama için veri taşıma, makine öğrenimi denemeleri sırasında veri çıkış maliyetlerikaydedin yardımcı olabilir. 

Azure Veri Fabrikası, 80'den fazla önceden oluşturulmuş bağlayıcıyla ek ücret ödemeden verimli ve esnek veri aktarımı sağlar. Bu bağlayıcılar arasında Azure veri hizmetleri, şirket içi veri kaynakları, Amazon S3 ve Redshift ve Google BigQuery yer almaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure makine öğrenme veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Modeli eğitme](how-to-train-ml-models.md)
* [Model dağıtma](how-to-deploy-and-where.md)
