---
title: Azure Data Lake Storage 2. & ACL 'Leri için Python kullanma (Önizleme)
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizin ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Python kullanın.
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: a65e8b2c5fbcdd4f6df0f8f39b24fa97b8deeca3
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534425"
---
# <a name="use-python-for-files--acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage 2. & ACL 'Leri için Python kullanma (Önizleme)

Bu makalede hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında Dizin, dosya ve izinleri oluşturmak ve yönetmek için Python 'un nasıl kullanılacağı gösterilmektedir. 

> [!IMPORTANT]
> Python için Azure Data Lake Storage istemci kitaplığı şu anda genel önizlemededir.

[Paket (Python paket dizini)](https://pypi.org/project/azure-storage-file-datalake/) | [örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples) | [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0b5/index.html) | [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [geri bildirim verme](https://github.com/Azure/azure-sdk-for-python/issues)

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

[PIP](https://pypi.org/project/pip/)'Yi kullanarak Python için Azure Data Lake Storage istemci kitaplığı 'nı kullanın.

```
pip install azure-storage-file-datalake --pre
```

Bu import deyimlerini, kod dosyanızın en üstüne ekleyin.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. Bir tane almanın en kolay yolu, hesap anahtarı kullanmaktır. 

Bu örnek, depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmak için bir hesap anahtarı kullanır. 

```python
try:  
    global service_client
        
    service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
        "https", storage_account_name), credential=storage_account_key)
    
except Exception as e:
    print(e)
```
 
- `storage_account_name` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

- `storage-account-key` yer tutucu değerini depolama hesabı erişim anahtarınızla değiştirin.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. **FileSystemDataLakeServiceClient. create_file_system** yöntemini çağırarak bir tane oluşturabilirsiniz.

Bu örnek `my-file-system`adlı bir dosya sistemi oluşturur.

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

Bu örnek, `my-directory` adlı bir dizini bir dosya sistemine ekler. 

```python
def create_directory():
    try:
        file_system_client.create_directory("my-directory")
    
    except Exception as e:
     print(e) 
```

## <a name="rename-or-move-a-directory"></a>Dizini yeniden adlandırma veya taşıma

**Datalakedirectoryclient. rename_directory** yöntemini çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, `my-subdirectory-renamed`adı için bir alt dizini yeniden adlandırır.

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

Bu örnek, `my-directory`adlı bir dizini siler.  

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

Bu örnek `my-directory`adlı bir dizinin ACL 'sini alır ve ayarlar. `rwxr-xrw-` dize, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

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

Bu örnek, `my-directory`adlı bir dizine bir metin dosyası yükler.   

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

## <a name="manage-file-permissions"></a>Dosya izinlerini yönetme

**Datalakefileclient. get_access_control** yöntemini çağırarak ve **datalakefileclient. SET_ACCESS_CONTROL** metodunu çağırarak ACL 'yi ayarlayarak bir dosyanın ERIŞIM denetim listesini (ACL) alın.

Bu örnek `my-file.txt`adlı bir dosyanın ACL 'sini alır ve ayarlar. `rwxr-xrw-` dize, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

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

        downloaded_bytes = file_client.read_file()

        local_file.write(downloaded_bytes)

        local_file.close()

    except Exception as e:
     print(e)
```
## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

**Filesystemclient. get_paths** yöntemini çağırarak ve sonra sonuçlar arasında sıralama yaparak dizin içeriğini listeleyin.

Bu örnek, `my-directory`adlı bir dizinde bulunan her bir alt dizinin ve dosyanın yolunu yazdırır.

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
