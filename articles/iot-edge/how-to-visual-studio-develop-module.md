---
title: Visual Studio 'da modül geliştirme ve hata ayıklama-Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge için modülleri geliştirmek ve hatalarını ayıklamak için Visual Studio 2019 kullanın
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 9cec4c436c6e8ea08e37ec0ddd8a9a01e493447c
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561691"
---
# <a name="use-visual-studio-2019-to-develop-and-debug-modules-for-azure-iot-edge"></a>Azure IoT Edge için modülleri geliştirmek ve hatalarını ayıklamak için Visual Studio 2019 kullanın

İş mantığınızı Azure IoT Edge için modüllere dönüştürebilirsiniz. Bu makalede, modülleri geliştirmek ve hatalarını ayıklamak için Visual Studio 2019 ' i ana araç olarak nasıl kullanacağınız gösterilmektedir.

Visual Studio için Azure IoT Edge araçları aşağıdaki avantajları sağlar:

- Yerel geliştirme bilgisayarınızda Azure IoT Edge çözümleri ve modülleri oluşturun, düzenleyin, derleyin, çalıştırın ve hata ayıklayın.
- Azure IoT Edge çözümünüzü Azure IoT Hub aracılığıyla Azure IoT Edge cihaza dağıtın.
- Azure IoT modüllerinizi C 'de veya C# Visual Studio geliştirmenin tüm avantajlarına sahip olacak şekilde kodlayın.
- Azure IoT Edge cihazları ve modülleri UI ile yönetin.

Bu makalede, IoT Edge modüllerinizi geliştirmek için Visual Studio 2019 Azure IoT Edge araçlarının nasıl kullanılacağı gösterilmektedir. Ayrıca, projenizi Azure IoT Edge cihazınıza dağıtmayı de öğreneceksiniz. Şu anda Visual Studio 2019, C ve C#içinde yazılmış modüller için destek sağlar. Desteklenen cihaz mimarileri Windows x64 ve Linux x64 veya ARM32. Desteklenen işletim sistemleri, diller ve mimariler hakkında daha fazla bilgi için bkz. [dil ve mimari desteği](module-development.md#language-and-architecture-support).
  
## <a name="prerequisites"></a>Önkoşullar

Bu makalede, geliştirme makineniz olarak Windows çalıştıran bir bilgisayar veya sanal makine kullandığınızı varsayar. Windows bilgisayarlarda, Windows veya Linux modülleri geliştirebilirsiniz. Windows modülleri geliştirmek için 1809/Build 17763 veya daha yeni sürümünü çalıştıran bir Windows bilgisayarı kullanın. Linux modülleri geliştirmek için, [Docker Desktop gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan bir Windows bilgisayarı kullanın. 

Bu makale, ana geliştirme aracı olarak Visual Studio 2019 ' i kullandığından, Visual Studio 'yu yükler. Visual Studio 2019 yüklemenizde **Azure geliştirme** ve **masaüstü geliştirme C++**  iş yüklerine dahil ettiğinizden emin olun. Gerekli iş yüklerini eklemek için [Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019) ' i değiştirebilirsiniz.

Visual Studio 2019 ' i kullanmaya başladıktan sonra, aşağıdaki araçlara ve bileşenlere de ihtiyacınız vardır:

- Visual Studio 2019 ' de bir IoT Edge projesi oluşturmak için Visual Studio marketi ' nden [Azure IoT Edge araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) indirin ve yükleyin.

> [!TIP]
> Visual Studio 2017 kullanıyorsanız, lütfen Visual Studio marketi 'nden VS 2017 için [Azure IoT Edge araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) indirip yükleyin

- Modül görüntülerinizi derlemek ve çalıştırmak için, geliştirme makinenize [Docker Community Edition](https://docs.docker.com/install/) indirin ve yükleyin. Docker CE 'yi Linux kapsayıcı modunda veya Windows kapsayıcı modunda çalışacak şekilde ayarlamanız gerekir.

- [Azure IoT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/)'nı yükleyerek IoT Edge çözümünüzü hata ayıklamak, çalıştırmak ve test etmek için yerel geliştirme ortamınızı ayarlayın. [Python (2.7/3.6 +) ve PIP](https://www.python.org/) 'yi yükleyip terminalinizde aşağıdaki komutu çalıştırarak **ıotedgehubdev** paketini yükledikten sonra. Azure IoT EdgeHub geliştirme aracı sürümünün 0.3.0 'den büyük olduğundan emin olun.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Depoyu kopyalayın ve Vcpkg kitaplığı yöneticisini yükledikten sonra Windows için **Azure-IoT-SDK-c paketini** yükledikten sonra.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) veya [Docker Hub 'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > Yerel bir Docker kayıt defteri 'ni, bir bulut kayıt defteri yerine prototip ve test amacıyla kullanabilirsiniz.

- Modülünüzü bir cihazda test etmek için en az bir IoT Edge cihazıyla etkin bir IoT Hub 'ınız olması gerekir. Bilgisayarınızı IoT Edge bir cihaz olarak kullanmak için, [Linux](quickstart-linux.md) veya [Windows](quickstart.md)Hızlı Başlangıç bölümündeki adımları izleyin. Geliştirme makinenizde IoT Edge Daemon çalıştırıyorsanız, Visual Studio 'da geliştirmeye başlamadan önce EdgeHub ve EdgeAgent 'ı durdurmanız gerekebilir.

### <a name="check-your-tools-version"></a>Araçlar sürümünüzü denetleyin

1. **Araçlar** menüsünde **Uzantılar ve güncelleştirmeler**' i seçin. **Yüklü > araçları** ' nı genişletin ve **Visual Studio için** **Azure IoT Edge araçları** ve bulut Gezgini ' ni bulabilirsiniz.

1. Yüklü sürümü göz önünde edin. Bu sürümü Visual Studio Market en son sürümüyle karşılaştırabilirsiniz ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS2019), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools))

