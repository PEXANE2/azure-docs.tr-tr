---
title: include dosyası
description: include dosyası
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 7b022f71e197c5695876f2049ee376c3616afc6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70049060"
---
<!-- put the ## header in the file that includes this file -->

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde bir aygıt kimliği oluşturursunuz. Aygıt, kimlik kayıt defterinde bir girdi olmadığı sürece hub'a bağlanamaz. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md#identity-registry-operations).

1. IoT hub gezinme menüsünde, **IoT Aygıtları'nı**açın ve Ardından IoT hub'ınıza aygıt eklemek için **Yeni'yi** seçin.

    ![Portalda aygıt kimliği oluşturma](./media/iot-hub-include-create-device/create-identity-portal-vs2019.png)

1. **Aygıt Oluştur'da** **myDeviceId**gibi yeni aygıtınız için bir ad sağlayın ve **Kaydet'i**seçin. Bu eylem, IoT hub'ınız için bir aygıt kimliği oluşturur.

   ![Yeni bir aygıt ekleme](./media/iot-hub-include-create-device/create-a-device-vs2019.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Cihaz oluşturulduktan sonra cihazı **IoT cihazları** bölmesindeki listeden açın. Daha sonra kullanmak üzere **Birincil Bağlantı Dizesini** kopyalayın.

    ![Aygıt bağlantı dizesi](./media/iot-hub-include-create-device/device-details-vs2019.png)

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
