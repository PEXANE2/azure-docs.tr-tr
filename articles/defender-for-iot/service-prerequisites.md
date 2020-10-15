---
title: Önkoşulları & bileşenler
description: IoT hizmeti önkoşulları için Azure Defender 'ı kullanmaya başlamak üzere gereken her şeyin ayrıntıları.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: rkarlin
ms.custom: references_regions
ms.openlocfilehash: b0913dc48f807c26396a38e31d293877b4561b7d
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089188"
---
# <a name="azure-defender-for-iot-prerequisites"></a>IoT önkoşulları için Azure Defender

Bu makalede, IoT hizmeti için Defender 'ın farklı bileşenlerinin, başlamak için ne yapmanız gerektiği ve hizmetin anlaşılması için temel kavramların açıklaması sağlanmaktadır.

## <a name="minimum-requirements"></a>Minimum gereksinimler

- IoT ve OT cihazları için Aracısız izleme (Six teknolojisine bağlı olarak)
    - Bağlantı noktası üzerinden trafik izlemeyi destekleyen ağ anahtarları
    - NTA algılayıcısı için donanım araçları, daha fazla bilgi için bkz. [sertifikalı donanım](https://aka.ms/AzureDefenderforIoTBareMetalAppliance)
    - Azure aboneliği **katkıda bulunan** rolü (yalnızca taahhüt edilen cihazları tanımlamaya yönelik ekleme sırasında gereklidir)
    - IoT Hub (ücretsiz veya Standart katman) **katkıda bulunan** rolü (bulut bağlı yönetimi için)
- Azure IoT Hub ile yönetilen yönetilen IoT cihazları için güvenlik
    - IoT Hub (Standart katman) **katılımcısı** rolü
    - IoT Hub: **IoT özelliği Için Azure Defender** geçişi etkinleştirilmelidir
    - Cihaz düzeyi güvenlik modülü desteği için  
        - IoT aracıları için Defender, büyüyen bir cihaz ve platform listesini destekler, [desteklenen platform listesine](how-to-deploy-agent.md) bakın


## <a name="supported-service-regions"></a>Desteklenen hizmet bölgeleri

Daha fazla bilgi için bkz. [IoT Hub desteklenen bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub) . 

IoT için Defender, tüm Avrupa bölgelerinden tüm trafiği Batı Avrupa bölgesel veri merkezine ve kalan tüm bölgeleri Orta ABD bölgesel veri merkezine yönlendirir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure IoT güvenliğine [genel bakış](overview.md) konusunu okuyun
- [Hizmeti etkinleştirmeyi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT Için Defender 'ı okuyun SSS](resources-frequently-asked-questions.md)
- Bkz. [Defender 'ı IoT uyarıları için nasıl anlayacağınızı](concept-security-alerts.md) keşfet
