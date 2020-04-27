---
title: Azure Active Directory kullanarak Azure Data Lake Storage 1. kimlik doğrulaması | Microsoft Docs
description: Azure Active Directory kullanarak Azure Data Lake Storage 1. kimlik doğrulaması yapmayı öğrenin
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
ms.openlocfilehash: f83cf183bee930dd07c707b0eb49125cecd70b84
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60193603"
---
# <a name="authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Active Directory kullanarak Azure Data Lake Storage 1. kimlik doğrulaması

Azure Data Lake Storage 1., kimlik doğrulaması için Azure Active Directory kullanır. Data Lake Storage 1. ile birlikte çalışarak bir uygulamayı yazmadan önce, Azure Active Directory (Azure AD) ile uygulamanızın kimliğini nasıl doğrulayacağınıza karar vermelisiniz.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

* **Son Kullanıcı kimlik doğrulaması** -son kullanıcının Azure kimlik bilgileri Data Lake Storage 1. kimlik doğrulaması için kullanılır. Data Lake Storage 1. ile çalışmak için oluşturduğunuz uygulama bu kullanıcı kimlik bilgilerini ister. Sonuç olarak, bu kimlik doğrulama mekanizması *etkileşimlidir* ve uygulama oturum açmış kullanıcının bağlamında çalışır. Daha fazla bilgi ve yönergeler için bkz. [Data Lake Storage 1. Için Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Hizmetten hizmete kimlik doğrulaması** -bir uygulamanın Data Lake Storage 1. ile kendi kimliğini doğrulamasını istiyorsanız bu seçeneği kullanın. Böyle durumlarda, bir Azure Active Directory (AD) uygulaması oluşturur ve Data Lake Storage 1. kimlik doğrulaması yapmak için Azure AD uygulamasından anahtarı kullanırsınız. Sonuç olarak, bu kimlik doğrulama mekanizması *etkileşimli*değil. Daha fazla bilgi ve yönergeler için bkz. [Data Lake Storage 1. Için hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md).

Aşağıdaki tabloda, son kullanıcı ve hizmetten hizmete kimlik doğrulama mekanizmalarının Data Lake Storage 1. için nasıl desteklendiği gösterilmektedir. Tabloyu nasıl okuyaöğreneceksiniz.

* ✔ * Simgesi, kimlik doğrulama seçeneğinin desteklendiğini ve kimlik doğrulama seçeneğinin nasıl kullanılacağını gösteren bir makaleye bağlantı olduğunu gösterir. 
* ✔ Simgesi, kimlik doğrulama seçeneğinin desteklendiğini gösterir. 
* Boş hücreler, kimlik doğrulama seçeneğinin desteklenmediğini gösterir.


|Bu kimlik doğrulama seçeneğini bununla birlikte kullan...                   |.NET         |Java     |PowerShell |Azure CLI | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Son Kullanıcı (MFA * * olmadan)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(kullanım dışı)     |    **[✔ *](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Son Kullanıcı (MFA ile)                           |    **[✔ *](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔ *](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔ *](data-lake-store-get-started-cli-2.0.md)**      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Hizmetten hizmete (istemci anahtarını kullanarak)         |    **[✔ *](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔ *](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔ *](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔ *](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Hizmetten hizmete (istemci sertifikası kullanarak) |    **[✔ *](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* <b>✔\* </b> Simgesine tıklayın. Bu bir bağlantıdır.</i><br>
<i>* * MFA Multi-Factor Authentication 'ı temsil eder</i>

Kimlik doğrulaması için Azure Active Directory kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory Için kimlik doğrulama senaryoları](../active-directory/develop/authentication-scenarios.md) .

## <a name="next-steps"></a>Sonraki adımlar

* [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)


