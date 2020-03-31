---
title: "Python: Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri | Microsoft Dokümanlar"
description: Azure Veri Gölü Depolama Gen1 hesap yönetimi işlemleriyle çalışmak için Python SDK'yı nasıl kullanacağınızı öğrenin.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c71136ecb57fac460514b5f4815ba19cc22d86cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76290623"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-python"></a>Python kullanarak Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Veri Gölü Depolama Gen1 hesabı oluşturma, Veri Gölü Depolama Gen1 hesaplarını listele gibi temel hesap yönetimi işlemlerini gerçekleştirmek için Azure Veri Gölü Depolama Gen1 için Python SDK'yı nasıl kullanacağınızı öğrenin. Python kullanarak Veri Gölü Depolama Gen1'de dosya sistemi işlemlerinin nasıl gerçekleştirileceklerine ilişkin talimatlar için Python [kullanarak Veri Gölü Depolama Gen1'deki Dosya Sistemi işlemlerine](data-lake-store-data-operations-python.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure kaynak grubu.** Talimatlar için bkz. [Azure kaynak grubu oluşturma](../azure-resource-manager/management/manage-resource-groups-portal.md).

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Veri Gölü Depolama Gen1 ile çalışmak için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* Azure `azure-mgmt-datalake-store` Veri Gölü Depolama Gen1 hesap yönetimi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen1 Yönetimi modülü başvurusuna](/python/api/azure-mgmt-datalake-store/)bakın.
* Azure `azure-datalake-store` Veri Gölü Depolama Gen1 dosya sistemi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [azure-datalake-store filesystem modülü başvurusuna](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core/)bakın.

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Tercih ettiğiniz IDE’de yeni bir Python uygulaması (örneğin, **örneğim.py**) oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki kod parçacığını ekleyin

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
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

## <a name="create-client-and-data-lake-storage-gen1-account"></a>İstemci ve Veri Gölü Depolama Gen1 hesabı oluşturma

Aşağıdaki parçacık ilk olarak Veri Gölü Depolama Gen1 hesap istemcisini oluşturur. Bir Veri Gölü Depolama Gen1 hesabı oluşturmak için istemci nesnesini kullanır. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create Data Lake Storage Gen1 account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Storage Gen1 account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-storage-gen1-accounts"></a>Veri Gölü Depolama Gen1 hesaplarını listele

    ## List the existing Data Lake Storage Gen1 accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabını silme

    ## Delete an existing Data Lake Storage Gen1 account
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Sonraki adımlar
* [Python kullanarak Veri Gölü Depolama Gen1 dosya sistemi işlemleri](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [azure-datalake-store Python (Filesystem) başvurusu](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core)
* [Azure Veri Gölü Depolama Gen1 ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)
