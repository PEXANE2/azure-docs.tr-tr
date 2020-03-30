---
title: SaaS Karşılama API'leri v1 | Azure Marketi
description: Azure Marketi'nde ilişkili Yerine Getirme v1 API'lerini kullanarak Bir SaaS teklifinin nasıl oluşturulup yönetilenini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 05/23/2019
ms.author: dsindona
ROBOTS: NOINDEX
ms.openlocfilehash: 3ec8373288a2ea5809ee5d349c52c57051586035
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288351"
---
# <a name="saas-fulfillment-apis-version-1-deprecated"></a>SaaS Karşılama API'leri sürüm 1 (amortismana ermiş)

Bu makalede, API'ler ile bir SaaS teklifi oluşturmak için nasıl açıklanır. REST yöntemleri ve uç noktalardan oluşan API'ler, Azure üzerinden satış seçtiyseniz, SaaS teklifinize abonelik izni vermek için gereklidir.  

> [!WARNING]
> SaaS Karşılama API'sinin bu ilk sürümü amortismana katif edilmiştir; bunun yerine, [SaaS Karşılama API V2](./pc-saas-fulfillment-api-v2.md)kullanın.  API'nin bu ilk sürümü şu anda yalnızca varolan yayıncılara hizmet vermek için korunuyor. 

Aşağıdaki API'ler, SaaS hizmetinizi Azure ile tümleştirmenize yardımcı olmak için sağlanır:

-   Çözümleme
-   Abone Ol
-   Dönüştür
-   Abonelikten ayrıl


## <a name="api-methods-and-endpoints"></a>API yöntemleri ve uç noktaları

Aşağıdaki bölümlerde, SaaS teklifi için abonelikleri etkinleştirmek için kullanılabilen API yöntemleri ve uç noktaları açıklanmıştır.


### <a name="marketplace-api-endpoint-and-api-version"></a>Marketplace API bitiş noktası ve API sürümü

Azure Marketi API'si için bitiş `https://marketplaceapi.microsoft.com`noktası.

