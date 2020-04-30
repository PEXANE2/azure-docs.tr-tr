---
title: Saydam ağ geçidi cihazı oluşturma-Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge cihazı, aşağı akış cihazlarından bilgileri işleyebileceği saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/03/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: e563e67b5e951b43e5782f8c845c8ec46ff3e9bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687156"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma

Bu makalede, bir IoT Edge cihazının diğer cihazların IoT Hub iletişim kurması için saydam bir ağ geçidi olarak çalışacak şekilde yapılandırılması için ayrıntılı yönergeler sağlanmaktadır. Bu makalede, saydam bir ağ geçidi olarak yapılandırılmış bir IoT Edge cihazına başvurmak için *IoT Edge ağ geçidi* terimi kullanılmaktadır. Daha fazla bilgi için bkz. [bir IoT Edge cihazının ağ geçidi olarak nasıl kullanılabileceği](./iot-edge-as-gateway.md).

>[!NOTE]
>Seçili
>
> * Kenar özellikli cihazlar IoT Edge ağ geçitlerine bağlanamaz.
> * Aşağı akış cihazları karşıya dosya yükleme kullanamaz.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede ilk adım ele alınmaktadır:

1. **Ağ Geçidi cihazının aşağı akış cihazlarına güvenli bir şekilde bağlanabilmesi, aşağı akış cihazlarından iletişim alabilmesi ve iletileri uygun hedefe yönlendirmesi gerekir.**
2. Aşağı akış cihazının IoT Hub kimlik doğrulaması yapabilmesi ve ağ geçidi cihazından iletişim kurmayı bilmesi için bir cihaz kimliği olması gerekir. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).
3. Aşağı akış cihazının ağ geçidi cihazına güvenli bir şekilde bağlanması gerekir. Daha fazla bilgi için bkz. bir [aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlama](how-to-connect-downstream-device.md).

Bir cihazın ağ geçidi olarak çalışması için, onun aşağı akış cihazlarına güvenli bir şekilde bağlanabilmesi gerekir. Azure IoT Edge, cihazlar arasında güvenli bağlantı kurmak için ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, bir aşağı akış cihazının saydam bir ağ geçidi görevi gören bir IoT Edge cihazına bağlanmasına izin veriyoruz. Makul güvenliği korumak için, aşağı akış cihazının ağ geçidi cihazının kimliğini onaylamasını gerekir. Bu kimlik denetimi, cihazlarınızın potansiyel olarak kötü amaçlı ağ geçitlerine bağlanmasını engeller.

