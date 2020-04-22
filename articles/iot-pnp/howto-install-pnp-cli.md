---
title: IoT Tak ve Çalıştır Önizleme cihazlarıyla etkileşimde kalmak için Azure CLI için Azure IoT uzantısını kullanın | Microsoft Dokümanlar
description: Azure CLI için Azure IoT uzantısını yükleyin ve IoT hub'ıma bağlı IoT Tak ve Çalıştır aygıtlarıyla etkileşim kurmak için kullanın.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1ccb32996cd8f15805a810dd5b5985aeb5f87c26
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770453"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Azure CLI için Azure IoT uzantısını yükleyin ve kullanın

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Hub gibi Azure kaynaklarını yönetmek için açık kaynaklı çapraz platform komut satırı aracıdır. Azure CLI, Windows, Linux ve MacOS'ta kullanılabilir. Azure CLI, [Azure Bulut Kabuğu'nda](https://shell.azure.com)da önceden yüklenir. Azure CLI, Azure IoT Hub kaynaklarını, Aygıt Sağlama Hizmeti örneklerini ve bağlantılı hub'ları herhangi bir uzantı yüklemeden yönetmenize olanak tanır.

Azure CLI için Azure IoT uzantısı, IoT Tak ve Çalıştır Önizleme aygıtlarıyla etkileşim kurmak ve test etmek için bir komut satırı aracıdır. Uzantıyı şu adreste kullanabilirsiniz:

- Bir aygıta bağlanın.
- Aygıtın gönderdiği telemetriyi görüntüleyin.
- Aygıt özellikleriyle çalışın.
- Çağrı aygıtı komutları.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure CLI için Azure IoT uzantısını yükleyin ve yapılaştırın.
- Aygıtlarınızla etkileşim kurmak ve test etmek için uzantıyı kullanın.
- Model deposundaki arabirimleri yönetmek için uzantıyı kullanın.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Azure CLI için Azure IoT uzantısını yükleme

### <a name="step-1---install-the-azure-cli"></a>Adım 1 - Azure CLI'yi yükleyin

Ortamınızda Azure CLI'yi ayarlamak için [yükleme yönergelerini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyin. Aşağıdaki tüm komutları kullanmak için Azure CLI sürümünüz sürüm 2.0.73 veya üzeri olmalıdır. Doğrulamak için `az -–version` kullanın.

### <a name="step-2---install-iot-extension"></a>Adım 2 - IoT uzantısı nı yükleyin

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-iot` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-iot` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-iot` kullanabilirsiniz.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Azure CLI için Azure IoT uzantısını kullanma

### <a name="prerequisites"></a>Ön koşullar

Azure aboneliğinizde oturum açmak için aşağıdaki komutu çalıştırın:

```azurecli
az login
```

> [!NOTE]
> Azure bulut uyşunu kullanıyorsanız, otomatik olarak oturum açmışolursunuz ve önceki komutu çalıştırmanız gerekmez.

Azure CLI için Azure IoT uzantısını kullanmak için şunları yapmanız gerekir:

- Azure IoT hub'ı. [Azure CLI'yi kullanarak Bir IoT hub'ı oluşturma](../iot-hub/iot-hub-create-using-cli.md)gibi Azure aboneliğinize Bir IoT hub'ı eklemenin birçok yolu vardır. Azure IoT uzantı komutlarını çalıştırmak için IoT hub'ın bağlantı dizesine ihtiyacınız vardır. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- IoT hub'ınıza kayıtlı bir aygıt. Bir cihazı kaydettirmek için aşağıdaki Azure CLI komutunu `{YourIoTHubName}` kullanabilirsiniz, yer tutucuları ve `{YourDeviceID}` yer tutucuları değerlerinizle değiştirdiğinizden emin olun:

    ```azurecli
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

- Bazı komutların şirket modeli deposu için bağlantı dizesi gerekir. [IoT](howto-onboard-portal.md)portalı için Azure Sertifikalı portalına ilk bindiğinizde şirketiniz için bir model deposu oluşturulur. Üçüncü bir taraf, arabirimlerine ve modellerine erişim sağlamak için model depo bağlantı dizesini sizinle paylaşabilir.

### <a name="interact-with-a-device"></a>Bir aygıtla etkileşim

Uzantıyı, bir IoT hub'ına bağlı IoT Tak ve Çalıştır aygıtlarını görüntülemek ve bunlarla etkileşimde kalmak için kullanabilirsiniz. Uzantı, IoT Tak ve Çalıştır aygıtını temsil eden dijital ikizle çalışır.

#### <a name="list-devices-and-interfaces"></a>Cihazları ve arabirimleri listele

Tüm aygıtları bir IoT Hub'ında listele:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

Bir IoT Tak ve Çalıştır aygıtı tarafından kayıtlı tüm arabirimleri listele:

```azurecli
az iot dt list-interfaces --hub-name {YourIoTHubName} --device-id {YourDeviceID}
```

#### <a name="properties"></a>Özellikler

Aygıttaki arabirimin tüm özelliklerini ve özellik değerlerini listele:

```azurecli
az iot dt list-properties --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login "{YourCompanyModelRepoConnectionString}"
```

Okuma yazma özelliğinin değerini ayarlayın:

```azurecli
az iot dt update-property --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface-payload {JSONPayload or FilePath}
```

Bir aygıtın **sensör** arabirimindeki **ad** özelliğini **Contoso'ya** ayarlamak için örnek bir yük dosyası aşağıdaki gibi görünür:

```json
{
  "sensor": {
    "properties": {
      "name": {
        "desired": {
          "value": "Contoso"
        }
      }
    }
  }
}
```

#### <a name="commands"></a>Komutlar

Aygıttaki arabirim için tüm komutları listele:

```azurecli
az iot dt list-commands --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login {YourCompanyModelRepoConnectionString}
```

`--repo-login` Parametre olmadan, bu komut ortak model deposunu kullanır.

Bir komut çağırın:

```azurecli
az iot dt invoke-command --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --cn {CommandName} --command-payload {CommandPayload or FilePath}
```

#### <a name="digital-twin-events"></a>Dijital ikiz olaylar

**$Default** etkinlik merkezi tüketici grubuna giderek belirli bir cihaz dan tüm IoT Tak ve Çalıştır dijital ikiz olayları izleyin:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID}
```

Belirli bir tüketici grubuna giden tüm IoT Tak ve Çalıştır dijital ikiz olaylarını belirli bir cihazdan ve arabirimden izleyin:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --consumer-group {YourConsumerGroup}
```

