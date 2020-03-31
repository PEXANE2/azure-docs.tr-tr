---
title: Azure IoT Hub'ı yükseltin | Microsoft Dokümanlar
description: Daha fazla ileti ve aygıt yönetimi özelliği elde etmek için IoT Hub'ın fiyatlandırma ve ölçek katmanını değiştirin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: robinsh
ms.openlocfilehash: 96c3a7b2cfda23f173f4caeff4fb7a92b1ddc438
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61440242"
---
# <a name="how-to-upgrade-your-iot-hub"></a>IoT hub'ınızı yükseltme

IoT çözümünüz büyüdükçe Azure IoT Hub'ı büyümenize yardımcı olmaya hazırdır. Azure IoT Hub, farklı özellikler kullanmak isteyen müşterileri barındırmak için temel (B) ve standart (S) olmak üzere iki katman sunar. Her katmaniçinde, her gün gönderilebilen ileti sayısını belirleyen üç boyut (1, 2 ve 3) vardır.

Daha fazla aygıtınız olduğunda ve daha fazla yeteneğe ihtiyacınız olduğunda, IoT hub'ınızı gereksinimlerinize göre ayarlamanın üç yolu vardır:

* IoT hub'ına birimler ekleyin. Örneğin, B1 IoT hub'ındaki her ek birim, günde 400.000 iletiye izin verir.

* IoT hub'ının boyutunu değiştirin. Örneğin, her birimin günlük olarak desteklenebildiği ileti sayısını artırmak için B1 katmanından B2 katmanına geçiş yapabilirsiniz.

* Daha yüksek bir katmana yükseltin. Örneğin, aynı ileti kapasitesine sahip gelişmiş özelliklere erişmek için B1 katmanından S1 katmanına yükseltin.

Bu değişikliklerin tümü varolan işlemleri kesintiye uğratmadan oluşabilir.

IoT hub'ınızı düşürmek istiyorsanız, birimleri kaldırabilir ve IoT hub'ının boyutunu küçültebilirsiniz, ancak daha düşük bir katmana indiremezsiniz. Örneğin, S2 katmanından S1 katmanına geçebilirsiniz, ancak S2 katmanından B1 katmanına geçemezsiniz. IoT Hub başına bir katman içinde yalnızca bir [Tür Iot Hub sürümü](https://azure.microsoft.com/pricing/details/iot-hub/) seçilebilir. Örneğin, birden çok S1 birimine sahip bir IoT Hub'ı oluşturabilirsiniz, ancak S1 ve B3 veya S1 ve S2 gibi farklı sürümlerden birimlerin karışımıyla oluşturamaz.

Bu örnekler, çözümünüz değiştikçe IoT hub'ınızı nasıl ayarlayacağınızı anlamanıza yardımcı olmak içindir. Her bir tierin yetenekleri hakkında belirli bilgiler için her zaman [Azure IoT Hub fiyatlandırmasına](https://azure.microsoft.com/pricing/details/iot-hub/)başvurmalısınız.

## <a name="upgrade-your-existing-iot-hub"></a>Mevcut IoT hub'ınızı yükseltin

1. [Azure portalında](https://portal.azure.com/) oturum açın ve IoT hub'ınıza gidin.

2. **Fiyatlandırma ve ölçek**seçin.

   ![Fiyatlandırma ve ölçek](./media/iot-hub-upgrade/pricing-scale.png)

3. Hub'ınızın katmanını değiştirmek için **Fiyatlandırma ve ölçek katmanını**seçin. Yeni katmanı seçin, ardından **seç'i**tıklatın.

   ![Fiyatlandırma ve ölçek katmanı](./media/iot-hub-upgrade/select-tier.png)

4. Hub'ınızdaki birim sayısını değiştirmek **için, IoT Hub birimlerinin**altına yeni bir değer girin.

5. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.

IoT hub'ınız artık ayarlandı ve yapılandırmalarınız değişmedi.

Temel katman IoT Hub'ı ve standart katman IoT Hub'ı için maksimum bölüm sınırı 32'dir. Çoğu IoT Hub'ı yalnızca 4 bölüme ihtiyaç duyar. IoT Hub oluşturulduğunda bölüm sınırı seçilir ve aygıttan buluta iletileri bu iletilerin eşzamanlı okuyucu sayısıyla ilişkilendirir. Temel katmandan standart katmana geçiş yaptığınızda bu değer değişmeden kalır.

## <a name="next-steps"></a>Sonraki adımlar

[Doğru IoT Hub katmanını seçme](iot-hub-scaling.md)hakkında daha fazla bilgi alın.