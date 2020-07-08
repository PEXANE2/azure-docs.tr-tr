---
title: REST-Service-Service kimlik doğrulaması-Data Lake Storage 1.-Azure
description: Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması elde etme hakkında bilgi edinin ve REST API kullanarak Azure Active Directory.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 725490316ef4fcbce197d5a29b7665b7de7014c9
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85857115"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>REST API kullanarak Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması yapmak için REST API nasıl kullanacağınızı öğreneceksiniz. REST API kullanarak Data Lake Storage 1. ile son kullanıcı kimlik doğrulaması için bkz. [REST API kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Web" uygulaması oluşturun**. [Azure Active Directory kullanarak, Data Lake Storage 1. Ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)adımlarını tamamlamış olmanız gerekir.

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

Bu senaryoda, uygulama, işlemleri gerçekleştirmek için kendi kimlik bilgilerini sağlar. Bunun için, aşağıdaki kod parçacığında gösterildiği gibi bir POST isteği oluşturmanız gerekir:

```console
curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
  -F grant_type=client_credentials \
  -F resource=https://management.core.windows.net/ \
  -F client_id=<CLIENT-ID> \
  -F client_secret=<AUTH-KEY>
```

İsteğin çıktısı, `access-token` daha sonra REST API çağrılarınız ile geçirdiğiniz bir yetkilendirme belirteci (aşağıdaki çıkışta belirtilen) içerir. Kimlik doğrulama belirtecini bir metin dosyasına kaydedin; Data Lake Storage 1. için bekleyen çağrılar yaparken buna ihtiyaç duyarsınız.

```output
{"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}
```

Bu makalede, **etkileşimli olmayan** yaklaşım kullanılmıştır. Etkileşimli olmayan seçeneği (hizmet-hizmet çağrıları) hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini kullanarak gerçekleştirilen hizmet-hizmet çağrıları](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, REST API kullanarak Data Lake Storage 1. kimlik doğrulaması yapmak için hizmetten hizmete kimlik doğrulamasını nasıl kullanacağınızı öğrendiniz. Artık, Data Lake Storage 1. çalışmak için REST API kullanma hakkında konuşabilecek aşağıdaki makalelere bakabilirsiniz.

* [REST API kullanarak Data Lake Storage 1. hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)
* [REST API kullanarak Data Lake Storage 1. veri işlemleri](data-lake-store-data-operations-rest-api.md)
