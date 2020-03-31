---
title: 'Quickstart: Azure Blob depolama kitaplığı v12 - Python'
description: Bu hızlı başlangıçta, Python için Blob depolama istemcisi kitaplığı sürüm 12'yi kullanarak Blob (nesne) depolama alanında bir kapsayıcı ve bir blob oluşturmayı öğrenirsiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 8daf7380e859cd2f9b5890c716f7b7d95e6c3fe4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061357"
---
# <a name="quickstart-manage-blobs-with-python-v12-sdk"></a>Quickstart: Python v12 SDK ile blobs yönetin

Bu hızlı başlangıçta, Python kullanarak lekeleri yönetmeyi öğrenirsiniz. Blobs, resimler, belgeler, akışlı ortam ve arşiv verileri de dahil olmak üzere büyük miktarda metin veya ikili veri tutabilen nesnelerdir. Blob'ları yükler, indirir ve listelersiniz ve kapsayıcılar oluşturup silebilirsiniz.

[API referans belgeleri](/python/api/azure-storage-blob) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-blob) | [Paketi (Python Paket Dizini)](https://pypi.org/project/azure-storage-blob/) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-python?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Azure Depolama hesabı. [Bir depolama hesabı oluşturun.](../common/storage-account-create.md)
- [Python](https://www.python.org/downloads/) 2.7, 3.5 veya üzeri.

> [!NOTE]
> Önceki SDK sürümüyle başlamak için [Quickstart: Python v2.1 SDK ile blob'ları yönet'](storage-quickstart-blobs-python-legacy.md)e bakın.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="setting-up"></a>Ayarlama

Bu bölüm, Python için Azure Blob depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*blob-quickstart-v12*adlı bir Python uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir blob-quickstart-v12
    ```

1. Yeni oluşturulan *blob-quickstart-v12* dizinine geçin.

    ```console
    cd blob-quickstart-v12
    ```

1. Yan *blob-quickstart-v12* dizin, *veri*adlı başka bir dizin oluşturun. Burada blob veri dosyaları oluşturulacak ve depolanır.

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>Paketi yükleyin

Uygulama dizinindeyken, komutu kullanarak Python paketi için Azure Blob `pip install` depolama istemcisi kitaplığını yükleyin.

```console
pip install azure-storage-blob
```

Bu komut, Python paketi için Azure Blob depolama istemcikitaplığını ve bağlı olduğu tüm kitaplıkları yükler. Bu durumda, bu Python için azure çekirdek kitaplığıdır.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod düzenleyicinizde yeni bir metin dosyası açma
1. Ekstre ekleme `import`
1. Temel özel durum işleme de dahil olmak üzere program için yapı oluşturma

    İşte kod:

    ```python
    import os, uuid
    from azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient

    try:
        print("Azure Blob storage v12 - Python quickstart sample")
        # Quick start code goes here
    except Exception as ex:
        print('Exception:')
        print(ex)
    ```

1. *Blob-quickstart-v12* dizininde yeni dosyayı *blob-quickstart-v12.py* olarak kaydedin.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Blob depolama, büyük miktarda yapılandırılmamış veri depolamak için optimize edilebiyi sunar. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanıma bağlı olmayan verilerdir. Blob depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kapsayıcı
* Konteynerde bir leke

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob depolama mimarisi diyagramı](./media/storage-blobs-introduction/blob1.png)

Bu kaynaklarla etkileşimkurmak için aşağıdaki Python sınıflarını kullanın:

* [BlobServiceClient](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient): `BlobServiceClient` Sınıf, Azure Depolama kaynaklarını ve blob kapsayıcılarını işlemenizi sağlar.
* [ContainerClient](/python/api/azure-storage-blob/azure.storage.blob.containerclient): `ContainerClient` Sınıf, Azure Depolama kapsayıcılarını ve bunların lekelerini işlemenizi sağlar.
* [BlobClient](/python/api/azure-storage-blob/azure.storage.blob.blobclient): `BlobClient` Sınıf, Azure Depolama lekelerini işlemenizi sağlar.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, Python için Azure Blob depolama istemcisi kitaplığıyla aşağıdakileri nasıl gerçekleştireceklerini gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kapsayıcı oluşturma](#create-a-container)
* [Blob'ları bir kapsayıcıya yükleme](#upload-blobs-to-a-container)
* [Blob’ları bir kapsayıcıda listeleme](#list-the-blobs-in-a-container)
* [Blob’ları indirme](#download-blobs)
* [Kapsayıcı silme](#delete-a-container)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, [depolama bağlantı dizesi bölümüyapılanınoluşturulan](#configure-your-storage-connection-string) ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu `try` bloğun içine ekleyin:

```python
# Retrieve the connection string for use with the application. The storage
# connection string is stored in an environment variable on the machine
# running the application called AZURE_STORAGE_CONNECTION_STRING. If the environment variable is
# created after the application is launched in a console or with Visual Studio,
# the shell or application needs to be closed and reloaded to take the
# environment variable into account.
connect_str = os.getenv('AZURE_STORAGE_CONNECTION_STRING')
```

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Yeni kapsayıcı için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kapsayıcı adına bir UUID değerini ekler.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

[from_connection_string](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient#from-connection-string-conn-str--credential-none----kwargs-) yöntemini arayarak [BlobServiceClient](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızdaki kapsayıcıyı oluşturmak için [create_container](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient#create-container-name--metadata-none--public-access-none----kwargs-) yöntemini arayın.

Bu kodu `try` bloğun sonuna ekleyin:

```python
# Create the BlobServiceClient object which will be used to create a container client
blob_service_client = BlobServiceClient.from_connection_string(connect_str)

# Create a unique name for the container
container_name = "quickstart" + str(uuid.uuid4())

# Create the container
container_client = blob_service_client.create_container(container_name)
```

### <a name="upload-blobs-to-a-container"></a>Blob'ları bir kapsayıcıya yükleme

Aşağıdaki kod parçacığı:

1. Yerel dizinde bir metin dosyası oluşturur.
1. [Bir kapsayıcı oluştur](#create-a-container) bölümünden [BlobServiceClient'daki](/python/api/azure-storage-blob/azure.storage.blob.blobserviceclient) [get_blob_client](/python/api/azure-storage-blob/azure.storage.blob.containerclient#get-blob-client-blob--snapshot-none-) yöntemini arayarak [BlobClient](/python/api/azure-storage-blob/azure.storage.blob.blobclient) nesnesine başvuru alır.
1. yerel metin dosyasını [upload_blob](/python/api/azure-storage-blob/azure.storage.blob.blobclient#upload-blob-data--blob-type--blobtype-blockblob---blockblob----length-none--metadata-none----kwargs-) yöntemini arayarak blob'a yükler.

Bu kodu `try` bloğun sonuna ekleyin:

```python
# Create a file in local data directory to upload and download
local_path = "./data"
local_file_name = "quickstart" + str(uuid.uuid4()) + ".txt"
upload_file_path = os.path.join(local_path, local_file_name)

# Write text to the file
file = open(upload_file_path, 'w')
file.write("Hello, World!")
file.close()

# Create a blob client using the local file name as the name for the blob
blob_client = blob_service_client.get_blob_client(container=container_name, blob=local_file_name)

print("\nUploading to Azure Storage as blob:\n\t" + local_file_name)

# Upload the created file
with open(upload_file_path, "rb") as data:
    blob_client.upload_blob(data)
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[list_blobs](/python/api/azure-storage-blob/azure.storage.blob.containerclient#list-blobs-name-starts-with-none--include-none----kwargs-) yöntemini arayarak konteynerdeki lekeleri listeleyin. Bu durumda, kapsayıcıya yalnızca bir blob eklenmiştir, bu nedenle listeleme işlemi yalnızca bir blob döndürür.

Bu kodu `try` bloğun sonuna ekleyin:

```python
print("\nListing blobs...")

# List the blobs in the container
blob_list = container_client.list_blobs()
for blob in blob_list:
    print("\t" + blob.name)
```

### <a name="download-blobs"></a>Blob’ları indirme

[download_blob](/python/api/azure-storage-blob/azure.storage.blob.blobclient#download-blob-offset-none--length-none----kwargs-) yöntemini arayarak daha önce oluşturulmuş blob'u indirin. Örnek kod, yerel dosya sisteminde her iki dosyayı görebilmeniz için dosya adına "DOWNLOAD" eki ekler.

Bu kodu `try` bloğun sonuna ekleyin:

```python
# Download the blob to a local file
# Add 'DOWNLOAD' before the .txt extension so you can see both files in the data directory
download_file_path = os.path.join(local_path, str.replace(local_file_name ,'.txt', 'DOWNLOAD.txt'))
print("\nDownloading blob to \n\t" + download_file_path)

with open(download_file_path, "wb") as download_file:
    download_file.write(blob_client.download_blob().readall())
```

### <a name="delete-a-container"></a>Kapsayıcı silme

Aşağıdaki kod, [delete_container](/python/api/azure-storage-blob/azure.storage.blob.containerclient#delete-container---kwargs-) yöntemini kullanarak tüm kapsayıcıyı kaldırarak uygulamanın oluşturduğu kaynakları temizler. İsterseniz yerel dosyaları da silebilirsiniz.

Uygulama, blob, kapsayıcı ve `input()` yerel dosyaları silmeden önce arayarak kullanıcı girişi için duraklar. Bu, kaynakların silinmeden önce doğru oluşturulduğunu doğrulamak için iyi bir şanstır.

Bu kodu `try` bloğun sonuna ekleyin:

```python
# Clean up
print("\nPress the Enter key to begin clean up")
input()

print("Deleting blob container...")
container_client.delete_container()

print("Deleting the local source and downloaded files...")
os.remove(upload_file_path)
os.remove(download_file_path)

print("Done")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama yerel klasörünüzde bir test dosyası oluşturur ve Blob depolama alanına yükler. Örnek daha sonra kapsayıcıdaki lekeleri listeler ve eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla karşıdan yükler.

*blob-quickstart-v12.py* dosyasını içeren dizine gidin ve `python` uygulamayı çalıştırmak için aşağıdaki komutu çalıştırın.

```console
python blob-quickstart-v12.py
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage v12 - Python quickstart sample

Uploading to Azure Storage as blob:
        quickstartcf275796-2188-4057-b6fb-038352e35038.txt

Listing blobs...
        quickstartcf275796-2188-4057-b6fb-038352e35038.txt

Downloading blob to
        ./data/quickstartcf275796-2188-4057-b6fb-038352e35038DOWNLOAD.txt

Press the Enter key to begin clean up

Deleting blob container...
Deleting the local source and downloaded files...
Done
```

Temizleme işlemine başlamadan önce, iki dosya için *veri* klasörünüzü denetleyin. Dosyaları açarak aynı olduklarını görebilirsiniz.

Dosyaları doğruladıktan sonra, test dosyalarını silmek ve demoyu bitirmek için **Enter** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Python'u kullanarak blob'ları yüklemeyi, indirmeyi ve listelemeyi öğrendiniz.

Blob depolama örnek uygulamalarını görmek için şunları yapmaya devam edin:

> [!div class="nextstepaction"]
> [Azure Blob depolama SDK v12 Python örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-blob/samples)

* Daha fazla bilgi için [Python için Azure SDK'ya](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/README.md)bakın.
* Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler [için Python Geliştiricileri için Azure'u](/azure/python/)ziyaret edin.
