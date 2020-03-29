---
title: SIM verilerini Uzaktan İzleme Çözümüne entegre etme - Azure| Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözümüne Telefónica SIM verilerinin nasıl entegre edilebildiğini açıklanmaktadır.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: b07e21131d9560a49d99644525835ac5ee3bac9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61442248"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>SIM verilerini Uzaktan İzleme çözümüne entegre etme

IoT aygıtları genellikle buluta her yerden veri akışı göndermelerine olanak tanıyan bir SIM kart kullanarak bağlanır. Azure IoT Uzaktan İzleme çözümü, ioT Yönetilen Bağlantı verilerinin tümleştirilmesine olanak sağlayarak operatörlerin IoT SIM tarafından sağlanan veriler aracılığıyla cihazın durumunu da izleyebilirsiniz.

Uzaktan İzleme, Telefónica IoT Bağlantısı ile kutu dışı entegrasyon sağlayarak, IoT Bağlantı Platformu'nu kullanan müşterilerin cihaz SIM bağlantı verilerini çözümleriyle senkronize etmelerine olanak tanır. Bu çözüm, GitHub [deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)aracılığıyla diğer IoT Bağlantı sağlayıcılarını desteklemek için genişletilebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* Telefónica IoT SIM verilerini Uzaktan İzleme çözümüne entegre etme
* Gerçek zamanlı telemetri verilerini görüntüleme
* SIM verilerini görüntüleme

## <a name="telefnica-iot-integration-setup"></a>Telefónica IoT entegrasyon kurulumu

### <a name="prerequisites"></a>Ön koşullar

Bu ek Uzaktan İzleme özelliği şu anda önizlemededir. Bağlantı verilerinizi Azure Uzaktan İzleme Çözümü ile senkronize etmek için aşağıdaki adımları izleyin:

1. [Telefónica'nın sitesindebir](https://iot.telefonica.com/contact)isteği doldurun , kişi verileriniz de dahil olmak üzere **Azure Uzaktan İzleme**seçeneğini seçin.
2. Telefónica hesabınızı etkinleştirir.
3. Henüz bir Telefónica istemcisi değilseniz ve bu veya diğer IoT Bağlantı Bulut Hazır hizmetlerinin keyfini çıkarmak istiyorsanız, [Telefónica sitesini](https://iot.telefonica.com/) ziyaret edin ve **Bağlantı**seçeneğini seçin.

### <a name="telefnica-sim-setup"></a>Telefónica SIM kurulumu
Telefónica SIM & Azure Twin aygıt kimliği ilişkisi Telefónica IoT SIM "takma ad" özelliğine dayanır. 

[Telefónica IoT Bağlantı Platformu Portalı>](https://m2m-movistar-es.telefonica.com/) sim envanter > SIM seçin ve istediğiniz Twin deviceID ile her SIM "takma" güncelleyin gidin. Bu görev toplu modda da yapılabilir (Telefónica IoT Bağlantı Platformu kullanım kılavuzlarına bakın).

Bu görev toplu modda da yapılabilir (Telefónica IoT Bağlantı Platformu kullanım kılavuzlarına bakın)

![Telefónica Güncelleme](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Cihazınızı Uzaktan İzleme'ye bağlamak [için, C](iot-accelerators-connecting-devices-linux.md) veya [Düğüm](iot-accelerators-connecting-devices-node.md)kullanarak bu eğitimleri takip edebilirsiniz. 

## <a name="view-device-telemetry-and-sim-properties"></a>Aygıt telemetrisini ve SIM Özelliklerini görüntüleme

Telefónica hesabınız düzgün bir şekilde yapılandırıldıktan ve cihazınız bağlandıktan sonra cihaz ayrıntılarını ve SIM verilerini görüntüleyebilirsiniz.

Aşağıdaki bağlantı parametreleri yayımlanır:

* ICCID
* IP
* Ağ varlığı
* SIM Durumu
* Ağ tabanlı konum
* Tüketilen veri trafiği

![Pano](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

SIM Verilerini Azure IoT Uzaktan İzleme'ye nasıl entegre ettiğinize dair genel bir bakışa sahip olduğunuza göre, çözüm hızlandırıcıları için aşağıdaki sonraki adımlar önerilir:

* [Azure IoT Uzaktan İzleme çözümünü çalıştırın](quickstart-remote-monitoring-deploy.md)
* [Gelişmiş izleme gerçekleştirme](iot-accelerators-remote-monitoring-monitor.md)
* [Cihazlarınızı yönetme](iot-accelerators-remote-monitoring-manage.md)
* [Cihaz sorunlarını giderme](iot-accelerators-remote-monitoring-maintain.md)