Geçerli API sürümü `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Aboneliği çözme

Son noktayı çözümle üzerine yapılan sonrası eylem, kullanıcıların kalıcı bir Kaynak Kimliği'ne yönelik bir pazar yeri belirteci sorununu çözmelerine olanak tanır.  Kaynak Kimliği, SAAS aboneliği için benzersiz tanımlayıcıdır. 

Bir kullanıcı bir ISV'nin web sitesine yönlendirildiğinde, URL sorgu parametrelerinde bir belirteç içerir. ISV'nin bu belirteci kullanması ve çözümiçin bir istekte bulunması beklenir. Yanıt, benzersiz SAAS abonelik kimliği, adı, teklif kimliği ve kaynak için plan içerir. Bu belirteç yalnızca bir saat için geçerlidir.

*İstek*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parametre Adı** |     **Açıklama**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-sürümü        |  Bu istek için kullanılacak işlemin sürümü.   |
|  |  |


*Üst Bilgiler*

| **Üstbilgi tuşu**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
| x-ms-korelasyonid | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu alan, istemci çalışmasından tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| İçerik türü       | Evet          | `application/json`                                        |
| yetkilendirme      | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                    |
| x-ms-marketplace-token| Evet| Kullanıcı Azure'dan SaaS ISV'nin web sitesine yönlendirildiğinde URL'deki belirteç sorgu parametresi. **Not:** Bu belirteç yalnızca 1 saat için geçerlidir. Ayrıca, URL kullanmadan önce tarayıcıdan belirteç değerini deşifre.|
|  |  |  |
  

*Yanıt Gövdesi*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                       |
|--------------------|---------------|---------------------------------------|
| id                 | Dize        | SaaS aboneliğinin kimliği.          |
| subscriptionName| Dize| SaaS hizmetine abone yken Azure'da kullanıcı tarafından ayarlanan SaaS aboneliğinin adı.|
| OfferId            | Dize        | Kullanıcının abone olduğu teklif kimliği. |
| planId             | Dize        | Kullanıcının abone olduğu plan kimliği.  |
|  |  |  |


*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token başarıyla çözüldü.                                                            |
| 400                  | `BadRequest`         | Gerekli üstbilgi eksik veya geçersiz bir api sürümü belirtilir. Belirteç bozuk veya süresi dolduğu için belirteç giderilemedi (belirteç oluşturulduktan sonra yalnızca 1 saat için geçerlidir). |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                                 |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme meşgul, daha sonra yeniden deneyin.                                |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak düştü, daha sonra yeniden deneyin.                                        |
|  |  |  |


*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu değer sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu kimlik, tüm mutabakatlar için kullanılır. |
| Yeniden Deneyin-Sonra        | Hayır           | Bu değer yalnızca 429 yanıtı için ayarlanır.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abone Ol

Abone bitiş noktası, kullanıcıların belirli bir plan için bir SaaS hizmetine abonelik başlatmalarına ve ticaret sisteminde faturalandırmayı etkinleştirmelerine olanak tanır.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Çözüm API'si aracılığıyla belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz kimliği.                              |
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

|  **Üstbilgi tuşu**        | **Gerekli** |  **Açıklama**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Hayır         | İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır. |
| x-ms-korelasyonid     |   Hayır         | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır. |
| if-match/if-none-match |   Hayır         |   Güçlü doğrulayıcı ETag değeri.                                                          |
| içerik türü           |   Evet        |    `application/json`                                                                   |
|  yetkilendirme         |   Evet        |    JSON web belirteci (JWT) taşıyıcı belirteci.                                               |
| x-ms-marketplace-oturum-modu| Hayır | SaaS teklifine abone olurken kuru çalıştırma modunu etkinleştirmek için bayrak işaretleyin. Ayarlanırsa, abonelik ücretlendirilmez. Bu, ISV sınama senaryoları için yararlıdır. Lütfen **'dryrun'** olarak ayarlayın|
|  |  |  |

*Gövde*

``` json
{
    "lanId": "",
}
```

| **Öğe adı** | **Veri türü** | **Açıklama**                      |
|------------------|---------------|--------------------------------------|
| planId           | (Gerekli) Dize        | SaaS hizmet kullanıcısının plan kimliği abone oluyor.  |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme belirli bir plan için alınan.                   |
| 400                  | `BadRequest`         | Gerekli başlıklar eksik ya da JSON'un gövdesi bozuk. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                   |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı                                  |
| 409                  | `Conflict`           | Abonelikle ilgili başka bir işlem devam ediyor.                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak düştü, daha sonra yeniden deneyin.                          |
|  |  |  |

202 yanıtı için, istek işleminin durumunu 'İşlem konumu' başlığından izleyin. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu değer sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu değer herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Evet          | İstemcinin durumu denetleyebileceği aralık.                                                       |
| Operasyon-Konum | Evet          | İşlem durumunu almak için kaynağa bağlantı kurun.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Plan bitiş noktasını değiştirme

Değişiklik bitiş noktası, kullanıcının şu anda abone olduğu planı yeni bir plana dönüştürmesine olanak tanır.

**Yama**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS aboneliğinin kimliği.                              |
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi tuşu**          | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri. Bir GUID önerin. Bu sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır.   |
| x-ms-korelasyonid      | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır. |
| if-match /if-none-match | Hayır           | Güçlü doğrulayıcı ETag değeri.                              |
| içerik türü            | Evet          | `application/json`                                        |
| yetkilendirme           | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                    |
|  |  |  |

*Gövde*

```json
{
    "planId": ""
}
```

|  **Öğe adı** |  **Veri türü**  | **Açıklama**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (Gerekli) Dize         | SaaS hizmet kullanıcısının plan kimliği abone oluyor.          |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme belirli bir plan için alınan.                   |
| 400                  | `BadRequest`         | Gerekli başlıklar eksik ya da JSON'un gövdesi bozuk. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                   |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı                                  |
| 409                  | `Conflict`           | Abonelikle ilgili başka bir işlem devam ediyor.                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak düştü, daha sonra yeniden deneyin.                          |
|  |  |  |

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu değer sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu değer herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Evet          | İstemcinin durumu denetleyebileceği aralık.                                                       |
| Operasyon-Konum | Evet          | İşlem durumunu almak için kaynağa bağlantı kurun.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Aboneliği silme

Abone bitiş noktasındaki Silme eylemi, kullanıcının belirli bir kimlikle aboneliği silmesine olanak tanır.

*İstek*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS aboneliğinin kimliği.                              |
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi tuşu**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri. Bir GUID önerin. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.                                                           |
| x-ms-korelasyonid | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır. |
| yetkilendirme      | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                    |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme belirli bir plan için alınan.                   |
| 400                  | `BadRequest`         | Gerekli başlıklar eksik ya da JSON'un gövdesi bozuk. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                   |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı                                  |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme ile meşgul, lütfen daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Servis geçici olarak düştü. Lütfen daha sonra tekrar deneyin.                          |
|  |  |  |

202 yanıtı için, istek işleminin durumunu 'İşlem konumu' başlığından izleyin. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu, herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Evet          | İstemcinin durumu denetleyebileceği aralık.                                                       |
| Operasyon-Konum | Evet          | İşlem durumunu almak için kaynağa bağlantı kurun.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>İşletim Durumu Alma

Bu uç nokta, kullanıcının tetiklenen bir async işleminin durumunu izlemesine olanak tanır (Abone/Aboneliği Kaldır/Değiştir planı).

*İstek*

**Al**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{operationId}*?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Başlatılan işlem için benzersiz kimlik.                |
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi tuşu**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri. Bir GUID önerin. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.   |
| x-ms-korelasyonid | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
| yetkilendirme      | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                    |
|  |  |  | 

*Yanıt Gövdesi*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | Dize        | Operasyonun kimliği.                                                                      |
| durum             | Sabit Listesi          | Çalışma durumu, aşağıdakilerden `In Progress`biri: , , `Succeeded`veya `Failed`.          |
| resourceLocation   | Dize        | Oluşturulan veya değiştirilen abonelmeye bağlantı. Bu, istemcinin güncelleştirilmiş durum sonrası işlemi elde etmek için yardımcı olur. Bu değer işlemler `Unsubscribe` için ayarlanmaz. |
| Oluşturulan            | DateTime      | UTC'de operasyon oluşturma süresi.                                                           |
| Lastmodified       | DateTime      | UTC'deki operasyonla ilgili son güncelleme.                                                      |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Başarılı bir şekilde alma isteği çözüldü ve gövde yanıtı içerir.    |
| 400                  | `BadRequest`         | Gerekli üstbilgi eksik veya geçersiz bir api sürümü belirtilmiştir. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                      |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı.                                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak düştü, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu, herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Evet          | İstemcinin durumu denetleyebileceği aralık.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Abonelik Alın

Abone bitiş noktasında ki get eylemi, kullanıcının belirli bir kaynak tanımlayıcısı olan bir aboneliği almasına olanak tanır.

*İstek*

**Al**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | SaaS aboneliğinin kimliği.                              |
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi tuşu**     | **Gerekli** | **Açıklama**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.                                                           |
| x-ms-korelasyonid | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| yetkilendirme      | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                                                                    |
|  |  |  |

*Yanıt Gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | Dize        | Azure'daki SaaS abonelik kaynağının kimliği.    |
| offerId                | Dize        | Kullanıcının abone olduğu teklif kimliği.         |
| planId                 | Dize        | Kullanıcının abone olduğu plan kimliği.          |
| saasSubscriptionName   | Dize        | SaaS aboneliğinin adı.                |
| saasSubscriptionStatus | Sabit Listesi          | Operasyon durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkindir.  <br/> - `Pending`: Kullanıcı kaynağı oluşturur, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı aboneliğini iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`: Azure aboneliği askıya alındı.  |
| Oluşturulan                | DateTime      | UTC'de abonelik oluşturma zaman damgası değeri. |
| Lastmodified           | DateTime      | UTC'de abonelik değiştirilen zaman damgası değeri. |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Başarılı bir şekilde alma isteği çözüldü ve gövde yanıtı içerir.    |
| 400                  | `BadRequest`         | Gerekli üstbilgi eksik veya geçersiz bir api sürümü belirtilmiştir. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                      |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak düştü, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu, herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Hayır           | İstemcinin durumu denetleyebileceği aralık.                                                       |
| Etag               | Evet          | İşlem durumunu almak için kaynağa bağlantı kurun.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Abonelikleri Alın

