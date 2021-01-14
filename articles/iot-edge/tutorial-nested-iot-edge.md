---
title: Öğretici-IoT Edge cihaz hiyerarşisi oluşturma-Azure IoT Edge
description: Bu öğreticide, ağ geçitlerini kullanarak IoT Edge cihazların hiyerarşik yapısını nasıl oluşturacağınız gösterilmektedir.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 11/10/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: a9591a394d80e7b4c60f28fda6c0a425ba3d0a4f
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98180073"
---
# <a name="tutorial-create-a-hierarchy-of-iot-edge-devices-preview"></a>Öğretici: IoT Edge cihazları hiyerarşisi oluşturma (Önizleme)

Hiyerarşik katmanlarda düzenlenmiş ağlarda Azure IoT Edge düğümleri dağıtın. Hiyerarşideki her katman, altındaki katmandaki cihazlardan gelen iletileri ve istekleri işleyen bir ağ geçidi aygıtıdır.

>[!NOTE]
>Bu özellik, Linux kapsayıcıları çalıştıran, genel önizlemede olan IoT Edge sürüm 1,2 gerektirir.

Yalnızca en üst katmanın buluta bağlantısı olması için bir cihaz hiyerarşisi oluşturabilir ve alt katmanlar yalnızca komşu Kuzey ve Güney katmanlarla iletişim kurabilir. Bu ağ katmanlama, [ISA-95 standardını](https://en.wikipedia.org/wiki/ANSI/ISA-95)izleyen en endüstriyel ağların temelidir.

Bu öğreticinin amacı, bir üretim ortamına benzetim yapan bir IoT Edge cihazları hiyerarşisi oluşturmaktır. Sonunda, kapsayıcı görüntülerini hiyerarşi aracılığıyla indirerek internet erişimi olmadan daha düşük bir katman cihazına [sanal sıcaklık algılayıcısı modülünü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.simulated-temperature-sensor) dağıtacaksınız.

Bu öğreticiyi başarmak için, bu öğreticide IoT Edge cihaz hiyerarşisi oluşturma, cihazlarınıza IoT Edge çalışma zamanı kapsayıcıları dağıtma ve cihazlarınızı yerel olarak yapılandırma işlemleri adım adım açıklanmıştır. Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> * IoT Edge cihazların hiyerarşisinde ilişkiler oluşturun ve tanımlayın.
> * Hiyerarşinizdeki cihazlarda IoT Edge çalışma zamanını yapılandırın.
> * Cihaz hiyerarşiniz genelinde tutarlı sertifikalar yükler.
> * Hiyerarşinizdeki cihazlara iş yükleri ekleyin.
> * Düşük katman cihazlarınızdan tek bir bağlantı noktası üzerinden HTTP trafiğini güvenli bir şekilde yönlendirmek için bir API proxy modülü kullanın.

Bu öğreticide, aşağıdaki ağ katmanları tanımlanmıştır:

* **Üst katman**: bu katmandaki IoT Edge cihazlar doğrudan buluta bağlanabilir.

* **Alt katman**: IoT Edge cihazlar bu katmanda doğrudan buluta bağlanamaz. Veri göndermek ve almak için bir veya daha fazla ara IoT Edge cihazlarından birine gitmeleri gerekir.

Bu öğreticide kolaylık sağlamak için iki cihaz hiyerarşisi kullanılmaktadır. Bir cihaz, **topLayerDevice**, hiyerarşinin en üst katmanında, doğrudan buluta bağlanabilecek bir cihazı temsil eder. Bu cihaza **ana cihaz** da denir. Diğer cihaz, küçük **harf Layerdevice**, hiyerarşinin alt katmanında doğrudan buluta bağlanamaz bir cihazı temsil eder. Bu cihaza **alt cihaz** da denir. Üretim ortamınızı temsil etmek için ek alt katman cihazları ekleyebilirsiniz. Diğer tüm ek katman cihazlarının yapılandırması, küçük **Layerdevice**'ın yapılandırmasını izler.

## <a name="prerequisites"></a>Ön koşullar

IoT Edge cihazlarının bir hiyerarşisini oluşturmak için şunlar gerekir:

* İnternet bağlantısı olan bir bilgisayar (Windows veya Linux).
* Geçerli aboneliği olan bir Azure hesabı. [Azure aboneliğiniz](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Azure 'da ücretsiz veya standart bir katman [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) .
* Azure CLı v 2.3.1, Azure IoT uzantısı v 0.10.6 veya üzeri yüklü. Bu öğretici [Azure Cloud Shell](../cloud-shell/overview.md)kullanır. Azure Cloud Shell hakkında bilgi sahibi değilseniz, [Ayrıntılar için hızlı başlangıç ' a bakın](./quickstart-linux.md#prerequisites).
* IoT Edge cihaz olarak yapılandırılacak iki Linux cihaz. Kullanılabilir cihazlar yoksa, aşağıdaki komutta yer tutucu metnini değiştirerek ve iki kez çalıştırarak iki Azure sanal makinesi oluşturabilirsiniz:

   ```azurecli-interactive
   az vm create \
    --resource-group <REPLACE_WITH_RESOURCE_GROUP> \
    --name <REPLACE_WITH_UNIQUE_NAMES_FOR_EACH_VM> \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password <REPLACE_WITH_PASSWORD>
   ```

* Aşağıdaki bağlantı noktalarının açık olduğundan emin olun: 8000, 443, 5671, 8883:
  * 8000: API proxy 'si aracılığıyla Docker kapsayıcı görüntülerini çekmek için kullanılır.
  * 443: REST API çağrıları için üst ve alt sınır hub 'ları arasında kullanılır.
  * 5671, 8883: AMQP ve MQTT için kullanılır.

  Daha fazla bilgi için bkz. [Azure portalıyla bir sanal makineye bağlantı noktaları açma](../virtual-machines/windows/nsg-quickstart-portal.md).

Ayrıca, bir fabrika ortamının benzetimini yapmak için Azure sanal makinelerini önceden yapılandırılmış cihazlar olarak dağıtan [endüstriyel IoT örneği için](https://aka.ms/iotedge-nested-sample)komut dosyalı Azure IoT Edge ' yi izleyerek bu senaryoyu deneyebilirsiniz.

## <a name="configure-your-iot-edge-device-hierarchy"></a>IoT Edge cihaz hiyerarşinizi yapılandırma

### <a name="create-a-hierarchy-of-iot-edge-devices"></a>IoT Edge cihaz hiyerarşisi oluşturma

IoT Edge cihazlarınızı oluşturmaya yönelik ilk adım, Azure portal veya Azure CLı aracılığıyla yapılabilir. Bu öğreticide, iki IoT Edge cihaz hiyerarşisi oluşturulur: **topLayerDevice** ve onun alt küçük **harf layerdevice**.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portal](https://ms.portal.azure.com/)IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. **+ IoT Edge cihaz ekle**' yi seçin. Bu cihaz üst katman cihazı olacak, uygun bir benzersiz cihaz KIMLIĞI girin. **Kaydet**’i seçin.

1. **+ IoT Edge cihaz ekle** ' yi seçin. Bu cihaz kenar alt katman cihazı olacak, uygun bir benzersiz cihaz KIMLIĞI girin.

1. **Bir üst cihaz ayarla**' yı seçin, cihaz listesinden en üst katman cihazınızı seçin ve **Tamam**' ı seçin. **Kaydet**’i seçin.

   ![Alt katman cihazı için üst öğe ayarlanıyor](./media/tutorial-nested-iot-edge/set-parent-device.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/), hub 'ınızda bir IoT Edge cihaz oluşturmak için aşağıdaki komutu girin. Bu cihaz üst katman cihazı olacak ve uygun bir benzersiz cihaz KIMLIĞI girin:

   ```azurecli-interactive
   az iot hub device-identity create --device-id {top_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

1. Alt IoT Edge cihazınızı oluşturmak ve cihazlar arasında üst-alt ilişkisi oluşturmak için aşağıdaki komutu girin:

    ```azurecli-interactive
    az iot hub device-identity create --device-id {lower_layer_device_id} --edge-enabled --pd {top_layer_device_id} --hub-name {iothub_name}
    ```

---

Her bir IoT Edge cihazının bağlantı dizesini unutmayın. Bunlar daha sonra kullanılacaktır.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://ms.portal.azure.com/), IoT Hub **IoT Edge** bölümüne gidin.

1. Cihazlar listesinde cihazlardan birinin cihaz KIMLIĞINE tıklayın.

1. **Birincil bağlantı dizesi** alanında **Kopyala** ' yı seçin ve tercih ettiğiniz bir yere kaydedin.

1. Diğer tüm cihazlar için yineleyin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/), her bir cihaz için, cihazınızın bağlantı dizesini almak ve istediğiniz bir yere kaydetmek için aşağıdaki komutu girin:

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id {device_id} --hub-name {hub_name}
   ```

---

### <a name="create-certificates"></a>Sertifika oluşturma

Bir [ağ geçidi senaryosunda](iot-edge-as-gateway.md) tüm cihazların aralarında güvenli bağlantıları ayarlaması için paylaşılan bir sertifika gerekir. Bu senaryodaki her iki cihaz için tanıtım sertifikaları oluşturmak için aşağıdaki adımları kullanın.

Bir Linux cihazında tanıtım sertifikaları oluşturmak için, kuşak betikleri klonlamanız ve bunları Bash 'te yerel olarak çalışacak şekilde ayarlamanız gerekir.

1. Tanıtım sertifikaları oluşturmak için betikler içeren IoT Edge Git deposunu kopyalayın.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

1. Üzerinde çalışmak istediğiniz dizine gidin. Tüm sertifika ve anahtar dosyaları bu dizinde oluşturulacaktır.

1. Yapılandırma ve betik dosyalarını kopyalanan IoT Edge deposundan çalışma dizininize kopyalayın.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

1. Kök CA sertifikasını ve bir ara sertifikayı oluşturun.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur, ancak ağ geçidi hiyerarşisi için **kök CA sertifikası** olarak aşağıdaki dosyayı kullanıyoruz:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

1. Aşağıdaki komutla iki IoT Edge cihaz CA sertifikası ve özel anahtar kümesi oluşturun: en üst katman cihazı için bir küme ve alt katman cihazı için bir küme. CA sertifikalarının birbirinden ayırt edilebilmesi için hatırlanabilen adlar sağlayın.

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "top-layer-device"
   ./certGen.sh create_edge_device_ca_certificate "lower-layer-device"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur, ancak her bir IoT Edge cihazında aşağıdaki sertifikayı ve anahtar çiftini kullanıyoruz ve config. YAML dosyasında başvurulur:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Her cihazın kök CA sertifikasının bir kopyasına ve kendi cihaz CA sertifikasının ve özel anahtarının bir kopyasına ihtiyacı vardır. Sertifikaları her cihaza taşımak için bir USB sürücüsü veya [güvenli dosya kopyalama](https://www.ssh.com/ssh/scp/) kullanabilirsiniz.

1. Sertifikalar aktarıldıktan sonra her bir cihaz için kök CA 'yı yükler.

   ```bash
   sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```

   Bu komut,/etc/SSL/certs' ye bir sertifika eklenmiş olmalıdır.

### <a name="install-iot-edge-on-the-devices"></a>Cihazlara IoT Edge yüklemesi

Her iki cihazda da bu adımları izleyerek IoT Edge 'yi yükler.

1. Cihaz işletim sisteminizle eşleşen depo yapılandırmasını yükler.

   * **Ubuntu Server 16,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Ubuntu Server 18,04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Raspberry PI OS Esnetme**:

     ```bash
     curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
     ```

1. Oluşturulan listeyi sources. List. d dizinine kopyalayın.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

1. Microsoft GPG ortak anahtarını yükler.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```
   
1. Cihazınızdaki paket listelerini güncelleştirin.

   ```bash
   sudo apt-get update
   ```

1. Moby altyapısını yükler.

   ```bash
   sudo apt-get install moby-engine
   ```

1. Hsmlib ve IoT Edge Daemon 'ı yükler. Diğer Linux dağıtımlarına yönelik varlıkları görmek için [GitHub sürümünü ziyaret edin](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1). <!-- Update with proper image links on release -->

   ```bash
   curl -L https://github.com/Azure/azure-iotedge/releases/download/1.2.0-rc1/libiothsm-std_1.2.0_rc1-1-1_debian9_amd64.deb -o libiothsm-std.deb
   curl -L https://github.com/Azure/azure-iotedge/releases/download/1.2.0-rc1/iotedge_1.2.0_rc1-1_debian9_amd64.deb -o iotedge.deb
   sudo dpkg -i ./libiothsm-std.deb
   sudo dpkg -i ./iotedge.deb
   ```

### <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma

Her iki cihazlarınızda bu adımları izleyerek IoT Edge çalışma zamanını yapılandırın. Cihazlarınız için IoT Edge çalışma zamanının yapılandırılması, hepsi IoT Edge yapılandırma dosyasını düzenleyerek gerçekleştirilen dört adımdan oluşur:

1. Cihaz bağlantı dizesini yapılandırma dosyasına ekleyerek her bir cihazı el ile sağlayın.

1. Yapılandırma dosyasını cihaz CA sertifikasına, cihaz CA özel anahtarına ve kök CA sertifikasına girerek cihazınızın sertifikalarını ayarlamayı tamamlayın.

1. IoT Edge aracısını kullanarak sistemi önyükleyebilirsiniz.

1. **En üst katman** cihazınız için **hostname** parametresini güncelleştirin ve **alt katman** cihazlarınız için **hostname** parametresini ve **parent_hostname** parametresini güncelleştirin.

Cihazlarınızı yapılandırmak için bu adımları tamamlayıp IoT Edge hizmetini yeniden başlatın.

1. Her cihazda IoT Edge yapılandırma dosyasını açın.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

1. Dosyanın sağlama yapılandırmalarını bulun ve **bir bağlantı dizesi kullanarak el ile sağlama yapılandırmasının** açıklamasını kaldırın.

1. **Device_connection_string** değerini IoT Edge cihazınızdan bağlantı dizesiyle güncelleştirin. Diğer sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun. **Sağlama:** satırının önünde boşluk olmadığından ve iç içe yerleştirilmiş öğelerin iki boşlukla girintilendiğinden emin olun.

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
     dynamic_reprovisioning: false
   ```

   >[!TIP]
   >Pano içeriğini nano `Shift+Right Click` veya Press 'e yapıştırmak için `Shift+Insert` .

1. **Sertifikalar** bölümünü bulun. Önceki bölümde oluşturduğunuz ve IoT Edge cihaza taşınan sertifikaları işaret etmek için üç sertifika alanını açıklama ve güncelleştirme. Dosya URI 'SI yollarını sağlayın, bu biçimi alır `file:///<path>/<filename>` .

   ```yml
   certificates:
     device_ca_cert: "<File URI path to the device CA certificate unique to this device.>"
     device_ca_pk: "<File URI path to the device CA private key unique to this device.>"
     trusted_ca_certs: "<File URI path to the root CA certificate shared by all devices in the gateway hierarchy.>"
   ```

1. **Üst katman** cihazınız için **ana bilgisayar adı** parametresini bulun. Değeri, IoT Edge cihazın tam etki alanı adı (FQDN) veya IP adresi olacak şekilde güncelleştirin. Hiyerarşinizdeki cihazlar arasında tutarlı olarak seçtiğiniz değeri kullanın.

   ```yml
   hostname: <device fqdn or IP>
   ```

1. **Daha düşük katmanlardaki** IoT Edge cihazlar için, yapılandırma dosyasını üst aygıtın ana **bilgisayar adı** alanındaki ile eşleşen FQDN veya IP 'nin IP 'sine işaret etmek üzere güncelleştirin. **Üst katmandaki** IoT Edge cihazlar için, bu parametreyi açıklama olarak bırakın.

   ```yml
   parent_hostname: <parent device fqdn or IP>
   ```

   > [!NOTE]
   > Birden fazla alt katman içeren Hiyerarşiler için, *parent_hostname* alanını hemen yukarıdaki katmanda bulunan cihaz FQDN 'siyle güncelleştirin.

1. **Üst katman** cihazınız için **Aracı** YAML bölümünü bulun ve görüntü değerini IoT Edge aracısının doğru sürümüne güncelleştirin. Bu durumda, en üst katmanın IoT Edge aracısını IoT Edge aracı görüntüsünün genel önizleme sürümü ile Azure Container Registry kullanıma sunacağız.

   ```yml
   agent:
     name: "edgeAgent"
     type: "docker"
     env: {}
     config:
       image: "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2"
       auth: {}
   ```

1. **Alt katmanlarda** IoT Edge cihazlar için, görüntü değerinin etki alanı adını, üst cihazın FQDN 'SINI veya IP 'sini ve ardından API proxy bağlantı noktası 8000 ' ü gösterecek şekilde güncelleştirin. Sonraki bölümde API proxy modülünü ekleyeceksiniz.

   ```yml
   agent:
     name: "edgeAgent"
     type: "docker"
     env: {}
     config:
       image: "<parent_device_fqdn_or_ip>:8000/azureiotedge-agent:1.2.0-rc2"
       auth: {}
   ```

1. Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

1. Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, arka plan programını yeniden başlatın:

   ```bash
   sudo systemctl restart iotedge
   ```

## <a name="deploy-modules-to-the-top-layer-device"></a>Modülleri üst katman cihazına dağıt

IoT Edge çalışma zamanının ve dağıtım iş yüklerinin yapılandırılmasını tamamlamaya yönelik kalan adımlar, Azure portal veya Azure CLı aracılığıyla buluttan yapılır.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://ms.portal.azure.com/):

1. IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. Cihazlar listesinde **en üst katman** sınır CIHAZıNıZıN cihaz kimliğine tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

1. Dişli simgesinin yanındaki **çalışma zamanı ayarları**' nı seçin.

1. **Edge hub**'ı altında, görüntü alanına girin `mcr.microsoft.com/azureiotedge-hub:1.2.0-rc2` .

   ![Edge hub görüntüsünü düzenleme](./media/tutorial-nested-iot-edge/edge-hub-image.png)

1. Aşağıdaki ortam değişkenlerini Edge hub modülünüzü ekleyin:

    | Ad | Değer |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

   ![Edge hub 'ının ortam değişkenlerini düzenleme](./media/tutorial-nested-iot-edge/edge-hub-environment-variables.png)

1. **Edge Aracısı** altında, görüntü alanına girin `mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2` . **Kaydet**’i seçin.

1. Docker kayıt defteri modülünü en üst katman cihazınıza ekleyin. **+ Ekle** ' yi seçin ve açılan listeden **IoT Edge modülü** ' nü seçin. `registry`Docker kayıt defteri modülünüzün adını sağlayın ve `registry:latest` görüntü URI 'si için girin. Ardından, ' den kapsayıcı görüntülerini indirmek ve kayıt defteri 'nde bu görüntüleri çalıştırmak için Microsoft Container Registry 'de yerel kayıt defteri modülünüzü işaret etmek üzere ortam değişkenleri ekleyin ve seçenekler oluşturun: 5000.

1. Ortam değişkenleri sekmesinde, aşağıdaki ortam değişkeni adı-değer çiftini girin:

    | Ad | Değer |
    | - | - |
    | `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

1. Kapsayıcı oluşturma seçenekleri sekmesinde aşağıdaki JSON 'u girin:

   ```json
   {
    "HostConfig": {
        "PortBindings": {
            "5000/tcp": [
                {
                    "HostPort": "5000"
                }
            ]
         }
      }
   }
   ```

1. Sonra, API proxy modülünü en üst katman cihazınıza ekleyin. **+ Ekle** ' yi seçin ve açılan listeden **Market modülü** ' nü seçin. Modülünü arayın `IoT Edge API Proxy` ve seçin. IoT Edge API proxy, 8000 bağlantı noktasını kullanır ve `registry` Varsayılan olarak bağlantı noktası 5000 ' de adlı kayıt defteri modülünüzü kullanacak şekilde yapılandırılmıştır.

1. Dağıtımı gerçekleştirmek için **gözden geçir + oluştur**' u ve ardından **Oluştur** ' u seçin. İnternet erişimi olan en üst katman cihazınızın IoT Edge çalışma zamanı, IoT Edge hub ve IoT Edge Aracısı için **genel önizleme** yapılandırmalarını çeker ve çalıştırır.

   ![Edge hub, Edge Aracısı, kayıt modülü ve API ara sunucu modülünü içeren dağıtımı tamamen yapın](./media/tutorial-nested-iot-edge/complete-top-layer-deployment.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/), dosyasında bir deployment.jsoluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   code deploymentTopLayer.json
   ```

1. Aşağıdaki JSON içeriğini deployment.jsüzerine kopyalayın, kaydedin ve kapatın.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "dockerContainerRegistry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "https://mcr.microsoft.com"
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       },
                       "IoTEdgeAPIProxy": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-api-proxy",
                               "createOptions": "{\"HostConfig\": {\"PortBindings\": {\"8000/tcp\": [{\"HostPort\":\"8000\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "NGINX_DEFAULT_PORT": {
                                   "value": "8000"
                               },
                               "DOCKER_REQUEST_ROUTE_ADDRESS": {
                                   "value": "registry:5000"
                               },
                               "BLOB_UPLOAD_ROUTE_ADDRESS": {
                                   "value": "AzureBlobStorageonIoTEdge:11002"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25",
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2.0-rc2",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. En üst katman sınır cihazınıza bir dağıtım oluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   az iot edge set-modules --device-id <top_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentTopLayer.json
   ```

---

## <a name="deploy-modules-to-the-lower-layer-device"></a>Modülleri alt katman cihazına dağıt

Buluttan iş yüklerini **alt katman** cihazlarınıza dağıtmak için hem Azure Portal hem de Azure CLI kullanabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://ms.portal.azure.com/):

1. IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. IoT Edge cihazlar listesinde alt katman cihazınızın cihaz KIMLIĞINE tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

1. Dişli simgesinin yanındaki **çalışma zamanı ayarları**' nı seçin.

1. **Edge hub**'ı altında, görüntü alanına girin `$upstream:8000/azureiotedge-hub:1.2.0-rc2` .

1. Aşağıdaki ortam değişkenlerini Edge hub modülünüzü ekleyin:

    | Ad | Değer |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

1. **Edge Aracısı** altında, görüntü alanına girin `$upstream:8000/azureiotedge-agent:1.2.0-rc2` . **Kaydet**’i seçin.

1. Sıcaklık algılayıcısı modülünü ekleyin. **+ Ekle** ' yi seçin ve açılan listeden **Market modülü** ' nü seçin. Modülünü arayın `Simulated Temperature Sensor` ve seçin.

1. **IoT Edge modüller** altında, `Simulated Temperature Sensor` az önce eklediğiniz modülü seçin ve görüntü URI 'sini öğesine işaret etmek için güncelleştirin `$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0` .

1. Dağıtımı tamamladıktan sonra **Kaydet**, **gözden geçir** ve **Oluştur** ' u seçin.

   ![Edge hub, Edge Aracısı ve sanal sıcaklık algılayıcısı içeren dağıtımı tamamen yapın](./media/tutorial-nested-iot-edge/complete-lower-layer-deployment.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/), dosyasında bir deployment.jsoluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   code deploymentLowerLayer.json
   ```

1. Aşağıdaki JSON içeriğini deployment.jsüzerine kopyalayın, kaydedin ve kapatın.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "simulatedTemperatureSensor": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0",
                               "createOptions": ""
                           },
                           "type": "docker",
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25",
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-agent:1.2.0-rc2",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-hub:1.2.0-rc2",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Alt katman uç cihazınıza bir küme modülleri dağıtımı oluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   az iot edge set-modules --device-id <lower_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentLowerLayer.json

---

Notice that the image URI that we used for the simulated temperature sensor module pointed to `$upstream:8000` instead of to a container registry. We configured this device to not have direct connections to the cloud, because it's in a lower layer. To pull container images, this device requests the image from its parent device instead. At the top layer, the API proxy module routes this container request to the registry module, which handles the image pull.

On the device details page for your lower layer IoT Edge device, you should now see the temperature sensor module listed along the system modules as **Specified in deployment**. It may take a few minutes for the device to receive its new deployment, request the container image, and start the module. Refresh the page until you see the temperature sensor module listed as **Reported by device**.

## IotEdge check

Run the `iotedge check` command to verify the configuration and to troubleshoot errors.

You can run `iotedge check` in a nested hierarchy, even if the child machines don't have direct internet access.

When you run `iotedge check` from the lower layer, the program tries to pull the image from the parent through port 443.

In this tutorial, we use port 8000, so we need to specify it:

```bash
sudo iotedge check --diagnostics-image-name <parent_device_fqdn_or_ip>:8000/azureiotedge-diagnostics:1.2.0-rc2
```
   
`azureiotedge-diagnostics`Değer, kayıt defteri modülüyle bağlantılı kapsayıcı kayıt defterinden çekilir. Bu öğretici, varsayılan olarak şu şekilde ayarlanmıştır https://mcr.microsoft.com:

| Ad | Değer |
| - | - |
| `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

Özel bir kapsayıcı kayıt defteri kullanıyorsanız, tüm görüntülerin (örneğin, ıotedgeapiproxy, edgeAgent, edgeHub ve Tanılamalar) kapsayıcı kayıt defterinde bulunduğundan emin olun.    
    
## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Gönderdiğiniz **sanal sıcaklık algılayıcı** modülü örnek ortam verileri oluşturur. Çevresel sıcaklık ve nem, makine sıcaklığı ve basınç ve zaman damgası içeren iletileri gönderir.

[Visual Studio Code Için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak IoT Hub 'ınıza gelen iletileri izleyebilirsiniz.

Bu iletileri ayrıca [Azure Cloud Shell](https://shell.azure.com/)aracılığıyla da görüntüleyebilirsiniz:

   ```azurecli-interactive
   az iot hub monitor-events -n <iothub_name> -d <lower-layer-device-name>
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ücretlerden kaçınmak için bu makalede oluşturduğunuz yerel konfigürasyonları ve Azure kaynaklarını silebilirsiniz.

Kaynakları silmek için:

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

2. IoT Edge test kaynaklarınızı içeren kaynak grubunun adını seçin. 

3. Kaynak grubunuzda bulunan kaynak listesini gözden geçirin. Tümünü silmek isterseniz **Kaynak grubunu sil**'i seçebilirsiniz. Kaynakların yalnızca bazılarını silmek istiyorsanız tek tek tıklayarak silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, iki IoT Edge cihazı ağ geçidi olarak yapılandırdığınıza ve bir tane diğerinin üst aygıtı olarak ayarlayaöğreneceksiniz. Daha sonra, bir ağ geçidi üzerinden bir kapsayıcı görüntüsünü alt cihaza çekiniz.

Azure IoT Edge sisteminin işletmeniz için oluşturabileceği ek çözümleri görmek için diğer öğreticilere geçin.

> [!div class="nextstepaction"]
> [Bir Azure Machine Learning modelini modül olarak dağıtma](tutorial-deploy-machine-learning.md)
