---
title: Set up IoT Hub Device Provisioning Service in the Azure portal
description: Quickstart - Set up the Azure IoT Hub Device Provisioning Service in the Azure portal
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: b983a9591743b1fda79e23aedc1aca88add2a3e2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228534"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Quickstart: Set up the IoT Hub Device Provisioning Service with the Azure portal

Bu adımlar, cihazlarınızı sağlamak için Azure bulut kaynaklarını ayarlamayı gösterir. Bu makalede şunlarla ilgili adımlar yer alır: IoT hub'ınızı oluşturma, yeni IoT Hub Cihazı Sağlama Hizmetini oluşturma ve iki hizmeti birbirine bağlama. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="create-an-iot-hub"></a>Bir IoT Hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]


## <a name="create-a-new-instance-for-the-iot-hub-device-provisioning-service"></a>IoT Hub’ı cihaz sağlama hizmeti için yeni bir örnek oluşturun

1. Select the **+ Create a resource** button again.

2. *Search the Marketplace* for the **Device Provisioning Service**. Select **IoT Hub Device Provisioning Service** and hit the **Create** button. 

3. Provide the following information for your new Device Provisioning Service instance and hit **Create**.

    * **Ad:** Yeni Cihaz Sağlama Hizmeti örneğiniz için benzersiz bir ad girin. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.
    * **Abonelik**: Bu Cihaz Sağlama Hizmeti örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin.
    * **Kaynak grubu:** Bu alan yeni örneği içerecek yeni kaynak grubunu oluşturmanızı veya mevcut bir kaynak grubunu seçmenizi sağlar. Yukarıda oluşturduğunuz IoT hub'ını içeren kaynak grubunu seçin; örneğin, **TestResources**. İlgili tüm kaynakları aynı gruba birlikte koyarak, bunları birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir. For more information, see [Manage Azure Resource Manager resource groups](../azure-resource-manager/manage-resource-groups-portal.md).
    * **Konum**: Cihazlarınıza en yakın konumu seçin.

      ![Enter basic information about your Device Provisioning Service instance in the portal blade](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Select the notification button to monitor the creation of the resource instance. Once the service is successfully deployed, select **Pin to dashboard**, and then **Go to resource**.

    ![Dağıtımı izleme bildirimi](./media/quick-setup-auto-provision/pin-to-dashboard.png)

## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>Link the IoT hub and your Device Provisioning Service

In this section, you will add a configuration to the Device Provisioning Service instance. Bu yapılandırma, cihazların sağlanacağı IOT hub'ı ayarlar.

1. Select the **All resources** button from the left-hand menu of the Azure portal. Önceki bölümde oluşturduğunuz Cihaz Sağlama Hizmeti örneğini seçin.  

2. From the Device Provisioning Service's menu, select **Linked IoT hubs**. Hit the **+ Add** button seen at the top. 

3. **IoT hub'a bağlantı ekle** sayfasında, yeni Cihaz Sağlama Hizmeti örneğinizi IoT hub'a bağlamak için aşağıdaki bilgileri sağlayın. Then hit **Save**. 

    * **Abonelik:** Yeni Cihaz Sağlama Hizmeti örneğinizle bağlamak istediğiniz IoT hub'ı içeren aboneliği seçin.
    * **Iot hub'ı:** Yeni Cihaz Sağlama Hizmeti örneğinize bağlanacak IoT hub'ı seçin.
    * **Erişim İlkesi:** IoT hub'ı ile bağlantı oluşturmak için kimlik bilgileri olarak **iothubowner** öğesini seçin.  

      ![Link the hub name to link to the Device Provisioning Service instance in the portal blade](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Seçili hub’ı **Bağlantılı IoT hub'ları** dikey penceresinde görürsünüz. You might need to hit **Refresh** for it to show up.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. From the left-hand menu in the Azure portal, select **All resources** and then select your Device Provisioning Service. At the top of the device detail pane, select **Delete**.  
2. From the left-hand menu in the Azure portal, select **All resources** and then select your IoT hub. At the top of the hub detail pane, select **Delete**.  

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you’ve deployed an IoT hub and a Device Provisioning Service instance, and linked the two resources. To learn how to use this setup to provision a simulated device, continue to the quickstart for creating a simulated device.

> [!div class="nextstepaction"]
> [Quickstart to create a simulated device](./quick-create-simulated-device.md)
