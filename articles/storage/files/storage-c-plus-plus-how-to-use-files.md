---
title: C++ ile Azure Dosyaları için geliştirin | Microsoft Dokümanlar
description: Dosya verilerini depolamak için Azure Dosyaları kullanan C++ uygulamalarını ve hizmetlerini nasıl geliştireceklerini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 97af40bd1f57acb5b26d3b6216984dfb8e3a5181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68699793"
---
# <a name="develop-for-azure-files-with-c"></a>C++ ile Azure Dosyaları için geliştirin

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Bu öğretici hakkında

Bu eğitimde, Azure Dosyaları'nda temel işlemleri nasıl gerçekleştireceğinizi öğreneceksiniz. C++'da yazılan örnekler aracılığıyla, paylaşımve dizin oluşturmayı, dosyaları yüklemeyi, listelemeyi ve silmeyi öğreneceksiniz. Azure Dosyaları'nda yeniyseniz, aşağıdaki bölümlerdeki kavramları gözden geçirmek, örnekleri anlamada yararlı olacaktır.

* Azure dosya paylaşımlarını oluşturma ve silme
* Dizin oluşturma ve silme
* Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin
* Dosyayükleme, indirme ve silme
* Azure dosya paylaşımı için kotayı (maksimum boyut) ayarlama
* Paylaşımda tanımlı bir paylaşılan erişim ilkesi kullanan bir dosya için paylaşılan erişim imzası (SAS anahtarı) oluşturma.

> [!Note]  
> Azure Dosyalarına Kobİ üzerinden erişilebildiği için, standart C++ G/Ç sınıfları ve işlevlerini kullanarak Azure dosya paylaşımına erişen basit uygulamalar yazmak mümkündür. Bu makalede, Azure Dosyaları ile konuşmak için [Dosya REST API'sini](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) kullanan Azure Depolama C++ SDK'sını kullanan uygulamaların nasıl yazıldığı açıklanmaktadır.

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma

Örnekleri oluşturmak için C++için Azure Depolama İstemci Kitaplığı 2.4.0'ı yüklemeniz gerekir. Bir Azure depolama hesabı da oluşturmuş olmalısınız.

C++için Azure Depolama İstemci 2.4.0'ı yüklemek için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* **Linux:** [C++ README için Azure Depolama İstemci Kitaplığı'nda](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) verilen yönergeleri izleyin.
* **Windows:** Visual Studio'da ** &gt; Araçlar NuGet &gt; Paket Yöneticisi Paket Yöneticisi Konsolu'nu**tıklatın. [NuGet Package Manager konsoluna](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) aşağıdaki komutu yazın ve **ENTER**tuşuna basın.
  

```powershell
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-azure-files"></a>Uygulamanızı Azure Dosyalarını kullanacak şekilde ayarlama

Azure Dosyalarını işlemek istediğiniz C++ kaynak dosyasının üst bölümüne aşağıdaki ifadeleri ekleyin:

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ayarlama

Dosya depolama alanını kullanmak için Azure depolama hesabınıza bağlanmanız gerekir. İlk adım, depolama hesabınıza bağlanmak için kullanacağımız bir bağlantı dizesini yapılandırmak olacaktır. Bunu yapmak için statik bir değişken tanımlayalım.

```cpp
// Define the connection-string with your values.
const utility::string_t
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a>Azure depolama hesabına bağlanma

Depolama Hesabı bilgilerinizi temsil etmek için **cloud_storage_account** sınıfını kullanabilirsiniz. Depolama bağlantı dizesinden depolama hesabı bilgilerini almak için **parse** yöntemini kullanabilirsiniz.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Azure dosya paylaşımındaki tüm dosyalar ve dizinler **Paylaşım**adı verilen bir kapsayıcıda bulunur. Depolama hesabınız, hesap kapasitenizin izin verdiği kadar hisseye sahip olabilir. Bir paylaşıma ve içeriğine erişim elde etmek için bir Azure Files istemcisi kullanmanız gerekir.

```cpp
// Create the Azure Files client.
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();
```

Azure Dosyaları istemcisini kullanarak, bir paylaşıma başvuru alabilirsiniz.

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

Paylaşımı oluşturmak için **cloud_file_share** nesnesinin **create_if_not_exists** yöntemini kullanın.

```cpp
if (share.create_if_not_exists()) {
    std::wcout << U("New share created") << std::endl;
}
```

Bu noktada, **hisse** **benim örnek-paylaşım**adlı bir paya bir başvuru tutar.

## <a name="delete-an-azure-file-share"></a>Azure dosya paylaşımını silme

Bir paylaşımı silme, cloud_file_share bir nesne üzerinde **delete_if_exists** yöntemini çağırarak yapılır. İşte bunu yapan örnek kod.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a>Dizin oluşturma

Dosyaları kök dizininde olması yerine alt dizinlerin içine koyarak depolama alanını düzenleyebilirsiniz. Azure Dosyaları, hesabınızın izin verdiği kadar dizin oluşturmanıza olanak tanır. Aşağıdaki kod, kök dizinin altında **benim örnek dizini** adlı bir dizin yanı sıra **benim-örnek-alt dizini**adlı bir alt dizini oluşturacaktır.

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a>Bir dizini silme

