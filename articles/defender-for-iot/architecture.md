---
title: Çözüm mimarisi
description: IoT hizmeti için Azure Defender 'daki bilgi akışı hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: elazark
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2020
ms.author: v-ekrieg
ms.openlocfilehash: 3fc695770350e5a60ae3da9ab1796da5cac99370
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843422"
---
# <a name="azure-defender-for-iot-architecture"></a>IoT mimarisi için Azure Defender

Bu makalede, IoT için Defender çözümünün işlevsel sistem mimarisi açıklanmaktadır.

## <a name="defender-for-iot-components"></a>IoT bileşenleri için Defender

IoT için Defender aşağıdaki bileşenlerden oluşur:

- IoT Hub tümleştirme
- Cihaz aracıları (isteğe bağlı)
- Güvenlik iletisi SDK 'sını gönder
- Analiz işlem hattı

### <a name="defender-for-iot-workflows"></a>IoT iş akışları için Defender

IoT için Defender, iki özellik iş akışlarından birinde çalışmaktadır: yerleşik ve geliştirilmiş

### <a name="built-in"></a>Yerleşik

**Yerleşik** modda, IoT Hub **güvenlik** seçeneğini etkinleştirmek istediğinizde IoT için Defender etkinleştirilir. Gerçek zamanlı izleme, öneriler ve uyarılar sunma, yerleşik mod, tek adımlı cihaz görünürlüğü ve eşleşmeyen güvenlik sunar. Derleme modu, hiçbir cihazda aracı yüklemesi gerektirmez ve alan cihazınızı çözümlemek ve korumak için günlüğe kaydedilen etkinliklerde gelişmiş çözümlemeler kullanır.

### <a name="enhanced"></a>Gelişmiş

**Gelişmiş** modda, IoT Hub **güvenlik** seçeneğini açıp, cihazlarınızdaki IoT cihaz aracıları için Defender 'ı yükledikten sonra, aracılar cihazlarınızdan ham güvenlik olaylarını toplar, toplar ve analiz eder. Ham güvenlik olayları, IP bağlantıları, işlem oluşturma, Kullanıcı oturum açmaları ve güvenlikle ilgili diğer bilgileri içerebilir. IoT cihaz aracıları için savunma 'lar, yüksek ağ aktarım hızını önlemeye yardımcı olmak için olay toplamayı da işler. Aracılar yüksek düzeyde özelleştirilebilir ve en hızlı SLA 'da yalnızca önemli bilgileri göndermek ya da kapsamlı güvenlik bilgilerini ve bağlamı daha büyük kesimlerde toplamak, daha yüksek hizmet maliyetlerine izin vermek gibi belirli görevler için bunları kullanmanıza olanak sağlar.

![IoT mimarisi için Defender](./media/architecture/azure-iot-security-architecture.png)

Cihaz aracıları ve diğer uygulamalar, güvenlik bilgilerini Azure IoT Hub 'a göndermek için **Azure güvenlik iletisi gönder SDK 'sını** kullanır. IoT Hub bu bilgileri alır ve IoT hizmeti için Defender 'a iletir.

IoT hizmeti için Defender etkinleştirildikten sonra, iletilen verilere ek olarak, IoT Hub IoT için Defender tarafından analiz edilmek üzere tüm iç verileri de gönderir. Bu veriler cihaz bulutu işlem günlükleri, cihaz kimlikleri ve hub yapılandırması içerir. Bu bilgilerin tümü, IoT Analytics işlem hattı için Defender oluşturmaya yardımcı olur.

IoT Analytics işlem hattı için Defender, Microsoft ve Microsoft iş ortakları içindeki çeşitli kaynaklardan ek tehdit bilgileri akışları da alır. Tüm analiz işlem hattı için Defender, hizmette yapılan tüm müşteri yapılandırmaları (özel uyarılar ve güvenlik iletisi gönder SDK 'Sı) ile birlikte kullanılır.

IoT için Defender, analiz işlem hattını kullanarak, eyleme dönüştürülebilir öneriler ve uyarılar oluşturmak için tüm bilgi akışlarını birleştirir. İşlem hattı, güvenlik araştırmacıları ve uzmanlar tarafından oluşturulan özel kuralların yanı sıra standart cihaz davranışından ve risk analizinden sapma arayan makine öğrenimi modellerini içerir.

Her müşterinin Log Analytics çalışma alanına IoT önerileri ve uyarıları için Defender (analiz işlem hattı çıkışı) yazılır. Çalışma alanındaki ham olayların yanı sıra uyarı ve öneriler de dahil olmak üzere, algılanan şüpheli etkinliklerin tam ayrıntılarını kullanarak derinlemesine araştırma ve sorgular sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT çözümü için Defender 'ın temel mimarisi ve iş akışı hakkında bilgi edindiniz. Önkoşullar hakkında daha fazla bilgi edinmek ve IoT Hub ' de Güvenlik çözümünüzü etkinleştirmek için aşağıdaki makalelere bakın:

- [Hizmet önkoşulları](service-prerequisites.md)
- [Başlarken](getting-started.md)
- [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
- [IoT Hub güvenliği etkinleştir](quickstart-onboard-iot-hub.md)
- [IoT için Defender SSS](resources-frequently-asked-questions.md)
- [IoT güvenlik uyarıları için Defender](concept-security-alerts.md)
