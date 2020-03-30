---
title: Bağlı Fabrika çözüm özellikleri - Azure | Microsoft Dokümanlar
description: Bu makalede, Bulut panosu, kurallar ve uyarılar gibi Bağlı Fabrika önceden yapılandırılmış çözümün özelliklerine genel bir bakış açıklanmaktadır.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: dobett
ms.openlocfilehash: c868aa0f1c2449ccf163523c9ded25a31d1d84c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820103"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>Connected Factory IoT çözüm hızlandırıcısı nedir?

Connected Factory, Microsoft'un açık kaynak çözümünde olduğu gibi paketlenmiş Azure Endüstriyel IoT başvuru mimarisinin bir uygulamasıdır. Ticari bir ürün için bir başlangıç noktası olarak kullanabilirsiniz. Azure [IoT çözüm hızlandırıcılarından](https://www.azureiotsolutions.com/#solutions/types/CF)Azure aboneliğinize Bağlı Fabrika çözümünün önceden oluşturulmuş bir sürümünü dağıtabilirsiniz.

![Bağlı Fabrika çözüm panosu](./media/iot-accelerators-connected-factory-features/dashboard.png)

Bağlı Fabrika çözüm hızlandırıcı [kodu GitHub'da mevcuttur.](https://github.com/Azure/azure-iot-connected-factory)

Bağlı Fabrika aşağıdaki özellikleri içerir:

## <a name="industrial-device-interoperability"></a>Endüstriyel cihaz birlikte çalışabilirliği

- OPC UA arabirimi yle endüstriyel varlıklara bağlanın.
- Onlardan canlı telemetri görmek için benzetimli üretim hatlarını (Docker kapsayıcılarında OPC UA sunucularını çalıştıran) kullanın.
- Bulut panosundan OPC UA sunucularının OPC UA bilgi modeline göz atın.

## <a name="remote-management"></a>Uzaktan yönetim

- OPC UA varlıklarınızı bulut panosundan yapılandırın (arama yöntemleri, okuma ve yazma verileri).
- OPC UA varlıklarınızdan telemetri verilerini bir bulut panosundan yayımlayın ve yayımlayın.

## <a name="cloud-dashboard"></a>Bulut panosu

- Telemetri önizlemelerini doğrudan bir bulut panosunda görüntüleyin.
- Telemetri verilerindeki eğilimleri görüntüleyin ve Time Series Insights Explorer panosunu kullanarak korelasyonoluşturun.
- Bulut panosundan hesaplanan Genel Ekipman Verimliliği (OEE) ve Anahtar Performans Göstergeleri 'ne (KP'ler) bakın.
- Endüstriyel varlık hiyerarşilerini bir ağaç topolojisinde ve etkileşimli bir haritada görüntüleyin.
- Bulut panosundan uyarıları görüntüleyin, onaylayın ve kapatın.

## <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

- [Azure Zaman Serisi Öngörüleri,](../time-series-insights/time-series-insights-overview.md) büyük miktarda zaman serisi verilerini depolamak, görselleştirmek ve sorgulamak için oluşturulmuştür. Bağlı Fabrika bu hizmetlerden yararlanır.
- Connected Factory, cihaz verilerinizin derin ve gerçek zamanlı analizini yapmanızı sağlayan bu hizmetle bütünleşir.

## <a name="rules-and-alerts"></a>Kurallar ve uyarılar

[Uyarılar için eşik tabanlı kuralları yapılandırın.](iot-accelerators-connected-factory-configure.md)

## <a name="end-to-end-security"></a>Uçuca güvenlik

- Role Tabanlı Erişim Denetimi (RBAC) kullanan kullanıcılar için güvenlik izinlerini yapılandırın.
- Uçuça şifreleme, OPC UA kimlik doğrulaması (X.509 sertifikaları kullanılarak) ve güvenlik belirteçleri kullanılarak uygulanır.

## <a name="customizability"></a>Customizability

- Belirli iş gereksinimlerini karşılamak için çözümü özelleştirin.
- Tam çözüm kaynak kodu GitHub mevcuttur. Bağlı [Fabrika önceden yapılandırılmış çözüm](https://github.com/Azure/azure-iot-connected-factory) deposuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bağlı Fabrika çözüm hızlandırıcısı hakkında daha fazla bilgi edinmek için, [endüstriyel IoT aygıtlarımı yönetmek için Hızlı Başlatma'ya](quickstart-connected-factory-deploy.md)bakın bulut tabanlı bir çözümü deneyin.
