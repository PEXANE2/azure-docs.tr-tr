---
title: Hızlı başlangıç-Azure portal kullanarak Stream Analytics işi oluşturma
description: Bu hızlı başlangıçta bir Stream Analytic işi oluşturma, girdileri ve çıktıları yapılandırma ve bir sorgu tanımlama yoluyla çalışmaya nasıl başlayacağınız gösterilmektedir.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 06/21/2019
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
ms.openlocfilehash: 65114f2ddb7567b47ac3951fbaf6664654e379f0
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74707364"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma

Bu hızlı başlangıçta bir Stream Analytics işini oluşturmaya nasıl başlanacağı gösterilmektedir. Bu hızlı başlangıçta, gerçek zamanlı akış verilerini okuyan bir Stream Analytics işi tanımlar ve bir sıcaklık 27 ' den büyük olan iletileri filtreler. Stream Analytics işiniz IoT Hub verileri okur, verileri dönüştürür ve BLOB depolama alanındaki verileri bir kapsayıcıya geri yazar. Bu hızlı başlangıçta kullanılan giriş verileri bir Raspberry PI Çevrimiçi simülatör tarafından oluşturulmuştur. 

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/)’ında oturum açın.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce, giriş verilerini hazırlamanız gerekir. Gerçek zamanlı algılayıcı verileri, daha sonra iş girişi olarak yapılandırılan IoT Hub alınır. İş için gereken giriş verilerini hazırlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.

