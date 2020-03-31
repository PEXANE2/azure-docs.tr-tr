---
title: Kayıt Yönetimi
description: Bu konu, anında iletme bildirimleri almak için aygıtların bildirim hub'larına nasıl kaydedilenle açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2020
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 04/08/2019
ms.openlocfilehash: 00de9c803ef796eda8da609a4009e0a8cfcb3664
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455376"
---
# <a name="registration-management"></a>Kayıt yönetimi

Bu konu, anında iletme bildirimleri almak için aygıtların bildirim hub'larına nasıl kaydedilenle açıklanmaktadır. Konu, kayıtları yüksek düzeyde açıklar, ardından aygıtları kaydetmek için iki ana delek sunar: aygıttan doğrudan bildirim merkezine kaydolmak ve bir uygulama arka ucu üzerinden kaydolmak.

## <a name="what-is-device-registration"></a>Cihaz kaydı nedir

Bildirim Hub'ı ile cihaz **kaydı, Kayıt** veya **Yükleme**kullanılarak gerçekleştirilir.

### <a name="registrations"></a>Kayıtlar

Kayıt, etiketleri ve şablonu olan bir aygıt için Platform Bildirim Hizmeti (PNS) tutamacını ilişkilendirer. PNS tutamacı bir ChannelURI, aygıt belirteci veya FCM kayıt kimliği olabilir. Etiketler, bildirimleri doğru aygıt tutamaçları kümesine yönlendirmek için kullanılır. Daha fazla bilgi için [Yönlendirme ve Etiket İfadeleri'ne](notification-hubs-tags-segment-push-message.md)bakın. Şablonlar, kayıt başına dönüştürmeyi uygulamak için kullanılır. Daha fazla bilgi için bkz. [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md).

> [!NOTE]
> Azure Bildirim Hub'ları cihaz başına en fazla 60 etiketi destekler.

### <a name="installations"></a>Yüklemeler

Yükleme, itme ile ilgili özellikleri bir çanta içeren gelişmiş bir kayıttır. Cihazlarınızı kaydetmek için en son ve en iyi yaklaşımdır. Ancak, henüz istemci tarafı .NET SDK ([Arka uç işlemleri için Bildirim Hub SDK)](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)tarafından desteklenmemektedir.  Bu, istemci aygıtının kendisinden kaydoluyorsanız, yüklemeleri desteklemek için [Bildirim Hub'ları REST API](/rest/api/notificationhubs/create-overwrite-installation) yaklaşımını kullanmanız gerekeceği anlamına gelir. Bir arka uç hizmeti kullanıyorsanız, [arka uç işlemleri için Bildirim Hub SDK'yı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)kullanabilmelisin.

Yüklemeleri kullanmanın bazı önemli avantajları şunlardır:

- Bir yükleme oluşturma veya güncelleme tamamen idempotent olduğunu. Böylece yinelenen kayıtlar hakkında herhangi bir endişe duymadan yeniden deneyebilirsiniz.
- Yükleme modeli, bildirimin doğrudan`$InstallationId:{INSTALLATION_ID}`belirli aygıta gönderilmesini sağlayan özel bir etiket biçimini () destekler. Örneğin, uygulamanın kodu bu aygıt `joe93developer` için bir yükleme kimliği ayarlarsa, bir geliştirici `$InstallationId:{joe93developer}` etikete bildirim gönderirken bu aygıtı hedefleyebilir. Bu, herhangi bir ek kodlama yapmanıza gerek kalmadan belirli bir aygıtı hedeflemenize olanak tanır.
- Yüklemeleri kullanmak, kısmi kayıt güncelleştirmeleri yapmanızı da sağlar. Bir yüklemenin kısmi güncelleştirmesi, [JSON-Patch standardını](https://tools.ietf.org/html/rfc6902)kullanan bir PATCH yöntemi yle istenir. Bu, kayıt üzerindeki etiketleri güncelleştirmek istediğinizde yararlıdır. Tüm kaydı aşağı çekmeniz ve ardından önceki etiketlerin tamamını yeniden göndermeniz gerekmez.

Yükleme aşağıdaki özellikleri içerebilir. Yükleme özelliklerinin tam listesi için, [REST API veya](/rest/api/notificationhubs/create-overwrite-installation) Yükleme [Özellikleri](/dotnet/api/microsoft.azure.notificationhubs.installation)ile Yükleme Oluştur veya Üzerine Yazma bölümüne bakın.

```json
// Example installation format to show some supported properties
{
    installationId: "",
    expirationTime: "",
    tags: [],
    platform: "",
    pushChannel: "",
    ………
    templates: {
        "templateName1" : {
            body: "",
            tags: [] },
        "templateName2" : {
            body: "",
            // Headers are for Windows Store only
            headers: {
                "X-WNS-Type": "wns/tile" }
            tags: [] }
    },
    secondaryTiles: {
        "tileId1": {
            pushChannel: "",
            tags: [],
            templates: {
                "otherTemplate": {
                    bodyTemplate: "",
                    headers: {
                        ... }
                    tags: [] }
            }
        }
    }
}
```

> [!NOTE]
> Varsayılan olarak, kayıtlar ve yüklemeler sona ermez.

Kayıtlar ve kurulumlar her cihaz/kanal için geçerli bir PNS tutamacı içermelidir. PNS tutamaçları yalnızca aygıttaki bir istemci uygulamasında elde edilebildiği için, bir desen doğrudan istemci uygulamasıyla bu cihaza kaydolmaktır. Diğer taraftan, etiketlerle ilgili güvenlik konuları ve iş mantığı, uygulamanın arka ucunda cihaz kaydını yönetmeniz gerektirebilir.

> [!NOTE]
> Yüklemeler API Baidu hizmetini desteklemez (Kayıtlar API'si olsa da). 

### <a name="templates"></a>Şablonlar

[Şablonlar](notification-hubs-templates-cross-platform-push-messages.md)kullanmak istiyorsanız, aygıt yüklemesi de json biçiminde bu aygıtla ilişkili tüm şablonları tutar (yukarıdaki örneğe bakın). Şablon adları, aynı aygıt için farklı şablonları hedeflemeye yardımcı olur.

Her şablon adı, bir şablon gövdesi ve isteğe bağlı bir etiket kümesiyle eşlenir. Ayrıca, her platform ek şablon özellikleri olabilir. Windows Mağazası (WNS kullanarak) ve Windows Phone 8 (MPNS kullanarak) için, şablonun bir parçası olabilir. APN'ler söz konusu olduğunda, bir son kullanma özelliğini sabit veya şablon ifadesine ayarlayabilirsiniz. Yükleme özelliklerinin tam listesi için [bkz.](/rest/api/notificationhubs/create-overwrite-installation)

### <a name="secondary-tiles-for-windows-store-apps"></a>Windows Mağazası Uygulamaları için İkincil Kutucuklar

Windows Mağazası istemci uygulamaları için, ikincil kutucuklara bildirim göndermek, birincil uygulamaya göndermekle aynıdır. Bu da yüklemelerde desteklenir. İkincil kutucuklar, istemci uygulamanızdaki SDK'nın saydam olarak işlediği farklı bir ChannelUri'ye sahiptir.

İkincil Kutucuklar sözlüğü, Windows Mağazası uygulamanızda İkincil Kutucuklar nesnesini oluşturmak için kullanılan aynı TileId'i kullanır. Birincil ChannelUri olduğu gibi, ikincil fayans ChannelUris her an değiştirebilirsiniz. Bildirim merkezindeki yüklemeleri güncel tutmak için, aygıtın ikincil kutucukların geçerli ChannelUris'i ile yenilemesi gerekir.

## <a name="registration-management-from-the-device"></a>Cihazdan kayıt yönetimi

İstemci uygulamalarından cihaz kaydını yönetirken, arka uç yalnızca bildirim göndermekten sorumludur. İstemci uygulamaları PNS işlemlerini güncel tutar ve etiketleri kaydeder. Aşağıdaki resim bu deseni göstermektedir.

![Cihazdan kayıt](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Aygıt önce PNS'den PNS tutamacını alır, sonra doğrudan bildirim hub'ına kaydeder. Kayıt başarılı olduktan sonra, uygulama arka ucu bu kaydı hedefleyen bir bildirim gönderebilir. Bildirimlerin nasıl gönderilen hakkında daha fazla bilgi için [Yönlendirme ve Etiketler İfadeleri'ne](notification-hubs-tags-segment-push-message.md)bakın.

Bu durumda, bildirim hub'larınıza aygıttan erişmek için yalnızca Dinleme haklarını kullanırsınız. Daha fazla bilgi için [Güvenlik'e](notification-hubs-push-notification-security.md)bakın.

Aygıttan kaydolmak en basit yöntemdir, ancak bazı dezavantajları vardır:

- İstemci uygulaması etiketlerini yalnızca uygulama etkin olduğunda güncelleyebilir. Örneğin, bir kullanıcının spor takımlarla ilgili etiketleri kaydeden iki aygıtı varsa, ilk aygıt ek bir etiket için kaydolduğunda (örneğin, Seahawks), ikinci cihazdaki uygulama gelene kadar Seahawks hakkındaki bildirimleri almaz ikinci kez yürütülür. Daha genel olarak, etiketler birden çok aygıttan etkilendiğinde, etiketleri arka uçtan yönetmek istenen bir seçenektir.
- Uygulamalar hacklenebildiği için, [güvenlik](notification-hubs-push-notification-security.md)makalesinde açıklandığı gibi, belirli etiketlere kaydın güvence altına alınması ekstra bakım gerektirir.

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Yükleme kullanan bir aygıttan bildirim hub'ına kaydolmak için örnek kod

Şu anda, bu yalnızca [Bildirim Hub'ları REST API](/rest/api/notificationhubs/create-overwrite-installation)kullanılarak desteklenir.

Yüklemeyi güncelleştirmek için [JSON-Patch standardını](https://tools.ietf.org/html/rfc6902) kullanarak PATCH yöntemini de kullanabilirsiniz.

```csharp
class DeviceInstallation
{
    public string installationId { get; set; }
    public string platform { get; set; }
    public string pushChannel { get; set; }
    public string[] tags { get; set; }
}

private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
        string hubName, string listenConnectionString)
{
    if (deviceInstallation.installationId == null)
        return HttpStatusCode.BadRequest;

    // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
    ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
    string hubResource = "installations/" + deviceInstallation.installationId + "?";
    string apiVersion = "api-version=2015-04";

    // Determine the targetUri that we will sign
    string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

    //=== Generate SaS Security Token for Authorization header ===
    // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
    string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

    using (var httpClient = new HttpClient())
    {
        string json = JsonConvert.SerializeObject(deviceInstallation);

        httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

        var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
        return response.StatusCode;
    }
}

var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

string installationId = null;
var settings = ApplicationData.Current.LocalSettings.Values;

// If we have not stored an installation ID in application data, create and store as application data.
if (!settings.ContainsKey("__NHInstallationId"))
{
    installationId = Guid.NewGuid().ToString();
    settings.Add("__NHInstallationId", installationId);
}

installationId = (string)settings["__NHInstallationId"];

var deviceInstallation = new DeviceInstallation
{
    installationId = installationId,
    platform = "wns",
    pushChannel = channel.Uri,
    //tags = tags.ToArray<string>()
};

var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                    "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

if (statusCode != HttpStatusCode.Accepted)
{
    var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
    dialog.Commands.Add(new UICommand("OK"));
    await dialog.ShowAsync();
}
else
{
    var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
    dialog.Commands.Add(new UICommand("OK"));
    await dialog.ShowAsync();
}
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Kayıt kullanarak bir cihazdan bildirim hub'ına kaydolmak için örnek kod

Bu yöntemler, çağrıldıkları aygıt için bir kayıt oluşturur veya güncelleştirin. Bu, tanıtıcıyı veya etiketleri güncelleştirmek için tüm kaydın üzerine yazmanız gerektiği anlamına gelir. Kayıtların geçici olduğunu unutmayın, bu nedenle belirli bir aygıtın ihtiyaç duyduğu geçerli etiketlere sahip güvenilir bir mağazanız olmalıdır.

```csharp
// Initialize the Notification Hub
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

// The Device ID from the PNS
var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

// If you are registering from the client itself, then store this registration ID in device
// storage. Then when the app starts, you can check if a registration ID already exists or not before
// creating.
var settings = ApplicationData.Current.LocalSettings.Values;

// If we have not stored a registration ID in application data, store in application data.
if (!settings.ContainsKey("__NHRegistrationId"))
{
    // make sure there are no existing registrations for this push handle (used for iOS and Android)    
    string newRegistrationId = null;
    var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
    foreach (RegistrationDescription registration in registrations)
    {
        if (newRegistrationId == null)
        {
            newRegistrationId = registration.RegistrationId;
        }
        else
        {
            await hub.DeleteRegistrationAsync(registration);
        }
    }

    newRegistrationId = await hub.CreateRegistrationIdAsync();

    settings.Add("__NHRegistrationId", newRegistrationId);
}

string regId = (string)settings["__NHRegistrationId"];

RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
registration.RegistrationId = regId;
registration.Tags = new HashSet<string>(YourTags);

try
{
    await hub.CreateOrUpdateRegistrationAsync(registration);
}
catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
{
    settings.Remove("__NHRegistrationId");
}
```

## <a name="registration-management-from-a-backend"></a>Arka uçtan kayıt yönetimi

Kayıtları arka uçtan yönetme ek kod yazmayı gerektirir. Aygıttaki uygulama, uygulama her başlatında (etiketler ve şablonlarla birlikte) güncelleştirilmiş PNS tutamacını arka uca sağlamalı ve arka uç bu tutamacı bildirim hub'ında güncelleştirmelidir. Aşağıdaki resim bu tasarımı göstermektedir.

![Kayıt yönetimi](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Kayıtları arka uçtan yönetmenin avantajları arasında, aygıttaki ilgili uygulama etkin olmasa bile etiketleri kayıtlara değiştirebilme ve kaydına bir etiket eklemeden önce istemci uygulamasının kimliğini nitre etme olanağı bulunur.

### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Yükleme kullanarak arka uçtan bir bildirim hub'ına kaydolmak için örnek kod

İstemci aygıtı hala pns tutamacını ve ilgili yükleme özelliklerini eskisi gibi alır ve arka uçta kayıt ve yetki etiketleri vb gerçekleştirebilirsiniz özel bir API çağırır. Arka uç, [arka uç işlemleri için Bildirim Hub SDK'dan](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)yararlanabilir.

Yüklemeyi güncelleştirmek için [JSON-Patch standardını](https://tools.ietf.org/html/rfc6902) kullanarak PATCH yöntemini de kullanabilirsiniz.

```csharp
// Initialize the Notification Hub
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

// Custom API on the backend
public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
{

    Installation installation = new Installation();
    installation.InstallationId = deviceUpdate.InstallationId;
    installation.PushChannel = deviceUpdate.Handle;
    installation.Tags = deviceUpdate.Tags;

    switch (deviceUpdate.Platform)
    {
        case "mpns":
            installation.Platform = NotificationPlatform.Mpns;
            break;
        case "wns":
            installation.Platform = NotificationPlatform.Wns;
            break;
        case "apns":
            installation.Platform = NotificationPlatform.Apns;
            break;
        case "fcm":
            installation.Platform = NotificationPlatform.Fcm;
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }


    // In the backend we can control if a user is allowed to add tags
    //installation.Tags = new List<string>(deviceUpdate.Tags);
    //installation.Tags.Add("username:" + username);

    await hub.CreateOrUpdateInstallationAsync(installation);

    return Request.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Kayıt kimliği kullanarak bir cihazdan bildirim hub'ına kaydolmak için örnek kod

Uygulama arka nızın arkasından, kayıtlarda temel CRUDS işlemlerini gerçekleştirebilirsiniz. Örnek:

```csharp
var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

// create a registration description object of the correct type, e.g.
var reg = new WindowsRegistrationDescription(channelUri, tags);

// Create
await hub.CreateRegistrationAsync(reg);

// Get by ID
var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

// update
r.Tags.Add("myTag");

// update on hub
await hub.UpdateRegistrationAsync(r);

// delete
await hub.DeleteRegistrationAsync(r);
```

Arka uç, kayıt güncelleştirmeleri arasında eşzamanlılık ele almalıdır. Service Bus, kayıt yönetimi için iyimser eşzamanlılık denetimi sunar. HTTP düzeyinde, bu kayıt yönetimi işlemlerinde ETag kullanımı ile uygulanır. Bu özellik, eşzamanlılık nedenleriyle bir güncelleştirme reddedilirse özel bir durum oluşturan Microsoft SDK'lar tarafından saydam olarak kullanılır. Uygulama arka ucu, bu özel durumları işlemekten ve gerekirse güncelleştirmeyi yeniden denemekten sorumludur.
