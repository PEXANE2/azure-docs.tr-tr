---
title: Azure Olay Izgara güvenliği ve kimlik doğrulaması
description: Bu makalede, Olay Izgara kaynaklarınıza (WebHook, abonelikler, özel konular) erişimin kimlik doğrulaması yapmanın farklı yolları açıklanmaktadır.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: babanisa
ms.openlocfilehash: 0b7c5b42ac6291c6687337ba8d6a9d35830b9bda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281021"
---
# <a name="authenticating-access-to-event-grid-resources"></a>Olay Izgara kaynaklarına erişimin doğrulanması

Azure Olay Izgarası'nın üç tür kimlik doğrulaması vardır:

* WebHook etkinlik teslimi
* Olay abonelikleri
* Özel konu yayımlama

## <a name="webhook-event-delivery"></a>WebHook Etkinliği teslimatı

Webhooks, Azure Olay Sgrid'den etkinlik almanın birçok yollarından biridir. Yeni bir olay hazır olduğunda, Olay Izgara hizmeti, istek gövdesindeki olayla birlikte yapılandırılan bitiş noktasına bir HTTP isteği verir.

Webhooks destekleyen diğer birçok hizmet gibi, Olay Grid bu uç noktaya olayları teslim başlamadan önce Webhook bitiş noktası nın sahipliğini kanıtlamak için gerektirir. Bu gereksinim, kötü amaçlı bir kullanıcının bitiş noktanızı olaylarla doldurmasını önler. Aşağıda listelenen üç Azure hizmetinden herhangi birini kullandığınızda, Azure altyapısı bu doğrulamayı otomatik olarak işler:

