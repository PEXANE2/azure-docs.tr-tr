---
title: Azure CLI kullanarak dosya yüklemeyi IoT Hub'a yapılandırma | Microsoft Dokümanlar
description: Platformlar arası Azure CLI'yi kullanarak dosya yüklemelerini Azure IoT Hub'a yapılandırma.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: robinsh
ms.openlocfilehash: df3c8d2abf59de6c9f685ad8d93e6689738df8e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302534"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Azure CLI kullanarak IoT Hub dosya yüklemelerini yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

[Bir aygıttan dosya yüklemek](iot-hub-devguide-file-upload.md)için öncelikle bir Azure Depolama hesabını IoT hub'ınızla ilişkilendirmeniz gerekir. Varolan bir depolama hesabı kullanabilir veya yeni bir depo oluşturabilirsiniz.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* Azure IoT hub'ı. Bir IoT hub'ıniz yoksa, bir tane oluşturmak için [ `az iot hub create` komutu](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) kullanabilir veya [portalı kullanarak bir IoT hub'ı oluşturabilirsiniz.](iot-hub-create-through-portal.md)

* Azure Depolama hesabı. Azure Depolama hesabınız yoksa, bir Azure Depolama hesabınız oluşturmak için Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md).

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde [oturum açma komutunu](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) çalıştırın:

    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

2. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Kullanabileceğiniz [Azure hesaplarını listelemek için aşağıdaki komutu](https://docs.microsoft.com/cli/azure/account) kullanın:

    ```azurecli
    az account list
    ```

    IoT hub'ınızı oluşturmak için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabı bilgilerinizi alma

Aşağıdaki adımlar, Depolama hesabınızı **Klasik** dağıtım modelini değil, **Kaynak Yöneticisi** dağıtım modelini kullanarak oluşturduğunuzu varsayar.

Aygıtlarınızdan dosya yüklemelerini yapılandırmak için bir Azure depolama hesabının bağlantı dizesine ihtiyacınız vardır. Depolama hesabı, IoT hub'ınızla aynı abonelikte olmalıdır. Ayrıca depolama hesabında bir blob konteyner adı gerekir. Depolama hesabı anahtarlarınızı almak için aşağıdaki komutu kullanın:

```azurecli
az storage account show-connection-string --name {your storage account name} \
  --resource-group {your storage account resource group}
```

**ConnectionString** değerine dikkat edin. Aşağıdaki adımlarda ihtiyacınız var.

Dosya yüklemeleriniz için varolan bir blob kapsayıcısı kullanabilir veya yeni bir tane oluşturabilirsiniz:

* Depolama hesabınızdaki varolan blob kaplarını listelemek için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Depolama hesabınızda bir blob kapsayıcısı oluşturmak için aşağıdaki komutu kullanın:

    ```azurecli
    az storage container create --name {container name} \
      --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Dosya yükleme

Artık IoT hub'ınızı, depolama hesabı bilgilerinizi kullanarak [IoT](iot-hub-devguide-file-upload.md) hub'ına dosya yükleme olanağı nı etkinleştirecek şekilde yapılandırabilirsiniz.

Yapılandırma aşağıdaki değerleri gerektirir:

* **Depolama kapsayıcısı**: IoT hub'ınızla ilişkilendirmek için geçerli Azure aboneliğinizdeki bir Azure depolama hesabındaki blob kapsayıcısı. Önceki bölümde gerekli depolama hesabı bilgilerini aldınız. IoT Hub, aygıtların dosya yüklerken kullanmaları için bu blob kapsayıcısına yazma izinleri ile otomatik olarak SAS URI'leri oluşturur.

* **Yüklenen dosyalar için bildirimler alın**: Dosya yükleme bildirimlerini etkinleştirin veya devre dışı.

* **SAS TTL**: Bu ayar, IoT Hub tarafından cihaza döndürülen SAS URI'lerinin canlı olarak zaman dilimidir. Varsayılan olarak bir saatolarak ayarlayın.

* **Dosya bildirim ayarları varsayılan TTL**: Süresi dolmadan önce dosya yükleme bildiriminin zaman aşımısüresi. Varsayılan olarak bir güne ayarlayın.

* **Dosya bildirimi maksimum teslimat sayısı**: IoT Hub'ının dosya yükleme bildirimini kaç kez sunmaya çalıştığı. Varsayılan olarak 10 olarak ayarlayın.

IoT hub'ınızdaki dosya yükleme ayarlarını yapılandırmak için aşağıdaki Azure CLI komutlarını kullanın:

<!--Robinsh this is out of date, add cloud powershell -->

Bir bash kabuk olarak, kullanın:

```azurecli
az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} \
  --set properties.enableFileUploadNotifications=true

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Aşağıdaki komutu kullanarak IoT hub'ınızdaki dosya yükleme yapılandırmasını gözden geçirebilirsiniz:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın dosya yükleme özellikleri hakkında daha fazla bilgi için bir [aygıttan dosya yükleme](iot-hub-devguide-file-upload.md)bölümüne bakın.

Azure IoT Hub'ı yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IoT çözümünüzü sıfırdan emniyete](../iot-fundamentals/iot-security-ground-up.md)
