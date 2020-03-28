---
title: Öğretici - Azure IoT Edge kullanarak Linux cihazları için modül geliştirme
description: Bu öğretici, Linux aygıtları için Linux kaplarını kullanarak IoT Edge modülleri geliştirmek için geliştirme makinenizi ve bulut kaynaklarınızı kurma da yürür
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a127c8c77f97708729cf91d6ced214e98924fb6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78943020"
---
# <a name="tutorial-develop-iot-edge-modules-for-linux-devices"></a>Öğretici: Linux cihazları için IoT Edge modülleri geliştirin

IoT Edge çalıştıran Linux aygıtlarına kod geliştirmek ve dağıtmak için Visual Studio Code'u kullanın.

Hızlı bir başlangıç olarak, Bir Linux sanal makine kullanarak bir IoT Edge aygıtı oluşturdunuz ve Azure Marketi'nden bir modül dağıttınız. Bu öğretici, kendi kodunuzu bir IoT Edge aygıtına geliştirme ve dağıtma yoluyla yürür. Bu makale, belirli programlama dilleri veya Azure hizmetleri hakkında daha fazla ayrıntıya giren diğer öğreticiler için yararlı bir ön koşuldur.

Bu öğretici, bir Linux **aygıtına C# modülü**dağıtma örneğini kullanır. Bu örnek, IoT Edge çözümleri için en yaygın geliştirici senaryosu olduğu için seçildi. Farklı bir dil kullanmayı veya bir Azure hizmetini dağıtmayı planlasanız bile, bu öğretici geliştirme araçları ve kavramları hakkında bilgi edinmek için yine de yararlıdır. Geliştirme sürecine giriş işlemini tamamlayın ve ayrıntılara dalmak için tercih ettiğiniz dili veya Azure hizmetini seçin.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Geliştirme makinenizi ayarlayın.
> * Yeni bir proje oluşturmak için Visual Studio Code için IoT Edge araçlarını kullanın.
> * Projenizi kapsayıcı olarak oluşturun ve azure kapsayıcı kayıt defterinde saklayın.
> * Kodunuzu bir IoT Edge aygıtına dağıtın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="key-concepts"></a>Önemli kavramlar

Bu öğretici bir IoT Edge modülü geliştirme yoluyla yürür. *Bir IoT Edge modülü,* ya da bazen sadece kısa *modül,* yürütülebilir kodlu bir kapsayıcıdır. Bir IoT Edge aygıtına bir veya daha fazla modül dağıtabilirsiniz. Modüller sensörlerden veri alma, verileri temizleme ve analiz etme veya bir IoT hub'ına ileti gönderme gibi belirli görevleri yerine getirir. Daha fazla bilgi için [bkz.](iot-edge-modules.md)

IoT Edge modülleri geliştirirken, geliştirme makinesi ile modülün sonunda dağıtılacak hedef IoT Edge aygıtı arasındaki farkı anlamak önemlidir. Modül kodunuzu tutmak için oluşturduğunuz kapsayıcı, *hedef aygıtın*işletim sistemiyle (OS) eşleşmelidir. Örneğin, en yaygın senaryo, Windows bilgisayarında IoT Edge çalıştıran bir Linux aygıtını hedeflemeyi amaçlayan bir modül geliştiren birisidir. Bu durumda, konteyner işletim sistemi Linux olacaktır. Bu öğretici geçmesi gibi, *geliştirme makinesi işletim sistemi* ve *konteyner işletim sistemi*arasındaki farkı akılda tutun.

Bu öğretici, IoT Edge çalıştıran Linux aygıtlarını hedefler. Geliştirme makineniz Linux kapsayıcıları çalıştırdığınız sürece tercih ettiğiniz işletim sistemini kullanabilirsiniz. Linux aygıtları için geliştirmek için Visual Studio Code'u kullanmanızı öneririz, bu nedenle bu öğreticinin kullanacağı şey budur. İki araç arasında destek farklılıkları olsa da Visual Studio'yu da kullanabilirsiniz.