1. Sürümünüz Visual Studio Market ' den daha eskiyse, Visual Studio 'da aşağıdaki bölümde gösterildiği gibi araçlarınızı güncelleştirin.

### <a name="update-your-tools"></a>Araçlarınızı güncelleştirme

1. **Uzantılar ve güncelleştirmeler** iletişim kutusunda **Visual Studio Market > güncelleştirmeler**' i genişletin, **Azure IoT Edge araçlar** ' ı veya **Visual Studio Için bulut Gezgini** ' ni seçin ve **Güncelleştir**' i seçin.

1. Araçlar güncelleştirmesi indirildikten sonra, VSıX yükleyicisini kullanarak araçlar güncelleştirmesini tetiklemek için Visual Studio 'Yu kapatın.

1. Yükleyicide başlamak için **Tamam** ' ı seçin ve ardından araçları güncelleştirmek için **değiştirin** .

1. Güncelleştirme tamamlandıktan sonra, **Kapat** ' ı seçin ve Visual Studio 'yu yeniden başlatın.

### <a name="create-an-azure-iot-edge-project"></a>Azure IoT Edge projesi oluşturma

Visual Studio 'daki Azure IoT Edge proje şablonu, Azure IoT Hub Azure IoT Edge cihazlara dağıtılabilecek bir proje oluşturur. İlk olarak bir Azure IoT Edge çözümü oluşturun ve ardından bu çözümdeki ilk modülü oluşturun. Her bir IoT Edge çözümü birden fazla modül içerebilir.

> [!TIP]
> Visual Studio tarafından oluşturulan IoT Edge proje yapısı Visual Studio Code ile aynı değildir.

1. Visual Studio yeni proje iletişim kutusunda **Azure IoT Edge** proje ' yi arayıp seçin ve **İleri**' ye tıklayın. Proje yapılandırması penceresinde, projeniz için bir ad girin ve konumu belirtin ve ardından **Oluştur**' u seçin. Varsayılan proje adı **AzureIoTEdgeApp1**' dir.

   ![Yeni proje oluştur](./media/how-to-visual-studio-develop-csharp-module/create-new.png)

