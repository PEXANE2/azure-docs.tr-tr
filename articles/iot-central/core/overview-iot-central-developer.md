---
title: Azure IoT Central için cihaz geliştirme | Microsoft Docs
description: Azure IoT Central, IoT çözümlerinin oluşturulmasını kolaylaştıran bir IoT uygulama platformudur. Bu makalede, IoT Central uygulamanıza bağlanmak üzere cihaz geliştirmeye yönelik bir genel bakış sunulmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 05/05/2020
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom:
- mvc
- device-developer
ms.openlocfilehash: 6fabd7d8cf5c19f05bd31c2d0b12863fd6e25382
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90017532"
---
# <a name="iot-central-device-development-overview"></a>IoT Central cihaz geliştirmeye genel bakış

*Bu makale, cihaz geliştiricileri için geçerlidir.*

IoT Central bir uygulama, yaşam döngüsü boyunca milyonlarca cihazı izlemenize ve yönetmenize olanak sağlar. Bu genel bakış, IoT Central bağlanan cihazlarda çalıştırılacak kodu uygulayan cihaz geliştiricilerine yöneliktir.

Cihazlar, aşağıdaki temel elemanlar kullanılarak bir IoT Central uygulamayla etkileşim kurar:

- _Telemetri_ , bir cihazın IoT Central için gönderdiği bir veri. Örneğin, bulunan bir sensörden sıcaklık değerleri akışı.
- _Özellikler_ , bir cihazın IoT Central rapor veren durum değerleridir. Örneğin, cihazın geçerli bellenim sürümü. Ayrıca, IoT Central cihazda güncelleştirebilirler yazılabilir özelliklere de sahip olabilirsiniz.
- Bir cihaz davranışını denetlemek için IoT Central _komutları_ çağrılır. Örneğin, IoT Central uygulamanız bir cihazı yeniden başlatmak için bir komut çağırabilir.

IoT Central Web Kullanıcı arabirimindeki panoları ve görünümleri yapılandırmak, özellikleri yönetmek ve komutları çağırmak için bir çözüm Oluşturucusu sorumludur.

## <a name="types-of-device"></a>Cihaz türleri

Aşağıdaki bölümlerde, bir IoT Central uygulamasına bağlanabildiğinizi ana cihaz türleri açıklanır:

### <a name="standalone-device"></a>Tek başına cihaz

Tek başına bir cihaz doğrudan IoT Central bağlanır. Tek başına bir cihaz genellikle, kendi yerleşik veya bağlı sensörlerinden IoT Central uygulamanıza telemetri gönderir. Tek başına cihazlar ayrıca özellik değerlerini rapor edebilir, yazılabilir özellik değerlerini alabilir ve komutlara yanıt verebilir.

### <a name="gateway-device"></a>Ağ geçidi cihazı

Bir ağ geçidi cihazı, IoT Central uygulamanıza bağlanan bir veya daha fazla aşağı akış aygıtını yönetir. Aşağı akış cihazları ve ağ geçidi cihazı arasındaki ilişkileri yapılandırmak için IoT Central kullanırsınız. Daha fazla bilgi edinmek için bkz. [Azure IoT Central uygulamanızda yeni bir IoT Ağ Geçidi cihaz türü tanımlama](./tutorial-define-gateway-device-type.md).

### <a name="edge-device"></a>Edge cihazı

Sınır cihazı doğrudan IoT Central bağlanır, ancak _yaprak cihazlar_olarak bilinen diğer cihazlar için bir aracı gibi davranır. Bir uç cihaz, genellikle bir aracı olarak hareket eden yaprak cihazlara yakın şekilde bulunur. Edge cihazları kullanan senaryolar şunlardır:

- Edge cihazından bağlanmak üzere IoT Central doğrudan bağlanamadaki cihazları etkinleştirin. Örneğin, bir yaprak cihaz uç cihaza bağlanmak için Bluetooth kullanabilir ve daha sonra IoT Central internet üzerinden bağlanır.
- IoT Central gönderilmeden önce Birleşik telemetri toplayın. Bu yaklaşım, IoT Central veri gönderme maliyetlerini azaltmaya yardımcı olabilir.
- İnternet üzerinden IoT Central bağlanma ile ilişkili gecikme süresini önlemek için yaprak cihazları yerel olarak denetleyin.

Sınır cihazı Ayrıca kendi telemetrisini gönderebilir, özelliklerini raporlar ve yazılabilir Özellik güncelleştirmelerine ve komutlarına yanıt verebilir.

