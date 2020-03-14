---
title: Bekleyen verilerin şifrelenmesi Content Moderator
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin şifrelenmesi Content Moderator.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: b41d822791f61fce274f628b87c478c3a986f4c3
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79372166"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Content Moderator

Content Moderator, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler, böylece kurumsal güvenlik ve uyumluluk hedeflerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için [Content moderator müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Content Moderator hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir Content Moderator kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile Content Moderator kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>Content Moderator ekibiniz için veri şifrelemeyi etkinleştirme

Content Moderator gözden geçirme ekibiniz için veri şifrelemeyi etkinleştirmek için bkz. [hızlı başlangıç: Web üzerinde Try Content moderator](quick-start.md#create-a-review-team).  

> [!NOTE]
> Content Moderator E0 fiyatlandırma katmanıyla bir _kaynak kimliği_ sağlamanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

* [Content Moderator müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
