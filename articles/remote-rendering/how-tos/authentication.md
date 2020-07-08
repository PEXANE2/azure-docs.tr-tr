---
title: Kimlik Doğrulaması
description: Kimlik doğrulamanın nasıl çalıştığını açıklar
author: florianborn71
ms.author: flborn
ms.date: 06/15/2020
ms.topic: how-to
ms.openlocfilehash: 8f3b144a7790c3122d59d27183b3037998ddadd1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565850"
---
# <a name="configure-authentication"></a>Kimlik doğrulamasını yapılandırma

Azure uzaktan Işleme, [Azure uzamsal bağlayıcı (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp)ile aynı kimlik doğrulama mekanizmasını kullanır. İstemcilerin REST API 'Leri başarıyla çağırmak için aşağıdakilerden *birini* ayarlaması gerekir:

* **Accountkey**: Azure Portal uzaktan işleme hesabının "anahtarlar" sekmesinde elde edilebilir. Hesap anahtarları yalnızca geliştirme/prototipleme için önerilir.
    ![Hesap Kimliği](./media/azure-account-primary-key.png)

* **Authenticationtoken**: [msal kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)kullanılarak elde edilebilir bir Azure AD belirtecidir. Kullanıcı kimlik bilgilerini kabul etmek için kullanılabilen birden çok farklı akış vardır ve bu kimlik bilgilerini bir erişim belirteci almak için kullanabilirsiniz.

* **Mraccesstoken**: Azure Karma Gerçeklik güvenlik belirteci hizmeti 'NDEN (STS) elde EDILEBILIR bir Mr belirtecidir. `https://sts.mixedreality.azure.com`Aşağıdaki çağrıya benzer BIR Rest çağrısı kullanılarak uç noktadan alınır:

    ```rest
    GET https://sts.mixedreality.azure.com/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
    Host: sts.mixedreality.azure.com
    Connection: Keep-Alive

    HTTP/1.1 200 OK
    Date: Tue, 24 Mar 2020 09:09:00 GMT
    Content-Type: application/json; charset=utf-8
    Content-Length: 1153
    Accept: application/json
    MS-CV: 05JLqWeKFkWpbdY944yl7A.0
    {"AccessToken":"eyJhbGciOiJSUzI1<truncated>uLkO2FvA"}
    ```

    Yetkilendirme üstbilgisinin şu şekilde biçimlendirildiği yer: `Bearer <Azure_AD_token>` veya `Bearer <accoundId>:<accountKey>` . İlki güvenlik için tercih edilir. Bu REST çağrısından döndürülen belirteç MR erişim belirtecidir.

## <a name="authentication-for-deployed-applications"></a>Dağıtılan uygulamalar için kimlik doğrulaması

Yalnızca geliştirme sırasında hızlı prototipleme için hesap anahtarları önerilir. Uygulamanızda ekli bir hesap anahtarı kullanarak uygulamanızı üretime göndermemelidir. Önerilen yaklaşım, Kullanıcı tabanlı veya hizmet tabanlı bir Azure AD kimlik doğrulaması yaklaşımını kullanmaktır.

 Azure AD kimlik doğrulaması, Azure [uzamsal bağlayıcı (ASA)](https://docs.microsoft.com/azure/spatial-anchors/) hizmeti 'nın [Azure AD Kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) bölümünde açıklanmaktadır.

 Daha fazla bilgi için bkz [. Öğretici: Azure uzaktan işleme ve model depolama-Azure Active Directory kimlik doğrulamasının güvenliğini sağlama](../tutorials/unity/security/security.md#azure-active-directory-azure-ad-authentication)

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Hizmetinize verilen erişim düzeyini denetlemeye yardımcı olmak için, rol tabanlı erişim verirken aşağıdaki rolleri kullanın:

* **Uzaktan Işleme Yöneticisi**: Azure uzaktan işleme için Kullanıcı dönüştürme, oturum, işleme ve Tanılama özelliklerini yönetme olanağı sunar.
* **Uzaktan Işleme istemcisi**: kullanıcıya Azure uzaktan işleme yönelik oturum, işleme ve Tanılama özelliklerini yönetme olanağı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Hesap oluşturma](create-an-account.md)
* [Kimlik doğrulaması için Azure ön uç API 'Lerini kullanma](frontend-apis.md)
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
