---
title: Fiyatlandırma ve ilişkili maliyetler
description: IoT için Defender ile ilişkili maliyetler ve bunları nasıl denetleyeceğinizi öğrenin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 24ae6c4014948639aa737a0d2d88ec15f98a7cb4
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941723"
---
# <a name="pricing-and-associated-costs"></a>Fiyatlandırma ve ilişkili maliyetler

Bu makalede IoT fiyatlandırma modeli için Defender açıklanır, tüm ilişkili maliyetler özetlenir ve bunların nasıl yönetileceği açıklanmaktadır.

## <a name="pricing"></a>Fiyatlandırma

IoT için Defender fiyatlandırma modeli iki bölümden oluşur ve bir IoT Hub IoT için [Defender 'da etkinleştirildikten](quickstart-onboard-iot-hub.md) sonra faturalandırılır:

- Cihaz, IoT Hub günlüklerin analizine dayalı olarak yerleşik güvenlik özelliklerine göre maliyetlidir.

- IoT Edge veya yaprak cihazlardan gelen güvenlik iletilerine dayalı olarak ileti ile gelişmiş güvenlik özelliklerine göre maliyet.

Daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>İlişkili maliyetler

IoT için Defender, doğrudan fiyatlandırma kapsamında olmayan ilişkili maliyetlere sahiptir:

- Log Analytics depolama maliyetleri

Belirli çözüm özelliklerinden yararlanarak ilişkili maliyetleri azaltabilirsiniz. Ayarlarınızı değiştirerek bu işlemi geri çevirin.

Ayarlarınızı değiştirmek için:

1. IoT Hub açın.

1. **Güvenlik**altında **Ayarlar**' a tıklayın.

1. **Veri koleksiyonu**' na tıklayın.

Aşağıdaki tabloda, her bir seçeneğin ilişkili maliyetlerinin ve etkilerine ilişkin bir özet verilmektedir.

| Seçenek | Kullanım | Yorum |
| --- | --- | --- |
| **Log Analytics depolama** |  |
| Cihaz önerisi ve uyarılar| Hizmet tarafından oluşturulan güvenlik önerisi ve uyarılar | İsteğe bağlı değil |
| Ham güvenlik verileri| Güvenlik aracıları tarafından toplanan IoT cihazlarından gelen ham güvenlik verileri | _Depo ham cihaz güvenlik olaylarını_ devre dışı bırak |
|

>[!Important]
> Kullanıma hazır olmak, IoT güvenlik özelliği kullanılabilirliği için Defender 'a ciddi etkileri vardır.

| Devre dışı | Etkileri |
| --- | --- |
| _İkizi meta veri koleksiyonu_ | [Özel uyarıları](quickstart-create-custom-alerts.md) devre dışı bırak |
| | IoT Edge bildirim önerilerini devre dışı bırak |
| | Cihaz kimliği tabanlı önerileri ve uyarıları devre dışı bırak |
| _Ham cihaz güvenlik olaylarını depola_ | Cihaz işletim sistemi temel önerileri ayrıntıları kullanılamıyor |
| | [Uyarı](concept-security-alerts.md) ve [öneri](concept-recommendations.md) araştırmamaları hakkındaki ayrıntılar kullanılamıyor |
|

## <a name="see-also"></a>Ayrıca bkz.

- [Ham güvenlik verilerinize](how-to-security-data-access.md) erişin
- [Cihazı araştırma](how-to-investigate-device.md)
- [Güvenlik önerilerini](concept-recommendations.md) anlayın ve araştırın
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama ve araştırma