Saydam bir ağ geçidi senaryosunda bir aşağı akış cihazı, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti ile oluşturulmuş bir kimliği olan herhangi bir uygulama veya platform olabilir. Çoğu durumda bu uygulamalar [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Tüm pratik amaçlar için, bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. Ancak, bir IoT Edge cihaz IoT Edge bir ağ geçidinin bir aşağı akış olamaz.

Cihaz ağ geçidi topolojiniz için gereken güveni sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, belirli bir IoT Hub (IoT Hub 'ı kök CA) ile ilişkili bir X. 509.952 CA sertifikasını, bu CA ile imzalanmış bir sertifika serisini ve IoT Edge cihazının CA 'sını içeren IoT Hub [x. 509.440 CA güvenliğini](../iot-hub/iot-hub-x509ca-overview.md) etkinleştirmek için kullandığınız sertifika kurulumunu kabul ediyoruz.

![Ağ Geçidi sertifikası kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, bir dağıtılmış sertifika yetkilisinin sertifika kökü olması gerekmeyen PKI sertifika zincirinin en üst yetkili ortak sertifikasına başvurur. Çoğu durumda, aslında bir ara CA genel sertifikasıdır.

IoT Edge güvenlik arka plan programı, bir iş yükü CA sertifikasını imzalamak için IoT Edge cihaz CA sertifikasını kullanır, bu da IoT Edge hub 'ı için bir sunucu sertifikasını imzalar. Ağ Geçidi, bağlantının başlatılması sırasında sunucu sertifikasını aşağı akış cihazına sunar. Aşağı akış cihazı, sunucu sertifikasının kök CA sertifikasına toplanan bir Sertifika zincirinin parçası olduğundan emin olmak için kontrol eder. Bu işlem, aşağı akış cihazının ağ geçidinin güvenilir bir kaynaktan geldiğini onaylamasını sağlar. Daha fazla bilgi için bkz. [Azure IoT Edge sertifikaları nasıl kullandığını anlama](iot-edge-certs.md).

Aşağıdaki adımlar, sertifikaları oluşturma ve bunları ağ geçidine doğru yerlere yükleme sürecinde size yol gösterir. Sertifikaları oluşturmak için herhangi bir makine kullanabilir ve sonra bunları IoT Edge cihazınıza kopyalayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Üretim sertifikaları](how-to-manage-device-certificates.md)ile yapılandırılan Azure IoT Edge bir cihaz.

## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub 'ı ağ geçidine dağıtma

IoT Edge bir cihaza ilk kez yüklediğinizde, otomatik olarak yalnızca bir sistem modülü başlatılır: IoT Edge Aracısı. İlk dağıtımı daha fazla bir cihaz oluşturduktan sonra, IoT Edge hub 'ı ikinci sistem modülü de başlatılır.

IoT Edge hub, aşağı akış cihazlarından gelen iletileri alırken ve bunları bir sonraki hedefe yönlendirmekten sorumludur. **Edgehub** modülü cihazınızda çalışmıyorsa, cihazınız için bir başlangıç dağıtımı oluşturun. Herhangi bir modül eklemediğinizden, dağıtım boş görünür, ancak her iki sistem modüllerinin de çalıştığından emin olur.

Azure portal cihaz ayrıntılarını denetleyerek, Visual Studio veya Visual Studio Code cihaz durumunu görüntüleyerek ya da cihazın kendisinde komutunu `iotedge list` çalıştırarak bir cihazda hangi modüllerin çalıştığını kontrol edebilirsiniz.

**Edgeagent** modülü **edgehub** modülü olmadan çalışıyorsa, aşağıdaki adımları kullanın:

1. Azure portalında IoT Hub'ınıza gidin.

2. **IoT Edge** gidin ve ağ geçidi olarak kullanmak istediğiniz IoT Edge cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **İleri**’yi seçin.

5. **Yolları belirtin** sayfasında, tüm modüllerden IoT Hub tüm iletileri gönderen bir varsayılan yolunuz olması gerekir. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```JSON
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

6. **Şablonu gözden geçir** sayfasında **Gönder**' i seçin.

## <a name="open-ports-on-gateway-device"></a>Ağ Geçidi cihazında bağlantı noktalarını açma

Standart IoT Edge cihazların herhangi bir gelen bağlantısı olması gerekmez, çünkü IoT Hub tüm iletişimler giden bağlantılar aracılığıyla yapılır. Ağ Geçidi cihazları, akış aygıtlarından iletiler alması gerektiğinden farklıdır. Bir güvenlik duvarının aşağı akış cihazları ve ağ geçidi cihazı arasında olması durumunda, iletişimin de güvenlik duvarından alınması gerekir.

Bir ağ geçidi senaryosunun çalışması için, IoT Edge hub 'ın desteklenen protokollerinden en az birinin, aşağı akış cihazlarından gelen trafik için açık olması gerekir. Desteklenen protokoller MQTT, AMQP, HTTPS, WebSockets üzerinden MQTT ve WebSockets üzerinden AMQP 'dir.

| Bağlantı noktası | Protokol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS |

## <a name="route-messages-from-downstream-devices"></a>İletileri aşağı akış cihazlarından yönlendir

IoT Edge çalışma zamanı, yukarı akış aygıtlarından gönderilen iletileri, tıpkı modüller tarafından gönderilen iletiler gibi yönlendirebilir. Bu özellik, buluta veri göndermeden önce ağ geçidinde çalışan bir modülde analiz gerçekleştirmenize olanak tanır.

Şu anda, aşağı akış cihazları tarafından gönderilen iletileri yönlendirmenin yolu, modülleri tarafından gönderilen iletilerden farklılaştırılacağından yapılır. Modüller tarafından gönderilen iletiler, **Connectionmoduleıd** adlı bir sistem özelliği içerir, ancak aşağı akış cihazları tarafından gönderilen iletiler değildir. Bu sistem özelliğini içeren tüm iletileri dışlamak için yolun WHERE yan tümcesini kullanabilirsiniz.

Aşağıdaki yol, herhangi bir aşağı akış aygıtından, adlı `ai_insights`bir modüle ve sonra IoT Hub ' `ai_insights` ye ileti gönderen bir örnektir.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")",
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream"
    }
}
```

İleti yönlendirme hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yolları oluşturma](./module-composition.md#declare-routes).

## <a name="enable-extended-offline-operation"></a>Genişletilmiş çevrimdışı işlemi etkinleştir

IoT Edge çalışma zamanının [v 1.0.4 sürümü](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) ile başlayarak, buna bağlanan ağ geçidi cihazı ve aşağı akış cihazları genişletilmiş çevrimdışı işlem için yapılandırılabilir.

Bu özellik sayesinde, yerel modüller veya aşağı akış cihazları gerektiği gibi IoT Edge cihazla yeniden kimlik doğrulaması yapabilir ve IoT Hub 'ından bağlantısı kesilse bile ileti ve yöntemleri kullanarak birbirleriyle iletişim kurabilir. Daha fazla bilgi için bkz. [IoT Edge cihazları, modülleri ve alt cihazları için genişletilmiş çevrimdışı özellikleri anlama](offline-capabilities.md).

Genişletilmiş çevrimdışı özellikleri etkinleştirmek için, bir IoT Edge ağ geçidi cihazı ile Bağlanılacak olan aşağı akış cihazları arasında bir üst-alt ilişkisi kurarsınız. Bu adımlar, [bir aşağı akış cihazının Azure IoT Hub kimlik doğrulaması](how-to-authenticate-downstream-device.md)konusunda daha ayrıntılı olarak açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Artık saydam bir ağ geçidi olarak çalışan bir IoT Edge cihazınız olduğuna göre, aşağı akış cihazlarınızı ağ geçidine güvenecek ve ona ileti gönderecek şekilde yapılandırmanız gerekir. Saydam ağ geçidi senaryonuzu ayarlamanın sonraki adımları için [bir aşağı akış cihazının kimliğini Azure IoT Hub doğrulamak](how-to-authenticate-downstream-device.md) için devam edin.
