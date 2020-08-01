---
title: IoT Tak ve Kullan önizleme cihazlarıyla etkileşimde bulunmak için Azure CLı için Azure IoT uzantısı 'nı kullanın | Microsoft Docs
description: Azure CLı için Azure IoT uzantısını yükleyip IoT Hub 'ma bağlı IoT Tak ve Kullan cihazlarıyla etkileşim kurmak için kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 07/20/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: dadb1f044547acd6e5f0d274143123e89d7dae46
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475490"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını yükleyip kullanma

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Hub gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLı, Windows, Linux ve macOS 'ta kullanılabilir. Azure CLı, herhangi bir uzantı yüklemeden Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub 'ları yönetmenizi sağlar.

Azure CLı için Azure IoT uzantısı IoT Tak ve Kullan önizleme cihazlarını test etmeye ve bunlarla etkileşime yönelik bir komut satırı aracıdır. Uzantıyı şu şekilde kullanabilirsiniz:

- Bir cihaza bağlanın.
- Cihazın gönderdiği Telemetriyi görüntüleyin.
- Cihaz özellikleriyle çalışın.
- Çağrı cihazı komutları.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure CLı için Azure IoT uzantısını yükleyip yapılandırın.
- Cihazlarınızı kullanarak cihazlarınızla etkileşim kurun ve test edin.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını yükler

### <a name="step-1---install-the-azure-cli"></a>1. adım-Azure CLı 'yı yüklemeyi

Ortamınızda Azure CLı 'yı ayarlamak için [yükleme yönergelerini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyin. En iyi deneyim için, Azure CLı sürümünüz sürüm 2.9.1 veya üzeri olmalıdır. Doğrulamak için `az -–version` kullanın.

### <a name="step-2---install-iot-extension"></a>2. adım-IoT uzantısını yüklemeyi

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-iot` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-iot` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Yazma sırasında, uzantı sürüm numarası olur `0.9.7` .

Uzantıyı kaldırmak için `az extension remove --name azure-iot` kullanabilirsiniz.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını kullanma

### <a name="prerequisites"></a>Ön koşullar

Azure aboneliğinizde oturum açmak için aşağıdaki komutu çalıştırın:

```azurecli
az login
```

Azure CLı için Azure IoT uzantısını kullanmak için şunlar gerekir:

- Azure IoT Hub 'ı. Azure aboneliğinize IoT Hub 'ı [oluşturma](../iot-hub/iot-hub-create-using-cli.md)gibi bir IoT Hub 'ı eklemenin birçok yolu vardır. Azure IoT uzantı komutlarını çalıştırmak için IoT Hub 'ın bağlantı dizesine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- IoT Hub 'ınıza kayıtlı bir cihaz. Bir cihazı kaydetmek için aşağıdaki Azure CLı komutunu kullanabilirsiniz, `{YourIoTHubName}` ve `{YourDeviceID}` yer tutucuları değerlerinizle değiştirdiğinizden emin olun:

    ```azurecli
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

### <a name="interact-with-a-device"></a>Cihazla etkileşim kurma

Bir IoT Hub 'ına bağlı IoT Tak ve Kullan cihazlarını görüntülemek ve bunlarla etkileşim kurmak için uzantısını kullanabilirsiniz. Uzantı, IoT Tak ve Kullan cihazını temsil eden dijital ikizi ile birlikte kullanılır.

#### <a name="list-devices"></a>Cihazları listeleyin

IoT Hub tüm cihazları listeleyin:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

#### <a name="view-digital-twin"></a>Dijital ikizi görüntüle

Bir cihazın dijital ikizi görüntüleyin:

```azurecli
az iot pnp twin show -n {iothub_name} -d {device_id}
```

#### <a name="properties"></a>Özellikler

Okuma-yazma özelliğinin değerini ayarlayın:

```azurecli
az iot pnp twin update --hub-name {iothub_name} --device-id {device_id} --json-patch '{"op":"add", "path":"/thermostat1/targetTemperature", "value": 54}'
```

#### <a name="commands"></a>Komutlar

Bir komut çağırın:

```azurecli
az iot pnp twin invoke-command --cn getMaxMinReport -n {iothub_name} -d {device_id} --component-path thermostat1
```

#### <a name="digital-twin-events"></a>Dijital ikizi olayları

Belirli bir cihazdan ve **$Default** Olay Hub 'ı tüketici grubuna giden arabirimden tüm IoT Tak ve kullan dijital ikizi olaylarını izleyin:

```azurecli
az iot hub monitor-events -n {iothub_name} -d {device_id} -i {interface_id}
```

### <a name="manage-models-in-the-model-repository"></a>Model deposundaki modelleri yönetme

Depodaki modelleri yönetmek için Azure CLı model deposu komutlarını kullanabilirsiniz.

#### <a name="create-model-repository"></a>Model deposu oluştur

Kiracınızdaki ilk Kullanıcı kullanıyorsanız kiracınız için yeni bir IoT Tak ve Kullan şirket deposu oluşturun:

```azurecli
az iot pnp repo create
```

#### <a name="manage-model-repository-tenant-roles"></a>Model deposu kiracı rollerini yönetme

Belirli bir kaynağa bir kullanıcı veya hizmet sorumlusu için bir rol ataması oluşturun.

Örneğin, user@consoso.com kiracı Için **Modelscreator** rolünü verin:

```azurecli
az iot pnp role-assignment create --resource-id {tenant_id} --resource-type Tenant --subject-id {user@contoso.com} --subject-type User --role ModelsCreator
```

Ya da user@consoso.com belirli bir model Için **Modeladministrator** rolünü verin:

```azurecli
az iot pnp role-assignment create --resource-id {model_id} --resource-type Model --subject-id {user@contoso.com} --subject-type User --role ModelAdministrator
```

#### <a name="create-a-model"></a>Bir model oluşturma

Şirket deposunda yeni bir model oluşturun:

```azurecli
az iot pnp model create --model {model_json or path_to_file}
```

#### <a name="search-a-model"></a>Model Arama

Belirli bir anahtar sözcükle eşleşen modelleri listeleyin:

```azurecli
az iot pnp model list -q {search_keyword}
```

#### <a name="publish-a-model"></a>Modeli yayımlama

Şirket deposunda bulunan bir cihaz modelini ortak depoya yayımlayın.

Örneğin, şu KIMLIĞE sahip modeli ortak yapın `dtmi:com:example:ClimateSensor;1` :

```azurecli
az iot pnp model publish --dtmi "dtmi:com:example:ClimateSensor;1"
```

Bir modeli yayımlamak için aşağıdaki gereksinimlerin karşılanması gerekir:

- Şirket veya kuruluş kiracısı bir Microsoft Iş ortağı olmalıdır. 
- Kullanıcı veya hizmet sorumlusu, depo kiracının **Yayımcı** rolünün bir üyesi olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde IoT Tak ve Kullan cihazlarınızla etkileşim kurmak için Azure CLı için Azure IoT uzantısını yüklemeyi ve kullanmayı öğrendiniz. Önerilen bir sonraki adım, [cihazlarınızla Azure IoT Explorer 'ın](./howto-use-iot-explorer.md)nasıl kullanılacağını öğrenirsiniz.
