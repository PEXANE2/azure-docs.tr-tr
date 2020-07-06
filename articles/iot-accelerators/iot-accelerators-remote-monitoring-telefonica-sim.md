---
title: Uzaktan Izleme çözümünde SIM verilerini tümleştirme-Azure | Microsoft Docs
description: Bu makalede, Telefónica SIM verilerinin uzaktan Izleme çözümüne nasıl tümleştirileceği açıklanır.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: b07e21131d9560a49d99644525835ac5ee3bac9e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61442248"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Uzaktan Izleme çözümünde SIM verilerini tümleştirme

IoT cihazları genellikle buluta, her yerden veri akışları gönderebileceği bir SIM kartı kullanarak bağlanır. Azure IoT uzaktan Izleme çözümü, IoT tarafından yönetilen bağlantı verilerinin tümleştirilmesine olanak tanır. böylece operatörler IoT SIM tarafından belirtilen veriler aracılığıyla cihazın sistem durumunu da izleyebilir.

Uzaktan Izleme, Telefónica IoT bağlantısı ile kullanıma hazır tümleştirme sağlar ve IoT bağlantı platformunu kullanan müşterilerin cihaz SIMs bağlantı verilerini çözümlerle eşitlemesine izin verir. Bu çözüm, GitHub [deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)aracılığıyla diğer IoT bağlantı sağlayıcılarını destekleyecek şekilde genişletilebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Telefónica IoT SIM verilerini uzaktan Izleme çözümüyle tümleştirme
* Gerçek zamanlı telemetri verilerini görüntüleme
* SIM verilerini görüntüle

## <a name="telefnica-iot-integration-setup"></a>Telefónica IoT tümleştirme kurulumu

### <a name="prerequisites"></a>Önkoşullar

Bu ek uzaktan Izleme özelliği şu anda önizlemededir. Bağlantı verilerinizi Azure uzaktan Izleme çözümüne eşitlemek için aşağıdaki adımları izleyin:

1. [Telefónica's sitesinde](https://iot.telefonica.com/contact)bir istek doldur, iletişim verileriniz dahil olmak üzere **Azure uzaktan izleme**seçeneğini belirleyin.
2. Telefónica, hesabınızı etkinleştirir.
3. Henüz bir Telefónica istemcisi değilseniz ve bu veya diğer IoT bağlantısı bulutu 'na yönelik hizmetler 'in tadını çıkarmak istiyorsanız, [Telefónica's sitesini](https://iot.telefonica.com/) ziyaret edin ve **bağlantı**seçeneğini belirleyin.

### <a name="telefnica-sim-setup"></a>Telefónica SIM kurulumu
Telefónica SIM & Azure Ikizi cihaz KIMLIĞI ilişkilendirmesi Telefónica IoT SIM "alias" özelliğine dayalıdır. 

[Telefónica IoT bağlantısı platformu portalı](https://m2m-movistar-es.telefonica.com/) > sım envanter > gidin ve SIM 'nizi seçin ve her bir SIM "diğer adı" nı Istediğiniz ikizi DeviceID ile güncelleştirin. Bu görev, toplu modda da yapılabilir (Telefónica IoT bağlantısı platformu Kullanıcı kılavuzlarına bakın).

Bu görev, toplu modda da yapılabilir (Telefónica IoT bağlantısı platformu Kullanıcı kılavuzlarına bakın)

![Telefónica güncelleştirmesi](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Cihazınızı uzaktan Izlemeye bağlamak için bu öğreticileri [C](iot-accelerators-connecting-devices-linux.md) veya [node](iot-accelerators-connecting-devices-node.md)kullanarak izleyebilirsiniz. 

## <a name="view-device-telemetry-and-sim-properties"></a>Cihaz telemetrisini ve SIM özelliklerini görüntüleme

Telefónica hesabınız düzgün bir şekilde yapılandırıldıktan ve cihazınız bağlandıktan sonra cihaz ayrıntılarını ve SIM verilerini görüntüleyebilirsiniz.

Aşağıdaki bağlantı parametreleri yayımlandı:

* ICCID
* IP
* Ağ iletişim durumu
* SIM durumu
* Ağ tabanlı konum
* Tüketilen veri trafiği

![Pano](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

SIM verilerini Azure IoT uzaktan Izleme ile tümleştirme konusunda bir genel bakışa sahip olduğunuza göre, çözüm hızlandırıcılarına yönelik sonraki adımlar aşağıda verilmiştir:

* [Azure IoT uzaktan Izleme çözümünü çalıştırma](quickstart-remote-monitoring-deploy.md)
* [Gelişmiş izleme gerçekleştirme](iot-accelerators-remote-monitoring-monitor.md)
* [Cihazlarınızı yönetme](iot-accelerators-remote-monitoring-manage.md)
* [Cihaz sorunlarını giderme](iot-accelerators-remote-monitoring-maintain.md)

