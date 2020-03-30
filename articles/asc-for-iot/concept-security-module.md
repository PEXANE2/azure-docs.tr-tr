---
title: IoT güvenlik modülü için Azure Güvenlik Merkezi'ni anlayın| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi'nde güvenlik modülü ikizleri kavramı ve bunların nasıl kullanıldığı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: ab3b6e740e644a1ed1495eb776045888be448047
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68596506"
---
# <a name="security-module"></a>Güvenlik modülü


Bu makalede, Azure Güvenlik Merkezi iot için aygıt ikizleri ve modüllerini nasıl kullandığı açıklanmaktadır. 

## <a name="device-twins"></a>Cihaz ikizleri

Azure'da yerleşik IoT çözümleri için aygıt ikizleri hem cihaz yönetiminde hem de proses otomasyonunda önemli bir rol oynar.  

Azure Güvenlik Merkezi IoT, mevcut IoT aygıt yönetim platformunuzla tam entegrasyon sunarak cihaz güvenlik durumunuzu yönetmenize ve mevcut aygıt kontrol özelliklerinden yararlanmanıza olanak tanır. Entegrasyon, IoT Hub çift mekanizmasından yararlanılarak sağlanır.  

Azure IoT Hub'da [aygıt ikizleri](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) kavramı hakkında daha fazla bilgi edinin. 

## <a name="security-module-twins"></a>Güvenlik modülü ikizleri

Azure Güvenlik Merkezi ioT, hizmetteki her aygıt için bir güvenlik modülü ikizi tutar.
Güvenlik modülü ikizi, çözümünüzdeki her bir cihaz için aygıt güvenliğiyle ilgili tüm bilgileri tutar.
Aygıt güvenlik özellikleri, daha güvenli iletişim ve daha az kaynak gerektiren güncelleştirmeleri ve bakımı etkinleştirmek için özel bir güvenlik modülü ikizinde korunur.  

Bkz. [İkiz'i](quickstart-create-security-twin.md) nasıl oluşturup, özelleştirecek ve yapılandıracak şekilde öğrenmek için güvenlik [aracılarını](how-to-agent-configuration.md) oluştur ve yapılandırır. IoT [Hub'da](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) modül ikizleri kavramı hakkında daha fazla bilgi edinmek için anlama modülü ikizleri bölümüne bakın. 
 

## <a name="see-also"></a>Ayrıca bkz.
- [IoT genel bakışı için Azure Güvenlik Merkezi](overview.md)
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
- [Güvenlik aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)