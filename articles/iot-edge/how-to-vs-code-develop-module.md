---
title: Azure IoT Edge için modüller geliştirin ve hata ayıklayın | Microsoft Docs
description: ', Python, Node. js, Java veya C kullanarak C#Azure IoT Edge için modül geliştirmek, derlemek ve hata ayıklamak için Visual Studio Code kullanın'
services: iot-edge
keywords: ''
author: shizn
ms.author: xshi
ms.date: 08/07/2019
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 5b37ea92869468001581c9299b1633869671886a
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457070"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Azure IoT Edge için modülleri geliştirmek ve hatalarını ayıklamak için Visual Studio Code kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede, modülleri geliştirme ve hata ayıklama için ana araç olarak Visual Studio Code nasıl kullanacağınız gösterilmektedir.

Visual Studio Code ' de, Node. js veya Java C#'da yazılan modüllerde hata ayıklamanın iki yolu vardır: bir işlemi bir modül kapsayıcısına iliştirebilir veya hata ayıklama modunda modül kodunu başlatabilirsiniz. Python veya C 'de yazılan modüllerde hata ayıklamak için yalnızca Linux AMD64 kapsayıcılarındaki bir işleme ekleyebilirsiniz.

Visual Studio Code hata ayıklama yeteneklerini bilmiyorsanız, [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging)hakkında bilgi edinin.

Bu makalede birden çok mimaride birden çok dilde modül geliştirme ve hata ayıklama için yönergeler sağlanmaktadır. Şu anda Visual Studio Code, C, Python, Node C#. js ve Java 'da yazılan modüller için destek sağlar. Desteklenen cihaz mimarileri x64 ve ARM32. Desteklenen işletim sistemleri, diller ve mimariler hakkında daha fazla bilgi için bkz. [dil ve mimari desteği](module-development.md#language-and-architecture-support).

>[!NOTE]
>Linux ARM64 cihazları için geliştirme ve hata ayıklama desteği [genel önizlemeye](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)sunuldu. Daha fazla bilgi için bkz. [Visual Studio Code IoT Edge modüllerini geliştirme ve hata ayıklama (Önizleme)](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview).

## <a name="prerequisites"></a>Ön koşullar

