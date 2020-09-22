---
title: Önkoşulları & bileşenler
description: IoT hizmeti önkoşulları için Azure Defender 'ı kullanmaya başlamak üzere gereken her şeyin ayrıntıları.
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
ms.date: 09/09/2020
ms.author: mlottner
ms.custom: references_regions
ms.openlocfilehash: 19263f8db58c8d20288d3ae74c24efd85667bc33
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941159"
---
# <a name="azure-defender-for-iot-prerequisites"></a>IoT önkoşulları için Azure Defender

Bu makalede, IoT hizmeti için Defender 'ın farklı bileşenlerinin, başlamak için ne yapmanız gerektiği ve hizmetin anlaşılması için temel kavramların açıklaması sağlanmaktadır.

## <a name="minimum-requirements"></a>Minimum gereksinimler

- Standart katman IoT Hub
  - Azure rolü **sahip** düzeyi ayrıcalıkları
- [Log Analytics Çalışma Alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)
- Azure Güvenlik Merkezi (önerilir)
  - Azure Güvenlik Merkezi 'nin kullanımı, gereksinim değil, öneridir. Azure Güvenlik Merkezi olmadan, diğer Azure kaynaklarınızı IoT Hub içinde görüntüleyemedik.

## <a name="working-with-defender-for-iot-service"></a>IoT hizmeti için Defender ile çalışma

Azure IoT Hub ve Azure Güvenlik Merkezi kullanılarak IoT öngörüleri ve raporlama için Defender kullanılabilir. Azure IoT Hub IoT için Defender 'ı etkinleştirmek üzere, **sahip** düzeyi ayrıcalıklarına sahip bir hesap gereklidir. IoT Hub IoT için ASC etkinleştirildikten sonra, IoT Öngörüler için Defender, Azure Güvenlik Merkezi 'nde Azure IoT Hub ve **IoT** olarak **güvenlik** özelliği olarak görüntülenir.

## <a name="supported-service-regions"></a>Desteklenen hizmet bölgeleri

IoT için Defender Şu anda aşağıdaki Azure bölgelerindeki IoT Hub 'Ları için desteklenmektedir:

- Central US
- Doğu ABD
- Doğu ABD 2
- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Güney Orta ABD
- Orta Kuzey ABD
- Orta Kanada
- Doğu Kanada
- Kuzey Avrupa
- Brezilya Güney
- Orta Fransa
- Batı Birleşik Krallık
- Güney Birleşik Krallık
- West Europe
- Kuzey Avrupa
- Batı Japonya
- Doğu Japonya
- Avustralya Güneydoğu
- Doğu Avustralya
- Doğu Asya
- Güneydoğu Asya
- Güney Kore - Orta
- Güney Kore - Güney
- Orta Hindistan
- Güney Hindistan

IoT için Defender, tüm Avrupa bölgelerinden tüm trafiği Batı Avrupa bölgesel veri merkezine ve kalan tüm bölgeleri Orta ABD bölgesel veri merkezine yönlendirir.

## <a name="wheres-my-iot-hub"></a>IoT Hub nerede?

Başlamadan önce hizmet kullanılabilirliğini doğrulamak için IoT Hub konumunuzu denetleyin.

1. IoT Hub açın.
1. **Genel Bakış**'a tıklayın.
1. Listelenen konumun [desteklenen hizmet bölgelerinden](#supported-service-regions)biriyle eşleştiğini doğrulayın.

## <a name="supported-platforms-for-agents"></a>Aracılar için desteklenen platformlar

IoT aracıları için Defender, büyüyen bir cihaz ve platform listesini destekler. Mevcut veya planlı cihaz kitaplığınızı denetlemek için [desteklenen platform listesine](how-to-deploy-agent.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure IoT güvenliğine [genel bakış](overview.md) konusunu okuyun
- [Hizmeti etkinleştirmeyi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT Için Defender 'ı okuyun SSS](resources-frequently-asked-questions.md)
- Bkz. [Defender 'ı IoT uyarıları için nasıl anlayacağınızı](concept-security-alerts.md) keşfet
