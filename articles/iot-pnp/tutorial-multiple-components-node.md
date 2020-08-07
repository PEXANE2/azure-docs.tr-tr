---
title: IoT Tak ve Kullan önizleme örneği Node.js bileşen cihazı kodunu IoT Hub bağlayın | Microsoft Docs
description: Birden çok bileşen kullanan ve IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek Node.js cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: olivakar
ms.author: olkar
ms.date: 07/10/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.custom: devx-track-javascript
ms.openlocfilehash: d26179ab82f29ce8f937f5b444463c1308d92047
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904114"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-nodejs"></a>Öğretici: bir örnek IoT Tak ve Kullan önizleme birden çok bileşen cihaz uygulamasını IoT Hub 'ye bağlama (Node.js)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Bu öğreticide, bileşenler ve kök arabirimiyle örnek IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT gezgin aracını kullanarak hub 'a gönderdiği bilgileri görüntüleme hakkında bilgi verilmektedir. Örnek uygulama Node.js için yazılmıştır ve Node.js için Azure IoT Hub cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için geliştirme makinenizde Node.js gerekir. [NodeJS.org](https://nodejs.org)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

### <a name="azure-iot-explorer"></a>Azure IoT Gezgini

Bu öğreticinin ikinci bölümünde örnek cihazla etkileşime geçmek için **Azure IoT gezgin** aracını kullanın. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) .

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu öğreticide kullanacaksınız.

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> IoT Hub bağlantı dizesini bulmak için Azure IoT gezgin aracını da kullanabilirsiniz.

Hub 'a eklediğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu öğreticide kullanacaksınız.

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Kodu indirme

Bu öğreticide, Node.js için Azure IoT Hub cihaz SDK 'sını klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. Node.jsGitHub deposunun [Microsoft Azure ıOT SDK 'sını](https://github.com/Azure/azure-iot-sdk-node) bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="install-required-libraries"></a>Gerekli kitaplıkları yükler

Dahil edilen örnek kodu oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama, bir IoT Hub 'ına bağlanan birden çok bileşeni ve kök arabirimini içeren bir Tak ve Kullan cihazına benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, kopyalanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-node/Device/Samples/PNP* klasörüne gidin. Ardından gerekli kitaplıkları yüklemek için aşağıdaki komutu çalıştırın:

```cmd/sh
npm install
```

Bu işlem, örnekleri klasöründe çalıştırmak için gereken ilgili NPM dosyalarını yükler.

1. Ortam değişkenini, daha önce bir değişiklik yaptığınız cihaz bağlantı dizesiyle yapılandırın:

```cmd/sh
set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
```

## <a name="review-the-code"></a>Kodu gözden geçirin

*Azure-iot-SDK-node\device\samples\pnp* klasörüne gidin.

*Azure-iot-SDK-node\device\samples\pnp* klasörü, IoT Tak ve kullan sıcaklık denetleyicisi cihazının örnek kodunu içerir.

*pnpTemperatureController.js* dosyasındaki kod, bir IoT Tak ve kullan sıcaklık denetleyicisi cihazı uygular. Bu örneğin uyguladığı model [birden çok bileşen](concepts-components.md)kullanır. [Sıcaklık cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) , cihazın uyguladığı telemetri, Özellikler ve komutları tanımlar.

*pnpTemperatureController.js* dosyasını seçtiğiniz bir kod düzenleyicisinde açın. Örnek kod, nasıl yapılacağını göstermektedir:

1. `modelId`Uygulanan cihaz IÇIN DTMI olduğunu tanımlayın. Bu DTMı Kullanıcı tanımlı ve [sıcaklık denetleyicisi DTDL modelinin](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)dtmı ile eşleşmelidir.

2. Sıcaklık denetleyicisi DTDL modelinde tanımlanan bileşenleri uygulayın. Gerçek bir sıcaklık denetleyicisindeki bileşenlerin bu iki arabirimi uygulaması gerekir. Bu iki arabirim zaten merkezi bir depoda yayımlanmış. Bu örnekte, iki arabirim şunlardır:
  - Saatiniz
  - Azure tarafından geliştirilen cihaz bilgileri

3. Bileşen adlarını tanımlayın. Bu örnekte iki adet termostats ve bir cihaz bilgileri bileşeni bulunur.

4. Komut adını tanımlayın. Bunlar, cihazın yanıt verdiği komutlardır.

5. Sabiti tanımlayın `serialNumber` . , `serialNumber` Belirli bir cihaz sabitlenmiştir.

6. Komut işleyicilerini tanımlayın.

7. Komut yanıtlarının gönderileceği işlevleri tanımlayın.

8. Komut isteklerini günlüğe kaydetmek için yardımcı işlevleri tanımlayın.

9. Özellikleri oluşturmak için bir yardımcı işlev tanımlayın.

10. Özellik güncelleştirmeleri için bir dinleyici tanımlayın.

11. Bu cihazdan telemetri göndermek için bir işlev tanımlayın. Hem termostats hem de kök bileşen telemetri gönderir. Bu işlev, bileşen adını parametre olarak alır.

12. Şu `main` şekilde bir işlev tanımlayın:

    1. Bir cihaz istemcisi oluşturmak ve IoT Hub 'ınıza bağlanmak için cihaz SDK 'sını kullanır. Cihaz, `modelId` IoT Hub cihazı ıot Tak ve Kullan cihazı olarak tanımlayabilmesi için sağlar.

    1. İşlevini kullanarak komut isteklerini dinlemeye başlar `onDeviceMethod` . İşlevi, hizmetten gelen komut istekleri için bir dinleyici ayarlar:
        - DTDL cihaz, `reboot` ve komutlarını tanımlar `getMaxMinReport` .
        - `commandHandler`İşlevi, cihazın bir komuta nasıl yanıt verdiğini tanımlar.

    1. Ve kullanarak telemetri göndermeye başlar `setInterval` `sendTelemetry` .

    1. `helperCreateReportedPropertiesPatch`Özelliklerini oluşturmak için işlevini ve özelliklerini güncelleştirmek için öğesini kullanır `updateComponentReportedProperties` .

    1. `desiredPropertyPatchListener`Özellik güncelleştirmelerini dinlemek için kullanır.

    1. Tüm dinleyicileri ve görevleri devre dışı bırakır ve **q** veya **q**tuşlarına bastığınızda döngüden çıkar.

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
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
