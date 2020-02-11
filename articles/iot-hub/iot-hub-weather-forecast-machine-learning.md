---
title: IoT Hub verilerle Azure Machine Learning kullanarak hava durumu tahmini
description: IoT Hub 'ınızın bir sensörden topladığı sıcaklık ve nem verilerine bağlı olarak yağmur olma olasılığını tahmin etmek için Azure Machine Learning kullanın.
author: robinsh
manager: philmea
keywords: Hava durumu tahmin makinesi öğrenimi
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/10/2020
ms.author: robinsh
ms.openlocfilehash: b71b86c14c55c312ef420a4d8517140fdded4072
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77122257"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Azure Machine Learning 'daki IoT Hub 'ından gelen algılayıcı verilerini kullanarak hava durumu tahmini

![Uçtan uca diyagram](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Makine öğrenimi, bilgisayarların, gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için mevcut verilerden öğrenmesini sağlayan bir veri bilimi tekniğidir. Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve tahmine dayalı analiz hizmetidir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure IoT Hub 'ınızdaki sıcaklık ve nem verilerini kullanarak hava durumu tahmini (yağmur şansı) için Azure Machine Learning kullanmayı öğreneceksiniz. Yağmur, hazırlanan Hava durumu tahmin modelinin çıktıdır. Model, sıcaklık ve nem oranına göre yağmur olasılığını tahmin etmek için geçmiş veriler üzerine kurulmuştur.

## <a name="what-you-do"></a>Yapabilecekleriniz

- Hava durumu tahmin modelini bir Web hizmeti olarak dağıtın.
- Bir tüketici grubu ekleyerek IoT Hub 'ınızı veri erişimi için hazırlayın.
- Stream Analytics işi oluşturun ve işi şu şekilde yapılandırın:
  - IoT Hub 'ınızdaki sıcaklık ve nem verilerini okuyun.
  - Web hizmetini arayarak yağmur şansı elde edin.
  - Sonucu bir Azure Blob depolama alanına kaydedin.
- Hava durumu tahminini görüntülemek için Microsoft Azure Depolama Gezgini kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- [Raspberry PI Çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğreticisini veya cihaz öğreticilerinin birini doldurun; Örneğin, [Node. js Ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu, aşağıdaki gereksinimleri kapsar:
  - Etkin bir Azure aboneliği.
  - Aboneliğiniz kapsamındaki bir Azure IoT Hub 'ı.
  - Azure IoT Hub 'ınıza ileti gönderen bir istemci uygulaması.
- [Azure Machine Learning Studio (klasik)](https://studio.azureml.net/) hesap.

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Hava durumu tahmin modelini Web hizmeti olarak dağıtma

Bu bölümde, Azure AI kitaplığından Hava durumu tahmin modelini alırsınız. Ardından, sıcaklığın ve nem verilerinin temizlenmesi için modele bir R-Script modülü eklersiniz. Son olarak, modeli bir tahmine dayalı Web hizmeti olarak dağıtırsınız.

### <a name="get-the-weather-prediction-model"></a>Hava durumu tahmin modelini al

Bu bölümde, hava durumu tahmin modelini Azure Yapay Zeka Galerisi alın ve Azure Machine Learning Studio (klasik) içinde açın.

1. [Hava durumu tahmin modeli sayfasına](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1)gidin.

   ![Azure Yapay Zeka Galerisi içinde hava durumu tahmin modeli sayfasını açın](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Modeli Microsoft Azure Machine Learning Studio (klasik) içinde açmak için **Studio 'Da aç (klasik)** seçeneğine tıklayın.

   ![Hava durumu tahmin modelini Azure Machine Learning Studio (klasik) açın](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Sıcaklık ve nem verilerini temizlemek için bir R-betik modülü ekleme

Modelin doğru şekilde davranması için sıcaklık ve nem verileri sayısal verilere dönüştürülebilir olmalıdır. Bu bölümde, sıcaklık veya nem için veri değerlerine sahip satırları kaldıran, sayısal değerlere dönüştürülemeyen bir R-Script modülü eklersiniz.

1. Azure Machine Learning Studio penceresinin sol tarafında, Araçlar panelini genişletmek için oka tıklayın. Arama kutusuna "Execute" yazın. **R betiği Yürüt** modülünü seçin.

   ![R betik modülünü Yürüt seçeneğini belirleyin](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. **Execute r betiği** modülünü **Temizleme eksik veri** modülünün yanına sürükleyin ve diyagramda var olan **r betik modülünü yürütün** . **Eksik verileri temizleme** ve **R betiği çalıştırma** modüllerini arasındaki bağlantıyı silin ve ardından yeni modülün giriş ve çıkışlarını gösterildiği gibi bağlayın.

   ![R betiği yürütme modülü Ekle](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Özellikler penceresini açmak için yeni **R betiği Yürüt** modülünü seçin. Aşağıdaki kodu kopyalayıp **R betik** kutusuna yapıştırın.

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   İşiniz bittiğinde, Özellikler penceresi şuna benzer görünmelidir:

   ![R betik modülünü yürütmek için kod ekleme](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Tahmine dayalı Web hizmeti dağıtma

Bu bölümde, modeli doğrular, modele göre tahmine dayalı bir Web hizmeti ayarlarsınız ve sonra Web hizmetini dağıtırsınız.

1. Modeldeki adımları doğrulamak için **Çalıştır** ' a tıklayın. Bu adımın tamamlanması birkaç dakika sürebilir.

   ![Adımları doğrulamak için denemeyi çalıştırın](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. **Web hizmeti** > tahmine **dayalı Web hizmeti**ayarla ' ya tıklayın. Tahmine dayalı deneme diyagramı açılır.

   ![Hava durumu tahmin modelini Azure Machine Learning Studio dağıtma (klasik)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. Tahmine dayalı deneme diyagramında, **Web hizmeti giriş** modülü ile üst kısımdaki **Hava durumu veri kümesi** arasındaki bağlantıyı silin. Ardından, **Web hizmeti giriş** modülünü **puan modeli** modülünün yakınında bir yere sürükleyin ve gösterildiği gibi bağlayın:

   ![Azure Machine Learning Studio iki modülü bağlama (klasik)](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)

1. Modeldeki adımları doğrulamak için **Çalıştır** ' a tıklayın.

1. Modeli bir Web hizmeti olarak dağıtmak için **Web Hizmeti Dağıt** ' a tıklayın.

1. Modelin panosunda, **istek/yanıt**için **Excel 2010 veya önceki bir çalışma kitabını** indirin.

   > [!Note]
   > Bilgisayarınızda Excel 'in daha yeni bir sürümünü çalıştırıyor olsanız bile **excel 2010 veya önceki bir çalışma kitabını** indirdiğinizden emin olun.

   ![Istek yanıtı uç noktası için Excel 'i indirin](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Excel çalışma kitabını açın, **Web hizmeti URL 'si** ve **erişim anahtarı**' nı bir yere getirin.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Stream Analytics işi oluşturma, yapılandırma ve çalıştırma

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. [Azure portalında](https://portal.azure.com/), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IoT Hub 'ınızın kullandığı kaynak grubunu kullanın.

   **Konum**: kaynak grubunuzda aynı konumu kullanın.

   **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

   ![Azure 'da Stream Analytics işi oluşturma](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Stream Analytics işini açın.
1. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.
1. **Girişler** bölmesinde, **Ekle**' ye tıklayın ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: girişin benzersiz diğer adı.

   **Kaynak**: **IoT Hub 'ı**seçin.

   **Tüketici grubu**: oluşturduğunuz tüketici grubunu seçin.

   ![Azure 'da Stream Analytics işine bir giriş ekleme](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.
1. **Çıktılar** bölmesinde, **Ekle**' ye tıklayın ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı.

   **Havuz**: **BLOB depolamayı**seçin.

   **Depolama hesabı**: BLOB depolama alanınızı depolama hesabı. Bir depolama hesabı oluşturabilir veya var olan bir hesap kullanabilirsiniz.

   **Kapsayıcı**: Blobun kaydedildiği kapsayıcı. Bir kapsayıcı oluşturabilir veya var olan bir kapsayıcı kullanabilirsiniz.

   **Olay serileştirme biçimi**: **CSV**'yi seçin.

   ![Azure 'da Stream Analytics işine bir çıktı ekleyin](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Dağıttığınız Web hizmetini çağırmak için Stream Analytics işine bir işlev ekleyin

1. **Iş topolojisi**altında **işlevler** > **Ekle**' ye tıklayın.
1. Aşağıdaki bilgileri girin:

   **Işlev diğer adı**: `machinelearning`girin.

   **Işlev türü**: **Azure ML**'yi seçin.

   **Içeri aktarma seçeneği**: **farklı bir abonelikten içeri aktar**' ı seçin.

   **URL**: Excel çalışma kitabından Not ETTIĞINIZ Web hizmeti URL 'sini girin.

   **Anahtar**: Excel çalışma kitabından Not ETTIĞINIZ erişim anahtarını girin.

   ![Azure 'da Stream Analytics işine bir işlev ekleme](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.
1. Mevcut kodu şu kodla değiştirin:

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

1. **Save (Kaydet)** düğmesine tıklayın.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Stream Analytics işini çalıştırma](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Hava durumu tahminini görüntülemek için Microsoft Azure Depolama Gezgini kullanın

IoT Hub 'ınıza sıcaklık ve nem verileri toplamaya ve göndermeye başlamak için istemci uygulamasını çalıştırın. IoT Hub 'ınızın aldığı her ileti için Stream Analytics işi, yağmur olma olasılığını üretmek için hava durumu tahmin Web hizmetini çağırır. Sonuç daha sonra Azure Blob depolama alanına kaydedilir. Azure Depolama Gezgini, sonucu görüntülemek için kullanabileceğiniz bir araçtır.

1. [Microsoft Azure Depolama Gezgini indirin ve yükleyin](https://storageexplorer.com/).
1. Azure Depolama Gezgini açın.
1. Azure hesabınızda oturum açın.
1. Aboneliğinizi seçin.
1. Depolama hesabınız > **BLOB kapsayıcıları** > > depolama hesabı > depolama **hesaplarınız** ' e tıklayın.
1. Sonucu görmek için bir. csv dosyası indirin. Son sütun, yağmur olma olasılığını kaydeder.

   ![Azure Machine Learning Hava durumu tahmin sonucu alın](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Özet

IoT Hub 'ınızın aldığı sıcaklık ve nem verilerine bağlı olarak yağmur olma olasılığını oluşturmak için Azure Machine Learning başarıyla kullandınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]