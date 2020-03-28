---
title: "Öğretici: Simüle edilmiş aygıt verileri oluşturma - Azure IoT Edge'de Makine Öğrenimi"
description: Daha sonra bir makine öğrenme modeli eğitmek için kullanılabilecek simüle telemetri oluşturmak sanal cihazlar oluşturun.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 1/20/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 8f7a971315183e867ae06b58801d5855f90462a1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76722401"
---
# <a name="tutorial-generate-simulated-device-data"></a>Öğretici: Simüle cihaz verileri oluşturma

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, Azure IoT Hub'ına telemetri gönderen bir aygıtı simüle etmek için makine öğrenimi eğitim verilerini kullanıyoruz. Girişte belirtildiği gibi, bu öğretici eğitim ve test için uçak motorları bir dizi veri simüle etmek için [Turbofan motor bozulması simülasyon veri seti](https://c3.nasa.gov/dashlink/resources/139/) kullanır.

Deneysel senaryomuzda şunu biliyoruz:

* Veriler birden çok değişkenli zaman serilerinden oluşur.
* Her veri kümesi eğitim ve test alt kümelerine ayrılır.
* Her zaman serisi farklı bir motordan.
* Her motor ilk aşınma ve üretim varyasyonfarklı derecelerde başlar.

Bu öğretici için, tek bir veri kümesinin (FD003) eğitim veri alt kümesini kullanırız.

Gerçekte, her motor bağımsız bir IoT cihaz olacaktır. Internet'e bağlı turbofan motorları koleksiyonunuz olmadığını varsayarsak, bu cihazlar için bir yazılım stand-in'i oluşturacağız.

Simülatör, sanal aygıtları IoT Hub'a programlı olarak kaydetmek için IoT Hub API'lerini kullanan bir C# programıdır. Daha sonra NASA tarafından sağlanan veri alt kümesinden her cihaz için verileri okumak ve simüle IoT Cihazı kullanarak IoT hub'ınıza gönderin. Öğreticinin bu bölümünün tüm kodlarını deponun DeviceHarness dizininde bulabilirsiniz.

DeviceHarness projesi, C# ile yazılmış dört sınıftan oluşan bir .NET çekirdek projesidir:

* **Program:** Kullanıcı girişi ve genel koordinasyonu işlemekten sorumlu yürütme giriş noktası.
* **TrainingFileManager:** Seçili veri dosyasını okuma ve ayrıştma sorumlusu.
* **CycleData:** İleti biçimine dönüştürülen bir dosyadaki tek bir veri satırını temsil eder.
* **TurbofanCihazı:** Verilerde tek bir aygıta (zaman serisi) karşılık gelen bir IoT Aygıtı oluşturmaktan ve verileri IoT Hub'ına aktarmaktan sorumludur.

Bu makalede açıklanan görevlerin tamamlanması yaklaşık 20 dakika sürer.

Bu adımdaki işin gerçek dünya eşdeğeri büyük olasılıkla aygıt geliştiricileri ve bulut geliştiricileri tarafından gerçekleştirilebilir.

## <a name="configure-visual-studio-code-and-build-deviceharness-project"></a>Visual Studio Code'u yapılandırın ve DeviceHarness projesini oluşturun

1. Geliştirme VM'inize uzak bir masaüstü oturumu açın.

1. Visual Studio kodunda `C:\source\IoTEdgeAndMlSample\DeviceHarness` klasörü açın.

1. Bu makinedeki uzantıları ilk kez kullandığınızdan, bazı uzantılar bağımlılıklarını günceller ve yükler. Uzantıyı güncelleştirmeniz istenebilir. Eğer öyleyse, **Pencereyi Yeniden Yükle'yi**seçin.

   Çıkış penceresinde OmniSharp hataları görünüyorsa, C# uzantısını kaldırmanız gerekir.

1. DeviceHarness için gerekli varlıkları eklemeniz istenir. Eklemek için **Evet'i** seçin.

   * Bildirimin görünmesi birkaç saniye sürebilir.
   * Bu bildirimi kaçırdıysanız, sağ alt köşedeki zil simgesine bakın.

   ![VS Kod uzantısı açılır pencere](media/tutorial-machine-learning-edge-03-generate-data/add-required-assets.png)

