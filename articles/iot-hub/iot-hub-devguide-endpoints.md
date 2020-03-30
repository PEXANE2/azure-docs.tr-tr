---
title: Azure IoT Hub uç noktalarını anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub aygıta bakan ve hizmete bakan uç noktalar hakkında başvuru bilgileri.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: da6d17e42407048b7ecbcacade67ef48046d7fe1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284648"
---
# <a name="reference---iot-hub-endpoints"></a>Referans - IoT Hub uç noktaları

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>IoT Hub adları

Uç noktalarınızı barındıran IoT hub'ının ana bilgisayar adını hub'ınızın **Genel Bakış** sayfasında portalda bulabilirsiniz. Varsayılan olarak, bir IoT hub'ının DNS adı aşağıdaki gibi görünür: `{your iot hub name}.azure-devices.net`.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Yerleşik IoT Hub uç noktaları listesi

Azure IoT Hub, işlevselliğini çeşitli aktörlere açık bir çok kiracılı hizmettir. Aşağıdaki diyagram, IoT Hub'ın gösterdiği çeşitli uç noktaları gösterir.

![IoT Hub uç noktaları](./media/iot-hub-devguide-endpoints/endpoints.png)

Aşağıdaki liste uç noktaları açıklar:

* **Kaynak sağlayıcısı.** IoT Hub kaynak sağlayıcısı bir [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) arabirimini ortaya çıkarır. Bu arabirim, Azure abonelik sahiplerinin IoT hub'ları oluşturmasına ve silebilmesini ve IoT hub özelliklerini güncelleştirmesini sağlar. IoT Hub özellikleri, aygıt düzeyinde erişim denetiminin aksine [hub düzeyinde güvenlik ilkelerini](iot-hub-devguide-security.md#access-control-and-permissions)ve bulut-aygıtve aygıt-bulut iletisi için işlevsel seçenekleri yönetir. IoT Hub kaynak sağlayıcısı ayrıca [aygıt kimliklerini dışa aktarmanızı](iot-hub-devguide-identity-registry.md#import-and-export-device-identities)da sağlar.

* **Cihaz kimlik yönetimi**. Her IoT hub'ı, aygıt kimliklerini yönetmek (oluşturma, alma, güncelleme ve silme) için bir dizi HTTPS REST uç noktasını ortaya çıkarır. [Aygıt kimlikleri](iot-hub-devguide-identity-registry.md) aygıt kimlik doğrulaması ve erişim denetimi için kullanılır.

* **Cihaz ikiz yönetimi**. Her IoT hub'ı, [aygıt ikizlerini](iot-hub-devguide-device-twins.md) sorgulamak ve güncelleştirmek için hizmete bakan BIR dizi HTTPS REST bitiş noktasını (güncelleştirme etiketleri ve özellikleri) ortaya çıkarır.

* **İş yönetimi.** Her IoT hub'ı, [işleri](iot-hub-devguide-jobs.md)sorgulamak ve yönetmek için hizmete bakan bir dizi HTTPS REST bitiş noktasını ortaya çıkarır.

