---
title: Azure IoT Central uygulamanıza bir Raspberry PI bağlama (Python) | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, Python kullanarak Raspberry Pi 'yi Azure IoT Central uygulamanıza bağlama.
author: dominicbetts
ms.author: dobett
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: bd506bf1210692feb017f3b526c3b6d4bca36004
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877419"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Azure IoT Central uygulamanıza bir Raspberry PI bağlama (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu makalede, bir cihaz geliştiricisi olarak, Python programlama dilini kullanarak bir Raspberry Pi 'yi Microsoft Azure IoT Central uygulamanıza nasıl bağlayabileceğinizi açıklamaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki bileşenlere ihtiyacınız vardır:

* **Örnek Devkits** uygulama şablonundan oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Raspbian işletim sistemini çalıştıran bir Raspberry PI cihazı. Raspberry PI, internet 'e bağlanabilmelidir. Daha fazla bilgi için bkz. [Raspberry PI 'Nizi ayarlama](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulaması

**Örnek Devkits** uygulama şablonundan oluşturulan bir uygulama, aşağıdaki özelliklere sahip bir **Raspberry PI** cihaz şablonu içerir:

- Telemetri, cihazın toplayacağı aşağıdaki ölçüleri içerir:
  - Nem
  - Sıcaklık
  - Basınç
  - Manyetik tometre (X, Y, Z)
  - İvme ölçer (X, Y, Z)
  - Jroscope (X, Y, Z)
- Ayarlar
  - Geril
  - Geçerli
  - Fan hızı
  - IR geçişi.
- Özellikler
  - Zar numarası cihaz özelliği
  - Konum bulutu özelliği

Cihaz şablonunun yapılandırmasının tam ayrıntıları için, [Raspberry PI cihaz şablonu ayrıntılarına](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)bakın.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Central uygulamanızda, **Raspberry PI** cihaz şablonundan gerçek bir cihaz ekleyin. Cihaz bağlantısı ayrıntılarını (**kapsam kimliği**, **cihaz kimliği**ve **birincil anahtar**) bir yere unutmayın. Daha fazla bilgi için bkz. [Azure IoT Central uygulamanıza gerçek bir cihaz ekleme](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Raspberry PI 'yi yapılandırma

Aşağıdaki adımlarda, örnek Python uygulamasının GitHub 'dan nasıl indirileceği ve yapılandırılacağı açıklanır. Bu örnek uygulama:

* Azure IoT Central telemetri ve özellik değerlerini gönderir.
* Azure IoT Central 'da yapılan ayar değişikliklerine yanıt verir.

Cihazı yapılandırmak için [GitHub 'daki adım adım yönergeleri izleyin](https://github.com/Azure/iot-central-firmware/blob/master/RaspberryPi/README.md).

1. Cihaz yapılandırıldığında, cihazınız Azure IoT Central telemetri ölçümleri göndermeye başlar.
1. Azure IoT Central uygulamanızda, Raspberry PI üzerinde çalışan kodun uygulamayla nasıl etkileşime gireceğini görebilirsiniz:

    * Gerçek cihazınızın **ölçümler** sayfasında, Raspberry Pi 'den gönderilen Telemetriyi görebilirsiniz.
    * **Ayarlar** sayfasında, Raspberry Pi üzerinde voltaj ve fan hızı gibi ayarları değiştirebilirsiniz. Raspberry Pi, değişikliği gösterdiğinde, ayar **eşitlenmiş**olarak gösterilir.

## <a name="raspberry-pi-device-template-details"></a>Raspberry PI cihaz şablonu ayrıntıları

**Örnek Devkits** uygulama şablonundan oluşturulan bir uygulama, aşağıdaki özelliklere sahip bir **Raspberry PI** cihaz şablonu içerir:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| kopyalar           | 20     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| Ivometerx | mg     | -2000   | 2000    | 0              |
| Iventery | mg     | -2000   | 2000    | 0              |
| Ivometerz | mg     | -2000   | 2000    | 0              |
| Jroscopex     | MDPS   | -2000   | 2000    | 0              |
| Jroscopey     | MDPS   | -2000   | 2000    | 0              |
| Jroscopez     | MDPS   | -2000   | 2000    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarlar

| Display name | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Geril      | Setvoltaj | Çalışmıyorken | 0              | 0       | 240     | 0       |
| Geçerli      | setCurrent | AMPS  | 0              | 0       | 100     | 0       |
| Fan hızı    | Fanın hızı   | RPM   | 0              | 0       | 1000    | 0       |

Ayarları aç

| Display name | Alan adı | Metinde | Kapalı metin | İlk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | Activateır | AÇIK      | KAPALI      | Kapalı     |

### <a name="properties"></a>Özellikler

| Type            | Display name | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Zar numarası   | dieNumber  | numarası    |
| Text            | Location     | location   | Yok       |

## <a name="next-steps"></a>Sonraki adımlar

Artık bir Raspberry Pi 'yi Azure IoT Central uygulamanıza bağlamayı öğrendiğinize göre, önerilen sonraki adım, kendi IoT cihazınız için [özel bir cihaz şablonu ayarlamayı](howto-set-up-template.md) öğrenirsiniz.
