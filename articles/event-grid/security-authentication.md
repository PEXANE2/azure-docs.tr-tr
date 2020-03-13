---
title: Azure Event Grid güvenliğini ve kimlik doğrulaması
description: Bu makalede, Event Grid kaynaklarınıza (Web kancası, abonelikler, özel konular) erişimin kimliklerinin nasıl doğrulanmasıyla ilgili farklı yollar açıklanmaktadır
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: babanisa
ms.openlocfilehash: 0b7c5b42ac6291c6687337ba8d6a9d35830b9bda
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79281021"
---
# <a name="authenticating-access-to-event-grid-resources"></a>Event Grid kaynaklarına erişim izni doğrulanıyor

Azure Event Grid, kimlik doğrulaması üç tür vardır:

* Web kancası olay teslimi
* Olay abonelikleri
* Özel Konu yayımlama

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancaları olayları Azure Event Grid'den almak için birçok yöntemlerinden biridir. Yeni bir olay hazır olduğunda, Event Grid hizmet istek gövdesinde olay ile yapılandırılmış uç noktasına bir HTTP isteği gönderir.

Web kancalarını destekleyen birçok diğer hizmetleri gibi Event Grid, olayları için bu endpoint göndermeye başlamadan sahipliğini Web kancası uç noktanızın kanıtlamak gerektirir. Bu gereksinim, uç noktanızı olaylarla taşmasını gelen kötü niyetli bir kullanıcı önler. Aşağıda listelenen üç Azure hizmetlerinden herhangi birinin kullandığınızda, Azure altyapısının bu doğrulama otomatik olarak işler:

