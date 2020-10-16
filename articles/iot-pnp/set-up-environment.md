---
title: IoT için ihtiyacınız olan IoT kaynaklarını ayarlayın Tak ve Kullan | Microsoft Docs
description: IoT Tak ve Kullan hızlı başlangıçlarını ve öğreticileri ile kullanmak üzere bir IoT Hub ve cihaz sağlama hizmeti örneği oluşturun.
author: dominicbetts
ms.author: dobett
ms.date: 08/11/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 3b7c9b51bad45bb348f70c8b0e433404b49b5aac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761371"
---
# <a name="set-up-your-environment-for-the-iot-plug-and-play-quickstarts-and-tutorials"></a>IoT Tak ve Kullan hızlı başlangıç ve öğreticiler için ortamınızı ayarlama

IoT Tak ve Kullan hızlı başlangıçların ve öğreticilerinin hiçbirini tamamlamadan önce Azure aboneliğinizde bir IoT Hub 'ı ve cihaz sağlama hizmeti 'ni (DPS) yapılandırmanız gerekir. Ayrıca, örnek uygulamalar ve Azure IoT gezgin aracı tarafından kullanılan model dosyalarının yerel kopyalarına de ihtiyacınız olacaktır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Azure CLı 'yı yerel olarak kurma gereksinimini ortadan kaldırmak için Azure Cloud Shell kullanarak bulut hizmetlerini ayarlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resources"></a>Kaynakları oluşturma

Kaynaklar için bir Azure Kaynak grubu oluşturun:

```azurecli-interactive
az group create --name my-pnp-resourcegroup --location centralus
```

IoT Hub 'ı oluşturun. Aşağıdaki komut, `my-pnp-hub` oluşturulacak IoT Hub 'ının adı için bir örnek olarak adı kullanır. IoT Hub 'ınız için şu yerde kullanılacak benzersiz bir ad seçin `my-pnp-hub` :

```azurecli-interactive
az iot hub create --name my-pnp-hub --resource-group my-pnp-resourcegroup --sku F1 --partition-count 2
```

Bir DPS örneği oluşturun. Aşağıdaki komut, `my-pnp-dps` oluşturulacak DPS örneğinin adı için bir örnek olarak adı kullanır. Şu yerine kullanılacak olan DPS örneğiniz için benzersiz bir ad seçin `my-pnp-dps` :

```azurecli-interactive
az iot dps create --name my-pnp-dps --resource-group my-pnp-resourcegroup
```

DPS örneğini IoT Hub 'ınıza bağlamak için aşağıdaki komutları kullanın. `my-pnp-dps`Ve ' i `my-pnp-hub` daha önce seçtiğiniz benzersiz adlarla değiştirin:

```azurecli-interactive
hubConnectionString=$(az iot hub connection-string show -n my-pnp-hub --key primary --query connectionString -o tsv)
az iot dps linked-hub create --dps-name my-pnp-dps --resource-group my-pnp-resourcegroup --location centralus --connection-string $hubConnectionString
```

## <a name="retrieve-the-settings"></a>Ayarları al

Bazı hızlı başlangıç ve öğreticiler, IoT Hub 'ınız için bağlantı dizesini kullanır. Ayrıca, Azure IoT gezgin aracını ayarlarken bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini alın ve şimdi bir yere göz önünde olun. `my-pnp-hub`IoT Hub 'ınız için seçtiğiniz benzersiz adla değiştirin:

```azurecli-interactive
az iot hub connection-string show -n my-pnp-hub --key primary --query connectionString
```

Hızlı başlangıç ve öğreticilerin çoğu, DPS yapılandırmanızın *kimlik kapsamını* kullanır. KIMLIK kapsamını alın ve şimdi bunu bir yere göz önünde yapın. `my-pnp-dps`, DPS örneğiniz için seçtiğiniz benzersiz adla değiştirin:

```azurecli-interactive
az iot dps show --name my-pnp-dps --query properties.idScope
```

Tüm hızlı başlangıç ve öğreticiler bir DPS cihaz kaydı kullanır. `my-pnp-device`DPS örneğiniz için *tek bir cihaz kaydı* oluşturmak üzere aşağıdaki komutu kullanın. `my-pnp-dps`, DPS örneğiniz için seçtiğiniz benzersiz adla değiştirin. Hızlı başlangıçlarda ve öğreticilerde kullanılacak kayıt KIMLIĞI ve birincil anahtar değerlerini bir yere iade edin:

```azurecli-interactive
az iot dps enrollment create --attestation-type symmetrickey --dps-name my-pnp-dps --resource-group my-pnp-resourcegroup --enrollment-id my-pnp-device --device-id my-pnp-device --query '{registrationID:registrationId,primaryKey:attestation.symmetricKey.primaryKey}'
```

## <a name="create-environment-variables"></a>Ortam değişkenlerini oluşturma

