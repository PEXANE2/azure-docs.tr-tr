---
title: Python ile Azure Dosyaları için geliştirin | Microsoft Dokümanlar
description: Dosya verilerini depolamak için Azure Dosyaları kullanan Python uygulamalarını ve hizmetlerini nasıl geliştireceklerini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 139e3009722761172b7bbd57805a7f5b07e55fc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68699388"
---
# <a name="develop-for-azure-files-with-python"></a>Python ile Azure Dosyaları için geliştirme
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

Bu öğretici, dosya verilerini depolamak için Azure Dosyaları kullanan uygulamalar veya hizmetler geliştirmek için Python'u kullanmanın temellerini gösterecektir. Bu öğreticide, basit bir konsol uygulaması oluşturacağız ve Python ve Azure Dosyaları ile temel işlemlerin nasıl gerçekleştirileceğini göstereceğiz:

* Azure dosya paylaşımları oluşturma
* Dizinoluşturma
* Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin
* Dosyayükleme, indirme ve silme

> [!Note]  
> Azure Dosyalarına Kobİ üzerinden erişilebildiği için, standart Python G/Ç sınıfları ve işlevlerini kullanarak Azure dosya paylaşımına erişen basit uygulamalar yazmak mümkündür. Bu makalede, Azure Dosyaları ile konuşmak için Azure Files REST [API'sini](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) kullanan Azure Depolama Python SDK'sını kullanan uygulamaların nasıl yazıldığı açıklanmaktadır.

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Python için Azure Depolama SDK'sını İndirin ve Yükleyin

[Python için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-python) Python 2.7, 3.3, 3.4, 3.5 veya 3.6 gerektirir.
 
## <a name="install-via-pypi"></a>PyPi üzerinden yükleyin

Python Paket Dizini (PyPI) üzerinden yüklemek için şunları yazın:

```bash
pip install azure-storage-file
```

> [!NOTE]
> Python sürümü 0.36 veya daha önceki Azure Depolama SDK'sından yükseltme alıyorsanız, `pip uninstall azure-storage` en son paketi yüklemeden önce eski SDK'yı kaldırın.

Alternatif yükleme yöntemleri için [GitHub'daki Python için Azure Depolama SDK'sını](https://github.com/Azure/azure-storage-python/)ziyaret edin.

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme
f Python'un Azure Dosyaları yla nasıl kullanılacağını gösteren bir örnek uygulamayı görüntülemek ve çalıştırmak için [Bkz. Azure Depolama: Python'da Azure Dosyalarıyla Başlarken](https://github.com/Azure-Samples/storage-file-python-getting-started). 

Örnek uygulamayı çalıştırmak için hem paketleri `azure-storage-file` yüklediğinizden `azure-storage-common` emin olun.

## <a name="set-up-your-application-to-use-azure-files"></a>Uygulamanızı Azure Dosyalarını kullanacak şekilde ayarlama
Azure Depolama'ya programlı olarak erişmek istediğiniz python kaynak dosyasının en üstüne aşağıdakileri ekleyin.

```python
from azure.storage.file import FileService
```

## <a name="set-up-a-connection-to-azure-files"></a>Azure Dosyaları'na bağlantı kurma 
Nesne, `FileService` paylaşımlar, dizinler ve dosyalarla çalışmanızı sağlar. Aşağıdaki kod, depolama `FileService` hesabı adı ve hesap anahtarını kullanarak bir nesne oluşturur. `<myaccount>` ve `<mykey>` ifadelerini hesap adınız ve anahtarınız ile değiştirin.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Aşağıdaki kod örneğinde, varsa `FileService` paylaşımı oluşturmak için bir nesne kullanabilirsiniz.

```python
file_service.create_share('myshare')
```

## <a name="create-a-directory"></a>Dizin oluşturma
Ayrıca, dosyaları kök dizininde olması yerine alt dizinlerin içine koyarak depolama alanını da düzenleyebilirsiniz. Azure Dosyaları, hesabınızın izin verdiği kadar dizin oluşturmanıza olanak tanır. Aşağıdaki kod kök dizinin altında **sampledir** adlı bir alt dizin oluşturur.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin
Dosyaları ve dizinleri bir paylaşımda listelemek için **liste\_dizinlerini ve\_\_dosya yöntemini** kullanın. Bu yöntem bir oluşturucu döndürür. Aşağıdaki kod, konsola bir paylaşımda her dosyanın **adını** ve dizininadını çıkartır.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Azure dosya paylaşımı en azından dosyaların bulunabileceği bir kök dizini içerir. Bu bölümde, yerel depolama dan bir paylaşımın kök dizinine dosya yüklemeyi öğreneceksiniz.

Bir dosya oluşturmak ve veri `create_file_from_path`yüklemek `create_file_from_stream` `create_file_from_bytes` için `create_file_from_text` , veya yöntemleri kullanın. Bunlar, verilerin boyutu 64 MB'ı aştığında gerekli öbeklemi gerçekleştiren üst düzey yöntemlerdir.

`create_file_from_path`belirtilen yoldan bir dosyanın içeriğini yükler `create_file_from_stream` ve içeriği zaten açılmış bir dosyadan/akıştan yükler. `create_file_from_bytes`bir dizi bayt yükler ve `create_file_from_text` belirtilen kodlamayı kullanarak belirtilen metin değerini yükler (varsayılan olarak UTF-8'e).

Aşağıdaki örnek, **sunset.png** dosyasının içeriğini **dosya dosyasına** yükler.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None,  # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

## <a name="download-a-file"></a>Dosya indirme
Bir dosyadan veri indirmek `get_file_to_path` `get_file_to_stream`için `get_file_to_bytes`, `get_file_to_text`, , veya . Bunlar, verilerin boyutu 64 MB'ı aştığında gerekli öbeklemi gerçekleştiren üst düzey yöntemlerdir.

Aşağıdaki örnek, `get_file_to_path` **myfile** dosyasının içeriğini indirmek ve **out-sunset.png** dosyasına saklamak için kullanarak gösterir.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Dosyayı silme
Son olarak, bir dosyayı silmek için. `delete_file`

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="create-share-snapshot"></a>Paylaşım anlık görüntüsü oluşturma
Tüm dosya paylaşımınızın zaman içinde bir nokta kopyasını oluşturabilirsiniz.

```python
snapshot = file_service.snapshot_share(share_name)
snapshot_id = snapshot.snapshot
```

**Meta verilerle paylaşım anlık görüntüsü oluşturma**

```python
metadata = {"foo": "bar"}
snapshot = file_service.snapshot_share(share_name, metadata=metadata)
```

## <a name="list-shares-and-snapshots"></a>Paylaşımları ve anlık görüntüleri listele 
Belirli bir paylaşım için tüm anlık görüntüleri listeleyebilirsiniz.

```python
shares = list(file_service.list_shares(include_snapshots=True))
```

## <a name="browse-share-snapshot"></a>Paylaşım anlık görüntüsüne göz atın
Zaman içinde bu noktadan dosya ve dizinleri almak için her paylaşım anlık görüntü içeriğine göz atabilirsiniz.

```python
directories_and_files = list(
    file_service.list_directories_and_files(share_name, snapshot=snapshot_id))
```

## <a name="get-file-from-share-snapshot"></a>Paylaşım anlık görüntüsünden dosya alma
Geri yükleme senaryonuz için bir paylaşım anlık görüntüsünden dosya indirebilirsiniz.

```python
with open(FILE_PATH, 'wb') as stream:
    file = file_service.get_file_to_stream(
        share_name, directory_name, file_name, stream, snapshot=snapshot_id)
```

## <a name="delete-a-single-share-snapshot"></a>Tek bir paylaşım anlık görüntüsünü silme  
Tek bir paylaşım anlık görüntüsünü silebilirsiniz.

```python
file_service.delete_share(share_name, snapshot=snapshot_id)
```

## <a name="delete-share-when-share-snapshots-exist"></a>Paylaşım anlık görüntüleri olduğunda paylaşımı silme
Anlık görüntü içeren bir paylaşım, tüm anlık görüntüler önce silinmedikçe silinemez.

```python
file_service.delete_share(share_name, delete_snapshots=DeleteSnapshot.Include)
```

## <a name="next-steps"></a>Sonraki adımlar
Artık Python ile Azure Dosyalarını nasıl manipüle edindiğinizi öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355)
* [Python için Microsoft Azure Depolama SDK](https://github.com/Azure/azure-storage-python)
