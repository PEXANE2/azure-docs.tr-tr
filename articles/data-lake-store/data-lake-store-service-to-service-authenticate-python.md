---
title: 'Hizmete kimlik doğrulama: Azure Active Directory kullanarak Azure Veri Gölü Depolama Gen1 ile Python | Microsoft Dokümanlar'
description: Python'u kullanarak Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması nasıl elde edilenöğrenin
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
ms.openlocfilehash: 009aff2703829e6d30f93b3c8e3696724594f29b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260299"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Python kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Veri Gölü Depolama Gen1 ile hizmet-hizmet kimlik doğrulaması yapmak için Python SDK'yı nasıl kullanacağınızı öğreneceksiniz. Python kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması için Python [kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-python.md)bakın.


## <a name="prerequisites"></a>Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizin "Web" Uygulaması oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Hizmetten Hizmete kimlik doğrulamaadımlarını](data-lake-store-service-to-service-authenticate-using-active-directory.md)tamamlamış olmalısınız.

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Veri Gölü Depolama Gen1 ile çalışmak için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* Veri `azure-mgmt-datalake-store` Gölü Depolama Gen1 hesap yönetimi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen1 Yönetimi modülü başvurusuna](/python/api/azure-mgmt-datalake-store/)bakın.
* Veri `azure-datalake-store` Gölü Depolama Gen1 dosya sistemi işlemlerini içeren modül. Bu modül hakkında daha fazla bilgi için [azure-datalake-store Filesystem modülü başvurusuna](https://docs.microsoft.com/python/api/azure-datalake-store/azure.datalake.store.core/)bakın.

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
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Hesap yönetimi için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Veri Gölü Depolama Gen1 hesabı oluşturma, Veri Gölü Depolama Gen1 hesabını silme vb. gibi Veri Gölü Depolama Gen1'deki hesap yönetimi işlemleri için Azure AD ile kimlik doğrulaması yapmak için bu snippet'i kullanın. Aşağıdaki parçacık, mevcut bir Azure AD "Web Uygulaması" uygulamasının bir uygulama / servis ilkesi için istemci sırrını kullanarak uygulamanızın kimliğini etkileşimli olmayan bir şekilde doğrulamak için kullanılabilir.

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    RESOURCE = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_credentials(RESOURCE, client_id, client_secret)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource=RESOURCE)

## <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Dosya sistemi işlemleri için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Klasör oluşturma, dosya yükleme vb. gibi Veri Gölü Depolama Gen1'deki dosya sistemi işlemleri için Azure AD ile kimlik doğrulaması yapmak için aşağıdaki snippet'i kullanın. Aşağıdaki parçacık, bir uygulama / servis sorumlusu için istemci sırrını kullanarak uygulamanızı etkileşimli olmayan bir şekilde doğrulamak için kullanılabilir. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    tenant = '<TENANT>'
    RESOURCE = 'https://datalake.azure.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    adlCreds = lib.auth(tenant_id = tenant,
                    client_secret = client_secret,
                    client_id = client_id,
                    resource = RESOURCE)

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Storage Gen1 such as create a Data Lake Storage Gen1 account, delete a Data Lake Storage Gen1 account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    resource_uri = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_cert = '<CLIENT_CERT>'
    client_cert_thumbprint = '<CLIENT_CERT_THUMBPRINT>'

    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_certificate(resource_uri, client_id, client_cert, client_cert_thumbprint)
    credentials = AADTokenCredentials(mgmt_token, client_id) -->

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Python kullanarak Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için hizmet-servis kimlik doğrulaması nasıl kullanılacağını öğrendiniz. Şimdi, Veri Gölü Depolama Gen1 ile çalışmak için Python'un nasıl kullanılacağı hakkında konuşulan aşağıdaki makalelere bakabilirsiniz.

* [Python kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-python.md)
* [Python kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-python.md)


