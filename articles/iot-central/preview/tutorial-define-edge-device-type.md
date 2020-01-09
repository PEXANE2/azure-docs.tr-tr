---
title: Öğretici-Azure IoT Central yeni bir Azure IoT Edge cihaz türü tanımlama
description: Bu öğreticide, Azure IoT Central uygulamanızda yeni bir Azure IoT Edge cihaz oluşturma, Oluşturucu olarak gösterilir. Türü için telemetri, durum, özellik ve komutları tanımlarsınız.
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e5aa41023e1d7f84e8de095d13835d5ebdf66f57
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434863"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Öğretici: Azure IoT Central uygulamanızda yeni bir Azure IoT Edge cihaz türü tanımlama (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, bir Oluşturucu olarak, Azure IoT Central uygulamanızda yeni bir Azure IoT Edge cihaz türü tanımlamak için bir cihaz şablonunun nasıl kullanılacağı gösterilmektedir. 

Genel bakış için bkz. [Azure IoT Central (Önizleme özellikleri) nedir?](overview-iot-central.md). 

IoT Edge üç bileşenden oluşur:
* **IoT Edge modüller** , Azure Hizmetleri, iş ortağı hizmetleri veya kendi kodunuzla çalışan kapsayıcılardır. Modüller IoT Edge cihazlara dağıtılır ve bu cihazlarda yerel olarak çalıştırılır.
* **IoT Edge çalışma zamanı** her bir IoT Edge cihazında çalışır ve her cihaza dağıtılan modülleri yönetir.
* **Bulut tabanlı bir arabirim** , IoT Edge cihazları uzaktan izlemenize ve yönetmenize olanak sağlar. IoT Central bulut arabirimidir.

**Azure IoT Edge** cihaz, IoT Edge cihazına bağlanan aşağı akış cihazları ile bir ağ geçidi cihazı olabilir. Bu öğretici, aşağı akış cihaz bağlantısı desenleri hakkında daha fazla bilgi paylaşır.

**Cihaz şablonu** , cihazınızın ve IoT Edge modüllerinizin yeteneklerini tanımlar. Özellikler modülün gönderdiği Telemetriyi, modül özelliklerini ve bir modülün yanıt verdiği komutları içerir.

Bu öğreticide, bir ortam algılayıcısı cihaz şablonu oluşturacaksınız. Bir ortam algılayıcı cihazı:

* Sıcaklık gibi telemetri gönderir.
* Bulutta güncelleştirildiği zaman, telemetri gönderme aralığı gibi yazılabilir özelliklere yanıt verir.
* Sıcaklık sıfırlama gibi komutlara yanıt verir.

Ayrıca, bu öğreticide bir ortam ağ geçidi cihaz şablonu oluşturacaksınız. Bir çevresel ağ geçidi cihazı:

* Sıcaklık gibi telemetri gönderir.
* Bulutta güncelleştirildiği zaman, telemetri gönderme aralığı gibi yazılabilir özelliklere yanıt verir.
* Sıcaklık sıfırlama gibi komutlara yanıt verir.
* Diğer cihaz yetenek modelleriyle ilişkilerin yapılmasına izin verir.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir cihaz cihaz şablonu oluşturun Azure IoT Edge.
> * Dağıtım bildirimini karşıya yükleyin.
> * Her modül için telemetri, özellik ve komutları içeren yetenekler oluşturun.
> * Modül telemetrisi için görselleştirme tanımlayın.
> * Aşağı akış cihaz şablonlarına ilişkiler ekleyin.
> * Cihaz şablonunuzu yayımlayın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için [bir Azure IoT Central uygulaması oluşturmanız](quick-deploy-iot-central.md)gerekir.


## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Ağ geçidi ve modüllerle aşağı akış cihaz ilişkileri

Aşağı akış cihazları `$edgeHub` modülü aracılığıyla bir IoT Edge Gateway cihazına bağlanabilir. Bu IoT Edge cihaz, bu senaryoda saydam bir ağ geçidi haline gelir.

