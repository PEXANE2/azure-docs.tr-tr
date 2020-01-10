---
title: IoT Tak ve Kullan Preview örnek cihaz kodunu IoT Hub 'ye bağlayın | Microsoft Docs
description: Node. js kullanarak IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: baanders
ms.author: baanders
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 64f478d9d5c3330167df81de9766ff02eb943c98
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75531301"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-nodejs"></a>Hızlı başlangıç: örnek bir IoT Tak ve Kullan Preview cihaz uygulamasını IoT Hub 'ye bağlama (node. js)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıçta, örnek bir IoT Tak ve Kullan cihaz uygulamasının nasıl oluşturulacağı, IoT Hub 'ınıza nasıl bağlanacağı ve hub 'a gönderdiği bilgileri görüntülemek için Azure IoT gezgin aracının nasıl kullanılacağı gösterilmektedir. Örnek uygulama, Node. js için yazılır ve Node. js için Azure IoT Hub cihaz SDK 'sına dahildir. Bir çözüm geliştiricisi, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Node. js gerekir. [NodeJS.org](https://nodejs.org)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

### <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

En son güncelleştirme için "varlıklar" altında. msi dosyasını seçerek aracın [Depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT Explorer** 'ın en son sürümünü indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Node. js için Azure IoT Hub cihaz SDK 'sını klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. [Node. js GitHub deposu için Microsoft Azure ıOT SDK 'sını](https://github.com/Azure/azure-iot-sdk-node) bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node --recursive -b digitaltwins-preview
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="install-required-libraries"></a>Gerekli kitaplıkları yükler

Dahil edilen örnek kodu oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan bir cihaza benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, kopyalanmış deponuzdaki klasöre gidin ve **/Azure-iot-SDK-node/digitaltwins/Samples/Device/JavaScript** klasörüne gidin. Ardından gerekli kitaplıkları yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install
    ```
1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

IoT Hub 'ınıza telemetri gönderen bir IoT Tak ve Kullan cihazının benzetimini yapmak için SDK 'da örnek bir uygulama çalıştırın. Örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

```cmd\sh
    node sample_device.js
```

Cihazın, telemetri verilerini hub 'a göndermeyi başladığını ve artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu belirten aşağıdaki çıktıyı görürsünüz.

   ![Cihaz onay iletileri](media/quickstart-connect-pnp-device/device-confirmation-node.png)

 Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanın

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın, cihazınızdan arabirim modeli tanımlarını okuyacağına emin olmak için **Ayarlar**' ı seçin. Ayarlar menüsünde, **bağlı cihazdaki** Tak ve kullan yapılandırmalarda zaten görünebilir. değilse, **+ modül tanımı kaynağı Ekle** ' yi seçin ve ardından **bağlı cihazda** ekleyin.

1. **Cihazların** genel bakış sayfasına geri döndüğünüzde, daha önce oluşturduğunuz cihaz kimliğini bulun. Cihaz uygulaması komut isteminde çalışmaya devam ettiğinden, cihazın Azure IoT Explorer 'daki **bağlantı durumunun** _bağlı_ olarak raporlandığından emin olun (Aksi takdirde, olana kadar **Yenile** ' ye basın). Daha fazla ayrıntı görüntülemek için cihazı seçin.

1. Arabirim ve IoT Tak ve Kullan temel elemanlar — özellikler, komutlar ve telemetri ' i göstermek için ID **urn: contoso: com: Environmentalalgılayıcı: 1** olan arabirimi genişletin.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: IoT Tak ve Kullan önizleme cihazına bağlanma ve bunlarla etkileşim kurma](howto-develop-solution.md)