1. **IoT Edge uygulama ve Modül Ekle** penceresinde,  **C# Module** veya **C modülünü** seçin ve ardından modül adı ve modül görüntü deponuzu belirtin. Visual Studio oto modül adını **localhost: 5000/< modülünüzün adı\>** olarak doldurur. Kendi kayıt defteri bilgileriniz ile değiştirin. Test için yerel bir Docker kayıt defteri kullanıyorsanız, **localhost** sorunsuz olur. Azure Container Registry kullanırsanız, kayıt defterinizin ayarlarından oturum açma sunucusunu kullanın. Oturum açma sunucusu,  **_\<\>. azurecr.io kayıt defteri adı_** gibi görünür. Yalnızca dizenin **localhost: 5000** kısmını, son sonucun **\<*kayıt defteri adı*\>. azurecr.io/ _\<modülünüzün adı\>_** gibi görünmesi için değiştirin. Varsayılan modül adı **IotEdgeModule1**

   ![Uygulama ve Modül Ekle](./media/how-to-visual-studio-develop-csharp-module/add-application-and-module.png)

1. Ya da C# C kullanan bir modülle Azure IoT Edge çözümünü oluşturmak için Tamam ' ı seçin.

Artık bir **AzureIoTEdgeApp1. Linux. amd64** projeniz veya bir **AzureIoTEdgeApp1. Windows. amd64** projeniz ve ayrıca çözümünüzde bir **IotEdgeModule1** projesi vardır. Her bir **AzureIoTEdgeApp1** projesi, IoT Edge çözümünüz için derlemek ve dağıtmak istediğiniz modülleri tanımlayan bir `deployment.template.json` dosyasına sahiptir ve ayrıca modüller arasındaki yolları tanımlar. Varsayılan çözümde bir **SimulatedTemperatureSensor** modülü ve bir **IotEdgeModule1** modülü vardır. **SimulatedTemperatureSensor** modülü, **IotEdgeModule1** modülüne sanal verileri üretir, ancak **IotEdgeModule1** modülündeki varsayılan kod, Azure IoT Hub ileti alma işlemleri için doğrudan kanallar sağlar.

Sanal sıcaklık algılayıcısı 'nın nasıl çalıştığını görmek için [SimulatedTemperatureSensor. csproj kaynak kodunu](https://github.com/Azure/iotedge/tree/master/edge-modules/SimulatedTemperatureSensor)görüntüleyin.

**IotEdgeModule1** projesi bir C# modülle bir .NET Core 2,1 konsol uygulamasıdır. Windows kapsayıcısı veya Linux kapsayıcısı ile çalışan IoT Edge cihazınız için gereken gerekli Docker dosyalarını içerir. `module.json` dosyası bir modülün meta verilerini açıklar. Azure IoT cihaz SDK 'sını bağımlılık olarak alan gerçek modül kodu `Program.cs` veya `main.c` dosyasında bulunur.

## <a name="develop-your-module"></a>Modülünüzü geliştirme