![Saydam ağ geçidi diyagramı](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Aşağı akış cihazları, özel bir modül aracılığıyla bir IoT Edge Gateway cihazına da bağlanabilir. Aşağıdaki senaryoda, aşağı akış cihazları bir Modbus özel modülü aracılığıyla bağlanır.

![Özel modül bağlantısının diyagramı](./media/tutorial-define-edge-device-type/gateway-module.png)

Aşağıdaki diyagramda her iki modül türü (özel ve `$edgeHub`) aracılığıyla bir IoT Edge ağ geçidi cihazına bağlantı gösterilmektedir.  

![Her iki bağlantı modülü ile bağlanma diyagramı](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Son olarak, aşağı akış cihazları birden çok özel modülle bir IoT Edge ağ geçidi cihazına bağlanabilir. Aşağıdaki diyagramda, bir Modbus özel modülü, uyumlu olmayan özel bir modül ve `$edgeHub` modülü aracılığıyla bağlanan aşağı akış cihazları gösterilmektedir. 

![Birden çok özel modül aracılığıyla bağlantı diyagramı](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Şablon oluşturma

Bir Oluşturucu olarak, uygulamanızda IoT Edge cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu yayımladıktan sonra, cihaz şablonunu uygulayan gerçek cihazlara bağlanabilirsiniz.

### <a name="select-device-template-type"></a>Cihaz şablonu türünü seçin 

Uygulamanıza yeni bir cihaz şablonu eklemek için sol bölmedeki **cihaz şablonları** ' nı seçin.

![Cihaz şablonları vurgulanmış şekilde, önizleme uygulamasının ekran görüntüsü](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Yeni bir cihaz şablonu oluşturmaya başlamak için **+ Yeni** ' yi seçin.

![Yeni vurgulanmış şekilde cihaz şablonları sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

**Şablon türü seç** sayfasında, **Azure IoT Edge**' yi seçin ve **İleri**' yi seçin.

![Şablon türü seç sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Cihaz şablonunu özelleştirme

IoT Edge, iş mantığını modül biçiminde dağıtabilir ve yönetebilirsiniz. IoT Edge modüller, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure Hizmetleri (Azure Stream Analytics gibi) veya kendi çözüme özgü kodunuzla bulunabilir. Modüllerin nasıl geliştirildiğini, dağıtıldığını ve yapıldığını anlamak için bkz. [IoT Edge modüller](../../iot-edge/iot-edge-modules.md).

Yüksek düzeyde, bir dağıtım bildirimi, istenen özellikleri ile yapılandırılan modül ikizlerini bir listesidir. Dağıtım bildirimi, hangi modüllerin yükleneceğini ve bunların nasıl yapılandırılacağını bir IoT Edge cihazına (veya bir cihaz grubuna) bildirir. Dağıtım bildirimleri, her modül için istenen özellikleri içerir ikizi. IoT Edge cihazlar her bir modülün bildirilen özelliklerini geri bildirir.

Dağıtım bildirimi oluşturmak için Visual Studio Code kullanın. Daha fazla bilgi için bkz. [Visual Studio Code Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Bu öğretici için kullanılacak örnek olarak bir modülle birlikte temel bir dağıtım bildirimi aşağıda verilmiştir. Aşağıdaki JSON 'u kopyalayın ve bir. JSON dosyası olarak kaydedin. 

   ```JSON
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {
              "SendData": true,
              "SendInterval": 10
         }
       }
     }
   }
   ```

#### <a name="upload-an-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini karşıya yükle

**Cihazı Özelleştir** sayfasında, **Azure IoT Edge dağıtım bildirimini yükle**' nin altında, **Araştır**' ı seçin. 

![Tarama vurgulanacak şekilde cihaz özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Bir IoT Edge ağ geçidi cihaz şablonu oluşturmayı planlıyorsanız, **ağ geçidi cihazını aşağı akış cihazlarıyla**seçtiğinizden emin olun.

![Aşağı akış cihazları vurgulanmış şekilde ağ geçidi aygıtıyla cihaz özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-upload.png)

Dosya seçimi iletişim kutusunda, dağıtım bildirim dosyasını seçin ve **Aç**' ı seçin.

IoT Edge, dağıtım bildirim dosyasını bir şemaya karşı doğrular. Doğrulama başarılı olursa, **gözden geçir**' i seçin.

![Dağıtım bildirimi ve gözden geçirme vurgulanmış şekilde cihaz özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

Aşağıdaki akış çizelgesinde IoT Central bir dağıtım bildirimi yaşam döngüsü gösterilmektedir.

![Dağıtım bildirimi yaşam döngüsü akış çizelgesi](./media/tutorial-define-edge-device-type/dmflow.png)

Ardından, dağıtım bildiriminin ayrıntılarını içeren bir inceleme sayfası görürsünüz. Bu sayfada dağıtım bildiriminden modüllerin bir listesi gösterilir. Bu öğreticide `SimulatedTemperatureSensor` modülünün listelendiğini unutmayın. **Oluştur**’u seçin.

![Modülle birlikte Inceleme sayfasının ekran görüntüsü ve vurgulanmış oluştur](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Bir ağ geçidi cihazı seçtiyseniz, aşağıdaki gözden geçirme sayfasını görürsünüz.

![Azure IoT Edge ağ geçidi vurgulanmış şekilde Inceleme sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-review.png)


Modül yetenek modelleriyle bir cihaz şablonu oluşturursunuz. Bu öğreticide, `SimulatedTemperatureSensor` modülü yetenek modeliyle bir cihaz şablonu oluşturacaksınız. 

Cihaz şablonunun başlığını **ortam algılayıcı cihaz şablonuna**değiştirin.

![Güncelleştirilmiş başlık vurgulanmış şekilde cihaz şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

IoT Edge cihaz ' de IoT Tak ve Kullan aşağıdaki şekilde modelleyin:
* Her IoT Edge cihaz şablonunda bir cihaz yetenek modeli vardır.
* Dağıtım bildiriminde listelenen her özel modül için bir modül yetenek modeli oluşturulur.
* Her modül yetenek modeli ve cihaz yetenek modeli arasında bir ilişki oluşturulur.
* Modül yetenek modeli modül arabirimlerini uygular.
* Her modül arabirimi telemetri, Özellikler ve komutlar içerir.

![IoT Edge modelleme diyagramı](./media/tutorial-define-edge-device-type/edgemodelling.png)

#### <a name="add-capabilities-to-a-module-capability-model"></a>Modül yetenek modeline özellikler ekleme

`SimulatedTemperatureSensor` modülünden bir örnek çıktı aşağıda verilmiştir:
```json
{

    "machine": {

        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

`SimulatedTemperatureSensor` modüle, önceki çıktıyı yansıtan yetenekler ekleyebilirsiniz. 

1. `SimulatedTemperatureSensor` Module yetenek modelinin bir arabirimini yönetmek için **yönet** > **Add capability**' yi seçin. 

    ![Ortam algılayıcısı şablonunun ekleme özelliği vurgulanmış ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
1. Bir makineyi nesne türü olarak ekleyin.
  
    ![Şema vurgulanmış şekilde ortam algılayıcısı Şablon Özellikleri sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

1. **Tanımla**' yı seçin. Görüntülenen iletişim kutusunda, nesne adını **makine**olarak değiştirin. Sıcaklık ve basınç özellikleri oluşturun ve **Uygula**' yı seçin.
  
    ![Çeşitli seçenekler vurgulanmış şekilde öznitelikler iletişim kutusunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
1. Bir nesne türü olarak **ortam** ekleyin.

1. **Tanımla**' yı seçin. Görüntülenen iletişim kutusunda, nesne adını **çevresel**olarak değiştirin. Sıcaklık ve nem özellikleri oluşturun ve **Uygula**' yı seçin.
  
    ![Çeşitli seçenekler vurgulanmış şekilde öznitelikler iletişim kutusunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
1. `timeCreated` `DateTime` türü olarak ekleyin ve **Kaydet**' i seçin.
  
    ![Kayıt vurgulandığında, ortam algılayıcısı şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>İlişki Ekle

Bir IoT Edge cihazını ağ geçidi cihazı olacak şekilde seçtiyseniz, ağ geçidi cihazına bağlamak istediğiniz cihazlar için cihaz yetenek modellerine aşağı akış ilişkileri ekleyebilirsiniz.
  
  ![Ilişki Ekle vurgulanmış olan ortam ağ geçidi şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

Bir cihaza veya modüle bir ilişki ekleyebilirsiniz.
  
  ![Cihaz ve modül düzeyi ilişkileri vurgulanmış şekilde ortam ağ geçidi şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Bir aşağı akış cihazı yetenek modeli seçebilir veya yıldız simgesini seçebilirsiniz. 
  
  ![Hedef vurgulanan ortam ağ geçidi şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  Bu öğretici için yıldız işaretini seçin. Bu seçenek, herhangi bir aşağı akış ilişkisine izin verir. Daha sonra **Kaydet**’e tıklayın.

  ![Hedef vurgulanan ortam ağ geçidi şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Bulut özellikleri ekle

Bir cihaz şablonu, bulut özellikleri içerebilir. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve bir cihazdan hiçbir şekilde gönderilmez veya buradan alınmaz.

1. **Bulut özelliklerini** seçin >  **+ bulut özelliği ekleyin**. Cihaz şablonunuza bir bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın.

    | Görünen ad      | Anlamsal tür | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Hiçbiri          | Tarih   |
    | Müşteri adı     | Hiçbiri          | Dize |

2. **Kaydet**’i seçin.

  
    ![Kayıt vurgulandığında, ortam algılayıcısı şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Özelleştirmeler ekleme

Bir arabirimi değiştirmek ya da cihaz yetenek modelinizi sürümünüzü gerektirmeyen bir özelliğe IoT Central özgü özellikler eklemek için özelleştirmeleri kullanın. Yetenek modeli taslak veya Yayımlanma durumundaysa alanları özelleştirebilirsiniz. Yalnızca arabirim uyumluluğunu kesen alanları özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

- Bir özelliğin görünen adını ve birimlerini özelleştirin.
- Değer bir grafikte göründüğünde kullanılacak varsayılan rengi ekleyin.
- Bir özellik için başlangıçtaki, minimum ve maksimum değerleri belirtin.

Yetenek adını veya yetenek türünü özelleştiremezsiniz.

Özelleştirmeyi bitirdiğinizde **Kaydet**' i seçin.
  
![Ortam algılayıcı şablonu özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Görünümleri oluşturma

Bir Oluşturucu olarak, uygulamayı bir işlecine çevresel algılayıcı cihazı hakkındaki ilgili bilgileri görüntüleyecek şekilde özelleştirebilirsiniz. Özelleştirmeleriniz, uygulamaya bağlı çevresel algılayıcı cihazlarını yönetmek için işlecini etkinleştirir. Cihazlarla etkileşim kurmak için kullanılacak bir operatör için iki tür görünüm oluşturabilirsiniz:

* Cihaz ve bulut özelliklerini görüntüleme ve düzenleme için formlar.
* Cihazları görselleştirmek için panolar.

### <a name="configure-a-view-to-visualize-devices"></a>Cihazları görselleştirmek için bir görünüm yapılandırma

Bir cihaz panosu, bir işlecin grafikleri ve ölçümleri kullanarak bir cihazı görselleştirmesine olanak sağlar. Bir Oluşturucu olarak, bir cihaz panosunda hangi bilgilerin göründüğünü tanımlayabilirsiniz. Cihazlar için birden çok Pano tanımlayabilirsiniz. Ortam algılayıcısı telemetrisini görselleştirmek üzere bir pano oluşturmak için, **cihazı görselleştirme** > **Görünümler** ' i seçin:

  
![Cihaz vurgulanmasını içeren ortam algılayıcısı şablon görünümleri sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Çevresel telemetri ve makine telemetrisi karmaşık nesnelerdir. Grafik oluşturmak için:

1. **Çevresel telemetri**' i sürükleyin ve **çizgi grafik**' i seçin. 
  
   ![Çevresel telemetri ve çizgi grafik vurgulanmış şekilde ortam algılayıcısı şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/sensorambientchart.png)

1. Yapılandır simgesini seçin. Verileri görselleştirmek için **sıcaklık** ve **nem** ' ı seçin ve **yapılandırmayı Güncelleştir**' i seçin. 
  
   ![Çeşitli seçenekler vurgulanmış şekilde ortam algılayıcısı şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

1. **Kaydet**’i seçin.

Diğer özellikleri veya telemetri değerlerini gösteren daha fazla kutucuk ekleyebilirsiniz. Statik metin, bağlantılar ve görüntüler de ekleyebilirsiniz. Panodaki bir kutucuğu taşımak veya yeniden boyutlandırmak için fare işaretçisini döşemenin üzerine taşıyın ve kutucuğu yeni bir konuma sürükleyin veya yeniden boyutlandırın.
  
![Ortam algılayıcısı şablon panosu görünümünün ekran görüntüsü](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Bir cihaz formu Ekle

Bir cihaz formu, bir işlecin yazılabilir cihaz özelliklerini ve bulut özelliklerini düzenlemesini sağlar. Bir Oluşturucu olarak, birden çok form tanımlayabilir ve her formda hangi cihaz ve bulut özelliklerinin gösterileceğini seçebilirsiniz. Ayrıca, salt okunurdur cihaz özelliklerini bir formda görüntüleyebilirsiniz.

Ortam algılayıcısı özelliklerini görüntülemek ve düzenlemek için bir form oluşturmak için:

1. **Çevresel algılayıcı şablonunda**, **Görünümler**' e gidin. Yeni bir görünüm eklemek için **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.
  
   ![Cihaz ve bulut verilerinin düzenlenme ile ortam algılayıcısı şablon görünümleri sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

1. **Çevresel algılayıcı özellikleri**form adını girin.

1. **Müşteri adı** ve **son hizmet tarihi** bulutu özelliklerini formdaki mevcut bölüme sürükleyin.
  
   ![Çeşitli seçenekler vurgulanmış şekilde çevresel algılayıcı şablonu görünümleri sayfasının ekran görüntüsü](./media/tutorial-define-edge-device-type/views-properties.png)

1. **Kaydet**’i seçin.

## <a name="publish-a-device-template"></a>Cihaz şablonunu yayımlama

Sanal bir ortam algılayıcısı oluşturmadan veya gerçek bir ortam sensöre bağlanmadan önce, cihaz şablonunuzu yayımlamanız gerekir.

Bir cihaz şablonunu yayımlamak için:

1. Cihaz **şablonları** sayfasından cihaz şablonunuza gidin.

2. **Yayımla**’yı seçin.
  
    ![Yayımlama vurgulanmış şekilde, ortam algılayıcısı şablonunun ekran görüntüsü](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. **Cihaz şablonu Yayımla** Iletişim kutusunda **Yayımla**' yı seçin.
  
    ![Bir cihaz şablonu Yayımla iletişim kutusunun, yayınla vurgulanmış ekran görüntüsü](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Bir cihaz şablonu yayımlandıktan sonra **cihazlar** sayfasında ve işlecine görünür. Yayımlanmış bir cihaz şablonunda, yeni bir sürüm oluşturmadan bir cihaz yetenek modeli düzenleyemezsiniz. Ancak, yayımlanmış bir cihaz şablonunda bulut özellikleri, özelleştirmeler ve görünümlerde güncelleştirmeler yapabilirsiniz. Bu güncelleştirmeler yeni bir sürümün oluşturulmasına neden olmaz. Herhangi bir değişiklik yaptıktan sonra, bu değişiklikleri işletmenizin dışına göndermek için **Yayımla** ' yı seçin.
  
![Yayınlanan şablonlar cihaz şablonları listesinin ekran görüntüsü](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Yaprak cihaz şablonu olarak yeni bir kenar oluşturun.
* Karşıya yüklenen bir dağıtım bildiriminden modüller üretin.
* Karmaşık tür telemetrisi ve özellikleri ekleyin.
* Bulut özellikleri oluşturun.
* Özelleştirmeler oluşturun.
* Cihaz telemetrisi için görselleştirme tanımlayın.
* Edge cihaz şablonunuzu yayımlayın.

Artık Azure IoT Central uygulamanızda bir cihaz şablonu oluşturduğunuza göre, bunu bir sonraki adımda yapabilirsiniz:

> [!div class="nextstepaction"]
> [Sınır cihazını bağlama](./tutorial-add-edge-as-leaf-device.md)
