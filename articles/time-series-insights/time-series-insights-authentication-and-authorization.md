---
title: Azure Time Series Insights bir API kullanarak kimlik doğrulaması ve yetkilendirme | Microsoft Docs
description: Bu makalede, Azure Time Series Insights API 'sini çağıran özel bir uygulama için kimlik doğrulama ve yetkilendirmeyi yapılandırma açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/29/2019
ms.custom: seodec18
ms.openlocfilehash: bdf0fbfb2b40e932f9e3627c3bc0356eb0afb472
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677941"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Azure Time Series Insights API 'SI için kimlik doğrulaması ve yetkilendirme

Bu belgede, yeni Azure Active Directory dikey penceresini kullanarak Azure Active Directory bir uygulamanın nasıl kaydedileceği açıklanmaktadır. Azure Active Directory kayıtlı uygulamalar, kullanıcıların kimlik doğrulaması yapmasını ve bir Time Series Insights ortamıyla ilişkili Azure zaman serisi Insight API 'sini kullanma yetkisine sahip olmasını sağlar.

## <a name="service-principal"></a>Hizmet sorumlusu

Aşağıdaki bölümlerde uygulama adına Time Series Insights API 'sine erişmek için bir uygulamanın nasıl yapılandırılacağı açıklanır. Uygulama daha sonra Azure Active Directory aracılığıyla kendi uygulama kimlik bilgilerini kullanarak Time Series Insights ortamında başvuru verilerini sorgulayabilir veya yayımlayabilir.

## <a name="summary-and-best-practices"></a>Özet ve en iyi uygulamalar

Azure Active Directory uygulama kayıt akışı üç ana adımdan oluşur.

1. Azure Active Directory [bir uygulamayı kaydedin](#azure-active-directory-app-registration) .
1. Uygulamayı [Time Series Insights ortamına veri erişimi](#granting-data-access)sağlamak için yetkilendirin.
1. [İstemci uygulamanızda](#client-app-initialization) `https://api.timeseries.azure.com/` bir belirteç almak için **uygulama kimliğini** ve istemci gizli anahtarını kullanın. Belirteç daha sonra Time Series Insights API 'sini çağırmak için kullanılabilir.

**3. adım**başına, uygulamanızın ve Kullanıcı kimlik bilgilerinizin ayrılması şunları yapmanıza olanak sağlar:

* Uygulama kimliğine kendi izininizden farklı izinler atayın. Genellikle, bu izinler yalnızca uygulamanın gerektirdiği şekilde kısıtlanır. Örneğin, uygulamanın yalnızca belirli bir Time Series Insights ortamından veri okumasına izin verebilirsiniz.
* Bir **Istemci gizli** dizisi veya güvenlik sertifikası kullanarak, kullanıcının kimlik doğrulama kimlik bilgileri oluşturma ile uygulamanın güvenliğini yalıtın. Sonuç olarak, uygulamanın kimlik bilgileri belirli bir kullanıcının kimlik bilgilerine bağımlı değildir. Kullanıcının rolü değişirse, uygulamanın yeni kimlik bilgileri veya daha fazla yapılandırma gerekmez. Kullanıcı parolasını değiştirirse, uygulamaya tüm erişim yeni kimlik bilgileri veya anahtarlar gerektirmez.
* Belirli bir kullanıcının kimlik bilgileri yerine bir **Istemci gizli anahtarı** veya güvenlik sertifikası kullanarak katılımsız bir komut dosyası çalıştırın (bunların mevcut olmasını gerektirin).
* Azure Time Series Insights API 'nize erişimi güvenli hale getirmek için parola yerine bir güvenlik sertifikası kullanın.

> [!IMPORTANT]
> Azure Time Series Insights güvenlik ilkenizi yapılandırırken **kaygıları ayırma** (yukarıdaki senaryo için yukarıda açıklanmıştır) ilkesini izleyin.

> [!NOTE]
> * Makale, uygulamanın yalnızca bir kuruluşta çalıştırılması amaçlanan tek kiracılı bir uygulamaya odaklanır.
> * Genellikle kuruluşunuzda çalışan iş kolu uygulamaları için tek kiracılı uygulamalar kullanırsınız.

## <a name="detailed-setup"></a>Ayrıntılı kurulum

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory uygulama kaydı

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Veri erişimi verme

1. Time Series Insights ortamı için, **veri erişim ilkeleri** ' ni seçin ve **Ekle**' yi seçin.

   [![Time Series Insights ortamına yeni veri erişim ilkesi ekleme](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. **Kullanıcı Seç** iletişim kutusunda uygulama **adı** ' nı ya da **uygulama kimliği** ' ni Azure Active Directory uygulama kaydı bölümünden yapıştırın.

   [![Kullanıcı Seç iletişim kutusunda bir uygulama bulma](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rolü seçin. Verileri sorgulamak ve başvuru verilerini değiştirmek için **okuyucu** ' yı seçin. **Tamam**’ı seçin.

   [![Kullanıcı rolü Seç iletişim kutusunda okuyucu veya katkıda bulunan seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. **Tamam ' ı**seçerek ilkeyi kaydedin.

   > [!TIP]
   > Azure Active Directory Time Series Insights ortamınıza [veri erişimi verme](./time-series-insights-data-access.md) hakkında bilgi edinin.

### <a name="client-app-initialization"></a>İstemci uygulaması başlatma

1. Uygulamanın adına belirteci almak için Azure Active Directory uygulama kaydı bölümünde **uygulama kimliği** ve **istemci gizli** anahtarını (uygulama anahtarı) kullanın.

    ' C#De, aşağıdaki kod, uygulama adına belirteci alabilir. Tüm örnek için bkz. [sorgu verileri kullanarak C# ](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/",
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

1. Belirteç daha sonra uygulama Time Series Insights API 'sini çağırdığında `Authorization` üstbilgiye geçirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- GA Time Series Insights API 'sini çağıran örnek kod için bkz. [sorgu verileri kullanma C# ](./time-series-insights-query-data-csharp.md).

- Time Series Insights API kodu örnekleri için bkz. [sorgu Önizleme verileri kullanılarak C# ](./time-series-insights-update-query-data-csharp.md).

- API başvuru bilgileri için bkz. [sorgu API 'si başvurusu](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- [Hizmet sorumlusu oluşturmayı](../active-directory/develop/howto-create-service-principal-portal.md)öğrenin.