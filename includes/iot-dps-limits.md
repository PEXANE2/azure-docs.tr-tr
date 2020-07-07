---
author: rothja
ms.service: iot-dps
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: bbe928084a758335e5fa3b0531726c77a6d599b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85838821"
---
Aşağıdaki tabloda, Azure IoT Hub cihaz sağlama hizmeti kaynakları için uygulanan sınırlar listelenmektedir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına maksimum cihaz sağlama hizmeti | 10 |
| En fazla kayıt sayısı | 1.000.000 |
| En fazla kayıt sayısı | 1.000.000 |
| En fazla kayıt grubu sayısı | 100 |
| CA sayısı üst sınırı | 25 |
| En fazla bağlantılı IoT Hub sayısı | 50 |
| En büyük ileti boyutu | 96 KB|

> [!NOTE]
> Sağlama hizmetinizdeki kayıt ve kayıt sayısını artırmak için [Microsoft desteği](https://azure.microsoft.com/support/options/)başvurun.

> [!NOTE]
> En fazla CA sayısının artırılması desteklenmez.

Cihaz sağlama hizmeti, aşağıdaki kotalar aşıldığında istekleri kısıtlar.

| Kısıtlama | Birim başına değer |
| --- | --- |
| Operations | 200/dk/hizmet |
| Cihaz kayıtları | 200/dk/hizmet |
| Cihaz yoklama işlemi | 5/10 sn/cihaz |
