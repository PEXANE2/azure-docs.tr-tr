---
title: VSCode için Azure IoT Araçları ile Azure IoT aygıt yönetimi
description: Doğrudan yöntemleri ve Twin'in istenen özellik yönetimi seçeneklerini içeren Azure IoT Hub aygıt yönetimi için Visual Studio Kodu için Azure IoT Araçlarını kullanın.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 9d4d82472664900c96b77b31740573d0463465b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75911927"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-for-azure-iot-hub-device-management"></a>Azure IoT Hub aygıt yönetimi için Visual Studio Kodu için Azure IoT Araçlarını kullanma

![Uçuça diyagram](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT Araçları,](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) IoT Hub yönetimini ve IoT uygulama geliştirmeyi kolaylaştıran kullanışlı bir Visual Studio Kodu uzantısıdır. Çeşitli görevleri gerçekleştirmek için kullanabileceğiniz yönetim seçenekleri ile birlikte gelir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                    |
|----------------------------|--------------------------------|
| Doğrudan yöntemler             | İleti göndermeyi başlatma veya durdurma veya aygıtı yeniden başlatma gibi bir aygıt eylemi yapın.                                        |
| Aygıt ikizini okuma           | Bir aygıtın bildirilen durumunu alın. Örneğin, aygıt LED'in yanıp söndüğünü bildiriyor.                                    |
| Aygıt ikizini güncelleştir         | Bir aygıtı, LED'i yeşile ayarlama veya telemetri gönderme aralığını 30 dakikaya ayarlama gibi belirli durumlara koyun.         |
| Buluttan cihaza iletiler   | Bildirimleri bir aygıta gönderin. Örneğin, "Bugün çok yağmur olasılığı yüksektir. Şemsiye getirmeyi unutma."              |

Bu seçenekleri kullanma konusundaki farklar ve kılavuzlar hakkında daha ayrıntılı açıklama için [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) ve [Buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md)bakın.

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IoT Hub, bağlanan her aygıt için bir aygıt ikizini kalıcı hale getirir. Cihaz ikizleri hakkında daha fazla bilgi için [bkz.](iot-hub-node-node-twin-getstarted.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleriyle Visual Studio Code için Azure IoT Tools'u kullanmayı öğrenirsiniz.

## <a name="what-you-do"></a>Ne yaparsınız

Çeşitli yönetim seçenekleriyle Visual Studio Code için Azure IoT Araçlarını çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

* Etkin bir Azure aboneliği.
* Aboneliğiniz altında bir Azure IoT hub'ı.
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Kodu için Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) veya [Visual Studio Code bu bağlantıyı açın.](vscode:extension/vsciot-vscode.azure-iot-tools)

## <a name="sign-in-to-access-your-iot-hub"></a>IoT hub'ınıza erişmek için oturum açın

1. VS **Code'un Explorer** görünümünde, sol alt köşedeki **Azure IoT Hub Cihazları** bölümünü genişletin.

2. Bağlam menüsünde **IoT Hub'ı seçin'i** tıklatın.

3. Azure'da ilk kez oturum açmanıza izin vermek için sağ alt köşede bir açılır pencere açılır.

4. Oturum açmanızdan sonra Azure Abonelik listeniz gösterilir ve ardından Azure Aboneliği ve IoT Hub'ı seçin.

5. Cihaz listesi birkaç saniye içinde **Azure IoT Hub Aygıtları** sekmesinde gösterilir.

   > [!Note]
   > Ayrıca, ayarlamayı tamamlamak için **IoT Hub Bağlantı Dizesini Ayarla**'yı seçebilirsiniz. IoT aygıtınızın açılan pencerede bağlandığı IoT hub'ı için **iothubowner** ilkesi bağlantı dizesini girin.

## <a name="direct-methods"></a>Doğrudan yöntemler

1. Cihazınıza sağ tıklayın ve **Doğrudan Çağır Yöntemi'ni**seçin. 

2. Giriş kutusuna yöntem adını ve yükü girin.

3. Sonuçlar **OUTPUT** > **Azure IoT Hub** görünümünde gösterilir.

## <a name="read-device-twin"></a>Aygıt ikizini okuma

1. Cihazınıza sağ tıklayın ve **Aygıtı İkiz'i Edit'i**seçin. 

2. Bir **azure-iot-device-twin.json** dosyası aygıt ikiz içeriği ile açılacaktır.

## <a name="update-device-twin"></a>Aygıt ikizini güncelleştir

1. **Etiketlerin** veya özelliklerin bazı durumlarını **yapın.istenilen** alan.

2. **Azure-iot-device-twin.json** dosyasına sağ tıklayın.

3. Aygıtı ikizi güncelleştirmek için **Aygıt İkini Güncelleştir'i** seçin.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

IoT hub'ınızdan cihazınıza bir ileti göndermek için aşağıdaki adımları izleyin:
 
1. Cihazınıza sağ tıklayın ve **Cihaza C2D İleti gönder'i**seçin. 

2. İletiyi giriş kutusuna girin.

3. Sonuçlar **OUTPUT** > **Azure IoT Hub** görünümünde gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Çeşitli yönetim seçenekleriyle Visual Studio Code için Azure IoT Araçları uzantısını nasıl kullanacağınızı öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
