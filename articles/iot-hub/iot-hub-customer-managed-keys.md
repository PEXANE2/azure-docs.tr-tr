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
ms.openlocfilehash: 609f68c79159c4ce17468fc1df13c0c9eae7f211
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79370585"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>IoT Hub için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi

IoT Hub, müşteri tarafından yönetilen anahtarlar (CMK) ile bekleyen verilerin şifrelenmesini destekler, Ayrıca, kendi anahtarını getir (BYOK) olarak da bilinen Azure IoT Hub desteği. Azure IoT Hub, bekleyen ve aktarım sırasında veri şifrelemesini sağlar. Varsayılan olarak IoT Hub, verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK desteğiyle, müşteriler artık [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)kullanarak müşterilerin yönettiği bir anahtar şifreleme anahtarıyla bekleyen verileri şifreleme seçeneği vardır.

Bu özellik, şu bölgelerden birinde yeni bir IoT Hub (temel veya Standart katman) oluşturulmasını gerektirir: Doğu ABD, Batı ABD 2, Orta Güney ABD veya US Gov. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)ile bizimle iletişim kurun. Microsoft Destek ile iletişim kurarken şirketinizin adını ve abonelik KIMLIĞINIZI paylaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
