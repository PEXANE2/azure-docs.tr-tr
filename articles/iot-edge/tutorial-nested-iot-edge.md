---
title: Öğretici-IoT Edge cihaz hiyerarşisi oluşturma-Azure IoT Edge
description: Bu öğreticide, ağ geçitlerini kullanarak IoT Edge cihazların hiyerarşik yapısını nasıl oluşturacağınız gösterilmektedir.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 2/26/2021
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: c1b30a1eafe9af92c1ef3f81773d213ccf96555c
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462040"
---
# <a name="tutorial-create-a-hierarchy-of-iot-edge-devices-preview"></a>Öğretici: IoT Edge cihazları hiyerarşisi oluşturma (Önizleme)

[!INCLUDE [iot-edge-version-202011](../../includes/iot-edge-version-202011.md)]

Hiyerarşik katmanlarda düzenlenmiş ağlarda Azure IoT Edge düğümleri dağıtın. Hiyerarşideki her katman, altındaki katmandaki cihazlardan gelen iletileri ve istekleri işleyen bir ağ geçidi aygıtıdır.

>[!NOTE]
>Bu özellik, Linux kapsayıcıları çalıştıran, genel önizlemede olan IoT Edge sürüm 1,2 gerektirir.

Yalnızca en üst katmanın buluta bağlantısı olması için bir cihaz hiyerarşisi oluşturabilir ve alt katmanlar yalnızca komşu Kuzey ve Güney katmanlarla iletişim kurabilir. Bu ağ katmanlama, [ISA-95 standardını](https://en.wikipedia.org/wiki/ANSI/ISA-95)izleyen en endüstriyel ağların temelidir.

Bu öğreticinin amacı, bir üretim ortamına benzetim yapan bir IoT Edge cihazları hiyerarşisi oluşturmaktır. Sonunda, kapsayıcı görüntülerini hiyerarşi aracılığıyla indirerek internet erişimi olmadan daha düşük bir katman cihazına [sanal sıcaklık algılayıcısı modülünü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.simulated-temperature-sensor) dağıtacaksınız.

Bu öğreticiyi başarmak için, bu öğreticide IoT Edge cihaz hiyerarşisi oluşturma, cihazlarınıza IoT Edge çalışma zamanı kapsayıcıları dağıtma ve cihazlarınızı yerel olarak yapılandırma işlemleri adım adım açıklanmıştır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * IoT Edge cihazların hiyerarşisinde ilişkiler oluşturun ve tanımlayın.
> * Hiyerarşinizdeki cihazlarda IoT Edge çalışma zamanını yapılandırın.
> * Cihaz hiyerarşiniz genelinde tutarlı sertifikalar yükler.
> * Hiyerarşinizdeki cihazlara iş yükleri ekleyin.
> * Daha düşük katman cihazlarınızdaki tek bir bağlantı noktası üzerinden HTTP trafiğini güvenli bir şekilde yönlendirmek için [IoT Edge API ara modülünü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.azureiotedge-api-proxy?tab=Overview) kullanın.

Bu öğreticide, aşağıdaki ağ katmanları tanımlanmıştır:

* **Üst katman**: bu katmandaki IoT Edge cihazlar doğrudan buluta bağlanabilir.

* **Alt katmanlar**: üst katmanın altındaki katmanlarda IoT Edge cihaz doğrudan buluta bağlanamaz. Veri göndermek ve almak için bir veya daha fazla ara IoT Edge cihazlarından birine gitmeleri gerekir.

Bu öğretici, aşağıda bulunan basitlik için iki cihaz hiyerarşisi kullanır. Bir cihaz, **üst düzey cihaz**, hiyerarşinin en üst katmanında, doğrudan buluta bağlanabilecek bir cihazı temsil eder. Bu cihaza **ana cihaz** da denir. Diğer cihaz, **alt katman cihazı**, hiyerarşinin alt katmanında bir cihazı temsil eder ve bu da doğrudan buluta bağlanamaz. Gerektiğinde üretim ortamınızı temsil etmek için ek daha düşük katman cihazları ekleyebilirsiniz. Alt katmanlarda yer alacak cihazlara de **alt cihaz** denir. Diğer alt katman cihazlarının yapılandırması, **alt katman cihazının** yapılandırmasını izler.

![İki cihaz içeren öğretici hiyerarşisinin yapısı: üst katman cihazı ve alt katman cihazı](./media/tutorial-nested-iot-edge/tutorial-hierarchy-diagram.png)

## <a name="prerequisites"></a>Önkoşullar

IoT Edge cihazlarının bir hiyerarşisini oluşturmak için şunlar gerekir:

* İnternet bağlantısı olan bir bilgisayar (Windows veya Linux).
* Geçerli aboneliği olan bir Azure hesabı. [Azure aboneliğiniz](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Azure 'da ücretsiz veya standart bir katman [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) .
* Azure CLı v 2.3.1, Azure IoT uzantısı v 0.10.6 veya üzeri yüklü. Bu öğretici [Azure Cloud Shell](../cloud-shell/overview.md)kullanır. Azure Cloud Shell hakkında bilgi sahibi değilseniz, [Ayrıntılar için hızlı başlangıç ' a bakın](./quickstart-linux.md#prerequisites).
  * Azure CLı modüllerinizin ve uzantılarının geçerli sürümlerini görmek için [az Version](/cli/azure/reference-index?#az_version)' ı çalıştırın.
* Hiyerarşinizdeki her cihaz için IoT Edge bir cihaz olarak yapılandırılacak bir Linux cihazı. Bu öğreticide iki cihaz kullanılmaktadır. Kullanılabilir cihazlar yoksa, aşağıdaki komutta yer tutucu metnini değiştirerek ve çalıştırarak hiyerarşinizdeki her bir cihaz için Azure sanal makineleri oluşturabilirsiniz:

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

>[!TIP]
>IoT Edge cihazların hiyerarşisini ayarlamaya yönelik otomatikleştirilmiş bir görünüm isterseniz, [endüstriyel IoT örneği için](https://aka.ms/iotedge-nested-sample)betikleştirilmiş Azure IoT Edge izleyebilirsiniz. Bu komut dosyalı senaryo, bir fabrika ortamının benzetimini yapmak için Azure sanal makinelerini önceden yapılandırılmış cihazlar olarak dağıtır.
>
>Örnek hiyerarşinin oluşturulmasına rağmen adım adım devam etmek isterseniz, aşağıdaki öğretici adımlarıyla devam edin.

## <a name="configure-your-iot-edge-device-hierarchy"></a>IoT Edge cihaz hiyerarşinizi yapılandırma

### <a name="create-a-hierarchy-of-iot-edge-devices"></a>IoT Edge cihaz hiyerarşisi oluşturma

IoT Edge cihazlar hiyerarşinizin katmanlarını yapar. Bu öğreticide, iki IoT Edge cihazın bir hiyerarşisi oluşturulur: **üst katman cihaz** ve alt **Katman cihazı**. Gerektiğinde ek alt aygıtlar oluşturabilirsiniz.

IoT Edge cihazlarınızı oluşturmak için Azure portal veya Azure CLı kullanabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portal](https://ms.portal.azure.com/)IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. **+ IoT Edge cihaz ekle**' yi seçin. Bu cihaz üst katman cihazı olacak, uygun bir benzersiz cihaz KIMLIĞI girin. **Kaydet**’i seçin.

1. **+ IoT Edge cihaz ekle** ' yi seçin. Bu cihaz daha düşük bir katman cihazı olacak, uygun bir benzersiz cihaz KIMLIĞI girin.

1. **Bir üst cihaz ayarla**' yı seçin, cihaz listesinden en üst katman cihazınızı seçin ve **Tamam**' ı seçin. **Kaydet**’i seçin.

   ![Alt katman cihazı için üst öğe ayarlanıyor](./media/tutorial-nested-iot-edge/set-parent-device.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/), hub 'ınızda bir IoT Edge cihaz oluşturmak için aşağıdaki komutu girin. Bu cihaz üst katman cihazı olacak ve uygun bir benzersiz cihaz KIMLIĞI girin:

   ```azurecli-interactive
   az iot hub device-identity create --device-id {top_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

   Başarılı bir cihaz oluşturulması, cihazın JSON yapılandırmasını çıktısını alacak.

   ![Başarılı bir cihaz oluşturma işleminin JSON çıkışı](./media/tutorial-nested-iot-edge/json-success-output.png)

1. Daha düşük bir katman IoT Edge cihazı oluşturmak için aşağıdaki komutu girin. Hiyerarşinizde daha fazla katman istiyorsanız birden fazla alt katman cihazı oluşturabilirsiniz. Her birine benzersiz cihaz kimlikleri sağladığınızdan emin olun.

   ```azurecli-interactive
   az iot hub device-identity create --device-id {lower_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

1. **En üst katman cihazından** ve **alt katman cihazındaki** her üst-alt ilişkiyi tanımlamak için aşağıdaki komutu girin. Hiyerarşinizdeki her bir alt katman aygıtı için bu komutu çalıştırdığınızdan emin olun.

   ```azurecli-interactive
   az iot hub device-identity parent set --device-id {lower_layer_device_id} --parent-device-id {top_layer_device_id} --hub-name {hub_name}
   ```

   Bu komutta açık çıkış yok.

---

Ardından, her bir IoT Edge cihazının bağlantı dizesini unutmayın. Bunlar daha sonra kullanılacaktır.

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

Yukarıdaki adımları doğru şekilde tamamladıysanız, üst-alt ilişkilerinizin Azure portal veya Azure CLı 'de doğru olup olmadığını denetlemek için aşağıdaki adımları kullanabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://ms.portal.azure.com/), IoT Hub **IoT Edge** bölümüne gidin.

1. Cihazlar listesinde **daha düşük bir katman cihazının** cihaz kimliğine tıklayın.

1. Cihazın ayrıntılar sayfasında, üst **Katman cihazının** **üst cihaz** alanının yanında listelenen kimliğini görmeniz gerekir.

   [Alt cihaz tarafından onaylanan üst cihaz](./media/tutorial-nested-iot-edge/lower-layer-device-parent.png)

Ayrıca, hiyerarşi ilişkinizin **üst katman cihazınız** aracılığıyla da alt öğesi oluşturabilirsiniz.

1. Cihazlar listesinde bir **üst katman cihazının** cihaz kimliğine tıklayın.

1. En üstteki **alt cihazları yönet** sekmesini seçin.

1. Cihaz listesi altında, **alt katman cihazınızı** görmeniz gerekir.

   [Üst cihaz tarafından onaylanan alt cihaz](./media/tutorial-nested-iot-edge/top-layer-device-child.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://shell.azure.com/) alt cihazın kabul edilen üst cihazının kimliğini alarak, alt cihazlarınızdan birinin üst cihazla ilişkilerini başarıyla kurdıklarından emin olabilirsiniz. Bu, yukarıda görüntülenenlerin bulunduğu üst cihazın JSON yapılandırmasını çıktı olarak dolacak:

   ```azurecli-interactive
   az iot hub device-identity parent show --device-id {lower_layer_device_id} --hub-name {hub_name}
   ```

Ayrıca, **üst katman cihazınızı** sorgulayarak hiyerarşi ilişkinizin da alt öğesi oluşturabilirsiniz.

1. Üst cihazın bildiği alt cihazları listeleyin:

    ```azurecli-interactive
    az iot hub device-identity children list --device-id {top_layer_device_id} --hub-name {hub_name}
    ```

---

Hiyerarşinizin doğru şekilde yapılandırılmış olması durumunda, devam etmeye hazırlanın.

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
   ./certGen.sh create_edge_device_ca_certificate "{top_layer_certificate_name}"
   ./certGen.sh create_edge_device_ca_certificate "{lower_layer_certificate_name}"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur, ancak her bir IoT Edge cihazında aşağıdaki sertifikayı ve anahtar çiftini kullanıyoruz ve yapılandırma dosyasında başvurulur:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Her cihazın kök CA sertifikasının bir kopyasına ve kendi cihaz CA sertifikasının ve özel anahtarının bir kopyasına ihtiyacı vardır. Sertifikaları her cihaza taşımak için bir USB sürücüsü veya [güvenli dosya kopyalama](https://www.ssh.com/ssh/scp/) kullanabilirsiniz.

1. Sertifikalar aktarıldıktan sonra her bir cihaz için kök CA 'yı yükler.

   ```bash
   sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```

   Bu komut, bir sertifikanın eklendiği çıkış olmalıdır `/etc/ssl/certs` .

   [Başarılı sertifika yükleme iletisi](./media/tutorial-nested-iot-edge/certificates-success-output.png)

Yukarıdaki adımları doğru şekilde tamamladıysanız, `/etc/ssl/certs` Bu dizine giderek ve yüklü sertifikalarınızı arayarak, sertifikalarınızın ' de yüklü olup olmadığını kontrol edebilirsiniz.

1. Şuraya gidin `/etc/ssl/certs` :

   ```bash
   cd /etc/ssl/certs
   ```

1. Yüklü sertifikaları ve için listeleyin `grep` `azure` :

   ```bash
   ll | grep azure
   ```

   Kök CA sertifikanız ile bağlantılı bir sertifika karması ve dizininizdeki kopyaya bağlı kök CA sertifikanız görmeniz gerekir `usr/local/share` .

   [Azure sertifikaları arama sonuçları](./media/tutorial-nested-iot-edge/certificate-list-results.png)

Sertifikalarınızın her cihaza yüklendiğinden emin olduktan sonra devam etmeye hazırlanın.

### <a name="install-iot-edge-on-the-devices"></a>Cihazlara IoT Edge yüklemesi

IoT Edge sürüm 1,2 çalışma zamanı görüntülerinin yüklenmesi, cihazlarınızın cihaz hiyerarşisi olarak çalışması için gereken özelliklere erişmesini sağlar.

IoT Edge yüklemek için uygun depo yapılandırmasını yüklemeniz, önkoşulları yüklemeniz ve gerekli sürüm varlıklarını yüklemeniz gerekir.

1. Cihaz işletim sisteminizle eşleşen depo yapılandırmasını yükler.

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

1. IoT Edge ve IoT kimlik hizmeti 'ni yükler.

   ```bash
   sudo apt-get install aziot-edge
   ```

Yukarıdaki adımları doğru tamamladıysanız, hiyerarşinizdeki her cihazda Azure IoT Edge yapılandırma dosyasını güncelleştirmenizi isteyen Azure IoT Edge başlık iletisini gördünüz `/etc/aziot/config.toml` . Öyleyse, devam etmeye hazırlanın.

### <a name="configure-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yapılandırma

Yapılandırma adımları, cihazlarınızın sağlanmasına ek olarak, daha önce oluşturduğunuz sertifikaları kullanarak hiyerarşinizdeki cihazlar arasında güvenilen iletişim kurar. Adımlar, hiyerarşinizin ağ yapısını oluşturmaya da başlar. En üst katman cihaz internet bağlantısını koruyacak ve bu, alt katman cihazları bu görüntülere erişmek için üst katman cihazından yönlendirilecektir.

IoT Edge çalışma zamanını yapılandırmak için yapılandırma dosyasının çeşitli bileşenlerini değiştirmeniz gerekir. Yapılandırmalar **üst katman cihazı** ve **daha düşük bir katman cihazı** arasında biraz farklılık gösterir, bu nedenle her adım için düzenlemekte olduğunuz cihaz yapılandırma dosyası en az olmalıdır. Cihazlarınız için IoT Edge çalışma zamanının yapılandırılması, hepsi IoT Edge yapılandırma dosyasını düzenleyerek gerçekleştirilen dört adımdan oluşur:

* Cihaz bağlantı dizesini yapılandırma dosyasına ekleyerek her bir cihazı el ile sağlayın.

* Yapılandırma dosyasını cihaz CA sertifikasına, cihaz CA özel anahtarına ve kök CA sertifikasına girerek cihazınızın sertifikalarını ayarlamayı tamamlayın.

* IoT Edge aracısını kullanarak sistemi önyükleyebilirsiniz.

* **En üst katman** cihazınız için **hostname** parametresini güncelleştirin ve **alt katman** cihazlarınız için **hostname** parametresini ve **parent_hostname** parametresini güncelleştirin.

Cihazlarınızı yapılandırmak için bu adımları tamamlayıp IoT Edge hizmetini yeniden başlatın.

>[!TIP]
>Bu yapılandırma dosyasında nano 'da gezinilirken, dosyadaki anahtar sözcükleri aramak için **CTRL + W** ' u kullanabilirsiniz.

1. Her cihazda, eklenen şablona göre bir yapılandırma dosyası oluşturun.

   ```bash
   sudo cp /etc/aziot/config.toml.edge.template /etc/aziot/config.toml

1. On each device, open the IoT Edge configuration file.

   ```bash
   sudo nano /etc/aziot/config.toml
   ```

1. **Üst katman** cihazınız Için **konak adı** bölümünü bulun. Parametrenin bulunduğu satırın açıklamasını kaldırın `hostname` ve değeri IoT Edge cihazın tam etki alanı adı (FQDN) veya IP adresi olacak şekilde güncelleştirin. Hiyerarşinizdeki cihazlar arasında tutarlı olarak seçtiğiniz değeri kullanın.

   ```toml
   hostname = "<device fqdn or IP>"
   ```

   >[!TIP]
   >Pano içeriğini nano `Shift+Right Click` veya Press 'e yapıştırmak için `Shift+Insert` .

1. **Alt katmanlarda** bulunan tüm IoT Edge cihazlar için **ana konak adı** bölümünü bulun. Parametreyi içeren satırın açıklamasını kaldırın `parent_hostname` ve değeri üst CIHAZıN FQDN 'sini veya IP 'sini işaret etmek için güncelleştirin. Üst cihazın **konak adı** alanına yerleştirdiğiniz tam değeri kullanın. **Üst katmandaki** IoT Edge cihaz için, bu parametreyi açıklama olarak bırakın.

   ```toml
   parent_hostname = "<parent device fqdn or IP>"
   ```

   > [!NOTE]
   > Birden fazla alt katman içeren Hiyerarşiler için, *parent_hostname* alanını hemen yukarıdaki katmanda bulunan cihaz FQDN 'siyle güncelleştirin.

1. Dosyanın **güven paketi sertifikası** bölümünü bulun. Parametresi ile satırın açıklamasını kaldırın `trust_bundle_cert` ve değeri, ağ geçidi hiyerarşisindeki tüm cihazlar tarafından paylaşılan kök CA sertifikası için dosya URI 'si yoluyla güncelleştirin.

   ```toml
   trust_bundle_cert = "<root CA certificate>"
   ```

1. Dosyanın **sağlama** bölümünü bulun. **Bağlantı dizesiyle el ile sağlama** için satırların açıklamasını kaldırın. Hiyerarşinizdeki her bir cihaz için **connection_string** değerini IoT Edge cihazınızdan bağlantı dizesiyle güncelleştirin.

   ```toml
   # Manual provisioning with connection string
   [provisioning]
   source = "manual"
   connection_string: "<Device connection string>"
   ```

1. **Varsayılan Edge Aracısı** bölümünü bulun.

   * **En üst katman** cihazınız Için, edgeAgent görüntü değerini Azure Container Registry modülün genel önizleme sürümüne güncelleştirin.
   
     ```toml
     [agent.config]
     image = "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4"
     ```

   * **Alt katmanlarda** bulunan IoT Edge her bir cihaz Için, edgeAgent görüntüsünü, üst cihazı işaret eden, ardından API proxy 'nin dinlediği bağlantı noktasını gösterecek şekilde güncelleştirin. Sonraki bölümde API proxy modülünü üst cihaza dağıtırsınız.
   
     ```toml
     [agent.config]
     image = "<parent hostname value>:8000/azureiotedge-agent:1.2.0-rc4"
     ```

1. **Edge CA sertifikası** bölümünü bulun. Bu bölümün ilk üç satırının açıklamasını kaldırın. Ardından, önceki bölümde oluşturduğunuz ve IoT Edge cihaza taşıdığınız cihaz CA sertifikası ve cihaz CA özel anahtar dosyalarını işaret etmek için iki parametreyi güncelleştirin. Biçimini kullanan dosya URI yollarını sağlayın ( `file:///<path>/<filename>` Örneğin,) `file:///certs/iot-edge-device-ca-top-layer-device.key.pem` .

   ```yml
   [edge_ca]
   cert = "<File URI path to the device full chain CA certificate unique to this device.>"
   pk = "<File URI path to the device CA private key unique to this device.>"
   ```

   >[!NOTE]
   >Alanı doldurmak için **tam zincir** CA sertifikası patika ve dosya adını kullandığınızdan emin olun `device_ca_cert` .

1. Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

1. Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, değişiklikleri uygulayın:

   ```bash
   sudo iotedge config apply
   ```

Devam etmeden önce, hiyerarşideki her bir cihazın yapılandırma dosyasını güncelleştirdiğinizden emin olun. Hiyerarşi yapısına bağlı olarak, bir **üst katman cihaz** ve bir veya daha fazla **alt katman cihaz** yapılandırdınız.

Yukarıdaki adımları doğru şekilde tamamladıysanız, cihazlarınızın doğru şekilde yapılandırıldığını kontrol edebilirsiniz.

1. Cihazlarınızda yapılandırma ve bağlantı denetimlerini çalıştırın:

   ```bash
   sudo iotedge check
   ```

**Üst katman cihazınızda**, birkaç iletme değerlendirmesi ve en az bir uyarı içeren bir çıktı görmeniz beklenir. İçin olan denetim, `latest security daemon` IoT Edge sürüm 1,2 genel önizlemede olduğundan, başka bir IoT Edge sürümünün en son kararlı sürüm olduğunu uyarır. Günlükler ilkeleri hakkında, ağınıza ve DNS ilkelerinize bağlı olarak ek uyarılar görebilirsiniz.

**Daha düşük bir katman cihazında**, en üst katman cihazına benzer bir çıktı görmeniz beklenir, ancak EdgeAgent modülünün yukarı akış üzerinden çekmeyeceğini belirten ek bir uyarıyla birlikte. Bu, IoT Edge API proxy modülü ve Docker Container Registry modülü olarak, alt katman cihazların görüntüleri üzerinden çekeceğini, **en üst katman cihazına** henüz dağıtılmadığını kabul edilebilir.

Örnek çıktısı `iotedge check` aşağıda gösterilmiştir:

[Örnek yapılandırma ve bağlantı sonuçları](./media/tutorial-nested-iot-edge/configuration-and-connectivity-check-results.png)

Yapılandırmaların her cihazda doğru olduğundan emin olduktan sonra devam etmeye hazırlanın.

## <a name="deploy-modules-to-the-top-layer-device"></a>Modülleri üst katman cihazına dağıt

Modüller dağıtımı ve IoT Edge çalışma zamanını cihazlarınıza tamamlamaya ve hiyerarşinizin yapısını daha da tanımlamaya yönelik olarak işlev yapar. IoT Edge API proxy modülü, alt katman cihazlarınızdan tek bir bağlantı noktası üzerinden HTTP trafiğini güvenli bir şekilde azaltır. Docker kayıt defteri modülü, alt katman cihazlarınızın yönlendirme görüntüsü tarafından erişebileceği bir Docker görüntüleri deposunun en üst katman cihaza çekmesine olanak tanır.

En üst katman cihazınıza modül dağıtmak için Azure portal veya Azure CLı kullanabilirsiniz.

>[!NOTE]
>IoT Edge çalışma zamanının ve dağıtım iş yüklerinin yapılandırılmasını tamamlamaya yönelik kalan adımlar IoT Edge cihazlarınızda yapılmaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://ms.portal.azure.com/):

1. IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. Cihazlar listesinde **en üst katman** sınır CIHAZıNıZıN cihaz kimliğine tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

1. Dişli simgesinin yanındaki **çalışma zamanı ayarları**' nı seçin.

1. **Edge hub**'ı altında, görüntü alanına girin `mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4` .

   ![Edge hub görüntüsünü düzenleme](./media/tutorial-nested-iot-edge/edge-hub-image.png)

1. Aşağıdaki ortam değişkenlerini Edge hub modülünüzü ekleyin:

    | Name | Değer |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

   ![Edge hub 'ının ortam değişkenlerini düzenleme](./media/tutorial-nested-iot-edge/edge-hub-environment-variables.png)

1. **Edge Aracısı** altında, görüntü alanına girin `mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4` . **Kaydet**’i seçin.

1. Docker kayıt defteri modülünü en üst katman cihazınıza ekleyin. **+ Ekle** ' yi seçin ve açılan listeden **IoT Edge modülü** ' nü seçin. `registry`Docker kayıt defteri modülünüzün adını sağlayın ve `registry:latest` görüntü URI 'si için girin. Ardından, ' den kapsayıcı görüntülerini indirmek ve kayıt defteri 'nde bu görüntüleri çalıştırmak için Microsoft Container Registry 'de yerel kayıt defteri modülünüzü işaret etmek üzere ortam değişkenleri ekleyin ve seçenekler oluşturun: 5000.

1. Ortam değişkenleri sekmesinde, aşağıdaki ortam değişkeni adı-değer çiftini girin:

    | Name | Değer |
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
                       "registry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "https://mcr.microsoft.com"
                               } 
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
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4",
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

Yukarıdaki adımları doğru bir şekilde tamamladıysanız, **en üst katman cihazınızın** dört modülü rapor etmesi gerekir: IoT Edge API proxy modülü, docker Container Registry modülü ve **dağıtım sırasında belirtilen** sistem modülleri. Cihazın yeni dağıtımını alması ve modülleri başlatması birkaç dakika sürebilir. **Cihaz tarafından bildirilen** sıcaklık algılayıcı modülünü görene kadar sayfayı yenileyin. Modüller cihaz tarafından bildirildikten sonra devam etmeye hazırlanın.

## <a name="deploy-modules-to-the-lower-layer-device"></a>Modülleri alt katman cihazına dağıt

Modüller ayrıca alt katman cihazlarınızın iş yükleri olarak da görev yapar. Sanal sıcaklık algılayıcı modülü, cihaz hiyerarşiniz aracılığıyla işlevsel bir veri akışı sağlamak için örnek telemetri verileri oluşturur.

Daha düşük katman cihazlarınıza modül dağıtmak için Azure portal veya Azure CLı kullanabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://ms.portal.azure.com/):

1. IoT Hub gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi** altında **IoT Edge**' yi seçin.

1. IoT Edge cihazlar listesinde alt katman cihazınızın cihaz KIMLIĞINE tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

1. Dişli simgesinin yanındaki **çalışma zamanı ayarları**' nı seçin.

1. **Edge hub**'ı altında, görüntü alanına girin `$upstream:8000/azureiotedge-hub:1.2.0-rc4` .

1. Aşağıdaki ortam değişkenlerini Edge hub modülünüzü ekleyin:

    | Name | Değer |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

1. **Edge Aracısı** altında, görüntü alanına girin `$upstream:8000/azureiotedge-agent:1.2.0-rc4` . **Kaydet**’i seçin.

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
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-hub:1.2.0-rc4",
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

If you completed the above steps correctly, your **lower layer device** should report three modules: the temperature sensor module and the system modules, as **Specified in Deployment**. It may take a few minutes for the device to receive its new deployment, request the container image, and start the module. Refresh the page until you see the temperature sensor module listed as **Reported by Device**. Once the modules are reported by the device, you are ready to continue.

## Troubleshooting

Run the `iotedge check` command to verify the configuration and to troubleshoot errors.

You can run `iotedge check` in a nested hierarchy, even if the child machines don't have direct internet access.

When you run `iotedge check` from the lower layer, the program tries to pull the image from the parent through port 443.

In this tutorial, we use port 8000, so we need to specify it:

```bash
sudo iotedge check --diagnostics-image-name <parent_device_fqdn_or_ip>:8000/azureiotedge-diagnostics:1.2.0-rc4
```

`azureiotedge-diagnostics`Değer, kayıt defteri modülüyle bağlantılı kapsayıcı kayıt defterinden çekilir. Bu öğretici, varsayılan olarak şu şekilde ayarlanmıştır https://mcr.microsoft.com:

| Name | Değer |
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
