---
title: "Python: Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri | Microsoft Dokümanlar"
description: Veri Gölü Depolama Gen1 dosya sistemiyle çalışmak için Python SDK'yı nasıl kullanacağınızı öğrenin.
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 17b53c508f0c4c5ba8fa257fcdc692cdaa45d470
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76294227"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-python"></a>Python kullanarak Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Bu makalede, Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri gerçekleştirmek için Python SDK'yı nasıl kullanacağınızı öğreneceksiniz. Python kullanarak Veri Gölü Depolama Gen1'de hesap yönetimi işlemlerinin nasıl gerçekleştirileceklerine ilişkin talimatlar için [Python'u kullanarak Veri Gölü Depolama Gen1'deki Hesap yönetimi işlemlerine](data-lake-store-get-started-python.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Veri Gölü Depolama Gen1 hesabı.** [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](data-lake-store-get-started-portal.md)yönergeleri izleyin.

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Veri Gölü Depolama Gen1 ile çalışmak için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* Azure `azure-mgmt-datalake-store` Veri Gölü Depolama Gen1 hesap yönetimi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [azure-mgmt-datalake-store modülü başvurusuna](/python/api/azure-mgmt-datalake-store/)bakın.
* Azure `azure-datalake-store` Veri Gölü Depolama Gen1 dosya sistemi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [azure-datalake-store dosya sistemi modülü başvurusuna](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core/)bakın.

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Tercih ettiğiniz IDE’de yeni bir Python uygulaması (örneğin, **örneğim.py**) oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki satırları ekleyin

   ```
   ## Use this only for Azure AD service-to-service authentication
   from azure.common.credentials import ServicePrincipalCredentials

   ## Use this only for Azure AD end-user authentication
   from azure.common.credentials import UserPassCredentials

   ## Use this only for Azure AD multi-factor authentication
   from msrestazure.azure_active_directory import AADTokenCredentials

   ## Required for Azure Data Lake Storage Gen1 account management
   from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
   from azure.mgmt.datalake.store.models import DataLakeStoreAccount

   ## Required for Azure Data Lake Storage Gen1 filesystem management
   from azure.datalake.store import core, lib, multithread

   ## Common Azure imports
   from azure.mgmt.resource.resources import ResourceManagementClient
   from azure.mgmt.resource.resources.models import ResourceGroup

   ## Use these as needed for your application
   import logging, getpass, pprint, uuid, time
   ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="authentication"></a>Kimlik doğrulaması

Bu bölümde Azure AD ile gerçekleştirilen farklı kimlik doğrulama yöntemlerinden bahsedeceğiz. Şu seçenekleri kullanabilirsiniz:

* Uygulamanızın son kullanıcı kimlik doğrulaması için [Python kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-python.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması için [Python'u kullanarak Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-python.md)bakın.

## <a name="create-filesystem-client"></a>Dosya sistemi istemcisini oluşturma

Aşağıdaki parçacık ilk olarak Veri Gölü Depolama Gen1 hesap istemcisini oluşturur. Bir Veri Gölü Depolama Gen1 hesabı oluşturmak için istemci nesnesini kullanır. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(adlCreds, store_name=adlsAccountName)

## <a name="create-a-directory"></a>Dizin oluşturma

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Dosya indirme

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Bir dizini silme

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="next-steps"></a>Sonraki adımlar
* [Python kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Veri Gölü Depolama Gen1 Python (Filesystem) Başvurusu](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core)
* [Azure Veri Gölü Depolama Gen1 ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)
