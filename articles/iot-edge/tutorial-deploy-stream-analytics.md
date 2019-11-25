---
title: 'Tutorial: Stream Analytics at the edge - Azure IoT Edge'
description: In this tutorial, you deploy Azure Stream Analytics as a module to an IoT Edge device
author: kgremban
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7fbbe32efcedd4fa2635db1cc21f7ce98557515b
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74452538"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>Tutorial: Deploy Azure Stream Analytics as an IoT Edge module

Many IoT solutions use analytics services to gain insight about data as it arrives in the cloud from IoT devices. Azure IoT Edge ile [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) mantığını alıp cihazın kendisine aktarabilirsiniz. Telemetri akışlarını uç cihazlarda işleyerek yüklenen veri miktarını ve eyleme dönüştürülebilir içgörülere tepki verme süresini azaltabilirsiniz.

Azure IoT Edge ve Azure Stream Analytics tümleşik olduğundan Azure portalında bir Azure Stream Analytics işi oluşturup ek kod yazmadan IoT Edge modülü olarak dağıtabilirsiniz.  

Azure Stream Analytics provides a richly structured query syntax for data analysis, both in the cloud and on IoT Edge devices. For more information, see [Azure Stream Analytics documentation](../stream-analytics/stream-analytics-edge.md).

Bu öğreticideki Stream Analytics modülü, tekrar eden 30 saniyelik dönemler içindeki ortalama sıcaklığı hesaplar. Bu ortalama değer 70'e ulaştığında modül, eyleme geçmek için cihazla ilgili bir uyarı gönderir. Bu durumda eylem, sıcaklık sensörü simülasyonunu sıfırlamaktır. Bir üretim ortamında bu işlevi kullanarak sıcaklık tehlikeli düzeylere ulaştığında bir makineyi kapatabilir veya önleyici işlemler gerçekleştirebilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri uç cihazlarda işlemek için bir Azure Stream Analytics işi oluşturma.
> * Yeni Azure Stream Analytics işini diğer IoT Edge modüllerine bağlama.
> * Azure Stream Analytics işini Azure portalından bir IoT Edge cihazına dağıtma.

<center>

