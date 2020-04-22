---
title: Azure IoT protokol ağ geçidi | Microsoft Dokümanlar
description: IoT Hub tarafından yerel olarak desteklenmeyen protokolleri kullanarak aygıtların hub'ınıza bağlanmasını sağlamak için IoT Hub özelliklerini ve protokol desteğini genişletmek için Azure IoT protokolü ağ geçidi nasıl kullanılır?
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/11/2017
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: bfd000e2b69f052e25f0ea6cd286b0ca3aef7519
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759714"
---
# <a name="support-additional-protocols-for-iot-hub"></a>IoT Hub için ek protokolleri destekleme

Azure IoT Hub, MQTT, AMQP ve HTTPS protokolleri üzerinden iletişimi yerel olarak destekler. Bazı durumlarda, aygıtlar veya alan ağ geçitleri bu standart protokollerden birini kullanamayabilir ve protokol uyarlaması gerektirebilir. Bu gibi durumlarda, özel bir ağ geçidi kullanabilirsiniz. Özel ağ geçidi, trafiği IoT Hub'a ve IoT Hub'dan köprüleyerek IoT Hub uç noktaları için protokol uyarlamasısağlar. IoT Hub için protokol uyarlamasını etkinleştirmek için [Azure IoT iletişim ağ geçidini](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) özel ağ geçidi olarak kullanabilirsiniz.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protokolü ağ geçidi

Azure IoT iletişim ağ geçidi, IoT Hub ile yüksek ölçekli, çift yönlü aygıt iletişimi için tasarlanmış protokol uyarlaması için bir çerçevedir. Protokol ağ geçidi, belirli bir protokol üzerinden aygıt bağlantılarını kabul eden bir geçiş bileşenidir. AmQP 1.0 üzerinden trafiği IoT Hub'a köprüler.

Azure Hizmet Kumaşı, Azure Bulut Hizmetleri çalışanı rolleri veya Windows Sanal Makineler kullanarak azure'daki protokol ağ geçidini yüksek ölçeklenebilir bir şekilde dağıtabilirsiniz. Ayrıca, protokol ağ geçidi alan ağ geçitleri gibi şirket içi ortamlarda dağıtılabilir.

Azure IoT iletişim ağ geçidi, gerekirse MQTT iletişim kuralı davranışını özelleştirmenize olanak tanıyan bir MQTT protokol bağdaştırıcısı içerir. IoT Hub, MQTT v3.1.1 protokolü için yerleşik destek sağladığından, yalnızca protokol özelleştirmeleri veya ek işlevsellik için özel gereksinimler gerekiyorsa MQTT protokol bağdaştırıcısını kullanmayı düşünmelisiniz.

MQTT bağdaştırıcısı, diğer protokoller için protokol bağdaştırıcıları oluşturmak için programlama modelini de gösterir. Buna ek olarak, Azure IoT ağ geçidi programlama modeli, özel kimlik doğrulama, ileti dönüşümleri, sıkıştırma/dekompresyon veya aygıtlar ve IoT Hub arasındaki trafiğin şifreleme/şifre çözme gibi özel işlemler için özel bileşenleri takmanıza olanak tanır.

Esneklik için, azure ioT protokolü ağ geçidi ve MQTT uygulaması bir açık kaynak yazılım projesinde sağlanır. Açık kaynak projesini, çeşitli protokoller ve protokol sürümleri için destek eklemek veya senaryonuz için uygulamayı özelleştirmek için kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT protokol ağ geçidi ve IoT çözümünüzün bir parçası olarak nasıl kullanılacağı ve dağıtılakullanılacağı hakkında daha fazla bilgi edinmek için bkz.

* [GitHub'da Azure IoT protokolü ağ geçidi deposu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)

* [Azure IoT protokolü ağ geçidi geliştirici kılavuzu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

IoT Hub dağıtımınızı planlama hakkında daha fazla bilgi edinmek için bkz:

* [Etkinlik Hub'larıyla karşılaştırın](iot-hub-compare-event-hubs.md)

* [Ölçekleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma](iot-hub-scaling.md)

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)