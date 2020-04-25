---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 11/09/2018
ms.author: dobett
ms.openlocfilehash: 30b8facfef6d90a444bd61d0ce041ed7dfef324e
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131726"
---
## <a name="automatic-device-management"></a>Otomatik cihaz yönetimi
Azure IoT Hub otomatik cihaz yönetimi, büyük cihaz filliklerini tümüyle yaşam döngülerine göre yönetmenin yinelenen ve karmaşık görevlerinin çoğunu otomatikleştirir. Otomatik cihaz yönetimi sayesinde, özelliklerini temel alarak bir cihaz kümesini hedefleyebilir, istenen yapılandırmayı tanımlayabilir ve IoT Hub cihazları kapsama geldiğinde güncelleştirme sağlayabilirsiniz.  [Otomatik cihaz yapılandırmalarının](../articles/iot-hub/iot-hub-auto-device-config.md) ve [IoT Edge otomatik dağıtımlarından](../articles/iot-edge/how-to-deploy-at-scale.md)oluşur.

## <a name="iot-edge"></a>IoT Edge
Azure IoT Edge, şirket içi cihazlara Azure hizmetlerinin ve çözüme özgü kodun bulut odaklı dağıtımına olanak sağlar. IoT Edge cihazlar, veriler buluta gönderilmeden önce bilgi işlem ve analiz işlemleri gerçekleştirmek için diğer cihazlardan verileri toplayabilir. Daha fazla bilgi için bkz. [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).

## <a name="iot-edge-agent"></a>IoT Edge Aracısı
IoT Edge çalışma zamanının, modülleri dağıtmaktan ve izlemekten sorumlu olan bölümü.

## <a name="iot-edge-device"></a>IoT Edge cihazı
IoT Edge cihazlarda IoT Edge çalışma zamanı yüklüdür ve cihaz ayrıntılarında **IoT Edge cihaz** olarak işaretlenir. [Linux önizlemesinde sanal bir cihazda Azure IoT Edge dağıtmayı](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)öğrenin.

## <a name="iot-edge-automatic-deployment"></a>Otomatik dağıtım IoT Edge
IoT Edge otomatik dağıtım, bir IoT Edge modülleri kümesini çalıştırmak için hedef IoT Edge cihaz kümesini yapılandırır. Her dağıtım, yeni cihazlar oluşturulduğunda veya hedef koşulla eşleşecek şekilde değiştirilse bile, hedef durumuyla eşleşen tüm cihazların belirtilen modül kümesini çalıştırdığından sürekli olarak emin olur. Her IoT Edge cihaz yalnızca hedef koşulunu karşılayan en yüksek öncelikli dağıtımı alır. [IoT Edge otomatik dağıtım](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring)hakkında daha fazla bilgi edinin.

## <a name="iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimi
Bir veya daha fazla IoT Edge cihazda, bir veya daha fazla cihaz, yol ve ilişkili modül istenen özellikleri dağıtmak için bir veya daha fazla cihaz (ler) de kopyalanacak bilgileri içeren bir JSON belgesi.

## <a name="iot-edge-gateway-device"></a>IoT Edge ağ geçidi cihazı
Aşağı akış cihazına sahip bir IoT Edge cihaz. Aşağı akış cihazı IoT Edge ya da IoT Edge cihaz olabilir.

## <a name="iot-edge-hub"></a>IoT Edge hub 'ı
Modül iletişimleri, yukarı akış (IoT Hub ve IoT Hub) iletişimlerine ait modülden sorumlu IoT Edge çalışma zamanının parçasıdır. 

## <a name="iot-edge-leaf-device"></a>IoT Edge yaprak cihaz
Aşağı akış cihazı olmayan bir IoT Edge cihaz. 

## <a name="iot-edge-module"></a>IoT Edge modülü
IoT Edge modülü, IoT Edge cihazlara dağıtabileceğiniz bir Docker kapsayıcısıdır. Bir cihazdan ileti almak, ileti dönüştürmek veya IoT Hub 'ına ileti göndermek gibi belirli bir görevi gerçekleştirir. Diğer modüllerle iletişim kurar ve IoT Edge çalışma zamanına veri gönderir. [IoT Edge modülleri geliştirmeye yönelik gereksinimleri ve araçları anlayın](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>IoT Edge modül kimliği
IoT Hub modül kimliği kayıt defterindeki bir kayıt, bir modül tarafından bir uç hub veya IoT Hub kimlik doğrulaması için kullanılacak güvenlik kimlik bilgilerini ayrıntılandıran bir kayıttır.

## <a name="iot-edge-module-image"></a>IoT Edge modülü resmi
Modül örneklerinin örneğini oluşturmak için IoT Edge çalışma zamanı tarafından kullanılan Docker görüntüsü.

## <a name="iot-edge-module-twin"></a>IoT Edge Module ikizi
Bir modül örneği için durum bilgilerini depolayan IoT Hub kalıcı olan bir JSON belgesi.

## <a name="iot-edge-priority"></a>IoT Edge önceliği
İki IoT Edge dağıtım aynı cihazı hedefdığında, yüksek önceliğe sahip dağıtım uygulanır. İki dağıtımda aynı öncelik varsa, daha sonra oluşturma tarihi ile dağıtım uygulanır. [Öncelik](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority)hakkında daha fazla bilgi edinin.

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı
IoT Edge Runtime, Microsoft 'un IoT Edge bir cihaza yükleneceği her şeyi içerir. Edge Aracısı, Edge hub ve IoT Edge güvenlik cini içerir.

## <a name="iot-edge-set-modules-to-a-single-device"></a>Modülleri tek bir cihaza ayarlamak IoT Edge
Bir IoT Edge bildiriminin içeriğini bir cihazdaki ' Module ikizi kopyalayan bir işlem. Temel alınan API, giriş olarak bir IoT Edge bildirimi alan genel bir ' yapılandırma uygulama ' dır.

## <a name="iot-edge-target-condition"></a>IoT Edge hedef koşulu
IoT Edge dağıtımında, hedef koşul, dağıtımın hedef cihazlarını seçmek için, Device TWINS ' etiketlerindeki tüm Boole koşullarıdır (örneğin, **Tag. Environment = üretim**). Hedef koşul sürekli olarak gereksinimleri karşılayan yeni cihazları içerecek şekilde değerlendirilir veya artık olmayan cihazları kaldırır. [Hedef koşul](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition) hakkında daha fazla bilgi edinin