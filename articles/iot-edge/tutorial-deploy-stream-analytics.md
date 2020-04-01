---
title: Öğretici - Azure IoT Edge kullanarak kenardaki Akış Analizi
description: Bu eğitimde, Azure Akış Analitiği'ni bir IoT Edge aygıtına modül olarak dağıtMış sınız
author: kgremban
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: d8c3bde0f32c1df6c98f6a71f6ab830c21256903
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76906280"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>Öğretici: Azure Akış Analitiğini IoT Edge modülü olarak dağıtma

Birçok IoT çözümü, IoT aygıtlarından buluta ulaştığında veriler hakkında bilgi edinmek için analitik hizmetlerini kullanır. Azure IoT Edge ile [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) mantığını alıp cihazın kendisine aktarabilirsiniz. Telemetri akışlarını uç cihazlarda işleyerek yüklenen veri miktarını ve eyleme dönüştürülebilir içgörülere tepki verme süresini azaltabilirsiniz.

Azure IoT Edge ve Azure Akış Analizi, iş yükü geliştirmenizi kolaylaştırmak için entegre edilmiştir. Azure portalında bir Azure Akışı Analizi işi oluşturabilir ve ek kodu olmayan bir IoT Edge modülü olarak dağıtabilirsiniz.  

Azure Akış Analizi, hem bulutta hem de IoT Edge aygıtlarında veri analizi için zengin yapılandırılmış bir sorgu sözdizimi sağlar. Daha fazla bilgi için Azure [Akış Analizi belgelerine](../stream-analytics/stream-analytics-edge.md)bakın.

Bu öğreticideki Stream Analytics modülü, tekrar eden 30 saniyelik dönemler içindeki ortalama sıcaklığı hesaplar. Bu ortalama değer 70'e ulaştığında modül, eyleme geçmek için cihazla ilgili bir uyarı gönderir. Bu durumda eylem, sıcaklık sensörü simülasyonunu sıfırlamaktır. Bir üretim ortamında bu işlevi kullanarak sıcaklık tehlikeli düzeylere ulaştığında bir makineyi kapatabilir veya önleyici işlemler gerçekleştirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Verileri uç cihazlarda işlemek için bir Azure Stream Analytics işi oluşturma.
> * Yeni Azure Stream Analytics işini diğer IoT Edge modüllerine bağlama.
> * Azure Stream Analytics işini Azure portalından bir IoT Edge cihazına dağıtma.

<center>

![Diyagram - Öğretici mimari: sahne ve ASA iş dağıtmak](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows aygıtları](quickstart.md)için hızlı başlatma adımlarını izleyerek Bir Azure sanal makinesini IoT Edge aygıtı olarak kullanabilirsiniz.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

## <a name="create-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi oluşturma

Bu bölümde, aşağıdaki adımları yapacak bir Azure Akışı Analizi işi oluşturursunuz:

* IoT Edge cihazınızdan veri alın.
* Belirli bir aralık dışındaki değerler için telemetri verilerini sorgula.
* Sorgu sonuçlarına göre IoT Edge aygıtında işlem alın.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

IoT Edge cihazında çalıştırmak üzere oluşturduğunuz Azure Stream Analytics işinin cihazdan çağrılabilecek bir şekilde depolanması gerekir. Varolan bir Azure Depolama hesabı kullanabilir veya şimdi yeni bir hesap oluşturabilirsiniz.

1. Azure portalında, > kaynak**Depolama Deposu** > **hesabı** **oluştur'a**gidin.

1. Depolama hesabınızı oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlatmaları ve öğreticileri için tüm test kaynaklarınız için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Adı | Depolama hesabınıza benzersiz bir ad verin. |
   | Konum | Size yakın bir konum seçin. |

1. Diğer alanlar için varsayılan değerleri tutun ve **Gözden Geçir + Oluştur'u**seçin.

1. Ayarlarınızı gözden geçirin ve **ardından Oluştur'u**seçin.

### <a name="create-a-new-job"></a>Yeni bir iş oluşturma

1. Azure portalında, **bir** >  > kaynak**Nesnelerin İnterneti**Akışı Analizi işi oluşturma **'na**gidin.

1. İşinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | İş adı | İşinize bir ad verin. Örneğin, **IoTEdgeJob** |
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Barındırma ortamı | **Kenar**'ı seçin. |

1. **Oluştur'u**seçin.

### <a name="configure-your-job"></a>İşinizi yapılandırma

Azure portalda Stream Analytics işiniz oluşturulduktan sonra iletilen verilerle çalışması için giriş, çıkış ve sorgu ile yapılandırabilirsiniz.

Bu bölümde giriş, çıkış ve sorgu öğelerini kullanarak IoT Edge cihazından sıcaklık verilerini alan bir iş oluşturabilirsiniz. Bu iş 30 saniyelik aralıklarla verileri analiz eder. Herhangi bir aralıktaki ortalama sıcaklık değeri 70 derecenin üzerine çıkarsa IoT Edge cihazına bir uyarı gönderilir. Bir sonraki bölümde işi dağıtırken verilerin geldiği ve gittiği yerleri belirteceksiniz.  