3. **IoT Hub** bölmesinde aşağıdaki bilgileri girin:
   
   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | IoT Hub barındırabileceğiniz coğrafi bir konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IoT Hub adı  | MyASAIoTHub  |   IoT Hub için bir ad seçin.   |

   ![IoT Hub'ı oluşturma](./media/stream-analytics-quick-create-portal/create-iot-hub.png)

4. **İleri ' yi seçin: boyut ve ölçek ayarla**.

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu hızlı başlangıçta, aboneliğinizde hala kullanılabiliyorsa **F1-ücretsiz** katmanını seçin. Daha fazla bilgi için bkz. [IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![IoT Hub boyutlandırın ve ölçeklendirin](./media/stream-analytics-quick-create-portal/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IoT Hub bilgilerinizi gözden geçirin ve **Oluştur**' a tıklayın. IoT Hub oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

7. IoT Hub gezinti menüsünde **IoT cihazları**altında **Ekle** ' ye tıklayın. Bir **CIHAZ kimliği** ekleyin ve **Kaydet**' e tıklayın.

   ![IoT Hub bir cihaz ekleyin](./media/stream-analytics-quick-create-portal/add-device-iot-hub.png)

8. Cihaz oluşturulduktan sonra **IoT cihazları** listesinden cihazı açın. **Bağlantı dizesini kopyalayın--birincil anahtar** ve daha sonra kullanmak için bir not defteri 'ne kaydedin.

   ![IoT Hub cihaz bağlantı dizesini Kopyala](./media/stream-analytics-quick-create-portal/save-iot-device-connection-string.png)

## <a name="create-blob-storage"></a>BLOB depolama oluştur

1. Azure portalının sol üst köşesinden **Kaynak oluştur** > **Depolama** > **Depolama hesabı**’nı seçin.

2. **Depolama hesabı oluştur** bölmesinde, bir depolama hesabı adı, konum ve kaynak grubu girin. Oluşturduğunuz IoT Hub aynı konum ve kaynak grubunu seçin. Ardından hesabı oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

   ![Depolama hesabı oluştur](./media/stream-analytics-quick-create-portal/create-storage-account.png)

3. Depolama Hesabınız oluşturulduktan sonra **genel bakış** bölmesinde **Bloblar** kutucuğunu seçin.

   ![Depolama hesabına genel bakış](./media/stream-analytics-quick-create-portal/blob-storage.png)

4. **BLOB hizmeti** sayfasından **kapsayıcı** ' yı seçin ve Kapsayıcınız için *kapsayıcı1*gibi bir ad sağlayın. **Ortak erişim düzeyini** **özel (anonim erişim yok)** olarak bırakın ve **Tamam**' ı seçin.

   ![Blob kapsayıcısı oluşturma](./media/stream-analytics-quick-create-portal/create-blob-container.png)

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure Portal’da oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.  

3. Sonuçlar listesinden **analiz** > **Stream Analytics işi** ' ni seçin.  

4. Stream Analytics işi sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |İş adı   |  MyASAJob   |   Stream Analytics işinizi tanımlamak için bir ad girin. Stream Analytics işinin adı yalnızca alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır. |
   |Abonelik  | \<Aboneliğiniz\> |  Bu iş için kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak grubu   |   asaquickstart-resourcegroup  |   IoT Hub aynı kaynak grubunu seçin. |
   |Konum  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | Stream Analytics işinizi barındırabileceğiniz coğrafi konumu seçin. Daha iyi performans elde etmek ve veri aktarımı maliyetini azaltmak için kullanıcılarınıza en yakın konumu seçin. |
   |Akış birimleri  | 1  |   Akış birimleri, bir işin yürütülmesi için gereken bilgi işlem kaynaklarını temsil eder. Varsayılan olarak, bu değer 1 olarak ayarlanır. Akış birimlerini ölçeklendirme hakkında bilgi edinmek için [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md) başlıklı makaleye bakın.   |
   |Barındırma ortamı  |  Bulut  |   Stream Analytics işleri buluta veya uca dağıtılabilir. Bulut, Azure bulutuna dağıtmanıza olanak tanır ve Edge bir IoT Edge cihazına dağıtmanıza olanak tanır. |

   ![İş oluştur](./media/stream-analytics-quick-create-portal/create-asa-job.png)

5. **Panoya sabitle** kutusunu işaretleyerek işinizi panonuza yerleştirin ve sonra **Oluştur**’u seçin.  

6. Tarayıcı pencerenizin sağ üst köşesinde bir *dağıtım devam ediyor...* bildirimi görmeniz gerekir. 

## <a name="configure-job-input"></a>İş girişi yapılandırma

Bu bölümde, Stream Analytics işine IoT Hub bir cihaz girişi yapılandıracaksınız. Hızlı başlangıç bölümünün önceki bölümünde oluşturduğunuz IoT Hub kullanın.

1. Stream Analytics işinize gidin.  

2.  > **akış girişi ekle** > **IoT Hub** **girişler** ' i seçin.  

3. **IoT Hub** sayfasını aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı  |  Iothubınput   |  İşin girdisini tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |IoT Hub  |  MyASAIoTHub |  Önceki bölümde oluşturduğunuz IoT Hub adını girin. |

4. Diğer seçenekleri varsayılan değerlerinde bırakın ve ayarları kaydetmek **Kaydet**’i seçin.  

   ![Girdi verilerini yapılandırma](./media/stream-analytics-quick-create-portal/configure-asa-input.png)
 
## <a name="configure-job-output"></a>İş çıkışını yapılandırma

1. Daha önce oluşturduğunuz Stream Analytics işine gidin.  

2.  > **BLOB depolaması** **eklemek** > **çıktılar** ' i seçin.  

3. **Blob depolama** sayfasını aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Çıkış diğer adı |   BlobOutput   |   İşin çıktısını tanımlamak için bir ad girin. |
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

3. Bu örnekte sorgu IoT Hub verileri okur ve Blobun yeni bir dosyaya kopyalar. **Kaydet**’i seçin.  

   ![İş dönüşümü yapılandırma](./media/stream-analytics-quick-create-portal/add-asa-query.png)

## <a name="run-the-iot-simulator"></a>IoT simülatörü çalıştırma

1. [Raspberry PI Azure IoT çevrimiçi simülatörünü](https://azure-samples.github.io/raspberry-pi-web-simulator/)açın.

2. 15. satırdaki yer tutucusunu, önceki bölümde kaydettiğiniz Azure IoT Hub cihaz bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktıda, IoT Hub gönderilen algılayıcı verileri ve iletileri gösterilmelidir.

   ![Raspberry PI Azure IoT Çevrimiçi simülatör](./media/stream-analytics-quick-create-portal/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. İşe genel bakış sayfasına dönün ve **Başlat**’ı seçin.

2. İşi **Başlat**' ın altında, **iş çıkışı başlangıç zamanı** alanı için **Şimdi**' yi seçin. Ardından, işinizi başlatmak için **Başlat** ' ı seçin.

3. Birkaç dakika sonra portalda işin çıktısı olarak yapılandırdığınız depolama hesabını ve kapsayıcıyı bulun. Çıktı dosyasını artık kapsayıcıda görebilirsiniz. İşin ilk kez başlatılması birkaç dakika sürer ve başlatıldıktan sonra veriler ulaştıkça çalışmaya devam eder.  

   ![Dönüştürülmüş çıktı](./media/stream-analytics-quick-create-portal/check-asa-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, Stream Analytics işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure portalı kullanarak basit bir Stream Analytics işi dağıttınız. Ayrıca [PowerShell](stream-analytics-quick-create-powershell.md), [Visual Studio](stream-analytics-quick-create-vs.md)ve [Visual Studio Code](quick-create-vs-code.md)kullanarak Stream Analytics işleri dağıtabilirsiniz.

Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
