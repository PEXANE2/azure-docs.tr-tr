---
title: Azure IoT protokol ağ geçidi | Microsoft Docs
description: Cihazların IoT Hub yerel olarak desteklenmeyen protokoller kullanarak hub 'ınıza bağlanmasını sağlamak üzere IoT Hub yeteneklerini ve protokol desteğini genişletmek için Azure IoT protokolü ağ geçidini kullanma.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81759714"
---
# <a name="support-additional-protocols-for-iot-hub"></a>IoT Hub için ek protokolleri destekleme

Azure IoT Hub, MQTT, AMQP ve HTTPS protokolleri üzerinden iletişimi yerel olarak destekler. Bazı durumlarda, cihazlar veya alan ağ geçitleri bu standart protokollerden birini kullanabilmeyebilir ve protokol uyarlaması gerektirebilir. Bu gibi durumlarda, özel bir ağ geçidi kullanabilirsiniz. Özel bir ağ geçidi IoT Hub trafiği köprüleyerek IoT Hub uç noktaları için protokol uyarlama sağlar. IoT Hub için protokol uyarlama özelliğini etkinleştirmek üzere [Azure IoT protokolü ağ geçidini](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) özel bir ağ geçidi olarak kullanabilirsiniz.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT protokolü ağ geçidi

Azure IoT protokolü ağ geçidi, IoT Hub ile yüksek ölçekli, çift yönlü cihaz iletişimi için tasarlanan protokol uyarlama için bir çerçevedir. Protokol ağ geçidi, belirli bir protokol üzerinden cihaz bağlantılarını kabul eden bir geçiş bileşenidir. Trafiği AMQP 1,0 üzerinden IoT Hub köprüler.

Azure Service Fabric, Azure Cloud Services çalışan rolleri veya Windows Sanal Makineleri kullanarak protokol ağ geçidini Azure 'da yüksek oranda ölçeklenebilir bir şekilde dağıtabilirsiniz. Ayrıca, protokol ağ geçidi, alan ağ geçitleri gibi şirket içi ortamlarda dağıtılabilir.

Azure IoT protokolü ağ geçidi, gerekirse MQTT protokol davranışını özelleştirmenizi sağlayan bir MQTT protokol bağdaştırıcısı içerir. IoT Hub MQTT v 3.1.1 protokolü için yerleşik destek sağladığından, yalnızca protokol özelleştirmeleri veya ek işlevsellik için özel gereksinimler gerekliyse MQTT protokol bağdaştırıcısını kullanmayı düşünmelisiniz.

MQTT bağdaştırıcısı Ayrıca diğer protokoller için protokol bağdaştırıcıları oluşturmaya yönelik programlama modelini gösterir. Buna ek olarak, Azure IoT protokolü ağ geçidi programlama modeli özel işleme, ileti dönüştürmeleri, sıkıştırma/sıkıştırmayı açma veya cihazlar ile IoT Hub arasında trafiği şifreleme/şifre çözme gibi özel işlemler için özel bileşenler eklemenize olanak tanır.

Esneklik için, Azure IoT protokolü ağ geçidi ve MQTT uygulamasının açık kaynaklı bir yazılım projesinde sağlanması gerekir. Çeşitli protokoller ve protokol sürümleri için destek eklemek üzere açık kaynaklı projeyi kullanabilir ya da senaryonuz için uygulamayı özelleştirebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT protokolü ağ geçidi hakkında daha fazla bilgi edinmek ve IoT çözümünüzün bir parçası olarak bunu kullanmak ve dağıtmak için bkz.:

* [GitHub 'da Azure IoT protokolü ağ geçidi deposu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)

* [Azure IoT protokolü ağ geçidi Geliştirici Kılavuzu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

IoT Hub Dağıtımınızı planlama hakkında daha fazla bilgi edinmek için bkz.:

* [Event Hubs ile karşılaştırın](iot-hub-compare-event-hubs.md)

* [Ölçeklendirme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma](iot-hub-scaling.md)

* [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)