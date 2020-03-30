---
title: Visual Studio'da modülgeliştirme ve hata ayıklama - Azure IoT Edge
description: Bir C veya C# IoT Edge modülü geliştirmek ve dağıtım bildirimi tarafından yapılandırılan IoT Hub'ınızdan ioT aygıtına itmek için Azure IoT Araçları ile Visual Studio'yı kullanın.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 3/27/2020
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 9722c7dec3a066d8f776424cb599be0d463416d9
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384866"
---
# <a name="use-visual-studio-2019-to-develop-and-debug-modules-for-azure-iot-edge"></a>Azure IoT Edge için modüller geliştirmek ve hata ayıklamak için Visual Studio 2019'u kullanın

Azure IoT Edge için iş mantığınızı modüllere dönüştürebilirsiniz. Bu makalede, Visual Studio 2019'un modülleri geliştirmek ve hata ayıklamak için ana araç olarak nasıl kullanılacağı gösterilmektedir.

Visual Studio için Azure IoT Edge Araçları aşağıdaki avantajları sağlar:

- Yerel geliştirme bilgisayarınızda Azure IoT Edge çözümleri ve modülleri oluşturun, oluşturun, oluşturun, çalıştırın ve hata ayıklanın.
- Azure IoT Edge çözümünüzü Azure IoT Hub üzerinden Azure IoT Edge aygıtına dağıtın.
- Azure IoT modüllerinizi C veya C# olarak kodlarken Visual Studio geliştirmenin tüm avantajlarından yararlanın.
- Azure IoT Edge aygıtlarını ve modüllerini UI ile yönetin.

Bu makalede, IoT Edge modüllerinizi geliştirmek için Visual Studio 2019 için Azure IoT Edge Araçlarını nasıl kullanacağınızı gösterilmektedir. Projenizi Azure IoT Edge cihazınıza nasıl dağıtabileceğinizi de öğrenirsiniz. Şu anda Visual Studio 2019, C ve C# ile yazılmış modüller için destek sağlamaktadır. Desteklenen aygıt mimarileri Windows X64 ve Linux X64 veya ARM32'dir. Desteklenen işletim sistemleri, diller ve mimariler hakkında daha fazla bilgi için [Dil ve mimari desteğine](module-development.md#language-and-architecture-support)bakın.
  
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, geliştirme makineniz olarak Windows çalıştıran bir bilgisayar veya sanal makine kullandığınız varsayar. Windows bilgisayarlarda Windows veya Linux modülleri geliştirebilirsiniz. Windows modülleri geliştirmek için sürüm 1809/build 17763 veya daha yeni çalışan bir Windows bilgisayarı kullanın. Linux modülleri geliştirmek için Docker Desktop [gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan bir Windows bilgisayarı kullanın.

Bu makalede Visual Studio 2019 ana geliştirme aracı olarak kullandığından, Visual Studio'yu yükleyin. Visual Studio 2019 kurulumunuza C++ iş yükleriyle **Azure geliştirme** ve **Masaüstü geliştirmeyi** eklediğinizden emin olun. Gerekli iş yüklerini eklemek için [Visual Studio 2019'u değiştirebilirsiniz.](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019)

Visual Studio 2019'unuz hazır olduktan sonra aşağıdaki araçlara ve bileşenlere de ihtiyacınız vardır:

- Visual Studio 2019'da bir IoT Edge projesi oluşturmak için Visual Studio pazar yerinden [Azure IoT Edge Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) indirin ve yükleyin.

> [!TIP]
> Visual Studio 2017 kullanıyorsanız, lütfen Visual Studio pazar yerinden VS 2017 için [Azure IoT Edge Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) indirin ve yükleyin

