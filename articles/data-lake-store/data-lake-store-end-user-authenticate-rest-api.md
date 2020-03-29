---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory kullanarak Azure Veri Gölü Depolama Gen1 ile REST API | Microsoft Dokümanlar'
description: REST API'yi kullanarak Azure Etkin Dizinini kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması nasıl elde edilenöğrenin
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
ms.openlocfilehash: 0ef65c23ee1bf4f064695779b71c8616427da204
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60877831"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>REST API'yi kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’sını kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması yapmak için REST API'sini nasıl kullanacağınızı öğrenirsiniz. REST API'yi kullanarak Data Lake Storage Gen1 ile servise hizmet kimlik doğrulaması için, [REST API'yi kullanarak Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-rest-api.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizini "Yerel" Uygulama Oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Son kullanıcı kimlik doğrulamaadımlarını](data-lake-store-end-user-authenticate-using-active-directory.md)tamamlamış olmalısınız.

* **[cURL](https://curl.haxx.se/)**. Bu makalede, bir Veri Gölü Depolama Gen1 hesabına karşı REST API çağrıları yapmak için nasıl göstermek için cURL kullanır.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Bir kullanıcının Azure AD kullanarak uygulamanızda oturum açmasını istiyorsanız, son kullanıcı kimlik doğrulaması önerilen yaklaşımdır. Uygulamanız, oturum açmış kullanıcıyla aynı erişim düzeyine sahip Azure kaynaklarına erişebilir. Uygulamanızın erişimi koruyabilmesi için kullanıcının kimlik bilgilerini düzenli aralıklarla sağlaması gerekir.

Son kullanıcı girişinin sonucu, uygulamanız için bir erişim belirteci ve yenileme belirteci verilmesidir. Erişim belirteci, Data Lake Storage Gen1 veya Data Lake Analytics'e yapılan her isteğe eklenir ve varsayılan olarak bir saat boyunca geçerlidir. Yenileme belirteci, yeni bir erişim belirteci elde etmek için kullanılabilir ve düzenli olarak kullanıldığında varsayılan olarak iki haftaya kadar geçerlidir. Son kullanıcı girişi için iki farklı yaklaşım kullanabilirsiniz.

Bu senaryoda, uygulama kullanıcıdan oturum açmasını ister ve tüm işlemler, kullanıcı bağlamında gerçekleştirilir. Aşağıdaki adımları uygulayın:

1. Uygulamanızı kullanarak kullanıcıyı şu URL'ye yönlendirin:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

   > [!NOTE]
   > \<REDIRECT-URI>, bir URL içinde kullanılmak üzere kodlanmalıdır. Yani, https://localhostiçin `https%3A%2F%2Flocalhost`, kullanmak )

    Bu öğreticinin amaçları doğrultusunda, yukarıdaki URL'deki yer tutucu değerlerini değiştirebilir ve bir web tarayıcısının adres çubuğuna yapıştırabilirsiniz. Azure oturum açma bilgilerinizi kullanarak kimlik doğrulaması gerçekleştirmeye yönlendirileceksiniz. Başarıyla oturum açtığınızda yanıt, tarayıcının adres çubuğunda görüntülenir. Yanıt şu biçimde olacaktır:

        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Yanıttaki yetki kodunu alın. Bu öğretici için, yetkilendirme kodunu web tarayıcısının adres çubuğundan kopyalayabilir ve aşağıdaki parçacıkta gösterildiği gibi POSTA isteğinde belirteç uç noktasına iletebilirsiniz:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>

   > [!NOTE]
   > Bu durumda, \<REDIRECT-URI> öğesinin kodlanması gerekmez.
   > 
   > 

3. Yanıt, erişim belirteci (örneğin,) `"access_token": "<ACCESS_TOKEN>"`ve yenileme belirteci (örneğin,) `"refresh_token": "<REFRESH_TOKEN>"`içeren bir JSON nesnesidir. Uygulamanız, Azure Veri Gölü Depolama Gen1'e erişirken erişim jetonunu ve erişim belirteci süresi dolduğunda başka bir erişim belirteci almak için yenileme belirteci kullanır.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4. Erişim belirteci süresi dolduğunda, aşağıdaki parçacıkta gösterildiği gibi, yenileme belirteci'ni kullanarak yeni bir erişim belirteci isteyebilirsiniz:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Etkileşimli kullanıcı kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Yetki kodu izin akışı](https://msdn.microsoft.com/library/azure/dn645542.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, REST API'yi kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için hizmete kimlik doğrulamayı nasıl kullanacağınızı öğrendiniz. Şimdi, Azure Veri Gölü Depolama Gen1 ile çalışmak için REST API'sini nasıl kullanacağınızı anlatan aşağıdaki makalelere bakabilirsiniz.

* [REST API kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)
* [REST API kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-rest-api.md)

