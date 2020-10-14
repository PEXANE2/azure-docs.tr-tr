---
title: IoT Tak ve Kullan Köprüsü | Microsoft Docs
description: IoT Tak ve Kullan köprüsünü ve Windows veya Linux ağ geçidine bağlı mevcut Cihazları IoT Tak ve Kullan cihazları olarak bağlamak için nasıl kullanacağınızı anlayın.
author: usivagna
ms.author: ugans
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0435fe3946118d59d786dd3e6cec350a5ab4eee4
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046460"
---
# <a name="iot-plug-and-play-bridge"></a>IoT Tak Çalıştır köprüsü

IoT Tak ve Kullan Köprüsü, Windows veya Linux ağ geçidine bağlı mevcut cihazların IoT Tak ve Kullan cihazları olarak bağlanmasına yönelik açık kaynaklı bir uygulamadır. Uygulamayı Windows veya Linux makinenizde yükledikten ve yapılandırdıktan sonra, ekli cihazları bir IoT Hub 'ına bağlamak için kullanabilirsiniz. IoT Tak ve Kullan arabirimlerini eklenen cihazların gönderdiği telemetriye eşlemek, cihaz özellikleriyle çalışmak ve komutları çağırmak için köprüyü kullanabilirsiniz.

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-high-level.png" alt-text="Sol tarafta, IoT Tak ve Kullan Köprüsü içeren bir Windows veya Linux BILGISAYARA eklenen mevcut sensörlerden (hem kablolu hem de kablosuz) biri vardır. IoT Tak ve Kullan köprüsü daha sonra sağ taraftaki bir IoT Hub 'ına bağlanır":::

IoT Tak ve Kullan Köprüsü, Windows 10 veya Linux çalıştıran herhangi bir IoT cihazında, endüstriyel PC, sunucuda veya ağ geçidinde tek başına yürütülebilir dosya olarak dağıtılabilir. Uygulama kodunuza de derlenebilir. Basit bir Configuration JSON dosyası, IoT Tak ve Kullan köprüsüne ekli cihazların/çevre birimlerinin Azure 'a sunulacağını söyler.

## <a name="supported-protocols-and-sensors"></a>Desteklenen protokoller ve algılayıcılar

IoT Tak ve Kullan Köprüsü, bağdaştırıcı belgelerinin bağlantılarıyla birlikte, varsayılan olarak aşağıdaki çevresel çevre türlerini destekler:

