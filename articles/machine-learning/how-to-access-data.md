---
title: Azure 'da Storage Services 'a bağlanma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile eğitim sırasında Azure depolama hizmetlerine güvenli bir şekilde bağlanmak için veri depolarını nasıl kullanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 11/03/2020
ms.custom: how-to, contperfq1, devx-track-python, data4ml
ms.openlocfilehash: f60d864bd367b5f44869abc9ccac4e4cc266075a
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93358108"
---
# <a name="connect-to-storage-services-azure"></a>Depolama Hizmetleri Azure 'a bağlanma

Bu makalede, **Azure 'da Azure Machine Learning veri depoları aracılığıyla depolama hizmetlerine nasıl bağlanacağınızı** öğrenin. Veri depoları, kimlik doğrulama kimlik bilgilerinizi ve risk altındaki özgün veri kaynağınızın bütünlüğünü yapmadan Azure Storage hizmetinize güvenli bir şekilde bağlanır. Bu kişiler, çalışma alanıyla ilişkili [Key Vault](https://azure.microsoft.com/services/key-vault/) abonelik kimliğiniz ve belirteç yetkilendirmesi gibi bağlantı bilgilerini depolar, böylece depolama alanınıza güvenli bir şekilde kod yazmanız gerekmeden depolamaya güvenle erişebilirsiniz. Veri depoları oluşturmak ve kaydettirmek için [Azure Machine Learning Python SDK 'sını](#python) veya [Azure Machine Learning Studio 'yu](how-to-connect-data-ui.md) kullanabilirsiniz.

Azure Machine Learning VS Code uzantısını kullanarak veri depoları oluşturup yönetmeyi tercih ediyorsanız daha fazla bilgi edinmek için [vs Code kaynak yönetimi nasıl yapılır kılavuzunu](how-to-manage-resources-vscode.md#datastores) ziyaret edin.

[Bu Azure depolama çözümlerinden](#matrix)veri depoları oluşturabilirsiniz. **Desteklenmeyen depolama çözümleri için** ve ml denemeleri sırasında veri çıkış maliyetini kaydetmek için, verilerinizi desteklenen bir Azure depolama çözümüne [taşıyın](#move) .  

Datamağazaların Azure Machine Learning genel veri erişimi iş akışına uygun olduğunu anlamak için, [güvenli erişim verileri](concept-data.md#data-workflow) makalesine bakın.

## <a name="prerequisites"></a>Önkoşullar

Şunlara ihtiyacınız var:
- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- [Desteklenen depolama türü](#matrix)olan bir Azure depolama hesabı.

- [Python için Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)veya [Azure Machine Learning Studio](https://ml.azure.com/)'ya erişim.

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da Python SDK 'sı aracılığıyla mevcut bir tane kullanın. 

    `Workspace`Ve sınıfını içeri aktarın `Datastore` ve işlevini kullanarak dosyadaki abonelik bilgilerinizi yükleyin `config.json` `from_config()` . Bu, varsayılan olarak geçerli dizindeki JSON dosyasını arar, ancak kullanarak dosyayı işaret etmek için bir yol parametresi de belirtebilirsiniz `from_config(path="your/file/path")` .

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

    Bir çalışma alanı oluşturduğunuzda, bir Azure Blob kapsayıcısı ve bir Azure dosya paylaşımının otomatik olarak çalışma alanına veri depoları olarak kaydedilir. Bunlar `workspaceblobstore` `workspacefilestore` sırasıyla ve olarak adlandırılır. , `workspaceblobstore` Çalışma alanı yapıtlarını ve makine öğrenimi deneme günlüklerinizi depolamak için kullanılır. Aynı zamanda **varsayılan veri deposu** olarak ayarlanır ve çalışma alanından silinemez. , `workspacefilestore` [İşlem örneği](./concept-compute-instance.md#accessing-files)aracılığıyla yetkilendirilmiş not defterlerini ve R betiklerini depolamak için kullanılır.
    
    > [!NOTE]
    > Azure Machine Learning tasarımcı, tasarımcı giriş sayfasında bir örnek açtığınızda otomatik olarak **azureml_globaldatasets** adlı bir veri deposu oluşturacaktır. Bu veri deposu yalnızca örnek veri kümelerini içerir. Gizli veri erişimi **için lütfen bu veri deposunu kullanmayın.**

<a name="matrix"></a>

## <a name="supported-data-storage-service-types"></a>Desteklenen veri depolama hizmeti türleri

Datamağazaların Şu anda aşağıdaki matriste listelenen depolama hizmetlerine bağlantı bilgilerini depolamayı desteklemektedir.

| Depolama &nbsp; türü | Kimlik doğrulama &nbsp; türü | [Azure &nbsp; Machine &nbsp; Learning Studio](https://ml.azure.com/) | [Azure &nbsp; Machine &nbsp; Learning &nbsp; Python SDK 'sı](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py) |  [Azure &nbsp; Machine &nbsp; Learning CLI](reference-azure-machine-learning-cli.md) | [Azure &nbsp; Machine &nbsp; Learning &nbsp; REST API 'si](/rest/api/azureml/) | VS Code
---|---|---|---|---|---|---
[Azure &nbsp; BLOB &nbsp; depolama](../storage/blobs/storage-blobs-overview.md)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓ |✓
[Azure &nbsp; Dosya &nbsp; paylaşma](../storage/files/storage-files-introduction.md)| Hesap anahtarı <br> SAS belirteci | ✓ | ✓ | ✓ |✓|✓
[Azure &nbsp; Data Lake &nbsp; depolama gen &nbsp; 1](../data-lake-store/index.yml)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓|
[Azure &nbsp; Data Lake &nbsp; depolama gen &nbsp; 2](../storage/blobs/data-lake-storage-introduction.md)| Hizmet sorumlusu| ✓ | ✓ | ✓ |✓|
[Azure &nbsp; SQL &nbsp; veritabanı](../azure-sql/database/sql-database-paas-overview.md)| SQL kimlik doğrulaması <br>Hizmet sorumlusu| ✓ | ✓ | ✓ |✓|
[Azure &nbsp; PostgreSQL](../postgresql/overview.md) | SQL kimlik doğrulaması| ✓ | ✓ | ✓ |✓|
[&nbsp; &nbsp; MySQL için Azure &nbsp; veritabanı](../mysql/overview.md) | SQL kimlik doğrulaması|  | ✓* | ✓* |✓*|
[Databricks &nbsp; Dosya &nbsp; sistemi](/azure/databricks/data/databricks-file-system)| Kimlik doğrulaması yok | | ✓** | ✓ ** |✓** |

\*MySQL yalnızca işlem hattı [Datatransferstep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?preserve-view=true&view=azure-ml-py) için destekleniyor<br />
\*\*Databricks yalnızca işlem hattı [Databricksstep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?preserve-view=true&view=azure-ml-py) için destekleniyor


### <a name="storage-guidance"></a>Depolama yönergeleri

[Azure Blob kapsayıcısı](../storage/blobs/storage-blobs-introduction.md)için bir veri deposu oluşturmanızı öneririz. Blob 'lar için hem standart hem de Premium depolama alanı kullanılabilir. Premium Depolama daha pahalı olsa da, daha hızlı üretilen iş hızları, özellikle büyük bir veri kümesiyle eğeceğiniz durumlarda eğitim çalışmalarınızın hızını iyileştirebilir. Depolama hesaplarının maliyeti hakkında daha fazla bilgi için bkz. [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

[Azure Data Lake Storage 2.](../storage/blobs/data-lake-storage-introduction.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json) , Azure Blob depolama alanının üzerine kurulmuştur ve kurumsal büyük veri analizi için tasarlanmıştır. Data Lake Storage 2. temel bir parçası, blob depolamaya [hiyerarşik bir ad alanının](../storage/blobs/data-lake-storage-namespace.md) eklenmesinin bir parçasıdır. Hiyerarşik ad alanı, etkili veri erişimi için nesneleri/dosyaları bir dizin hiyerarşisi halinde düzenler.

## <a name="storage-access-and-permissions"></a>Depolama erişimi ve izinleri

Azure depolama hizmetinize güvenli bir şekilde bağlanabildiğinizden emin olmak için Azure Machine Learning, ilgili veri depolama kapsayıcısına erişmek için izninizin olmasını gerektirir. Bu erişim, veri deposunu kaydetmek için kullanılan kimlik doğrulama kimlik bilgilerine bağlıdır. 

### <a name="virtual-network"></a>Sanal ağ 

Veri depolama hesabınız bir **Sanal ağda** ise Azure Machine Learning verilerinize erişiminin olduğundan emin olmak için ek yapılandırma adımları gerekir. Veri deposundan kayıt oluşturup kaydettiğinizde uygun yapılandırma adımlarının uygulandığından emin olmak için bkz. [Azure sanal ağında Azure Machine Learning Studio 'Yu kullanma](how-to-enable-studio-virtual-network.md) .  

### <a name="access-validation"></a>Erişim doğrulaması

**İlk veri deposu oluşturma ve kayıt sürecinin bir parçası olarak** , Azure Machine Learning otomatik olarak temel alınan depolama hizmetinin olduğunu ve belirtilen kullanıcının (Kullanıcı adı, hizmet sorumlusu veya SAS belirteci) belirtilen depolama alanına erişimi olduğunu doğrular.

**Veri deposu oluşturulduktan sonra** , bu doğrulama yalnızca, veri deposu nesneleri her alınışında **değil** , temeldeki depolama kapsayıcısına erişim gerektiren yöntemler için gerçekleştirilir. Örneğin, veri deposundan dosyaları indirmek isterseniz doğrulama gerçekleşir; ancak yalnızca varsayılan veri deposundan değişiklik yapmak istiyorsanız doğrulama gerçekleşmez.

Temel depolama hizmetine erişiminizin kimlik doğrulamasını yapmak için, `register_azure_*()` oluşturmak istediğiniz veri deposu türünün karşılık gelen yönteminde hesap anahtarınızı, paylaşılan erişim imzaları (SAS) belirteçlerini veya hizmet sorumlusunu sağlayabilirsiniz. [Depolama türü matrisi](#matrix) , her veri deposu türüne karşılık gelen desteklenen kimlik doğrulama türlerini listeler.

Hesap anahtarını, SAS belirtecini ve hizmet sorumlusu bilgilerini [Azure Portal](https://portal.azure.com)bulabilirsiniz.

* Kimlik doğrulaması için bir hesap anahtarı veya SAS belirteci kullanmayı planlıyorsanız, sol bölmede **depolama hesapları** ' nı seçin ve kaydetmek istediğiniz depolama hesabını seçin. 
  * **Genel bakış** sayfası hesap adı, kapsayıcı ve dosya paylaşma adı gibi bilgiler sağlar. 
      1. Hesap anahtarları için, **Ayarlar** bölmesinde **erişim tuşları** ' na gidin. 
      1. SAS belirteçleri için, **Ayarlar** bölmesinde **paylaşılan erişim imzaları** ' na gidin.

* Kimlik doğrulaması için bir hizmet sorumlusu kullanmayı planlıyorsanız, **uygulama kayıtları** gidin ve kullanmak istediğiniz uygulamayı seçin. 
    * Kendisine karşılık gelen **genel bakış** sayfası, Kiracı kimliği ve istemci kimliği gibi gerekli bilgileri içerir.

> [!IMPORTANT]
> * Bir Azure depolama hesabı (hesap anahtarı veya SAS belirteci) için erişim anahtarlarınızı değiştirmeniz gerekiyorsa, yeni kimlik bilgilerini çalışma alanım ve ona bağlı veri depolarıyla eşitlediğinizden emin olun. [Güncelleştirilmiş kimlik bilgilerinizi eşitlemeyi](how-to-change-storage-access-key.md)öğrenin. 
### <a name="permissions"></a>İzinler

Azure Blob kapsayıcısı ve Azure Data Lake Gen 2 depolaması için, kimlik doğrulama kimlik bilgilerinizin **Depolama Blobu veri okuyucusu** erişimi olduğundan emin olun. [Depolama Blobu veri okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader)hakkında daha fazla bilgi edinin. Hesap SAS belirteci varsayılan olarak izin vermez. 
* Veri **okuma erişimi** için kimlik doğrulama kimlik bilgileriniz, kapsayıcılar ve nesneler için en az liste ve okuma izinlerine sahip olmalıdır. 

* Veri **yazma erişimi** için, yazma ve ekleme izinleri de gereklidir.

<a name="python"></a>

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Bir Azure depolama çözümünü bir veri deposu olarak kaydettiğinizde, bu veri deposunu otomatik olarak oluşturur ve belirli bir çalışma alanına kaydedersiniz. Sanal ağ senaryolarında yönergeler ve gerekli kimlik doğrulama kimlik bilgilerinin nerede bulunacağı hakkında bilgi edinmek için [depolama erişimi & izinleri](#storage-access-and-permissions) bölümünü gözden geçirin. 

Bu bölüm içinde, aşağıdaki depolama türleri için Python SDK 'Sı aracılığıyla bir veri deposu oluşturma ve kaydetme örnekleri verilmiştir. Bu örneklerde belirtilen parametreler, bir veri deposu oluşturmak ve kaydettirmek için **gereken parametrelerdir** .

* [Azure Blob kapsayıcısı](#azure-blob-container)
* [Azure dosya paylaşma](#azure-file-share)
* [Azure Data Lake Storage 2. nesil](#azure-data-lake-storage-generation-2)

 Desteklenen diğer depolama hizmetleri için veri depoları oluşturmak için, [ilgili `register_azure_*` Yöntemler için başvuru belgelerine](/python/api/azureml-core/azureml.core.datastore.datastore?preserve-view=true&view=azure-ml-py#&preserve-view=truemethods)bakın.

Düşük kod deneyimini tercih ediyorsanız bkz. [Azure Machine Learning Studio ile verilere bağlanma](how-to-connect-data-ui.md).
>[!IMPORTANT]
> Aynı ada sahip bir veri deposunu silip yeniden kaydettirir ve başarısız olursa, çalışma alanınızın Azure Key Vault geçici silme etkin olmayabilir. Varsayılan olarak, çalışma alanınız tarafından oluşturulan Anahtar Kasası örneği için geçici silme etkindir, ancak var olan bir Anahtar Kasası kullandıysanız veya 2020 Ekim ' den önce oluşturulmuş bir çalışma alanına sahipseniz bu işlem etkinleştirilmemiş olabilir. Geçici silme özelliğini etkinleştirme hakkında daha fazla bilgi için bkz. [var olan bir Anahtar Kasası Için geçici silme özelliğini etkinleştirme]( https://docs.microsoft.com/azure/key-vault/general/soft-delete-change#turn-on-soft-delete-for-an-existing-key-vault). "

> [!NOTE]
> Veri deposu adı yalnızca küçük harflerden, rakamlardan ve alt çizgilerden oluşmalıdır. 

### <a name="azure-blob-container"></a>Azure Blob kapsayıcısı

Bir Azure Blob kapsayıcısını bir veri deposu olarak kaydetmek için kullanın [`register_azure_blob_container()`](/python/api/azureml-core/azureml.core.datastore%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueregister-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-) .

Aşağıdaki kod, veri deposunu oluşturur ve `blob_datastore_name` `ws` çalışma alanına kaydeder. Bu veri deposu, `my-container-name` `my-account-name` belirtilen hesap erişim anahtarını kullanarak depolama hesabındaki blob kapsayıcısına erişir. Sanal ağ senaryolarında yönergeler ve gerekli kimlik doğrulama kimlik bilgilerinin nerede bulunacağı hakkında bilgi edinmek için [depolama erişimi & izinleri](#storage-access-and-permissions) bölümünü gözden geçirin. 

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

### <a name="azure-file-share"></a>Azure dosya paylaşımı

Bir Azure dosya paylaşımının kaydını bir veri deposu olarak kaydetmek için kullanın [`register_azure_file_share()`](/python/api/azureml-core/azureml.core.datastore%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueregister-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-) . 

Aşağıdaki kod, veri deposunu oluşturur ve `file_datastore_name` `ws` çalışma alanına kaydeder. Bu veri deposu `my-fileshare-name` `my-account-name` , belirtilen hesap erişim anahtarını kullanarak depolama hesabındaki dosya paylaşımına erişir. Sanal ağ senaryolarında yönergeler ve gerekli kimlik doğrulama kimlik bilgilerinin nerede bulunacağı hakkında bilgi edinmek için [depolama erişimi & izinleri](#storage-access-and-permissions) bölümünü gözden geçirin. 

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

### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage 2. nesil

Bir Azure Data Lake Storage 2. nesil (ADLS Gen 2) veri deposu için, [hizmet sorumlusu izinleriyle](../active-directory/develop/howto-create-service-principal-portal.md)bir Azure Data Lake Gen 2 depolamasına bağlı bir kimlik bilgisi veri deposunu kaydetmek için [register_azure_data_lake_gen2 ()](/python/api/azureml-core/azureml.core.datastore.datastore?preserve-view=true&view=azure-ml-py#&preserve-view=trueregister-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) kullanın.  

Hizmet sorumlunuzu kullanabilmeniz için, [uygulamanızı kaydetmeniz](../active-directory/develop/app-objects-and-service-principals.md) ve **Depolama Blobu veri okuyucusu** erişimi ile hizmet sorumlusu 'na vermeniz gerekir. [ADLS Gen 2 için erişim denetimi ayarlama](../storage/blobs/data-lake-storage-access-control.md)hakkında daha fazla bilgi edinin. 

Aşağıdaki kod, veri deposunu oluşturur ve `adlsgen2_datastore_name` `ws` çalışma alanına kaydeder. Bu veri deposu `test` `account_name` , belirtilen hizmet sorumlusu kimlik bilgilerini kullanarak depolama hesabındaki dosya sistemine erişir. Sanal ağ senaryolarında yönergeler ve gerekli kimlik doğrulama kimlik bilgilerinin nerede bulunacağı hakkında bilgi edinmek için [depolama erişimi & izinleri](#storage-access-and-permissions) bölümünü gözden geçirin. 

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

<a name="arm"></a>

## <a name="create-datastores-using-azure-resource-manager"></a>Azure Resource Manager kullanarak veri depoları oluşturma

Konumunda [https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-datastore-create-*](https://github.com/Azure/azure-quickstart-templates/tree/master/) veri depoları oluşturmak için kullanılabilecek çeşitli şablonlar vardır.

Bu şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md).

<a name="train"></a>
## <a name="use-data-in-your-datastores"></a>Veri Mağazalarınız içindeki verileri kullanma

Bir veri deposu oluşturduktan sonra verilerinizle etkileşimde bulunmak için [bir Azure Machine Learning veri kümesi oluşturun](how-to-create-register-datasets.md) . Veri kümeleri, eğitim gibi makine öğrenimi görevleri için verilerinizi geç tarafından değerlendirilen bir tüketilebilir nesneye paketleyin. Ayrıca, Azure Blob depolama ve ADLS Gen 2 gibi Azure depolama hizmetlerinden herhangi bir biçimdeki dosyaları [indirme veya bağlama](how-to-train-with-datasets.md#mount-vs-download) olanağı da sağlar. Bunları bir Pandas veya Spark DataFrame 'e tablo verileri yüklemek için de kullanabilirsiniz.

<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Çalışma alanınızdan veri depoları alın

Geçerli çalışma alanında kayıtlı belirli bir veri deposunu almak için, [`get()`](/python/api/azureml-core/azureml.core.datastore%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-workspace--datastore-name-) sınıfında statik yöntemi kullanın `Datastore` :

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Belirli bir çalışma alanıyla kaydedilen veri depolarının listesini almak için, [`datastores`](/python/api/azureml-core/azureml.core.workspace%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedatastores) özelliği bir çalışma alanı nesnesi üzerinde kullanabilirsiniz:

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

## <a name="access-data-during-scoring"></a>Puanlama sırasında verilere erişin

Azure Machine Learning, modellerinizi Puanlama için kullanmanın birkaç yolunu sağlar. Bu yöntemlerin bazıları veri depolarına erişim sağlamaz. Puanlama sırasında veri depolarına erişmenize izin veren yöntemleri anlamak için aşağıdaki tabloyu kullanın:

| Yöntem | Veri deposu erişimi | Açıklama |
| ----- | :-----: | ----- |
| [Toplu tahmin](./tutorial-pipeline-batch-scoring-classification.md) | ✔ | Büyük miktarlarda verileri zaman uyumsuz olarak tahmin edin. |
| [Web hizmeti](how-to-deploy-and-where.md) | &nbsp; | Modelleri bir Web hizmeti olarak dağıtın. |
| [Azure IoT Edge modülü](how-to-deploy-and-where.md) | &nbsp; | Modelleri IoT Edge cihazlara dağıtın. |

SDK 'nın veri depolarına erişim sağlamadığı durumlarda, verilere erişmek için ilgili Azure SDK 'sını kullanarak özel kod oluşturabilirsiniz. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python) , bloblarda veya dosyalarda depolanan verilere erişmek için kullanabileceğiniz bir istemci kitaplığıdır.

<a name="move"></a>

## <a name="move-data-to-supported-azure-storage-solutions"></a>Verileri desteklenen Azure depolama çözümlerine taşıma

Azure Machine Learning, Azure Blob depolama, Azure dosyaları, Azure Data Lake Storage 1., Azure Data Lake Storage 2., Azure SQL veritabanı ve PostgreSQL için Azure veritabanı 'ndan veri erişimini destekler. Desteklenmeyen depolama kullanıyorsanız, [Azure Data Factory ve bu adımları](../data-factory/quickstart-create-data-factory-copy-data-tool.md)kullanarak verilerinizi desteklenen Azure depolama çözümlerine taşımanızı öneririz. Verileri desteklenen depolamaya taşımak, Machine Learning denemeleri sırasında veri çıkış maliyetlerini kaydetmenize yardımcı olabilir. 

Azure Data Factory, ek ücret ödemeden 80 taneden fazla yerleşik bağlayıcı ile verimli ve dayanıklı veri aktarımı sağlar. Bu bağlayıcılar Azure veri Hizmetleri, şirket içi veri kaynakları, Amazon S3 ve Redshift ve Google BigQuery içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning veri kümesi oluşturma](how-to-create-register-datasets.md)
* [Modeli eğitme](how-to-set-up-training-targets.md)
* [Model dağıtma](how-to-deploy-and-where.md)
