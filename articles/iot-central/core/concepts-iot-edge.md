---
title: Azure IoT Edge ve Azure IoT Merkezi | Microsoft Dokümanlar
description: IoT Central uygulamasıyla Azure IoT Edge'in nasıl kullanılacağını öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 12/12/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 69660152458de26e9dbcbf1f50db6ce6824351d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77027075"
---
# <a name="connect-azure-iot-edge-devices-to-an-azure-iot-central-application"></a>Azure IoT Edge aygıtlarını Azure IoT Merkezi uygulamasına bağlama

IoT Edge üç bileşenden oluşur:

* **IoT Edge modülleri,** Azure hizmetlerini, iş ortağı hizmetlerini veya kendi kodunuzu çalıştıran kapsayıcılardır. Modüller IoT Edge aygıtlarına dağıtılır ve bu aygıtlarda yerel olarak çalıştırılır.
* **IoT Edge çalışma süresi** her IoT Edge aygıtında çalışır ve her aygıta dağıtılan modülleri yönetir.
* **Bulut tabanlı bir arayüz,** IoT Edge aygıtlarını uzaktan izlemenizi ve yönetmeniz sağlar. IoT Central bulut arabirimidir.

**Azure IoT Edge** aygıtı, alt aygıtların IoT Edge aygıtına bağlanabildiği bir ağ geçidi aygıtı olabilir. Bu makalede, alt akış aygıtı bağlantı desenleri hakkında daha fazla bilgi paylaşın.

**Aygıt şablonu,** cihazınızın ve IoT Edge modüllerinizin özelliklerini tanımlar. Yetenekler, modülün gönderdiği telemetriyi, modül özelliklerini ve bir modülün yanıt olarak yanıtlayış komutlarını içerir.

## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Ağ geçidi ve modüllerle alt akış aygıtı ilişkileri

Akış aşağı aygıtları modül üzerinden bir IoT Edge ağ geçidi aygıtına `$edgeHub` bağlanabilir. Bu IoT Edge aygıtı bu senaryoda saydam bir ağ geçidi olur.

![Saydam ağ geçidi diyagramı](./media/concepts-iot-edge/gateway-transparent.png)

Aşağı akış aygıtları da özel bir modül üzerinden bir IoT Edge ağ geçidi aygıtına bağlanabilir. Aşağıdaki senaryoda, akış aşağı aygıtlar bir Modbus özel modülü üzerinden bağlanır.

![Özel modül bağlantısı diyagramı](./media/concepts-iot-edge/gateway-module.png)

Aşağıdaki diyagram, her iki modül türü (özel ve) `$edgeHub`aracılığıyla bir IoT Edge ağ geçidi aygıtına bağlantıyı gösterir.  

![Her iki bağlantı modülü üzerinden bağlantı diyagramı](./media/concepts-iot-edge/gateway-module-transparent.png)

Son olarak, akış aşağı aygıtları birden çok özel modül aracılığıyla bir IoT Edge ağ geçidi aygıtına bağlanabilir. Aşağıdaki diyagram, modbus özel modülü, BLE özel modülü ve modül `$edgeHub` üzerinden bağlanan aşağı akım aygıtlarını gösterir. 

![Birden çok özel modül üzerinden bağlanma diyagramı](./media/concepts-iot-edge/gateway-module2-transparent.png)

## <a name="deployment-manifests-and-device-templates"></a>Dağıtım bildirimleri ve aygıt şablonları

IoT Edge'de, iş mantığını modüller biçiminde dağıtabilir ve yönetebilirsiniz. IoT Edge modülleri, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure hizmetlerini (Azure Akış Analizi gibi) veya çözüme özel kodunuzu içerebilir. Modüllerin nasıl geliştirildiğini, dağıtılabildiğini ve bakımını anlamak için [IoT Edge modüllerine](../../iot-edge/iot-edge-modules.md)bakın.

Yüksek düzeyde, dağıtım bildirimi, istenen özelliklere göre yapılandırılan modül ikizlerinin listesidir. Dağıtım bildirimi, bir IoT Edge aygıtına (veya bir aygıt grubuna) hangi modüllerin yüklenmesini ve bunları nasıl yapılandırılabildiğini söyler. Dağıtım bildirimleri, her modül ikizi için istenen özellikleri içerir. IoT Edge aygıtları her modül için bildirilen özellikleri geri bildirir.

Dağıtım bildirimi oluşturmak için Visual Studio Code'u kullanın. Daha fazla bilgi için [Visual Studio Kodu için Azure IoT Edge'e](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)bakın.

Azure IoT Central'da, aygıt şablonu oluşturmak için dağıtım bildirimini içe aktarabilirsiniz. Aşağıdaki akış şeması, IoT Central'da bir dağıtım bildirimi yaşam döngüsünü gösterir.

![Dağıtım manifestosu yaşam döngüsünün akış şeması](./media/concepts-iot-edge/dmflow.png)

IoT Tak ve Çalıştır (önizleme) bir IoT Edge cihazını aşağıdaki gibi modeller:

* Her IoT Edge aygıt şablonu bir aygıt yeteneği modeline sahiptir.
* Dağıtım bildiriminde listelenen her özel modül için bir modül yetenek modeli oluşturulur.
* Her modül yetenek modeli ile aygıt yetenek modeli arasında bir ilişki kurulur.
* Bir modül yetenek modeli modül arayüzleri uygular.
* Her modül arabirimi telemetri, özellikler ve komutlar içerir.

![IoT Edge modelleme diyagramı](./media/concepts-iot-edge/edgemodelling.png)

## <a name="iot-edge-gateway-devices"></a>IoT Edge ağ geçidi cihazları

Bir IoT Edge aygıtını ağ geçidi aygıtı olarak seçtiyseniz, ağ geçidi aygıtına bağlanmak istediğiniz aygıtlar için aygıt özelliği modellerine akış aşağı ilişkileri ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Merkezi uygulama şablonlarının ne olduğunu bildiğinize [göre, bir IoT Merkezi Uygulaması oluşturarak](quick-deploy-iot-central.md)başlayın.