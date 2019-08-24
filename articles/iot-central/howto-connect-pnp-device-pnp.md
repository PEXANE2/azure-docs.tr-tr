---
title: IoT Tak ve Kullan cihazını IoT Central 'a bağlama | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, IoT Central bağlanan bir IoT Tak ve Kullan cihazı oluşturmak ve test etmek için Visual Studio Code kullanma hakkında bilgi edinin.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 6eb73c1c2b768d82143890415702950905f261f6
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997239"
---
# <a name="use-visual-studio-code-to-create-an-iot-plug-and-play-device-that-connects-to-iot-central"></a>IoT Central bağlanan bir IoT Tak ve Kullan cihazı oluşturmak için Visual Studio Code kullanın

Bu nasıl yapılır kılavuzunda, nasıl yapılacağı gösterilmektedir:

* Bir operatör olarak, Azure IoT Central uygulamanızda gerçek bir cihaz eklemek ve yapılandırmak için.

* Bir cihaz geliştiricisi olarak, IoT Central uygulamanıza bağlanan bir [ıot Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md) cihazı oluşturmak için Visual Studio Code kullanın.

IoT Central bağlanan cihazı tanımlamak için bir [cihaz yetenek modeli](./concepts-architecture-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) kullanın. Bu kılavuzda, bir ortam algılayıcı cihazı tanımlayan bir model kullanırsınız.

Cihaz geliştiricisi cihaz istemci kodu oluşturmak için modeli kullanır ve Oluşturucu, IoT Central ' de [bir cihaz şablonu oluşturmak](./howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) için modeli kullanır. Model, cihazınız ile IoT Central uygulamanız arasında bir sözleşme işlevi görür.

Bu kılavuzda, oluşturulan kodun nasıl oluşturulacağını açıklayan bölüm, Windows kullandığınızı varsayar.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

* Cihaz yetenek modelini IoT Central bir cihaz şablonuna aktarma
* Cihaza cihaz telemetrisini görüntüleyen bir pano ekleme
* Şablondan gerçek bir cihaz ekleme
* Cihaz yetenek modelini Visual Studio Code içine aktarma
* Modelden C cihaz istemci uygulaması oluşturma
* C cihaz istemci uygulamasını oluşturun ve IoT Central bağlayın

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzdaki cihaz kodunuzu test etmek için **Önizleme** uygulaması şablonundan oluşturulmuş bir IoT Central uygulamasına ihtiyacınız vardır. Kullanmak üzere bir IoT Central uygulamanız yoksa hızlı başlangıç [Azure IoT Central uygulaması oluşturma (Önizleme özellikleri)](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)' ni doldurun:

Bu kılavuzdaki cihaz yetenek modeliyle çalışmak için şunlar gerekir:

