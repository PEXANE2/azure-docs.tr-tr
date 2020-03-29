---
title: Python ile Bildirim Hub'ları nasıl kullanılır?
description: Bir Python uygulamasından Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 1ff8c382813654b1dee38a99bf2cc0ca67afbedd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76313836"
---
# <a name="how-to-use-notification-hubs-from-python"></a>Python'dan Bildirim Hub'ları nasıl kullanılır?

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

MSDN makale [Bildirim Hub'ları REST API'lerinde](https://msdn.microsoft.com/library/dn223264.aspx)açıklandığı gibi Bildirim Hub'ı REST arabirimini kullanarak java/PHP/Python/Ruby arka ucundan tüm Bildirim Hub'ları özelliklerine erişebilirsiniz.

> [!NOTE]
> Bu Python gönderir bildirim uygulamak için örnek bir başvuru uygulamasıdır ve resmi olarak desteklenen Bildirimler Hub Python SDK değildir. Örnek Python 3.4 kullanılarak oluşturuldu.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Python'daki Bildirim Hub'ları özellikleri için bir REST istemcisi oluşturun.
- Python arabirimini kullanarak bildirimleri Bildirim Hub REST API'lerine gönderin.
- Hata ayıklama/eğitim amaçlı HTTP REST isteği/yanıtı dökümünün dökümünün alın.

Python'da arka uç bölümünü uygulayarak seçtiğiniz mobil platform için [Başlat eğitimini](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) takip edebilirsiniz.

> [!NOTE]
> Örneğin kapsamı yalnızca bildirim göndermekle sınırlıdır ve herhangi bir kayıt yönetimi yapmaz.

## <a name="client-interface"></a>İstemci arabirimi

Ana istemci arabirimi [.NET Bildirim Hub'ları SDK'da](https://msdn.microsoft.com/library/jj933431.aspx)bulunan aynı yöntemleri sağlayabilir. Bu arayüz doğrudan tüm öğreticiler ve örnekler şu anda bu sitede mevcut çevirmek için izin verir ve internet üzerinde topluluk tarafından katkıda.

[Python REST sarıcı örneğinde]bulunan tüm kodları bulabilirsiniz.

Örneğin, bir istemci oluşturmak için:

```python
isDebug = True
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

Windows tost bildirimi göndermek için:

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

## <a name="implementation"></a>Uygulama

Zaten yapmadıysanız, arka uç uygulamanız gereken son bölüme kadar [Başlat'ın öğreticisini] izleyin.

Tam bir REST sarıcı uygulamak için tüm ayrıntıları [MSDN](https://msdn.microsoft.com/library/dn530746.aspx)bulunabilir. Bu bölümde, Bildirim Hub'larına erişmek ve bildirim göndermek için gereken ana adımların Python uygulaması açıklanmaktadır

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluşturma
3. HTTP REST API'yi kullanarak bildirim gönderme

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

Burada, kurucu bağlantı dizesini ayrıştıran istemciyi uygulayan ana sınıf verem:

```python
class NotificationHub:
    API_VERSION = "?api-version=2013-10"
    DEBUG_SEND = "&test"

    def __init__(self, connection_string=None, hub_name=None, debug=0):
        self.HubName = hub_name
        self.Debug = debug

        # Parse connection string
        parts = connection_string.split(';')
        if len(parts) != 3:
            raise Exception("Invalid ConnectionString.")

        for part in parts:
            if part.startswith('Endpoint'):
                self.Endpoint = 'https' + part[11:]
            if part.startswith('SharedAccessKeyName'):
                self.SasKeyName = part[20:]
            if part.startswith('SharedAccessKey'):
                self.SasKeyValue = part[16:]
```

### <a name="create-security-token"></a>Güvenlik belirteci oluşturma

Güvenlik belirteci oluşturma [ayrıntılarına buradan](https://msdn.microsoft.com/library/dn495627.aspx)ulaşabilirsiniz.
Geçerli isteğin `NotificationHub` URI'sini ve bağlantı dizesinden çıkarılan kimlik bilgilerini temel alan belirteci oluşturmak için sınıfa aşağıdaki yöntemleri ekleyin.

```python
@staticmethod
def get_expiry():
    # By default returns an expiration of 5 minutes (=300 seconds) from now
    return int(round(time.time() + 300))


@staticmethod
def encode_base64(data):
    return base64.b64encode(data)


def sign_string(self, to_sign):
    key = self.SasKeyValue.encode('utf-8')
    to_sign = to_sign.encode('utf-8')
    signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
    digest = signed_hmac_sha256.digest()
    encoded_digest = self.encode_base64(digest)
    return encoded_digest


def generate_sas_token(self):
    target_uri = self.Endpoint + self.HubName
    my_uri = urllib.parse.quote(target_uri, '').lower()
    expiry = str(self.get_expiry())
    to_sign = my_uri + '\n' + expiry
    signature = urllib.parse.quote(self.sign_string(to_sign))
    auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
    sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
    return sas_token
```

### <a name="send-a-notification-using-http-rest-api"></a>HTTP REST API'yi kullanarak bildirim gönderme

İlk olarak, kullanımı bir bildirimi temsil eden bir sınıf tanımlayın.

```python
class Notification:
    def __init__(self, notification_format=None, payload=None, debug=0):
        valid_formats = ['template', 'apple', 'fcm',
                         'windows', 'windowsphone', "adm", "baidu"]
        if not any(x in notification_format for x in valid_formats):
            raise Exception(
                "Invalid Notification format. " +
                "Must be one of the following - 'template', 'apple', 'fcm', 'windows', 'windowsphone', 'adm', 'baidu'")

        self.format = notification_format
        self.payload = payload

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
        # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        self.headers = None
```

Bu sınıf, yerel bildirim gövdesi veya şablon bildiriminin özellikleri kümesi, biçim (yerel platform veya şablon) ve platforma özgü özellikleri (Apple son kullanma özelliği ve WNS üstbilgiler gibi) içeren bir üstbilgi kümesi için bir kapsayıcıdır.

[Bildirim Hub'ları REST API belgelerine](https://msdn.microsoft.com/library/dn495827.aspx) ve mevcut tüm seçenekler için belirli bildirim platformlarının biçimlerine bakın.

Şimdi bu sınıfile, `NotificationHub` sınıfın içinde bildirim gönder yöntemlerini yazın.

```python
def make_http_request(self, url, payload, headers):
    parsed_url = urllib.parse.urlparse(url)
    connection = http.client.HTTPSConnection(
        parsed_url.hostname, parsed_url.port)

    if self.Debug > 0:
        connection.set_debuglevel(self.Debug)
        # adding this querystring parameter gets detailed information about the PNS send notification outcome
        url += self.DEBUG_SEND
        print("--- REQUEST ---")
        print("URI: " + url)
        print("Headers: " + json.dumps(headers, sort_keys=True,
                                       indent=4, separators=(' ', ': ')))
        print("--- END REQUEST ---\n")

    connection.request('POST', url, payload, headers)
    response = connection.getresponse()

    if self.Debug > 0:
        # print out detailed response information for debugging purpose
        print("\n\n--- RESPONSE ---")
        print(str(response.status) + " " + response.reason)
        print(response.msg)
        print(response.read())
        print("--- END RESPONSE ---")

    elif response.status != 201:
        # Successful outcome of send message is HTTP 201 - Created
        raise Exception(
            "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

    connection.close()


def send_notification(self, notification, tag_or_tag_expression=None):
    url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

    json_platforms = ['template', 'apple', 'fcm', 'adm', 'baidu']

    if any(x in notification.format for x in json_platforms):
        content_type = "application/json"
        payload_to_send = json.dumps(notification.payload)
    else:
        content_type = "application/xml"
        payload_to_send = notification.payload

    headers = {
        'Content-type': content_type,
        'Authorization': self.generate_sas_token(),
        'ServiceBusNotification-Format': notification.format
    }

    if isinstance(tag_or_tag_expression, set):
        tag_list = ' || '.join(tag_or_tag_expression)
    else:
        tag_list = tag_or_tag_expression

    # add the tags/tag expressions to the headers collection
    if tag_list != "":
        headers.update({'ServiceBusNotification-Tags': tag_list})

    # add any custom headers to the headers collection that the user may have added
    if notification.headers is not None:
        headers.update(notification.headers)

    self.make_http_request(url, payload_to_send, headers)


def send_apple_notification(self, payload, tags=""):
    nh = Notification("apple", payload)
    self.send_notification(nh, tags)


def send_fcm_notification(self, payload, tags=""):
    nh = Notification("fcm", payload)
    self.send_notification(nh, tags)


def send_adm_notification(self, payload, tags=""):
    nh = Notification("adm", payload)
    self.send_notification(nh, tags)


def send_baidu_notification(self, payload, tags=""):
    nh = Notification("baidu", payload)
    self.send_notification(nh, tags)


def send_mpns_notification(self, payload, tags=""):
    nh = Notification("windowsphone", payload)

    if "<wp:Toast>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'toast',
                      'X-NotificationClass': '2'}
    elif "<wp:Tile>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'tile',
                      'X-NotificationClass': '1'}

    self.send_notification(nh, tags)


def send_windows_notification(self, payload, tags=""):
    nh = Notification("windows", payload)

    if "<toast>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/toast'}
    elif "<tile>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/tile'}
    elif "<badge>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/badge'}

    self.send_notification(nh, tags)


def send_template_notification(self, properties, tags=""):
    nh = Notification("template", properties)
    self.send_notification(nh, tags)
```

Bu yöntemler, bildirim hub'ınızın /ileti bitiş noktasına, bildirimi göndermek için doğru gövde ve üstbilgiyle bir HTTP POST isteği gönderir.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Ayrıntılı günlüğe kaydetmeyi etkinleştirmek için hata ayıklama özelliğini kullanma

Bildirim Hub'ını açarken hata ayıklama özelliğini etkinleştirme, HTTP isteği ve yanıt dökümü hakkında ayrıntılı günlük bilgilerinin yanı sıra ayrıntılı Bildirim iletisi gönderme sonucunu da yazar.
[Bildirim Hub'ları TestSend özelliği,](https://docs.microsoft.com/previous-versions/azure/reference/dn495827(v=azure.100)) bildirim gönderme sonucu hakkında ayrıntılı bilgi verir.
Kullanmak için - aşağıdaki kodu kullanarak başlatma:

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

Bildirim Hub Gönder isteği HTTP URL sonuç olarak bir "test" sorgu dizesi ile eklenir.

## <a name="complete-the-tutorial"></a><a name="complete-tutorial"></a>Öğreticiyi tamamlayın

Artık bir Python arka uçtan bildirim göndererek Başlat eğitim tamamlayabilirsiniz.

Bildirim Hub'ları istemcinizi başlatın [(Başlat öğreticisinde]belirtildiği gibi bağlantı dizesini ve hub adını değiştirin):

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName")
```

Ardından, hedef mobil platformunuza bağlı olarak gönderme kodunu ekleyin. Bu örnek, örneğin windows için send_windows_notification gibi platforma dayalı bildirim göndermeyi etkinleştirmek için daha üst düzey yöntemler de ekler; send_apple_notification (elma için) vb.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Mağazası ve Windows Phone 8.1 (Silverlight olmayan)

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight

```python
hub.send_mpns_notification(toast)
```

### <a name="ios"></a>iOS

```python
alert_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_apple_notification(alert_payload)
```

### <a name="android"></a>Android

```python
fcm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_fcm_notification(fcm_payload)
```

### <a name="kindle-fire"></a>Kindle Yangın

```python
adm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_adm_notification(adm_payload)
```

### <a name="baidu"></a>Baidu

```python
baidu_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_baidu_notification(baidu_payload)
```

Python kodunuzu çalıştırmak, hedef aygıtınızda görünen bir bildirim oluşturmalıdır.

## <a name="examples"></a>Örnekler

### <a name="enabling-the-debug-property"></a>Özelliği etkinleştirme `debug`

NotificationHub'ı alırken hata ayıklama bayrağını etkinleştirdiğinizde, ayrıntılı HTTP isteği ve yanıt dökümü nün yanı sıra, istekte NE HTTP üstbilgilerinin geçtiğini ve HTTP yanıtının ne olduğunu anlayabileceğiniz aşağıdaki gibi Bildirim Sonucu'nu görürsünüz Bildirim Hub'ından alınan:

![][1]

Örneğin ayrıntılı Bildirim Hub sonucunu görürsünüz.

- ileti Anında İtme Bildirimi Hizmeti'ne başarıyla gönderildiğinde.
    ```xml
    <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
    ```
- Herhangi bir anında iletme bildirimi için hedef bulunamadıysa, büyük olasılıkla aşağıdaki çıktıyı yanıt olarak görürsünüz (bu da bildirimi teslim etmek için herhangi bir kayıt bulunmadığını gösterir, çünkü kayıtlarda bazı uyumsuzluklar vardı etiketleri)
    ```xml
    '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="https://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'
    ```

### <a name="broadcast-toast-notification-to-windows"></a>Windows'a tost bildirimi yayın

Windows istemcisine bir yayın tost bildirimi gönderirken gönderilen üstbilgidikkat edin.

```python
hub.send_windows_notification(wns_payload)
```

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Etiket (veya etiket ifadesi) belirten bildirim gönderme

HTTP isteğine eklenen Etiketler HTTP üstbilgisine dikkat edin (aşağıdaki örnekte, bildirim yalnızca 'spor' yükü olan kayıtlara gönderilir)

```python
hub.send_windows_notification(wns_payload, "sports")
```

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Birden çok etiket belirten bildirim gönderme

Birden çok etiket gönderildiğinde Etiketler HTTP üstbilgisinin nasıl değiştiğine dikkat edin.

```python
tags = {'sports', 'politics'}
hub.send_windows_notification(wns_payload, tags)
```

![][4]

### <a name="templated-notification"></a>Şablonlanmış bildirim

Http biçim üstbilgisinin değiştiğine ve yük gövdesinin HTTP istek gövdesinin bir parçası olarak gönderildiğine dikkat edin:

**İstemci tarafı - kayıtlı şablon:**

```python
var template = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
```

**Sunucu tarafı - taşıma gönderme:**

```python
template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
hub.send_template_notification(template_payload)
```

![][5]

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede, Bildirim Hub'ları için bir Python REST istemcisi nasıl oluşturulacak gösterin. Burada yapabilecekleriniz:

- Bu makaledeki tüm kodu içeren tam [Python REST sarıcı örneğini]indirin.
- [Son Dakika Haberleri öğreticisinde] Bildirim Hub'ları etiketleme özelliği hakkında bilgi edinmeye devam edin
- [Yerelleştirme Haberleri öğreticisinde] Bildirim Hub'ları Şablonları özelliği hakkında bilgi edinmeye devam edin

<!-- URLs -->
[Python REST sarıcı örneği]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Başlangıç öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Son Dakika Haberleri]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Haberler öğreticinin yerelleştirilmesi]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
