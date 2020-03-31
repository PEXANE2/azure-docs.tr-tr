---
title: Sorun giderme Azure IoT Hub hatası 504101 Ağ GeçidiTimeout
description: Hata 504101 GatewayTimeout nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: b74a93e15d533bf9b15797e6371a25230f7a08f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960678"
---
# <a name="504101-gatewaytimeout"></a>504101 GatewayTimeout

Bu makalede, **504101 GatewayTimeout** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub'dan aygıta doğrudan bir yöntem çağırmaya çalışırken, istek **504101 GatewayTimeout**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

### <a name="cause-1"></a>Neden 1

IoT Hub bir hatayla karşılaştı ve zamanlamadan önce doğrudan yöntemin tamamlanıp tamamlanmadığını doğrulayamadı.

### <a name="cause-2"></a>Neden 2

Azure IoT C# SDK'nın (<1.19.0) önceki bir sürümünü kullanırken, aygıt ve IoT Hub arasındaki AMQP bağlantısı bir hata nedeniyle sessizce bırakılabilir.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Yeniden deneme.

### <a name="solution-2"></a>Çözüm 2

Azure IOT C# SDK'nın en son sürümüne yükseltin.