### <a name="manage-interfaces-in-a-model-repository"></a>Bir model deposunda arabirimleri yönetme

Aşağıdaki komutlar ortak IoT Tak ve Çalıştır modeli deposunu kullanır. Şirket modeli deposunu kullanmak için, `--login` bağımsız değişkeni model deposu bağlantı dizeninizle ekleyin.

Ortak IoT Tak ve Çalıştır model deposundaki arayüzleri listele:

```azurecli
az iot pnp interface list
```

Ortak IoT Tak ve Çalıştır modeli deposunda bir arabirim göster:

```azurecli
az iot pnp interface show --interface {YourInterfaceId}
```

IoT Tak ve Çalıştır şirket model deponuzda bir arayüz oluşturun:

```azurecli
az iot pnp interface create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposunda doğrudan bir arabirim oluşturamazsınız.

IoT Tak ve Çalıştır şirket model deponuzdaki arabirimi güncelleştirin:

```azurecli
az iot pnp interface update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposundaki bir arabirimi doğrudan güncelleştiremezsiniz.

IoT Tak ve Çalıştır şirket model deponuzdan ortak model deposuna bir arayüz yayımlayın. Bu işlem arabirimi değişmez hale getirir:

```azurecli
az iot pnp interface publish --interface {YourInterfaceID} --login {YourCompanyModelRepoConnectionString}
```

Arabirimleri ortak model deposunda yalnızca Microsoft iş ortakları yayımlayabilir.

### <a name="manage-device-capability-models-in-a-model-repository"></a>Model deposundaki aygıt yetenek modellerini yönetme

Aşağıdaki komutlar ortak IoT Tak ve Çalıştır modeli deposunu kullanır. Şirket modeli deposunu kullanmak için, `--login` bağımsız değişkeni model deposu bağlantı dizeninizle ekleyin.

IoT Tak ve Ortak model deposundaki aygıt yetenek modellerini listele:

```azurecli
az iot pnp capability-model list
```

IoT Tak ve Ortak model deposunda bir aygıt yetenek modeli göster:

```azurecli
az iot pnp capability-model show --model {YourModelID}
```

IoT Tak ve Çalıştır şirket model deposunda aygıt yeteneği modeli oluşturun:

```azurecli
az iot pnp capability-model create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Genel model deposunda doğrudan bir model oluşturamazsınız.

IoT Tak ve Çalıştır şirket model deposundaki aygıt yetenek modelini güncelleştirin:

```azurecli
az iot pnp capability-model update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposundaki bir modeli doğrudan güncelleştiremezsiniz.

IoT Tak ve Çalıştır şirket model deponuzdan ortak model deposuna bir aygıt yetenek modeli yayımlayın. Bu işlem modeli değişmez hale getirir:

```azurecli
az iot pnp capability-model publish --model {YourModelID} --login {YourCompanyModelRepoConnectionString}
```

Modelleri ortak model deposunda yalnızca Microsoft iş ortakları yayımlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, Tak ve Çalıştır aygıtlarınızla etkileşimde kalmak için Azure CLI için Azure IoT uzantısını nasıl yükleyip kullanacağınızı öğrendiniz. Önerilen bir sonraki [adım, modelleri](./howto-manage-models.md)nasıl yönetinin öğrenmektir.
