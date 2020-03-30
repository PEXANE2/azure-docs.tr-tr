---
title: IoT ön koşullar için Azure Güvenlik Merkezi| Microsoft Dokümanlar
description: Azure Güvenlik Merkezi ioT hizmeti ön koşulları ile başlamak için gereken her şeyin ayrıntıları.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2019
ms.author: mlottner
ms.openlocfilehash: 4440fec98d1f561da6375bcaadba4282076cc53b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71299474"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>IoT ön koşullar için Azure Güvenlik Merkezi

Bu makalede, Azure Güvenlik Merkezi'nin IoT hizmeti için farklı yapı taşları, başlamanız gerekenler hakkında bir açıklama sağlar ve hizmeti anlamanıza yardımcı olacak temel kavramları açıklar. 

## <a name="minimum-requirements"></a>Minimum gereksinimler

- IoT Hub Standart katmanı
    - RBAC rol **Sahibi** düzeyi ayrıcalıkları 
- [Günlük Analitik Çalışma Alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Güvenlik Merkezi (önerilir)
    - Azure Güvenlik Merkezi'nin kullanımı bir öneridir ve bir gereklilik değildir. Azure Güvenlik Merkezi olmadan, IoT Hub'daki diğer Azure kaynaklarınızı görüntüleyemezsiniz. 
 
## <a name="working-with-azure-security-center-for-iot-service"></a>IoT hizmeti için Azure Güvenlik Merkezi ile çalışma

Azure IoT öngörüleri ve raporlamaiçin Azure Güvenlik Merkezi, Azure IoT Hub ve Azure Güvenlik Merkezi'ni kullanarak kullanılabilir. Azure IoT Hub'ınızda IoT için Azure Güvenlik Merkezi'ni etkinleştirmek **için, Sahip** düzeyi ayrıcalıklarına sahip bir hesap gereklidir. IoT Hub'ınızda IoT için ASC'yi etkinleştirdikten sonra, IoT öngörüleri için Azure Güvenlik Merkezi, Azure IoT Hub'ında **Güvenlik** özelliği ve Azure Güvenlik Merkezi'nde **IoT** olarak görüntülenir. 

## <a name="supported-service-regions"></a>Desteklenen hizmet bölgeleri 

IoT için Azure Güvenlik Merkezi şu anda aşağıdaki Azure bölgelerinde KiOT Hub'ları için desteklenir:
  - Orta ABD  
  - Doğu ABD 
  - Doğu ABD 2
  - Orta Batı ABD
  - Batı ABD
  - Batı ABD 2
  - Orta ABD Güney
  - Orta Kuzey ABD
  - Orta Kanada
  - Doğu Kanada 
  - Kuzey Avrupa    
  - Güney Brezilya
  - Orta Fransa  
  - Batı Birleşik Krallık 
  - Güney Birleşik Krallık
  - Batı Avrupa 
  - Kuzey Avrupa 
  - Batı Japonya  
  - Doğu Japonya  
  - Güneydoğu Avustralya
  - Doğu Avustralya
  - Doğu Asya   
  - Güneydoğu Asya
  - Güney Kore - Orta
  - Güney Kore - Güney 
  - Orta Hindistan
  - Güney Hindistan

Azure Güvenlik Merkezi IoT, tüm Avrupa bölgelerinden Batı Avrupa bölgesel veri merkezine ve kalan tüm bölgeleri Orta ABD bölgesel veri merkezine yönlendirir.  
  
## <a name="wheres-my-iot-hub"></a>IoT Hub'ım nerede?

Başlamadan önce hizmet kullanılabilirliğini doğrulamak için IoT Hub konumunuzu kontrol edin. 

1. IoT Hub'ınızı açın. 
2. **Genel Bakış**'a tıklayın. 
3. Listelenen [konumun desteklenen hizmet bölgelerinden](#supported-service-regions)biriyle eşleştiğini doğrulayın. 


## <a name="supported-platforms-for-agents"></a>Aracılar için desteklenen platformlar 

IoT aracıları için Azure Güvenlik Merkezi, büyüyen bir aygıt ve platform listesini destekler. Varolan veya planlanan aygıt kitaplığınızı kontrol etmek için [desteklenen platform listesine](how-to-deploy-agent.md) bakın.  

## <a name="next-steps"></a>Sonraki adımlar
- Azure IoT Güvenlik [Genel Bakışını](overview.md) Okuyun
- [Hizmeti etkinleştirmeyi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT SSS için Azure Güvenlik Merkezi'ni](resources-frequently-asked-questions.md) okuyun
- [IoT uyarıları için Azure Güvenlik Merkezi'ni nasıl anlayacağınizi](concept-security-alerts.md) keşfedin
