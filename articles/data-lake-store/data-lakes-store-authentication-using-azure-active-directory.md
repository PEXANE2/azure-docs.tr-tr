---
title: Azure Etkin Dizini kullanarak Azure Veri Gölü Depolama Gen1'de kimlik doğrulama | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmayı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60193603"
---
# <a name="authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Etkin Dizin'i kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulama

Azure Veri Gölü Depolama Gen1 kimlik doğrulaması için Azure Etkin Dizini kullanır. Veri Gölü Depolama Gen1 ile çalışan bir uygulama yazmadan önce, Azure Etkin Dizini (Azure AD) ile uygulamanızın kimliğini nasıl doğruladığını belirlemeniz gerekir.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

* **Son kullanıcı kimlik doğrulaması** - Son kullanıcının Azure kimlik bilgileri, Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için kullanılır. Veri Gölü Depolama Gen1 ile çalışmak için oluşturduğunuz uygulama bu kullanıcı kimlik bilgileri için istemleri. Sonuç olarak, bu kimlik doğrulama mekanizması *etkileşimlidir* ve uygulama günlüğe kaydedilmiş kullanıcı bağlamında çalışır. Daha fazla bilgi ve yönergeler [için, Veri Gölü Depolama Gen1 için Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)bakın.

* **Servise hizmet kimlik doğrulaması** - Bir uygulamanın Veri Gölü Depolama Gen1 ile kimliğini doğrulamak istiyorsanız bu seçeneği kullanın. Bu gibi durumlarda, bir Azure Etkin Dizin (AD) uygulaması oluşturur sunuz ve Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için Azure AD uygulamasındaki anahtarı kullanırsınız. Sonuç olarak, bu kimlik doğrulama mekanizması *etkileşimli değildir.* Daha fazla bilgi ve yönergeler [için Veri Gölü Depolama Gen1 için hizmete hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)bakın.

Aşağıdaki tablo, Veri Gölü Depolama Gen1 için son kullanıcı ve hizmetten hizmete kimlik doğrulama mekanizmalarının nasıl destekleniyi göstermektedir. Masayı şöyle okuyabiliyorsun.

* ✔* simgesi, kimlik doğrulama seçeneğinin desteklenmesini ve kimlik doğrulama seçeneğinin nasıl kullanılacağını gösteren bir makaleye bağlantı verdiğini belirtir. 
* ✔ simgesi, kimlik doğrulama seçeneğinin desteklenmesini gösterir. 
* Boş hücreler kimlik doğrulama seçeneğinin desteklenmediğini gösterir.


|Bu kimlik doğrulama seçeneğini kullanın...                   |.NET         |Java     |PowerShell |Azure CLI | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Son kullanıcı (MFA**olmadan)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(amortismana uğradı)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Son kullanıcı (MFA ile)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Servise servis (istemci anahtarını kullanarak)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Servise hizmet (istemci sertifikasını kullanma) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* <b>✔\* </b> sembolüne tıklayın. Bu bir bağlantı.</i><br>
<i>** MFA çok faktörlü kimlik doğrulama anlamına gelir</i>

Kimlik doğrulama için Azure Etkin Dizininin nasıl kullanılacağı hakkında daha fazla bilgi için Azure Etkin Dizin için [Kimlik Doğrulama Senaryoları'na](../active-directory/develop/authentication-scenarios.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)


