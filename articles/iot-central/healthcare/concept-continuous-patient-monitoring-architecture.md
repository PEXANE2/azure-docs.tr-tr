---
title: Azure IoT Central'da sürekli hasta izleme mimarisi | Microsoft Dokümanlar
description: Sürekli hasta izleme çözüm mimarisi hakkında bilgi edinin.
author: philmea
ms.author: philmea
ms.date: 10/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 92eb4157abb55b7056952d1fb064c7c7d7500335
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77021705"
---
# <a name="continuous-patient-monitoring-architecture"></a>Sürekli hasta izleme mimarisi



Sürekli hasta izleme çözümleri, sağlanan uygulama şablonu kullanılarak ve aşağıda kılavuz olarak özetlenen mimari kullanılarak oluşturulabilir.

>[!div class="mx-imgBorder"] 
>![CPM Mimarisi](media/cpm-architecture.png)

1. Bluetooth Düşük Enerji (BLE) kullanarak iletişim kuran tıbbi cihazlar
1. BLE verilerini alan ve IoT Central'a gönderen cep telefonu ağ geçidi
1. FHIR için Azure API'sine hasta sağlığı verilerinin sürekli veri aktarım&reg;
1. Birlikte çalışabilir verilere dayalı makine öğrenimi
1. FHIR verileri üzerine inşa edilen bakım ekibi panosu

## <a name="details"></a>Ayrıntılar
Bu bölümde, mimari diyagramın her bölümü daha ayrıntılı olarak özetlenmiştir.

### <a name="ble-medical-devices"></a>BLE tıbbi cihazlar
Sağlık IoT alanında kullanılan birçok tıbbi giyilebilir Bluetooth Düşük Enerji cihazlardır. Bulutla doğrudan konuşamıyorlar ve bir ağ geçidinden geçmeleri gerekecek. Bu mimari, bu ağ geçidi olarak bir cep telefonu uygulaması kullanma önerir.

### <a name="mobile-phone-gateway"></a>Cep telefonu ağ geçidi
Cep telefonu uygulamasının birincil işlevi, tıbbi cihazlardaki BLE verilerini alıp Azure IoT Central'a iletmektir. Ayrıca, uygulama bir cihaz kurulumu ve sağlama akışı ile hastalara rehberlik yardımcı olabilir ve onları kişisel sağlık verilerinin bir görünüm görmek yardımcı olur. Diğer çözümler, aynı iletişim akışını sağlamak için bir hastane odası içinde yse tablet ağ geçidi veya statik ağ geçidi kullanabilir.

### <a name="export-to-azure-api-for-fhirreg"></a>FHIR için Azure API'sine dışa aktarma&reg;
Azure IoT Central HIPAA uyumludur&reg; ve HITRUST sertifikalıdır, ancak hasta sağlığıyla ilgili verileri FHIR için Azure API'sine de göndermek isteyebilirsiniz. [FHIR için Azure API,](../../healthcare-apis/overview.md) klinik sağlık verileri için, sağlık verilerinizle yeni etkileşim sistemleri oluşturmanıza olanak tanıyan, tam olarak yönetilen, standartlara dayalı, uyumlu bir API'dir. Bulutta yönetilen bir Hizmet Olarak Platform (PaaS) teklifiyle desteklenen FHIR API'leri aracılığıyla hızlı veri alışverişine olanak tanır. IoT Central'ın Sürekli Veri Verme işlevini kullanarak, FHIR için Azure API'sine veri gönderebilirsiniz.

### <a name="machine-learning"></a>Makine öğrenimi
Verilerinizi bir araya getirip FHIR formatına çevirdikten sonra, öngörüleri zenginleştirebilen ve bakım ekibiniz için daha akıllı karar verme olanağı sağlayan makine öğrenimi modelleri oluşturabilirsiniz. Makine öğrenimi modelleri oluşturmak, eğitmek ve dağıtmak için kullanılabilecek farklı hizmet türleri vardır. Azure'un makine öğrenimi tekliflerinin nasıl kullanılacağı hakkında daha fazla bilgiyi [makine öğrenimi belgelerimizde](../../machine-learning/index.yml)bulabilirsiniz.

### <a name="provider-dashboard"></a>Sağlayıcı panosu
FHIR için Azure API'sinde bulunan veriler, hasta öngörüleri panosu oluşturmak için kullanılabilir veya bakım ekiplerinin hasta durumunu görselleştirmelerine yardımcı olmak için doğrudan bir EMR'ye entegre edilebilir. Bakım ekipleri bu gösterge tablosundan yardıma ihtiyacı olan hastalarla ilgilenmek ve erken uyarı belirtilerini tespit etmek için kullanabilirler. Power BI gerçek zamanlı sağlayıcı panosunu nasıl oluşturizleyeceğimizi öğrenmek için nasıl [yapılacağını zedelekılavuzumuzu takip edin.](howto-health-data-triage.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Sürekli hasta izleme uygulama şablonu nasıl dağıtılayizleyeceğini öğrenin](tutorial-continuous-patient-monitoring.md)