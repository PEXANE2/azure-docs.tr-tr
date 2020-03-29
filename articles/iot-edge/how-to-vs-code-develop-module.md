---
title: Azure IoT Edge için modülgeliştirme ve hata ayıklama | Microsoft Dokümanlar
description: C#, Python, Node.js, Java veya C kullanarak Azure IoT Edge için bir modül geliştirmek, oluşturmak ve hata ayıklamak için Visual Studio Kodunu kullanın
services: iot-edge
keywords: ''
author: shizn
ms.author: xshi
ms.date: 08/07/2019
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 10c8008d73390174c44ec503f708c1e2c0011e09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944309"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Azure IoT Edge için modüller geliştirmek ve hata ayıklamak için Visual Studio Kodunu kullanın

Azure IoT Edge için iş mantığınızı modüllere dönüştürebilirsiniz. Bu makalede, modülleri geliştirmek ve hata ayıklamak için ana araç olarak Visual Studio Code'un nasıl kullanılacağı gösterilmektedir.

Visual Studio Code'da C#, Node.js veya Java ile yazılmış modülleri hata ayıklamanın iki yolu vardır: Bir işlemi bir modül kabına ekleyebilirsiniz veya hata ayıklama modunda modül kodunu başlatabilirsiniz. Python veya C'de yazılmış modülleri hata ayıklamak için yalnızca Linux amd64 kapsayıcılarında bir işleme ekleyebilirsiniz.

Visual Studio Code'un hata ayıklama özelliklerine aşina değilseniz, [Hata Ayıklama](https://code.visualstudio.com/Docs/editor/debugging)hakkında bilgi edinin.

Bu makalede, birden çok mimari için birden çok dilde modülgeliştirme ve hata ayıklama için yönergeler sağlar. Şu anda Visual Studio Code C#, C, Python, Node.js ve Java ile yazılmış modüller için destek sağlar. Desteklenen aygıt mimarileri X64 ve ARM32'dir. Desteklenen işletim sistemleri, diller ve mimariler hakkında daha fazla bilgi için [Dil ve mimari desteğine](module-development.md#language-and-architecture-support)bakın.

