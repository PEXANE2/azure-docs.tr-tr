---
title: Saydam bir ağ geçidi cihazı - Azure IOT Edge oluştur | Microsoft Docs
description: Azure IOT Edge cihazı, aşağı akış cihazlardan bilgi işleyebilen saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1deeb17e4d55c81d6161855ee2e6dc4766bdcdca
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772474"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma

Bu makalede, bir IoT Edge cihazının diğer cihazların IoT Hub iletişim kurması için saydam bir ağ geçidi olarak çalışacak şekilde yapılandırılması için ayrıntılı yönergeler sağlanmaktadır. Bu makalede, saydam bir ağ geçidi olarak yapılandırılmış bir IoT Edge cihazına başvurmak için *IoT Edge ağ geçidi* terimi kullanılmaktadır. Daha fazla bilgi için bkz. [bir IoT Edge cihazının ağ geçidi olarak nasıl kullanılabileceği](./iot-edge-as-gateway.md).

>[!NOTE]
>Şu anda:
> * Edge özellikli cihazlar IOT Edge ağ geçidi için bağlantı kurulamıyor. 
> * Aşağı Akış cihazları karşıya dosya yükleme kullanamazsınız.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede ilk adım ele alınmaktadır:

1. **Ağ Geçidi cihazının aşağı akış cihazlarına güvenli bir şekilde bağlanabilmesi, aşağı akış cihazlarından iletişim alabilmesi ve iletileri uygun hedefe yönlendirmesi gerekir.**
2. Aşağı akış cihazının IoT Hub kimlik doğrulaması yapabilmesi ve ağ geçidi cihazından iletişim kurmayı bilmesi için bir cihaz kimliği olması gerekir. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).
3. Aşağı akış cihazının ağ geçidi cihazına güvenli bir şekilde bağlanması gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).


Bir cihazın ağ geçidi olarak çalışması için, onun aşağı akış cihazlarına güvenli bir şekilde bağlanabilmesi gerekir. Azure IOT Edge cihazları arasında güvenli bağlantılar kurmak için bir ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, biz saydam bir ağ geçidi olarak görev yapan bir IOT Edge cihazına bağlamak için bir aşağı akış cihazı vermiş olursunuz. Makul güvenliği korumak için, aşağı akış cihazının ağ geçidi cihazının kimliğini onaylamasını gerekir. Bu kimlik denetimi, cihazlarınızın potansiyel olarak kötü amaçlı ağ geçitlerine bağlanmasını engeller.

Saydam bir ağ geçidi senaryosunda bir aşağı akış cihazı, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti ile oluşturulmuş bir kimliği olan herhangi bir uygulama veya platform olabilir. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Tüm pratik amacıyla bir aşağı akış cihazın IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama bile olabilir. Ancak, bir IoT Edge cihaz IoT Edge bir ağ geçidinin bir aşağı akış olamaz. 

