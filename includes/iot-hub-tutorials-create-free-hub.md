---
title: dosya dahil etme
description: dosya dahil etme
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 367a0b1d17f8d5ebe4f46835ace963b00e75354e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "68229336"
---
Azure portalı kullanarak IoT Hub oluşturmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. IoT Hub **nesnelerin interneti kaynak oluştur**' u seçin  >  **Internet of Things**  >  **IoT Hub**.

    ![IoT Hub’ı yüklemek için seçin](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. Ücretsiz katmanlı IoT Hub’ınızı oluşturmak için aşağıdaki tabloda bulunan değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | Yeni oluşturun. Bu öğretici **tutorials-iot-hub-rg** adını kullanır. |
    | Bölge | Bu öğreticide **Batı ABD** kullanılır. Size en yakın bölgeyi seçebilirsiniz. |
    | Adı | Aşağıdaki ekran görüntüsü **tutorials-iot-hub** adını kullanır. Hub'ınızı oluştururken kendi benzersiz adınızı seçmeniz gerekir. |

    ![Hub ayarları 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | Ayar | Değer |
    | ------- | ----- |
    | Fiyatlandırma ve ölçek katmanı | F1 Ücretsiz. Bir abonelikte yalnızca bir ücretsiz katmanlı hub’ınız olabilir. |
    | IoT Hub birimleri | 1 |

    ![Hub ayarları 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. **Oluştur**’a tıklayın. Hub’ın oluşturulması birkaç dakika sürebilir.

    ![Hub ayarları 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. Seçtiğiniz IoT hub adını not edin. Bu değeri öğreticinin sonraki bölümlerinde kullanacaksınız.
