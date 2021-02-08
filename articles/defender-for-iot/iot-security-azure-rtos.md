---
title: Azure RTOS için güvenlik modülü genel bakış
description: IoT için Azure Defender 'ın bir parçası olarak Azure RTOS desteği ve uygulama için güvenlik modülü hakkında daha fazla bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2021
ms.author: shhazam
ms.openlocfilehash: 47b2b49a21a82be13c987b95c7c775f5014a532e
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809091"
---
# <a name="overview-defender-for-iot-security-module-for-azure-rtos-preview"></a>Genel Bakış: Azure RTOS için IoT güvenlik modülü için Defender (Önizleme)

IoT mikro modülü için Azure Defender, Azure RTOS kullanan cihazlar için kapsamlı bir güvenlik çözümü sağlar. Ortak tehditler ve gerçek zamanlı işletim sistemi (RTOS) cihazlarındaki olası kötü amaçlı etkinlikler için kapsam sağlar. Azure RTOS, içinde yerleşik olarak bulunan Azure IoT güvenlik modülü ile birlikte kullanıma sunulmuştur.

:::image type="content" source="./media/architecture/azure-rtos-security-monitoring.png" alt-text="IoT Azure RTOS için Defender görselleştirme.":::


Azure RTOS için mikro modül aşağıdaki özellikleri sunar:

- Kötü amaçlı ağ etkinliği algılama
- Özel uyarı tabanlı cihaz davranışı taban çizgisi
- İyileştirilmiş cihaz güvenliği durumu

## <a name="detect-malicious-network-activities"></a>Kötü amaçlı ağ etkinliklerini Algıla

Her cihazın gelen ve giden ağ etkinliği izlenir. Desteklenen protokoller, IPv4 ve IPv6 üzerinde TCP, UDP ve ıCMP ' dir. IoT için Defender, bu ağ etkinliklerinin her birini Microsoft Threat Intelligence akışına karşı inceler. Akış, dünya genelinde toplanan milyonlarca benzersiz tehdit göstergesi ile gerçek zamanlı olarak güncelleştirilir.

## <a name="device-behavior-baselining-based-on-custom-alerts"></a>Özel uyarılara dayalı olarak cihaz davranışı taban çizgisi

Taban çizgisi, güvenlik gruplarına cihaz kümelemesini ve her bir grubun beklenen davranışını tanımlamayı sağlar. IoT cihazları genellikle iyi tanımlanmış ve sınırlı senaryolarda çalışacak şekilde tasarlandığından, bir dizi parametre kullanarak beklenen davranışlarını tanımlayan bir taban çizgisi oluşturmak kolaydır. Taban çizgisinin herhangi bir sapması bir uyarıyı tetikler.

## <a name="improve-your-device-security-hygiene"></a>Cihazınızın güvenlik durumu 'yi geliştirme

IoT için önerilen altyapı Defender 'ı kullanarak, ortamınızdaki sorunlar hakkında bilgi ve Öngörüler sunarak cihazlarınızın güvenlik duruşunu etkiler ve bu bilgileri elde edebilirsiniz. Zayıf bir IoT-cihaz güvenlik duruşi, değişmeden ayrıldıysa olası saldırıların başarılı olmasını sağlayabilir. Güvenlik, her zaman kuruluş içindeki en zayıf bağlantı ile ölçülür.

## <a name="get-started-protecting-azure-rtos-devices"></a>Azure RTOS cihazlarını korumaya başlama

Azure RTOS için güvenlik modülü, cihazlarınız için ücretsiz bir indirme olarak sunulmaktadır. IoT bulut hizmeti için Defender, Azure aboneliği başına 30 günlük deneme sürümü ile kullanılabilir. Başlamak için [Azure RTOS güvenlik modülünü](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/defender-for-iot/iot-security-azure-rtos.md)indirin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure RTOS için güvenlik modülü hakkında bilgi edindiniz. Güvenlik modülü hakkında daha fazla bilgi edinmek ve kullanmaya başlamak için aşağıdaki makalelere bakın:

- [Azure RTOS IoT güvenlik modülü kavramları](concept-rtos-security-module.md)
- [Hızlı başlangıç: Azure RTOS IoT güvenlik modülü](quickstart-azure-rtos-security-module.md)
