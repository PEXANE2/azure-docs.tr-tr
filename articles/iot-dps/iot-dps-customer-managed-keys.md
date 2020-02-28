---
title: Müşteri tarafından yönetilen anahtarlar aracılığıyla bekleyen Azure cihaz sağlama hizmeti veri şifrelemesi | Microsoft Docs
description: Cihaz sağlama hizmeti için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi
author: chrissie926
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: menchi
ms.openlocfilehash: b065135715868094b7e248aebfca35f3b55a4c48
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77675149"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Cihaz sağlama hizmeti için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi

## <a name="overview"></a>Genel Bakış

Cihaz sağlama hizmeti (DPS), müşteri tarafından yönetilen anahtarlarla (CMK) kalan verilerin şifrelenmesini destekler (BYOK) olarak da bilinir. DPS, bekleyen ve aktarım sırasında veri şifrelemesini sağlar. Varsayılan olarak, DPS verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK desteğiyle, müşteriler artık [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)kullanarak müşterilerin yönettiği bir anahtar şifreleme anahtarıyla bekleyen verileri şifreleme seçeneği vardır.

Bu yetenek, şu bölgelerden birinde yeni bir DPS oluşturulmasını gerektirir: Doğu ABD, Batı ABD 2 veya Orta Güney ABD. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)ile bizimle iletişim kurun. Microsoft Destek ile iletişim kurarken şirketinizin adını ve abonelik KIMLIĞINIZI paylaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz sağlama hizmeti hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-dps/)

* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)