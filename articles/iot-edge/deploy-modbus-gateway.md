---
title: Modbus protokollerini ağ geçitleriyle çevirin - Azure IoT Edge | Microsoft Dokümanlar
description: IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: kgremban
ms.openlocfilehash: 23fbbd87230ea0a0147dc9d90c77729f4d531e98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511153"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Modbus TCP aygıtlarını bir IoT Edge aygıt ağ geçidi üzerinden bağlayın

Modbus TCP veya RTU protokollerini kullanan IoT aygıtlarını Azure IoT hub'ına bağlamak istiyorsanız, ağ geçidi olarak bir IoT Edge aygıtı kullanabilirsiniz. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir.

![Modbus aygıtları IoT Edge ağ geçidi aracılığıyla IoT Hub'a bağlanır](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır.

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. Modülü Modbus RTU'yu destekleyecek şekilde nasıl yapılandırılabilirsiniz hakkında daha fazla bilgi için GitHub'daki [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesine bakın.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure IoT Edge cihazı. Nasıl ayarlır diye [ilgili](quickstart.md) bir gözden geçirme için [bkz.](quickstart-linux.md)
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz. IPv4 adresini bilmeniz gerekir.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. Modüle Azure Marketi, [Modbus](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview)veya URI görüntüsüyle `mcr.microsoft.com/azureiotedge/modbus:1.0`erişebilirsiniz.

Kendi modülünüzü oluşturmak ve ortamınız için özelleştirmek istiyorsanız, GitHub'da açık kaynak kodlu bir [Azure IoT Edge Modbus modül](https://github.com/Azure/iot-edge-modbus) projesi vardır. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. Bir kapsayıcı görüntüsü oluşturmak için [Visual Studio'da C# modülleri geliştirme](how-to-visual-studio-develop-csharp-module.md) veya [Visual Studio Code'daki modülgeliştirme'ye](how-to-vs-code-develop-module.md)bakın. Bu makaleler, yeni modüller oluşturma ve kapsayıcı görüntülerinin bir kayıt defterine yayımlama hakkında yönergeler sağlar.

## <a name="try-the-solution"></a>Çözümü deneyin

Bu bölüm, Microsoft'un örnek Modbus modüllerini IoT Edge aygıtınıza dağıtmayoluyla yürür.

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.

2. **IoT Edge'e** gidin ve IoT Edge cihazınıza tıklayın.

3. **Modülleri Ayarla'yı**seçin.

4. **IoT Edge Modülleri** bölümünde Modbus modüllerini ekleyin:

   1. Açılan **ekle'yi** tıklatın ve **Market Modülü'nü**seçin.
   2. Microsoft `Modbus` tarafından **Modbus TCP Modülünü** arayın ve seçin.
   3. Modül, IoT Hub'ınız için otomatik olarak yapılandırılır ve IoT Edge Modülleri listesinde görünür. Rotalar da otomatik olarak yapılandırılır. **İncele ve oluştur**’u seçin.
   4. Dağıtım bildirimini gözden geçirin ve **Oluştur'u**seçin.

5. Listede modbus modülünü `ModbusTCPModule`seçin ve Modül **İkiz Ayarları** sekmesini seçin. Modül ikiz istenilen özellikleri için gerekli JSON otomatik doldurulur.

6. JSON'daki **SlaveConnection** özelliğini arayın ve değerini Modbus aygıtınızın IPv4 adresine ayarlayın.

7. **Güncelleştir** seçeneğini belirleyin.

8. **Gözden Geçir + oluştur'** u seçin, dağıtımı gözden geçirin ve sonra **Oluştur'u**seçin.

9. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Yeni `ModbusTCPModule` modülü nIoT Edge çalışma süresiyle birlikte görmeniz gerekir.

## <a name="view-data"></a>Verileri görüntüleme

Modbus modülünden gelen verileri görüntüleyin:

```cmd/sh
iotedge logs modbus
```

[Visual Studio Code için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eski adıyla Azure IoT Toolkit uzantısı) kullanarak aygıtın gönderdiği telemetriyi de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Edge aygıtlarının ağ geçidi olarak nasıl hareket edebileceği hakkında daha fazla bilgi edinmek için saydam [ağ geçidi görevi gören bir IoT Edge aygıtı oluştur'a](./how-to-create-transparent-gateway.md)bakın.
* IoT Edge modüllerinin nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](iot-edge-modules.md)
