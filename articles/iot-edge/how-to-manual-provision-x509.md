---
title: X. 509.440 sertifikalarıyla Sağlama-Azure IoT Edge | Microsoft Docs
description: Yükleme sonrasında cihaz kimlik sertifikalarına sahip bir IoT Edge cihaz sağlayın ve IoT Hub için kimlik doğrulaması yapın
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: kgremban
ms.openlocfilehash: b1aa12bd73772b5d6332a36d749ec4d7d10d4026
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048194"
---
# <a name="set-up-an-azure-iot-edge-device-with-x509-certificate-authentication"></a>X. 509.440 sertifikası kimlik doğrulaması ile bir Azure IoT Edge cihazı ayarlama

Bu makalede, IoT Hub yeni bir IoT Edge cihazı kaydetme ve cihazı X. 509.952 sertifikalarıyla kimlik doğrulaması yapacak şekilde yapılandırma adımları sağlanmaktadır.

Bu makaledeki adımlarda, her cihazı IoT Hub 'ına el ile bağlayabileceğiniz el ile sağlama adlı bir süreç gösterilmektedir. Alternatif, sağlamanız gereken birçok cihazınız olduğunda faydalı olan IoT Hub cihaz sağlama hizmeti kullanılarak otomatik sağlanmasıdır.

<!--TODO: Add auto-provision info/links-->

El ile sağlama için, IoT Edge cihazların kimliğini doğrulamak için iki seçeneğiniz vardır:

* **Simetrik anahtar**: IoT Hub yeni bir cihaz kimliği oluşturduğunuzda, hizmet iki anahtar oluşturur. Bir cihazdan anahtardan birini yerleştirebilirsiniz ve kimlik doğrulanırken IoT Hub anahtarı gösterir.

  Bu kimlik doğrulama yöntemi kullanmaya başlamak için daha hızlıdır, ancak güvenli değildir.

* **X. 509.440 otomatik imzalı**: iki X. 509.440 kimlik sertifikası oluşturup cihaza yerleştirebilirsiniz. IoT Hub yeni bir cihaz kimliği oluşturduğunuzda, her iki sertifika için de parmak izleri sağlarsınız. Cihaz IoT Hub kimlik doğrulaması yaparken, sertifikalarını gösterir ve IoT Hub parmak izleriyle eşleştiğini doğrulayabilirler.

  Bu kimlik doğrulama yöntemi daha güvenlidir ve üretim senaryoları için önerilir.

