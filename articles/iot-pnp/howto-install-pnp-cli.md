---
title: IoT Tak ve Kullan önizleme cihazlarıyla etkileşimde bulunmak için Azure CLı için Azure IoT uzantısı 'nı kullanın | Microsoft Docs
description: Azure CLı için Azure IoT uzantısını yükleyip IoT Hub 'ma bağlı IoT Tak ve Kullan cihazlarıyla etkileşim kurmak için kullanın.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1ccb32996cd8f15805a810dd5b5985aeb5f87c26
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81770453"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını yükleyip kullanma

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Hub gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLı, Windows, Linux ve MacOS 'ta kullanılabilir. Azure CLı [Azure Cloud Shell](https://shell.azure.com)de önceden yüklenir. Azure CLı, herhangi bir uzantı yüklemeden Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub 'ları yönetmenizi sağlar.

Azure CLı için Azure IoT uzantısı IoT Tak ve Kullan önizleme cihazlarını test etmeye ve bunlarla etkileşime yönelik bir komut satırı aracıdır. Uzantıyı şu şekilde kullanabilirsiniz:

- Bir cihaza bağlanın.
- Cihazın gönderdiği Telemetriyi görüntüleyin.
- Cihaz özellikleriyle çalışın.
- Çağrı cihazı komutları.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure CLı için Azure IoT uzantısını yükleyip yapılandırın.
- Cihazlarınızı kullanarak cihazlarınızla etkileşim kurun ve test edin.
- Model deposundaki arabirimleri yönetmek için uzantıyı kullanın.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını yükler

### <a name="step-1---install-the-azure-cli"></a>1. adım-Azure CLı 'yı yüklemeyi

Ortamınızda Azure CLı 'yı ayarlamak için [yükleme yönergelerini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyin. Aşağıdaki tüm komutları kullanmak için Azure CLı sürümünüz sürüm 2.0.73 veya üzeri olmalıdır. Doğrulamak için `az -–version` kullanın.

### <a name="step-2---install-iot-extension"></a>2. adım-IoT uzantısını yüklemeyi

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-iot` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-iot` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-iot` kullanabilirsiniz.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını kullanma

### <a name="prerequisites"></a>Ön koşullar

Azure aboneliğinizde oturum açmak için aşağıdaki komutu çalıştırın:

```azurecli
az login
```

> [!NOTE]
> Azure Cloud Shell kullanıyorsanız, otomatik olarak oturumunuz açık demektir ve önceki komutu çalıştırmanız gerekmez.

Azure CLı için Azure IoT uzantısını kullanmak için şunlar gerekir:

- Azure IoT Hub 'ı. Azure aboneliğinize IoT Hub 'ı [oluşturma](../iot-hub/iot-hub-create-using-cli.md)gibi bir IoT Hub 'ı eklemenin birçok yolu vardır. Azure IoT uzantı komutlarını çalıştırmak için IoT Hub 'ın bağlantı dizesine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- IoT Hub 'ınıza kayıtlı bir cihaz. Bir cihazı kaydetmek için aşağıdaki Azure CLı komutunu kullanabilirsiniz, `{YourIoTHubName}` ve `{YourDeviceID}` yer tutucuları değerlerinizle değiştirdiğinizden emin olun:

    ```azurecli
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

- Bazı komutlara bir şirket modeli deposu için bağlantı dizesi gerekir. [IoT Için Azure Sertifikalı bir portala](howto-onboard-portal.md)ilk kez eklediğinizde şirketiniz için bir model deposu oluşturulur. Bir üçüncü taraf, kendi arabirimlerine ve modellerine erişim sağlamak için kendi model deposu bağlantı dizesini sizinle paylaşabilir.

### <a name="interact-with-a-device"></a>Cihazla etkileşim kurma

Bir IoT Hub 'ına bağlı IoT Tak ve Kullan cihazlarını görüntülemek ve bunlarla etkileşim kurmak için uzantısını kullanabilirsiniz. Uzantı, IoT Tak ve Kullan cihazını temsil eden dijital ikizi ile birlikte kullanılır.

#### <a name="list-devices-and-interfaces"></a>Cihazları ve arabirimleri listeleme

IoT Hub tüm cihazları listeleyin:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

IoT Tak ve Kullan cihazı tarafından kaydedilen tüm arabirimleri listeleyin:

```azurecli
az iot dt list-interfaces --hub-name {YourIoTHubName} --device-id {YourDeviceID}
```

#### <a name="properties"></a>Özellikler

Bir cihazdaki bir arabirim için tüm özellikleri ve özellik değerlerini listeleyin:

