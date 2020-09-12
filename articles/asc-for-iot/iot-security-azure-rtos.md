---
title: Azure RTOS için güvenlik modülü genel bakış
description: IoT hizmeti için Azure Güvenlik Merkezi 'nin bir parçası olarak Azure RTOS desteği ve uygulama için güvenlik modülü hakkında daha fazla bilgi edinin.
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
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: bcf98f503b0f0c8bf76435d9884297d5f53d65e6
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514484"
---
# <a name="overview-security-module-for-azure-rtos-preview"></a>Genel Bakış: Azure RTOS için güvenlik modülü (Önizleme)

IoT RTOS güvenlik modülü için Azure Güvenlik Merkezi, Azure RTOS cihazları için kapsamlı bir güvenlik çözümü sağlar. Azure RTOS artık yerleşik Azure IoT güvenlik modülü ile birlikte gelir ve gerçek zamanlı işletim sistemi cihazlarındaki yaygın tehditler ve potansiyel kötü amaçlı etkinlikler için kapsam sağlar. 

![IoT Azure RTOS için Azure Güvenlik Merkezi](./media/architecture/azure-rtos-security-monitoring.png)


Azure RTOS için güvenlik modülü aşağıdaki özellikleri sunar: 
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

Azure RTOS için güvenlik modülü, cihazlarınız için ücretsiz bir indirme olarak sunulmaktadır. IoT bulut hizmeti için Azure Güvenlik Merkezi, Azure aboneliği başına 30 günlük deneme sürümü ile kullanılabilir. Başlamak için [Azure RTOS güvenlik modülünü](https://github.com/azure-rtos/iot-security-module-preview) indirin. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure RTOS hizmeti için güvenlik modülü hakkında bilgi edindiniz. Güvenlik modülü hakkında daha fazla bilgi edinmek ve kullanmaya başlamak için aşağıdaki makalelere bakın:

- [Azure RTOS IoT güvenlik modülü kavramları](concept-rtos-security-module.md)
- [Hızlı başlangıç: Azure RTOS IoT güvenlik modülü](quickstart-azure-rtos-security-module.md)