* [Visual Studio Code](https://code.visualstudio.com/download): Visual Studio Code birden çok platformda kullanılabilir
* Visual Studio Code için Azure IoT cihaz çalışma ekranı uzantısı. Azure IoT cihaz çalışma ekranı uzantısını VS Code yüklemek için aşağıdaki adımları kullanın:

    1. VS Code, **Uzantılar** sekmesini seçin.
    1. **Azure IoT cihaz çalışma ekranı**için arama yapın.
    1. **Yükle**’yi seçin.

    > [!NOTE]
    > Uzantıdaki kod oluşturucunun geçerli sürümü, **Geopoint** ve **vektör** şema türlerini ya da **hızlandırma**, **hız**ve **konum** anlam türlerini desteklemez. Bu şema ve anlamsal türler IoT Central tarafından desteklenir.

    > [!NOTE]
    > IoT Central çalışmak için, cihaz yetenek modelinde aynı dosyada satır içi tanımlanmış tüm arabirimlerin olması gerekir.

Bu kılavuzda Windows üzerinde oluşturulan C kodunu derlemek için şunlar gerekir:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) -Visual Studio 'yu yüklerken **NuGet Paket Yöneticisi** bileşenini ve iş yüküyle **Masaüstü geliştirmeyi C++**  eklediğinizden emin olun.
* [Git](https://git-scm.com/download)
* [CMake](https://cmake.org/download/) -CMake 'iyüklediğinizde, **sistem yoluna CMake Ekle**seçeneğini belirleyin.
* Azure IoT C SDK 'sının yerel bir kopyası:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

Bu nasıl yapılır 'nin sonundaki cihaz-ilk yönergeleri izlemek için aşağıdakileri de yüklemeniz gerekir:

* [Node.js](https://nodejs.org)
* [DPS-keygen aracı](https://www.npmjs.com/package/dps-keygen):

    ```cmd/sh
    npm i -g dps-keygen
    ```

## <a name="create-device-template"></a>Cihaz şablonu oluştur

Klonladığınız Azure IoT C SDK 'sını içeren **Azure-IoT-SDK-c** klasöründe **pnp_app** adlı bir klasör oluşturun. GitHub 'dan [Environmentalalgılayıcı. capabilitymodel. JSON](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) cihaz yetenek modelini indirin ve dosyayı **pnp_app** klasörüne kaydedin. İki örneğini `<YOUR_COMPANY_NAME_HERE>` adıyla değiştirin. Yalnızca a-z, A-Z, 0-9 ve alt çizgi karakterlerini kullanın. Cihaz yetenek modelinizi benzersiz bir şekilde tanımlayan `"@id"` alanın tam değerini bir yere, daha sonra ihtiyacınız olduğunu unutmayın. Bu model iki arabirim içerir:

* **DeviceManagement** ortak arabirimi.
* **Environmentalalgılayıcı** özel arabirimi.

IoT Central 'de ortam algılayıcı cihazı için bir cihaz şablonu oluşturmak için:

1. **Cihaz şablonları** sayfasına gidin ve **+ Yeni**' yi seçin. Ardından **özel**' i seçin.

1. Cihaz şablonu adı olarak **çevresel algılayıcı** girin.

1. **Yetenek modelini Içeri aktar**' ı seçin. Sonra oluşturduğunuz **Environmentalsensörü. capabilitymodel. JSON** ' ı bulun ve **Aç**' ı seçin. **Cihaz bilgileri** ve **ortam algılayıcısı**olmak üzere Iki arabirim, **ortam algılayıcısı yetenek modelinde**görüntülenir.

1. **Görünümler** ' i ve ardından **cihazı görselleştirmeyi**seçin.

1. Panoya ekleyebileceğiniz alanlar listesinde, iki **telemetri** değerini, **nem** ve **sıcaklık** ' ı seçin ve **Birleştir**' i seçin. Ardından **Kaydet**'i seçin.

Artık **çevresel algılayıcı** modelini kullanan ve bir cihazdan telemetri göstermek için basit bir panoya sahip olan bir cihaz şablonunuz vardır.

## <a name="publish-the-template-and-add-a-real-device"></a>Şablonu yayımlayın ve gerçek bir cihaz ekleyin

Şablonu yayımlamak ve gerçek bir cihaz eklemek için **cihaz şablonları** sayfasına gidin ve **çevresel algılayıcı** cihaz şablonunu seçin. **Yayımla**' yı seçin, bilgileri gözden geçirin ve **Yayımla**' yı seçin.

Bir istemci uygulamasını bağlanmadan önce **cihazlar** sayfasına bir cihaz eklemeniz ve bağlantı bilgilerini almanız gerekir:

1. **Cihazlar** sayfasına gidin ve **ortam algılayıcısı**' nı seçin. **Cihazlar** sayfası, uygulamanıza bağlanabilecek cihazları yönetmenizi sağlar.

1. Gerçek bir cihaz eklemek için **+ Yeni**' yi seçin, cihaz kimliğini **sensor01**olarak değiştirin ve **Oluştur**' u seçin. **Benzetimli** 'un **kapalı**olduğundan emin olun.

1. Cihaz listesinde, **ortam algılayıcı** cihazınızı seçin. Ardından **Bağlan**' ı seçin.

1. **Kapsam kimliği**, **cihaz kimliği**ve **birincil anahtarı**bir yere göz önünde alın. Ardından **Kapat**' ı seçin.

## <a name="generate-a-device-client-application"></a>Bir cihaz istemci uygulaması oluşturma

Cihaz yetenek modelinden bir iskelet cihaz istemci uygulaması oluşturmak için Visual Studio Code kullanırsınız:

1. Visual Studio Code başlatın ve **pnp_app** klasörünü açın.

1. Komut paletini açmak için **CTRL + SHIFT + P** tuşlarını kullanın.

1. **Tak ve kullan** girin ve ardından **cihaz kodu saplama oluştur**' u seçin.

1. **Environmentalalgılayıcı. capabilitymodel. JSON** cihaz yetenek modeli dosyasını seçin.

1. Proje adı olarak **sensor_app** girin.

1. Dil olarak **ANSI C** 'yi seçin.

1. Hedef olarak **CMake projesini** seçin.

1. Bağlantı yöntemi olarak, **DPS ile (cihaz sağlama hizmeti) simetrik anahtar** arasında seçim yapın.

Visual Studio Code, **sensor_app** klasöründe oluşturulan C koduna sahip yeni bir pencere açar.

## <a name="add-connection-details-to-the-device-client"></a>Cihaz istemcisine bağlantı ayrıntıları ekleme

Bağlantı bilgilerini cihaz istemcinizi eklemek için, oluşturulan kodunuzu içeren klasörde **Main. c** ' yi açın:

1. Daha `[DPS Id Scope]` önce bir Note yaptığınız **kapsam kimliği** değeriyle değiştirin.

1. Daha `[DPS symmetric key]` önce bir notunuz yaptığınız **birincil anahtar** değeriyle değiştirin.

1. Daha `[device registration Id]` önce bir örneği yaptığınız **cihaz kimliği** değeriyle değiştirin.

1. Değişiklikleri kaydedin.

## <a name="build-and-run-the-client"></a>İstemciyi derleyin ve çalıştırın

İstemcisini derlemek ve çalıştırmak için, Azure IoT C SDK 'sının derlemesini özelleştirmeniz gerekir:

1. **Azure-IoT-SDK-c** klasörünün kökündeki **cmakelists. txt** dosyasını açın.

1. Yeni istemci uygulamasını yapıya dahil etmek için bu dosyanın sonuna aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory(pnp_app/sensor_app)
    ```

1. Değişiklikleri kaydedin.

1. Bir komut istemi açın ve **Azure-IoT-SDK-c** klasörüne gidin.

1. Uygulamayı derlemek için aşağıdaki komutları çalıştırın:

    ```cmd
    mkdir cmake
    cd cmake
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -Dskip_samples:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

1. Uygulamayı çalıştırmak için, aynı komut isteminden aşağıdaki komutu çalıştırın:

    ```cmd
    pnp_app\sensor_app\Release\sensor_app.exe
    ```

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleme

Birkaç dakika sonra, cihaz panosundaki IoT Central uygulamanızdaki Telemetriyi görebilirsiniz.

## <a name="connect-device-first"></a>Cihazı bağla-önce

Bir IoT Tak ve Kullan cihazını, [bağlantı](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)bölümünde açıklandığı gibi cihaz-ilk bağlantı aracılığıyla bağlayabilirsiniz. Bulma işlemi şu sırada izler:

1. IoT Central uygulamasında zaten yayımlandıysa cihaz şablonuyla ilişkilendirir.

1. Yayınlanan ve sertifikalı yetenek modellerinin [ortak deposundan](https://aka.ms/ACFI) yetenek modelini getirir.

Visual Studio Code ve yukarıda belirtilen **cihaz kodu oluşturma saplama** komutunu kullanarak, cihazı bağlamak için bu adımları izleyebilir ve ortak depodan yayınlanan cihaz yetenek modelinizi IoT Central ' e otomatik olarak getirebilirsiniz:

1. Ortak depoda yayımlanmış mevcut bir cihaz yetenek modeli kullanın. Tam cihaz yetenek modeli gerekir ve bu modelin URN 'sini göz önünde bulabilirsiniz.

1. Visual Studio Code kullanılacak [bir cihaz istemci uygulaması oluşturmak](#generate-a-device-client-application) ve cihaz kodunu oluşturmak için yukarıdaki adımları izleyin.

1. IoT Central uygulamanızdan **Yönetim** sekmesine gidin ve **cihaz bağlantısı** bölümünü seçin. Uygulamanız için **kapsam kimliğini** ve **birincil anahtar** değerlerini bir yere unutmayın.

    > [!NOTE]
    > Bu sayfada gösterilen **birincil anahtar** , uygulamanız için birden çok cihaz anahtarı oluşturmak için kullanabileceğiniz bir grup paylaşılan erişim imzasıdır.

1. Daha önce bir değişiklik yaptığınız birincil anahtardan bir cihaz anahtarı oluşturmak için [DPS keygen](https://www.npmjs.com/package/dps-keygen) aracını kullanın. Cihaz anahtarını oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dps-keygen -mk:<Primary_Key> -di:<device_id>
    ```

1. **Kapsam kimliği**, **birincil anahtar**ve **cihaz kimliği**eklemek için önceki bölümdeki adımları izleyerek [cihaz istemcisine bağlantı ayrıntılarını ekleyin](#add-connection-details-to-the-device-client) .

1. [Istemcisini derlemek ve çalıştırmak](#build-and-run-the-client)için önceki bölümdeki adımları izleyin.

1. Artık cihazın IoT Central uygulamanıza bağlanmasını ve cihaz özelliği modelini bir cihaz şablonu olarak ortak depodan otomatik olarak getirmeyi görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Artık gerçek bir cihazı IoT Central 'e bağlamayı öğrendiğinize göre, bir sonraki adım, cihaz [şablonu ayarlama](howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bölümünde cihaz şablonları hakkında daha fazla bilgi edinmek