1. Azure portalda Stream Analytics işinize gidin.

1. **İş Topolojisi** bölümünde **Girişler**'i ve **Akış girişi ekle**'yi seçin.

   ![Azure Akış Analizi - giriş ekle](./media/tutorial-deploy-stream-analytics/asa-input.png)

1. Açılan listeden **Edge Hub**'ını seçin.

1. **Yeni giriş** bölmesinde giriş diğer adı olarak **temperature** yazın.

1. Diğer alanlar için varsayılan değerleri kullanın ve **Kaydet**'i seçin.

1. **İş Topolojisi**'nin altında **Çıkışlar**'ı açın ve **Ekle**'yi seçin.

   ![Azure Akış Analizi - çıktı ekleme](./media/tutorial-deploy-stream-analytics/asa-output.png)

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

1. **Kaydet'i**seçin.

### <a name="configure-iot-edge-settings"></a>IoT Edge ayarlarını yapılandırma

IoT Edge cihazınıza dağıtılacak Stream Analytics işinizi hazırlamak için işi depolama hesabındaki bir kapsayıcıyla ilişkilendirmeniz gerekir. İşinizi dağıttığınızda iş tanımı depolama kapsayıcısına aktarılır.

1. **Yapılandırma**altında, **Depolama hesabı ayarlarını** seçin ve ardından **depolama hesabı ekle'yi**seçin.

   ![Azure Akış Analizi - depolama hesabı ekleyin](./media/tutorial-deploy-stream-analytics/add-storage-account.png)

1. Bu öğreticinin başında oluşturduğunuz **Depolama hesabını** açılır menüden seçin.

1. **Kapsayıcı** alanında **Yeni oluştur**'u seçip depolama kapsayıcısı için bir ad girin.

1. **Kaydet'i**seçin.

## <a name="deploy-the-job"></a>İşi dağıtma

Artık Azure Stream Analytics işinizi IoT Edge cihazınıza dağıtmaya hazırsınız.

Bu bölümde Azure portaldaki **Modülleri Ayarlama** sihirbazını kullanarak bir *dağıtım bildirimi* oluşturacaksınız. Dağıtım bildirimi bir cihaza dağıtılacak tüm modülleri, modül görüntülerinin depolandığı kapsayıcı kayıt defterlerini, modüllerin yönetilme şeklini ve modüllerin birbirleriyle iletişim kurma şeklini belirten bir JSON dosyasıdır. IoT Edge cihazınız, dağıtım bildirimini IoT Hub'dan aldıktan sonra içindeki bilgileri kullanarak tüm atanmış modülleri dağıtır ve yapılandırır.

Bu öğreticide iki modül dağıtacaksınız. İlk ilik ve nem sensörünü simüle eden bir modül olan **SimulatedTemperatureSensor'dur.** İkincisi ise Stream Analytics işinizdir. Sensör modülü, iş sorgunuzun analiz edeceği veri akışını sağlar.

1. Azure portalında IoT Hub'ınıza gidin.

1. **IoT Edge'e**gidin ve ardından IoT Edge aygıtınızın ayrıntılar sayfasını açın.

1. **Modülleri Ayarla'yı**seçin.  

1. Bu cihazda Daha önce Simüle Edilen TemperatureSensor modülünü dağıttıysanız, otomatik olarak doldurulabilir. Aksi takdirde aşağıdaki adımlarla modülü ekleyin:

   1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
   1. Adı **için, SimüleTemperatureSensor**yazın.
   1. Görüntü URI'si alanına **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** yazın.
   1. Diğer ayarları değiştirmeden bırakın ve **Kaydet'i**seçin.

1. Aşağıdaki adımları uygulayarak Azure Stream Analytics Edge işinizi ekleyin:

   1. **Ekle**'ye tıklayıp **Azure Stream Analytics Modülü**'nü seçin.
   1. Aboneliğinizi ve oluşturduğunuz Azure Stream Analytics Edge işini seçin.
   1. **Kaydet'i**seçin.

   Değişikliklerinizi kaydedince, Stream Analytics işinizin ayrıntıları oluşturduğunuz depolama kapsayıcısında yayınlanır.

1. Akış Analizi modülü modüller listesine eklendiğinde, nasıl yapılandırdığını görmek için adını seçin ve **Güncelleme IoT Edge Modülü** sayfasındaayarlarını güncelleyin.

   **Modül Ayarları** sekmesinde, standart bir Azure Akış Analizi resmine işaret eden **URI resmi** bulunur. Bu resim, bir IoT Edge aygıtına dağıtılan her Stream Analytics modülü için kullanılır.

   **Modül İkiz Ayarları** sekmesi, **ASAJobInfo**adı verilen Azure Akışı Analizi (ASA) özelliğini tanımlayan JSON'u gösterir. Bu özelliğin değeri, depolama kapsayıcınızdaki iş tanımını gösterir. Bu özellik, Akış Analizi görüntüsünün özel iş ayrıntılarınız ile nasıl yapılandırıldığıdır.

   Varsayılan olarak, Akış Analizi modülü dayandığı iş ile aynı adı alır. İsterseniz bu sayfadaki modül adını değiştirebilirsiniz, ancak gerekli değildir.

