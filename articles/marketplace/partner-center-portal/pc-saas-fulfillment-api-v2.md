---
title: SaaS Karşılama API v2 | Azure Marketi
description: Bu makalede, ilişkili yerine getirme v2 API'lerini kullanarak AppSource ve Azure Marketi'nde bir SaaS teklifinin nasıl oluşturulup yönetilen açıklanmaktadır.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 10/18/2019
ms.author: dsindona
ms.openlocfilehash: ca49418013357ecaae62ea5e91374eaa1cbde59d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275773"
---
# <a name="saas-fulfillment-apis-version-2"></a>SaaS gerçekleştirme API'leri, sürüm 2 

Bu makalede, iş ortaklarının SaaS uygulamalarını AppSource marketinde ve Azure Marketi'nde satabilmelerine olanak tanıyan API'ler ayrıntılarıyla anlatılır. Bu API'ler AppSource ve Azure Marketi'nde işlenebilir SaaS teklifleri için bir gerekliliktir.

## <a name="managing-the-saas-subscription-life-cycle"></a>SaaS abonelik yaşam döngüsünü yönetme

Azure SaaS, Bir SaaS abonelik satın alma işleminin tüm yaşam döngüsünü yönetir. Gerçek yerine getirilmesi, planlarda değişiklik ve ortakla aboneliğin silinmesini sağlamak için yerine getirme API'lerini bir mekanizma olarak kullanır. Müşterinin faturası, Microsoft'un koruduğu SaaS aboneliğinin durumuna bağlıdır. Aşağıdaki diyagram, durumlar arasındaki değişiklikleri yönlendiren durumları ve işlemleri görüntüler.

