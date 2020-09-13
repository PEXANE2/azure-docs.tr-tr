---
title: Fiyatlandırma ve ilişkili maliyetler
description: IoT için Azure Güvenlik Merkezi ile ilişkili maliyetler ve bunları nasıl denetleyeceğinizi öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 2c76a79f09629b32767c1dcc78989dfa47c678fd
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2020
ms.locfileid: "89504581"
---
# <a name="pricing-and-associated-costs"></a>Fiyatlandırma ve ilişkili maliyetler

Bu makalede IoT fiyatlandırma modeli için Azure Güvenlik Merkezi açıklanır, tüm ilişkili maliyetler özetlenir ve bunların nasıl yönetileceği açıklanmaktadır.

## <a name="pricing"></a>Fiyatlandırma

IoT fiyatlandırma modeli için Azure Güvenlik Merkezi, iki bölümden oluşur ve IoT için Azure Güvenlik [Merkezi 'nde IoT Hub etkinleştirildikten sonra](quickstart-onboard-iot-hub.md) faturalandırılır:

- Cihaz, IoT Hub günlüklerin analizine dayalı olarak yerleşik güvenlik özelliklerine göre maliyetlidir.

- IoT Edge veya yaprak cihazlardan gelen güvenlik iletilerine dayalı olarak ileti ile gelişmiş güvenlik özelliklerine göre maliyet.

Daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>İlişkili maliyetler

IoT için Azure Güvenlik Merkezi 'nin, doğrudan fiyatlandırma kapsamında olmayan ilişkili maliyetleri vardır:

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
> Azure Güvenlik Merkezi 'Nde IoT güvenlik özelliği kullanılabilirliği için önemli etkileri vardır.

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
