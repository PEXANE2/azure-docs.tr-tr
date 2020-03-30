---
title: Azure CLI için IoT uzantısı ile Azure IoT aygıt yönetimi | Microsoft Dokümanlar
description: Azure IoT Hub aygıt yönetimi için Azure CLI için IoT uzantısını kullanın, Doğrudan yöntemleri ve Twin'in istenen özellik yönetimi seçeneklerini içeren.
author: chrissie926
manager: ''
keywords: azure iot cihaz yönetimi, azure iot hub cihaz yönetimi, cihaz yönetimi iot, iot hub cihaz yönetimi
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 88c3d1f4213b161d5e322349a7f0e1bc1dd952e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239655"
---
# <a name="use-the-iot-extension-for-azure-cli-for-azure-iot-hub-device-management"></a>Azure IoT Hub aygıt yönetimi için Azure CLI için IoT uzantısını kullanma

![Uçuça diyagram](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure CLI için IoT uzantısı,](https://github.com/Azure/azure-iot-cli-extension) [Azure CLI'nin](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)özelliklerine ekleyen bir açık kaynak IoT uzantısıdır. Azure CLI, Azure Kaynak Yöneticisi ve yönetim bitiş noktalarıyla etkileşim kurma komutlarını içerir. Örneğin, Bir Azure VM veya IoT hub'ı oluşturmak için Azure CLI'yi kullanabilirsiniz. CLI uzantısı, bir Azure hizmetinin Azure CLI'yi genişletmesine olanak tanır ve hizmete özel ek özelliklere erişmenizi sağlar. IoT uzantısı, IoT geliştiricilere tüm IoT Hub, IoT Edge ve IoT Hub Aygıt Sağlama Hizmeti özelliklerine komut satırı erişimi sağlar.

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev  |
|----------------------------|-----------|
| Doğrudan yöntemler             | İleti göndermeyi başlatma veya durdurma veya aygıtı yeniden başlatma gibi bir aygıt eylemi yapın.                                        |
| İkiz istenilen özellikler    | Bir aygıtı, LED'i yeşile ayarlama veya telemetri gönderme aralığını 30 dakikaya ayarlama gibi belirli durumlara koyun.         |
| İkiz bildirilen özellikler   | Bir aygıtın bildirilen durumunu alın. Örneğin, aygıt LED'in yanıp söndüğünü bildiriyor.                                    |
| İkiz etiketler                  | Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir otomatın dağıtım konumu.                         |
| Aygıt ikiz sorguları        | Bu ikizleri, kullanıma hazır aygıtları tanımlamak gibi rasgele koşullarla almak için tüm aygıt ikizlerini sorgulayın. |

Bu seçenekleri kullanma konusundaki farklar ve kılavuzlar hakkında daha ayrıntılı açıklama için [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) ve [Buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md)bakın.

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IoT Hub, bağlanan her aygıt için bir aygıt ikizini kalıcı hale getirir. Cihaz ikizleri hakkında daha fazla bilgi için [bkz.](iot-hub-node-node-twin-getstarted.md)

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleriyle Azure CLI için IoT uzantısını kullanmayı öğrenirsiniz.

## <a name="what-you-do"></a>Ne yaparsınız

Azure CLI ve Azure CLI için IoT uzantısını çeşitli yönetim seçenekleriyle çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

* [Raspberry Pi online simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya cihaz öğreticiler birini tamamlayın; örneğin, [ahududu Pi düğüm ile.js](iot-hub-raspberry-pi-kit-node-get-started.md). Bu öğeler aşağıdaki gereksinimleri kapsar:

  - Etkin bir Azure aboneliği.
  - Aboneliğiniz altında bir Azure IoT hub'ı.
  - Azure IoT hub'ınıza ileti gönderen bir istemci uygulaması.

* Bu eğitim sırasında cihazınızın istemci uygulamasıyla birlikte çalıştığını unutmayın.

* [Python 2.7x veya Python 3.x](https://www.python.org/downloads/)

* The Azure CLI. Yüklemeniz gerekiyorsa, Azure [CLI'yi yükleyin'](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)e bakın. Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az –version` kullanın.

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

* IoT uzantısını yükleyin. En basit yol `az extension add --name azure-iot` komutunu çalıştırmaktır. [IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar.

## <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

```azurecli
az login
```

## <a name="direct-methods"></a>Doğrudan yöntemler

```azurecli
az iot hub invoke-device-method --device-id <your device id> \
  --hub-name <your hub name> \
  --method-name <the method name> \
  --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Cihaz ikiz istenilen özellikleri

Aşağıdaki komutu çalıştırarak istenilen özellik aralığı = 3000 ayarlayın:

```azurecli
az iot hub device-twin update -n <your hub name> \
  -d <your device id> --set properties.desired.interval = 3000
```

Bu özellik cihazınızdan okunabilir.

## <a name="device-twin-reported-properties"></a>Aygıt ikizi bildirilen özellikler

Aşağıdaki komutu çalıştırarak aygıtın bildirilen özelliklerini alın:

```azurecli
az iot hub device-twin show -n <your hub name> -d <your device id>
```

Bildirilen iki çift özelliklerden biri, cihaz uygulamasının bildirilen özellik kümesini en son ne zaman güncelleştiren $metadata,$lastUpdated'dir.

## <a name="device-twin-tags"></a>Aygıt ikiz etiketleri

Aşağıdaki komutu çalıştırarak aygıtın etiketlerini ve özelliklerini görüntüleyin:

```azurecli
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Aşağıdaki komutu çalıştırarak cihaza bir alan rolü = sıcaklık&nem ekleyin:

```azurecli
az iot hub device-twin update \
  --hub-name <your hub name> \
  --device-id <your device id> \
  --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Aygıt ikiz sorguları

Rol etiketi = 'sıcaklık&nem' etiketine sahip aygıtları aşağıdaki komutu çalıştırarak sorgulayın:

```azurecli
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Rol etiketi = 'sıcaklık&nem' etiketine sahip olanlar dışındaki tüm cihazları aşağıdaki komutu çalıştırarak sorgulayın:

```azurecli
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Sonraki adımlar

IoT aygıtınızla Azure IoT Hub'ınız arasında aygıttan buluta iletileri izlemeyi ve buluttan cihaza iletigöndermeyi öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
