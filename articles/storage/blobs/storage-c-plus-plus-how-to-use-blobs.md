---
title: C++'dan nesne (Blob) depolama nasıl kullanılır - Azure | Microsoft Dokümanlar
description: Azure Blob (nesne) depolama alanıyla yapılandırılmamış verileri bulutta depolayın.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/21/2018
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 0a9015e33f5456efeac7f7c887995ac4a69f0259
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941801"
---
# <a name="how-to-use-blob-storage-from-c"></a>C++'dan Blob depolama nasıl kullanılır?

Bu kılavuz, Azure Blob depolama alanını kullanarak sık karşılaşılan senaryoların nasıl gerçekleştirilini gösterir. Örnekler, blob'ların nasıl yüklendiğini, listelendiğini, indirilebildiğini ve silineceklerini gösterir. Örnekler C++ dilinde yazılmıştır ve [C++ için Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)’nı kullanır.   

Blob depolama hakkında daha fazla bilgi edinmek için Azure [Blob depolamasına Giriş 'e](storage-blobs-introduction.md)bakın.

> [!NOTE]
> Bu kılavuz C++ için Azure Depolama İstemci Kitaplığı sürüm 1.0.0 ve üzerini hedefler. Microsoft, [NuGet](https://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp)üzerinden kullanılabilen C++için Depolama İstemci Kitaplığı'nın en son sürümünü kullanmanızı önerir.

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, c++ uygulaması içinde çalıştırılabilen depolama özelliklerini kullanırsınız.  

Bunu yapmak için, C++ için Azure Depolama İstemci Kitaplığı’nı yüklemeniz ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.   

C++ için Azure Depolama İstemci Kitaplığı’nı aşağıdaki yöntemleri kullanarak yükleyebilirsiniz:

* **Linux:** [C++ README için Azure Depolama İstemci Kitaplığı'nda verilen yönergeleri izleyin: Linux sayfasından başlarken.](https://github.com/Azure/azure-storage-cpp#getting-started-on-linux)
* **Windows:** Windows'da, bağımlılık yöneticisi olarak [vcpkg](https://github.com/microsoft/vcpkg) kullanın. Vcpkg'ı başlatmak için [hızlı başlat'ı](https://github.com/microsoft/vcpkg#quick-start) izleyin. Ardından, kitaplığı yüklemek için aşağıdaki komutu kullanın:

```powershell
.\vcpkg.exe install azure-storage-cpp
```

[README](https://github.com/Azure/azure-storage-cpp#download--install) dosyasında kaynak kodunu niçin oluşturup NuGet'e dışa aktarabileceğinize yönelik bir kılavuz bulabilirsiniz.

## <a name="configure-your-application-to-access-blob-storage"></a>Uygulamanızı Blob depolama alanına erişmek için yapılandırın
Blob'lara erişmek için Azure depolama API'lerini kullanmak istediğiniz C++ dosyasının üst bölümüne aşağıdaki ifadeleri ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
#include <cpprest/filestream.h>  
#include <cpprest/containerstream.h> 
```

## <a name="setup-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi kurma
Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. İstemci uygulamasında çalışırken, muhasebe hesabınızın adını ve *Hesap Adı* ve *Hesap Anahtarı* değerleri için [Azure Portalı'nda](https://portal.azure.com) listelenen depolama hesabının depolama erişim anahtarını kullanarak depolama bağlantı dizesini aşağıdaki biçimde sağlamanız gerekir. Depolama hesapları ve erişim anahtarları hakkında bilgi için [Azure Depolama Hesapları Hakkında'ya](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın. Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Uygulamanızı yerel Windows bilgisayarınızda sınamak için Azure [SDK](https://azure.microsoft.com/downloads/)ile yüklü olan Microsoft Azure [depolama emülatörü](../storage-use-emulator.md) kullanabilirsiniz. Depolama emülatörü, yerel geliştirme makinenizde Azure'da bulunan Blob, Queue ve Tablo hizmetlerini taklit eden bir yardımcı programdır. Aşağıdaki örnekte bağlantı dizesini yerel depolama öykünücünüzde tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure depolama emülatörübaşlatmak için **Başlat** düğmesini seçin veya **Windows** tuşuna basın. Azure **Depolama Emülatörü**yazmaya başlayın ve uygulamalar listesinden **Microsoft Azure Depolama Emülatörü'nü** seçin.  

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.  

## <a name="retrieve-your-storage-account"></a>Depolama hesabınızı alın
Depolama Hesabı bilgilerinizi temsil etmek için **cloud_storage_account** sınıfını kullanabilirsiniz. Depolama bağlantı dizesinden depolama hesabı bilgilerini almak için **parse** yöntemini kullanabilirsiniz.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Ardından, Blob depolama alanı içinde depolanan kapsayıcıları ve lekeleri temsil eden nesneleri almanıza olanak tanıyan **cloud_blob_client** sınıfına başvuru alın. Aşağıdaki kod, yukarıda aldığımız depolama hesabı nesnesini kullanarak **cloud_blob_client** bir nesne oluşturur:  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Nasıl yapılsın: Kapsayıcı oluşturma
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Bu örnek, zaten yoksa, nasıl bir kapsayıcı oluşturulacağını gösterir:  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create the container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

Varsayılan olarak, yeni kapsayıcı özeldir ve bu kapsayıcıdan blobs indirmek için depolama erişim anahtarınızı belirtmeniz gerekir. Kapsayıcıdaki dosyaları (blobs) herkesin kullanımına açmak istiyorsanız, kapsayıcıyı aşağıdaki kodu kullanarak herkese açık olacak şekilde ayarlayabilirsiniz:  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Internet'teki herkes ortak bir kapsayıcıda lekeler görebilir, ancak yalnızca uygun erişim anahtarına sahipseniz bunları değiştirebilir veya silebilirsiniz.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Nasıl yapilir: Bir blob'u bir kaba yükleme
Azure Blob depolama bloğu lekeleri ve sayfa lekeleri destekler. Çoğu durumda kullanılması önerilen blob türü blok blobudur.  

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve blok blob başvurusu almak için kullanın. Bir blob başvurusu yaptıktan sonra, **upload_from_stream** yöntemini arayarak herhangi bir veri akışını yükleyebilirsiniz. Bu işlemle, eğer önceden oluşturulmadıysa bir blob oluşturulacaktır, aksi takdirde üzerine yazılacaktır. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite the "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference to a blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

Alternatif olarak, bir dosyayı bir blok blob'una yüklemek için **upload_from_file** yöntemini kullanabilirsiniz.

## <a name="how-to-list-the-blobs-in-a-container"></a>Nasıl yapılsın: Lekeleri bir kapta listeleyin
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Daha sonra içindeki lekeleri ve/veya dizinleri almak için kabın **list_blobs** yöntemini kullanabilirsiniz. İade edilen bir **list_blob_item**için zengin özellik ve yöntem kümesine erişmek için, **cloud_blob** bir nesne almak için **list_blob_item.as_blob** yöntemini veya cloud_blob_directory bir nesne almak için **list_blob.as_directory** yöntemini aramanız gerekir. Aşağıdaki kod, **örnek imletimkapsayıcısındaki** her maddenin URI'sinin nasıl alınıp çıkarı yapılacağını gösterir:

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

Listeleme işlemleri hakkında daha fazla bilgi için [bkz.](../storage-c-plus-plus-enumeration.md)

## <a name="how-to-download-blobs"></a>Nasıl yapılsın: Damlaları indirin
Blobs indirmek için, ilk blob başvuru almak ve sonra **download_to_stream** yöntemini arayın. Aşağıdaki örnek, blob içeriğini daha sonra yerel bir dosyaya devam edebilirsiniz bir akış nesnesine aktarmak için **download_to_stream** yöntemini kullanır.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents to a file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

Alternatif olarak, bir blob içeriğini bir dosyaya indirmek için **download_to_file** yöntemini kullanabilirsiniz.
Buna ek olarak, bir metin dizesi olarak bir blob içeriğini indirmek için **download_text** yöntemini de kullanabilirsiniz.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download the contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a>Nasıl yapılsın: Lekeleri silme
Bir blob silmek için, ilk bir blob referans almak ve sonra üzerinde **delete_blob** yöntemi ni arayın.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete the blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a>Sonraki adımlar
Blob depolamanın temellerini öğrendiğiniz için Azure Depolama hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin.  

* [C++’tan Kuyruk Depolama kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [C++'dan Tablo Depolama nasıl kullanılır?](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Azure Depolama Kaynaklarını C++'da listele](../storage-c-plus-plus-enumeration.md)
* [C++ Başvurusu için Depolama İstemci Kitaplığı](https://azure.github.io/azure-storage-cpp)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](../storage-use-azcopy.md)

