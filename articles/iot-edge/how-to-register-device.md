---
title: Yeni bir Azure IoT Edge cihaz kaydetme | Microsoft Docs
description: Yeni bir IOT Edge cihazı kaydedin ve bağlantı dizesini almak için Azure CLI için IOT uzantısı kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/08/2020
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 32121681b14989f23e29c3701826b4494988c263
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772440"
---
# <a name="register-an-azure-iot-edge-device"></a>Azure IoT Edge cihazı kaydetme

IoT cihazlarınızı Azure IoT Edge ile kullanabilmeniz için, bunları IoT Hub 'ınıza kaydetmeniz gerekir. Bir cihaz kaydedildikten sonra, cihazınızı IoT Edge iş yükleri için ayarlamak üzere bir bağlantı dizesi alabilirsiniz.

Aşağıdaki araçlardan birini kullanarak kayıt seçeneğiniz vardır:

* Azure kaynaklarını oluşturmak, görüntülemek ve yönetmek için grafik kullanıcı arabirimini tercih ediyorsanız [bir cihazı Azure Portal kaydedin](#register-in-the-azure-portal) .
* Azure IoT kaynaklarını IoT çözümlerinizi geliştirdiğiniz yerde yönetmeyi tercih ediyorsanız [bir cihazı Visual Studio Code kaydedin](#register-with-visual-studio-code) .
* Azure kaynaklarını yönetmek için komut satırı araçlarını tercih ediyorsanız veya görevleri otomatik hale getirmeyi planlıyorsanız, [Azure CLI ile bir cihaz kaydettirin](#register-with-the-azure-cli) .

## <a name="register-in-the-azure-portal"></a>Azure portal kaydetme

Azure portal tüm kayıt görevlerini gerçekleştirebilirsiniz.

### <a name="prerequisites-for-the-azure-portal"></a>Azure portal önkoşulları

Azure aboneliğinizde ücretsiz veya standart bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Azure portal IoT Edge cihaz oluşturma

Azure portal IoT Hub, IoT Edge cihazlar kenar etkin olmayan ıOT cihazlarından ayrı olarak oluşturulur ve yönetilir.

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
2. Sol bölmede, menüden **IoT Edge** ' yi seçin.
3. **IoT Edge cihaz ekle**' yi seçin.
4. Açıklayıcı bir cihaz kimliği sağlayın. Kimlik doğrulama anahtarlarını otomatik oluşturmak ve yeni cihazı hub 'ınıza bağlamak için varsayılan ayarları kullanın.
5. **Kaydet**’i seçin.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Azure portal IoT Edge cihazları görüntüleme

IOT hub'ınıza bağlanan tüm edge özellikli cihazlar listelenir **IOT Edge** sayfası.

![IOT hub'ına tüm IOT Edge cihazları görüntüle](./media/how-to-register-device/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Azure portal bağlantı dizesini alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. Portaldaki **IoT Edge** sayfasında, IoT Edge cihazları LISTESINDEN cihaz kimliği ' ne tıklayın.
2. **Birincil bağlantı dizesinin** veya **İkincil bağlantı dizesinin**değerini kopyalayın.

## <a name="register-with-visual-studio-code"></a>Visual Studio Code Kaydet

VS Code'da çoğu işlemi gerçekleştirmek için birden çok yolu vardır. Bu makalede gezgin kullanılmaktadır, ancak adımları çalıştırmak için komut paletini de kullanabilirsiniz.

### <a name="prerequisites-for-visual-studio-code"></a>Visual Studio Code önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki
* [Visual Studio Code](https://code.visualstudio.com/)
* Visual Studio Code için [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

### <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

IoT Hub işlemleri gerçekleştirmek için Azure IoT uzantılarını Visual Studio Code kullanabilirsiniz. Bu işlemlerin çalışması için Azure hesabınızda oturum açmanız ve IoT Hub seçmeniz gerekir.

1. Visual Studio Code'da açmak **Gezgini** görünümü.
1. Gezgin 'in alt kısmındaki **Azure IoT Hub** bölümünü genişletin.

   ![Azure IOT Hub cihazları bölümü genişletin](./media/how-to-register-device/azure-iot-hub-devices.png)

1. **Azure IoT Hub** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin.
1. Seçin **IOT hub'ını seçin**.
1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.
1. Azure aboneliğinizi seçin.
1. IOT hub'ınızı seçin.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Visual Studio Code ile IoT Edge cihaz oluşturma

1. VS Code Gezgininde genişletin **Azure IOT Hub cihazları** bölümü.
1. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, tıklayın veya üst bilgisinin üzerinde gezdirin.
1. Seçin **IOT Edge cihazı oluşturma**.
1. Açılan metin kutusuna, cihaz kimliği verin.

Çıkış ekranında, komutun sonucuna ilişkin bakın. İçeren cihaz bilgileri yazdırılır **DeviceID** sağladığınız ve **connectionString** fiziksel Cihazınızı IOT hub'ınıza bağlanmak için kullanabilirsiniz.

Çıkış ekranında, komutun sonucuna ilişkin bakın. İçeren cihaz bilgileri yazdırılır **DeviceID** sağladığınız ve **connectionString** fiziksel Cihazınızı IOT hub'ınıza bağlanmak için kullanabilirsiniz.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Visual Studio Code olan IoT Edge cihazları görüntüleme

IoT Hub 'ınıza bağlanan tüm cihazlar, Visual Studio Code Gezgini 'nin **Azure IoT Hub** bölümünde listelenmiştir. IoT Edge cihazlar, farklı bir simgeyle uç olmayan cihazlardan ayırt edilemez ve **$edgeAgent** ve **$edgeHub** modüllerinin her bir IoT Edge cihazına dağıtılmasıdır.

![IOT hub'ına tüm IOT Edge cihazları görüntüle](./media/how-to-register-device/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Bağlantı dizesini Visual Studio Code alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. **Azure IoT Hub** bölümünde cihazınızın kimliğine sağ tıklayın.
1. Seçin **cihaz bağlantı dizesini kopyalayın**.

   Bağlantı dizesi panonuza kopyalanır.

Belirleyebilirsiniz **cihaz bilgi al** tüm cihaz bilgileri görmek için sağ tıklama menüsünden bağlantı dizesi dahil olmak üzere çıktı penceresinde.

## <a name="register-with-the-azure-cli"></a>Azure CLı ile kaydolun

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) , IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure IOT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenizi sağlar. Yeni IOT uzantısı, Azure CLI cihaz yönetimi ve tam IOT Edge özelliği gibi özelliklerle zenginleştirir.

### <a name="prerequisites-for-the-azure-cli"></a>Azure CLı önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ortamınızdaki. En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihaz oluşturma

IoT Hub 'ınızda yeni bir cihaz kimliği oluşturmak için [az IoT Hub Device-Identity Create](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) komutunu kullanın. Örneğin:

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametreleri içerir:

* **cihaz kimliği**: IOT hub'ınıza benzersiz bir açıklayıcı ad girin.
* **hub adı**: IOT hub'ınızın adını sağlayın.
* **Edge özellikli**: Bu parametre cihazın IOT Edge ile kullanılmak üzere olduğunu bildirir.

   ![Çıkış az IOT hub cihaz kimliği oluşturma](./media/how-to-register-device/Create-edge-device.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Azure CLı ile IoT Edge cihazları görüntüleme

IoT Hub 'ınızdaki tüm cihazları görüntülemek için [az IoT Hub Device-Identity List](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list) komutunu kullanın. Örneğin:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

IOT Edge cihazı özelliğine sahip şekilde kayıtlı herhangi bir CİHAZDAN **capabilities.iotEdge** kümesine **true**.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Azure CLı ile bağlantı dizesini alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır. Tek bir cihaza yönelik bağlantı dizesini döndürmek için [az IoT Hub cihazı-Identity Show-Connection-String](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) komutunu kullanın:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

`device-id` parametresinin değeri büyük/küçük harfe duyarlıdır. Bağlantı dizesi etrafındaki tırnak işaretlerini bulundurmanıza gerek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Hub 'ınıza kayıtlı bir cihaz kimliğiniz olduğuna göre, cihazlarınıza IoT Edge çalışma zamanını yüklemeye hazırsınız demektir. Çalışma zamanını cihazın işletim sistemine göre yükler:

* [Windows 'a Azure IoT Edge yüklemesi](how-to-install-iot-edge-windows.md)
* [Linux üzerinde Azure IoT Edge çalışma zamanını yükler](how-to-install-iot-edge-linux.md)