1. **İptal** veya **Kaydet'i**seçin.

1. Bir sonraki adımda ihtiyacınız olacağınız için Akış Analizi modülünüzün adını not edin ve ardından Devam Edecek **Sonraki: Rotalar'ı** seçin.

1. **Rotalar** sekmesinde, iletilerin modüller ve IoT Hub'ı arasında nasıl geçirildiğini tanımlarsınız. İletiler ad/değer çiftleri kullanılarak oluşturulur. Varsayılan `route` ve `upstream` ad ve değerleri aşağıdaki tabloda gösterilen çiftlerle, aşağıdaki ad/değer çiftleri ile değiştirin ve _{moduleName}_ örneklerini Azure Akış Analytics modülünüzün adıyla değiştirin.

    | Adı | Değer |
    | --- | --- |
    | `telemetryToCloud` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO $upstream` |
    | `alertsToCloud` | `FROM /messages/modules/{moduleName}/* INTO $upstream` |
    | `alertsToReset` | `FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint("/modules/SimulatedTemperatureSensor/inputs/control")` |
    | `telemetryToAsa` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO BrokeredEndpoint("/modules/{moduleName}/inputs/temperature")`|

    Burada belirttiğiniz rotalar, IoT Edge cihazından veri akışını belirler. Simüle TemperatureSensor'un telemetri verileri IoT Hub'a ve Stream Analytics işinde yapılandırılan **sıcaklık** girdisine gönderilir. **Uyarı** çıkış iletileri sıfırlama komutunu tetiklemek için IoT Hub'a ve Simüle Edilen Sıcaklık Sensörü modülüne gönderilir.

1. **Sonraki'ni Seçin: Gözden Geçirin + Oluştur**.

1. Gözden **Geçirme + Oluştur** sekmesinde, sihirbazda sağladığınız bilgilerin Nasıl JSON dağıtım bildirimine dönüştürüldüğünü görebilirsiniz. Bildirimi gözden geçirmeyi bitirdiğinizde **Oluştur'u**seçin.

1. Aygıt ayrıntıları sayfasına döndürülür. **Yenile'yi**seçin.  

    Yeni Stream Analytics modülünün, IoT Edge aracısı ve IoT Edge hub modülleriyle birlikte çalıştığını görmeniz gerekir. Bilgilerin IoT Edge cihazınıza ulaşması ve ardından yeni modüllerin başlaması birkaç dakika sürebilir. Modüllerin hemen çalıştığını görmüyorsanız, sayfayı yenilemeye devam edin.

    ![Cihaz tarafından bildirilen Simüle TemperatureSensor ve ASA modülü](./media/tutorial-deploy-stream-analytics/module-output2.png)

## <a name="view-data"></a>Verileri görüntüleme

Artık Azure Akış Analizi modülü ile Simüle Edilen Sıcaklık Sensörü modülü arasındaki etkileşimi kontrol etmek için IoT Edge cihazınıza gidebilirsiniz.

1. Docker'daki tüm modüllerin çalıştığından emin olun:

   ```cmd/sh
   iotedge list  
   ```

1. Tüm sistem günlüklerini ve ölçüm verilerini görüntüleyin. Stream Analytics modülünün adını kullanın:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

1. Sıfırlama komutunu görüntüleyin sensör günlüklerini görüntüleyerek Simüle Edilen Sıcaklık Sensörü'ni etkiler:

   ```cmd/sh
   iotedge logs SimulatedTemperatureSensor
   ```

   Makinenin sıcaklığının 30 saniye boyunca 70 dereceye ulaşana kadar kademeli olarak yükselmesini izleyebilirsiniz. Bu noktada Stream Analytics modülü bir sıfırlama işlemini tetikler ve makine sıcaklığı 21'e düşer.

   ![Komut çıktısını modül günlüklerine sıfırlama](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Aksi takdirde, ücretlendirmeleri önlemek için bu makalede kullandığınız yerel yapılandırmaları ve Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınızdan gelen verileri analiz etmek için bir Azure Streaming Analytics işi yapılandırdınız. Ardından bu Azure Stream Analytics modülünü IoT Edge cihazınıza yükleyerek yerel ortamdaki sıcaklık artışını izleme ve buna tepki göstermeye ek olarak toplanan veri akışının buluta gönderilmesini sağladınız. Azure IoT Edge sisteminin işletmeniz için oluşturabileceği ek çözümleri görmek için diğer öğreticilere geçin.

> [!div class="nextstepaction"]
> [Bir Azure Machine Learning modelini modül olarak dağıtma](tutorial-deploy-machine-learning.md)
