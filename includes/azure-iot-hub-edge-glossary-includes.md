---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 11/09/2018
ms.author: dobett
ms.openlocfilehash: c95bca125ea70cf32acad0d5ea67c3ad195ed704
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188490"
---
## <a name="automatic-device-management"></a>Otomatik Cihaz Yönetimi
Azure IoT Hub'daki Otomatik Aygıt Yönetimi, büyük aygıt filolarını tüm yaşam döngüleri boyunca yönetmenin yinelenen ve karmaşık görevlerinin çoğunu otomatikleştirir. Otomatik Aygıt Yönetimi ile, özelliklerine göre bir aygıt kümesini hedefleyebilir, istenen yapılandırmayı tanımlayabilir ve ioT Hub'ın aygıtları kapsama girdiklerinde güncelleştirmesine izin verebilirsiniz.  Otomatik [aygıt yapılandırmaları](../articles/iot-hub/iot-hub-auto-device-config.md) ve [IoT Edge otomatik dağıtımlarından](../articles/iot-edge/how-to-deploy-monitor.md)oluşur.

## <a name="iot-edge"></a>IoT Edge
Azure IoT Edge, Azure hizmetlerinin ve çözüme özel kodun şirket içi aygıtlara bulut odaklı şekilde dağıtılmasını sağlar. IoT Edge aygıtları, veriler buluta gönderilmeden önce bilgi işlem ve analiz gerçekleştirmek için diğer aygıtlardan veri toplayabilir. Daha fazla bilgi için [Azure IoT Edge'e](https://docs.microsoft.com/azure/iot-edge/)bakın.

## <a name="iot-edge-agent"></a>IoT Edge aracısı
IoT Edge çalışma zamanının modüllerin dağıtımı ve izlenmesinden sorumlu bölümü.

## <a name="iot-edge-device"></a>IoT Edge cihazı
IoT Edge aygıtları IoT Edge çalışma zamanı yüklüdür ve aygıt ayrıntılarında **IoT Edge aygıtı** olarak işaretlenir. [Azure IoT Edge'i Linux'ta simüle edilmiş bir cihazda nasıl dağıtabileceğinizi](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)öğrenin - önizleme.

## <a name="iot-edge-automatic-deployment"></a>IoT Edge otomatik dağıtım
IoT Edge otomatik dağıtım, bir hedef IoT Edge aygıtı kümesini ioT Edge modülleri kümesini çalıştırmak için yapılandırır. Her dağıtım sürekli olarak, hedef durumuna uyan tüm aygıtların, yeni aygıtlar oluşturulsa veya hedef koşulla eşleşecek şekilde değiştirilse bile, belirtilen modül kümesini çalıştırmasını sağlar. Her IoT Edge aygıtı yalnızca hedef koşulunu karşılayan en yüksek öncelikli dağıtımı alır. [IoT Edge otomatik dağıtım](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring)hakkında daha fazla bilgi edinin.

## <a name="iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimi
Bir veya daha fazla IoT Edge aygıtının modül ikiz(ler) modül, rotalar ve ilişkili modül istenilen özellikleri dağıtmak için kopyalanacak bilgileri içeren bir Json belgesi.

## <a name="iot-edge-gateway-device"></a>IoT Edge ağ geçidi cihazı
Akış aşağı aygıtı olan bir IoT Edge cihazı. Aşağı akım aygıtı IoT Edge veya IoT Edge aygıtı olabilir.

## <a name="iot-edge-hub"></a>IoT Edge hub
IoT Edge çalışma zamanı modülünden modüliletişimine, yukarı akımdan (IoT Hub'a doğru) ve aşağı akım (IoT Hub'ından uzakta) iletişimden sorumlu bölümü. 

## <a name="iot-edge-leaf-device"></a>IoT Edge yaprak cihaz
Akış aşağı aygıtı olmayan bir IoT Edge cihazı. 

## <a name="iot-edge-module"></a>IoT Edge modülü
IoT Edge modülü, IoT Edge aygıtlarına dağıtabileceğiniz bir Docker kapsayıcıstır. Aygıttan ileti alma, iletiyi dönüştürme veya bir IoT hub'ına ileti gönderme gibi belirli bir görevi gerçekleştirir. Diğer modüllerle iletişim kurar ve Verileri IoT Edge çalışma zamanına gönderir. [IoT Edge modülleri geliştirmek için gereksinimleri ve araçları anlayın.](https://docs.microsoft.com/azure/iot-edge/module-development)

## <a name="iot-edge-module-identity"></a>IoT Edge modül kimliği
IoT Hub modülü kimlik kayıt defterinde, bir modül tarafından kenar hub'ı veya IoT Hub ile kimlik doğrulamak için kullanılacak varlığı ve güvenlik kimlik bilgilerini ayrıntılı olarak açıklayan bir kayıt.

## <a name="iot-edge-module-image"></a>IoT Edge modül görüntüsü
Modül örneklerini anlık olarak anımsamak için IoT Edge çalışma zamanı tarafından kullanılan docker görüntüsü.

## <a name="iot-edge-module-twin"></a>IoT Edge modülü ikiz
IoT Hub'ında, durum bilgilerini bir modül örneği için depolayan bir Json belgesi kalıcıdır.

## <a name="iot-edge-priority"></a>IoT Edge önceliği
İki IoT Edge dağıtımı aynı aygıtı hedef aldığında, daha yüksek önceliğe sahip dağıtım uygulanır. İki dağıtım aynı önceliğe sahipse, sonraki oluşturma tarihiyle dağıtım uygulanır. [Öncelik](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority)hakkında daha fazla bilgi edinin.

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı
IoT Edge çalışma süresi, Microsoft'un bir IoT Edge aygıtına yüklenecek şekilde dağıttığı her şeyi içerir. Edge aracısı, Edge hub'ı ve IoT Edge güvenlik daemonu içerir.

## <a name="iot-edge-set-modules-to-a-single-device"></a>IoT Edge modülleri tek bir cihaza ayarlama
Bir IoT Edge'in içeriğini bir aygıtın modül ikizinde kopyalayan bir işlem. Altta yatan API, yalnızca bir giriş olarak bir IoT Edge bildirimi ni alan genel bir 'uygulamalı yapılandırma'dır.

## <a name="iot-edge-target-condition"></a>IoT Edge hedef durumu
Bir IoT Edge dağıtımında, Hedef koşulu, dağıtımın hedef aygıtlarını seçmek için aygıt ikizlerinin etiketlerinde herhangi bir Boolean koşuludur, örneğin **tag.environment = prod**. Hedef koşul, gereksinimleri karşılayan veya artık olmayan aygıtları kaldıran yeni aygıtları içerecek şekilde sürekli olarak değerlendirilir. [Hedef durumu](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition) hakkında daha fazla bilgi edinin