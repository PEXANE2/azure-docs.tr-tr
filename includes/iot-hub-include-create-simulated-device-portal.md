---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/26/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d8f8021925ac9c9e427dd6571ecaa1a30c85497
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67188993"
---
<!-- This is the instructions for creating a simulated device you can use for testing routing.-->

Sonra bir cihaz kimliği oluşturun ve anahtarını daha sonra kullanmak üzere kaydedin. Bu cihaz kimliği simülasyon uygulaması tarafından IoT hub'ına iletileri göndermek için kullanılır. Bu özellik PowerShell'de veya Azure Kaynak Yöneticisi şablonu kullanırken kullanılamaz. Aşağıdaki adımlar, [Azure portalını](https://portal.azure.com)kullanarak simüle edilen aygıtı nasıl oluşturabileceğinizi size söyler.

1. [Azure portalını](https://portal.azure.com) açın ve Azure hesabınızla oturum açın.

2. **Kaynak gruplarını** seçin ve ardından kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

3. Kaynaklar listesinde IoT hub'ınızı seçin. Bu öğreticide **ContosoTestHub** kullanılır. Hub bölmesinden **IoT Cihazları**'nı seçin.

4. **+ Ekle** öğesini seçin. Cihaz Ekle bölmesinde cihaz kimliğini girin. Bu öğreticide **Contoso-Test-Device** kullanılır. Anahtarları boş bırakın ve **Anahtarları Otomatik Olarak Oluştur**'a tıklayın. **Cihazı IoT Hub'ına bağla** seçeneğinin etkinleştirildiğinden emin olun. **Kaydet'i**seçin.

   ![Add-device ekranı](./media/iot-hub-include-create-simulated-device-portal/add-device.png)

5. Oluşturulduğuna göre, oluşturulan anahtarları görmek için aygıtı seçin. Birincil tuşu üzerindeki Kopya simgesini seçin ve bu öğreticinin test aşaması için Not Defteri gibi bir yere kaydedin.

   ![Anahtarlar da dahil olmak üzere cihaz ayrıntıları](./media/iot-hub-include-create-simulated-device-portal/device-details.png)