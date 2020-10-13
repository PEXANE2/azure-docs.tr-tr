---
title: Simetrik Anahtarlarla Sağlama-Azure IoT Edge | Microsoft Docs
description: Yükleme sonrasında, bağlantı dizesini kullanarak simetrik anahtarlarla IoT Edge bir cihaz sağlayın ve IoT Hub kimlik doğrulamasını yapın
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: kgremban
ms.openlocfilehash: 9e288bcbebe4118bfc8cfa7cff46c79d7075555a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979885"
---
# <a name="set-up-an-azure-iot-edge-device-with-symmetric-key-authentication"></a>Simetrik anahtar kimlik doğrulaması ile Azure IoT Edge cihazı ayarlama

Bu makalede, IoT Hub yeni bir IoT Edge cihazı kaydetme ve cihazı simetrik anahtarlarla kimlik doğrulaması yapacak şekilde yapılandırma adımları sağlanmaktadır.

Bu makaledeki adımlarda, her cihazı IoT Hub 'ına el ile bağlayabileceğiniz el ile sağlama adlı bir süreç gösterilmektedir. Alternatif, sağlamanız gereken birçok cihazınız olduğunda faydalı olan IoT Hub cihaz sağlama hizmeti kullanılarak otomatik sağlanmasıdır.

<!--TODO: Add auto-provision info/links-->

El ile sağlama için, IoT Edge cihazların kimliğini doğrulamak için iki seçeneğiniz vardır:

* **Simetrik anahtar**: IoT Hub yeni bir cihaz kimliği oluşturduğunuzda, hizmet iki anahtar oluşturur. Bir cihazdan anahtardan birini yerleştirebilirsiniz ve kimlik doğrulanırken IoT Hub anahtarı gösterir.

  Bu kimlik doğrulama yöntemi kullanmaya başlamak için daha hızlıdır, ancak güvenli değildir.

* **X. 509.440 otomatik imzalı**: iki X. 509.440 kimlik sertifikası oluşturup cihaza yerleştirebilirsiniz. IoT Hub yeni bir cihaz kimliği oluşturduğunuzda, her iki sertifika için de parmak izleri sağlarsınız. Cihaz IoT Hub kimlik doğrulaması yaparken, sertifikalarını gösterir ve IoT Hub parmak izleriyle eşleştiğini doğrulayabilirler.

  Bu kimlik doğrulama yöntemi daha güvenlidir ve üretim senaryoları için önerilir.

Bu makalede, simetrik anahtar kimlik doğrulamasıyla kayıt ve sağlama sürecinde adım adım gösterilmektedir. X. 509.952 sertifikalarıyla bir cihaz ayarlamayı öğrenmek isterseniz, bkz. [x. 509.440 sertifikası kimlik doğrulaması ile bir Azure IoT Edge cihazı ayarlama](how-to-manual-provision-x509.md).

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemeden önce, üzerinde IoT Edge çalışma zamanı yüklü bir cihazınız olmalıdır. Aksi takdirde, [Azure IoT Edge çalışma zamanını yükleme veya kaldırma](how-to-install-iot-edge.md)bölümündeki adımları izleyin.

## <a name="register-a-new-device"></a>Yeni bir cihaz Kaydet

Bir IoT Hub bağlanan her cihazın, buluttan cihaza veya cihazdan buluta iletişimleri izlemek için kullanılan bir cihaz KIMLIĞI vardır. Bir cihazı, IoT Hub ana bilgisayar adı, cihaz KIMLIĞI ve cihazın IoT Hub kimlik doğrulaması için kullandığı bilgileri içeren bağlantı bilgileriyle yapılandırırsınız.

Simetrik anahtar kimlik doğrulaması için, bu bilgiler IoT Hub alabileceğiniz bir *bağlantı dizesinde* toplanır ve sonra IoT Edge cihazınıza yerleştirebilirsiniz.

IoT Hub yeni bir IoT Edge cihazı kaydetmek ve tercihinize bağlı olarak bağlantı dizesini almak için birkaç araç kullanabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="prerequisites-for-the-azure-portal"></a>Azure portal önkoşulları

Azure aboneliğinizde ücretsiz veya standart bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Azure portal IoT Edge cihaz oluşturma

Azure portal IoT Hub, IoT Edge cihazlar kenar etkin olmayan ıOT cihazlarından ayrı olarak oluşturulur ve yönetilir.

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.

1. Sol bölmede, menüden **IoT Edge** ' i seçin ve ardından **IoT Edge cihaz ekle**' yi seçin.

   ![Azure portal IoT Edge bir cihaz ekleyin](./media/how-to-manual-provision-symmetric-key/portal-add-iot-edge-device.png)

1. **Cihaz oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

   * Açıklayıcı bir cihaz KIMLIĞI oluşturun.
   * Kimlik doğrulama türü olarak **simetrik anahtar** ' ı seçin.
   * Kimlik doğrulama anahtarlarını otomatik oluşturmak ve yeni cihazı hub 'ınıza bağlamak için varsayılan ayarları kullanın.

