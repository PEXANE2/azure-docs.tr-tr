---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/14/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: cc53f7ae6eb6254eaa05bf643ecfa0188650df95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77461886"
---
Bu bölümde, [Azure portalını](https://portal.azure.com)kullanarak bir IoT hub'ı nasıl oluşturulacak açıklanmaktadır.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure ana sayfasından **+ Kaynak Oluştur** düğmesini seçin ve ardından Pazar Yeri **Arama** alanına *IoT Hub'ı* girin.

1. Arama sonuçlarından **IoT Hub'ını** seçin ve ardından **Oluştur'u**seçin.

1. Temel **Bilgiler** sekmesinde, alanları aşağıdaki gibi tamamlayın:

   - **Abonelik**: Hub'ınız için kullanılacak aboneliği seçin.

   - **Kaynak Grubu**: Bir kaynak grubu seçin veya yeni bir grup oluşturun. Yeni bir tane oluşturmak için **yeni oluştur'u** seçin ve kullanmak istediğiniz adı doldurun. Varolan bir kaynak grubunu kullanmak için bu kaynak grubunu seçin. Daha fazla bilgi için [bkz.](../articles/azure-resource-manager/management/manage-resource-groups-portal.md)

   - **Bölge**: Merkezinizin bulunmasını istediğiniz bölgeyi seçin. Size en yakın konumu seçin. [IoT Hub aygıt akışları](../articles/iot-hub/iot-hub-device-streams-overview.md)gibi bazı özellikler yalnızca belirli bölgelerde kullanılabilir. Bu sınırlı özellikler için desteklenen bölgelerden birini seçmeniz gerekir.

   - **IoT Hub Adı**: Hub'ınız için bir ad girin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Azure portalında hub oluşturma](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics.png)

1. Sonraki'ni seçin: Hub'ınızı oluşturmaya devam etmek için **boyut ve ölçeklendi.**

   ![Azure portalını kullanarak yeni bir hub'ın boyutunu ve ölçeğini ayarlama](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

   Varsayılan ayarları burada kabul edebilirsiniz. İstenirseniz, aşağıdaki alanlardan herhangi birini değiştirebilirsiniz: 

    - **Fiyatlandırma ve ölçek katmanı**: Seçtiğiniz katman. İstediğiniz özellik kaç kişiye ve çözümünüz aracılığıyla günde kaç ileti gönderdiğinize bağlı olarak birkaç katman arasından seçim yapabilirsiniz. Ücretsiz katman test ve değerlendirme için tasarlanmıştır. Hub'a 500 aygıtın bağlanmasına ve günde 8.000'e kadar iletiye izin verir. Her Azure aboneliği, ücretsiz katmanda bir IoT hub'ı oluşturabilir. 

      IoT Hub aygıt akışları için bir Quickstart üzerinden çalışıyorsanız, boş katmanı seçin.

    - **IoT Hub birimleri**: Günlük birim başına izin verilen ileti sayısı hub'ınızın fiyatlandırma katmanına bağlıdır. Örneğin, hub'ın 700.000 ileti girişini desteklemesini istiyorsanız, iki S1 katmanı birimi seçersiniz.
    Diğer katman seçenekleri hakkında ayrıntılı bilgi için, [doğru IoT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md)ye bakın.

    - **Azure Güvenlik Merkezi**: IoT ve cihazlarınız için ek bir tehdit koruması katmanı eklemek için bunu açın. Bu seçenek, boş katmandaki hub'lar için kullanılamaz. Bu özellik hakkında daha fazla bilgi için [IoT için Azure Güvenlik Merkezi'ne](https://docs.microsoft.com/azure/asc-for-iot/)bakın.

    - **Gelişmiş Ayarlar** > **Aygıttan buluta bölümler**: Bu özellik, aygıttan buluta iletileri iletilerin eşzamanlı okuyucu sayısıyla ilişkilendirir. Hub'ların çoğu yalnızca dört bölüme ihtiyaç duyar.

1.  **Sonraki'ni seçin: Sonraki** ekrana devam etmek için etiketler.

    Etiketler ad/değer çiftleridir. Kaynakları kategorilere ayırmak ve faturalandırmayı birleştirmek için aynı etiketi birden çok kaynağa ve kaynak grubuna atayabilirsiniz. daha fazla bilgi için [Azure kaynaklarınızı düzenlemek için etiketleri kullan'a](../articles/azure-resource-manager/management/tag-resources.md)bakın.

    ![Azure portalını kullanarak hub için etiket atama](./media/iot-hub-include-create-hub/iot-hub-create-tabs.png)

1.  Sonraki'ni seçin: Seçimlerinizi gözden geçirmek için **Gözden Geçir + oluşturun.** Bu ekrana benzer bir şey görürsünüz, ancak hub'ı oluştururken seçtiğiniz değerlerle. 

    ![Yeni hub oluşturmak için bilgileri gözden geçirme](./media/iot-hub-include-create-hub/iot-hub-create-review.png)

1.  Yeni hub'ınızı oluşturmak için **Oluştur'u** seçin. Hub'ı oluşturmak birkaç dakika sürer.
