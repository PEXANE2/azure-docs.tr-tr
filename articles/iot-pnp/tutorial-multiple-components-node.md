---
title: Öğretici-IoT Tak ve Kullan örnek Node.js bileşen cihazı kodunu Azure 'a bağlama IoT Hub | Microsoft Docs
description: Öğretici-birden çok bileşen kullanan ve IoT Hub 'ına bağlanan IoT Tak ve Kullan örnek Node.js cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: olivakar
ms.author: olkar
ms.date: 07/10/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.custom: devx-track-js
ms.openlocfilehash: 70fa10f5319de072ee8ea7dad4e73c58d4bdeede
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421439"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-multiple-component-device-application-to-iot-hub-nodejs"></a>Öğretici: bir örnek IoT Tak ve Kullan birden çok bileşen cihaz uygulamasını IoT Hub 'ye bağlama (Node.js)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Bu öğreticide, bileşenleriyle bir örnek IoT Tak ve Kullan cihaz uygulaması oluşturma, bunları IoT Hub 'ınıza bağlama ve Azure IoT Gezgini aracını kullanarak hub 'a gönderdiği bilgileri görüntüleme hakkında bilgi verilmektedir. Örnek uygulama Node.js için yazılmıştır ve Node.js için Azure IoT Hub cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu öğreticiyi tamamlayabilmeniz için geliştirme makinenizde Node.js gerekir. [NodeJS.org](https://nodejs.org)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

## <a name="download-the-code"></a>Kodu indirme

[Hızlı başlangıç: Windows üzerinde çalışan bir örnek ıot Tak ve kullan cihaz uygulamasını IoT Hub (node) olarak bağladığınızda](quickstart-connect-device-node.md), depoyu zaten Klonladığınız.

Seçtiğiniz dizinde bir komut istemi açın. Node.jsGitHub deposunun [Microsoft Azure ıOT SDK 'sını ](https://github.com/Azure/azure-iot-sdk-node) bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

## <a name="install-required-libraries"></a>Gerekli kitaplıkları yükler

Dahil edilen örnek kodu oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan birden çok bileşeni olan bir Tak ve Kullan cihazına benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, kopyalanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-node/Device/Samples/PNP* klasörüne gidin. Ardından gerekli kitaplıkları yüklemek için aşağıdaki komutu çalıştırın:

```cmd/sh
npm install
```

Bu işlem, örnekleri klasöründe çalıştırmak için gereken ilgili NPM dosyalarını yükler.

## <a name="review-the-code"></a>Kodu gözden geçirin

*Azure-iot-SDK-node\device\samples\pnp* klasörüne gidin.

*Azure-iot-SDK-node\device\samples\pnp* klasörü, IoT Tak ve kullan sıcaklık denetleyicisi cihazının örnek kodunu içerir.

*pnpTemperatureController.js* dosyasındaki kod, bir IoT Tak ve kullan sıcaklık denetleyicisi cihazı uygular. Bu örneğin uyguladığı model [birden çok bileşen](concepts-components.md)kullanır. [Sıcaklık cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) , cihazın uyguladığı telemetri, Özellikler ve komutları tanımlar.

*pnpTemperatureController.js* dosyasını seçtiğiniz bir kod düzenleyicisinde açın. Örnek kod, nasıl yapılacağını göstermektedir:

- `modelId`Uygulanan cihaz IÇIN DTMI olduğunu tanımlayın. Bu DTMı Kullanıcı tanımlı ve [sıcaklık denetleyicisi DTDL modelinin](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)dtmı ile eşleşmelidir.

- Sıcaklık denetleyicisi DTDL modelinde tanımlanan bileşenleri uygulayın. Gerçek bir sıcaklık denetleyicisindeki bileşenlerin bu iki arabirimi uygulaması gerekir. Bu iki arabirim zaten merkezi bir depoda yayımlanmış. Bu örnekte, iki arabirim şunlardır:

  - Saatiniz
  - Azure tarafından geliştirilen cihaz bilgileri

- Bileşen adlarını tanımlayın. Bu örnekte iki adet termostats ve bir cihaz bilgileri bileşeni bulunur.

- Komut adını tanımlayın. Bunlar, cihazın yanıt verdiği komutlardır.

- Sabiti tanımlayın `serialNumber` . , `serialNumber` Belirli bir cihaz sabitlenmiştir.

- Komut işleyicilerini tanımlayın.

- Komut yanıtlarının gönderileceği işlevleri tanımlayın.

- Komut isteklerini günlüğe kaydetmek için yardımcı işlevleri tanımlayın.

- Özellikleri oluşturmak için bir yardımcı işlev tanımlayın.

- Özellik güncelleştirmeleri için bir dinleyici tanımlayın.

- Bu cihazdan telemetri göndermek için bir işlev tanımlayın. Hem termostats hem de varsayılan bileşen telemetri gönderir. Bu işlev, bileşen adını parametre olarak alır.

- Şu `main` şekilde bir işlev tanımlayın:

  - Bir cihaz istemcisi oluşturmak ve IoT Hub 'ınıza bağlanmak için cihaz SDK 'sını kullanır. Cihaz, `modelId` IoT Hub cihazı ıot Tak ve Kullan cihazı olarak tanımlayabilmesi için sağlar.

  - İşlevini kullanarak komut isteklerini dinlemeye başlar `onDeviceMethod` . İşlevi, hizmetten gelen komut istekleri için bir dinleyici ayarlar:

    - DTDL cihaz, `reboot` ve komutlarını tanımlar `getMaxMinReport` .
    - `commandHandler`İşlevi, cihazın bir komuta nasıl yanıt verdiğini tanımlar.

  - Ve kullanarak telemetri göndermeye başlar `setInterval` `sendTelemetry` .

  - `helperCreateReportedPropertiesPatch`Özelliklerini oluşturmak için işlevini ve özelliklerini güncelleştirmek için öğesini kullanır `updateComponentReportedProperties` .

  - `desiredPropertyPatchListener`Özellik güncelleştirmelerini dinlemek için kullanır.

  - Tüm dinleyicileri ve görevleri devre dışı bırakır ve **q** veya **q** tuşlarına bastığınızda döngüden çıkar.

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Örnek yapılandırma hakkında daha fazla bilgi edinmek için bkz. [örnek Readme](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/pnp/readme.md).

Artık kodu gördüğünüze göre, örneği çalıştırmak için aşağıdaki komutu kullanın:

```cmd\sh
node pnpTemperatureController.js
```

Cihazın, telemetri verilerini hub 'a göndermeyi başladığını ve artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu belirten aşağıdaki çıktıyı görürsünüz.

![Cihaz onay iletileri](media/tutorial-multiple-components-node/multiple-component.png)

Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Hub 'ına bileşenlerle IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan modelleme Geliştirici Kılavuzu](concepts-developer-guide-device-csharp.md)
