---
title: IoT maliyetleri için Azure Güvenlik Merkezi'ni anlayın| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi ile ilişkili maliyetler ve bunları nasıl denetlenenler hakkında bilgi edinin.
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
ms.date: 09/27/2019
ms.author: mlottner
ms.openlocfilehash: dc9dcbfd00b5205fa5c66e334b30c76d549d8a42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71348511"
---
# <a name="pricing-and-associated-costs"></a>Fiyatlandırma ve ilişkili maliyetler

Bu makalede, IoT fiyatlandırma modeli için Azure Güvenlik Merkezi açıklanmaktadır, ilişkili tüm maliyetleri özetler ve bunların nasıl yönetileceğimi açıklar.

## <a name="pricing"></a>Fiyatlandırma

IoT fiyatlandırma modeli için Azure Güvenlik Merkezi iki bölümden oluşur ve IoT Için Azure Güvenlik Merkezi'nde bir IoT Hub [etkinleştirildiğinde](quickstart-onboard-iot-hub.md) faturalandırılır:

- Cihaza göre maliyet - IoT Hub günlüklerinin analizine dayalı yerleşik güvenlik yetenekleri.

- İleti ile maliyet - IoT Edge veya yaprak aygıtlardan gelen güvenlik iletilerine dayalı gelişmiş güvenlik özellikleri.


Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/security-center/)bakın.

## <a name="associated-costs"></a>İlişkili maliyetler

Azure Güvenlik Merkezi ioT için doğrudan fiyatlandırmanın bir parçası olmayan ilişkili maliyetlere sahiptir:


- Log Analytics depolama maliyetleri

Belirli çözüm özelliklerini devre dışı kalarak ilişkili maliyetleri azaltabilirsiniz. Ayarlarınızı değiştirerek devre dışı bırakmak.

Ayarlarınızı değiştirmek için:

1. IoT Hub'ı açın.

2. **Güvenlik**Altında , **Genel Bakış'ı**tıklatın.

3. **Ayarlar**'a tıklayın.

Aşağıdaki tablo, ilişkili maliyetlerin ve her seçeneğin sonuçlarının bir özetini sağlar.

|     | Kullanım | Açıklama |
| --- | --- | --- |
| **Günlük Analizi depolama** |  |
| Cihaz önerisi ve uyarıları| Hizmet tarafından oluşturulan güvenlik önerisi ve uyarılar | İsteğe bağlı değil |
| Ham güvenlik verileri| Güvenlik ajanları tarafından toplanan IoT aygıtlarından gelen ham güvenlik verileri | _Ham cihaz güvenlik olaylarını depolamayı_ devre dışı bırakın |
|

>[!Important]
> Devre dışı bırakmanın, IoT güvenlik özelliği kullanılabilirliği için Azure Güvenlik Merkezi'ne ciddi etkileri vardır. 
  
| Devre dışı bırakma | Etkileri |
| --- | --- |
| _İkiz meta veri toplama_ | [Özel uyarıları](quickstart-create-custom-alerts.md) devre dışı |
| | IoT Edge bildirim önerilerini devre dışı |
| | Aygıt kimlik tabanlı önerileri ve uyarıları devre dışı bırakın |
| _Ham cihaz güvenlik olaylarını depolama_ | Aygıt işletim sistemi taban çizgisi önerileriyle ilgili ayrıntılar kullanılamıyor |
| | [Uyarı](concept-security-alerts.md) ve [tavsiye](concept-recommendations.md) soruşturmaları ile ilgili ayrıntılar mevcut değil |
|


## <a name="see-also"></a>Ayrıca bkz.

- Ham [güvenlik verilerinize](how-to-security-data-access.md) erişin
- [Cihazı araştırma](how-to-investigate-device.md)
- [Güvenlik önerilerini](concept-recommendations.md) anlama ve keşfetme
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama ve keşfetme
