---
title: Yeni bir Azure IoT Edge aygıtı kaydedin | Microsoft Dokümanlar
description: Yeni bir IoT Edge aygıtı kaydetmek ve bağlantı dizesini almak için Azure CLI için IoT uzantısını kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/08/2020
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6fb776b4c1ff537401a23eb272526b3043fdb1e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235717"
---
# <a name="register-an-azure-iot-edge-device"></a>Azure IoT Edge aygıtını kaydetme

IoT aygıtlarınızı Azure IoT Edge ile kullanamadan önce bunları IoT hub'ınıza kaydettirmelisiniz. Bir aygıt kaydedildikten sonra, aygıtınızı IoT Edge iş yükleri için ayarlamak için bir bağlantı dizesi alabilirsiniz.

Aşağıdaki araçlardan birini kullanarak kayıt seçeneğiniz vardır:

* Azure kaynaklarını oluşturmak, görüntülemek ve yönetmek için grafik kullanıcı arabirimini tercih ediyorsanız, [bir aygıtı Azure portalına](#register-in-the-azure-portal) kaydedin.
* Azure IoT kaynaklarını IoT çözümlerinizi geliştirdiğiniz yerde yönetmeyi tercih ediyorsanız, Visual Studio Code ile [bir aygıt kaydedin.](#register-with-visual-studio-code)
* Azure kaynaklarını yönetmek için komut satırı araçlarını tercih ediyorsanız veya görevleri otomatikleştirmek [istiyorsanız, bir aygıtı Azure CLI'ye](#register-with-the-azure-cli) kaydedin.

## <a name="register-in-the-azure-portal"></a>Azure portalına kaydolun

Azure portalındaki tüm kayıt görevlerini gerçekleştirebilirsiniz.

### <a name="prerequisites-for-the-azure-portal"></a>Azure portalı için ön koşullar

Azure aboneliğinizde ücretsiz veya standart [bir IoT](../iot-hub/iot-hub-create-through-portal.md) hub'ı.

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Azure portalında bir IoT Edge aygıtı oluşturma

Azure portalındaki IoT Hub'ınızda, IoT Edge aygıtları oluşturulur ve kenar etkin olmayan IOT aygıtlarından ayrı olarak yönetilir.

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.
2. Sol bölmede menüden **IoT Edge'i** seçin.
3. **IoT Edge aygıtı ekle'yi**seçin.
4. Açıklayıcı bir aygıt kimliği sağlayın. Kimlik doğrulama anahtarlarını otomatik olarak oluşturmak ve yeni aygıtı hub'ınıza bağlamak için varsayılan ayarları kullanın.
5. **Kaydet'i**seçin.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Azure portalında IoT Edge aygıtlarını görüntüleme

IoT hub'ınıza bağlanan kenar özellikli tüm aygıtlar **IoT Edge** sayfasında listelenir.

![Tüm IoT Edge aygıtlarını IoT hub'ınızda görüntüleme](./media/how-to-register-device/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Azure portalındaki bağlantı dizesini alma

Cihazınızı kurmaya hazır olduğunuzda, fiziksel aygıtınızı IoT hub'ındaki kimliğiyle ilişkilendiren bağlantı dizesine ihtiyacınız vardır.

1. Portaldaki **IoT Edge** sayfasından, IoT Edge aygıtları listesindeki aygıt kimliğine tıklayın.
2. **Birincil Bağlantı Dizesi** veya **İkincil Bağlantı Dizesi**değerini kopyalayın.

## <a name="register-with-visual-studio-code"></a>Visual Studio Kodu ile Kaydol

VS Code'da çoğu işlemi gerçekleştirmenin birden çok yolu vardır. Bu makalede Explorer kullanır, ancak adımları çalıştırmak için Komut Paleti de kullanabilirsiniz.

### <a name="prerequisites-for-visual-studio-code"></a>Visual Studio Code için Ön Koşullar

* Azure aboneliğinizde bir [IoT hub'ı](../iot-hub/iot-hub-create-through-portal.md)
* [Visual Studio Code](https://code.visualstudio.com/)
* Visual Studio Kodu için [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

### <a name="sign-in-to-access-your-iot-hub"></a>IoT hub'ınıza erişmek için oturum açın

IoT Hub'ınızla işlemleri gerçekleştirmek için Visual Studio Code için Azure IoT uzantılarını kullanabilirsiniz. Bu işlemlerin işe yaraması için Azure hesabınızda oturum açmanız ve IoT Hub'ınızı seçmeniz gerekir.

1. Visual Studio Code'da **Explorer** görünümünü açın.
1. Explorer'ın alt kısmında **Azure IoT Hub** bölümünü genişletin.

   ![Azure IoT Hub Aygıtları bölümünü genişletme](./media/how-to-register-device/azure-iot-hub-devices.png)

1. **Azure IoT Hub** **bölümündeki ...** bölümünü tıklatın. Elipsleri görmüyorsanız, üstbilginin üzerine tıklayın veya üstbilginin üzerine gidin.
1. **IoT Hub'ı seçin.**
1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.
1. Azure aboneliğinizi seçin.
1. IoT hub'ınızı seçin.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Visual Studio Code ile bir IoT Edge cihazı oluşturma

1. VS Code Explorer'da **Azure IoT Hub Aygıtları** bölümünü genişletin.
1. **Azure IoT Hub Aygıtları** **bölümündeki ...** bölümünü tıklatın. Elipsleri görmüyorsanız, üstbilginin üzerine tıklayın veya üstbilginin üzerine gidin.
1. **IoT Edge Cihazı Oluştur'u**seçin.
1. Açılan metin kutusunda cihazınıza bir kimlik verin.

Çıktı ekranında komutun sonucunu görürsünüz. Aygıt bilgileri, sağladığınız **deviceId'i** ve fiziksel aygıtınızı IoT hub'ınıza bağlamak için kullanabileceğiniz **connectionString'i** içeren yazdırılır.

Çıktı ekranında komutun sonucunu görürsünüz. Aygıt bilgileri, sağladığınız **deviceId'i** ve fiziksel aygıtınızı IoT hub'ınıza bağlamak için kullanabileceğiniz **connectionString'i** içeren yazdırılır.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Visual Studio Code ile IoT Edge cihazlarını görüntüleme

IoT hub'ınıza bağlanan tüm aygıtlar Visual Studio Code Explorer'ın **Azure IoT Hub** bölümünde listelenir. IoT Edge aygıtları, farklı bir simgeye sahip Kenar dışı aygıtlardan ve **$edgeAgent** ve **$edgeHub** modüllerinin her IoT Edge aygıtına dağıtılmasından ayırt edilebilir.

![Tüm IoT Edge aygıtlarını IoT hub'ınızda görüntüleme](./media/how-to-register-device/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Visual Studio Code ile bağlantı dizesini alın

Cihazınızı kurmaya hazır olduğunuzda, fiziksel aygıtınızı IoT hub'ındaki kimliğiyle ilişkilendiren bağlantı dizesine ihtiyacınız vardır.

1. **Azure IoT Hub** bölümündecihazınızın kimliğine sağ tıklayın.
1. **Aygıt Bağlantı Dizelerini Kopyala'yı**seçin.

   Bağlantı dizesi panonuza kopyalanır.

Ayrıca, çıkış penceresinde bağlantı dizesi de dahil olmak üzere tüm aygıt bilgilerini görmek için sağ tıklama menüsünden **Aygıt Bilgilerini Al'ı** da seçebilirsiniz.

## <a name="register-with-the-azure-cli"></a>Azure CLI ile kaydolun

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Edge gibi Azure kaynaklarını yönetmek için açık kaynaklı çapraz platform komut satırı aracıdır. Azure IoT Hub kaynaklarını, aygıt sağlama hizmeti örneklerini ve bağlantılı hub'ları kutudan yönetmenize olanak tanır. IoT uzantısı, Azure CLI'yi aygıt yönetimi ve tam IoT Edge özelliği gibi özelliklerle zenginleştirir.

### <a name="prerequisites-for-the-azure-cli"></a>Azure CLI için ön koşullar

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-using-cli.md)
* Ortamınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IoT uzantısı.](https://github.com/Azure/azure-iot-cli-extension)

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Azure CLI ile bir IoT Edge aygıtı oluşturma

IoT hub'ınızda yeni bir aygıt kimliği oluşturmak için [az iot hub aygıt kimliği oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametre içerir:

* **device-id**: IoT hub'ınıza özgü açıklayıcı bir ad sağlayın.
* **hub-name**: IoT hub'ınızın adını sağlayın.
* **kenar özellikli**: Bu parametre, aygıtın IoT Edge ile kullanılmak üzere olduğunu bildirir.

   ![az iot hub cihaz-kimlik çıkış oluşturmak](./media/how-to-register-device/Create-edge-device.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>IoT Edge cihazlarını Azure CLI ile görüntüleme

IoT [hub'ınızdaki](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list) tüm aygıtları görüntülemek için az iot hub aygıt kimlik listesi komutunu kullanın. Örnek:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

Bir IoT Edge aygıt olarak kayıtlı herhangi bir cihaz özelliği **yetenekleri.iotEdge** **gerçek**ayarlanır.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Azure CLI ile bağlantı dizesini alma

Cihazınızı kurmaya hazır olduğunuzda, fiziksel aygıtınızı IoT hub'ındaki kimliğiyle ilişkilendiren bağlantı dizesine ihtiyacınız vardır. Tek bir aygıt için bağlantı dizesini döndürmek için [az iot hub aygıt-kimlik göster-bağlantı-dize](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) komutunu kullanın:

   ```azurecli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

Parametrenin `device-id` değeri büyük/küçük harf duyarlıdır. Bağlantı dizesinin etrafındaki tırnak işaretlerini kopyalamayın.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT hub'ınızda kayıtlı bir aygıt kimliğiniz olduğuna göre, IoT Edge çalışma zamanını aygıtlarınıza yüklemeye hazırsınız. Çalışma süresini aygıtın işletim sistemine göre yükleyin:

* [Azure IoT Edge'i Windows'a yükleme](how-to-install-iot-edge-windows.md)
* [Azure IoT Edge çalışma süresini Linux'a yükleme](how-to-install-iot-edge-linux.md)
