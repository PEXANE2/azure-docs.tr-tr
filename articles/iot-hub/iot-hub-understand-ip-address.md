---
title: IoT Hub 'ınızın IP adresini anlama | Microsoft Docs
description: IoT Hub IP adresinizi ve özelliklerini sorgulamayı öğrenin. IoT Hub 'ınızın IP adresi, olağanüstü durum kurtarma veya bölgesel yük devretme gibi belirli senaryolar sırasında değiştirilebilir.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: 9c9d02eeead4ccbed372e9a55d9b551da83caa39
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642299"
---
# <a name="understanding-the-ip-address-of-your-iot-hub"></a>IoT Hub 'ınızın IP adresini anlama

IoT Hub 'ınızın IP adresi, özel bir IP adresi DEĞIL, hub 'ınız için yük dengeli bir genel bitiş noktasıdır. Adres, dağıtıldığı Azure bölgesinin ağ adresi aralığına göre belirlenir. Bu IP adresi bildirimde bulunmadan değiştirilebilir. Veri merkezi ağ güncelleştirmeleri, Datacenter olağanüstü durum kurtarma veya IoT Hub 'ın bölgesel yük devretmesi, IoT Hub IP adresini değiştirebilir. Azure IoT Hub bölgesel yük devretme ve kullanılabilirlik hakkında daha fazla ayrıntı için bkz. [IoT Hub yüksek kullanılabilirlik ve olağanüstü durum kurtarma](iot-hub-ha-dr.md) .

IoT Hub 'ınızın IP adresi, başka bir bölgeye yük devretmeden sonra değişir. [Bir IoT Hub 'ının el ile yük devretmesini gerçekleştirme](tutorial-manual-failover.md)öğreticisini izleyerek bu işlevselliği test edebilirsiniz.

## <a name="discover-your-iot-hub-ip-address"></a>IoT Hub IP adresinizi bulma

IoT Hub IP adresiniz, CNAME ([*IoT-Hub-Name*]. Azure-Devices.net) üzerinde BIR ters DNS araması kullanılarak bulunabilir. Bir IoT Hub örneğinin IP adresini doğrulamak için **nslookup** kullanabilirsiniz:

```cmd/sh
nslookup {YourIoTHubName}.azure-devices.net
```

Bu IP adresi bildirimde bulunmaksızın değiştirilebilir. Yük devretme veya olağanüstü durum kurtarma senaryosunda, IoT Hub IP adresinizi ikincil bölgede yoklamak zorunda olacaksınız.

## <a name="outbound-firewall-rules-for-iot-hub"></a>IoT Hub için giden güvenlik duvarı kuralları

IoT Hub ana bilgisayar adına veya etki alanına göre güvenlik duvarı kuralları ve filtreleme oluşturmayı deneyin. Yalnızca belirli adreslere giden trafiğe izin verirseniz, IoT Hub IP adresinizi düzenli olarak yoklayın ve güvenlik duvarı kurallarınızı güncelleştirebilirsiniz.