1. Paket bağımlılıklarını geri yüklemek için **Geri Yükle'yi** seçin.

   ![VS Kodu geri yükleme istemi](media/tutorial-machine-learning-edge-03-generate-data/restore-package-dependencies.png)

   Bu bildirimleri alamazsanız, Visual Studio Code'u kapatın, Visual Studio `C:\source\IoTEdgeAndMlSample\DeviceHarness`Code'daki bin ve obj dizinlerini silin, Visual Studio Code'u açın ve DeviceHarness klasörünü yeniden açın.

1. Bir yapıyı, **Ctrl** + **Shift** + **B'yi**veya **Terminal** > **Çalıştır Yapı Görevini**tetikleyerek ortamınızın düzgün şekilde ayarladığını doğrulayın.

1. Çalışacak yapı görevini seçmeniz istenir. **Yapı'yı**seçin.

1. Yapı çalışır ve bir başarı iletisi çıktırıyor.

   ![Başarılı çıktı iletisi oluşturma](media/tutorial-machine-learning-edge-03-generate-data/build-success.png)

1. **Terminal** > **Yapılandırma Varsayılan Yapı Görevi'ni** seçerek ve komut isteminden **Yapı'yı** seçerek bu yapıyı varsayılan yapı görevi yapabilirsiniz.

## <a name="connect-to-iot-hub-and-run-deviceharness"></a>IoT Hub'a bağlanın ve DeviceHarness'i çalıştırın

Artık proje oluşturmaya sahip olduğumuza göre, bağlantı dizesine erişmek ve veri oluşturmanın ilerlemesini izlemek için IoT hub'ınıza bağlanın.

### <a name="sign-in-to-azure-in-visual-studio-code"></a>Visual Studio Code'da Azure'da oturum açma

1. Komut paletini açarak Visual Studio Kodu'nda `Ctrl + Shift + P` Azure aboneliğinizde oturum açın veya**Komut Paletini** **Görüntüle.** > 

1. **Azure'u arayın: Oturum Aç** komutu.

   Bir tarayıcı penceresi açılır ve kimlik bilgileriniz için sizi ister. Bir başarı sayfasına yönlendirildiyseniz, tarayıcıyı kapatabilirsiniz.

### <a name="connect-to-your-iot-hub-and-retrieve-hub-connection-string"></a>IoT hub'ınıza bağlanın ve hub bağlantı dizesini alın

1. Visual Studio Code explorer'ın alt bölümünde, genişletmek için **Azure IoT Hub** çerçevesini seçin.

1. Genişletilmiş çerçevede, **IoT Hub'ı seçin'i**tıklatın.

1. İstendiğinde, Azure aboneliğinizi ve ardından IoT hub'ınızı seçin.

1. Daha fazla eylem için **Azure IoT Hub'ının** sağındaki **...** seçeneğini tıklatın. **Copy IoT Hub bağlantı dizesi'ni**seçin.

   ![Copy IoT Hub bağlantı dizesi](media/tutorial-machine-learning-edge-03-generate-data/copy-hub-connection-string.png)

### <a name="run-the-deviceharness-project"></a>DeviceHarness projesini çalıştırın

1. Visual Studio Code terminalini açmak için **Görünüm** > **Terminali'ni** seçin.

   Bir istem görmüyorsanız Enter tuşuna basın.

1. Terminale `dotnet run` girin.

1. IoT Hub Bağlantı Dizesi için istendiğinde, önceki bölümde kopyalanan bağlantı dizesini yapıştırın.

1. Azure **IoT Hub aygıtları** çerçevesiiçinde, yenile düğmesini tıklatın.

   ![IoT Hub cihaz listesini yenile](media/tutorial-machine-learning-edge-03-generate-data/refresh-hub-device-list.png)

1. Aygıtların IoT Hub'ına eklendiğini ve verilerin bu aygıt üzerinden gönderildiğini belirtmek için aygıtların yeşil açıkta olduğunu unutmayın. Aygıtlar IoT hub'ına ileti gönderdikten sonra bağlantıyı keser ve mavi görünür.

