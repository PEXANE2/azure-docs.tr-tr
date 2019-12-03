---
title: Öğretici-Azure IoT Hub cihaz sağlama hizmeti 'ni kullanarak yük dengeli hub 'larda cihaz sağlama
description: Bu öğreticide cihaz sağlama hizmeti 'nin Azure portalındaki yük dengeli IoT Hub 'ları arasında otomatik cihaz sağlamayı nasıl etkinleştirdiğini gösterir
author: wesmc7777
ms.author: wesmc
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: cf1246862a455cfa71f3e170478736e5e07ee6f7
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74701967"
---
# <a name="tutorial-provision-devices-across-load-balanced-iot-hubs"></a>Öğretici: yük dengeli IoT Hub 'larda cihaz sağlama

Bu öğreticide, Cihaz Sağlama Hizmeti kullanılarak birden çok yük dengeli IoT Hub'ı için cihazları sağlama işlemi gösterilir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama 
> * İkinci cihaza bir kayıt listesi girdisi ekleme
> * Cihaz Sağlama Hizmeti ayırma ilkesini **eşit dağılım** olarak ayarlayın
> * Yeni IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, önceki [Hub'a cihaz sağlama](tutorial-provision-device-to-hub.md) öğreticisinin üzerine kurulmuştur.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama

Başka bir IoT Hub'ına ikinci cihazı sağlamak için, [Hub'a cihaz sağlama](tutorial-provision-device-to-hub.md) öğreticisindeki adımları izleyin.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>İkinci cihaza bir kayıt listesi girdisi ekleme

Kayıt listesi Cihaz Sağlama Hizmeti'ne cihazla hangi kanıtlama yöntemini (cihazın kimliğini onaylama yöntemi) kullandığını bildirir. Sonraki adım, ikinci cihaz için bir kayıt listesi girdisi eklemektir. 

1. Cihaz Sağlama Hizmeti sayfanızda **Kayıtları yönetme**'ye tıklayın. **Kayıt listesi girdisi ekleme** sayfası görüntülenir. 
2. Sayfanın en üstündeki **Ekle**'ye tıklayın.
2. Alanları doldurun ve ardından **Kaydet**'e tıklayın.

## <a name="set-the-device-provisioning-service-allocation-policy"></a>Cihaz Sağlama Hizmeti ayırma ilkesini ayarlama

Ayırma ilkesi, bir IoT hub’a cihazların nasıl atandığını belirleyen bir Cihaz Sağlama Hizmeti ayarıdır. Desteklenen üç ayırma ilkesi vardır: 

1. **En düşük gecikme**: Cihaza yönelik en düşük gecikme ile hub’a dayalı bir IoT hub’a cihazlar sağlanabilir.
2. **Eşit ağırlıklı dağılım** (varsayılan): Bağlı IoT hub’lara cihaz sağlanma olasılığı eşittir. Bu varsayılan ayardır. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz. 
3. **Kayıt listesi aracılığıyla statik yapılandırma**: Kayıt listesindeki istenen IoT hub’ın belirtimi, Cihaz Sağlama Hizmeti düzeyindeki ayırma ilkesinden önceliklidir.

Ayırma ilkesini ayarlamak için şu adımları izleyin:

1. Ayırma ilkesini ayarlamak için Cihaz Sağlama Hizmeti sayfasında **Ayırma ilkesini yönetme** seçeneğine tıklayın.
2. Ayırma ilkesi olarak **Eşit ağırlıklı dağılım**'ı ayarlayın.
3. **Kaydet** düğmesine tıklayın.

## <a name="link-the-new-iot-hub-to-the-device-provisioning-service"></a>Yeni IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama

Cihaz Sağlama Hizmetini ve IoT hub'ını, Cihaz Sağlama Hizmeti bu hub'a kaydolabilecek şekilde bağlayın.

1. **Tüm kaynaklar** sayfasında, önceden oluşturduğunuz Cihaz Sağlama Hizmeti'ne tıklayın.
2. Cihaz Sağlama Hizmeti sayfasında **Bağlı IoT hub’lar** seçeneğine tıklayın.
3. **Ekle**'ye tıklayın.
4. **IoT hub'a bağlantı ekleme** sayfasında, bağlı IoT hub’ın geçerli abonelikte mi yoksa farklı bir abonelikte mi bulunduğunu belirtmek için radyo düğmelerini kullanın. Sonra **IoT hub** kutusunda IoT hub’ın adını seçin.
5. **Kaydet** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalını kullanarak ikinci bir IoT Hub'ı için ikinci cihazı sağlama 
> * İkinci cihaza bir kayıt listesi girdisi ekleme
> * Cihaz Sağlama Hizmeti ayırma ilkesini **eşit dağılım** olarak ayarlayın
> * Yeni IoT hub’ı Cihaz Sağlama Hizmeti’ne bağlama

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps]()
-->
