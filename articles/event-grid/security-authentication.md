---
title: Azure Event Grid güvenlik ve kimlik doğrulaması
description: Bu makalede, Event Grid kaynaklarınıza (Web kancası, abonelikler, özel konular) erişimin kimliklerinin nasıl doğrulanmasıyla ilgili farklı yollar açıklanmaktadır
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: babanisa
ms.openlocfilehash: 4b2d65c9523f32eed01baa8d63c3d0119d00de1b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81532402"
---
# <a name="authenticating-access-to-event-grid-resources"></a>Event Grid kaynaklarına erişim izni doğrulanıyor

Azure Event Grid üç tür kimlik doğrulamaya sahiptir:

- Web kancası olay teslimi
- Olay abonelikleri
- Özel konu yayımlama

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancaları Azure Event Grid olayların alınacağı birçok yol vardır. Yeni bir etkinlik hazırsanız Event Grid hizmet, istek gövdesinde olay ile yapılandırılmış uç noktaya bir HTTP isteği gönderir.

Web kancalarını destekleyen birçok farklı hizmet gibi Event Grid, bu uç noktaya yönelik olayları sunmaya başlamadan önce Web kancası uç noktanızın sahipliğini kanıtlamanızı gerektirir. Bu gereksinim, kötü niyetli bir kullanıcının uç noktanızı etkinliklerle taşmasını önler. Aşağıda listelenen üç Azure hizmetinden birini kullandığınızda, Azure altyapısı bu doğrulamayı otomatik olarak işler:

- [Event Grid Bağlayıcısı](https://docs.microsoft.com/connectors/azureeventgrid/) ile Azure Logic Apps
- [Web kancası](../event-grid/ensure-tags-exists-on-new-virtual-machines.md) aracılığıyla Azure Otomasyonu
- [Event Grid tetikleyicisiyle](../azure-functions/functions-bindings-event-grid.md) Azure işlevleri

HTTP tetikleyici tabanlı bir Azure işlevi gibi başka herhangi bir uç nokta türü kullanıyorsanız, uç nokta kodunuzun Event Grid bir doğrulama el sıkışmasına katılması gerekir. Event Grid, aboneliği doğrulamak için iki yolu destekler.

1. **Zaman uyumlu el sıkışma**: olay aboneliği oluşturma sırasında Event Grid, uç noktanıza bir abonelik doğrulama olayı gönderir. Bu olayın şeması, diğer tüm Event Grid olayına benzerdir. Bu olayın veri bölümü bir `validationCode` özelliği içerir. Uygulamanız, doğrulama isteğinin beklenen bir olay aboneliği olduğunu doğrular ve yanıt içindeki doğrulama kodunu zaman uyumlu olarak döndürür. Bu el sıkışma mekanizması tüm Event Grid sürümlerinde desteklenir.

2. **Zaman uyumsuz el sıkışma**: belirli durumlarda, yanıt olarak yanıt olarak doğrulama kodunu geri dönemiyoruz. Örneğin, üçüncü taraf bir hizmet kullanıyorsanız (veya [IFTTT](https://ifttt.com/)gibi [`Zapier`](https://zapier.com) ), doğrulama koduyla program aracılığıyla yanıt veremez.

   Sürüm 2018-05-01-önizleme ile başlayarak, Event Grid el ile doğrulama anlaşmasını destekler. API sürümü 2018-05-01-Preview veya sonraki bir sürümünü kullanan bir SDK veya araçla bir olay aboneliği oluşturuyorsanız, Event Grid Abonelik doğrulama olayının veri bölümünde `validationUrl` bir özellik gönderir. El sıkışmasını gerçekleştirmek için, bu URL 'YI olay verilerinde bulun ve bir GET isteği yapın. Bir REST istemcisi ya da Web tarayıcınızı kullanabilirsiniz.

   Belirtilen URL **5 dakika**için geçerlidir. Bu süre boyunca, olay aboneliğinin sağlama durumu olur `AwaitingManualAction`. El ile doğrulamayı 5 dakika içinde tamamlamazsanız, sağlama durumu olarak `Failed`ayarlanır. El ile doğrulamayı başlatmadan önce olay aboneliği oluşturmanız gerekir.

   Bu kimlik doğrulama mekanizması Ayrıca, el ile doğrulama moduna alınmadan önce doğrulama olayının GÖNDERISINI kabul ettiğini bilmesi için Web kancası uç noktasının 200 HTTP durum kodunu döndürmesini gerektirir. Diğer bir deyişle, uç nokta 200 döndürürse ancak bir doğrulama yanıtını zaman uyumlu olarak geri döndürmezse mod el ile doğrulama moduna geçirilir. Doğrulama URL 'sinde 5 dakika içinde bir GET varsa, doğrulama el sıkışması başarılı olarak kabul edilir.

> [!NOTE]
> Doğrulama için otomatik olarak imzalanan sertifikalar kullanılması desteklenmez. Bunun yerine bir sertifika yetkilisinden (CA) imzalı bir sertifika kullanın.

### <a name="validation-details"></a>Doğrulama ayrıntıları

- Olay aboneliği oluşturma/güncelleştirme sırasında, Event Grid bir abonelik doğrulama olayını hedef uç noktaya gönderir.
- Olay, "AEG-Event-Type: SubscriptionValidation" başlık değerini içerir.
- Olay gövdesi diğer Event Grid olaylarıyla aynı şemaya sahip.
- Olayın eventType özelliği `Microsoft.EventGrid.SubscriptionValidationEvent`.
- Olayın Data özelliği rastgele oluşturulmuş bir dizeye sahip `validationCode` bir özelliği içerir. Örneğin, "validationCode: acb13...".
- Olay verileri, aboneliği el ile `validationUrl` doğrulamak için URL içeren bir özellik de içerir.
- Dizi yalnızca doğrulama olayını içerir. Diğer olaylar, doğrulama kodunu yankıladıktan sonra ayrı bir istekte gönderilir.
- EventGrid veri düzlemi SDK 'larının, abonelik doğrulama olay verileri ve abonelik doğrulama yanıtına karşılık gelen sınıfları vardır.

Aşağıdaki örnekte bir SubscriptionValidationEvent örneği gösterilmektedir:

```json
[
  {
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
  }
]
```

Uç nokta sahipliğini kanıtlamak için, aşağıdaki örnekte gösterildiği gibi validationResponse özelliğindeki doğrulama kodunu geri yankılayın:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Bir HTTP 200 Tamam yanıt durum kodu döndürmelidir. Kabul edilen HTTP 202, geçerli bir Event Grid Abonelik doğrulama yanıtı olarak tanınmıyor. Http isteğinin 30 saniye içinde tamamlaması gerekir. İşlem 30 saniye içinde sonlanmazsa, işlem iptal edilir ve 5 saniye sonra yeniden denenecek. Tüm denemeler başarısız olursa, doğrulama el sıkışma hatası olarak kabul edilir.

Ya da, doğrulama URL 'sine bir GET isteği göndererek aboneliği el ile doğrulayabilirsiniz. Olay aboneliği, onaylanana kadar bekleyen bir durumda kalır. Doğrulama URL 'Si 553 numaralı bağlantı noktasını kullanır. Güvenlik Duvarı kurallarınız bağlantı noktası 553 ' i engellerseniz, başarılı el ile el sıkışma için kuralların güncellenmesi gerekebilir.

Abonelik doğrulama anlaşmasını işleme bir örnek için bkz. bir [C# örneği](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

## <a name="troubleshooting-eventsubsciption-validation"></a>EventSubsciption doğrulama sorunlarını giderme

Olay aboneliği oluşturma sırasında, "belirtilen uç noktayı doğrulama denemesi https:\//Your-Endpoint-burada başarısız oldu gibi bir hata iletisi görüyorsanız. Daha fazla ayrıntı için https:\//aka.MS/esvalidation "adresini ziyaret edin, doğrulama el sıkışmasında bir hata olduğunu gösterir. Bu hatayı çözmek için aşağıdaki noktaları doğrulayın:

- Postman veya kıvrık ya da benzer bir aracı kullanarak [örnek bir SubscriptionValidationEvent](#validation-details) istek gövdesi ile Web kancası URL 'niz IÇIN BIR http gönderisi yapın.
- Web kancası, zaman uyumlu doğrulama el sıkışma mekanizması uygualıyorsa, yanıtın bir parçası olarak ValidationCode 'un döndürüldüğünden emin olun.
- Web kancası, zaman uyumsuz doğrulama el sıkışma mekanizmasını uygulamadıysanız, HTTP POST 'un 200 doğru olduğunu doğrulayın.
- Web kancası yanıt karşılığında 403 (yasak) döndürüyorsa, Web kancasının bir Azure Application Gateway veya Web uygulaması güvenlik duvarının arkasında olup olmadığını denetleyin. Bu durumda, bu güvenlik duvarı kurallarını devre dışı bırakıp bir HTTP POST işlemini yeniden gerçekleştirmeniz gerekir:

  920300 (istekte bir Accept üst bilgisi eksik, bunu çözebiliriz)

  942430 (kısıtlanan SQL karakter anomali algılama (args): özel karakter sayısı aşıldı (12))

  920230 (birden çok URL kodlaması algılandı)

  942130 (SQL ekleme saldırısı: SQL tautology algılandı.)

  931130 (olası uzaktan dosya ekleme (RFı) saldırısı = kapalı etki alanı başvurusu/bağlantı)

### <a name="event-delivery-security"></a>Olay teslimi güvenliği

#### <a name="azure-ad"></a>Azure AD

Azure Active Directory kullanarak Web kancası uç noktanızı güvenli hale getirerek olayları, uç noktalarınıza yayımlamak üzere Event Grid kimlik doğrulaması ve yetkilendirmek için kullanabilirsiniz. Bir Azure Active Directory uygulaması oluşturmanız, Event Grid uygulamanızda bir rol ve hizmet ilkesi oluşturmanız ve olay aboneliğini Azure AD uygulamasını kullanacak şekilde yapılandırmanız gerekir. [AAD 'yi Event Grid ile yapılandırmayı öğrenin](secure-webhook-delivery.md).

#### <a name="query-parameters"></a>Sorgu parametreleri

Bir olay aboneliği oluştururken Web kancası URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktanızı güvenli hale getirebilirsiniz. Bu sorgu parametrelerinden birini, [erişim belirteci](https://en.wikipedia.org/wiki/Access_token)gibi bir gizli dizi olacak şekilde ayarlayın. Web kancası, olayın geçerli izinlerle Event Grid geldiğini tanımak için gizli anahtarı kullanabilir. Event Grid, Web kancasına her olay teslimine bu sorgu parametrelerini dahil eder.

Olay aboneliği düzenlenirken, Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)'de [--Include-Full-Endpoint-URL](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi kullanılmadığı takdirde sorgu parametreleri gösterilmez veya döndürülmez.

Son olarak, Azure Event Grid yalnızca HTTPS Web kancası uç noktalarını desteklediğini unutmayın.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone olmak için olay kaynağına ve işleyicisine erişiminizin olduğunu kanıtlamanız gerekir. Bir Web kancasına sahip olmanız, önceki bölümde ele alınmıştır. Web kancası olmayan bir olay işleyicisi (örneğin, bir olay hub 'ı veya kuyruk depolaması) kullanıyorsanız, bu kaynağa yazma erişimi gerekir. Bu izin denetimi yetkisiz bir kullanıcının kaynağa olay göndermesini engeller.

Olay kaynağı olan kaynakta **Microsoft. EventGrid/Eventabonelikleriniz/Write** izninizin olması gerekir. Kaynak kapsamına yeni bir abonelik yazıyorsanız, bu izne ihtiyacınız vardır. Gerekli kaynak, bir sistem konusuna veya özel konuya abone olup olmadığınız temel alınarak farklılık gösterir. Her iki tür de bu bölümde açıklanmaktadır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcıları)

Sistem konuları için, olayı yayımlayan kaynağın kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, **myacct**adlı bir depolama hesabındaki bir olaya abone olmak için Microsoft. Eventgrid/Eventaboneliklerin/Write izninizin olması gerekir:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular

Özel konularda, olay Kılavuzu konusunun kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, **MyTopic**adlı özel bir konuya abone olmak için Microsoft. Eventgrid/Eventaboneliklerin/Write izninizin olması gerekir:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Özel konu yayımlama

Özel konular, paylaşılan erişim Imzası (SAS) veya anahtar kimlik doğrulaması kullanır. SAS önerilir, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok mevcut Web kancası yayımcısıyla uyumludur.

Kimlik doğrulama değerini HTTP üstbilgisine dahil edersiniz. SAS için, üst bilgi değeri için **AEG-SAS-Token** ' ı kullanın. Anahtar kimlik doğrulaması için, üst bilgi değeri için **AEG-SAS-Key** kullanın.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması en basit kimlik doğrulama biçimidir. Şu biçimi kullanın:`aeg-sas-key: <your key>`

Örneğin, şunu içeren bir anahtar geçirirsiniz:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Event Grid için SAS belirteçleri, kaynak, sona erme saati ve imza içerir. SAS belirtecinin biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak, olayları gönderdiğiniz olay Kılavuzu konusunun yoludur. Örneğin, geçerli bir kaynak yolu: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01`. Desteklenen tüm API sürümlerini görmek için bkz. [Microsoft. EventGrid kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions). 

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

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Event Grid hizmeti tarafından diske yazılan tüm olaylar veya veriler, bekleyen bir şekilde şifrelendiğinden emin olmak için Microsoft tarafından yönetilen bir anahtarla şifrelenir. Ayrıca, olayların veya verilerin saklanacağı en uzun süre, [Event Grid yeniden deneme ilkesiyle](delivery-and-retry.md)ilgili olarak 24 saat olur. Event Grid, 24 saat sonra tüm olayları veya verileri otomatik olarak siler veya olayın yaşam süresi (hangisi daha az) olur.

## <a name="endpoint-validation-with-cloudevents-v10"></a>CloudEvents v 1.0 ile uç nokta doğrulaması
Zaten Event Grid biliyorsanız, uygunsuz kullanımı önlemek için Event Grid uç nokta doğrulama elsıkışmasının farkında olabilirsiniz. CloudEvents v 1.0, HTTP SEÇENEKLERI metodunu kullanarak kendi [uygunsuz kullanım koruma semantiğini](security-authentication.md#webhook-event-delivery) uygular. Bunun hakkında daha fazla bilgiyi [buradan](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection) edinebilirsiniz. Çıkış için CloudEvents şeması kullanılırken, Event Grid Event Grid doğrulama olay mekanizması yerine CloudEvents v 1.0 kötüye kullanımı koruması ile birlikte kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

- Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
