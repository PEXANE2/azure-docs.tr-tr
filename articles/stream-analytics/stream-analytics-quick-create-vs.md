---
title: Quickstart - Visual Studio'u kullanarak Azure Akışı Analizi işi oluşturun
description: Bu hızlı başlangıçta Visual Studio ile bir Stream Analytics işi oluşturma, girdileri ve çıktıları yapılandırma ve bir sorgu tanımlama yoluyla çalışmaya nasıl başlayacağınız gösterilmektedir.
author: mamccrea
ms.author: mamccrea
ms.date: 06/11/2019
ms.topic: quickstart
ms.service: stream-analytics
ms.openlocfilehash: 5e654fe5d31a225a855da8477e073ceeb1a68634
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767242"
---
# <a name="quickstart-create-an-azure-stream-analytics-job-by-using-visual-studio"></a>Hızlı başlangıç: Visual Studio'u kullanarak Azure Akışı Analizi işi oluşturun

Bu hızlı başlangıçta Visual Studio için Azure Stream Analytics araçlarını kullanarak bir Stream Analytics işi oluşturma ve çalıştırma adımları gösterilmektedir. Örnek iş, bir IoT Hub aygıtından veri akışı okur. 27° üzerindeyken ortalama sıcaklığı hesaplayan ve ortaya çıkan çıktı olaylarını blob depolamasındaki yeni bir dosyaya yazan bir iş tanımlarsınız.

> [!NOTE]
> Visual Studio ve Visual Studio Code araçları Çin Doğu, Çin Kuzey, Almanya Orta ve Almanya KuzeyDoğu bölgelerindeki işleri desteklemez.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

* Visual Studio 2019, Visual Studio 2015 veya Visual Studio 2013 Güncelleme 4'ü yükleyin. Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez.

* [Yükleme talimatlarını](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install) izleyerek Visual Studio için Stream Analytics araçlarını yükleyin.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Akış Analizi işini tanımlamadan önce, daha sonra iş girişi olarak yapılandırılan verileri hazırlamanız gerekir. İşin gerektirdiği giriş verilerini hazırlamak için aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. IoT >  **Hub'ın kaynak** > **Internet'ini**seçin.**IoT Hub**

