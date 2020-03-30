---
title: Quickstart - Azure portalını kullanarak Akış Analizi işi oluşturun
description: Bu hızlı başlangıçta bir Stream Analytic işi oluşturma, girdileri ve çıktıları yapılandırma ve bir sorgu tanımlama yoluyla çalışmaya nasıl başlayacağınız gösterilmektedir.
author: mamccrea
ms.author: mamccrea
ms.date: 06/21/2019
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
ms.openlocfilehash: 4abf5078a005f9d928397d9666e7f2bc55d65f19
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75431545"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma

Bu hızlı başlangıçta bir Stream Analytics işini oluşturmaya nasıl başlanacağı gösterilmektedir. Bu hızlı başlangıçta, gerçek zamanlı akış verilerini okuyan ve 27'den büyük bir sıcaklıkla iletileri filtreleyen bir Akış Analizi işi tanımlarsınız. Akış Analizi işin, IoT Hub'daki verileri okuyacak, verileri dönüştürecek ve verileri blob depolama daki bir kapsayıcıya geri yazacaktır. Bu hızlı başlatmada kullanılan giriş verileri Raspberry Pi çevrimiçi simülatörü tarafından oluşturulur. 

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Akış Analizi işini tanımlamadan önce giriş verilerini hazırlamanız gerekir. Gerçek zamanlı sensör verileri IoT Hub'a yutularak daha sonra iş girişi olarak yapılandırılır. İşin gerektirdiği giriş verilerini hazırlamak için aşağıdaki adımları tamamlayın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. IoT >  **Hub'ın kaynak** > **Internet'ini**seçin.**IoT Hub**

