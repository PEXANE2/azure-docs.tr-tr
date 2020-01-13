---
title: Azure IoT Hub, müşteri tarafından yönetilen anahtarlar aracılığıyla bekleyen veri şifrelemesi | Microsoft Docs
description: IoT Hub için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: asrastog
ms.openlocfilehash: 1bb55d593878026bb3e57014a317b4fc0158d734
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913140"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>IoT Hub için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi

IoT Hub, müşteri tarafından yönetilen anahtarlar (CMK) ile bekleyen verilerin şifrelenmesini destekler, Ayrıca, kendi anahtarını getir (BYOK) olarak da bilinen Azure IoT Hub desteği. Azure IoT Hub, bekleyen ve aktarım sırasında veri şifrelemesini sağlar. Varsayılan olarak IoT Hub, verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK desteğiyle, müşteriler artık [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)kullanarak müşterilerin yönettiği bir anahtar şifreleme anahtarıyla bekleyen verileri şifreleme seçeneği vardır.

Bu özellik, şu bölgelerden birinde yeni bir IoT Hub (temel veya Standart katman) oluşturulmasını gerektirir: Doğu ABD, Batı ABD 2 veya Orta Güney ABD. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)ile bizimle iletişim kurun. Microsoft Destek ile iletişim kurarken şirketinizin adını ve abonelik KIMLIĞINIZI paylaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)