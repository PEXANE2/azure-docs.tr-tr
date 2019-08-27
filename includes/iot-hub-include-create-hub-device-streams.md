---
title: içerme dosyası (cihaz akışları Önizleme)
description: içerme dosyası (cihaz akışları Önizleme)
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a02c16a75bbdf4827ce6a6c5f50751caf9c5aab8
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "67446020"
---
Bu bölümde, [Azure Portal](https://portal.azure.com)kullanarak IoT Hub 'ı oluşturma açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Kaynak oluştur**' u seçin ve ardından **nesnelerin interneti**' yi seçin.

1. Sağdaki listede **IoT Hub**' ı seçin. IoT Hub 'ı oluşturmaya yönelik ilk sayfa açılır.

   ![Azure portal IoT Hub 'ı oluşturma](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-01.png)

   Şu alanları doldurun:

   a. **Abonelik** açılan listesinde, IoT Hub 'ınız için kullanılacak aboneliği seçin.

   b. **Kaynak grubu**için aşağıdakilerden birini yapın: 
      * Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve kullanmak istediğiniz adı girin. 
      * Mevcut bir kaynak grubunu kullanmak için **Varolanı kullan** ' ı seçin ve ardından aşağı açılan listeden kaynak grubunu seçin. 
      
        Daha fazla bilgi için bkz. [Azure Resource Manager kaynak gruplarını yönetme](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   c. **Bölge** açılan listesinde, hub 'ınızın bulunmasını istediğiniz bölgeyi seçin. **Orta ABD** veya **Orta ABD euap**IoT Hub cihaz akışları önizlemesini destekleyen bir bölge seçin.

   d. **IoT Hub adı** kutusuna IoT Hub 'ınızın adını girin. Adın genel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

1. IoT Hub 'ınızı oluşturmaya devam etmek için İleri **'yi seçin: Boyut ve ölçek**.

   ![Azure portal kullanarak yeni bir IoT Hub için boyut ve ölçek ayarlama](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   Bu bölmede, varsayılan ayarları kabul edebilir ve alt kısımdaki **gözden geçir + oluştur** ' u seçebilirsiniz. Aşağıdaki seçenekleri göz önünde bulundurun:

   * **Fiyatlandırma ve ölçek katmanı** açılan listesinde, standart katmanlardan birini (**S1**, **S2**veya **S3**) veya **F1 ' i seçin: Ücretsiz katman**. Bu seçenek ayrıca, dünyanın her tarafında, kullandığınız akış olmayan iş yüklerinizin ve (örneğin telemetri iletileri) boyutu ile de yapılabilir. Örneğin, ücretsiz katman test ve değerlendirme için tasarlanmıştır. 500 cihazların IoT Hub 'ına ve günde en fazla 8.000 iletiye bağlanmasını sağlar. Her Azure aboneliği ücretsiz katmanda bir IoT Hub 'ı oluşturabilir. 

   * **IoT Hub birim sayısı**için: Bu seçenek, hub 'ınızda bekleyen akış olmayan iş yüküne bağlıdır. Şimdilik 1 seçeneğini belirleyebilirsiniz.

   Katman seçenekleri hakkında daha fazla bilgi için bkz. [doğru IoT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

1. Seçimlerinizi gözden geçirmek için, **gözden geçir + oluştur** sekmesini seçin. Açılan pencere şuna benzer:

   ![Yeni IoT Hub 'ı oluşturma bilgileri](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

1. Yeni IoT Hub 'ınızı oluşturmak için **Oluştur**' u seçin. İşlem birkaç dakika sürer.
