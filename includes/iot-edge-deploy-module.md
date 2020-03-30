---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/31/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: abbde0cb8137650faad563248c8fafe7dc0f469a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75564669"
---
Azure IoT Edge'in temel özelliklerinden biri, buluttan IoT Edge aygıtlarınıza kod dağıtabilmektir. **IoT Edge modülleri** kapsayıcı olarak uygulanan çalıştırılabilir paketlerdir. Bu bölümde, Azure Marketi'nin [IoT Edge Modülleri bölümünden](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) doğrudan Azure IoT Hub'ınızdan önceden oluşturulmuş bir modül dağıtırsınız.

Bu bölümde dağıttığınız modül bir sensörü simüle eder ve oluşturulan verileri gönderir. Bu modül, ioT Edge ile başlarken yararlı bir kod dur, çünkü benzetimli verileri geliştirme ve sınama için kullanabilirsiniz. Bu modülün tam olarak ne yaptığını görmek istiyorsanız, [simüle edilen sıcaklık sensörü kaynak kodunu](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs)görüntüleyebilirsiniz.

Azure Marketi'nden ilk modülünüzü dağıtmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.

1. Otomatik **Aygıt Yönetimi**altında sol bölmedeki menüden **IoT Edge'i**seçin.

1. Aygıtlar listesinden hedef aygıtın aygıt kimliğine tıklayın.

1. Üst çubuğunda **Modülleri Ayarla'yı**seçin.

1. Sayfanın **IoT Kenar Modülleri** bölümünde **Ekle'yi**tıklatın.

1. Açılan menüden **Pazar Yeri Modülü'nü**seçin.

   ![Azure portalında Sıcaklık Sensörü Simülasyonu araması](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

1. **IoT Edge Modül Pazarı'nda**"Simüle Sıcaklık Sensörü"nü arayın ve bu modülü seçin.

1. Simüle TemperatureSensor modülüotomatik doldurulur dikkat edin. Öğreticilerde, dağıtımınıza ek modüller eklemek için bu sayfayı kullanırsınız. Bu hızlı başlangıç için, yalnızca bu bir modülü dağıtın. Herkese açık olduğu için kimlik bilgileri gerekmez.

   ![Modülleri cihazda ayarlama](./media/iot-edge-deploy-module/set-modules-on-device.png)

   **Sonraki'ni seçin:** Sihirbazın bir sonraki adımına devam etmek için rotalar.

1. Sihirbazın **Rotalar** sekmesinde, iletilerin modüller ve IoT Hub'ı arasında nasıl geçirildiğini tanımlarsınız. İletiler ad/değer çiftleri kullanılarak oluşturulur. Hızlı başlangıç için, tüm modüllerden gelen tüm iletilerin IoT`$upstream`Hub'a gitmesini istiyorsunuz ( ). Otomatik olarak doldurulmuyorsa, **Ad** `upstream`için **Değer** için aşağıdaki kodu ekleyin:

   ```sql
    FROM /messages/* INTO $upstream
   ```

   **Sonraki'ni seçin:** Derleme + sihirbazın bir sonraki adımına devam etmek için oluşturun.

1. **Sihirbazın Gözden Geçir + oluştur** sekmesinde, IoT Edge aygıtınıza dağıtılan tüm modülleri tanımlayan JSON dosyasını önizleyebilirsiniz. **Simüle TemperatureSensor** modülü ve **edgeAgent** ve **edgeHub**olarak adlandırılan iki ek sistem modülü dahil olduğuna dikkat edin. İncelemeyi bitirdiğinde **Oluştur'u** seçin.

   IoT Edge cihazına yeni bir dağıtım gönderdiğinizde cihazınıza hiçbir şey gönderilmez. Bunun yerine cihaz, IoT Hub'ı düzenli olarak sorgulayarak yeni yönergeler olup olmadığını denetler. Güncelleştirilmiş bir dağıtım bildirimi bulması halinde cihaz yeni dağıtımla ilgili bilgileri kullanarak buluttaki modül görüntülerini çeker ve modülleri yerel ortamda çalıştırmaya başlar. Bu işlem birkaç dakika sürebilir.

1. Modül dağıtım ayrıntılarını oluşturduktan sonra sihirbaz sizi IoT hub'ınızın **IoT Edge** sayfasına döndürür. Ayrıntılarını görmek için IoT Edge cihazları listesinden cihazınızı seçin.

1. Aygıt ayrıntıları sayfasında, **Modüller** sekmesine gidin. Üç modül listelenmelidir: $edgeAgent, $edgeHub ve Simüle TemperatureSensor. Modüllerden biri veya birkaçı dağıtımda belirtildiği şekilde listelenmişse ancak aygıt tarafından bildirilmemişse, IoT Edge aygıtınız bunları başlatmaya devam eder. Birkaç dakika bekleyin ve sayfanın üst kısmında **Yenile'yi** seçin.

   ![Dağıtılan modüller listesinde Simüle Edilen SıcaklıkSensörünü görüntüleyin](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)
