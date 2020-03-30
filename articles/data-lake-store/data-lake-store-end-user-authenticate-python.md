---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory kullanarak Azure Veri Gölü Depolama Gen1 ile Python | Microsoft Dokümanlar'
description: Python ile Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması nasıl elde edilemeyi öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c69f6c1f587285c5c52280c4c49008764d5b20d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265603"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Python kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’sını kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması yapmak için Python SDK'yı nasıl kullanacağınızı öğreneceksiniz. Son kullanıcı kimlik doğrulaması iki kategoriye ayrılabilir:

* Çok faktörlü kimlik doğrulamaolmadan son kullanıcı kimlik doğrulaması
* Çok faktörlü kimlik doğrulama ile son kullanıcı kimlik doğrulaması

Bu seçeneklerin her ikisi de bu makalede ele alınmıştır. Python kullanarak Data Lake Storage Gen1 ile servise hizmet kimlik doğrulaması için [Python'u kullanarak Data Lake Storage Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-python.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizini "Yerel" Uygulama Oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Son kullanıcı kimlik doğrulamaadımlarını](data-lake-store-end-user-authenticate-using-active-directory.md)tamamlamış olmalısınız.

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Veri Gölü Depolama Gen1 ile çalışmak için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* Azure `azure-mgmt-datalake-store` Veri Gölü Depolama Gen1 hesap yönetimi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen1 Yönetimi modülü başvurusuna](/python/api/azure-mgmt-datalake-store/)bakın.
* Azure `azure-datalake-store` Veri Gölü Depolama Gen1 dosya sistemi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [azure-datalake-store Filesystem modülü başvurusuna](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core/)bakın.

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Seçtiğiniz IDE'de, **örneğin, mysample.py**yeni bir Python uygulaması oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki kod parçacığını ekleyin

    ```
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="end-user-authentication-with-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama ile son kullanıcı kimlik doğrulaması

### <a name="for-account-management"></a>Hesap yönetimi için

Veri Gölü Depolama Gen1 hesabındahesap yönetimi işlemleri için Azure AD ile kimlik doğrulaması yapmak için aşağıdaki snippet'i kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Varolan bir Azure AD **yerel** uygulaması için aşağıdaki değerleri sağlayın.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource = RESOURCE)

### <a name="for-filesystem-operations"></a>Dosya sistemi işlemleri için

Veri Gölü Depolama Gen1 hesabındaki dosya sistemi işlemleri için Azure AD ile kimlik doğrulaması yapmak için bunu kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Varolan bir Azure AD **yerel** uygulaması için aşağıdaki değerleri sağlayın.

    adlCreds = lib.auth(tenant_id='FILL-IN-HERE', resource = 'https://datalake.azure.net/')

## <a name="end-user-authentication-without-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamaolmadan son kullanıcı kimlik doğrulaması

Bu amortismana lı. Daha fazla bilgi için [Python SDK'yı kullanarak Azure Kimlik Doğrulaması'na](/azure/python/python-sdk-azure-authenticate)bakın.
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Python kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için son kullanıcı kimlik doğrulamasını nasıl kullanacağınızı öğrendiniz. Artık Azure Veri Gölü Depolama Gen1 ile çalışmak için Python'un nasıl kullanılacağı hakkında aşağıdaki makalelere bakabilirsiniz.

* [Python kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-python.md)
* [Python kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-python.md)