* [Event Grid Bağlayıcısı](https://docs.microsoft.com/connectors/azureeventgrid/) ile Azure Logic Apps
* [Web kancası](../event-grid/ensure-tags-exists-on-new-virtual-machines.md) aracılığıyla Azure Otomasyonu
* [Event Grid tetikleyicisiyle](../azure-functions/functions-bindings-event-grid.md) Azure işlevleri

HTTP tetikleyicisi tabanlı Azure işlevi gibi başka türde bir uç noktasını kullanıyorsanız, Event Grid ile bir doğrulama anlaşması'na katılmak uç nokta kodunuzun gerekir. Event Grid aboneliği doğrulama iki şekilde destekler.

1. **Validationcode Handshake (programlı)** : uç noktanız için kaynak kodu denetederseniz bu yöntem önerilir. Olay aboneliği oluşturma sırasında Event Grid aboneliği doğrulama olayı uç noktanıza gönderir. Bu olayın şeması için başka bir Event Grid olayı benzerdir. Bu olayın veri bölümü bir `validationCode` özelliği içerir. Uygulamanızı doğrulama isteği için beklenen olay aboneliği ve doğrulama kodu Event grid'e yankılayan olduğunu doğrular. Bu anlaşma mekanizması tüm Event Grid sürümlerinde desteklenir.

2. **Validationurl el sıkışma (el ile)** : belirli durumlarda, validationcode anlaşmasını uygulamak için uç noktanın kaynak koduna erişemezsiniz. Örneğin, üçüncü taraf bir hizmet kullanıyorsanız ( [Zapier](https://zapier.com) veya [ifttt](https://ifttt.com/)gibi), doğrulama koduyla program aracılığıyla yanıt veremez.

   Event Grid, sürümü 2018-05-01-preview ile başlayarak, el ile doğrulama el sıkışması destekler. API sürümü 2018-05-01-Preview veya sonraki bir sürümünü kullanan bir SDK veya araçla bir olay aboneliği oluşturuyorsanız, Event Grid Abonelik doğrulama olayının veri bölümünde bir `validationUrl` özelliği gönderir. Anlaşma tamamlamak için bulma olay verileri ve el ile bu URL'yi bir GET isteği gönder. Bir REST istemcisi ya da web tarayıcınızı kullanabilirsiniz.

   Belirtilen URL 5 dakika için geçerlidir. Bu süre boyunca, olay aboneliğinin sağlama durumu `AwaitingManualAction`. El ile doğrulamayı 5 dakika içinde tamamlamazsanız, sağlama durumu `Failed`olarak ayarlanır. Olay aboneliği el ile doğrulama başlatmadan önce yeniden oluşturmanız gerekir.

    Bu kimlik doğrulama mekanizması Ayrıca, el ile doğrulama moduna alınmadan önce doğrulama olayının GÖNDERISINI kabul ettiğini bilmesi için Web kancası uç noktasının 200 HTTP durum kodunu döndürmesini gerektirir. Diğer bir deyişle, uç nokta 200 döndürürse ancak programlı olarak bir doğrulama yanıtı geri dönmezse, mod el ile doğrulama moduna geçirilir. Doğrulama URL 'sinde 5 dakika içinde bir GET varsa, doğrulama el sıkışması başarılı olarak kabul edilir.

> [!NOTE]
> Doğrulama için otomatik olarak imzalanan sertifikalar kullanılması desteklenmez. Bunun yerine bir sertifika yetkilisinden (CA) imzalı bir sertifika kullanın.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme zaman Event Grid aboneliği doğrulama olayı hedef uç noktasına gönderir. 
* Olayı bir üst bilgi değeri içeren "aeg olay türü: SubscriptionValidation".
* Olay gövdesinde diğer Event Grid olaylarına aynı şemaya sahip.
* Olayın eventType özelliği `Microsoft.EventGrid.SubscriptionValidationEvent`.
* Olayın Data özelliği rastgele oluşturulmuş bir dizeye sahip bir `validationCode` özelliği içerir. Örneğin, "validationCode: acb13...".
* Olay verileri, aboneliği el ile doğrulamak için URL ile birlikte bir `validationUrl` özelliği de içerir.
* Dizi doğrulama olay içeriyor. Doğrulama kodu geri echo sonra gelen diğer olayları ayrı bir istek gönderilir.
* EventGrid veri düzlemi SDK'ları abonelik doğrulama olay verileri ve abonelik doğrulama yanıt karşılık gelen sınıfları içerir.

Bir örnek SubscriptionValidationEvent aşağıdaki örnekte gösterilmiştir:

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

Uç nokta sahipliği kanıtlamak için geri validationResponse özelliğinde doğrulama kodu aşağıdaki örnekte gösterildiği gibi echo:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Bir HTTP 200 Tamam yanıt durum kodu döndürmelidir. Kabul edilen HTTP 202, geçerli bir Event Grid Abonelik doğrulama yanıtı olarak tanınmıyor. Http isteğinin 30 saniye içinde tamamlaması gerekir. İşlem 30 saniye içinde sonlanmazsa, işlem iptal edilir ve 5 saniye sonra yeniden denenecek. Tüm denemeler başarısız olursa, doğrulama el sıkışma hatası olarak kabul edilir.

Veya doğrulama URL'si için bir GET isteği göndererek abonelik el ile doğrulayabilirsiniz. Olay aboneliği doğrulandı kadar bir bekleme durumunda kalır. Doğrulama URL 'Si 553 numaralı bağlantı noktasını kullanır. Güvenlik Duvarı kurallarınız bağlantı noktası 553 ' i engellerseniz, başarılı el ile el sıkışma için kuralların güncellenmesi gerekebilir.

Abonelik doğrulama anlaşmasını işleme bir örnek için bkz. bir [ C# örnek](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

### <a name="checklist"></a>Denetim Listesi

Olay aboneliği oluşturma sırasında, "belirtilen uç nokta https 'yi doğrulama denemesi:\//Your-Endpoint-Here hatasıyla birlikte bir hata mesajı görüyorsanız. Daha fazla ayrıntı için https:\//aka.ms/esvalidation "adresini ziyaret edin. Bu, doğrulama el sıkışmasının bir hata olduğunu gösterir. Bu hatayı gidermek için aşağıdaki durumlara doğrulayın:

* Hedef uç noktada çalışan uygulama kodunu kontrol ediyor musunuz? Örneğin, Azure işlevi bir HTTP tetikleyici tabanlı yazıyorsanız, değişiklik yapmak için uygulama koduna erişmeniz gerekiyor?
* Uygulama koduna erişiminiz varsa, yukarıdaki örnekte gösterildiği gibi temel ValidationCode el sıkışması mekanizması uygulayın.

* Uygulama kodu (örneğin, Web kancalarını destekleyen üçüncü taraf bir hizmet kullanıyorsanız) erişimi yoksa, el ile anlaşma mekanizması kullanabilirsiniz. Doğrulama olay validationUrl almak için 2018-05-01-Önizleme API sürümü veya üzeri (yükleme Event Grid Azure CLI uzantısı) kullandığınızdan emin olun. El ile doğrulama el sıkışmasını gerçekleştirmek için `validationUrl` özelliğinin değerini alın ve Web tarayıcınızda bu URL 'YI ziyaret edin. Doğrulama başarılı olursa, doğrulama başarılı olduğunu, web tarayıcınızda bir ileti görürsünüz. Olay aboneliğinin provisioningState "başarılı olduğunu" görürsünüz. 

### <a name="event-delivery-security"></a>Olay teslimi güvenliği

#### <a name="azure-ad"></a>Azure AD

Azure Active Directory kullanarak Web kancası uç noktanızı güvenli hale getirerek olayları, uç noktalarınıza yayımlamak üzere Event Grid kimlik doğrulaması ve yetkilendirmek için kullanabilirsiniz. Bir Azure Active Directory uygulaması oluşturmanız, Event Grid uygulamanızda bir rol ve hizmet ilkesi oluşturmanız ve olay aboneliğini Azure AD uygulamasını kullanacak şekilde yapılandırmanız gerekir. [AAD 'yi Event Grid ile yapılandırmayı öğrenin](secure-webhook-delivery.md).

#### <a name="query-parameters"></a>Sorgu parametreleri
Bir olay aboneliği oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek, Web kancası uç noktası güvenli hale getirebilirsiniz. Bu sorgu parametrelerinden birini, [erişim belirteci](https://en.wikipedia.org/wiki/Access_token)gibi bir gizli dizi olacak şekilde ayarlayın. Web kancası olay geçerli izinleriyle Event Grid'den gelen tanımak için gizli anahtarı kullanabilirsiniz. Olay Kılavuzu her Web kancası olay teslimi bu sorgu parametreleri içerir.

Olay aboneliği düzenlenirken, Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)'de [--Include-Full-Endpoint-URL](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi kullanılmadığı takdirde sorgu parametreleri gösterilmez veya döndürülmez.

Son olarak, Azure Event Grid yalnızca HTTPS Web kancası uç noktaları desteklediğini unutmayın.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone olmak için olay kaynağı ve işleyici erişimi olmasını kanıtlamaları gerekir. Bir Web kancası sahibi olduğunu doğrulayan ve önceki bölümde ele. Bir Web kancası (örneğin, bir olay hub'ı veya kuyruk depolama) olmayan bir olay işleyicisi kullanıyorsanız, bu kaynağa yazma erişimi gerekir. Bu izinleri denetimi, yetkisiz bir kullanıcı, kaynağınıza olayları göndermesini engeller.

Olay kaynağı olan kaynakta **Microsoft. EventGrid/Eventabonelikleriniz/Write** izninizin olması gerekir. Kaynağın yeni bir abonelik kapsamda yazmakta olduğunuz çünkü bu iznine sahip olmanız gerekir. Gerekli kaynak sistem konusu ya da özel konuya abone göre farklılık gösterir. Bu bölümde iki türü de açıklanmaktadır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcılar)

Sistem konuları için yeni bir olay aboneliği kapsamında olayın yayımlanması kaynak yazma izni gerekir. Kaynağın biçimi: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, **myacct**adlı bir depolama hesabındaki bir olaya abone olmak Için, Microsoft. Eventgrid/Eventaboneliklerin/Write iznine şunlar gerekir: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular

Özel konu için yeni olay aboneliğinizi kapsamında olay Kılavuzu konusu yazma izni gerekir. Kaynağın biçimi: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, **MyTopic**adlı özel bir konuya abone olmak Için, Microsoft. Eventgrid/Eventaboneliklerin/Write iznine şunlar gerekir: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Özel Konu yayımlama

Özel konu, paylaşılan erişim imzası (SAS) veya anahtar kimlik doğrulaması kullanın. SAS öneririz, ancak anahtar kimlik doğrulaması basit bir programlama sağlar ve birçok mevcut Web kancası yayımcıları ile uyumludur. 

HTTP üst bilgisi kimlik doğrulaması değeri içerir. SAS için, üst bilgi değeri için **AEG-SAS-Token** ' ı kullanın. Anahtar kimlik doğrulaması için, üst bilgi değeri için **AEG-SAS-Key** kullanın.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması, kimlik doğrulamasının en basit biçimidir. Şu biçimi kullanın: `aeg-sas-key: <your key>`

Örneğin, bir anahtar ile geçirin:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Event Grid için SAS belirteci, kaynak, sona erme süresini ve imza içerir. SAS belirtecinin biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak için olayları gönderiyorsunuz olay Kılavuzu konusu yoludur. Örneğin, geçerli bir kaynak yolu: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Bir anahtar, imza üret

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

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Event Grid hizmeti tarafından diske yazılan tüm olaylar veya veriler, bekleyen bir şekilde şifrelendiğinden emin olmak için Microsoft tarafından yönetilen bir anahtarla şifrelenir. Ayrıca, olayların veya verilerin saklanacağı en uzun süre, [Event Grid yeniden deneme ilkesiyle](delivery-and-retry.md)ilgili olarak 24 saat olur. Event Grid, 24 saat sonra tüm olayları veya verileri otomatik olarak siler veya olayın yaşam süresi (hangisi daha az) olur.

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
