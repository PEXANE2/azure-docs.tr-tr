---
title: IoT için Azure Defender
description: IoT için Azure Defender hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 2f41aac085fef62e0356a93a07823f21d7ba3667
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91448337"
---
# <a name="introduction-to-azure-defender-for-iot"></a>IoT için Azure Defender 'a giriş

Güvenlik yönetimini bütünleştirin ve hibrit bulut iş yükleri ve Azure IoT çözümünüz genelinde uçtan uca tehdit algılamayı ve analizini etkinleştirin.

IoT için Azure Defender, şunları sağlar:

- Cihaz üreticisi, cihaz türü ve cihazların ağ üzerinde nasıl iletişim kurduğu gibi ayrıntılar da dahil olmak üzere **varlık bulma ve ağ eşleme**
- **Güvenlik açığı yönetimi**, ckollar, açık bağlantı noktaları ve yetkisiz internet bağlantıları hakkında bilgiler de dahil
- Hedeflenmiş saldırı veya kötü amaçlı yazılım gibi anormal veya yetkisiz etkinlikleri belirten gerçek zamanlı uyarılarla **sürekli tehdit izleme**

Tüm Ayrıntılar [adanmış belgelerde](https://docs.microsoft.com/azure/asc-for-iot/overview)kullanılabilir.

## <a name="availability"></a>Kullanılabilirlik
|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|[Azure Defender](security-center-pricing.md) gerektirir|
|Gerekli roller ve izinler:|Makinenin NSG 'leri üzerinde yazma izinleri|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-devices-can-azure-defender-for-iot-secure"></a>IoT için Azure Defender hangi cihazları güvende?
Güvenlik Merkezi, Azure Defender ve Six Agentless teknolojisinin yeteneklerini birleştirerek şunları gerçekleştirebilirsiniz:

- IoT Hub ile bağlı olan **doğa alanı cihazları** . Bu, kuruluşların hem modern, hem de internet Native cihazları ve geleneksel ICS/SCADA cihazlarını tek bir birleştirilmiş çözümle güvenli hale getirerek IoT girişimlerini hızlandırmalarını sağlar.
    - Güvenlik standartlarıyla uyumluluğu ve geliştirilmiş güvenlik duruşunu sağlamak için yeni cihazlar ekleme ve iş yükleriniz genelinde güvenlik ilkeleri uygulama (yaprak cihazlar, Microsoft Edge cihazları, IoT Hub).

- Üretim, yönetim sistemleri (BMS), yaşam bilimleri, enerji ve su yardımcı programları, yağ & gaz ve lojistik gibi operasyonel teknoloji (OT) ortamlarında kullanılan **yönetilmeyen brownfield cihazları** . 
    - Yönetilmeyen cihazları korumak için IoT için Azure Defender, OT ortamları üzerinde sıfır performans etkisi olan pasif, yerleşik olmayan ağ trafiği analizi (NTA) için şirket içi bir algılayıcı kullanır. 
    - Ayrıca, özel IoT/OT protokollerinin yanı sıra, patentli, IoT/OT kullanan davranış analizi ve makine öğrenimi ile birlikte, herhangi bir kural veya imza yapılandırma gereksinimini ortadan kaldırmak için, her türlü kuralı ve imzayı yapılandırma ihtiyacını ortadan kaldırır. 


## <a name="azure-defender-for-iot-integration-with-azure-sentinel"></a>Azure Sentinel ile IoT tümleştirmesi için Azure Defender
Birleşik BT/OT güvenlik izleme ve idare özelliğini etkinleştirmek için IoT için Azure Defender, [Azure Sentinel](../sentinel/overview.md)ile yerel olarak tümleştirilir.

SecOps ekipleri şunları yapabilir:

- Sentinel 'e özgü SOAR PlayBook 'lar aracılığıyla IoT/OT tehditleri hızla algılayın ve yanıtlayın
- Microsoft 'un Şirket içi IoT/OT tehdit bilgileri ekibi, Bölüm 52 tarafından sağlanan IoT/OT 'e özgü tehdit zekasından yararlanın
- Mevcut SOC iş akışlarıyla ve splunk, IBM QRadar ve ServiceNow gibi üçüncü taraf güvenlik araçlarıyla IoT için Azure Defender 'ı tümleştirin


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Güvenlik Merkezi 'nde IoT için Azure Defender hakkında bilgi edindiniz. Daha fazla bilgi için bkz.

- [IoT için Azure Güvenlik Merkezi’ne giriş](../asc-for-iot/overview.md)
