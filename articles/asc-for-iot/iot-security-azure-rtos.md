---
title: Azure RTOS desteği
description: IoT hizmeti için Azure Güvenlik Merkezi 'nde Azure RTOS desteği hakkında bilgi edinin.
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
ms.date: 07/15/2020
ms.author: mlottner
ms.openlocfilehash: af4579d25b94fc12f67cfc91d0f6a5af4b35c810
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099750"
---
# <a name="azure-security-center-for-iot-security-solution-for-azure-rtos"></a>Azure RTOS için IoT güvenlik çözümü için Azure Güvenlik Merkezi 

IoT güvenlik modülü için Azure Güvenlik Merkezi, Azure RTOS cihazları için kapsamlı bir güvenlik çözümü sağlar. Azure RTOS, gerçek zamanlı işletim sistemi cihazlarındaki yaygın tehditleri ele alan yerleşik bir güvenlik modülüyle birlikte dağıtılır. 

![IoT Azure RTOS için Azure Güvenlik Merkezi](./media/architecture/azure-rtos-security-monitoring.png)


Azure RTOS desteğiyle IoT güvenlik modülü için Azure Güvenlik Merkezi aşağıdaki özellikleri sunar: 
- Kötü amaçlı ağ etkinliği algılama
- Özel uyarı tabanlı, cihaz davranışı taban çizgisi
- Cihaz güvenliğini iyileştirme durumu

### <a name="detection-of-malicious-network-activities"></a>Kötü amaçlı ağ etkinliklerinin algılanması

Her cihazın gelen ve giden ağ etkinliği izlenir (desteklenen protokoller: TCP, UDP, IPv4 ve IPv6 üzerinde ıCMP). IoT için Azure Güvenlik Merkezi, bu ağ etkinliklerinin her birini Microsoft Threat Intelligence akışına karşı denetler. Akış, dünya genelinde toplanan milyonlarca benzersiz tehdit göstergesi ile gerçek zamanlı olarak güncelleştirilir. 

### <a name="device-behavior-baselining-based-on-custom-alerts"></a>Özel uyarılara dayalı olarak cihaz davranışı taban çizgisi

Taban çizgisi, güvenlik gruplarına cihaz kümelemesini ve her bir grubun beklenen davranışını tanımlamayı sağlar. IoT cihazları genellikle iyi tanımlanmış ve sınırlı senaryolarda çalışacak şekilde tasarlandığından, bir dizi parametre kullanarak beklenen davranışlarını tanımlayan bir taban çizgisi oluşturmak kolaydır. Taban çizgisinden alınan her türlü sapma, bir uyarıyı tetikler. 

### <a name="improve-your-device-security-hygiene"></a>Cihazınızın güvenlik durumu 'yi geliştirme

IoT için Azure Güvenlik Merkezi 'nin önerilen altyapısından yararlanarak, ortamınızdaki sorunlar hakkında bilgi ve Öngörüler sunarak cihazlarınızın güvenlik duruşunu etkiler ve bu makaleye zarar verir. Zayıf IoT cihaz güvenliği, her zaman bir kuruluştaki en zayıf bağlantı ile ölçüldüğünde, kötü bir şekilde bırakılırsa olası saldırıların başarılı olmasını sağlayabilir. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Azure RTOS cihazlarını korumaya başlama

- Azure RTOS için IoT güvenlik modülü için Azure Güvenlik Merkezi, cihazlarınız için ücretsiz bir indirme olarak sunulmaktadır. IoT bulut hizmeti için Azure Güvenlik Merkezi, Azure aboneliği başına 30 günlük deneme sürümü ile kullanılabilir. Kullanmaya başlamak için [Azure RTOS Için IoT güvenlik modülünü Azure Güvenlik Merkezi](https://github.com/azure-rtos/iot-security-module-preview) ' ni indirin. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede IoT Azure RTOS desteği için Azure Güvenlik Merkezi hakkında bilgi edindiniz. IoT Hub 'de Güvenlik çözümünüzü kullanmaya başlamanızı ve etkinleştirmeyi öğrenmek için aşağıdaki makalelere bakın:

- [Hizmet önkoşulları](service-prerequisites.md)
- [Başlarken](getting-started.md)
- [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
- [IoT Hub güvenliği etkinleştir](quickstart-onboard-iot-hub.md)
- [IoT için Azure Güvenlik Merkezi SSS](resources-frequently-asked-questions.md)
- [IoT için Azure Güvenlik Merkezi güvenlik uyarıları](concept-security-alerts.md)
