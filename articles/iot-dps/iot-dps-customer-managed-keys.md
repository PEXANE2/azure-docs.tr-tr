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
ms.openlocfilehash: d22a01bab81fc330484e7715a65c89a1cfd7802c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94967185"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Cihaz sağlama hizmeti için müşteri tarafından yönetilen anahtarlarla bekleyen verilerin şifrelenmesi

## <a name="overview"></a>Genel Bakış

Cihaz sağlama hizmeti (DPS), müşteri tarafından yönetilen anahtarlarla (CMK) kalan verilerin şifrelenmesini destekler (BYOK) olarak da bilinir. DPS, veri merkezlerimizde yazıldığı sırada verilerin geri kalanı ve aktarım sırasında şifrelenmesini sağlar ve siz de ona erişirken şifresini çözer. Varsayılan olarak, DPS, bekleyen verileri şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. CMK ile, bekleyen verileri bir anahtar şifreleme anahtarıyla şifrelemeyi, [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)aracılığıyla yönetilen varsayılan platform şifrelemesi üzerinde ek bir şifreleme katmanı edinebilirsiniz. Bu, anahtarları oluşturma, döndürme, devre dışı bırakma ve iptal etme esnekliği sağlar. DPS için CMK yapılandırılmışsa, verilerinizi etkin bir şekilde koruyan iki koruma katmanı ile çift şifrelemenin etkin hale gelir. 

Bu özellik yeni bir DPS oluşturulmasını gerektirir. Bu özelliği denemek için [Microsoft desteği](https://azure.microsoft.com/support/create-ticket/)ile bizimle iletişim kurun. Microsoft Destek ile iletişim kurarken şirketinizin adını ve abonelik KIMLIĞINIZI paylaşabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz sağlama hizmeti hakkında daha fazla bilgi edinin](./index.yml)

* [Azure Key Vault hakkında daha fazla bilgi edinin](../key-vault/general/overview.md)