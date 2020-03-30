---
title: Uzaktan İzleme çözümünü yerel olarak dağıtın - IntelliJ IDE - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılılır kılavuzu, test ve geliştirme için IntelliJ'i kullanarak Uzaktan İzleme çözüm hızlandırıcısını yerel makinenize nasıl dağıtabileceğinizi gösterir.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: 779ee1e057d74b11c5e0ba58dc2fd32b803f1e0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888822"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtın - IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, uzaktan izleme çözüm hızlandırıcısını test ve geliştirme için yerel makinenize nasıl dağıtabileceğinizgösterilmektedir. IntelliJ'de mikro servisleri nasıl çalıştıracağını öğreneceksin. Yerel bir mikro hizmetler dağıtımı aşağıdaki bulut hizmetlerini kullanır: IoT Hub, Azure Cosmos DB, Azure Akış Analizi ve Azure Zaman Serisi Öngörüleri.

Docker'daki Uzaktan İzleme çözüm hızlandırıcısını yerel makinenizde çalıştırmak istiyorsanız, [uzaktan izleme çözüm hızlandırıcısını yerel olarak dağıtın - Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md)' a bakın.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan İzleme çözüm hızlandırıcısı tarafından kullanılan Azure hizmetlerini dağıtmak için etkin bir Azure aboneliğine ihtiyacınız var.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine kurulumu

