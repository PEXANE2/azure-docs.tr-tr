---
title: PHP ile Azure Bildirim Hub'ları nasıl kullanılır?
description: Php arka uçtan Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 9a77a9d9c8b2d71197089f66d81e07d56c780e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263855"
---
# <a name="how-to-use-notification-hubs-from-php"></a>PHP'den Bildirim Hub'ları nasıl kullanılır?

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

MSDN konu [Bildirim Hub'ları REST API'lerinde](https://msdn.microsoft.com/library/dn223264.aspx)açıklandığı gibi Bildirim Hub'ı REST arabirimini kullanarak Java/PHP/Ruby arka ucundan tüm Bildirim Hub'ları özelliklerine erişebilirsiniz.

Bu konuda nasıl göstereceğimiz:

* PHP'deki Bildirim Hub'ları özellikleri için bir REST istemcisi oluşturun;
* PHP'de arka uç bölümünü uygulayarak seçtiğiniz mobil platform için [Eğitime Başla'yı](notification-hubs-ios-apple-push-notification-apns-get-started.md) izleyin.

## <a name="client-interface"></a>İstemci arabirimi

Ana istemci [arabirimi, .NET Bildirim Hub'ları SDK'da](https://msdn.microsoft.com/library/jj933431.aspx)bulunan ve şu anda bu sitede bulunan tüm öğreticileri ve örnekleri doğrudan tercüme etmenizi sağlayan ve topluluk tarafından internet üzerinden katkıda bulunan aynı yöntemleri sağlayabilir.

[PHP REST sarıcı örneğinde]bulunan tüm kodları bulabilirsiniz.

Örneğin, bir istemci oluşturmak için:

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

iOS yerel bildirimi göndermek için:

    ```php
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);
    ```

## <a name="implementation"></a>Uygulama

Zaten yapmadıysanız, arka uç uygulamanız gereken son bölüme kadar [Başlat'ın öğreticisini] izleyin.
Ayrıca, isterseniz [PHP REST sarıcı örneğinden] kodu kullanabilirsiniz ve doğrudan öğretici bölümüne [tamamlayın.](#complete-tutorial)

Tam bir REST sarıcı uygulamak için tüm ayrıntıları [MSDN](https://msdn.microsoft.com/library/dn530746.aspx)bulunabilir. Bu bölümde, Bildirim Hub'larına erişmek için gerekli ana adımların PHP uygulamasını açıklarız:

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirteci oluşturma
3. HTTP aramasını gerçekleştirin

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

Bağlantı dizesini ayrıştıran oluşturucu istemciyi uygulayan ana sınıf aşağıda veda edebilirsiniz:

    ```php
    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }
    ```

### <a name="create-a-security-token"></a>Güvenlik belirteci oluşturma

[SAS Güvenlik Belirteci Oluşturma](https://docs.microsoft.com/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token)hakkında bilgi için Azure belgelerine bakın.

Geçerli `generateSasToken` isteğin `NotificationHub` URI'sini ve bağlantı dizesinden çıkarılan kimlik bilgilerini temel alan belirteci oluşturmak için sınıfa yöntemi ekleyin.

    ```php
    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }
    ```

### <a name="send-a-notification"></a>Bildirim gönderme

İlk olarak, bir bildirimi temsil eden bir sınıf tanımlayalım.

    ```php
    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "fcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }
    ```

Bu sınıf, yerel bildirim gövdesi için bir kapsayıcı veya şablon bildirimi durumunda bir dizi özellik ve biçim (yerel platform veya şablon) ve platforma özgü özellikleri (Apple son kullanma özelliği ve WNS gibi) içeren bir üstbilgi kümesidir üstbilgi).

[Bildirim Hub'ları REST API belgelerine](https://msdn.microsoft.com/library/dn495827.aspx) ve mevcut tüm seçenekler için belirli bildirim platformlarının biçimlerine bakın.

Bu sınıfla donanan, artık sınıfın içinde `NotificationHub` bildirim gönder yöntemlerini yazabiliriz:

    ```php
    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "fcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 
    ```

Yukarıdaki yöntemler, bildirimi göndermek için `/messages` doğru gövde ve üstbilgiile bildirim hub'ınızın bitiş noktasına bir HTTP POST isteği gönderir.

## <a name="complete-the-tutorial"></a><a name="complete-tutorial"></a>Öğreticiyi tamamlayın

Artık bildirimi PHP arka ucundan göndererek Başlat eğitimini tamamlayabilirsiniz.

Bildirim Hub'ları istemcinizi başlatın [(Başlat öğreticisinde]belirtildiği gibi bağlantı dizesini ve hub adını değiştirin):

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Ardından, hedef mobil platformunuza bağlı olarak gönderme kodunu ekleyin.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Mağazası ve Windows Phone 8.1 (Silverlight olmayan)

    ```php
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);
    ```

### <a name="ios"></a>iOS

    ```php
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);
    ```

### <a name="android"></a>Android

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("fcm", $message);
    $hub->sendNotification($notification, null);
    ```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ve 8.1 Silverlight

    ```php
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);
    ```

### <a name="kindle-fire"></a>Kindle Yangın

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);
    ```

PHP kodunuzu çalıştırmak, artık hedef aygıtınızda görünen bir bildirim oluşturmalıdır.

## <a name="next-steps"></a>Sonraki Adımlar

Bu konuda, Bildirim Hub'ları için basit bir Java REST istemcisi nasıl oluşturulabileceğimizi gösterdik. Burada yapabilecekleriniz:

* Yukarıdaki tüm kodu içeren tam [PHP REST sarıcı örneğini]indirin.
* Bildirim Hub'ları etiketleme özelliği hakkında bilgi edinmeye devam edin [Son Dakika Haberleri öğretici]
* [Kullanıcıları Bildir" öğreticisinde tek tek kullanıcılara bildirim leri iletme hakkında bilgi edinin

Daha fazla bilgi için [PHP Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/php/)de bakın.

[PHP REST sarıcı numunesi]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Başlangıç öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
