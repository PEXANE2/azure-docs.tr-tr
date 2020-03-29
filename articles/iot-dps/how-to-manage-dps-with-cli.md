---
title: Azure CLI & IoT uzantılarını kullanarak IoT Hub Aygıt Sağlama Hizmetini yönetme
description: IoT Hub Aygıt Sağlama Hizmeti'ni (DPS) yönetmek için Azure CLI ve IoT uzantısını nasıl kullanacağınızı öğrenin
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 03ec0b41ad910ff0d1dcdc17148e01ec94ea9fb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78674507"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>IoT Hub Cihaz Sağlama Hizmetini yönetmek için Azure CLI ve IoT uzantısı nasıl kullanılır?

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Edge gibi Azure kaynaklarını yönetmek için açık kaynaklı çapraz platform komut satırı aracıdır. Azure CLI Windows, Linux ve MacOS'ta kullanılabilir. Azure CLI, Azure IoT Hub kaynaklarını, Aygıt Sağlama hizmeti örneklerini ve bağlantılı hub'ları kutudan yönetmenize olanak tanır.

IoT uzantısı, Azure CLI'yi aygıt yönetimi ve tam IoT Edge özelliği gibi özelliklerle zenginleştirir.

Bu eğitimde, önce Azure CLI ve IoT uzantısını kurmak için adımları tamamlarsınız. Ardından, temel Aygıt Sağlama Hizmeti işlemlerini gerçekleştirmek için CLI komutlarının nasıl çalıştırılabildiğini öğrenirsiniz. 

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="installation"></a>Yükleme 

### <a name="install-python"></a>Python'ı yükleme

[Python 2.7x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Azure CLI'yi ortamınızda kurmak için [yükleme yönergesini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyin. Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az –version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir.

### <a name="install-iot-extension"></a>IoT uzantısını yükleme

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-iot` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-iot` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-iot` kullanabilirsiniz.


## <a name="basic-device-provisioning-service-operations"></a>Temel Cihaz Sağlama Hizmeti işlemleri

Örnek, Azure hesabınızda nasıl oturum açabileceğinizi, Azure Kaynak Grubu (Azure çözümü için ilgili kaynakları barındıran bir kapsayıcı), bir IoT Hub'ı oluşturma, Aygıt Sağlama hizmeti oluşturma, mevcut Aygıt Sağlama hizmetlerini listeleme ve CLI komutlarına sahip bağlantılı bir IoT hub'ı oluşturun. 

Başlamdan önce daha önce açıklanan yükleme adımlarını tamamlayın. Henüz bir Azure hesabınız yoksa hemen [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabına giriş yapın
  
    az login

![oturum aç](./media/how-to-manage-dps-with-cli/login.jpg)

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Eastus bir kaynak grubu IoTHubBlogDemo oluşturun

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma](./media/how-to-manage-dps-with-cli/create-resource-group.jpg)


### <a name="3-create-two-device-provisioning-services"></a>3. İki Cihaz Sağlama hizmeti oluşturun

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Cihaz Sağlama Hizmeti Oluştur](./media/how-to-manage-dps-with-cli/create-dps.jpg)

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Bu kaynak grubu altındaki mevcut tüm Cihaz Sağlama hizmetlerini listele

    az iot dps list --resource-group IoTHubBlogDemo

![Cihaz Sağlama Hizmetlerini Listele](./media/how-to-manage-dps-with-cli/list-dps.jpg)


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Yeni oluşturulan kaynak grubu altında bir IoT Hub blogDemoHub oluşturun

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub oluşturun](./media/how-to-manage-dps-with-cli/create-hub.jpg)

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Mevcut bir IoT Hub'ı Aygıt Sağlama hizmetine bağlama

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

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
