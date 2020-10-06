---
title: Azure RTOS için güvenlik modülü genel bakış
description: IoT hizmeti için Defender 'ın bir parçası olarak Azure RTOS desteği ve uygulama için güvenlik modülü hakkında daha fazla bilgi edinin
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
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: 22bd12bbdcccef2fd0e9010f926cd18e95d42967
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91761864"
---
# <a name="overview-defender-for-iot-security-module-for-azure-rtos-preview"></a>Genel Bakış: Azure RTOS için IoT güvenlik modülü için Defender (Önizleme)

RTOS için IoT güvenlik modülü için Defender, Azure RTOS cihazları için kapsamlı bir güvenlik çözümü sağlar. Azure RTOS artık yerleşik Azure IoT güvenlik modülü ile birlikte gelir ve gerçek zamanlı işletim sistemi cihazlarındaki yaygın tehditler ve potansiyel kötü amaçlı etkinlikler için kapsam sağlar. 

![IoT için Defender Azure RTOS](./media/architecture/azure-rtos-security-monitoring.png)


Azure RTOS için güvenlik modülü aşağıdaki özellikleri sunar: 
- Kötü amaçlı ağ etkinliği algılama
- Özel uyarı tabanlı, cihaz davranışı taban çizgisi
- Cihaz güvenliğini iyileştirme durumu

## <a name="detection-of-malicious-network-activities"></a>Kötü amaçlı ağ etkinliklerinin algılanması

Her cihazın gelen ve giden ağ etkinliği izlenir (desteklenen protokoller: TCP, UDP, IPv4 ve IPv6 üzerinde ıCMP). IoT için Defender, bu ağ etkinliklerinin her birini Microsoft Threat Intelligence akışına karşı inceler. Akış, dünya genelinde toplanan milyonlarca benzersiz tehdit göstergesi ile gerçek zamanlı olarak güncelleştirilir. 

## <a name="device-behavior-baselining-based-on-custom-alerts"></a>Özel uyarılara dayalı olarak cihaz davranışı taban çizgisi

Taban çizgisi, güvenlik gruplarına cihaz kümelemesini ve her bir grubun beklenen davranışını tanımlamayı sağlar. IoT cihazları genellikle iyi tanımlanmış ve sınırlı senaryolarda çalışacak şekilde tasarlandığından, bir dizi parametre kullanarak beklenen davranışlarını tanımlayan bir taban çizgisi oluşturmak kolaydır. Taban çizgisinden alınan her türlü sapma, bir uyarıyı tetikler. 

## <a name="improve-your-device-security-hygiene"></a>Cihazınızın güvenlik durumu 'yi geliştirme

IoT için önerilen altyapı Defender 'ı kullanarak, ortamınızdaki sorunları etkileyen ve cihazlarınızın güvenlik duruşunu etkileyebilecek sorunlar hakkında bilgi ve Öngörüler elde edin. Zayıf IoT cihaz güvenliği, her zaman bir kuruluştaki en zayıf bağlantı ile ölçüldüğünde, kötü bir şekilde bırakılırsa olası saldırıların başarılı olmasını sağlayabilir. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Azure RTOS cihazlarını korumaya başlama

Azure RTOS için güvenlik modülü, cihazlarınız için ücretsiz bir indirme olarak sunulmaktadır. IoT bulut hizmeti için Defender, Azure aboneliği başına 30 günlük deneme sürümü ile kullanılabilir. Başlamak için [Azure RTOS güvenlik modülünü](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/defender-for-iot/iot-security-azure-rtos.md) indirin. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure RTOS hizmeti için güvenlik modülü hakkında bilgi edindiniz. Güvenlik modülü hakkında daha fazla bilgi edinmek ve kullanmaya başlamak için aşağıdaki makalelere bakın:

- [Azure RTOS IoT güvenlik modülü kavramları](concept-rtos-security-module.md)
- [Hızlı başlangıç: Azure RTOS IoT güvenlik modülü](quickstart-azure-rtos-security-module.md)
