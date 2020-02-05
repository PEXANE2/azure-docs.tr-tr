---
title: Azure IoT Edge ve Azure IoT Central | Microsoft Docs
description: Azure IoT Edge IoT Central bir uygulamayla nasıl kullanacağınızı anlayın.
author: dominicbetts
ms.author: dobett
ms.date: 12/12/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 69660152458de26e9dbcbf1f50db6ce6824351d0
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027075"
---
# <a name="connect-azure-iot-edge-devices-to-an-azure-iot-central-application"></a>Azure IoT Edge cihazlarını bir Azure IoT Central uygulamasına bağlama

IoT Edge üç bileşenden oluşur:

* **IoT Edge modüller** , Azure Hizmetleri, iş ortağı hizmetleri veya kendi kodunuzla çalışan kapsayıcılardır. Modüller IoT Edge cihazlara dağıtılır ve bu cihazlarda yerel olarak çalıştırılır.
* **IoT Edge çalışma zamanı** her bir IoT Edge cihazında çalışır ve her cihaza dağıtılan modülleri yönetir.
* **Bulut tabanlı bir arabirim** , IoT Edge cihazları uzaktan izlemenize ve yönetmenize olanak sağlar. IoT Central bulut arabirimidir.

**Azure IoT Edge** cihaz, IoT Edge cihazına bağlanan aşağı akış cihazları ile bir ağ geçidi cihazı olabilir. Bu makale, aşağı akış cihaz bağlantısı desenleri hakkında daha fazla bilgi paylaşır.

**Cihaz şablonu** , cihazınızın ve IoT Edge modüllerinizin yeteneklerini tanımlar. Özellikler modülün gönderdiği Telemetriyi, modül özelliklerini ve bir modülün yanıt verdiği komutları içerir.

## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Ağ geçidi ve modüllerle aşağı akış cihaz ilişkileri

Aşağı akış cihazları `$edgeHub` modülü aracılığıyla bir IoT Edge Gateway cihazına bağlanabilir. Bu IoT Edge cihaz, bu senaryoda saydam bir ağ geçidi haline gelir.

![Saydam ağ geçidi diyagramı](./media/concepts-iot-edge/gateway-transparent.png)

Aşağı akış cihazları, özel bir modül aracılığıyla bir IoT Edge Gateway cihazına da bağlanabilir. Aşağıdaki senaryoda, aşağı akış cihazları bir Modbus özel modülü aracılığıyla bağlanır.

![Özel modül bağlantısının diyagramı](./media/concepts-iot-edge/gateway-module.png)

Aşağıdaki diyagramda her iki modül türü (özel ve `$edgeHub`) aracılığıyla bir IoT Edge ağ geçidi cihazına bağlantı gösterilmektedir.  

![Her iki bağlantı modülü ile bağlanma diyagramı](./media/concepts-iot-edge/gateway-module-transparent.png)

Son olarak, aşağı akış cihazları birden çok özel modülle bir IoT Edge ağ geçidi cihazına bağlanabilir. Aşağıdaki diyagramda, bir Modbus özel modülü, uyumlu olmayan özel bir modül ve `$edgeHub` modülü aracılığıyla bağlanan aşağı akış cihazları gösterilmektedir. 

![Birden çok özel modül aracılığıyla bağlantı diyagramı](./media/concepts-iot-edge/gateway-module2-transparent.png)

## <a name="deployment-manifests-and-device-templates"></a>Dağıtım bildirimleri ve cihaz şablonları

IoT Edge, iş mantığını modül biçiminde dağıtabilir ve yönetebilirsiniz. IoT Edge modüller, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure Hizmetleri (Azure Stream Analytics gibi) veya kendi çözüme özgü kodunuzla bulunabilir. Modüllerin nasıl geliştirildiğini, dağıtıldığını ve yapıldığını anlamak için bkz. [IoT Edge modüller](../../iot-edge/iot-edge-modules.md).

Yüksek düzeyde, bir dağıtım bildirimi, istenen özellikleri ile yapılandırılan modül ikizlerini bir listesidir. Dağıtım bildirimi, hangi modüllerin yükleneceğini ve bunların nasıl yapılandırılacağını bir IoT Edge cihazına (veya bir cihaz grubuna) bildirir. Dağıtım bildirimleri, her modül için istenen özellikleri içerir ikizi. IoT Edge cihazlar her bir modülün bildirilen özelliklerini geri bildirir.

Dağıtım bildirimi oluşturmak için Visual Studio Code kullanın. Daha fazla bilgi için bkz. [Visual Studio Code Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Azure IoT Central, bir cihaz şablonu oluşturmak için dağıtım bildirimini içeri aktarabilirsiniz. Aşağıdaki akış çizelgesinde IoT Central bir dağıtım bildirimi yaşam döngüsü gösterilmektedir.

![Dağıtım bildirimi yaşam döngüsü akış çizelgesi](./media/concepts-iot-edge/dmflow.png)

IoT Tak ve Kullan (Önizleme) IoT Edge bir cihazı aşağıdaki şekilde modeller:

* Her IoT Edge cihaz şablonunda bir cihaz yetenek modeli vardır.
* Dağıtım bildiriminde listelenen her özel modül için bir modül yetenek modeli oluşturulur.
* Her modül yetenek modeli ve cihaz yetenek modeli arasında bir ilişki oluşturulur.
* Modül yetenek modeli modül arabirimlerini uygular.
* Her modül arabirimi telemetri, Özellikler ve komutlar içerir.

![IoT Edge modelleme diyagramı](./media/concepts-iot-edge/edgemodelling.png)

## <a name="iot-edge-gateway-devices"></a>IoT Edge ağ geçidi cihazları

Bir IoT Edge cihazını ağ geçidi cihazı olacak şekilde seçtiyseniz, ağ geçidi cihazına bağlamak istediğiniz cihazlar için cihaz yetenek modellerine aşağı akış ilişkileri ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama şablonlarının ne IoT Central olduğunu öğrenolduğunuza göre, [bir IoT Central uygulaması oluşturarak](quick-deploy-iot-central.md)başlayın.