---
title: Öğretici - Azure IoT Edge kullanan Windows aygıtları için modül geliştirme
description: Bu öğretici, Windows aygıtları için Windows kapsayıcılarını kullanarak IoT Edge modülleri geliştirmek için geliştirme makinenizi ve bulut kaynaklarınızı kurma da geçer
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 58a63c9e11cf86318f0e9f051d034cbbaf7c40a9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76772258"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Öğretici: Windows aygıtları için IoT Edge modülleri geliştirin

IoT Edge çalıştıran Windows aygıtlarına kod geliştirmek ve dağıtmak için Visual Studio'yı kullanın.

Hızlı bir başlangıç olarak, Windows sanal makinesini kullanarak bir IoT Edge aygıtı oluşturdunuz ve Azure Marketi'nden önceden oluşturulmuş bir modül dağıttınız. Bu öğretici, kendi kodunuzu geliştirmek ve bir IoT Edge aygıtına dağıtmak için gerekenleri gözden geçirir. Bu öğretici, belirli programlama dilleri veya Azure hizmetleri hakkında daha fazla ayrıntıya giren diğer öğreticiler için yararlı bir ön koşuldur.

Bu öğretici, Bir Windows **aygıtına C# modülü**dağıtma örneğini kullanır. Bu örnek, en yaygın geliştirme senaryosu olduğu için seçildi. Farklı bir dilde geliştirmek istiyorsanız veya Azure hizmetlerini modül olarak dağıtmayı planlıyorsanız, bu öğretici geliştirme araçları hakkında bilgi edinmek için yine de yararlı olacaktır. Geliştirme kavramlarını anladıktan sonra, ayrıntılara dalmak için tercih ettiğiniz dili veya Azure hizmetini seçebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Geliştirme makinenizi ayarlayın.
> * Yeni bir proje oluşturmak için Visual Studio için IoT Edge araçlarını kullanın.
> * Projenizi kapsayıcı olarak oluşturun ve azure kapsayıcı kayıt defterinde saklayın.
> * Kodunuzu bir IoT Edge aygıtına dağıtın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="key-concepts"></a>Önemli kavramlar

Bu öğretici bir IoT Edge modülü geliştirme yoluyla yürür. *Bir IoT Edge modülü,* ya da bazen sadece kısa *modül,* yürütülebilir kod içeren bir kapsayıcıdır. Bir IoT Edge aygıtına bir veya daha fazla modül dağıtabilirsiniz. Modüller sensörlerden veri alma, veri analitiği veya veri temizleme işlemleri gerçekleştirme veya bir IoT hub'ına ileti gönderme gibi belirli görevleri gerçekleştirir. Daha fazla bilgi için [bkz.](iot-edge-modules.md)

IoT Edge modülleri geliştirirken, geliştirme makinesi ile modülün sonunda dağıtılacak hedef IoT Edge aygıtı arasındaki farkı anlamak önemlidir. Modül kodunuzu tutmak için oluşturduğunuz kapsayıcı, *hedef aygıtın*işletim sistemiyle (OS) eşleşmelidir. Windows kapsayıcı geliştirme için bu kavram daha basittir, çünkü Windows kapsayıcıları yalnızca Windows işletim sistemlerinde çalışır. Ancak, örneğin, Linux IoT Edge aygıtları için modüller oluşturmak için Windows geliştirme makinenizi kullanabilirsiniz. Bu senaryoda, geliştirme makinenizin Linux kapsayıcıları çalıştırdığından emin olabilirsiniz. Bu öğretici geçmesi gibi, geliştirme makinesi *işletim sistemi* ve *konteyner işletim sistemi*arasındaki farkı akılda tutun.

Bu öğretici, IoT Edge çalıştıran Windows aygıtlarını hedefler. Windows IoT Edge aygıtları Windows kapsayıcılarını kullanır. Windows aygıtları için geliştirmek için Visual Studio'u kullanmanızı öneririz, bu nedenle bu öğretici bunu kullanır. İki araç arasında destek farklılıkları olsa da Visual Studio Code'u da kullanabilirsiniz.