Abonelikler bitiş noktası yla ilgili alın eylemi, kullanıcının ISV'den gelen tüm teklifleriçin tüm abonelikleri almasına olanak tanır.

*İstek*

**Al**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Parametre Adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| api-sürümü         | Bu istek için kullanılacak işlemin sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi tuşu**     | **Gerekli** | **Açıklama**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden gelen isteği izlemek için benzersiz bir dize değeri. Bir GUID önerin. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.             |
| x-ms-korelasyonid | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, istemci çalışmasındaki tüm olayları sunucu tarafındaki olaylarla ilişkilendirme içindir. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| yetkilendirme      | Evet          | JSON web belirteci (JWT) taşıyıcı belirteci.                    |
|  |  |  |

*Yanıt Gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | Dize        | Azure'daki SaaS abonelik kaynağının kimliği    |
| offerId                | Dize        | Kullanıcının abone olduğu teklif kimliği         |
| planId                 | Dize        | Kullanıcının abone olduğu plan kimliği          |
| saasSubscriptionName   | Dize        | SaaS aboneliğinin adı                |
| saasSubscriptionStatus | Sabit Listesi          | Operasyon durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkindir.  <br/> - `Pending`: Kullanıcı kaynağı oluşturur, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı aboneliğini iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`: Azure aboneliği askıya alındı.  |
| Oluşturulan                | DateTime      | UTC'de abonelik oluşturma zaman damgası değeri |
| Lastmodified           | DateTime      | UTC'de abonelik değiştirilen zaman damgası değeri |
|  |  |  |

*Yanıt Kodları*

| **HTTP Durum Kodu** | **Hata Kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Başarılı bir şekilde alma isteği çözüldü ve gövde yanıtı içerir.    |
| 400                  | `BadRequest`         | Gerekli üstbilgi eksik veya geçersiz bir api sürümü belirtilmiştir. |
| 403                  | `Forbidden`          | Arayan bu işlemi gerçekleştirmek için yetkili değildir.                      |
| 404                  | `NotFound`           | Verilen kimlikle birlikte abonelik bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işleme ile meşgul, lütfen daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Servis geçici olarak düştü. Lütfen daha sonra tekrar deneyin.                             |
|  |  |  |

*Yanıt Üst Bilgileri*

| **Üstbilgi Anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstemciden alınan istek kimliği.                                                                   |
| x-ms-korelasyonid | Evet          | Bağıntılı Kimlik istemci tarafından geçti, aksi takdirde bu sunucu korelasyon kimliğidir.                   |
| x-ms-activityid    | Evet          | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu, herhangi bir mutabakat için kullanılır. |
| Yeniden Deneyin-Sonra        | Hayır           | İstemcinin durumu denetleyebileceği aralık.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Webhook

SaaS webhook, değişiklikleri SaaS hizmetine proaktif olarak bildirmek için kullanılır. Bu POST API'nin kimliği doğrulanmamış olması beklenir ve Microsoft hizmeti tarafından çağrılacaktır. SaaS hizmetinin, webhook bildirimi nde işlem yapmadan önce doğrulamak ve yetkilendirmek için operasyon API'sini çağırması beklenir. 

*Gövde*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | Dize       | Başlatılan işlem için benzersiz kimlik.                |
| Activityıd   | Dize        | Hizmetten gelen isteği izlemek için benzersiz bir dize değeri. Bu, herhangi bir mutabakat için kullanılır.               |
| subscriptionId                     | Dize        | Azure'daki SaaS abonelik kaynağının kimliği.    |
| offerId                | Dize        | Kullanıcının abone olduğu teklif kimliği. Yalnızca "Güncelleştirme" eylemiyle sağlanır.        |
| publisherId                | Dize        | SaaS teklifinin yayıncı kimliği         |
| planId                 | Dize        | Kullanıcının abone olduğu plan kimliği. Yalnızca "Güncelleştirme" eylemiyle sağlanır.          |
| action                 | Dize        | Bu bildirimi tetikleyen eylem. Olası değerler - Etkinleştirme, Silme, Askıya Alma, Geri Getirme, Güncelleme          |
| Zaman damgası                 | Dize        | Bu bildirim tetiklendiğinde UTC'deki TImestamp değeri.          |
|  |  |  |


## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca [Bulut Ortağı Portal REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview)kullanarak iş yüklerini, teklifleri ve yayıncı profillerini programlı bir şekilde alabilir ve iş yüklerini, teklifleri ve yayıncı profillerini kullanabilir.
