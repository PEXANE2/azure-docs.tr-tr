---
title: Azure IoT Hub veri şifrelemesi müşteri tarafından yönetilen anahtarlar aracılığıyla istirahatte| Microsoft Dokümanlar
description: IoT Hub için müşteri tarafından yönetilen anahtarlarla veri şifrelemesi
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: asrastog
ms.openlocfilehash: 609f68c79159c4ce17468fc1df13c0c9eae7f211
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370585"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>IoT Hub için müşteri tarafından yönetilen anahtarlarla veri şifrelemesi

IoT Hub, Azure IoT Hub desteği olan kendi anahtarınızı getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlarla (CMK) veri şifrelemesini destekler. Azure IoT Hub, veri şifrelemesini istirahat te ve aktarım sırasında sağlar. Varsayılan olarak, IoT Hub verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK desteği ile müşteriler artık Azure Anahtar Kasası'nı kullanarak müşteriler tarafından yönetilen anahtar [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)şifreleme anahtarıyla verileri şifreleme seçeneğine sahip.

Bu özellik, aşağıdaki bölgelerden birinde yeni bir IoT Hub 'ı (temel veya standart katman) oluşturulmasını gerektirir: Doğu ABD, Batı ABD 2, Güney Orta ABD veya ABD Gov. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)aracılığıyla bize ulaşın. Microsoft desteğine başvururken şirket adınızı ve abonelik kimliğinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
