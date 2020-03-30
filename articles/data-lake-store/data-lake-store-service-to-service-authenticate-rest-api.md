---
title: Servise hizmet kimlik doğrulaması - Veri Gölü Depolama Gen1 - REST API
description: REST API'sini kullanarak Azure Veri Gölü Depolama Gen1 ve Azure Etkin Dizini ile hizmete hizmet kimlik doğrulaması nasıl elde edebilirsiniz öğrenin.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 59d0bf20b16beda47d76e6a9940ac9fa4436da3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73904524"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>REST API'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Veri Gölü Depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için REST API'sini nasıl kullanacağınızı öğreneceksiniz. REST API kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması için, [REST API kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizin "Web" Uygulaması oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Hizmetten Hizmete kimlik doğrulamaadımlarını](data-lake-store-service-to-service-authenticate-using-active-directory.md)tamamlamış olmalısınız.

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

Bu senaryoda, uygulama işlemleri gerçekleştirmek için kendi kimlik bilgilerini sağlar. Bunun için, aşağıdaki parçacıkta gösterildiği gibi bir POST isteği düzenlemeniz gerekir:

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

İsteğin çıktısı, daha sonra REST API çağrılarınızla geçtiğiniz bir yetkilendirme belirteci (aşağıdaki `access-token` çıktıda belirtilir) içerir. Kimlik doğrulama belirteci'ni metin dosyasına kaydedin; Data Lake Storage Gen1'e REST aramaları yaparken ihtiyacınız olacaktır.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Bu makalede, **etkileşimli olmayan** yaklaşım kullanılmıştır. Etkileşimli olmayan seçeneği (hizmet-hizmet çağrıları) hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini kullanarak gerçekleştirilen hizmet-hizmet çağrıları](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, REST API kullanarak Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için hizmet-servis kimlik doğrulaması nasıl kullanılacağını öğrendiniz. Şimdi Data Lake Storage Gen1 ile çalışmak için REST API'nin nasıl kullanılacağı hakkında konuşulan aşağıdaki makalelere bakabilirsiniz.

* [REST API kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)
* [REST API kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-rest-api.md)
