---
title: VS Kodu için Azure IoT Araçlarını kullanarak bir Azure IoT Hub'ı oluşturun | Microsoft Dokümanlar
description: Kaynak grubunda bir Azure IoT hub'ı oluşturmak için Visual Studio Code için Azure IoT araçlarını nasıl kullanacağınızı öğrenin.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 668087ae596688e86b7b84a16bc5c0fd0f9fcef6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75912232"
---
# <a name="create-an-iot-hub-using-the-azure-iot-tools-for-visual-studio-code"></a>Visual Studio Code için Azure IoT Araçlarını kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede, bir Azure IoT hub'ı oluşturmak [için Visual Studio Code için Azure IoT Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) nasıl kullanacağınızı gösterilmektedir. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makaleyi tamamlamak için aşağıdakileri yapmanız gerekir:

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- [Visual Studio Code](https://code.visualstudio.com/)

- Visual Studio Code için [Azure IoT Araçları.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. Visual Studio Code'da **Explorer** görünümünü açın.

2. Explorer'ın alt **kısmında, Azure IoT Hub Aygıtları** bölümünü genişletin. 

   ![Azure IoT Hub Aygıtlarını Genişletme](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. **Azure IoT Hub Aygıtları** **bölümündeki ...** bölümünü tıklatın. Elipsleri görmüyorsanız, başlığın üzerine gidin. 

4. **IoT Hub oluştur'u**seçin.

5. Azure'da ilk kez oturum açmanıza izin vermek için sağ alt köşede bir açılır pencere açılır.

6. Azure aboneliğini seçin. 

7. Kaynak grubunu seçin.

8. Konumu seçin.

9. Fiyatlandırma katmanını seçin.

10. IoT Hub'ınız için genel olarak benzersiz bir ad girin.

11. IoT Hub'ı oluşturulana kadar birkaç dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Artık Visual Studio Code için Azure IoT Araçlarını kullanarak bir IoT hub'ı dağıttınız. Daha fazla bilgi için aşağıdaki makalelere göz atın:

* [Aygıtınız ve Bir IoT Hub'ı arasında ileti göndermek ve almak için Visual Studio Kodu için Azure IoT Araçlarını kullanın.](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)

* [Azure IoT Hub aygıt yönetimi için Görsel Stüdyo Kodu için Azure IoT Araçlarını kullanma](iot-hub-device-management-iot-toolkit.md)

* [VS Code wiki sayfası için Azure IoT Hub'ına bakın.](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki)
