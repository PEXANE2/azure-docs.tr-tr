---
title: IoT Tak ve Çalıştır Preview örnek cihaz kodunu IoT Hub'ına bağlayın | Microsoft Dokümanlar
description: Java'yı kullanarak, bir IoT hub'ına bağlanan IoT Tak ve Çalıştır Preview örnek aygıt kodu oluşturun ve çalıştırın. Aygıt tarafından hub'a gönderilen bilgileri görüntülemek için Azure IoT gezgini aracını kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: aa676dd374eccf2a4b5c4622689ed402c8679e5a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76964844"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-java"></a>Quickstart: Örnek bir IoT Tak ve Çalıştır Önizleme aygıtı uygulamasını IoT Hub'ına (Java) bağlayın

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıç, örnek bir IoT Tak ve Çalıştır aygıtı uygulamasını nasıl oluşturabileceğinizi, IoT hub'ınıza nasıl bağlayabileceğinizi ve hub'a gönderdiği bilgileri görüntülemek için Azure IoT gezgini aracını nasıl kullanacağınızı gösterir. Örnek uygulama Java'da yazılır ve Java koleksiyonu için Azure IoT Örnekleri'nin bir parçası olarak sağlanır. Bir çözüm geliştiricisi, herhangi bir aygıt kodunu görüntülemeye gerek kalmadan bir IoT Tak ve Çalıştır aygıtının özelliklerini anlamak için Azure IoT explorer aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için geliştirme makinenizde Java SE 8'e ihtiyacınız vardır. Ayrıca Maven 3 yüklemeniz gerekir.

Bunlarla nasıl ayarlanacağınız hakkında ayrıntılı bilgi için, geliştirme ortamınızı Java için Microsoft Azure IoT aygıtı SDK'da [hazırlayın'](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md) a bakın.

### <a name="install-the-azure-iot-explorer"></a>Azure IoT gezginini yükleme

En son güncelleştirme için "Varlıklar" altında .msi dosyasını seçerek aracın [depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT explorer'ın** en son sürümünden indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Java için Azure IoT Örneklerini klonlamak ve oluşturmak için kullanabileceğiniz bir geliştirme ortamı hazırlayın.

Seçtiğiniz dizinde bir terminal penceresi açın. Java GitHub deposu [için Azure IoT Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-java) bu konuma klonlamak için aşağıdaki komutu uygulayın:

```cmd/sh
git clone https://github.com/Azure-Samples/azure-iot-samples-java
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="build-the-code"></a>Kodu oluşturma

Bir IoT hub'ına bağlanan bir aygıtı taklit eden bir uygulama oluşturmak için klonlanmış örnek kodunu kullanırsınız. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. Yerel bir terminal penceresinde, klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-java/digital-twin/Samples/device/JdkSample** klasörüne gidin. Ardından, gerekli kitaplıkları yüklemek ve benzetimli aygıt uygulamasını oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. _Aygıt bağlantı dizesini_yapılandır:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

## <a name="run-the-device-sample"></a>Aygıt örneğini çalıştırma

IoT hub'ınıza telemetri gönderen bir IoT Tak ve Çalıştır aygıtını simüle etmek için örnek bir uygulama çalıştırın. Örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

```cmd\sh
java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
```

Aygıtın bağlı olduğunu, çeşitli kurulum adımlarını gerçekleştirdiğini ve servis güncelleştirmelerini beklediğini ve ardından telemetri günlüklerini beklediğini belirten iletiler görürsünüz. Bu, aygıtın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub'a telemetri verilerini göndermeye başladığını gösterir. Sonraki adımları tamamlarken numunenin çalışmasını engelleyin.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanma

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın arabirim modeli tanımlarını cihazınızdan okuyabilmesini sağlamak için **Ayarlar'ı**seçin. Ayarlar menüsünde, **bağlı cihazda** Tak ve Çalıştır yapılandırmalarında zaten görünebilir; yoksa, **+ Ekle modül tanım kaynağını** seçin ve sonra eklemek için bağlı **aygıtta.**

1. **Cihazlara** genel bakış sayfasında, daha önce oluşturduğunuz aygıt kimliğini bulun. Aygıt uygulaması hala komut isteminde çalışırken, Aygıtın Azure IoT gezginindeki **Bağlantı durumunun** _Bağlı_ olarak bildirdiğini kontrol edin (değilse, yenilenene kadar **Yenile'ye** vurun). Daha fazla ayrıntı görüntülemek için aygıtı seçin.

1. IKimlik **vazosu:java_sdk_sample:EnvironmentalSensor:1** ile arabirimi genişleterek arayüzü ve IoT Plug and Play primitives-properties, commands ve telemetriyi ortaya çıkarın.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını bir IoT hub'ına nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanma ve etkileşimde](howto-develop-solution.md)
