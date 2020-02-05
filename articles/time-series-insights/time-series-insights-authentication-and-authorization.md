---
title: API kimlik doğrulaması ve yetkilendirme-Azure Time Series Insights | Microsoft Docs
description: Bu makalede, Azure Time Series Insights API 'sini çağıran özel bir uygulama için kimlik doğrulama ve yetkilendirmeyi yapılandırma açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: ff5f7a80e2dcedb1795bae14ee9140c2842303a5
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76984599"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Azure Time Series Insights API 'SI için kimlik doğrulaması ve yetkilendirme

Bu belgede, yeni Azure Active Directory dikey penceresini kullanarak Azure Active Directory bir uygulamanın nasıl kaydedileceği açıklanmaktadır. Azure Active Directory kayıtlı uygulamalar, kullanıcıların kimlik doğrulaması yapmasını ve bir Time Series Insights ortamıyla ilişkili Azure zaman serisi Insight API 'sini kullanma yetkisine sahip olmasını sağlar.

> [!IMPORTANT]
> Azure Time Series Insights, aşağıdaki kimlik doğrulama kitaplıklarının her ikisini de destekler:
> * Daha yeni [Microsoft kimlik doğrulama kitaplığı (msal)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)
> * [Azure Active Directory kimlik doğrulama kitaplığı (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="service-principal"></a>Hizmet sorumlusu

Aşağıdaki bölümlerde uygulama adına Time Series Insights API 'sine erişmek için bir uygulamanın nasıl yapılandırılacağı açıklanır. Uygulama daha sonra Azure Active Directory aracılığıyla kendi uygulama kimlik bilgilerini kullanarak Time Series Insights ortamında başvuru verilerini sorgulayabilir veya yayımlayabilir.

## <a name="summary-and-best-practices"></a>Özet ve en iyi uygulamalar

Azure Active Directory uygulama kayıt akışı üç ana adımdan oluşur.

1. Azure Active Directory [bir uygulamayı kaydedin](#azure-active-directory-app-registration) .
1. Uygulamayı [Time Series Insights ortamına veri erişimi](#granting-data-access)sağlamak için yetkilendirin.
1. [İstemci uygulamanızda](#client-app-initialization)`https://api.timeseries.azure.com/` belirteç almak IÇIN **uygulama kimliği** ve **istemci gizli** anahtarını kullanın. Belirteç daha sonra Time Series Insights API 'sini çağırmak için kullanılabilir.

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

   [Time Series Insights ortamına yeni veri erişim ilkesi eklemek ![](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. **Kullanıcı Seç** iletişim kutusunda uygulama **adı** ' nı ya da **uygulama kimliği** ' ni Azure Active Directory uygulama kaydı bölümünden yapıştırın.

   [Kullanıcı Seç iletişim kutusunda uygulama bulma ![](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rolü seçin. Verileri sorgulamak ve başvuru verilerini değiştirmek **Için** **okuyucu** ' yı seçin. **Tamam**’ı seçin.

   [Kullanıcı rolü Seç iletişim kutusunda okuyucu veya katkıda bulunan ![seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. **Tamam ' ı**seçerek ilkeyi kaydedin.

   > [!TIP]
   > Gelişmiş veri erişimi seçenekleri için [veri erişimi verme](./time-series-insights-data-access.md)konusunu okuyun.

### <a name="client-app-initialization"></a>İstemci uygulaması başlatma

* Geliştiriciler Azure Time Series Insights kimlik doğrulaması yapmak için [Microsoft kimlik doğrulama kitaplığı 'nı (msal)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) veya [Azure Active Directory kimlik doğrulama KITAPLıĞı 'nı (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) kullanabilir.

* Örneğin, ADAL kullanarak kimlik doğrulamak için:

   1. Uygulamanın adına belirteci almak için Azure Active Directory uygulama kaydı bölümünde **uygulama kimliği** ve **istemci gizli** anahtarını (uygulama anahtarı) kullanın.

   1. ' C#De, aşağıdaki kod, uygulama adına belirteci alabilir. Tüm örnek için, [ C#kullanarak sorgu verilerini ](time-series-insights-query-data-csharp.md)okuyun.

        [!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs?range=170-199)]

   1. Belirteç daha sonra uygulama Time Series Insights API 'sini çağırdığında `Authorization` üst bilgisine geçirilebilir.

* Alternatif olarak, geliştiriciler MSAL kullanarak kimlik doğrulaması yapabilir. [Msal 'e geçme](https://docs.microsoft.com/azure/active-directory/develop/msal-net-migration) hakkında bilgi edinin ve daha fazla bilgi edinmek için makaleyi [ C# kullanarak Azure Time Series Insights ortamı için GA-Manage GA başvuru verilerini](time-series-insights-manage-reference-data-csharp.md) inceleyin. 

## <a name="common-headers-and-parameters"></a>Ortak üstbilgiler ve parametreler

Bu bölümde, Time Series Insights GA ve Preview API 'Lerinde sorgu yapmak için kullanılan genel HTTP istek üstbilgileri ve parametreleri açıklanmaktadır. API 'ye özgü gereksinimler [Time Series Insights REST API başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/)daha ayrıntılı bir şekilde ele alınmıştır.

> [!TIP]
> REST API 'Leri kullanma, HTTP istekleri yapma ve HTTP yanıtlarını işleme hakkında daha fazla bilgi edinmek için [Azure REST API başvurusunu](https://docs.microsoft.com/rest/api/azure/) okuyun.

### <a name="authentication"></a>Kimlik Doğrulaması

[TIME SERIES INSIGHTS REST API 'lerinde](https://docs.microsoft.com/rest/api/time-series-insights/)kimliği doğrulanmış sorgular gerçekleştirmek için, seçtiğiniz Rest Istemcisi (Postman, JavaScript, C#) kullanılarak [Yetkilendirme üst bilgisinde](/rest/api/apimanagement/2019-01-01/authorizationserver/createorupdate) geçerli bir OAuth 2,0 taşıyıcı belirtecinin geçirilmesi gerekir. 

> [!TIP]
> [JavaScript istemci SDK 'sını](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) grafik ve grafiklerle birlikte kullanarak Time Series Insights API 'lerinde program aracılığıyla nasıl kimlik doğrulaması yapılacağını öğrenmek için barındırılan Azure TIME SERIES INSIGHTS [istemci SDK örnek görselleştirmesini](https://tsiclientsample.azurewebsites.net/) okuyun.

### <a name="http-headers"></a>HTTP üstbilgileri

Gerekli istek üstbilgileri aşağıda açıklanmıştır.

| Gerekli istek üst bilgisi | Açıklama |
| --- | --- |
| Yetkilendirme | Time Series Insights kimlik doğrulaması yapmak için, **Yetkilendirme** üst bilgisinde geçerli bir OAuth 2,0 taşıyıcı belirtecinin geçirilmesi gerekir. | 

> [!IMPORTANT]
> Belirtecin `https://api.timeseries.azure.com/` kaynağına tam olarak verilmesi gerekir (Ayrıca belirtecin "hedef kitle" olarak da bilinir).
> * [Postman](https://www.getpostman.com/) **AuthUrl** 'niz şu şekilde olacaktır: `https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/`
> * `https://api.timeseries.azure.com/` geçerlidir ancak `https://api.timeseries.azure.com` değildir.

İsteğe bağlı istek üstbilgileri aşağıda açıklanmıştır.

| İsteğe bağlı istek üst bilgisi | Açıklama |
| --- | --- |
| İçerik türü | yalnızca `application/json` desteklenir. |
| x-MS-Client-Request-ID | İstemci istek KIMLIĞI. Hizmet bu değeri kaydeder. Hizmetin işlemleri hizmetler arasında izlemesini sağlar. |
| x-MS-Client-Session-ID | İstemci oturum KIMLIĞI. Hizmet bu değeri kaydeder. Hizmetin, hizmetler arasında bir grup işlem izlemesini sağlar. |
| x-MS-Client-Application-Name | Bu isteği oluşturan uygulamanın adı. Hizmet bu değeri kaydeder. |

İsteğe bağlı ancak önerilen yanıt üstbilgileri aşağıda açıklanmıştır.

| Yanıt üst bilgisi | Açıklama |
| --- | --- |
| İçerik türü | yalnızca `application/json` desteklenir. |
| x-MS-istek kimliği | Sunucu tarafından oluşturulan istek KIMLIĞI. , Bir isteği araştırmak üzere Microsoft 'a başvurmak için kullanılabilir. |
| x-MS-Property-Found-Behavior | GA API isteğe bağlı yanıt üst bilgisi. Olası değerler `ThrowError` (varsayılan) veya `UseNull`. |

### <a name="http-parameters"></a>HTTP parametreleri

> [!TIP]
> [Başvuru belgelerindeki](https://docs.microsoft.com/rest/api/time-series-insights/)gerekli ve isteğe bağlı sorgu bilgileri hakkında daha fazla bilgi edinin.

Gerekli URL sorgu dizesi parametreleri API sürümüne bağımlıdır.

| Yayınlayın | Olası API sürümü değerleri |
| --- |  --- |
| Genel Erişilebilirlik | `api-version=2016-12-12`|
| Önizleme | `api-version=2018-11-01-preview` |
| Önizleme | `api-version=2018-08-15-preview` |

İsteğe bağlı URL sorgu dizesi parametreleri, HTTP istek yürütme süreleri için bir zaman aşımı ayarlamayı içerir.

| İsteğe bağlı sorgu parametresi | Açıklama | Sürüm |
| --- |  --- | --- |
| `timeout=<timeout>` | HTTP istek yürütmesi için sunucu tarafı zaman aşımı. Yalnızca [ortam olaylarını al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api) ve [ortam toplamlarını al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api) API 'leri için geçerlidir. Zaman aşımı değeri ISO 8601 Duration biçiminde olmalıdır, örneğin `"PT20S"` ve `1-30 s`aralığında olmalıdır. Varsayılan değer `30 s`. | Genel Kullanım |
| `storeType=<storeType>` | Sıcak mağaza özellikli önizleme ortamları için, sorgu `WarmStore` veya `ColdStore`yürütülür. Sorgudaki bu parametre, sorgunun hangi depoda yürütülmesi gerektiğini tanımlar. Tanımlı değilse, sorgu soğuk depoda yürütülür. Isınma mağazasını sorgulamak için, **Stoyeniden yazma** 'nın `WarmStore`olarak ayarlanması gerekir. Tanımlı değilse, sorgu soğuk depoya karşı yürütülür. | Önizleme |

## <a name="next-steps"></a>Sonraki adımlar

- GA Time Series Insights API 'sini çağıran örnek kod için, [ C#kullanarak sorgu verilerini ](./time-series-insights-query-data-csharp.md)okuyun.

- Preview Time Series Insights API kodu örnekleri için [kullanarak C#sorgu önizleme verilerini ](./time-series-insights-update-query-data-csharp.md)okuyun.

- API başvuru bilgileri için [sorgu API 'si başvuru](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api) belgelerini okuyun.

- [Hizmet sorumlusu oluşturmayı](../active-directory/develop/howto-create-service-principal-portal.md)öğrenin.