3. **IoT Hub** bölmesine aşağıdaki bilgileri girin:
   
   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | IoT Hub'ınızı barındırabileceğiniz bir coğrafi konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IoT Hub Adı  | MyASAIoTHub  |   IoT Hub'ınız için bir ad seçin.   |

   ![IoT Hub'ı oluşturma](./media/stream-analytics-quick-create-vs/create-iot-hub.png)

4. **Sonraki'ni seçin: Boyut ve ölçek ayarlayın.**

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu hızlı başlangıç için, aboneliğinizde hala mevcutsa **F1 - Free** katmanını seçin. Boş katman kullanılamıyorsa, kullanılabilir en düşük katmanı seçin. Daha fazla bilgi için [IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/)için bkz.

   ![IoT Hub'ınızı boyutlandırın ve ölçeklendirin](./media/stream-analytics-quick-create-vs/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IoT Hub bilgilerinizi gözden geçirin ve **Oluştur'u**tıklatın. IoT Hub'ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

7. IoT Hub gezinme menüsünde, **IoT aygıtları**altında **ekle'yi** tıklatın. Aygıt **Kimliği** ekleyin ve **Kaydet'i**tıklatın.

   ![IoT Hub'ınıza aygıt ekleme](./media/stream-analytics-quick-create-vs/add-device-iot-hub.png)

8. Aygıt oluşturulduktan sonra aygıtı **IoT aygıtlar** listesinden açın. Birincil **anahtar olan Bağlantı dizesini** kopyalayın ve daha sonra kullanmak üzere not defterine kaydedin.

   ![Copy IoT Hub aygıt bağlantı dizesi](./media/stream-analytics-quick-create-vs/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>Blob depolama oluşturma

1. Azure portalının sol üst köşesinden > kaynak**Depolama Deposu** > **hesabı** **oluştur'u**seçin.

2. Depolama **hesabı oluştur** bölmesine bir depolama hesabı adı, konum ve kaynak grubu girin. Oluşturduğunuz IoT Hub ile aynı konum ve kaynak grubunu seçin. Ardından hesabı oluşturmak için **Gözden Geçir + oluştur'u** tıklatın.

   ![Depolama hesabı oluştur](./media/stream-analytics-quick-create-portal/create-storage-account.png)

3. Depolama hesabınız oluşturulduktan **sonra, Genel Bakış** bölmesindeki **Blobs** döşemesini seçin.

   ![Depolama hesabına genel bakış](./media/stream-analytics-quick-create-portal/blob-storage.png)

4. **Blob Servis** sayfasından **Kapsayıcı'yı** seçin ve *kapsayıcınız*1 gibi bir ad sağlayın. Genel **erişim düzeyini** Özel olarak bırakın **(anonim erişim yok)** ve **Tamam'ı**seçin.

   ![Blob kapsayıcısı oluşturma](./media/stream-analytics-quick-create-portal/create-blob-container.png)

## <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma

1. Visual Studio’yu çalıştırın.

2. **Dosya > Yeni Proje**'yi seçin.  

3. Sol taraftaki şablon listesinden **Stream Analytics**'i ve ardından **Azure Stream Analytics Uygulaması**'nı seçin.  

4. Projenin **Ad**, **Konum** ve **Çözüm adı** değerlerini girip **Tamam**'ı seçin.

   ![Stream Analytics projesi oluşturma](./media/stream-analytics-quick-create-vs/create-stream-analytics-project.png)

Azure Stream Analytics projesine dahil edilen öğelere dikkat edin.

   <img src="./media/stream-analytics-quick-create-vs/stream-analytics-project.png" alt="Azure Stream Analytics project elements" width="300px"/>


## <a name="choose-the-required-subscription"></a>Gerekli aboneliği seçin

1. Visual Studio'nun **Görünüm** menüsünde **Sunucu Gezgini**'ni seçin.

2. **Azure**'a sağ tıklayıp **Microsoft Azure Aboneliğine Bağlan**'ı seçin ve Azure hesabınızla oturum açın.

## <a name="define-input"></a>Girişi tanımlama

1. **Çözüm Gezgini**'nde **Girişler** düğümünü genişletin ve **Input.json** dosyasına çift tıklayın.

2. **Stream Analytics Giriş Yapılandırması**'na aşağıdaki değerleri ekleyin:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**   |
   |---------|---------|---------|
   |Girdi Diğer Adı  |  Giriş   |  İşin girdisini tanımlamak için bir ad girin.   |
   |Kaynak Türü   |  Veri Akışı |  Uygun giriş kaynağını seçin: Veri Akışı veya Başvuru Verileri.   |
   |Kaynak  |  IoT Hub |  Uygun giriş kaynağını seçin.   |
   |Kaynak  | Geçerli hesaptaki veri kaynağını seçin | Verileri el ile girin veya var olan bir hesabı seçin.   |
   |Abonelik  |  \<Aboneliğiniz\>   | Oluşturduğunuz IoT Hub'ına sahip Azure aboneliğini seçin.   |
   |IoT Hub  |  MyASAIoTHub   |  IoT Hub'ınızın adını seçin veya girin. IoT Hub adları, aynı abonelikte oluşturulduklarında otomatik olarak algılanır.   |
   
3. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Girdi verilerini yapılandırma](./media/stream-analytics-quick-create-vs/stream-analytics-vs-input.png)

## <a name="define-output"></a>Çıkışı tanımlama

1. **Çözüm Gezgini**'nde **Çıkışlar** düğümünü genişletin ve **Output.json** dosyasına çift tıklayın.

2. **Stream Analytics Çıkış Yapılandırması**'na aşağıdaki değerleri ekleyin:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**   |
   |---------|---------|---------|
   |Çıktı Diğer Adı  |  Çıktı   |  İşin çıktısını tanımlamak için bir ad girin.   |
   |Havuz   |  Blob Depolama |  Uygun havuzu seçin.    |
   |Kaynak  |  Veri kaynağı ayarlarını el ile sağlayın |  Verileri el ile girin veya var olan bir hesabı seçin.   |
   |Abonelik  |  \<Aboneliğiniz\>   | Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır.   |
   |Depolama Hesabı  |  asaquickstartstorage   |  Depolama hesabının adını seçin veya girin. Depolama hesabı adları aynı abonelikte oluşturulursa otomatik olarak algılanır.   |
   |Kapsayıcı  |  kapsayıcı1   |  Depolama hesabınızda oluşturduğunuz mevcut kapsayıcıyı seçin.   |
   |Yol Deseni  |  çıkış   |  Kapsayıcıda oluşturulacak dosya yolunun adını girin.   |
   
3. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Çıkış verilerini yapılandırma](./media/stream-analytics-quick-create-vs/stream-analytics-vs-output.png)

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

1. Visual Studio'da **Çözüm Gezgini**'nden **Script.asaql** dosyasını açın.

2. Aşağıdaki sorguyu ekleyin:

   ```sql
   SELECT *
   INTO BlobOutput
   FROM IoTHubInput
   HAVING Temperature > 27
   ```

## <a name="submit-a-stream-analytics-query-to-azure"></a>Azure'a bir Stream Analytics sorgusu gönderme

1. **Sorgu Düzenleyicisi**'nin betik düzenleyici bölümünde **Azure'a Gönder**'i seçin.

2. **Yeni bir Azure Stream Analytics işi oluştur**'u seçip bir **İş Adı** girin. Hızlı başlangıcın başında kullandığınız **Abonelik**, **Kaynak Grubu** ve **Konum** bilgilerini seçin.

   ![İşi Azure'a gönderme](./media/stream-analytics-quick-create-vs/stream-analytics-job-to-azure.png)

## <a name="run-the-iot-simulator"></a>IoT simülatörünü çalıştırın

1. [Raspberry Pi Azure IoT Online Simulator'u](https://azure-samples.github.io/raspberry-pi-web-simulator/) yeni bir tarayıcı sekmesinde veya penceresinde açın.

2. Satır 15'teki yer tutucuyu önceki bölümde kaydettiğiniz Azure IoT Hub aygıt bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IoT Hub'ınıza gönderilen sensör verilerini ve iletilerini göstermelidir.

   ![Raspberry Pi Azure IoT Çevrimiçi Simülatörü](./media/stream-analytics-quick-create-portal/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. İşiniz oluşturulduğunda otomatik olarak iş görünümü açılır. İşi başlatmak için yeşil ok düğmesini seçin.

   ![Stream Analytics işini başlatma](./media/stream-analytics-quick-create-vs/start-stream-analytics-job-vs.png)

2. İş **çıktısı başlangıç modunu** **JobStartTime** olarak değiştirin ve **Başlat'ı**seçin.

   ![İş yapılandırmasını başlatma](./media/stream-analytics-quick-create-vs/stream-analytics-start-configuration.png)

3. İş durumunun **Çalışıyor** olarak değiştiğine ve giriş/çıkış olayları olduğuna dikkat edin. Bu birkaç dakika sürebilir.

   ![Stream Analytics işini çalıştırma](./media/stream-analytics-quick-create-vs/stream-analytics-job-running.png)

4. Sonuçları görüntülemek için **Görünüm** menüsünde **Bulut Gezgini**'ni seçin ve kaynak grubunuzdaki depolama hesabına gidin. **Blob Kapsayıcıları** bölümünde **container1**'e ve ardından **output** dosya yoluna çift tıklayın.

   ![Sonuçları görüntüleme](./media/stream-analytics-quick-create-vs/stream-analytics-vs-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Visual Studio kullanarak basit bir Stream Analytics işi dağıttınız. Stream Analytics işlerini [Azure portalı](stream-analytics-quick-create-portal.md) ve [PowerShell](stream-analytics-quick-create-powershell.md)’i kullanarak da dağıtabilirsiniz. 

Visual Studio için Azure Akış Analizi araçları hakkında bilgi edinmek için aşağıdaki makaleye devam edin:

> [!div class="nextstepaction"]
> [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
