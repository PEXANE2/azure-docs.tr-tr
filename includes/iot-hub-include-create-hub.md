---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: bf63a9fb552b2a41ce7b9945faeda362592d8efb
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558721"
---
Bu bölümde [Azure Portal](https://portal.azure.com)kullanarak IoT Hub 'ı oluşturma açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Kaynak oluştur**' u seçin ve ardından Market ' i **Ara** alanına *IoT Hub* girin.

1. Arama sonuçlarından **IoT Hub** seçin ve ardından **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, alanları aşağıdaki gibi doldurun:

   - **Abonelik**: Hub 'ınız için kullanılacak aboneliği seçin.

   - **Kaynak grubu**: Bir kaynak grubu seçin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için **Yeni oluştur** ' u seçin ve kullanmak istediğiniz adı girin. Var olan bir kaynak grubunu kullanmak için bu kaynak grubunu seçin. Daha fazla bilgi için bkz. [Azure Resource Manager kaynak gruplarını yönetme](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   - **Bölge**: Hub 'ınızın bulunmasını istediğiniz bölgeyi seçin. Size en yakın konumu seçin.

   - **IoT Hub adı**: Hub 'ınız için bir ad girin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Azure portal hub oluşturma](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics-vs2019.png)

1. İleri **' yi seçin: Hub 'ınızı oluşturmaya** devam etmek için boyut ve ölçek.

   ![Azure portal kullanarak yeni bir hub için boyut ve ölçek ayarlama](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

    Bu ekran aşağıdaki değerleri ayarlamanıza olanak sağlar:

    - **Fiyatlandırma ve ölçek katmanı**: Seçtiğiniz katman. İstediğiniz sayıda özelliğe ve her gün çözümünüz aracılığıyla kaç tane ileti gönderdiğinize bağlı olarak çeşitli katmanlardan seçim yapabilirsiniz. Ücretsiz katman, test ve değerlendirme için tasarlanmıştır. 500 cihazların hub 'a ve günde en fazla 8.000 iletiye bağlanmasını sağlar. Her Azure aboneliği ücretsiz katmanda bir IoT Hub oluşturabilir.

    - **IoT Hub birimleri**: Günlük birim başına izin verilen ileti sayısı, hub 'ın fiyatlandırma katmanına bağlıdır. Örneğin, hub 'ın 700.000 ileti girişini desteklemesini istiyorsanız iki adet S1 katmanı birimi seçersiniz.
    Diğer katman seçenekleri hakkında daha fazla bilgi için, bkz. [doğru IoT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

    - **Gelişmiş ayarlar** > **cihazdan buluta bölümler**: Bu özellik cihazdan buluta iletileri, iletilerin eşzamanlı okuyucu sayısıyla ilişkilendirir. Çoğu hub 'da yalnızca dört bölüm olması gerekir.

1. Bu öğreticide, varsayılan seçimleri kabul edin ve ardından seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Bu ekrana benzer bir şey görürsünüz.

   ![Yeni hub oluşturma bilgilerini gözden geçirin](./media/iot-hub-include-create-hub/iot-hub-create-review-vs2019.png)

1. Yeni hub 'ınızı oluşturmak için **Oluştur** ' u seçin. Hub 'ın oluşturulması birkaç dakika sürer.
