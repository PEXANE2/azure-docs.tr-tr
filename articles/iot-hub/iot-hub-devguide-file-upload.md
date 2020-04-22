---
title: Azure IoT Hub dosya yüklemeyi anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - Bir aygıttan Azure depolama blob kapsayıcısına dosya yüklemeyi yönetmek için IoT Hub'ın dosya yükleme özelliğini kullanın.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/07/2018
ms.custom: mqtt
ms.openlocfilehash: 35337a99706f25d62964e08a5b16cd8e81f315c6
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730289"
---
# <a name="upload-files-with-iot-hub"></a>IoT Hub ile dosyaları karşıya yükleme

[IoT Hub uç noktaları](iot-hub-devguide-endpoints.md) makalesinde ayrıntılı olarak belirtildiği gibi, bir aygıt aygıta bakan bitiş noktası **(/devices/{deviceId}/files)** üzerinden bildirim göndererek dosya yüklemesini başlatabilir. Bir aygıt IoT Hub'a yüklemenin tamamladığını bildirdiğinde, IoT Hub **/iletiler/servisbağlı/filebildirimler** ihizmete bakan bitiş noktası üzerinden bir dosya yükleme bildirim iletisi gönderir.

IoT Hub üzerinden iletilere aracılık etmek yerine, IoT Hub bunun yerine ilişkili bir Azure Depolama hesabına gönderici olarak hareket eder. Aygıt, IoT Hub'dan aygıtın yüklemek istediği dosyaya özgü bir depolama belirteci ister. Cihaz, dosyayı depolama alanına yüklemek için SAS URI'yi kullanır ve yükleme tamamlandığında cihaz IoT Hub'a bir tamamlanma bildirimi gönderir. IoT Hub, dosya yüklemesinin tamamlanıp tamamlanıp hizmete bakan dosya bildirimi bitiş noktasına dosya yükleme bildirim iletisi ekler.

Bir aygıttan IoT Hub'a dosya yüklemeden önce, [bir Azure Depolama](iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub) hesabıyla ilişkilendirerek hub'ınızı yapılandırmanız gerekir.

Cihazınız daha sonra [bir yüklemenin başlatılmasını](iot-hub-devguide-file-upload.md#initialize-a-file-upload) sağlayabilir ve yükleme tamamlandığında [IoT hub'ına bildirebilir.](iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload) İsteğe bağlı olarak, bir aygıt Yüklemenin tamamladığını IoT Hub'ına bildirdiğinde, hizmet bir [bildirim iletisi](iot-hub-devguide-file-upload.md#file-upload-notifications)oluşturabilir.

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Ortam dosyalarını ve bant genişliğini kaydetmek için aralıklı olarak bağlanan aygıtlar tarafından yüklenen veya sıkıştırılmış büyük telemetri toplu işlerini göndermek için dosya yüklemeyi kullanın.

Bildirilen özellikleri, aygıttan buluta iletileri veya dosya yükleme arasında şüpheniz varsa [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) bakın.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Azure Depolama hesabını IoT Hub ile ilişkilendirme

Dosya yükleme işlevini kullanmak için öncelikle bir Azure Depolama hesabını IoT Hub'ına bağlamanız gerekir. Bu görevi Azure portalı üzerinden veya [IoT Hub kaynak sağlayıcısı REST API'leri](/rest/api/iothub/iothubresource)aracılığıyla programlı olarak tamamlayabilirsiniz. Bir Azure Depolama hesabını IoT Hub'ınızla ilişkilendirdikten sonra, aygıt dosya yükleme isteği başlattığında hizmet bir SAS URI'yi aygıta döndürür.

IoT Hub nasıl yapılacağını niçin kılavuzları [ile cihazınızdan buluta Yükleme dosyaları,](iot-hub-csharp-csharp-file-upload.md) dosya yükleme işleminin tam bir gözden geçirinir. Bu nasıl yapılsA kılavuzları, bir depolama hesabını bir IoT hub'ı ile ilişkilendirmek için Azure portalını nasıl kullanacağınızı gösterir.