![SaaS abonelik yaşam döngüsü durumları](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>SaaS aboneliğinin durumları

Aşağıdaki tabloda, her biri için bir açıklama ve sıralı diyagram da dahil olmak üzere (varsa) bir SaaS aboneliği için hüküm durumları listelenir. 

#### <a name="provisioning"></a>Sağlama

Bir müşteri satın alma başlattığında, iş ortağı bu bilgileri URL parametresi kullanan müşteri etkileşimli bir web sayfasındaki yetkilendirme kodunda alır. İş Ortağı `https://contoso.com/signup?token=..`Merkezi'ndeki açılış sayfası URL'si `https://contoso.com/signup`ise bir örnektir. Yetkilendirme kodu, ÇÖZÜM API'sini arayarak doğrulanabilir ve sağlama hizmetinin ayrıntılarıyla değiştirilebilir.  Bir SaaS hizmeti sağlamayı bitirdiğinde, yerine getirmenin tamamlanıp müşterinin faturalandırılabildiği sinyalini vermek için bir etkinleştirme çağrısı gönderir. 

Aşağıdaki diyagram, bir sağlama senaryosu için API çağrılarının sırasını gösterir.  

![API bir SaaS hizmeti sağlanması için çağrıda bulunuyor](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Sağlan

Bu durum, sağlanan bir hizmetin sabit durumudur.

##### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama 

Bu durum, varolan bir hizmetiçin bir güncelleştirme beklemede olduğunu belirtir. Bu tür bir güncelleştirme müşteri tarafından pazardan veya SaaS hizmetinden başlatılabilir (yalnızca doğrudan müşteriye yapılan işlemler için).

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Güncelleştirme için sağlama (pazardan başlatıldığında)

Aşağıdaki diyagram, pazar yerinde bir güncelleştirme başlatıldığında eylemlerin sırasını gösterir.

![Güncelleştirme pazardan başlatıldığında API çağrıları](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>Güncelleştirme için sağlama (SaaS hizmetinden başlatıldığında)

Aşağıdaki diyagram, SaaS hizmetinden bir güncelleştirme başlatıldığında ki eylemleri gösterir. (Webhook araması, SaaS hizmeti tarafından başlatılan abonelik güncelleştirmesi ile değiştirilir.) 

![Güncelleştirme SaaS hizmetinden başlatıldığında API çağırır](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Askıya alındı

Bu durum, müşterinin ödemesinin alınmadığını gösterir. İlke olarak, müşteriye aboneliği iptal etmeden önce bir yetkisiz kullanım süresi sağlarız. Bir abonelik bu durumdaolduğunda: 

- İş ortağı olarak, kullanıcının hizmete erişimini düşürmeyi veya engellemeyi seçebilirsiniz.
- Abonelik, veri veya ayar kaybı olmadan tam işlevselliği geri yükleyebilecek kurtarılabilir bir durumda tutulmalıdır. 
- Yetkisiz kullanım AP'leri veya yetkisiz kullanım isteği nin süresinin sonunda bu abonelik için yeniden bir istek almayı bekleyin. 

#### <a name="unsubscribed"></a>Aboneliği 

Abonelikler, açık bir müşteri isteğine veya aidat ödememe lerine yanıt olarak bu duruma ulaşır. Ortaktan beklenti, müşterinin verilerinin istek üzerine belirli bir gün sayısı için geri kazanımı için tutulması ve ardından silinmesidir. 


## <a name="api-reference"></a>API başvurusu

Bu bölümde, SaaS *Abonelik API* ve *İşlemLER API'si*dokümanteder.  Sürüm 2 `api-version` API'ler için parametrenin değeri `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Parametre ve varlık tanımları

Aşağıdaki tabloda, yerine getirme API'leri tarafından kullanılan ortak parametreler ve varlıklar için tanımlar listelenir.

|     Varlık/Parametre     |     Tanım                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Bir SaaS kaynağı için GUID tanımlayıcısı.  |
| `name`                   | Müşteri tarafından bu kaynak için sağlanan kolay bir ad. |
| `publisherId`            | Her yayımcı için benzersiz bir dize tanımlayıcısı (örneğin: "contoso"). |
| `offerId`                | Her teklif için benzersiz bir dize tanımlayıcısı (örneğin: "offer1").  |
| `planId`                 | Her plan/SKU için benzersiz bir dize tanımlayıcısı (örneğin: "gümüş"). |
| `operationId`            | Belirli bir işlem için GUID tanımlayıcısı.  |
|  `action`                | Bir kaynak üzerinde gerçekleştirilen eylem, `Unsubscribe`ya `ChangePlan`, `ChangeQuantity` `Suspend` `Transfer`, `Reinstate`, , , , . |
|   |   |

Genel olarak benzersiz[tanımlayıcılar (GUID'ler)](https://en.wikipedia.org/wiki/Universally_unique_identifier)genellikle otomatik olarak oluşturulan 128 bit (32-hexadecimal) sayılardır. 

#### <a name="resolve-a-subscription"></a>Aboneliği çözme 

Çözüm bitiş noktası, yayımcının kalıcı bir kaynak kimliğine pazar yeri belirteci çözmesini sağlar. Kaynak kimliği, Bir SaaS aboneliği için benzersiz tanımlayıcıdır. Bir kullanıcı bir ortağın web sitesine yönlendirildiğinde, URL sorgu parametrelerinde bir belirteç içerir. Ortağın bu belirteci kullanması ve çözümiçin bir istekte bulunması beklenir. Yanıt, benzersiz SaaS abonelik kimliği, adı, teklif kimliği ve kaynak için plan içerir. Bu belirteç yalnızca bir saat için geçerlidir. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Yayımla<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |

*İstek üstleri:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      | `application/json` |
|  x-ms-requestid    |  İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Örneğin: "`Bearer <access_token>`". |
|  x-ms-marketplace-token  |  Kullanıcı Azure'dan SaaS iş ortağının web sitesine yönlendirildiğinde URL'deki belirteç sorgu `https://contoso.com/signup?token=..`parametresi (örneğin: ). *Not:* URL, kullanmadan önce tarayıcıdaki belirteç değerini çözer.  |

*Yanıt kodları:*

Ürün Kodu: 200<br>
Opak belirteci Bir SaaS aboneliğine giderir. Yanıt gövdesi:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Ürün Kodu: 400<br>
Kötü istek. x-ms-marketplace-token eksik, bozuk veya süresi dolmuş.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>Abonelik API'si

Abonelik API aşağıdaki HTTPS işlemleri destekler: **Get**, **Post**, **Yama**, ve **Sil**.


#### <a name="list-subscriptions"></a>Abonelikleri listele

Bir yayımcının tüm SaaS aboneliklerini listeler.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Al<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersiyon  |  Bu istek için kullanılacak işlemin sürümü.  |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| İçerik Türü       |  `application/json`  |
| x-ms-requestid     |  İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
| yetkilendirme      |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Örneğin: "`Bearer <access_token>`".  |

*Yanıt kodları:*

Ürün Kodu: 200 <br/>
Kimlik doğrulama belirteci temel alınarak yayımcıyı ve ilgili abonelikleri yayımcının tüm teklifleri için alır.

>[!Note]
>Teklifi ilk geliştirdiğiniz zaman [sahte API'ler](#mock-apis) kullanılırken, teklifi yayınlarken gerçek API'lerin kullanılması gerekir.  Gerçek API'ler ve Mock API'ler kodun ilk satırına göre farklılık gösterir.  Bu `subscription` bölüm sahte API için yok iken gerçek API'de bölüm vardır.

Sahte API için yanıt yükü:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": "true", // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```
Ve gerçek API için: <br>

```json
{
  "subscriptions": [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant, object id and email address for which SaaS subscription is purchased.
              "emailId": "<email>",
              "objectId": "<guid>",                     
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant, object id and email address that purchased the SaaS subscription. These could be different for reseller scenario
              "emailId": "<email>",
              "objectId": "<guid>",                      
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": true, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.(optional field - default false)
          "isTest": false, //indicating whether the current subscription is a test asset
          "sandboxType": "None", // Possible Values: None, Csp (Csp sandbox purchase)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "@nextLink": ""
}
```
Devamı belirteci yalnızca geri alma planlarının ek "sayfaları" varsa kullanılabilir. 

Ürün Kodu: 403 <br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor. 

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abonelik alın

Belirtilen SaaS aboneliğini alır. Lisans bilgilerini almak ve bilgi planlamak için bu aramayı kullanın.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Belirteci Çözümle API üzerinden çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.   |
|  ApiVersiyon        |   Bu istek için kullanılacak işlemin sürümü.   |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      |  `application/json`  |
|  x-ms-requestid    |  İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Örneğin: "`Bearer <access_token>`".  |

*Yanıt kodları:*

Ürün Kodu: 200<br>
SaaS aboneliğini tanımlayıcıdan alır. Yanıt yükü:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "isFreeTrial": "true", // true - customer subscription is currently in free trial, false - customer subscription is not currently in free trial.
        "status": "Subscribed", // Indicates the status of the operation.
          "term": { //This gives the free trial term start and end date
            "startDate": "2019-05-31",
            "endDate": "2019-06-29",
            "termUnit": "P1M" //where P1M: Monthly, P1Y: Yearly 
        },
}
```

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.<br> 

Ürün Kodu: 500<br>
İç sunucu hatası.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Kullanılabilir planları listele

Geçerli yayımcı için özel veya genel teklifler olup olmadığını öğrenmek için bu aramayı kullanın.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |   Bu istek için kullanılacak işlemin sürümü.  |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik Türü     |  `application/json` |
|   x-ms-requestid   |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  x-ms-korelasyonid  | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`". |

*Yanıt kodları:*

Ürün Kodu: 200<br>
Bir müşteri için kullanılabilir planların listesini alır. Yanıt gövdesi:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Ürün Kodu: 404<br>
Bulunamadı.<br> 

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor. <br> 

Ürün Kodu: 500<br>
İç sunucu hatası.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Aboneliği etkinleştirme

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Yayımla<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |

*İstek üstleri:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      | `application/json`  |
|  x-ms-requestid    | İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  x-ms-korelasyonid  | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu dize, istemci işleminden kaynaklanan tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`". |

*Taşıma isteği:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Yanıt kodları:*

Ürün Kodu: 200<br>
Aboneliği etkinleştirir.<br>

Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Abonelikle ilgili planı değiştirme

Abonelik le ilgili planı güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      | `application/json` |
|  x-ms-requestid    |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  x-ms-korelasyonid  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.    |
| yetkilendirme      |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`".  |

*Taşıma isteği:*

```json
Request Body:
{
    "planId": "gold"
}
```

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operasyon-Konum | İşlemin durumunu almak için bir kaynağa bağlantı.   |

*Yanıt kodları:*

Ürün Kodu: 202<br>
Planı değiştirme isteği kabul edildi. Ortağın, bir başarı veya başarısızlığı belirlemek için Operasyon Yeri'ni yoklaması bekleniyor. <br>

Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Her ikisi de değil, aynı anda yalnızca bir plan veya miktar yamalı olabilir. **Güncelleştirme** ile bir abonelik teresyr' de `allowedCustomerOperations`yok.

#### <a name="change-the-quantity-on-the-subscription"></a>Abonelikteki miktarı değiştirme

Abonelikteki miktarı güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Yama:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      | `application/json` |
|  x-ms-requestid    |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  x-ms-korelasyonid  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.    |
| yetkilendirme      |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`".  |

*Taşıma isteği:*

```json
Request Body:
{
    "quantity": 5
}
```

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operasyon-Konum | İşlemin durumunu almak için kaynağa bağlantı kurun.   |

*Yanıt kodları:*

Ürün Kodu: 202<br>
Miktarı değiştirme isteği kabul edildi. Ortağın, bir başarı veya başarısızlığı belirlemek için Operasyon Yeri'ni yoklaması bekleniyor. <br>

Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.


Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Her ikisi de değil, aynı anda yalnızca bir plan veya miktar yamalı olabilir. **Güncelleştirme** ile bir abonelik teresyr' de `allowedCustomerOperations`yok.

#### <a name="delete-a-subscription"></a>Aboneliği silme

Aboneliği iptal edin ve belirtilen aboneliği silin.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Sil<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |

*İstek üstleri:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik Türü     |  `application/json` |
|  x-ms-requestid    |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.   |
|  x-ms-korelasyonid  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.   |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`".  |

*Yanıt kodları:*

Ürün Kodu: 202<br>
Ortak, Bir SaaS aboneliğinin aboneliğini iptal etmek için bir çağrı başlattı.<br>

Ürün Kodu: 400<br>
**Silme** ile bir abonelikte `allowedCustomerOperations`silme .

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>Operasyon API'si

Operasyonlar API aşağıdaki Yama ve Get işlemlerini destekler.

#### <a name="list-outstanding-operations"></a>Bekleyen işlemleri listele 

Geçerli yayımcının bekleyen işlemlerini listeler. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersiyon                |   Bu istek için kullanılacak işlemin sürümü.                |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |

*İstek üstleri:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik Türü     |  `application/json` |
|  x-ms-requestid    |  İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`".  |

*Yanıt kodları:*

Ürün Kodu: 200<br> Abonelikte bekleyen işlemlerin listesini alır. Yanıt yükü:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Operasyon durumunu alma

Yayımcının, belirtilen tetiklenen async işleminin `Subscribe`durumunu (, `Unsubscribe` `ChangePlan`, `ChangeQuantity`, veya ).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersiyon        |  Bu istek için kullanılacak işlemin sürümü.  |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik Türü      |  `application/json`   |
|  x-ms-requestid    |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır.  |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Örneğin: "`Bearer <access_token>`".  |

*Yanıt kodları:*<br>

Ürün Kodu: 200<br> Belirtilen bekleyen SaaS işlemini alır. Yanıt yükü:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.
 
Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 500<br> İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>İşlemin durumunu güncelleştirme

Sağlanan değerlerle başarı veya başarısızlığı belirtmek için işlemin durumunu güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersiyon       |  Bu istek için kullanılacak işlemin sürümü.  |
| subscriptionId     | Çözüm API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliğinin benzersiz bir tanımlayıcısı.  |
|  operationId       | Tamamlanan operasyon. |

*İstek üstleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik Türü     | `application/json`   |
|   x-ms-requestid   |   İstemciden gelen isteği izlemek için benzersiz bir dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  x-ms-korelasyonid |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
|  yetkilendirme     |  [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Örneğin: "`Bearer <access_token>`".  |

*Taşıma isteği:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Yanıt kodları:*

Ürün Kodu: 200<br> Ortak taraftaki bir operasyonun tamamlanılmasını bildirmek için bir çağrı. Örneğin, bu yanıt koltuk veya plan değişikliğinin sinyalini verebilir.

Ürün Kodu: 400<br>
Kötü istek: doğrulama hataları.

Ürün Kodu: 403<br>
Yetki -siz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz veya istek geçerli yayımcıya ait olmayan bir edinmeye erişmeye çalışıyor.

Ürün Kodu: 404<br>
Bulunamadı.

Ürün Kodu: 409<br>
Çakışma. Örneğin, daha yeni bir işlem zaten yerine getirilmiştir.

Ürün Kodu: 500<br> İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>SaaS hizmetinde bir webhook uygulama

Yayımcı, kullanıcıların hizmetindeki değişiklikleri proaktif olarak bildirmek için bu SaaS hizmetinde bir web hook uygulaması gerekir. SaaS hizmetinin, webhook bildirimi nde bir işlem yapmadan önce işlemleri doğrulamak ve yetkilendirmek için API'yi çağırması beklenir.


```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher's name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Eylemin aşağıdakilerden biri olabileceği durumlar: 
- `Unsubscribe`(kaynak silindiğinde)
- `ChangePlan`(değişiklik planı işlemi tamamlandığında)
- `ChangeQuantity`(değişiklik miktarı işlemi tamamlandığında)
- `Suspend`(kaynak askıya alındığı zaman)
- `Reinstate`(kaynağın askıya alındıktan sonra geri alınması)

Durumun aşağıdakilerden biri olabileceği durumlar: 
- **Başlatılmama** <br>
 - **Devam Ediyor** <br>
- **Başarılı oldu** <br>
- **Başarısız** <br>
- **Çakışma** <br>

Bir webhook bildiriminde, işlem yapılan durumlar ya **Başarılı** ve **Başarısız oldu.** Bir işlemin yaşam döngüsü **NotStarted'dan** **Başarılı**, **Başarısız**veya **Çakışma**gibi bir terminal durumuna geçilir. **Başlatılmama** veya **Devam etme**alırsanız, işlem harekete geçmeden önce bir terminal durumuna ulaşana kadar GET API aracılığıyla durumu istemeye devam edin. 

## <a name="mock-apis"></a>Sahte API'ler

Sahte API'lerimizi, özellikle prototip oluşturma nın yanı sıra projeleri test etme gibi geliştirmeye başlamanıza yardımcı olmak için kullanabilirsiniz. 

Ana bilgisayar `https://marketplaceapi.microsoft.com/api` bitiş noktası: (kimlik doğrulama sı gerekmez)<br/>
API sürümü:`2018-09-15`<br/>
Örnek URI:`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

API bitiş noktası yolları hem sahte hem de gerçek API'ler arasında aynıdır, ancak API sürümleri farklıdır. Sürüm sahte `2018-09-15` sürümü ve `2018-08-31` üretim sürümü içindir. 

Bu makaledeki API aramalarından herhangi biri sahte ana bilgisayar bitiş noktasına yapılabilir. Genel olarak, bir yanıt olarak sahte veri geri almak için bekliyoruz. Sahte API'deki güncelleştirme abonelik yöntemlerine yapılan aramalar her zaman 500 döndürülür. 

## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca [Bulut Ortağı Portal REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview)kullanarak iş yüklerini, teklifleri ve yayıncı profillerini programlı bir şekilde alabilir ve iş yüklerini iş leyebilir ve işleyebilir.
