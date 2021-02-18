---
title: Aracı tabanlı güvenlik özel uyarıları
titleSuffix: Azure Defender for IoT
description: IoT cihazının Özellikler ve hizmeti için Defender 'ı kullanarak özelleştirilebilir güvenlik uyarıları ve önerilen düzeltme hakkında bilgi edinin.
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
ms.date: 2/16/2021
ms.author: shhazam
ms.openlocfilehash: e885566dd067d70fd4800ca96b8729494464da85
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100642588"
---
# <a name="defender-for-iot-devices-custom-security-alerts"></a>IoT cihazları için Defender özel güvenlik uyarıları

IoT için Defender, gelişmiş analiz ve tehdit bilgilerini kullanarak IoT çözümünüzü sürekli olarak analiz ederek kötü amaçlı etkinliğe karşı sizi uyarır.

Uyarıların, benzersiz kurumsal dağıtımda ve yatay olarak olası güvenliğin en verimli göstergeleri olarak davranmasını sağlamak için beklenen cihaz davranışı hakkındaki bilginiz temelinde özel uyarılar oluşturmanızı öneririz.

IoT uyarıları için aşağıdaki Defender listeleri, beklenen IoT cihaz davranışına göre tanımlanabilir. Her uyarının nasıl özelleştirileceği hakkında daha fazla bilgi için bkz. [özel uyarılar oluşturma](quickstart-create-custom-alerts.md).

## <a name="agent-based-security-custom-alerts"></a>Aracı tabanlı güvenlik özel uyarıları

| Önem derecesi | Uyarı adı | Veri kaynağı | Description | Önerilen düzeltme |
|--|--|--|--|--|
| Düşük | Özel uyarı-etkin bağlantı sayısı izin verilen aralığın dışında | Klasik Güvenlik modülü, Azure RTOS | Belirli bir zaman penceresi içindeki etkin bağlantı sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında. | Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen bağlantı listesine ekleyin. |
| Düşük | Özel uyarı-izin verilmeyen bir IP 'ye oluşturulan giden bağlantı | Klasik Güvenlik modülü, Azure RTOS | İzin verilen IP listenizin dışında bir IP 'ye giden bağlantı oluşturuldu. | Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen IP listesine ekleyin. |
| Düşük | Özel uyarı-başarısız yerel oturum açma sayısı izin verilen aralığın dışında | Klasik Güvenlik modülü, Azure RTOS | Belirli bir zaman penceresi içindeki başarısız yerel oturum açma sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-izin verilen kullanıcı listesinde olmayan bir kullanıcının oturum açması | Klasik Güvenlik modülü, Azure RTOS | İzin verilen Kullanıcı listenizin dışında, cihazda oturum açmış yerel bir kullanıcı. | Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidin ve verileri kullanarak cihazı araştırın, kaynağı tanımlayabilir ve ardından bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın. |
| Düşük | Özel uyarı-izin verilmeyen bir işlem yürütüldü | Klasik Güvenlik modülü, Azure RTOS | Cihazda izin verilmeyen bir işlem yürütüldü. | Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidin ve verileri kullanarak cihazı araştırın, kaynağı tanımlayabilir ve ardından bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın. |
|

## <a name="next-steps"></a>Sonraki adımlar

- [Bir uyarıyı özelleştirmeyi](quickstart-create-custom-alerts.md) öğrenin
- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
