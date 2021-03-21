---
title: Aracı portföyüne genel bakış ve işletim sistemi desteği (Önizleme)
description: IoT için Azure Defender, cihaz türüne göre büyük bir aracı portföyü sağlar.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/20/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: f731b034b5d4f795bae51107e9ff4e2e90788d7d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102120156"
---
# <a name="agent-portfolio-overview-and-os-support-preview"></a>Aracı portföyüne genel bakış ve işletim sistemi desteği (Önizleme)

IoT için Azure Defender, cihaz türüne göre büyük bir aracı portföyü sağlar. 

## <a name="standalone-agent"></a>Tek başına aracı

Tek başına aracı, ikili bir paket olarak veya üretici yazılımının bir parçası olarak birleştirilebilir ve müşteri gereksinimlerine göre değişiklik ve özelleştirmeye izin veren bir kaynak kod olarak dağıtılabilen Linux işletim sistemlerinin çoğunu ele alır. İşletim sistemi desteği örneği: 

| İşletim sistemi | 'TÜR | ARM32v7 |
|--|--|--|
| Debian 9 | ✓ | ✓ |
| Ubuntu 18.04 | ✓ |  |
| Ubuntu 20.04 | ✓ |  |

Daha fazla ayrıntı, işletim sistemi desteği veya kaynak koda erişim istemek için, bunu cihaz belleniminin bir parçası olarak birleştirebilmeniz, hesap yöneticinize başvurmanız veya adresine e-posta göndermeniz gerekir <defender_micro_agent@microsoft.com> . 

## <a name="azure-rtos-micro-agent"></a>Azure RTOS mikro aracı

IoT Micro Agent için Azure Defender, Azure RTOS kullanan cihazlar için kapsamlı ve hafif bir güvenlik çözümü sağlar. IoT Micro Agent için Azure Defender, yaygın tehditler ve gerçek zamanlı işletim sistemi (RTOS) cihazlarındaki olası kötü amaçlı etkinlikler için kapsam sağlar. Mikro Aracı, Azure RTOS NetX Duo bileşeninin bir parçası olarak yerleşik olarak sunulur ve cihazın ağ etkinliğini izler. 

IoT Micro Agent için Azure Defender, Azure RTOS NetX Duo bileşeninin bir parçası olarak yerleşik olarak sunulur ve cihazın ağ etkinliğini izler. Mikro Aracı, yaygın tehditler ve gerçek zamanlı bir işletim sistemi (RTOS) cihazlarındaki olası kötü amaçlı etkinlikler için kapsam sağlayan kapsamlı ve hafif bir güvenlik çözümünden oluşur.

## <a name="next-steps"></a>Sonraki adımlar

[Tek başına mikro aracıya genel bakış (Önizleme)](concept-standalone-micro-agent-overview.md)hakkında daha fazla bilgi edinin.