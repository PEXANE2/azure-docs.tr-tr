---
title: Azure portal cihaz sağlama hizmeti IoT Hub ayarlama
description: Hızlı başlangıç-Azure portal Azure IoT Hub cihaz sağlama hizmeti 'ni (DPS) ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 765b7e5bb7341974a6a16d3f1ed0bcabf415f9f3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76029170"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Hızlı başlangıç: IoT Hub cihaz sağlama hizmetini Azure portal ile ayarlama

IoT Hub cihaz sağlama hizmeti, istenen IoT Hub 'ına insan müdahalesine gerek kalmadan tam zamanında sağlamayı etkinleştirmek için IoT Hub ile birlikte kullanılabilir ve müşterilerin Milyonlarca IoT cihazını güvenli ve ölçeklenebilir bir şekilde sağlamasını sağlar. Azure IoT Hub cihaz sağlama hizmeti, TPM, simetrik anahtar ve X. 509.440 sertifika kimlik doğrulamaları ile IoT cihazlarını destekler. Daha fazla bilgi için lütfen [IoT Hub cihaz sağlama hizmetine genel bakış](./about-iot-dps.md) bölümüne bakın

Bu hızlı başlangıçta, aşağıdaki adımlarla cihazlarınızı sağlamak için Azure portalında IoT Hub cihaz sağlama hizmetini ayarlamayı öğreneceksiniz:
> [!div class="checklist"]
> * IoT Hub oluşturmak için Azure portal kullanın
> * IoT Hub Cihazı Sağlama Hizmeti oluşturmak ve kimlik kapsamını almak için Azure portalını kullanma
> * IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]


## <a name="create-a-new-iot-hub-device-provisioning-service"></a>Yeni bir IoT Hub cihaz sağlama hizmeti oluşturun

1. **+ Kaynak oluştur** düğmesini yeniden seçin.

2. **Cihaz sağlama hizmeti**için *Market 'te arama* yapın. **IoT Hub cihaz sağlama hizmeti** ' ni seçin ve **Oluştur** düğmesine basın. 

3. Yeni cihaz sağlama hizmeti örneğiniz için aşağıdaki bilgileri sağlayın ve **Oluştur**' a basın.

    * **Ad:** Yeni Cihaz Sağlama Hizmeti örneğiniz için benzersiz bir ad girin. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.
    * **Abonelik**: Bu Cihaz Sağlama Hizmeti örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin.
    * **Kaynak grubu:** Bu alan yeni örneği içerecek yeni kaynak grubunu oluşturmanızı veya mevcut bir kaynak grubunu seçmenizi sağlar. Yukarıda oluşturduğunuz IoT hub'ını içeren kaynak grubunu seçin; örneğin, **TestResources**. İlgili tüm kaynakları aynı gruba birlikte koyarak, bunları birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir. Daha fazla bilgi için bkz. [Azure Resource Manager kaynak gruplarını yönetme](../azure-resource-manager/management/manage-resource-groups-portal.md).
    * **Konum:** Cihazlarınız için en yakın konumu seçin.

      ![Portal dikey penceresinde cihaz sağlama hizmeti örneğiniz hakkındaki temel bilgileri girin](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Kaynak örneğinin oluşturulmasını izlemek için bildirim düğmesini seçin. Hizmet başarıyla dağıtıldıktan sonra **panoya sabitle**' yi seçin ve ardından kaynak ' a **gidin**.

    ![Dağıtımı izleme bildirimi](./media/quick-setup-auto-provision/pin-to-dashboard.png)

## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>IoT Hub 'ı ve cihaz sağlama hizmetinizi bağlama

Bu bölümde, cihaz sağlama hizmeti örneğine bir yapılandırma ekleyeceksiniz. Bu yapılandırma, cihazların sağlanacağı IOT hub'ı ayarlar.

1. Azure portal sol taraftaki menüden **tüm kaynaklar** düğmesini seçin. Önceki bölümde oluşturduğunuz Cihaz Sağlama Hizmeti örneğini seçin. 

    Menü, Portal ayarlarında **yerleşik** mod yerine **açılır pencere** kullanılarak yapılandırıldıysa sol üstteki 3 satıra tıklayarak Portal menüsünü sol tarafta açmanız gerekir.  

2. Cihaz sağlama hizmeti menüsünden **bağlantılı IoT Hub 'ları**' nı seçin. En üstte görülen **+ Ekle** düğmesine basın. 

3. **IoT hub'a bağlantı ekle** sayfasında, yeni Cihaz Sağlama Hizmeti örneğinizi IoT hub'a bağlamak için aşağıdaki bilgileri sağlayın. Sonra **Kaydet**' e basın. 

    * **Abonelik:** Yeni Cihaz Sağlama Hizmeti örneğinizle bağlamak istediğiniz IoT hub'ı içeren aboneliği seçin.
    * **Iot hub'ı:** Yeni Cihaz Sağlama Hizmeti örneğinize bağlanacak IoT hub'ı seçin.
    * **Erişim İlkesi:** IoT hub'ı ile bağlantı oluşturmak için kimlik bilgileri olarak **iothubowner** öğesini seçin.  

      ![Portal dikey penceresinde cihaz sağlama hizmeti örneğine bağlanmak için hub adını bağlayın](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Seçili hub’ı **Bağlantılı IoT hub'ları** dikey penceresinde görürsünüz. Gösterilmesi için **yenileme** isabet etmeniz gerekebilir.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Cihaz ayrıntısı bölmesinin üst kısmında **Sil**' i seçin.  
2. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. Merkez ayrıntısı bölmesinin üst kısmında **Sil**' i seçin.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT Hub 'ı ve bir cihaz sağlama hizmeti örneği dağıttık ve iki kaynağı bağladınız. Sanal bir cihaz sağlamak üzere bu kurulumu nasıl kullanacağınızı öğrenmek için, sanal cihaz oluşturma hızlı başlangıç ile devam edin.

> [!div class="nextstepaction"]
> [Sanal cihaz oluşturmak için hızlı başlangıç](./quick-create-simulated-device-symm-key.md)