Aşağıdaki tabloda, Visual Studio Code ve Visual Studio'daki **Windows kapsayıcıları** için desteklenen geliştirme senaryoları listelenir.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Azure hizmetleri** | Azure İşlevleri <br> Azure Stream Analytics |   |
| **Diller** | C# (hata ayıklama desteklenmiyor) | C <br> C# |
| **Daha fazla bilgi** | [Görsel Stüdyo Kodu için Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Visual Studio 2017 için Azure IoT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools)<br>[Visual Studio için Azure IoT Edge Araçları 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

## <a name="prerequisites"></a>Ön koşullar

Geliştirme makinesi:

* Windows 10 1809 güncelleme veya yeni ile.
* Geliştirme tercihlerinize bağlı olarak kendi bilgisayarınızı veya sanal makinenizi kullanabilirsiniz.
  * Geliştirme makinenizin iç içe sanallaştırmayı desteklediğinden emin olun. Bu özellik, bir sonraki bölümde yüklediğiniz bir kapsayıcı altyapısını çalıştırmak için gereklidir.
* [Git'i yükleyin.](https://git-scm.com/)

Windows'da bir Azure IoT Edge aygıtı:

* Geliştirme makinenizde IoT Edge'i çalıştırmamanızı, bunun yerine ayrı bir aygıt kullanmanızı öneririz. Geliştirme makinesi ve IoT Edge aygıtı arasındaki bu ayrım gerçek bir dağıtım senaryosunu daha doğru bir şekilde yansıtır ve farklı kavramların düz tutulmasına yardımcı olur.
* Kullanılabilir ikinci bir aygıtınız yoksa, [Windows sanal makinesiyle](quickstart.md)Azure'da bir IoT Edge aygıtı oluşturmak için hızlı başlangıç makalesini kullanın.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katmanlı [Bir IoT](../iot-hub/iot-hub-create-through-portal.md) hub'ı.

## <a name="install-container-engine"></a>Konteyner motorini yükleme

IoT Edge modülleri kapsayıcı olarak paketlenir, bu nedenle konteynerleri oluşturmak ve yönetmek için geliştirme makinenizde bir konteyner motoruna ihtiyacınız vardır. Bir konteyner motoru olarak birçok özelliği ve popülerliği nedeniyle geliştirme için Docker Desktop kullanmanızı öneririz. Windows bilgisayarındaki Docker Desktop ile, farklı IoT Edge aygıtları için modüller kolayca geliştirebilmeniz için Linux kapsayıcıları ve Windows kapsayıcıları arasında geçiş yapabilirsiniz.

Geliştirme makinenize yüklemek için Docker belgelerini kullanın:

* [Docker for Windows'u yükleme](https://docs.docker.com/docker-for-windows/install/)

  * Windows için Docker Desktop'ı yüklediğinizde, Linux veya Windows kapsayıcıları kullanmak isteyip istemediğiniz sorulur. Bu öğretici için **Windows kapsayıcılarını**kullanın. Daha fazla bilgi için Windows [ve Linux kapsayıcıları arasında geçiş](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)bilgisine bakın.

## <a name="set-up-visual-studio-and-tools"></a>Visual Studio ve araçları ayarlama

Visual Studio için IoT uzantıları, IoT Edge modüllerini geliştirmenize yardımcı olur. Bu uzantılar proje şablonları sağlar, dağıtım bildiriminin oluşturulmasını otomatikleştirin ve IoT Edge aygıtlarını izlemenize ve yönetmenize olanak tanır. Bu bölümde Visual Studio'yu ve IoT Edge uzantısını yükler, ardından Visual Studio'daki IoT Hub kaynaklarını yönetmek için Azure hesabınızı ayarlarsınız.

Bu öğretici Visual Studio 2019 için geliştirme adımlarını öğretir. Visual Studio 2017 kullanıyorsanız (sürüm 15.7 veya üzeri), adımlar benzer. Visual Studio Code'u kullanmayı tercih ediyorsanız, [Azure IoT Edge için modüller geliştirmek ve hata ayıklamak için Visual Studio Code'u kullanın'daki yönergelere](how-to-vs-code-develop-module.md)bakın.

1. Görsel Stüdyo 2019'u geliştirme makinenizde hazırlayın.

   * Geliştirme makinenizde Visual Studio yoksa, Visual [Studio 2019'u](https://docs.microsoft.com/visualstudio/install/install-visual-studio) aşağıdaki iş yükleriyle yükleyin:

      * Azure geliştirme
      * C++ ile masaüstü geliştirme
      * .NET Core çoklu platform geliştirme

   * Geliştirme makinenizde Visual Studio 2019 zaten varsa, gerekli iş yüklerini eklemek için [Visual Studio'yu değiştir'deki](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) adımları izleyin.

2. Visual Studio 2019 için [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısını indirin ve yükleyin.

   Visual Studio 2017 kullanıyorsanız (sürüm 15.7 veya üzeri), [Visual Studio 2017 için Azure IoT Edge Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools)indirin ve yükleyin.

3. Yüklemeleriniz tamamlandığında Visual Studio 2019'u açın ve **kodsuz Devam'ı**seçin.

4. **Bulut Gezgini'ni** **Görüntüle'yi** > seçin.

5. Bulut gezginindeki profil simgesini seçin ve daha önce oturum açmadıysanız Azure hesabınızda oturum açın.

6. Oturum açmanızdan sonra Azure abonelikleriniz listelenir. IoT hub'ınıza sahip aboneliği genişletin.

7. Aboneliğiniz **altında, IoT Hub'larını ve IoT** hub'ınızı genişletin. IoT aygıtlarınızın bir listesini görmeniz gerekir ve bunları yönetmek için bu gezgini kullanabilirsiniz.

   ![Bulut Gezgini'nde IoT Hub kaynaklarına erişin](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Yeni bir modül projesi oluşturma

Azure IoT Edge Tools uzantısı, Visual Studio'daki desteklenen tüm IoT Edge modül dilleri için proje şablonları sağlar. Bu şablonlar, IoT Edge'i sınamak veya şablonu kendi iş mantığınızla özelleştirmeniz için bir başlangıç noktası vermek için bir çalışma modülü dağıtmanız gereken tüm dosyalara ve kodlara sahiptir.

1. **Dosya** > **Yeni** > **Proje'yi seçin...**

2. Yeni proje **penceresinde, IoT Edge'i** arayın ve **Azure IoT Edge (Windows amd64)** projesini seçin. **İleri**'ye tıklayın.

   ![Yeni bir Azure IoT Edge projesi oluşturun](./media/tutorial-develop-for-windows/new-project.png)

3. Yeni proje pencerenizi yapılandırıldığında, projeyi ve çözümü **CSharpTutorialApp gibi**açıklayıcı bir şeye yeniden adlandırın. Projeyi oluşturmak için **Oluştur'u** tıklatın.

   ![Yeni bir Azure IoT Edge projesini yapılandırma](./media/tutorial-develop-for-windows/configure-project.png)

4. Modül Ekle penceresinde, projenizi aşağıdaki değerlerle yapılandırın:

   | Alan | Değer |
   | ----- | ----- |
   | Görsel Stüdyo Şablonu | **C# modülünü**seçin. |
   | Modül Adı | Varsayılan **IotEdgeModule1'i kabul edin.** |
   | Depo Url'si | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz modül proje ad değerinden önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. **Azure portalındaki** konteyner kayıt defterinizin **Genel Bakış** sayfasından Giriş sunucusu değerini alabilirsiniz. <br><br> Son resim deposu kayıt defteri \<adı\>.azurecr.io/iotedgemodule1 gibi görünüyor. |

      ![Projenizi hedef aygıt, modül türü ve konteyner kayıt defteri için yapılandırın](./media/tutorial-develop-for-windows/add-module-to-solution.png)

5. Modülü oluşturmak için **Ekle'yi** seçin.

Yeni projeniz Visual Studio penceresine yüklendikten sonra, oluşturduğu dosyalarla ilgili bilgi edinebilirsiniz:

* **CSharpTutorialApp**adlı bir IoT Edge projesi.
  * **Modüller** klasörü, projede yer alan modüllere işaretçiler içerir. Bu durumda, sadece IotEdgeModule1 olmalıdır.
  * Gizli **.env** dosyası, kimlik bilgilerini konteyner kayıt defterinize tutar. Bu kimlik bilgileri, kapsayıcı görüntülerini çekebilmek için IoT Edge aygıtınızla paylaşılır.
  * **Deployment.template.json** dosyası, bir dağıtım bildirimi oluşturmanıza yardımcı olacak bir şablondur. *Dağıtım bildirimi,* bir aygıtta tam olarak hangi modüllerin dağıtılmasını istediğinizi, bunların nasıl yapılandırılması gerektiğini ve birbirleriyle ve bulutla nasıl iletişim kurabileceklerini tanımlayan bir dosyadır.
    > [!TIP]
    > Kayıt bilgileri bölümünde, adresi oluşturduğunuzda sağladığınız bilgilerden otomatik olarak doldurulur. Ancak, .env dosyasında depolanan kullanıcı adı ve parola başvuru değişkenleri. Bu güvenlik içindir, çünkü .env dosyası git yoksayılır, ancak dağıtım şablonu yok değildir.
* **IotEdgeModule1**adlı bir IoT Edge modül projesi.
  * Program.cs **program.cs** dosyası, proje şablonuyla birlikte gelen varsayılan C# modül kodunu içerir. Varsayılan modül bir kaynaktan giriş alır ve ioT Hub'a iletin.
  * **Module.json** dosyası, modül hakkında tam görüntü deposu, görüntü sürümü ve desteklenen her platform için hangi Dockerfile'ın kullanılacağı gibi ayrıntıları tutar.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Kayıt kimlik bilgilerinizi IoT Edge aracısına sağlama

IoT Edge çalışma zamanı, kapsayıcı görüntülerinizi IoT Edge aygıtına çekmek için kayıt defteri kimlik bilgilerinize ihtiyaç duyar. IoT Edge uzantısı, kapsayıcı kayıt defteri bilgilerinizi Azure'dan çekmeye ve dağıtım şablonunda doldurmaya çalışır.

1. Modül çözümünde **deployment.template.json** dosyasını açın.

1. İstenilen $edgeAgent özelliklerinde **kayıt kimlik bilgileri** özelliğini bulun. Projeyi oluştururken sağladığınız bilgilerden kayıt defteri adresinizotomatik olarak doldurulmalı ve ardından kullanıcı adı ve parola alanları değişken adlar içermelidir. Örnek:

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "$CONTAINER_REGISTRY_USERNAME_<registry name>",
       "password": "$CONTAINER_REGISTRY_PASSWORD_<registry name>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

1. Modül çözümünde **.env** dosyasını açın. (Çözüm Gezgini'nde varsayılan olarak gizlenir, bu nedenle görüntülemek için **Tüm Dosyaları Göster** düğmesini seçmeniz gerekebilir.)

1. Azure kapsayıcı kayıt defterinizden kopyaladığınız **Kullanıcı Adı** ve **Parola** değerlerini ekleyin.

1. Değişikliklerinizi .env dosyasına kaydedin.

### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirme

Oluşturduğunuz çözüm şablonu, bir IoT Edge modülü için örnek kod içerir. Bu örnek modül sadece iletileri alır ve sonra bunları iletilir. Boru hattı işlevi, IoT Edge'de önemli bir kavramı gösterir, bu da modüllerin birbirleriyle nasıl iletişim kurduklarıdır.

Her modül, kodlarında birden çok *giriş* ve *çıkış* kuyruğu bildirilebilir. Aygıtta çalışan IoT Edge hub'ı, bir modülün çıktısından gelen iletileri bir veya daha fazla modülün girişine yönlendirir. Giriş ve çıktıları bildirmek için özel dil diller arasında değişir, ancak kavram tüm modüller arasında aynıdır. Modüller arasında yönlendirme hakkında daha fazla bilgi [için, bildir rotalarını bildir'e](module-composition.md#declare-routes)bakın.

Proje şablonuyla birlikte gelen örnek C# kodu, .NET için IoT Hub SDK'daki [ModuleClient Sınıfını](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet) kullanır.

1. **program.cs** **dosyasında, SetInputMessageHandlerAsync** yöntemini bulun.

2. [SetInputMessageHandlerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.setinputmessagehandlerasync?view=azure-dotnet) yöntemi gelen iletileri almak için bir giriş sırası ayarlar. Bu yöntemi gözden geçirin ve **giriş1**adı verilen bir giriş kuyruğunun nasıl baş harfe geldiğini görün.

   ![SetInputMessageHandlserAsync oluşturucudaki giriş adını bulma](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Ardından **SendEventAsync** yöntemini bulun.

4. [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.sendeventasync?view=azure-dotnet) yöntemi, iletileri alır ve bunları iletmek için bir çıkış sırası oluşturur. Bu yöntemi gözden geçirin ve **output1**adı verilen bir çıkış kuyruğunun başlatılmasını görün.

   ![SendEventAsync oluşturucuda çıktı adını bulma](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. **deployment.template.json** dosyasını açın.

6. İstenilen $edgeAgent özelliklerinin **modülözelliklerini** bulun.

   Burada listelenen iki modül olmalıdır. İlki, modüllerinizi test etmek için kullanabileceğiniz simüle sıcaklık verilerini sağlamak için varsayılan olarak tüm şablonlara dahil olan **SimulatedTemperatureSensor'dur.** İkincisi, bu projenin bir parçası olarak oluşturduğunuz **IotEdgeModule1** modülüdür.

   Bu modülözelliği, cihazınıza veya aygıtlarınıza dağıtıma hangi modüllerin dahil edilmesi gerektiğini bildirir.

7. İstenilen $edgeHub özelliklerinin **güzergah** özelliğini bulun.

   IoT Edge hub modülünün işlevlerinden biri, iletileri dağıtımdaki tüm modüller arasında yönlendirmektir. Rotaözelliğindeki değerleri gözden geçirin. İlk rota, **IotEdgeModule1ToIoTHub , IotEdgeModule1**modülündeki herhangi bir çıkış kuyruğundan gelen herhangi bir iletiyi eklemek için joker karakter ()**\*** kullanır. Bu iletiler, IoT Hub'ı gösteren ayrılmış bir ad olan *$upstream'a*gider. İkinci rota olan **sensorToIotEdgeModule1,** Simüle EdilenTemperatureSensor modülünden gelen mesajları alır ve bunları IotEdgeModule1 modülünün *giriş1* giriş kuyruğuna yönlendirir.

   ![Deployment.template.json'daki yolları gözden geçirin](./media/tutorial-develop-for-windows/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>Çözümünüzü oluşturun ve itin

Bazı anahtar dağıtım kavramlarını anlamak için modül kodunu ve dağıtım şablonlarını gözden geçirdiniz. Şimdi, IotEdgeModule1 konteyner görüntüsünü oluşturmaya ve konteyner kayıt defterinize itmeye hazırsınız. Visual Studio için IoT araçları uzantısı ile bu adım, şablon dosyasındaki bilgilere ve çözüm dosyalarındaki modül bilgilerine dayalı dağıtım bildirimini de oluşturur.

### <a name="sign-in-to-docker"></a>Docker ile oturum açın

Konteyner kayıt kimlik bilgilerinizi geliştirme makinenizdeki Docker'a sağlayın, böylece konteyner resminizi kayıt defterinde depolanacak şekilde zorlayabilir.

1. PowerShell'i veya komut istemini açın.

2. Kayıt defterini oluşturduktan sonra kaydettiğiniz Azure kapsayıcı kayıt kimlik bilgileriyle Docker'da oturum açın.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

### <a name="build-and-push"></a>Oluşturma ve itme

Geliştirme makineniz artık konteyner kayıt defterinize erişebilir ve IoT Edge aygıtlarınız da erişebilir. Proje kodunu kapsayıcı görüntüsüne dönüştürmenin zamanı var.

1. **CSharpTutorialApp** proje klasörüne sağ tıklayın ve **IoT Edge Modüllerini Oluştur ve İtizorla'yı**seçin.

   ![IoT Edge modülleri oluşturma ve itme](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, dağıtım şablonundaki ve diğer çözüm dosyalarındaki bilgiler dışında oluşturulmuş, tam dağıtım bildirimini tutan **config** adlı çözümde yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

   Bu işlem ilk seferinde birkaç dakika sürebilir, ancak komutları bir sonraki kez çalıştırdığınızda daha hızlıdır.

2. Yeni oluşturulan config klasöründe **deployment.windows-amd64.json** dosyasını açın. (Config klasörü Visual Studio'daki Çözüm Gezgini'nde görünmeyebilir. Bu durumda, Çözüm Gezgini görev çubuğundaki **tüm dosyaları göster** simgesini seçin.)

3. IotEdgeModule1 bölümünün **görüntü** parametresini bulun. Görüntünün module.json dosyasından ad, sürüm ve mimari etiketiyle tam görüntü deposu içerdiğine dikkat edin.

4. IotEdgeModule1 klasöründe **module.json** dosyasını açın.

5. Modül görüntüsünün sürüm numarasını değiştirin. (Sürüm, $schema versiyonu değil.) Örneğin, yama sürüm numarasını modül kodunda küçük bir düzeltme yapmışız gibi **0.0.2'ye** kadar artım.

   >[!TIP]
   >Modül sürümleri sürüm denetimini sağlar ve güncelleştirmeleri üretime dağıtmadan önce değişiklikleri küçük bir aygıt kümesinde sınamanıza olanak tanır. Modül sürümünü oluşturmadan ve itmeden önce artırmazsanız, depoyu konteyner kayıt defterinize yazarsınız.

6. Değişikliklerinizi module.json dosyasına kaydedin.

7. **CSharpTutorialApp** proje klasörüne tekrar sağ tıklayın ve **IoT Edge modüllerini** yeniden Oluştur ve Itme'yi seçin.

8. **Deployment.windows-amd64.json** dosyasını yeniden açın. Yapı ve itme komutunu yeniden çalıştırdığınızda yeni bir dosyanın oluşturulmadığına dikkat edin. Bunun yerine, aynı dosya değişiklikleri yansıtacak şekilde güncelleştirildi. IotEdgeModule1 görüntüsü artık kapsayıcının 0.0.2 sürümüne işaret ediyor. Dağıtım bildirimindeki bu değişiklik, IoT Edge aygıtına bir modülün yeni bir sürümünün çekilebildiğini nasıl söylediğinizdir.

9. Yapı ve itme komutunun ne yaptığını daha fazla doğrulamak için [Azure portalına](https://portal.azure.com) gidin ve konteyner kayıt defterinize gidin.

10. Konteyner kayıt defterinizde, **Depositories'i** seçin ve **iotedgemodule1'i**seçin. Görüntünün her iki sürümünün de kayıt defterine itildiğini doğrulayın.

    ![Konteyner kayıt defterinde her iki resim sürümlerini görüntüleme](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>Sorun giderme

Modül görüntünüzinşa ederken ve iterken hatalarla karşılaşırsanız, genellikle geliştirme makinenizdeki Docker yapılandırmasıyla ilgili olmalıdır. Yapılandırmanızı gözden geçirmek için aşağıdaki denetimleri kullanın:

* Komutu, `docker login` konteyner kayıt defterinizden kopyaladığınız kimlik bilgilerini kullanarak çalıştırdınız mı? Bu kimlik bilgileri, Azure'da oturum açtığınızda kullandığınız kimlik bilgilerinden farklıdır.
* Konteyner deponuz doğru mu? Doğru konteyner kayıt defteri adınız ve doğru modül adınız var mı? Kontrol etmek için iotEdgeModule1 klasöründeki **module.json** dosyasını açın. Depo değeri ** \<kayıt defteri\>adı .azurecr.io/iotedgemodule1**gibi görünmelidir.
* Modülünüz için **IotEdgeModule1'den** farklı bir ad kullandıysanız, bu ad çözüm boyunca tutarlı mıdır?
* Makineniz, inşa ettiğiniz konteynerlerin aynısı mı çalışıyor? Bu öğretici Windows IoT Edge aygıtları içindir, bu nedenle Visual Studio dosyalarınızda **windows-amd64** uzantısı olmalıdır ve Docker Desktop Windows kapsayıcıları çalıştırMalıdır.

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Yapılı kapsayıcı görüntülerinin konteyner kayıt defterinizde depolandığını doğruladınız, bu nedenle bunları bir aygıta dağıtma nın zamanı geldiğinden. IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio'da Bulut Gezgini'ni açın ve IoT hub'ınıziçin ayrıntıları genişletin.

2. Dağıtmak istediğiniz aygıtın adını seçin. **Eylemler** listesinde Dağıtım **Oluştur'u**seçin.

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-develop-for-windows/create-deployment.png)

3. Dosya gezgininde, projenizin config klasörüne gidin ve **deployment.windows-amd64.json** dosyasını seçin. Bu dosya genellikle`C:\Users\<username>\source\repos\CSharpTutorialApp\CSharpTutorialApp\config\deployment.windows-amd64.json`

   Tam modül görüntü değerleri bulunmayan deployment.template.json dosyasını kullanmayın.

4. Aygıtınızdaki modülleri görmek için Bulut Gezgini'ndeki IoT Edge aygıtınızın ayrıntılarını genişletin.

5. Simüle Edilen TemperatureSensor ve IotEdgeModule1 modüllerinin cihazınıza dağıtılmış olduğunu görmek için cihaz durumunu güncellemek için **Yenile** düğmesini kullanın.

   ![IoT Edge cihazınızda çalışan modülleri görüntüleme](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Aygıttan gelen iletileri görüntüleme

IotEdgeModule1 kodu iletileri giriş kuyruğundan alır ve çıkış kuyruğundan geçirir. Dağıtım bildirimi, Simüle Edilen TemperatureSensor'dan IotEdgeModule1'e iletiler geçen yolları ve ardından IotEdgeModule1'den IoT Hub'a iletilen yolları beyan etti. Visual Studio için Azure IoT Edge araçları, iletileri ioT Hub'a bireysel aygıtlarınızdan geldiklerinde görmenizi sağlar.

1. Visual Studio bulut gezgininde, dağıtdığınız IoT Edge aygıtının adını seçin.

2. **Eylemler** menüsünde, **Yerleşik Etkinlik Bitiş Noktasını İzlemeyi Başlat'ı**seçin.

3. IoT hub'ınıza gelen iletileri görmek için Visual Studio'daki **Çıkış** bölümünü izleyin.

   Her iki modülün de başlaması birkaç dakika sürebilir. IoT Edge çalışma zamanının yeni dağıtım bildirimini alması, kapsayıcı çalışma zamanındaki modül görüntülerini aşağı çekmesi ve ardından her yeni modülü başlatması gerekir.

   ![Gelen aygıtı bulut iletilerine görüntüleme](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Aygıttaki değişiklikleri görüntüleme

Cihazınızda neler olduğunu görmek istiyorsanız, IoT Edge çalışma süresini ve cihazınızda çalışan modülleri incelemek için bu bölümdeki komutları kullanın.

Bu bölümdeki komutlar geliştirme makineniz için değil, IoT Edge aygıtınız içindir. IoT Edge aygıtınız için sanal bir makine kullanıyorsanız, hemen bağlanın. Azure'da, sanal makinenin genel bakış sayfasına gidin ve uzak masaüstü bağlantısına erişmek için **Bağlan'ı** seçin. Aygıtta, `iotedge` komutları çalıştırmak için bir komut veya PowerShell penceresi açın.

* Cihazınıza dağıtılan tüm modülleri görüntüleyin ve durumlarını kontrol edin:

   ```cmd
   iotedge list
   ```

   Dört modül görmelisiniz: iki IoT Edge çalışma zamanı modülü, Simüle TemperatureSensor ve IotEdgeModule1. Dördü de çalışan olarak listelenmelidir.

* Belirli bir modül için günlükleri inceleyin:

   ```cmd
   iotedge logs <module name>
   ```

   IoT Edge modülleri büyük/küçük harf duyarlıdır.

   Simüle TemperatureSensor ve IotEdgeModule1 günlükleri, işledikleri iletileri göstermelidir. EdgeAgent modülü diğer modülleri başlatmaktan sorumludur, bu nedenle günlükleri dağıtım bildiriminin uygulanması hakkında bilgi sahibi olacaktır. Herhangi bir modül listede yoksa veya çalışmıyorsa, edgeAgent günlüklerinde büyük olasılıkla hatalar olacaktır. edgeHub modülü, modüller ve IoT Hub arasındaki iletişimden sorumludur. Modüller çalışır durumdaysa, ancak iletiler IoT hub'ınıza gelmiyorsa, edgeHub günlüklerinde büyük olasılıkla hatalar olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde Visual Studio 2019'u geliştirme makinenizde kurdunuz ve ilk IoT Edge modülünüzü ondan dağıttınız. Artık temel kavramları bildiğinize göre, içinden geçen verileri analiz edebilsin diye bir modüle işlevsellik eklemeyi deneyin. Tercih ettiğiniz dili seçin:

> [!div class="nextstepaction"]
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)
