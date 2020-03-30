---
title: '.NET SDK: Azure Veri Gölü Depolama Gen1 dosya sistemi işlemleri'
description: Veri Gölü Depolama Gen1'deki dosya sistemi işlemleri için Azure Veri Gölü Depolama Gen1 .NET SDK'yı kullanın klasör oluşturma vb.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 01/03/2020
ms.author: twooley
ms.openlocfilehash: 7e33ecbbb49fc2b0683d0757da36deec72796806
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638910"
---
# <a name="filesystem-operations-on-data-lake-storage-gen1-using-the-net-sdk"></a>.NET SDK kullanarak Veri Gölü Depolama Gen1 dosya sistemi işlemleri

> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Bu makalede, .NET SDK'yı kullanarak Veri Gölü Depolama Gen1'de dosya sistemi işlemlerinin nasıl gerçekleştirildirilecek olduğunu öğreneceksiniz. Dosya sistemi işlemleri, Veri Gölü Depolama Gen1 hesabında klasör oluşturma, dosya yükleme, dosya indirme vb. içerir.

.NET SDK'yı kullanarak Veri Gölü Depolama Gen1'de hesap yönetimi işlemlerinin nasıl yapılacağını öğrenmek için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1'deki Hesap yönetimi işlemlerine](data-lake-store-get-started-net-sdk.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Visual Studio 2013 ve üzeri**. Bu makaledeki yönergelerde Visual Studio 2019 kullanılmaktadır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Veri Gölü Depolama Gen1 hesabı.** Hesap oluşturma yla ilgili talimatlar için [bkz.](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma

[GitHub’da](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

1. Visual Studio'da **Dosya** menüsünü, **Yeni'yi**ve ardından **Project'i**seçin.
1. **Konsol Uygulaması (.NET Framework) seçeneğini**belirleyin ve ardından **İleri'yi**seçin.
1. **Proje adına**, `CreateADLApplication`girin ve sonra **Oluştur'u**seçin.
1. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   1. **NuGet Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org**olarak ayarlandıklarına emin olun. Ayrıca, Ön **Sürüm Ekle** onay kutusunun seçildiğinden emin olun.
   1. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.DataLake.Store`- Bu makalede v1.0.0 kullanır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`- Bu makalede v2.3.1 kullanır.

      **NuGet Paket Yöneticisini**kapatın.

1. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

    ```
    using System;
    using System.IO;using System.Threading;
    using System.Linq;
    using System.Text;
    using System.Collections.Generic;
    using System.Security.Cryptography.X509Certificates; // Required only if you're using an Azure AD application created with certificates
                
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.DataLake.Store;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

1. Aşağıda gösterilen değişkenleri bildirin ve yer tutucu değerlerini sağlayın. Ayrıca, burada sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun.

    ```
    namespace SdkSample
    {
        class Program
        {
            private static string _adlsg1AccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net";
        }
    }
    ```

Makalenin geri kalan bölümlerinde, kimlik doğrulama, dosya yükleme gibi işlemleri yapmak için kullanılabilir .NET yöntemlerinin nasıl kullanılacağını görebilirsiniz.

## <a name="authentication"></a>Kimlik doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-net-sdk.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile servise hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-net-sdk.md)bakın.

## <a name="create-client-object"></a>İstemci nesnesi oluşturma

Aşağıdaki parçacık, hizmete istek vermek için kullanılan Veri Gölü Depolama Gen1 dosya sistemi istemci nesnesini oluşturur.

```
// Create client objects
AdlsClient client = AdlsClient.CreateClient(_adlsg1AccountName, adlCreds);
```

## <a name="create-a-file-and-directory"></a>Dosya ve dizin oluşturma

Aşağıdaki kod parçacığını uygulamanıza ekleyin. Bu parçacık, bir dosya ve var olmayan herhangi bir üst dizini ekler.

```
// Create a file - automatically creates any parent directories that don't exist
// The AdlsOutputStream preserves record boundaries - it does not break records while writing to the store

using (var stream = client.CreateFile(fileName, IfExists.Overwrite))
{
    byte[] textByteArray = Encoding.UTF8.GetBytes("This is test data to write.\r\n");
    stream.Write(textByteArray, 0, textByteArray.Length);

    textByteArray = Encoding.UTF8.GetBytes("This is the second line.\r\n");
    stream.Write(textByteArray, 0, textByteArray.Length);
}
```

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme

Aşağıdaki parçacık verileri Veri Gölü Depolama Gen1 hesabındaki varolan bir dosyaya ekler.

```
// Append to existing file

using (var stream = client.GetAppendStream(fileName))
{
    byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
    stream.Write(textByteArray, 0, textByteArray.Length);
}
```

## <a name="read-a-file"></a>Dosya okuma

Aşağıdaki parçacık, Veri Gölü Depolama Gen1'deki bir dosyanın içeriğini okur.

```
//Read file contents

using (var readStream = new StreamReader(client.GetReadStream(fileName)))
{
    string line;
    while ((line = readStream.ReadLine()) != null)
    {
        Console.WriteLine(line);
    }
}
```

## <a name="get-file-properties"></a>Dosya özelliklerini alma

Aşağıdaki kod parçacığı bir dosya veya dizin ile ilişkili özellikleri döndürür.

```
// Get file properties
var directoryEntry = client.GetDirectoryEntry(fileName);
PrintDirectoryEntry(directoryEntry);
```

Yöntemin `PrintDirectoryEntry` tanımı [GitHub'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)örneğin bir parçası olarak kullanılabilir.

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma

Aşağıdaki parçacık, Bir Veri Gölü Depolama Gen1 hesabındavarolan bir dosyayı yeniden adlandırır.

```
// Rename a file
string destFilePath = "/Test/testRenameDest3.txt";
client.Rename(fileName, destFilePath, true);
```

## <a name="enumerate-a-directory"></a>Dizin listeleme

Aşağıdaki parçacık, bir Data Lake Storage Gen1 hesabındaki dizinleri sayıyor.

```
// Enumerate directory
foreach (var entry in client.EnumerateDirectory("/Test"))
{
    PrintDirectoryEntry(entry);
}
```

Yöntemin `PrintDirectoryEntry` tanımı [GitHub'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)örneğin bir parçası olarak kullanılabilir.

## <a name="delete-directories-recursively"></a>Dizinleri yinelemeli olarak silme

Aşağıdaki parçacık bir dizini ve tüm alt dizinlerini özyinelemeli olarak siler.

```
// Delete a directory and all its subdirectories and files
client.DeleteRecursive("/Test");
```

## <a name="samples"></a>Örnekler

Burada Veri Gölü Depolama Gen1 Filesystem SDK nasıl kullanılacağını gösteren birkaç örnek verilmiştir.

* [GitHub'da temel örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [GitHub'da gelişmiş örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [.NET SDK kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Veri Gölü Depolama Gen1 .NET SDK Referans](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
