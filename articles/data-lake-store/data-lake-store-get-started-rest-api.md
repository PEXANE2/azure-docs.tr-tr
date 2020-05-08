---
title: REST ile Azure Data Lake Storage 1. hesabını yönetme
description: Bir Azure Data Lake Storage 1. hesabında hesap yönetim işlemleri gerçekleştirmek için WebileREST API kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8a106b55fb90f320b90c81216a205dd10a9bf934
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692086"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>REST API kullanarak Azure Data Lake Storage 1. hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, REST API kullanarak Azure Data Lake Storage 1. hesap yönetim işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Hesap yönetimi işlemleri Data Lake Storage 1. hesap oluşturmayı, Data Lake Storage 1. hesabı silmeyi vb. içerir. REST API kullanarak Data Lake Storage 1. dosya sistemi işlemlerinin nasıl gerçekleştirileceği hakkında yönergeler için bkz. [REST API kullanarak Data Lake Storage 1. dosya sistemi işlemleri](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **[kıvır](https://curl.haxx.se/)**. Bu makalede, bir Data Lake Storage 1. hesabına yönelik REST API çağrılarının nasıl yapılacağını göstermek için kıvrımlı kullanılır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızda (etkileşimli) Son Kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md).
* Uygulamanıza yönelik hizmetten hizmete kimlik doğrulaması için (etkileşimli olmayan), bkz. [.NET SDK kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/create) tanımlanan REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. ** \<Yourstoragegen1name>** değiştirin Data Lake Storage 1. adı.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Yukarıdaki komutta; \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **input.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği aşağıdaki kod parçacığına benzer:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. hesabı silme
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete) tanımlanan REST API çağrısını temel alır.

Bir Data Lake Storage 1. hesabını silmek için aşağıdaki kıvrımlı komutunu kullanın. ** \<Yourstoragegen1name>** değerini Data Lake Storage 1. hesap adınızla değiştirin.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview

Aşağıdaki kod parçacığı gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Sonraki adımlar
* [REST API kullanarak Data Lake Storage 1. dosya sistemi işlemleri](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Storage 1. REST API başvurusu](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Data Lake Storage 1. uyumlu açık kaynak büyük veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