Çözümle birlikte gelen varsayılan modül kodu **IotEdgeModule1** > **program.cs** (for C#) veya **Main. c** (c) konumunda bulunur. Modül ve `deployment.template.json` dosyası, çözümü derlemenize, kapsayıcı Kayıt defterinize gönderebilmeniz ve herhangi bir koda dokunmadan teste başlamak için bir cihaza dağıtabilmeniz için ayarlanır. Modül bir kaynaktan giriş almak için oluşturulmuştur (Bu durumda, verileri taklit eden **SimulatedTemperatureSensor** modülü) ve Azure IoT Hub 'yi kanal oluşturarak.

Modül şablonunu kendi kodunuzla özelleştirmeye hazır olduğunuzda, güvenlik, cihaz yönetimi ve güvenilirlik gibi IoT çözümlerinin temel ihtiyaçlarını ele alan modüller oluşturmak için [Azure IoT Hub SDK](../iot-hub/iot-hub-devguide-sdks.md) 'larını kullanın.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>IoT Edge cihaz bağlantı dizesiyle ıotedgehubdev başlatma

1. Herhangi bir IoT Edge cihazının bağlantı dizesini Visual Studio Cloud Explorer 'daki **birincil bağlantı dizesinden** kopyalayın. Bir IoT Edge cihazının simgesi Edge olmayan bir cihazın simgesinden farklı olduğundan, uç olmayan bir cihazın bağlantı dizesini kopyalamadığınızdan emin olun.

   ![Sınır cihaz bağlantı dizesini Kopyala](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. **Araçlar** > **Azure IoT Edge araçlar** **IoT Edge simülatör > kurulum**' a gidin, bağlantı dizesini yapıştırın ve **Tamam**' a tıklayın.

   ![Uç bağlantı dizesini ayarla penceresini aç](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. İlk adımdan bağlantı dizesini girip **Tamam**' ı seçin.

> [!NOTE]
> Sonuçlar, sonraki tüm Azure IoT Edge çözümlerine otomatik olarak uygulandığından, geliştirme bilgisayarınızda yalnızca bir kez bu adımları izlemeniz gerekir. Bu yordam, farklı bir bağlantı dizesine değiştirmeniz gerekiyorsa yeniden izlenebilir.

## <a name="build-and-debug-single-module"></a>Tek modülle derleme ve hata ayıklama

Genellikle, birden çok modülle bir çözümün tamamında çalıştırmadan önce her modülü test etmek ve hatalarını ayıklamak isteyeceksiniz.

1. **IotEdgeModule1** öğesine sağ tıklayın ve bağlam menüsünden **Başlangıç projesi olarak ayarla** ' yı seçin.

   ![Başlangıç projesini ayarla](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Modülü çalıştırmak için **F5** tuşuna basın veya aşağıdaki düğmeye tıklayın; Bu işlem ilk kez&ndash;20 saniye sürebilir.

   ![Modül Çalıştır](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Modül başarıyla başlatılmışsa bir .NET Core konsol uygulaması başlatın.

   ![Modül çalışıyor](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. İçinde C#geliştirme yapıyorsanız, **program.cs**içinde `PipeMessage()` işlevinde bir kesme noktası ayarlayın. C kullanıyorsanız **Main. C**içindeki `InputQueue1Callback()` işlevinde bir kesme noktası ayarlayın. Ardından, **Git Bash** veya **WSL Bash** kabuğu 'nda aşağıdaki komutu çalıştırarak bir ileti göndererek test edebilirsiniz. (`curl` komutunu bir PowerShell veya komut isteminden çalıştıramazsınız.)

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Tek modülün hatalarını ayıkla](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Kesme noktasının tetiklenmesi gerekir. Değişkenleri Visual Studio **Yereller** penceresinde izleyebilirsiniz.

   > [!TIP]
   > `curl`yerine ileti göndermek için [Postman](https://www.getpostman.com/) veya diğer API araçlarını da kullanabilirsiniz.

1. Hata ayıklamayı durdurmak için **CTRL + F5** tuşlarına basın veya Durdur düğmesine tıklayın.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Birden çok modülle IoT Edge çözüm oluşturma ve hata ayıklama

Tek bir modülün geliştirilmesini tamamladıktan sonra, birden çok modülle bir çözümün tamamını çalıştırmak ve hatalarını ayıklamak isteyebilirsiniz.

1. **AzureIoTEdgeApp1** öğesine sağ tıklayıp > **Yeni IoT Edge modülü** **Ekle** ' yi seçerek çözüme ikinci bir modül ekleyin. İkinci modülün varsayılan adı **IotEdgeModule2** ' dür ve başka bir kanal modülü olarak görev görür.

1. Dosya `deployment.template.json` açın, **modüller** bölümüne **IotEdgeModule2** eklendiğini görürsünüz. **Rotalar** bölümünü aşağıdaki kodla değiştirin. Modül adlarınızı özelleştirdiyseniz, bu adları eşleşecek şekilde güncelleştirdiğinizden emin olun.

    ```json
        "routes": {
          "IotEdgeModule1ToIoTHub": "FROM /messages/modules/IotEdgeModule1/outputs/* INTO $upstream",
          "sensorToIotEdgeModule1": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule1/inputs/input1\")",
          "IotEdgeModule2ToIoTHub": "FROM /messages/modules/IotEdgeModule2/outputs/* INTO $upstream",
          "sensorToIotEdgeModule2": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule2/inputs/input1\")"
        },
    ```

1. **AzureIoTEdgeApp1** öğesine sağ tıklayın ve bağlam menüsünden **Başlangıç projesi olarak ayarla** ' yı seçin.

1. Kesme noktalarınızı oluşturun ve ardından aynı anda birden fazla modülü çalıştırmak ve hatalarını ayıklamak için **F5** 'e basın Farklı bir modülü temsil eden birden çok .NET Core konsol uygulaması penceresi görmeniz gerekir.

   ![Birden çok modülün hatalarını ayıklama](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Hata ayıklamayı durdurmak için **CTRL + F5** tuşlarına basın veya Durdur düğmesini seçin.

## <a name="build-and-push-images"></a>Görüntü oluşturma ve gönderme

1. **AzureIoTEdgeApp1** 'in başlangıç projesi olduğundan emin olun. Modül görüntüleriniz için derlemek üzere yapılandırma olarak **Hata Ayıkla** veya **Yayınla** ' yı seçin.

    > [!NOTE]
    > **Hata ayıklamayı**seçerken, Visual Studio Docker görüntülerini derlemek için `Dockerfile.(amd64|windows-amd64).debug` kullanır. Bu, derleme sırasında kapsayıcı görüntünde .NET Core komut satırı hata ayıklayıcısı VSDBG içerir. Üretime yönelik kullanıma yönelik IoT Edge modüller için, VSDBG olmayan `Dockerfile.(amd64|windows-amd64)` kullanan **yayın** yapılandırmasını kullanmanızı öneririz.

1. Azure Container Registry gibi özel bir kayıt defteri kullanıyorsanız, oturum açmak için aşağıdaki Docker komutunu kullanın. Yerel kayıt defteri kullanıyorsanız [yerel bir kayıt defteri çalıştırabilirsiniz](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Azure Container Registry gibi özel bir kayıt defteri kullanıyorsanız, kayıt defteri oturum açma bilgilerinizi dosya `deployment.template.json`bulunan çalışma zamanı ayarlarına eklemeniz gerekir. Yer tutucuları gerçek ACR yönetici kullanıcı adınızla, parolanızla ve kayıt defteri adıyla değiştirin.

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

1. **AzureIoTEdgeApp1** öğesine sağ tıklayın ve her modül Için Docker görüntüsünü derlemek ve göndermek üzere **Oluştur ve Gönder uç çözümünü** seçin.

   ![Görüntü oluşturma ve gönderme](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)

## <a name="deploy-the-solution"></a>Çözümü dağıtma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Ayrıca, Visual Studio için Cloud Explorer 'ı kullanarak modüller dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildiriminiz zaten var, `deployment.json` dosyası ve tüm yapmanız gereken, dağıtımı alacak bir cihaz seçin.

1. **Cloud explorer > ** **görüntüle** ' ye tıklayarak **Cloud Explorer 'ı** açın. Visual Studio 2019 ' de oturum açtığınızdan emin olun.

1. **Cloud Explorer**'da aboneliğinizi genişletin, Azure IoT Hub ve dağıtmak istediğiniz Azure IoT Edge cihazını bulun.

1. Dağıtım oluşturmak için IoT Edge cihaza sağ tıklayın, `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`altında dağıtım bildirim dosyasını seçmeniz gerekir.

   > [!NOTE]
   > `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json` seçmemelidir

1. **SimulatedTemperatureSensor** modülüyle ve **$edgeAgent** ve **$edgeHub**birlikte çalışan yeni modülleri görmek için Yenile düğmesine tıklayın.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. Belirli bir cihaz için D2C iletisini izlemek üzere, listeden cihazı seçin ve ardından **eylem** penceresinde **yerleşik olay uç noktasını izlemeye başla** ' ya tıklayın.

1. İzleme verilerini durdurmak için listeden cihazı seçin ve ardından **eylem** penceresinde **yerleşik olay uç noktasını İzlemeyi Durdur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge cihazlarınız için özel modüller geliştirmek üzere [Azure IoT Hub SDK 'Larını anlayın ve kullanın](../iot-hub/iot-hub-devguide-sdks.md).
