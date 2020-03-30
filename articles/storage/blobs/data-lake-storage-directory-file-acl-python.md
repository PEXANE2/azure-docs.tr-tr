---
title: Dosyalar & ACD'ler için Azure Veri Gölü Depolama Gen2 Python SDK
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında Python yönetme dizinlerini ve dosya ve dizin erişim denetim listelerini (ACL) kullanın.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: b43bfe5979b8232f1fee2f5c1c6873c9c62695a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061522"
---
# <a name="use-python-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Python'u kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkin leştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için Python'u nasıl kullanacağınızı gösterir. 

[Paket (Python Paket Endeksi)](https://pypi.org/project/azure-storage-file-datalake/) | [Örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples) | [API referans](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0/azure.storage.filedatalake.html) | [Gen1'den Gen2 haritaya](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | Geri bildirim[verin](https://github.com/Azure/azure-sdk-for-python/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

[Pip](https://pypi.org/project/pip/)kullanarak Python için Azure Veri Gölü Depolama istemci kitaplığını yükleyin.

```
pip install azure-storage-file-datalake --pre
```

Bu alma deyimlerini kod dosyanızın en üstüne ekleyin.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

## <a name="connect-to-the-account"></a>Hesaba bağlanın

Bu makaledeki parçacıkları kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı nı kullanarak bağlanma

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
 
- `storage_account_name` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

- `storage_account_key` Yer tutucu değerini depolama hesabı erişim anahtarınızla değiştirin.

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Etkin Dizin (AD) kullanarak bağlanın

Uygulamanızın kimliğini Azure AD ile doğrulamak [için Python için Azure kimlik istemcisi kitaplığını](https://pypi.org/project/azure-identity/) kullanabilirsiniz.

Bu örnek, bir istemci kimliği, istemci sırrı ve kiracı kimliği kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için, [istemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin'e](../common/storage-auth-aad-app.md)bakın.

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
> Daha fazla örnek için Python belgeleri [için Azure kimlik istemcisi kitaplığına](https://pypi.org/project/azure-identity/) bakın.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. **FileSystemDataLakeServiceClient.create_file_system** yöntemini arayarak bir tane oluşturabilirsiniz.

Bu örnek, .. `my-file-system`

```python
def create_file_system():
    try:
        global file_system_client

        file_system_client = service_client.create_file_system(file_system="my-file-system")
    
    except Exception as e:
        print(e) 
```


## <a name="create-a-directory"></a>Dizin oluşturma

**FileSystemClient.create_directory** yöntemini arayarak bir dizin başvurusu oluşturun.

Bu örnek, dosya `my-directory` sistemine adlandırılmış bir dizin ekler. 

```python
def create_directory():
    try:
        file_system_client.create_directory("my-directory")
    
    except Exception as e:
     print(e) 
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**DataLakeDirectoryClient.rename_directory** yöntemini arayarak bir dizini yeniden adlandırın veya taşıyın. İstenilen dizin bir parametre yolunu geçirin. 

Bu örnek, bir alt dizinin adını `my-subdirectory-renamed`yeniden adlandırır.

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

**DataLakeDirectoryClient.delete_directory** yöntemini arayarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler.  

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

**DataLakeDirectoryClient.get_access_control** yöntemini arayarak bir dizinin erişim denetim listesini (ACL) alın ve **DataLakeDirectoryClient.set_access_control** yöntemini arayarak ACL'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

Bu örnek, adlı `my-directory`bir dizinin ACL'sini alır ve ayarlar. Dize, `rwxr-xrw-` sahibi kullanıcıya okuma, yazma ve izin yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve diğerlerine okuma ve yazma izni verir.

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

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme 

İlk olarak, **DataLakeFileClient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **DataLakeFileClient.append_data** yöntemini arayarak dosya yükleyin. **DataLakeFileClient.flush_data** yöntemini arayarak yüklemeyi tamamladığından emin olun.

Bu örnek, bir metin dosyasını `my-directory`adlı bir dizine yükler.   

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
> Dosya boyutunuz büyükse, kodunuz **DataLakeFileClient.append_data** yöntemine birden çok arama yapmak zorunda kalacaktır. Bunun yerine **DataLakeFileClient.upload_data** yöntemini kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir aramada yükleyebilirsiniz. 

## <a name="upload-a-large-file-to-a-directory"></a>Büyük bir dosyayı dizine yükleme

**DataLakeFileClient.append_data** yöntemine birden fazla arama yapmak zorunda kalmadan büyük dosyaları yüklemek için **DataLakeFileClient.upload_data** yöntemini kullanın.

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

**DataLakeFileClient.get_access_control** yöntemini arayarak bir dosyanın erişim denetim listesini (ACL) alın ve **DataLakeFileClient.set_access_control** yöntemini arayarak ACL'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

Bu örnek, adlı `my-file.txt`bir dosyanın ACL'sini alır ve ayarlar. Dize, `rwxr-xrw-` sahibi kullanıcıya okuma, yazma ve izin yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve diğerlerine okuma ve yazma izni verir.

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

## <a name="download-from-a-directory"></a>Bir dizinden indirin 

Yazmak için yerel bir dosya açın. Ardından, indirmek istediğiniz dosyayı temsil eden bir **DataLakeFileClient** örneği oluşturun. **DataLakeFileClient.read_file** dosyadan bayt okumak için arayın ve sonra yerel dosyaya bu bayt yazın. 

```python
def download_file_from_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        local_file = open("C:\\file-to-download.txt",'wb')

        file_client = directory_client.get_file_client("uploaded-file.txt")

        downloaded_bytes = file_client.read_file()

        local_file.write(downloaded_bytes)

        local_file.close()

    except Exception as e:
     print(e)
```
## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

**FileSystemClient.get_paths** yöntemini arayarak dizin içeriğini listeleyip sonuçlara kaydedin.

Bu örnek, adlı `my-directory`bir dizinde bulunan her alt dizini ve dosyanın yolunu yazdırır.

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
* [Paket (Python Paket Endeksi)](https://pypi.org/project/azure-storage-file-datalake/)
* [Örnekler](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
* [Gen1 - Gen2 haritalama](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde](https://github.com/Azure/azure-sdk-for-python/issues)
