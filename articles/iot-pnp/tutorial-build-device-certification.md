---
title: Sertifikaya hazır bir IoT Tak ve Çalıştır Önizleme cihazı oluşturun | Microsoft Dokümanlar
description: Bir aygıt geliştiricisi olarak, sertifikaya hazır bir IoT Tak ve Çalıştır Önizleme aygıtını nasıl oluşturabileceğiniz hakkında bilgi edinin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 12/28/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e97aa07d2a43a03805fd881c674157ee676c37b4
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239898"
---
# <a name="build-an-iot-plug-and-play-preview-device-thats-ready-for-certification"></a>Sertifikaya hazır bir IoT Tak ve Çalıştır Önizleme cihazı oluşturun

Bu öğretici, bir aygıt geliştiricisi olarak, sertifikaya hazır bir IoT Tak ve Çalıştır Önizleme aygıtı nı nasıl oluşturabileceğinizi açıklar.

Sertifika testleri aşağıdakileri denetler:

- IoT Tak ve Çalıştır aygıt kodunuz cihazınıza yüklenir.
- IoT Tak ve Çalıştır aygıt kodunuz Azure IoT SDK ile oluşturulmuştür.
- Cihaz kodunuz [Azure IoT Hub Aygıt Sağlama Hizmetini](../iot-dps/about-iot-dps.md)destekler.
- Aygıt kodunuz Aygıt Bilgileri arabirimini uygular.
- Yetenek modeli ve aygıt kodu IoT Central ile çalışır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Visual Studio Code](https://code.visualstudio.com/download)
- VS Kodu uzatma paketi [için Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

Ayrıca, Windows için bir aygıt hızlı başlatma oluşturmak için [aygıt özelliği modelini kullan'ı](quickstart-create-pnp-device-windows.md) da tamamlamanız gerekir. Hızlı başlangıç, Vcpkg kullanarak geliştirme ortamınızı nasıl ayarlayıp örnek bir proje oluşturabileceğinizi gösterir.

## <a name="store-a-capability-model-and-interfaces"></a>Yetenek modelini ve arabirimlerini depolama

IoT Tak ve Çalıştır aygıtları için, aygıtın yeteneklerini JSON dosyaları olarak tanımlayan bir yetenek modeli ve arabirimleri yazmanız gerekir.

Bu JSON dosyalarını üç farklı konumda depolayabilirsiniz:

- Genel model deposu.
- Şirket model deponuz.
- Cihazınızda.

Şu anda, aygıtınızı onaylamak için dosyaların şirket model deponuzda veya genel model deposunda depolanmalıdır.

## <a name="include-the-required-interfaces"></a>Gerekli arabirimleri ekleme

Sertifika işlemini geçmek için, aygıt **bilgileri** arabirimini yetenek modelinize eklemeniz ve uygulamanız gerekir. Bu arabirimaşağıdaki tanımlamaya sahiptir:

```json
"@id": "urn:azureiot:DeviceManagement:DeviceInformation:1"
```

> [!NOTE]
> [Quickstart: Bir aygıt oluşturmak için bir aygıt yeteneği modelini kullandıysanız,](quickstart-create-pnp-device-windows.md)cihazınıza **Aygıt Bilgileri** arabirimini zaten dahil etmişsinizdir.

**Aygıt Bilgi** arabirimini aygıt modelinize eklemek için, `implements` arabirim kimliğini yetenek modelinin özelliğine ekleyin:

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

VS Kodu'nda **Aygıt Bilgileri** arabirimini görüntülemek için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Tak ve Açık Model Deposu** komutunu seçin. **Açık Genel Model Deposu'nu**seçin. Genel model deposu VS Code'da açılır.

1. Ortak model **deposunda, Arabirimler** sekmesini seçin, filtre simgesini seçin ve filtre alanına **Aygıt Bilgileri** girin.

1. **Aygıt Bilgileri** arabiriminin yerel bir kopyasını oluşturmak için filtre uygulanmış listede seçin ve ardından **İndir'i**seçin. VS Code arabirim dosyasını görüntüler.

Azure CLI'yi kullanarak **Aygıt Bilgileri** arabirimini görüntülemek için:

1. [Azure IoT CLI uzantısını yükleyin.](howto-install-pnp-cli.md)

1. Aygıt Bilgileri arabirimi kimliğiyle bir arabirim göstermek için aşağıdaki Azure CLI komutunu kullanın:

    ```azurecli
    az iot pnp interface show --interface urn:azureiot:DeviceManagement:DeviceInformation:1
    ```

Daha fazla bilgi için Azure [CLI için Azure IoT uzantısını yükle'ye](howto-install-pnp-cli.md)bakın ve kullanın.

## <a name="update-device-code"></a>Aygıt kodunu güncelleştir

### <a name="enable-device-provisioning-through-the-azure-iot-device-provisioning-service-dps"></a>Azure IoT Aygıt Sağlama Hizmeti (DPS) aracılığıyla aygıt sağlama yı etkinleştirme

Aygıtı onaylamak için Azure [IoT Aygıt Sağlama Hizmeti (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps)aracılığıyla sağlama olanağı sağlaması gerekir. DPS'yi kullanma özelliğini eklemek için VS kodunda C kodu saplaması oluşturabilirsiniz. Şu adımları uygulayın:

1. VS Kodu'nda DCM dosyası olan klasörü açın, komut paletini açmak için **Ctrl+Shift+P** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Aygıt Kodu Saplaması Oluştur'u**seçin.

1. Aygıt kodu saplaması oluşturmak için kullanmak istediğiniz DCM dosyasını seçin.

1. proje adını girin, örneğin **sample_device.** Bu, aygıt uygulamanızın adıdır.

1. Dil olarak **ANSI C'yi** seçin.

1. Bağlantı yöntemi olarak **Via DPS (Aygıt Sağlama Hizmeti) simetrik anahtarını** seçin.

1. Proje şablonu olarak **Windows'da CMake Project'i** seçin.

1. SDK cihazını eklemenin yolu olarak **Via Vcpkg'ı** seçin.

1. VS Code oluşturulan aygıt kodu saplama dosyaları ile yeni bir pencere açar.

## <a name="build-and-run-the-code"></a>Kodu oluşturma ve çalıştırma

Oluşturulan aygıt kodu saplaması oluşturmak için Vcpkg paketini kullanırsınız. Oluşturduğunuz uygulama, bir IoT hub'ına bağlanan bir aygıtı simüle eder. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. `sample_device` Klasörde bir `cmake` alt dizin oluşturun ve bu klasöre gidin:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Oluşturulan kod saplaması oluşturmak için aşağıdaki komutları çalıştırın (yer tutucuyu Vcpkg repo diziniile değiştirme):

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Visual Studio 2017 veya 2015 kullanıyorsanız, kullandığınız yapı araçlarına göre CMake jeneratörü belirtmeniz gerekir:
    >```cmd
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > Cmake C++ derleyicinizi bulamazsa, önceki komutu çalıştırdığınızda yapı hataları alırsınız. Bu durumda, [Visual Studio komut istemibu komutu](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)çalıştırmayı deneyin.

1. Yapı başarıyla tamamlandıktan sonra, uygulama için parametreler olarak DPS kimlik bilgilerini **(DPS Id Scope**, **DPS Simetrik Anahtar**, Aygıt **Kimliği)** girin. Sertifika bilgilerini sertifika portalından almak için [Bağlan'a bakın ve IoT Tak ve Çalıştır cihazınızı test edin.](tutorial-certification-test.md#connect-and-discover-interfaces)

    ```cmd\sh
    .\Debug\sample_device.exe [Device ID] [DPS ID Scope] [DPS symmetric key]
    ```

### <a name="implement-standard-interfaces"></a>Standart arabirimleri uygulama

#### <a name="implement-the-model-information-and-sdk-information-interfaces"></a>Model Bilgileri ve SDK Bilgi arayüzlerini uygulayın

Azure IoT aygıtı SDK, Model Bilgileri ve SDK Bilgi arabirimlerini uygular. VS Code'da kod oluşturma işlevini kullanıyorsanız, aygıt kodunuz IoT Tak ve Çalıştır aygıtı SDK'yı kullanır.

Azure IoT aygıtı SDK'yı kullanmamayı seçtiyseniz, SDK kaynak kodunu kendi uygulamanız için başvuru olarak kullanabilirsiniz.

#### <a name="implement-the-device-information-interface"></a>Aygıt Bilgileri arabirimini uygulayın

**Cihazınıza Aygıt Bilgileri** arabirimini uygulayın ve çalışma zamanında cihazdan aygıta özgü bilgiler sağlayın.

[Linux](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) için **Aygıt Bilgileri** arabiriminin örnek bir uygulamasını referans olarak kullanabilirsiniz.

### <a name="implement-all-the-capabilities-defined-in-your-model"></a>Modelinizde tanımlanan tüm yetenekleri uygulayın

Sertifikasyonu sırasında cihazınız, arabirimlerinde tanımlanan yetenekleri uyguladığından emin olmak için programlı olarak sınanır. Cihazınız bunları uygulamazsa okuma yazma özelliği ve komut isteklerine yanıt vermek için HTTP durum kodu 501'i kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Artık sertifikaya hazır bir IoT Tak ve Çalıştır cihazı inşa ettiğinize göre önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Cihazınızı nasıl onaylarsanız öğrenin](tutorial-certification-test.md)
