---
title: Azure Event Grid güvenlik ve kimlik doğrulaması
description: Azure Event Grid ve kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: f22d8c57b0127e646321a20587d0cd89f5c9ea45
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72325414"
---
# <a name="event-grid-security-and-authentication"></a>Event Grid güvenlik ve kimlik doğrulaması 

Azure Event Grid üç tür kimlik doğrulamaya sahiptir:

* Web kancası olay teslimi
* Olay abonelikleri
* Özel konu yayımlama

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancaları Azure Event Grid olayların alınacağı birçok yol vardır. Yeni bir etkinlik hazırsanız Event Grid hizmet, istek gövdesinde olay ile yapılandırılmış uç noktaya bir HTTP isteği gönderir.

Web kancalarını destekleyen birçok farklı hizmet gibi Event Grid, bu uç noktaya yönelik olayları sunmaya başlamadan önce Web kancası uç noktanızın sahipliğini kanıtlamanızı gerektirir. Bu gereksinim, kötü niyetli bir kullanıcının uç noktanızı etkinliklerle taşmasını önler. Aşağıda listelenen üç Azure hizmetinden birini kullandığınızda, Azure altyapısı bu doğrulamayı otomatik olarak işler:

* [Event Grid Bağlayıcısı](https://docs.microsoft.com/connectors/azureeventgrid/) ile Azure Logic Apps
* [Web kancası](../event-grid/ensure-tags-exists-on-new-virtual-machines.md) aracılığıyla Azure Otomasyonu
* [Event Grid tetikleyicisiyle](../azure-functions/functions-bindings-event-grid.md) Azure işlevleri

HTTP tetikleyici tabanlı bir Azure işlevi gibi başka herhangi bir uç nokta türü kullanıyorsanız, uç nokta kodunuzun Event Grid bir doğrulama el sıkışmasına katılması gerekir. Event Grid, aboneliği doğrulamak için iki yolu destekler.

1. **Validationcode Handshake (programlı)** : uç noktanız için kaynak kodu denetederseniz bu yöntem önerilir. Olay aboneliği oluşturma sırasında, Event Grid uç noktanıza bir abonelik doğrulama olayı gönderir. Bu olayın şeması, diğer tüm Event Grid olayına benzerdir. Bu olayın veri kısmı `validationCode` özelliği içerir. Uygulamanız, doğrulama isteğinin beklenen bir olay aboneliği olduğunu doğrular ve Event Grid doğrulama kodunu gösterir. Bu el sıkışma mekanizması tüm Event Grid sürümlerinde desteklenir.

2. **Validationurl el sıkışma (el ile)** : belirli durumlarda, validationcode anlaşmasını uygulamak için uç noktanın kaynak koduna erişemezsiniz. Örneğin, üçüncü taraf bir hizmet kullanıyorsanız ( [Zapier](https://zapier.com) veya [ifttt](https://ifttt.com/)gibi), doğrulama koduyla program aracılığıyla yanıt veremez.

   Sürüm 2018-05-01-önizleme ile başlayarak, Event Grid el ile doğrulama anlaşmasını destekler. API sürümü 2018-05-01-Preview veya sonraki bir sürümünü kullanan bir SDK veya araçla bir olay aboneliği oluşturuyorsanız, Event Grid Abonelik doğrulama olayının veri bölümünde bir `validationUrl` özelliği gönderir. El sıkışmasını gerçekleştirmek için, bu URL 'YI olay verilerinde bulun ve ona el ile bir GET isteği gönderin. Bir REST istemcisi ya da Web tarayıcınızı kullanabilirsiniz.

   Belirtilen URL 5 dakika için geçerlidir. Bu süre boyunca, olay aboneliğinin sağlama durumu `AwaitingManualAction` ' dır. El ile doğrulamayı 5 dakika içinde tamamlamazsanız, sağlama durumu `Failed` olarak ayarlanır. El ile doğrulamayı başlatmadan önce olay aboneliği oluşturmanız gerekir.

    Bu kimlik doğrulama mekanizması Ayrıca, el ile doğrulama moduna alınmadan önce doğrulama olayının GÖNDERISINI kabul ettiğini bilmesi için Web kancası uç noktasının 200 HTTP durum kodunu döndürmesini gerektirir. Diğer bir deyişle, uç nokta 200 döndürürse ancak programlı olarak bir doğrulama yanıtı geri dönmezse, mod el ile doğrulama moduna geçirilir. Doğrulama URL 'sinde 5 dakika içinde bir GET varsa, doğrulama el sıkışması başarılı olarak kabul edilir.

> [!NOTE]
> Doğrulama için otomatik olarak imzalanan sertifikalar kullanılması desteklenmez. Bunun yerine bir sertifika yetkilisinden (CA) imzalı bir sertifika kullanın.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme sırasında, Event Grid bir abonelik doğrulama olayını hedef uç noktaya gönderir. 
* Olay, "AEG-Event-Type: SubscriptionValidation" başlık değerini içerir.
* Olay gövdesi diğer Event Grid olaylarıyla aynı şemaya sahip.
* Olayın eventType özelliği `Microsoft.EventGrid.SubscriptionValidationEvent` ' dır.
* Olayın Data özelliği rastgele oluşturulmuş bir dizeye sahip bir `validationCode` özelliği içerir. Örneğin, "validationCode: acb13...".
* Olay verileri, aboneliği el ile doğrulamak için bir URL ile `validationUrl` özelliği de içerir.
* Dizi yalnızca doğrulama olayını içerir. Diğer olaylar, doğrulama kodunu yankıladıktan sonra ayrı bir istekte gönderilir.
* EventGrid veri düzlemi SDK 'larının, abonelik doğrulama olay verileri ve abonelik doğrulama yanıtına karşılık gelen sınıfları vardır.

Aşağıdaki örnekte bir SubscriptionValidationEvent örneği gösterilmektedir:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=512d38b6-c7b8-40c8-89fe-f46f9e9622b6&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1A1A1A1A"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Uç nokta sahipliğini kanıtlamak için, aşağıdaki örnekte gösterildiği gibi validationResponse özelliğindeki doğrulama kodunu geri yankılayın:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Bir HTTP 200 Tamam yanıt durum kodu döndürmelidir. Kabul edilen HTTP 202, geçerli bir Event Grid Abonelik doğrulama yanıtı olarak tanınmıyor.

Ya da, doğrulama URL 'sine bir GET isteği göndererek aboneliği el ile doğrulayabilirsiniz. Olay aboneliği, onaylanana kadar bekleyen bir durumda kalır.

Abonelik doğrulama anlaşmasını işleme bir örnek için bkz. bir [ C# örnek](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

### <a name="checklist"></a>Denetim Listesi

Olay aboneliği oluşturma sırasında, "belirtilen uç noktayı doğrulama denemesi: \//-uç noktanız-burada başarısız oldu gibi bir hata iletisi görüyorsanız. Daha fazla ayrıntı için https: \//aka. MS/esvalidation "adresini ziyaret edin. Bu, doğrulama el sıkışmasının bir hata olduğunu gösterir. Bu hatayı çözmek için aşağıdaki noktaları doğrulayın:

* Hedef uç noktasındaki uygulama kodu denetimidir? Örneğin, bir HTTP tetikleyici tabanlı Azure Işlevi yazıyorsanız, üzerinde değişiklik yapmak için uygulama koduna erişiminiz var mı?
* Uygulama koduna erişiminiz varsa, yukarıdaki örnekte gösterildiği gibi ValidationCode tabanlı el sıkışma mekanizmasını uygulayın.

* Uygulama koduna erişiminiz yoksa (örneğin, Web kancalarını destekleyen bir üçüncü taraf hizmeti kullanıyorsanız) el ile el sıkışma mekanizmasını kullanabilirsiniz. Doğrulama olayında validationUrl 'Yi almak için 2018-05-01-Preview API sürümünü veya üstünü (Event Grid Azure CLı uzantısı ' nı) kullandığınızdan emin olun. El ile doğrulama el sıkışmasını gerçekleştirmek için `validationUrl` özelliğinin değerini alın ve Web tarayıcınızda bu URL 'YI ziyaret edin. Doğrulama başarılı olursa, Web tarayıcınızda doğrulamanın başarılı olduğunu belirten bir ileti görmeniz gerekir. Olay aboneliğinin provisioningState değerini "başarılı" olarak görürsünüz. 

### <a name="event-delivery-security"></a>Olay teslimi güvenliği

Bir olay aboneliği oluştururken Web kancası URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktanızı güvenli hale getirebilirsiniz. Bu sorgu parametrelerinden birini, [erişim belirteci](https://en.wikipedia.org/wiki/Access_token)gibi bir gizli dizi olacak şekilde ayarlayın. Web kancası, olayın geçerli izinlerle Event Grid geldiğini tanımak için gizli anahtarı kullanabilir. Event Grid, Web kancasına her olay teslimine bu sorgu parametrelerini dahil eder.

Olay aboneliği düzenlenirken, Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)'de [--Include-Full-Endpoint-URL](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi kullanılmadığı takdirde sorgu parametreleri gösterilmez veya döndürülmez.

Son olarak, Azure Event Grid yalnızca HTTPS Web kancası uç noktalarını desteklediğini unutmayın.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone olmak için olay kaynağına ve işleyicisine erişiminizin olduğunu kanıtlamanız gerekir. Bir Web kancasına sahip olmanız, önceki bölümde ele alınmıştır. Web kancası olmayan bir olay işleyicisi (örneğin, bir olay hub 'ı veya kuyruk depolaması) kullanıyorsanız, bu kaynağa yazma erişimi gerekir. Bu izin denetimi yetkisiz bir kullanıcının kaynağa olay göndermesini engeller.

Olay kaynağı olan kaynakta **Microsoft. EventGrid/Eventabonelikleriniz/Write** izninizin olması gerekir. Kaynak kapsamına yeni bir abonelik yazıyorsanız, bu izne ihtiyacınız vardır. Gerekli kaynak, bir sistem konusuna veya özel konuya abone olup olmadığınız temel alınarak farklılık gösterir. Her iki tür de bu bölümde açıklanmaktadır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcıları)

Sistem konuları için, olayı yayımlayan kaynağın kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, **myacct**adlı bir depolama hesabındaki bir olaya abone olmak Için, Microsoft. Eventgrid/Eventaboneliklerin/Write iznine şunlar gerekir: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel Konular

Özel konularda, olay Kılavuzu konusunun kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, **MyTopic**adlı özel bir konuya abone olmak Için, Microsoft. Eventgrid/Eventaboneliklerin/Write iznine şunlar gerekir: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Özel konu yayımlama

Özel konular, paylaşılan erişim Imzası (SAS) veya anahtar kimlik doğrulaması kullanır. SAS önerilir, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok mevcut Web kancası yayımcısıyla uyumludur. 

Kimlik doğrulama değerini HTTP üstbilgisine dahil edersiniz. SAS için, üst bilgi değeri için **AEG-SAS-Token** ' ı kullanın. Anahtar kimlik doğrulaması için, üst bilgi değeri için **AEG-SAS-Key** kullanın.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması en basit kimlik doğrulama biçimidir. Şu biçimi kullanın: `aeg-sas-key: <your key>`

Örneğin, şunu içeren bir anahtar geçirirsiniz:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Event Grid için SAS belirteçleri, kaynak, sona erme saati ve imza içerir. SAS belirtecinin biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak, olayları gönderdiğiniz olay Kılavuzu konusunun yoludur. Örneğin, geçerli bir kaynak yolu: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

İmza bir anahtardan oluşturulur.

Örneğin, geçerli bir **AEG-SAS-Token** değeri:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Aşağıdaki örnek, Event Grid ile kullanmak için bir SAS belirteci oluşturur:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Yönetim Access Control

Azure Event Grid, farklı kullanıcılara verilen erişim düzeyini, olay abonelikleri listeleme, yenilerini oluşturma ve anahtar oluşturma gibi çeşitli yönetim işlemlerini yapmak için denetlemenizi sağlar. Event Grid, Azure 'un rol tabanlı erişim denetimi 'ni (RBAC) kullanır.

### <a name="operation-types"></a>İşlem türleri

Event Grid aşağıdaki eylemleri destekler:

* Microsoft. EventGrid/*/Read
* Microsoft. EventGrid/*/Write
* Microsoft. EventGrid/*/Delete
* Microsoft. EventGrid/Eventabonelikler/getFullUrl/eylem
* Microsoft. EventGrid/konular/listKeys/ACTION
* Microsoft. EventGrid/konular/regenerateKey/Action

Son üç işlem, normal okuma işlemlerinden filtre uygulanan olası gizli bilgileri döndürür. Bu işlemlere erişimi kısıtlamanız önerilir. 

### <a name="built-in-roles"></a>Yerleşik roller

Event Grid, olay aboneliklerini yönetmek için iki yerleşik rol sağlar. Kullanıcılara olay etki alanındaki konulara abone olmaları için ihtiyaç duydukları izinleri verdiklerinden, bunlar [olay etki alanlarını](event-domains.md) uygularken önemlidir. Bu roller olay aboneliklerine odaklanılmıştır ve konu başlıkları oluşturma gibi eylemler için erişim vermez.

[Bu rolleri bir kullanıcıya veya gruba atayabilirsiniz](../role-based-access-control/quickstart-assign-role-user-portal.md).

**Eventgrid EventSubscription katılımcısı (Önizleme)** : Event Grid abonelik işlemlerini yönetme

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**Eventgrid EventSubscription okuyucusu (Önizleme)** : Event Grid abonelikleri okuma

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

### <a name="custom-roles"></a>Özel roller

Yerleşik rollerden farklı izinleri belirtmeniz gerekiyorsa, özel roller oluşturabilirsiniz.

Aşağıda, kullanıcıların farklı eylemler yapmasına izin veren örnek Event Grid rol tanımları verilmiştir. Bu özel roller, yalnızca olay aboneliklerinden daha geniş erişim verdiklerinden, yerleşik rollerden farklıdır.

**Eventgridreadonlyrole. JSON**: yalnızca salt okuma işlemlerine izin verir.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**Eventgridnodeletelistkeysrole. JSON**: kısıtlı gönderi eylemlerine izin verin ancak silme eylemlerine izin vermeyin.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole. JSON**: tüm olay Kılavuzu eylemlerine izin verir.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

[PowerShell](../role-based-access-control/custom-roles-powershell.md), [Azure CLI](../role-based-access-control/custom-roles-cli.md)ve [rest](../role-based-access-control/custom-roles-rest.md)ile özel roller oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
