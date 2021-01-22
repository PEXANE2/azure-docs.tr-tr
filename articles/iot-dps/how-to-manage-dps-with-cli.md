---
title: Azure CLı & IoT uzantısı kullanarak IoT Hub cihaz sağlama hizmetini yönetme
description: IoT Hub cihaz sağlama hizmeti 'ni (DPS) yönetmek için Azure CLı ve IoT uzantısını kullanmayı öğrenin
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
ms.custom: devx-track-azurecli
services: iot-dps
ms.openlocfilehash: dd0564fbb23a0695d849852fd464308cd1b5fac9
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678971"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>IoT Hub cihaz sağlama hizmetini yönetmek için Azure CLı ve IoT uzantısını kullanma

[Azure CLI](/cli/azure) , IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLı, Windows, Linux ve macOS 'ta kullanılabilir. Azure CLı, Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub 'ları kutudan dışarı yönetmenize olanak sağlar.

IoT uzantısı, Azure CLı 'yi cihaz yönetimi ve tam IoT Edge özelliği gibi özelliklerle zenginleştirir.

Bu öğreticide ilk olarak Azure CLı ve IoT uzantısını ayarlama adımlarını tamamlayabilirsiniz. Ardından, temel cihaz sağlama hizmeti işlemlerini gerçekleştirmek için CLı komutlarının nasıl çalıştırılacağını öğreneceksiniz. 

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Python 2.7x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Bu makale, Azure CLı 'nin sürüm 2.0.70 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="basic-device-provisioning-service-operations"></a>Temel cihaz sağlama hizmeti işlemleri

Örnekte, Azure hesabınızda oturum açma, Azure Kaynak grubu oluşturma (bir Azure çözümü için ilgili kaynakları tutan bir kapsayıcı), IoT Hub oluşturma, cihaz sağlama hizmeti oluşturma, mevcut cihaz sağlama hizmetlerini listeleme ve CLı komutlarıyla bağlı bir IoT Hub oluşturma işlemi gösterilmektedir. 

Başlamdan önce daha önce açıklanan yükleme adımlarını tamamlayın. Henüz bir Azure hesabınız yoksa hemen [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabında oturum açın
  
```azurecli
az login
```

![Ekran görüntüsü az Login komutunu çalıştıran bir komut istemi penceresi gösterir.](./media/how-to-manage-dps-with-cli/login.jpg)

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. eastus 'de IoTHubBlogDemo kaynak grubu oluşturma

```azurecli
az group create -l eastus -n IoTHubBlogDemo
```

![Kaynak grubu oluşturma](./media/how-to-manage-dps-with-cli/create-resource-group.jpg)


### <a name="3-create-two-device-provisioning-services"></a>3. iki cihaz sağlama hizmeti oluşturun

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps
```

![Cihaz sağlama hizmeti oluşturma](./media/how-to-manage-dps-with-cli/create-dps.jpg)

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps2
```

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. bu kaynak grubunun altındaki tüm mevcut cihaz sağlama hizmetlerini listeleyin

```azurecli
az iot dps list --resource-group IoTHubBlogDemo
```

![Cihaz sağlama hizmetlerini listeleme](./media/how-to-manage-dps-with-cli/list-dps.jpg)


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. yeni oluşturulan kaynak grubu altında bir IoT Hub blogDemoHub oluşturun

```azurecli
az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo
```

![IoT Hub oluşturun](./media/how-to-manage-dps-with-cli/create-hub.jpg)

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. mevcut bir IoT Hub bir cihaz sağlama hizmetine bağlama

```azurecli
az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus
```

![Hub’ı bağlayın](./media/how-to-manage-dps-with-cli/create-hub.jpg)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihazı kaydedin
> * Cihazı başlatın
> * Cihaz kayıtlı olduğunu doğrulayın

Yük dengeli hublar arasında birden çok cihaz sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IoT Hub'larında cihazları sağlama](./tutorial-provision-multiple-hubs.md)