Yerel dağıtımı tamamlamak için, yerel geliştirme makinenize aşağıdaki araçların yüklenmesi gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Topluluk Sürümü](https://www.jetbrains.com/idea/download/)
* [IntelliJ Scala eklentisi](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ SBT eklentisi](https://plugins.jetbrains.com/plugin/5007-sbt)
* [IntelliJ SBT Uygulayıcı eklentisi](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Düğüm.js v8](https://nodejs.org/)

Node.js v8, komut dosyalarının Azure kaynakları oluşturmak için kullandığı PCS CLI için bir ön koşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> IntelliJ IDE, Windows ve Mac için kullanılabilir.

## <a name="download-the-source-code"></a>Kaynak kodunu indirin

Uzaktan İzleme kaynak kodu depoları, docker görüntülerini çalıştırmak için gereken kaynak kodu ve Docker yapılandırma dosyalarını içerir.

Deponun yerel bir sürümünü klonlamak ve oluşturmak için, yerel makinenizde uygun bir klasöre gitmek için komut satırı ortamınızı kullanın. Ardından Java deposunu klonlamak için aşağıdaki komut kümelerinden birini çalıştırın:

* Java microservice uygulamalarının en son sürümünü indirmek için aşağıdaki komutu çalıştırın:

  ```cmd/sh
  git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git
  ```

* En son alt modülleri almak için aşağıdaki komutları çalıştırın:

   ```cmd/sh
   cd azure-iot-pcs-remote-monitoring-java
   git submodule foreach git pull origin master
    ```

> [!NOTE]
> Bu komutlar, mikro hizmetleri yerel olarak çalıştırmak için kullandığınız komutlara ek olarak tüm mikro hizmetlerin kaynak kodunu karşıdan yükler. Docker'daki mikro hizmetleri çalıştırmak için kaynak koduna ihtiyacınız yok. Ancak kaynak kodu, daha sonra çözüm hızlandırıcısını değiştirmeyi ve değişikliklerinizi yerel olarak test etmeyi planlıyorsanız yararlıdır.

## <a name="deploy-the-azure-services"></a>Azure hizmetlerini dağıtma

Bu makalede, mikro hizmetlerin yerel olarak nasıl çalıştırılacakları gösterilse de, bunlar bulutta çalışan Azure hizmetlerine bağlıdır. Azure hizmetlerini dağıtmak için aşağıdaki komut dosyasını kullanın. Komut dosyası örnekleri, Windows makinesinde Java deposunu kullandığınızı varsayar. Başka bir ortamda çalışıyorsanız, yolları, dosya uzantılarını ve yol ayırıcılarını uygun şekilde ayarlayın.

### <a name="create-new-azure-resources"></a>Yeni Azure kaynakları oluşturun

Gerekli Azure kaynaklarını henüz oluşturmadıysanız aşağıdaki adımları izleyin:

1. Komut satırı ortamınızda, deponun klonlanmış kopyasındaki **\services\scripts\local\launch** klasörüne gidin.

1. **Pcs** CLI aracını yüklemek ve Azure hesabınızda oturum açabilmek için aşağıdaki komutları çalıştırın:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. **start.cmd** komut dosyasını çalıştırın. Komut dosyası sizi aşağıdaki bilgiler için ister:

   * Çözüm adı.
   * Kullanılacak Azure aboneliği.
   * Kullanılacak Azure veri merkezinin konumu.

   Komut dosyası, Azure'da çözüm adınızı içeren bir kaynak grubu oluşturur. Bu kaynak grubu, çözüm hızlandırıcının kullandığı Azure kaynaklarını içerir. Karşılık gelen kaynaklara artık ihtiyacınız kalmadıktan sonra bu kaynak grubunu silebilirsiniz.

   Komut dosyası, yerel makinenize bir ortam değişkenleri kümesi de ekler. Her değişken adı **pcs**öneki vardır. Bu ortam değişkenleri, Uzaktan İzleme'nin yapılandırma değerlerini bir Azure Anahtar Kasası kaynağından okumasına izin veren ayrıntılar sağlar.

   > [!TIP]
   > Komut dosyası bittiğinde, ortam değişkenlerini ** \<ev klasörünüz\>\\.pcs\\\<çözüm adı\>.env**olarak adlandırılan bir dosyaya kaydeder. Bunları gelecekteki çözüm hızlandırıcı dağıtımları için kullanabilirsiniz. Yerel makinenizde ayarlanan herhangi bir ortam değişkeninin, **docker-compose'i**çalıştırdığınızda **yerel\\\\\\.env** dosyasındaki hizmetler komut dosyalarındaki değerleri geçersiz kıldığında dikkat edin.

1. Komut satırı ortamınızı kapatın.

### <a name="use-existing-azure-resources"></a>Varolan Azure kaynaklarını kullanma

Gerekli Azure kaynaklarını zaten oluşturduysanız, ilgili ortam değişkenlerini yerel makinenizde ayarlayın:
* **PCS_KEYVAULT_NAME**: Key Vault kaynağının adı.
* **PCS_AAD_APPID**: Azure Etkin Dizin (Azure AD) uygulama kimliği.
* **PCS_AAD_APPSECRET**: Azure AD uygulaması gizlidir.

Yapılandırma değerleri bu Key Vault kaynağından okunacaktır. Bu ortam değişkenleri, dağıtımdan ** \<ev\>\\klasörünüze kaydedilebilir .pcs\\\<çözüm adı\>.env** dosyası. Yerel makinenizde ayarlanan ortam **değişkenlerinin, docker-compose'i**çalıştırdığınızda **yerel\\\\\\.env** dosyasındaki hizmetler komut dosyalarındaki değerleri geçersiz kıldığında dikkat edin.

Mikro hizmetin gerektirdiği yapılandırmanın bir kısmı, ilk dağıtımda oluşturulan Key Vault örneğinde depolanır. Anahtar kasasındaki karşılık gelen değişkenler gerektiği gibi değiştirilmelidir.

## <a name="run-the-microservices"></a>Mikro hizmetleri çalıştırın

Bu bölümde, Uzaktan İzleme mikro hizmetlerini çalıştırın. Çalıştırın:

* Web UI yerel olarak.
* Docker'daki Azure IoT Aygıt Simülasyonu, Auth ve Azure Akış Analizi Yöneticisi hizmetleri.
* IntelliJ'deki mikro hizmetler.

### <a name="run-the-device-simulation-service"></a>Cihaz Simülasyon hizmetini çalıştırın

Yeni bir Komut İstemi penceresi açın. Önceki bölümde **start.cmd** komut dosyası tarafından ayarlanan ortam değişkenlerine erişebildiğinizden denetleyin.

Cihaz Simülasyonu hizmeti için Docker konteynerini açmak için aşağıdaki komutu çalıştırın. Hizmet, Uzaktan İzleme çözümü için aygıtları simüle eder.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Auth hizmetini çalıştırın

Yeni bir Komut İstem penceresi açın ve ardından Auth hizmeti için Docker kapsayıcısını açmak için aşağıdaki komutu çalıştırın. Bu hizmeti kullanarak, Azure IoT çözümlerine erişmeye yetkili kullanıcıları yönetebilirsiniz.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-stream-analytics-manager-service"></a>Akış Analiz Yöneticisi hizmetini çalıştırın

Yeni bir Komut İstem penceresi açın ve ardından Stream Analytics Manager hizmeti için Docker kapsayıcısını açmak için aşağıdaki komutu çalıştırın. Bu hizmetle Stream Analytics işlerini yönetebilirsiniz. Bu yönetim, iş yapılandırması ayarlamayı ve iş durumunu başlatmayı, durdurmayı ve izlemeyi içerir.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-your-local-machine"></a>Diğer tüm mikro hizmetleri yerel makinenize dağıtın

Aşağıdaki adımlar, IntelliJ'de Uzaktan İzleme mikro hizmetlerini nasıl çalıştırabileceğinizi gösterir.

#### <a name="import-a-project"></a>Proje alma

1. IntelliJ IDE'yi açın.
1. **Alma Projesi'ni**seçin.
1. **Azure-iot-pcs-remote-monitoring-java\services\build.sbt'yi**seçin.

#### <a name="create-run-configurations"></a>Çalıştır yapılandırmaları oluşturma

1. **Yapılandırmaları** > **Çalıştır'ı**seçin.
1. **Yeni Yapılandırma** > sbt görev ekle'yi seçin.**sbt task**
1. **Adı**girin ve ardından **Görevler'i çalıştır**olarak girin. **Tasks**
1. Çalıştırmak istediğiniz hizmete göre **Çalışma Dizini'ni** seçin.
1. Seçimlerinizi kaydetmek için**Tamam'ı** **Uygula'yı** > seçin.
1. Aşağıdaki web hizmetleri için çalıştırma yapılandırmaları oluşturun:
    * WebService (hizmetler\config)
    * WebService (hizmetler\cihaz-telemetri)
    * WebService (hizmetler\iothub-yöneticisi)
    * WebService (hizmetler\depolama-bağdaştırıcı)

Örnek olarak, aşağıdaki resim, bir hizmet için yapılandırmanın nasıl ekleyeceğini gösterir:

[![IntelliJ IDE Çalıştır/Hata Ayıklama Yapılandırmaları penceresinin ekran görüntüsü, sol bölmedeki sbt görevler listesinde vurgulanan depolamaAdapter seçeneğini ve sağ bölmedeki Ad, Görevler, Çalışma dizini ve VM parametreleri kutularındaki girişleri gösterir.](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)

#### <a name="create-a-compound-configuration"></a>Bileşik yapılandırma oluşturma

1. Tüm hizmetleri birlikte çalıştırmak için Yeni Yapılandırma > **Bileşimi** **Ekle'yi**seçin.
1. **Ad**girin ve sonra **sbt görevleri ekle'yi**seçin.
1. Seçimlerinizi kaydetmek için**Tamam'ı** **Uygula'yı** > seçin.

Örnek olarak, aşağıdaki resim, tüm sbt görevlerinin tek bir yapılandırmaya nasıl ekleyeceğini gösterir:

[![IntelliJ IDE Çalıştır/Hata Ayıklama Yapılandırmaları penceresinin ekran görüntüsü, sol bölmedeki Bileşik listesinde vurgulanan AllServices seçeneğini ve sağ bölmede vurgulanan sbt Task 'deviceTelemetry' seçeneğini gösterir.](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Web hizmetlerini yerel makinede oluşturmak ve çalıştırmak için **Çalıştır'ı** seçin.

Her web hizmeti bir Komut İstemi penceresi ve web tarayıcısı penceresi açar. Komut isteminde, çalışan hizmetten çıktı görürsünüz. Tarayıcı penceresi durumu izlemenizi sağlar. Bu eylemler web hizmetini durdurdukları için Komut İstem pencerelerini veya web sayfalarını kapatmayın.

Hizmetlerin durumuna erişmek için aşağıdaki URL'lere gidin:

* IoT-Hub Yöneticisi:[http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Cihaz Telemetrisi:[http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* Config:[http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* depolama adaptörü:[http://localhost:9022/v1/status](http://localhost:9022/v1/status)

### <a name="start-the-stream-analytics-job"></a>Akış Analizi işini başlatın

Akış Analizi işini başlatmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com)gidin.
1. Çözümünüz için oluşturulan **Kaynak grubuna** gidin. Kaynak grubunun **adı, start.cmd** komut dosyasını çalıştırdığınızda çözümünüz için seçtiğiniz addır.
1. Kaynaklar listesinde **Akış Analizi işini** seçin.
1. Akış Analizi iş **genel bakış** sayfasında **Başlat** düğmesini seçin ve ardından işe başlamak için **Başlat'ı** seçin.

### <a name="run-the-web-ui"></a>Web UI'yi çalıştırın

Bu adımda, web ui başlatın. Yeni bir Komut İstemi penceresi açın. **Start.cmd** komut dosyası tarafından ayarlanan ortam değişkenlerine erişebildiğinizden denetleyin. Deponun yerel kopyasındaki **webui** klasörüne gidin ve ardından aşağıdaki komutları çalıştırın:

```cmd
npm install
npm start
```

**Başlangıç** komutu tamamlandığında, tarayıcınız sayfayı adreste [http://localhost:3000/dashboard](http://localhost:3000/dashboard)görüntüler. Bu sayfadaki hatalar bekleniyor. Uygulamayı hatasız görüntülemek için aşağıdaki adımları tamamlayın.

### <a name="configure-and-run-nginx"></a>Nginx'i yapılandırma ve çalıştırma

Web uygulamasını yerel makinenizde çalışan mikro hizmetlere bağlayan ters proxy sunucusu ayarlayın:

1. Nginx\conf yükleme dizinine deponun yerel kopyasında **webui\scripts\localhost** klasöründen **nginx.conf** dosyasını kopyalayın. **nginx\conf**
1. Nginx'i yönet.

Nginx çalıştırma hakkında daha fazla bilgi için [Windows için nginx'e](https://nginx.org/en/docs/windows.html)bakın.

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan İzleme çözüm panosuna erişmek http://localhost:9000 için tarayıcınıza gidin.

## <a name="clean-up"></a>Temizleme

Gereksiz ücretlerden kaçınmak için, testinizi tamamladıktan sonra bulut hizmetlerini Azure aboneliğinizden kaldırın. Hizmetleri kaldırmak için Azure [portalına](https://ms.portal.azure.com)gidin ve **start.cmd** komut dosyasının oluşturduğu kaynak grubunu silin.

Ayrıca, Kaynak Kodu GitHub'dan klonladığınızda oluşturulan Uzaktan İzleme deposunun yerel kopyasını da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan İzleme çözümünü dağıttığınız için bir sonraki adım [çözüm panosunun yeteneklerini keşfetmektir.](quickstart-remote-monitoring-deploy.md)
