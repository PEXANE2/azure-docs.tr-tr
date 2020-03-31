---
title: IoT Hub verileriyle Azure Machine Learning'i kullanarak hava tahmini
description: IoT hub'ınızın bir sensörden topladığı sıcaklık ve nem verilerine göre yağmur olasılığını tahmin etmek için Azure Machine Learning'i kullanın.
author: robinsh
manager: philmea
keywords: hava tahmini makine öğrenme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/10/2020
ms.author: robinsh
ms.openlocfilehash: b71b86c14c55c312ef420a4d8517140fdded4072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122257"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Azure Machine Learning'deki IoT merkezinizdeki sensör verilerini kullanarak hava tahmini

![Uçuça diyagram](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Makine öğrenimi, bilgisayarların gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için varolan verilerden öğrenmelerine yardımcı olan bir veri bilimi tekniğidir. Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve tahmine dayalı analiz hizmetidir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure IoT hub'ınızdaki sıcaklık ve nem verilerini kullanarak hava tahmini (yağmur olasılığı) yapmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenirsiniz. Yağmur şansı hazırlanmış bir hava tahmin modelinin çıktısi. Model sıcaklık ve nem dayalı yağmur şansını tahmin etmek için tarihi veriler üzerine inşa edilmiştir.

## <a name="what-you-do"></a>Ne yaparsınız

- Hava durumu tahmin modelini bir web hizmeti olarak dağıtın.
- Bir tüketici grubu ekleyerek IoT hub'ınızı veri erişimine hazırlayın.
- Bir Akış Analizi işi oluşturun ve işi aşağıdakilere göre yapılandırın:
  - IoT hub'ınızdaki sıcaklık ve nem verilerini okuyun.
  - Yağmur şansı elde etmek için web servisini arayın.
  - Sonucu Azure blob depolama alanına kaydedin.
- Hava tahminini görüntülemek için Microsoft Azure Depolama Gezgini'ni kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- [Raspberry Pi online simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya cihaz öğreticiler birini tamamlayın; örneğin, [ahududu Pi düğüm ile.js](iot-hub-raspberry-pi-kit-node-get-started.md). Bunlar aşağıdaki gereksinimleri kapsar:
  - Etkin bir Azure aboneliği.
  - Aboneliğiniz altında bir Azure IoT hub'ı.
  - Azure IoT hub'ınıza ileti gönderen bir istemci uygulaması.
- [Azure Machine Learning Studio (klasik)](https://studio.azureml.net/) hesabı.

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Hava durumu tahmin modelini bir web hizmeti olarak dağıtma

Bu bölümde, Azure AI Kitaplığı'ndan hava durumu tahmin modelini alırsınız. Daha sonra sıcaklık ve nem verilerini temizlemek için modele bir R-script modülü eklersiniz. Son olarak, modeli tahmine dayalı bir web hizmeti olarak dağıtMış sınız.

### <a name="get-the-weather-prediction-model"></a>Hava tahmini modelini alın

Bu bölümde, Azure AI Galerisi'nden hava tahmini modelini alır ve Azure Machine Learning Studio'da (klasik) açarsınız.

1. [Hava tahmini modeli sayfasına](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1)gidin.

   ![Azure AI Galerisi'nde hava tahmini modeli sayfasını açın](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Modeli Microsoft Azure Machine Learning Studio'da (klasik) açmak için **Studio'da Aç'ı (klasik)** tıklatın.

   ![Azure Machine Learning Studio'da hava tahmini modelini açın (klasik)](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Sıcaklık ve nem verilerini temizlemek için bir R-script modülü ekleme

Modelin doğru şekilde hareket edilebis olması için sıcaklık ve nem verilerinin sayısal verilere dönüştürülebilen olması gerekir. Bu bölümde, sayısal değerlere dönüştürülemeyen sıcaklık veya nem için veri değerlerine sahip satırları kaldıran hava durumu tahmin modeline bir R-script modülü eklersiniz.

1. Azure Machine Learning Studio penceresinin sol tarafında, araçlar panelini genişletmek için oku tıklatın. Arama kutusuna "Yürüt" girin. Execute **R Script** modülünü seçin.

   ![Yürüt R Script modülünü seçin](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. Yürütme **R Script** modüllerini **Temiz Eksik Veri** modülüne ve diyagramdaki mevcut Execute R **Script** modülüne yakın sürükleyin. **Temiz Eksik Veri** ile Execute **R Script** modülleri arasındaki bağlantıyı silin ve gösterildiği gibi yeni modülün giriş ve çıktılarını bağlayın.

   ![Execute R Script modülü ekle](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Özellikler penceresini açmak için yeni **Execute R Script** modülünü seçin. Aşağıdaki kodu **R Script** kutusuna kopyalayıp yapıştırın.

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   Bitirdikten sonra, özellikler penceresi aşağıdakilere benzer olmalıdır:

   ![R Script modüllerini yürütmek için kod ekleme](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Tahmine dayalı web hizmetini dağıtma

Bu bölümde, modeli doğrular, modele dayalı bir tahmine dayalı web hizmeti ayarlar sınız ve ardından web hizmetini dağıtırsınız.

1. Modeldeki adımları doğrulamak için **Çalıştır'ı** tıklatın. Bu adımın tamamlanması birkaç dakika sürebilir.

   ![Adımları doğrulamak için denemeyi çalıştırın](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. WEB HİzMETİ > **PREDICTIVe Web Service'i** **AYARLA'yı**tıklatın. Tahmine dayalı deney diyagramı açılır.

   ![Azure Machine Learning Studio'da hava tahmini modelini dağıtın (klasik)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. Tahmine dayalı deneme diyagramında, **Web hizmeti giriş** modülü ile en üstteki Hava Durumu Veri **Kümesi** arasındaki bağlantıyı silin. Ardından **Web hizmeti giriş modülünü** **Score Model** modülüne yakın bir yere sürükleyin ve gösterildiği gibi bağlayın:

   ![Azure Machine Learning Studio'da iki modül bağlayın (klasik)](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)

1. Modeldeki adımları doğrulamak için **KAÇ'ı** tıklatın.

1. Modeli bir web hizmeti olarak dağıtmak için **WEB HİzMETİ DAĞıT'ı** tıklatın.

1. Modelin panosuna, **İstek/YANıT**için **Excel 2010 veya önceki çalışma kitabını** indirin.

   > [!Note]
   > Bilgisayarınızda Excel'in daha sonraki bir sürümünü çalıştırıyor olsanız bile **Excel 2010** veya önceki çalışma kitabını indirdiğinizden emin olun.

   ![İsteK YANıT bitiş noktası için Excel'i indirin](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Excel çalışma kitabını açın, WEB **SERVICE URL'sini** ve **ACCESS KEY'i**not edin.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Akış Analizi işi oluşturma, yapılandırma ve çalıştırma

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. [Azure portalında](https://portal.azure.com/), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IoT hub'ınızın kullandığı kaynak grubunu kullanın.

   **Konum**: Kaynak grubunuzla aynı konumu kullanın.

   **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

   ![Azure'da Akış Analizi işi oluşturun](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. **Oluştur'u**tıklatın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Akış Analizi işini açın.
1. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.
1. **Girişler** bölmesinde **Ekle'yi**tıklatın ve ardından aşağıdaki bilgileri girin:

   **Giriş takma adı**: Giriş için benzersiz takma ad.

   **Kaynak**: **IoT hub'ı**seçin.

   **Tüketici grubu**: Oluşturduğunuz tüketici grubunu seçin.

   ![Azure'daki Akış Analizi işine giriş ekleme](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. **Oluştur'u**tıklatın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.
1. **Çıktılar** bölmesinde **Ekle'yi**tıklatın ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı.

   **Lavabo**: **Blob Depolama**seçin.

   **Depolama hesabı**: Blob depolamanızın depolama hesabı. Bir depolama hesabı oluşturabilir veya varolan bir hesap kullanabilirsiniz.

   **Konteyner**: Blob'un kaydedildiği konteyner. Bir kapsayıcı oluşturabilir veya varolan bir kapsayıcı kullanabilirsiniz.

   **Olay serileştirme biçimi**: **CSV'yi**seçin.

   ![Azure'daki Akış Analizi işine çıktı ekleme](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. **Oluştur'u**tıklatın.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Dağıttığınız web hizmetini aramak için Stream Analytics işine bir işlev ekleme

1. **İş Topolojisi**altında, **İşlevler** > **Ekle'yi**tıklatın.
1. Aşağıdaki bilgileri girin:

   **İşlev Diğer** `machinelearning`Adı : Girin .

   **İşlev Türü**: **Azure ML'yi**seçin.

   **Alma seçeneği**: **Farklı bir abonelikten içe aktarma'yı**seçin.

   **URL**: Excel çalışma kitabında niçin not ettiğiniz WEB SERVICE URL'sini girin.

   **Anahtar**: Excel çalışma kitabında belirttiğiniz ACCESS TUŞUNu girin.

   ![Azure'daki Akış Analizi işine işlev ekleme](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. **Oluştur'u**tıklatın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.
1. Varolan kodu aşağıdaki kodla değiştirin:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.

   `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.

1. **Kaydet**'e tıklayın.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Akış Analizi işinde, > **Şimdi** > **Başlat'ı**tıklatın. **Start** İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Stream Analytics işini çalıştırma](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Hava tahminini görüntülemek için Microsoft Azure Depolama Gezgini'ni kullanın

Sıcaklık ve nem verilerini IoT hub'ınıza toplamaya ve göndermeye başlamak için istemci uygulamasını çalıştırın. IoT hub'ınızın aldığı her ileti için, Stream Analytics işi yağmur olasılığı üretmek için hava tahmini web hizmetini çağırır. Sonuç daha sonra Azure blob depolama bilgisayarınıza kaydedilir. Azure Depolama Gezgini, sonucu görüntülemek için kullanabileceğiniz bir araçtır.

1. [Microsoft Azure Depolama Gezgini'ni indirin ve yükleyin.](https://storageexplorer.com/)
1. Azure Depolama Gezgini'ni açın.
1. Azure hesabınızda oturum açın.
1. Aboneliğinizi seçin.
1. Depolama Hesabınız>, konteynerinize > **Blob Konteynerleri** > aboneliğinizi > **Depolama Hesapları'nı** tıklatın.
1. Sonucu görmek için bir .csv dosyası indirin. Son sütunda yağmur ihtimali yazıyor.

   ![Azure Machine Learning ile hava tahmini sonucunu alın](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Özet

IoT hub'ınızın aldığı sıcaklık ve nem verilerine göre yağmur olasılığını üretmek için Azure Machine Learning'i başarıyla kullandınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]