Aşağıdaki tabloda Visual Studio Code ve Visual **Studio'da Linux kapsayıcıları** için desteklenen geliştirme senaryoları listelenmiştir.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux cihaz mimarisi** | Linux AMD64 <br> Linux ARM32 | Linux AMD64 <br> Linux ARM32 |
| **Azure hizmetleri** | Azure İşlevleri <br> Azure Stream Analytics <br> Azure Machine Learning |   |
| **Diller** | C <br> C# <br> Java <br> Node.js <br> Python | C <br> C# |
| **Daha fazla bilgi** | [Görsel Stüdyo Kodu için Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Visual Studio 2017 için Azure IoT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) <br> [Visual Studio için Azure IoT Edge Araçları 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

>[!NOTE]
>Linux ARM64 cihazları için destek [genel önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)mevcuttur. Daha fazla bilgi için Visual [Studio Code(önizleme) 'deki ARM64 IoT Edge modüllerini geliştir](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview)in ve hata ayıklama bölümüne bakın.

Bu öğretici Visual Studio Code için geliştirme adımları öğretir. Visual Studio'yu kullanmayı tercih ediyorsanız, [Azure IoT Edge için modüller geliştirmek ve hata ayıklamak için Visual Studio 2019'u kullanın'daki yönergelere](how-to-visual-studio-develop-module.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Geliştirme makinesi:

* Geliştirme tercihlerinize bağlı olarak kendi bilgisayarınızı veya sanal makinenizi kullanabilirsiniz.
  * Geliştirme makinenizin iç içe sanallaştırmayı desteklediğinden emin olun. Bu özellik, bir sonraki bölümde yüklediğiniz bir kapsayıcı altyapısını çalıştırmak için gereklidir.
* Bir konteyner motoru çalıştırabilen çoğu işletim sistemi, Linux aygıtları için IoT Edge modülleri geliştirmek için kullanılabilir. Bu öğretici bir Windows bilgisayarı kullanır, ancak MacOS veya Linux'taki bilinen farklılıklara işaret eder.
* Bu öğreticide modül şablon paketlerini çekmek için [Git'i](https://git-scm.com/)yükleyin.  
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

Linux'ta bir Azure IoT Edge cihazı:

* Geliştirme makinenizde IoT Edge'i çalıştırmamanızı, bunun yerine ayrı bir aygıt kullanmanızı öneririz. Geliştirme makinesi ve IoT Edge aygıtı arasındaki bu ayrım gerçek bir dağıtım senaryosunu daha doğru bir şekilde yansıtır ve farklı kavramların düz tutulmasına yardımcı olur.
* Kullanılabilir ikinci bir aygıtınız yoksa, [Azure'da Bir Linux sanal makinesiyle](quickstart-linux.md)bir IoT Edge aygıtı oluşturmak için hızlı başlangıç makalesini kullanın.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katmanlı [Bir IoT](../iot-hub/iot-hub-create-through-portal.md) hub'ı.

## <a name="install-container-engine"></a>Konteyner motorini yükleme

IoT Edge modülleri kapsayıcı olarak paketlenir, bu nedenle geliştirme makinenizde bunları oluşturmak ve yönetmek için bir konteyner motoruna ihtiyacınız vardır. Docker Desktop'ı özellik desteği ve popülerliği nedeniyle geliştirme için öneriyoruz. Windows'daki Docker Desktop, farklı IoT Edge aygıtları için modülleri kolayca geliştirebilmeniz için Linux kapsayıcıları ve Windows kapsayıcıları arasında geçiş yapmanızı sağlar.

Geliştirme makinenize yüklemek için Docker belgelerini kullanın:

* [Docker for Windows'u yükleme](https://docs.docker.com/docker-for-windows/install/)

  * Windows için Docker Desktop'ı yüklediğinizde, Linux veya Windows kapsayıcıları kullanmak isteyip istemediğiniz sorulur. Bu karar kolay bir anahtar kullanılarak her zaman değiştirilebilir. Bu öğretici için, modüllerimiz Linux cihazlarını hedeflediği için Linux kapsayıcılarını kullanıyoruz. Daha fazla bilgi için Windows [ve Linux kapsayıcıları arasında geçiş](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)bilgisine bakın.

* [Mac için Docker Desktop'ı yükleyin](https://docs.docker.com/docker-for-mac/install/)

* Çeşitli Linux platformlarında kurulum bilgileri için [Docker CE hakkında](https://docs.docker.com/install/) bilgi edinin.

## <a name="set-up-vs-code-and-tools"></a>VS Kodu ve araçları ayarlama

IoT Edge modüllerini geliştirmek için Visual Studio Code için IoT uzantılarını kullanın. Bu uzantılar proje şablonları sağlar, dağıtım bildiriminin oluşturulmasını otomatikleştirin ve IoT Edge aygıtlarını izlemenize ve yönetmenize olanak tanır. Bu bölümde, Visual Studio Code ve IoT uzantısını yükler, ardından Visual Studio Code içinden IoT Hub kaynaklarını yönetmek için Azure hesabınızı ayarlarsınız.

1. Geliştirme makinenize [Visual Studio Kodu](https://code.visualstudio.com/) yükleyin.

2. Yükleme tamamlandıktan sonra**Uzantıları** **Görüntüle'yi** > seçin.

3. IoT Hub ve IoT aygıtlarıyla etkileşimkurmanıza ve IoT Edge modülleri geliştirmenize yardımcı olan bir uzantı koleksiyonu olan **Azure IoT Araçlarını**arayın.

4. **Yükle**’yi seçin. Her dahil uzantısı ayrı ayrı yükler.

5. Uzantıları yükleme bittiğinde, > **Komut Paleti**Görüntüle'yi **View**seçerek komut paletini açın.

6. Komut paletinde Azure'u arayın ve **seçin: Oturum açın.** Azure hesabınızda oturum açmak için yönergeleri izleyin.

7. Yine komut paletinde Azure IoT Hub'ını arayın ve **seçin: IoT Hub'ını seçin.** Azure aboneliğinizi ve IoT hub'ınızı seçmek için istemleri izleyin.

8. Soldaki etkinlik çubuğundaki simgeyi seçerek veya **Görünüm** > **Gezgini'ni**seçerek Visual Studio Code'un explorer bölümünü açın.

9. Explorer bölümünün alt kısmında, daraltılmış **Azure IoT Hub Aygıtları** menüsünü genişletin. Komut paleti aracılığıyla seçtiğiniz IoT hub'ı ile ilişkili aygıtları ve IoT Edge aygıtlarını görmeniz gerekir.

   ![Aygıtları IoT hub'ınızda görüntüleme](./media/tutorial-develop-for-linux/view-iot-hub-devices.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Yeni bir modül projesi oluşturma

Azure IoT Araçları uzantısı, Visual Studio Code'daki desteklenen tüm IoT Edge modül dilleri için proje şablonları sağlar. Bu şablonlar, IoT Edge'i sınamak veya şablonu kendi iş mantığınızla özelleştirmeniz için bir başlangıç noktası vermek için bir çalışma modülü dağıtmanız gereken tüm dosyalara ve kodlara sahiptir.

Bu öğretici için, en yaygın olarak kullanılan şablon olduğu için C# modülü şablonu kullanırız.

### <a name="create-a-project-template"></a>Proje şablonu oluşturma

Visual Studio Code komut paletinde **Azure IoT Edge: New IoT Edge Solution'ı**arayın ve seçin. İstemleri izleyin ve çözümünüzü oluşturmak için aşağıdaki değerleri kullanın:

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan **EdgeSolution'ı**kabul edin. |
   | Modül şablonunu seçin | **C# modülseçin.** |
   | Modül adı sağlayın | Varsayılan **Örnek Modül'u**kabul edin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı resminiz, son adımda sağladığınız addan önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son resim deposu kayıt defteri \<adı\>.azurecr.io/samplemodule gibi görünüyor. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-develop-for-linux/image-repository.png)

Yeni çözümünüz Visual Studio Code penceresine yüklendikten sonra, oluşturduğu dosyalarla ilgili bilgi edinebilirsiniz:

* **.vscode** klasörü, hata ayıklama modülleri için kullanılan **launch.json**adlı bir dosya içerir.
* **Modüller** klasörü, çözümünüzdeki her modül için bir klasör içerir. Şu anda, bu sadece **SampleModule**veya modüle hangi ismi verdiyseniz olmalıdır. SampleModule klasörü ana program kodunu, modül meta verilerini ve birkaç Docker dosyasını içerir.
* **.env** dosyası, konteyner kayıt defterinize kimlik bilgilerini tutar. Bu kimlik bilgileri, kapsayıcı görüntülerini çekebilmek için IoT Edge aygıtınızla paylaşılır.
* **Deployment.debug.template.json** dosyası ve **deployment.template.json** dosyası, dağıtım bildirimi oluşturmanıza yardımcı olan şablonlardır. *Dağıtım bildirimi,* bir aygıtta tam olarak hangi modüllerin dağıtılmasını istediğinizi, bunların nasıl yapılandırılması gerektiğini ve birbirleriyle ve bulutla nasıl iletişim kurabileceklerini tanımlayan bir dosyadır. Şablon dosyaları bazı değerler için işaretçiler kullanır. Şablonu gerçek bir dağıtım bildirimine dönüştürdüğünüzde, işaretçiler diğer çözüm dosyalarından alınan değerlerle değiştirilir. Dağıtım şablonunuzda iki ortak yer tutucuyu bulun:

  * Kayıt bilgileri bölümünde, adresi oluşturduğunuzda sağladığınız bilgilerden otomatik olarak doldurulur. Ancak, kullanıcı adı ve parola .env dosyasında depolanan değişkenlere başvurur. Bu yapılandırma güvenlik içindir, çünkü .env dosyası git yoksayılır, ancak dağıtım şablonu yok değildir.
  * Örnekmodül bölümünde, çözümü oluştururken görüntü deposunu sağlamanız alamamış olsa bile kapsayıcı görüntüsü doldurulmaz. Bu yer tutucu, SampleModule klasöründeki **module.json** dosyasını işaret ediyor. Bu dosyaya giderseniz, görüntü alanının deponun değil, aynı zamanda sürümün ve kapsayıcının platformundan oluşan bir etiket değerini içerdiğini görürsünüz. Geliştirme döngünüzün bir parçası olarak sürümü el ile yineleyebilir ve bu bölümde daha sonra tanıttığımız bir anahtar cıkullanarak kapsayıcı platformunu seçebilirsiniz.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Kayıt kimlik bilgilerinizi IoT Edge aracısına sağlama

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Kapsayıcı görüntülerinizi IoT Edge aygıtına çekmek için çalışma zamanının bu kimlik bilgilerine ihtiyacı vardır.

IoT Edge uzantısı, kapsayıcı kayıt defteri kimlik bilgilerinizi Azure'dan çekmeye ve bunları ortam dosyasında doldurmaya çalışır. Kimlik bilgilerinizin zaten dahil olup olmadığını denetleyin. Değilse, şimdi ekleyin:

1. Modül çözümünde **.env** dosyasını açın.
2. Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerlerini ekleyin.
3. Değişikliklerinizi .env dosyasına kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code, Linux AMD64 ve ARM32v7 cihazları için C# modülleri geliştirebilir. Her çözümde hangi mimariyi hedeflediğinizi seçmeniz gerekir, çünkü bu kapsayıcının nasıl oluşturulup çalıştığını etkiler. Varsayılan Linux AMD64 olduğunu.

1. Komut paletini açın ve **Azure IoT Edge: Edge Solution için Varsayılan Hedef Platformu'nu ayarlayın**veya pencerenin altındaki yan çubuktaki kısayol simgesini seçin.

   ![Yan çubuktaki mimari simgesini seçin](./media/tutorial-develop-for-linux/select-architecture.png)

2. Komut paletinde, seçenekler listesinden hedef mimarisini seçin. Bu öğretici için, biz IoT Edge cihaz olarak bir Ubuntu sanal makine kullanıyoruz, bu yüzden varsayılan **amd64**devam edecektir.

### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirme

Oluşturduğunuz çözüm şablonu, bir IoT Edge modülü için örnek kod içerir. Bu örnek modül sadece iletileri alır ve sonra bunları iletilir. Boru hattı işlevi, IoT Edge'de önemli bir kavramı gösterir, bu da modüllerin birbirleriyle nasıl iletişim kurduklarıdır.

Her modül, kodlarında birden çok *giriş* ve *çıkış* kuyruğu bildirilebilir. Aygıtta çalışan IoT Edge hub'ı, bir modülün çıktısından gelen iletileri bir veya daha fazla modülün girişine yönlendirir. Giriş ve çıktıları bildirmek için özel dil diller arasında değişir, ancak kavram tüm modüller arasında aynıdır. Modüller arasında yönlendirme hakkında daha fazla bilgi [için, bildir rotalarını bildir'e](module-composition.md#declare-routes)bakın.

Proje şablonuyla birlikte gelen örnek C# kodu, .NET için IoT Hub SDK'daki [ModuleClient Sınıfını](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet) kullanır.

1. **Modüllerin/SampleModule/klasörün** içinde bulunan **Program.cs** dosyasını açın.

2. program.cs, **SetInputMessageHandlerAsync** yöntemini bulun.

3. [SetInputMessageHandlerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.setinputmessagehandlerasync?view=azure-dotnet) yöntemi gelen iletileri almak için bir giriş sırası ayarlar. Bu yöntemi gözden geçirin ve **giriş1**adı verilen bir giriş kuyruğunun nasıl baş harfe geldiğini görün.

   ![SetInputMessageCallback constructor'da giriş adını bulma](./media/tutorial-develop-for-linux/declare-input-queue.png)

4. Ardından **SendEventAsync** yöntemini bulun.

5. [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.sendeventasync?view=azure-dotnet) yöntemi, iletileri alır ve bunları iletmek için bir çıkış sırası oluşturur. Bu yöntemi gözden geçirin ve **output1**adı verilen bir çıkış kuyruğunun başlatılmasını görün.

   ![Çıkış adını SendEventToOutputAsync'de bulun](./media/tutorial-develop-for-linux/declare-output-queue.png)

6. **deployment.template.json** dosyasını açın.

7. İstenilen $edgeAgent özelliklerinin **modülözelliklerini** bulun.

   Burada listelenen iki modül olmalıdır. İlki, modüllerinizi test etmek için kullanabileceğiniz simüle sıcaklık verilerini sağlamak için varsayılan olarak tüm şablonlara dahil olan **SimulatedTemperatureSensor'dur.** İkincisi, bu çözümün bir parçası olarak oluşturduğunuz **Örnek Modül** modülüdür.

8. Dosyanın alt kısmında, **$edgeHub** modülü için istenen özellikleri bulun.

   IoT Edge hub modülünün işlevlerinden biri, iletileri dağıtımdaki tüm modüller arasında yönlendirmektir. Rotaözelliğindeki değerleri gözden **geçirin.** İlk rota, **SampleModuleToIoTHub**, Örnek Modülü**\*** modülündeki herhangi bir çıkış kuyruğundan gelen iletileri belirtmek için joker karakter () kullanır. Bu iletiler, IoT Hub'ı gösteren ayrılmış bir ad olan *$upstream'a*gider. İkinci rota olan sensorToSampleModule, Simüle Edilen TemperatureSensor modülünden gelen iletileri alır ve ÖrnekModül kodunda başharfe bürünmüş gördüğünüz *giriş1* giriş kuyruğuna yönlendirir.

   ![Deployment.template.json'daki yolları gözden geçirin](./media/tutorial-develop-for-linux/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>Çözümünüzü oluşturun ve itin

Bazı anahtar dağıtım kavramlarını anlamak için modül kodunu ve dağıtım şablonlarını gözden geçirdiniz. Şimdi, SampleModule kapsayıcı görüntüsünü oluşturmaya ve konteyner kayıt defterinize itmeye hazırsınız. Visual Studio Code için IoT araçları uzantısı ile bu adım, şablon dosyasındaki bilgilere ve çözüm dosyalarındaki modül bilgilerine dayalı dağıtım bildirimini de oluşturur.

### <a name="sign-in-to-docker"></a>Docker ile oturum açın

Konteyner resminizi kayıt defterinde depolanacak şekilde itebilmesi için konteyner kayıt kimlik bilgilerinizi Docker'a sağlayın.

1. **View** > **Terminalini**seçerek Visual Studio Code entegre terminalini açın.

2. Kayıt defterini oluşturduktan sonra kaydettiğiniz Azure kapsayıcı kayıt kimlik bilgileriyle Docker'da oturum açın.

   ```cmd/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

### <a name="build-and-push"></a>Oluşturma ve itme

Visual Studio Code artık konteyner kayıt defterinize erişebilir, bu nedenle çözüm kodunu bir kapsayıcı görüntüsüne dönüştürme nin zamanı gelmiştir.

1. Visual Studio Code gezgininde **deployment.template.json** dosyasına sağ tıklayın ve **IoT Edge Solution'ı Oluştur ve İtimi'ni**seçin.

   ![IoT Edge modülleri oluşturma ve itme](./media/tutorial-develop-for-linux/build-and-push-modules.png)

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, dağıtım şablonundaki ve diğer çözüm dosyalarındaki bilgiler dışında oluşturulmuş, tam dağıtım bildirimini tutan **config** adlı çözümde yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

   Bu işlem ilk seferinde birkaç dakika sürebilir, ancak komutları bir sonraki kez çalıştırdığınızda daha hızlıdır.

2. **Deployment.amd64.json** dosyasını yeni oluşturulan config klasöründe açın. Dosya adı hedef mimariyi yansıtır, bu nedenle farklı bir mimari seçerseniz farklı olacaktır.

3. Yer tutucuları olan iki parametrenin uygun değerleriyle doldurulduğuna dikkat edin. **Kayıt bilgileri bölümünde** kayıt defteri kullanıcı adınızı ve şifrenizi .env dosyasından çıkarınız. **SampleModule,** module.json dosyasındaki ad, sürüm ve mimari etiketiyle tam görüntü deposuna sahiptir.

4. SampleModule klasöründeki **module.json** dosyasını açın.

5. Modül görüntüsünün sürüm numarasını değiştirin. (Sürüm, $schema versiyonu değil.) Örneğin, yama sürüm numarasını modül kodunda küçük bir düzeltme yapmışız gibi **0.0.2'ye** kadar artım.

   >[!TIP]
   >Modül sürümleri sürüm denetimini sağlar ve güncelleştirmeleri üretime dağıtmadan önce değişiklikleri küçük bir aygıt kümesinde sınamanıza olanak tanır. Modül sürümünü oluşturmadan ve itmeden önce artırmazsanız, depoyu konteyner kayıt defterinize yazarsınız.

6. Değişikliklerinizi module.json dosyasına kaydedin.

7. **Deployment.template.json** dosyasına tekrar sağ tıklayın ve yine **IoT Edge Solution'ı Oluştur ve It' i'yi**seçin.

8. **Deployment.amd64.json** dosyasını yeniden açın. Yapı ve itme komutunu yeniden çalıştırdığınızda yeni bir dosyanın oluşturulmadığına dikkat edin. Bunun yerine, aynı dosya değişiklikleri yansıtacak şekilde güncelleştirildi. SampleModule görüntüsü artık kapsayıcının 0.0.2 sürümüne işaret ediyor.

9. Yapı ve itme komutunun ne yaptığını daha fazla doğrulamak için [Azure portalına](https://portal.azure.com) gidin ve konteyner kayıt defterinize gidin.

10. Konteyner kayıt defterinizde, **Depositories'i** seçin ve **örnek modülü.** Görüntünün her iki sürümünün de kayıt defterine itildiğini doğrulayın.

    ![Konteyner kayıt defterinde her iki resim sürümlerini görüntüleme](./media/tutorial-develop-for-linux/view-repository-versions.png)

<!--Alternative steps: Use VS Code Docker tools to view ACR images with tags-->

### <a name="troubleshoot"></a>Sorun giderme

Modül görüntünüzinşa ederken ve iterken hatalarla karşılaşırsanız, genellikle geliştirme makinenizdeki Docker yapılandırmasıyla ilgili olmalıdır. Yapılandırmanızı gözden geçirmek için aşağıdaki denetimleri kullanın:

* Komutu, `docker login` konteyner kayıt defterinizden kopyaladığınız kimlik bilgilerini kullanarak çalıştırdınız mı? Bu kimlik bilgileri, Azure'da oturum açtığınızda kullandığınız kimlik bilgilerinden farklıdır.
* Konteyner deponuz doğru mu? Doğru konteyner kayıt defteri adınız ve doğru modül adınız var mı? Kontrol etmek için SampleModule klasöründeki **module.json** dosyasını açın. Depo değeri ** \<kayıt defteri\>adı .azurecr.io/samplemodule**gibi görünmelidir.
* Modülünüz için Örnek **Modül'den** farklı bir ad kullandıysanız, bu ad çözüm boyunca tutarlı mıdır?
* Makineniz, inşa ettiğiniz konteynerlerin aynısı mı çalışıyor? Bu öğretici Linux IoT Edge cihazlar için, bu nedenle Visual Studio Code yan çubuğunda **amd64** veya **arm32v7** söylemek gerekir ve Docker Masaüstü Linux kapsayıcıları çalışıyor olmalıdır.  

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Yapılı kapsayıcı görüntülerinin konteyner kayıt defterinizde depolandığını doğruladınız, bu nedenle bunları bir aygıta dağıtma nın zamanı geldiğinden. IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio Code gezgininde Azure IoT Hub Aygıtları bölümünü genişletin.

2. Dağıtmak istediğiniz IoT Edge aygıtına sağ tıklayın ve ardından **Tek Aygıt için Dağıtım Oluştur'u**seçin.

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-develop-for-linux/create-deployment.png)

3. Dosya gezgininde, **config** klasörüne gidin ve ardından **deployment.amd64.json** dosyasını seçin.

   Kapsayıcı kayıt defteri kimlik bilgileri veya modül görüntü değerleri bulunmayan deployment.template.json dosyasını kullanmayın. Bir Linux ARM32 aygıtını hedefliyorsanız, dağıtım bildirimi dağıtım.arm32v7.json olarak adlandırılır.

4. IoT Edge cihazınızın ayrıntılarını genişletin ve ardından cihazınızın **Modüller** listesini genişletin.

5. Cihazınızda çalışan Simüle Sıcaklık Sensörü ve Örnek Modül modüllerini görene kadar cihaz görünümünü güncellemek için yenileme düğmesini kullanın.

   Her iki modülün de başlaması birkaç dakika sürebilir. IoT Edge çalışma zamanının yeni dağıtım bildirimini alması, kapsayıcı çalışma zamanındaki modül görüntülerini aşağı çekmesi ve ardından her yeni modülü başlatması gerekir.

   ![IoT Edge cihazınızda çalışan modülleri görüntüleme](./media/tutorial-develop-for-linux/view-running-modules.png)

## <a name="view-messages-from-device"></a>Aygıttan gelen iletileri görüntüleme

ÖrnekModül kodu iletileri giriş kuyruğundan alır ve çıkış kuyruğundan geçirir. Dağıtım bildirimi, simüle edilen TemperatureSensor'dan Örnek Modüle iletileri ileten yolları beyan etti ve ardından SampleModule'den IoT Hub'a iletiler gönderdi. Visual Studio Code için Azure IoT araçları, iletileri ioT Hub'a bireysel aygıtlarınızdan geldiklerinde görmenizi sağlar.

1. Visual Studio Code gezgininde, izlemek istediğiniz IoT Edge aygıtına sağ tıklayın ve ardından Yerleşik **Etkinlik Bitiş Noktasını İzle'yi başlat'ı**seçin.

2. IoT hub'ınıza gelen iletileri görmek için Visual Studio Code'daki çıkış penceresini izleyin.

   ![Gelen aygıtı bulut iletilerine görüntüleme](./media/tutorial-develop-for-linux/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Aygıttaki değişiklikleri görüntüleme

Cihazınızda neler olduğunu görmek istiyorsanız, IoT Edge çalışma süresini ve cihazınızda çalışan modülleri incelemek için bu bölümdeki komutları kullanın.

Bu bölümdeki komutlar geliştirme makineniz için değil, IoT Edge aygıtınız içindir. IoT Edge aygıtınız için sanal bir makine kullanıyorsanız, hemen bağlanın. Azure'da, sanal makinenin genel bakış sayfasına gidin ve güvenli kabuk bağlantısına erişmek için **Bağlan'ı** seçin.

* Cihazınıza dağıtılan tüm modülleri görüntüleyin ve durumlarını kontrol edin:

   ```bash
   iotedge list
   ```

   Dört modül görmelisiniz: iki IoT Edge çalışma zamanı modülü, Simüle TemperatureSensor ve SampleModule. Dördü de çalışan olarak listelenmelidir.

* Belirli bir modül için günlükleri inceleyin:

   ```bash
   iotedge logs <module name>
   ```

   IoT Edge modülleri büyük/küçük harf duyarlıdır.

   Simüle Edilen Sıcaklık Sensörü ve Örnek Modül günlükleri, işledikleri iletileri göstermelidir. EdgeAgent modülü diğer modülleri başlatmaktan sorumludur, bu nedenle günlükleri dağıtım bildiriminin uygulanması hakkında bilgi sahibi olacaktır. Herhangi bir modül listede yoksa veya çalışmıyorsa, edgeAgent günlüklerinde büyük olasılıkla hatalar olacaktır. edgeHub modülü, modüller ve IoT Hub arasındaki iletişimden sorumludur. Modüller çalışır durumdaysa, ancak iletiler IoT hub'ınıza gelmiyorsa, edgeHub günlüklerinde büyük olasılıkla hatalar olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, geliştirme makinenizde Visual Studio Code'u kurdunuz ve ilk IoT Edge modülünüzü ondan dağıttınız. Artık temel kavramları bildiğinize göre, içinden geçen verileri analiz edebilsin diye bir modüle işlevsellik eklemeyi deneyin. Tercih ettiğiniz dili seçin:

> [!div class="nextstepaction"]
> [C](tutorial-c-module.md)
> [C#](tutorial-csharp-module.md)
> [Java](tutorial-java-module.md)
> [Düğümü.js](tutorial-node-module.md)
> [Python](tutorial-python-module.md)
