---
title: Azure Aygıt Sağlama Hizmeti veri şifrelemesi müşteri tarafından yönetilen anahtarlar aracılığıyla istirahatte| Microsoft Dokümanlar
description: Cihaz Sağlama Hizmeti için müşteri tarafından yönetilen anahtarlarla veri şifreleme
author: chrissie926
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: menchi
ms.openlocfilehash: b065135715868094b7e248aebfca35f3b55a4c48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77675149"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Cihaz Sağlama Hizmeti için müşteri tarafından yönetilen anahtarlarla veri şifreleme

## <a name="overview"></a>Genel Bakış

Aygıt Sağlama Hizmeti (DPS), kendi anahtarınızı getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlarla (CMK) veri şifrelemesini destekler. DPS, veri şifrelemesini istirahat te ve aktarım sırasında sağlar. Varsayılan olarak DPS, verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK desteği ile müşteriler artık Azure Anahtar Kasası'nı kullanarak müşteriler tarafından yönetilen anahtar [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)şifreleme anahtarıyla verileri şifreleme seçeneğine sahip.

Bu yetenek, aşağıdaki bölgelerden birinde yeni bir DPS oluşturulmasını gerektirir: Doğu ABD, Batı ABD 2 veya Güney Orta ABD. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)aracılığıyla bize ulaşın. Microsoft desteğine başvururken şirket adınızı ve abonelik kimliğinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz Sağlama Hizmeti hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-dps/)

* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)