* [Olay Izgara Bağlayıcısı](https://docs.microsoft.com/connectors/azureeventgrid/) ile Azure Mantık Uygulamaları
* Webhook üzerinden Azure [Otomasyonu](../event-grid/ensure-tags-exists-on-new-virtual-machines.md)
* [Olay Izgara Tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) ile Azure Fonksiyonları

HTTP tetikleyici tabanlı Azure işlevi gibi başka bir uç nokta türü kullanıyorsanız, bitiş noktası kodunuz Olay Grid ile doğrulama el sıkışmasına katılması gerekir. Olay Grid aboneliği doğrulamanın iki yolunu destekler.

1. **ValidationCode el sıkışma (programlı)**: Bitiş noktanızın kaynak kodunu kontrol ediyorsanız, bu yöntem önerilir. Olay aboneliği oluşturuldurma sırasında, Olay Grid bitiş noktanıza bir abonelik doğrulama olayı gönderir. Bu olayın şeması diğer Olay Izgara olayına benzer. Bu olayın veri bölümü `validationCode` bir özellik içerir. Uygulamanız doğrulama isteğinin beklenen bir olay aboneliği için olduğunu doğrular ve doğrulama kodunu Olay Grid'ine yansıtır. Bu el sıkışma mekanizması tüm Event Grid sürümlerinde desteklenir.

2. **ValidationURL el sıkışma (manuel)**: Bazı durumlarda, Doğrulama Kodu el sıkışmauygulamak için bitiş noktasının kaynak koduna erişemezsiniz. Örneğin, bir üçüncü taraf hizmeti [(Zapier](https://zapier.com) veya [IFTTT](https://ifttt.com/)gibi) kullanıyorsanız, doğrulama koduyla programlı olarak yanıt veremezsiniz.

   Sürüm 2018-05-01 önizleme ile başlayarak, Olay Grid manuel doğrulama el sıkışma destekler. API sürümü 2018-05-01 önizleme veya daha sonra kullanan bir SDK veya araçla bir `validationUrl` olay aboneliği oluşturuyorsanız, Olay Grid abonelik doğrulama olayının veri bölümünde bir özellik gönderir. El sıkışmayı tamamlamak için, olay verilerindeki URL'yi bulun ve el ile get isteği gönderin. Rest istemcisi veya web tarayıcınızı kullanabilirsiniz.

   Sağlanan URL 5 dakika süreyle geçerlidir. Bu süre zarfında, olay aboneliğinin sağlama `AwaitingManualAction`durumu . Kılavuz doğrulamayı 5 dakika içinde tamamlamazsanız, sağlama durumu ' `Failed`ya ayarlanır. El kitabı doğrulamayı başlatmadan önce olay aboneliğini yeniden oluşturmanız gerekir.

    Bu kimlik doğrulama mekanizması, webhook bitiş noktasının 200'lük bir HTTP durum kodunu döndürmesini de gerektirir, böylece doğrulama olayı için POST'un el ile doğrulama moduna konulmadan önce kabul edildiğini bilir. Başka bir deyişle, bitiş noktası 200 döndürür ancak programlı bir doğrulama yanıtı geri dönmezse, mod el ile doğrulama moduna geçirilir. Doğrulama URL'sinde 5 dakika içinde GET varsa, doğrulama el sıkışması başarılı olarak kabul edilir.

> [!NOTE]
> Doğrulama için kendi imzalı sertifikaların kullanılması desteklenmez. Bunun yerine bir sertifika yetkilisinden (CA) imzalı bir sertifika kullanın.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme sırasında, Olay Izgarası hedef bitiş noktasına bir abonelik doğrulama olayı gönderir. 
* Olay bir üstbilgi değeri "aeg-olay türü: Abonelik Geçersiz" içerir.
* Olay gövdesi, diğer Olay Izgara olayları ile aynı şema sahiptir.
* Olayın eventType özelliği . `Microsoft.EventGrid.SubscriptionValidationEvent`
* Olayın veri özelliği rasgele `validationCode` oluşturulan dize ile bir özellik içerir. Örneğin, "doğrulamaKodu: acb13...".
* Olay verileri, aboneliği `validationUrl` el ile doğrulamak için URL'si olan bir özellik de içerir.
* Dizi yalnızca doğrulama olayını içerir. Doğrulama kodunu geri döndürdünen sonra diğer olaylar ayrı bir istekle gönderilir.
* EventGrid DataPlane SDK'ların abonelik doğrulama olay verilerine ve abonelik doğrulama yanıtına karşılık gelen sınıflar vardır.

Bir örnek SubscriptionValidationEvent aşağıdaki örnekte gösterilir:

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

Bitiş noktası sahipliğini kanıtlamak için, aşağıdaki örnekte gösterildiği gibi doğrulamaYanıtı özelliğindeki doğrulama kodunu geri döndürün:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

HTTP 200 Ok yanıt durum kodunu döndürmeniz gerekir. HTTP 202 Kabul geçerli bir Olay Grid abonelik doğrulama yanıtı olarak tanınmıyor. Http isteği 30 saniye içinde tamamlanmalıdır. İşlem 30 saniye içinde bitmezse, işlem iptal edilir ve 5 saniye sonra yeniden denenebilir. Tüm denemeler başarısız olursa, o zaman doğrulama el sıkışma hatası olarak kabul edilecektir.

Veya doğrulama URL'sine get isteği göndererek aboneliği el ile doğrulayabilirsiniz. Olay aboneliği doğrulanına kadar bekleme durumunda kalır. Doğrulama Url'si 553 bağlantı noktasını kullanır. Güvenlik duvarı kurallarınız bağlantı noktası 553'ü engelliyorsa, başarılı el ile el sıkışma için kuralların güncellenmesi gerekebilir.

Abonelik doğrulama el sıkışmasını işleme örneği için [bir C# örneğine](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs)bakın.

### <a name="checklist"></a>Denetim Listesi

Olay aboneliği oluşturma sırasında, "Sağlanan bitiş noktası https doğrulama girişimi:\//bitiş noktası-burada başarısız oldu gibi bir hata iletisi görüyorsanız. Daha fazla bilgi için\/https: /aka.ms/esvalidation" adresini ziyaret edin, doğrulama el sıkışmasında bir hata olduğunu gösterir. Bu hatayı gidermek için aşağıdaki yönleri doğrulayın:

* Hedef bitiş noktasında çalışan uygulama kodunu denetler misiniz? Örneğin, HTTP tetikleyici tabanlı Azure İşlevi yazıyorsanız, uygulamada değişiklik yapmak için uygulama koduna erişiminiz var mı?
* Uygulama koduna erişiminiz varsa, yukarıdaki örnekte gösterildiği gibi Doğrulama Kodu tabanlı el sıkışma mekanizmasını uygulayın.

* Uygulama koduna erişiminiz yoksa (örneğin, web hooks'u destekleyen bir üçüncü taraf hizmeti kullanıyorsanız), manuel el sıkışma mekanizmasını kullanabilirsiniz. Doğrulama olayındaki doğrulamaUrl'sini almak için 2018-05-01 önizleme API sürümünü veya daha sonrasını (Olay Grid Azure CLI uzantısını yükleyin) kullandığınızdan emin olun. Manuel doğrulama el sıkışmasını tamamlamak için `validationUrl` özelliğin değerini alın ve web tarayıcınızdaki url'yi ziyaret edin. Doğrulama başarılı olursa, web tarayıcınızda doğrulamanın başarılı olduğuna dair bir ileti görmeniz gerekir. Olay aboneliğinin provisioningState'inin "Başarılı" olduğunu görürsünüz. 

### <a name="event-delivery-security"></a>Olay teslim güvenliği

#### <a name="azure-ad"></a>Azure AD

Olay Sgara'nın kimliğini doğrulamak ve olayları uç noktalarınızda yayımlamak için yetkilendirmek için Azure Active Directory'yi kullanarak webhook bitiş noktanızı güvenebilirsiniz. Bir Azure Etkin Dizin Uygulaması oluşturmanız, Etkinlik Ağıtını yetkilendirme uygulamanızda bir rol ve hizmet ilkesi oluşturmanız ve etkinlik aboneliğini Azure AD Uygulamasını kullanacak şekilde yapılandırmanız gerekir. [AAD'yi Olay Izgarasıyla nasıl yapılandırılamayı öğrenin.](secure-webhook-delivery.md)

#### <a name="query-parameters"></a>Sorgu parametreleri
Olay Aboneliği oluştururken sorgu parametrelerini webhook URL'sine ekleyerek webhook bitiş noktanızı güvenebilirsiniz. Bu sorgu parametrelerinden birini [erişim belirteci](https://en.wikipedia.org/wiki/Access_token)gibi gizli olarak ayarlayın. Webhook, olayın geçerli izinlerle Olay Izgara'dan geldiğini tanımak için sırrı kullanabilir. Olay Grid webhook her olay teslim bu sorgu parametreleri içerecektir.

Olay Aboneliği'ni düzenlerken, Azure [CLI'de](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) [--dahil-tam bitiş noktası-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi kullanılmadığı sürece sorgu parametreleri görüntülenmez veya döndürülemez.

Son olarak, Azure Olay Izgara'nın yalnızca HTTPS webhook uç noktalarını desteklediğini unutmayın.

## <a name="event-subscription"></a>Olay aboneliği

Bir etkinliğe abone olmak için, olay kaynağına ve işleyicisine erişiminiz olduğunu kanıtlamanız gerekir. Bir WebHook'a sahip olduğunuzu kanıtlayan bir önceki bölümde yer alıyordu. WebHook olmayan bir olay işleyicisi (olay hub'ı veya sıra depolama alanı gibi) kullanıyorsanız, bu kaynağa yazma erişimi ne gerekir. Bu izin denetimi, yetkisiz bir kullanıcının kaynağınıza olay göndermesini engeller.

Etkinlik kaynağı olan kaynakta **Microsoft.EventGrid/EventSubscriptions/Write** iznine sahip olmalısınız. Kaynağın kapsamında yeni bir abonelik yazdığından bu izne ihtiyacınız var. Gerekli kaynak, bir sistem konusuna mı yoksa özel bir konuya mı abone olduğunuza bağlı olarak farklılık gösterir. Her iki tür de bu bölümde açıklanmıştır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcılar)

Sistem konuları için, olayı yayımlama kaynağıkapsamında yeni bir olay aboneliği yazmak için izin almanız gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, **myacct**adlı bir depolama hesabındaki bir etkinliğe abone olmak için Microsoft.EventGrid/EventSubscriptions/Write iznine ihtiyacınız vardır:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konu başlıkları

Özel konular için, olay ızgarası konusukapsamında yeni bir olay aboneliği yazmak için izin almanız gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, **mytopic**adlı özel bir konuya abone olmak için Microsoft.EventGrid/EventSubscriptions/Write iznine ihtiyacınız vardır:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Özel konu yayımlama

Özel konular Paylaşılan Erişim İmzası (SAS) veya anahtar kimlik doğrulaması kullanır. SAS'ı öneririz, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok varolan webhook yayımcısıyla uyumludur. 

Http üstbilgisinde kimlik doğrulama değerini eklersiniz. SAS için, üstbilgi değeri için **aeg-sas-token** kullanın. Anahtar kimlik doğrulaması için üstbilgi değeri için **aeg-sas-key'i** kullanın.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması, kimlik doğrulamanın en basit biçimidir. Biçimi kullanın:`aeg-sas-key: <your key>`

Örneğin, bir anahtarı niçin geçersiniz:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Olay Izgarası için SAS belirteçleri kaynak, bir son kullanma süresi ve imza içerir. SAS belirteci biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak, olayları gönderdiğiniz olay ızgarası konusunun yoludur. Örneğin, geçerli bir kaynak yolu:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

İmzayı bir anahtardan oluşturursunuz.

Örneğin, geçerli bir **aeg-sas-belirteç** değeri:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Aşağıdaki örnek, Olay Grid'de kullanılmak üzere bir SAS belirteci oluşturur:

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

Olay Izgara hizmeti tarafından diske yazılan tüm olaylar veya veriler, microsoft tarafından yönetilen bir anahtarla şifrelenir ve bu durum istirahatte şifrelenir. Ayrıca, olayların veya verilerin tutulduğu maksimum [süre, Olay Izgarayeniden deneme ilkesine](delivery-and-retry.md)bağlı olarak 24 saattir. Olay Grid, 24 saat sonra tüm olayları veya verileri veya hangisi daha azsa, etkinlik süresini otomatik olarak siler.

## <a name="next-steps"></a>Sonraki adımlar

* Olay Izgarasına giriş için [Olay Izgarası Hakkında'ya](overview.md) bakın