> [!NOTE]
> [Azure IoT SDK'ları,](iot-hub-devguide-sdks.md) SAS URI'yi alma, dosyayı yükleme ve tamamlanmış bir yükleme yi IoT Hub'a bildirme işlemlerini otomatik olarak işler.

## <a name="initialize-a-file-upload"></a>Dosya yüklemeyi başlatma
IoT Hub'ın, aygıtların dosya yüklemek için depolama alanı için SAS URI istemesi için özel bir bitiş noktası vardır. Dosya yükleme işlemini başlatmak için, aygıt `{iot hub}.azure-devices.net/devices/{deviceId}/files` aşağıdaki JSON gövdesiile bir POST isteği gönderir:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub, aygıtın dosyayı yüklemek için kullandığı aşağıdaki verileri döndürür:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "yourstorageaccount.blob.core.windows.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Deprecated: GET ile bir dosya yükleme başlatma

> [!NOTE]
> Bu bölümde, IoT Hub'ından bir SAS URI nasıl alınabilmek için amortismana alınan işlevsellik açıklanmaktadır. Daha önce açıklanan POST yöntemini kullanın.

IoT Hub'ın dosya yüklemesini desteklemek için iki REST bitiş noktası vardır, biri depolama için SAS URI'yi almak, diğeri ise tamamlanmış bir yüklemeyi IoT hub'ına bildirmek için. Cihaz, ioT hub'ına get göndererek dosya `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`yükleme işlemini başlatır. IoT hub'ı döndürür:

* Yüklenecek dosyaya özgü bir SAS URI.

* Yükleme tamamlandıktan sonra kullanılacak bir korelasyon kimliği.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Tamamlanmış bir dosya yüklemesini IoT Hub'a bildirme

Aygıt, Dosyayı Azure Depolama SDK'larını kullanarak depolama alanına yükler. Yükleme tamamlandığında, cihaz aşağıdaki JSON gövdesiile `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` bir POST isteği gönderir:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Dosyanın `isSuccess` değeri, dosyanın başarıyla yüklenip yüklenmediğini gösteren bir Boolean'dır. Durum kodu, `statusCode` dosyanın depolama alanına yüklenmesiiçin durumdur `statusDescription` ve `statusCode`dosyanın .

## <a name="reference-topics"></a>Referans konuları:

Aşağıdaki başvuru konuları, bir aygıttan dosya yükleme hakkında daha fazla bilgi sağlar.

## <a name="file-upload-notifications"></a>Dosya yükleme bildirimleri

İsteğe bağlı olarak, bir aygıt IoT Hub'a yüklemenin tamamladığını bildirdiğinde, IoT Hub bir bildirim iletisi oluşturur. Bu ileti, dosyanın adını ve depolama konumunu içerir.

[Endpoints'te](iot-hub-devguide-endpoints.md)açıklandığı gibi, IoT Hub dosya yükleme bildirimlerini hizmete bakan bir uç nokta **(/ileti/servis/servis/fileuploadbildirimleri)** aracılığıyla ileti olarak sunar. Dosya yükleme bildirimleri için alma semantikleri, buluttan aygıta iletilerle aynıdır ve aynı [ileti yaşam döngüsüne](iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-life-cycle)sahiptir. Dosya yükleme bildirimi bitiş noktasından alınan her ileti, aşağıdaki özelliklere sahip bir JSON kaydıdır:

| Özellik | Açıklama |
| --- | --- |
| EnqueuedTimeUtc |Bildirimin ne zaman oluşturulduğunu belirten zaman damgası. |
| DeviceId |**Dosyayı** yükleyen aygıtın DeviceId'i. |
| BlobUri |Yüklenen dosyanın URI. |
| BlobName |Yüklenen dosyanın adı. |
| LastUpdatedTime |Dosyanın en son ne zaman güncelleştirıldığını gösteren zaman damgası. |
| BlobSizeBytes |Yüklenen dosyanın boyutu. |

**Örnek**. Bu örnek, dosya yükleme bildirim iletisinin gövdesini gösterir.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Dosya yükleme bildirimi yapılandırma seçenekleri

Her IoT hub'ı dosya yükleme bildirimleri için aşağıdaki yapılandırma seçeneklerine sahiptir:

| Özellik | Açıklama | Aralık ve varsayılan |
| --- | --- | --- |
| **etkinleştirFileUploadBildirimler** |Dosya yükleme bildirimlerinin dosya bildirimlerinin bitiş noktasına yazıp yazılmadığını denetler. |Bool. Varsayılan: True. |
| **dosyaBildirimler.ttlAsO8601** |Dosya yükleme bildirimleri için varsayılan TTL. |48H'ye (en az 1 dakika) kadar ISO_8601 aralığı. Varsayılan: 1 saat. |
| **fileNotifications.lockDuration** |Dosya yükleme bildirimleri kuyruğuiçin kilit süresi. |5 ila 300 saniye (en az 5 saniye). Varsayılan: 60 saniye. |
| **fileNotifications.maxDeliveryCount** |Dosya yükleme bildirim sırası için maksimum teslimat sayısı. |1'den 100'e kadar. Varsayılan: 100. |

Bu özellikleri Azure portalını, Azure CLI'yi veya PowerShell'i kullanarak IoT hub'ınızda ayarlayabilirsiniz. Nasıl yapılacağını öğrenmek [için, dosya yüklemeyi Yapılandır'ın](iot-hub-configure-file-upload.md)altındaki konulara bakın.

## <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları,](iot-hub-devguide-endpoints.md) çalışma zamanı ve yönetim işlemleri için çeşitli IoT hub uç noktalarını açıklar.

* [Azaltma ve kotalar,](iot-hub-devguide-quotas-throttling.md) IoT Hub hizmetiiçin geçerli olan kotaları ve azaltma davranışlarını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT Hub ile etkileşimde bulunan hem aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [IoT Hub sorgu dili, IoT](iot-hub-devguide-query-language.md) Hub'dan aygıtınız ve işleriniz hakkında bilgi almak için kullanabileceğiniz sorgu dilini açıklar.

* [IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi IoT Hub kullanarak cihazlardan dosya yüklemeyi öğrendiniz, aşağıdaki IoT Hub geliştirici kılavuzu konuları ilginizi çekebilir:

* [IoT Hub'da aygıt kimliklerini yönetme](iot-hub-devguide-identity-registry.md)

* [IoT Hub’a erişimi denetleme](iot-hub-devguide-security.md)

* [Durumu ve yapılandırmaları eşitlemek için aygıt ikizlerini kullanma](iot-hub-devguide-device-twins.md)

* [Aygıtta doğrudan bir yöntem çağırma](iot-hub-devguide-direct-methods.md)

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan bazı kavramları denemek için aşağıdaki IoT Hub öğreticisine bakın:

* [IoT Hub ile cihazlardan buluta dosya yükleme](iot-hub-csharp-csharp-file-upload.md)