Geliştirme makineniz olarak Windows, macOS veya Linux çalıştıran bir bilgisayar veya sanal makine kullanabilirsiniz. Windows bilgisayarlarda, Windows veya Linux modülleri geliştirebilirsiniz. Windows modülleri geliştirmek için 1809/Build 17763 veya daha yeni sürümünü çalıştıran bir Windows bilgisayarı kullanın. Linux modülleri geliştirmek için, [Docker Desktop gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan bir Windows bilgisayarı kullanın. 

Önce [Visual Studio Code](https://code.visualstudio.com/) yükleyip aşağıdaki uzantıları ekleyin:

- [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Geliştirmekte olduğunuz dile özel Visual Studio uzantıları:
  - C#Azure işlevleri dahil: [ C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Visual Studio Code Için Java Uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [c/C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Ayrıca modülünüzü geliştirmek için dile özgü bazı ek araçlar yüklemeniz gerekir:

- C#Azure Işlevleri dahil: [.NET Core 2,1 SDK](https://www.microsoft.com/net/download)

- Python: Python paketlerini yüklemek için [Python](https://www.python.org/downloads/) ve [PIP](https://pip.pypa.io/en/stable/installing/#installation) (genellikle Python yüklemenize dahildir).

- Node. js: [Node. js](https://nodejs.org). [Yeumman](https://www.npmjs.com/package/yo) ve [Azure IoT Edge Node. js modül oluşturucuyu](https://www.npmjs.com/package/generator-azure-iot-edge-module)de yüklemek isteyeceksiniz.

- Java: [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [Maven](https://maven.apache.org/). [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize işaret etmek için ayarlamanız gerekir.

Modül görüntünüzü derlemek ve dağıtmak için, modül görüntüsünü ve modül görüntüsünü tutacak bir kapsayıcı kayıt defteri oluşturmak üzere Docker gereklidir:

- Geliştirme makinenizde [Docker Community Edition](https://docs.docker.com/install/) .

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) veya [Docker Hub 'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz.

Modülünüzü C 'de geliştirmediğiniz takdirde, IoT Edge çözümünüzü hata ayıklamak, çalıştırmak ve test etmek üzere yerel geliştirme ortamınızı ayarlamak için Python tabanlı [Azure IoT EdgeHub geliştirme aracına](https://pypi.org/project/iotedgehubdev/) da ihtiyacınız vardır. Daha önce yapmadıysanız, [Python (2.7/3.6/3.7) ve PIP](https://www.python.org/) 'yi yükleyip bu komutu terminalinizde çalıştırarak **ıotedgehubdev** ' i yükleyebilirsiniz.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```
> [!NOTE]
> Şu anda, ıotedgehubdev, Python 3,8 ile uyumlu olmayan bir Docker-çi kitaplığı kullanıyor.
>
> Önceden yüklenmiş Python 2,7 (örneğin, Ubuntu veya macOS) dahil birden çok Python varsa, **ıotedgehubdev** 'yi yüklemek için doğru `pip` veya `pip3` kullandığınızdan emin olun

Modülünüzü bir cihazda test etmek için en az bir IoT Edge cihazıyla etkin bir IoT Hub 'ınız olması gerekir. Bilgisayarınızı IoT Edge bir cihaz olarak kullanmak için, [Linux](quickstart-linux.md) veya [Windows](quickstart.md)Hızlı Başlangıç bölümündeki adımları izleyin. Geliştirme makinenizde IoT Edge Daemon çalıştırıyorsanız, bir sonraki adıma geçmeden önce EdgeHub ve EdgeAgent 'ı durdurmanız gerekebilir.

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturun

Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT araçları kullanarak tercih ettiğiniz geliştirme dilinizde (Azure Işlevleri de dahil olmak üzere C#) IoT Edge bir modülün nasıl oluşturulacağı gösterilmektedir. Bir çözüm oluşturup sonra bu çözümdeki ilk modülü oluşturarak başlayın. Her çözüm birden çok modül içerebilir.

1.  > **komut paletini** **görüntüle** ' yi seçin.

1. Komut paletinde, **Azure IoT Edge: yeni IoT Edge çözümü**komutunu girin ve çalıştırın.

   ![Yeni IoT Edge çözüm Çalıştır](./media/how-to-develop-csharp-module/new-solution.png)

1. Yeni çözümü oluşturmak istediğiniz klasöre gidin ve ardından **Klasör Seç**' i seçin.

1. Çözümünüz için bir ad girin.

1. Tercih ettiğiniz geliştirme diliniz için çözümdeki ilk modül olacak bir modül şablonu seçin.

1. Modülünüzün adını girin. Kapsayıcı kayıt defteriniz dahilinde benzersiz olan bir ad seçin.

1. Modülün görüntü deposunun adını belirtin. Visual Studio Code, modül adını **localhost: 5000/< modülünüzün adı\>** olarak doldurur. Kayıt defteri kendi bilgilerinizle değiştirin. Test için yerel bir Docker kayıt defteri kullanıyorsanız, **localhost** sorunsuz olur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu,  **_\<\>. azurecr.io kayıt defteri adı_** gibi görünür. Yalnızca dizenin **localhost: 5000** kısmını, son sonucun **\<*kayıt defteri adı*\>. azurecr.io/ _\<modülünüzün adı\>_** gibi görünmesi için değiştirin.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code verdiğiniz bilgileri alır, bir IoT Edge çözümü oluşturur ve sonra yeni bir pencerede yükler.

Çözüm içinde dört öğe vardır:

- Bir **. vscode** klasörü hata ayıklama yapılandırması içerir.

- **Modüller** klasörü her modül için alt klasörlere sahiptir.  Her modülün klasörü içinde, modüllerin nasıl oluşturulup dağıtıldığını denetleyen **Module. JSON**dosyası vardır.  Bu dosyanın modül dağıtım kapsayıcısı kayıt defteri 'ni localhost 'dan uzak bir kayıt defterine değiştirmek için değiştirilmesi gerekir. Bu noktada yalnızca bir modüldür.  Ancak, komut paletinde **Azure IoT Edge: add IoT Edge Module**komutuyla daha fazla ek ekleyebilirsiniz.

- Bir **. env** dosyası ortam değişkenlerinizi listeler. Kayıt defteriniz Azure Container Registry, içinde bir Azure Container Registry Kullanıcı adı ve parola görürsünüz.

  > [!NOTE]
  > Ortam dosyası yalnızca modül için bir görüntü deposu sağlarsanız oluşturulur. Yerel olarak test etme ve hata ayıklama için localhost varsayılan değerlerini kabul ettiyseniz, ortam değişkenlerini bildirmeniz gerekmez.

- Bir **Deployment. Template. JSON** dosyası yeni modülünüzü, test için kullanabileceğiniz verileri taklit eden örnek bir **SimulatedTemperatureSensor** modülüyle birlikte listeler. Dağıtım bildirimlerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [dağıtım bildirimlerini kullanarak modül dağıtma ve rotalar oluşturma](module-composition.md)hakkında bilgi edinin.

## <a name="add-additional-modules"></a>Ek modüller ekleme

Çözümünüze ek modüller eklemek için, komut paletinden **Azure IoT Edge: IoT Edge modülü Ekle** komutunu çalıştırın. Ayrıca, Visual Studio Code Gezgini görünümündeki **modüller** klasörüne veya `deployment.template.json` dosyasına sağ tıklayıp **IoT Edge modülü Ekle**' yi seçebilirsiniz.

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümle birlikte gelen varsayılan modül kodu şu konumda bulunur:

- Azure işlevi (C#): **modüller *&lt;* modül adınızı&gt; > modülünüzün *adı* &lt;>. cs**&gt;
- C#: **modüller > *&lt;modül adınızı&gt;* > program.cs**
- Python: **modüller > *&lt;modül adınızı&gt;* > Main.py**
- Node. js: **modüller > *&lt;modül adınızı&gt;* app. js >**
- Java: **modüller > *&lt;&gt;modül adınızı* > src > main > java > com > edgemodulemodules > App. Java**
- C: **modüller > *&lt;modül adınızı&gt;* > Main. c**

Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, bir kaynaktan giriş yapmak için oluşturulmuştur (Bu durumda, verileri taklit eden SimulatedTemperatureSensor modülü) ve IoT Hub için kanal oluşturma.

Şablonu kendi kodunuzla özelleştirmeye hazır olduğunuzda, güvenlik, cihaz yönetimi ve güvenilirlik gibi IoT çözümlerinin temel ihtiyaçlarını ele alan modüller oluşturmak için [Azure IoT Hub SDK](../iot-hub/iot-hub-devguide-sdks.md) 'larını kullanın.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Kapsayıcı olmadan bir modülün hatalarını ayıklama (C#, Node. js, Java)

, Node. js veya C#Java 'da geliştiriyorsanız, modülünüzün iletileri başlatabilmesi, çalıştırmaları ve yönlendirebilmesi için varsayılan modül kodunda bir **moduleclient** nesnesinin kullanılması gerekir. Ayrıca, modül iletileri aldığında işlem yapmak için varsayılan **input1** giriş kanalını da kullanacaksınız.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge çözümü için IoT Edge simülatör ayarlama

Geliştirme makinenizde, IoT Edge çözümünüzü çalıştırabilmeniz için IoT Edge güvenlik cini yüklemek yerine IoT Edge simülatörü başlatabilirsiniz.

1. Cihaz Gezgini ' nde, sol taraftaki IoT Edge cihaz KIMLIĞINIZ ' ne sağ tıklayın ve ardından cihaz bağlantı dizesiyle simülatör 'yi başlatmak için **IoT Edge simülatör kurulum** ' u seçin.
1. Tümleşik terminalde ilerleme ayrıntısı ' nı okuyarak IoT Edge Benzeticisinin başarıyla ayarlandığını görebilirsiniz.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>Tek modüllü uygulama için IoT Edge simülatör ayarlama

Simülatörü ayarlamak ve başlatmak için, Visual Studio Code komut paletinden **tek bir modül için Azure IoT Edge: IoT Edge hub simülatörü başlatma** komutunu çalıştırın. İstendiğinde, uygulamanızın giriş adı olarak varsayılan modül kodundan (veya kodunuzda eşdeğer değerden) **input1** değerini kullanın. Komut, **ıotedgehubdev** CLI öğesini tetikler ve ardından IoT Edge simülatörü ve bir test yardımcı programı modül kapsayıcısını başlatır. Simülatör Tek modüllü modda başarıyla başlatılmışsa, tümleşik terminalde aşağıdaki çıktıları görebilirsiniz. Ayrıca, ile ileti gönderilmesine yardımcı olması için bir `curl` komutu görebilirsiniz. Daha sonra bu adı kullanacaksınız.

   ![Tek modüllü uygulama için IoT Edge simülatör ayarlama](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Modülün çalışır durumda olduğunu görmek için Visual Studio Code Docker Gezgin görünümünü kullanabilirsiniz.

   ![Simülatör modülü durumu](media/how-to-develop-csharp-module/simulator-status.png)

   **Edgehubdev** kapsayıcısı, yerel IoT Edge simülatör 'in temel sayısıdır. Geliştirme makinenizde IoT Edge güvenlik cini olmadan çalışabilir ve yerel modül uygulamanız veya modül kapsayıcılarınız için ortam ayarları sağlar. **Giriş** kapsayıcısı, iletilerinizi modüldeki hedef giriş kanalına KÖPRÜLEMEK Için REST API 'lerini kullanıma sunar.

### <a name="debug-module-in-launch-mode"></a>Başlatma modunda hata ayıklama modülü

1. Geliştirme dilinizin gereksinimlerine göre ortamınızı hata ayıklama için hazırlayın, modülünüzün bir kesme noktası ayarlayın ve kullanılacak hata ayıklama yapılandırmasını seçin:
   - **C#**
     - Visual Studio Code tümleşik terminalde, dizini ***Modül adı&gt;klasörü&lt;*** değiştirin ve ardından .NET Core uygulaması derlemek için aşağıdaki komutu çalıştırın.

       ```cmd
       dotnet build
       ```

     - `Program.cs` dosyasını açın ve bir kesme noktası ekleyin.

     - Visual Studio Code hata ayıklama görünümüne giderek **görünüm > hata ayıkla**' yı seçin. ***Modül adınızın* açılan menüden yerel hata ayıklama (.net Core)&gt;&lt;** hata ayıklama yapılandırmasını seçin.

        > [!NOTE]
        > .NET Core `TargetFramework` `launch.json`program yolunuza uygun değilse, Visual Studio Code bu programı başarıyla başlatabilmesi için `launch.json` program yolunu. csproj dosyanızdaki `TargetFramework` eşleşecek şekilde el ile güncelleştirmeniz gerekir.

   - **Node.js**
     - Visual Studio Code tümleşik terminalde, dizini ***Modül adı&gt;klasörü&lt;*** değiştirin ve ardından düğüm paketlerini yüklemek için aşağıdaki komutu çalıştırın

       ```cmd
       npm install
       ```

     - `app.js` dosyasını açın ve bir kesme noktası ekleyin.

     - Visual Studio Code hata ayıklama görünümüne giderek **görünüm > hata ayıkla**' yı seçin. ***Modül adınızın* açılan menüden yerel hata ayıklama (node. js)&gt;&lt;** hata ayıklama yapılandırmasını seçin.
   - **Java**
     - `App.java` dosyasını açın ve bir kesme noktası ekleyin.

     - Visual Studio Code hata ayıklama görünümüne giderek **görünüm > hata ayıkla**' yı seçin. ***Modül adınızın* açılan menüden yerel hata ayıklama (Java)&gt;&lt;** hata ayıklama yapılandırmasını seçin.

1. Hata **ayıklamayı Başlat** ' a tıklayın veya hata ayıklama oturumu başlatmak için **F5** 'e basın.

1. Visual Studio Code tümleşik terminalde, modülünüzü bir **Merhaba Dünya** iletisi göndermek için aşağıdaki komutu çalıştırın. Bu, önceki adımlarda IoT Edge simülatör ' i ayarlarken gösterilen komuttur.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Windows kullanıyorsanız, Visual Studio Code tümleşik terminalinin kabuğun **Git Bash** veya **WSL Bash**olduğundan emin olun. PowerShell veya komut isteminden `curl` komutunu çalıştıramazsınız.
   > [!TIP]
   > Ayrıca, `curl`yerine ileti göndermek için [Postman](https://www.getpostman.com/) veya diğer API araçlarını da kullanabilirsiniz.

1. Visual Studio Code hata ayıklama görünümünde, sol panelde değişkenleri görürsünüz.

1. Hata ayıklama oturumunuzu durdurmak için Durdur düğmesini seçin veya **SHIFT + F5**tuşlarına basın ve ardından Azure IoT Edge çalıştırın: benzeticiyi durdurmak ve temizlemek için komut paletinde **IoT Edge simülatörü durdur** .

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>IoT Edge simülatör ile iliştirme modunda hata ayıklamaC#(, Node. js, Java, Azure işlevleri)

Varsayılan çözümünüz iki modül içerir, biri sanal bir sıcaklık algılayıcısı modülüdür ve diğeri kanal modülüdür. Sanal sıcaklık algılayıcısı, kanal modülüne ileti gönderir ve ardından iletilerin IoT Hub gönderilir. Oluşturduğunuz modül klasöründe, farklı kapsayıcı türleri için birkaç Docker dosyası vardır. Modülle biten dosyaları kullanın. test için modülünüzü derlemek için **hata ayıklayın** .

Şu anda iliştirme modunda hata ayıklama yalnızca aşağıdaki gibi desteklenir:

- C#Azure Işlevleri dahil olmak üzere modüller, Linux AMD64 kapsayıcılarında hata ayıklamayı destekler
- Node. js modülleri, Linux amd64 ve arm32v7 kapsayıcılarında ve Windows AMD64 kapsayıcılarında hata ayıklamayı destekler
- Java modülleri, Linux amd64 ve arm32v7 kapsayıcılarında hata ayıklamayı destekler

> [!TIP]
> Visual Studio Code durum çubuğundaki öğeye tıklayarak IoT Edge çözümünüz için varsayılan platformun seçenekleri arasında geçiş yapabilirsiniz.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge çözümü için IoT Edge simülatör ayarlama

Geliştirme makinenizde, IoT Edge çözümünüzü çalıştırabilmeniz için IoT Edge güvenlik cini yüklemek yerine IoT Edge simülatörü başlatabilirsiniz.

1. Cihaz Gezgini ' nde, sol taraftaki IoT Edge cihaz KIMLIĞINIZ ' ne sağ tıklayın ve ardından cihaz bağlantı dizesiyle simülatör 'yi başlatmak için **IoT Edge simülatör kurulum** ' u seçin.

1. Tümleşik terminalde ilerleme ayrıntısı ' nı okuyarak IoT Edge Benzeticisinin başarıyla ayarlandığını görebilirsiniz.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Hata ayıklama ve hata ayıklama için kapsayıcıyı iliştirme modunda oluştur ve Çalıştır

1. Modül dosyanızı açın (`Program.cs`, `app.js`, `App.java`veya `<your module name>.cs`) ve bir kesme noktası ekleyin.

1. Visual Studio Code gezgin görünümünde, çözümünüz için `deployment.debug.template.json` dosyasına sağ tıklayın ve ardından **derleme ve çalıştır IoT Edge çözümünü Benzetici**' ı seçin. Tüm modül kapsayıcısı günlüklerini aynı pencerede izleyebilirsiniz. Ayrıca, kapsayıcı durumunu izlemek için Docker görünümüne gidebilirsiniz.

   ![Değişkenleri izle](media/how-to-vs-code-develop-module/view-log.png)

1. Visual Studio Code hata ayıklama görünümüne gidin ve modülünüzün hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı  ***modül adınızı&gt;* uzaktan hata ayıklama&lt;benzer olmalıdır**

1. **Hata ayıklamayı Başlat** ' ı seçin veya **F5**tuşuna basın. İliştirilecek işlemi seçin.

1. Visual Studio Code hata ayıklama görünümünde, sol panelde değişkenleri görürsünüz.

1. Hata ayıklama oturumunu durdurmak için öncelikle Durdur düğmesini seçin veya **SHIFT + F5**tuşlarına basın ve ardından **Azure IoT Edge: komut paletinden IoT Edge simülatörünü durdur** ' u seçin.

> [!NOTE]
> Yukarıdaki örnekte, kapsayıcılarda IoT Edge modüllerinin nasıl ayıklanacağı gösterilmektedir. Modülünüzün kapsayıcı `createOptions` ayarlarına sunulan bağlantı noktalarını ekledi. Modüllerinizin hatalarını ayıklamayı bitirdikten sonra, bu açık bağlantı noktalarını üretime yönelik IoT Edge modüller için kaldırmanızı öneririz.
>
> Azure Işlevleri de dahil C#olmak üzere, içinde yazılan modüller için bu örnek, derleme sırasında kapsayıcı görüntünde .NET Core komut satırı hata ayıklayıcısını (vsdbg) içeren `Dockerfile.amd64.debug`hata ayıklama sürümünü temel alır. Modüllerinizi C# hata ayıkladıktan sonra, üretim için hazır IoT Edge modüller Için Dockerfile 'ı vsdbg olmadan doğrudan kullanmanızı öneririz.

## <a name="debug-a-module-with-the-iot-edge-runtime"></a>IoT Edge çalışma zamanına sahip bir modülün hatalarını ayıklama

Her modül klasöründe, farklı kapsayıcı türleri için birkaç Docker dosyası vardır. Modülle biten dosyaları kullanın. test için modülünüzü derlemek için **hata ayıklayın** .

Bu yöntemi kullanarak modüller hata ayıklarken, modülleriniz IoT Edge çalışma zamanının üstünde çalışmaktadır. IoT Edge cihaz ve Visual Studio Code aynı makinede olabilir veya daha fazla genellikle geliştirme makinesinde Visual Studio Code ve IoT Edge çalışma zamanı ve modüller başka bir fiziksel makinede çalışıyor. Visual Studio Code hata ayıklamak için şunları yapmanız gerekir:

- IoT Edge cihazınızı ayarlayın, IoT Edge modüllerinizi **. Debug** dockerfile ile derleyin ve ardından IoT Edge cihaza dağıtın.
- Hata ayıklayıcının iliştirilebileceği şekilde modülün IP ve bağlantı noktasını kullanıma sunun.
- Visual Studio Code uzak makinedeki kapsayıcıda işleme eklenebilmesi için `launch.json` güncelleştirin. Bu dosya, çalışma alanınızdaki `.vscode` klasöründe bulunur ve hata ayıklamayı destekleyen yeni bir modül eklediğiniz her seferinde güncellenir.

### <a name="build-and-deploy-your-module-to-the-iot-edge-device"></a>Modülünüzü IoT Edge cihaza derleyin ve dağıtın

1. Visual Studio Code ' de, uygun `createOptions` değerler ayarlanmış modül görüntülerinin hata ayıklama sürümünü içeren `deployment.debug.template.json` dosyasını açın.

1. Modülünüzü Python 'da geliştiriyorsanız devam etmeden önce şu adımları izleyin:
   - Dosya `main.py` açın ve içeri aktarma bölümünden sonra bu kodu ekleyin:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```

   - Hata ayıklamak istediğiniz geri aramaya aşağıdaki tek satırlık kod satırını ekleyin:

      ```python
      ptvsd.break_into_debugger()
      ```

     Örneğin, `receive_message_listener` işlevinde hata ayıklamak istiyorsanız, bu kod satırını aşağıda gösterildiği gibi ekleyebilirsiniz:

      ```python
      def receive_message_listener(client):
          ptvsd.break_into_debugger()
          global RECEIVED_MESSAGES
          while True:
              message = client.receive_message_on_input("input1")   # blocking call
              RECEIVED_MESSAGES += 1
              print("Message received on input1")
              print( "    Data: <<{}>>".format(message.data) )
              print( "    Properties: {}".format(message.custom_properties))
              print( "    Total calls received: {}".format(RECEIVED_MESSAGES))
              print("Forwarding message to output1")
              client.send_message_to_output(message, "output1")
              print("Message successfully forwarded")
      ```

1. Visual Studio Code komut paletinde:
   1. Komutu çalıştırın **Azure IoT Edge: IoT Edge çözüm oluşturun ve gönderin**.

   1. Çözümünüz için `deployment.debug.template.json` dosyasını seçin.

1. Visual Studio Code Gezgini görünümündeki **Azure IoT Hub cihazları** bölümünde:
   1. IoT Edge cihaz KIMLIĞINE sağ tıkladıktan sonra **tek cihaz Için dağıtım oluştur**' u seçin.

      > [!TIP]
      > Seçtiğiniz cihazın IoT Edge bir cihaz olduğunu doğrulamak için, modül listesini genişletmek ve **$edgeHub** ve **$edgeAgent**varlığını doğrulamak için seçin. Her IoT Edge cihaz bu iki modülü içerir.

   1. Çözümünüzün **yapılandırma** klasörüne gidin, `deployment.debug.amd64.json` dosyasını seçin ve ardından **kenar dağıtım bildirimini Seç**' i seçin.

Dağıtımı, tümleşik terminalde bir dağıtım KIMLIĞIYLE başarıyla oluşturulduğunu görürsünüz.

Terminalde `docker ps` komutunu çalıştırarak kapsayıcı durumunuzu kontrol edebilirsiniz. Visual Studio Code ve IoT Edge çalışma zamanı aynı makinede çalışıyorsa, Visual Studio Code Docker görünümündeki durumu da denetleyebilirsiniz.

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger"></a>Hata ayıklayıcı için modülün IP ve bağlantı noktasını kullanıma sunma

Modülleriniz Visual Studio Code ile aynı makinede çalışıyorsa, kapsayıcıya eklemek için localhost kullandığınızda ve **. Debug** dockerfile, modülünün kapsayıcı `createOptions` ayarları ve `launch.json` dosyasında zaten doğru bağlantı noktası ayarları varsa, bu bölümü atlayabilirsiniz. Modülleriniz ve Visual Studio Code ayrı makinelerde çalışıyorsa, geliştirme diliniz için adımları izleyin.

- **C#Azure Işlevleri dahil**

  [Geliştirme makinenizde ve cihaz IoT Edge SSH kanalını yapılandırın](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes) ve sonra iliştirilecek `launch.json` dosyayı düzenleyin.

- **Node.js**

  - Hata ayıklamakta olan makinedeki modülün çalışır durumda olduğundan ve hata ayıklayıcılarına eklemek için, bağlantı noktası 9229 ' nin dışarıdan erişilebilir olduğundan emin olun. Bunu, hata ayıklayıcı makinesinde `http://<target-machine-IP>:9229/json` açarak doğrulayabilirsiniz. Bu URL, Ayıklanacak Node. js modülüyle ilgili bilgileri göstermelidir.
  
  - Geliştirme makinenizde Visual Studio Code açın ve `launch.json` düzenleyin; böylece modül  ***adı&lt;&gt;* uzak hata ayıklama (node. js)** profili (ya da modül  ***adı&lt;* uzak hata ayıklama (Windows kapsayıcısında Node. js** ) profili, hata ayıklanmakta olan makinenin IP 'si olur.&gt;

- **Java**

  - `ssh -f <username>@<target-machine> -L 5005:127.0.0.1:5005 -N`çalıştırarak hata ayıklaması yapılacak makineye bir SSH tüneli oluşturun.
  
  - Geliştirme makinenizde Visual Studio Code açın ve hedef makineye iliştirebilmeniz için  ***modül adı&gt;* uzaktan hata ayıklama (Java)** profili `launch.json`&lt;düzenleyin. Visual Studio Code ile Java `launch.json` ve hata ayıklama hakkında daha fazla bilgi edinmek için, [hata ayıklayıcıyı yapılandırma](https://code.visualstudio.com/docs/java/java-debugging#_configuration)konusundaki bölümüne bakın.

- **Python**

  - Hata ayıklamakta olan makinedeki 5678 numaralı bağlantı noktasının açık ve erişilebilir olduğundan emin olun.

  - Daha önce `main.py`içine eklediğiniz kod `ptvsd.enable_attach(('0.0.0.0', 5678))`, **0.0.0.0** değerini, hata AYıKLANACAK makinenin IP adresine değiştirin. IoT Edge modülünüzü yeniden derleyin, gönderin ve dağıtın.

  - Geliştirme makinenizde Visual Studio Code açın ve ardından `launch.json` düzenleyerek  ***modül adı&gt;* uzaktan hata ayıklama (Python)** profilinizin `host`&lt;değeri, `localhost`yerine hedef makinenin IP adresini kullanır.

### <a name="debug-your-module"></a>Modülünüzün hatalarını ayıklama

1. Visual Studio Code hata ayıklama görünümünde modülünüzün hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı  ***modül adınızı&gt;* uzaktan hata ayıklama&lt;benzer olmalıdır**

1. Geliştirme diliniz için modül dosyasını açın ve bir kesme noktası ekleyin:

   - **Azure işlevi (C#)** : kesme noktasını dosyaya ekleyin `<your module name>.cs`.
   - **C#** : Kesme noktasını dosyaya `Program.cs`ekleyin.
   - **Node. js**: kesme noktasını dosyaya `app.js`ekleyin.
   - **Java**: dosya `App.java`kesme noktası ekleyin.
   - **Python**: `ptvsd.break_into_debugger()` satırını eklediğiniz geri çağırma yönteminde dosya `main.py`kesme noktası ekleyin.
   - **C**: kesme noktasını dosyaya `main.c`ekleyin.

1. **Hata ayıklamayı Başlat** ' ı veya **F5**' i seçin. İliştirilecek işlemi seçin.

1. Visual Studio Code hata ayıklama görünümünde, sol panelde değişkenleri görürsünüz.

> [!NOTE]
> Yukarıdaki örnekte, kapsayıcılarda IoT Edge modüllerinin nasıl ayıklanacağı gösterilmektedir. Modülünüzün kapsayıcı `createOptions` ayarlarına sunulan bağlantı noktalarını ekledi. Modüllerinizin hatalarını ayıklamayı bitirdikten sonra, bu açık bağlantı noktalarını üretime yönelik IoT Edge modüller için kaldırmanızı öneririz.

## <a name="build-and-debug-a-module-remotely"></a>Modülde uzaktan derleme ve hata ayıklama

Hem Docker hem de Moby altyapılarında SSH bağlantılarını destekleyen son değişiklikler ve Azure IoT araçlarında ortam ayarlarının Visual Studio Code komut paleti ve Azure IoT Edge terminallere eklenmesine izin veren yeni bir ayar ile, artık derleme ve hata ayıklama yapabilirsiniz uzak cihazlardaki modüller.

Daha fazla bilgi ve adım adım yönergeler için bu [IoT Geliştirici blogu girişine](https://devblogs.microsoft.com/iotdev/easily-build-and-debug-iot-edge-modules-on-your-remote-device-with-azure-iot-edge-for-vs-code-1-9-0/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzü oluşturduktan sonra [Visual Studio Code Azure IoT Edge modüllerin nasıl dağıtılacağını](how-to-deploy-modules-vscode.md)öğrenin.

IoT Edge cihazlarınız için modüller geliştirmek üzere [Azure IoT Hub SDK 'Larını anlayın ve kullanın](../iot-hub/iot-hub-devguide-sdks.md).