* **Aygıt uç noktaları**. IoT Hub, kimlik kayıt defterindeki her bir cihaz için belirli uç noktaları kullanıma sunar:

  * *Aygıttan buluta iletiler gönderin.* Aygıttan [buluta iletigöndermek](iot-hub-devguide-messages-d2c.md)için bu bitiş noktasını kullanır.

  * *Buluttan cihaza iletiler alın.* Bir aygıt hedeflenen [buluttan aygıta iletileri](iot-hub-devguide-messages-c2d.md)almak için bu bitiş noktasını kullanır.

  * *Dosya yüklemelerini başlatın.* Bir aygıt, [bir dosyayı yüklemek](iot-hub-devguide-file-upload.md)için IoT Hub'ından bir Azure Depolama SAS URI almak için bu bitiş noktasını kullanır.

  * *Aygıt ikiz özelliklerini alın ve güncelleyin.* Bir aygıt, [aygıtının ikiz](iot-hub-devguide-device-twins.md)özelliklerine erişmek için bu bitiş noktasını kullanır.

  * *Doğrudan yöntem isteklerini alın.* Bir aygıt [doğrudan yöntemin](iot-hub-devguide-direct-methods.md)isteklerini dinlemek için bu bitiş noktasını kullanır.

    Bu uç noktalar [MQTT v3.1.1](https://mqtt.org/), HTTPS 1.1 ve [AMQP 1.0](https://www.amqp.org/) protokolleri kullanılarak açıklanır. AMQP, 443 bağlantı [noktasındaki WebSockets](https://tools.ietf.org/html/rfc6455) üzerinden de kullanılabilir.

* **Hizmet bitiş noktaları.** Her IoT hub'ı, aygıtlarınizle iletişim kurmak için çözümünüz için bir dizi uç noktayı ortaya çıkarır. Bir istisna dışında, bu uç noktalar yalnızca [AMQP](https://www.amqp.org/) protokolü kullanılarak açıklanır. Yöntem çağırma bitiş noktası HTTPS protokolü üzerinde ortaya çıkarır.
  
  * *Aygıttan buluta iletileri alın.* Bu bitiş noktası [Azure Etkinlik Hub'ları](https://azure.microsoft.com/documentation/services/event-hubs/)ile uyumludur. Arka uç hizmeti, aygıtlarınız tarafından gönderilen [aygıttan buluta iletileri](iot-hub-devguide-messages-d2c.md) okumak için kullanabilir. Bu yerleşik bitiş noktasına ek olarak IoT hub'ınızda özel uç noktalar oluşturabilirsiniz.
  
  * *Buluttan cihaza iletiler gönderin ve teslim bildirimleri alın.* Bu uç noktalar, çözümünüzün arka uçtan güvenilir [buluttan cihaza iletiler](iot-hub-devguide-messages-c2d.md)göndermesini ve ilgili teslim veya son kullanma bildirimlerini almasını sağlar.
  
  * *Dosya bildirimleri alın.* Bu ileti bitiş noktası, aygıtlarınızın bir dosyayı başarıyla yüklediklerine ait bildirimleri almanızı sağlar. 
  
  * *Doğrudan yöntem çağırma*. Bu uç nokta, bir arka uç hizmetinin aygıtta doğrudan bir [yöntem](iot-hub-devguide-direct-methods.md) çağırmasına olanak tanır.
  
  * *Olayları izleme işlemleri alın.* Bu bitiş noktası, IoT hub'ınız bunları yayacak şekilde yapılandırıldıysa, operasyon izleme olaylarını almanızı sağlar. Daha fazla bilgi için [IoT Hub işlemleri izleme](iot-hub-operations-monitoring.md)ye bakın.

[Azure IoT SDKs](iot-hub-devguide-sdks.md) makalesi, bu uç noktalara erişmenin çeşitli yollarını açıklar.

Tüm IoT Hub uç noktaları [TLS](https://tools.ietf.org/html/rfc5246) protokolünü kullanır ve şifrelenmemiş/güvenli olmayan kanallarda hiçbir bitiş noktası açıklanır.

## <a name="custom-endpoints"></a>Özel uç noktalar

Aboneliğinizdeki mevcut Azure hizmetlerini IoT hub'ınıza bağlayarak ileti yönlendirmeiçin uç nokta olarak hareket edebilirsiniz. Bu uç noktalar hizmet bitiş noktaları olarak hareket eder ve ileti yolları için lavabo olarak kullanılır. Aygıtlar doğrudan ek uç noktalara yazılamaz. [İleti yönlendirme](../iot-hub/iot-hub-devguide-messages-d2c.md)hakkında daha fazla bilgi edinin.

IoT Hub şu anda ek uç nokta olarak aşağıdaki Azure hizmetlerini destekler:

* Azure Depolama kapsayıcıları
* Event Hubs
* Service Bus Kuyrukları
* Service Bus Konuları

Ekleyebileceğiniz uç nokta sayısının sınırları için [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)bölümüne bakın.

Endpoints sağlık durumunu almak için REST API [Get Endpoint Health'i](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) kullanabilirsiniz. Uç nokta sağlık ölü veya sağlıksız olduğunda hataları tanımlamak ve hata ayıklamak için yönlendirme ileti gecikmesi ile ilgili [IoT Hub ölçümlerini](iot-hub-metrics.md) kullanmanızı öneririz, çünkü bitiş noktası bu durumlardan birindeyken gecikmenin daha yüksek olmasını bekleriz.

|Sistem Durumu|Açıklama|
|---|---|
|Sağlıklı|Bitiş noktası beklendiği gibi iletileri kabul etmektir.|
|Sağlıksız|Bitiş noktası beklendiği gibi iletileri kabul etmiyor ve IoT Hub bu bitiş noktasına veri göndermeye çalışıyor. IoT Hub sonunda tutarlı bir sağlık durumu oluşturduğunda sağlıksız bir bitiş noktasının durumu sağlıklı olarak güncelleştirilir.|
|Bilinmeyen|IoT Hub bitiş noktasıyla bağlantı kurmadı. Bu son noktadan itibaren hiçbir ileti teslim edilmedi veya reddedilmedi.|
|Ölü|Bitiş noktası, IoT Hub yeniden deneme dönemi için ileti göndermeyi yeniden denedikten sonra iletileri kabul etmemektir.|

## <a name="field-gateways"></a>Alan ağ geçitleri

Bir IoT çözümünde, aygıtlarınız ile IoT Hub uç noktalarınız arasında bir *alan ağ geçidi* bulunur. Genellikle aygıtlarınıza yakın bulunur. Aygıtlarınız, aygıtlar tarafından desteklenen bir protokol kullanarak alan ağ geçidiyle doğrudan iletişim kurar. Alan ağ geçidi, IoT Hub tarafından desteklenen bir iletişim kuralı nı kullanarak bir IoT Hub bitiş noktasına bağlanır. Alan ağ geçidi özel bir donanım aygıtı veya özel ağ geçidi yazılımı çalıştıran düşük güçlü bir bilgisayar olabilir.

Bir alan ağ geçidi uygulamak için [Azure IoT Edge'i](/azure/iot-edge/) kullanabilirsiniz. IoT Edge, birden çok cihazdan aynı IoT Hub bağlantısına birden fazla iletişim gibi işlevler sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dili](iot-hub-devguide-query-language.md)
* [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT desteği](iot-hub-mqtt-support.md)
* [IoT hub IP adresinizi anlama](iot-hub-understand-ip-address.md)