---
title: Saydam ağ geçidi aygıtı oluşturma - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge aygıtını, akış aşağı aygıtlardan gelen bilgileri işleyebilen saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6069e0782f69d0dfb73d9be2998cbb11d59d7d22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529178"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma

Bu makalede, bir IoT Edge aygıtının diğer aygıtların IoT Hub ile iletişim kurması için saydam bir ağ geçidi olarak çalışacak şekilde yapılandırılmak için ayrıntılı yönergeler sağlanmaktadır. Bu makalede, saydam ağ geçidi olarak yapılandırılan bir IoT Edge aygıtına başvurmak için *IoT Edge ağ geçidi* terimi kullanır. Daha fazla bilgi için, [IoT Edge aygıtının ağ geçidi olarak nasıl kullanılabileceğini](./iot-edge-as-gateway.md)görün.

>[!NOTE]
>Şu anda:
>
> * Kenar özellikli aygıtlar IoT Edge ağ geçitlerine bağlanamaz.
> * Alt akış aygıtları dosya yüklemeyi kullanamaz.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makale, ilk adımı kapsar:

1. **Ağ geçidi aygıtının akış aşağı aygıtlara güvenli bir şekilde bağlanabilmesi, akış aşağı aygıtlarından iletişim alabilmeleri ve iletileri uygun hedefe yönlendirebilmesi gerekir.**
2. Alt akış aygıtının IoT Hub ile kimlik doğrulaması yapabilmesi ve ağ geçidi aygıtı üzerinden iletişim kurabilmesi için bir aygıt kimliğine sahip olması gerekir. Daha fazla bilgi için [bkz.](how-to-authenticate-downstream-device.md)
3. Akış aşağı aygıtının ağ geçidi aygıtına güvenli bir şekilde bağlanması gerekir. Daha fazla bilgi için [bkz.](how-to-connect-downstream-device.md)

Bir aygıtın ağ geçidi olarak çalışabilmesi için, alt akış aygıtlarına güvenli bir şekilde bağlanabilmesi gerekir. Azure IoT Edge, aygıtlar arasında güvenli bağlantılar kurmak için ortak anahtar altyapısını (PKI) kullanmanıza olanak tanır. Bu durumda, akış aşağı bir aygıtın saydam ağ geçidi görevi yapan bir IoT Edge aygıtına bağlanmasına izin veririz. Makul güvenliği sağlamak için, akış aşağı aygıtıağ aygıtının kimliğini doğrulamalıdır. Bu kimlik denetimi, aygıtlarınızın kötü amaçlı ağ geçitlerine bağlanmasını engeller.