Hızlı başlangıçlarda, IoT Hub 'ınıza bağlanmak üzere cihaz sağlama hizmeti 'ni (DPS) kullanmak üzere hızlı başlangıçların ve öğreticilerdeki örnekleri yapılandırmak için beş ortam değişkeni oluşturun:

* **IOTHUB_DEVICE_SECURITY_TYPE**: değer `DPS` .
* **IOTHUB_DEVICE_DPS_ID_SCOPE**: daha önce bir notunuz YAPTıĞıNıZ DPS kimlik kapsamı.
* **IOTHUB_DEVICE_DPS_DEVICE_ID**: değer `my-pnp-device` .
* **IOTHUB_DEVICE_DPS_DEVICE_KEY**: Kayıt birincil anahtarı daha önce bir kopyasını yaptınız.
* **IOTHUB_DEVICE_DPS_ENDPOINT**: değer `global.azure-devices-provisioning.net`

Hizmet örnekleri, bağlanılacak hub ve cihazı belirlemek için aşağıdaki ortam değişkenlerine ihtiyaç duyar:

* **IOTHUB_CONNECTION_STRING**: IoT Hub bağlantı dizesi daha önce bir nota yaptınız.
* **IOTHUB_DEVICE_ID**: `my-pnp-device` .

Örneğin, bir Linux bash kabuğu 'nda:

```bash
export IOTHUB_DEVICE_SECURITY_TYPE="DPS"
export IOTHUB_DEVICE_DPS_ID_SCOPE="<Your ID scope>"
export IOTHUB_DEVICE_DPS_DEVICE_ID="my-pnp-device"
export IOTHUB_DEVICE_DPS_DEVICE_KEY="<Your enrolment primary key>"
export IOTHUB_DEVICE_DPS_ENDPOINT="global.azure-devices-provisioning.net"
export IOTHUB_CONNECTION_STRING="<Your IoT hub connection string>"
export IOTHUB_DEVICE_ID="my-pnp-device"
```

Örneğin, Windows komut satırında:

```cmd
set IOTHUB_DEVICE_SECURITY_TYPE=DPS
set IOTHUB_DEVICE_DPS_ID_SCOPE=<Your ID scope>
set IOTHUB_DEVICE_DPS_DEVICE_ID=my-pnp-device
set IOTHUB_DEVICE_DPS_DEVICE_KEY=<Your enrolment primary key>
set IOTHUB_DEVICE_DPS_ENDPOINT=global.azure-devices-provisioning.net
set IOTHUB_CONNECTION_STRING=<Your IoT hub connection string>
set IOTHUB_DEVICE_ID=my-pnp-device
```

## <a name="download-the-model-files"></a>Model dosyalarını indirin

Hızlı başlangıç ve öğreticiler, sıcaklık denetleyicisi ve termostat cihazları için örnek model dosyalarını kullanır. Örnek model dosyalarını indirmek için:

1. Yerel makinenizde *modeller* adlı bir klasör oluşturun.

1. [TemperatureController.jsüzerinde](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/TemperatureController.json) sağ tıklayın ve json dosyasını *modeller* klasörüne kaydedin.

1. [Thermostat.jsüzerinde](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/Thermostat.json) sağ tıklayın ve json dosyasını *modeller* klasörüne kaydedin.

## <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

Hızlı başlangıç ve öğreticiler, **Azure IoT gezgin** aracını kullanır. [Azure IoT Explorer sürümleri](https://github.com/Azure/azure-iot-explorer/releases) ' ne gidin ve en son sürümün varlık listesini genişletin. İşletim sisteminiz için uygulamanın en son sürümünü indirin ve yükleyin.

Aracı ilk kez çalıştırdığınızda IoT Hub bağlantı dizesi istenir. Daha önce bir nota yaptığınız bağlantı dizesini kullanın.

Daha önce indirdiğiniz model dosyalarını kullanmak için aracı yapılandırın. Araçtaki giriş sayfasından **ıot Tak ve kullan ayarları**' nı ve sonra da **> yerel klasör ekle**' yi seçin. Daha önce oluşturduğunuz *modeller* klasörünü seçin. Sonra ayarları kaydetmek için **Kaydet** ' i seçin.

Daha fazla bilgi için bkz. [Azure IoT Explorer 'ı yüklemek ve kullanmak](howto-use-iot-explorer.md).

## <a name="remove-the-resources"></a>Kaynakları kaldırma

IoT Hub ve DPS örneğini tüm IoT Tak ve Kullan hızlı başlangıç ve öğreticiler için kullanabilirsiniz. bu nedenle, bu makaledeki adımları yalnızca bir kez gerçekleştirmeniz gerekir. İşiniz bittiğinde, aşağıdaki komutla aboneliğinizden kaldırabilirsiniz:

```azurecli-interactive
az group delete --name my-pnp-resourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızı ayarladığınıza göre, hızlı başlangıç veya öğreticilerden birini deneyebilirsiniz:

> [!div class="nextstepaction"]
> [Örnek IoT Tak ve Kullan cihaz uygulamasını IoT Hub 'ye bağlama (Node.js)](quickstart-connect-device-node.md)
