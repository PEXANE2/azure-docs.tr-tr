---
title: Azure IoT Hub, müşteri tarafından yönetilen anahtarlar aracılığıyla bekleyen veri şifrelemesi | Microsoft Docs
description: IoT Hub için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: asrastog
ms.openlocfilehash: 352da24b36124ff0446a81c1ecbc584da545bb16
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92142199"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>IoT Hub için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi

IoT Hub, kendi anahtarını getir (BYOK) olarak da bilinen, müşteri tarafından yönetilen anahtarlarla (CMK) kalan verilerin şifrelenmesini destekler. Azure IoT Hub, veri merkezlerimizde yazıldığı sırada verilerin, bekleyen ve aktarım sırasında şifrelenmesini sağlar ve ona erişirken sizin için şifresini çözer. IoT Hub, varsayılan olarak, bekleyen verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK ile, varsayılan şifrelemenin en üstünde başka bir şifreleme katmanı alabilir ve [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), bekleyen verileri bir anahtar şifreleme anahtarıyla, bu anahtarla yönetilen bir anahtarla şifrelemeyi tercih edebilirsiniz. Bu, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme esnekliği sağlar. IoT Hub için BYOK yapılandırıldıysa, ikinci bir koruma katmanı sunan ve Azure Key Vault bir şifreleme anahtarı denetlemenizi sağlayan çift şifrelemeyi de sağlarız.

Bu yetenek, yeni bir IoT Hub (temel veya Standart katman) oluşturulmasını gerektirir. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)ile bizimle iletişim kurun. Microsoft Destek ile iletişim kurarken şirketinizin adını ve abonelik KIMLIĞINIZI paylaşabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub hakkında daha fazla bilgi edinin](./about-iot-hub.md)

* [Azure Key Vault hakkında daha fazla bilgi edinin](../key-vault/general/overview.md)