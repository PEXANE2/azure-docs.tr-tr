---
title: Azure CLı & IoT uzantısı kullanarak IoT Hub cihaz sağlama hizmetini yönetme
description: IoT Hub cihaz sağlama hizmetini yönetmek için Azure CLı ve IoT uzantısını kullanmayı öğrenin
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 0e276ca32d7cd02f9668b33b3729757404b13b00
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74229736"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>IoT Hub cihaz sağlama hizmetini yönetmek için Azure CLı ve IoT uzantısını kullanma

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) , IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLı, Windows, Linux ve MacOS 'ta kullanılabilir. Azure CLı, Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub 'ları kutudan dışarı yönetmenize olanak sağlar.

IoT uzantısı, Azure CLı 'yi cihaz yönetimi ve tam IoT Edge özelliği gibi özelliklerle zenginleştirir.

Bu öğreticide ilk olarak Azure CLı ve IoT uzantısını ayarlama adımlarını tamamlayabilirsiniz. Ardından, temel cihaz sağlama hizmeti işlemlerini gerçekleştirmek için CLı komutlarının nasıl çalıştırılacağını öğreneceksiniz. 

## <a name="installation"></a>Yükleme 

### <a name="step-1---install-python"></a>Adım 1 - Python yükleme

[Python 2.7x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="step-2---install-the-azure-cli"></a>2\. adım-Azure CLı 'yı yüklemeyi

Ortamınızda Azure CLı 'yı kurmak için [yükleme yönergesini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) izleyin. En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az –version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir.

### <a name="step-3---install-iot-extension"></a>Adım 3 - IoT uzantısı yükleme

[IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar. En basit yol `az extension add --name azure-cli-iot-ext` komutunu çalıştırmaktır. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayabilir veya `az extension show --name azure-cli-iot-ext` kullanarak IoT uzantısına ilişkin ayrıntıları görebilirsiniz. Uzantıyı kaldırmak için `az extension remove --name azure-cli-iot-ext` kullanabilirsiniz.


## <a name="basic-device-provisioning-service-operations"></a>Temel cihaz sağlama hizmeti işlemleri
Örnek, Azure hesabınızda oturum açma, bir Azure Kaynak grubu oluşturma (bir Azure çözümü için ilgili kaynakları tutan bir kapsayıcı), IoT Hub oluşturma, cihaz sağlama hizmeti oluşturma, mevcut cihaz sağlama hizmetlerini listeleme ve CLı komutlarıyla bağlı bir IoT Hub 'ı oluşturun. 

Başlamdan önce daha önce açıklanan yükleme adımlarını tamamlayın. Henüz bir Azure hesabınız yoksa hemen [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Azure hesabında oturum açın
  
    az login

![oturum açma][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. eastus 'de IoTHubBlogDemo kaynak grubu oluşturma

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma][2]


### <a name="3-create-two-device-provisioning-services"></a>3. iki cihaz sağlama hizmeti oluşturun

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Cihaz sağlama hizmeti oluşturma][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. bu kaynak grubunun altındaki tüm mevcut cihaz sağlama hizmetlerini listeleyin

    az iot dps list --resource-group IoTHubBlogDemo

![Cihaz sağlama hizmetlerini listeleme][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. yeni oluşturulan kaynak grubu altında bir IoT Hub blogDemoHub oluşturun

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IoT Hub oluşturun][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. mevcut bir IoT Hub bir cihaz sağlama hizmetine bağlama

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Hub’ı bağlayın][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihazı kaydedin
> * Cihazı başlatın
> * Cihaz kayıtlı olduğunu doğrulayın

Yük dengeli hublar arasında birden çok cihaz sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IoT hubları arasında cihaz sağlama](./tutorial-provision-multiple-hubs.md)