Bir dizin silmek basit bir görevdir, ancak hala dosya veya diğer dizinler içeren bir dizini silemeyeceğiniz unutulmamalıdır.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory =
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin

Bir hisse içindeki dosya ve dizinlerin listesini **almak, cloud_file_directory** bir referansta **list_files_and_directories** arayarak kolayca yapılır. İade edilen bir **list_file_and_directory_item**için zengin özellik ve yöntem kümesine erişmek **için, cloud_file** bir nesne almak için **list_file_and_directory_item.as_file** yöntemini veya **cloud_file_directory** bir nesne almak için **list_file_and_directory_item.as_directory** yöntemini aramanız gerekir.

Aşağıdaki kod, paylaşımın kök dizinindeki her öğenin URI'sinin nasıl alınıp çıktı alındığını gösterir.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir =
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_directory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }
}
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme

En azından, bir Azure dosya paylaşımı dosyaların bulunabileceği bir kök dizini içerir. Bu bölümde, yerel depolama dan bir paylaşımın kök dizinine dosya yüklemeyi öğreneceksiniz.

Bir dosyayı yüklemenin ilk adımı, bulunduğu dizin için bir başvuru elde etmektir. Bunu paylaşım nesnesinin **get_root_directory_reference** yöntemini çağırarak yaparsınız.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Artık paylaşımın kök dizinine bir başvurunuz olduğuna göre, üzerine bir dosya yükleyebilirsiniz. Bu örnek, bir dosyadan, metinden ve bir akıştan yüklenir.

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream =
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));
```

## <a name="download-a-file"></a>Dosya indirme

Dosyaları indirmek için önce bir dosya başvurusu alın ve ardından dosya içeriğini bir akış nesnesine aktarmak için **download_to_stream** yöntemini çağırın ve bu yöntem yerel bir dosyada devam edebilirsiniz. Alternatif olarak, bir dosyanın içeriğini yerel bir dosyaya indirmek için **download_to_file** yöntemini kullanabilirsiniz. Bir dosyanın içeriğini metin dizesi olarak indirmek için **download_text** yöntemini kullanabilirsiniz.

Aşağıdaki örnek, önceki bölümlerde oluşturulan dosyaları indirmeyi göstermek için **download_to_stream** ve **download_text** yöntemlerini kullanır.

```cpp
// Download as text
azure::storage::cloud_file text_file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="delete-a-file"></a>Dosyayı silme

Başka bir yaygın Azure Dosyaları işlemi dosya silme olduğunu. Aşağıdaki kod, kök dizinin altında depolanan my-sample-file-3 adlı bir dosyayı siler.

```cpp
// Get a reference to the root directory for the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir =
  share.get_root_directory_reference();

azure::storage::cloud_file file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-the-quota-maximum-size-for-an-azure-file-share"></a>Azure dosya paylaşımı için kotayı (maksimum boyut) ayarlama

Bir dosya paylaşımı için kotayı (veya maksimum boyutu) gigabaytolarak ayarlayabilirsiniz. Paylaşımda halihazırda ne kadar verinin depolandığını da kontrol edebilirsiniz.

Paylaşım için kota ayarlayarak paylaşımda depolanan toplam dosya boyutunu kısıtlayabilirsiniz. Paylaşımdaki toplam dosya boyutu belirlediğiniz kotayı aşarsa, istemciler mevcut dosyaların boyutunu artıramaz veya boş olmamaları halinde yeni dosyalar oluşturamaz.

Aşağıdaki örnekte, paylaşımdaki mevcut kullanımını nasıl kontrol edileceği veya paylaşım için nasıl kota ayarlanacağı gösterilmiştir.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Dosya veya dosya paylaşımı için paylaşılan erişim imzası oluşturma

Dosya paylaşımı veya tek bir dosya için paylaşılan erişim imzası (SAS) oluşturabilirsiniz. Ayrıca, paylaşılan erişim imzalarını yönetmek için dosya paylaşımında bir paylaşılan erişim ilkesi oluşturabilirsiniz. Gizliliğinin tehlikeye girdiği durumlarda SAS’yi iptal etme aracı olarak kullanılabilmesi nedeniyle bir paylaşılan erişim ilkesi oluşturmanız önerilir.

Aşağıdaki örnekte, paylaşım için bir paylaşılan erişim ilkesi oluşturulur, daha sonra bu ilke paylaşımdaki bir dosyada bulunan SAS için sınırlamalar sağlamak amacıyla kullanılır.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy =
      azure::storage::file_shared_access_policy();

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() +
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir =
        share.get_root_directory_reference();
    azure::storage::cloud_file file =
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share
    //  and associate this access policy with it.
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");
    file_with_sas.upload_text(text);

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();
    ucout << downloaded_text << std::endl;
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Storage hakkında daha fazla bilgi için şu kaynakları araştırın:

* [C++ için Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp)
* [C++'da Azure Depolama Dosya Hizmeti Örnekleri](https://github.com/Azure-Samples/storage-file-cpp-getting-started)
* [Azure Storage Gezgini](https://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)