Bu makalede, X. 509.440 sertifika kimlik doğrulamasıyla kayıt ve sağlama sürecinde adım adım gösterilmektedir. Simetrik Anahtarlarla bir cihaz ayarlamayı öğrenmek isterseniz bkz. [simetrik anahtar kimlik doğrulamasıyla Azure IoT Edge cihazı ayarlama](how-to-manual-provision-symmetric-key.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları izlemeden önce, üzerinde IoT Edge çalışma zamanı yüklü bir cihazınız olmalıdır. Aksi takdirde, [Azure IoT Edge çalışma zamanını yükleme veya kaldırma](how-to-install-iot-edge.md)bölümündeki adımları izleyin.

X. 509.440 sertifikaları ile el ile sağlama, IoT Edge sürüm 1.0.10 veya daha yenisini gerektirir.

## <a name="create-certificates-and-thumbprints"></a>Sertifika ve parmak izleri oluşturma



<!-- TODO -->

## <a name="register-a-new-device"></a>Yeni bir cihaz Kaydet

Bir IoT Hub bağlanan her cihazın, buluttan cihaza veya cihazdan buluta iletişimleri izlemek için kullanılan bir cihaz KIMLIĞI vardır. Bir cihazı, IoT Hub ana bilgisayar adı, cihaz KIMLIĞI ve cihazın IoT Hub kimlik doğrulaması için kullandığı bilgileri içeren bağlantı bilgileriyle yapılandırırsınız.

X. 509.440 sertifikası kimlik doğrulaması için bu bilgiler, cihaz kimliği sertifikalarınızın içindeki *parmak izleri* biçiminde sağlanır. Bu parmak izlerinin cihaz kaydı sırasında IoT Hub, hizmetin bağlandığı sırada cihazı tanıyabilmesi için bu parmak izleri verilmiştir.

IoT Hub yeni bir IoT Edge cihazı kaydetmek ve sertifika parmak izlerini karşıya yüklemek için çeşitli araçları kullanabilirsiniz. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="prerequisites-for-the-azure-portal"></a>Azure portal önkoşulları

Azure aboneliğinizde ücretsiz veya standart bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Azure portal IoT Edge cihaz oluşturma

Azure portal IoT Hub, IoT Edge cihazlar kenar etkin olmayan ıOT cihazlarından ayrı olarak oluşturulur ve yönetilir.

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.

1. Sol bölmede, menüden **IoT Edge** ' i seçin ve ardından **IoT Edge cihaz ekle**' yi seçin.

   ![Azure portal IoT Edge bir cihaz ekleyin](./media/how-to-manual-provision-symmetric-key/portal-add-iot-edge-device.png)

1. **Cihaz oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

   * Açıklayıcı bir cihaz KIMLIĞI oluşturun. Sonraki bölümde kullanacağınız gibi, bu cihaz KIMLIĞINI bir yere unutmayın.
   * Kimlik doğrulama türü olarak **X. 509.440 otomatik olarak imzalanan** ' ı seçin.
   * Birincil ve ikincil kimlik sertifikası parmak izlerini sağlayın. Parmak izi değerleri, SHA-1 karmaları için 40 onaltılı karakterler veya SHA-256 karmaları için 64 onaltılık karakterdir.

1. **Kaydet**'i seçin.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Azure portal IoT Edge cihazları görüntüleme

IoT Hub 'ınıza bağlanan tüm Edge özellikli cihazlar **IoT Edge** sayfasında listelenir.

![IoT Hub 'ınızdaki tüm IoT Edge cihazları görüntüleme](./media/how-to-manual-provision-symmetric-key/portal-view-devices.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="prerequisites-for-the-azure-cli"></a>Azure CLı önkoşulları

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-using-cli.md) .
* Ortamınızdaki [Azure CLI](/cli/azure/install-azure-cli) . Azure CLı sürümünüz en azından 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihaz oluşturma

IoT Hub 'ınızda yeni bir cihaz kimliği oluşturmak için [az IoT Hub Device-Identity Create](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-create) komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled --auth-method x509_thumbprint --primary-thumbprint [SHA thumbprint] --secondary-thumbprint [SHA thumbprint]
   ```

Bu komut çeşitli parametreleri içerir:

* `--device-id` ya da `-d` : IoT Hub 'ınız için benzersiz olan açıklayıcı bir ad sağlayın. Sonraki bölümde kullanacağınız gibi, bu cihaz KIMLIĞINI bir yere unutmayın.
* `hub-name` ya da `-n` : IoT Hub 'ınızın adını sağlayın.
* `--edge-enabled` veya `--ee` : cihazın IoT Edge bir cihaz olduğunu bildirin.
* `--auth-method` veya `--am` : cihazın kullanacağı Yetkilendirme türünü bildirin. Bu durumda, X. 509.440 sertifikası parmak izlerini kullanıyoruz.
* `--primary-thumbprint` ya da `--ptp` : birincil anahtar olarak kullanılacak bir X. 509.440 sertifika parmak izi sağlayın.
* `--secondary-thumbprint` ya da `--stp` : ikincil anahtar olarak kullanılacak bir X. 509.440 sertifika parmak izi sağlayın.

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihazları görüntüleme

IoT Hub 'ınızdaki tüm cihazları görüntülemek için [az IoT Hub Device-Identity List](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-list) komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

`--edge-enabled` `--ee` Yalnızca IoT hub 'ınızdaki IoT Edge cihazları listelemek için bayrak ekleyin.

IoT Edge cihaz olarak kaydedilmiş tüm cihazlarda özellik **özellikleri olur. ıotedge** **doğru**olarak ayarlanır.

--- 

## <a name="configure-an-iot-edge-device"></a>IoT Edge cihazını yapılandırma

IoT Edge cihazın IoT Hub bir kimliği varsa, cihazı bulut kimliğiyle, kimlik sertifikaları ile yapılandırmanız gerekir.

Bir Linux cihazında, bu bilgileri bir config. YAML dosyasını düzenleyerek sağlarsınız. Bir Windows cihazında, bu bilgileri bir PowerShell betiği çalıştırarak sağlarsınız.

# <a name="linux"></a>[Linux](#tab/linux)

1. IoT Edge cihazda yapılandırma dosyasını açın.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

1. Dosyanın sağlama yapılandırması bölümünü bulun. 

1. **Bir bağlantı dizesi kullanarak el ile sağlama yapılandırmasını** Açıklama bölümüne koyun.

1. **X. 509.440 kimlik sertifikası bölümünü kullanarak el ile sağlama yapılandırmasının** açıklamasını kaldırın. **Sağlama:** satırının önünde boşluk olmadığından ve iç içe yerleştirilmiş öğelerin iki boşlukla girintilendiğinden emin olun.

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     authentication:
       method: "x509"
       iothub_hostname: "<REQUIRED IOTHUB HOSTNAME>"
       device_id: "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
       identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
       identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
     dynamic_reprovisioning: false
   ```

1. Aşağıdaki alanları güncelleştirin:

   * **iothub_hostname**: cihazın bağlanacağı IoT Hub 'ının ana bilgisayar adı. Örneğin, `{IoT hub name}.azure-devices.net`.
   * **device_id**: cihazı kaydettirdiğiniz sırada verdiğiniz kimlik.
   * **identity_cert**: cihazdaki bir kimlik sertifikası için URI. Örneğin, `file:///path/identity_certificate.pem`.
   * **identity_pk**: belirtilen kimlik sertifikası için özel anahtar dosyasının URI 'si. Örneğin, `file:///path/identity_key.pem`.

1. Dosyayı kaydedin ve kapatın.

   `CTRL + X`, `Y`, `Enter`

1. Yapılandırma dosyasına sağlama bilgilerini girdikten sonra, arka plan programını yeniden başlatın:

   ```bash
   sudo systemctl restart iotedge
   ```

# <a name="windows"></a>[Windows](#tab/windows)

1. IoT Edge cihazda, PowerShell 'i yönetici olarak çalıştırın.

2. Makinenizde IoT Edge çalışma zamanını yapılandırmak için [Initialize-ıotedge](reference-windows-scripts.md#initialize-iotedge) komutunu kullanın.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualX509
   ```

   * Linux kapsayıcıları kullanıyorsanız, `-ContainerOs` bayrağına parametresini ekleyin. Daha önce çalıştırdığınız komutla seçtiğiniz kapsayıcı seçeneği ile tutarlı olun `Deploy-IoTEdge` .

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ManualX509 -ContainerOs Linux
      ```

   * IoTEdgeSecurityDaemon.ps1 betiğini çevrimdışı veya belirli bir sürüm yüklemesi için cihazınıza indirdiyseniz, betiğin yerel kopyasına başvurduğunuzdan emin olun.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. İstendiğinde, aşağıdaki bilgileri sağlayın:

   * **Iothubhostname**: cihazın bağlanacağı IoT Hub 'ının ana bilgisayar adı. Örneğin, `{IoT hub name}.azure-devices.net`.
   * **DeviceID**: cihazı kaydettirdiğiniz sırada verdiğiniz kimlik.
   * **X509IdentityCertificate**: cihazdaki bir kimlik sertifikasının mutlak yolu. Örneğin, `C:\path\identity_certificate.pem`.
   * **X509IdentityPrivateKey**: belirtilen kimlik sertifikası için özel anahtar dosyasının mutlak yolu. Örneğin, `C:\path\identity_key.pem`.

Bir cihazı el ile sağladığınızda, aşağıdakileri de içeren ek parametreleri kullanarak işlemi değiştirebilirsiniz:

* Bir proxy sunucusundan gelen trafiği doğrudan gönder
* Belirli bir edgeAgent kapsayıcı görüntüsü bildirin ve özel bir kayıt defterindeki kimlik bilgilerini sağlayın

Bu ek parametreler hakkında daha fazla bilgi için bkz. [Windows üzerinde IoT Edge Için PowerShell betikleri](reference-windows-scripts.md).

---

[!INCLUDE [Verify and troubleshoot installation](../../includes/iot-edge-verify-troubleshoot-install.md)]

## <a name="next-steps"></a>Sonraki adımlar

Modülleri cihazınıza dağıtmayı öğrenmek için [IoT Edge modüllerini dağıtmaya](how-to-deploy-modules-portal.md) devam edin.