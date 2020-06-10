---
title: dosya dahil etme
description: dosya dahil etme
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 7b022f71e197c5695876f2049ee376c3616afc6d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "70049060"
---
<!-- put the ## header in the file that includes this file -->

Bu bölümde, IoT Hub 'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturacaksınız. Kimlik kayıt defterinde bir girişi olmadığı takdirde bir cihaz bir hub 'a bağlanamaz. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md#identity-registry-operations).

1. IoT Hub 'ınız gezinti menüsünde **IoT cihazları**' nı açın ve ardından IoT Hub 'ınıza bir cihaz eklemek için **Yeni** ' yi seçin.

    ![Portalda cihaz kimliği oluşturma](./media/iot-hub-include-create-device/create-identity-portal-vs2019.png)

1. **Cihaz oluştur**' da, yeni cihazınız Için **mydeviceıd**gibi bir ad girin ve **Kaydet**' i seçin. Bu eylem, IoT Hub 'ınız için bir cihaz kimliği oluşturur.

   ![Yeni cihaz ekle](./media/iot-hub-include-create-device/create-a-device-vs2019.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Cihaz oluşturulduktan sonra cihazı **IoT cihazları** bölmesindeki listeden açın. Daha sonra kullanmak üzere **birincil bağlantı dizesini** kopyalayın.

    ![Cihaz bağlantı dizesi](./media/iot-hub-include-create-device/device-details-vs2019.png)

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
