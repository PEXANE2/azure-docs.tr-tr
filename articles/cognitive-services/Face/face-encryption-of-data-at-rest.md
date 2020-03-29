---
title: Verilerin yüz hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Veri şifrelemesini yerinde yüz.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 793c21bb2341033aa51ff8c639846e57ada4bae3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372221"
---
# <a name="face-service-encryption-of-data-at-rest"></a>Verilerin yüz hizmeti şifrelemesi

Face hizmeti, verilerinizi bulutta kalıcı hale geldiğinde otomatik olarak şifreler. Face hizmeti şifrelemesi verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanabilme isteğinde bulunmak [için, Yüz Hizmetleri Müşteri Tarafından Yönetilen Anahtar İstek Formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderin. İsteğinizin durumunu duymak yaklaşık 3-5 iş günü sürer. İsteğe bağlı olarak, bir kuyruğa yerleştirilebilir ve alan kullanılabilir hale geldikçe onaylanabilirsiniz. Face hizmetinde CMK'yı kullanmak üzere onaylandıktan sonra, yeni bir Yüz kaynağı oluşturmanız ve Fiyatlandırma Katmanı olarak E0'i seçmeniz gerekir. E0 fiyatlandırma katmanına sahip Yüz kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault'u kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Yüz Hizmetleri Müşteri Tarafından Yönetilen Anahtar Talep Formu](https://aka.ms/cogsvc-cmk)
* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


