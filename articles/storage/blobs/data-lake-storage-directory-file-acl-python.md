---
title: "& ACL 'Ler için Python SDK Azure Data Lake Storage 2."
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizin ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Python kullanın.
author: normesta
ms.service: storage
ms.date: 04/10/2020
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 06c263d751f6452e18765efb928ae6425ac50099
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84466043"
---
# <a name="use-python-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. dizinleri, dosyaları ve ACL 'Leri yönetmek için Python kullanma

Bu makalede hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında Dizin, dosya ve izinleri oluşturmak ve yönetmek için Python 'un nasıl kullanılacağı gösterilmektedir. 

[Paket (Python paket dizini)](https://pypi.org/project/azure-storage-file-datalake/)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)  |  [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0/azure.storage.filedatalake.html)  |  [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)  |  [Geri bildirimde](https://github.com/Azure/azure-sdk-for-python/issues) bulunun

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

[PIP](https://pypi.org/project/pip/)'Yi kullanarak Python için Azure Data Lake Storage istemci kitaplığı 'nı kullanın.

```
pip install azure-storage-file-datalake
```

Bu import deyimlerini, kod dosyanızın en üstüne ekleyin.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

```python
try:  
    global service_client
        
    service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
        "https", storage_account_name), credential=storage_account_key)
    
except Exception as e:
    print(e)
```
 
- `storage_account_name`Yer tutucu değerini depolama hesabınızın adıyla değiştirin.

- `storage_account_key`Yer tutucu değerini depolama hesabı erişim anahtarınızla değiştirin.

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Active Directory kullanarak bağlanma (AD)

Azure AD ile uygulamanızın kimliğini doğrulamak için [Python Için Azure kimlik istemci kitaplığını](https://pypi.org/project/azure-identity/) kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md).

```python
def initialize_storage_account_ad(storage_account_name, client_id, client_secret, tenant_id):
    
    try:  
        global service_client

        credential = ClientSecretCredential(tenant_id, client_id, client_secret)

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
            "https", storage_account_name), credential=credential)
    
    except Exception as e:
        print(e)
```

> [!NOTE]
> Daha fazla örnek için bkz. [Python Için Azure kimlik istemci kitaplığı](https://pypi.org/project/azure-identity/) belgeleri.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. **FileSystemDataLakeServiceClient. create_file_system** yöntemini çağırarak bir tane oluşturabilirsiniz.

Bu örnek adlı bir dosya sistemi oluşturur `my-file-system` .

```python
def create_file_system():
    try:
        global file_system_client

        file_system_client = service_client.create_file_system(file_system="my-file-system")
    
    except Exception as e:
        print(e) 
```


## <a name="create-a-directory"></a>Dizin oluşturma

**Filesystemclient. create_directory** yöntemini çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir dosya sistemine adlı bir dizin ekler `my-directory` . 

```python
def create_directory():
    try:
        file_system_client.create_directory("my-directory")
    
    except Exception as e:
     print(e) 
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Datalakedirectoryclient. rename_directory** yöntemini çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, bir alt dizini ada yeniden adlandırır `my-subdirectory-renamed` .

```python
def rename_directory():
    try:
       
       file_system_client = service_client.get_file_system_client(file_system="my-file-system")
       directory_client = file_system_client.get_directory_client("my-directory")
       
       new_dir_name = "my-directory-renamed"
       directory_client.rename_directory(rename_destination=directory_client.file_system_name + '/' + new_dir_name)

    except Exception as e:
     print(e) 
```

## <a name="delete-a-directory"></a>Bir dizini silme

**Datalakedirectoryclient. delete_directory** yöntemini çağırarak bir dizini silin.

Bu örnek adlı bir dizini siler `my-directory` .  

```python
def delete_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")
        directory_client = file_system_client.get_directory_client("my-directory")

        directory_client.delete_directory()
    except Exception as e:
     print(e) 
```

## <a name="manage-directory-permissions"></a>Dizin izinlerini yönetme

**Datalakedirectoryclient. get_access_control** yöntemini çağırarak ve **datalakedirectoryclient. SET_ACCESS_CONTROL** metodunu çağırarak ACL 'yi ayarlayarak bir dizinin ERIŞIM denetim listesini (ACL) alın.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

Bu örnek, adlı bir dizinin ACL 'sini alır ve ayarlar `my-directory` . Dize, `rwxr-xrw-` sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

```python
def manage_directory_permissions():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])
        
        new_dir_permissions = "rwxr-xrw-"
        
        directory_client.set_access_control(permissions=new_dir_permissions)
        
        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])
    
    except Exception as e:
     print(e) 