1. **Kaydet**'i seçin.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Azure portal IoT Edge cihazları görüntüleme

IoT Hub 'ınıza bağlanan tüm Edge özellikli cihazlar **IoT Edge** sayfasında listelenir.

![IoT Hub 'ınızdaki tüm IoT Edge cihazları görüntülemek için Azure portal kullanın](./media/how-to-manual-provision-symmetric-key/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Azure portal bağlantı dizesini alma

Cihazınızı ayarlamaya hazırsanız, fiziksel cihazınızı IoT Hub 'ındaki kimliğiyle bağlayan bağlantı dizesine ihtiyacınız vardır.

1. Portaldaki **IoT Edge** sayfasında, IoT Edge cihazları LISTESINDEN cihaz kimliği ' ne tıklayın.
2. **Birincil bağlantı dizesinin** veya **İkincil bağlantı dizesinin**değerini kopyalayın.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

### <a name="prerequisites-for-visual-studio-code"></a>Visual Studio Code önkoşulları

* Azure aboneliğinizde ücretsiz veya standart bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md)
* [Visual Studio Code](https://code.visualstudio.com/)
* Visual Studio Code için [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

### <a name="sign-in-to-access-your-iot-hub"></a>IoT Hub 'ınıza erişmek için oturum açın

IoT Hub işlemleri gerçekleştirmek için Azure IoT uzantılarını Visual Studio Code kullanabilirsiniz. Bu işlemlerin çalışması için Azure hesabınızda oturum açmanız ve IoT Hub seçmeniz gerekir.

1. Visual Studio Code ' de **Gezgin** görünümünü açın.
1. Gezgin 'in alt kısmındaki **Azure IoT Hub** bölümünü genişletin.

   ![Azure IoT Hub cihazları bölümünü Genişlet](./media/how-to-manual-provision-symmetric-key/azure-iot-hub-devices.png)

1. **Azure IoT Hub** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta görmüyorsanız, üzerine tıklayın veya üstbilginin üzerine gelin.
1. **IoT Hub Seç ' i**seçin.
1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.
1. Azure aboneliğinizi seçin.
1. IoT Hub 'ınızı seçin.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Visual Studio Code ile IoT Edge cihaz oluşturma

1. VS Code Gezgini ' nde, **Azure IoT Hub cihazları** bölümünü genişletin.
1. **Azure IoT Hub Devices** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta görmüyorsanız, üzerine tıklayın veya üstbilginin üzerine gelin.
1. **IoT Edge cihaz oluştur**' u seçin.
1. Açılan metin kutusunda, cihazınıza bir KIMLIK verin.

Çıkış ekranında, komutun sonucunu görürsünüz. Belirttiğiniz **DeviceID** 'yi ve fiziksel cihazınızı IoT Hub 'ınıza bağlamak Için kullanabileceğiniz **ConnectionString** öğesini içeren cihaz bilgileri yazdırılır.

Çıkış ekranında, komutun sonucunu görürsünüz. Belirttiğiniz **DeviceID** 'yi ve fiziksel cihazınızı IoT Hub 'ınıza bağlamak Için kullanabileceğiniz **ConnectionString** öğesini içeren cihaz bilgileri yazdırılır.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Visual Studio Code olan IoT Edge cihazları görüntüleme

IoT Hub 'ınıza bağlanan tüm cihazlar, Visual Studio Code Gezgini 'nin **Azure IoT Hub** bölümünde listelenmiştir. IoT Edge cihazlar, farklı bir simgeyle uç olmayan cihazlardan ayırt edilemez ve **$edgeAgent** ve **$edgeHub** modüllerinin her bir IoT Edge cihazına dağıtılmasıdır.

![IoT Hub 'ınızdaki tüm IoT Edge cihazları görüntülemek için VS Code kullanma](./media/how-to-manual-provision-symmetric-key/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Bağlantı dizesini Visual Studio Code alma

Cihazınızı ayarlamaya hazırsanız, fiziksel cihazınızı IoT Hub 'ındaki kimliğiyle bağlayan bağlantı dizesine ihtiyacınız vardır.

1. **Azure IoT Hub** bölümünde cihazınızın kimliğine sağ tıklayın.
1. **Cihaz bağlantı dizesini Kopyala**' yı seçin.

   Bağlantı dizesi panonuza kopyalanır.

Ayrıca, çıkış penceresindeki bağlantı dizesi dahil tüm cihaz bilgilerini görmek için sağ tıklama menüsünden **cihaz bilgilerini al** ' ı da seçebilirsiniz.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="prerequisites-for-the-azure-cli"></a>Azure CLı önkoşulları

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-using-cli.md) .
* Ortamınızdaki [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) . Azure CLı sürümünüz en azından 2.0.70 veya daha yeni olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihaz oluşturma

IoT Hub 'ınızda yeni bir cihaz kimliği oluşturmak için [az IoT Hub Device-Identity Create](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-create) komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametre içerir:

* `--device-id` ya da `-d` : IoT Hub 'ınız için benzersiz olan açıklayıcı bir ad sağlayın.
* `hub-name` ya da `-n` : IoT Hub 'ınızın adını sağlayın.
* `--edge-enabled` veya `--ee` : cihazın IoT Edge bir cihaz olduğunu bildirin.

   ![az IoT Hub cihazı-kimlik oluşturma çıkışı](./media/how-to-manual-provision-symmetric-key/create-edge-device-cli.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihazları görüntüleme

IoT Hub 'ınızdaki tüm cihazları görüntülemek için [az IoT Hub Device-Identity List](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-list) komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

IoT Edge cihaz olarak kaydedilmiş tüm cihazlarda özellik **özellikleri olur. ıotedge** **doğru**olarak ayarlanır.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Azure CLı ile bağlantı dizesini alma

Cihazınızı ayarlamaya hazırsanız, fiziksel cihazınızı IoT Hub 'ındaki kimliğiyle bağlayan bağlantı dizesine ihtiyacınız vardır. Tek bir cihaza yönelik bağlantı dizesini döndürmek için [az IoT Hub cihazı-Identity Show-Connection-String](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-show-connection-string) komutunu kullanın:

   ```azurecli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

`device-id`Parametresinin değeri büyük/küçük harfe duyarlıdır. Bağlantı dizesinin etrafında tırnak işaretlerini kopyalamayın.

---

## <a name="provision-an-iot-edge-device"></a>IoT Edge cihazı hazırlama

IoT Edge cihaz IoT Hub bir kimliğe ve kimlik doğrulaması için kullanabileceği bir bağlantı dizesine sahip olduktan sonra, bu bilgilerle cihazın kendisini sağlamanız gerekir.

Bir Linux cihazında, bir config. YAML dosyasını düzenleyerek bağlantı dizesini sağlarsınız. Bir Windows cihazında, bir PowerShell betiği çalıştırarak bağlantı dizesini sağlarsınız.

# <a name="linux"></a>[Linux](#tab/linux)

IoT Edge cihazda yapılandırma dosyasını açın.

```bash
sudo nano /etc/iotedge/config.yaml
```

Dosyanın sağlama yapılandırmalarını bulun ve **bir bağlantı dizesi kullanarak el ile sağlama yapılandırmasının** açıklamasını kaldırın. 

**Device_connection_string** değerini IoT Edge cihazınızdan bağlantı dizesiyle güncelleştirin. Diğer sağlama bölümlerinin açıklama olarak belirlendiğinden emin olun. **Sağlama:** satırının önünde boşluk olmadığından ve iç içe yerleştirilmiş öğelerin iki boşlukla girintilendiğinden emin olun.

```yml
# Manual provisioning configuration using a connection string
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  dynamic_reprovisioning: false
```

Pano içeriğini nano `Shift+Right Click` veya Press 'e yapıştırmak için `Shift+Insert` .

Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, arka plan programını yeniden başlatın:

```bash
sudo systemctl restart iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

1. IoT Edge cihazda, PowerShell 'i yönetici olarak çalıştırın.

2. Makinenizde IoT Edge çalışma zamanını yapılandırmak için [Initialize-ıotedge](reference-windows-scripts.md#initialize-iotedge) komutunu kullanın. Komut, Windows kapsayıcıları ile el ile sağlamayı varsayılan olarak belirler.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualConnectionString -ContainerOs Windows
   ```

   * Linux kapsayıcıları kullanıyorsanız, `-ContainerOs` bayrağına parametresini ekleyin. Daha önce çalıştırdığınız komutla seçtiğiniz kapsayıcı seçeneği ile tutarlı olun `Deploy-IoTEdge` .

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ContainerOs Linux
      ```

   * IoTEdgeSecurityDaemon.ps1 betiğini çevrimdışı veya belirli bir sürüm yüklemesi için cihazınıza indirdiyseniz, betiğin yerel kopyasına başvurduğunuzdan emin olun.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. İstendiğinde, önceki bölümde aldığınız cihaz bağlantı dizesini belirtin. Cihaz bağlantı dizesi, fiziksel cihazı IoT Hub bir cihaz KIMLIĞIYLE ilişkilendirir ve kimlik doğrulama bilgilerini sağlar.

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemelidir: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

Bir cihazı el ile sağladığınızda, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Belirli bir edgeAgent kapsayıcı görüntüsü bildirin ve özel bir kayıt defterindeki kimlik bilgilerini sağlayın

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

---

[!INCLUDE [Verify and troubleshoot installation](../../includes/iot-edge-verify-troubleshoot-install.md)]

## <a name="next-steps"></a>Sonraki adımlar

Modülleri cihazınıza dağıtmayı öğrenmek için [IoT Edge modüllerini dağıtmaya](how-to-deploy-modules-portal.md) devam edin.
