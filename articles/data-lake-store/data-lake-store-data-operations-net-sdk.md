---
title: .NET SDK-Data Lake Storage 1. dosya sistemi işlemleri-Azure
description: Klasör oluşturma gibi Data Lake Storage 1. dosya sistemi işlemleri için Azure Data Lake Storage 1. .NET SDK 'yı kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 01/03/2020
ms.author: twooley
ms.openlocfilehash: eb950f6029511cf834791c161e6a730bcadcabcc
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85515662"
---
# <a name="filesystem-operations-on-data-lake-storage-gen1-using-the-net-sdk"></a>.NET SDK kullanarak Data Lake Storage 1. dosya sistemi işlemleri

> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Bu makalede, .NET SDK kullanarak Data Lake Storage 1. dosya sistemi işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Dosya sistemi işlemleri bir Data Lake Storage 1. hesapta klasör oluşturmayı, dosyaları karşıya yüklemeyi, dosyaları indirmeyi vb. içerir.

.NET SDK kullanarak Data Lake Storage 1. hesap yönetimi işlemlerinin nasıl yapılacağı hakkında yönergeler için bkz. [.NET SDK kullanarak Data Lake Storage 1. üzerinde hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Ön koşullar

* **Visual Studio 2013 veya üzeri**. Bu makaledeki yönergeler Visual Studio 2019 kullanır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Storage 1. hesabı**. Hesap oluşturma yönergeleri için bkz. kullanmaya [başlama Azure Data Lake Storage 1.](data-lake-store-get-started-portal.md).

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma

[GitHub’da](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

1. Visual Studio 'da **Dosya** menüsünü, **Yeni**' yi ve ardından **Proje**' yi seçin.
1. **Konsol uygulaması (.NET Framework)** öğesini seçin ve ardından **İleri**' yi seçin.
1. **Proje adı**alanına girin `CreateADLApplication` ve ardından **Oluştur**' u seçin.
1. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   1. **NuGet Paket Yöneticisi** sekmesinde, **paket kaynağının** **NuGet.org**olarak ayarlandığından emin olun. Ayrıca, **ön sürümü dahil** et onay kutusunun işaretli olduğundan emin olun.
   1. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.DataLake.Store`-Bu makalede v 1.0.0 kullanılmaktadır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-Bu makalede v 2.3.1 kullanılmaktadır.

      **NuGet Paket Yöneticisi 'ni**kapatın.

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

Makalenin kalan bölümlerinde, kullanılabilir .NET yöntemlerinin kimlik doğrulama, dosya yükleme vb. işlemleri yapmak için nasıl kullanılacağını görebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanıza yönelik hizmetten hizmete kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="create-client-object"></a>İstemci nesnesi oluşturma

Aşağıdaki kod parçacığı, hizmete istek vermek için kullanılan Data Lake Storage 1. dosya sistemi istemci nesnesini oluşturur.

```
// Create client objects
AdlsClient client = AdlsClient.CreateClient(_adlsg1AccountName, adlCreds);
```

## <a name="create-a-file-and-directory"></a>Dosya ve dizin oluşturma

Aşağıdaki kod parçacığını uygulamanıza ekleyin. Bu kod parçacığı bir dosya ve var olmayan bir üst dizin ekler.

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

Aşağıdaki kod parçacığı Data Lake Storage 1. hesabındaki mevcut bir dosyaya veri ekler.

```
// Append to existing file

using (var stream = client.GetAppendStream(fileName))
{
    byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
    stream.Write(textByteArray, 0, textByteArray.Length);
}
```

## <a name="read-a-file"></a>Dosya okuma

Aşağıdaki kod parçacığı Data Lake Storage 1. içindeki bir dosyanın içeriğini okur.

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

Metodun tanımı, `PrintDirectoryEntry` [GitHub 'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)örneğin bir parçası olarak kullanılabilir.

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma

Aşağıdaki kod parçacığı bir Data Lake Storage 1. hesabındaki mevcut bir dosyayı yeniden adlandırır.

```
// Rename a file
string destFilePath = "/Test/testRenameDest3.txt";
client.Rename(fileName, destFilePath, true);
```

## <a name="enumerate-a-directory"></a>Dizin listeleme

Aşağıdaki kod parçacığında bir Data Lake Storage 1. hesabındaki dizinler numaralandırılır.

```
// Enumerate directory
foreach (var entry in client.EnumerateDirectory("/Test"))
{
    PrintDirectoryEntry(entry);
}
```

Metodun tanımı, `PrintDirectoryEntry` [GitHub 'daki](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)örneğin bir parçası olarak kullanılabilir.

## <a name="delete-directories-recursively"></a>Dizinleri yinelemeli olarak silme

Aşağıdaki kod parçacığı bir dizini ve tüm alt dizinlerini yinelemeli olarak siler.

```
// Delete a directory and all its subdirectories and files
client.DeleteRecursive("/Test");
```

## <a name="samples"></a>Örnekler

Data Lake Storage 1. dosya sistemi SDK 'sının nasıl kullanılacağını gösteren birkaç örnek aşağıda verilmiştir.

* [GitHub 'da temel örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [GitHub 'da gelişmiş örnek](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [.NET SDK kullanarak Data Lake Storage 1. hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [Data Lake Storage 1. .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
