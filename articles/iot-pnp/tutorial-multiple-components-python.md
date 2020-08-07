---
title: IoT Tak ve Kullan Preview örnek Python bileşeni Cihaz kodunu IoT Hub 'ye bağlayın | Microsoft Docs
description: Birden çok bileşen kullanan ve IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek Python cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 571f0e0ceff0adfbf1814abc627fcab6b23acbe1
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905865"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-python"></a>Öğretici: bir örnek IoT Tak ve Kullan önizleme birden çok bileşen cihaz uygulamasını IoT Hub 'ye bağlama (Python)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Bu öğreticide, bileşenler ve kök arabirimiyle örnek IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT gezgin aracını kullanarak hub 'a gönderdiği bilgileri görüntüleme hakkında bilgi verilmektedir. Örnek uygulama Python 'da yazılmıştır ve Python için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için geliştirme makinenizde Python 3,7 gerekir. [Python.org](https://www.python.org/)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz. Python sürümünüzü aşağıdaki komutla kontrol edebilirsiniz:  

```cmd/sh
python --version
```

[Python.org](https://www.python.org/)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

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

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Bu paket, genel önizleme yenilemesi için bir PıP olarak yayımlandı. Paket sürümü en son veya`2.1.4`

Yerel Python ortamınızda dosyayı şu şekilde yüklemelisiniz:

```cmd/sh
pip install azure-iot-device
```

Python SDK IoT deposunu kopyalayın ve **pnp-önizleme-Yenile**' ye göz atın:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu örnek, IoT Tak ve Kullan ısı denetleyicisi cihazı uygular. Bu örneğin uyguladığı model [birden çok bileşen](concepts-components.md)kullanır. [Sıcaklık cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) , cihazın uyguladığı telemetri, Özellikler ve komutları tanımlar.

*Azure-iot-SDK-python\azure-iot-device\samples\pnp* klasörü, IoT Tak ve kullan cihazının örnek kodunu içerir. Sıcaklık denetleyicisi örneği dosyaları şunlardır:

- pnp_temp_controller_with_thermostats. Kopyala
- pnp_helper_preview_refresh. Kopyala

Sıcaklık denetleyicisi, sıcaklık denetleyicisi DTDL modeline bağlı olarak birden çok bileşene ve bir kök arabirime sahiptir.

*Pnp_temp_controller_with_thermostats. Kopyala* dosyasını istediğiniz düzenleyicide açın. Bu dosyadaki kod:

1. `pnp_helper_preview_refresh.py`Yardımcı yöntemlere erişim sağlamak için içeri aktarır.

2. DTDL modelinde tanımlanan iki farklı arabirimi benzersiz olarak temsil eden iki dijital ikizi model tanımlayıcısını (Dtmıs) tanımlar. Gerçek bir sıcaklık denetleyicisindeki bileşenlerin bu iki arabirimi uygulaması gerekir. Bu iki arabirim zaten merkezi bir depoda yayımlanmış. Bu Dtmıs, Kullanıcı tarafından bilinmelidir ve cihaz uygulamasının senaryosuna bağlı olarak değişiklik gösterir. Geçerli örnek için, bu iki arabirim şunları temsil eder:

  - Bir termostat
  - Azure tarafından geliştirilen cihaz bilgileri.

3. `model_id`Uygulanan cihaz IÇIN DTMı 'yi tanımlar. DTMı Kullanıcı tanımlı ve DTDL model dosyasındaki DTMı ile eşleşmelidir.

4. DTDL dosyasındaki bileşenlere verilen adları tanımlar. DTDL ve bir cihaz bilgileri bileşeninde iki adet termostats vardır. Adlı bir sabit `serial_number` , kök arabirimde da tanımlanır. Bir `serial_number` cihaz için değişiklik yapamıyor.

5. Komut işleyici uygulamalarını tanımlar. Bu, cihazın komut istekleri aldığında ne yaptığını tanımlar.

6. Komut yanıtı oluşturmak için işlevleri tanımlar. Bu, cihazın komut istekleri ile nasıl yanıt vereceğini tanımlar. Bir komutun IoT Hub 'ına özel bir yanıt gönderebilmesi gerekiyorsa komut yanıt işlevleri oluşturursunuz. Bir komut için bir yanıt işlevi sağlanmazsa, genel bir yanıt gönderilir. Bu örnekte, yalnızca **Getmaxminreport** komutunda özel bir yanıt vardır.

7. Bu cihazdan telemetri göndermek için bir işlev tanımlar. Hem termostats hem de kök arabirim telemetri gönderir. Bu işlev, bir bileşenin Telemetriyi gönderdiğini belirlemesini sağlamak için isteğe bağlı bir bileşen adı parametresi alır.

8. Komut istekleri için bir dinleyici tanımlar.

9. İstenen özellik güncelleştirmeleri için bir dinleyici tanımlar.

10. Şu `main` şekilde bir işleve sahiptir:

    1. Bir cihaz istemcisi oluşturmak ve IoT Hub 'ınıza bağlanmak için cihaz SDK 'sını kullanır. Cihaz, `model_id` IoT Hub 'ın cihazı ıot Tak ve Kullan cihazı olarak tanımlayabilmesi için gönderir.

    1. `create_reported_properties`Özellikleri oluşturmak için yardımcı dosyadaki işlevini kullanır. Bileşen adını ve özelliklerini bu işleve anahtar değer çiftleri olarak geçirin.

    1. Çağırarak bileşenlerinin okunabilir özelliklerini güncelleştirir `patch_twin_reported_properties` .

    1. İşlevini kullanarak komut isteklerini dinlemeye başlar `execute_command_listener` . İşlevi, hizmetten gelen komut istekleri için bir dinleyici ayarlar. Dinleyiciyi ayarlarken `method_name` , `user_command_handler` ve isteğe bağlı `create_user_response_handler` olarak parametre olarak belirtin.
        - `method_name`Komut isteğini tanımlar. Bu örnekte model, **yeniden başlatma**komutları ve **Getmaxminreport**' ı tanımlar.
        - `user_command_handler`İşlevi, bir komut aldığında cihazın ne yapması gerektiğini tanımlar.
        - `create_user_response_handler`İşlevi, bir komut başarıyla yürütüldüğünde IoT Hub 'ınıza gönderilecek bir yanıt oluşturur. Bu yanıtı portalda görüntüleyebilirsiniz. Bu işlev sağlanmazsa, hizmete genel bir yanıt gönderilir.

    1. `execute_property_listener`Özellik güncelleştirmelerini dinlemek için kullanır.

    1. Kullanarak telemetri göndermeye başlar `send_telemetry` . Örnek kod, üç telemetri gönderme işlevini çağırmak için bir döngü kullanır. Her biri sekiz saniyede bir çağrılır

    1. Tüm dinleyicileri ve görevleri devre dışı bırakır ve **q** veya **q**tuşlarına bastığınızda döngüden çıkar.

Kodu gördüğünüze göre, daha önce bir nota yaptığınız cihaz bağlantı dizesini depolamak için **IOTHUB_DEVICE_CONNECTION_STRING** adlı bir ortam değişkeni oluşturun. Örneği çalıştırmak için aşağıdaki komutu kullanın:

```cmd/sh
python pnp_temp_controller_with_thermostats.py
```

Örnek cihaz, IoT Hub 'ınıza her beş saniyede bir telemetri iletisi gönderir.

Aygıtın telemetri verilerini hub 'a gönderdiğini ve artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu gösteren aşağıdaki çıktıyı görürsünüz.

![Cihaz onay iletileri](media/tutorial-multiple-components-python/multiple-component.png)

Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Hub 'ına bileşenlerle IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