```azurecli
az iot dt list-properties --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login "{YourCompanyModelRepoConnectionString}"
```

Okuma-yazma özelliğinin değerini ayarlayın:

```azurecli
az iot dt update-property --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface-payload {JSONPayload or FilePath}
```

Bir cihazın **algılayıcı** arabirimindeki **Name** özelliğini **contoso** olarak ayarlamak için örnek bir yük dosyası aşağıdaki gibi görünür:

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

Bir cihazdaki bir arabirim için tüm komutları listeleyin:

```azurecli
az iot dt list-commands --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login {YourCompanyModelRepoConnectionString}
```

Parametresi olmadan `--repo-login` , bu komut ortak model deposunu kullanır.

Bir komut çağırın:

```azurecli
az iot dt invoke-command --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --cn {CommandName} --command-payload {CommandPayload or FilePath}
```

#### <a name="digital-twin-events"></a>Dijital ikizi olayları

Belirli bir cihazdan ve **$Default** Olay Hub 'ı tüketici grubuna giden arabirimden tüm IoT Tak ve kullan dijital ikizi olaylarını izleyin:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID}
```

Belirli bir cihazdan ve belirli bir tüketici grubuna giden arabirimden tüm IoT Tak ve Kullan dijital ikizi olaylarını izleyin:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --consumer-group {YourConsumerGroup}
```

### <a name="manage-interfaces-in-a-model-repository"></a>Bir model deposundaki arabirimleri yönetme

Aşağıdaki komutlar, genel IoT Tak ve Kullan modeli havuzunu kullanır. Bir şirket modeli deposu kullanmak için, `--login` model deposu bağlantı dizeniz ile bağımsız değişkeni ekleyin.

Genel IoT Tak ve Kullan modeli deposundaki arabirimleri listeleyin:

```azurecli
az iot pnp interface list
```

Genel IoT Tak ve Kullan modeli deposunda bir arabirim göster:

```azurecli
az iot pnp interface show --interface {YourInterfaceId}
```

IoT Tak ve Kullan şirket modeli deponuzda bir arabirim oluşturun:

```azurecli
az iot pnp interface create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposunda doğrudan bir arabirim oluşturamazsınız.

IoT Tak ve Kullan şirket modeli deponuzdaki bir arabirimi güncelleştirin:

```azurecli
az iot pnp interface update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposundaki bir arabirimi doğrudan güncelleştiremezsiniz.

IoT Tak ve Kullan şirket modeli deponuzdan ortak model deposuna bir arabirim yayımlayın. Bu işlem, arabirimi sabit yapar:

```azurecli
az iot pnp interface publish --interface {YourInterfaceID} --login {YourCompanyModelRepoConnectionString}
```

Yalnızca Microsoft iş ortakları, genel model deposuna arabirim yayımlayabilir.

### <a name="manage-device-capability-models-in-a-model-repository"></a>Bir model deposundaki cihaz yetenek modellerini yönetme

Aşağıdaki komutlar, genel IoT Tak ve Kullan modeli havuzunu kullanır. Bir şirket modeli deposu kullanmak için, `--login` model deposu bağlantı dizeniz ile bağımsız değişkeni ekleyin.

IoT Tak ve Kullan ortak model deposundaki cihaz yetenek modellerini listeleyin:

```azurecli
az iot pnp capability-model list
```

IoT Tak ve Kullan ortak model deposunda cihaz yetenek modeli gösterme:

```azurecli
az iot pnp capability-model show --model {YourModelID}
```

IoT Tak ve Kullan şirket modeli deposunda cihaz yetenek modeli oluşturma:

```azurecli
az iot pnp capability-model create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposunda doğrudan bir model oluşturamazsınız.

IoT Tak ve Kullan şirket modeli deposundaki cihaz yetenek modelini güncelleştirin:

```azurecli
az iot pnp capability-model update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Ortak model deposundaki bir modeli doğrudan güncelleştiremezsiniz.

IoT Tak ve Kullan şirket modeli deponuzdan ortak model deposuna bir cihaz yetenek modeli yayımlayın. Bu işlem, modeli sabit yapar:

```azurecli
az iot pnp capability-model publish --model {YourModelID} --login {YourCompanyModelRepoConnectionString}
```

Yalnızca Microsoft iş ortakları modelleri ortak model deposuna yayımlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde, Tak ve Kullan cihazlarınızla etkileşim kurmak için Azure CLı için Azure IoT uzantısını yüklemeyi ve kullanmayı öğrendiniz. Önerilen bir sonraki adım [modelleri yönetmeyi](./howto-manage-models.md)öğrenirsiniz.
