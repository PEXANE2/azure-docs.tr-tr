---
title: Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, kişiselleştirici için bekleyen veri şifreleme ve CMK 'nin nasıl etkinleştirileceği ve yönetileceği ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: 1a27199930587c1a096dd99462ebd0c9d65054ee
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100524504"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi

Kişiselleştirici hizmeti, buluta kalıcı hale geldiğinde verilerinizi otomatik olarak şifreler. Kişiselleştirici hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma yeteneği istemek için, [Kişiselleştirme hizmeti Customer-Managed anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Kişiselleştirici hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir kişiselleştirici kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile kişiselleştirici kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Kişiselleştirici hizmeti Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](../../key-vault/general/overview.md)