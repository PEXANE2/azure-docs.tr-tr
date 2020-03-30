---
title: Azure IoT Central ile sağlık çözümleri oluşturun | Microsoft Dokümanlar
description: Azure IoT Central uygulama şablonlarını kullanarak sağlık hizmeti çözümü oluşturmayı öğrenin.
author: philmea
ms.author: philmea
ms.date: 09/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 2aee5c56b267f9243b69a48dfd0f5f64196c5f23
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77021500"
---
# <a name="building-healthcare-solutions-with-azure-iot-central"></a>Azure IoT Central ile sağlık çözümleri oluşturma 



Uygulama şablonlarını kullanarak Azure IoT Central ile sağlık çözümleri oluşturmayı öğrenin.

## <a name="what-is-continuous-patient-monitoring-template"></a>Sürekli hasta izleme şablonu nedir?

Sağlık IoT alanında, Sürekli Hasta İzleme geri kabul riskini azaltmanın, kronik hastalıkların daha etkili bir şekilde yönetilmesinin ve hasta sonuçlarının iyileştirilmesinin en önemli itici kaynaklarından biridir. Sürekli Hasta İzleme iki ana kategoriye ayrılabilir:

1. **Hasta takibi**: Hastanede tıbbi giyilebilir cihazlar ve diğer cihazları kullanarak, bakım ekipleri hastayı günde birkaç kez kontrol etmesi için bir hemşire göndermek zorunda kalmadan hastanın hayati belirtilerini ve tıbbi durumlarını izleyebilir. Bakım ekipleri, hastanın bildirimler yoluyla kritik ilgiye ihtiyaç duyduğu anı anlayabilir ve zamanlarını etkin bir şekilde önceliklendirebilir.
1. **Uzaktan hasta takibi**: Hastane dışındaki hastaları izlemek için tıbbi giyilebilir cihazlar ve hasta raporu sonuçları (PrO' lar) kullanılarak geri kabul riski azaltılabilir. Kronik hastalık hastalarından ve rehabilitasyon hastalarından elde edilen veriler, hastaların bakım planlarına bağlı kalmalarını ve hasta bozulması uyarılarının kritik hale gelmeden önce bakım ekiplerine su yüzüne çıkarılabilmesini sağlamak için toplanabilir.

Bu uygulama şablonu, Sürekli Hasta İzleme'nin her iki kategorisi için çözümler oluşturmak için kullanılabilir. Avantajlara şunlar dahildir:

* Farklı türde tıbbi giyilebilir leri IoT Central örneğine sorunsuz bir şekilde bağlayın.
* Sağlıklı kalmalarını sağlamak için cihazları izleyin ve yönetin.
* Uygun uyarıları tetiklemek için aygıt verileri etrafında özel kurallar oluşturun.
* Uyumlu bir veri deposu olan FHIR için hasta sağlık verilerinizi Azure API'sine aktarın.
* Toplanan öngörüleri varolan veya yeni iş uygulamalarına aktarın.

>[!div class="mx-imgBorder"] 
>![CPM panosu](media/in-patient-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Sürekli Hasta izleme çözümü oluşturmaya başlamak için:

* [Uygulama şablonu dağıtma](tutorial-continuous-patient-monitoring.md)
* [Örnek mimariye bakın](concept-continuous-patient-monitoring-architecture.md)