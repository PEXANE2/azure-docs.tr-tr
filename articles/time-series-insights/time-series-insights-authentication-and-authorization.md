---
title: API kimlik doğrulama ve yetkilendirme - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Bu makalede, Azure Zaman Serisi Öngörüleri API'sini çağıran özel bir uygulama için kimlik doğrulaması ve yetkilendirmenasıl yapılandırılabilen açıklanmaktadır.
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
ms.openlocfilehash: 373fc2829e599d0989b59c6386757c8f5f3e1d66
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81251733"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Azure Time Series Insights API’si için kimlik doğrulaması ve yetkilendirme

Bu belge, yeni Azure Active Directory bıçağını kullanarak bir uygulamanın Azure Etkin Dizini'nde nasıl kaydedilen olduğunu açıklar. Azure Active Directory'de kayıtlı uygulamalar, kullanıcıların Time Series Insights ortamıyla ilişkili Azure Zaman Serisi Öngörü API'sini kimlik doğrulamasına ve kullanma yetkisine sahip olmasını sağlar.

> [!IMPORTANT]
> Azure Time Series Öngörüleri, aşağıdaki kimlik doğrulama kitaplıklarının her ikisini de destekler:
> * Daha yeni [Microsoft Kimlik Doğrulama Kitaplığı (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)
> * [Azure Etkin Dizin Kimlik Doğrulama Kitaplığı (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="service-principal"></a>Hizmet sorumlusu

Aşağıdaki bölümlerde, bir uygulama adına Time Series Insights API'sine erişmek için bir uygulamanın nasıl yapılandırılabildiğini açıklayınız. Uygulama daha sonra Azure Active Directory aracılığıyla kendi uygulama kimlik bilgilerini kullanarak Zaman Serisi Öngörüleri ortamında başvuru verilerini sorgulayabilir veya yayımlayabilir.

## <a name="summary-and-best-practices"></a>Özet ve en iyi uygulamalar

Azure Active Directory uygulama kayıt akışı üç ana adımı içerir.

1. Azure Active Directory'de [bir uygulama kaydedin.](#azure-active-directory-app-registration)
1. [Zaman Serisi Öngörüler ortamına veri erişimi](#granting-data-access)için uygulama yetkilendirin.
1. [Müşteri uygulamanızdan](#client-app-initialization)bir belirteç `https://api.timeseries.azure.com/` elde etmek için Uygulama **Kimliği** ve **İstemci Sırrı'nı** kullanın. Belirteç daha sonra Zaman Serisi Öngörüleri API'sını aramak için kullanılabilir.

**Adım 3**başına, uygulamanızın ve kullanıcı kimlik bilgilerinizin ayrılması şunları yapmanızı sağlar:

* Kendi izinlerinizden farklı olan uygulama kimliğine izinler atayın. Genellikle, bu izinler yalnızca uygulamanın gerektirdiği şeylerle sınırlıdır. Örneğin, uygulamanın verileri yalnızca belirli bir Zaman Serisi Öngörüleri ortamından okumasına izin verebilirsiniz.
* **Bir İstemci Gizli** veya güvenlik sertifikası kullanarak uygulamanın güvenliğini, kullanıcının kimlik doğrulama kimlik bilgilerini oluşturan dan yalıtın. Sonuç olarak, uygulamanın kimlik bilgileri belirli bir kullanıcının kimlik bilgilerine bağlı değildir. Kullanıcının rolü değişirse, uygulama mutlaka yeni kimlik bilgileri veya daha fazla yapılandırma gerektirmez. Kullanıcı parolasını değiştirirse, uygulamaya tüm erişim yeni kimlik bilgileri veya anahtar lar gerektirmez.
* Belirli bir kullanıcının kimlik bilgileri yerine Bir **İstemci Gizli** veya güvenlik sertifikası kullanarak gözetimsiz bir komut dosyası çalıştırın (bunların bulunmasını zorunlu kılması).
* Azure Time Series Insights API'nize erişimi güvence altına almak için parola yerine güvenlik sertifikası kullanın.

> [!IMPORTANT]
> Azure Time Series Insights güvenlik ilkenizi yapılandırırken **Endişelerin Ayrılması** ilkesini (yukarıdaki senaryoda açıklanan) uygulayın.

> [!NOTE]
> * Makale, uygulamanın yalnızca bir kuruluşta çalışması amaçlanan tek kiracılı bir uygulamaya odaklanır.
> * Genellikle kuruluşunuzda çalışan iş yeri uygulamaları için tek kiracılı uygulamalar kullanırsınız.

## <a name="detailed-setup"></a>Ayrıntılı kurulum

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory uygulama kaydı

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Veri erişimi verme

1. Time Series Öngörüleri ortamı için **Veri Erişim İlkeleri'ni** seçin ve **Ekle'yi**seçin.

   [![Time Series Insights ortamına yeni veri erişim ilkesi ekleme](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Kullanıcı **Seç** iletişim kutusunda, Azure Active Directory uygulama kayıt bölümünden **Uygulama Adı** veya **Uygulama Kimliği'ni** yapıştırın.

   [![Kullanıcı Seç iletişim kutusunda bir uygulama bulma](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rolü seçin. Verileri sorgulamak için **Reader'ı** veya **Katılımcıyı** sorgulamak ve referans verilerini değiştirmek için Reader'ı seçin. **Tamam'ı**seçin.

   [![Kullanıcı Rolü Seç iletişim kutusunda Okuyucu veya Katılımcı seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. **Tamam'ı**seçerek ilkeyi kaydedin.

   > [!TIP]
   > Gelişmiş veri erişim seçenekleri için veri [erişimi verme'yi](./time-series-insights-data-access.md)okuyun.

### <a name="client-app-initialization"></a>İstemci uygulaması başlatma

* Geliştiriciler, Azure Zaman Serisi Öngörüleri ile kimlik doğrulaması yapmak için [Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) veya [Azure Etkin Dizin Kimlik Doğrulama Kitaplığını (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) kullanabilir.

* Örneğin, ADAL kullanarak kimlik doğrulaması yapmak için:

   1. Uygulama adına belirteci elde etmek için Azure Active Directory uygulama kayıt bölümündeki **Uygulama Kimliği** ve **İstemci Sırrı** (Uygulama Anahtarı) kullanın.

   1. C#'da, aşağıdaki kod başvuru adına belirteci edinebilir. Tam bir örnek için [C# kullanarak Sorgu verilerini](time-series-insights-query-data-csharp.md)okuyun.

        [!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs?range=170-199)]

   1. Uygulama Zaman Serisi Öngörüleri `Authorization` API'sini aradığında belirteç üstbilgiden geçirilebilir.

* Alternatif olarak, geliştiriciler MSAL kullanarak kimlik doğrulaması yapmayı seçebilir. Daha fazla bilgi edinmek için C# makalesini kullanarak [MSAL'a geçiş](https://docs.microsoft.com/azure/active-directory/develop/msal-net-migration) hakkında bilgi edinin ve [Azure Zaman Serisi Öngörüleri ortamı için GA başvuru verilerini yönet'e](time-series-insights-manage-reference-data-csharp.md) bakın. 

## <a name="common-headers-and-parameters"></a>Ortak üstbilgi ve parametreler

Bu bölümde, Zaman Serisi Öngörüleri GA ve Önizleme API'lerine karşı sorgu yapmak için kullanılan yaygın HTTP istek üstbilgileri ve parametreleri açıklanmaktadır. API'ye özel gereksinimler Time [Series Insights REST API referans belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/)daha ayrıntılı olarak ele alınmıştır.

> [!TIP]
> REST API'leri nasıl tüketileyeceğiniz, HTTP isteklerini nasıl yapacağınız ve HTTP yanıtlarını nasıl işleyeceğiniz hakkında daha fazla bilgi edinmek için [Azure REST API Başvurusu'nu](https://docs.microsoft.com/rest/api/azure/) okuyun.

### <a name="authentication"></a>Kimlik Doğrulaması

[Zaman Serisi Öngörüleri REST API'lerine](https://docs.microsoft.com/rest/api/time-series-insights/)karşı kimlik doğrulaması yapılan sorguları gerçekleştirmek için, seçtiğiniz bir REST istemcisi (Postacı, JavaScript, C#) kullanılarak [Yetkilendirme üstbilgisinde](/rest/api/apimanagement/2019-12-01/authorizationserver/createorupdate) geçerli bir OAuth 2.0 taşıyıcı belirteci geçilmelidir. 

> [!TIP]
> Zaman Serisi Öngörüleri API'leri ile grafik ve grafiklerin yanı sıra [JavaScript İstemci SDK'sını](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) programlı bir şekilde kullanarak kimlik doğrulaması yapmayı öğrenmek için barındırılan Azure Time Series Insights [istemciSi SDK örnek görselleştirmesini](https://tsiclientsample.azurewebsites.net/) okuyun.

### <a name="http-headers"></a>HTTP üstbilgi

Gerekli istek üstbilgisi aşağıda açıklanmıştır.

| Gerekli istek üstbilgi | Açıklama |
| --- | --- |
| Yetkilendirme | Time Series Insights ile kimlik doğrulaması yapmak için **Yetkilendirme** başlığında geçerli bir OAuth 2.0 Taşıyıcı belirteci geçilmelidir. | 

> [!IMPORTANT]
> Belirteç tam olarak `https://api.timeseries.azure.com/` kaynağa verilmelidir (belirteç "hedef kitle" olarak da bilinir).
> * [Postacı](https://www.getpostman.com/) **AuthURL** bu nedenle olacaktır:`https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/`
> * `https://api.timeseries.azure.com/`geçerlidir `https://api.timeseries.azure.com` ancak geçerli değildir.

İsteğe bağlı istek üstbilgisi aşağıda açıklanmıştır.

| İsteğe bağlı istek üst bilgisi | Açıklama |
| --- | --- |
| İçerik türü | yalnızca `application/json` desteklenir. |
| x-ms-istemci-istek-id | İstemci, kimlik isteğinde. Hizmet bu değeri kaydeder. Hizmetin hizmetler arasında işlemi izlemesini sağlar. |
| x-ms-istemci-oturum-id | İstemci oturum kimliği. Hizmet bu değeri kaydeder. Hizmetin hizmetler arasında ilgili bir grup işlemi izlemesine olanak tanır. |
| x-ms-istemci-uygulama-adı | Bu isteği oluşturan uygulamanın adı. Hizmet bu değeri kaydeder. |

İsteğe bağlı ancak önerilen yanıt üstbilgisi aşağıda açıklanmıştır.

| Yanıt üstbilgi | Açıklama |
| --- | --- |
| İçerik türü | Yalnızca `application/json` desteklenir. |
| x-ms-istek-id | Sunucu tarafından oluşturulan istek kimliği. Bir isteği araştırmak için Microsoft'a başvurmak için kullanılabilir. |
| x-ms-özellik-bulunamadı-davranış | GA API isteğe bağlı yanıt üstbilgi. Olası değerler `ThrowError` (varsayılan) `UseNull`veya . |

### <a name="http-parameters"></a>HTTP parametreleri

> [!TIP]
> [Başvuru belgelerinde](https://docs.microsoft.com/rest/api/time-series-insights/)gerekli ve isteğe bağlı sorgu bilgileri hakkında daha fazla bilgi edinin.

Gerekli URL sorgu dize parametreleri API sürümüne bağlıdır.

| Yayınla | Olası API sürüm değerleri |
| --- |  --- |
| Genel Kullanılabilirlik | `api-version=2016-12-12`|
| Önizleme | `api-version=2018-11-01-preview` |
| Önizleme | `api-version=2018-08-15-preview` |

İsteğe bağlı URL sorgu dize parametreleri, HTTP isteği yürütme süreleri için bir zaman ayarı içerir.

| İsteğe bağlı sorgu parametresi | Açıklama | Sürüm |
| --- |  --- | --- |
| `timeout=<timeout>` | HTTP isteği yürütmesi için sunucu tarafı zaman ayarı. Yalnızca Ortam [Etkinlikleri Al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api) ve [Çevre Toplama API'leri alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api) için geçerlidir. Zaman aralığı değeri ISO 8601 süre biçiminde `"PT20S"` olmalıdır, örneğin `1-30 s`ve aralığında olmalıdır. Varsayılan değer. `30 s` | GA |
| `storeType=<storeType>` | Sıcak depo etkinleştirilmiş Önizleme ortamları için sorgu, `WarmStore` `ColdStore`ya da . Sorgudaki bu parametre, sorgunun hangi depoda yürütülmesi gerektiğini tanımlar. Tanımlanmamışsa, sorgu soğuk depoda yürütülür. Sıcak depoyu sorgulamak için **storeType'ın** `WarmStore`'' ' a ayarlanması gerekir. Tanımlanmamışsa, sorgu soğuk depoya karşı yürütülür. | Önizleme |

## <a name="next-steps"></a>Sonraki adımlar

- GA Time Series Insights API çağıran örnek kod için [C# kullanarak Sorgu verilerini](./time-series-insights-query-data-csharp.md)okuyun.

- Önizleme Zamanı Serisi Öngörüleri API kod örnekleri için [C# kullanarak Sorgu Önizleme verilerini](./time-series-insights-update-query-data-csharp.md)okuyun.

- API başvuru bilgileri için [Sorgu API başvuru](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api) belgelerini okuyun.

- [Hizmet ilkesini](../active-directory/develop/howto-create-service-principal-portal.md)nasıl oluşturabilirsiniz öğrenin.
