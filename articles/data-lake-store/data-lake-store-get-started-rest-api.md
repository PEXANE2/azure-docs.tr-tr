---
title: "REST API: Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri | Microsoft Dokümanlar"
description: Veri Gölü Depolama Gen1 hesabında hesap yönetimi işlemleri gerçekleştirmek için Azure Veri Gölü Depolama Gen1 ve WebHDFS REST API'yi kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 97fe33309f36cd7545f8c9d6c2d34671641caa1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60877121"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>REST API'yi kullanarak Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, REST API'sini kullanarak Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemlerinin nasıl gerçekleştirildirileceğini öğreneceksiniz. Hesap yönetimi işlemleri arasında bir Veri Gölü Depolama Gen1 hesabı oluşturma, Veri Gölü Depolama Gen1 hesabı nın silmesi vb. yer alır. REST API kullanarak Veri Gölü Depolama Gen1'de dosya sistemi işlemlerinin nasıl gerçekleştirileceklerine ilişkin talimatlar için, [REST API'yi kullanarak Veri Gölü Depolama Gen1'deki Dosya Sistemi işlemlerine](data-lake-store-data-operations-rest-api.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](https://curl.haxx.se/)**. Bu makalede, bir Veri Gölü Depolama Gen1 hesabına karşı REST API çağrıları yapmak için nasıl göstermek için cURL kullanır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızın son kullanıcı kimlik doğrulaması için (etkileşimli), [.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-rest-api.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması (etkileşimli olmayan) için , [.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-rest-api.md)bakın.


## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/create) tanımlanan REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. ** \<Storagegengen1name>'yi** Veri Gölü Depolama Gen1 adınız ile değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Yukarıdaki komutta; \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **input.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği aşağıdaki kod parçacığına benzer:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabını silme
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete) tanımlanan REST API çağrısını temel alır.

Bir Veri Gölü Depolama Gen1 hesabını silmek için aşağıdaki cURL komutunu kullanın. ** \<Storagegengen1name>'yi** Data Lake Storage Gen1 hesap adınız ile değiştirin.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview

Aşağıdaki kod parçacığı gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Sonraki adımlar
* [REST API kullanarak Veri Gölü Depolama Gen1 dosya sistemi işlemleri](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Göl Depolama Gen1 REST API Başvuru](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Veri Gölü Depolama Gen1 ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

