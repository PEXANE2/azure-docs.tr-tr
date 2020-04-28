---
title: Azure IoT Hub 'yi yükseltme | Microsoft Docs
description: Daha fazla mesajlaşma ve cihaz yönetimi özelliği almak için IoT Hub fiyatlandırma ve ölçek katmanını değiştirin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: robinsh
ms.openlocfilehash: 96c3a7b2cfda23f173f4caeff4fb7a92b1ddc438
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61440242"
---
# <a name="how-to-upgrade-your-iot-hub"></a>IoT hub'ınızı yükseltme

IoT çözümünüz büyüdükçe Azure IoT Hub, ölçeği büyütmenize yardımcı olmaya hazırız. Azure IoT Hub, farklı özellikler kullanmak isteyen müşterileri barındırmak için temel (B) ve standart olan iki katman sunar. Her bir katmanın içinde, her gün gönderilebilecek ileti sayısını belirten üç boyut (1, 2 ve 3) vardır.

Daha fazla cihazınız varsa ve daha fazla özelliğe ihtiyacınız olduğunda, IoT Hub 'ınızı gereksinimlerinize uyacak şekilde ayarlamanız için üç yol vardır:

* IoT Hub içinde birim ekleyin. Örneğin, bir B1 IoT Hub 'ındaki her ek birim günde ek 400.000 ileti sağlar.

* IoT Hub 'ın boyutunu değiştirin. Örneğin, her bir birimin günde destekleyebileceği ileti sayısını artırmak için B1 katmanından B2 katmanına geçiş yapın.

* Daha yüksek bir katmana yükseltin. Örneğin, aynı mesajlaşma kapasitesine sahip gelişmiş özelliklere erişmek için B1 katmanından S1 katmanına yükseltin.

Bu değişikliklerin hepsi, var olan işlemleri kesintiye uğramadan oluşabilir.

IoT Hub 'ınızı düşürmek istiyorsanız birimleri kaldırabilir ve IoT Hub 'ının boyutunu azaltabilirsiniz, ancak daha düşük bir katmana indirgeyemezsiniz. Örneğin, S2 katmanından S1 katmanına geçebilir, ancak S2 katmanından B1 katmanına geçiş yapabilirsiniz. Her IoT Hub için bir katmanda yalnızca bir [IoT Hub sürümü](https://azure.microsoft.com/pricing/details/iot-hub/) türü seçilebilir. Örneğin, S1 ve B3 veya S1 ve S2 gibi farklı sürümlerden birim karışımı olmadan, birden fazla S1 birimli bir IoT Hub oluşturabilirsiniz.

Bu örnekler, çözüm değiştikçe IoT Hub 'ınızı nasıl ayarlayabileceğinizi anlamanıza yardımcı olmak için tasarlanmıştır. Her katmanın özellikleri hakkında belirli bilgiler için, her zaman [Azure IoT Hub fiyatlandırmasına](https://azure.microsoft.com/pricing/details/iot-hub/)başvurmanız gerekir.

## <a name="upgrade-your-existing-iot-hub"></a>Mevcut IoT Hub 'ınızı yükseltin

1. [Azure Portal](https://portal.azure.com/) oturum açın ve IoT Hub 'ınıza gidin.

2. **Fiyatlandırma ve ölçek '** i seçin.

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/pricing-scale.png)

3. Hub 'ınızın katmanını değiştirmek için **fiyatlandırma ve ölçek katmanı**' nı seçin. Yeni katmanı seçin ve ardından **Seç**' e tıklayın.

   ![Fiyatlandırma ve ölçek katmanı](./media/iot-hub-upgrade/select-tier.png)

4. Hub 'ınızdaki birim sayısını değiştirmek için **IoT Hub birimleri**altına yeni bir değer girin.

5. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.

IoT Hub 'ınız artık ayarlandı ve yapılandırmanız değiştirilmedi.

Temel katman IoT Hub ve Standart katman IoT Hub için maksimum bölüm sınırı 32 ' dir. Birçok IoT Hub 'ı yalnızca 4 bölümden yeterlidir. IoT Hub oluşturulduğunda bölüm sınırı seçilir ve cihazdan buluta iletileri bu iletilerin eşzamanlı okuyucuları sayısına ilişkilendirir. Temel katmandan Standart katmana geçiş yaparken bu değer değişmeden kalır.

## <a name="next-steps"></a>Sonraki adımlar

[Doğru IoT Hub katmanını seçme](iot-hub-scaling.md)hakkında daha fazla bilgi alın.