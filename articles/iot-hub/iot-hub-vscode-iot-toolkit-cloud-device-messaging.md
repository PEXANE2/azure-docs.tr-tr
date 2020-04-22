---
title: BT Hub iletisini yöneticiletmek için VSCode için Azure IoT Araçlarını kullanma
description: Azure IoT Hub'da cihazları buluta buluta göndermek ve aygıt iletilerine bulut göndermek için Visual Studio Code için Azure IoT Araçlarını nasıl kullanacağınızı öğrenin.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/18/2019
ms.author: junhan
ms.openlocfilehash: 31a5d55d1067b9dd946c1667118d0bde5ee3d59e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682509"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Cihazınız ve IoT Hub arasında mesaj göndermek ve almak için Visual Studio Kodu için Azure IoT Araçlarını kullanın

![Uçuça diyagram](./media/iot-hub-vscode-iot-toolkit-cloud-device-messaging/e-to-e-diagram.png)

[Azure IoT Araçları,](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) IoT Hub yönetimini ve IoT uygulama geliştirmeyi kolaylaştıran kullanışlı bir Visual Studio Kodu uzantısıdır. Bu makalede, aygıtınız ile IoT hub'ınız arasında ileti göndermek ve almak için Visual Studio Kodu için Azure IoT Araçları'nın nasıl kullanılacağı üzerinde duruluyor.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Ne öğreneceksiniz

Görsel Stüdyo Kodu için Azure IoT Araçlarını aygıttan buluta iletileri izlemek ve buluttan cihaza iletigöndermek için nasıl kullanacağınızı öğrenirsiniz. Aygıttan buluta iletiler, cihazınızın topladığı ve ioT hub'ınıza gönderdiği sensör verileri olabilir. Buluttan cihaza iletiler, IoT hub'ınızın cihazınıza bağlı bir LED'i kırpmak için cihazınıza gönderdiği komutlar olabilir.

## <a name="what-you-will-do"></a>Ne yapacaksınız

* Aygıttan buluta iletileri izlemek için Visual Studio Kodu için Azure IoT Araçlarını kullanın.

* Buluttan cihaza iletigöndermek için Visual Studio Kodu için Azure IoT Araçlarını kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

* Etkin bir Azure aboneliği.

* Aboneliğiniz altında bir Azure IoT hub'ı.

* [Visual Studio Code](https://code.visualstudio.com/)

* [VS Kodu için Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) veya bu URL'yi kopyalayıp bir tarayıcı penceresine yapıştırın:`vscode:extension/vsciot-vscode.azure-iot-tools`

## <a name="sign-in-to-access-your-iot-hub"></a>IoT hub'ınıza erişmek için oturum açın

1. VS **Code'un Explorer** görünümünde, sol alt köşedeki **Azure IoT Hub Cihazları** bölümünü genişletin.

2. Bağlam menüsünde **IoT Hub'ı seçin'i** tıklatın.

3. Azure'da ilk kez oturum açmanıza izin vermek için sağ alt köşede bir açılır pencere açılır.

4. Oturum açmanızdan sonra Azure Abonelik listeniz gösterilir ve ardından Azure Aboneliği ve IoT Hub'ı seçin.

5. Cihaz listesi birkaç saniye içinde **Azure IoT Hub Aygıtları** sekmesinde gösterilir.

   > [!Note]
   > Ayrıca, ayarlamayı tamamlamak için **IoT Hub Bağlantı Dizesini Ayarla**'yı seçebilirsiniz. IoT aygıtınızın açılan pencerede bağlandığı IoT hub'ı için **iothubowner** ilkesi bağlantı dizesini girin.

## <a name="monitor-device-to-cloud-messages"></a>Aygıttan buluta iletileri izleme

Cihazınızdan IoT hub'ınıza gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. Cihazınıza sağ tıklayın ve **Yerleşik Etkinlik Bitiş Noktasını İzlemeye Başla'yı**seçin.

2. İzlenen iletiler **OUTPUT** > **Azure IoT Hub** görünümünde gösterilir.

3. İzlemeyi durdurmak için **OUTPUT** görünümüne sağ tıklayın ve **Yerleşik Etkinlik Bitiş Noktasını Izlemeyi Durdur'u**seçin.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

IoT hub'ınızdan cihazınıza bir ileti göndermek için aşağıdaki adımları izleyin:

1. Cihazınıza sağ tıklayın ve **Cihaza C2D İleti gönder'i**seçin.

2. İletiyi giriş kutusuna girin.

3. Sonuçlar **OUTPUT** > **Azure IoT Hub** görünümünde gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

IoT aygıtınızla Azure IoT Hub'ınız arasında aygıttan buluta iletileri izlemeyi ve buluttan cihaza iletigöndermeyi öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]