1. Hub'a gönderilen iletileri herhangi bir cihaza sağ tıklayarak ve **Yerleşik Etkinlik Bitiş Noktası'nı Başlat'ı**seçerek görüntüleyebilirsiniz. İletiler Visual Studio Code'daki çıktı bölmesinde gösterecektir.

1. **Azure IoT Hub** çıkış bölmesini tıklatarak izlemeyi durdurun ve **Yerleşik Etkinlik Bitiş Noktasını Izlemeyi Durdur'u**seçin.

1. Uygulamanın tamamlanmasına izin verin, bu da birkaç dakika sürer.

## <a name="check-iot-hub-for-activity"></a>Etkinlik için IoT Hub'ı kontrol edin

DeviceHarness tarafından gönderilen veriler, Azure portalında doğrulayabildiğiniz IoT hub'ınıza gitti.

1. Azure [portalını](https://portal.azure.com/) açın ve bu öğretici için oluşturulan IoT hub'ına gidin.

1. Sol bölme menüsünden, **İzleme**altında, **Ölçümler'i**seçin.

1. Grafik tanım sayfasında, **Metrik** açılır lay'i tıklatın, listeyi aşağı kaydırın ve **Yönlendirme'yi seçin: depolama alanına teslim edilen veriler.** Grafik, verilerin depolamaya yönlendirildiğindeki artışını göstermelidir.

   ![Grafik, depolama alanına teslim edilen veriler de artış gösterir](media/tutorial-machine-learning-edge-03-generate-data/iot-hub-usage.png)

## <a name="validate-data-in-azure-storage"></a>Azure Depolama'da verileri doğrulama

Az önce IoT hub'ınıza gönderdiğimiz veriler, önceki makalede oluşturduğumuz depolama kapsayıcısına yönlendirildi. Depolama hesabımızdaki verilere bakalım.

1. Azure portalında depolama hesabınıza gidin.

1. Depolama hesabı gezgininden **Storage Explorer'ı (önizleme)** seçin.

1. Depolama gezgininde **Blob Kapsayıcıları'nı** seçin. `devicedata`

1. İçerik bölmesinde, IoT hub'ının adını n için klasöre tıklayın ve ardından yıl, ay, gün ve saat' i takip edin. Verilerin yazıldığı dakikaları temsil eden birkaç klasör görürsünüz.

   ![Blob depolamasında klasörleri görüntüleme](media/tutorial-machine-learning-edge-03-generate-data/confirm-data-storage-results.png)

1. Bölüme karşılık gelen **00** ve **01** etiketli veri dosyalarını bulmak için bu klasörlerden birini tıklatın.

1. Dosyalar [Avro](https://avro.apache.org/) formatında yazılmıştır. Başka bir tarayıcı sekmesini açmak ve verileri kısmen işlemek için bu dosyalardan birine çift tıklayın. Bir programda dosyayı açmanız istenirse, VS Kodu'nu seçebilirsiniz ve dosya doğru şekilde işlenir.

1. Şu anda verileri okumaya veya yorumlamaya gerek yoktur; bir sonraki makalede yapacağız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir dizi sanal IoT aygıtı oluşturmak ve verileri ioT merkezimize ve bir Azure Depolama kapsayıcısına göndermek için bir .NET Core projesi kullandık. Bu proje, fiziksel IoT aygıtlarının bir IoT Hub'ına ve daha sonra da bir depoya veri gönderdiği gerçek bir senaryoyu simüle eder. Bu veriler sensör okumalarını, çalışma ayarlarını, arıza sinyallerini ve modlarını içerir. Yeterli veri toplandıktan sonra, cihaz için kalan yararlı ömrü (RUL) tahmin eden modelleri eğitmek için kullanırız. Bu makine öğrenimini bir sonraki makalede göstereceğiz.

Verilerle bir makine öğrenme modeli eğitmek için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Bir Azure Machine Learning modelini eğitme ve dağıtma](tutorial-machine-learning-edge-04-train-model.md)
