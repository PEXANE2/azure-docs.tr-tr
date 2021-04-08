---
title: Azure 'da depolama hizmetlerine kimlik tabanlı veri erişimi
titleSuffix: Azure Machine Learning
description: Azure 'da depolama hizmetlerine bağlanmak için kimlik tabanlı veri erişimini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 02/22/2021
ms.custom: how-to, contperf-fy21q1, devx-track-python, data4ml
ms.openlocfilehash: 68d07481e228b1d1b2f4571a783f925add261cff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102520022"
---
# <a name="connect-to-storage-with-identity-based-data-access-preview"></a>Kimlik tabanlı veri erişimi (Önizleme) ile depolama 'ya bağlanma

>[!IMPORTANT]
> Bu makalede sunulan işlevler önizlemededir ve herhangi bir zamanda değişebilir, [deneysel](/python/api/overview/azure/ml/#stable-vs-experimental) Önizleme özellikleri olarak kabul edilmelidir.

Bu makalede, Azure 'daki depolama hizmetlerine kimlik tabanlı veri erişimi ve [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro)aracılığıyla Azure Machine Learning veri depoları ile nasıl bağlanacağınızı öğreneceksiniz.  

Genellikle, veri depoları depolama hizmetine erişim izninizin olduğunu onaylamak için kimlik bilgisi tabanlı veri erişimini kullanır. Bunlar, çalışma alanıyla ilişkili [Key Vault](https://azure.microsoft.com/services/key-vault/) abonelik kimliğiniz ve belirteç yetkilendirmesi gibi bağlantı bilgilerini tutar. Kimlik tabanlı veri erişimi kullanan bir veri deposu oluşturduğunuzda, depolama hizmetine erişim izninizin olduğunu doğrulamak için Azure oturum açma ([Azure Active Directory belirteç](../active-directory/fundamentals/active-directory-whatis.md)) kullanılır. Bu senaryoda, kimlik doğrulama kimlik bilgileri kaydedilmez ve yalnızca depolama hesabı bilgileri veri deposunda depolanır. 

Erişim anahtarları veya hizmet sorumluları gibi kimlik bilgisi tabanlı kimlik doğrulaması kullanan veri depoları oluşturmak için bkz. [Azure 'da depolama hizmetlerine bağlanma](how-to-access-data.md).

## <a name="identity-based-data-access-in-azure-machine-learning"></a>Azure Machine Learning 'de kimlik tabanlı veri erişimi

Azure Machine Learning ' de kimlik tabanlı veri erişimi uygulamak için iki alan vardır. Özellikle, gizli verilerle çalışırken ve daha ayrıntılı veri erişim yönetimine ihtiyaç duyduğunuzda. 

1. Depolama hizmetlerine erişme.
1. Özel verilerle makine öğrenimi modellerini eğitim.

### <a name="accessing-storage-services"></a>Depolama hizmetlerine erişme

Depolama hizmetlerine, Azure Machine Learning veri depoları veya [Azure Machine Learning veri kümeleri](how-to-create-register-datasets.md)ile kimlik tabanlı veri erişimi aracılığıyla bağlanabilirsiniz. 

Genellikle, kimlik doğrulama kimlik bilgileriniz depolama hizmetine erişim izninizin olduğundan emin olmak için kullanılan bir veri deposunda tutulur. Bu kimlik bilgileri veri depolarıyla kaydedildiğinde, çalışma alanı *okuyucusu* rolüne sahip tüm kullanıcılar bunları alabilir; bu da bazı kuruluşlarda ilgili bir güvenlik sorunu olabilir. [Çalışma alanı *okuyucu* rolü hakkında daha fazla bilgi edinin](how-to-assign-roles.md#default-roles). 

Kimlik tabanlı veri erişimi kullandığınızda Azure Machine Learning, veri deposunda kimlik bilgilerinizi tutmak yerine, veri erişimi kimlik doğrulaması için Azure Active Directory belirtecinizi ister. Bu, depolama düzeyinde veri erişimi yönetimine izin veren ve kimlik bilgilerini gizli tutar. 

Aynı davranış,

* [Doğrudan depolama URL 'lerinden bir veri kümesi oluşturun](#use-data-in-storage). 
* Yerel makinenizdeki veya [işlem örneğinizin](concept-compute-instance.md)bir Jupyter Not defteri aracılığıyla verilerle etkileşimli olarak çalışın.

> [!NOTE]
> Kimlik bilgisi tabanlı kimlik doğrulaması kullanılarak depolanan kimlik bilgileri şunlardır: abonelik KIMLIĞI, paylaşılan erişim imzası (SAS) belirteçleri, depolama erişim anahtarları ve hizmet sorumlusu bilgileri, örneğin, istemci KIMLIĞI ve kiracı KIMLIĞI.

### <a name="model-training-on-private-data"></a>Özel veriler üzerinde model eğitimi

Belirli makine öğrenimi senaryoları, özel verilerle eğitim modelleri içerir. Bu gibi durumlarda, veri bilimcilerinin, gizli giriş verilerine maruz kalmadan eğitim iş akışlarını çalıştırması gerekir. Bu senaryoda, veri erişimi kimlik doğrulaması için eğitim işlem tarafından yönetilen bir kimlik kullanılır. Bu şekilde, depolama yöneticileri Eğitim sürecinin bireysel veri bilimcileri yerine eğitim işini çalıştırmak için kullandığı yönetilen kimliğe **Depolama Blobu veri okuyucusu** erişimi verebilir. [Bir işlem üzerinde yönetilen kimlik ayarlamayı](how-to-create-attach-compute-cluster.md#managed-identity)öğrenin.


## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

- Desteklenen depolama türü olan bir Azure depolama hesabı. Önizleme sürümünde aşağıdaki depolama türleri desteklenir. 
    - [Azure Blob Depolama](../storage/blobs/storage-blobs-overview.md)
    - [Azure Data Lake Gen 1](../data-lake-store/index.yml)
    - [Azure Data Lake Gen 2](../storage/blobs/data-lake-storage-introduction.md)
    - [Azure SQL veritabanı](../azure-sql/database/sql-database-paas-overview.md)

- [Python için Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/install).

- Azure Machine Learning çalışma alanı.
  
  [Azure Machine Learning bir çalışma alanı oluşturun](how-to-manage-workspace.md) ya da [Python SDK 'sı aracılığıyla mevcut bir tane](how-to-manage-workspace.md#connect-to-a-workspace)kullanın. 

## <a name="storage-access-permissions"></a>Depolama erişim izinleri

Azure 'da depolama hizmetinize güvenli bir şekilde bağlantığınızdan emin olmak için Azure Machine Learning, karşılık gelen veri depolamaya erişmek için izninizin olmasını gerektirir.

Kimlik tabanlı veri erişimi yalnızca aşağıdaki depolama hizmetlerine yönelik bağlantıları destekler:

* Azure Blob Depolama
* Azure Data Lake oluşturma 1
* Azure Data Lake 2. nesil
* Azure SQL veritabanı

Bu depolama hizmetlerine erişmek için en az **Depolama Blobu veri okuyucusu** erişiminizin olması gerekir. [Depolama Blobu veri okuyucu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader)hakkında daha fazla bilgi edinin. Yalnızca depolama hesabı sahipleri [Azure Portal aracılığıyla erişim düzeyinizi değiştirebilir](../storage/common/storage-auth-aad-rbac-portal.md).

Bir modeli uzak bir işlem hedefinde eğliyorsanız, depolama kimliği depolama hizmetinden en az **Depolama Blobu veri okuyucusu** rolüyle verilmelidir. [İşlem sırasında yönetilen kimlik ayarlamayı](how-to-create-attach-compute-cluster.md#managed-identity)öğrenin.

## <a name="work-with-virtual-networks"></a>Sanal ağlarla çalışma

Azure Machine Learning, varsayılan olarak, güvenlik duvarının arkasındaki veya bir sanal ağ içindeki bir depolama hesabıyla iletişim kuramaz.

Depolama hesapları yalnızca belirli sanal ağlardan erişime izin verecek şekilde yapılandırılabilir. Bu, verilerin ağ dışından sızmasını sağlamak için ek yapılandırma gerektirir. Bu davranış, kimlik bilgisi tabanlı veri erişimi için aynıdır, [Hangi yapılandırmaların gerekli olduğunu ve sanal ağ senaryolarında nasıl uygulanacağını](how-to-access-data.md#virtual-network)öğrenin. 

## <a name="create-and-register-datastores"></a>Veri depoları oluşturma ve kaydetme

Azure 'da bir depolama hizmetini bir veri deposu olarak kaydettiğinizde, bu veri deposunu otomatik olarak oluşturur ve belirli bir çalışma alanına kaydedersiniz. Bu bölümleri inceleyin: gerekli izin türleri hakkında rehberlik için [depolama erişim izinleri](#storage-access-permissions) ve sanal ağların arkasında veri depolamaya bağlanma hakkında ayrıntılı bilgi için [sanal ağla birlikte çalışın](#work-with-virtual-networks) .

Aşağıdaki kodda,,, `sas_token` `account_key` `subscription_id` veya hizmet sorumlusu gibi kimlik doğrulama parametrelerinin yokluğuna dikkat edin `client_id` . Bu atlama, Azure Machine Learning kimlik doğrulaması için kimlik tabanlı veri erişiminin kullanılacağını gösterir. Veri depolarının oluşturulması genellikle bir not defterinde veya Studio aracılığıyla etkileşimli olarak yapıldığından, Azure Active Directory belirteciniz veri erişimi kimlik doğrulaması için kullanılır.

> [!NOTE]
> Veri deposu adları yalnızca küçük harflerden, rakamlardan ve alt çizgilerden oluşmalıdır. 

### <a name="azure-blob-container"></a>Azure Blob kapsayıcısı

Bir Azure Blob kapsayıcısını bir veri deposu olarak kaydetmek için kullanın [`register_azure_blob_container()`](/python/api/azureml-core/azureml.core.datastore%28class%29#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-) .

Aşağıdaki kod, `credentialless_blob` veri deposunu oluşturup `ws` çalışma alanına kaydettirir ve değişkenine atar `blob_datastore` . Bu veri deposu, `my_container_name` depolama hesabındaki blob kapsayıcısına erişir `my-account-name` .

```Python
# create blob datastore without credentials
blob_datastore = Datastore.register_azure_blob_container(workspace=ws,
                                                      datastore_name='credentialless_blob',
                                                      container_name='my_container_name',
                                                      account_name='my_account_name')
```

### <a name="azure-data-lake-storage-generation-1"></a>Azure Data Lake Storage oluşturma 1

Azure Data Lake Storage nesil 1 (ADLS Gen 1) veri deposu için, Azure Data Lake Generation 1 depolamasına bağlanan bir veri deposunu kaydetmek için [register_azure_data_lake ()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-workspace--datastore-name--store-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--subscription-id-none--resource-group-none--overwrite-false--grant-workspace-access-false-) kullanın.

Aşağıdaki kod, `credentialless_adls1` veri deposunu oluşturup `workspace` çalışma alanına kaydettirir ve değişkenine atar `adls_dstore` . Bu veri deposu `adls_storage` Azure Data Lake Store depolama hesabına erişir.

```Python
# create adls gen1 without credentials
adls_dstore = Datastore.register_azure_data_lake(workspace = workspace,
                                                 datastore_name='credentialless_adls1',
                                                 store_name='adls_storage')

```

### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage 2. nesil

Bir Azure Data Lake Storage 2. nesil (ADLS Gen 2) veri deposu için, bir Azure Data Lake Gen 2 depolamasına bağlanan bir veri deposunu kaydetmek için [register_azure_data_lake_gen2 ()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-) kullanın.

Aşağıdaki kod, `credentialless_adls2` veri deposunu oluşturup `ws` çalışma alanına kaydettirir ve değişkenine atar `adls2_dstore` . Bu veri deposu, depolama hesabındaki dosya sistemine erişir `tabular` `myadls2` .  

```python
# createn adls2 datastore without credentials
adls2_dstore = Datastore.register_azure_data_lake_gen2(workspace=ws, 
                                                       datastore_name='credentialless_adls2', 
                                                       filesystem='tabular', 
                                                       account_name='myadls2')
```

## <a name="use-data-in-storage"></a>Depolamadaki verileri kullanma

[Azure Machine Learning veri kümeleri](how-to-create-register-datasets.md) , Azure Machine Learning depolamayla verilerinizle etkileşimde bulunmak için önerilen yoldur. 

Veri kümeleri, eğitim gibi makine öğrenimi görevleri için verilerinizi geç tarafından değerlendirilen bir tüketilebilir nesneye paketleyin. Ayrıca, veri kümeleri ile, Azure Blob depolama ve Azure veri Lakes gibi Azure depolama hizmetlerinden herhangi bir biçimdeki dosyaları bir işlem hedefine [indirebilir veya bağlayabilirsiniz](how-to-train-with-datasets.md#mount-vs-download) .


**Kimlik tabanlı veri erişimi olan veri kümeleri oluşturmak için** aşağıdaki seçeneklere sahip olursunuz. Bu tür veri kümesi oluşturma, veri erişimi kimlik doğrulaması için Azure Active Directory belirtecinizi kullanır. 

*  Kimlik tabanlı veri erişimi de kullanan veri mağazalarından başvuru yolları. 
<br>Aşağıdaki örnekte, `blob_datastore` daha önce kimlik tabanlı veri erişimi kullanılarak oluşturulmuştur.   

    ```python
    blob_dataset = Dataset.Tabular.from_delimited_files(blob_datastore,'test.csv') 
    ```

* Veri deposu oluşturmayı atlayın ve doğrudan depolama URL 'lerinden veri kümeleri oluşturun. Şu anda bu işlevsellik yalnızca Azure Bloblarını destekler ve 1 ve 2. nesil Azure Data Lake Storage.

    ```python
    blob_dset = Dataset.File.from_files('https://myblob.blob.core.windows.net/may/keras-mnist-fashion/')
    ```

**Ancak, kimlik tabanlı veri erişimiyle oluşturulan bir veri kümesini kullanan bir eğitim işi gönderdiğinizde**, eğitim sürecinin yönetilen kimliği, Azure Active Directory belirteciniz yerine veri erişimi kimlik doğrulaması için kullanılır. Bu senaryo için, işlem yönetilen kimliğinin depolama hizmetinden en az **Depolama Blobu veri okuyucusu** rolüyle verildiğinden emin olun. [İşlem sırasında yönetilen kimlik ayarlamayı](how-to-create-attach-compute-cluster.md#managed-identity)öğrenin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure Machine Learning veri kümesi oluşturun](how-to-create-register-datasets.md).
* [Veri kümeleriyle eğitme](how-to-train-with-datasets.md).
* [Anahtar tabanlı veri erişimi ile bir veri deposu oluşturun](how-to-access-data.md).
