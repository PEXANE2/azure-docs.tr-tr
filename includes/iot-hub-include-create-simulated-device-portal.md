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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67188993"
---
<!-- This is the instructions for creating a simulated device you can use for testing routing.-->

Sonra bir cihaz kimliği oluşturun ve anahtarını daha sonra kullanmak üzere kaydedin. Bu cihaz kimliği simülasyon uygulaması tarafından IoT hub'ına iletileri göndermek için kullanılır. Bu özellik, PowerShell 'de veya Azure Resource Manager şablonu kullanılırken kullanılamaz. Aşağıdaki adımlarda, [Azure Portal](https://portal.azure.com)kullanarak sanal cihazı nasıl oluşturacağınız açıklanır.

1. [Azure portalını](https://portal.azure.com) açın ve Azure hesabınızla oturum açın.

2. **Kaynak grupları** ' nı seçin ve ardından kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

3. Kaynak listesinde IoT Hub 'ınızı seçin. Bu öğreticide **ContosoTestHub** kullanılır. Hub bölmesinden **IoT Cihazları**'nı seçin.

4. **+ Ekle** öğesini seçin. Cihaz Ekle bölmesinde cihaz kimliğini girin. Bu öğreticide **Contoso-Test-Device** kullanılır. Anahtarları boş bırakın ve **Anahtarları Otomatik Olarak Oluştur**'a tıklayın. **Cihazı IoT Hub'ına bağla** seçeneğinin etkinleştirildiğinden emin olun. **Kaydet**’i seçin.

   ![Cihaz Ekle ekranı](./media/iot-hub-include-create-simulated-device-portal/add-device.png)

5. Artık oluşturulduğuna göre, oluşturulan anahtarları görmek için cihazı seçin. Birincil anahtarda Kopyala simgesini seçin ve Bu öğreticinin test aşaması için Not Defteri gibi bir yere kaydedin.

   ![Anahtarlar dahil cihaz ayrıntıları](./media/iot-hub-include-create-simulated-device-portal/device-details.png)