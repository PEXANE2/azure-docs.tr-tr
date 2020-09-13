---
title: Azure RTOS için güvenlik modülünü yapılandırma ve özelleştirme
description: Azure RTOS için güvenlik modülünüzü yapılandırma ve özelleştirme hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: mlottner
ms.openlocfilehash: eed0422f574e54ff6d7df486b4929850a26418fa
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514933"
---
# <a name="configure-and-customize-security-module-for-azure-rtos-preview"></a>Azure RTOS için güvenlik modülünü yapılandırma ve özelleştirme (Önizleme)

Cihazınızın davranışını yapılandırmak için aşağıdaki dosyayı kullanın.

## <a name="azure_iot_security_moduleincasc_porth"></a>azure_iot_security_module/inc/asc_port. h

 Her yapılandırmanın varsayılan davranışı aşağıdaki tablolarda verilmiştir: 

### <a name="general"></a>Genel

| Ad | Tür | Varsayılan | Ayrıntılar |
| - | - | - | - |
| ASC_SECURITY_MODULE_ID | Dize | --- | Cihazın benzersiz tanımlayıcısı  |
| ASC_SECURITY_MODULE_PENDING_TIME  | Sayı | 300 | Güvenlik modülünün bekleme süresi (saniye). Süre, durum değişikliğini askıya al olarak aşarsa. |

#### <a name="collection"></a>Koleksiyon

| Ad | Tür | Varsayılan | Ayrıntılar |
| - | - | - | - |
| ASC_HIGH_PRIORITY_INTERVAL | Sayı | 10 | Yüksek öncelikli Grup aralığını saniye cinsinden toplayıcılar. |
| ASC_MEDIUM_PRIORITY_INTERVAL | Sayı | 30 | Saniye cinsinden toplayıcı orta öncelik grubu aralığı. |
| ASC_LOW_PRIORITY_INTERVAL | Sayı | 145.440  | Saniyeler içinde düşük öncelikli Grup aralığı toplayıcıları. |

#### <a name="collector-network-activity"></a>Toplayıcı ağ etkinliği

Toplayıcı ağ etkinlik yapılandırmanızı özelleştirmek için aşağıdakileri kullanın:

| Ad | Tür | Varsayılan | Ayrıntılar |
| - | - | - | - |
| ASC_COLLECTOR_NETWORK_ACTIVITY_TCP_DISABLED | Boole | yanlış | `TCP`Ağ etkinliğini filtrele |
| ASC_COLLECTOR_NETWORK_ACTIVITY_UDP_DISABLED | Boole | yanlış | `UDP`Ağ etkinliği olaylarını filtrele |
| ASC_COLLECTOR_NETWORK_ACTIVITY_ICMP_DISABLED | Boole | yanlış | `ICMP`Ağ etkinliği olaylarını filtrele |
| ASC_COLLECTOR_NETWORK_ACTIVITY_CAPTURE_UNICAST_ONLY | Boole | true | Yalnızca bir tek noktaya yayın gelen paketleri yakala, Ayrıca yanlış yakalama olarak ayarlandığında yayın ve çok noktaya yayın |
| ASC_COLLECTOR_NETWORK_ACTIVITY_MAX_IPV4_OBJECTS_IN_CACHE | Sayı | 64 | Bellekte depolanacak en fazla IPv4 ağ olayı sayısı |
| ASC_COLLECTOR_NETWORK_ACTIVITY_MAX_IPV6_OBJECTS_IN_CACHE | Sayı | 64  | Bellekte depolanacak en fazla IPv6 ağ olayı sayısı |


## <a name="compile-flags"></a>Derleme bayrakları
Derleme bayrakları, önceden tanımlanmış konfigürasyonları geçersiz kılmanıza olanak tanır.

### <a name="collectors"></a>Toplayıcıları
| Ad | Tür | Varsayılan | Ayrıntılar |
| - | - | - | - |
| collector_heartbeat_enabled | Boole | AÇIK | Sinyal toplayıcıyı etkinleştirme |
| collector_network_activity_enabled | Boole | AÇIK | Ağ etkinliği toplayıcıyı etkinleştir |
| collector_system_information_enabled | Boole | AÇIK | Sistem bilgisi toplayıcıyı etkinleştir |

## <a name="supported-security-alerts-and-recommendations"></a>Desteklenen güvenlik uyarıları ve önerileri

Azure RTOS için güvenlik modülü, belirli güvenlik uyarılarını ve önerilerini destekler. Hizmetiniz için [ilgili uyarı ve öneri değerlerini incelediğinizden ve özelleştirdiğinizden](concept-rtos-security-alerts-recommendations.md) emin olun.

## <a name="log-analytics-optional"></a>Log Analytics (isteğe bağlı)

İsteğe bağlı ve gerekli olmadığında, cihaz olaylarını ve etkinliklerini daha fazla araştırmak istediğinizde Log Analytics etkinleştirme ve yapılandırma yararlı olabilir. Daha fazla bilgi edinmek için [IoT hizmeti Için Azure Güvenlik Merkezi ile Log Analytics](how-to-security-data-access.md#log-analytics) ayarlama ve kullanma hakkında bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar

- Azure RTOS [güvenlik uyarıları ve önerileri](concept-rtos-security-alerts-recommendations.md) Için güvenlik modülünü gözden geçirin ve özelleştirin
- Gerektiğinde [Azure RTOS API 'Sinin güvenlik modülüne](azure-rtos-security-module-api.md) bakın.