IoT Central, sınır cihazına bağlı yaprak cihazların değil, yalnızca Edge cihazını görür.

Daha fazla bilgi edinmek için bkz. [Azure IoT Central uygulamanıza Azure IoT Edge cihaz ekleme](./tutorial-add-edge-as-leaf-device.md).

## <a name="connect-a-device"></a>Cihaz bağlama

Azure IoT Central, tüm cihaz kayıt ve bağlantılarını yönetmek için [azure IoT Hub cihaz sağlama hizmeti 'ni (DPS)](../../iot-dps/about-iot-dps.md) kullanır.

DPS kullanılması şunları sunar:

- Cihazları ölçeklendirmeye ekleme ve bağlamayı desteklemek için IoT Central.
- Cihazları IoT Central kullanıcı arabirimi aracılığıyla kaydetmeden cihaz kimlik bilgilerini oluşturup cihazları çevrimdışı olarak yapılandırabilirsiniz.
- IoT Central cihazları kaydetmek için kendi cihaz kimliklerinizi kullanmanız gerekir. Kendi cihaz kimliklerinizin kullanılması, mevcut arka ofis sistemleriyle tümleştirmeyi basitleştirir.
- Cihazları IoT Central bağlamak için tek ve tutarlı bir yol.

Daha fazla bilgi edinmek için bkz. [Azure IoT Central 'ye bağlanma](./concepts-get-connected.md).

### <a name="security"></a>Güvenlik

Bir cihaz ile IoT Central uygulamanız arasındaki bağlantı, [paylaşılan erişim imzaları](./concepts-get-connected.md#connect-devices-at-scale-using-sas) ya da sektör standardı [X. 509.440 sertifikaları](./concepts-get-connected.md#connect-devices-using-x509-certificates)kullanılarak güvenlidir.

### <a name="communication-protocols"></a>İletişim protokolleri

Bir cihazın IoT Central bağlanmak için kullanabileceği iletişim protokolleri MQTT, AMQP ve HTTPS içerir. Dahili olarak, IoT Central cihaz bağlantısını etkinleştirmek için bir IoT Hub 'ı kullanır. Cihaz bağlantısı için IoT Hub desteklediği iletişim protokolleri hakkında daha fazla bilgi için bkz. [iletişim protokolü seçme](../../iot-hub/iot-hub-devguide-protocols.md).

## <a name="implement-the-device"></a>Cihazı uygulama

Cihazınızın davranışını uygulamak için [Azure IoT cihaz SDK](#languages-and-sdks) 'larından birini kullanın. Kod şu şekilde olmalıdır:

- Cihazı DPS 'e kaydedin ve IoT Central uygulamanızdaki iç IoT Hub 'ına bağlanmak için DPS içindeki bilgileri kullanın.
- Telemetri IoT Central ' deki cihaz şablonunun belirttiği biçimde gönderin. IoT Central, görselleştirme ve analiz için Telemetriyi nasıl kullanacağınızı öğrenmek için cihaz şablonunu kullanır.
- Cihaz ve IoT Central arasındaki özellik değerlerini eşitler. Cihaz şablonu, IoT Central bilgileri görüntüleyebilmesi için özellik adlarını ve veri türlerini belirtir.
- Komutlar için komut işleyicilerini uygulama cihaz şablonunda belirtir. Cihaz şablonu, cihazın kullanması gereken komut adlarını ve parametreleri belirtir.

Cihaz şablonlarının rolü hakkında daha fazla bilgi için bkz. [cihaz şablonları nelerdir?](./concepts-device-templates.md).

Örnek kod için, bkz. [Node.js istemci uygulaması oluşturma ve bağlama](./tutorial-connect-device-nodejs.md) veya [Python istemci uygulaması oluşturma ve bağlama](./tutorial-connect-device-python.md).

### <a name="languages-and-sdks"></a>Diller ve SDK 'lar

Desteklenen diller ve SDK 'lar hakkında daha fazla bilgi için bkz. [Azure IoT Hub cihaz SDK 'Larını anlama ve kullanma](../../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks).

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisiyseniz ve bazı kodlara gitmek istiyorsanız, önerilen sonraki adım, [bir istemci uygulamasını oluşturmak ve Azure IoT Central uygulamanıza bağlamak](./tutorial-connect-device-nodejs.md)için kullanılır.

IoT Central kullanma hakkında daha fazla bilgi edinmek istiyorsanız, önerilen sonraki adımlar, [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md)ile başlayarak hızlı başlangıç adımlarını denemenize olanak sağlar.
