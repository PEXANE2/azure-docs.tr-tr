---
title: Ölçüm Danışmanı hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı hizmet bekleyen verilerin şifrelenmesi.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 9d6a2f8a69d41d29e635b03425ed738484d6f408
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941535"
---
# <a name="metrics-advisor-service-encryption-of-data-at-rest"></a>Ölçüm Danışmanı hizmet bekleyen verilerin şifrelenmesi

Ölçüm Danışmanı hizmeti, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Ölçüm Danışmanı hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma yeteneği istemek için, [ölçüm Danışmanı hizmeti Customer-Managed anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Ölçüm Danışmanı hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir ölçüm Danışmanı kaynağı oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile ölçüm Danışmanı kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Danışmanı hizmeti Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
