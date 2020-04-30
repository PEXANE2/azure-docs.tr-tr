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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75564669"
---
Azure IoT Edge temel yetilerinden biri, buluttan IoT Edge cihazlarınıza kod dağıtabiyor. **IoT Edge modüller** , kapsayıcı olarak uygulanan yürütülebilir paketlerdir. Bu bölümde, [Azure Marketi 'nin IoT Edge modüller bölümünden](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) doğrudan Azure IoT Hub ' den önceden oluşturulmuş bir modül dağıtırsınız.

Bu bölümde dağıttığınız modül bir algılayıcısı taklit eder ve üretilen verileri gönderir. Bu modül, geliştirme ve test amacıyla sanal verileri kullanabilmeniz için IoT Edge kullanmaya başlarken yararlı bir kod parçasıdır. Bu modülün tam olarak ne yaptığını görmek isterseniz, [sanal sıcaklık algılayıcısı kaynak kodunu](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs)görüntüleyebilirsiniz.

Azure Marketi 'nden ilk modülünüzü dağıtmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi**altında **IoT Edge**' yi seçin.

1. Cihazların listesinden hedef cihazın cihaz KIMLIĞINE tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.

1. Aşağı açılan menüden **Market modülü**' nü seçin.

   ![Azure portalında Sıcaklık Sensörü Simülasyonu araması](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

1. **IoT Edge Module marketi**' nde, "sanal sıcaklık algılayıcısı" araması yapın ve bu modülü seçin.

1. SimulatedTemperatureSensor modülünün otomatik olarak doldurulduğuna dikkat edin. Öğreticilerde, dağıtımınıza ek modüller eklemek için bu sayfayı kullanın. Bu hızlı başlangıçta yalnızca bu bir modülü dağıtın. Ortak olduğundan kimlik bilgileri gerekli değildir.

   ![Cihazda modül ayarla](./media/iot-edge-deploy-module/set-modules-on-device.png)

   **İleri ' yi seçin:** Sihirbazın bir sonraki adımına devam etmek için yollar.

1. Sihirbazın **rotalar** sekmesinde, iletilerin modüller arasında nasıl geçtiğini tanımlar ve IoT Hub. İletiler ad/değer çiftleri kullanılarak oluşturulur. Hızlı başlangıç için tüm modüllerden gelen tüm iletilerin IoT Hub (`$upstream`) konumuna gitmesini istiyorsunuz. Otomatik olarak doldurulmamışsa, **adın** `upstream` **değeri** için aşağıdaki kodu ekleyin:

   ```sql
    FROM /messages/* INTO $upstream
   ```

   **İleri ' yi seçin:** Sihirbazın bir sonraki adımına geçmek için ve Oluştur ' a tıklayın.

1. Sihirbazın **gözden geçir + oluştur** sekmesinde, IoT Edge cihazınıza dağıtılan tüm MODÜLLERI tanımlayan JSON dosyasının önizlemesini yapabilirsiniz. **SimulatedTemperatureSensor** modülünün dahil edildiğini ve **edgeagent** ve **edgehub**adlı iki ek sistem modülü olduğunu unutmayın. Gözden geçirmeyi bitirdiğinizde **Oluştur** ' u seçin.

   IoT Edge cihazına yeni bir dağıtım gönderdiğinizde cihazınıza hiçbir şey gönderilmez. Bunun yerine cihaz, IoT Hub'ı düzenli olarak sorgulayarak yeni yönergeler olup olmadığını denetler. Güncelleştirilmiş bir dağıtım bildirimi bulması halinde cihaz yeni dağıtımla ilgili bilgileri kullanarak buluttaki modül görüntülerini çeker ve modülleri yerel ortamda çalıştırmaya başlar. Bu işlem birkaç dakika sürebilir.

1. Modül dağıtım ayrıntılarını oluşturduktan sonra, sihirbaz sizi IoT Hub 'ınızın **IoT Edge** sayfasına döndürür. Ayrıntılarını görmek için IoT Edge cihazları listesinden cihazınızı seçin.

1. Cihaz ayrıntıları sayfasında, **modüller** sekmesine gidin. Üç modülün listelenmesi gerekir: $edgeAgent, $edgeHub ve SimulatedTemperatureSensor. Bir veya daha fazla modül dağıtımda belirtilen şekilde listeleniyorsa, ancak cihaz tarafından bildirilmediyse, IoT Edge cihazınız hala başlatılıyor demektir. Birkaç dakika bekleyip sayfanın üst kısmında **Yenile** ' yi seçin.

   ![Dağıtılan modüller listesinde SimulatedTemperatureSensor görüntüle](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)
