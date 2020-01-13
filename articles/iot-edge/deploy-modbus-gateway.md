---
title: Ağ geçidi - Azure IOT Edge modbus protokollerini çevirme | Microsoft Docs
description: IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: kgremban
ms.openlocfilehash: d4d9d2ff899a169ee81544761fe87fed3cca06c4
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75912320"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Bir IOT Edge ağ geçidi cihazı aracılığıyla Modbus TCP cihazlarını bağlama

Modbus TCP veya RTU protokollerini kullanan IoT cihazlarını bir Azure IoT Hub 'ına bağlamak istiyorsanız, ağ geçidi olarak bir IoT Edge cihazı kullanabilirsiniz. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir.

![Modbus Cihazları IoT Edge Ağ Geçidi üzerinden IoT Hub bağlanır](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır.

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. Modülün Modbus RTU 'yi destekleyecek şekilde nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. GitHub 'da [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesi.

## <a name="prerequisites"></a>Ön koşullar
* Bir Azure IoT Edge cihazı. Bir adım ayarlama hakkında yönergeler için bkz. Windows veya [Linux](quickstart-linux.md) [üzerinde Azure IoT Edge dağıtma](quickstart.md) .
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz. IPv4 adresini bilmeniz gerekecektir.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. Modüle Azure Marketi, [Modbus](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview)veya görüntü URI 'si ile `mcr.microsoft.com/azureiotedge/modbus:1.0`erişebilirsiniz.

Kendi modülünüzü oluşturmak ve ortamınız için özelleştirmek istiyorsanız, GitHub 'da açık kaynaklı [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesi vardır. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. Bir kapsayıcı görüntüsü oluşturmak için, [Visual Studio 'da C# modül geliştirme](how-to-visual-studio-develop-csharp-module.md) veya [Visual Studio Code modüller geliştirme](how-to-vs-code-develop-module.md)bölümüne bakın. Bu makaleler, yeni modüller oluşturma ve kapsayıcı görüntülerini bir kayıt defterine yayımlama hakkında yönergeler sağlar.

## <a name="try-the-solution"></a>Çözümü deneyin

Bu bölümde Microsoft 'un örnek Modbus modülünün IoT Edge cihazınıza dağıtımı gösterilmektedir.

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.

2. Git **IOT Edge** ve IOT Edge Cihazınızda'ı tıklatın.

3. **Modül ayarla**’yı seçin.

4. **IoT Edge modüller** bölümünde, Modbus modülünü ekleyin:

   1. Açılan menü **Ekle** ' ye tıklayın ve **Market modülü**' nü seçin.
   2. `Modbus` arayın ve Microsoft tarafından **Modbus TCP modülünü** seçin.
   3. Modül, IoT Hub için otomatik olarak yapılandırılır ve IoT Edge modülleri listesinde görünür. Yollar da otomatik olarak yapılandırılır. **İncele ve oluştur**’u seçin.
   4. Dağıtım bildirimini gözden geçirin ve **Oluştur**' u seçin.

5. Listede, `ModbusTCPModule`, ve **Modül Ikizi ayarları** sekmesini seçerek Modbus modülünü seçin. Module ikizi istenen özellikler için gerekli JSON otomatik olarak doldurulur.

6. JSON 'daki **SLA Veconnection** özelliğini bulun ve değerini Modbus cihazınızın IPv4 adresi olarak ayarlayın.

7. **Güncelleştir** seçeneğini belirleyin.

8. **Gözden geçir + oluştur**' u seçin, dağıtımı gözden geçirin ve ardından **Oluştur**' u seçin.

9. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Yeni `ModbusTCPModule` modülünü, IoT Edge çalışma zamanı ile birlikte görmeniz gerekir.

## <a name="view-data"></a>Verileri görüntüleme

Modbus modülünden gelen verileri görüntüleyin:

```cmd/sh
iotedge logs modbus
```

Ayrıca, cihazın gönderdiği Telemetriyi, [Visual Studio Code Için azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eskiden Azure IoT araç seti uzantısı) kullanarak da görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Edge cihazların ağ geçitleri olarak nasıl davranabileceği hakkında daha fazla bilgi edinmek için bkz. [saydam ağ geçidi olarak davranan bir IoT Edge cihaz oluşturma](./how-to-create-transparent-gateway.md).
* IoT Edge modüllerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure IoT Edge modüllerini anlama](iot-edge-modules.md).