>[!NOTE]
>Linux ARM64 cihazları için destek geliştirme ve hata ayıklama [genel önizlemede.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Daha fazla bilgi için Visual [Studio Code(önizleme) 'deki ARM64 IoT Edge modüllerini geliştir](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview)in ve hata ayıklama bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Geliştirme makineniz olarak Windows, macOS veya Linux çalıştıran bir bilgisayar veya sanal makine kullanabilirsiniz. Windows bilgisayarlarda Windows veya Linux modülleri geliştirebilirsiniz. Windows modülleri geliştirmek için sürüm 1809/build 17763 veya daha yeni çalışan bir Windows bilgisayarı kullanın. Linux modülleri geliştirmek için Docker Desktop [gereksinimlerini](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)karşılayan bir Windows bilgisayarı kullanın.

Önce [Visual Studio Code'u](https://code.visualstudio.com/) yükleyin ve ardından aşağıdaki uzantıları ekleyin:

- [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Geliştirdiğiniz dile özgü Visual Studio uzantısı(lar) :
  - Azure Fonksiyonları dahil C#: [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  - Python: [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Visual Studio Code için Java Uzatma Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Ayrıca, modülünüzü geliştirmek için dile özel bazı ek araçlar yüklemeniz gerekir:

- Azure Fonksiyonları dahil C#: [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)

- Python: Python paketlerini yüklemek için [Python](https://www.python.org/downloads/) ve [Pip](https://pip.pypa.io/en/stable/installing/#installation) (genellikle Python yüklemenize dahildir).

- Düğüm.js: [Düğüm.js](https://nodejs.org). Ayrıca [Yeoman](https://www.npmjs.com/package/yo) ve [Azure IoT Edge Düğüm.js Modül Jeneratör](https://www.npmjs.com/package/generator-azure-iot-edge-module)yüklemek isteyeceksiniz.

- Java: [Java SE Geliştirme Kiti 10](https://aka.ms/azure-jdks) ve [Maven](https://maven.apache.org/). JDK yüklemenizi işaret edecek [şekilde ortam `JAVA_HOME` değişkenini ayarlamanız](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) gerekir.

Modül görüntünüzün oluşturulması ve dağıtılması için, modül görüntüsünü ve modül görüntüsünü tutmak için bir konteyner kayıt defteri oluşturmak için Docker'a ihtiyacınız vardır:

- Geliştirme makinenizde [Docker Community Edition.](https://docs.docker.com/install/)

- [Azure Konteyner Kayıt Defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Bulut kayıt defteri yerine prototip ve test amacıyla yerel bir Docker kayıt defteri kullanabilirsiniz.

Modülünüzü C'de geliştirmediğiniz sürece, yerel geliştirme ortamınızı hata ayıklamak, çalıştırmak ve ioT Edge çözümünüzü test etmek için oluşturmak için Python tabanlı [Azure IoT EdgeHub Geliştirme Aracı'na](https://pypi.org/project/iotedgehubdev/) da ihtiyacınız vardır. Bunu daha önce yapmadıysanız, [Python (2.7/3.6/3.7) ve Pip'i](https://www.python.org/) yükleyin ve terminalinizde bu komutu çalıştırarak **iotedgehubdev'i** yükleyin.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```
   
> [!NOTE]
> Şu anda, iotedgehubdev Python 3.8 ile uyumlu olmayan bir docker-py kitaplığı kullanır.
>
> Önceden yüklenmiş python 2.7 (örneğin, Ubuntu veya macOS'ta) dahil olmak üzere birden `pip` `pip3` fazla Python'unuz varsa, doğru python'u kullandığınızdan veya **iotedgehubdev'i** yüklediğinizden emin olun

Modülünüzü bir aygıtta test etmek için en az bir IoT Edge aygıtına sahip etkin bir IoT hub'ına ihtiyacınız vardır. Bilgisayarınızı IoT Edge aygıtı olarak kullanmak [için, Linux](quickstart-linux.md) veya [Windows](quickstart.md)için hızlı başlatma adımlarını izleyin. Geliştirme makinenizde IoT Edge daemon çalıştırıyorsanız, bir sonraki adıma geçmeden önce EdgeHub ve EdgeAgent'ı durdurmanız gerekebilir.

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Aşağıdaki adımlar, Visual Studio Code ve Azure IoT Araçlarını kullanarak tercih ettiğiniz geliştirme dilinde (C#'da yazılmış Azure Fonksiyonları dahil) bir IoT Edge modülü oluşturmanızı gösterir. Bir çözüm oluşturarak başlarsınız ve sonra bu çözümdeki ilk modülü oluşturursunuz. Her çözüm birden fazla modül içerebilir.

1. **Komut Paletini** **Görüntüle'yi** > seçin.

1. Komut paletinde **Azure IoT Edge: New IoT Edge Solution**komutunu girin ve çalıştırın.

   ![Yeni IoT Edge Çözümlerini Çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

1. Yeni çözüm oluşturmak istediğiniz klasöre göz atın ve ardından **klasörü seçin.**

1. Çözümünüz için bir ad girin.

1. Tercih ettiğiniz geliştirme dilinin çözümdeki ilk modül olması için bir modül şablonu seçin.

1. Modüliçin bir ad girin. Konteyner kayıt defterinizde benzersiz bir ad seçin.

1. Modülün görüntü deposunun adını girin. Visual Studio Code, modül adını **localhost:5000/<ile\>otomatik olarak karşılar.** Kendi kayıt defteri bilgilerinizle değiştirin. Test etmek için yerel bir Docker kayıt defteri kullanıyorsanız, **localhost** iyidir. Azure Kapsayıcı Kayıt Defteri kullanıyorsanız, kayıt defterinizin ayarlarından giriş sunucusunu kullanın. Giriş sunucusu kayıt ** _ \<defteri adı\>_.azurecr.io**gibi görünüyor. Yalnızca **yerel host:5000** parçasını değiştirerek, nihai sonucun ** \<kayıt defteri adı .azurecr.io/_\<modül\>_ *adıgibi*\>** görünmesi için.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code sağladığınız bilgileri alır, bir IoT Edge çözümü oluşturur ve sonra yeni bir pencereye yükler.

Çözüm içinde dört öğe vardır:

- **.vscode** klasörü hata ayıklama yapılandırmaları içerir.

- **Modülklasöründe** her modül için alt klasörler bulunur.  Her modül için klasör içinde bir dosya, **module.json**, modülleri inşa ve dağıtılır nasıl denetler.  Bu dosyanın, modül dağıtım kapsayıcı kayıt defterini localhost'tan uzak bir kayıt defterine değiştirmek için değiştirilmesi gerekir. Bu noktada, sadece bir modül var.  Ancak Azure IoT Edge komutu yla komut paletine daha fazlasını **ekleyebilirsiniz: IoT Edge Modülü ekleyin.**

- Bir **.env** dosyası ortam değişkenlerinizi listeler. Azure Kapsayıcı Kayıt Defteri kayıt defterinizse, içinde bir Azure Kapsayıcı Kayıt Defteri kullanıcı adı ve parolası olur.

  > [!NOTE]
  > Ortam dosyası yalnızca modül için bir görüntü deposu sağlarsanız oluşturulur. Yerel olarak test etmek ve hata ayıklamak için localhost varsayılanlarını kabul ettiyseniz, ortam değişkenlerini bildirmeniz gerekmez.

- **Deployment.template.json** dosyası, yeni modülünüzü test etmek için kullanabileceğiniz verileri simüle eden bir örnek **Simüle Sıcaklık Sensörü** modülüyle birlikte listeler. Dağıtım bildirimlerinin nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](module-composition.md)

Simüle edilen sıcaklık modülünün nasıl çalıştığını görmek [için, Simüle Edilen SıcaklıkSensörü.csproj kaynak kodunu](https://github.com/Azure/iotedge/tree/master/edge-modules/SimulatedTemperatureSensor)görüntüleyin.

## <a name="add-additional-modules"></a>Ek modüller ekleme

Çözümünüze ek modüller eklemek için **Azure IoT Edge: IoT Edge Modüllerini** komut paletinden ekleyin komutunu çalıştırın. Ayrıca Visual Studio Code Explorer görünümünde `deployment.template.json` **modüller** klasörüne veya dosyaya sağ tıklayıp **IoT Edge Modülü Ekle'yi**seçebilirsiniz.

## <a name="develop-your-module"></a>Modülünüzü geliştirin

Çözümle birlikte gelen varsayılan modül kodu aşağıdaki konumda bulunur:

- Azure İşlevi (C#): ***&lt;&gt;* * &lt;modüladınız&gt;* > > modüller modül adınızı .cs**
- C#: **modül * &lt;adı&gt; nızı* > modüller > Program.cs**
- Python: **modül * &lt;adınız&gt; * > modüller > main.py**
- Node.js: ** * &lt;&gt; modül adı >* app.js > modülleri**
- Java: **modül * &lt;adı&gt; >* > src > ana > java > com > edgemodulemodules > App.java**
- C: **modül * &lt;adı>&gt; * modülleri > main.c**

Modül ve deployment.template.json dosyası, çözümü oluşturabilmeniz, konteyner kayıt defterinize itebilmeniz ve herhangi bir koda dokunmadan sınama başlatmak için aygıta dağıtabilmeniz için ayarlanır. Modül sadece bir kaynaktan giriş almak için inşa edilmiştir (bu durumda, veri simüle Simüle Simüle TemperatureSensor modülü) ve IoT Hub için boru.

Şablonu kendi kodunuzla özelleştirmeye hazır olduğunuzda, güvenlik, aygıt yönetimi ve güvenilirlik gibi IoT çözümlerinin temel gereksinimlerini karşılayan modüller oluşturmak için [Azure IoT Hub SDK'larını](../iot-hub/iot-hub-devguide-sdks.md) kullanın.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Kapsayıcıolmadan bir modülü hata ayıklama (C#, Node.js, Java)

C#, Node.js veya Java'da geliştiriyorsanız, modülünüzün iletileri başlatabilmesi, çalıştırabilmesi ve yönlendirebilmesi için varsayılan modül kodunda bir **ModuleClient** nesnesinin kullanılması gerekir. Modül iletileri aldığında harekete geçmek için varsayılan giriş kanalı **girişi1'i** de kullanırsınız.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge çözümü için IoT Edge simülatörü ayarlama

Geliştirme makinenizde, IoT Edge güvenlik daemonu yüklemek yerine bir IoT Edge simülatörü başlatabilirsiniz, böylece IoT Edge çözümünüzü çalıştırabilirsiniz.

1. Sol taraftaki aygıt gezgininde, IoT Edge aygıt kimliğinize sağ tıklayın ve ardından aygıt bağlantı dizesiyle simülatörünü başlatmak için **Kurulum IoT Edge Simulator'u** seçin.
1. IoT Edge Simulator başarıyla entegre terminalde ilerleme ayrıntıları okuyarak kurulmuştur görebilirsiniz.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>Tek modül uygulaması için IoT Edge simülatörünü ayarlama

Simülatörü kurmak ve başlatmak için Visual Studio Code komut paletinden **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** komutunu çalıştırın. İstendiğinde, varsayılan modül kodundan (veya kodundan eşdeğer değer) gelen değer **girişi1'i** uygulamanızın giriş adı olarak kullanın. Komut **iotedgehubdev** CLI tetikler ve sonra IoT Edge simülatörü ve bir test programı modülü konteyner başlar. Simülatör tek modül modunda başarıyla başlatılmışsa, aşağıdaki çıktıları entegre terminalde görebilirsiniz. İleti göndermeye yardımcı `curl` olacak bir komut da görebilirsiniz. Daha sonra bu adı kullanacaksınız.

   ![Tek modül uygulaması için IoT Edge simülatörünü ayarlama](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Modülün çalışma durumunu görmek için Visual Studio Code'daki Docker Explorer görünümünü kullanabilirsiniz.

   ![Simülatör modülü durumu](media/how-to-develop-csharp-module/simulator-status.png)

   **EdgeHubDev** konteyner yerel IoT Edge simülatörü çekirdeğidir. IoT Edge güvenlik daemon olmadan geliştirme makinenizde çalıştırabilirsiniz ve yerel modül uygulaması veya modül kapsayıcıları için ortam ayarları sağlar. **Giriş** kapsayıcısı, iletileri modülünüzdeki hedef giriş kanalına köprülemeye yardımcı olmak için REST API'lerini ortaya çıkarır.

### <a name="debug-module-in-launch-mode"></a>Başlatma modunda hata ayıklama modülü

1. Ortamınızı geliştirme dilinizin gereksinimlerine göre hata ayıklama için hazırlayın, modülünüzde bir kesme noktası ayarlayın ve kullanılacak hata ayıklama yapılandırmasını seçin:
   - **C #**
     - Visual Studio Code tümleşik terminalinde, dizini *** &lt;modül adı&gt; klasörünüzle*** değiştirin ve sonra .NET Core uygulamasını oluşturmak için aşağıdaki komutu çalıştırın.

       ```cmd
       dotnet build
       ```

     - Dosyayı `Program.cs` açın ve bir kesme noktası ekleyin.

     - Hata Ayıklama > **Görüntüle'yi**seçerek Visual Studio Kodu Hata Ayıklama görünümüne gidin. Açılan dosyadan ** * &lt;modül adınız&gt; * Yerel Hata Ayıklama (.NET Core)** hata ayıklama yapılandırmasını seçin.

        > [!NOTE]
        > `TargetFramework` .NET Core'unuz program yolunuzla `launch.json`tutarlı değilse, Visual Studio Code'un `launch.json` bu `TargetFramework` programı başarıyla başlatabilmesi için .csproj dosyanızdakiyle eşleşecek şekilde program yolunu el ile güncelleştirmeniz gerekir.

   - **Node.js**
     - Visual Studio Code tümleşik terminalinde, dizini *** &lt;modül adı&gt; klasörünüzle*** değiştirin ve düğüm paketlerini yüklemek için aşağıdaki komutu çalıştırın

       ```cmd
       npm install
       ```

     - Dosyayı `app.js` açın ve bir kesme noktası ekleyin.

     - Hata Ayıklama > **Görüntüle'yi**seçerek Visual Studio Kodu Hata Ayıklama görünümüne gidin. Açılan dosyadan ** * &lt;modül adınız&gt; * Yerel Hata Ayıklama (Node.js)** hata ayıklama yapılandırmasını seçin.
   - **Java**
     - Dosyayı `App.java` açın ve bir kesme noktası ekleyin.

     - Hata Ayıklama > **Görüntüle'yi**seçerek Visual Studio Kodu Hata Ayıklama görünümüne gidin. Açılan dosyadan ** * &lt;modül adınız&gt; * Yerel Hata Ayıklama (Java)** hata ayıklama yapılandırmasını seçin.

1. **Hata Ayıklama'yı Başlat'ı** tıklatın veya hata ayıklama oturumunu başlatmak için **F5** tuşuna basın.

1. Visual Studio Code entegre terminalinde, modülünüze **bir Merhaba Dünya** mesajı göndermek için aşağıdaki komutu çalıştırın. Bu, IoT Edge simülatörü ayarladığınızda önceki adımlarda gösterilen komutdur.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Windows kullanıyorsanız, Visual Studio Code entegre terminalinizin kabuğunun **Git Bash** veya **WSL Bash**olduğundan emin olun. Komutu `curl` powershell veya komut isteminden çalıştıramazsınız.
   > [!TIP]
   > İleti göndermek için [PostMan](https://www.getpostman.com/) veya diğer API araçlarını `curl`da kullanabilirsiniz.

1. Visual Studio Code Hata Ayıklama görünümünde, değişkenleri sol panelde görürsünüz.

1. Hata ayıklama oturumunuzu durdurmak için, Durdur düğmesini seçin veya **Shift + F5**tuşuna basın ve ardından simülatörü durdurmak ve temizlemek için komut paletinde **Azure IoT Edge: Stop IoT Edge Simulator'u** çalıştırın.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>IoT Edge Simulator (C#, Node.js, Java, Azure Fonksiyonları) ile ekleme modunda hata ayıklama

Varsayılan çözümünüz iki modül içerir, biri simüle edilmiş sıcaklık sensörü modülü, diğeri ise boru modülüdür. Simüle edilen sıcaklık sensörü boru modülüne ileti gönderir ve ardından iletiler IoT Hub'ına iletilir. Oluşturduğunuz modül klasöründe, farklı kapsayıcı türleri için birkaç Docker dosyası vardır. Modülünüzü test etmek için oluşturmak için uzantı **.hata ayıklama** ile biten dosyalardan herhangi birini kullanın.

Şu anda, ekleme modunda hata ayıklama yalnızca aşağıdaki gibi desteklenir:

- Azure İşlevleri de dahil olmak üzere C# modülleri, Linux amd64 kaplarında hata ayıklamayı destekler
- Node.js modülleri Linux amd64 ve arm32v7 kaplarında hata ayıklama desteği ve Windows amd64 kapsayıcıları
- Java modülleri Linux amd64 ve arm32v7 kaplarında hata ayıklama desteği

> [!TIP]
> Visual Studio Code durum çubuğundaki öğeyi tıklatarak IoT Edge çözümünüz için varsayılan platform seçenekleri arasında geçiş yapabilirsiniz.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge çözümü için IoT Edge simülatörü ayarlama

Geliştirme makinenizde, IoT Edge güvenlik daemonu yüklemek yerine bir IoT Edge simülatörü başlatabilirsiniz, böylece IoT Edge çözümünüzü çalıştırabilirsiniz.

1. Sol taraftaki aygıt gezgininde, IoT Edge aygıt kimliğinize sağ tıklayın ve ardından aygıt bağlantı dizesiyle simülatörünü başlatmak için **Kurulum IoT Edge Simulator'u** seçin.

1. IoT Edge Simulator başarıyla entegre terminalde ilerleme ayrıntıları okuyarak kurulmuştur görebilirsiniz.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Takma modunda hata ayıklama ve hata ayıklama için kapsayıcı oluşturma ve çalıştırma

1. Modül dosyanızı`Program.cs`açın `app.js` `App.java`( `<your module name>.cs`, , , veya ) ve bir kesme noktası ekleyin.

1. Visual Studio Code Explorer görünümünde, `deployment.debug.template.json` çözümünüz için dosyayı sağ tıklatın ve ardından **Simülatörde IoT Edge çözümünü oluştur ve çalıştır'ı**seçin. Tüm modül konteyner günlüklerini aynı pencerede izleyebilirsiniz. Ayrıca konteyner durumunu izlemek için Docker görünümüne de gidebilirsiniz.

   ![Değişkenleri İzle](media/how-to-vs-code-develop-module/view-log.png)

1. Visual Studio Code Hata Ayıklama görünümüne gidin ve modülünüz için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı ** * &lt;modül adınıza&gt; * benzer olmalıdır Uzaktan Hata Ayıklama**

1. **Hata Ayıklama başlat'ı** seçin veya **F5 tuşuna**basın. Ekleyecek işlemi seçin.

1. Visual Studio Code Hata Ayıklama görünümünde, değişkenleri sol panelde görürsünüz.

1. Hata ayıklama oturumunu durdurmak için önce Durdur düğmesini seçin veya **Shift + F5**tuşuna basın ve ardından komut paletinden **Azure IoT Edge: Stop IoT Edge Simulator'u** seçin.

> [!NOTE]
> Önceki örnek, kapsayıcılarda IoT Edge modüllerinin nasıl hata ayıklanır yapılacağını gösterir. Modülünüzün konteyner `createOptions` ayarlarına maruz kalan bağlantı noktaları eklendi. Modüllerinizin hata ayıklamaişini tamamladıktan sonra, üretime hazır IoT Edge modülleri için bu açıkta kalan bağlantı noktalarını kaldırmanızı öneririz.
>
> Azure İşlevleri de dahil olmak üzere C#'da yazılan modüller `Dockerfile.amd64.debug`için bu örnek, kapsayıcı resminizde .NET Core komut satırı hata ayıklayıcısını (VSDBG) içeren hata ayıklama sürümünü temel alır. C# modüllerinizi hata ayıklamadan sonra, üretime hazır IoT Edge modülleri için VSDBG olmadan Dockerfile'ı doğrudan kullanmanızı öneririz.

## <a name="debug-a-module-with-the-iot-edge-runtime"></a>IoT Edge çalışma süresine sahip bir modülü hata ayıklama

Her modül klasöründe, farklı kapsayıcı türleri için birkaç Docker dosyası vardır. Modülünüzü test etmek için oluşturmak için uzantı **.hata ayıklama** ile biten dosyalardan herhangi birini kullanın.

Bu yöntemi kullanarak modülleri hata ayıklarken, modülleriniz IoT Edge çalışma zamanının üstünde çalışır. IoT Edge cihazı ve Visual Studio Code aynı makineüzerinde olabilir, ya da daha tipik olarak, Visual Studio Code geliştirme makinesi ve IoT Edge çalışma süresi ve modülleri başka bir fiziksel makine üzerinde çalışıyor. Visual Studio Code'tan hata ayıklamak için şunları

- IoT Edge aygıtınızı kurun, IoT Edge modülü(ler) ile **.debug** Dockerfile ile oluşturun ve ardından IoT Edge aygıtına dağıtın.
- Hata ayıklamanın eklenabilmesi için modülün IP'sini ve bağlantı noktasını açığa alın.
- Visual `launch.json` Studio Code'un uzak makinedeki kapsayıcıdaki işleme ekleyebileceği şekilde güncelleştirin. Bu dosya çalışma alanınızdaki `.vscode` klasörde bulunur ve hata ayıklamayı destekleyen yeni bir modül eklediğinizde güncellenir.

### <a name="build-and-deploy-your-module-to-the-iot-edge-device"></a>Modülünüzü IoT Edge aygıtına oluşturun ve dağıtın

1. Visual Studio Code'da, modül görüntülerinizin hata ayıklama sürümünü içeren dosyayı `deployment.debug.template.json` uygun `createOptions` değerler kümesiyle açın.

1. Python'da modülünüzü geliştiriyorsanız, devam etmeden önce aşağıdaki adımları izleyin:
   - Dosyayı `main.py` açın ve alma bölümünden sonra bu kodu ekleyin:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```

   - Hata ayıklamak istediğiniz geri arama için aşağıdaki tek kod satırını ekleyin:

      ```python
      ptvsd.break_into_debugger()
      ```

     Örneğin, `receive_message_listener` işlevi hata ayıklamak istiyorsanız, bu kod satırını aşağıda gösterildiği gibi eklersiniz:

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
   1. Azure IoT Edge komutunu **çalıştırın: IoT Edge çözümlerini oluşturun ve itin.**

   1. Çözümünüz `deployment.debug.template.json` için dosyayı seçin.

1. Visual Studio Code Explorer görünümünün **Azure IoT Hub Aygıtları** bölümünde:
   1. Bir IoT Edge aygıt kimliğine sağ tıklayın ve ardından **Tek Aygıt için Dağıtım Oluştur'u**seçin.

      > [!TIP]
      > Seçtiğiniz aygıtın bir IoT Edge aygıtı olduğunu doğrulamak için, modül listesini genişletmek ve **$edgeHub** ve **$edgeAgent**varlığını doğrulamak için aygıtı seçin. Her IoT Edge cihazı bu iki modülü içerir.

   1. Çözümünüzün **config** klasörüne gidin, `deployment.debug.amd64.json` dosyayı seçin ve ardından **Kenar Dağıtım Bildirimi'ni seçin.**

Tümleşik terminalde dağıtım kimliğiyle başarıyla oluşturulan dağıtımı görürsünüz.

Terminaldeki komutu `docker ps` çalıştırarak konteyner durumunuzu kontrol edebilirsiniz. Visual Studio Code ve IoT Edge çalışma süreniz aynı makinede çalışıyorsa, Visual Studio Code Docker görünümündeki durumu da kontrol edebilirsiniz.

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger"></a>Hata ayıklama için modülün IP ve bağlantı noktasını ortaya çıkarma

Modülleriniz Visual Studio Code ile aynı makinede çalışıyorsa, kapsayıcıya takmak için localhost kullanıyorsanız ve **"hata ayıklama** Dockerfile"de, modülün `createOptions` kapsayıcı `launch.json` ayarlarında ve dosyasında zaten doğru bağlantı noktası ayarlarına sahipseniz bu bölümü atlayabilirsiniz. Modülleriniz ve Visual Studio Code'unuz ayrı makinelerde çalışıyorsa, geliştirme diliniz için aşağıdaki adımları izleyin.

- **Azure Fonksiyonları dahil C#**

  [Geliştirme makinenizdeki ve IoT Edge aygıtınızdaki SSH kanalını yapılandırın](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes) ve ardından dosyayı ekecek şekilde düzenle. `launch.json`

- **Node.js**

  - Hata ayıklanacak makinedeki modülün çalışırken ve hata ayıklayıcıların takması için hazır olduğundan ve 9229 bağlantı noktasının harici olarak erişilebilir olduğundan emin olun. Hata ayıklama makinesini açarak `http://<target-machine-IP>:9229/json` bunu doğrulayabilirsiniz. Bu URL, debugged için Düğüm.js modülü hakkında bilgi göstermelidir.
  
  - Geliştirme makinenizde Visual Studio Code'u `launch.json` açın ve modül ** * &lt;adınız&gt; * Uzaktan Hata Ayıklama (Node.js) profilinin** (veya ** * &lt;modül adınız&gt; * Uzaktan Hata Ayıklama (Windows Kapsayıcısındaki Node.js) profilinizin** adres değerinin, modül Windows kapsayıcısı olarak çalışıyorsa) hata ayıklanan makinenin IP'si olacak şekilde değiştirin.

- **Java**

  - Çalıştırarak `ssh -f <username>@<target-machine> -L 5005:127.0.0.1:5005 -N`debugged olmak için makineye bir SSH tünel oluşturun.
  
  - Geliştirme makinenizde Visual Studio Code'u açın ve hedef makineye ekleyebilmeniz için ** * &lt;modül adınızı&gt; * Uzaktan Hata Ayıklama (Java)** profilini `launch.json` düzeltin. Visual Studio Code `launch.json` ile Java'yı düzenleme ve hata ayıklama hakkında daha fazla bilgi edinmek için [hata ayıklayıcıyı yapılandırma](https://code.visualstudio.com/docs/java/java-debugging#_configuration)bölümüne bakın.

- **Python**

  - Debugged olmak için makineüzerinde bağlantı noktası 5678 açık ve erişilebilir olduğundan emin olun.

  - Daha önce `ptvsd.enable_attach(('0.0.0.0', 5678))` eklediğiniz `main.py`kodda, **0.0.0.0'ı** debugged için makinenin IP adresine değiştirin. IoT Edge modülünüzü yeniden oluşturun, itin ve dağıtın.

  - Geliştirme makinenizde Visual Studio Code'u `launch.json` açın ve `host` ardından ** * &lt;modül adınız&gt; * Uzaktan Hata Ayıklama (Python) profilinin** değerinin `localhost`hedef makinenin IP adresini yerine kullanması için bir de düzeltin.

### <a name="debug-your-module"></a>Modülünüzü hata ayıklama

1. Visual Studio Code Hata Ayıklama görünümünde, modülünüz için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı ** * &lt;modül adınıza&gt; * benzer olmalıdır Uzaktan Hata Ayıklama**

1. Geliştirme diliniz için modül dosyasını açın ve bir kesme noktası ekleyin:

   - **Azure İşlevi (C#)**: Kesme `<your module name>.cs`noktanızı dosyaya ekleyin.
   - **C#**: Kesme noktanızı `Program.cs`dosyaya ekleyin.
   - **Düğüm.js**: Kesme noktanızı dosyaya `app.js`ekleyin.
   - **Java**: Dosyaya kesme `App.java`noktanızı ekleyin.
   - **Python**: Satırı eklediğiniz `main.py`geri arama yönteminde kesme `ptvsd.break_into_debugger()` noktanızı dosyaya ekleyin.
   - **C**: Kesme noktanızı `main.c`dosyaya ekleyin.

1. **Hata Ayıklama başlat'ı** seçin veya **F5'i**seçin. Ekleyecek işlemi seçin.

1. Visual Studio Code Hata Ayıklama görünümünde, değişkenleri sol panelde görürsünüz.

> [!NOTE]
> Önceki örnek, kapsayıcılarda IoT Edge modüllerinin nasıl hata ayıklanır yapılacağını gösterir. Modülünüzün konteyner `createOptions` ayarlarına maruz kalan bağlantı noktaları eklendi. Modüllerinizin hata ayıklamaişini tamamladıktan sonra, üretime hazır IoT Edge modülleri için bu açıkta kalan bağlantı noktalarını kaldırmanızı öneririz.

## <a name="build-and-debug-a-module-remotely"></a>Bir modülü uzaktan oluşturma ve hata ayıklama

SSH bağlantılarını desteklemek için hem Docker hem de Moby motorlarında yapılan son değişiklikler ve Visual Studio Code komut paletine ve Azure IoT Edge terminallerine ortam ayarları nın enjekte edilmesini sağlayan Azure IoT Araçlarında yeni bir ayar sayesinde artık hata ayıklama oluşturabilirsiniz ve hata ayıklayabilirsiniz uzak cihazlarda modülleri.

Daha fazla bilgi ve adım adım talimatlar için bu [IoT Geliştirici blog girişine](https://devblogs.microsoft.com/iotdev/easily-build-and-debug-iot-edge-modules-on-your-remote-device-with-azure-iot-edge-for-vs-code-1-9-0/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzü yaptıktan sonra Visual Studio [Code'tan Azure IoT Edge modüllerini nasıl dağıtabileceğinizi](how-to-deploy-modules-vscode.md)öğrenin.

IoT Edge aygıtlarınız için modüller geliştirmek için [Azure IoT Hub SDK'larını anlayın ve kullanın.](../iot-hub/iot-hub-devguide-sdks.md)
