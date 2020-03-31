---
title: "REST API: Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri | Microsoft Dokümanlar"
description: Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri gerçekleştirmek için WebHDFS REST API'lerini kullanın
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
ms.openlocfilehash: 351c92f1e1a698893f61004d523ba79ebca253e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60878792"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>REST API kullanarak Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Bu makalede, Azure Veri Gölü Depolama Gen1'de dosya sistemi işlemleri gerçekleştirmek için WebHDFS REST API'lerini ve Veri Gölü Depolama Gen1 REST API'lerini nasıl kullanacağınızı öğreneceksiniz. REST API'yi kullanarak Veri Gölü Depolama Gen1'de hesap yönetimi işlemlerinin nasıl gerçekleştirileceklerine ilişkin talimatlar için, [REST API'yi kullanarak Veri Gölü Depolama Gen1'deki Hesap yönetimi işlemlerine](data-lake-store-get-started-rest-api.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Veri Gölü Depolama Gen1 hesabı.** [Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın'daki](data-lake-store-get-started-portal.md)yönergeleri izleyin.

* **[cURL](https://curl.haxx.se/)**. Bu makalede, bir Veri Gölü Depolama Gen1 hesabına karşı REST API çağrıları yapmak için nasıl göstermek için cURL kullanır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızın son kullanıcı kimlik doğrulaması için (etkileşimli), [.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-rest-api.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması (etkileşimli olmayan) için , [.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-rest-api.md)bakın.


## <a name="create-folders"></a>Klasör oluşturma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komut, Veri Gölü Depolama Gen1 hesabınızın kök klasörü altında **mytempdir** adlı bir dizin oluşturur.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

    {"boolean":true}

## <a name="list-folders"></a>Klasörleri listeleme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "<GUID>",
            "group": "<GUID>"
        }]
    }
    }

## <a name="upload-data"></a>Karşıya veri yükleme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

Yukarıdaki söz diziminde **-T** parametresi karşıya yüklediğiniz dosyanın konumudur.

Çıkış aşağıdaki kod parçacığına benzer:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data"></a>Verileri okuma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Veri Gölü Depolama Gen1 hesabından veri okuma iki adımlı bir işlemdir.

* İlk olarak `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN` uç noktası için bir GET isteği gönderirsiniz. Bu çağrı, sonraki GET isteğini göndermek için bir konum döndürür.
* Ardından `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true` uç noktası için GET isteğini gönderirsiniz. Bu çağrı, dosyanın içeriğini görüntüler.

Ancak giriş parametrelerinde birinci ve ikinci adım arasında fark olmadığından, ilk isteği göndermek için `-L` parametresini kullanabilirsiniz. `-L` seçeneği, temelde iki isteği tek istekte birleştirir ve cURL'nin isteği yeni konumda yeniden gerçekleştirmesini sağlar. Son olarak, aşağıdaki kod parçacığında gösterildiği gibi, tüm istek çağrılarının çıktısı görüntülenir. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı yeniden adlandırmak için aşağıdaki cURL komutunu kullanın. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file"></a>Dosyayı silme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı silmek için aşağıdaki cURL komutunu kullanın. ** \<Mağaza adınızı>** Veri Gölü Depolama Gen1 hesap adınız ile değiştirin.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Aşağıdaki gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="next-steps"></a>Sonraki adımlar
* [REST API kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri.](data-lake-store-get-started-rest-api.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Göl Depolama Gen1 REST API Başvuru](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Veri Gölü Depolama Gen1 ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

