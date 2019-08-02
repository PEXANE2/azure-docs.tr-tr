---
title: IoT maliyetleri için Azure Güvenlik Merkezi 'Ni anlama | Microsoft Docs
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
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 603df1def011232ad2120c37ad1ba256f2a30526
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596524"
---
# <a name="pricing-and-associated-costs"></a>Fiyatlandırma ve ilişkili maliyetler

Bu makalede IoT fiyatlandırma modeli için Azure Güvenlik Merkezi açıklanır, tüm ilişkili maliyetler özetlenir ve bunların nasıl yönetileceği açıklanmaktadır.

## <a name="pricing"></a>Fiyatlandırma

IoT fiyatlandırma modeli için Azure Güvenlik Merkezi, iki bölümden oluşur ve IoT için Azure Güvenlik [Merkezi 'nde IoT Hub](quickstart-onboard-iot-hub.md) etkinleştirildikten sonra faturalandırılır:

- Cihaz, IoT Hub günlüklerin analizine dayalı olarak yerleşik güvenlik özelliklerine göre maliyetlidir.

- IoT Edge veya yaprak cihazlardan gelen güvenlik iletilerine dayalı olarak ileti ile gelişmiş güvenlik özelliklerine göre maliyet.

  >[!Note]
  > Güvenlik iletileri ayrıca IoT Hub kota tüketimine da neden olabilir.

Daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>İlişkili maliyetler

IoT için Azure Güvenlik Merkezi 'nin, doğrudan fiyatlandırma kapsamında olmayan iki tür ilişkilendirilmiş maliyeti vardır:

- IoT Hub kota tüketimi

- Log Analytics depolama maliyetleri

Ayarlarınızı değiştirerek, belirli özelliklerden yararlanarak ilişkili maliyetleri azaltabilirsiniz.

Ayarlarınızı değiştirmek için:

1. IoT Hub açın.

2. **Güvenlik**altında **Genel Bakış ' a**tıklayın.

3. **Ayarlar**' a tıklayın.

Aşağıdaki tabloda, her bir seçeneğin ilişkili maliyetlerinin ve etkilerine ilişkin bir özet verilmektedir.

|     | Kullanım | Yorum |
| --- | --- | --- |
| **IoT Hub kota tüketimi** |  |
| [Cihaz Işini dışarı aktarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-bulk-identity-mgmt#export-devices) (ikizi Export) | Günde bir kez | _İkizi meta veri toplamayı_ devre dışı bırak |
| **Log Analytics depolama** |  |
| Cihaz önerisi ve uyarılar| Hizmet tarafından oluşturulan güvenlik önerisi ve uyarılar | İsteğe bağlı değil |
| Ham güvenlik verileri| Güvenlik aracıları tarafından toplanan IoT cihazlarından gelen ham güvenlik verileri | _Depo ham cihaz güvenlik olaylarını_ devre dışı bırak |

>[!Important]
> Kullanıma alma, kullanılabilir güvenlik özelliklerinin ciddi etkileri vardır.
  
| Ayrıl | Etkileri |
| --- | --- |
| _İkizi meta veri koleksiyonu_ | [Özel uyarıları](quickstart-create-custom-alerts.md) devre dışı bırak |
| | IoT Edge bildirim önerilerini devre dışı bırak |
| | Cihaz kimliği tabanlı önerileri ve uyarıları devre dışı bırak |
| _Ham cihaz güvenlik olaylarını depola_ | Cihaz işletim sistemi temel önerileri ayrıntıları kullanılamıyor |
| | [Uyarı](concept-security-alerts.md) ve [öneri](concept-recommendations.md) araştırmamaları hakkındaki ayrıntılar kullanılamıyor |
|


## <a name="see-also"></a>Ayrıca bkz.

- [Ham güvenlik verilerinize](how-to-security-data-access.md) erişin
- [Bir cihazı araştırın](how-to-investigate-device.md)
- [Güvenlik önerilerini](concept-recommendations.md) anlayın ve araştırın
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama ve araştırma
