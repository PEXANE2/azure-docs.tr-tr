---
title: IoT Tak ve Kullan Preview örnek cihaz kodunu IoT Hub 'ye bağlayın | Microsoft Docs
description: Java kullanarak IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: aa676dd374eccf2a4b5c4622689ed402c8679e5a
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964844"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-java"></a>Hızlı başlangıç: örnek bir IoT Tak ve Kullan Preview cihaz uygulamasını IoT Hub 'a bağlama (Java)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıçta, örnek bir IoT Tak ve Kullan cihaz uygulamasının nasıl oluşturulacağı, IoT Hub 'ınıza nasıl bağlanacağı ve hub 'a gönderdiği bilgileri görüntülemek için Azure IoT gezgin aracının nasıl kullanılacağı gösterilmektedir. Örnek uygulama Java 'da yazılmıştır ve Java için Azure IoT örnekleri koleksiyonunun bir parçası olarak sağlanır. Bir çözüm geliştiricisi, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Java, 8 gerekir. Ayrıca Maven 3 ' ü yüklemeniz gerekir.

Bunlarla nasıl ayarlanacağına ilişkin ayrıntılar için bkz. Java için Microsoft Azure IoT cihaz SDK 'sında [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md) .

### <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

En son güncelleştirme için "varlıklar" altında. msi dosyasını seçerek aracın [Depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT Explorer** 'ın en son sürümünü indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Java için Azure IoT örneklerini klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir Terminal penceresi açın. Java GitHub deposunun [Azure IoT örneklerini](https://github.com/Azure-Samples/azure-iot-samples-java) bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure-Samples/azure-iot-samples-java
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="build-the-code"></a>Kodu oluşturma

Bir IoT Hub 'ına bağlanan bir cihaza benzetim yapan bir uygulama oluşturmak için klonlanan örnek kodu kullanırsınız. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, kopyalanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-Java/Digital-Twin/Samples/Device/JdkSample** klasörüne gidin. Ardından, gerekli kitaplıkları yüklemek ve sanal cihaz uygulamasını derlemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

IoT Hub 'ınıza telemetri gönderen bir IoT Tak ve Kullan cihazının benzetimini yapmak için örnek bir uygulama çalıştırın. Örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

```cmd\sh
java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
```

Cihazın bağlı olduğunu, çeşitli kurulum adımları gerçekleştirdiğini ve hizmet güncelleştirmelerini beklediğini, ardından telemetri günlüklerini bildiren iletileri görürsünüz. Bu, cihazın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub 'a telemetri verileri göndermeye başlamış olduğunu gösterir. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanın

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın, cihazınızdan arabirim modeli tanımlarını okuyacağına emin olmak için **Ayarlar**' ı seçin. Ayarlar menüsünde, **bağlı cihazdaki** Tak ve kullan yapılandırmalarda zaten görünebilir. değilse, **+ modül tanımı kaynağı Ekle** ' yi seçin ve ardından **bağlı cihazda** ekleyin.

1. **Cihazların** genel bakış sayfasına geri döndüğünüzde, daha önce oluşturduğunuz cihaz kimliğini bulun. Cihaz uygulaması komut isteminde çalışmaya devam ettiğinden, cihazın Azure IoT Explorer 'daki **bağlantı durumunun** _bağlı_ olarak raporlandığından emin olun (Aksi takdirde, olana kadar **Yenile** ' ye basın). Daha fazla ayrıntı görüntülemek için cihazı seçin.

1. Arabirim ve IoT Tak ve Kullan temelleri (özellikler, komutlar ve telemetri) göstermek için ID **urn: java_sdk_sample: Environmentalalgılayıcı: 1** olan arabirimi genişletin.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: IoT Tak ve Kullan önizleme cihazına bağlanma ve bunlarla etkileşim kurma](howto-develop-solution.md)