![Diagram - Tutorial architecture, stage and deploy ASA job](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* You can use an Azure virtual machine as an IoT Edge device by following the steps in the quickstart for [Linux](quickstart-linux.md) or [Windows devices](quickstart.md).

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 


## <a name="create-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi oluşturma

In this section, you create an Azure Stream Analytics job that will do the following steps:
* Receive data from your IoT Edge device.
* Query the telemetry data for values outside a set range.
* Take action on the IoT Edge device based on the query results. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

IoT Edge cihazında çalıştırmak üzere oluşturduğunuz Azure Stream Analytics işinin cihazdan çağrılabilecek bir şekilde depolanması gerekir. Mevcut bir Azure depolama hesabını kullanabilir veya yeni bir hesap oluşturabilirsiniz. 

1. In the Azure portal, go to **Create a resource** > **Storage** > **Storage account**. 

1. Depolama hesabınızı oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Adı | Depolama hesabınıza benzersiz bir ad verin. | 
   | Konum | Size yakın bir konum seçin. |


1. Keep the default values for the other fields and select **Review + Create**.

1. Review your settings then select **Create**.

### <a name="create-a-new-job"></a>Yeni bir iş oluşturma

1. In the Azure portal, go to **Create a resource** > **Internet of Things** > **Stream Analytics job**.

1. İşinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | İş adı | İşinize bir ad verin. Örneğin, **IoTEdgeJob** | 
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. | 
   | Barındırma ortamı | **Kenar**'ı seçin. |
 
1. **Oluştur**'u seçin.

### <a name="configure-your-job"></a>İşinizi yapılandırma

Azure portalda Stream Analytics işiniz oluşturulduktan sonra iletilen verilerle çalışması için giriş, çıkış ve sorgu ile yapılandırabilirsiniz. 

Bu bölümde giriş, çıkış ve sorgu öğelerini kullanarak IoT Edge cihazından sıcaklık verilerini alan bir iş oluşturabilirsiniz. Bu iş 30 saniyelik aralıklarla verileri analiz eder. Herhangi bir aralıktaki ortalama sıcaklık değeri 70 derecenin üzerine çıkarsa IoT Edge cihazına bir uyarı gönderilir. Bir sonraki bölümde işi dağıtırken verilerin geldiği ve gittiği yerleri belirteceksiniz.  

1. Azure portalda Stream Analytics işinize gidin. 

1. **İş Topolojisi** bölümünde **Girişler**'i ve **Akış girişi ekle**'yi seçin.

   ![Azure Stream Analytics - add input](./media/tutorial-deploy-stream-analytics/asa-input.png)

1. Açılan listeden **Edge Hub**'ını seçin.

1. **Yeni giriş** bölmesinde giriş diğer adı olarak **temperature** yazın. 

1. Diğer alanlar için varsayılan değerleri kullanın ve **Kaydet**'i seçin.

1. **İş Topolojisi**'nin altında **Çıkışlar**'ı açın ve **Ekle**'yi seçin.

   ![Azure Stream Analytics - add output](./media/tutorial-deploy-stream-analytics/asa-output.png)

1. Açılan listeden **Edge Hub**'ını seçin.

1. **Yeni çıkış** bölmesinde çıkış diğer adı olarak **alert** yazın. 

1. Diğer alanlar için varsayılan değerleri kullanın ve **Kaydet**'i seçin.

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

1. Varsayılan metni aşağıdaki sorguyla değiştirin. 30 saniyelik süre içindeki ortalama makine sıcaklığı 70 dereceye ulaşırsa SQL kodu uyarı çıkışına bir sıfırlama komutu gönderir. Sıfırlama komutu, gerçekleştirilebilecek bir eylem olarak önceden sensöre programlanmıştır. 

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

1. **Kaydet**’i seçin.

### <a name="configure-iot-edge-settings"></a>IoT Edge ayarlarını yapılandırma

IoT Edge cihazınıza dağıtılacak Stream Analytics işinizi hazırlamak için işi depolama hesabındaki bir kapsayıcıyla ilişkilendirmeniz gerekir. İşinizi dağıttığınızda iş tanımı depolama kapsayıcısına aktarılır. 

1. Under **Configure**, select **Storage account settings** then select **Add storage account**. 

   ![Azure Stream Analytics - add storage account](./media/tutorial-deploy-stream-analytics/add-storage-account.png)

1. Select the **Storage account** that you created at the beginning of this tutorial from the drop-down menu.

1. **Kapsayıcı** alanında **Yeni oluştur**'u seçip depolama kapsayıcısı için bir ad girin. 

1. **Kaydet**’i seçin. 

## <a name="deploy-the-job"></a>İşi dağıtma

Artık Azure Stream Analytics işinizi IoT Edge cihazınıza dağıtmaya hazırsınız. 

Bu bölümde Azure portaldaki **Modülleri Ayarlama** sihirbazını kullanarak bir *dağıtım bildirimi* oluşturacaksınız. Dağıtım bildirimi bir cihaza dağıtılacak tüm modülleri, modül görüntülerinin depolandığı kapsayıcı kayıt defterlerini, modüllerin yönetilme şeklini ve modüllerin birbirleriyle iletişim kurma şeklini belirten bir JSON dosyasıdır. IoT Edge cihazınız, dağıtım bildirimini IoT Hub'dan aldıktan sonra içindeki bilgileri kullanarak tüm atanmış modülleri dağıtır ve yapılandırır. 

Bu öğreticide iki modül dağıtacaksınız. The first is **SimulatedTemperatureSensor**, which is a module that simulates a temperature and humidity sensor. İkincisi ise Stream Analytics işinizdir. Sensör modülü, iş sorgunuzun analiz edeceği veri akışını sağlar. 

1. Azure portalında IoT Hub'ınıza gidin.

1. Go to **IoT Edge**, and then open the details page for your IoT Edge device.

1. **Modül ayarla**’yı seçin.  

1. If you previously deployed the SimulatedTemperatureSensor module on this device, it might autopopulate. Aksi takdirde aşağıdaki adımlarla modülü ekleyin:

   1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
   1. For the name, type **SimulatedTemperatureSensor**.
   1. Görüntü URI'si alanına **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** yazın. 
   1. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.

1. Aşağıdaki adımları uygulayarak Azure Stream Analytics Edge işinizi ekleyin:

   1. **Ekle**'ye tıklayıp **Azure Stream Analytics Modülü**'nü seçin.
   1. Aboneliğinizi ve oluşturduğunuz Azure Stream Analytics Edge işini seçin. 
   1. **Kaydet**’i seçin.

   Once you save your changes, the details of your Stream Analytics job are published to the storage container that you created. 

1. When the Stream Analytics module is added to the list of modules, select **Configure** to see how it's structured. 

   Görüntü URI'si, standart bir Azure Stream Analytics görüntüsünü işaret eder. This one image is used for every Stream Analytics module that gets deployed to an IoT Edge device. 

   Modül ikizi, **ASAJobInfo** adlı istenen özellikle yapılandırılır. Bu özelliğin değeri, depolama kapsayıcınızdaki iş tanımını gösterir. This property is how the Stream Analytics image is configured with your specific job details. 

   By default, the Stream Analytics module takes the same name as the job it's based on. You can change the module name on this page if you like, but it's not necessary. 

1. Close the module configuration page.

1. Bir sonraki adımda kullanacağınız için Stream Analytics modülünüzün adını not edin ve devam etmek için **İleri**'yi seçin.

1. **Rotalar** bölümündeki varsayılan değeri aşağıdaki kodla değiştirin. _{moduleName}_ alanının üç örneğini Azure Stream Analytics modülünüzün adıyla güncelleştirin. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/SimulatedTemperatureSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/SimulatedTemperatureSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/SimulatedTemperatureSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

   Burada belirttiğiniz rotalar, IoT Edge cihazından veri akışını belirler. The telemetry data from SimulatedTemperatureSensor are sent to IoT Hub and to the **temperature** input that was configured in the Stream Analytics job. The **alert** output messages are sent to IoT Hub and to the SimulatedTemperatureSensor module to trigger the reset command. 

1. **İleri**’yi seçin.

1. In the **Review Deployment** step, you can see how the information you provided in the wizard is converted into a JSON deployment manifest. When you're done reviewing the manifest, select **Submit**.

1. Cihaz ayrıntıları sayfasına dönüp **Yenile**'yi seçin.  

    You should see the new Stream Analytics module running, along with the IoT Edge agent and IoT Edge hub modules. It may take a few minutes for the information to reach your IoT Edge device, and then for the new modules to start. If you don't see the modules running right away, continue refreshing the page.

    ![SimulatedTemperatureSensor and ASA module reported by device](./media/tutorial-deploy-stream-analytics/module-output2.png)

## <a name="view-data"></a>Verileri görüntüleme

Now you can go to your IoT Edge device to check out the interaction between the Azure Stream Analytics module and the SimulatedTemperatureSensor module.

1. Docker'daki tüm modüllerin çalıştığından emin olun:

   ```cmd/sh
   iotedge list  
   ```
   <!--
   ![Docker output](./media/tutorial-deploy-stream-analytics/docker_output.png)
   -->
1. Tüm sistem günlüklerini ve ölçüm verilerini görüntüleyin. Stream Analytics modülünün adını kullanın:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

1. View the reset command affect the SimulatedTemperatureSensor by viewing the sensor logs:

   ```cmd/sh
   iotedge logs SimulatedTemperatureSensor
   ```

   You can watch the machine's temperature gradually rise until it reaches 70 degrees for 30 seconds. Bu noktada Stream Analytics modülü bir sıfırlama işlemini tetikler ve makine sıcaklığı 21'e düşer. 

   ![Reset command output into module logs](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Otherwise, you can delete the local configurations and the Azure resources that you used in this article to avoid charges. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınızdan gelen verileri analiz etmek için bir Azure Streaming Analytics işi yapılandırdınız. Ardından bu Azure Stream Analytics modülünü IoT Edge cihazınıza yükleyerek yerel ortamdaki sıcaklık artışını izleme ve buna tepki göstermeye ek olarak toplanan veri akışının buluta gönderilmesini sağladınız. Azure IoT Edge sisteminin işletmeniz için oluşturabileceği ek çözümleri görmek için diğer öğreticilere geçin.

> [!div class="nextstepaction"] 
> [Bir Azure Machine Learning modelini modül olarak dağıtma](tutorial-deploy-machine-learning.md)