```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme 

İlk olarak, **Datalakefileclient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **Datalakefileclient. append_data** yöntemini çağırarak bir dosyayı karşıya yükleyin. **Datalakefileclient. flush_data** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek, adlı bir dizine bir metin dosyası yükler `my-directory` .   

```python
def upload_file_to_directory():
    try:

        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.create_file("uploaded-file.txt")
        local_file = open("C:\\file-to-upload.txt",'r')

        file_contents = local_file.read()

        file_client.append_data(data=file_contents, offset=0, length=len(file_contents))

        file_client.flush_data(len(file_contents))

    except Exception as e:
      print(e) 
```

> [!TIP]
> Dosya boyutunuz büyükse, kodunuzun **Datalakefileclient. append_data** metoduna birden çok çağrı yapması gerekir. Bunun yerine **Datalakefileclient. upload_data** yöntemini kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir çağrıda karşıya yükleyebilirsiniz. 

## <a name="upload-a-large-file-to-a-directory"></a>Bir dizine büyük bir dosya yükleme

**Datalakefileclient. append_data** yöntemine birden çok çağrı yapmak zorunda kalmadan büyük dosyaları karşıya yüklemek Için **datalakefileclient. upload_data** yöntemini kullanın.

```python
def upload_file_to_directory_bulk():
    try:

        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.get_file_client("uploaded-file.txt")

        local_file = open("C:\\file-to-upload.txt",'r')

        file_contents = local_file.read()

        file_client.upload_data(file_contents, overwrite=True)

    except Exception as e:
      print(e) 
```

## <a name="manage-file-permissions"></a>Dosya izinlerini yönetme

**Datalakefileclient. get_access_control** yöntemini çağırarak ve **datalakefileclient. SET_ACCESS_CONTROL** metodunu çağırarak ACL 'yi ayarlayarak bir dosyanın ERIŞIM denetim listesini (ACL) alın.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

Bu örnek, adlı bir dosyanın ACL 'sini alır ve ayarlar `my-file.txt` . Dize, `rwxr-xrw-` sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

```python
def manage_file_permissions():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.get_file_client("uploaded-file.txt")

        acl_props = file_client.get_access_control()
        
        print(acl_props['permissions'])
        
        new_file_permissions = "rwxr-xrw-"
        
        file_client.set_access_control(permissions=new_file_permissions)
        
        acl_props = file_client.get_access_control()
        
        print(acl_props['permissions'])

    except Exception as e:
     print(e) 
```

## <a name="download-from-a-directory"></a>Bir dizinden indir 

Yazmak için yerel bir dosya açın. Ardından, indirmek istediğiniz dosyayı temsil eden bir **Datalakefileclient** örneği oluşturun. Dosyadaki baytları okumak için **Datalakefileclient. read_file** çağırın ve ardından bu baytları yerel dosyaya yazın. 

```python
def download_file_from_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        local_file = open("C:\\file-to-download.txt",'wb')

        file_client = directory_client.get_file_client("uploaded-file.txt")

        download = file_client.download_file()

        downloaded_bytes = download.readall()

        local_file.write(downloaded_bytes)

        local_file.close()

    except Exception as e:
     print(e)
```
## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

**Filesystemclient. get_paths** yöntemini çağırarak ve sonra sonuçlar arasında sıralama yaparak dizin içeriğini listeleyin.

Bu örnek, adlı bir dizinde bulunan her bir alt dizin ve dosyanın yolunu yazdırır `my-directory` .

```python
def list_directory_contents():
    try:
        
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        paths = file_system_client.get_paths(path="my-directory")

        for path in paths:
            print(path.name + '\n')

    except Exception as e:
     print(e) 
```

## <a name="see-also"></a>Ayrıca bkz.

* [API başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0b5/index.html)
* [Paket (Python paket dizini)](https://pypi.org/project/azure-storage-file-datalake/)
* [Örnekler](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-python/issues)