3. **IoT Hub** bölmesine aşağıdaki bilgileri girin:
   
   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | IoT Hub'ınızı barındırabileceğiniz bir coğrafi konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IoT Hub Adı  | MyASAIoTHub  |   IoT Hub'ınız için bir ad seçin.   |

   ![IoT Hub'ı oluşturma](./media/stream-analytics-quick-create-portal/create-iot-hub.png)

4. **Sonraki'ni seçin: Boyut ve ölçek ayarlayın.**

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu hızlı başlangıç için, aboneliğinizde hala mevcutsa **F1 - Free** katmanını seçin. Daha fazla bilgi için [IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/)için bkz.

   ![IoT Hub'ınızı boyutlandırın ve ölçeklendirin](./media/stream-analytics-quick-create-portal/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IoT Hub bilgilerinizi gözden geçirin ve **Oluştur'u**tıklatın. IoT Hub'ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

7. IoT Hub gezinme menüsünde, **IoT aygıtları**altında **ekle'yi** tıklatın. Aygıt **Kimliği** ekleyin ve **Kaydet'i**tıklatın.

   ![IoT Hub'ınıza aygıt ekleme](./media/stream-analytics-quick-create-portal/add-device-iot-hub.png)

8. Aygıt oluşturulduktan sonra aygıtı **IoT aygıtlar** listesinden açın. Birincil **anahtar olan Bağlantı dizesini** kopyalayın ve daha sonra kullanmak üzere not defterine kaydedin.

   ![Copy IoT Hub aygıt bağlantı dizesi](./media/stream-analytics-quick-create-portal/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>Blob depolama oluşturma

1. Azure portalının sol üst köşesinden > kaynak**Depolama Deposu** > **hesabı** **oluştur'u**seçin.

2. Depolama **hesabı oluştur** bölmesine bir depolama hesabı adı, konum ve kaynak grubu girin. Oluşturduğunuz IoT Hub ile aynı konum ve kaynak grubunu seçin. Ardından hesabı oluşturmak için **Gözden Geçir + oluştur'u** tıklatın.

   ![Depolama hesabı oluştur](./media/stream-analytics-quick-create-portal/create-storage-account.png)

3. Depolama hesabınız oluşturulduktan **sonra, Genel Bakış** bölmesindeki **Blobs** döşemesini seçin.

   ![Depolama hesabına genel bakış](./media/stream-analytics-quick-create-portal/blob-storage.png)

4. **Blob Servis** sayfasından **Kapsayıcı'yı** seçin ve *kapsayıcınız*1 gibi bir ad sağlayın. Genel **erişim düzeyini** Özel olarak bırakın **(anonim erişim yok)** ve **Tamam'ı**seçin.

   ![Blob kapsayıcısı oluşturma](./media/stream-analytics-quick-create-portal/create-blob-container.png)

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure Portal’da oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.  

3. Sonuçlar listesinden **Analytics** > **Stream Analytics işini** seçin.  

4. Stream Analytics işi sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |İş adı   |  MyASAJob   |   Stream Analytics işinizi tanımlamak için bir ad girin. Stream Analytics işinin adı yalnızca alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır. |
   |Abonelik  | \<Aboneliğiniz\> |  Bu iş için kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   IoT Hub'ınızla aynı kaynak grubunu seçin. |
   |Konum  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | Stream Analytics işinizi barındırabileceğiniz coğrafi konumu seçin. Daha iyi performans elde etmek ve veri aktarımı maliyetini azaltmak için kullanıcılarınıza en yakın konumu seçin. |
   |Akış birimleri  | 1  |   Akış birimleri, bir işin yürütülmesi için gereken bilgi işlem kaynaklarını temsil eder. Varsayılan olarak, bu değer 1 olarak ayarlanır. Akış birimlerini ölçeklendirme hakkında bilgi edinmek için [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md) başlıklı makaleye bakın.   |
   |Barındırma ortamı  |  Bulut  |   Stream Analytics işleri buluta veya uca dağıtılabilir. Bulut, Azure Bulut'a, Edge ise bir IoT Edge aygıtına dağıtmanıza olanak tanır. |

   ![İş oluştur](./media/stream-analytics-quick-create-portal/create-asa-job.png)

5. **Panoya sabitle** kutusunu işaretleyerek işinizi panonuza yerleştirin ve sonra **Oluştur**’u seçin.  

6. Tarayıcı pencerenizin sağ üst kısmında görüntülenen bir *Dağıtım...* bildiriminin devam ettiğini görmeniz gerekir. 

## <a name="configure-job-input"></a>İş girişi yapılandırma

Bu bölümde, Stream Analytics işine bir IoT Hub aygıt girişi yapılandıracaksınız. Quickstart'ın önceki bölümünde oluşturduğunuz IoT Hub'ını kullanın.

1. Stream Analytics işinize gidin.  

2. **Girişleri** > Seç Akış girişi > **IoT****Hub'ı ekleyin.**  

3. **IoT Hub** sayfasını aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı  |  IoTHubInput   |  İşin girdisini tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |IoT Hub  |  MyASAIoTHub |  Önceki bölümde oluşturduğunuz IoT Hub'ının adını girin. |

4. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Girdi verilerini yapılandırma](./media/stream-analytics-quick-create-portal/configure-asa-input.png)
 
## <a name="configure-job-output"></a>İş çıkışını yapılandırma

1. Daha önce oluşturduğunuz Stream Analytics işine gidin.  

2. **Çıkışları** > Seçin**Blob depolama****ekleyin.** >   

3. **Blob depolama** sayfasını aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Çıktı diğer adı |   BlobOutput   |   İşin çıktısını tanımlamak için bir ad girin. |
   |Abonelik  |  \<Aboneliğiniz\>  |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |Depolama hesabı |  asaquickstartstorage |   Depolama hesabının adını seçin veya girin. Depolama hesabı adları aynı abonelikte oluşturulursa otomatik olarak algılanır.       |
   |Kapsayıcı |   kapsayıcı1  |  Depolama hesabınızda oluşturduğunuz mevcut kapsayıcıyı seçin.   |

4. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Çıktı yapılandırma](./media/stream-analytics-quick-create-portal/configure-asa-output.png)
 
## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

1. Daha önce oluşturduğunuz Stream Analytics işine gidin.  

2. **Sorgu**’yu seçin ve sorguyu aşağıdaki gibi güncelleştirin:  

   ```sql
   SELECT *
   INTO BlobOutput
   FROM IoTHubInput
   HAVING Temperature > 27
   ```

3. Bu örnekte, sorgu IoT Hub'daki verileri okur ve blob'daki yeni bir dosyaya kopyalar. **Kaydet'i**seçin.  

   ![İş dönüşümü yapılandırma](./media/stream-analytics-quick-create-portal/add-asa-query.png)

## <a name="run-the-iot-simulator"></a>IoT simülatörünü çalıştırın

1. [Raspberry Pi Azure IoT Online Simülatörü'nü](https://azure-samples.github.io/raspberry-pi-web-simulator/)açın.

2. Satır 15'teki yer tutucuyu önceki bölümde kaydettiğiniz Azure IoT Hub aygıt bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IoT Hub'ınıza gönderilen sensör verilerini ve iletilerini göstermelidir.

   ![Raspberry Pi Azure IoT Çevrimiçi Simülatörü](./media/stream-analytics-quick-create-portal/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. İşe genel bakış sayfasına dönün ve **Başlat**’ı seçin.

2. **İş çıktısı** **başlangıç zamanı** alanı için Başlat işi altında, **Şimdi'yi**seçin. Ardından, işe başlamak için **Başlat'ı** seçin.

3. Birkaç dakika sonra portalda işin çıktısı olarak yapılandırdığınız depolama hesabını ve kapsayıcıyı bulun. Çıktı dosyasını artık kapsayıcıda görebilirsiniz. İşin ilk kez başlatılması birkaç dakika sürer ve başlatıldıktan sonra veriler ulaştıkça çalışmaya devam eder.  

   ![Dönüştürülmüş çıktı](./media/stream-analytics-quick-create-portal/check-asa-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, Akış Analizi işini ve ilgili tüm kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure portalı kullanarak basit bir Stream Analytics işi dağıttınız. Ayrıca [PowerShell,](stream-analytics-quick-create-powershell.md) [Visual Studio](stream-analytics-quick-create-vs.md)ve Visual Studio [Code](quick-create-vs-code.md)kullanarak Stream Analytics işleri dağıtabilirsiniz.

Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