Saydam ağ geçidi senaryosundaki bir akış aşağı aygıtı, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmetiyle oluşturulmuş bir kimliğe sahip herhangi bir uygulama veya platform olabilir. Çoğu durumda, bu uygulamalar [Azure IoT aygıtı SDK'yı](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Tüm pratik amaçlar için, bir downstream cihaz bile IoT Edge ağ geçidi aygıtı kendisi üzerinde çalışan bir uygulama olabilir. Ancak, bir IoT Edge aygıtı bir IoT Edge ağ geçidinin aşağı akışı olamaz.

Aygıt ağ geçidi topolojiniz için gereken güveni sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, IoT Hub'da [X.509 CA güvenliğini](../iot-hub/iot-hub-x509ca-overview.md) etkinleştirmek için kullanacağınız sertifika kurulumunun, belirli bir IoT hub hub'ı (IoT hub kökü CA), bu CA ile imzalanmış bir dizi sertifika ve IoT Edge aygıtı için bir CA içeren Bir X.509 CA sertifikasını içerdiğini varsayıyoruz.

![Ağ geçidi sertifikası kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>Bu makale boyunca kullanılan "kök CA" terimi, PKI sertifika zincirinin en üstteki yetkili kamu sertifikasını ifade eder ve sendikasyon sertifika yetkilisinin sertifika kökünü ifade eder. Çoğu durumda, aslında bir ara CA ortak sertifika.

Ağ geçidi, bağlantının başlatılması sırasında IoT Edge aygıtCA sertifikasını alt aygıta sunar. Alt akış aygıtı, IoT Edge aygıt CA sertifikasının kök CA sertifikası tarafından imzalandığından emin olmak için denetler. Bu işlem, alt aygıt ağ geçidigüvenilir bir kaynaktan geldiğini onaylamak için izin verir.

Aşağıdaki adımlar, sertifikaları oluşturma ve ağ geçidinde doğru yerlere yükleme işleminde size yol açar. Sertifikaları oluşturmak için herhangi bir makineyi kullanabilir ve bunları IoT Edge aygıtınıza kopyalayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Üretim sertifikalarıyla](how-to-manage-device-certificates.md)yapılandırılan bir Azure IoT Edge aygıtı.

## <a name="deploy-edgehub-to-the-gateway"></a>edgeHub'ı ağ geçidine dağıtın

IoT Edge'i bir aygıta ilk yüklediğinizde, yalnızca bir sistem modülü otomatik olarak başlar: IoT Edge aracısı. İlk dağıtımdaha fazla bir aygıt oluşturduğunuzda, ikinci sistem modülü, IoT Edge hub'ı da başlatılır.

IoT Edge hub'ı, akış aşağı aygıtlardan gelen iletileri almaktan ve bunları bir sonraki hedefe yönlendirmekten sorumludur. **edgeHub** modülü cihazınızda çalışmıyorsa, cihazınız için bir başlangıç dağıtımı oluşturun. Herhangi bir modül eklemediğiniz için dağıtım boş görünür, ancak her iki sistem modülünün de çalıştığından emin olur.

Azure portalında aygıt ayrıntılarını kontrol ederek, Visual Studio veya Visual Studio Code'da aygıt durumunu görüntüleyerek veya aygıtın kendi komutunu `iotedge list` çalıştırarak bir cihazda hangi modüllerin çalıştığını kontrol edebilirsiniz.

**edgeAgent** modülü **edgeHub** modülü olmadan çalışıyorsa, aşağıdaki adımları kullanın:

1. Azure portalında IoT Hub'ınıza gidin.

2. **IoT Edge'e** gidin ve ağ geçidi olarak kullanmak istediğiniz IoT Edge aygıtınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **Sonraki'ni**seçin.

5. **Rotaları Belirt** sayfasında, tüm modüllerden tüm iletileri IoT Hub'ına gönderen varsayılan bir rotanız olmalıdır. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```JSON
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

6. Gözden **Geçir şablonu** sayfasında **Gönder'i**seçin.

## <a name="open-ports-on-gateway-device"></a>Ağ geçidi aygıtındaki bağlantı noktalarını açma

Standart IoT Edge aygıtlarının çalışması için herhangi bir gelen bağlantıya gerek yoktur, çünkü IoT Hub ile tüm iletişim giden bağlantılar aracılığıyla yapılır. Ağ geçidi aygıtları, akış aşağı aygıtlarından ileti almaları gerektiğinden farklıdır. Aşağı akım aygıtları ile ağ geçidi aygıtı arasında bir güvenlik duvarı varsa, güvenlik duvarı üzerinden de iletişimin mümkün olması gerekir.

Ağ geçidi senaryosunun çalışması için, IoT Edge hub'ının desteklenen protokollerinden en az birinin akış aşağı aygıtlardan gelen trafik için açık olması gerekir. Desteklenen protokoller MQTT, AMQP, HTTPS, WebSockets üzerinden MQTT ve WebSockets üzerinden AMQP vardır.

| Bağlantı noktası | Protokol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP+WS |

## <a name="route-messages-from-downstream-devices"></a>İletileri akış aşağı aygıtlardan yönlendirme

IoT Edge çalışma süresi, modüller tarafından gönderilen iletiler gibi akış aşağı aygıtlarından gönderilen iletileri yönlendirebilir. Bu özellik, buluta herhangi bir veri göndermeden önce ağ geçidinde çalışan bir modülde analitik gerçekleştirmenize olanak tanır.

Şu anda, akış aşağı aygıtlar tarafından gönderilen iletileri yönlendirme şekliniz, bunları modüller tarafından gönderilen iletilerden ayırt etmektir. Modüller tarafından gönderilen iletilerin tümü **connectionModuleId** adı verilen bir sistem özelliği içerir, ancak akış aşağı aygıtları tarafından gönderilen iletiler içermez. Bu sistem özelliğini içeren iletileri hariç tutmak için rotanın WHERE yan tümcesini kullanabilirsiniz.

Aşağıdaki rota, herhangi bir akış aşağı aygıtından, daha sonra `ai_insights`IoT `ai_insights` Hub adlı bir modüle ileti gönderen bir örnektir.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")",
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream"
    }
}
```

İleti yönlendirme siması hakkında daha fazla bilgi için [modülleri dağıt'a bakın ve rotalar kurun.](./module-composition.md#declare-routes)

## <a name="enable-extended-offline-operation"></a>Genişletilmiş çevrimdışı işlemi etkinleştirme

IoT Edge çalışma zamanının [v1.0.4 sürümünden](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) başlayarak, ağ geçidi aygıtı ve ona bağlanan akış aşağı aygıtları genişletilmiş çevrimdışı çalışma için yapılandırılabilir.

Bu özellik sayesinde, yerel modüller veya akış aşağı aygıtları gerektiğinde IoT Edge aygıtıyla yeniden kimlik doğrulayabilir ve IoT hub'ından bağlantısı kesilse bile iletiler ve yöntemler kullanarak birbirleriyle iletişim kurabilir. Daha fazla bilgi için bkz. [IoT Edge aygıtları, modülleri ve alt aygıtları için genişletilmiş çevrimdışı özellikleri anlayın.](offline-capabilities.md)

Genişletilmiş çevrimdışı özellikleri etkinleştirmek için, bir IoT Edge ağ geçidi aygıtı ile ona bağlanacak alt akış aygıtları arasında bir üst-alt ilişkisi kurarsınız. Bu adımlar, [bir alt aygıtı Azure IoT Hub'ına doğrulamada](how-to-authenticate-downstream-device.md)daha ayrıntılı olarak açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Artık saydam bir ağ geçidi olarak çalışan bir IoT Edge aygıtınız olduğuna göre, alt akış aygıtlarınızı ağ geçidine güvenecek ve ona ileti gönderecek şekilde yapılandırmanız gerekir. Saydam ağ geçidi senaryonuzu ayarlamaya yönelik sonraki adımlar için [bir alt aygıtla Azure IoT Hub'ına kimlik doğrulamaya](how-to-authenticate-downstream-device.md) devam edin.