|Çevre birimi|Windows|Linux|
|---------|---------|---------|
|[Bluetooth LE](https://aka.ms/iot-pnp-bridge-bluetooth)       |Yes|Hayır|
|[Kameralar](https://aka.ms/iot-pnp-bridge-camera)               |Yes|Hayır|
|[Modbus](https://aka.ms/iot-pnp-bridge-modbus)                |Yes|Yes|
|[MQTT](https://aka.ms/iot-pnp-bridge-mqtt)                    |Yes|Yes|
|[Ardışık](https://aka.ms/iot-pnp-bridge-serial)                |Yes|Yes|
|[Windows USB çevre birimleri](https://aka.ms/iot-pnp-bridge-usb)  |Yes|Geçerli değil|

>[!Important]
>Geliştiriciler IoT **[Tak ve Kullan köprüsü geliştirici belgelerindeki](https://aka.ms/iot-pnp-bridge-dev-doc)** yönergeler aracılığıyla diğer cihaz protokollerini desteklemek için IoT Tak ve kullan köprüsünü genişletebilir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="os-platform"></a>İşletim sistemi platformu

Aşağıdaki işletim sistemi platformları ve sürümleri desteklenir:

|Platform  |Desteklenen Sürümler  |
|---------|---------|
|Windows 10 |     Tüm Windows SKU 'Ları desteklenir. Örneğin: IoT Enterprise, Server, Desktop, IoT Core. *Kamera sistem durumu izleme işlevselliği için 20H1 veya üzeri derleme önerilir. Tüm Windows 10 Derlemeleriyle tüm diğer işlevler kullanılabilir.*  |
|Linux     |Ubuntu 18,04 üzerinde sınanmış ve desteklenmiş, diğer dağıtımların işlevselliği sınanmamıştır.         |
||

### <a name="hardware"></a>Donanım

- Yukarıdaki işletim sistemi SKU 'Larını ve sürümlerini destekleyen herhangi bir donanım platformu.
- Seri, USB, Bluetooth ve kamera çevreörleri ve algılayıcılar yerel olarak desteklenir. IoT Tak ve Kullan Köprüsü, herhangi bir özel çevre birimi veya algılayıcıyı desteklemek için genişletilebilir ([Yukarıdaki çevre birimleri bölümüne bakın](#iot-plug-and-play-bridge)).

### <a name="development-environment"></a>Geliştirme Ortamı

IoT Tak ve Kullan köprüsü oluşturmak, genişletmek ve geliştirmek için gerekenler şunlardır:  

- C++ derlemesini destekleyen bir geliştirme ortamı: [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/)-Visual Studio 'Yu yüklerken c++ iş yüküyle masaüstü geliştirmeyi eklediğinizden emin olun.
- [CMake](https://cmake.org/download/) -CMake 'i yüklediğinizde seçeneğini belirleyin `Add CMake to the system PATH` .
- Windows üzerinde oluşturuyorsanız, Windows 17763 SDK 'sını de indirmeniz gerekir: [https://developer.microsoft.com/windows/downloads/windows-10-sdk](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
- [Azure IoT Hub Device C SDK](https://github.com/Azure/azure-iot-sdk-c). Bu depoya dahil edilen derleme betikleri, sizin için gerekli Azure IoT C SDK 'sını otomatik olarak klonlayabilir.

### <a name="azure-iot-products-and-tools"></a>Azure IoT ürünleri ve araçları

- **Azure IoT Hub** -cihazınızı uygulamasına bağlamak için Azure aboneliğinizde bir [Azure IoT Hub](../iot-hub/index.yml) 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/). Bir IoT Hub 'ınız yoksa, [oluşturmak için bu yönergeleri izleyin](../iot-hub/iot-hub-create-using-cli.md).

> [!Note]
> IoT Tak ve Kullan şu anda Orta ABD, Kuzey Avrupa ve Doğu Japonya bölgelerinde oluşturulan IoT Hub 'larda sunulmaktadır. IoT Tak ve Kullan desteği temel katmanlı IoT Hub 'larına dahil değildir. IoT Tak ve Kullan cihazlarınızla etkileşim kurmak için Azure IoT gezgin aracını kullanabilirsiniz. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) .

## <a name="iot-plug-and-play-bridge-architecture"></a>IoT Tak ve Kullan köprü mimarisi

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-components.png" alt-text="Sol tarafta, IoT Tak ve Kullan Köprüsü içeren bir Windows veya Linux BILGISAYARA eklenen mevcut sensörlerden (hem kablolu hem de kablosuz) biri vardır. IoT Tak ve Kullan köprüsü daha sonra sağ taraftaki bir IoT Hub 'ına bağlanır":::

## <a name="download-iot-plug-and-play-bridge"></a>IoT Tak ve Kullan köprüsü indir

Bir köprünün önceden oluşturulmuş bir sürümünü [ıot Tak ve kullan köprü sürümlerinde](https://aka.ms/iot-pnp-bridge-releases) desteklenen bağdaştırıcılarla indirebilir ve en son sürümün varlık listesini genişletebilirsiniz. İşletim sisteminiz için uygulamanın en son sürümünü indirin.

Ayrıca, [GitHub 'Daki ıot Tak ve kullan köprüsünün](https://aka.ms/bridge)kaynak kodunu indirebilir ve görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IoT Tak ve Kullan köprüsü mimarisine genel bir bakış edinolduğunuza göre, sonraki adımlar hakkında daha fazla bilgi edinebilirsiniz:

- [IoT Tak ve Kullan köprüsü kullanma](./howto-use-iot-pnp-bridge.md)
- [IoT Tak ve Kullan köprüsü için GitHub geliştirici başvurusuna bakın](https://aka.ms/iot-pnp-bridge-dev-doc)
- [GitHub 'da IoT Tak ve Kullan Köprüsü](https://aka.ms/iotplugandplaybridge)