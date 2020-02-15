---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/13/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: bbdafde85097d0052edd5984b594fd37066dc1e6
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77279604"
---
Bu bölümde [Azure Portal](https://portal.azure.com)kullanarak IoT Hub 'ı oluşturma açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. Azure giriş sayfasından **+ kaynak oluştur** düğmesini seçin ve ardından **Market 'te ara** alanına *IoT Hub* girin.

1. Arama sonuçlarından **IoT Hub** seçin ve ardından **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, alanları aşağıdaki gibi doldurun:

   - **Abonelik**: hub 'ınız için kullanılacak aboneliği seçin.

   - **Kaynak grubu**: bir kaynak grubu seçin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için **Yeni oluştur** ' u seçin ve kullanmak istediğiniz adı girin. Var olan bir kaynak grubunu kullanmak için bu kaynak grubunu seçin. Daha fazla bilgi için bkz. [Azure Resource Manager kaynak gruplarını yönetme](../articles/azure-resource-manager/management/manage-resource-groups-portal.md).

   - **Bölge**: hub 'ınızın bulunmasını istediğiniz bölgeyi seçin. Size en yakın konumu seçin.

   - **IoT Hub adı**: hub 'ınız için bir ad girin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Azure portal hub oluşturma](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics.png)

1. **İleri ' yi seçin:** hub 'ınızı oluşturmaya devam etmek için boyut ve ölçek.

   ![Azure portal kullanarak yeni bir hub için boyut ve ölçek ayarlama](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

    Bu ekran aşağıdaki değerleri ayarlamanıza olanak sağlar:

    - **Fiyatlandırma ve ölçek katmanı**: seçtiğiniz katman. İstediğiniz sayıda özelliğe ve her gün çözümünüz aracılığıyla kaç tane ileti gönderdiğinize bağlı olarak çeşitli katmanlardan seçim yapabilirsiniz. Ücretsiz katman, test ve değerlendirme için tasarlanmıştır. 500 cihazların hub 'a ve günde en fazla 8.000 iletiye bağlanmasını sağlar. Her Azure aboneliği ücretsiz katmanda bir IoT Hub 'ı oluşturabilir.

    - **IoT Hub birimler**: günlük birim başına izin verilen ileti sayısı, hub 'ın fiyatlandırma katmanına bağlıdır. Örneğin, hub 'ın 700.000 ileti girişini desteklemesini istiyorsanız iki adet S1 katmanı birimi seçersiniz.
    Diğer katman seçenekleri hakkında daha fazla bilgi için, bkz. [doğru IoT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

    - **Azure Güvenlik Merkezi**: IoT ve cihazlarınıza ek bir tehdit koruması katmanı eklemek için bunu açın. Bu seçenek ücretsiz katmanda hub 'lar için kullanılamaz. Bu özellik hakkında daha fazla bilgi için bkz. [IoT Için Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/asc-for-iot/).

    - **Gelişmiş ayarlar** **cihazdan buluta bölümler** > : Bu özellik cihazdan buluta iletileri, iletilerin eşzamanlı okuyucu sayısıyla ilişkilendirir. Çoğu hub 'da yalnızca dört bölüm gereklidir.

1. Bu makalede, varsayılan seçimleri kabul edin ve ardından İleri **: Etiketler** ' i seçerek sonraki ekrana geçin.

    Etiketler ad/değer çiftleridir. Kaynakları kategorilere ayırarak ve faturalandırmayı birleştirmek için birden fazla kaynağa ve kaynak grubuna aynı etiketi atayabilirsiniz.

   ![Azure portal kullanarak yeni bir hub için boyut ve ölçek ayarlama](./media/iot-hub-include-create-hub/iot-hub-create-tabs.png)

    **İleri ' yi seçin:** seçimlerinizi gözden geçirmek için ve Oluştur ' a tıklayın. Bu ekrana benzer bir şey görürsünüz.

   ![Yeni hub oluşturma bilgilerini gözden geçirin](./media/iot-hub-include-create-hub/iot-hub-create-review.png)

1. Yeni hub 'ınızı oluşturmak için **Oluştur** ' u seçin. Hub 'ın oluşturulması birkaç dakika sürer.