- Modül görüntülerinizi oluşturmak ve çalıştırmak için Geliştirme makinenize [Docker Community Edition'ı](https://docs.docker.com/install/) indirin ve yükleyin. Docker CE'yi Linux kapsayıcı modunda veya Windows kapsayıcı modunda çalışacak şekilde ayarlamanız gerekir.

- [Azure IoT EdgeHub Dev Aracı'nı](https://pypi.org/project/iotedgehubdev/)yükleyerek ioT Edge çözümünüzü hata ayıklamak, çalıştırmak ve test etmek için yerel geliştirme ortamınızı ayarlayın. [Python (2.7/3.6+) ve Pip'i](https://www.python.org/) yükleyin ve terminalinizde aşağıdaki komutu çalıştırarak **iotedgehubdev** paketini yükleyin. Azure IoT EdgeHub Dev Aracı sürümünün 0.3.0'dan büyük olduğundan emin olun.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Depoyu klonlayın ve Vcpkg kitaplık yöneticisini yükleyin ve ardından Windows için **azure-iot-sdk-c paketini** yükleyin.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure Kapsayıcı Kayıt Defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker Hub'](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)ı.

  > [!TIP]
  > Bulut kayıt defteri yerine prototip ve test amacıyla yerel bir Docker kayıt defteri kullanabilirsiniz.

- Modülünüzü bir aygıtta test etmek için en az bir IoT Edge aygıtına sahip etkin bir IoT hub'ına ihtiyacınız vardır. Bilgisayarınızı IoT Edge aygıtı olarak kullanmak [için, Linux](quickstart-linux.md) veya [Windows](quickstart.md)için hızlı başlatma adımlarını izleyin. Geliştirme makinenizde IoT Edge daemon çalıştırıyorsanız, Visual Studio'da geliştirmeye başlamadan önce EdgeHub ve EdgeAgent'ı durdurmanız gerekebilir.

### <a name="check-your-tools-version"></a>Araç sürümünüzü kontrol edin

1. **Uzantılar** menüsünden **Uzantıları Yönet'i**seçin. **Yüklü > Araçlarını** Genişletin ve Visual Studio için Azure **IoT Edge Araçlarını** ve Visual Studio için Cloud **Explorer'ı**bulabilirsiniz.

1. Yüklenen sürüme dikkat edin. Bu sürümü Visual Studio Marketplace'in en son sürümüyle karşılaştırabilirsiniz ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS2019), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools))

1. Sürümünüz Visual Studio Marketplace'te mevcut olandan daha eskiyse, aşağıdaki bölümde gösterildiği gibi Visual Studio'daki araçlarınızı güncelleyin.

### <a name="update-your-tools"></a>Araçlarınızı güncelleştirin

1. Uzantıları Yönet penceresinde, **Visual Studio Market>** **Güncelleştirmeleri** genişletin, Visual Studio için **Azure IoT Edge Tools** veya **Cloud Explorer'ı** seçin ve **Güncelleştir'i**seçin.

1. Araçlar güncelleştirmesi indirildikten sonra, VSIX yükleyicisini kullanarak araç güncellemesini tetiklemek için Visual Studio'yu kapatın.

1. Yükleyicide, başlatmak için **Tamam'ı** ve araçları güncelleştirmek için **Değiştir'i** seçin.

1. Güncelleştirme tamamlandıktan **sonra, Visual Studio'yu kapat'ı** seçin ve Visual Studio'yu yeniden başlatın.

### <a name="create-an-azure-iot-edge-project"></a>Azure IoT Edge projesi oluşturma

Visual Studio'daki Azure IoT Edge proje şablonu, Azure IoT Hub'daki Azure IoT Edge aygıtlarına dağıtılabilen bir proje oluşturur. Önce bir Azure IoT Edge çözümü oluşturursunuz, sonra da bu çözümdeki ilk modülü oluşturursunuz. Her IoT Edge çözümü birden fazla modül içerebilir.

> [!TIP]
> Visual Studio tarafından oluşturulan IoT Edge proje yapısı Visual Studio Code ile aynı değildir.

1. Visual Studio yeni proje iletişim kutusunda, Arama ve **Azure IoT Edge** proje seçin ve **İleri'yi**tıklatın. Proje yapılandırma penceresinde, projeniz için bir ad girin ve konumu belirtin ve sonra **Oluştur'u**seçin. Varsayılan proje adı **AzureIoTEdgeApp1'dir.**

   ![Yeni Proje Oluştur](./media/how-to-visual-studio-develop-csharp-module/create-new.png)

1. **IoT Edge Uygulama ve Modül Ekle** penceresinde **C# Modülü** veya C **Modülü'nü** seçin ve ardından modül adınızı ve modül görüntü deponuzu belirtin. Visual Studio, modül adını **localhost:5000/<ile\>otomatik**olarak doldurur. Kendi kayıt defteri bilgilerinizle değiştirin. Test etmek için yerel bir Docker kayıt defteri kullanıyorsanız, **localhost** iyidir. Azure Kapsayıcı Kayıt Defteri kullanıyorsanız, kayıt defterinizin ayarlarından giriş sunucusunu kullanın. Giriş sunucusu kayıt ** _ \<defteri adı\>_.azurecr.io**gibi görünüyor. Yalnızca **yerel host:5000** parçasını değiştirerek, nihai sonucun ** \<kayıt defteri adı .azurecr.io/_\<modül\>_ *adıgibi*\>** görünmesi için. Varsayılan modül adı **IotEdgeModule1**

   ![Uygulama ve Modül Ekle](./media/how-to-visual-studio-develop-csharp-module/add-application-and-module.png)

1. C# veya C kullanan bir modülle Azure IoT Edge çözümünü oluşturmak için **Tamam'ı** seçin.

Artık bir **AzureIoTEdgeApp1.Linux.Amd64** projeniz veya **bir AzureIoTEdgeApp1.Windows.Amd64** projeniz ve ayrıca çözümünüzde bir **IotEdgeModule1** projeniz var. Her **AzureIoTEdgeApp1** projesinde, IoT Edge çözümünüz için oluşturmak ve dağıtmak istediğiniz modülleri tanımlayan ve modüller arasındaki yolları tanımlayan bir `deployment.template.json` dosya vardır. Varsayılan çözüm, **Simüle TemperatureSensor** modülü ve **IotEdgeModule1** modülüne sahiptir. **Simüle Edilen TemperatureSensor** modülü **IotEdgeModule1** modülüne simüle edilmiş veriler üretirken, **IotEdgeModule1** modülündeki varsayılan kod doğrudan borular Azure IoT Hub'a ileti ler alır.

Simüle edilen sıcaklık sensörünün nasıl çalıştığını görmek [için, Simüle Edilen SıcaklıkSensörü.csproj kaynak kodunu](https://github.com/Azure/iotedge/tree/master/edge-modules/SimulatedTemperatureSensor)görüntüleyin.

**IotEdgeModule1** projesi bir .NET Core 2.1 konsol uygulamasıdır, c# modülüise. Bir Windows kapsayıcısı veya Linux kapsayıcısı ile çalışan IoT Edge aygıtınız için gereken Docker dosyalarını içerir. Dosya, `module.json` bir modülün meta verilerini açıklar. Azure IoT Aygıt SDK'yı bağımlılık olarak alan gerçek modül `Program.cs` kodu, dosyada bulunur. `main.c`

## <a name="develop-your-module"></a>Modülünüzü geliştirin

Çözümle birlikte gelen varsayılan modül kodu **IotEdgeModule1** > **Program.cs** (C#) veya **main.c** (C) adresinde bulunur. Modül ve `deployment.template.json` dosya, çözümü oluşturabilecek, konteyner kayıt defterinize itebilecek ve herhangi bir koda dokunmadan test başlatmak için aygıta dağıtacak şekilde ayarlanır. Modül, bir kaynaktan (bu durumda, verileri simüle eden **Simüle Edilen Sıcaklık Sensörü** modülü) giriş almak ve azure IoT Hub'ına aktaracak şekilde oluşturulmuştür.

Modül şablonunu kendi kodunuzla özelleştirmeye hazır olduğunuzda, güvenlik, aygıt yönetimi ve güvenilirlik gibi IoT çözümlerinin temel gereksinimlerini karşılayan modüller oluşturmak için [Azure IoT Hub SDK'larını](../iot-hub/iot-hub-devguide-sdks.md) kullanın.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>IoT Edge aygıt bağlantı dizesi ile iotedgehubdev'i başlatma

1. Visual Studio Cloud Explorer'daki Birincil **Bağlantı Dizesinden** herhangi bir IoT Edge aygıtının bağlantı dizesini kopyalayın. IoT Edge aygıtının simgesi Kenar olmayan aygıtın simgesinden farklı olduğundan, Kenar olmayan bir aygıtın bağlantı dizesini kopyalamamaya devam edin.

   ![Kenar Aygıtı Bağlantı Dizesi Kopyala](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. **Araçlar** menüsünden **Azure IoT Edge Tools** > **Kurulumu IoT Edge Simulator'u**seçin, bağlantı dizesini yapıştırın ve **Tamam'ı**tıklatın.

   ![Set Kenar Bağlantı Dize Penceresi Aç](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. İlk adımdan bağlantı dizesini girin ve sonra **Tamam'ı**seçin.

> [!NOTE]
> Sonuçlar sonraki tüm Azure IoT Edge çözümlerine otomatik olarak uygulandığından, bu adımları geliştirme bilgisayarınızda yalnızca bir kez izlemeniz gerekir. Farklı bir bağlantı dizesini değiştirmeniz gerekiyorsa, bu yordam tekrar izlenebilir.

## <a name="build-and-debug-single-module"></a>Tek modül oluşturma ve hata ayıklama

Genellikle, birden çok modüliçeren tüm bir çözüm içinde çalıştırmadan önce her modülü sınamak ve hata ayıklamak istersiniz.

1. **Solution**Explorer'da, **IotEdgeModule1'e** sağ tıklayın ve bağlam menüsünden **StartUp Project olarak ayarlayın'ı** seçin.

   ![Başlangıç Projesi'ni Ayarlama](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Modülü çalıştırmak için **F5** tuşuna basın veya aşağıdaki düğmeye tıklayın; bunu ilk kez&ndash;10 20 saniye sürebilir.

   ![Modül Çalıştır](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Modül başarıyla başlatılmışsa bir .NET Core konsol uygulaması başlatılan bir uygulama görmeniz gerekir.

   ![Modül Çalıştırma](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. C#'da gelişiyorsanız, **Program.cs** `PipeMessage()` işlevinde bir kesme noktası ayarlayın; C kullanıyorsanız, **main.c** `InputQueue1Callback()` işlevinde bir kesme noktası ayarlayın. Daha sonra bir **Git Bash** veya **WSL Bash** kabuk aşağıdaki komutu çalıştırarak bir mesaj göndererek test edebilirsiniz. (Komutu `curl` PowerShell veya komut isteminden çalıştıramazsınız.)

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Hata Ayıklama Tek Modül](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Kesme noktası tetiklenmelidir. Visual Studio **Locals** penceresinden değişkenleri izleyebilirsiniz.

   > [!TIP]
   > İleti göndermek için [PostMan](https://www.getpostman.com/) veya diğer API araçlarını `curl`da kullanabilirsiniz.

1. **Ctrl + F5** tuşuna basın veya hata ayıklamayı durdurmak için durdur düğmesine tıklayın.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Birden fazla modülle IoT Edge çözümü oluşturma ve hata ayıklama

Tek bir modül geliştirmeyi bitirdikten sonra, birden çok modüliçeren tüm bir çözümü çalıştırmak ve hata ayıklamak isteyebilirsiniz.

1. **Solution Explorer'da,** **AzureIoTEdgeApp1'e** sağ tıklayarak ve Yeni**IoT Edge Modülü** **Ekle'yi** > seçerek çözüme ikinci bir modül ekleyin. İkinci modülün varsayılan adı **IotEdgeModule2'dir** ve başka bir boru modülü olarak hareket edecektir.

1. Dosyayı `deployment.template.json` açın ve **modüller** bölümünde **IotEdgeModule2** eklendiğini göreceksiniz. **Rotalar** bölümünü aşağıdakilerle değiştirin. Modül adlarınızı özelleştirmişseniz, bu adları eşleşecek şekilde güncelleştirdiğinizden emin olun.

    ```json
        "routes": {
          "IotEdgeModule1ToIoTHub": "FROM /messages/modules/IotEdgeModule1/outputs/* INTO $upstream",
          "sensorToIotEdgeModule1": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule1/inputs/input1\")",
          "IotEdgeModule2ToIoTHub": "FROM /messages/modules/IotEdgeModule2/outputs/* INTO $upstream",
          "sensorToIotEdgeModule2": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule2/inputs/input1\")"
        },
    ```

1. **AzureIoTEdgeApp1'e** sağ tıklayın ve bağlam menüsünden **Başlangıç Projesi olarak ayarla'yı** seçin.

1. Kesme noktalarınızı oluşturun ve birden çok modülü aynı anda çalıştırmak ve hata ayıklamak için **F5** tuşuna basın. Her pencerenin farklı bir modülü temsil ettiği birden çok .NET Core konsol uygulaması penceresi görmeniz gerekir.

   ![Hata Ayıklama Çoklu Modülleri](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. **Ctrl + F5** tuşuna basın veya hata ayıklamayı durdurmak için dur düğmesini seçin.

## <a name="build-and-push-images"></a>Görüntüler oluşturma ve itme

1. **AzureIoTEdgeApp1'in** başlangıç projesi olduğundan emin olun. Modül görüntüleriniz için yapılandırma olarak **Hata Ayıklama** veya **Sürüm'u** seçin.

    > [!NOTE]
    > **Hata Ayıklama**seçerken, `Dockerfile.(amd64|windows-amd64).debug` Visual Studio Docker görüntüleri oluşturmak için kullanır. Buna, konteyner resminizde .NET Core komut satırı hata ayıklama vsdbg içerir. Üretime hazır IoT Edge modülleri için, **Release** VSDBG olmadan `Dockerfile.(amd64|windows-amd64)` kullanılan Release yapılandırmasını kullanmanızı öneririz.

1. Azure Kapsayıcı Kayıt Defteri (ACR) gibi özel bir kayıt defteri kullanıyorsanız, oturum açabilmek için aşağıdaki Docker komutunu kullanın.  Azure portalındaki kayıt defterinizin **Erişim tuşları** sayfasından kullanıcı adı ve parolayı alabilirsiniz. Yerel kayıt defteri kullanıyorsanız, [yerel bir kayıt defteri çalıştırabilirsiniz.](https://docs.docker.com/registry/deploying/#run-a-local-registry)

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Azure Kapsayıcı Kayıt Defteri gibi özel bir kayıt defteri kullanıyorsanız, kayıt defteri giriş bilgilerinizi dosyada `deployment.template.json`bulunan çalışma zamanı ayarlarına eklemeniz gerekir. Yer tutucuları gerçek ACR yönetici kullanıcı adınızı, parolanızı ve kayıt defteri adınızı değiştirin.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

1. **Solution Explorer'da** **AzureIoTEdgeApp1'e** sağ tıklayın ve her modül için Docker görüntüsünü oluşturmak ve itmek için **IoT Edge Modülleri Oluştur ve Itme'yi** seçin.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Visual Studio için Bulut Gezgini'ni kullanarak modülleri de dağıtabilirsiniz. Zaten senaryonuz, dosyanız `deployment.json` için hazırlanmış bir dağıtım bildiriminiz var ve tek yapmanız gereken dağıtımı almak için bir aygıt seçmektir.

1. Bulut **Gezginini** **Görüntüle'yi** > tıklatarak Bulut**Gezgini'ni**açın. Visual Studio 2019'a giriş yaptığınızdan emin olun.

1. **Bulut Gezgini'nde**aboneliğinizi genişletin, Azure IoT Hub'ınızı ve dağıtmak istediğiniz Azure IoT Edge aygıtını bulun.

1. Dağıtım oluşturmak için IoT Edge aygıtına sağ tıklayın. Visual Studio çözümünüzdeki **config** klasöründe bulunan platformunuz için yapılandırılan dağıtım `deployment.arm32v7.json`bildirimine gidin, örneğin.

1. **Simüle Edilen TemperatureSensor** modülü ve **$edgeAgent** ve $edgeHub ile birlikte çalışan yeni modülleri görmek için yenile **düğmesini**tıklatın.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. Belirli bir IoT-Edge aygıtının D2C iletisini izlemek için, **Bulut Gezgini'ndeki** IoT hub'ınızda seçin ve ardından **Eylem** penceresinde İzleme Yerleşik Olay Bitiş **Noktasını Başlat'ı** tıklatın.

1. Verileri izlemeyi durdurmak **için, Eylem** penceresinde **Yerleşik Olay Bitiş Noktasını Izlemeyi Durdur'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge aygıtlarınız için özel modüller geliştirmek için [Azure IoT Hub SDK'larını anlayın ve kullanın.](../iot-hub/iot-hub-devguide-sdks.md)
