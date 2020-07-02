---
title: dosya dahil etme
description: dosya dahil etme
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/30/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 76c8eb7acf20d8cf68d3573defd947efbc6c3c43
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85801745"
---
Azure IoT Edge temel yetilerinden biri, buluttan IoT Edge cihazlarınıza kod dağıtabiyor. *IoT Edge modüller* , kapsayıcı olarak uygulanan yürütülebilir paketlerdir. Bu bölümde, [Azure Marketi 'nin IoT Edge modüller bölümünden](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) doğrudan Azure IoT Hub ' den önceden oluşturulmuş bir modül dağıtırsınız.

Bu bölümde dağıttığınız modül bir algılayıcısı taklit eder ve üretilen verileri gönderir. Bu modül, geliştirme ve test amacıyla sanal verileri kullanabilmeniz için IoT Edge kullanmaya başlarken yararlı bir kod parçasıdır. Bu modülün tam olarak ne yaptığını görmek isterseniz, [sanal sıcaklık algılayıcısı kaynak kodunu](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs)görüntüleyebilirsiniz.

Azure Marketi 'nden ilk modülünüzü dağıtmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.

1. Sol bölmedeki menüden, **otomatik cihaz yönetimi**altında **IoT Edge**' yi seçin.

1. Cihazların listesinden hedef cihazın cihaz KIMLIĞINE tıklayın.

1. Üstteki çubukta **modülleri ayarla**' yı seçin.

   ![Cihaz ayrıntıları sayfasından modülleri ayarla ' yı seçin.](./media/iot-edge-deploy-module/select-set-modules.png)

1. Sayfanın **IoT Edge modüller** bölümünde, **Ekle** ' ye tıklayın ve açılan menüden **Market modülü** ' nü seçin.

   ![Market modülü Ekle](./media/iot-edge-deploy-module/add-marketplace-module.png)

1. **IoT Edge Module marketi**' nde, "sanal sıcaklık algılayıcısı" araması yapın ve bu modülü seçin.

1. SimulatedTemperatureSensor modülünün, istenen durumu **çalıştıran**IoT Edge modüller bölümüne eklendiğini unutmayın.

   **İleri ' yi seçin:** Sihirbazın bir sonraki adımına devam etmek için yollar.

   ![Sıcaklık Algılayıcı modülü listelendikten sonra bir sonraki adıma geçin](./media/iot-edge-deploy-module/view-temperature-sensor-next-routes.png)

1. Sihirbazın **rotalar** sekmesinde, iletilerin modüller arasında nasıl geçtiğini tanımlayabilir ve IoT Hub. Yollar ad/değer çiftleri kullanılarak oluşturulur. Bu sayfada iki yol görmeniz gerekir. **Yol** olarak adlandırılan varsayılan yol, tüm iletileri IoT Hub gönderir (denir `$upstream` ). **SimulatedTemperatureSensorToIoTHub** adlı ikinci bir yol, marketten modül eklediğinizde otomatik olarak oluşturulmuştur. Bu yol, tüm iletileri özellikle sanal sıcaklık modülünden IoT Hub olarak gönderir. Bu durumda gereksiz olduğu için varsayılan yolu silebilirsiniz.

   **İleri ' yi seçin:** Sihirbazın bir sonraki adımına geçmek için ve Oluştur ' a tıklayın.

   ![Varsayılan yolu silin ve sonraki adıma geçin](./media/iot-edge-deploy-module/delete-route-next-review-create.png)

1. Sihirbazın **gözden geçir + oluştur** sekmesinde, IoT Edge cihazınıza dağıtılan tüm MODÜLLERI tanımlayan JSON dosyasının önizlemesini yapabilirsiniz. **SimulatedTemperatureSensor** modülünün, **edgeagent** ve **edgehub**olmak üzere iki çalışma zamanı modülünü de dahil edildiğini unutmayın. Gözden geçirmeyi bitirdiğinizde **Oluştur** ' u seçin.

   IoT Edge cihazına yeni bir dağıtım gönderdiğinizde cihazınıza hiçbir şey gönderilmez. Bunun yerine cihaz, IoT Hub'ı düzenli olarak sorgulayarak yeni yönergeler olup olmadığını denetler. Güncelleştirilmiş bir dağıtım bildirimi bulması halinde cihaz yeni dağıtımla ilgili bilgileri kullanarak buluttaki modül görüntülerini çeker ve modülleri yerel ortamda çalıştırmaya başlar. Bu işlem birkaç dakika sürebilir.

1. Modül dağıtım ayrıntılarını oluşturduktan sonra, sihirbaz sizi cihaz ayrıntıları sayfasına döndürür. Cihaz ayrıntıları sayfasında, **modüller** sekmesinde dağıtım durumunu görüntüleyin. Üç modülün listelenmesi gerekir: $edgeAgent, $edgeHub ve SimulatedTemperatureSensor. Bir veya daha fazla modül dağıtımda belirtilen şekilde listeleniyorsa, ancak cihaz tarafından bildirilmediyse, IoT Edge cihazınız hala başlatılıyor demektir. Birkaç dakika bekleyip sayfanın üst kısmında **Yenile** ' yi seçin.

   ![Dağıtılan modüller listesinde SimulatedTemperatureSensor görüntüle](./media/iot-edge-deploy-module/view-deployed-modules.png)
