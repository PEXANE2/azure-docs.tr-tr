---
title: IoT Hub 'ınızın IP adresini anlama | Microsoft Docs
description: IoT Hub IP adresinizi ve özelliklerini sorgulamayı öğrenin. IoT Hub 'ınızın IP adresi, olağanüstü durum kurtarma veya bölgesel yük devretme gibi belirli senaryolar sırasında değiştirilebilir.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 5c77fb30ef60c1ad82d0a87442bc8af186c54321
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383907"
---
# <a name="iot-hub-ip-addresses"></a>IoT Hub IP adresleri

IoT Hub 'ın IP adresi önekleri, *AzureIoTHub* [Service etiketi](../virtual-network/service-tags-overview.md)altında düzenli aralıklarla yayımlanır. Doğru işlemi sağlamak için, IoT cihazlarınızın *AzureIoTHub* hizmet etiketi altında listelenen ön ekleri karşılamak üzere giden bağlantılara sahip olması gerekir. IoT uygulama hizmetlerinizin Ayrıca, *EventHub* hizmet etiketi altında listelenen ön ekleri karşılamak için giden bağlantısı olması gerekir.


## <a name="best-practices"></a>En iyi uygulamalar

* IoT Hub 'ının IP adresi önekleri değişebilir. Bu değişiklikler, etkin olmadan önce hizmet etiketleri aracılığıyla düzenli olarak yayımlanır. Bu nedenle, en son hizmet etiketlerini düzenli olarak almak ve kullanmak için işlem geliştirmeniz önemlidir. Bu işlem, [hizmet etiketleri bulma API 'si](../virtual-network/service-tags-overview.md#service-tags-in-on-premises)aracılığıyla otomatikleştirilebilir.
* AzureIoTHub kullanın *. [ bölge adı]* etiketi, belirli bir bölgedeki IoT Hub uç noktaları tarafından kullanılan IP öneklerini belirler. Veri merkezi olağanüstü durum kurtarmaya veya [bölgesel yük devretmeye](iot-hub-ha-dr.md) yönelik hesaba gitmek için IoT Hub coğrafi ÇIFT bölgesinin IP öneklerine yönelik bağlantıların de etkinleştirildiğinden emin olun.


## <a name="support-for-ipv6"></a>IPv6 desteği 

IPv6 Şu anda IoT Hub desteklenmiyor.