---
title: IoT Hub ağ gereksinimleri için cihaz güncelleştirmesi | Microsoft Docs
description: IoT Hub cihaz güncelleştirmesi, farklı amaçlar için çeşitli ağ bağlantı noktalarını kullanır.
author: philmea
ms.author: philmea
ms.date: 1/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: e72ff144a56f44ccaa695b7dab328e42052fce39
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680089"
---
# <a name="ports-used-with-device-update-for-iot-hub"></a>IoT Hub için cihaz güncelleştirmesiyle kullanılan bağlantı noktaları
ADU farklı amaçlar için çeşitli ağ bağlantı noktaları kullanır.

## <a name="default-ports"></a>Varsayılan bağlantı noktaları

Amaç|Bağlantı Noktası Numarası |
---|---
Ağlardan/CDNs 'den indir  | 80 (HTTP protokolü)
MCC/CDNs 'den indir | 80 (HTTP protokolü)
Azure IoT Hub ADU Aracısı bağlantısı  | 8883 (MQTT Protokolü)

## <a name="use-azure-iot-hub-supported-protocols"></a>Desteklenen Azure IoT Hub protokollerini kullanın
ADU Aracısı desteklenen Azure IoT Hub protokollerinden birini kullanacak şekilde değiştirilebilir.

Desteklenen protokollerin geçerli listesi hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols#:~:text=Table%202%20%20%20,%201%20more%20rows) .
