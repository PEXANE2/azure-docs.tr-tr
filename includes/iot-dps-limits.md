---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b8d60da507aa1eb312d1a8505f756ff337085ff
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77592449"
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
| İşlemler | 200/dk/hizmet |
| Cihaz kayıtları | 200/dk/hizmet |
| Cihaz yoklama işlemi | 5/10 sn/cihaz |
