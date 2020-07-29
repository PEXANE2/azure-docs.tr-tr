---
title: IoT Tak ve Kullan Preview örnek Python Cihaz kodunu Azure IoT Hub bağlayın | Microsoft Docs
description: IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek cihaz kodunu derlemek ve çalıştırmak için Python 'u kullanın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 84ef7ff18c294097da20640c1de237b41900cb40
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352986"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-python"></a>Hızlı başlangıç: örnek bir IoT Tak ve Kullan Preview cihaz uygulamasını IoT Hub 'ye bağlama (Python)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

Bu hızlı başlangıçta örnek bir IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT Explorer aracını kullanarak gönderdiği Telemetriyi görüntüleme gösterilmektedir. Örnek uygulama Python için yazılmıştır ve Python için Azure IoT Hub cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Python 3,7 gerekir. [Python.org](https://www.python.org/)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz. Python sürümünüzü aşağıdaki komutla kontrol edebilirsiniz:  

```cmd/sh
python --version
```

### <a name="azure-iot-explorer"></a>Azure IoT Gezgini

Bu hızlı başlangıç bölümünün ikinci bölümünde örnek cihazla etkileşime geçmek için **Azure IoT gezgin** aracını kullanın. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) .

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> IoT Hub bağlantı dizesini bulmak için Azure IoT gezgin aracını da kullanabilirsiniz.

Hub 'a eklediğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Bu paket, genel önizleme yenilemesi için bir PıP olarak yayımlandı. Paket sürümü en son veya`2.1.4`

Yerel Python ortamınızda dosyayı şu şekilde yüklemelisiniz:

```cmd/sh
pip install azure-iot-device
```

Python SDK IoT deposunu kopyalayın ve **asıl**kullanıma çekin:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

*Azure-iot-SDK-python\azure-iot-device\samples\pnp* klasörü, IoT Tak ve kullan cihazının örnek kodunu içerir. Bu hızlı başlangıçta *pnp_thermostat. Kopyala* dosyası kullanılmaktadır. Bu örnek kod, IoT Tak ve Kullan uyumlu bir cihaz uygular ve Azure IoT Python cihaz Istemci kitaplığını kullanır.

Daha önce bir nota yaptığınız cihaz bağlantı dizesini depolamak için **IOTHUB_DEVICE_CONNECTION_STRING** adlı bir ortam değişkeni oluşturun.

**Pnp_thermostat. Kopyala** dosyasını bir metin düzenleyicisinde açın. Nasıl yapıldığını fark edin:

1. [Termostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json)'yi benzersiz bir şekilde temsil eden tek bir cihaz ikizi model tanımlayıcısını (dtmı) tanımlar. Bir DTMı, Kullanıcı tarafından bilinmelidir ve cihaz uygulamasının senaryosuna bağlı olarak değişir. Geçerli örnek için model, telemetri, Özellikler ve izleme sıcaklığında ilişkili komutlara sahip bir termostat 'yi temsil eder.

1. Komut işleyici uygulamalarını tanımlamak için işlevlere sahiptir. Cihazın komut isteklerine nasıl yanıt verdiğini tanımlamak için bu işleyicileri yazarsınız.

1. Komut yanıtı tanımlamak için bir işleve sahiptir. IoT Hub 'ınıza yanıt göndermek için komut yanıt işlevleri oluşturursunuz.

1. Uygulamadan çıkmanızı sağlamak için bir giriş klavye dinleyicisi işlevi tanımlar.

1. **Ana** işleve sahiptir. **Main** işlevi:

    1. Bir cihaz istemcisi oluşturmak ve IoT Hub 'ınıza bağlanmak için cihaz SDK 'sını kullanır.

    1. Güncelleştirme özellikleri. Kullandığımız **model, termostat,** `targetTemperature` `maxTempSinceLastReboot` termostat için iki özelliği tanımlar, bu sayede, kullanacağız. Özellikler `patch_twin_reported_properties` , üzerinde tanımlanan yöntemi kullanılarak güncellenir `device_client` .

    1. **Execute_command_listener** işlevini kullanarak komut isteklerini dinlemeye başlar. İşlevi, hizmetten gelen komutları dinlemek için bir ' dinleyicisi ' kurar. ,, Ve sağlayan dinleyiciyi ayarlarken `method_name` `user_command_handler` `create_user_response_handler` . 
        - `user_command_handler`İşlevi, bir komut aldığında cihazın ne yapması gerektiğini tanımlar. Örneğin, alarmınız kapalıysa, bu komutu almanın etkisi uyanmaya başlar. Bunu, çağrılan komutun ' etkisi ' olarak düşünün.
        - `create_user_response_handler`İşlevi, bir komut başarıyla yürütüldüğünde IoT Hub 'ınıza gönderilecek bir yanıt oluşturur. Örneğin, alarmınız devre dışı bırakılırsa, hizmete geri bildirimde bulunmak üzere bir erteleme vererek yanıt alırsınız. Bunu, hizmete verdiğiniz yanıt olarak düşünün. Bu yanıtı portalda görüntüleyebilirsiniz.

    1. Telemetri göndermeye başlar. **Pnp_send_telemetry** , pnp_methods. Kopyala dosyasında tanımlanmıştır. Örnek kod, her sekiz saniyede bir bu işlevi çağırmak için bir döngü kullanır.

    1. Tüm dinleyicileri ve görevleri devre dışı bırakır ve **soru** **-cevap '** i tıklattığınızda döngüyü yok.

Artık kodu gördüğünüze göre, örneği çalıştırmak için aşağıdaki komutu kullanın:

```cmd/sh
python pnp_thermostat.py
```

Aygıtın telemetri verilerini hub 'a gönderdiğini ve şu anda komutları ve özellik güncelleştirmelerini almaya hazır olduğunu gösteren aşağıdaki çıktıyı görürsünüz:

```cmd/sh
Connecting using Connection String HostName=<your hub name>.azure-devices.net;DeviceId=<your device id>;SharedAccessKey=<your device shared access key>
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
```

Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşime geçin](quickstart-service-python.md)
