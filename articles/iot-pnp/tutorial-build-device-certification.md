---
title: Sertifika için Ready bir IoT Tak ve Kullan önizleme cihazı oluşturun | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, sertifika için HAZIRAN IoT Tak ve Kullan önizleme cihazını nasıl oluşturabileceğiniz hakkında bilgi edinin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 12/28/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: ce7d3ee8a0d05d837bc0049cba688cffe14d8a8c
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721651"
---
# <a name="build-an-iot-plug-and-play-preview-device-thats-ready-for-certification"></a>Sertifika için hazırlamış bir IoT Tak ve Kullan önizleme cihazı oluşturun

Bu öğreticide, bir cihaz geliştiricisi olarak, sertifika için kullanılabilecek bir IoT Tak ve Kullan önizleme cihazı nasıl oluşturabileceğiniz açıklanmaktadır.

Sertifika sınamaları şunları denetler:

- IoT Tak ve Kullan cihaz kodunuz cihazınıza yüklendi.
- IoT Tak ve Kullan cihaz kodunuz Azure IoT SDK ile oluşturulmuştur.
- Cihaz kodunuz [Azure IoT Hub cihaz sağlama hizmetini](../iot-dps/about-iot-dps.md)destekler.
- Cihaz kodunuz cihaz bilgileri arabirimini uygular.
- Yetenek modeli ve cihaz kodu IoT Central birlikte çalışır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Visual Studio Code](https://code.visualstudio.com/download)
- [Vs Code Uzantı paketi Için Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

Ayrıca, Windows için bir cihaz hızlı başlangıcı [oluşturmak üzere cihaz yetenek modeli kullanma](quickstart-create-pnp-device-windows.md) ' yı da doldurmanız gerekir. Hızlı başlangıç, Vcpkg kullanarak geliştirme ortamınızı ayarlamayı ve örnek bir proje oluşturmayı gösterir.

## <a name="store-a-capability-model-and-interfaces"></a>Yetenek modeli ve arabirimleri depolayın

IoT Tak ve Kullan cihazlarda, cihazın yeteneklerini JSON dosyaları olarak tanımlayan bir yetenek modeli ve arabirimler yazmanız gerekir.

Bu JSON dosyalarını üç farklı konuma kaydedebilirsiniz:

- Ortak model deposu.
- Şirket modeli deponuz.
- Cihazınızda.

Şu anda, cihazınızı onaylamak için dosyaların şirket modeli deponuzda veya ortak model deposunda depolanması gerekir.

## <a name="include-the-required-interfaces"></a>Gerekli arabirimleri dahil et

Sertifika sürecini geçirmek için, **cihaz bilgileri** arabirimini yetenek modelinize dahil etmeniz ve uygulamanız gerekir. Bu arabirim aşağıdaki tanıma sahiptir:

```json
"@id": "urn:azureiot:DeviceManagement:DeviceInformation:1"
```

> [!NOTE]
> [Hızlı başlangıç: bir cihaz oluşturmak için bir cihaz yetenek modeli kullanın](quickstart-create-pnp-device-windows.md), modelinize **cihaz bilgileri** arabirimini eklemiş oldunuz.

Cihaz modelinize **cihaz bilgileri** arabirimini dahil etmek için, arabirim kimliğini yetenek modelinin `implements` özelliğine ekleyin:

```json
{
  "@id": "urn:yourcompanyname:sample:ThermostatDevice:1",
  "@type": "CapabilityModel",
  "displayName": "Thermostat T-1000",
  "implements": [
    "urn:yourcompanyname:sample:Thermostat:1",
    "urn:azureiot:DeviceManagement:DeviceInformation:1"
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

**Cihaz bilgileri** arabirimini vs Code görüntülemek için:

1. Komut paletini açmak için **CTRL + SHIFT + P** tuşlarını kullanın.

1. **Tak ve kullan** girin ve ardından **IoT Tak ve kullan Open model deposu** komutunu seçin. **Ortak model deposunu aç**' ı seçin. Ortak model deposu VS Code açılır.

1. Ortak model deposunda, **arabirimler** sekmesini seçin, filtre simgesini seçin ve **cihaz bilgilerini** filtre alanına girin.

1. **Cihaz bilgileri** arabiriminin yerel bir kopyasını oluşturmak için filtrelenmiş listeden seçin ve ardından **İndir**' i seçin. VS Code arabirim dosyasını görüntüler.

Azure CLı kullanarak **cihaz bilgileri** arabirimini görüntülemek için:

1. [Azure ıOT CLI uzantısını yükler](howto-install-pnp-cli.md).

1. Aşağıdaki Azure CLı komutunu kullanarak cihaz bilgileri arabirim KIMLIĞIYLE bir arabirim gösterebilirsiniz:

    ```cmd/sh
    az iot pnp interface show --interface urn:azureiot:DeviceManagement:DeviceInformation:1
    ```

Daha fazla bilgi için bkz. [Azure CLI Için Azure IoT uzantısını yükleyip kullanma](howto-install-pnp-cli.md).

## <a name="update-device-code"></a>Cihaz kodunu Güncelleştir

### <a name="enable-device-provisioning-through-the-azure-iot-device-provisioning-service-dps"></a>Azure IoT cihaz sağlama hizmeti (DPS) aracılığıyla cihaz sağlamayı etkinleştirme

Cihazı onaylamak için, [Azure IoT cihaz sağlama hizmeti (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps)aracılığıyla sağlamayı etkinleştirmesi gerekir. DPS kullanma yeteneğini eklemek için, VS Code 'da C kodu saplaması oluşturabilirsiniz. Şu adımları uygulayın:

1. VS Code içinde DCM dosyası olan klasörü açın, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve **cihaz kodu saplama oluştur**' u seçin.

1. Cihaz kodu saplaması oluşturmak için kullanmak istediğiniz DCM dosyasını seçin.

1. **Sample_device**gibi proje adını girin. Bu, cihaz uygulamanızın adıdır.

1. Dil olarak **ANSI C** 'yi seçin.

1. Bağlantı yöntemi olarak, **DPS (cihaz sağlama hizmeti) ile simetrik anahtar** arasında seçim yapın.

1. Proje şablonunuz olarak **Windows 'Da CMake projesi '** ni seçin.

1. Cihaz SDK 'sını dahil etmek için **Vcpkg aracılığıyla** öğesini seçin.

1. VS Code, oluşturulan cihaz kodu saplama dosyaları ile yeni bir pencere açar.

## <a name="build-and-run-the-code"></a>Kodu derleyin ve çalıştırın

Oluşturulan cihaz kodu saplaması oluşturmak için Vcpkg paketini kullanın. Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan bir cihaza benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. `sample_device` klasöründe bir `cmake` alt dizini oluşturun ve bu klasöre gidin:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Oluşturulan kod Saplaması oluşturmak için aşağıdaki komutları çalıştırın (yer tutucuyu Vcpkg depoağınızın diziniyle değiştirin):

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Visual Studio 2017 veya 2015 kullanıyorsanız, kullanmakta olduğunuz yapı araçlarına göre CMake oluşturucuyu belirtmeniz gerekir:
    >```cmd
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > CMake derleyicinizi C++ bulamazsa, önceki komutu çalıştırdığınızda derleme hataları alırsınız. Bu durumda, [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)bu komutu çalıştırmayı deneyin.

1. Oluşturma işlemi başarıyla tamamlandıktan sonra, DPS kimlik bilgilerini (**DPS kimlik kapsamı**, **DPS simetrik anahtar**, **cihaz kimliği**) uygulamanın parametreleri olarak girin. Sertifika portalından kimlik bilgilerini almak için bkz. [ıot Tak ve kullan cihazınızı bağlama ve test](tutorial-certification-test.md#connect-and-discover-interfaces)etme.

    ```cmd\sh
    .\Debug\sample_device.exe [Device ID] [DPS ID Scope] [DPS symmetric key]
    ```

### <a name="implement-standard-interfaces"></a>Standart arabirimleri Uygula

#### <a name="implement-the-model-information-and-sdk-information-interfaces"></a>Model bilgilerini ve SDK bilgi arabirimlerini uygulama

Azure IoT cihaz SDK 'Sı, model bilgilerini ve SDK bilgi arabirimlerini uygular. VS Code içinde kod üretme işlevini kullanırsanız, cihaz kodunuz IoT Tak ve Kullan cihaz SDK 'sını kullanır.

Azure IoT cihaz SDK 'sını kullanmayı tercih ederseniz, kendi uygulamanız için SDK kaynak kodunu başvuru olarak kullanabilirsiniz.

#### <a name="implement-the-device-information-interface"></a>Cihaz bilgileri arabirimini uygulama

Cihaza cihaz **bilgileri** arabirimini uygulayın ve çalışma zamanında cihazdan cihaza özgü bilgiler sağlayın.

[Linux](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) Için **cihaz bilgisi** arabiriminin örnek bir uygulamasını başvuru olarak kullanabilirsiniz.

### <a name="implement-all-the-capabilities-defined-in-your-model"></a>Modelinizde tanımlanan tüm özellikleri uygulayın

Sertifika sırasında, cihazınız, arabirimlerinde tanımlanan özellikleri uyguladığından emin olmak için programlı olarak test edilir. Cihazınız uygulamadıysanız okuma yazma özelliğine ve komut isteklerine yanıt vermek için 501 HTTP durum kodunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Tak ve Kullan bir cihaz oluşturduğunuzdan, önerilen sonraki adım şunları yapmanız gerekir:

> [!div class="nextstepaction"]
> [Cihazınızı nasıl doğrulayacağınızı öğrenin](tutorial-certification-test.md)