Ağ geçidi cihazı topolojiniz için gerekli güven sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, belirli bir IoT Hub (IoT Hub 'ı kök CA) ile ilişkili bir X. 509.952 CA sertifikasını, bu CA ile imzalanmış bir sertifika serisini ve IoT Edge cihazının CA 'sını içeren IoT Hub [x. 509.440 CA güvenliğini](../iot-hub/iot-hub-x509ca-overview.md) etkinleştirmek için kullandığınız sertifika kurulumunu kabul ediyoruz.

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, bir dağıtılmış sertifika yetkilisinin sertifika kökü olması gerekmeyen PKI sertifika zincirinin en üst yetkili ortak sertifikasına başvurur. Çoğu durumda, aslında bir ara CA genel sertifikasıdır. 

Ağ Geçidi, bağlantının başlatılması sırasında IoT Edge cihaz CA sertifikasını aşağı akış cihazına sunar. Aşağı akış cihazı, IoT Edge cihaz CA sertifikasının kök CA sertifikası tarafından imzalandığından emin olmak için kontrol eder. Bu işlem, aşağı akış cihazının ağ geçidinin güvenilir bir kaynaktan geldiğini onaylamasını sağlar.

Aşağıdaki adımlar, sertifikaları oluşturma ve bunları ağ geçidine doğru yerlere yükleme sürecinde size yol gösterir. Herhangi bir makine sertifikalarını oluşturmak için kullanın ve ardından bunları IOT Edge cihazınıza kopyalayabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

[Üretim sertifikaları](how-to-install-production-certificates.md)ile yapılandırılan Azure IoT Edge bir cihaz.

## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub 'ı ağ geçidine dağıtma

IoT Edge bir cihaza ilk kez yüklediğinizde, otomatik olarak yalnızca bir sistem modülü başlatılır: IoT Edge Aracısı. İlk dağıtımı daha fazla bir cihaz oluşturduktan sonra, IoT Edge hub 'ı ikinci sistem modülü de başlatılır. 

IoT Edge hub, aşağı akış cihazlarından gelen iletileri alırken ve bunları bir sonraki hedefe yönlendirmekten sorumludur. **Edgehub** modülü cihazınızda çalışmıyorsa, cihazınız için bir başlangıç dağıtımı oluşturun. Herhangi bir modül eklemediğinizden, dağıtım boş görünür, ancak her iki sistem modüllerinin de çalıştığından emin olur. 

Azure portal cihaz ayrıntılarını denetleyerek, Visual Studio veya Visual Studio Code cihaz durumunu görüntüleyerek ya da cihazın kendisinde komut `iotedge list` çalıştırarak bir cihazda hangi modüllerin çalıştığını kontrol edebilirsiniz. 

**Edgeagent** modülü **edgehub** modülü olmadan çalışıyorsa, aşağıdaki adımları kullanın:

1. Azure portalında IoT Hub'ınıza gidin.

2. Git **IOT Edge** ve ağ geçidi olarak kullanacağınız IOT Edge Cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **İleri**’yi seçin.

5. İçinde **yolları belirtin** sayfasında, tüm iletileri tüm modüllerden IOT Hub'ına gönderir ve varsayılan bir yol olmalıdır. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```JSON
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

6. İçinde **gözden geçirme şablonu** sayfasında **Gönder**.

## <a name="open-ports-on-gateway-device"></a>Ağ Geçidi cihazında bağlantı noktalarını açma

Standart IoT Edge cihazların herhangi bir gelen bağlantısı olması gerekmez, çünkü IoT Hub tüm iletişimler giden bağlantılar aracılığıyla yapılır. Ağ Geçidi cihazları, akış aygıtlarından iletiler alması gerektiğinden farklıdır. Bir güvenlik duvarının aşağı akış cihazları ve ağ geçidi cihazı arasında olması durumunda, iletişimin de güvenlik duvarından alınması gerekir.

Bir ağ geçidi senaryosunun çalışması için, IoT Edge hub 'ın desteklenen protokollerinden en az birinin, aşağı akış cihazlarından gelen trafik için açık olması gerekir. Desteklenen protokoller MQTT, AMQP, HTTPS, WebSockets üzerinden MQTT ve WebSockets üzerinden AMQP 'dir. 

| Bağlantı noktası | Protokol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS | 

## <a name="route-messages-from-downstream-devices"></a>Aşağı Akış cihazlardan yönlendirme iletileri
IOT Edge çalışma zamanı yalnızca modülleri tarafından gönderilen iletiler gibi aşağı akış cihazlardan gönderilen iletiler yönlendirebilirsiniz. Bu özellik, buluta veri göndermeden önce ağ geçidinde çalışan bir modülde analiz gerçekleştirmenize olanak tanır. 

Şu anda, aşağı akış cihazlar tarafından gönderilen iletileri yönlendiren bunları modülleri tarafından gönderilen iletilerden farklılaştırılması tarafından yoludur. Tüm modüller tarafından gönderilen iletiler olarak adlandırılan bir sistem özelliği içeren **connectionModuleId** ancak aşağı akış cihazlar tarafından gönderilen iletileri yapın. Bu sistem özelliği içeren herhangi bir iletiyi hariç tutmak için rota WHERE yan tümcesini kullanabilirsiniz. 

Aşağıdaki yol, herhangi bir aşağı akış cihazdan `ai_insights`adlı bir modüle ileti gönderen ve sonra `ai_insights` IoT Hub ' e kadar olan bir örnektir.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

İleti yönlendirme hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar'kurmak](./module-composition.md#declare-routes).


## <a name="enable-extended-offline-operation"></a>Genişletilmiş çevrimdışı işlemi etkinleştir

IoT Edge çalışma zamanının [v 1.0.4 sürümü](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) ile başlayarak, buna bağlanan ağ geçidi cihazı ve aşağı akış cihazları genişletilmiş çevrimdışı işlem için yapılandırılabilir. 

Bu özellik sayesinde, yerel modüller veya aşağı akış cihazları gerektiği gibi IoT Edge cihazla yeniden kimlik doğrulaması yapabilir ve IoT Hub 'ından bağlantısı kesilse bile ileti ve yöntemleri kullanarak birbirleriyle iletişim kurabilir. Daha fazla bilgi için [anlayın cihazları, modülleri ve alt cihazlar bu çevrimdışı özellikleri IOT Edge için Genişletilmiş](offline-capabilities.md).

Genişletilmiş çevrimdışı özellikleri etkinleştirmek için, bir IoT Edge ağ geçidi cihazı ile Bağlanılacak olan aşağı akış cihazları arasında bir üst-alt ilişkisi kurarsınız. Bu adımlar, [bir aşağı akış cihazının Azure IoT Hub kimlik doğrulaması](how-to-authenticate-downstream-device.md)konusunda daha ayrıntılı olarak açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi olarak çalışan bir IOT Edge cihazına sahip olduğunuza göre ağ geçidi güven ve iletileri göndermek için aşağı akış cihazlarınızı yapılandırmak gerekir. Saydam ağ geçidi senaryonuzu ayarlamanın sonraki adımları için [bir aşağı akış cihazının kimliğini Azure IoT Hub doğrulamak](how-to-authenticate-downstream-device.md) için devam edin. 
