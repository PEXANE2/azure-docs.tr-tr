---
title: Çözüm mimarisi
description: Azure Güvenlik Merkezi IoT hizmetinde bilgi akışı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 4cc7d1982555f058a4ea23f7d8a6fdc2d83e484d
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311761"
---
# <a name="azure-security-center-for-iot-architecture"></a>IoT için Azure Güvenlik Merkezi mimarisi

Bu makalede, IoT çözümü için Azure Güvenlik Merkezi'nin işlevsel sistem mimarisi açıklanmaktadır.

## <a name="azure-security-center-for-iot-components"></a>IoT bileşenleri için Azure Güvenlik Merkezi

Azure IoT Güvenlik Merkezi aşağıdaki bileşenlerden oluşur:

- IoT Hub entegrasyonu
- Cihaz aracıları (isteğe bağlı)
- Güvenlik mesajı Gönder SDK
- Analitik boru hattı

### <a name="azure-security-center-for-iot-workflows"></a>IoT iş akışları için Azure Güvenlik Merkezi

Azure Güvenlik Merkezi IoT iki özellik iş akışından birinde çalışır: Yerleşik ve Gelişmiş

### <a name="built-in"></a>Yerleşik

**Yerleşik** modda, IoT Hub'ınızdaki **Güvenlik** seçeneğini açmayı seçdiğinizde IoT için Azure Güvenlik Merkezi etkinleştirilir. Gerçek zamanlı izleme, öneriler ve uyarılar sunan Yerleşik mod, tek adımlı cihaz görünürlüğü ve eşsiz güvenlik sunar. Geliştirme modu, herhangi bir cihaza aracı yüklemesi gerektirmez ve alan aygıtınızı analiz etmek ve korumak için günlüğe kaydedilmiş etkinliklerde gelişmiş analizler kullanır.

### <a name="enhanced"></a>Gelişmiş

**Gelişmiş** modda, IoT Hub'ınızdaki **Güvenlik** seçeneğini açıp aygıtlarınıza IoT aygıt aracıları için Azure Güvenlik Merkezi yükledikten sonra aracılar aygıtlarınızdaki ham güvenlik olaylarını toplar, toplar ve analiz eler. Ham güvenlik olayları IP bağlantıları, işlem oluşturma, kullanıcı oturum açma ve diğer güvenlikle ilgili bilgileri içerebilir. IoT aygıt aracıları için Azure Güvenlik Merkezi, yüksek ağ işlerinden kaçınmaya yardımcı olmak için olay toplama işlemlerini de işler. Aracılar, yalnızca en hızlı SLA'da yalnızca önemli bilgileri göndermek veya geniş güvenlik bilgilerini ve bağlamını daha büyük segmentlere ayırmak ve daha yüksek hizmet maliyetlerinden kaçınmak gibi belirli görevler için bunları kullanmanıza olanak tanıyan son derece özelleştirilebilir.

![IoT için Azure Güvenlik Merkezi mimarisi](./media/architecture/azure-iot-security-architecture.png)

Aygıt aracıları ve diğer uygulamalar, Güvenlik bilgilerini Azure IoT Hub'ına göndermek için **Azure güvenlik iletisi SDK'yı** kullanır. IoT Hub bu bilgileri alır ve IoT hizmeti için Azure Güvenlik Merkezi'ne ileter.

IoT için Azure Güvenlik Merkezi etkinleştirildiğinde, iletilen verilere ek olarak, IoT Hub tüm dahili verilerini IoT için Azure Güvenlik Merkezi tarafından analiz için gönderir. Bu veriler aygıt bulutu işlem günlüklerini, aygıt kimliklerini ve Hub yapılandırmalarını içerir. Tüm bu bilgiler, IoT analiz leri için Azure Güvenlik Merkezi'nin oluşturulmasına yardımcı olur.

Azure Güvenlik Merkezi ioT analitik ardışık boru hattı, Microsoft ve Microsoft iş ortaklarının çeşitli kaynaklarından ek tehdit istihbaratı akışları da alır. Azure Güvenlik Merkezi IoT tüm analitik ardışık ardışık işlem, hizmette yapılan her müşteri yapılandırmasıyla (özel uyarılar ve güvenlik iletisi Gönder SDK'nın kullanımı gibi) çalışır.

IoT için Azure Güvenlik Merkezi, analitik ardışık etki düzenlerini kullanarak, işlem yapılabilir öneriler ve uyarılar oluşturmak için tüm bilgi akışlarını birleştirir. Boru hattı, hem güvenlik araştırmacıları ve uzmanları tarafından oluşturulan özel kuralları hem de standart cihaz davranışı ve risk analizinden sapma arayan makine öğrenimi modellerini içerir.

IoT önerileri ve uyarıları için Azure Güvenlik Merkezi (analitik ardışık işlem çıkışı), her müşterinin Log Analytics çalışma alanına yazılır. Çalışma alanındaki ham olayların yanı sıra uyarılar ve öneriler de dahil olmak üzere, algılanan şüpheli etkinliklerin tam ayrıntılarını kullanarak derin dalış incelemeleri ve sorguları sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT çözümü için Azure Güvenlik Merkezi'nin temel mimarisi ni ve iş akışını öğrendiniz. Ön koşullar hakkında daha fazla bilgi edinmek, nasıl başlatılabilirsiniz ve IoT Hub'da güvenlik çözümünüzü etkinleştirmek için aşağıdaki makalelere bakın:

- [Hizmet önkoşulları](service-prerequisites.md)
- [Başlarken](getting-started.md)
- [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
- [IoT Hub'da güvenliği etkinleştirme](quickstart-onboard-iot-hub.md)
- [IoT SSS için Azure Güvenlik Merkezi](resources-frequently-asked-questions.md)
- [IoT için Azure Güvenlik Merkezi güvenlik uyarıları](concept-security-alerts.md)
